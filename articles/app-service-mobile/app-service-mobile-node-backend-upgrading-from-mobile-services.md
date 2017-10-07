---
title: "aaaUpgrade från Mobile Services tooAzure App Service - Node.js"
description: "Lär dig hur tooeasily uppgradera din Mobile Services programmet tooan Apptjänst Mobile App"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 722cda244d4f633247827f58ea6f1397137ea600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-tooapp-service"></a>Uppgradera din befintliga Mobiltjänst för Node.js Azure tooApp Service
Apptjänst Mobile är ett nytt sätt toobuild mobila program med Microsoft Azure. Det finns fler toolearn [vad är Mobilappar?].

Det här avsnittet beskrivs hur tooupgrade ett befintligt Node.js-serverdel program från Azure Mobile Services tooa nya Mobile Apps i Apptjänst. Befintliga Mobile Services programmet kan fortsätta toooperate medan du utför uppgraderingen.  Om du behöver tooupgrade ett Node.js-serverdel program, se för[uppgradera din Mobile Services .NET](app-service-mobile-net-upgrading-from-mobile-services.md).

När en mobilserverdel är uppgraderade tooAzure Apptjänst, har den åtkomst tooall App Service-funktionerna och är debiteras enligt för[priser för Apptjänst], inte Mobile Services priser.

## <a name="migrate-vs-upgrade"></a>Migrera kontra uppgradering
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Vi rekommenderar att du [utför en migrering](app-service-mobile-migrating-from-mobile-services.md) innan du fortsätter med uppgraderingen. På så sätt kan du placera båda versionerna av programmet hello samma App Service-Plan och innebära utan extra kostnad.
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Förbättringar i Mobile Apps Node.js server-SDK
Uppgradera toohello nya [Mobile Apps-SDK](https://www.npmjs.com/package/azure-mobile-apps) innehåller många förbättringar, inklusive:

* Baserat på hello [Express framework](http://expressjs.com/en/index.html), hello nya nod SDK är inaktiverad och är utformad tookeep upp med den nya noden versioner när de kommer. Du kan anpassa hello programmets beteende med Express-mellanprogrammet.
* Betydande prestandaförbättringar jämfört med toohello Mobile Services SDK.
* Du kan nu vara värd för en webbplats tillsammans med din mobila serverdel; på samma sätt är det enkelt tooadd hello Azure Mobile SDK tooany befintligt express.v4 program.
* Bygger för flera plattformar och lokala utveckling hello Mobile Apps-SDK kan utvecklas och köras lokalt på Windows, Linux och OS x-plattformar. Nu är det enkelt toouse vanliga nod utvecklingstekniker som körs [Mocha](https://mochajs.org/) testar tidigare toodeployment.

## <a name="overview"></a>Uppgradera översikt
tooaid vid uppgradering av en Node.js-serverdel Azure App Service tillhandahåller ett paket för kompatibilitet.  Efter uppgraderingen måste en niew plats som kan vara distribuerade tooa nya App Service-platsen.

hello Mobile Services-klienten SDK: er **inte** kompatibel med hello ny Mobile Apps server SDK. I ordning tooprovide tjänstens stabilitet för din app, bör du inte publicera ändringar tooa plats som betjänar publicerade klienter. I stället bör du skapa en ny mobil app som fungerar som en dubblett. Du kan publicera programmet hello samma Apptjänst planerar tooavoid medför ytterligare kostnader.

Du måste sedan två versioner av programmet hello: en som förblir hello samma fungerar publicerade appar i hello jokertecken och andra som du kan sedan uppgradera och mål med en ny klient. Du kan flytta och testa din kod i din takt, men du bör se till att alla felkorrigeringar du komma tillämpade tooboth. När du anser att ett önskat antal klientappar i jokertecken har uppdaterat toohello senaste versionen, ta bort hello ursprungliga migrerade appen om du vill ha. Den innebära inte någon ytterligare monetära kostnader, om finns i samma Apptjänst planera som din Mobilapp hello.

hello fullständig disposition för hello uppgraderingsprocessen är följande:

1. Hämta din befintliga (migrerade) Azure-mobiltjänst.
2. Konvertera hello projektet tooan Azure Mobile App använder hello kompatibilitet paketet.
3. Åtgärda eventuella skillnader (t.ex inställningar för autentisering).
4. Distribuera din konverterade Azure Mobile App-projekt tooa nya App Service.
5. Släpp en ny version av ditt klientprogram att använda hello nya Mobilapp.
6. (Valfritt) Ta bort appen för migrerade mobiltjänst.

Borttagningen kan inträffa när du inte ser någon trafik på din ursprungliga migrerade mobiltjänst.

## <a name="install-npm-package"></a>Installera förutsättningar för hello
Du bör installera [Node] på den lokala datorn.  Du bör också installera hello kompatibilitet paketet.  När noden har installerats kan du köra följande kommando från en ny cmd eller PowerShell-Kommandotolken hello:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Hämta Azure Mobile Services-skript
* Logga in toohello [Azure Portal].
* Med hjälp av **alla resurser** eller **Apptjänster**, hitta Mobile Services-plats.
* Inom hello plats klickar du på **verktyg** -> **Kudu** -> **Gå** tooopen hello Kudu plats.
* Klicka på **Felsökningskonsolen** -> **PowerShell** tooopen hello Felsökningskonsolen.
* Navigera för`site/wwwroot/App_Data/config` genom att klicka på varje katalog i sin tur
* Klicka på hello download ikonen nästa toohello `scripts` directory.

Detta hämtar hello skript i ZIP-format.  Skapa en ny katalog på den lokala datorn och packa upp hello `scripts.ZIP` filen i hello katalog.  Detta skapar en `scripts` directory.

## <a name="scaffold-app"></a>Autogenerera hello nya Azure Mobile Apps-serverdel
Kör följande kommando från hello katalog som innehåller katalogen för hello skript hello:

```scaffold-mobile-app scripts out```

Detta skapar en autogenererade Azure Mobile Apps-serverdel i hello `out` directory.  Även om de inte krävs, är det en bra idé toocheck hello `out` katalogen i en databas med källan kod du väljer.

## <a name="deploy-ama-app"></a>Distribuera Azure Mobile Apps-serverdel
Under distributionen behöver du toodo hello följande:

1. Skapa en ny Mobile App i hello [Azure Portal].
2. Kör hello `createViews.sql` skript på den anslutna databasen.
3. Länken hello-databas som är länkade tooyour Mobiltjänst tooyour nya App Service.
4. Länka andra resurser (till exempel Meddelandehubbar)-toohello nya App Service.
5. Distribuera hello genererade koden tooyour ny plats.

### <a name="create-a-new-mobile-app"></a>Skapa en ny Mobile App
1. Logga in på hello [Azure Portal].
2. Klicka på **+NY** > **Webb + Mobil** > **Mobilapp** och ange sedan ett namn för din serverdel för mobilappen.
3. För hello **resursgruppen**, Välj en befintlig resursgrupp eller skapa en ny (med hjälp av hello samma namn som din app.)

    Du kan antingen välja en annan App Service-plan eller skapa en ny. Mer information om App Service-planer och hur toocreate en ny plan i en annan prisnivå på datanivå och på din önskade plats finns [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. För hello **programtjänstplanen**, hello standardplanen (i hello [standardnivån](https://azure.microsoft.com/pricing/details/app-service/)) är markerad. Du kan också välja en annan plan eller [skapa en ny](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). hello App Service-planens inställningar avgör hello [plats, funktioner, kostnad och beräkningsresurser](https://azure.microsoft.com/pricing/details/app-service/) som är associerade med din app.

    När du bestämmer dig för hello planen klickar du på **skapa**. Detta skapar hello mobilappsserverdel.

### <a name="run-createviewssql"></a>Kör CreateViews.SQL
hello autogenererade appen innehåller en fil med namnet `createViews.sql`.  Det här skriptet måste köras mot måldatabasen.  hello anslutningssträngen för hello måldatabasen kan hämtas från mobiltjänsten migrerade från hello **inställningar** bladet under **anslutningssträngar**.  Det heter `MS_TableConnectionString`.

Du kan köra skriptet i SQL Server Management Studio eller Visual Studio.

### <a name="link-hello-database-tooyour-app-service"></a>Länka hello databasen tooyour Apptjänst
Länka hello befintlig databas tooyour Apptjänst:

* I hello [Azure Portal], öppna din Apptjänst.
* Välj **alla inställningar** -> **dataanslutningar**.
* Klicka på **+ Lägg till**.
* I hello listrutan, Välj **SQL-databas**
* Under **SQL-databas**, Välj den befintliga databasen, och klicka sedan på **Välj**.
* Under **anslutningssträngen**, ange hello användarnamn och lösenord för hello-databasen, och klicka sedan på **OK**.
* I hello **lägga till dataanslutningar** bladet, klickar du på **OK**.

hello användarnamn och lösenord finns genom att visa hello anslutningssträngen för hello måldatabasen i din migrerade mobiltjänst.

### <a name="set-up-authentication"></a>Konfigurera autentisering
Azure Mobile Apps kan du tooconfigure Azure Active Directory, Facebook, Google, Microsoft och Twitter autentisering inom hello-tjänsten.  Anpassad autentisering måste toobe utvecklats separat.  Referera till hello [Autentiseringskoncept] dokumentation och [autentisering Quickstart] dokumentationen för mer information.  

## <a name="updating-clients"></a>Uppdatera mobila klienter
När du har en operativa mobilappsserverdel kan arbeta du med en ny version av ditt klientprogram som använder den. Mobile Apps innehåller också en ny version av hello klienten SDK: er och liknande toohello serveruppgraderingen ovan, behöver du tooremove alla referenser toohello Mobile Services SDK innan du installerar Mobile Apps-versioner.

En av hello huvudsakliga ändringar mellan hello versioner är att hello konstruktorer inte längre behöver en tangent.
Du nu skickar hello URL för din Mobilapp. Till exempel på hello .NET-klienter, hello `MobileServiceClient` konstruktorn är nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of hello Mobile App
        );

Du kan läsa om hur du installerar hello nya SDK: er och använder hello ny struktur via hello länkarna nedan:

* [Android-versionen 2.2 eller senare](app-service-mobile-android-how-to-use-client-library.md)
* [iOS version 3.0.0 eller senare](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) version 2.0.0 eller senare](app-service-mobile-dotnet-how-to-use-client-library.md)
* [Apache Cordova version 2.0 eller senare](app-service-mobile-cordova-how-to-use-client-library.md)

Om ditt program gör användning av push-meddelanden, anteckna hello registreringen instruktioner för varje plattform, har eftersom det ändringar det också.

När du har hello nya klientversionen redo prova mot projektet uppgraderade servern. Du kan släppa en ny version av programmet-toocustomers efter verifierar att den fungerar. Slutligen, när dina kunder har fått en chans tooreceive dessa uppdateringar, du kan ta bort hello Mobile Services version av appen. Nu är du har uppgraderat tooan App Service-Mobilapp med hjälp av hello senaste Mobile Apps server-SDK.

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Vad är Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[Priser för Apptjänst]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Autentiseringskoncept]: ../app-service/app-service-authentication-overview.md
[Snabbstart för autentisering]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
