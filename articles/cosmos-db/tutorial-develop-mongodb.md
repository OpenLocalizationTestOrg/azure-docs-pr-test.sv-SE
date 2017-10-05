---
title: "Använda Azure Cosmos DB API för MongoDB för att skapa en webbapp | Microsoft Docs"
description: "En självstudiekurs om Azure Cosmos DB som skapar en onlinedatabas webbprogram som använder API: et för MongoDB."
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
ms.openlocfilehash: ff277c7f88359cd977424f2e0958c69e2547a2af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-connect-to-a-mongodb-app-using-net"></a><span data-ttu-id="fc7b5-104">Azure Cosmos DB: Ansluta till en MongoDB-app med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="fc7b5-104">Azure Cosmos DB: Connect to a MongoDB app using .NET</span></span>

<span data-ttu-id="fc7b5-105">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-105">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="fc7b5-106">Du kan snabbt skapa och ställa frågor mot databaser med dokument, nyckel/värde-par och grafer. Du får fördelar av den globala distributionen och den horisontella skalningsförmågan som ligger i grunden hos Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-106">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="fc7b5-107">Den här kursen visar hur du skapar ett Azure DB som Cosmos-konto med Azure-portalen och hur du skapar en databas och samling för att lagra data med hjälp av den [MongoDB API](mongodb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fc7b5-107">This tutorial demonstrates how to create an Azure Cosmos DB account using the Azure portal, and how to create a database and collection to store data using the [MongoDB API](mongodb-introduction.md).</span></span> 

<span data-ttu-id="fc7b5-108">Den här kursen ingår följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="fc7b5-108">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fc7b5-109">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="fc7b5-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="fc7b5-110">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="fc7b5-110">Update your connection string</span></span>
> * <span data-ttu-id="fc7b5-111">Skapa en MongoDB-app på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="fc7b5-111">Create a MongoDB app on a virtual machine</span></span> 


## <a name="create-a-database-account"></a><span data-ttu-id="fc7b5-112">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="fc7b5-112">Create a database account</span></span>

<span data-ttu-id="fc7b5-113">Börja med att skapa ett Azure DB som Cosmos-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-113">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="fc7b5-114">Redan har ett Azure DB som Cosmos-konto?</span><span class="sxs-lookup"><span data-stu-id="fc7b5-114">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="fc7b5-115">I så fall, gå vidare till [konfigurera Visual Studio-lösning](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="fc7b5-115">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="fc7b5-116">Hade du ett Azure DocumentDB-konto?</span><span class="sxs-lookup"><span data-stu-id="fc7b5-116">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="fc7b5-117">Om ditt konto är nu ett Azure DB som Cosmos-konto och du kan gå vidare till så [konfigurera Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="fc7b5-117">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="fc7b5-118">Om du använder Azure Cosmos DB-emulatorn, följer du stegen i [Azure Cosmos DB emulatorn](local-emulator.md) konfigurera emulatorn och gå vidare till [ställa in din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="fc7b5-118">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a><span data-ttu-id="fc7b5-119">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="fc7b5-119">Update your connection string</span></span>

1. <span data-ttu-id="fc7b5-120">I Azure-portalen i den **Azure Cosmos DB** väljer API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-120">In the Azure portal, in the **Azure Cosmos DB** page, select the API for MongoDB account.</span></span> 
2. <span data-ttu-id="fc7b5-121">I fältet till vänster på konto-bladet, klickar du på **Snabbstart**.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-121">In the left bar of the account blade, click **Quick start**.</span></span> 
3. <span data-ttu-id="fc7b5-122">Välj din plattform (*.NET drivrutinen*, *Node.js-drivrutinen*, *MongoDB Shell*, *Java-drivrutinen*, *Python-drivrutinen*).</span><span class="sxs-lookup"><span data-stu-id="fc7b5-122">Choose your platform (*.NET driver*, *Node.js driver*, *MongoDB Shell*, *Java driver*, *Python driver*).</span></span> <span data-ttu-id="fc7b5-123">Om du inte ser ditt drivrutin eller verktyg, oroa dig inte, vi dokumentera kontinuerligt mer kodfragment för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-123">If you don't see your driver or tool listed, don't worry, we continuously document more connection code snippets.</span></span> 
4. <span data-ttu-id="fc7b5-124">Kopiera och klistra in kodfragmentet till MongoDB-app, och du är redo att sätta igång.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-124">Copy and paste the code snippet into your MongoDB app, and you are ready to go.</span></span>

## <a name="set-up-your-mongodb-app"></a><span data-ttu-id="fc7b5-125">Konfigurera din app för MongoDB</span><span class="sxs-lookup"><span data-stu-id="fc7b5-125">Set up your MongoDB app</span></span>

<span data-ttu-id="fc7b5-126">Du kan använda den [skapa en webbapp i Azure som ansluter till MongoDB som körs på en virtuell dator](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) självstudiekurs med minimala ändringar, att snabbt konfigurera en MongoDB-programmet (antingen lokalt eller publiceras på en Azure-app) som ansluter till en API för MongoDB-kontot.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-126">You can use the [Create a web app in Azure that connects to MongoDB running on a virtual machine](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) tutorial, with minimal modification, to quickly setup a MongoDB application (either locally or published to an Azure web app) that connects to an API for MongoDB account.</span></span>  

1. <span data-ttu-id="fc7b5-127">Följ självstudierna med en enda ändring.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-127">Follow the tutorial, with one modification.</span></span>  <span data-ttu-id="fc7b5-128">Ersätt Koden Dal.cs med detta:</span><span class="sxs-lookup"><span data-stu-id="fc7b5-128">Replace the Dal.cs code with this:</span></span>

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
   
            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from the MongoDB server.        
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
   
            // Creates a Task and inserts it into the collection in MongoDB.
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

2. <span data-ttu-id="fc7b5-129">Ändra följande variabler i filen Dal.cs per inställningarna för ditt konto från sidan nycklar i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="fc7b5-129">Modify the following variables in the Dal.cs file per your account settings from the Keys page in the Azure portal:</span></span>

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. <span data-ttu-id="fc7b5-130">Använd appen!</span><span class="sxs-lookup"><span data-stu-id="fc7b5-130">Use the app!</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="fc7b5-131">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="fc7b5-131">Clean up resources</span></span>

<span data-ttu-id="fc7b5-132">Om du inte planerar att använda den här appen mer följer du stegen nedan för att ta bort alla resurser som du har skapat i den här självstudiekursen på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-132">If you're not going to continue to use this app, use the following steps to delete all resources created by this tutorial in the Azure portal.</span></span> 

1. <span data-ttu-id="fc7b5-133">Klicka på **Resursgrupper** på den vänstra menyn i Azure Portal och sedan på namnet på den resurs du skapade.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-133">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="fc7b5-134">På sidan med resursgrupper klickar du på **Ta bort**, skriver in namnet på resursen att ta bort i textrutan och klickar sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-134">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc7b5-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc7b5-135">Next steps</span></span>

<span data-ttu-id="fc7b5-136">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="fc7b5-136">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fc7b5-137">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="fc7b5-137">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="fc7b5-138">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="fc7b5-138">Update your connection string</span></span>
> * <span data-ttu-id="fc7b5-139">Skapa en MongoDB-app på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="fc7b5-139">Create a MongoDB app on a virtual machine</span></span>

<span data-ttu-id="fc7b5-140">Du kan gå vidare till nästa kurs och importera MongoDB-data till Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fc7b5-140">You can proceed to the next tutorial and import your MongoDB data to Azure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="fc7b5-141">Importera MongoDB-data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fc7b5-141">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)

