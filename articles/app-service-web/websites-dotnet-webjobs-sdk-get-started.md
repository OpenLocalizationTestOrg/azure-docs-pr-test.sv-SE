---
title: aaaCreate ett Webbjobb .NET i Azure App Service | Microsoft Docs
description: "Skapa en flernivåapp med ASP.NET MVC och Azure. hello front end körs i en webbapp i Azure App Service och hello tillbaka slutet körs som ett Webbjobb. hello appen använder Entity Framework, SQL Database och Azure storage-köer och blobbar."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: mollybos
ms.assetid: 99cb9917-483a-45f8-a98d-07d19c68c753
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/14/2017
ms.author: glenga
ms.openlocfilehash: d92fc4b81cc0d58f8e900f257e747af56d32b911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-webjob-in-azure-app-service"></a>Skapa ett .NET WebJob i Azure App Service
Den här kursen visar hur toowrite Platskod för en enkel ASP.NET MVC 5 flernivåapp som använder hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Hej syftet hello [WebJobs SDK](websites-webjobs-resources.md) är toosimplify hello koden du skriver för vanliga uppgifter som ett Webbjobb kan utföra, till exempel bild bearbetning, kö bearbetning, RSS aggregering, filunderhåll, och skicka e-post. Hej WebJobs SDK har inbyggda funktioner för att arbeta med Azure Storage- och Service Bus för schemalagda aktiviteter och hantera fel och för många andra vanliga scenarier. Dessutom har utformats för toobe extensible och det finns en [öppen källkod lagringsplatsen för tillägg](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

hello exempelprogrammet är en anslagstavla för annonser. Användarna kan ladda upp bilder för annonser och en backend-process konverterar hello bilder toothumbnails. hello ad sidan visar hello miniatyrer och hello ad informationssidan visar hello bilden. Här är en skärmbild:

![Annonslista](./media/websites-dotnet-webjobs-sdk-get-started/list.png)

Det här exempelprogrammet fungerar med [Azure köer](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) och [Azure BLOB](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage). hello kursen visar hur toodeploy hello program för[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) och [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).

## <a id="prerequisites"></a>Förhandskrav
hello kursen förutsätter att du vet hur toowork med [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projekt i Visual Studio.

hello kursen ursprungligen skrevs för Visual Studio 2013, men kan användas med senare versioner av Visual Studio. Om du använder Visual Studio 2015 eller 2017, notera att innan du kör programmet hello lokalt, måste du ändra hello `Data Source` tillhör hello SQL Server LocalDB-anslutningssträngen i hello Web.config och App.config filer från `Data Source=(localdb)\v11.0` för`Data Source=(LocalDb)\MSSQLLocalDB`.

> [!NOTE]
> <a name="note"></a>Du måste ha en Azure-konto toocomplete den här kursen:
>
> * Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): du får kredit att du kan använda tootry ut betald Azure-tjänster och även när de används, kan du hålla hello-konto och använda kostnadsfria Azure-tjänster, till exempel Websites. Ditt kreditkort kommer aldrig att debiteras, såvida du inte uttryckligen ändrar dina inställningar och be toobe debiteras.
> * Du kan [aktivera månatlig Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): din prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.
>
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
>
>

## <a id="learn"></a>Lär du dig
hello kursen visar hur toodo hello följande uppgifter:

* Aktivera datorn för Azure-utveckling genom att installera hello Azure SDK (endast för Visual Studio 2013 och 2015 användare).
* Skapa ett konsolprogram projekt som distribuerar automatiskt som en Azure-Webbjobb när du distribuerar hello associerade webbprojekt.
* Testa en WebJobs-SDK-serverdel lokalt på utvecklingsdatorn hello.
* Publicera ett program med en WebJobs backend tooa webbapp i App Service.
* Ladda upp filer och lagrar dem i hello Azure Blob-tjänsten.
* Använda hello Azure WebJobs SDK toowork med Azure Storage-köer och blobbar.

## <a id="contosoads"></a>Programarkitektur
hello exempelprogrammet använder hello [köcentriska mönster](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff belastningen hello processorintensiva arbetet med att skapa miniatyrbilder tooa backend-processen.

hello appen lagrar annonser i en SQL-databas med hjälp av Entity Framework Code First toocreate hello tabellerna och komma åt hello data. För varje ad hello databasen lagrar två URL: en för hello bilden och en för hello miniatyr.

![Annonstabell](./media/websites-dotnet-webjobs-sdk-get-started/adtable.png)

När en användare laddar upp en bild, hello webbprogram lagrar hello bilden i en [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), och den lagrar Hej ad i hello-databas med en URL som pekar toohello blob. AT hello samma time, skriver den ett meddelande tooan Azure kön. I en backend-process som körs som en Azure-Webbjobb, avsöker hello WebJobs SDK hello kön efter nya meddelanden. När ett nytt meddelande visas hello Webbjobb skapar en miniatyrbild för den bilden och uppdateringar hello miniatyrbildens URL-databasfält för den annonsen. Här är ett diagram som visar hur hello delar av programmet hello samverkar:

![Contoso Ads-arkitektur](./media/websites-dotnet-webjobs-sdk-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]  
hello självstudiekursen instruktioner gäller tooAzure SDK för .NET 2.7.1 eller senare.

## <a id="storage"></a>Skapa ett Azure Storage-konto
Ett Azure storage-konto innehåller resurser för att lagra kö-och blobbdata i hello molnet. Det används också av hello WebJobs SDK toostore loggningsdata för hello instrumentpanelen.

I ett riktigt program skapar du vanligtvis separata konton för programdata jämfört med loggningsdata och separata konton för testdata jämfört med produktionsdata. Under den här kursen använder du bara ett konto.

1. Öppna hello **Server Explorer** fönstret i Visual Studio.
2. Högerklicka på hello **Azure** noden och klicka sedan på **ansluta tooMicrosoft Azure-prenumerationen...** .
   
   ![Ansluta tooAzure](./media/websites-dotnet-webjobs-sdk-get-started/connaz.png)

3. Logga in med dina autentiseringsuppgifter för Azure.
4. Högerklicka på **lagring** under hello Azure noden och klicka sedan på **skapa Lagringskonto**.
   
   ![Skapa Lagringskonto](./media/websites-dotnet-webjobs-sdk-get-started/createstor.png)
   
5. I hello **skapa Lagringskonto** dialogrutan, ange ett namn för hello storage-konto.

    hello namn måste vara unika (ingen Azure storage-konto kan ha hello samma namn). Om hello namnet redan används, får du en chans toochange den.

    Hej URL tooaccess storage-konto kommer att *{name}*. core.windows.net.
6. Ange hello **Region eller Affinitetsgrupp** listrutan toohello region närmaste tooyou.

    Den här inställningen anger vilket Azure-datacenter ska vara värd för ditt lagringskonto. I den här självstudien blir valet inte någon märkbar skillnad. Men för en webbapp för produktion, du vill att webbservern och toobe din storage-konto i hello samma region toominimize svarstid och data utgång avgifter. hello-webbprogram (som du skapar senare) datacenter ska vara så nära som möjligt toohello webbläsare åtkomst till webbprogrammet hello i ordning toominimize latens.
7. Ange hello **replikering** nedrullningsbara listan för**lokalt redundant**.

    När geo-replikering är aktiverat för ett lagringskonto är hello lagras innehållet replikerade tooa sekundärt datacenter tooenable redundans toothat plats vid en större katastrof på hello primär plats. Geo-replikering kan medföra ytterligare kostnader. För testning och utveckling konton vill du normalt inte toopay för geo-replikering. Mer information finns i [Skapa, hantera eller ta bort ett lagringskonto](../storage/common/storage-create-storage-account.md).
8. Klicka på **Skapa**.

    ![Nytt lagringskonto](./media/websites-dotnet-webjobs-sdk-get-started/newstorage.png)

## <a id="download"></a>Hämta hello-program
1. Hämta och packa upp hello [färdiga lösningen](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).
2. Starta Visual Studio.
3. Från hello **filen** väljer du menyn **Öppna > projekt/lösning**, navigera toowhere som du hämtade hello lösningen och öppna sedan lösningsfilen hello.
4. Tryck på CTRL + SKIFT + B toobuild hello lösning.

    Som standard återställer Visual Studio automatiskt hello NuGet-paketinnehållet som inte ingick i hello *.zip* fil. Om inte Återställ hello paket installera dem manuellt genom att gå toohello **hantera NuGet-paket för lösningen** dialogrutan och klicka på hello **återställa** längst hello uppe till höger.
5. I **Solution Explorer**, se till att **ContosoAdsWeb** är markerad som hello Startprojekt.

## <a id="configurestorage"></a>Konfigurera hello programmet toouse storage-konto
1. Öppna programmet hello *Web.config* filen i hello ContosoAdsWeb-projektet.

    hello-filen innehåller en SQL-anslutningssträng och ett Azure storage-anslutningssträngen för att arbeta med blobbar och köer.

    hello SQL-anslutningssträng pekar tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databas.

    Hej lagringsanslutningssträng är ett exempel som har platshållare för hello lagringskontots namn och åtkomstnyckel. Du måste du ersätta en anslutningssträng som har hello namn och nyckel för ditt lagringskonto.  

    ```
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
    </connectionStrings>
    ```
    Hej lagringsanslutningssträng heter AzureWebJobsStorage eftersom det är hello namn hello WebJobs SDK använder som standard. hello samma namn används här så att du har tooset bara en anslutning strängvärde i hello Azure-miljön.
2. I **Server Explorer**, högerklicka på ditt lagringskonto under hello **lagring** noden och klicka sedan på **egenskaper**.

    ![Klicka på Lagringskontoegenskaperna](./media/websites-dotnet-webjobs-sdk-get-started/storppty.png)
3. I hello **egenskaper** -fönstret klickar du på **Lagringskontonycklar**, och klicka sedan på hello tre punkter.

    ![Lagringskontonycklar](./media/websites-dotnet-webjobs-sdk-get-started/stor-account-keys.png)
4. Kopiera hello **anslutningssträngen**.

    ![Lagring dialogruta för nycklar](./media/websites-dotnet-webjobs-sdk-get-started/cpak.png)
5. Ersätt hello lagringsanslutningssträngen i hello *Web.config* fil med hello anslutningssträngen som du precis har kopierats. Kontrollera att du väljer allt inuti hello citattecken, men inte inklusive hello citattecken innan du klistrar in.
6. Öppna hello *App.config* fil i hello ContosoAdsWebJob projekt.

    Den här filen har två storage-anslutningssträngar, ett för programdata och ett för loggning. Du kan använda separata storage-konton för programdata och loggning och du kan använda [flera lagringskonton för data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs). Den här kursen använder du ett enda storage-konto. hello anslutningssträngar har platshållare för hello lagringskontonycklar.

    ```
    <configuration>
        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;"/>
    </connectionStrings>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    </configuration>

    ```

    Som standard söker hello WebJobs-SDK för anslutningssträngar som heter AzureWebJobsStorage och AzureWebJobsDashboard. Alternativt kan du kan [lagra hello anslutningssträngen, men du vill skicka in explicit toohello `JobHost` objektet](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).
7. Ersätt båda storage-anslutningssträngar med hello anslutningssträngen som du kopierade tidigare.
8. Spara ändringarna.

## <a id="run"></a>Kör hello programmet lokalt
1. toostart hello web klientdel av programmet hello, tryck på CTRL + F5.

    hello standardwebbläsaren toohello startsidan. (hello webbprojekt körs eftersom du har gjort det hello Startprojekt.)

    ![Contoso Ads-startsida](./media/websites-dotnet-webjobs-sdk-get-started/home.png)
2. toostart hello Webbjobb serverdelen av programmet hello, högerklicka på hello ContosoAdsWebJob projekt i **Solution Explorer**, och klicka sedan på **felsöka** > **Starta ny instans** .

    Ett konsolfönster-programmet öppnas och visar meddelandeloggning som visar hello WebJobs SDK JobHost objektet har startat toorun.

    ![Kör programmet konsolfönstret visar att hello-serverdelen](./media/websites-dotnet-webjobs-sdk-get-started/backendrunning.png)
3. I webbläsaren, klickar du på **skapar en annons**.
4. Ange lite testdata, Välj en bild tooupload och klicka sedan på **skapa**.

    ![Sidan Create (Skapa)](./media/websites-dotnet-webjobs-sdk-get-started/create.png)

    hello appen går toohello indexsidan, men den visar inte en miniatyrbild för hello nya annonsen eftersom den bearbetningen inte har utförts än.

    Under tiden visar meddelandet loggning i hello konsolfönstret program efter en kort vänta att ett kömeddelande togs emot och har bearbetats.

    ![Programmet konsolfönstret visar att ett kömeddelande har bearbetats](./media/websites-dotnet-webjobs-sdk-get-started/backendlogs.png)
5. När du ser hello loggar meddelanden i hello konsolfönster för programmet, uppdatera hello Index toosee hello miniatyrbilden.

    ![Sidan Index](./media/websites-dotnet-webjobs-sdk-get-started/list.png)
6. Klicka på **information** för bilden din ad toosee hello.

    ![Sidan Details (Detaljer)](./media/websites-dotnet-webjobs-sdk-get-started/details.png)

Du har kört programmet hello på den lokala datorn och den använder en SQL Server-databas som finns på datorn, men fungerar med köer och blobbar i hello molnet. I följande avsnitt hello kör du programmet hello i hello molnet med hjälp av en databas i molnet samt molnet blobbar och köer.  

## <a id="runincloud"></a>Kör programmet hello i hello moln
Gör du hello följande steg toorun hello program i molnet hello:

* Distribuera tooWeb appar. Visual Studio skapar automatiskt en ny webbapp i App Service och en instans av SQL-databas.
* Konfigurera hello web app toouse kontot för Azure SQL-databasen och lagring.

När du har skapat några annonser när det körs i molnet hello, ska du visa hello WebJobs SDK instrumentpanelen toosee hello omfattande övervakning toooffer ännu.

### <a name="deploy-tooweb-apps"></a>Distribuera tooWeb appar

1. Stäng hello webbläsare och hello konsolfönster för programmet.
2. Åtgärderna i hello hello [publicera tooAzure med SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) avsnitt.
3. När du har slutfört hello steg för att distribuera fortsätter du med hello återstående uppgifterna i den här artikeln.

### <a name="configure-hello-web-app-toouse-your-azure-sql-database-and-storage-account"></a>Konfigurera hello web app toouse kontot för Azure SQL-databasen och lagring
Det är en bra säkerhetsrutin för[undvika att sätta känslig information, till exempel anslutningssträngar i filer som lagras i källkodslager](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets). Azure tillhandahåller en sätt toodo som: du kan ange anslutningssträngen och andra inställningsvärden i hello Azure-miljön och ASP.NET-konfiguration för API: er automatiskt välja upp dessa värden när hello app körs i Azure. Du kan ange dessa värden i Azure med hjälp av **Server Explorer**, hello Azure-portalen, Windows PowerShell, eller hello plattformsoberoende kommandoradsgränssnittet. Mer information finns i [hur programmet strängar och anslutningen strängar arbete](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).

I det här avsnittet kan du använda **Server Explorer** tooset anslutningssträngarnas värden i Azure.

1. I **Server Explorer**, högerklicka på ditt webbprogram under **Azure > Apptjänst > {din resursgrupp}**, och klicka sedan på **visningsinställningarna**.

    Hej **Azure Web App** öppnas på hello **Configuration** fliken.
2. Ändra hello namnet på hello DefaultConnection toohello namn för anslutningssträngen du valde när du konfigurerade hello SQL-databas i hello [publicera tooAzure med SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) artikel.

    Azure skapas automatiskt den här anslutningssträngen när du skapade hello webbprogrammet med en associerad databas så att den redan har hello rätt värde. Du ändrar bara hello namn toowhat koden är ute efter.
3. Lägga till två nya anslutningssträngar som heter AzureWebJobsStorage och AzureWebJobsDashboard. Ange hello databas för**anpassad**, och ange hello anslutning sträng värdet toohello samma värde som du tidigare för hello *Web.config* och *App.config* filer. (Vara säker på att du inkluderar hello hela anslutningssträngen, inte bara hello snabbtangent, och inte innehåller hello citattecken).

    Dessa anslutningssträngar som används av hello WebJobs SDK, ett för programdata och ett för loggning. Som du såg tidigare används hello en för programdata också av hello web front-end-kod.
4. Klicka på **Spara**.

    ![Anslutningssträngar i Azure Portal](./media/websites-dotnet-webjobs-sdk-get-started/azconnstr.png)
5. I **Server Explorer**högerklickar du på hello webbapp och klicka sedan på **stoppa**.
6. När hello webbprogrammet slutar, högerklicka på hello webbprogrammet igen och klickar på **starta**.

   Hej Webbjobb startar automatiskt när du publicerar, men den stoppas när du ändrar konfigurationen. toorestart, kan du antingen starta om webbprogrammet hello eller starta om hello Webbjobb i hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). Det är vanligtvis rekommenderade toorestart hello webbprogrammet efter en konfigurationsändring.
7. Uppdatera hello webbläsarfönster som har hello web app-URL i dess adressfältet.

    hello startsidan visas.
8. Skapa en annons som du gjorde när du [kördes hello program lokalt](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).

   hello indexsidan visar utan en miniatyrbild först.
9. Uppdatera hello sidan efter några sekunder och hello miniatyr visas.

   Om hello miniatyren inte visas kan kanske du toowait en minut för hello Webbjobb toorestart. Om du efter en stund fortfarande visas inte inte kanske har startats hello miniatyr när du uppdaterar sidan hello hello Webbjobb automatiskt. I så fall gå toohello **Apptjänster** bladet i hello [Azure-portalen](https://portal.azure.com/), leta upp ditt webbprogram och klicka sedan på **starta**.

### <a name="view-hello-webjobs-sdk-dashboard"></a>Visa hello WebJobs SDK-instrumentpanelen
1. I hello [Azure-portalen](https://portal.azure.com/)väljer hello **Apptjänster bladet**, leta upp ditt webbprogram och välj **WebJobs**.
3. Välj hello **loggar** fliken.

    ![Fliken loggar](./media/websites-dotnet-webjobs-sdk-get-started/log-tab.png)

    En ny webbläsarflik öppnas toohello WebJobs-SDK-instrumentpanelen. hello instrumentpanelen visar att hello Webbjobb körs och visar en lista över funktioner i koden som hello WebJobs SDK utlöses.
4. Klicka på en hello funktioner toosee information om körs.

    ![Instrumentpanel för WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjdashboardhome.png)

    ![Instrumentpanel för WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjfunctiondetails.png)

    Hej **Replay-funktionen** knappen på den här sidan gör hello WebJobs SDK framework toocall hello funktionen igen och den ger dig en möjlighet toochange hello data som skickas toohello funktion först.

> [!NOTE]
> När du är klar testning, bör du ta bort hello webbapp, storage-konto och din SQL Database-instans. hello webbprogrammet är ledig, men hello SQL storage-konto och databasinstans påförs kostnader (men, minimal på grund av toohello liten storlek). Om du lämnar hello-webbapp körs, kan alla som hittar URL: en skapa och visa annonser. 
>
>

### <a name="delete-your-web-app"></a>Ta bort ditt webbprogram
Hello portal, gå toohello **Apptjänster** bladet, leta upp och välj ditt webbprogram och klicka sedan på **ta bort**. Om du bara vill tootemporarily förhindra andra från att komma åt hello webbprogram, klickar du på **stoppa** i stället. I så fall fortsätter avgifterna tooaccrue för hello SQL-databasen och Lagringskontot.

### <a name="delete-your-storage-account"></a>Ta bort ditt lagringskonto
toodelete ditt lagringskonto finns [ta bort ett lagringskonto](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account). 

### <a name="delete-your-database"></a>Ta bort databasen
toodelete din SQL-databas, se hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) dokumentation.

## <a id="create"></a>Skapa hello programmet från grunden
I det här avsnittet gör du hello följande uppgifter:

* Skapa en Visual Studio-lösning med ett webbprojekt.
* Lägg till en klassbiblioteksprojektet för hello data access-lagret som delas mellan hello klientdelen och serverdelen.
* Lägga till ett konsolprogram projekt för hello serverdel med WebJobs distribution aktiverad.
* Lägg till NuGet-paket.
* Ange projektreferenser.
* Kopiera koden och konfigurationen programfiler från hello hämtat program som du arbetade med i hello föregående avsnitt i hello kursen.
* Granska hello delar av hello kod som fungerar med Azure-blobbar och köer och hello WebJobs-SDK.

### <a name="create-a-visual-studio-solution-with-a-web-project-and-class-library-project"></a>Skapa en Visual Studio-lösning med ett webbprojekt och klassbiblioteksprojektet
1. I Visual Studio väljer **filen** > **ny** > **projekt**.
2. I hello **nytt projekt** dialogrutan Välj **Visual C#** > **Web** > **ASP.NET-webbprogram (.NET Framework)**.
3. Namnge projektet hello ContosoAdsWeb, namnge hello lösning ContosoAdsWebJobsSDK (ändra hello lösningens namn om du ska lägga i hello samma mapp som hello hämtade lösningen), och klicka sedan på **OK**.

    ![Nytt projekt](./media/websites-dotnet-webjobs-sdk-get-started/newproject.png)
4. I hello **nytt ASP.NET-webbprogram** dialogrutan Välj hello MVC-mallen och välj **ändra autentisering**.

    ![Ändra autentisering](./media/websites-dotnet-webjobs-sdk-get-started/chgauth.png)
5. I hello **ändra autentisering** dialogrutan Välj **ingen autentisering**, och klicka sedan på **OK**.

    ![Ingen autentisering](./media/websites-dotnet-webjobs-sdk-get-started/noauth.png)
6. I hello **nytt ASP.NET-webbprogram** dialogrutan klickar du på **OK**.

    Visual Studio skapar hello lösningen och hello webbprojekt.
7. I **Solution Explorer**, högerklicka på hello lösningen (inte hello projekt) och välj **Lägg till** > **nytt projekt**.
8. I hello **Lägg till nytt projekt** dialogrutan Välj **Visual C#** > **klassiska Windows-skrivbordet** > **Class Library (.NET Framework)** mall.  
9. Namnet hello projektet *ContosoAdsCommon*, och klicka sedan på **OK**.

    Det här projektet innehåller hello Entity Framework-kontexten och datamodellen hello som båda hello klientdelen och serverdelen använder. Som ett alternativ kan du definiera hello EF-relaterade klasserna i hello webbprojektet och referera till det projektet från hello Webbjobb projekt. Men sedan projektet Webbjobb skulle ha en referens tooweb sammansättningar som det inte behöver.

### <a name="add-a-console-application-project-that-has-webjobs-deployment-enabled"></a>Lägga till ett konsolprogram projekt som har WebJobs distribution aktiverad
1. Högerklicka på hello webbprojekt (inte hello lösning eller hello klassbiblioteksprojektet) och klicka sedan på **Lägg till** > **nytt projekt för Azure Webjobs**.

    ![Nya Menyvalet för Azure Webjobs-projekt](./media/websites-dotnet-webjobs-sdk-get-started/newawjp.png)
2. I hello **lägga till Azure Webjobs** dialogrutan Ange ContosoAdsWebJob som båda hello **projektnamn** och hello **webbjobbsnamnet**. Lämna **Webbjobb kör läge** ställa in också**kör kontinuerligt**.
3. Klicka på **OK**.

   Visual Studio skapar ett konsolprogram som är konfigurerade toodeploy som ett Webbjobb när du distribuerar hello webbprojekt. toodo att det utförs hello följande uppgifter när du har skapat hello projektet:

   * Lägga till en *webbjobb publicera settings.json* filen i hello Webbjobb projektmappen egenskaper.
   * Lägga till en *webjobs list.json* filen i hello webbprogram projektmapp egenskaper.
   * Installerat hello Microsoft.Web.WebJobs.Publish NuGet-paketet i hello Webbjobb projekt.

   Mer information om dessa ändringar finns [hur toodeploy WebJobs med hjälp av Visual Studio](websites-dotnet-deploy-webjobs.md).

### <a name="add-nuget-packages"></a>Lägg till NuGet-paket
hello nytt projekt mall för ett Webbjobb projekt installerar automatiskt hello WebJobs SDK NuGet-paketet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) och dess beroenden.

En av hello WebJobs SDK beroenden som installeras automatiskt i hello Webbjobb projektet är hello Azure Storage Client bibliotek (SCL). Du måste dock tooadd den toohello web project toowork med blobbar och köer.

1. Öppna hello **hantera NuGet-paket** dialogrutan för hello lösningen.
2. I hello vänster och välj **installerade paket**.
3. Hitta hello *Azure Storage* paketet och klickar sedan på **hantera**.
4. I hello **Välj projekt** rutan, Välj hello **ContosoAdsWeb** kryssrutan och klicka sedan på **OK**.

    Alla tre projekt använda hello Entity Framework toowork med data i SQL-databas.
5. I hello vänster och välj **Online**.
6. Hitta hello *EntityFramework* NuGet-paketet och installera det i alla tre projekt.

### <a name="set-project-references"></a>Ange projektreferenser
Webb- och Webbjobb projekt arbeta med hello SQL-databas, så att båda måste en referens toohello ContosoAdsCommon-projektet.

1. Ange en referens toohello ContosoAdsCommon-projektet i ContosoAdsWeb-projektet hello. (Högerklicka på hello ContosoAdsWeb-projektet och klicka sedan på **Lägg till** > **referens**. 
2. I hello **Reference Manager** dialogrutan **projekt** > **lösning** > **ContosoAdsCommon**, och klicka sedan på **OK**.)
   
    Hej Webbjobb projekt behöver referenser för att arbeta med bilder och för att komma åt anslutningssträngar.

4. I hello ContosoAdsWebJob projekt kan du ange en referens för`System.Drawing` och `System.Configuration`.

### <a name="add-code-and-configuration-files"></a>Lägg till kod och konfigurationsfiler
Den här kursen visar inte hur för[skapar MVC-kontrollanter och vyer med scaffold-teknik](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), hur för[skriver Entity Framework-kod som fungerar med SQL Server-databaser](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), eller [hello grunderna i asynkron programmering i ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async). Så, allt som är kvar toodo kopiera koden och konfigurationen filer från hello hämtade lösningen till hello ny lösning. När du gör det hello följande avsnitt visas och förklaras viktiga delar av hello kod.

tooadd filer tooa projekt eller en mapp, högerklickar du på hello projektet eller mappen och klicka på **Lägg till** > **befintlig artikel**. Välj hello filer du vill ha och klicka på **Lägg till**. Om du blir tillfrågad om du vill tooreplace befintliga filer, klickar du på **Ja**.

1. Ta bort hello i hello ContosoAdsCommon-projektet *Class1.cs* och Lägg till i dess ställe hello följande filer från hello hämtade projektet.

   * *AD.CS*
   * *ContosoAdscontext.cs*
   * *BlobInformation.cs*
2. Lägg till hello följande filer från hello hämtade projektet i ContosoAdsWeb-projektet hello.

   * *Web.config*
   * *Global.asax.CS*  
   * I hello *domänkontrollanter* mapp: *AdController.cs*
   * I hello *Views\Shared* mapp: *_Layout.cshtml* fil
   * I hello *Views\Home* mapp: *Index.cshtml*
   * I hello *Views\Ad* mapp (skapa hello mappen först): fem *.cshtml* filer
3. Lägg till hello följande filer från hello hämtade projektet i hello ContosoAdsWebJob-projektet.

   * *App.config* (ändra hello filfilter typ för**alla filer**)
   * *Program.CS*
   * *Functions.CS*

Du kan nu skapa, köra och distribuera hello program enligt anvisningarna tidigare i kursen hello. Innan du gör det kan dock stoppa hello Webbjobb som körs fortfarande i hello första webbapp du har distribuerat till. I annat fall att Webbjobb ska bearbeta meddelanden i kö skapas lokalt eller av hello app körs i en ny webbapp, eftersom alla använder hello samma lagringskonto.

## <a id="code"></a>Granska hello programkod
hello följande avsnitt beskrivs hello kod relaterade tooworking med hello WebJobs-SDK och Azure Storage-blobbar och köer.

> [!NOTE]
> Hello kod av specifika toohello WebJobs SDK, gå i toohello [Program.cs och Functions.cs](#programcs) avsnitt.
>
>

### <a name="contosoadscommon---adcs"></a>ContosoAdsCommon – Ad.cs
filen för hello Ad.cs definierar en uppräkning för annonskategorier och en POCO-entitetsklass för annonsinformation.

        public enum Category
        {
            Cars,
            [Display(Name="Real Estate")]
            RealEstate,
            [Display(Name = "Free Stuff")]
            FreeStuff
        }

        public class Ad
        {
            public int AdId { get; set; }

            [StringLength(100)]
            public string Title { get; set; }

            public int Price { get; set; }

            [StringLength(1000)]
            [DataType(DataType.MultilineText)]
            public string Description { get; set; }

            [StringLength(1000)]
            [DisplayName("Full-size Image")]
            public string ImageURL { get; set; }

            [StringLength(1000)]
            [DisplayName("Thumbnail")]
            public string ThumbnailURL { get; set; }

            [DataType(DataType.Date)]
            [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
            public DateTime PostedDate { get; set; }

            public Category? Category { get; set; }
            [StringLength(12)]
            public string Phone { get; set; }
        }

### <a name="contosoadscommon---contosoadscontextcs"></a>ContosoAdsCommon – ContosoAdsContext.cs
Hej ContosoAdsContext-klassen anger att hello annonsklassen används i en DbSet-samling som Entity Framework lagrar i en SQL-databas.

        public class ContosoAdsContext : DbContext
        {
            public ContosoAdsContext() : base("name=ContosoAdsContext")
            {
            }
            public ContosoAdsContext(string connString)
                : base(connString)
            {
            }
            public System.Data.Entity.DbSet<Ad> Ads { get; set; }
        }

hello-klassen har två konstruktorer. hello först används av hello webbprojektet och anger hello namnet på en anslutningssträng som lagras i Web.config-filen för hello eller hello Azure körningsmiljön. hello andra konstruktorn gör toopass i hello faktiska anslutningssträngen. Det krävs av hello Webbjobb projektet eftersom det inte har en Web.config-fil. Du såg tidigare där den här anslutningssträngen lagras, och du ser hur hello koden hämtar anslutningssträngen hello när den instantierar DbContext-klassen hello.

### <a name="contosoadscommon---blobinformationcs"></a>ContosoAdsCommon – BlobInformation.cs
Hej `BlobInformation` klassen är används toostore information om en avbildning blob i ett meddelande i kön.

        public class BlobInformation
        {
            public Uri BlobUri { get; set; }

            public string BlobName
            {
                get
                {
                    return BlobUri.Segments[BlobUri.Segments.Length - 1];
                }
            }
            public string BlobNameWithoutExtension
            {
                get
                {
                    return Path.GetFileNameWithoutExtension(BlobName);
                }
            }
            public int AdId { get; set; }
        }


### <a name="contosoadsweb---globalasaxcs"></a>ContosoAdsWeb – Global.asax.cs
Kod som anropas från hello `Application_Start` metoden skapar en *bilder* blob-behållaren och en *bilder* kö om de inte redan finns. Detta säkerställer att varje gång du startar med ett nytt lagringskonto hello nödvändiga blobbehållaren och kön skapas automatiskt.

Hej kod hämtar åtkomst toohello storage-konto med hjälp av hello lagringsanslutningssträngen från hello *Web.config* fil- eller Azure körningsmiljön.

        var storageAccount = CloudStorageAccount.Parse
            (ConfigurationManager.ConnectionStrings["AzureWebJobsStorage"].ToString());

Sedan hämtar den en referens toohello *bilder* , skapar hello behållaren om den inte redan finns och anger åtkomstbehörighet för hello ny behållare. Som standard tillåter nya behållare enbart klienter med storage-konto autentiseringsuppgifter tooaccess blobbar. hello webbprogrammet måste hello blobbar toobe offentliga så att den kan visa bilder med URL: er som punkt toohello bildblobbarna.

        var blobClient = storageAccount.CreateCloudBlobClient();
        var imagesBlobContainer = blobClient.GetContainerReference("images");
        if (imagesBlobContainer.CreateIfNotExists())
        {
            imagesBlobContainer.SetPermissions(
                new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                });
        }

Liknande kod hämtar en referens toohello *thumbnailrequest* kön och skapar en ny kö. I så fall krävs ingen ändring av behörigheter.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("thumbnailrequest");
        imagesQueue.CreateIfNotExists();

### <a name="contosoadsweb---layoutcshtml"></a>ContosoAdsWeb – _Layout.cshtml
Hej *_Layout.cshtml* filen anger hello programnamn i hello sidhuvud och sidfot och skapar en menypost ”Ads”.

### <a name="contosoadsweb---viewshomeindexcshtml"></a>ContosoAdsWeb – Views\Home\Index.cshtml
Hej *Views\Home\Index.cshtml* visar kategorilänkar på startsidan för hello. hello länkarna överför hello heltalsvärde hello `Category` uppräkning i en querystring-variabel toohello Ads-indexsidan.

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### <a name="contosoadsweb---adcontrollercs"></a>ContosoAdsWeb – AdController.cs
I hello *AdController.cs* fil, hello konstruktorn anrop hello `InitializeStorage` metoden toocreate Azure Storage-klientbibliotek objekt som tillhandahåller en API för att arbeta med blobbar och köer.

Sedan hello kod hämtar en referens toohello *bilder* blob-behållare som du såg tidigare i *Global.asax.cs*. Under tiden som anger den standard [standardpolicy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) lämplig för ett webbprogram. hello standardprincipen exponentiell backoff försök kan hänga hello webbappen längre än en minut vid upprepade återförsök för ett tillfälligt fel. Hej återförsökspolicyn som anges här väntar tre sekunder efter varje försök i upp toothree försök.

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

Liknande kod hämtar en referens toohello *bilder* kön.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("blobnamerequest");

De flesta hello kontrollantkoden är typisk när du arbetar med en Entity Framework-datamodell med en DbContext-klassen. Ett undantag är hello HttpPost `Create` metod som överför en fil och sparar den i blob storage. Hej modellbindaren tillhandahåller ett [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) objekt toohello metod.

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

Om hello användaren har valt en fil tooupload hello kod överför hello-fil, sparar den i en blob och uppdaterar hello Ad-databasposten med en URL som pekar toohello blob.

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

hello kod som hello uppladdningen är i hello `UploadAndSaveBlobAsync` metod. Den skapar ett Guidenamn för hello blob, överföringar och sparar hello fil och returnerar en referens toohello sparade blob.

        private async Task<CloudBlockBlob> UploadAndSaveBlobAsync(HttpPostedFileBase imageFile)
        {
            string blobName = Guid.NewGuid().ToString() + Path.GetExtension(imageFile.FileName);
            CloudBlockBlob imageBlob = imagesBlobContainer.GetBlockBlobReference(blobName);
            using (var fileStream = imageFile.InputStream)
            {
                await imageBlob.UploadFromStreamAsync(fileStream);
            }
            return imageBlob;
        }

Efter hello HttpPost `Create` metoden laddar upp en blob och uppdateringar Hej databas, skapar en kö meddelandet tooinform hello serverdelsprocessen att en bild är redo för konvertering tooa miniatyr.

        BlobInformation blobInfo = new BlobInformation() { AdId = ad.AdId, BlobUri = new Uri(ad.ImageURL) };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        await thumbnailRequestQueue.AddMessageAsync(queueMessage);

Hej koden för hello HttpPost `Edit` metoden är liknande, förutom att om hello användaren markerar en ny bildfil, alla blobbar som redan finns för den här ad måste tas bort.

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

Här är hello-kod som tar bort blobbar när du tar bort en annons:

        private async Task DeleteAdBlobsAsync(Ad ad)
        {
            if (!string.IsNullOrWhiteSpace(ad.ImageURL))
            {
                Uri blobUri = new Uri(ad.ImageURL);
                await DeleteAdBlobAsync(blobUri);
            }
            if (!string.IsNullOrWhiteSpace(ad.ThumbnailURL))
            {
                Uri blobUri = new Uri(ad.ThumbnailURL);
                await DeleteAdBlobAsync(blobUri);
            }
        }
        private static async Task DeleteAdBlobAsync(Uri blobUri)
        {
            string blobName = blobUri.Segments[blobUri.Segments.Length - 1];
            CloudBlockBlob blobToDelete = imagesBlobContainer.GetBlockBlobReference(blobName);
            await blobToDelete.DeleteAsync();
        }

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a>ContosoAdsWeb – Views\Ad\Index.cshtml och Details.cshtml
Hej *Index.cshtml* visar miniatyrbilder med hello andra ad-data:

        <img  src="@Html.Raw(item.ThumbnailURL)" />

Hej *Details.cshtml* visar hello bilden:

        <img src="@Html.Raw(Model.ImageURL)" />

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a>ContosoAdsWeb – Views\Ad\Create.cshtml och Edit.cshtml
Hej *Create.cshtml* och *Edit.cshtml* filer ange kodningen som aktiverar hello controller tooget hello `HttpPostedFileBase` objekt.

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

En `<input>` element instruerar hello webbläsare tooprovide en dialogruta för filval.

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### <a id="programcs"></a>ContosoAdsWebJob - Program.cs
När hello Webbjobb startar hello `Main` metodanrop hello WebJobs SDK `JobHost.RunAndBlock` metoden toobegin körning av utlösta funktioner i hello aktuella tråden.

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

### <a id="generatethumbnail"></a>ContosoAdsWebJob - Functions.cs - GenerateThumbnail metod
Hej WebJobs SDK anropar den här metoden när ett kömeddelande tas emot. hello-metoden skapar en miniatyrbild och placeringar hello miniatyr URL i hello-databasen.

        public static void GenerateThumbnail(
        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,
        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)
        {
            using (Stream output = outputBlob.OpenWrite())
            {
                ConvertImageToThumbnailJPG(input, output);
                outputBlob.Properties.ContentType = "image/jpeg";
            }

            // Entity Framework context class is not thread-safe, so it must
            // be instantiated and disposed within hello function.
            using (ContosoAdsContext db = new ContosoAdsContext())
            {
                var id = blobInfo.AdId;
                Ad ad = db.Ads.Find(id);
                if (ad == null)
                {
                    throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", id.ToString()));
                }
                ad.ThumbnailURL = outputBlob.Uri.ToString();
                db.SaveChanges();
            }
        }

* Hej `QueueTrigger` attributet leder hello WebJobs SDK toocall den här metoden när ett nytt meddelande tas emot på hello thumbnailrequest kön.

        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,

    Hej `BlobInformation` objekt i kö hello-meddelande är automatiskt avserialiserat i hello `blobInfo` parameter. Hej kömeddelande tas bort när hello-metoden har slutförts. Om hello metoden misslyckas innan du slutför hälsningsmeddelande kön inte att ta bort; När ett 10 minuters lån upphör att gälla är hello-meddelande utgivna toobe tas upp igen och bearbetas. Den här sekvensen upprepas inte under obestämd tid om ett meddelande som leder alltid ett undantag. Efter 5 misslyckade försök tooprocess ett meddelande, hello-meddelande har flyttats tooa kö med namnet {könamn}-skadligt. hello högsta antal försök kan konfigureras.
* Hej två `Blob` attribut ger objekt som är bundna tooblobs: en toohello befintliga avbildningsbloben och en tooa nya miniatyr blob som hello metoden skapas.

        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)

    Blobbnamnen kommer från egenskaperna för hello `BlobInformation` objekt som tas emot i kön hälsningsmeddelande (`BlobName` och `BlobNameWithoutExtension`). tooget hello fullständiga funktionaliteten hos hello Storage-klientbibliotek, du kan använda hello `CloudBlockBlob` klassen toowork med blobbar. Om du vill tooreuse kod som har skrivits toowork med `Stream` objekt, kan du använda hello `Stream` klass.

Mer information om hur toowrite fungerar som använder WebJobs-SDK-attribut, se hello följande resurser:

* [Hur toouse Azure kö lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Hur toouse Azure blob storage med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Hur toouse Azure table storage med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Hur toouse Azure Service Bus med hello WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)

> [!NOTE]
> * Om ditt webbprogram som körs på flera virtuella datorer, flera Webbjobb ska köras samtidigt och i vissa fall kan detta resultera i hello samma data komma bearbetas flera gånger. Detta är inte ett problem om du använder hello inbyggda kön, blob och Service Bus-utlösare. hello SDK garanterar att dina funktioner kommer att bearbetas bara en gång för varje meddelande eller blob.
> * Information om hur tooimplement korrekt avslutning, se [korrekt avslutning](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).
> * Hej koden i hello `ConvertImageToThumbnailJPG` metod (visas inte) använder klasser i hello `System.Drawing` namnområde för enkelhetens skull. Dock har hello klasser i det här namnområdet utformats för användning med Windows Forms. De stöds inte för användning i en Windows- eller ASP.NET-tjänst. Mer information om alternativ för bildbearbetning finns i [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) och [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).
>
>

## <a name="next-steps"></a>Nästa steg
I den här kursen har du sett ett enkelt program i flera nivåer som använder hello WebJobs-SDK för backend-bearbetning. Det här avsnittet innehåller några förslag för att lära dig mer om ASP.NET-program för flera nivåer och WebJobs.

### <a name="missing-features"></a>Funktioner som saknas
programmet hello har förenklats för en komma igång-kursen. I ett riktigt program du vill implementera [beroendeinmatning](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) och hello [centrallager och arbetsenhetsmönster](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), använda [ett gränssnitt för loggning](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), Använd [ EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data modellera ändringar och använda [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage tillfälliga nätverksfel.

### <a name="scaling-webjobs"></a>Skalning WebJobs
WebJobs körs i hello kontexten för ett webbprogram och är inte skalbar separat. Till exempel om du har en Standard web app-instansen du har endast en instans av din bakgrunden körs och den använder vissa hello serverresurser (processor, minne, etc.) som annars skulle vara tillgängliga tooserve webbinnehåll.

Trafik för olika dag eller veckodag och om hello backend bearbetning av du behöver toodo kan vänta, kan du schemalägga WebJobs-toorun vid tidpunkter med låg trafik. Om hello belastningen fortfarande är för högt för lösningen kan köra du hello backend som ett Webbjobb i en separat webbapp som är dedikerad för detta ändamål. Du kan sedan skala ditt webbprogram för serverdelen separat från ditt frontend-webbprogram.

Mer information finns i [skalning WebJobs](websites-webjobs-resources.md#scale).

### <a name="avoiding-web-app-timeout-shut-downs"></a>Undvika web app timeout stängs-listrutor
toomake till din WebJobs alltid körs och körs på alla instanser av ditt webbprogram, du har tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) funktion.

### <a name="using-hello-webjobs-sdk-outside-of-webjobs"></a>Med hjälp av hello WebJobs SDK utanför WebJobs
Ett program som använder hello WebJobs SDK har inte toorun i Azure i ett Webbjobb. Det kan köras lokalt och det kan också köras i andra miljöer, till exempel en molntjänst arbetsroll eller en Windows-tjänst. Du kan endast öppna hello WebJobs SDK instrumentpanelen via en Azure-webbapp. toouse hello instrumentpanelen som du har tooconnect hello web app toohello storage-konto du använder genom att ange hello AzureWebJobsDashboard anslutningssträngen för hello **konfigurera** för hello klassiska portalen. Sedan kan du hämta toohello instrumentpanelen genom att använda hello följande URL:

https://{webappname}.SCM.azurewebsites.NET/azurejobs/#/Functions

Mer information finns i [får en instrumentpanel för lokal utveckling med hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), men Observera att den visar en gamla namn för anslutningssträngen.

### <a name="more-webjobs-documentation"></a>Mer WebJobs-dokumentation
Mer information finns i [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?LinkId=390226).
