---
title: "aaaEnable offlinesynkronisering för Azure Mobile App (Cordova) | Microsoft Docs"
description: "Lär dig hur toouse Apptjänst Mobile App toocache och synkronisera offlinedata i Cordova-program"
documentationcenter: cordova
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 4e6ae96c3d96dac8ebb3749354b83a04686831b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Aktivera offlinesynkronisering av Cordova-mobilapp
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Den här kursen introducerar hello offlinesynkronisering funktion i Azure Mobile Apps för Cordova. Offlinesynkronisering kan slutet användare toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning. Ändringarna sparas i en lokal databas.  När hello enheten är online igen, har ändringarna synkroniserats med hello fjärrtjänsten.

Den här kursen är baserad på hello Cordova quickstart lösning för Mobilappar som du skapar när du slutför hello kursen [Apache Cordova Snabbstart]. I den här kursen kan du uppdatera hello quickstart lösning tooadd offline funktioner i Azure Mobile Apps.  Vi kan också markera hello offline-specifik kod i hello app.

toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps]. Mer information om API-användning, finns hello [API-dokumentationen](https://azure.github.io/azure-mobile-apps-js-client).

## <a name="add-offline-sync-toohello-quickstart-solution"></a>Lägg till offlinesynkronisering toohello quickstart lösning
hello offlinesynkronisering kod måste läggas till toohello app. Offlinesynkronisering kräver hello cordova-sqlite-storage plugin-program, som hämtar läggs automatiskt tooyour app när hello Azure Mobile Apps plugin-programmet ingår i hello-projekt. Hej snabbstartsprojektet innehåller båda dessa plugin-program.

1. Öppna index.js i Visual Studio Solution Explorer och Ersätt hello följande kod

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable;      // Reference tooa table endpoint on backend

    med den här koden:

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable,      // Reference tooa table endpoint on backend
           syncContext;        // Reference toooffline data sync context

2. Ersätt sedan hello följande kod:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    med den här koden:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              complete: 'boolean',
              version: 'string'
          }
        });

        // Get hello sync context from hello client
        syncContext = client.getSyncContext();

    hello föregående kod tillägg initiera hello lokalt Arkiv och definiera en lokal tabell som matchar hello kolumnvärdena används i din Azure-serverdel. (Du behöver inte tooinclude alla kolumnvärdena i den här koden.)  Hej `version` underhålls av hello mobilserverdel och används för konfliktlösning.

    Du får en kontext som referens toohello synkronisering genom att anropa **getSyncContext**. hello sync kontexten bevaras relationerna mellan tabellerna genom att spåra och överföra ändringar i alla tabeller som ett klientprogram har ändrat när `.push()` anropas.

3. Uppdatera hello programmet URL tooyour Mobilapp programmets URL.

4. Ersätt sedan den här koden:

        todoItemTable = client.getTable('todoitem'); // todoitem is hello table name

    med den här koden:

        // Initialize hello sync context with hello store
        syncContext.initialize(store).then(function () {

        // Get hello local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle hello conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert tooserver's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle hello error
                  // In hello simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function tooperform hello actual sync
        syncBackend();

        // Refresh hello todoItems
        refreshDisplay();

        // Wire up hello UI Event Handler for hello Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    hello föregående kod initierar hello sync kontext och anropar sedan getSyncTable (i stället för getTable) tooget en toohello lokala referenstabell.

    Den här koden använder hello lokal databas för alla skapa, läsa, uppdatera och ta bort CRUD-tabellåtgärder.

    Det här exemplet utför enkel felhantering på eventuella konflikter. Ett riktigt program skulle hantera hello olika fel som nätverksförhållanden och servern är i konflikt. Kodexempel finns hello [offlinesynkronisering exempel].

5. Lägg till funktionen tooperform hello faktiska synkroniseringen.

        function syncBackend() {

          // Sync local store tooAzure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from hello Azure table after syncing tooAzure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Du bestämmer dig för när toopush ändras toohello mobilappsserverdel genom att anropa **syncContext.push()**. Du kan exempelvis kalla **syncBackend** i en händelsehanterare bundet tooa sync knappen.

## <a name="offline-sync-considerations"></a>Offlinesynkronisering överväganden

I exemplet hello hello **push** metod för **syncContext** endast anropas på appen startas i hello Återanropsfunktionen för inloggning.  I ett riktigt program kan du också göra funktionen sync utlöses manuellt eller när hello nätverket tillstånd ändras.

När en pull körs mot en tabell som har väntande spåras lokala uppdateringar av hello kontext som hämtar åtgärden automatiskt utlösare en push. När du uppdaterar, lägga till och slutföra objekt i det här exemplet, kan du utelämna hello explicit **push** anropa, eftersom det kan vara redundant.

I hello tillhandahålls kod alla poster i hello remote todoItem-tabell frågas, men är det också möjligt toofilter poster genom att skicka en fråge-id och fråga för**push**. Mer information finns i avsnittet hello *inkrementell synkronisering* i [Offline datasynkronisering i Azure Mobile Apps].

## <a name="optional-disable-authentication"></a>(Valfritt) Inaktivera autentisering

Om du inte vill tooset konfigurerar autentisering innan du testar offlinesynkronisering, kommentera ut hello Återanropsfunktionen för inloggning, men lämna hello kod inuti hello Återanropsfunktionen tagit bort kommentarstecken från.  Efter att kommentera bort hello inloggningen rader hello kod visas nedan:

      // Login toohello service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave hello rest of hello code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Nu hello app synkroniseras med hello Azure-serverdel när du kör hello app.

## <a name="run-hello-client-app"></a>Kör hello-klientappen
Med offlinesynkronisering har nu aktiverats, kan du köra hello klientprogrammet minst en gång på varje plattform för att fylla i databasen för hello lokal lagring. Senare, simulera ett offline scenario och ändra hello data i hello lokalt Arkiv medan hello appen är offline.

## <a name="optional-test-hello-sync-behavior"></a>(Valfritt) Testa hello sync beteende
I det här avsnittet Ändra hello klienten projektet toosimulate ett offline-scenario med en ogiltig programmets URL för din serverdel. När du lägger till eller ändra dataobjekt ändringarna lagras i det lokala arkivet, men är inte synkroniserade toohello backend-datalagret förrän hello anslutningen har återupprättats.

1. Öppna projektfilen för hello index.js hello Solution Explorer, och ändra hello programmet URL toopoint till en ogiltig URL som hello följande kod:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Uppdatera hello CSP i index.html `<meta>` element med hello samma ogiltig URL.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Skapa och kör hello klientapp och Lägg märke till att ett undantag loggas i konsolen hello när hello-appen försöker synkronisera med hello backend efter inloggning. Alla nya objekt som du lägger till finns bara i hello lokalt Arkiv tills de pushas toohello mobilserverdel. hello-klientappen fungerar som om den är ansluten toohello backend.

4. Stäng hello app och starta om den tooverify att hello nya objekt som du skapade är beständiga toohello lokalt Arkiv.

5. (Valfritt) Använd Visual Studio tooview toosee din Azure SQL Database-tabellen som hello data i hello backend-databasen inte har ändrats.

    Öppna i Visual Studio **Server Explorer**. Navigera tooyour databasen i **Azure**->**SQL-databaser**. Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**. Nu kan du bläddra tooyour SQL-databastabell och dess innehåll.

## <a name="optional-test-hello-reconnection-tooyour-mobile-backend"></a>(Valfritt) Testa hello återanslutning tooyour mobila serverdel

I det här avsnittet kan du återansluta hello app toohello mobilserverdel, som simulerar hello app komma tillbaka till onlineläge. När du loggar in är data synkroniserade tooyour mobilserverdel.

1. Öppna index.js och återställa hello programmets URL.
2. Öppna index.html och korrigera hello programmets URL i hello CSP `<meta>` element.
3. Bygg och kör hello klientapp. hello app försöker toosync med hello mobilappsserverdel efter inloggning. Kontrollera att inga undantag loggas i hello Felsökningskonsolen.
4. (Valfritt) Visa hello uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler. Meddelande hello data har synkroniserats mellan hello backend-databas och hello lokalt Arkiv.

    Observera hello data har synkroniserats mellan hello databasen och hello lokalt Arkiv och innehåller hello-objekt som du har lagt till medan kopplades från din app.

## <a name="additional-resources"></a>Ytterligare resurser
* [Datasynkronisering offline i Azure Mobile Apps]
* [Visual Studio Tools för Apache Cordova]

## <a name="next-steps"></a>Nästa steg
* Granska mer avancerade offlinesynkronisering funktioner, till exempel konfliktlösning i hello [offlinesynkronisering exempel]
* Granska hello offlinesynkronisering API-referens i hello [API-dokumentationen](https://azure.github.io/azure-mobile-apps-js-client).

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova-Snabbstart]: app-service-mobile-cordova-get-started.md
[synkronisering offline-exempel]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[Datasynkronisering offline i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with hello .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools för Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
