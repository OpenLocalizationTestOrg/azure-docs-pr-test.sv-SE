---
title: "Aktivera offlinesynkronisering för Azure Mobile App (Android)"
description: "Lär dig mer om Apptjänst Mobilappar cache och synkronisering offline data i din Android-App"
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
ms.openlocfilehash: 304323c1816302e8c1f68f36a029aee55e02c54e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a><span data-ttu-id="d6b70-103">Aktivera offlinesynkronisering av din Android-mobilapp</span><span class="sxs-lookup"><span data-stu-id="d6b70-103">Enable offline sync for your Android mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="d6b70-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d6b70-104">Overview</span></span>
<span data-ttu-id="d6b70-105">Den här kursen ingår funktionen offlinesynkronisering i Azure Mobilappar för Android.</span><span class="sxs-lookup"><span data-stu-id="d6b70-105">This tutorial covers the offline sync feature of Azure Mobile Apps for Android.</span></span> <span data-ttu-id="d6b70-106">Offlinesynkronisering kan slutanvändarna interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="d6b70-106">Offline sync allows end users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="d6b70-107">Ändringarna sparas i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="d6b70-107">Changes are stored in a local database.</span></span> <span data-ttu-id="d6b70-108">När enheten är online igen, har ändringarna synkroniserats med fjärr-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="d6b70-108">Once the device is back online, these changes are synced with the remote backend.</span></span>

<span data-ttu-id="d6b70-109">Om det här är din första upplevelse med Azure Mobile Apps bör du först slutföra kursen [skapa en Android-App].</span><span class="sxs-lookup"><span data-stu-id="d6b70-109">If this is your first experience with Azure Mobile Apps, you should first complete the tutorial [Create an Android App].</span></span> <span data-ttu-id="d6b70-110">Om du inte använder serverprojekt hämtade Snabbstart, måste du lägga till data access-tilläggspaket projektet.</span><span class="sxs-lookup"><span data-stu-id="d6b70-110">If you do not use the downloaded quick start server project, you must add the data access extension packages to your project.</span></span> <span data-ttu-id="d6b70-111">Mer information om server tilläggspaket finns [arbeta med serverdelen .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d6b70-111">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="d6b70-112">Mer information om funktionen offlinesynkronisering finns i avsnittet [offlinesynkronisering Data i Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="d6b70-112">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="update-the-app-to-support-offline-sync"></a><span data-ttu-id="d6b70-113">Uppdatera program för att stödja offlinesynkronisering</span><span class="sxs-lookup"><span data-stu-id="d6b70-113">Update the app to support offline sync</span></span>
<span data-ttu-id="d6b70-114">Med offlinesynkronisering, för att läsa och skriva från en *sync tabell* (med hjälp av den *IMobileServiceSyncTable* interface), som ingår i en **SQLite** databasen på din enhet.</span><span class="sxs-lookup"><span data-stu-id="d6b70-114">With offline sync, you read to and write from a *sync table* (using the *IMobileServiceSyncTable* interface), which is part of a **SQLite** database on your device.</span></span>

<span data-ttu-id="d6b70-115">För att push och pull-ändringar mellan enheten och Azure Mobile Services måste du använda en *synkroniseringskontext* (*MobileServiceClient.SyncContext*), som du initiera med lokal databas för lagring data lokalt.</span><span class="sxs-lookup"><span data-stu-id="d6b70-115">To push and pull changes between the device and Azure Mobile Services, you use a *synchronization context* (*MobileServiceClient.SyncContext*), which you initialize with the local database to store data locally.</span></span>

1. <span data-ttu-id="d6b70-116">I `TodoActivity.java`, kommentera ut den befintliga definitionen av `mToDoTable` och Avkommentera synkroniserade tabell versionen:</span><span class="sxs-lookup"><span data-stu-id="d6b70-116">In `TodoActivity.java`, comment out the existing definition of `mToDoTable` and uncomment the sync table version:</span></span>
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. <span data-ttu-id="d6b70-117">I den `onCreate` metod, kommentera befintliga initieringen av `mToDoTable` och Avkommentera den här definitionen:</span><span class="sxs-lookup"><span data-stu-id="d6b70-117">In the `onCreate` method, comment out the existing initialization of `mToDoTable` and uncomment this definition:</span></span>
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. <span data-ttu-id="d6b70-118">I `refreshItemsFromTable` kommentera ut definitionen av `results` och Avkommentera den här definitionen:</span><span class="sxs-lookup"><span data-stu-id="d6b70-118">In `refreshItemsFromTable` comment out the definition of `results` and uncomment this definition:</span></span>
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. <span data-ttu-id="d6b70-119">Kommentera ut definitionen av `refreshItemsFromMobileServiceTable`.</span><span class="sxs-lookup"><span data-stu-id="d6b70-119">Comment out the definition of `refreshItemsFromMobileServiceTable`.</span></span>
5. <span data-ttu-id="d6b70-120">Ta bort kommentarerna definitionen av `refreshItemsFromMobileServiceTableSyncTable`:</span><span class="sxs-lookup"><span data-stu-id="d6b70-120">Uncomment the definition of `refreshItemsFromMobileServiceTableSyncTable`:</span></span>
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. <span data-ttu-id="d6b70-121">Ta bort kommentarerna definitionen av `sync`:</span><span class="sxs-lookup"><span data-stu-id="d6b70-121">Uncomment the definition of `sync`:</span></span>
   
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

## <a name="test-the-app"></a><span data-ttu-id="d6b70-122">Testa appen</span><span class="sxs-lookup"><span data-stu-id="d6b70-122">Test the app</span></span>
<span data-ttu-id="d6b70-123">I det här avsnittet testa fungerar med WiFi på och stäng sedan av WiFi att skapa ett offline-scenario.</span><span class="sxs-lookup"><span data-stu-id="d6b70-123">In this section, you test the behavior with WiFi on, and then turn off WiFi to create an offline scenario.</span></span>

<span data-ttu-id="d6b70-124">När du lägger till data som de lagras i det lokala arkivet SQLite, men inte synkroniserats med den mobila tjänsten tills du trycker på **uppdatera** knappen.</span><span class="sxs-lookup"><span data-stu-id="d6b70-124">When you add data items, they are held in the local SQLite store, but not synced to the mobile service until you press the **Refresh** button.</span></span> <span data-ttu-id="d6b70-125">Andra appar kan ha olika krav angående när data måste synkroniseras, men för demonstration den här självstudiekursen har användaren explicit begära den.</span><span class="sxs-lookup"><span data-stu-id="d6b70-125">Other apps may have different requirements regarding when data needs to be synchronized, but for demo purposes this tutorial has the user explicitly request it.</span></span>

<span data-ttu-id="d6b70-126">När du trycker på knappen startar en ny bakgrundsuppgift.</span><span class="sxs-lookup"><span data-stu-id="d6b70-126">When you press that button, a new background task starts.</span></span> <span data-ttu-id="d6b70-127">Den första skickar alla ändringar som gjorts i det lokala arkivet med hjälp av synkroniseringskontext hämtar alla ändrade data från Azure till lokala tabellen.</span><span class="sxs-lookup"><span data-stu-id="d6b70-127">It first pushes all changes made to the local store using synchronization context, then pulls all changed data from Azure to the local table.</span></span>

### <a name="offline-testing"></a><span data-ttu-id="d6b70-128">Testa offline</span><span class="sxs-lookup"><span data-stu-id="d6b70-128">Offline testing</span></span>
1. <span data-ttu-id="d6b70-129">Placera enheten eller simulator i *Flygplansläge*.</span><span class="sxs-lookup"><span data-stu-id="d6b70-129">Place the device or simulator in *Airplane Mode*.</span></span> <span data-ttu-id="d6b70-130">Detta skapar ett offline-scenario.</span><span class="sxs-lookup"><span data-stu-id="d6b70-130">This creates an offline scenario.</span></span>
2. <span data-ttu-id="d6b70-131">Lägga till några *ToDo* funktioner eller markera vissa objekt som slutförd.</span><span class="sxs-lookup"><span data-stu-id="d6b70-131">Add some *ToDo* items, or mark some items as complete.</span></span> <span data-ttu-id="d6b70-132">Avsluta enheten eller simulator (eller framtvingar stänga appen) och starta om.</span><span class="sxs-lookup"><span data-stu-id="d6b70-132">Quit the device or simulator (or forcibly close the app) and restart.</span></span> <span data-ttu-id="d6b70-133">Kontrollera att dina ändringar har sparats på enheten eftersom de hade i det lokala arkivet SQLite.</span><span class="sxs-lookup"><span data-stu-id="d6b70-133">Verify that your changes have been persisted on the device because they are held in the local SQLite store.</span></span>
3. <span data-ttu-id="d6b70-134">Visa innehållet i Azure *TodoItem* tabell antingen med ett SQL-verktyg som *SQL Server Management Studio*, eller en REST-klient som *Fiddler* eller  *Postman*.</span><span class="sxs-lookup"><span data-stu-id="d6b70-134">View the contents of the Azure *TodoItem* table either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span> <span data-ttu-id="d6b70-135">Kontrollera att de nya objekt har *inte* har synkroniserats till servern</span><span class="sxs-lookup"><span data-stu-id="d6b70-135">Verify that the new items have *not* been synced to the server</span></span>
   
       + <span data-ttu-id="d6b70-136">För Node.js-serverdel, gå till den [Azure-portalen](https://portal.azure.com/), och i din Mobilapp backend på **enkelt tabeller** > **TodoItem** att visa innehållet i `TodoItem`tabell.</span><span class="sxs-lookup"><span data-stu-id="d6b70-136">For a Node.js backend, go to the [Azure portal](https://portal.azure.com/), and in your Mobile App backend click **Easy Tables** > **TodoItem** to view the contents of the `TodoItem` table.</span></span>
       + <span data-ttu-id="d6b70-137">Visa innehåll antingen med en SQL-verktyget som för en .NET-serverdel *SQL Server Management Studio*, eller en REST-klient som *Fiddler* eller *Postman*.</span><span class="sxs-lookup"><span data-stu-id="d6b70-137">For a .NET backend, view the table contents either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span>
4. <span data-ttu-id="d6b70-138">Aktivera WiFi i enheten eller simulatorn.</span><span class="sxs-lookup"><span data-stu-id="d6b70-138">Turn on WiFi in the device or simulator.</span></span> <span data-ttu-id="d6b70-139">Tryck sedan på den **uppdatera** knappen.</span><span class="sxs-lookup"><span data-stu-id="d6b70-139">Next, press the **Refresh** button.</span></span>
5. <span data-ttu-id="d6b70-140">Visa TodoItem igen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d6b70-140">View the TodoItem data again in the Azure portal.</span></span> <span data-ttu-id="d6b70-141">Nya och ändrade TodoItems bör nu visas.</span><span class="sxs-lookup"><span data-stu-id="d6b70-141">The new and changed TodoItems should now appear.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6b70-142">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d6b70-142">Additional Resources</span></span>
* <span data-ttu-id="d6b70-143">[offlinesynkronisering Data i Azure Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="d6b70-143">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="d6b70-144">[Molnet omfattar: Offlinesynkronisering i Azure Mobile Services] \(Obs: videon är på Mobile Services, men offlinesynkronisering fungerar på liknande sätt i Azure Mobile Apps\)</span><span class="sxs-lookup"><span data-stu-id="d6b70-144">[Cloud Cover: Offline Sync in Azure Mobile Services] \(note: the video is on Mobile Services, but offline sync works in a similar way in Azure Mobile Apps\)</span></span>

<!-- URLs. -->

<span data-ttu-id="d6b70-145">[offlinesynkronisering Data i Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="d6b70-145">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>

<span data-ttu-id="d6b70-146">[skapa en Android-App]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="d6b70-146">[Create an Android App]: app-service-mobile-android-get-started.md</span></span>

<span data-ttu-id="d6b70-147">[Molnet omfattar: Offlinesynkronisering i Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span><span class="sxs-lookup"><span data-stu-id="d6b70-147">[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span></span>
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

