---
title: "Aktivera offlinesynkronisering för Azure Mobile App (Cordova) | Microsoft Docs"
description: "Lär dig mer om Apptjänst Mobile App till cache och synkronisera offlinedata i Cordova-program"
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
ms.openlocfilehash: 45e80ca672dfdb6defc6e5c1aac3d29f5479125c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a><span data-ttu-id="85325-103">Aktivera offlinesynkronisering av Cordova-mobilapp</span><span class="sxs-lookup"><span data-stu-id="85325-103">Enable offline sync for your Cordova mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

<span data-ttu-id="85325-104">Den här självstudiekursen beskriver funktionen offlinesynkronisering i Azure Mobile Apps för Cordova.</span><span class="sxs-lookup"><span data-stu-id="85325-104">This tutorial introduces the offline sync feature of Azure Mobile Apps for Cordova.</span></span> <span data-ttu-id="85325-105">Offlinesynkronisering kan slutanvändarna interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="85325-105">Offline sync allows end users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="85325-106">Ändringarna sparas i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="85325-106">Changes are stored in a local database.</span></span>  <span data-ttu-id="85325-107">När enheten är online igen kan har de här ändringarna synkroniserats med tjänsten remote.</span><span class="sxs-lookup"><span data-stu-id="85325-107">Once the device is back online, these changes are synced with the remote service.</span></span>

<span data-ttu-id="85325-108">Den här kursen är baserad på Cordova quickstart lösning för Mobilappar som du skapar när du har slutfört vägledningen [Apache Cordova Snabbstart].</span><span class="sxs-lookup"><span data-stu-id="85325-108">This tutorial is based on the Cordova quickstart solution for Mobile Apps that you create when you complete the tutorial [Apache Cordova quick start].</span></span> <span data-ttu-id="85325-109">I den här kursen kan du uppdatera quickstart lösningen för att lägga till offline funktioner i Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="85325-109">In this tutorial, you update the quickstart solution to add offline features of Azure Mobile Apps.</span></span>  <span data-ttu-id="85325-110">Vi kan också markera offline-specifika koden i appen.</span><span class="sxs-lookup"><span data-stu-id="85325-110">We also highlight the offline-specific code in the app.</span></span>

<span data-ttu-id="85325-111">Mer information om funktionen offlinesynkronisering finns i avsnittet [offlinesynkronisering Data i Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="85325-111">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps].</span></span> <span data-ttu-id="85325-112">Mer information om API-användning finns i [API-dokumentationen](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="85325-112">For details of API usage, see the [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

## <a name="add-offline-sync-to-the-quickstart-solution"></a><span data-ttu-id="85325-113">Lägga till offlinesynkronisering i Snabbstart-lösning</span><span class="sxs-lookup"><span data-stu-id="85325-113">Add offline sync to the quickstart solution</span></span>
<span data-ttu-id="85325-114">Synkronisering offline-koden måste läggas till appen.</span><span class="sxs-lookup"><span data-stu-id="85325-114">The offline sync code must be added to the app.</span></span> <span data-ttu-id="85325-115">Offlinesynkronisering kräver pluginprogrammet cordova-sqlite-storage hämtar läggs automatiskt till din app när plugin-program för Azure Mobile Apps ingår i projektet.</span><span class="sxs-lookup"><span data-stu-id="85325-115">Offline sync requires the cordova-sqlite-storage plugin, which automatically gets added to your app when the Azure Mobile Apps plugin is included in the project.</span></span> <span data-ttu-id="85325-116">Snabbstartsprojektet innehåller båda dessa plugin-program.</span><span class="sxs-lookup"><span data-stu-id="85325-116">The Quickstart project includes both of these plugins.</span></span>

1. <span data-ttu-id="85325-117">Öppna index.js i Visual Studio Solution Explorer och Ersätt följande kod</span><span class="sxs-lookup"><span data-stu-id="85325-117">In Visual Studio's Solution Explorer, open index.js and replace the following code</span></span>

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable;      // Reference to a table endpoint on backend

    <span data-ttu-id="85325-118">med den här koden:</span><span class="sxs-lookup"><span data-stu-id="85325-118">with this code:</span></span>

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable,      // Reference to a table endpoint on backend
           syncContext;        // Reference to offline data sync context

2. <span data-ttu-id="85325-119">Ersätt sedan följande kod:</span><span class="sxs-lookup"><span data-stu-id="85325-119">Next, replace the following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    <span data-ttu-id="85325-120">med den här koden:</span><span class="sxs-lookup"><span data-stu-id="85325-120">with this code:</span></span>

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

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    <span data-ttu-id="85325-121">Föregående kod tilläggen initiera det lokala arkivet och definiera en lokal tabell som matchar kolumnvärdena används i din Azure-serverdel.</span><span class="sxs-lookup"><span data-stu-id="85325-121">The preceding code additions initialize the local store and define a local table that matches the column values used in your Azure back end.</span></span> <span data-ttu-id="85325-122">(Du behöver inte inkludera alla kolumnvärdena i den här koden.)  Den `version` underhålls av mobila serverdel och används för konfliktlösning.</span><span class="sxs-lookup"><span data-stu-id="85325-122">(You don't need to include all column values in this code.)  The `version` field is maintained by the mobile backend and is used for conflict resolution.</span></span>

    <span data-ttu-id="85325-123">Du kan hämta en referens till sync-kontexten genom att anropa **getSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="85325-123">You get a reference to the sync context by calling **getSyncContext**.</span></span> <span data-ttu-id="85325-124">Kontexten sync bevaras relationerna mellan tabellerna genom att spåra och överföra ändringar i alla tabeller som ett klientprogram har ändrat när `.push()` anropas.</span><span class="sxs-lookup"><span data-stu-id="85325-124">The sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when `.push()` is called.</span></span>

3. <span data-ttu-id="85325-125">Uppdatera programmets URL till din Mobilapp programmets URL.</span><span class="sxs-lookup"><span data-stu-id="85325-125">Update the application URL to your Mobile App application URL.</span></span>

4. <span data-ttu-id="85325-126">Ersätt sedan den här koden:</span><span class="sxs-lookup"><span data-stu-id="85325-126">Next, replace this code:</span></span>

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    <span data-ttu-id="85325-127">med den här koden:</span><span class="sxs-lookup"><span data-stu-id="85325-127">with this code:</span></span>

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    <span data-ttu-id="85325-128">Föregående kod initierar sync-kontexten och anropar sedan getSyncTable (i stället för getTable) för att hämta en referens till den lokala tabellen.</span><span class="sxs-lookup"><span data-stu-id="85325-128">The preceding code initializes the sync context and then calls getSyncTable (instead of getTable) to get a reference to the local table.</span></span>

    <span data-ttu-id="85325-129">Den här koden använder den lokala databasen för alla skapa, läsa, uppdatera och ta bort CRUD-tabellåtgärder.</span><span class="sxs-lookup"><span data-stu-id="85325-129">This code uses the local database for all create, read, update, and delete (CRUD) table operations.</span></span>

    <span data-ttu-id="85325-130">Det här exemplet utför enkel felhantering på eventuella konflikter.</span><span class="sxs-lookup"><span data-stu-id="85325-130">This sample performs simple error handling on sync conflicts.</span></span> <span data-ttu-id="85325-131">Ett riktigt program skulle hantera olika fel som nätverksförhållanden och servern är i konflikt.</span><span class="sxs-lookup"><span data-stu-id="85325-131">A real application would handle the various errors like network conditions, server conflicts, and others.</span></span> <span data-ttu-id="85325-132">Kodexempel, finns det [offlinesynkronisering exempel].</span><span class="sxs-lookup"><span data-stu-id="85325-132">For code examples, see the [offline sync sample].</span></span>

5. <span data-ttu-id="85325-133">Lägg till den här funktionen om du vill utföra den faktiska synkroniseringen.</span><span class="sxs-lookup"><span data-stu-id="85325-133">Next, add this function to perform the actual sync.</span></span>

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    <span data-ttu-id="85325-134">Du har bestämt när att skicka ändringar till serverdelen för Mobilappen genom att anropa **syncContext.push()**.</span><span class="sxs-lookup"><span data-stu-id="85325-134">You decide when to push changes to the Mobile App backend by calling **syncContext.push()**.</span></span> <span data-ttu-id="85325-135">Du kan exempelvis kalla **syncBackend** i en händelsehanterare för knappen knuten till en knapp för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="85325-135">For example, you could call **syncBackend** in a button event handler tied to a sync button.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="85325-136">Offlinesynkronisering överväganden</span><span class="sxs-lookup"><span data-stu-id="85325-136">Offline sync considerations</span></span>

<span data-ttu-id="85325-137">I det här exemplet i **push** metod för **syncContext** endast anropas på appen startas i Återanropsfunktionen för inloggning.</span><span class="sxs-lookup"><span data-stu-id="85325-137">In the sample, the **push** method of **syncContext** is only called on app startup in the callback function for login.</span></span>  <span data-ttu-id="85325-138">I ett riktigt program kan du också göra funktionen sync utlöses manuellt eller när nätverket tillstånd ändras.</span><span class="sxs-lookup"><span data-stu-id="85325-138">In a real-world application, you could also make this sync functionality triggered manually or when the network state changes.</span></span>

<span data-ttu-id="85325-139">När en pull körs mot en tabell som har väntande spåras lokala uppdateringar av sammanhanget, som hämtar åtgärden automatiskt utlösare en push.</span><span class="sxs-lookup"><span data-stu-id="85325-139">When a pull is executed against a table that has pending local updates tracked by the context, that pull operation automatically triggers a push.</span></span> <span data-ttu-id="85325-140">När du uppdaterar, lägga till och slutföra objekt i det här exemplet kan du utelämna en explicit **push** anropa, eftersom det kan vara redundant.</span><span class="sxs-lookup"><span data-stu-id="85325-140">When refreshing, adding, and completing items in this sample, you can omit the explicit **push** call, since it may be redundant.</span></span>

<span data-ttu-id="85325-141">I den angivna koden, alla poster i tabellen remote todoItem frågas, men det är också möjligt att filtrera poster genom att skicka en fråge-id och fråga till **push**.</span><span class="sxs-lookup"><span data-stu-id="85325-141">In the provided code, all records in the remote todoItem table are queried, but it is also possible to filter records by passing a query id and query to **push**.</span></span> <span data-ttu-id="85325-142">Mer information finns i avsnittet *inkrementell synkronisering* i [offlinesynkronisering Data i Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="85325-142">For more information, see the section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="optional-disable-authentication"></a><span data-ttu-id="85325-143">(Valfritt) Inaktivera autentisering</span><span class="sxs-lookup"><span data-stu-id="85325-143">(Optional) Disable authentication</span></span>

<span data-ttu-id="85325-144">Om du inte vill konfigurera autentisering innan tester offlinesynkronisering kommentera ut Återanropsfunktionen för inloggning, men lämna kod inuti Återanropsfunktionen tagit bort kommentarstecken från.</span><span class="sxs-lookup"><span data-stu-id="85325-144">If you don't want to set up authentication before testing offline sync, comment out the callback function for login, but leave the code inside the callback function uncommented.</span></span>  <span data-ttu-id="85325-145">Efter kommentera bort inloggningen raderna koden som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="85325-145">After commenting out the login lines, the code follows:</span></span>

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

<span data-ttu-id="85325-146">Nu kan de app synkroniseras med Azure-serverdel när du kör appen.</span><span class="sxs-lookup"><span data-stu-id="85325-146">Now, the app syncs with the Azure backend when you run the app.</span></span>

## <a name="run-the-client-app"></a><span data-ttu-id="85325-147">Kör klientapp</span><span class="sxs-lookup"><span data-stu-id="85325-147">Run the client app</span></span>
<span data-ttu-id="85325-148">Med offlinesynkronisering har nu aktiverats, kan du köra klientprogrammet minst en gång på varje plattform för att fylla i databasen för lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="85325-148">With offline sync now enabled, you can run the client application at least once on each platform to populate the local store database.</span></span> <span data-ttu-id="85325-149">Senare, simulera ett offline scenario och ändra data i det lokala arkivet medan appen är offline.</span><span class="sxs-lookup"><span data-stu-id="85325-149">Later, simulate an offline scenario and modify the data in the local store while the app is offline.</span></span>

## <a name="optional-test-the-sync-behavior"></a><span data-ttu-id="85325-150">(Valfritt) Testa sync-beteende</span><span class="sxs-lookup"><span data-stu-id="85325-150">(Optional) Test the sync behavior</span></span>
<span data-ttu-id="85325-151">I det här avsnittet kan du ändra klientprojektet för att simulera ett offline-scenario med en ogiltig programmets URL för din serverdel.</span><span class="sxs-lookup"><span data-stu-id="85325-151">In this section, you modify the client project to simulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="85325-152">När du lägger till eller ändra dataobjekt ändringarna lagras i det lokala arkivet, men synkroniserats inte med datalagret backend tills anslutningen har återupprättats.</span><span class="sxs-lookup"><span data-stu-id="85325-152">When you add or change data items, these changes are held in the local store, but are not synced to the backend data store until the connection is re-established.</span></span>

1. <span data-ttu-id="85325-153">Öppna projektfilen index.js i Solution Explorer och ändra URL-Adressen för programmet att peka till en ogiltig URL som följande kod:</span><span class="sxs-lookup"><span data-stu-id="85325-153">In the Solution Explorer, open the index.js project file and change the application URL to point to  an invalid URL, like the following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. <span data-ttu-id="85325-154">Uppdatera CSP: N i index.html `<meta>` element med samma ogiltig URL.</span><span class="sxs-lookup"><span data-stu-id="85325-154">In index.html, update the CSP `<meta>` element with the same invalid URL.</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. <span data-ttu-id="85325-155">Skapa och köra klientappen och Lägg märke till att ett undantag loggas i konsolen när appen försöker synkronisera med serverdelen efter inloggning.</span><span class="sxs-lookup"><span data-stu-id="85325-155">Build and run the client app and notice that an exception is logged in the console when the app attempts to sync with the backend after login.</span></span> <span data-ttu-id="85325-156">Alla nya objekt som du lägger till finns bara i det lokala arkivet tills de pushas till mobila serverdel.</span><span class="sxs-lookup"><span data-stu-id="85325-156">Any new items you add exist only in the local store until they are pushed to the mobile backend.</span></span> <span data-ttu-id="85325-157">Klientappen fungerar som om den är ansluten till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="85325-157">The client app behaves as if it is connected to the backend.</span></span>

4. <span data-ttu-id="85325-158">Stänga appen och starta om den för att kontrollera att de nya objekt som du skapade sparas på det lokala arkivet.</span><span class="sxs-lookup"><span data-stu-id="85325-158">Close the app and restart it to verify that the new items you created are persisted to the local store.</span></span>

5. <span data-ttu-id="85325-159">(Valfritt) Du kan använda Visual Studio för att visa din Azure SQL Database-tabellen för att se data i databasen inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="85325-159">(Optional) Use Visual Studio to view your Azure SQL Database table to see that the data in the backend database has not changed.</span></span>

    <span data-ttu-id="85325-160">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="85325-160">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="85325-161">Navigera till din databas i **Azure**->**SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="85325-161">Navigate to your database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="85325-162">Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="85325-162">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="85325-163">Nu kan du bläddra till SQL-databastabell och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="85325-163">Now you can browse to your SQL database table and its contents.</span></span>

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a><span data-ttu-id="85325-164">(Valfritt) Testa återanslutning till din mobila serverdel</span><span class="sxs-lookup"><span data-stu-id="85325-164">(Optional) Test the reconnection to your mobile backend</span></span>

<span data-ttu-id="85325-165">I det här avsnittet kan du ansluta appen till mobila serverdelen som simulerar appen komma tillbaka till onlineläge.</span><span class="sxs-lookup"><span data-stu-id="85325-165">In this section, you reconnect the app to the mobile backend, which simulates the app coming back to an online state.</span></span> <span data-ttu-id="85325-166">När du loggar in, synkroniseras data till din mobila serverdel.</span><span class="sxs-lookup"><span data-stu-id="85325-166">When you log in, data is synced to your mobile backend.</span></span>

1. <span data-ttu-id="85325-167">Öppna index.js och återställa programmets URL.</span><span class="sxs-lookup"><span data-stu-id="85325-167">Reopen index.js and restore the application URL.</span></span>
2. <span data-ttu-id="85325-168">Öppna index.html och korrigera programmets URL i CSP `<meta>` element.</span><span class="sxs-lookup"><span data-stu-id="85325-168">Reopen index.html and correct the application URL in the CSP `<meta>` element.</span></span>
3. <span data-ttu-id="85325-169">Bygg och kör klientapp.</span><span class="sxs-lookup"><span data-stu-id="85325-169">Rebuild and run the client app.</span></span> <span data-ttu-id="85325-170">Appen försöker synkronisera med serverdelen för mobilappen efter inloggning.</span><span class="sxs-lookup"><span data-stu-id="85325-170">The app attempts to sync with the mobile app backend after login.</span></span> <span data-ttu-id="85325-171">Kontrollera att inga undantag loggas i Felsökningskonsolen.</span><span class="sxs-lookup"><span data-stu-id="85325-171">Verify that no exceptions are logged in the debug console.</span></span>
4. <span data-ttu-id="85325-172">(Valfritt) Visa den uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler.</span><span class="sxs-lookup"><span data-stu-id="85325-172">(Optional) View the updated data using either SQL Server Object Explorer or a REST tool like Fiddler.</span></span> <span data-ttu-id="85325-173">Observera att data har synkroniserats mellan backend-databas och det lokala arkivet.</span><span class="sxs-lookup"><span data-stu-id="85325-173">Notice the data has been synchronized between the backend database and the local store.</span></span>

    <span data-ttu-id="85325-174">Observera att data har synkroniserats mellan databasen och det lokala arkivet och innehåller objekt som du har lagt till medan kopplades från din app.</span><span class="sxs-lookup"><span data-stu-id="85325-174">Notice the data has been synchronized between the database and the local store and contains the items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85325-175">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="85325-175">Additional resources</span></span>
* <span data-ttu-id="85325-176">[offlinesynkronisering Data i Azure Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="85325-176">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="85325-177">[Visual Studio Tools för Apache Cordova]</span><span class="sxs-lookup"><span data-stu-id="85325-177">[Visual Studio Tools for Apache Cordova]</span></span>

## <a name="next-steps"></a><span data-ttu-id="85325-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85325-178">Next steps</span></span>
* <span data-ttu-id="85325-179">Granska mer avancerade offlinesynkronisering funktioner, till exempel konfliktlösning i den [offlinesynkronisering exempel]</span><span class="sxs-lookup"><span data-stu-id="85325-179">Review more advanced offline sync features such as conflict resolution in the [offline sync sample]</span></span>
* <span data-ttu-id="85325-180">Granska offlinesynkronisering API-referens i den [API-dokumentationen](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="85325-180">Review the offline sync API reference in the [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
<span data-ttu-id="85325-181">[Apache Cordova Snabbstart]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="85325-181">[Apache Cordova quick start]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="85325-182">[offlinesynkronisering exempel]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling</span><span class="sxs-lookup"><span data-stu-id="85325-182">[offline sync sample]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling</span></span>
<span data-ttu-id="85325-183">[offlinesynkronisering Data i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="85325-183">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
<span data-ttu-id="85325-184">[Visual Studio Tools för Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span><span class="sxs-lookup"><span data-stu-id="85325-184">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span></span>
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
