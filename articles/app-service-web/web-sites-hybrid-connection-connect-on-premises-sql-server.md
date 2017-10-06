---
title: "aaaConnect tooon lokal SQL Server från en webbapp i Azure App Service med Hybridanslutningar"
description: Skapa en webbapp i Microsoft Azure och koppla den tooan lokala SQL Server-databas
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
ms.openlocfilehash: 2e8f8f7e0b9733cfb0433697615faba4358c6023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Ansluta tooon lokal SQL Server från en webbapp i Azure App Service med Hybridanslutningar
Hybridanslutningar kan ansluta [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps tooon lokala resurser som använder en statisk TCP-port. Resurser som stöds är Microsoft SQL Server, MySQL, http-webb-API: er, användning och de flesta anpassade webbtjänster.

I den här kursen får du lära dig hur toocreate en App Service web app i hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), ansluta hello web app tooyour lokala lokala SQL Server-databas med hjälp av hello ny Hybridanslutning funktion, skapa en enkel ASP.NET program som ska använda hello hybridanslutning och distribuera hello programmet toohello App Service webbapp. hello slutförts webbapp på Azure lagrar autentiseringsuppgifter i en databas för medlemskap som är lokalt. hello självstudiekursen förutsätter några tidigare erfarenheter med hjälp av Azure eller ASP.NET.

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> hello Web Apps del av hello Hybridanslutningar funktionen är endast tillgänglig i hello [Azure Portal](https://portal.azure.com). toocreate en anslutning i BizTalk-tjänst finns [Hybridanslutningar](http://go.microsoft.com/fwlink/p/?LinkID=397274).  
> 
> 

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande produkter. Alla är tillgängliga i ledigt versioner, så du kan börja utveckla för Azure och helt kostnadsfritt.

* **Azure-prenumeration** – för en kostnadsfri prenumeration finns [kostnadsfri utvärderingsversion av Azure](/pricing/free-trial/).
* **Visual Studio 2013** -toodownload en kostnadsfri utvärderingsversion av Visual Studio 2013 finns [Visual Studio-hämtningar](http://www.visualstudio.com/downloads/download-visual-studio-vs). Installera detta innan du fortsätter.
* **Microsoft .NET Framework 3.5 servicepack 1** -om operativsystemet är Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 eller Windows Server 2008 R2, kan du aktivera det på Kontrollpanelen > program och funktioner > Aktivera Windows-funktioner på eller stänga av. Annars kan du hämta det från hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).
* **SQL Server 2014 Express med verktyg** -hämta Microsoft SQL Server Express kostnadsfritt på hello [sidan Microsoft Web Platform databas](http://www.microsoft.com/web/platform/database.aspx). Välj hello **Express** (inte LocalDB) version. Hej **Express med verktyg** version omfattar SQL Server Management Studio, som du ska använda i den här självstudiekursen.
* **SQL Server Management Studio Express** - detta ingår i SQL Server 2014 Express hello verktyg hämta som nämns ovan, men om du behöver tooinstall den separat, du kan hämta och installera det från hello [SQL Server Express hämtningssidan](http://www.microsoft.com/web/platform/database.aspx).

hello kursen förutsätter att du har en Azure-prenumeration, att du har installerat Visual Studio 2013 och att du har installerat eller aktiverat .NET Framework 3.5. hello kursen visar hur tooinstall SQL Server 2014 Express i en konfiguration som fungerar bra med hello Azure Hybridanslutningar funktion (en standardinstans med en statisk TCP-port). Hämta SQL Server 2014 Express med verktyg hello-plats som nämns ovan om du inte installerade SQL Server innan du startar hello kursen.

### <a name="notes"></a>Anteckningar
toouse en lokal SQL Server eller SQL Server Express-databas med en hybridanslutning, måste TCP/IP toobe aktiverad på en statisk port. Standardinstanser på SQL Server använder statisk port 1433, inte namngivna instanser.

hello-dator som du installerar hello lokal Hybridanslutningshanterare agent:

* Måste ha utgående anslutning tooAzure över:

| Port | Varför |
| --- | --- |
| 80 |**Krävs** för HTTP-porten för valideringen av servercertifikatet och eventuellt för dataanslutning. |
| 443 |**Valfria** för dataanslutning. Om utgående anslutning too443 inte är tillgänglig används TCP-port 80. |
| 5671 och 9352 |**Rekommenderade** men valfritt för dataanslutning. Observera att det här läget vanligtvis ger högre genomströmning. Om utgående anslutning toothese portar är tillgänglig, används TCP-port 443. |

* Måste vara kan tooreach hello *värdnamn*:*portnumber* på din lokala resursen.

hello stegen i den här artikeln förutsätter att du använder hello webbläsaren från hello-dator som är värd för hello lokalt hybrid anslutning agent.

Om du redan har SQL Server installeras i en konfiguration och i en miljö som uppfyller villkoren för hello som beskrivs ovan kan du gå vidare och börja med [och skapa en SQL Server-databas lokalt](#CreateSQLDB).

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. Installera SQL Server Express, aktivera TCP/IP och skapa en SQL Server-databas lokalt
Det här avsnittet visar hur tooinstall SQL Server Express, aktivera TCP/IP och skapa en databas så att webbprogrammet fungerar med hello Azure-portalen.

### <a name="install-sql-server-express"></a>Installera SQL Server Express
1. tooinstall SQL Server Express, kör hello **SQLEXPRWT_x64_ENU.exe** eller **SQLEXPR_x86_ENU.exe** filen som du hämtade. hello SQL Server Installationscenter guiden visas.
   
    ![SQL Server-installation][SQLServerInstall]
2. Välj **fristående nya SQL Server-installation eller Lägg till en befintlig installation av funktioner tooan**. Följ instruktionerna för hello, acceptera hello standardalternativen och inställningar, tills du får toohello **instanskonfiguration** sidan.
3. På hello **instanskonfiguration** väljer **standardinstansen**.
   
    ![Välj standardinstansen][ChooseDefaultInstance]
   
    Som standard lyssnar hello standardinstansen av SQL Server för begäranden från SQL Server-klienter på statisk port 1433, vilket är vilka hello Hybridanslutningar funktionen kräver. Namngivna instanser använder dynamiska portar och UDP, vilket inte stöds av Hybridanslutningar.
4. Acceptera hello standardinställningar på hello **serverkonfiguration** sidan.
5. På hello **konfiguration av databasmotor** sidan under **autentiseringsläge**, Välj **blandat läge (SQL Server-autentisering och Windows-autentisering)**, och tillhandahålla ett lösenord.
   
    ![Välj blandat läge][ChooseMixedMode]
   
    I den här självstudiekursen kommer du att använda SQL Server-autentisering. Vara säker på att tooremember hello lösenord som du anger eftersom du behöver senare.
6. Gå igenom hello resten av hello guiden toocomplete hello installation.

### <a name="enable-tcpip"></a>Aktivera TCP/IP
tooenable TCP/IP, ska du använda SQL Server Configuration Manager, som installerades när du installerade SQL Server Express. Gör så hello i [aktivera nätverksprotokoll för TCP/IP för SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) innan du fortsätter.

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a>Skapa en SQL Server-databas lokalt
Ditt webbprogram för Visual Studio kräver en databas för medlemskap som kan nås av Azure. Detta kräver en SQL Server eller SQL Server Express-databas (inte hello LocalDB-databas som hello MVC-mallen använder som standard), så ska du skapa hello medlemskapsdatabasen nästa.

1. Anslut toohello SQL Server som du precis har installerat i SQL Server Management Studio. (Om hello **ansluta tooServer** dialogrutan inte visas automatiskt, navigera för**Object Explorer** i hello vänstra rutan klickar du på **Anslut**, och klicka sedan på **Database Engine**.) ![Ansluta tooServer][SSMSConnectToServer]
   
    För **servertyp**, Välj **databasmotorn**. För **servernamn**, kan du använda **localhost** eller hello namnet på hello-dator som du använder. Välj **SQL Server-autentisering**, och sedan logga in med användarnamnet för hello sa och hello lösenord som du skapade tidigare.
2. toocreate en ny databas med hjälp av SQL Server Management Studio, högerklicka på **databaser** i Object Explorer och klicka sedan på **ny databas**.
   
    ![Skapa en ny databas][SSMScreateNewDB]
3. I hello **ny databas** dialogrutan Ange MembershipDB för hello databasens namn och klicka sedan på **OK**.
   
    ![Ange databasnamn][SSMSprovideDBname]
   
    Observera att du inte göra några ändringar toohello databasen nu. information om medlemskap i hello läggas till automatiskt senare av ditt webbprogram när den körs.
4. I Object Explorer, om du expanderar **databaser**, ser du att hello medlemskapsdatabasen har skapats.
   
    ![MembershipDB skapas][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-hello-azure-portal"></a>B. Skapa en webbapp i hello Azure-portalen
> [!NOTE]
> Om du redan har skapat ett webbprogram i hello Azure-portalen som du vill toouse för den här självstudiekursen, du kan gå vidare för[skapa en Hybrid-anslutning och en BizTalk Service](#CreateHC) och fortsätta därifrån.
> 
> 

1. I hello [Azure Portal](https://portal.azure.com), klickar du på **ny** > **webb + mobilt** > **webbapp**.
   
    ![Knappen Nytt][New]
2. Konfigurera ditt webbprogram och klicka sedan på **skapa**.
   
    ![Namn på webbplats][WebsiteCreationBlade]
3. Hello webbprogram skapas och dess web app bladet visas efter en liten stund. hello bladet är en lodrätt rullningsbara instrumentpanel där du kan hantera ditt webbprogram.
   
    ![Webbplatsen som kör][WebSiteRunningBlade]
   
    tooverify hello webbprogrammet är aktiv, kan du klicka på hello **Bläddra** ikonen toodisplay hello standardsida.

Därefter skapar du en hybridanslutning och BizTalk-tjänst för hello webbprogrammet.

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. Skapa en Hybridanslutning och BizTalk-tjänst
1. Tillbaka i hello Portal, gå toosettings och klicka på **nätverk** > **konfigurera slutpunkter för din hybridanslutning**.
   
    ![Hybridanslutningar][CreateHCHCIcon]
2. Klicka på hello Hybrid anslutningar bladet **Lägg till** > **ny hybridanslutning**.
3. På hello **skapa hybridanslutning** bladet:
   
   * För **namn**, ange ett namn för hello-anslutning.
   * För **värdnamn**, ange hello datornamnet på värddatorn för SQL Server.
   * För **Port**, ange 1433 (hello-standardporten för SQL Server).
   * Klicka på **BizTalk Service** > **ny BizTalk-tjänst** och ange ett namn för hello BizTalk-tjänst.
     
     ![Skapa en hybridanslutning][TwinCreateHCBlades]
4. Klicka på **OK** två gånger.
   
    När hello processen har slutförts hello **meddelanden** område blinkar en grön **lyckade** och hello **hybridanslutning** bladet visar hello ny hybridanslutning med Hej status som **inte ansluten**.
   
    ![En hybridanslutning skapas][CreateHCOneConnectionCreated]

Du har nu slutfört en viktig del av hello molninfrastruktur hybrid-anslutning. Därefter skapar du en motsvarande typ lokalt.

<a name="InstallHCM"></a>

## <a name="d-install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a>D. Installera hello lokal Hybridanslutningshanterare toocomplete hello anslutning
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Nu hello hybrid anslutning infrastrukturen är klar skapar du ett program som använder den.

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-hello-database-connection-string-and-run-hello-project-locally"></a>E. Skapa ett grundläggande webbprojekt i ASP.NET, redigera hello databasanslutningssträng och kör hello projektet lokalt
### <a name="create-a-basic-aspnet-project"></a>Skapa ett grundläggande ASP.NET-projekt
1. I Visual Studio på hello **filen** menyn skapa ett nytt projekt:
   
    ![Nytt projekt i Visual Studio][HCVSNewProject]
2. I hello **mallar** avsnitt i hello **nytt projekt** markerar **Web** och välj **ASP.NET-webbprogram**, och klicka sedan på  **OK**.
   
    ![Välj ASP.NET-webbprogram][HCVSChooseASPNET]
3. I hello **nytt ASP.NET-projekt** dialogrutan Välj **MVC**, och klicka sedan på **OK**.
   
    ![Välja MVC][HCVSChooseMVC]
4. Hello programmet viktigt-sidan visas när hello projektet har skapats. Kör inte hello webbprojekt ännu.
   
    ![Viktigt-sida][HCVSReadmePage]

### <a name="edit-hello-database-connection-string-for-hello-application"></a>Redigera hello databasanslutningssträng för hello program
I det här steget kan du redigera hello anslutningssträngen som talar om ditt program där toofind dina lokala SQLServer Express databasen. hello anslutningssträngen är i hello programmets Web.config-fil som innehåller konfigurationsinformation för hello-programmet.

> [!NOTE]
> tooensure som används för programmet hello-databas som du skapade i SQL Server Express och inte hello något i Visual Studio standard LocalDB, är det viktigt att du har slutfört det här steget innan du kör projektet.
> 
> 

1. I Solution Explorer dubbelklickar du på hello Web.config-filen.
   
    ![Web.config][HCVSChooseWebConfig]
2. Redigera hello **connectionStrings** avsnittet toopoint toohello SQL Server-databas på den lokala datorn hello syntaxen i följande exempel hello:
   
    ![Anslutningssträng][HCVSConnectionString]
   
    När du skriver hello anslutningssträngen, Kom ihåg hello följande:
   
   * Om du ansluter tooa namngivna instans i stället för en standardinstans (till exempel YourServer\SQLEXPRESS), måste du konfigurera din SQL Server-toouse statiska portar. Information om hur du konfigurerar statiska portar finns [hur tooconfigure SQL Server toolisten på en viss port](http://support.microsoft.com/kb/823938). Som standard använder UDP- och dynamiska portar, vilket inte stöds av Hybridanslutningar för namngivna instanser.
   * Vi rekommenderar att du anger hello port (1433 som standard, som visas i exemplet hello) på hello anslutningssträngen så att du kan vara säker på att dina lokala SQL Server har TCP aktiverad och använder hello rätt port.
   * Kom ihåg toouse SQL Server-autentisering tooconnect, att ange hello användar-ID och lösenord i anslutningssträngen.
3. Klicka på **spara** i Visual Studio toosave hello Web.config-fil.

### <a name="run-hello-project-locally-and-register-a-new-user"></a>Kör hello projektet lokalt och registrera en ny användare
1. Nu kan köra lokalt webbprojektet nya genom att klicka på bläddringsknappen hello under felsökning. Det här exemplet använder Internet Explorer.
   
    ![Kör projektet][HCVSRunProject]
2. Välj på hello övre högra hörnet av hello standardwebbsidan, **registrera** tooregister ett nytt konto:
   
    ![Registrera ett nytt konto][HCVSRegisterLocally]
3. Ange ett användarnamn och lösenord:
   
    ![Ange användarnamn och lösenord][HCVSCreateNewAccount]
   
    Detta skapar automatiskt en databas på den lokala SQL-servern som innehåller information om hello gruppmedlemskap för ditt program. En av tabellerna hello (**dbo. AspNetUsers**) innehåller web app användarautentiseringsuppgifter som hello som du angav. Den här tabellen visas senare i självstudiekursen hello.
4. Stäng webbläsarfönstret för hello av hello standardwebbsidan. Detta stoppar hello program i Visual Studio.

Du är nu redo för nästa steg i hello, vilket är toopublish hello programmet tooAzure och testa den.

<a name="PubNTest"></a>

## <a name="f-publish-hello-web-application-tooazure-and-test-it"></a>F. Publicera hello web application tooAzure och testa den
Nu kan du publicera dina program tooyour App Service web-appen och sedan testa den toosee hur hello hybridanslutning som du tidigare har konfigurerat är att använda tooconnect web app toohello databasen på den lokala datorn.

### <a name="publish-hello-web-application"></a>Publicera hello-webbprogram
1. Du kan ladda ned din publiceringsprofil för hello Apptjänst-webbapp i hello Azure-portalen. Hello bladet för ditt webbprogram, klicka på **Get publiceringsprofil**, och sedan spara hello filen tooyour dator.
   
    ![Hämta Publicera profil][PortalDownloadPublishProfile]
   
    Därefter importerar du den här filen till ditt webbprogram för Visual Studio.
2. Högerklicka på hello projektnamnet i Solution Explorer i Visual Studio och välj **publicera**.
   
    ![Välj publicera][HCVSRightClickProjectSelectPublish]
3. I hello **Publicera webbplats** dialogrutan på hello **profil** , Välj **importera**.
   
    ![Importera][HCVSPublishWebDialogImport]
4. Bläddra tooyour hämtas publiceringsprofilen markerar du den och klicka sedan på **OK**.
   
    ![Bläddra tooprofile][HCVSBrowseToImportPubProfile]
5. Din publiceringsinformation har importerats och visar på hello **anslutning** fliken hello dialogrutan.
   
    ![Klicka på Publicera][HCVSClickPublish]
   
    Klicka på **Publicera**.
   
    När publiceringen är klar startar webbläsaren och visar nu välkända ASP.NET-programmet--förutom att den är nu aktiv i hello Azure-molnet!

Därefter använder du din live web application toosee dess Hybridanslutning i åtgärden.

### <a name="test-hello-completed-web-application-on-azure"></a>Testa hello slutförts webbprogram på Azure
1. Hello överst höger på din webbsida på Azure, Välj **logga in**.
   
    ![Testa inloggningen][HCTestLogIn]
2. Din App Service webbappen är nu ansluten tooyour webbprogrammets medlemskapsdatabasen på den lokala datorn. tooverify detta, logga in med samma autentiseringsuppgifter som du angett i hello lokal databas tidigare hello.
   
    ![Hello hälsning][HCTestHelloContoso]
3. toofurther testa nya hybridanslutningen, logga ut från ditt webbprogram för Azure och registrera som en annan användare. Ange ett nytt användarnamn och lösenord och klicka sedan på **registrera**.
   
    ![Testa registrera en annan användare][HCTestRegisterRelecloud]
4. tooverify att hello den nya användarens autentiseringsuppgifter har lagrats i den lokala databasen via din hybridanslutning Öppna SQL Management Studio på den lokala datorn. I Object Explorer, expandera hello **MembershipDB** databasen och expandera sedan **tabeller**. Högerklicka på hello **dbo. AspNetUsers** medlemskap tabell och välj **Välj de 1000 översta raderna** tooview hello resultat.
   
    ![Visa hello resultat][HCTestSSMSTree]
5. Medlemskap i lokal tabellen visar nu båda kontona - hello skapats lokalt och hello som du skapade i hello Azure-molnet. hello som du skapade i hello molnet har sparats tooyour lokala-databasen via funktionen för Azures Hybridanslutning.
   
    ![Registrerade användare i en lokal databas][HCTestShowMemberDb]

Du har nu skapats och distribuerats ASP.NET-webbprogram som använder en hybridanslutning mellan ett webbprogram i hello Azure-molnet och en lokal SQL Server-databas. Grattis!

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
