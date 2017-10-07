---
title: 'Azure Cosmos DB: Skapa en webbapp med .NET och hello MongoDB API | Microsoft Docs'
description: "Anger en .NET-kodexempel som du kan använda tooconnect tooand query hello Azure Cosmos DB MongoDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: c85cc47f772a19aaa7181611b75a8acaedbc4c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-hello-azure-portal"></a>Azure DB Cosmos: Skapa en webbapp i MongoDB-API med .NET och hello Azure-portalen

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen. Du sedan skapa och distribuera en uppgifter lista-webbapp som bygger på hello [MongoDB .NET drivrutinen](https://docs.mongodb.com/ecosystem/drivers/csharp/). 

## <a name="prerequisites"></a>Krav

Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Skapa ett databaskonto

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Nu ska vi klona en MongoDB-API-app från github, ange hello anslutningssträngen och kör den. Du ser hur enkelt som det är att toowork med data programmässigt. 

1. Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.  

2. Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. Öppna sedan hello lösningsfilen i Visual Studio. 

## <a name="review-hello-code"></a>Granska hello kod

Låt oss göra en snabb genomgång av vad som händer i hello app. Öppna hello **Dal.cs** fil under hello **DAL** directory och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser. 

* Initiera hello Mongo-klienten.

    ```cs
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
    ```

* Hämta hello databas och hello samling.

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* Hämta alla dokument.

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.

1. I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **anslutningssträngen**, och klicka sedan på **skrivskyddad nycklar**. I hello Dal.cs fil i hello nästa steg ska du använda hello kopiera knappar hello höger på hello skärmen toocopy hello användarnamn, lösenord och värden.

2. Öppna hello **Dal.cs** filen i hello **DAL** directory. 

3. Kopiera ditt **användarnamn** värde från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello **användarnamn** i din **Dal.cs** fil. 

4. Kopiera ditt **värden** värdet från hello-portalen och gör den hello värdet för hello **värden** i din **Dal.cs** fil. 

5. Slutligen Kopiera ditt **lösenord** värdet från hello-portalen och gör den hello värdet för hello **lösenord** i din **Dal.cs** fil. 

Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB. 
    
## <a name="run-hello-web-app"></a>Köra hello-webbprogram

1. Högerklicka på hello-projekt i Visual Studio **Solution Explorer** och klicka sedan på **hantera NuGet-paket**. 

2. I hello NuGet **Bläddra** skriver *MongoDB.Driver*.

3. Hello resultat för att installera hello **MongoDB.Driver** bibliotek. Detta installerar hello MongoDB.Driver paket samt alla beroenden.

4. Klicka på CTRL + F5 toorun hello program. Appen visas i din webbläsare. 

5. Klicka på **skapa** i hello webbläsare och skapa några nya aktiviteter i appen uppgiften lista.

## <a name="review-slas-in-hello-azure-portal"></a>Granska SLA: er i hello Azure-portalen

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten du har lärt dig hur toocreate ett Azure DB som Cosmos-konto och kör en web app med hjälp av hello API för MongoDB. Nu kan du importera ytterligare data tooyour Cosmos-DB-konto. 

> [!div class="nextstepaction"]
> [Importera data till Azure Cosmos DB för hello MongoDB-API](mongodb-migrate.md)

