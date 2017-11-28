---
title: "aaaEnable offlinesynkronisering för Azure Mobile App (Android)"
description: "Lär dig hur toouse Apptjänst Mobilappar toocache och sync offline data i din Android-App"
documentationcenter: android
author: ggailey777
manager: syntaxc4
services: app-service\mobile
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 34508c7394610cf9127e1753637940826b8fd06a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a><span data-ttu-id="4327c-103">Aktivera offlinesynkronisering av din Android-mobilapp</span><span class="sxs-lookup"><span data-stu-id="4327c-103">Enable offline sync for your Android mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="4327c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4327c-104">Overview</span></span>
<span data-ttu-id="4327c-105">Den här kursen ingår hello offlinesynkronisering funktion i Azure Mobilappar för Android.</span><span class="sxs-lookup"><span data-stu-id="4327c-105">This tutorial covers hello offline sync feature of Azure Mobile Apps for Android.</span></span> <span data-ttu-id="4327c-106">Offlinesynkronisering kan slutet användare toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="4327c-106">Offline sync allows end users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="4327c-107">Ändringarna sparas i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="4327c-107">Changes are stored in a local database.</span></span> <span data-ttu-id="4327c-108">När hello enheten är online igen, synkroniseras ändringarna med fjärråtkomst hello-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="4327c-108">Once hello device is back online, these changes are synced with hello remote backend.</span></span>

<span data-ttu-id="4327c-109">Om det här är din första upplevelse med Azure Mobile Apps du bör först kursen hello [skapa en Android-App].</span><span class="sxs-lookup"><span data-stu-id="4327c-109">If this is your first experience with Azure Mobile Apps, you should first complete hello tutorial [Create an Android App].</span></span> <span data-ttu-id="4327c-110">Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello data access paket tooyour tilläggsprojekt.</span><span class="sxs-lookup"><span data-stu-id="4327c-110">If you do not use hello downloaded quick start server project, you must add hello data access extension packages tooyour project.</span></span> <span data-ttu-id="4327c-111">Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="4327c-111">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="4327c-112">toolearn mer om hello offlinesynkronisering funktionen hello avsnittet [offlinesynkronisering Data i Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="4327c-112">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="update-hello-app-toosupport-offline-sync"></a><span data-ttu-id="4327c-113">Uppdatera hello app toosupport offlinesynkronisering</span><span class="sxs-lookup"><span data-stu-id="4327c-113">Update hello app toosupport offline sync</span></span>
<span data-ttu-id="4327c-114">Med offlinesynkronisering, du läsa tooand skriva från en *sync tabell* (med hello *IMobileServiceSyncTable* interface), som ingår i en **SQLite** databasen på din enhet.</span><span class="sxs-lookup"><span data-stu-id="4327c-114">With offline sync, you read tooand write from a *sync table* (using hello *IMobileServiceSyncTable* interface), which is part of a **SQLite** database on your device.</span></span>

<span data-ttu-id="4327c-115">toopush och pull ändringar mellan hello enheten och Azure Mobile Services som du använder en *synkroniseringskontext* (*MobileServiceClient.SyncContext*), som du initiera med hello lokal databas toostore data lokalt.</span><span class="sxs-lookup"><span data-stu-id="4327c-115">toopush and pull changes between hello device and Azure Mobile Services, you use a *synchronization context* (*MobileServiceClient.SyncContext*), which you initialize with hello local database toostore data locally.</span></span>

1. <span data-ttu-id="4327c-116">I `TodoActivity.java`, kommentera hello befintliga definitionen av `mToDoTable` och Avkommentera hello synkroniserade tabell versionen:</span><span class="sxs-lookup"><span data-stu-id="4327c-116">In `TodoActivity.java`, comment out hello existing definition of `mToDoTable` and uncomment hello sync table version:</span></span>
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. <span data-ttu-id="4327c-117">I hello `onCreate` metod, kommentera hello befintliga initieringen av `mToDoTable` och Avkommentera den här definitionen:</span><span class="sxs-lookup"><span data-stu-id="4327c-117">In hello `onCreate` method, comment out hello existing initialization of `mToDoTable` and uncomment this definition:</span></span>
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. <span data-ttu-id="4327c-118">I `refreshItemsFromTable` kommentera hello definition av `results` och Avkommentera den här definitionen:</span><span class="sxs-lookup"><span data-stu-id="4327c-118">In `refreshItemsFromTable` comment out hello definition of `results` and uncomment this definition:</span></span>
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. <span data-ttu-id="4327c-119">Kommentera hello definitionen av `refreshItemsFromMobileServiceTable`.</span><span class="sxs-lookup"><span data-stu-id="4327c-119">Comment out hello definition of `refreshItemsFromMobileServiceTable`.</span></span>
5. <span data-ttu-id="4327c-120">Ta bort kommentarerna hello definitionen av `refreshItemsFromMobileServiceTableSyncTable`:</span><span class="sxs-lookup"><span data-stu-id="4327c-120">Uncomment hello definition of `refreshItemsFromMobileServiceTableSyncTable`:</span></span>
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync hello data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. <span data-ttu-id="4327c-121">Ta bort kommentarerna hello definitionen av `sync`:</span><span class="sxs-lookup"><span data-stu-id="4327c-121">Uncomment hello definition of `sync`:</span></span>
   
        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-hello-app"></a><span data-ttu-id="4327c-122">Testa hello app</span><span class="sxs-lookup"><span data-stu-id="4327c-122">Test hello app</span></span>
<span data-ttu-id="4327c-123">I det här avsnittet testa hello fungerar med WiFi på och stäng sedan av WiFi toocreate ett offline-scenario.</span><span class="sxs-lookup"><span data-stu-id="4327c-123">In this section, you test hello behavior with WiFi on, and then turn off WiFi toocreate an offline scenario.</span></span>

<span data-ttu-id="4327c-124">När du lägger till data som de förvaras i hello lokala SQLite store, men inte synkroniserats toohello mobila tjänsten tills du trycker på hello **uppdatera** knappen.</span><span class="sxs-lookup"><span data-stu-id="4327c-124">When you add data items, they are held in hello local SQLite store, but not synced toohello mobile service until you press hello **Refresh** button.</span></span> <span data-ttu-id="4327c-125">Andra appar kan ha olika krav angående när data måste toobe synkroniseras, men för demonstration har den här kursen hello användaren explicit begära den.</span><span class="sxs-lookup"><span data-stu-id="4327c-125">Other apps may have different requirements regarding when data needs toobe synchronized, but for demo purposes this tutorial has hello user explicitly request it.</span></span>

<span data-ttu-id="4327c-126">När du trycker på knappen startar en ny bakgrundsuppgift.</span><span class="sxs-lookup"><span data-stu-id="4327c-126">When you press that button, a new background task starts.</span></span> <span data-ttu-id="4327c-127">Den första skickar alla ändringar som gjorts toohello lokalt Arkiv med hjälp av synkroniseringskontext hämtar alla ändrade data från Azure toohello lokal tabell.</span><span class="sxs-lookup"><span data-stu-id="4327c-127">It first pushes all changes made toohello local store using synchronization context, then pulls all changed data from Azure toohello local table.</span></span>

### <a name="offline-testing"></a><span data-ttu-id="4327c-128">Testa offline</span><span class="sxs-lookup"><span data-stu-id="4327c-128">Offline testing</span></span>
1. <span data-ttu-id="4327c-129">Plats hello enhet eller simulator i *Flygplansläge*.</span><span class="sxs-lookup"><span data-stu-id="4327c-129">Place hello device or simulator in *Airplane Mode*.</span></span> <span data-ttu-id="4327c-130">Detta skapar ett offline-scenario.</span><span class="sxs-lookup"><span data-stu-id="4327c-130">This creates an offline scenario.</span></span>
2. <span data-ttu-id="4327c-131">Lägga till några *ToDo* funktioner eller markera vissa objekt som slutförd.</span><span class="sxs-lookup"><span data-stu-id="4327c-131">Add some *ToDo* items, or mark some items as complete.</span></span> <span data-ttu-id="4327c-132">Avsluta hello enhet eller simulator (eller framtvingar Stäng hello app) och starta om.</span><span class="sxs-lookup"><span data-stu-id="4327c-132">Quit hello device or simulator (or forcibly close hello app) and restart.</span></span> <span data-ttu-id="4327c-133">Kontrollera att dina ändringar har sparats på hello enhet eftersom de lagras i hello lokala SQLite-arkivet.</span><span class="sxs-lookup"><span data-stu-id="4327c-133">Verify that your changes have been persisted on hello device because they are held in hello local SQLite store.</span></span>
3. <span data-ttu-id="4327c-134">Visa hello innehållet i hello Azure *TodoItem* tabell antingen med ett SQL-verktyg som *SQL Server Management Studio*, eller en REST-klient som *Fiddler* eller  *Postman*.</span><span class="sxs-lookup"><span data-stu-id="4327c-134">View hello contents of hello Azure *TodoItem* table either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span> <span data-ttu-id="4327c-135">Kontrollera att hello nya objekt har *inte* har synkroniserats toohello server</span><span class="sxs-lookup"><span data-stu-id="4327c-135">Verify that hello new items have *not* been synced toohello server</span></span>
   
       + <span data-ttu-id="4327c-136">Node.js-serverdel finns toohello [Azure-portalen](https://portal.azure.com/), och i din Mobilapp backend på **enkelt tabeller** > **TodoItem** tooview hello innehållet i hello `TodoItem` tabell.</span><span class="sxs-lookup"><span data-stu-id="4327c-136">For a Node.js backend, go toohello [Azure portal](https://portal.azure.com/), and in your Mobile App backend click **Easy Tables** > **TodoItem** tooview hello contents of hello `TodoItem` table.</span></span>
       + <span data-ttu-id="4327c-137">För en .NET-serverdel visa hello tabell innehållet antingen med ett SQL-verktyg som *SQL Server Management Studio*, eller en REST-klient som *Fiddler* eller *Postman*.</span><span class="sxs-lookup"><span data-stu-id="4327c-137">For a .NET backend, view hello table contents either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span>
4. <span data-ttu-id="4327c-138">Aktivera WiFi i hello enhet eller simulatorn.</span><span class="sxs-lookup"><span data-stu-id="4327c-138">Turn on WiFi in hello device or simulator.</span></span> <span data-ttu-id="4327c-139">Tryck sedan på hello **uppdatera** knappen.</span><span class="sxs-lookup"><span data-stu-id="4327c-139">Next, press hello **Refresh** button.</span></span>
5. <span data-ttu-id="4327c-140">Visa hello TodoItem data i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4327c-140">View hello TodoItem data again in hello Azure portal.</span></span> <span data-ttu-id="4327c-141">hello nya och ändrade TodoItems bör nu visas.</span><span class="sxs-lookup"><span data-stu-id="4327c-141">hello new and changed TodoItems should now appear.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4327c-142">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4327c-142">Additional Resources</span></span>
* <span data-ttu-id="4327c-143">[offlinesynkronisering Data i Azure Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="4327c-143">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="4327c-144">[Molnet omfattar: Offlinesynkronisering i Azure Mobile Services] \(Obs: hello video finns i Mobile Services, men offlinesynkronisering fungerar på ett liknande sätt i Azure Mobile Apps\)</span><span class="sxs-lookup"><span data-stu-id="4327c-144">[Cloud Cover: Offline Sync in Azure Mobile Services] \(note: hello video is on Mobile Services, but offline sync works in a similar way in Azure Mobile Apps\)</span></span>

<!-- URLs. -->

[offlinesynkronisering Data i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md

[skapa en Android-App]: app-service-mobile-android-get-started.md

[Molnet omfattar: Offlinesynkronisering i Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

