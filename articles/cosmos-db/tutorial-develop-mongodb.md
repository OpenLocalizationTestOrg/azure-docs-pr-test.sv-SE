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
# <a name="azure-cosmos-db-connect-tooa-mongodb-app-using-net"></a>Azure Cosmos databasen: Anslut tooa MongoDB-app med hjälp av .NET

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här kursen visar hur toocreate ett Azure DB som Cosmos-kontot med hello Azure-portalen och hur toocreate en databas och samling toostore data med hello [MongoDB API](mongodb-introduction.md). 

Den här kursen ingår hello följande uppgifter:

> [!div class="checklist"]
> * Skapa ett Azure Cosmos DB-konto 
> * Uppdatera din anslutningssträng
> * Skapa en MongoDB-app på en virtuell dator 


## <a name="create-a-database-account"></a>Skapa ett databaskonto

Börja med att skapa ett Azure DB som Cosmos-konto i hello Azure-portalen.  

> [!TIP]
> * Redan har ett Azure DB som Cosmos-konto? I så fall, kan du gå vidare för[konfigurera Visual Studio-lösning](#SetupVS)
> * Hade du ett Azure DocumentDB-konto? Om ditt konto är nu en Azure DB som Cosmos-konto och du gå vidare för så[konfigurera Visual Studio-lösning](#SetupVS).  
> * Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[ställa in din Visual Studio-lösning](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

1. I hello Azure-portalen i hello **Azure Cosmos DB** väljer hello API för MongoDB-kontot. 
2. I hello vänster-fältet för hello-kontoblad klickar du på **Snabbstart**. 
3. Välj din plattform (*.NET drivrutinen*, *Node.js-drivrutinen*, *MongoDB Shell*, *Java-drivrutinen*, *Python-drivrutinen*). Om du inte ser ditt drivrutin eller verktyg, oroa dig inte, vi dokumentera kontinuerligt mer kodfragment för anslutningen. 
4. Kopiera och klistra in hello kodstycke i din app MongoDB, och du är redo toogo.

## <a name="set-up-your-mongodb-app"></a>Konfigurera din app för MongoDB

Du kan använda hello [skapa en webbapp i Azure som ansluter tooMongoDB som körs på en virtuell dator](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) självstudiekurs med minimala ändringar, tooquickly installationsprogrammet ett MongoDB-program (antingen lokalt eller publicerade tooan Azure webbapp) som ansluter tooan API för MongoDB-kontot.  

1. Följ hello självstudier med en enda ändring.  Ersätt hello Dal.cs kod med detta:

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

2. Ändra hello följande variabler i hello Dal.cs fil per inställningarna för ditt konto från hello nycklar sida i hello Azure-portalen:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. Använd hello appen!

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, använda hello följande steg toodelete alla resurser som skapats av den här kursen i hello Azure-portalen. 

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Skapa ett Azure Cosmos DB-konto 
> * Uppdatera din anslutningssträng
> * Skapa en MongoDB-app på en virtuell dator

Du kan fortsätta toohello nästa kurs och importera din MongoDB data tooAzure Cosmos DB.  

> [!div class="nextstepaction"]
> [Importera MongoDB-data till Azure Cosmos DB](mongodb-migrate.md)

