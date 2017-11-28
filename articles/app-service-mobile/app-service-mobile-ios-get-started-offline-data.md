---
title: aaaEnable offline synkroniserar med iOS-appar | Microsoft Docs
description: "Lär dig hur toouse Azure Apptjänst mobilappar toocache och synkronisera offlinedata i iOS-program."
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 570ea7cf6694ab7317c977331038929b64508ad3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a><span data-ttu-id="ff971-103">Aktivera offline synkroniserar med iOS-appar</span><span class="sxs-lookup"><span data-stu-id="ff971-103">Enable offline syncing with iOS mobile apps</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="ff971-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ff971-104">Overview</span></span>
<span data-ttu-id="ff971-105">Den här kursen ingår offline synkroniseras med funktionerna för hello Mobile Apps i Azure App Service för iOS.</span><span class="sxs-lookup"><span data-stu-id="ff971-105">This tutorial covers offline syncing with hello Mobile Apps feature of Azure App Service for iOS.</span></span> <span data-ttu-id="ff971-106">Med offline synkroniserar slutanvändare kan interagera med en mobil app tooview, lägga till eller ändra data, även om de har någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="ff971-106">With offline syncing end-users can interact with a mobile app tooview, add, or modify data, even when they have no network connection.</span></span> <span data-ttu-id="ff971-107">Ändringarna sparas i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="ff971-107">Changes are stored in a local database.</span></span> <span data-ttu-id="ff971-108">När hello enheten är online igen, har hello ändringarna synkroniserats med hello remote serverdel.</span><span class="sxs-lookup"><span data-stu-id="ff971-108">After hello device is back online, hello changes are synced with hello remote back end.</span></span>

<span data-ttu-id="ff971-109">Om detta är din första upplevelse med Mobilappar kan du bör först kursen hello [skapa en iOS-App].</span><span class="sxs-lookup"><span data-stu-id="ff971-109">If this is your first experience with Mobile Apps, you should first complete hello tutorial [Create an iOS App].</span></span> <span data-ttu-id="ff971-110">Om du inte använder hello hämtas Snabbkurs serverprojekt måste du lägga till hello dataåtkomst tillägget paket tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="ff971-110">If you do not use hello downloaded quick-start server project, you must add hello data-access extension packages tooyour project.</span></span> <span data-ttu-id="ff971-111">Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="ff971-111">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="ff971-112">toolearn mer om hello offlinesynkronisering funktionen finns [offlinesynkronisering Data i Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="ff971-112">toolearn more about hello offline sync feature, see [Offline Data Sync in Mobile Apps].</span></span>

## <span data-ttu-id="ff971-113"><a name="review-sync"></a>Granska hello klientkod för synkronisering</span><span class="sxs-lookup"><span data-stu-id="ff971-113"><a name="review-sync"></a>Review hello client sync code</span></span>
<span data-ttu-id="ff971-114">Hej klientprojektet som du hämtade för hello [skapa en iOS-App] kursen redan innehåller kod som har stöd för offlinesynkronisering med hjälp av en lokal databas för kärnor databaserad.</span><span class="sxs-lookup"><span data-stu-id="ff971-114">hello client project that you downloaded for hello [Create an iOS App] tutorial already contains code that supports offline synchronization using a local Core Data-based database.</span></span> <span data-ttu-id="ff971-115">Det här avsnittet sammanfattas ingår redan i självstudiekursen hello-kod.</span><span class="sxs-lookup"><span data-stu-id="ff971-115">This section summarizes what is already included in hello tutorial code.</span></span> <span data-ttu-id="ff971-116">En översikt över funktionen hello finns [offlinesynkronisering Data i Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="ff971-116">For a conceptual overview of hello feature, see [Offline Data Sync in Mobile Apps].</span></span>

<span data-ttu-id="ff971-117">Funktionen hello datasynkronisering offline för Mobile Apps kan kan slutanvändarna interagera med en lokal databas även om hello nätverket är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="ff971-117">Using hello offline data-sync feature of Mobile Apps, end-users can interact with a local database even when hello network is inaccessible.</span></span> <span data-ttu-id="ff971-118">toouse funktionerna i appen du initiera hello sync kontexten för `MSClient` och referera till ett lokalt Arkiv.</span><span class="sxs-lookup"><span data-stu-id="ff971-118">toouse these features in your app, you initialize hello sync context of `MSClient` and reference a local store.</span></span> <span data-ttu-id="ff971-119">Sedan du refererar till tabellen via hello **MSSyncTable** gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ff971-119">Then you reference your table through hello **MSSyncTable** interface.</span></span>

<span data-ttu-id="ff971-120">I **QSTodoService.m** (Objective-C) eller **ToDoTableViewController.swift** (Swift), Lägg märke till att hello typ av hello medlem **syncTable** är  **MSSyncTable**.</span><span class="sxs-lookup"><span data-stu-id="ff971-120">In **QSTodoService.m** (Objective-C) or **ToDoTableViewController.swift** (Swift), notice that hello type of hello member **syncTable** is **MSSyncTable**.</span></span> <span data-ttu-id="ff971-121">Offlinesynkronisering använder sync tabell gränssnittet i stället för **MSTable**.</span><span class="sxs-lookup"><span data-stu-id="ff971-121">Offline sync uses this sync table interface instead of **MSTable**.</span></span> <span data-ttu-id="ff971-122">När en tabell med synkronisering används gå toohello lokalt Arkiv alla åtgärder och synkroniseras med hello remote serverdel med explicit push och pull-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ff971-122">When a sync table is used, all operations go toohello local store and are synchronized only with hello remote back end with explicit push and pull operations.</span></span>

 <span data-ttu-id="ff971-123">tooget referens tooa sync tabellen använder hello **syncTableWithName** metod på `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="ff971-123">tooget a reference tooa sync table, use hello **syncTableWithName** method on `MSClient`.</span></span> <span data-ttu-id="ff971-124">tooremove offlinesynkronisering funktioner, Använd **tableWithName** i stället.</span><span class="sxs-lookup"><span data-stu-id="ff971-124">tooremove offline sync functionality, use **tableWithName** instead.</span></span>

<span data-ttu-id="ff971-125">Hello lokalt arkiv måste initieras innan alla tabellåtgärder kan utföras.</span><span class="sxs-lookup"><span data-stu-id="ff971-125">Before any table operations can be performed, hello local store must be initialized.</span></span> <span data-ttu-id="ff971-126">Här är hello relevant kod:</span><span class="sxs-lookup"><span data-stu-id="ff971-126">Here is hello relevant code:</span></span>

* <span data-ttu-id="ff971-127">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="ff971-127">**Objective-C**.</span></span> <span data-ttu-id="ff971-128">I hello **QSTodoService.init** metod:</span><span class="sxs-lookup"><span data-stu-id="ff971-128">In hello **QSTodoService.init** method:</span></span>

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* <span data-ttu-id="ff971-129">**SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="ff971-129">**Swift**.</span></span> <span data-ttu-id="ff971-130">I hello **ToDoTableViewController.viewDidLoad** metod:</span><span class="sxs-lookup"><span data-stu-id="ff971-130">In hello **ToDoTableViewController.viewDidLoad** method:</span></span>

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of hello Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   <span data-ttu-id="ff971-131">Den här metoden skapar ett lokalt Arkiv med hjälp av hello `MSCoreDataStore` gränssnitt som hello Mobile Apps-SDK tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="ff971-131">This method creates a local store by using hello `MSCoreDataStore` interface, which hello Mobile Apps SDK provides.</span></span> <span data-ttu-id="ff971-132">Du kan också ange ett annat lokalt Arkiv genom att implementera hello `MSSyncContextDataSource` protokoll.</span><span class="sxs-lookup"><span data-stu-id="ff971-132">Alternatively, you can provide a different local store by implementing hello `MSSyncContextDataSource` protocol.</span></span> <span data-ttu-id="ff971-133">Dessutom hello första parametern för **MSSyncContext** är används toospecify en konflikt-hanterare.</span><span class="sxs-lookup"><span data-stu-id="ff971-133">Also, hello first parameter of **MSSyncContext** is used toospecify a conflict handler.</span></span> <span data-ttu-id="ff971-134">Eftersom vi har klarat `nil`, vi hämta hello konflikt standardprogram, som inte på någon konflikt.</span><span class="sxs-lookup"><span data-stu-id="ff971-134">Because we have passed `nil`, we get hello default conflict handler, which fails on any conflict.</span></span>

<span data-ttu-id="ff971-135">Nu ska vi utföra hello faktiska synkroniseringsåtgärd och hämta data från hello remote serverdel:</span><span class="sxs-lookup"><span data-stu-id="ff971-135">Now, let's perform hello actual sync operation, and get data from hello remote back end:</span></span>

* <span data-ttu-id="ff971-136">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="ff971-136">**Objective-C**.</span></span> <span data-ttu-id="ff971-137">`syncData`först skickar nya ändringar och anropar sedan **pullData** tooget data från hello remote serverdel.</span><span class="sxs-lookup"><span data-stu-id="ff971-137">`syncData` first pushes new changes and then calls **pullData** tooget data from hello remote back end.</span></span> <span data-ttu-id="ff971-138">I sin tur hello **pullData** metoden hämtar nya data som matchar en fråga:</span><span class="sxs-lookup"><span data-stu-id="ff971-138">In turn, hello **pullData** method gets new data that matches a query:</span></span>

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in hello sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from hello remote server into hello local table.
       // We're pulling all items and filtering in hello view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets hello caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* <span data-ttu-id="ff971-139">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="ff971-139">**Swift**:</span></span>
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc via hello MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep hello server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation tooitem \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting tooserver's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

<span data-ttu-id="ff971-140">I hello Objective-C-versionen i `syncData`, vi först anropa **pushWithCompletion** på hello sync-kontext.</span><span class="sxs-lookup"><span data-stu-id="ff971-140">In hello Objective-C version, in `syncData`, we first call **pushWithCompletion** on hello sync context.</span></span> <span data-ttu-id="ff971-141">Den här metoden är medlem i `MSSyncContext` (och inte hello sync själva tabellen) eftersom den skickar ändringarna över alla tabeller.</span><span class="sxs-lookup"><span data-stu-id="ff971-141">This method is a member of `MSSyncContext` (and not hello sync table itself) because it pushes changes across all tables.</span></span> <span data-ttu-id="ff971-142">Endast de poster som har ändrats på något sätt lokalt (via CUD operations) skickas toohello server.</span><span class="sxs-lookup"><span data-stu-id="ff971-142">Only records that have been modified in some way locally (through CUD operations) are sent toohello server.</span></span> <span data-ttu-id="ff971-143">Hej helper **pullData** anropas, som anropar **MSSyncTable.pullWithQuery** tooretrieve fjärrdata och lagra den på hello lokal databas.</span><span class="sxs-lookup"><span data-stu-id="ff971-143">Then hello helper **pullData** is called, which calls **MSSyncTable.pullWithQuery** tooretrieve remote data and store it in hello local database.</span></span>

<span data-ttu-id="ff971-144">Hello Swift version, eftersom hello utgivarinitierade åtgärden inte var absolut nödvändigt det finns inga anrop för**pushWithCompletion**.</span><span class="sxs-lookup"><span data-stu-id="ff971-144">In hello Swift version, because hello push operation was not strictly necessary, there is no call too**pushWithCompletion**.</span></span> <span data-ttu-id="ff971-145">Om det finns några väntande ändringar i hello sync kontexten för hello-tabell som gör en push-åtgärd, utfärdar pull alltid en push först.</span><span class="sxs-lookup"><span data-stu-id="ff971-145">If there are any changes pending in hello sync context for hello table that is doing a push operation, pull always issues a push first.</span></span> <span data-ttu-id="ff971-146">Om du har mer än en synkronisering tabell är den anropet push av bästa tooexplicitly-tooensure som att allt är konsekvent på relaterade tabeller.</span><span class="sxs-lookup"><span data-stu-id="ff971-146">However, if you have more than one sync table, it is best tooexplicitly call push tooensure that everything is consistent across related tables.</span></span>

<span data-ttu-id="ff971-147">I både hello Objective-C och Swift versioner, kan du använda hello **pullWithQuery** metoden toospecify en fråga toofilter hello poster du vill ha tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="ff971-147">In both hello Objective-C and Swift versions, you can use hello **pullWithQuery** method toospecify a query toofilter hello records you want tooretrieve.</span></span> <span data-ttu-id="ff971-148">I det här exemplet hello fråga hämtar alla poster i hello remote `TodoItem` tabell.</span><span class="sxs-lookup"><span data-stu-id="ff971-148">In this example, hello query retrieves all records in hello remote `TodoItem` table.</span></span>

<span data-ttu-id="ff971-149">Hej andra parametern för **pullWithQuery** är en fråge-ID som används för *inkrementell synkronisering*. Inkrementell synkronisering hämtar poster som har ändrats sedan hello senast synkroniserades med hello post `UpdatedAt` tidsstämpeln (kallas `updatedAt` i hello lokal lagring.) hello fråge-ID ska vara en beskrivande sträng som är unik för varje logisk fråga i din app.</span><span class="sxs-lookup"><span data-stu-id="ff971-149">hello second parameter of **pullWithQuery** is a query ID that is used for *incremental sync*. Incremental sync retrieves only records that were modified since hello last sync, using hello record's `UpdatedAt` time stamp (called `updatedAt` in hello local store.) hello query ID should be a descriptive string that is unique for each logical query in your app.</span></span> <span data-ttu-id="ff971-150">tooopt utanför inkrementell synkronisering skicka `nil` som hello fråga-ID.</span><span class="sxs-lookup"><span data-stu-id="ff971-150">tooopt out of incremental sync, pass `nil` as hello query ID.</span></span> <span data-ttu-id="ff971-151">Den här metoden kan vara potentiellt ineffektiv eftersom den hämtar alla poster på varje pull-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ff971-151">This approach can be potentially inefficient, because it retrieves all records on each pull operation.</span></span>

<span data-ttu-id="ff971-152">hello Objective-C-app som ska synkroniseras när du ändrar eller lägger till data, när användaren utför hello uppdatera gest och startas.</span><span class="sxs-lookup"><span data-stu-id="ff971-152">hello Objective-C app syncs when you modify or add data, when a user performs hello refresh gesture, and on launch.</span></span>

<span data-ttu-id="ff971-153">hello Swift app synkroniserar när hello användaren utför hello uppdatera gest och startas.</span><span class="sxs-lookup"><span data-stu-id="ff971-153">hello Swift app syncs when hello user performs hello refresh gesture and on launch.</span></span>

<span data-ttu-id="ff971-154">Eftersom hello app synkroniseringar när data har ändrats (Objective-C) eller när hello appen startar (Objective-C och Swift), förutsätter hello app hello användaren är online.</span><span class="sxs-lookup"><span data-stu-id="ff971-154">Because hello app syncs whenever data is modified (Objective-C) or whenever hello app starts (Objective-C and Swift), hello app assumes that hello user is online.</span></span> <span data-ttu-id="ff971-155">I ett senare avsnitt ska du uppdatera hello app så att användare kan redigera även när de är offline.</span><span class="sxs-lookup"><span data-stu-id="ff971-155">In a later section, you will update hello app so that users can edit even when they are offline.</span></span>

## <span data-ttu-id="ff971-156"><a name="review-core-data"></a>Granska hello Core-datamodell</span><span class="sxs-lookup"><span data-stu-id="ff971-156"><a name="review-core-data"></a>Review hello Core Data model</span></span>
<span data-ttu-id="ff971-157">När du använder hello Core offline datalagret, måste du definiera specifika tabeller och fält i datamodellen.</span><span class="sxs-lookup"><span data-stu-id="ff971-157">When you use hello Core Data offline store, you must define particular tables and fields in your data model.</span></span> <span data-ttu-id="ff971-158">Hej exempelapp innehåller redan en datamodell med hello rätt format.</span><span class="sxs-lookup"><span data-stu-id="ff971-158">hello sample app already includes a data model with hello right format.</span></span> <span data-ttu-id="ff971-159">I det här avsnittet går vi igenom dessa tabeller tooshow hur de används.</span><span class="sxs-lookup"><span data-stu-id="ff971-159">In this section, we walk through these tables tooshow how they are used.</span></span>

<span data-ttu-id="ff971-160">Öppna **QSDataModel.xcdatamodeld**.</span><span class="sxs-lookup"><span data-stu-id="ff971-160">Open **QSDataModel.xcdatamodeld**.</span></span> <span data-ttu-id="ff971-161">Fyra är definierad--tre som används av hello SDK och en som används för att göra hello konfigurationsobjekt själva:</span><span class="sxs-lookup"><span data-stu-id="ff971-161">Four tables are defined--three that are used by hello SDK and one that's used for hello to-do items themselves:</span></span>
  * <span data-ttu-id="ff971-162">MS_TableOperations: Spårar hello objekt behöver toobe synkroniseras med hello-servern.</span><span class="sxs-lookup"><span data-stu-id="ff971-162">MS_TableOperations: Tracks hello items that need toobe synchronized with hello server.</span></span>
  * <span data-ttu-id="ff971-163">MS_TableOperationErrors: Spårar alla fel som inträffar under offlinesynkronisering.</span><span class="sxs-lookup"><span data-stu-id="ff971-163">MS_TableOperationErrors: Tracks any errors that happen during offline synchronization.</span></span>
  * <span data-ttu-id="ff971-164">MS_TableConfig: Spårar hello senast uppdaterades för hello senaste synkroniseringsåtgärd för alla pull-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ff971-164">MS_TableConfig: Tracks hello last updated time for hello last sync operation for all pull operations.</span></span>
  * <span data-ttu-id="ff971-165">TodoItem: Lagrar hello arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ff971-165">TodoItem: Stores hello to-do items.</span></span> <span data-ttu-id="ff971-166">Hej Systemkolumner **createdAt**, **updatedAt**, och **version** är valfria Systemegenskaper.</span><span class="sxs-lookup"><span data-stu-id="ff971-166">hello system columns **createdAt**, **updatedAt**, and **version** are optional system properties.</span></span>

> [!NOTE]
> <span data-ttu-id="ff971-167">hello Mobile Apps-SDK reserverar kolumnnamn som börjar med ”**``**”.</span><span class="sxs-lookup"><span data-stu-id="ff971-167">hello Mobile Apps SDK reserves column names that begin with "**``**".</span></span> <span data-ttu-id="ff971-168">Använd inte det här prefixet med något annat än Systemkolumner.</span><span class="sxs-lookup"><span data-stu-id="ff971-168">Do not use this prefix with anything other than system columns.</span></span> <span data-ttu-id="ff971-169">Annars ändras kolumnnamn som när du använder hello remote serverdel.</span><span class="sxs-lookup"><span data-stu-id="ff971-169">Otherwise, your column names are modified when you use hello remote back end.</span></span>
>
>

<span data-ttu-id="ff971-170">När du använder funktionen för hello offlinesynkronisering definiera hello tre systemtabeller och hello datatabell.</span><span class="sxs-lookup"><span data-stu-id="ff971-170">When you use hello offline sync feature, define hello three system tables and hello data table.</span></span>

### <a name="system-tables"></a><span data-ttu-id="ff971-171">Systemtabeller</span><span class="sxs-lookup"><span data-stu-id="ff971-171">System tables</span></span>

<span data-ttu-id="ff971-172">**MS_TableOperations**</span><span class="sxs-lookup"><span data-stu-id="ff971-172">**MS_TableOperations**</span></span>  

![MS_TableOperations attribut][defining-core-data-tableoperations-entity]

| <span data-ttu-id="ff971-174">Attribut</span><span class="sxs-lookup"><span data-stu-id="ff971-174">Attribute</span></span> | <span data-ttu-id="ff971-175">Typ</span><span class="sxs-lookup"><span data-stu-id="ff971-175">Type</span></span> |
| --- | --- |
| <span data-ttu-id="ff971-176">id</span><span class="sxs-lookup"><span data-stu-id="ff971-176">id</span></span> | <span data-ttu-id="ff971-177">Heltal 64</span><span class="sxs-lookup"><span data-stu-id="ff971-177">Integer 64</span></span> |
| <span data-ttu-id="ff971-178">itemId</span><span class="sxs-lookup"><span data-stu-id="ff971-178">itemId</span></span> | <span data-ttu-id="ff971-179">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-179">String</span></span> |
| <span data-ttu-id="ff971-180">properties</span><span class="sxs-lookup"><span data-stu-id="ff971-180">properties</span></span> | <span data-ttu-id="ff971-181">Binära Data</span><span class="sxs-lookup"><span data-stu-id="ff971-181">Binary Data</span></span> |
| <span data-ttu-id="ff971-182">Tabell</span><span class="sxs-lookup"><span data-stu-id="ff971-182">table</span></span> | <span data-ttu-id="ff971-183">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-183">String</span></span> |
| <span data-ttu-id="ff971-184">tableKind</span><span class="sxs-lookup"><span data-stu-id="ff971-184">tableKind</span></span> | <span data-ttu-id="ff971-185">Heltal 16</span><span class="sxs-lookup"><span data-stu-id="ff971-185">Integer 16</span></span> |


<span data-ttu-id="ff971-186">**MS_TableOperationErrors**</span><span class="sxs-lookup"><span data-stu-id="ff971-186">**MS_TableOperationErrors**</span></span>

 ![MS_TableOperationErrors attribut][defining-core-data-tableoperationerrors-entity]

| <span data-ttu-id="ff971-188">Attribut</span><span class="sxs-lookup"><span data-stu-id="ff971-188">Attribute</span></span> | <span data-ttu-id="ff971-189">Typ</span><span class="sxs-lookup"><span data-stu-id="ff971-189">Type</span></span> |
| --- | --- |
| <span data-ttu-id="ff971-190">id</span><span class="sxs-lookup"><span data-stu-id="ff971-190">id</span></span> |<span data-ttu-id="ff971-191">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-191">String</span></span> |
| <span data-ttu-id="ff971-192">Åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="ff971-192">operationId</span></span> |<span data-ttu-id="ff971-193">Heltal 64</span><span class="sxs-lookup"><span data-stu-id="ff971-193">Integer 64</span></span> |
| <span data-ttu-id="ff971-194">properties</span><span class="sxs-lookup"><span data-stu-id="ff971-194">properties</span></span> |<span data-ttu-id="ff971-195">Binära Data</span><span class="sxs-lookup"><span data-stu-id="ff971-195">Binary Data</span></span> |
| <span data-ttu-id="ff971-196">tableKind</span><span class="sxs-lookup"><span data-stu-id="ff971-196">tableKind</span></span> |<span data-ttu-id="ff971-197">Heltal 16</span><span class="sxs-lookup"><span data-stu-id="ff971-197">Integer 16</span></span> |

 <span data-ttu-id="ff971-198">**MS_TableConfig**</span><span class="sxs-lookup"><span data-stu-id="ff971-198">**MS_TableConfig**</span></span>

 ![][defining-core-data-tableconfig-entity]

| <span data-ttu-id="ff971-199">Attribut</span><span class="sxs-lookup"><span data-stu-id="ff971-199">Attribute</span></span> | <span data-ttu-id="ff971-200">Typ</span><span class="sxs-lookup"><span data-stu-id="ff971-200">Type</span></span> |
| --- | --- |
| <span data-ttu-id="ff971-201">id</span><span class="sxs-lookup"><span data-stu-id="ff971-201">id</span></span> |<span data-ttu-id="ff971-202">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-202">String</span></span> |
| <span data-ttu-id="ff971-203">key</span><span class="sxs-lookup"><span data-stu-id="ff971-203">key</span></span> |<span data-ttu-id="ff971-204">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-204">String</span></span> |
| <span data-ttu-id="ff971-205">KeyType</span><span class="sxs-lookup"><span data-stu-id="ff971-205">keyType</span></span> |<span data-ttu-id="ff971-206">Heltal 64</span><span class="sxs-lookup"><span data-stu-id="ff971-206">Integer 64</span></span> |
| <span data-ttu-id="ff971-207">Tabell</span><span class="sxs-lookup"><span data-stu-id="ff971-207">table</span></span> |<span data-ttu-id="ff971-208">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-208">String</span></span> |
| <span data-ttu-id="ff971-209">värde</span><span class="sxs-lookup"><span data-stu-id="ff971-209">value</span></span> |<span data-ttu-id="ff971-210">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-210">String</span></span> |

### <a name="data-table"></a><span data-ttu-id="ff971-211">Datatabell</span><span class="sxs-lookup"><span data-stu-id="ff971-211">Data table</span></span>

<span data-ttu-id="ff971-212">**TodoItem**</span><span class="sxs-lookup"><span data-stu-id="ff971-212">**TodoItem**</span></span>

| <span data-ttu-id="ff971-213">Attribut</span><span class="sxs-lookup"><span data-stu-id="ff971-213">Attribute</span></span> | <span data-ttu-id="ff971-214">Typ</span><span class="sxs-lookup"><span data-stu-id="ff971-214">Type</span></span> | <span data-ttu-id="ff971-215">Obs!</span><span class="sxs-lookup"><span data-stu-id="ff971-215">Note</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ff971-216">id</span><span class="sxs-lookup"><span data-stu-id="ff971-216">id</span></span> | <span data-ttu-id="ff971-217">Strängen som markerats krävs</span><span class="sxs-lookup"><span data-stu-id="ff971-217">String, marked required</span></span> |<span data-ttu-id="ff971-218">primärnyckeln i fjärranslutna store</span><span class="sxs-lookup"><span data-stu-id="ff971-218">Primary key in remote store</span></span> |
| <span data-ttu-id="ff971-219">Slutför</span><span class="sxs-lookup"><span data-stu-id="ff971-219">complete</span></span> | <span data-ttu-id="ff971-220">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="ff971-220">Boolean</span></span> | <span data-ttu-id="ff971-221">Fältet för att göra-objekt</span><span class="sxs-lookup"><span data-stu-id="ff971-221">To-do item field</span></span> |
| <span data-ttu-id="ff971-222">Text</span><span class="sxs-lookup"><span data-stu-id="ff971-222">text</span></span> |<span data-ttu-id="ff971-223">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-223">String</span></span> |<span data-ttu-id="ff971-224">Fältet för att göra-objekt</span><span class="sxs-lookup"><span data-stu-id="ff971-224">To-do item field</span></span> |
| <span data-ttu-id="ff971-225">CreatedAt</span><span class="sxs-lookup"><span data-stu-id="ff971-225">createdAt</span></span> | <span data-ttu-id="ff971-226">Date</span><span class="sxs-lookup"><span data-stu-id="ff971-226">Date</span></span> | <span data-ttu-id="ff971-227">(valfritt) Mappar för**createdAt** Systemegenskapen</span><span class="sxs-lookup"><span data-stu-id="ff971-227">(optional) Maps too**createdAt** system property</span></span> |
| <span data-ttu-id="ff971-228">updatedAt</span><span class="sxs-lookup"><span data-stu-id="ff971-228">updatedAt</span></span> | <span data-ttu-id="ff971-229">Date</span><span class="sxs-lookup"><span data-stu-id="ff971-229">Date</span></span> | <span data-ttu-id="ff971-230">(valfritt) Mappar för**updatedAt** Systemegenskapen</span><span class="sxs-lookup"><span data-stu-id="ff971-230">(optional) Maps too**updatedAt** system property</span></span> |
| <span data-ttu-id="ff971-231">Version</span><span class="sxs-lookup"><span data-stu-id="ff971-231">version</span></span> | <span data-ttu-id="ff971-232">Sträng</span><span class="sxs-lookup"><span data-stu-id="ff971-232">String</span></span> | <span data-ttu-id="ff971-233">(valfritt) Använda toodetect konflikter, maps tooversion</span><span class="sxs-lookup"><span data-stu-id="ff971-233">(optional) Used toodetect conflicts, maps tooversion</span></span> |

## <span data-ttu-id="ff971-234"><a name="setup-sync"></a>Ändra hello hello appens sync beteende</span><span class="sxs-lookup"><span data-stu-id="ff971-234"><a name="setup-sync"></a>Change hello sync behavior of hello app</span></span>
<span data-ttu-id="ff971-235">I det här avsnittet Ändra hello app så att den inte synkroniseras på appen startas eller när du infogar och uppdatera objekt.</span><span class="sxs-lookup"><span data-stu-id="ff971-235">In this section, you modify hello app so that it does not sync on app start or when you insert and update items.</span></span> <span data-ttu-id="ff971-236">Det synkroniserar bara när hello uppdateringsknappen gest utförs.</span><span class="sxs-lookup"><span data-stu-id="ff971-236">It syncs only when hello refresh gesture button is performed.</span></span>

<span data-ttu-id="ff971-237">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="ff971-237">**Objective-C**:</span></span>

1. <span data-ttu-id="ff971-238">I **QSTodoListViewController.m**, ändra hello **viewDidLoad** metoden tooremove hello anrop för`[self refresh]` hello slutet av hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="ff971-238">In **QSTodoListViewController.m**, change hello **viewDidLoad** method tooremove hello call too`[self refresh]` at hello end of hello method.</span></span> <span data-ttu-id="ff971-239">Nu hello data inte har synkroniserats med hello server på appen startas.</span><span class="sxs-lookup"><span data-stu-id="ff971-239">Now hello data is not synced with hello server on app start.</span></span> <span data-ttu-id="ff971-240">I stället är den synkroniserad med hello innehållet i hello lokalt Arkiv.</span><span class="sxs-lookup"><span data-stu-id="ff971-240">Instead, it's synced with hello contents of hello local store.</span></span>
2. <span data-ttu-id="ff971-241">I **QSTodoService.m**, ändra hello definitionen av `addItem` så att den inte synkronisera efter hello objektet infogas.</span><span class="sxs-lookup"><span data-stu-id="ff971-241">In **QSTodoService.m**, modify hello definition of `addItem` so that it doesn't sync after hello item is inserted.</span></span> <span data-ttu-id="ff971-242">Ta bort hello `self syncData` blockera och Ersätt den med hello följande:</span><span class="sxs-lookup"><span data-stu-id="ff971-242">Remove hello `self syncData` block and replace it with hello following:</span></span>

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. <span data-ttu-id="ff971-243">Ändra hello definitionen av `completeItem` som tidigare nämnts.</span><span class="sxs-lookup"><span data-stu-id="ff971-243">Modify hello definition of `completeItem` as mentioned previously.</span></span> <span data-ttu-id="ff971-244">Ta bort hello-block för `self syncData` och Ersätt den med hello följande:</span><span class="sxs-lookup"><span data-stu-id="ff971-244">Remove hello block for `self syncData` and replace it with hello following:</span></span>
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

<span data-ttu-id="ff971-245">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="ff971-245">**Swift**:</span></span>

<span data-ttu-id="ff971-246">I `viewDidLoad`i **ToDoTableViewController.swift**, kommentera hello två rader som visas här, toostop synkroniserar appen startas.</span><span class="sxs-lookup"><span data-stu-id="ff971-246">In `viewDidLoad`, in **ToDoTableViewController.swift**, comment out hello two lines shown here, toostop syncing on app start.</span></span> <span data-ttu-id="ff971-247">När hello detta skrivs uppdaterar hello Swift Todo-appen inte hello-tjänsten när någon lägger till eller ett objekt slutförs.</span><span class="sxs-lookup"><span data-stu-id="ff971-247">At hello time of this writing, hello Swift Todo app does not update hello service when someone adds or completes an item.</span></span> <span data-ttu-id="ff971-248">Uppdaterar den hello tjänsten endast på appen startas.</span><span class="sxs-lookup"><span data-stu-id="ff971-248">It updates hello service only on app start.</span></span>

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <span data-ttu-id="ff971-249"><a name="test-app"></a>Testa hello app</span><span class="sxs-lookup"><span data-stu-id="ff971-249"><a name="test-app"></a>Test hello app</span></span>
<span data-ttu-id="ff971-250">I det här avsnittet kan du ansluta tooan Ogiltigt URL toosimulate ett offline-scenario.</span><span class="sxs-lookup"><span data-stu-id="ff971-250">In this section, you connect tooan invalid URL toosimulate an offline scenario.</span></span> <span data-ttu-id="ff971-251">När du lägger till data som de ska lagras i hello lokala Core datalager, men de är inte synkroniserade med hello mobilapp serverdel.</span><span class="sxs-lookup"><span data-stu-id="ff971-251">When you add data items, they're held in hello local Core Data store, but they're not synced with hello mobile-app back end.</span></span>

1. <span data-ttu-id="ff971-252">Ändra hello mobile app-URL i **QSTodoService.m** tooan Ogiltigt URL och kör hello app igen:</span><span class="sxs-lookup"><span data-stu-id="ff971-252">Change hello mobile-app URL in **QSTodoService.m** tooan invalid URL, and run hello app again:</span></span>

   <span data-ttu-id="ff971-253">**Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="ff971-253">**Objective-C**.</span></span> <span data-ttu-id="ff971-254">I QSTodoService.m:</span><span class="sxs-lookup"><span data-stu-id="ff971-254">In QSTodoService.m:</span></span>
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   <span data-ttu-id="ff971-255">**SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="ff971-255">**Swift**.</span></span> <span data-ttu-id="ff971-256">I ToDoTableViewController.swift:</span><span class="sxs-lookup"><span data-stu-id="ff971-256">In ToDoTableViewController.swift:</span></span>
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. <span data-ttu-id="ff971-257">Lägga till vissa arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ff971-257">Add some to-do items.</span></span> <span data-ttu-id="ff971-258">Avsluta hello simulator (eller framtvingar Stäng hello app) och starta om den.</span><span class="sxs-lookup"><span data-stu-id="ff971-258">Quit hello simulator (or forcibly close hello app), and then restart it.</span></span> <span data-ttu-id="ff971-259">Kontrollera att spara dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="ff971-259">Verify that your changes persist.</span></span>

3. <span data-ttu-id="ff971-260">Visa hello innehållet i hello remote **TodoItem** tabell:</span><span class="sxs-lookup"><span data-stu-id="ff971-260">View hello contents of hello remote **TodoItem** table:</span></span>
   * <span data-ttu-id="ff971-261">En Node.js-serverdel finns toohello [Azure-portalen](https://portal.azure.com/) och på din mobila app serverdel **enkelt tabeller** > **TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="ff971-261">For a Node.js back end, go toohello [Azure portal](https://portal.azure.com/) and, in your mobile-app back end, click **Easy Tables** > **TodoItem**.</span></span>  
   * <span data-ttu-id="ff971-262">Avsluta, använda ett SQL-verktyg, till exempel SQL Server Management Studio eller en REST-klient, till exempel Fiddler eller Postman för en .NET tillbaka.</span><span class="sxs-lookup"><span data-stu-id="ff971-262">For a .NET back end, use either a SQL tool, such as SQL Server Management Studio, or a REST client, such as Fiddler or Postman.</span></span>  

4. <span data-ttu-id="ff971-263">Kontrollera att hello nya objekt har *inte* har synkroniserats med hello-server.</span><span class="sxs-lookup"><span data-stu-id="ff971-263">Verify that hello new items have *not* been synced with hello server.</span></span>

5. <span data-ttu-id="ff971-264">Ändra hello URL: en bakre toohello korrigera i **QSTodoService.m**, och kör hello app.</span><span class="sxs-lookup"><span data-stu-id="ff971-264">Change hello URL back toohello correct one in **QSTodoService.m**, and rerun hello app.</span></span>

6. <span data-ttu-id="ff971-265">Utföra hello uppdatera gest genom att dra nedåt hello listan med objekt.</span><span class="sxs-lookup"><span data-stu-id="ff971-265">Perform hello refresh gesture by pulling down hello list of items.</span></span>  
<span data-ttu-id="ff971-266">En rotationsruta förloppet visas.</span><span class="sxs-lookup"><span data-stu-id="ff971-266">A progress spinner is displayed.</span></span>

7. <span data-ttu-id="ff971-267">Visa hello **TodoItem** data igen.</span><span class="sxs-lookup"><span data-stu-id="ff971-267">View hello **TodoItem** data again.</span></span> <span data-ttu-id="ff971-268">hello nya och ändrade arbetsuppgifter ska nu visas.</span><span class="sxs-lookup"><span data-stu-id="ff971-268">hello new and changed to-do items should now be displayed.</span></span>

## <a name="summary"></a><span data-ttu-id="ff971-269">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ff971-269">Summary</span></span>
<span data-ttu-id="ff971-270">toosupport hello offlinesynkronisering funktionen vi använde hello `MSSyncTable` gränssnitt och initieras `MSClient.syncContext` med ett lokalt Arkiv.</span><span class="sxs-lookup"><span data-stu-id="ff971-270">toosupport hello offline sync feature, we used hello `MSSyncTable` interface and initialized `MSClient.syncContext` with a local store.</span></span> <span data-ttu-id="ff971-271">I det här fallet var hello lokalt Arkiv en Core databaserad databas.</span><span class="sxs-lookup"><span data-stu-id="ff971-271">In this case, hello local store was a Core Data-based database.</span></span>

<span data-ttu-id="ff971-272">När du använder ett lokalt Arkiv grundläggande information, måste du definiera flera tabeller med hello [korrigera Systemegenskaper](#review-core-data).</span><span class="sxs-lookup"><span data-stu-id="ff971-272">When you use a Core Data local store, you must define several tables with hello [correct system properties](#review-core-data).</span></span>

<span data-ttu-id="ff971-273">hello normal skapa, läsa, uppdatera och ta bort CRUD-åtgärder för mobilappar fungerar som om hello app fortfarande är ansluten, men alla hello åtgärder mot hello lokalt Arkiv.</span><span class="sxs-lookup"><span data-stu-id="ff971-273">hello normal create, read, update, and delete (CRUD) operations for mobile apps work as if hello app is still connected, but all hello operations occur against hello local store.</span></span>

<span data-ttu-id="ff971-274">När vi synkroniseras hello lokalt Arkiv med hello server använde vi hello **MSSyncTable.pullWithQuery** metod.</span><span class="sxs-lookup"><span data-stu-id="ff971-274">When we synchronized hello local store with hello server, we used hello **MSSyncTable.pullWithQuery** method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff971-275">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ff971-275">Additional resources</span></span>
* <span data-ttu-id="ff971-276">[offlinesynkronisering Data i Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="ff971-276">[Offline Data Sync in Mobile Apps]</span></span>
* <span data-ttu-id="ff971-277">[Molnet omfattar: Offlinesynkronisering i Azure Mobile Services] \(hello video handlar om Mobile Services, men Mobile Apps offline synkroniseras fungerar på liknande sätt.\)</span><span class="sxs-lookup"><span data-stu-id="ff971-277">[Cloud Cover: Offline Sync in Azure Mobile Services] \(hello video is about Mobile Services, but Mobile Apps offline sync works in a similar way.\)</span></span>

<!-- URLs. -->


[skapa en iOS-App]: app-service-mobile-ios-get-started.md
[offlinesynkronisering Data i Mobile Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Molnet omfattar: Offlinesynkronisering i Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
