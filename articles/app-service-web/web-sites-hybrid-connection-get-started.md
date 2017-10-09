---
title: "aaaAccess lokala resurser genom att använda hybridanslutningar i Azure App Service"
description: "Skapa en anslutning mellan en webbapp i Azure App Service och en lokal resurs som använder en statisk TCP-port"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: a46ed26b-df8e-4fc3-8e05-2d002a6ee508
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: cephalin
ms.openlocfilehash: de7c57b94f4bd6250a93757817178e8455daae4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a>Få tillgång till lokala resurser med hybridanslutningar i Azure App Service
Du kan ansluta en Azure App Service app tooany lokal resurs som använder en statisk TCP-port, till exempel SQL Server, MySQL, http-webb-API: er och de flesta anpassade webbtjänster. Den här artikeln beskrivs hur du toocreate en hybridanslutning mellan App Service och en lokal SQL Server-databas.

> [!NOTE]
> hello Web Apps del av hello Hybridanslutningar funktionen är endast tillgänglig i hello [Azure Portal](https://portal.azure.com). toocreate en anslutning i BizTalk-tjänst finns [Hybridanslutningar](http://go.microsoft.com/fwlink/p/?LinkID=397274). 
> 
> Det här innehållet gäller även tooMobile appar i Azure App Service. 
> 
> 

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. För en kostnadsfri prenumeration finns [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/). 
  
    Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
* toouse en lokal SQL Server eller SQL Server Express-databas med en hybridanslutning, måste TCP/IP toobe aktiverad på en statisk port. En standardinstans rekommenderas på SQL Server eftersom den använder statisk port 1433. Information om hur du installerar och konfigurerar SQL Server Express för användning med hybridanslutningar finns [Anslut tooan lokala SQL Server från en Azure-webbplats med Hybridanslutningar](http://go.microsoft.com/fwlink/?LinkID=397979).
* hello-dator som du installerar hello lokal Hybridanslutningshanterare agent beskrivs senare i den här artikeln:
  
  * Måste vara kan tooconnect tooAzure via port 5671
  * Måste vara kan tooreach hello *värdnamn*:*portnumber* på din lokala resursen. 

> [!NOTE]
> hello stegen i den här artikeln förutsätter att du använder hello webbläsaren från hello-dator som är värd för hello lokalt hybrid anslutning agent.
> 
> 

## <a name="create-a-web-app-in-hello-azure-portal"></a>Skapa en webbapp i hello Azure-portalen
> [!NOTE]
> Om du redan har skapat ett webbprogram eller en mobilappsserverdel i hello Azure-portalen som du vill toouse för den här självstudiekursen, du kan gå vidare för[skapa en Hybrid-anslutning och en BizTalk Service](#CreateHC) och starta därifrån.
> 
> 

1. I hello övre vänstra hörnet hello [Azure Portal](https://portal.azure.com), klickar du på **ny** > **webb + mobilt** > **Web App**.
   
    ![Ny webbapp][NewWebsite]
2. På hello **webbapp** bladet anger en URL och klickar på **skapa**. 
   
    ![Namn på webbplats][WebsiteCreationBlade]
3. Hello webbprogram skapas och dess web app bladet visas efter en liten stund. hello bladet är en lodrätt rullningsbara instrumentpanel där du kan hantera webbplatsen.
   
    ![Webbplatsen som kör][WebSiteRunningBlade]
4. tooverify hello webbplatsen är aktiv, kan du klicka på hello **Bläddra** ikonen toodisplay hello standardsida.
   
    ![Klicka på Bläddra toosee ditt webbprogram][Browse]
   
    ![Standardsida web app][DefaultWebSitePage]

Därefter skapar du en hybridanslutning och BizTalk-tjänst för hello webbprogrammet.

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a>Skapa en Hybridanslutning och BizTalk-tjänst
1. I ditt webb-app-blad klickar du på **alla inställningar** > **nätverk** > **konfigurera slutpunkter för din hybridanslutning**.
   
    ![Hybridanslutningar][CreateHCHCIcon]
2. Klicka på hello Hybrid anslutningar bladet **Lägg till**.
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. Hej **lägga till en hybridanslutning** blad öppnas.  Eftersom det är ditt första hybridanslutning hello **ny hybridanslutning** är förvalt och hello **skapa hybridanslutning** blad öppnas för dig.
   
    ![Skapa en hybridanslutning][TwinCreateHCBlades]
   
    På hello **skapa hybrid anslutning bladet**:
   
   * För **namn**, ange ett namn för hello-anslutning.
   * För **värdnamn**, ange hello namnet på hello lokala dator som är värd för din resurs.
   * För **Port**, ange hello portnummer som använder lokal resurs (1433 för en SQL Server-standardinstans).
   * Klicka på **Biz kontakta Service**
4. Hej **skapa BizTalk-tjänst** blad öppnas. Ange ett namn för hello BizTalk-tjänst och klicka sedan på **OK**.
   
    ![Skapa BizTalk-tjänst][CreateHCCreateBTS]
   
    Hej **skapa BizTalk-tjänst** blad stängs och du kommer tillbaka toohello **skapa hybridanslutning** bladet.
5. Klicka på hello skapa hybrid anslutning bladet **OK**. 
   
    ![Klicka på OK][CreateBTScomplete]
6. När hello är klart informerar hello meddelandefältet i hello Portal dig om att hello anslutningen har skapats.
   
    <!--- TODO
   
    Everything fails at this step. I can't create a BizTalk service in hello dogfood portal. I switch toohello classic portal
    (full portal) and created hello BizTalk service but it doesn't seem toolet you connnect them - When you finish the
    Create hybrid conn step, you get hello following error
    Failed toocreate hybrid connection RelecIoudHC. hello 
    resource type could not be found in hello namespace 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
   
    hello error indicates it couldn't find hello type, not hello instance.
    ![Success notification][CreateHCSuccessNotification]
    -->
7. På hello webbapps blad hello **hybridanslutningar** ikon visas nu att 1 hybridanslutningen har skapats.
   
    ![En hybridanslutning skapas][CreateHCOneConnectionCreated]

Du har nu slutfört en viktig del av hello molninfrastruktur hybrid-anslutning. Därefter skapar du en motsvarande typ lokalt.

<a name="InstallHCM"></a>

## <a name="install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a>Installera hello lokal Hybridanslutningshanterare toocomplete hello anslutning
1. Klicka på hello webbapps blad **alla inställningar** > **nätverk** > **konfigurera slutpunkter för din hybridanslutning**. 
   
    ![Ikon för hybrid-anslutningar][HCIcon]
2. På hello **hybridanslutningar** bladet, hello **Status** för hello nyligen har lagt till slutpunkten visar **inte ansluten**. Klicka på hello anslutning tooconfigure den.
   
    ![Inte ansluten][NotConnected]
   
    hello Hybrid anslutning blad öppnas.
   
    ![NotConnectedBlade][NotConnectedBlade]
3. På hello-bladet klickar du på **lyssnare installationsprogrammet**.
   
    ![Klicka på lyssnaren installationsprogrammet][ClickListenerSetup]
4. Hej **Hybrid anslutningsegenskaper** blad öppnas. Under **på lokal Hybridanslutningshanterare**, Välj **Klicka här tooinstall**.
   
    ![Klicka på tooinstall][ClickToInstallHCM]
5. Hello programmet köras varning dialogrutan säkerhet, Välj **kör** toocontinue.
   
    ![Välj Kör toocontinue][ApplicationRunWarning]
6. I hello **User Account Control** dialogrutan Välj **Ja**.
   
   ![Välj Ja][UAC]
7. Hej Hybridanslutningshanteraren hämtas och installeras automatiskt. 
   
    ![Installera][HCMInstalling]
8. När hello-installationen har slutförts klickar du på **Stäng**.
   
    ![Klicka på Stäng][HCMInstallComplete]
   
    På hello **hybridanslutningar** bladet, hello **Status** kolumnen visar nu **ansluten**. 
   
    ![Anslutna Status][HCStatusConnected]

Nu hello hybrid anslutning infrastrukturen är klar kan skapa du en hybridprogram som använder den. 

> [!NOTE]
> hello följande avsnitt visar hur toouse en hybridanslutning med ett projekt för Mobile Apps .NET-serverdel.
> 
> 

## <a name="configure-hello-mobile-app-net-backend-project-tooconnect-toohello-sql-server-database"></a>Konfigurera hello Mobile App .NET-serverdel projektet tooconnect toohello SQL Server-databas
I App Service är en Mobile Apps .NET serverdelsprojektet bara en ASP.NET webbapp med en ytterligare Mobile Apps-SDK har installerats och initierats. toouse ditt webbprogram som en Mobile Apps-serverdel måste du [ladda ned och initiera hello Mobile Apps .NET-serverdel SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).  

För Mobilappar du även behöver toodefine en anslutningssträng för hello lokala-databasen och ändra hello backend toouse den här anslutningen. 

1. Leta upp hello i Solution Explorer i Visual Studio, öppna hello Web.config-filen för din mobila App .NET-serverdel **connectionStrings** lägger du till en ny post i SqlClient som hello nedan, vilken punkter toohello lokalt SQL Server-databas:
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    Kom ihåg tooreplace `<**secure_password**>` i den här strängen med hello lösenordet du skapade för *HybridConnectionLogin*.
2. Klicka på **spara** i Visual Studio toosave hello Web.config-fil.
   
   > [!NOTE]
   > Den här anslutningsinställningen används när körs på hello lokala datorn. När den körs i Azure är den här inställningen åsidosatts av hello anslutningsinställningen som definierats i hello-portalen.
   > 
   > 
3. Expandera hello **modeller** mapp och öppna hello modellen datafil som slutar i *Context.cs*.
4. Ändra hello **DbContext** instans-konstruktorn toopass hello värdet `OnPremisesDBConnection` toohello grundläggande **DbContext** konstruktor, liknande toohello följande utdrag:
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    hello-tjänsten använder nu hello ny anslutning toohello SQL Server-databas.

## <a name="update-hello-mobile-app-backend-toouse-hello-on-premises-connection-string"></a>Uppdatera hello Mobilapp backend toouse hello lokala anslutningssträngen
Därefter behöver du tooadd en appinställning för den här nya anslutningssträngen så att den kan användas från Azure.  

1. Tillbaka i hello [Azure-portalen](https://portal.azure.com) i hello web app backend-kod för Mobilappen, klickar du på **alla inställningar**, sedan **programinställningar**.
2. I hello **Web app-inställningar** bladet rulla nedåt för**anslutningssträngar** och lägga till en ny **SQL Server** anslutningssträngen med namnet `OnPremisesDBConnection` med ett värde som `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.
   
    Ersätt `<**secure_password**>` med hello säkert lösenord för lokala databasen.
   
    ![Anslutningssträngen för lokala-databasen](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. Tryck på **spara** toosave hello hybridanslutning och anslutningssträngen du nyss skapade.

Nu kan du publicera hello serverprojekt och testa hello ny anslutning med dina befintliga Mobile Apps-klienter. Data läses och skrivs toohello lokal databas med hjälp av hello hybridanslutning.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Nästa steg
* Information om hur du skapar ett ASP.NET-webbprogram som använder en hybridanslutning finns [Anslut tooan lokala SQL Server från en Azure-webbplats med Hybridanslutningar](http://go.microsoft.com/fwlink/?LinkID=397979). 

### <a name="additional-resources"></a>Ytterligare resurser
[Översikt över hybrid-anslutningar](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist introducerar hybridanslutningar (Channel 9 video)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Webbplats för hybrid-anslutningar](https://azure.microsoft.com/services/biztalk-services/)

[BizTalk-tjänst: Instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutning flikar](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Skapa en verklig Hybridmoln med sömlös programmet Portability (Channel 9 video)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Ansluta tooan lokala SQL Server från Azure Mobile Services med hjälp av Hybridanslutningar (Channel 9 video)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[New]:./media/web-sites-hybrid-connection-get-started/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-get-started/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-get-started/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-get-started/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-get-started/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-get-started/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-get-started/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-get-started/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-get-started/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-get-started/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-get-started/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-get-started/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-get-started/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-get-started/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-get-started/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-get-started/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-get-started/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-get-started/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-get-started/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-get-started/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-get-started/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-get-started/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-get-started/D10HCStatusConnected.png
