---
title: "aaaEnable offlinesynkronisering för Azure Mobile App (Xamarin.Forms) | Microsoft Docs"
description: "Lär dig hur toouse Apptjänst Mobile App toocache och synkronisera offlinedata i Xamarin.Forms-program"
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
ms.openlocfilehash: 4b41404fc9507e82068fdf8aa612d57cbeb366bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a><span data-ttu-id="8a7eb-103">Aktivera synkronisering offline för din Xamarin.Forms-mobilapp</span><span class="sxs-lookup"><span data-stu-id="8a7eb-103">Enable offline sync for your Xamarin.Forms mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="8a7eb-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8a7eb-104">Overview</span></span>
<span data-ttu-id="8a7eb-105">Den här kursen introducerar hello offlinesynkronisering funktion i Azure Mobilappar för Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-105">This tutorial introduces hello offline sync feature of Azure Mobile Apps for Xamarin.Forms.</span></span> <span data-ttu-id="8a7eb-106">Offlinesynkronisering kan slutanvändarna interagera med en mobil app – visa, lägga till eller ändra data, även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-106">Offline sync allows end users to interact with a mobile app--viewing, adding, or modifying data--even when there is no network connection.</span></span> <span data-ttu-id="8a7eb-107">Ändringarna sparas i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-107">Changes are stored in a local database.</span></span> <span data-ttu-id="8a7eb-108">När hello enheten är online igen, har ändringarna synkroniserats med hello fjärrtjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-108">Once hello device is back online, these changes are synced with hello remote service.</span></span>

<span data-ttu-id="8a7eb-109">Den här kursen är baserad på hello quickstart Xamarin.Forms-lösningen för Mobilappar som du skapar när du har slutfört vägledningen [skapa en Xamarin iOS-app].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-109">This tutorial is based on hello Xamarin.Forms quickstart solution for Mobile Apps that you create when you complete the tutorial [Create a Xamarin iOS app].</span></span> <span data-ttu-id="8a7eb-110">hello quickstart lösning för Xamarin.Forms innehåller hello kod toosupport offlinesynkronisering, som bara behöver toobe aktiverat.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-110">hello quickstart solution for Xamarin.Forms contains hello code toosupport offline sync, which just needs toobe enabled.</span></span> <span data-ttu-id="8a7eb-111">I den här kursen kan du uppdatera hello quickstart lösning tooturn på hello offline funktioner i Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-111">In this tutorial, you update hello quickstart solution tooturn on hello offline features of Azure Mobile Apps.</span></span> <span data-ttu-id="8a7eb-112">Vi kan också markera hello offline-specifik kod i hello app.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-112">We also highlight hello offline-specific code in hello app.</span></span> <span data-ttu-id="8a7eb-113">Om du inte använder hello hämtas quickstart lösningen måste du lägga till hello data access paket tooyour tilläggsprojekt.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-113">If you do not use hello downloaded quickstart solution, you must add hello data access extension packages tooyour project.</span></span> <span data-ttu-id="8a7eb-114">Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-114">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps][1].</span></span>

<span data-ttu-id="8a7eb-115">toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-115">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="enable-offline-sync-functionality-in-hello-quickstart-solution"></a><span data-ttu-id="8a7eb-116">Aktivera offlinesynkronisering funktioner i hello quickstart lösning</span><span class="sxs-lookup"><span data-stu-id="8a7eb-116">Enable offline sync functionality in hello quickstart solution</span></span>
<span data-ttu-id="8a7eb-117">hello offlinesynkronisering kod ingår i hello-projekt med hjälp av C# preprocessor-direktiv.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-117">hello offline sync code is included in hello project by using C# preprocessor directives.</span></span> <span data-ttu-id="8a7eb-118">När hello **OFFLINE\_SYNC\_AKTIVERAD** symbol har definierats, dessa kodsökvägar ingår i hello build.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-118">When hello **OFFLINE\_SYNC\_ENABLED** symbol is defined, these code paths are included in hello build.</span></span> <span data-ttu-id="8a7eb-119">För Windows-appar måste du också installera hello SQLite plattform.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-119">For Windows apps, you must also install hello SQLite platform.</span></span>

1. <span data-ttu-id="8a7eb-120">Högerklicka på hello lösningen i Visual Studio > **hantera NuGet-paket för lösningen...** , och Sök efter och installera den **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paket för alla projekt i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-120">In Visual Studio, right-click hello solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in hello solution.</span></span>
2. <span data-ttu-id="8a7eb-121">I hello Solution Explorer, öppna hello TodoItemManager.cs filen från hello-projekt med **bärbara** i hello namn som är bärbara klassbiblioteket projekt, ta sedan bort kommentarerna hello följande preprocessor direktiv:</span><span class="sxs-lookup"><span data-stu-id="8a7eb-121">In hello Solution Explorer, open hello TodoItemManager.cs file from hello project with **Portable** in hello name, which is Portable Class Library project, then uncomment hello following preprocessor directive:</span></span>

        #define OFFLINE_SYNC_ENABLED
3. <span data-ttu-id="8a7eb-122">(Valfritt) toosupport Windows-enheter, installera en av följande SQLite runtime paket hello:</span><span class="sxs-lookup"><span data-stu-id="8a7eb-122">(Optional) toosupport Windows devices, install one of hello following SQLite runtime packages:</span></span>

   * <span data-ttu-id="8a7eb-123">**Windows 8.1 Runtime:** installera [SQLite för Windows 8.1][3].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-123">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="8a7eb-124">**Windows Phone 8.1:** installera [SQLite för Windows Phone 8.1][4].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-124">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="8a7eb-125">**Uwp** installera [SQLite för hello Universal Windows Universal][5].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-125">**Universal Windows Platform** Install [SQLite for hello Universal Windows Universal][5].</span></span>

     <span data-ttu-id="8a7eb-126">Även om hello Snabbstart inte innehåller en universell Windows-projektet, stöds hello universella Windows-plattformen med Xamarin Forms.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-126">Although hello quickstart does not contain a Universal Windows project, hello Universal Windows platform is supported with Xamarin Forms.</span></span>
4. <span data-ttu-id="8a7eb-127">(Valfritt) Högerklicka i varje Windows-approjekt **referenser** > **Lägg till referens...** , expandera hello **Windows** mappen > **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-127">(Optional) In each Windows app project, right-click **References** > **Add Reference...**, expand hello **Windows** folder > **Extensions**.</span></span>
    <span data-ttu-id="8a7eb-128">Aktivera lämpliga hello **SQLite för Windows** SDK tillsammans med hello **Visual C++ 2013 Runtime för Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-128">Enable hello appropriate **SQLite for Windows** SDK along with hello **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="8a7eb-129">Hej varierar SQLite SDK namn något med varje Windows-plattformen.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-129">hello SQLite SDK names vary slightly with each Windows platform.</span></span>

## <a name="review-hello-client-sync-code"></a><span data-ttu-id="8a7eb-130">Granska hello klientkod för synkronisering</span><span class="sxs-lookup"><span data-stu-id="8a7eb-130">Review hello client sync code</span></span>
<span data-ttu-id="8a7eb-131">Här är en kort översikt över vad ingår redan i hello självstudiekursen kod inuti hello `#if OFFLINE_SYNC_ENABLED` direktiven.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-131">Here is a brief overview of what is already included in hello tutorial code inside hello `#if OFFLINE_SYNC_ENABLED` directives.</span></span> <span data-ttu-id="8a7eb-132">Offlinesynkronisering-funktionen är i hello TodoItemManager.cs projektfilen i hello bärbara klassbiblioteket projekt.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-132">The offline sync functionality is in hello TodoItemManager.cs project file in hello Portable Class Library project.</span></span> <span data-ttu-id="8a7eb-133">En översikt över funktionen hello finns [Offline datasynkronisering i Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-133">For a conceptual overview of hello feature, see [Offline Data Sync in Azure Mobile Apps][2].</span></span>

* <span data-ttu-id="8a7eb-134">Hello lokalt arkiv måste initieras innan alla tabellåtgärder kan utföras.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-134">Before any table operations can be performed, hello local store must be initialized.</span></span> <span data-ttu-id="8a7eb-135">hello lokalt Arkiv databasen initieras i hello **TodoItemManager** klasskonstruktor med hjälp av hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="8a7eb-135">hello local store database is initialized in hello **TodoItemManager** class constructor by using hello following code:</span></span>

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    <span data-ttu-id="8a7eb-136">Den här koden skapar en ny lokal SQLite-databas med hello **MobileServiceSQLiteStore** klass.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-136">This code creates a new local SQLite database using hello **MobileServiceSQLiteStore** class.</span></span>

    <span data-ttu-id="8a7eb-137">Hej **DefineTable** metoden skapar en tabell i hello lokala arkivet som matchar hello fält i hello angivna typen.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-137">hello **DefineTable** method creates a table in hello local store that matches hello fields in hello provided type.</span></span>  <span data-ttu-id="8a7eb-138">hello typen har ingen tooinclude alla hello-kolumner i hello fjärrdatabasen.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-138">hello type doesn't have tooinclude all hello columns that are in hello remote database.</span></span> <span data-ttu-id="8a7eb-139">Det är möjligt toostore en delmängd med kolumner.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-139">It is possible toostore a subset of columns.</span></span>
* <span data-ttu-id="8a7eb-140">Hej **todoTable** i **TodoItemManager** är en **IMobileServiceSyncTable** Skriv i stället för **IMobileServiceTable**.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-140">hello **todoTable** field in **TodoItemManager** is an **IMobileServiceSyncTable** type instead of **IMobileServiceTable**.</span></span> <span data-ttu-id="8a7eb-141">Den här klassen använder hello lokal databas för alla skapa, läsa, uppdatera och ta bort CRUD-tabellåtgärder.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-141">This class uses hello local database for all create, read, update, and delete (CRUD) table operations.</span></span> <span data-ttu-id="8a7eb-142">Du bestämmer dig för när ändringarna är pushas toohello mobilappsserverdel genom att anropa **PushAsync** på hello **IMobileServiceSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-142">You decide when those changes are pushed toohello Mobile App backend by calling **PushAsync** on hello **IMobileServiceSyncContext**.</span></span> <span data-ttu-id="8a7eb-143">hello sync kontexten bevaras relationerna mellan tabellerna genom att spåra och överföra ändringar i alla tabeller som ett klientprogram har ändrat när **PushAsync** anropas.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-143">hello sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when **PushAsync** is called.</span></span>

    <span data-ttu-id="8a7eb-144">hello följande **SyncAsync** metoden anropas toosync med hello mobilappsserverdel:</span><span class="sxs-lookup"><span data-stu-id="8a7eb-144">hello following **SyncAsync** method is called toosync with hello Mobile App backend:</span></span>

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
                        //Update failed, reverting tooserver's copy.
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

    <span data-ttu-id="8a7eb-145">Det här exemplet använder enkla felhantering med hello standardprogram för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-145">This sample uses simple error handling with hello default sync handler.</span></span> <span data-ttu-id="8a7eb-146">Ett riktigt program skulle hantera hello olika fel som nätverksförhållanden och servern är i konflikt med hjälp av en anpassad **IMobileServiceSyncHandler** implementering.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-146">A real application would handle hello various errors like network conditions and server conflicts by using a custom **IMobileServiceSyncHandler** implementation.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="8a7eb-147">Offlinesynkronisering överväganden</span><span class="sxs-lookup"><span data-stu-id="8a7eb-147">Offline sync considerations</span></span>
<span data-ttu-id="8a7eb-148">I exemplet hello hello **SyncAsync** är bara att anropa metoden på Start- och när en synkronisering har begärts.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-148">In hello sample, hello **SyncAsync** method is only called on start-up and when a sync is requested.</span></span>  <span data-ttu-id="8a7eb-149">tooinitiate en synkronisering i en Android eller iOS-app, nedrullningsbara på listan över hello objekt; för Windows, Använd hello **Sync** knappen.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-149">tooinitiate a sync in an Android or iOS app, pull down on hello items list; for Windows, use hello **Sync** button.</span></span> <span data-ttu-id="8a7eb-150">I ett riktigt program kan du också göra hello sync utlösare när hello nätverket tillstånd ändras.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-150">In a real-world application, you could also make hello sync trigger when hello network state changes.</span></span>

<span data-ttu-id="8a7eb-151">När en pull körs mot en tabell som har väntande spåras lokala uppdateringar av hello kontext som hämtar åtgärden automatiskt utlösare en föregående kontexten push.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-151">When a pull is executed against a table that has pending local updates tracked by hello context, that pull operation automatically triggers a preceding context push.</span></span> <span data-ttu-id="8a7eb-152">När du uppdaterar, lägga till och slutföra objekt i det här exemplet, kan du utelämna hello explicit **PushAsync** anropa.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-152">When refreshing, adding, and completing items in this sample, you can omit hello explicit **PushAsync** call.</span></span>

<span data-ttu-id="8a7eb-153">I hello tillhandahålls kod alla poster i TodoItem hello fjärrtabell frågas, men är det också möjligt toofilter poster genom att skicka en fråge-id och fråga för**PushAsync**.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-153">In hello provided code, all records in hello remote TodoItem table are queried, but it is also possible toofilter records by passing a query id and query too**PushAsync**.</span></span> <span data-ttu-id="8a7eb-154">Mer information finns i avsnittet hello *inkrementell synkronisering* i [Offline datasynkronisering i Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-154">For more information, see hello section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="run-hello-client-app"></a><span data-ttu-id="8a7eb-155">Kör hello-klientappen</span><span class="sxs-lookup"><span data-stu-id="8a7eb-155">Run hello client app</span></span>
<span data-ttu-id="8a7eb-156">Med offlinesynkronisering har nu aktiverats, kör du hello klientprogrammet minst en gång för varje plattform toopopulate hello lokalt Arkiv-databasen.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-156">With offline sync now enabled, run hello client application at least once on each platform toopopulate hello local store database.</span></span> <span data-ttu-id="8a7eb-157">Senare, simulera ett offline scenario och ändra hello data i hello lokalt Arkiv medan hello appen är offline.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-157">Later, simulate an offline scenario and modify hello data in hello local store while hello app is offline.</span></span>

## <a name="update-hello-sync-behavior-of-hello-client-app"></a><span data-ttu-id="8a7eb-158">Uppdatera hello sync funktionssätt hello-klientappen</span><span class="sxs-lookup"><span data-stu-id="8a7eb-158">Update hello sync behavior of hello client app</span></span>
<span data-ttu-id="8a7eb-159">I det här avsnittet Ändra hello klienten projektet toosimulate ett offline-scenario med en ogiltig programmets URL för din serverdel.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-159">In this section, modify hello client project toosimulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="8a7eb-160">Du kan också du kan stänga av Nätverksanslutningar genom att flytta din enhet för ”Flygplansläge”.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-160">Alternatively, you can turn off network connections by moving your device too"Airplane mode."</span></span>  <span data-ttu-id="8a7eb-161">När du lägger till eller ändra dataobjekt ändringarna lagras i hello lokalt arkiv men har inte synkroniserats toohello backend datalager tills hello anslutningen har återupprättats.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-161">When you add or change data items, these changes are held in hello local store, but not synced toohello backend data store until hello connection is re-established.</span></span>

1. <span data-ttu-id="8a7eb-162">I hello Solution Explorer, Öppna projektfilen för hello Constants.cs från hello **bärbar** projektet och ändra hello värdet för `ApplicationURL` toopoint tooan ogiltig URL:</span><span class="sxs-lookup"><span data-stu-id="8a7eb-162">In hello Solution Explorer, open hello Constants.cs project file from hello **Portable** project and change hello value of `ApplicationURL` toopoint tooan invalid URL:</span></span>

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. <span data-ttu-id="8a7eb-163">Öppna hello TodoItemManager.cs filen från hello **bärbar** projektet och sedan lägga till en **fånga** för grundläggande hello **undantag** klassen toohello **försök... catch** blockera i **SyncAsync**.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-163">Open hello TodoItemManager.cs file from hello **Portable** project, then add a **catch** for hello base **Exception** class toohello **try...catch** block in **SyncAsync**.</span></span> <span data-ttu-id="8a7eb-164">Detta **fånga** block skriver hello undantag meddelandet toohello-konsolen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8a7eb-164">This **catch** block writes hello exception message toohello console, as follows:</span></span>

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. <span data-ttu-id="8a7eb-165">Skapa och köra hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-165">Build and run hello client app.</span></span>  <span data-ttu-id="8a7eb-166">Lägga till några nya objekt.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-166">Add some new items.</span></span> <span data-ttu-id="8a7eb-167">Observera att ett undantag loggas i hello konsol för varje försök toosync med hello-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-167">Notice that an exception is logged in hello console for each attempt toosync with hello backend.</span></span> <span data-ttu-id="8a7eb-168">Dessa nya objekt finns bara i hello lokalt Arkiv tills de flyttas toohello mobilserverdel.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-168">These new items exist only in hello local store until they can be pushed toohello mobile backend.</span></span> <span data-ttu-id="8a7eb-169">hello-klientappen fungerar som om den är ansluten toohello backend, stöder alla skapa, läsa, uppdatera, borttagningsåtgärder (CRUD).</span><span class="sxs-lookup"><span data-stu-id="8a7eb-169">hello client app behaves as if it is connected toohello backend, supporting all create, read, update, delete (CRUD) operations.</span></span>
4. <span data-ttu-id="8a7eb-170">Stäng hello app och starta om den tooverify att hello nya objekt som du skapade är beständiga toohello lokalt Arkiv.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-170">Close hello app and restart it tooverify that hello new items you created are persisted toohello local store.</span></span>
5. <span data-ttu-id="8a7eb-171">(Valfritt) Använd Visual Studio tooview toosee din Azure SQL Database-tabellen som hello data i hello backend-databasen inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-171">(Optional) Use Visual Studio tooview your Azure SQL Database table toosee that hello data in hello backend database has not changed.</span></span>

    <span data-ttu-id="8a7eb-172">Öppna i Visual Studio **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-172">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="8a7eb-173">Navigera tooyour databasen i **Azure**->**SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-173">Navigate tooyour database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="8a7eb-174">Högerklicka på databasen och välj **öppna i SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-174">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="8a7eb-175">Nu kan du bläddra tooyour SQL-databastabell och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-175">Now you can browse tooyour SQL database table and its contents.</span></span>

## <a name="update-hello-client-app-tooreconnect-your-mobile-backend"></a><span data-ttu-id="8a7eb-176">Uppdatera hello klienten app tooreconnect din mobila serverdel</span><span class="sxs-lookup"><span data-stu-id="8a7eb-176">Update hello client app tooreconnect your mobile backend</span></span>
<span data-ttu-id="8a7eb-177">Återanslut hello app toohello mobilserverdel, som simulerar hello app komma tillbaka tooan online tillstånd i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-177">In this section, reconnect hello app toohello mobile backend, which simulates hello app coming back tooan online state.</span></span> <span data-ttu-id="8a7eb-178">När du utför hello uppdatera gest är data synkroniserade tooyour mobilserverdel.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-178">When you perform hello refresh gesture, data is synced tooyour mobile backend.</span></span>

1. <span data-ttu-id="8a7eb-179">Öppna Constants.cs.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-179">Reopen Constants.cs.</span></span> <span data-ttu-id="8a7eb-180">Rätt hello `applicationURL` toopoint toohello korrigera URL.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-180">Correct hello `applicationURL` toopoint toohello correct URL.</span></span>
2. <span data-ttu-id="8a7eb-181">Bygg och kör hello klientapp.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-181">Rebuild and run hello client app.</span></span> <span data-ttu-id="8a7eb-182">hello app försöker toosync med hello mobilappsserverdel när du startar.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-182">hello app attempts toosync with hello mobile app backend after launching.</span></span> <span data-ttu-id="8a7eb-183">Kontrollera att inga undantag loggas i hello Felsökningskonsolen.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-183">Verify that no exceptions are logged in hello debug console.</span></span>
3. <span data-ttu-id="8a7eb-184">(Valfritt) Visa hello uppdaterade data med hjälp av SQL Server Object Explorer eller ett REST-verktyg som Fiddler eller [Postman][6].</span><span class="sxs-lookup"><span data-stu-id="8a7eb-184">(Optional) View hello updated data using either SQL Server Object Explorer or a REST tool like Fiddler or [Postman][6].</span></span> <span data-ttu-id="8a7eb-185">Meddelande hello data har synkroniserats mellan hello backend-databas och hello lokalt Arkiv.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-185">Notice hello data has been synchronized between hello backend database and hello local store.</span></span>

    <span data-ttu-id="8a7eb-186">Observera hello data har synkroniserats mellan hello databasen och hello lokalt Arkiv och innehåller hello-objekt som du har lagt till medan kopplades från din app.</span><span class="sxs-lookup"><span data-stu-id="8a7eb-186">Notice hello data has been synchronized between hello database and hello local store and contains hello items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a7eb-187">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8a7eb-187">Additional Resources</span></span>
* <span data-ttu-id="8a7eb-188">[Datasynkronisering offline i Azure Mobile Apps][2]</span><span class="sxs-lookup"><span data-stu-id="8a7eb-188">[Offline Data Sync in Azure Mobile Apps][2]</span></span>
* <span data-ttu-id="8a7eb-189">[Azure Mobile Apps .NET SDK ta][8]</span><span class="sxs-lookup"><span data-stu-id="8a7eb-189">[Azure Mobile Apps .NET SDK HOWTO][8]</span></span>

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
