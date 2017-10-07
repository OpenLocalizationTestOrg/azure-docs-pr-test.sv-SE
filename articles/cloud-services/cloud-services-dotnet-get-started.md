---
title: "aaaGet igång med Azure Cloud Services och ASP.NET | Microsoft Docs"
description: "Lär dig hur toocreate en flernivåapp med ASP.NET MVC och Azure. hello appen körs i en molntjänst med webbroll och en arbetsroll. Appen använder Entity Framework, SQL Database och Azure Storage-köer och -blobbar."
services: cloud-services, storage
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: d7aa440d-af4a-4f80-b804-cc46178df4f9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/15/2017
ms.author: adegeo
ms.openlocfilehash: 86271c29b79fad3f01f8ea0e88fd00c7aefc970c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cloud-services-and-aspnet"></a>Kom igång med Azure Cloud Services och ASP.NET

## <a name="overview"></a>Översikt
Den här kursen visar hur toocreate en .NET-flernivåapp med en ASP.NET MVC frontend, och distribuerar den tooan [Azure-molntjänst](cloud-services-choose-me.md). Hej program använder [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [Azure Blob-tjänsten](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), och hello [Azure-kötjänsten](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern). Du kan [hämta hello Visual Studio-projekt](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) från hello MSDN Code Gallery.

hello kursen visar hur toobuild och kör hello programmet lokalt, hur toodeploy den tooAzure och kör i hello molnet, och hur toobuild det från grunden. Du kan börja med att skapa från grunden och sedan hello test och distributionsstegen efteråt om du föredrar.

## <a name="contoso-ads-application"></a>Contoso Ads-program
hello program är en anslagstavla för annonser. Användare skapar en annons genom att skriva in text och ladda upp en bild. De kan se en lista över annonser med miniatyrbilder och de kan se hello bilden i full storlek när de väljer ett ad toosee hello information.

![Annonslista](./media/cloud-services-dotnet-get-started/list.png)

hello programmet använder hello [köcentriska mönster](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff belastningen hello processorintensiva arbetet med att skapa miniatyrbilder tooa backend-processen.

## <a name="alternative-architecture-websites-and-webjobs"></a>Alternativ arkitektur: Websites och WebJobs
Den här kursen visar hur toorun både frontend och backend i ett Azure cloud service. Ett alternativ är toorun hello klientdelen i en [Azure-webbplatsen](/services/web-sites/) och använda hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) funktionen (för närvarande under förhandsgranskning) för hello serverdel. En självstudiekurs som använder WebJobs finns [Kom igång med hello Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md). För information om hur toochoose hello tjänster som bäst passar din situation, se [jämförelse mellan Azure Websites, Cloud Services och virtuella datorer](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="what-youll-learn"></a>Detta får du får lära dig
* Hur tooenable datorn för Azure-utveckling genom att installera hello Azure SDK.
* Hur toocreate Visual Studio cloud service-projekt med en ASP.NET MVC-webbroll och en arbetsroll.
* Hur hello tootest molntjänstprojektet lokalt med hjälp av hello Azure storage-emulatorn.
* Hur toopublish hello molnet projektet tooan Azure cloud service och testa med Azure storage-konto.
* Hur tooupload filer och lagrar dem i hello Azure Blob-tjänsten.
* Hur toouse hello Azure-Kötjänsten för kommunikation mellan nivåerna.

## <a name="prerequisites"></a>Krav
hello kursen förutsätter att du förstår [grundläggande koncept om Azure-molntjänster](cloud-services-choose-me.md) som *webbroll* och *arbetsrollen* terminologi.  Det förutsätts även att du vet hur toowork med [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) eller [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projekt i Visual Studio. hello exempelprogrammet använder MVC, men de flesta av hello kursen gäller också tooWeb formulär.

Du kan köra hello appen lokalt utan en Azure-prenumeration, men du behöver ett toodeploy hello programmet toohello moln. Om du inte har ett konto kan du [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) eller [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).

hello självstudiekursen instruktioner fungerar med antingen hello följande produkter:

* Visual Studio 2013
* Visual Studio 2015
* Visual Studio 2017

Om du inte har någon av dessa, kan Visual Studio installeras automatiskt när du installerar hello Azure SDK.

## <a name="application-architecture"></a>Programarkitektur
hello appen lagrar annonser i en SQL-databas med hjälp av Entity Framework Code First toocreate hello tabellerna och komma åt hello data. För varje ad hello hello databasen lagrar två URL: er, en för bilden och en för hello miniatyr.

![Annonstabell](./media/cloud-services-dotnet-get-started/adtable.png)

När en användare laddar upp en bild, hello klientdelen som körs i en webbroll lagrar hello bilden i en [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), och den lagrar Hej ad i hello-databas med en URL som pekar toohello blob. AT hello samma time, skriver den ett meddelande tooan Azure kön. En serverdelsprocess som körs i en arbetsroll regelbundet avsöker hello kön efter nya meddelanden. När ett nytt meddelande visas hello arbetsrollen skapar en miniatyrbild för den bilden och uppdateringar hello miniatyrbildens URL-databasfält för den annonsen. hello följande diagram visar hur hello delar av programmet hello samverkar.

![Contoso Ads-arkitektur](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]

## <a name="download-and-run-hello-completed-solution"></a>Hämta och kör hello färdiga lösningen
1. Hämta och packa upp hello [färdiga lösningen](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).
2. Starta Visual Studio.
3. Från hello **filen** väljer du menyn **Open Project**, navigera toowhere som du hämtade hello lösningen och öppna sedan lösningsfilen hello.
4. Tryck på CTRL + SKIFT + B toobuild hello lösning.

    Som standard återställer Visual Studio automatiskt hello NuGet-paketinnehållet som inte ingick i hello *.zip* fil. Om inte Återställ hello paket installera dem manuellt genom att gå toohello **hantera NuGet-paket för lösningen** dialogrutan och klicka på hello **återställa** längst hello uppe till höger.
5. I **Solution Explorer**, se till att **ContosoAdsCloudService** är markerad som hello Startprojekt.
6. Om du använder Visual Studio 2015 eller högre, ändrar hello SQL Server-anslutningssträngen i programmet hello *Web.config* filen hello ContosoAdsWeb-projektet och i hello *ServiceConfiguration.Local.cscfg* -filen för hello ContosoAdsCloudService-projektet. I båda fallen kan du ändra ”(localdb) \v11.0” för ”(localdb) \MSSQLLocalDB”.
7. Tryck på CTRL + F5 toorun hello program.

    När du kör ett molntjänstprojekt lokalt anropar Visual Studio automatiskt hello Azure *beräkningsemulatorn* och Azure *lagringsemulatorn*. hello compute emulator använder datorns resurser toosimulate hello webbroll och worker roll-miljöer. hello storage-emulatorn använder en [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databasen toosimulate Azure molnlagring.

    hello tar första gången du kör ett molntjänstprojekt en minut för hello emulatorerna toostart upp. När emulatorerna har startats öppnas standardwebbläsaren hello toohello programmets startsida.

    ![Contoso Ads-arkitektur](./media/cloud-services-dotnet-get-started/home.png)
8. Klicka på **Create an Ad** (Skapa en annons).
9. Ange lite testdata och välj en *.jpg* bild tooupload och klicka sedan på **skapa**.

    ![Sidan Create (Skapa)](./media/cloud-services-dotnet-get-started/create.png)

    hello appen går toohello indexsidan, men den visar inte en miniatyrbild för hello nya annonsen eftersom den bearbetningen inte har utförts än.
10. Vänta en stund och uppdatera sedan hello Index toosee hello miniatyrbilden.

     ![Sidan Index](./media/cloud-services-dotnet-get-started/list.png)
11. Klicka på **information** för bilden din ad toosee hello.

     ![Sidan Details (Detaljer)](./media/cloud-services-dotnet-get-started/details.png)

Du har kört programmet hello helt på din lokala dator utan anslutning toohello moln. Hej lagringsemulatorn lagrar kö hello och blob-data i en SQL Server Express LocalDB-databas och hello programmet lagrar hello ad-data i en annan LocalDB-databas. Entity Framework Code First automatiskt skapade hello ad-databasen hello första gången hello webbappen försökte tooaccess den.

I följande avsnitt hello får du konfigurera hello lösning toouse Azure-molnresurser för köer, blobbar och programdatabasen hello när den körs i molnet hello. Om du vill toocontinue toorun lokalt men använda molnet och databasresurser kan du göra det. Det är bara gäller att ställa in anslutningssträngar, som du ser hur toodo.

## <a name="deploy-hello-application-tooazure"></a>Distribuera hello programmet tooAzure
Gör du hello följande steg toorun hello program i molnet hello:

* Skapa en Azure-molntjänst.
* Skapa en Azure SQL Database.
* Skapa ett Azure-lagringskonto.
* Konfigurera hello lösning toouse Azure SQL database när den körs i Azure.
* Konfigurera hello lösning toouse Azure storage-konto när den körs i Azure.
* Distribuera hello projektet tooyour Azure-molntjänst.

### <a name="create-an-azure-cloud-service"></a>Skapa en Azure-molntjänst
En Azure-molntjänst är hello miljö hello programmet ska köras.

1. Öppna i din webbläsare hello [Azure-portalen](https://portal.azure.com).
2. Klicka på **New > Compute > Cloud Service (Ny > Compute > Molntjänst**.

3. Ange ett URL-prefix för hello Molntjänsten i hello DNS-namn på indata.

    Den här Webbadressen har toobe unikt.  Du får ett felmeddelande om hello prefix som du har valt används redan.
4. Ange en ny resursgrupp för hello-tjänsten. Klicka på **Skapa nytt** och skriv sedan ett namn i hello resursen inkommande gruppruta, till exempel CS_contososadsRG.

5. Välj hello region där du vill att toodeploy hello.

    Det här fältet anger vilket datacenter som ska vara värd åt molntjänsten. För ett produktionsprogram väljer du hello region närmaste tooyour kunder. Välj hello region närmaste tooyou för den här kursen.
5. Klicka på **Skapa**.

    I följande bild hello, skapas en molntjänst med hello URL CSvccontosoads.cloudapp.net.

    ![Ny molntjänst](./media/cloud-services-dotnet-get-started/newcs.png)

### <a name="create-an-azure-sql-database"></a>Skapa en Azure SQL Database
Om hello app körs i molnet hello, använder en molnbaserad databas.

1. I hello [Azure-portalen](https://portal.azure.com), klickar du på **New > databaser > SQL Database**.
2. I hello **databasnamnet** ange *contosoads*.
3. I hello **resursgruppen**, klickar du på **använda befintliga** och välj hello resursgruppen som används för hello-Molntjänsten.
4. Hello följande bild, klicka på **Server – konfigurera nödvändiga inställningar** och **skapa en ny server**.

    ![Tunnel toodatabase server](./media/cloud-services-dotnet-get-started/newdb.png)

    Alternativt, om din prenumeration redan har en server, kan du välja den servern hello nedrullningsbara listan.
5. I hello **servernamn** ange *csvccontosodbserver*.

6. Ange ett **inloggningsnamn** och **lösenord** med administratörsbehörighet.

    Om du har valt **Skapa en ny server** anger du inte ett befintligt namn och lösenord här. Du har angett ett nytt namn och lösenord som du definierar nu toouse senare när du använder hello-databasen. Om du valde en server som du skapade tidigare, uppmanas du ange hello lösenord toohello administrativt användarkonto du redan skapat.
7. Välj hello samma **plats** som du valde för Molntjänsten hello.

    När hello Molntjänsten och databasen är i olika datacenter (olika regioner), ökar svarstiden och du kommer att debiteras för bandbredden utanför hello datacenter. Bandbredd inom ett datacenter är kostnadsfri.
8. Kontrollera **Tillåt azure-tjänster tooaccess server**.
9. Klicka på **Välj** för nya hello-server.

    ![Ny SQL-databasserver](./media/cloud-services-dotnet-get-started/newdbserver.png)
10. Klicka på **Skapa**.

### <a name="create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto
Ett Azure storage-konto innehåller resurser för att lagra kö-och blobbdata i hello molnet.

I ett riktigt program skapar du vanligtvis separata konton för programdata jämfört med loggningsdata, samt separata konton för testdata jämfört med produktionsdata. Under den här kursen använder du bara ett konto.

1. I hello [Azure-portalen](https://portal.azure.com), klickar du på **New > Storage > lagringskonto - blob, fil, tabell, kö**.
2. I hello **namn** ange ett URL-prefix.

    Det här prefixet och hello text som du ser under rutan hello blir hello unik URL tooyour storage-konto. Om hello-prefix du anger har redan använts av någon annan, har du toochoose ett annat prefix.
3. Ange hello **distributionsmodellen** för*klassiska*.

4. Ange hello **replikering** nedrullningsbara listan för**lokalt redundant lagring**.

    När geo-replikering är aktiverat för ett lagringskonto, är hello lagras innehållet replikerade tooa sekundärt datacenter tooenable redundans om en större katastrof inträffar på hello primära platsen. Geo-replikering kan medföra ytterligare kostnader. För testning och utveckling konton vill du normalt inte toopay för geo-replikering. Mer information finns i [Skapa, hantera eller ta bort ett lagringskonto](../storage/common/storage-create-storage-account.md).

5. I hello **resursgruppen**, klickar du på **använda befintliga** och välj hello resursgruppen som används för hello-Molntjänsten.
6. Ange hello **plats** listrutan toohello samma region som du valde för Molntjänsten hello.

    När hello cloud service och lagringskontot tillhör i olika datacenter (olika regioner), ökar svarstiden och du kommer att debiteras för bandbredden utanför hello datacenter. Bandbredd inom ett datacenter är kostnadsfri.

    Azure-tillhörighetsgrupper tillhandahåller en mekanism toominimize hello avståndet mellan resurser i datacentret, vilket kan förkorta svarstiden. Under den här kursen används inte tillhörighetsgrupper. Mer information finns i [hur tooCreate tillhörighet gruppen i Azure](http://msdn.microsoft.com/library/jj156209.aspx).
7. Klicka på **Skapa**.

    ![Nytt lagringskonto](./media/cloud-services-dotnet-get-started/newstorage.png)

    Hello bild skapas ett lagringskonto med hello URL `csvccontosoads.core.windows.net`.

### <a name="configure-hello-solution-toouse-your-azure-sql-database-when-it-runs-in-azure"></a>Konfigurera hello lösning toouse Azure SQL database när den körs i Azure
hello webbprojekt hello arbetsrollsprojektet varje har sin egen databasanslutningssträng och varje måste toopoint toohello Azure SQL database när hello app körs i Azure.

Du använder en [Web.config-transformering](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) för hello-webbroll och en cloud service miljöinställning för hello worker-rollen.

> [!NOTE]
> I det här avsnittet och hello nästa avsnitt kan lagra du autentiseringsuppgifter i projektfiler. [Lagra inte känsliga data i offentliga källkodslager](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).
>
>

1. Öppna i ContosoAdsWeb-projektet hello hello *Web.Release.config* transformationsfil för programmet hello *Web.config* fil, ta bort hello kommentarblocket som innehåller en `<connectionStrings>` element, och klistra in hello följande kod i dess ställe.

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="{connectionstring}"
        providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
    </connectionStrings>
    ```

    Lämna hello filen öppen för redigering.
2. I hello [Azure-portalen](https://portal.azure.com), klickar du på **SQL-databaser** hello vänster klickar du på hello-databasen som du skapade för den här självstudiekursen och klicka sedan på **visa anslutningssträngar**.

    ![Visa anslutningssträngar](./media/cloud-services-dotnet-get-started/showcs.png)

    hello portalen visar anslutningssträngar med en platshållare för hello lösenord.

    ![Anslutningssträngar](./media/cloud-services-dotnet-get-started/connstrings.png)
3. I hello *Web.Release.config* transformeringsfilen, ta bort `{connectionstring}` och klistra in i dess ställe hello ADO.NET-anslutningssträngen från hello Azure-portalen.
4. I hello anslutningssträngen som du klistrade in hello *Web.Release.config* transformeringsfilen ersätter `{your_password_here}` med hello lösenordet du skapade för hello ny SQL-databas.
5. Spara hello-filen.  
6. Markera och kopiera hello anslutningssträngen (utan omgivande citattecken hello) för användning i hello följande steg för att konfigurera hello arbetsrollsprojektet.
7. I **Solution Explorer**under **roller** i hello molntjänstprojektet och högerklickar du på **ContosoAdsWorker** och klicka sedan på **egenskaper**.

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)
8. Klicka på hello **inställningar** fliken.
9. Ändra **tjänstkonfiguration** för**moln**.
10. Välj hello **värdet** för hello `ContosoAdsDbConnectionString` inställningen och klistra in hello anslutningssträngen som du kopierade tidigare hello-delen av kursen hello.

     ![Databasanslutningssträng för arbetsrollen](./media/cloud-services-dotnet-get-started/workerdbcs.png)
11. Spara ändringarna.  

### <a name="configure-hello-solution-toouse-your-azure-storage-account-when-it-runs-in-azure"></a>Konfigurera hello lösning toouse Azure storage-konto när den körs i Azure
Azure-lagringskontots anslutningssträngar för både hello webbrollsprojektet och arbetsrollsprojektet hello lagras i miljöinställningar i molntjänstprojektet hello. Det finns en separat uppsättning inställningar toobe används när hello programmet körs lokalt och när den körs i molnet hello för varje projekt. Du ska uppdatera hello molnmiljöinställningarna för både webb-och arbetsrollsprojektet.

1. I **Solution Explorer**, högerklicka på **ContosoAdsWeb** under **roller** i hello **ContosoAdsCloudService** projektet och klicka sedan på **Egenskaper**.

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/roleproperties.png)
2. Klicka på hello **inställningar** fliken. I hello **tjänstkonfiguration** listrutan väljer du **moln**.

    ![Molnkonfiguration](./media/cloud-services-dotnet-get-started/sccloud.png)
3. Välj hello **StorageConnectionString** post, och sedan ser du en ellipsknapp (**...** ) längst hello högra ände hello rad. Klicka på hello ellips knappen tooopen hello **skapa Lagringsanslutningssträng för kontot** dialogrutan.

    ![Öppna dialogrutan för att skapa anslutningssträng](./media/cloud-services-dotnet-get-started/opencscreate.png)
4. I hello **skapa Lagringsanslutningssträng** dialogrutan klickar du på **prenumerationen**, väljer hello storage-konto som du skapade tidigare och klicka sedan på **OK**. Om du inte redan har loggat in uppmanas du ange dina Azure-autentiseringsuppgifter.

    ![Skapa lagringsanslutningssträng](./media/cloud-services-dotnet-get-started/createstoragecs.png)
5. Spara ändringarna.
6. Följ hello samma procedur som du använde för hello `StorageConnectionString` anslutning sträng tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` anslutningssträngen.

    Den här anslutningssträngen används för loggning.
7. Följ hello samma procedur som du använde för hello **ContosoAdsWeb** rollen tooset båda anslutningssträngar för hello **ContosoAdsWorker** roll. Glöm inte tooset **tjänstkonfiguration** för**moln**.

Hej rollmiljöinställningarna som du har konfigurerat med hello Visual Studio-Gränssnittet lagras i följande filer i ContosoAdsCloudService-projektet hello hello:

* *ServiceDefinition.csdef* -definierar hello inställningsnamnen.
* *ServiceConfiguration.Cloud.cscfg* – tillhandahåller värden när hello appen körs i hello molnet.
* *ServiceConfiguration.Local.cscfg* – tillhandahåller värden när hello appen körs lokalt.

Hej ServiceDefinition.csdef innehåller till exempel följande definitioner hello:

```xml
<ConfigurationSettings>
    <Setting name="StorageConnectionString" />
    <Setting name="ContosoAdsDbConnectionString" />
</ConfigurationSettings>
```

Och hello *ServiceConfiguration.Cloud.cscfg* filen innehåller hello värden för dessa inställningar i Visual Studio.

```xml
<Role name="ContosoAdsWorker">
    <Instances count="1" />
    <ConfigurationSettings>
        <Setting name="StorageConnectionString" value="{yourconnectionstring}" />
        <Setting name="ContosoAdsDbConnectionString" value="{yourconnectionstring}" />
        <!-- other settings not shown -->

    </ConfigurationSettings>
    <!-- other settings not shown -->

</Role>
```

Hej `<Instances>` anger hello antalet virtuella datorer som Azure ska köras hello worker rollen koden på. Hej [nästa steg](#next-steps) avsnittet innehåller länkar toomore information om att skala ut en tjänst i molnet

### <a name="deploy-hello-project-tooazure"></a>Distribuera hello projekt tooAzure
1. I **Solution Explorer**, högerklicka på hello **ContosoAdsCloudService** molnprojektet och väljer sedan **publicera**.

   ![Menyn Publish (Publicera)](./media/cloud-services-dotnet-get-started/pubmenu.png)
2. I hello **logga in** steg i hello **publicera Azure-programmet** guiden, klickar du på **nästa**.

    ![Inloggningssteg](./media/cloud-services-dotnet-get-started/pubsignin.png)
3. I hello **inställningar** steg hello guiden klickar du på **nästa**.

    ![Inställningssteg](./media/cloud-services-dotnet-get-started/pubsettings.png)

    Hej standardinställningarna i hello **Avancerat** är bra för den här självstudiekursen. Information om hello på fliken Avancerat finns [Publiceringsguiden för Azure-program](http://msdn.microsoft.com/library/hh535756.aspx).
4. I hello **sammanfattning** , klicka på **publicera**.

    ![Sammanfattningssteg](./media/cloud-services-dotnet-get-started/pubsummary.png)

   Hej **Azure-aktivitetsloggen** öppnas i Visual Studio.
5. Klicka på hello högerpilen ikonen tooexpand hello distributionsinformation.

    hello distributionen kan ta upp too5 minuter eller mer toocomplete.

    ![Fönstret med Azure-aktivitetsloggen](./media/cloud-services-dotnet-get-started/waal.png)
6. När hello Distributionsstatus är klar klickar du på hello **Webbappens URL** toostart hello program.
7. Nu kan du testa hello app genom att skapa, visa och redigera vissa annonser, som du gjorde när du körde hello programmet lokalt.

> [!NOTE]
> När du är klar testa, ta bort eller stoppa hello-Molntjänsten. Även om du inte använder hello Molntjänsten uppstår avgifter eftersom virtuella datorresurser är reserverade för den. Om du låter den köra kan dessutom alla som hittar URL:en skapa och visa annonser. I hello [Azure-portalen](https://portal.azure.com), gå toohello **översikt** för Molntjänsten och klicka sedan på hello **ta bort** knappen hello överst på hello sidan. Om du bara vill tootemporarily förhindra andra från att komma åt hello plats, klickar du på **stoppa** i stället. I så fall fortsätter avgifterna tooaccrue. Du kan följa en liknande procedur toodelete hello SQL-databasen och storage-konto när du inte längre behöver.
>
>

## <a name="create-hello-application-from-scratch"></a>Skapa hello programmet från grunden
Om du inte redan har hämtat [hello slutförts programmet](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), göra det nu. Du måste kopiera filer från hello hämtade projektet till hello nytt projekt.

Skapa hello Contoso Ads-programmet omfattar hello följande steg:

* Skapa en Visual Studio-lösning med molntjänst.
* Uppdatera och lägg till NuGet-paket.
* Ange projektreferenser.
* Konfigurera anslutningssträngar.
* Lägg till kodfiler.

När du har skapat hello lösning ska du granska hello-kod som är unik toocloud service-projekt och Azure-blobbar och köer.

### <a name="create-a-cloud-service-visual-studio-solution"></a>Skapa en Visual Studio-lösning med molntjänst
1. I Visual Studio väljer **nytt projekt** från hello **filen** menyn.
2. Hello vänster i hello **nytt projekt** dialogrutan Expandera **Visual C#** och välj **moln** mallar, och välj sedan hello **Azure Cloud Service** mall.
3. Namnge hello projektet och lösningen ContosoAdsCloudService och klicka sedan på **OK**.

    ![Nytt projekt](./media/cloud-services-dotnet-get-started/newproject.png)
4. I hello **nya Azure Cloud Service** dialogrutan Lägg till en webbroll och en arbetsroll. Namnge hello webbroll ContosoAdsWeb och namnet hello arbetsrollen ContosoAdsWorker. (Använd pennikonen hello i hello högra fönstret toochange hello standardnamnen hello roller.)

    ![Nytt molntjänstprojekt](./media/cloud-services-dotnet-get-started/newcsproj.png)
5. När du ser hello **nytt ASP.NET-projekt** dialogrutan för hello webbrollen, Välj hello MVC-mallen och klicka sedan på **ändra autentisering**.

    ![Ändra autentisering](./media/cloud-services-dotnet-get-started/chgauth.png)
6. I hello **ändra autentisering** dialogrutan Välj **ingen autentisering**, och klicka sedan på **OK**.

    ![Ingen autentisering](./media/cloud-services-dotnet-get-started/noauth.png)
7. I hello **nytt ASP.NET-projekt** dialogrutan klickar du på **OK**.
8. I **Solution Explorer**, högerklicka på hello lösningen (inte en hello projekt) och välj **Lägg till – nytt projekt**.
9. I hello **Lägg till nytt projekt** dialogrutan Välj **Windows** under **Visual C#** i hello till vänster och klicka sedan på hello **klassbiblioteket** mall.  
10. Namnet hello projektet *ContosoAdsCommon*, och klicka sedan på **OK**.

    Du måste tooreference hello Entity Framework kontexten och hello datamodellen från både webb-och arbetsrollsprojektet. Som ett alternativ kan du definiera hello EF-relaterade klasserna i webbrollsprojektet hello och referera till det projektet från arbetsrollsprojektet hello. Men i hello alternativa metoden måste arbetsrollsprojektet måste en tooweb för referenssammansättningar som inte behövs.

### <a name="update-and-add-nuget-packages"></a>Uppdatera och lägga till NuGet-paket
1. Öppna hello **hantera NuGet-paket** dialogrutan för hello lösning.
2. Överst hello i hello fönster, Välj **uppdateringar**.
3. Leta efter hello *WindowsAzure.Storage* paketet, och om den är i hello listan markerar du den och välj hello webb- och arbetsroller projekt tooupdate det och klicka sedan på **uppdatering**.

    hello lagringsklientbiblioteket uppdateras oftare än Visual Studio-projektmallar, så ofta hittar du den hello-versionen i ett projekt som nyligen skapats måste toobe uppdateras.
4. Överst hello i hello fönster, Välj **Bläddra**.
5. Hitta hello *EntityFramework* NuGet-paketet och installera det i alla tre projekt.
6. Hitta hello *Microsoft.WindowsAzure.ConfigurationManager* NuGet-paketet och installera det i arbetsrollsprojektet hello.

### <a name="set-project-references"></a>Ange projektreferenser
1. Ange en referens toohello ContosoAdsCommon-projektet i ContosoAdsWeb-projektet hello. Högerklicka på hello ContosoAdsWeb-projektet och klicka sedan på **referenser** - **Add References**. I hello **Reference Manager** dialogrutan **lösning – projekt** i hello vänster och välj **ContosoAdsCommon**, och klicka sedan på **OK**.
2. Ange en referens toohello Contosoadscommon-projektet i ContosoAdsWorker-projektet hello.

    ContosoAdsCommon innehåller hello Entity Framework data modellen och -kontextklassen som kommer att användas av båda hello frontend- och.
3. I hello ContosoAdsWorker-projektet, ange en referens för`System.Drawing`.

    Den här sammansättningen används av hello backend-tooconvert bilder toothumbnails.

### <a name="configure-connection-strings"></a>Konfigurera anslutningssträngar
I det här avsnittet konfigurerar du Azure Storage- och SQL-anslutningssträngar för lokal testning. Hej distributionsanvisningarna tidigare i kursen hello förklarar hur tooset hello anslutning strängarna för när hello appen körs i hello molnet.

1. I hello ContosoAdsWeb-projektet, öppna hello webbprogrammets Web.config-fil och infoga hello följande `connectionStrings` elementet efter hello `configSections` element.

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    Om du använder Visual Studio 2015 eller högre ersätter du ”v11.0” med ”MSSQLLocalDB”.
2. Spara ändringarna.
3. Hej ContosoAdsCloudService-projektet, högerklicka på ContosoAdsWeb under **roller**, och klicka sedan på **egenskaper**.

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/roleproperties.png)
4. I hello **ContosAdsWeb [roll]** i fönstret klickar du på hello **inställningar** fliken och klicka sedan på **Lägg till inställning**.

    Lämna **tjänstkonfiguration** ställa in också**alla konfigurationer av**.
5. Lägg till en inställning med namnet *StorageConnectionString*. Ange **typen** för*ConnectionString*, och ange **värdet** för*UseDevelopmentStorage = true*.

    ![Ny anslutningssträng](./media/cloud-services-dotnet-get-started/scall.png)
6. Spara ändringarna.
7. Följ hello samma procedur tooadd en lagringsanslutningssträng i ContosoAdsWorker-Rollegenskaperna hello.
8. Fortfarande i hello **ContosoAdsWorker [roll]** i fönstret Lägg till ytterligare en anslutningssträng:

   * Namn: ContosoAdsDbConnectionString
   * Typ: Sträng
   * Värde: Klistra in hello samma anslutningssträng som du använde för webbrollsprojektet hello. (följande exempel hello används för Visual Studio 2013. Glöm inte toochange hello datakälla om du kopierar det här exemplet och du använder Visual Studio 2015 eller högre.)

       ```
       Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;
       ```

### <a name="add-code-files"></a>Lägga till kodfiler
I det här avsnittet Kopiera kodfiler från hello hämtade lösningen till hello ny lösning. hello följande avsnitt visas och förklaras viktiga delar av den här koden.

tooadd filer tooa projekt eller en mapp, högerklickar du på hello projektet eller mappen och klicka på **Lägg till** - **befintlig artikel**. Markerar hello filer och klicka sedan på **Lägg till**. Om du blir tillfrågad om du vill tooreplace befintliga filer, klickar du på **Ja**.

1. Ta bort hello i hello ContosoAdsCommon-projektet *Class1.cs* och Lägg till i dess ställe hello *Ad.cs* och *ContosoAdscontext.cs* filer från hello hämtade projektet.
2. Lägg till hello följande filer från hello hämtade projektet i ContosoAdsWeb-projektet hello.

   * *Global.asax.cs*.  
   * I hello *Views\Shared* mapp:  *\_Layout.cshtml*.
   * I hello *Views\Home* mapp: *Index.cshtml*.
   * I hello *domänkontrollanter* mapp: *AdController.cs*.
   * I hello *Views\Ad* mapp (skapa hello mappen först): fem *.cshtml* filer.
3. Lägg till i ContosoAdsWorker-projektet hello *WorkerRole.cs* hello hämtat projektet.

Du kan nu skapa och köra programmet hello enligt anvisningarna tidigare i kursen hello och hello appen kommer att använda lokala databasen och storage-emulatorn-resurser.

hello följande avsnitt beskrivs hello kod relaterade tooworking med hello Azure-miljön, blobbar och köer. Den här kursen förklaras inte hur toocreate MVC-kontrollanter och vyer med scaffold-teknik, hur toowrite Entity Framework-kod som fungerar med SQL Server-databaser eller hello grunderna för asynkron programmering i ASP.NET 4.5. Information om de här ämnena finns hello följande resurser:

* [Kom igång med MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [Kom igång med EF 6 och MVC 5](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* [Introduktion tooasynchronous programmering i .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).

### <a name="contosoadscommon---adcs"></a>ContosoAdsCommon – Ad.cs
filen för hello Ad.cs definierar en uppräkning för annonskategorier och en POCO-entitetsklass för annonsinformation.

```csharp
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
```

### <a name="contosoadscommon---contosoadscontextcs"></a>ContosoAdsCommon – ContosoAdsContext.cs
Hej ContosoAdsContext-klassen anger att hello annonsklassen används i en DbSet-samling som Entity Framework lagrar i en SQL-databas.

```csharp
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
```

hello-klassen har två konstruktorer. Hej först av dem används av hello webbprojektet och anger en anslutningssträng som lagras i Web.config-filen för hello hello namn. hello andra konstruktorn gör toopass i faktiska hello anslutningssträngen som används av hello arbetsrollsprojektet eftersom det inte har en Web.config-fil. Du såg tidigare där den här anslutningssträngen lagras, och du ser hur hello koden hämtar anslutningssträngen hello när den instantierar DbContext-klassen hello.

### <a name="contosoadsweb---globalasaxcs"></a>ContosoAdsWeb – Global.asax.cs
Kod som anropas från hello `Application_Start` metoden skapar en *bilder* blob-behållaren och en *bilder* kö om de inte redan finns. Detta säkerställer att när du börjar med ett nytt lagringskonto eller börjar använda lagringsemulatorn hello på en ny dator, hello nödvändiga blobbehållaren och kön skapas automatiskt.

Hej kod hämtar åtkomst toohello storage-konto med hjälp av hello lagringsanslutningssträngen från hello *.cscfg* fil.

```csharp
var storageAccount = CloudStorageAccount.Parse
    (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));
```

Sedan hämtar den en referens toohello *bilder* , skapar hello behållaren om den inte redan finns och anger åtkomstbehörighet för hello ny behållare. Som standard tillåter nya behållare enbart klienter med lagringskonto autentiseringsuppgifter tooaccess blobbar. hello webbplats måste hello blobbar toobe offentliga så att den kan visa bilder med URL: er som punkt toohello bildblobbarna.

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
var imagesBlobContainer = blobClient.GetContainerReference("images");
if (imagesBlobContainer.CreateIfNotExists())
{
    imagesBlobContainer.SetPermissions(
        new BlobContainerPermissions
        {
            PublicAccess =BlobContainerPublicAccessType.Blob
        });
}
```

Liknande kod hämtar en referens toohello *bilder* kön och skapar en ny kö. I så fall krävs ingen ändring av behörigheter.

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
var imagesQueue = queueClient.GetQueueReference("images");
imagesQueue.CreateIfNotExists();
```

### <a name="contosoadsweb---layoutcshtml"></a>ContosoAdsWeb – \_Layout.cshtml
Hej *_Layout.cshtml* filen anger hello programnamn i hello sidhuvud och sidfot och skapar en menypost ”Ads”.

### <a name="contosoadsweb---viewshomeindexcshtml"></a>ContosoAdsWeb – Views\Home\Index.cshtml
Hej *Views\Home\Index.cshtml* visar kategorilänkar på startsidan för hello. hello länkarna överför hello heltalsvärde hello `Category` uppräkning i en querystring-variabel toohello Ads-indexsidan.

```razor
<li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
<li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
<li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
<li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>
```

### <a name="contosoadsweb---adcontrollercs"></a>ContosoAdsWeb – AdController.cs
I hello *AdController.cs* fil, hello konstruktorn anrop hello `InitializeStorage` metoden toocreate Azure Storage-klientbibliotek objekt som tillhandahåller en API för att arbeta med blobbar och köer.

Sedan hello kod hämtar en referens toohello *bilder* blob-behållare som du såg tidigare i *Global.asax.cs*. När den gör det anger den en [standardpolicy för återförsök](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) som är lämplig för en webbapp. hello standardprincipen exponentiell backoff försök kan hänga hello webbappen längre än en minut vid upprepade återförsök för ett tillfälligt fel. Hej återförsökspolicyn som anges här väntar tre sekunder efter varje försök i upp toothree försök.

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesBlobContainer = blobClient.GetContainerReference("images");
```

Liknande kod hämtar en referens toohello *bilder* kön.

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesQueue = queueClient.GetQueueReference("images");
```

De flesta hello kontrollantkoden är typisk när du arbetar med en Entity Framework-datamodell med en DbContext-klassen. Ett undantag är hello HttpPost `Create` metod som överför en fil och sparar den i blob storage. Hej modellbindaren tillhandahåller ett [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) objekt toohello metod.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<ActionResult> Create(
    [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
    HttpPostedFileBase imageFile)
```

Om hello användaren har valt en fil tooupload hello kod överför hello-fil, sparar den i en blob och uppdaterar hello Ad-databasposten med en URL som pekar toohello blob.

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    blob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = blob.Uri.ToString();
}
```

hello kod som hello uppladdningen är i hello `UploadAndSaveBlobAsync` metod. Den skapar ett Guidenamn för hello blob, överföringar och sparar hello fil och returnerar en referens toohello sparade blob.

```csharp
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
```

Efter hello HttpPost `Create` metoden laddar upp en blob och uppdateringar Hej databas, skapar en kö meddelandet tooinform backend-processen att en bild är redo för konvertering tooa miniatyr.

```csharp
string queueMessageString = ad.AdId.ToString();
var queueMessage = new CloudQueueMessage(queueMessageString);
await queue.AddMessageAsync(queueMessage);
```

Hej koden för hello HttpPost `Edit` metoden är liknande, förutom att om hello användaren markerar en ny bildfil måste alla blobbar som redan finns tas bort.

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    await DeleteAdBlobsAsync(ad);
    imageBlob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = imageBlob.Uri.ToString();
}
```

hello nästa exempel visar hello-kod som tar bort blobbar när du tar bort en annons.

```csharp
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
```

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a>ContosoAdsWeb – Views\Ad\Index.cshtml och Details.cshtml
Hej *Index.cshtml* visar miniatyrbilder med hello andra ad-data.

```razor
<img src="@Html.Raw(item.ThumbnailURL)" />
```

Hej *Details.cshtml* visar hello bilden.

```razor
<img src="@Html.Raw(Model.ImageURL)" />
```

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a>ContosoAdsWeb – Views\Ad\Create.cshtml och Edit.cshtml
Hej *Create.cshtml* och *Edit.cshtml* filer ange kodningen som aktiverar hello controller tooget hello `HttpPostedFileBase` objekt.

```razor
@using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))
```

En `<input>` element instruerar hello webbläsare tooprovide en dialogruta för filval.

```razor
<input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />
```

### <a name="contosoadsworker---workerrolecs---onstart-method"></a>ContosoAdsWorker – WorkerRole.cs – OnStart-metoden
hello Azure worker-rollen miljö anropar hello `OnStart` metod i hello `WorkerRole` klassen när hello worker-rollen är igång och anropar hello `Run` metod när hello `OnStart` metod har avslutats.

Hej `OnStart` metoden hämtar hello databasanslutningssträngen från hello *.cscfg* filen och skickar den toohello Entity Framework DbContext-klassen. hello SQLClient-leverantören används som standard så hello-providern inte har angetts toobe.

```csharp
var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
db = new ContosoAdsContext(dbConnString);
```

Efter det hello metoden hämtar en referens toohello storage-konto och skapar hello blobbehållaren och kön om de inte finns. hello-koden för som är liknande toowhat som du redan har sett i hello webbroll `Application_Start` metod.

### <a name="contosoadsworker---workerrolecs---run-method"></a>ContosoAdsWorker – WorkerRole.cs – Run-metoden
Hej `Run` metoden anropas när hello `OnStart` metod har avslutat initieringsarbetet. hello metoden Kör en oändlig loop som söker efter nya Kömeddelanden och bearbetar dem när de tas emot.

```csharp
public override void Run()
{
    CloudQueueMessage msg = null;

    while (true)
    {
        try
        {
            msg = this.imagesQueue.GetMessage();
            if (msg != null)
            {
                ProcessQueueMessage(msg);
            }
            else
            {
                System.Threading.Thread.Sleep(1000);
            }
        }
        catch (StorageException e)
        {
            if (msg != null && msg.DequeueCount > 5)
            {
                this.imagesQueue.DeleteMessage(msg);
            }
            System.Threading.Thread.Sleep(5000);
        }
    }
}
```

Om inget kömeddelande hittades efter varje upprepning av loopen hello viloläge hello program i en sekund. Detta förhindrar att arbetsrollen hello medför överdriven CPU tid och lagring transaktionskostnader. hello Microsoft Customer Advisory Team berättar om en utvecklare som glömde bort tooinclude, distribuerats tooproduction, och åkte på semester. När han fick tillbaka kostade hans misstag mer än hello semester.

Ibland orsakar hello innehållet i ett kömeddelande ett fel i bearbetningen. Detta kallas en *skadligt meddelande*, och om du precis har loggat ett fel och startas om hello loop, du oändlighet försöka tooprocess meddelandet.  Därför innehåller hello catch-blocket en if-sats som kontrollerar toosee hur många gånger hello appen har försökt tooprocess hello aktuella meddelandet och om den har mer än 5 gånger hello-meddelande tas bort från hello kö.

`ProcessQueueMessage` anropas när ett kömeddelande hittas.

```csharp
private void ProcessQueueMessage(CloudQueueMessage msg)
{
    var adId = int.Parse(msg.AsString);
    Ad ad = db.Ads.Find(adId);
    if (ad == null)
    {
        throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", adId.ToString()));
    }

    CloudBlockBlob inputBlob = this.imagesBlobContainer.GetBlockBlobReference(ad.ImageURL);

    string thumbnailName = Path.GetFileNameWithoutExtension(inputBlob.Name) + "thumb.jpg";
    CloudBlockBlob outputBlob = this.imagesBlobContainer.GetBlockBlobReference(thumbnailName);

    using (Stream input = inputBlob.OpenRead())
    using (Stream output = outputBlob.OpenWrite())
    {
        ConvertImageToThumbnailJPG(input, output);
        outputBlob.Properties.ContentType = "image/jpeg";
    }

    ad.ThumbnailURL = outputBlob.Uri.ToString();
    db.SaveChanges();

    this.imagesQueue.DeleteMessage(msg);
}
```

Den här koden läser hello databasen tooget hello bild-URL, konverterar hello bildens tooa miniatyrbild, sparar hello miniatyren i en blob, uppdaterar hello databasen med hello miniatyr blob URL och tar bort hälsningsmeddelande för kön.

> [!NOTE]
> Hej koden i hello `ConvertImageToThumbnailJPG` metoden använder klasser i namnområdet för hello System.Drawing för enkelhetens skull. Dock har hello klasser i det här namnområdet utformats för användning med Windows Forms. De stöds inte för användning i en Windows- eller ASP.NET-tjänst. Mer information om alternativ för bildbearbetning finns i [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) och [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).
>
>

## <a name="troubleshooting"></a>Felsökning
Om något inte fungerar när du följer hello instruktioner i den här självstudiekursen, här är några vanliga fel och hur tooresolve dem.

### <a name="serviceruntimeroleenvironmentexception"></a>ServiceRuntime.RoleEnvironmentException
Hej `RoleEnvironment` tillhandahålls av Azure när du kör ett program i Azure eller när du kör lokalt med hjälp av hello Azure-beräkningsemulatorn.  Om du får detta felmeddelande när du kör lokalt, kontrollerar du att du har angett hello ContosoAdsCloudService-projektet som Startprojekt hello. Detta konfigurerar hello projektet toorun med hello Azure-beräkningsemulatorn.

Hello saker hello program använder hello Azure RoleEnvironment för är tooget hello anslutningssträngarnas värden som lagras i hello *.cscfg* filerna, så en annan orsak till det här undantaget är en anslutningssträng som saknas. Kontrollera att som du skapade hello StorageConnectionString-inställningen både för moln och lokala konfigurationer i ContosoAdsWeb-projektet hello och att du har skapat båda anslutningssträngar för båda konfigurationerna i ContosoAdsWorker-projektet hello. Om du gör en **Sök alla** Sök efter StorageConnectionString i hela hello-lösning bör du se den 9 gånger i 6-filer.

### <a name="cannot-override-tooport-xxx-new-port-below-minimum-allowed-value-8080-for-protocol-http"></a>Det går inte att åsidosätta tooport xxx. New port below minimum allowed value 8080 for protocol http. (Ny port under minsta tillåtna värde 8080 för protokollet http.)
Försök att ändra hello-portnumret som används av hello webbprojekt. Högerklicka på hello ContosoAdsWeb-projektet och klicka sedan på **egenskaper**. Klicka på hello **Web** fliken och sedan ändra hello portnumret i hello **Url för** inställningen.

Ett annat alternativ som kan lösa problemet hello finns hello efter avsnittet.

### <a name="other-errors-when-running-locally"></a>Andra fel när du kör programmet lokalt
Serviceprojekt använder som standard för nya molntjänster hello Azure compute emulator express toosimulate hello Azure-miljön. Detta är en förenklad version av hello fullständiga beräkningsemulatorn och under vissa förhållanden hello fullständiga emulatorn fungerar när hello express-version som inte.  

toochange hello projektet toouse hello fullständiga emulatorn, högerklickar hello ContosoAdsCloudService-projektet och klicka sedan på **egenskaper**. I hello **egenskaper** fönstret klickar du på hello **Web** fliken och klicka sedan på hello **Använd fullständig Emulator** knappen.

I ordning toorun hello program med hello fullständiga emulatorn har du tooopen Visual Studio med administratörsbehörighet.

## <a name="next-steps"></a>Nästa steg
hello Contoso Ads-programmet har avsikt förenklats för en komma igång-kursen. Till exempel den implementerar inte [beroendeinmatning](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) eller hello [centrallager och arbetsenhetsmönster](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), inte [används ett gränssnitt för loggning](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), den använder inte [ EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage datamodell ändringar eller [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage tillfälligt nätverksfel och så vidare.

Här följer några exempelprogram för molntjänster som visar fler verklighetsbaserade kodningsexempel, i från mindre komplex toomore komplexa:

* [PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31). Liknande koncept tooContoso Ads men implementerar flera funktioner och fler verklighetsbaserade kodningsexempel.
* [Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36). Introducerar Azure Storage-tabeller samt blobbar och köer. Baserat på en äldre version av hello Azure SDK för .NET, kräver vissa ändringar toowork med hello nuvarande version.
* [Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ett omfattande exempel som visar ett stort urval med bästa arbetsmetoder som produceras av hello Microsoft Patterns and Practices-gruppen.

Allmän information om hur du utvecklar för molnet hello finns [skapa verkliga Molnappar med Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

Bästa metoder och mönster för en videopresentation tooAzure lagring, se [Microsoft Azure Storage – What's New, Best Practices and Patterns](http://channel9.msdn.com/Events/Build/2014/3-628).

Mer information finns i hello följande resurser:

* [Azure Cloud Services, del 1: Inledning](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [Hur toomanage Cloud Services](cloud-services-how-to-manage.md)
* [Azure Storage](/documentation/services/storage/)
* [Hur toochoose ett moln-leverantörer](https://azure.microsoft.com/overview/choosing-a-cloud-service-provider/)
