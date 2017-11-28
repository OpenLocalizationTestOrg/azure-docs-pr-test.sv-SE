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
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a><span data-ttu-id="a3733-103">Aktivera offlinesynkronisering av Cordova-mobilapp</span><span class="sxs-lookup"><span data-stu-id="a3733-103">Enable offline sync for your Cordova mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

<span data-ttu-id="a3733-104">Den här kursen introducerar hello offlinesynkronisering funktion i Azure Mobile Apps för Cordova.</span><span class="sxs-lookup"><span data-stu-id="a3733-104">This tutorial introduces hello offline sync feature of Azure Mobile Apps for Cordova.</span></span> <span data-ttu-id="a3733-105">Offlinesynkronisering kan slutet användare toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="a3733-105">Offline sync allows end users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="a3733-106">Ändringarna sparas i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="a3733-106">Changes are stored in a local database.</span></span>  <span data-ttu-id="a3733-107">När hello enheten är online igen, har ändringarna synkroniserats med hello fjärrtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a3733-107">Once hello device is back online, these changes are synced with hello remote service.</span></span>

<span data-ttu-id="a3733-108">Den här kursen är baserad på hello Cordova quickstart lösning för Mobilappar som du skapar när du slutför hello kursen [Apache Cordova Snabbstart].</span><span class="sxs-lookup"><span data-stu-id="a3733-108">This tutorial is based on hello Cordova quickstart solution for Mobile Apps that you create when you complete hello tutorial [Apache Cordova quick start].</span></span> <span data-ttu-id="a3733-109">I den här kursen kan du uppdatera hello quickstart lösning tooadd offline funktioner i Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="a3733-109">In this tutorial, you update hello quickstart solution tooadd offline features of Azure Mobile Apps.</span></span>  <span data-ttu-id="a3733-110">Vi kan också markera hello offline-specifik kod i hello app.</span><span class="sxs-lookup"><span data-stu-id="a3733-110">We also highlight hello offline-specific code in hello app.</span></span>

<span data-ttu-id="a3733-111">toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="a3733-111">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps].</span></span> <span data-ttu-id="a3733-112">Mer information om API-användning, finns hello [API-dokumentationen](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="a3733-112">For details of API usage, see hello [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

## <a name="add-offline-sync-toohello-quickstart-solution"></a><span data-ttu-id="a3733-113">Lägg till offlinesynkronisering toohello quickstart lösning</span><span class="sxs-lookup"><span data-stu-id="a3733-113">Add offline sync toohello quickstart solution</span></span>
<span data-ttu-id="a3733-114">hello offlinesynkronisering kod måste läggas till toohello app.</span><span class="sxs-lookup"><span data-stu-id="a3733-114">hello offline sync code must be added toohello app.</span></span> <span data-ttu-id="a3733-115">Offlinesynkronisering kräver hello cordova-sqlite-storage plugin-program, som hämtar läggs automatiskt tooyour app när hello Azure Mobile Apps plugin-programmet ingår i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a3733-115">Offline sync requires hello cordova-sqlite-storage plugin, which automatically gets added tooyour app when hello Azure Mobile Apps plugin is included in hello project.</span></span> <span data-ttu-id="a3733-116">Hej snabbstartsprojektet innehåller båda dessa plugin-program.</span><span class="sxs-lookup"><span data-stu-id="a3733-116">hello Quickstart project includes both of these plugins.</span></span>

1. <span data-ttu-id="a3733-117">Öppna index.js i Visual Studio Solution Explorer och Ersätt hello följande kod</span><span class="sxs-lookup"><span data-stu-id="a3733-117">In Visual Studio's Solution Explorer, open index.js and replace hello following code</span></span>

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable;      // Reference tooa table endpoint on backend

    <span data-ttu-id="a3733-118">med den här koden:</span><span class="sxs-lookup"><span data-stu-id="a3733-118">with this code:</span></span>

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable,      // Reference tooa table endpoint on backend
           syncContext;        // Reference toooffline data sync context

2. <span data-ttu-id="a3733-119">Ersätt sedan hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="a3733-119">Next, replace hello following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    <span data-ttu-id="a3733-120">med den här koden:</span><span class="sxs-lookup"><span data-stu-id="a3733-120">with this code:</span></span>

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

    <span data-ttu-id="a3733-121">hello föregående kod tillägg initiera hello lokalt Arkiv och definiera en lokal tabell som matchar hello kolumnvärdena används i din Azure-serverdel.</span><span class="sxs-lookup"><span data-stu-id="a3733-121">hello preceding code additions initialize hello local store and define a local table that matches hello column values used in your Azure back end.</span></span> <span data-ttu-id="a3733-122">(Du behöver inte tooinclude alla kolumnvärdena i den här koden.)  Hej `version` underhålls av hello mobilserverdel och används för konfliktlösning.</span><span class="sxs-lookup"><span data-stu-id="a3733-122">(You don't need tooinclude all column values in this code.)  hello `version` field is maintained by hello mobile backend and is used for conflict resolution.</span></span>

    <span data-ttu-id="a3733-123">Du får en kontext som referens toohello synkronisering genom att anropa **getSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="a3733-123">You get a reference toohello sync context by calling **getSyncContext**.</span></span> <span data-ttu-id="a3733-124">hello sync kontexten bevaras relationerna mellan tabellerna genom att spåra och överföra ändringar i alla tabeller som ett klientprogram har ändrat när `.push()` anropas.</span><span class="sxs-lookup"><span data-stu-id="a3733-124">hello sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when `.push()` is called.</span></span>

3. <span data-ttu-id="a3733-125">Uppdatera hello programmet URL tooyour Mobilapp programmets URL.</span><span class="sxs-lookup"><span data-stu-id="a3733-125">Update hello application URL tooyour Mobile App application URL.</span></span>

4. <span data-ttu-id="a3733-126">Ersätt sedan den här koden:</span><span class="sxs-lookup"><span data-stu-id="a3733-126">Next, replace this code:</span></span>

        todoItemTable = client.getTable('todoitem'); // todoitem is hello table name

    <span data-ttu-id="a3733-127">med den här koden:</span><span class="sxs-lookup"><span data-stu-id="a3733-127">with this code:</span></span>

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

    <span data-ttu-id="a3733-128">hello föregående kod initierar hello sync kontext och anropar sedan getSyncTable (i stället för getTable) tooget en toohello lokala referenstabell.</span><span class="sxs-lookup"><span data-stu-id="a3733-128">hello preceding code initializes hello sync context and then calls getSyncTable (instead of getTable) tooget a reference toohello local table.</span></span>

    <span data-ttu-id="a3733-129">Den här koden använder hello lokal databas för alla skapa, läsa, uppdatera och ta bort CRUD-tabellåtgärder.</span><span class="sxs-lookup"><span data-stu-id="a3733-129">This code uses hello local database for all create, read, update, and delete (CRUD) table operations.</span></span>

    <span data-ttu-id="a3733-130">Det här exemplet utför enkel felhantering på eventuella konflikter.</span><span class="sxs-lookup"><span data-stu-id="a3733-130">This sample performs simple error handling on sync conflicts.</span></span> <span data-ttu-id="a3733-131">Ett riktigt program skulle hantera hello olika fel som nätverksförhållanden och servern är i konflikt.</span><span class="sxs-lookup"><span data-stu-id="a3733-131">A real application would handle hello various errors like network conditions, server conflicts, and others.</span></span> <span data-ttu-id="a3733-132">Kodexempel finns hello [offlinesynkronisering exempel].</span><span class="sxs-lookup"><span data-stu-id="a3733-132">For code examples, see hello [offline sync sample].</span></span>

5. <span data-ttu-id="a3733-133">Lägg till funktionen tooperform hello faktiska synkroniseringen.</span><span class="sxs-lookup"><span data-stu-id="a3733-133">Next, add this function tooperform hello actual sync.</span></span>

        function syncBackend() {

          // Sync local store tooAzure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from hello Azure table after syncing tooAzure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    <span data-ttu-id="a3733-134">Du bestämmer dig för när toopush ändras toohello mobilappsserverdel genom att anropa **syncContext.push()**.</span><span class="sxs-lookup"><span data-stu-id="a3733-134">You decide when toopush changes toohello Mobile App backend by calling **syncContext.push()**.</span></span> <span data-ttu-id="a3733-135">Du kan exempelvis kalla **syncBackend** i en händelsehanterare bundet tooa sync knappen.</span><span class="sxs-lookup"><span data-stu-id="a3733-135">For example, you could call **syncBackend** in a button event handler tied tooa sync button.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="a3733-136">Offlinesynkronisering överväganden</span><span class="sxs-lookup"><span data-stu-id="a3733-136">Offline sync considerations</span></span>

<span data-ttu-id="a3733-137">I exemplet hello hello **push** metod för **syncContext** endast anropas på appen startas i hello Återanropsfunktionen för inloggning.</span><span class="sxs-lookup"><span data-stu-id="a3733-137">In hello sample, hello **push** method of **syncContext** is only called on app startup in hello callback function for login.</span></span>  <span data-ttu-id="a3733-138">I ett riktigt program kan du också göra funktionen sync utlöses manuellt eller när hello nätverket tillstånd ändras.</span><span class="sxs-lookup"><span data-stu-id="a3733-138">In a real-world application, you could also make this sync functionality triggered manually or when hello network state changes.</span></span>

<span data-ttu-id="a3733-139">När en pull körs mot en tabell som har väntande spåras lokala uppdateringar av hello kontext som hämtar åtgärden automatiskt utlösare en push.</span><span class="sxs-lookup"><span data-stu-id="a3733-139">When a pull is executed against a table that has pending local updates tracked by hello context, that pull operation automatically triggers a push.</span></span> <span data-ttu-id="a3733-140">När du uppdaterar, lägga till och slutföra objekt i det här exemplet, kan du utelämna hello explicit **push** anropa, eftersom det kan vara redundant.</span><span class="sxs-lookup"><span data-stu-id="a3733-140">When refreshing, adding, and completing items in this sample, you can omit hello explicit **push** call, since it may be redundant.</span></span>

<span data-ttu-id="a3733-141">I hello tillhandahålls kod alla poster i hello remote todoItem-tabell frågas, men är det också möjligt toofilter poster genom att skicka en fråge-id och fråga för**push**.</span><span class="sxs-lookup"><span data-stu-id="a3733-141">In hello provided code, all records in hello remote todoItem table are queried, but it is also possible toofilter records by passing a query id and query too**push**.</span></span> <span data-ttu-id="a3733-142">Mer information finns i avsnittet hello *inkrementell synkronisering* i [Offline datasynkronisering i Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="a3733-142">For more information, see hello section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="optional-disable-authentication"></a><span data-ttu-id="a3733-143">(Valfritt) Inaktivera autentisering</span><span class="sxs-lookup"><span data-stu-id="a3733-143">(Optional) Disable authentication</span></span>

<span data-ttu-id="a3733-144">Om du inte vill tooset konfigurerar autentisering innan du testar offlinesynkronisering, kommentera ut hello Återanropsfunktionen för inloggning, men lämna hello kod inuti hello Återanropsfunktionen tagit bort kommentarstecken från.</span><span class="sxs-lookup"><span data-stu-id="a3733-144">If you don't want tooset up authentication before testing offline sync, comment out hello callback function for login, but leave hello code inside hello callback function uncommented.</span></span>  <span data-ttu-id="a3733-145">Efter att kommentera bort hello inloggningen rader hello kod visas nedan:</span><span class="sxs-lookup"><span data-stu-id="a3733-145">After commenting out hello login lines, hello code follows:</span></span>

      // Login toohello service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave hello rest of hello code in this callback function  uncommented.
                ...
        });
      // }, handleError);

<span data-ttu-id="a3733-146">Nu hello app synkroniseras med hello Azure-serverdel när du kör hello app.</span><span class="sxs-lookup"><span data-stu-id="a3733-146">Now, hello app syncs with hello Azure backend when you run hello app.</span></span>

## <a name="run-hello-client-app"></a><span data-ttu-id="a3733-147">Kör hello-klientappen</span><span class="sxs-lookup"><span data-stu-id="a3733-147">Run hello client app</span></span>
<span data-ttu-id="a3733-148">Med offlinesynkronisering har nu aktiverats, kan du köra hello klientprogrammet minst en gång på varje plattform för att fylla i databasen för hello lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="a3733-148">With offline sync now enabled, you can run hello client application at least once on each platform to populate hello local store database.</span></span> <span data-ttu-id="a3733-149">Senare, simulera ett offline scenario och ändra hello data i hello lokalt Arkiv medan hello appen är offline.</span><span class="sxs-lookup"><span data-stu-id="a3733-149">Later, simulate an offline scenario and modify hello data in hello local store while hello app is offline.</span></span>

## <a name="optional-test-hello-sync-behavior"></a><span data-ttu-id="a3733-150">(Valfritt) Testa hello sync beteende</span><span class="sxs-lookup"><span data-stu-id="a3733-150">(Optional) Test hello sync behavior</span></span>
<span data-ttu-id="a3733-151">I det här avsnittet Ändra hello klienten projektet toosimulate ett offline-scenario med en ogiltig programmets URL för din serverdel.</span><span class="sxs-lookup"><span data-stu-id="a3733-151">In this section, you modify hello client project toosimulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="a3733-152">När du lägger till eller ändra dataobjekt ändringarna lagras i det lokala arkivet, men är inte synkroniserade toohello backend-datalagret förrän hello anslutningen har återupprättats.</span><span class="sxs-lookup"><span data-stu-id="a3733-152">When you add or change data items, these changes are held in the local store, but are not synced toohello backend data store until hello connection is re-established.</span></span>

1. <span data-ttu-id="a3733-153">Öppna projektfilen för hello index.js hello Solution Explorer, och ändra hello programmet URL toopoint till en ogiltig URL som hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="a3733-153">In hello Solution Explorer, open hello index.js project file and change hello application URL toopoint to  an invalid URL, like hello following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. <span data-ttu-id="a3733-154">Uppdatera hello CSP i index.html `<meta>` element med hello samma ogiltig URL.</span><span class="sxs-lookup"><span data-stu-id="a3733-154">In index.html, update hello CSP `<meta>` element with hello same invalid URL.</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. <span data-ttu-id="a3733-155">Skapa och kör hello klientapp och Lägg märke till att ett undantag loggas i konsolen hello när hello-appen försöker synkronisera med hello backend efter inloggning.</span><span class="sxs-lookup"><span data-stu-id="a3733-155">Build and run hello client app and notice that an exception is logged in hello console when hello app attempts to sync with hello backend after login.</span></span> <span data-ttu-id="a3733-156">Alla nya objekt som du lägger till finns bara i hello lokalt Arkiv tills de pushas toohello mobilserverdel.</span><span class="sxs-lookup"><span data-stu-id="a3733-156">Any new items you add exist only in hello local store until they are pushed toohello mobile backend.</span></span> <span data-ttu-id="a3733-157">hello-klientappen fungerar som om den är ansluten toohello backend.</span><span class="sxs-lookup"><span data-stu-id="a3733-157">hello client app behaves as if it is connected toohello backend.</span></span>

4. <span data-ttu-id="a3733-158">Stäng hello app och starta om den tooverify att hello nya objekt som du skapade är beständiga toohello lokalt Arkiv.</span><span class="sxs-lookup"><span data-stu-id="a3733-158">Close hello app and restart it tooverify that hello new items you created are persisted toohello local store.</span></span>

5. <span data-ttu-id="a3733-159">(Valfritt) Använd Visual Studio tooview toosee din Azure SQL Database-tabellen som hello data i hello backend-databasen inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="a3733-159">(Optional) Use Visual Studio tooview your Azure SQL Database table toosee that hello data in hello backend database has not changed.</span></span>

    <span data-ttu-id="a3733-160">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a3733-160">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="a3733-161">Navigera tooyour databasen i **Azure**->**SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="a3733-161">Navigate tooyour database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="a3733-162">Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a3733-162">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="a3733-163">Nu kan du bläddra tooyour SQL-databastabell och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="a3733-163">Now you can browse tooyour SQL database table and its contents.</span></span>

## <a name="optional-test-hello-reconnection-tooyour-mobile-backend"></a><span data-ttu-id="a3733-164">(Valfritt) Testa hello återanslutning tooyour mobila serverdel</span><span class="sxs-lookup"><span data-stu-id="a3733-164">(Optional) Test hello reconnection tooyour mobile backend</span></span>

<span data-ttu-id="a3733-165">I det här avsnittet kan du återansluta hello app toohello mobilserverdel, som simulerar hello app komma tillbaka till onlineläge.</span><span class="sxs-lookup"><span data-stu-id="a3733-165">In this section, you reconnect hello app toohello mobile backend, which simulates hello app coming back to an online state.</span></span> <span data-ttu-id="a3733-166">När du loggar in är data synkroniserade tooyour mobilserverdel.</span><span class="sxs-lookup"><span data-stu-id="a3733-166">When you log in, data is synced tooyour mobile backend.</span></span>

1. <span data-ttu-id="a3733-167">Öppna index.js och återställa hello programmets URL.</span><span class="sxs-lookup"><span data-stu-id="a3733-167">Reopen index.js and restore hello application URL.</span></span>
2. <span data-ttu-id="a3733-168">Öppna index.html och korrigera hello programmets URL i hello CSP `<meta>` element.</span><span class="sxs-lookup"><span data-stu-id="a3733-168">Reopen index.html and correct hello application URL in hello CSP `<meta>` element.</span></span>
3. <span data-ttu-id="a3733-169">Bygg och kör hello klientapp.</span><span class="sxs-lookup"><span data-stu-id="a3733-169">Rebuild and run hello client app.</span></span> <span data-ttu-id="a3733-170">hello app försöker toosync med hello mobilappsserverdel efter inloggning.</span><span class="sxs-lookup"><span data-stu-id="a3733-170">hello app attempts toosync with hello mobile app backend after login.</span></span> <span data-ttu-id="a3733-171">Kontrollera att inga undantag loggas i hello Felsökningskonsolen.</span><span class="sxs-lookup"><span data-stu-id="a3733-171">Verify that no exceptions are logged in hello debug console.</span></span>
4. <span data-ttu-id="a3733-172">(Valfritt) Visa hello uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler.</span><span class="sxs-lookup"><span data-stu-id="a3733-172">(Optional) View hello updated data using either SQL Server Object Explorer or a REST tool like Fiddler.</span></span> <span data-ttu-id="a3733-173">Meddelande hello data har synkroniserats mellan hello backend-databas och hello lokalt Arkiv.</span><span class="sxs-lookup"><span data-stu-id="a3733-173">Notice hello data has been synchronized between hello backend database and hello local store.</span></span>

    <span data-ttu-id="a3733-174">Observera hello data har synkroniserats mellan hello databasen och hello lokalt Arkiv och innehåller hello-objekt som du har lagt till medan kopplades från din app.</span><span class="sxs-lookup"><span data-stu-id="a3733-174">Notice hello data has been synchronized between hello database and hello local store and contains hello items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3733-175">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a3733-175">Additional resources</span></span>
* <span data-ttu-id="a3733-176">[Datasynkronisering offline i Azure Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="a3733-176">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="a3733-177">[Visual Studio Tools för Apache Cordova]</span><span class="sxs-lookup"><span data-stu-id="a3733-177">[Visual Studio Tools for Apache Cordova]</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3733-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a3733-178">Next steps</span></span>
* <span data-ttu-id="a3733-179">Granska mer avancerade offlinesynkronisering funktioner, till exempel konfliktlösning i hello [offlinesynkronisering exempel]</span><span class="sxs-lookup"><span data-stu-id="a3733-179">Review more advanced offline sync features such as conflict resolution in hello [offline sync sample]</span></span>
* <span data-ttu-id="a3733-180">Granska hello offlinesynkronisering API-referens i hello [API-dokumentationen](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="a3733-180">Review hello offline sync API reference in hello [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

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
