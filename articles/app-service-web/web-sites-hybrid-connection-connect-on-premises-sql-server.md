---
title: "Ansluta till lokal SQL Server från en webbapp i Azure App Service med Hybridanslutningar"
description: Skapa en webbapp i Microsoft Azure och koppla den till en lokal SQL Server-databas
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 2b4e0539-1a0b-4aa1-8a69-b4b053c3b2e5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: cephalin
ms.openlocfilehash: 12456ef3e2aecfa7a03cca97de2ff6ffd9602357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Ansluta till lokal SQL Server från en webbapp i Azure App Service med Hybridanslutningar
Hybridanslutningar kan ansluta [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps till lokala resurser som använder en statisk TCP-port. Resurser som stöds är Microsoft SQL Server, MySQL, http-webb-API: er, användning och de flesta anpassade webbtjänster.

I kursen får du lära dig hur du skapar en App Service webbapp i den [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), ansluta webbappen till lokala lokala SQL Server-databasen med hjälp av funktionen Hybridanslutning, skapa ett enkelt ASP.NET-program som ska använda hybridanslutning och distribuerar program till App Service webbapp. Slutförda webbappen i Azure lagrar autentiseringsuppgifter i en databas för medlemskap som är lokalt. Kursen riktar sig till några tidigare erfarenheter med hjälp av Azure eller ASP.NET.

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> Web Apps-delen av funktionen Hybridanslutningar är endast tillgänglig i den [Azure Portal](https://portal.azure.com). Om du vill skapa en anslutning i BizTalk-tjänst finns [Hybridanslutningar](http://go.microsoft.com/fwlink/p/?LinkID=397274).  
> 
> 

## <a name="prerequisites"></a>Krav
Den här kursen behöver du följande produkter. Alla är tillgängliga i ledigt versioner, så du kan börja utveckla för Azure och helt kostnadsfritt.

* **Azure-prenumeration** – för en kostnadsfri prenumeration finns [kostnadsfri utvärderingsversion av Azure](/pricing/free-trial/).
* **Visual Studio 2013** – om du vill hämta en kostnadsfri utvärderingsversion av Visual Studio 2013, se [Visual Studio-hämtningar](http://www.visualstudio.com/downloads/download-visual-studio-vs). Installera detta innan du fortsätter.
* **Microsoft .NET Framework 3.5 servicepack 1** -om operativsystemet är Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 eller Windows Server 2008 R2, kan du aktivera det på Kontrollpanelen > program och funktioner > Aktivera Windows-funktioner på eller stänga av. Annars kan du hämta det från den [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).
* **SQL Server 2014 Express med verktyg** -hämta Microsoft SQL Server Express kostnadsfritt på den [sidan Microsoft Web Platform databas](http://www.microsoft.com/web/platform/database.aspx). Välj den **Express** (inte LocalDB) version. Den **Express med verktyg** version omfattar SQL Server Management Studio, som du ska använda i den här självstudiekursen.
* **SQL Server Management Studio Express** - detta ingår i SQL Server 2014 Express verktyg hämta som nämns ovan, men om du måste installera separat, du kan hämta och installera den från den [hämtningssidan för SQL Server Express](http://www.microsoft.com/web/platform/database.aspx).

Kursen förutsätter att du har en Azure-prenumeration, att du har installerat Visual Studio 2013 och att du har installerat eller aktiverat .NET Framework 3.5. Kursen visar hur du installerar SQL Server 2014 Express i en konfiguration som fungerar med funktionen Azure Hybridanslutningar (en standardinstans med en statisk TCP-port). Hämta SQL Server 2014 Express med verktyg från den plats som nämns ovan om du inte installerade SQL Server innan du påbörjar självstudiekursen.

### <a name="notes"></a>Anteckningar
Om du vill använda en lokal SQL Server eller SQL Server Express-databas med en hybridanslutning måste TCP/IP vara aktiverat på en statisk port. Standardinstanser på SQL Server använder statisk port 1433, inte namngivna instanser.

Datorn där du installerar lokal Hybridanslutningshanterare agenten:

* Måste ha en utgående anslutning till Azure via:

| Port | Varför |
| --- | --- |
| 80 |**Krävs** för HTTP-porten för valideringen av servercertifikatet och eventuellt för dataanslutning. |
| 443 |**Valfria** för dataanslutning. Om utgående anslutning till 443 är tillgänglig, används TCP-port 80. |
| 5671 och 9352 |**Rekommenderade** men valfritt för dataanslutning. Observera att det här läget vanligtvis ger högre genomströmning. Om utgående anslutning till dessa portar är tillgänglig, används TCP-port 443. |

* Måste kunna nå den *värdnamn*:*portnumber* på din lokala resursen.

Stegen i den här artikeln förutsätter att du använder webbläsaren från den dator som är värd för agenten lokalt hybrid-anslutning.

Om du redan har SQL Server installeras i en konfiguration och i en miljö som uppfyller de villkor som beskrivs ovan kan du gå vidare och börja med [och skapa en SQL Server-databas lokalt](#CreateSQLDB).

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. Installera SQL Server Express, aktivera TCP/IP och skapa en SQL Server-databas lokalt
Det här avsnittet visar hur du installerar SQL Server Express, aktivera TCP/IP och skapa en databas så att webbprogrammet fungerar med Azure Portal.

### <a name="install-sql-server-express"></a>Installera SQL Server Express
1. Installera SQL Server Express, köra den **SQLEXPRWT_x64_ENU.exe** eller **SQLEXPR_x86_ENU.exe** filen som du hämtade. Guiden för SQL Server Installationscenter visas.
   
    ![SQL Server-installation][SQLServerInstall]
2. Välj **fristående nya SQL Server-installation eller Lägg till funktioner i en befintlig installation**. Följ instruktionerna, accepterar du standardalternativen och inställningar, tills du kommer till den **instanskonfiguration** sidan.
3. På den **instanskonfiguration** väljer **standardinstansen**.
   
    ![Välj standardinstansen][ChooseDefaultInstance]
   
    Som standard lyssnar standardinstansen av SQL Server för begäranden från SQL Server-klienter på statisk port 1433, vilket är Hybridanslutningar-funktionen ska fungera. Namngivna instanser använder dynamiska portar och UDP, vilket inte stöds av Hybridanslutningar.
4. Godkänn standardinställningarna på den **serverkonfiguration** sidan.
5. På den **konfiguration av databasmotor** sidan under **autentiseringsläge**, Välj **blandat läge (SQL Server-autentisering och Windows-autentisering)**, och ange ett lösenord.
   
    ![Välj blandat läge][ChooseMixedMode]
   
    I den här självstudiekursen kommer du att använda SQL Server-autentisering. Glöm inte att komma ihåg det lösenord som du anger, eftersom du behöver senare.
6. Gå igenom resten av guiden för att slutföra installationen.

### <a name="enable-tcpip"></a>Aktivera TCP/IP
Om du vill aktivera TCP/IP, använder du SQL Server Configuration Manager, som installerades när du installerade SQL Server Express. Följ stegen i [aktivera nätverksprotokoll för TCP/IP för SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) innan du fortsätter.

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a>Skapa en SQL Server-databas lokalt
Ditt webbprogram för Visual Studio kräver en databas för medlemskap som kan nås av Azure. Detta kräver en SQL Server eller SQL Server Express-databas (inte LocalDB-databasen som MVC-mallen använder som standard), så ska du skapa medlemskapsdatabasen nästa.

1. Anslut till SQL Server som du precis har installerat i SQL Server Management Studio. (Om den **Anslut till Server** dialogrutan inte visas automatiskt, gå till **Object Explorer** i den vänstra rutan klickar du på **Anslut**, och klicka sedan på **databasmotorn**.) ![Ansluta till servern][SSMSConnectToServer]
   
    För **servertyp**, Välj **databasmotorn**. För **servernamn**, kan du använda **localhost** eller namnet på den dator som du använder. Välj **SQL Server-autentisering**, och sedan logga in med sa-användarnamnet och lösenordet som du skapade tidigare.
2. Om du vill skapa en ny databas med hjälp av SQL Server Management Studio, högerklicka på **databaser** i Object Explorer och klicka sedan på **ny databas**.
   
    ![Skapa en ny databas][SSMScreateNewDB]
3. I den **ny databas** dialogrutan Ange MembershipDB för databasens namn och klicka sedan på **OK**.
   
    ![Ange databasnamn][SSMSprovideDBname]
   
    Observera att du inte göra några ändringar i databasen nu. Information om gruppmedlemskap läggas till automatiskt senare av ditt webbprogram när den körs.
4. I Object Explorer, om du expanderar **databaser**, ser du att medlemskapsdatabasen har skapats.
   
    ![MembershipDB skapas][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-the-azure-portal"></a>B. Skapa en webbapp i Azure Portal
> [!NOTE]
> Om du redan har skapat en webbapp i Azure Portal som du vill använda för den här självstudiekursen, du kan gå vidare till [skapa en Hybrid-anslutning och en BizTalk Service](#CreateHC) och fortsätta därifrån.
> 
> 

1. I den [Azure Portal](https://portal.azure.com), klickar du på **ny** > **webb + mobilt** > **webbapp**.
   
    ![Knappen Nytt][New]
2. Konfigurera ditt webbprogram och klicka sedan på **skapa**.
   
    ![Namn på webbplats][WebsiteCreationBlade]
3. Webbprogrammet har skapats och dess web app bladet visas efter en liten stund. Bladet är en lodrätt rullningsbara instrumentpanel där du kan hantera ditt webbprogram.
   
    ![Webbplatsen som kör][WebSiteRunningBlade]
   
    För att verifiera webbappen är aktiv, kan du klicka på den **Bläddra** ikon för att visa sidan.

Därefter skapar du en hybridanslutning och BizTalk-tjänst för webbprogrammet.

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. Skapa en Hybridanslutning och BizTalk-tjänst
1. Tillbaka i portalen går du till inställningar och klickar på **nätverk** > **konfigurera slutpunkter för din hybridanslutning**.
   
    ![Hybridanslutningar][CreateHCHCIcon]
2. Klicka på bladet Hybrid anslutningar **Lägg till** > **ny hybridanslutning**.
3. På den **skapa hybridanslutning** bladet:
   
   * För **namn**, ange ett namn för anslutningen.
   * För **värdnamn**, ange namnet på värddatorn för SQL Server.
   * För **Port**, ange 1433 (standardporten för SQL Server).
   * Klicka på **BizTalk Service** > **ny BizTalk-tjänst** och ange ett namn för BizTalk-tjänst.
     
     ![Skapa en hybridanslutning][TwinCreateHCBlades]
4. Klicka på **OK** två gånger.
   
    När processen har slutförts i **meddelanden** område blinkar en grön **lyckade** och **hybridanslutning** ny hybridanslutning med status som visas i bladet **inte ansluten**.
   
    ![En hybridanslutning skapas][CreateHCOneConnectionCreated]

Du har nu slutfört en viktig del av infrastrukturen för hybrid-anslutning. Därefter skapar du en motsvarande typ lokalt.

<a name="InstallHCM"></a>

## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D. Installera lokal Hybridanslutningshanterare för att slutföra anslutningen
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Anslutningen hybridinfrastruktur har slutförts, skapar du ett program som använder den.

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E. Skapa ett grundläggande webbprojekt i ASP.NET, redigera anslutningssträng för databasen och köra projektet lokalt
### <a name="create-a-basic-aspnet-project"></a>Skapa ett grundläggande ASP.NET-projekt
1. I Visual Studio på den **filen** menyn skapa ett nytt projekt:
   
    ![Nytt projekt i Visual Studio][HCVSNewProject]
2. I den **mallar** avsnitt i den **nytt projekt** markerar **Web** och välj **ASP.NET-webbprogram**, och klicka sedan på **OK**.
   
    ![Välj ASP.NET-webbprogram][HCVSChooseASPNET]
3. I den **nytt ASP.NET-projekt** dialogrutan Välj **MVC**, och klicka sedan på **OK**.
   
    ![Välja MVC][HCVSChooseMVC]
4. När projektet har skapats visas appen viktig information på sidan. Kör inte webbprojektet ännu.
   
    ![Viktigt-sida][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>Redigera anslutningssträng för databasen för programmet
I det här steget kan redigera du den anslutningssträng som talar om programmet var du hittar den lokala SQL Server Express-databasen. Anslutningssträngen är i programmets Web.config-fil som innehåller konfigurationsinformation för programmet.

> [!NOTE]
> För att säkerställa att programmet använder databasen som du skapade i SQL Server Express och inte det i Visual Studio standard LocalDB, är det viktigt att du har slutfört det här steget innan du kör projektet.
> 
> 

1. Dubbelklicka på filen Web.config i Solution Explorer.
   
    ![Web.config][HCVSChooseWebConfig]
2. Redigera den **connectionStrings** avsnittet att peka till SQL Server-databasen på din lokala dator enligt syntaxen i följande exempel:
   
    ![Anslutningssträng][HCVSConnectionString]
   
    Tänk på följande när du skriver anslutningssträngen:
   
   * Om du ansluter till en namngiven instans i stället för en standardinstans (till exempel YourServer\SQLEXPRESS), måste du konfigurera SQL Server för att använda statiska portar. Information om hur du konfigurerar statiska portar finns [hur du konfigurerar SQL Server för att lyssna på en viss port](http://support.microsoft.com/kb/823938). Som standard använder UDP- och dynamiska portar, vilket inte stöds av Hybridanslutningar för namngivna instanser.
   * Vi rekommenderar att du anger porten (1433 som standard, som visas i exemplet) i anslutningssträngen så att du kan kontrollera att dina lokala SQL Server har TCP aktiverad och använder rätt port.
   * Kom ihåg att använda SQL Server-autentisering för att ansluta, ange användar-ID och lösenord i anslutningssträngen.
3. Klicka på **spara** i Visual Studio för att spara filen Web.config.

### <a name="run-the-project-locally-and-register-a-new-user"></a>Köra projektet lokalt och registrera en ny användare
1. Nu kan köra lokalt webbprojektet nya genom att klicka på bläddringsknappen under felsökning. Det här exemplet använder Internet Explorer.
   
    ![Kör projektet][HCVSRunProject]
2. I övre högra standardwebbsidan, väljer **registrera** att registrera ett nytt konto:
   
    ![Registrera ett nytt konto][HCVSRegisterLocally]
3. Ange ett användarnamn och lösenord:
   
    ![Ange användarnamn och lösenord][HCVSCreateNewAccount]
   
    Detta skapar automatiskt en databas på den lokala SQL-servern som innehåller information om gruppmedlemskap för ditt program. En av tabellerna (**dbo. AspNetUsers**) innehåller web app autentiseringsuppgifter med de som du har angett. Den här tabellen visas senare under kursen.
4. Stäng webbläsarfönstret på webbsidan som standard. Detta hindrar program i Visual Studio.

Du är nu redo för nästa steg, vilket är att publicera program till Azure och testa den.

<a name="PubNTest"></a>

## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F. Publicera webbappen till Azure och testa den
Nu kan du publicera programmet till din App Service webbapp och sedan testa den för att se hur hybridanslutning som du tidigare har konfigurerat används för att ansluta ditt webbprogram till databasen på den lokala datorn.

### <a name="publish-the-web-application"></a>Publicera webbprogrammet
1. Du kan ladda ned din publiceringsprofil för Apptjänst-webbapp i Azure Portal. Klicka på bladet för din webbapp **Get publiceringsprofil**, och sedan spara filen på datorn.
   
    ![Hämta Publicera profil][PortalDownloadPublishProfile]
   
    Därefter importerar du den här filen till ditt webbprogram för Visual Studio.
2. Högerklicka på projektnamnet i Solution Explorer i Visual Studio och välj **publicera**.
   
    ![Välj publicera][HCVSRightClickProjectSelectPublish]
3. I den **Publicera webbplats** dialogrutan på den **profil** , Välj **importera**.
   
    ![Importera][HCVSPublishWebDialogImport]
4. Bläddra till din hämtade publiceringsprofilen markerar du den och klicka sedan på **OK**.
   
    ![Bläddra till profil][HCVSBrowseToImportPubProfile]
5. Din publiceringsinformation har importerats och visas på den **anslutning** fliken i dialogrutan.
   
    ![Klicka på Publicera][HCVSClickPublish]
   
    Klicka på **Publicera**.
   
    När publiceringen är klar startar webbläsaren och visar nu välkända ASP.NET-programmet--förutom att det är nu live i Azure-molnet!

Sedan använder du webbprogrammet live för att visa dess Hybridanslutning i åtgärden.

### <a name="test-the-completed-web-application-on-azure"></a>Testa det färdiga webbprogrammet på Azure
1. Högst upp höger på din webbsida på Azure, Välj **logga in**.
   
    ![Testa inloggningen][HCTestLogIn]
2. Din App Service web-appen är nu ansluten till ditt webbprogram medlemskap databas på den lokala datorn. Kontrollera detta genom att logga in med samma autentiseringsuppgifter som du angav tidigare i den lokala databasen.
   
    ![Hello hälsning][HCTestHelloContoso]
3. Om du vill testa ytterligare en ny hybridanslutning, logga ut från ditt webbprogram för Azure och registrera som en annan användare. Ange ett nytt användarnamn och lösenord och klicka sedan på **registrera**.
   
    ![Testa registrera en annan användare][HCTestRegisterRelecloud]
4. Öppna SQL Management Studio på den lokala datorn för att kontrollera att den nya användarens autentiseringsuppgifter har lagrats i den lokala databasen via hybridanslutningen. I Object Explorer, expandera den **MembershipDB** databasen och expandera sedan **tabeller**. Högerklicka på den **dbo. AspNetUsers** medlemskap tabell och välj **Välj de 1000 översta raderna** att visa resultat.
   
    ![Visa resultaten][HCTestSSMSTree]
5. Medlemskap i lokal tabellen visar nu båda kontona - konto som du skapade lokalt och ett som du skapade i Azure-molnet. Det konto som du skapade i molnet har sparats till den lokala databasen via funktionen för Azures Hybridanslutning.
   
    ![Registrerade användare i en lokal databas][HCTestShowMemberDb]

Du har nu skapats och distribuerats ASP.NET-webbprogram som använder en hybridanslutning mellan en webbapp i Azure-molnet och en lokal SQL Server-databas. Grattis!

## <a name="see-also"></a>Se även
[Översikt över hybrid-anslutningar](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist introducerar hybridanslutningar (Channel 9 video)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Översikt över hybrid-anslutningar](/services/biztalk-services/)

[BizTalk-tjänst: Instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutning flikar](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Skapa en verklig Hybridmoln med sömlös programmet Portability (Channel 9 video)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service](web-sites-hybrid-connection-get-started.md)

[Översikt över ASP.NET-identitet](http://www.asp.net/identity)

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
