---
title: "aaaUse Azure Cosmos DBS API: et för MongoDB toobuild en webbapp | Microsoft Docs"
description: "En självstudiekurs om Azure Cosmos DB som skapar en onlinedatabas webbprogrammet med hello API för MongoDB."
keywords: mongodb-exempel
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 6dfa7fef49fc53ea2fcfcfbad3b3fcf97ac18e94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-connect-tooa-mongodb-app-using-net"></a><span data-ttu-id="dfa11-104">Azure Cosmos databasen: Anslut tooa MongoDB-app med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="dfa11-104">Azure Cosmos DB: Connect tooa MongoDB app using .NET</span></span>

<span data-ttu-id="dfa11-105">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="dfa11-105">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="dfa11-106">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dfa11-106">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="dfa11-107">Den här kursen visar hur toocreate ett Azure DB som Cosmos-kontot med hello Azure-portalen och hur toocreate en databas och samling toostore data med hello [MongoDB API](mongodb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dfa11-107">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal, and how toocreate a database and collection toostore data using hello [MongoDB API](mongodb-introduction.md).</span></span> 

<span data-ttu-id="dfa11-108">Den här kursen ingår hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="dfa11-108">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfa11-109">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="dfa11-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="dfa11-110">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="dfa11-110">Update your connection string</span></span>
> * <span data-ttu-id="dfa11-111">Skapa en MongoDB-app på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="dfa11-111">Create a MongoDB app on a virtual machine</span></span> 


## <a name="create-a-database-account"></a><span data-ttu-id="dfa11-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="dfa11-112">Create a database account</span></span>

<span data-ttu-id="dfa11-113">Börja med att skapa ett Azure DB som Cosmos-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dfa11-113">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="dfa11-114">Redan har ett Azure DB som Cosmos-konto?</span><span class="sxs-lookup"><span data-stu-id="dfa11-114">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="dfa11-115">I så fall, kan du gå vidare för[konfigurera Visual Studio-lösning](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="dfa11-115">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="dfa11-116">Hade du ett Azure DocumentDB-konto?</span><span class="sxs-lookup"><span data-stu-id="dfa11-116">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="dfa11-117">Om ditt konto är nu en Azure DB som Cosmos-konto och du gå vidare för så[konfigurera Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="dfa11-117">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="dfa11-118">Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[ställa in din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="dfa11-118">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a><span data-ttu-id="dfa11-119">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="dfa11-119">Update your connection string</span></span>

1. <span data-ttu-id="dfa11-120">I hello Azure-portalen i hello **Azure Cosmos DB** väljer hello API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="dfa11-120">In hello Azure portal, in hello **Azure Cosmos DB** page, select hello API for MongoDB account.</span></span> 
2. <span data-ttu-id="dfa11-121">I hello vänster-fältet för hello-kontoblad klickar du på **Snabbstart**.</span><span class="sxs-lookup"><span data-stu-id="dfa11-121">In hello left bar of hello account blade, click **Quick start**.</span></span> 
3. <span data-ttu-id="dfa11-122">Välj din plattform (*.NET drivrutinen*, *Node.js-drivrutinen*, *MongoDB Shell*, *Java-drivrutinen*, *Python-drivrutinen*).</span><span class="sxs-lookup"><span data-stu-id="dfa11-122">Choose your platform (*.NET driver*, *Node.js driver*, *MongoDB Shell*, *Java driver*, *Python driver*).</span></span> <span data-ttu-id="dfa11-123">Om du inte ser ditt drivrutin eller verktyg, oroa dig inte, vi dokumentera kontinuerligt mer kodfragment för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="dfa11-123">If you don't see your driver or tool listed, don't worry, we continuously document more connection code snippets.</span></span> 
4. <span data-ttu-id="dfa11-124">Kopiera och klistra in hello kodstycke i din app MongoDB, och du är redo toogo.</span><span class="sxs-lookup"><span data-stu-id="dfa11-124">Copy and paste hello code snippet into your MongoDB app, and you are ready toogo.</span></span>

## <a name="set-up-your-mongodb-app"></a><span data-ttu-id="dfa11-125">Konfigurera din app för MongoDB</span><span class="sxs-lookup"><span data-stu-id="dfa11-125">Set up your MongoDB app</span></span>

<span data-ttu-id="dfa11-126">Du kan använda hello [skapa en webbapp i Azure som ansluter tooMongoDB som körs på en virtuell dator](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) självstudiekurs med minimala ändringar, tooquickly installationsprogrammet ett MongoDB-program (antingen lokalt eller publicerade tooan Azure webbapp) som ansluter tooan API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="dfa11-126">You can use hello [Create a web app in Azure that connects tooMongoDB running on a virtual machine](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) tutorial, with minimal modification, tooquickly setup a MongoDB application (either locally or published tooan Azure web app) that connects tooan API for MongoDB account.</span></span>  

1. <span data-ttu-id="dfa11-127">Följ hello självstudier med en enda ändring.</span><span class="sxs-lookup"><span data-stu-id="dfa11-127">Follow hello tutorial, with one modification.</span></span>  <span data-ttu-id="dfa11-128">Ersätt hello Dal.cs kod med detta:</span><span class="sxs-lookup"><span data-stu-id="dfa11-128">Replace hello Dal.cs code with this:</span></span>

    ```csharp   
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;
    using System.Security.Authentication;
   
    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            //private MongoServer mongoServer = null;
            private bool disposed = false;
   
            // toodo: update hello connection string with hello DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  hello database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from hello MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }
   
            // Creates a Task and inserts it into hello collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }
   
            private IMongoCollection<MyTask> GetTasksCollection()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10255);
                settings.UseSsl = true;
                settings.SslSettings = new SslSettings();
                settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
   
                MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                MongoIdentityEvidence evidence = new PasswordEvidence(password);
   
                settings.Credentials = new List<MongoCredential>()
                {
                    new MongoCredential("SCRAM-SHA-1", identity, evidence)
                };
   
                MongoClient client = new MongoClient(settings);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10255);
                settings.UseSsl = true;
                settings.SslSettings = new SslSettings();
                settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
   
                MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                MongoIdentityEvidence evidence = new PasswordEvidence(password);
   
                settings.Credentials = new List<MongoCredential>()
                {
                    new MongoCredential("SCRAM-SHA-1", identity, evidence)
                };
                MongoClient client = new MongoClient(settings);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            # region IDisposable
   
            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
   
            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                    }
                }
   
                this.disposed = true;
            }
   
            # endregion
        }
    }
    ```

2. <span data-ttu-id="dfa11-129">Ändra hello följande variabler i hello Dal.cs fil per inställningarna för ditt konto från hello nycklar sida i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="dfa11-129">Modify hello following variables in hello Dal.cs file per your account settings from hello Keys page in hello Azure portal:</span></span>

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. <span data-ttu-id="dfa11-130">Använd hello appen!</span><span class="sxs-lookup"><span data-stu-id="dfa11-130">Use hello app!</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="dfa11-131">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="dfa11-131">Clean up resources</span></span>

<span data-ttu-id="dfa11-132">Om du inte kommer toocontinue toouse den här appen, använda hello följande steg toodelete alla resurser som skapats av den här kursen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dfa11-132">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span> 

1. <span data-ttu-id="dfa11-133">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="dfa11-133">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="dfa11-134">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="dfa11-134">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfa11-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dfa11-135">Next steps</span></span>

<span data-ttu-id="dfa11-136">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="dfa11-136">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfa11-137">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="dfa11-137">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="dfa11-138">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="dfa11-138">Update your connection string</span></span>
> * <span data-ttu-id="dfa11-139">Skapa en MongoDB-app på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="dfa11-139">Create a MongoDB app on a virtual machine</span></span>

<span data-ttu-id="dfa11-140">Du kan fortsätta toohello nästa kurs och importera din MongoDB data tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dfa11-140">You can proceed toohello next tutorial and import your MongoDB data tooAzure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="dfa11-141">Importera MongoDB-data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="dfa11-141">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)

