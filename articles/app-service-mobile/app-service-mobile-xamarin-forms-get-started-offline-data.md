---
title: "Aktivera offlinesynkronisering för Azure Mobile App (Xamarin.Forms) | Microsoft Docs"
description: "Lär dig mer om Apptjänst Mobile App till cache och synkronisera offlinedata i Xamarin.Forms-program"
documentationcenter: xamarin
author: ggailey777
manager: yochayk
editor: 
services: app-service\mobile
ms.assetid: acf0f874-3ea5-4410-bd22-b0e72140f3b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: glenga
ms.openlocfilehash: f2bed0a7124517319cc82405c4ab6b4d79aacfe1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a><span data-ttu-id="4b3e0-103">Aktivera synkronisering offline för din Xamarin.Forms-mobilapp</span><span class="sxs-lookup"><span data-stu-id="4b3e0-103">Enable offline sync for your Xamarin.Forms mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="4b3e0-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4b3e0-104">Overview</span></span>
<span data-ttu-id="4b3e0-105">Den här kursen introducerar funktionen offlinesynkronisering i Azure Mobilappar för Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-105">This tutorial introduces the offline sync feature of Azure Mobile Apps for Xamarin.Forms.</span></span> <span data-ttu-id="4b3e0-106">Offlinesynkronisering kan slutanvändarna interagera med en mobil app – visa, lägga till eller ändra data, även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-106">Offline sync allows end users to interact with a mobile app--viewing, adding, or modifying data--even when there is no network connection.</span></span> <span data-ttu-id="4b3e0-107">Ändringarna sparas i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-107">Changes are stored in a local database.</span></span> <span data-ttu-id="4b3e0-108">När enheten är online igen kan har de här ändringarna synkroniserats med tjänsten remote.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-108">Once the device is back online, these changes are synced with the remote service.</span></span>

<span data-ttu-id="4b3e0-109">Den här kursen är baserad på quickstart Xamarin.Forms-lösningen för Mobilappar som du skapar när du har slutfört vägledningen [skapa en Xamarin iOS-app].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-109">This tutorial is based on the Xamarin.Forms quickstart solution for Mobile Apps that you create when you complete the tutorial [Create a Xamarin iOS app].</span></span> <span data-ttu-id="4b3e0-110">Snabbstart-lösning för Xamarin.Forms innehåller koden för att stödja offlinesynkronisering som behöver aktiveras.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-110">The quickstart solution for Xamarin.Forms contains the code to support offline sync, which just needs to be enabled.</span></span> <span data-ttu-id="4b3e0-111">I den här kursen kan du uppdatera quickstart-lösningen för att aktivera Azure Mobile Apps offline funktioner.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-111">In this tutorial, you update the quickstart solution to turn on the offline features of Azure Mobile Apps.</span></span> <span data-ttu-id="4b3e0-112">Vi kan också markera offline-specifika koden i appen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-112">We also highlight the offline-specific code in the app.</span></span> <span data-ttu-id="4b3e0-113">Om du inte använder den hämta quickstart lösningen måste du lägga till data access-tilläggspaket projektet.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-113">If you do not use the downloaded quickstart solution, you must add the data access extension packages to your project.</span></span> <span data-ttu-id="4b3e0-114">Mer information om server tilläggspaket finns [arbeta med serverdelen .NET SDK för Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-114">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps][1].</span></span>

<span data-ttu-id="4b3e0-115">Mer information om funktionen offlinesynkronisering finns i avsnittet [offlinesynkronisering Data i Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-115">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a><span data-ttu-id="4b3e0-116">Aktivera offlinesynkronisering funktionerna i Snabbstart-lösningen</span><span class="sxs-lookup"><span data-stu-id="4b3e0-116">Enable offline sync functionality in the quickstart solution</span></span>
<span data-ttu-id="4b3e0-117">Koden offlinesynkronisering ingår i projektet med hjälp av C# preprocessor-direktiv.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-117">The offline sync code is included in the project by using C# preprocessor directives.</span></span> <span data-ttu-id="4b3e0-118">När den **OFFLINE\_SYNC\_AKTIVERAD** symbol har definierats, dessa kodsökvägar ingår i versionen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-118">When the **OFFLINE\_SYNC\_ENABLED** symbol is defined, these code paths are included in the build.</span></span> <span data-ttu-id="4b3e0-119">För Windows-appar måste du också installera SQLite-plattformen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-119">For Windows apps, you must also install the SQLite platform.</span></span>

1. <span data-ttu-id="4b3e0-120">Högerklicka på lösningen i Visual Studio > **hantera NuGet-paket för lösningen...** , och Sök efter och installera den **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paket för alla projekt i lösningen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-120">In Visual Studio, right-click the solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in the solution.</span></span>
2. <span data-ttu-id="4b3e0-121">Öppna filen TodoItemManager.cs i Solution Explorer från projektet med **bärbara** i namnet, som bärbara klassbiblioteket projekt, ta sedan bort kommentarerna följande preprocessor direktiv:</span><span class="sxs-lookup"><span data-stu-id="4b3e0-121">In the Solution Explorer, open the TodoItemManager.cs file from the project with **Portable** in the name, which is Portable Class Library project, then uncomment the following preprocessor directive:</span></span>

        #define OFFLINE_SYNC_ENABLED
3. <span data-ttu-id="4b3e0-122">(Valfritt) Installera en av följande SQLite runtime paket för att stödja Windows-enheter:</span><span class="sxs-lookup"><span data-stu-id="4b3e0-122">(Optional) To support Windows devices, install one of the following SQLite runtime packages:</span></span>

   * <span data-ttu-id="4b3e0-123">**Windows 8.1 Runtime:** installera [SQLite för Windows 8.1][3].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-123">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="4b3e0-124">**Windows Phone 8.1:** installera [SQLite för Windows Phone 8.1][4].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-124">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="4b3e0-125">**Universella Windows-plattformen** installera [SQLite för universell Windows Universal][5].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-125">**Universal Windows Platform** Install [SQLite for the Universal Windows Universal][5].</span></span>

     <span data-ttu-id="4b3e0-126">Även om Snabbstart inte innehåller en universell Windows-projektet, stöds universella Windows-plattformen med Xamarin Forms.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-126">Although the quickstart does not contain a Universal Windows project, the Universal Windows platform is supported with Xamarin Forms.</span></span>
4. <span data-ttu-id="4b3e0-127">(Valfritt) Högerklicka i varje Windows-approjekt **referenser** > **Lägg till referens...** , expandera den **Windows** mappen > **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-127">(Optional) In each Windows app project, right-click **References** > **Add Reference...**, expand the **Windows** folder > **Extensions**.</span></span>
    <span data-ttu-id="4b3e0-128">Aktivera lämpliga **SQLite för Windows** SDK tillsammans med den **Visual C++ 2013 Runtime för Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-128">Enable the appropriate **SQLite for Windows** SDK along with the **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="4b3e0-129">SQLite SDK namnen varierar något med varje Windows-plattformen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-129">The SQLite SDK names vary slightly with each Windows platform.</span></span>

## <a name="review-the-client-sync-code"></a><span data-ttu-id="4b3e0-130">Granska klientkod för synkronisering</span><span class="sxs-lookup"><span data-stu-id="4b3e0-130">Review the client sync code</span></span>
<span data-ttu-id="4b3e0-131">Här är en kort översikt över vad ingår redan i självstudiekursen kod i den `#if OFFLINE_SYNC_ENABLED` direktiven.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-131">Here is a brief overview of what is already included in the tutorial code inside the `#if OFFLINE_SYNC_ENABLED` directives.</span></span> <span data-ttu-id="4b3e0-132">Synkronisering offline-funktionen är i projektfilen TodoItemManager.cs i bärbara klassbiblioteket projektet.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-132">The offline sync functionality is in the TodoItemManager.cs project file in the Portable Class Library project.</span></span> <span data-ttu-id="4b3e0-133">En översikt över funktionen finns [Offline datasynkronisering i Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-133">For a conceptual overview of the feature, see [Offline Data Sync in Azure Mobile Apps][2].</span></span>

* <span data-ttu-id="4b3e0-134">Det lokala arkivet måste initieras innan alla tabellåtgärder kan utföras.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-134">Before any table operations can be performed, the local store must be initialized.</span></span> <span data-ttu-id="4b3e0-135">Lokalt Arkiv databasen initieras i den **TodoItemManager** klasskonstruktor med hjälp av följande kod:</span><span class="sxs-lookup"><span data-stu-id="4b3e0-135">The local store database is initialized in the **TodoItemManager** class constructor by using the following code:</span></span>

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    <span data-ttu-id="4b3e0-136">Den här koden skapar en ny lokal SQLite databasen med hjälp av **MobileServiceSQLiteStore** klass.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-136">This code creates a new local SQLite database using the **MobileServiceSQLiteStore** class.</span></span>

    <span data-ttu-id="4b3e0-137">Den **DefineTable** metoden skapar en tabell i det lokala arkivet som matchar fälten i den angivna typen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-137">The **DefineTable** method creates a table in the local store that matches the fields in the provided type.</span></span>  <span data-ttu-id="4b3e0-138">Typen måste inte innehålla alla kolumner som tillhör fjärrdatabasen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-138">The type doesn't have to include all the columns that are in the remote database.</span></span> <span data-ttu-id="4b3e0-139">Det är möjligt att lagra en delmängd med kolumner.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-139">It is possible to store a subset of columns.</span></span>
* <span data-ttu-id="4b3e0-140">Den **todoTable** i **TodoItemManager** är en **IMobileServiceSyncTable** Skriv i stället för **IMobileServiceTable**.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-140">The **todoTable** field in **TodoItemManager** is an **IMobileServiceSyncTable** type instead of **IMobileServiceTable**.</span></span> <span data-ttu-id="4b3e0-141">Den här klassen använder den lokala databasen för alla skapa, läsa, uppdatera och ta bort CRUD-tabellåtgärder.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-141">This class uses the local database for all create, read, update, and delete (CRUD) table operations.</span></span> <span data-ttu-id="4b3e0-142">Du bestämmer dig för när ändringarna pushas till serverdelen för Mobilappen genom att anropa **PushAsync** på den **IMobileServiceSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-142">You decide when those changes are pushed to the Mobile App backend by calling **PushAsync** on the **IMobileServiceSyncContext**.</span></span> <span data-ttu-id="4b3e0-143">Kontexten sync bevaras relationerna mellan tabellerna genom att spåra och överföra ändringar i alla tabeller som ett klientprogram har ändrat när **PushAsync** anropas.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-143">The sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when **PushAsync** is called.</span></span>

    <span data-ttu-id="4b3e0-144">Följande **SyncAsync** metoden anropas ska synkroniseras med serverdelen för Mobilappen:</span><span class="sxs-lookup"><span data-stu-id="4b3e0-144">The following **SyncAsync** method is called to sync with the Mobile App backend:</span></span>

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    <span data-ttu-id="4b3e0-145">Det här exemplet använder enkla felhantering med standardprogram för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-145">This sample uses simple error handling with the default sync handler.</span></span> <span data-ttu-id="4b3e0-146">Ett riktigt program skulle hantera olika fel som nätverksförhållanden och servern är i konflikt med hjälp av en anpassad **IMobileServiceSyncHandler** implementering.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-146">A real application would handle the various errors like network conditions and server conflicts by using a custom **IMobileServiceSyncHandler** implementation.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="4b3e0-147">Offlinesynkronisering överväganden</span><span class="sxs-lookup"><span data-stu-id="4b3e0-147">Offline sync considerations</span></span>
<span data-ttu-id="4b3e0-148">I det här exemplet i **SyncAsync** är bara att anropa metoden på Start- och när en synkronisering har begärts.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-148">In the sample, the **SyncAsync** method is only called on start-up and when a sync is requested.</span></span>  <span data-ttu-id="4b3e0-149">Om du vill initiera en synkronisering i en Android eller iOS-app dra nedåt i listan objekt; Windows, Använd den **Sync** knappen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-149">To initiate a sync in an Android or iOS app, pull down on the items list; for Windows, use the **Sync** button.</span></span> <span data-ttu-id="4b3e0-150">I ett riktigt program kan du också göra sync utlösaren när nätverket tillstånd ändras.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-150">In a real-world application, you could also make the sync trigger when the network state changes.</span></span>

<span data-ttu-id="4b3e0-151">När en pull körs mot en tabell som har väntande spåras lokala uppdateringar av sammanhanget, som hämtar åtgärden automatiskt utlösare en föregående kontexten push.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-151">When a pull is executed against a table that has pending local updates tracked by the context, that pull operation automatically triggers a preceding context push.</span></span> <span data-ttu-id="4b3e0-152">När du uppdaterar, lägga till och slutföra objekt i det här exemplet kan du utelämna en explicit **PushAsync** anropa.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-152">When refreshing, adding, and completing items in this sample, you can omit the explicit **PushAsync** call.</span></span>

<span data-ttu-id="4b3e0-153">I den angivna koden, alla poster i fjärrtabellen TodoItem frågas, men det är också möjligt att filtrera poster genom att skicka en fråge-id och fråga till **PushAsync**.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-153">In the provided code, all records in the remote TodoItem table are queried, but it is also possible to filter records by passing a query id and query to **PushAsync**.</span></span> <span data-ttu-id="4b3e0-154">Mer information finns i avsnittet *inkrementell synkronisering* i [Offline datasynkronisering i Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-154">For more information, see the section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="run-the-client-app"></a><span data-ttu-id="4b3e0-155">Kör klientapp</span><span class="sxs-lookup"><span data-stu-id="4b3e0-155">Run the client app</span></span>
<span data-ttu-id="4b3e0-156">Med offlinesynkronisering har nu aktiverats, kör du klientprogrammet minst en gång på varje plattform för att fylla i databasen för lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-156">With offline sync now enabled, run the client application at least once on each platform to populate the local store database.</span></span> <span data-ttu-id="4b3e0-157">Senare, simulera ett offline scenario och ändra data i det lokala arkivet medan appen är offline.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-157">Later, simulate an offline scenario and modify the data in the local store while the app is offline.</span></span>

## <a name="update-the-sync-behavior-of-the-client-app"></a><span data-ttu-id="4b3e0-158">Uppdatera sync beteendet för klientappen</span><span class="sxs-lookup"><span data-stu-id="4b3e0-158">Update the sync behavior of the client app</span></span>
<span data-ttu-id="4b3e0-159">Ändra klientprojektet för att simulera ett offline-scenario med en ogiltig programmets URL för din serverdel i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-159">In this section, modify the client project to simulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="4b3e0-160">Du kan också kan du stänga av Nätverksanslutningar genom att flytta din enhet till ”Flygplansläge”.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-160">Alternatively, you can turn off network connections by moving your device to "Airplane mode."</span></span>  <span data-ttu-id="4b3e0-161">När du lägger till eller ändra dataobjekt ändringarna lagras i det lokala arkivet, men har inte synkroniserats till datalagret backend tills anslutningen har återupprättats.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-161">When you add or change data items, these changes are held in the local store, but not synced to the backend data store until the connection is re-established.</span></span>

1. <span data-ttu-id="4b3e0-162">I Solution Explorer, Öppna projektfilen Constants.cs från den **bärbar** projektet och ändra värdet för `ApplicationURL` att peka till en ogiltig URL:</span><span class="sxs-lookup"><span data-stu-id="4b3e0-162">In the Solution Explorer, open the Constants.cs project file from the **Portable** project and change the value of `ApplicationURL` to point to an invalid URL:</span></span>

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. <span data-ttu-id="4b3e0-163">Öppna filen TodoItemManager.cs från den **bärbar** projektet och sedan lägga till en **fånga** för basen **undantag** undergrupp till den **försök... catch** blockera i **SyncAsync**.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-163">Open the TodoItemManager.cs file from the **Portable** project, then add a **catch** for the base **Exception** class to the **try...catch** block in **SyncAsync**.</span></span> <span data-ttu-id="4b3e0-164">Detta **fånga** block skriver Undantagsmeddelandet till konsolen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4b3e0-164">This **catch** block writes the exception message to the console, as follows:</span></span>

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. <span data-ttu-id="4b3e0-165">Skapa och köra klientappen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-165">Build and run the client app.</span></span>  <span data-ttu-id="4b3e0-166">Lägga till några nya objekt.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-166">Add some new items.</span></span> <span data-ttu-id="4b3e0-167">Observera att ett undantag loggas i konsolen för varje försök att synkronisera med serverdelen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-167">Notice that an exception is logged in the console for each attempt to sync with the backend.</span></span> <span data-ttu-id="4b3e0-168">Dessa nya objekt finns bara i det lokala arkivet tills de flyttas till den mobila serverdelen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-168">These new items exist only in the local store until they can be pushed to the mobile backend.</span></span> <span data-ttu-id="4b3e0-169">Klientappen fungerar som om den är ansluten till backend, stöder alla skapa, läsa, uppdatera, borttagningsåtgärder (CRUD).</span><span class="sxs-lookup"><span data-stu-id="4b3e0-169">The client app behaves as if it is connected to the backend, supporting all create, read, update, delete (CRUD) operations.</span></span>
4. <span data-ttu-id="4b3e0-170">Stänga appen och starta om den för att kontrollera att de nya objekt som du skapade sparas på det lokala arkivet.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-170">Close the app and restart it to verify that the new items you created are persisted to the local store.</span></span>
5. <span data-ttu-id="4b3e0-171">(Valfritt) Du kan använda Visual Studio för att visa din Azure SQL Database-tabellen för att se data i databasen inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-171">(Optional) Use Visual Studio to view your Azure SQL Database table to see that the data in the backend database has not changed.</span></span>

    <span data-ttu-id="4b3e0-172">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-172">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="4b3e0-173">Navigera till din databas i **Azure**->**SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-173">Navigate to your database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="4b3e0-174">Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-174">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="4b3e0-175">Nu kan du bläddra till SQL-databastabell och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-175">Now you can browse to your SQL database table and its contents.</span></span>

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a><span data-ttu-id="4b3e0-176">Uppdatera klientappen för att återansluta din mobila serverdel</span><span class="sxs-lookup"><span data-stu-id="4b3e0-176">Update the client app to reconnect your mobile backend</span></span>
<span data-ttu-id="4b3e0-177">Anslut appen till mobila serverdelen som simulerar appen komma tillbaka till onlineläge i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-177">In this section, reconnect the app to the mobile backend, which simulates the app coming back to an online state.</span></span> <span data-ttu-id="4b3e0-178">När du utför en uppdatering gest, synkroniseras data till din mobila serverdel.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-178">When you perform the refresh gesture, data is synced to your mobile backend.</span></span>

1. <span data-ttu-id="4b3e0-179">Öppna Constants.cs.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-179">Reopen Constants.cs.</span></span> <span data-ttu-id="4b3e0-180">Korrigera det `applicationURL` att peka till rätt URL.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-180">Correct the `applicationURL` to point to the correct URL.</span></span>
2. <span data-ttu-id="4b3e0-181">Bygg och kör klientapp.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-181">Rebuild and run the client app.</span></span> <span data-ttu-id="4b3e0-182">Appen försöker synkronisera med serverdelen för mobilappen när du startar.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-182">The app attempts to sync with the mobile app backend after launching.</span></span> <span data-ttu-id="4b3e0-183">Kontrollera att inga undantag loggas i Felsökningskonsolen.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-183">Verify that no exceptions are logged in the debug console.</span></span>
3. <span data-ttu-id="4b3e0-184">(Valfritt) Visa den uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler eller [Postman][6].</span><span class="sxs-lookup"><span data-stu-id="4b3e0-184">(Optional) View the updated data using either SQL Server Object Explorer or a REST tool like Fiddler or [Postman][6].</span></span> <span data-ttu-id="4b3e0-185">Observera att data har synkroniserats mellan backend-databas och det lokala arkivet.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-185">Notice the data has been synchronized between the backend database and the local store.</span></span>

    <span data-ttu-id="4b3e0-186">Observera att data har synkroniserats mellan databasen och det lokala arkivet och innehåller objekt som du har lagt till medan kopplades från din app.</span><span class="sxs-lookup"><span data-stu-id="4b3e0-186">Notice the data has been synchronized between the database and the local store and contains the items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b3e0-187">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4b3e0-187">Additional Resources</span></span>
* <span data-ttu-id="4b3e0-188">[Datasynkronisering offline i Azure Mobile Apps][2]</span><span class="sxs-lookup"><span data-stu-id="4b3e0-188">[Offline Data Sync in Azure Mobile Apps][2]</span></span>
* <span data-ttu-id="4b3e0-189">[Azure Mobile Apps .NET SDK ta][8]</span><span class="sxs-lookup"><span data-stu-id="4b3e0-189">[Azure Mobile Apps .NET SDK HOWTO][8]</span></span>

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
