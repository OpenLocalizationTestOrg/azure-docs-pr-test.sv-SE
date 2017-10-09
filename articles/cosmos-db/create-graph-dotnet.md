---
title: en Azure Cosmos DB .NET-program med aaaBuild hello Graph API | Microsoft Docs
description: "Anger en .NET-kodexempel som du kan använda tooconnect tooand fråga Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/28/2017
ms.author: denlee
ms.openlocfilehash: f28790fcb8c9f57c7bb3d844d8276fa04abcc39c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-hello-graph-api"></a>Azure Cosmos DB: Skapa ett .NET-program med hjälp av hello Graph API

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här snabbstartsguide visar hur toocreate Azure DB som Cosmos-kontot, databas och diagram (behållaren) med hjälp av hello Azure-portalen. Du sedan skapa och köra en konsolapp som bygger på hello [Graph API](graph-sdk-dotnet.md) (förhandsversion).  

## <a name="prerequisites"></a>Krav

Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Skapa ett databaskonto

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Lägga till en graf

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Nu ska vi klona Graph-API-app från github, ange hello anslutningssträngen och kör den. Du ser hur enkelt som det är att toowork med data programmässigt. 

1. Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.  

2. Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Öppna Visual Studio och öppna hello lösningsfilen. 

## <a name="review-hello-code"></a>Granska hello kod

Låt oss göra en snabb genomgång av vad som händer i hello app. Öppna hello Program.cs-filen och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser. 

* Hej DocumentClient har initierats. I hello preview vi har lagt till ett tillägg för graph API på hello Azure DB som Cosmos-klienten. Vi arbetar på en fristående diagrammet klient frikopplad från hello Azure Cosmos DB-klient och resurser.

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* En ny databas skapas.

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* En ny graf skapas.

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* En serie Gremlin steg utförs med hjälp av hello `CreateGremlinQuery` metod.

    ```csharp
    // hello CreateGremlinQuery method extensions allow you tooexecute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.

1. Öppna hello App.config-filen i Visual Studio 2017. 

2. I hello Azure-portalen i ditt Azure DB som Cosmos-konto, klickar du på **nycklar** i hello vänstra navigeringsrutan. 

    ![Visa och kopiera en primärnyckel i hello Azure-portalen på hello nycklar sida](./media/create-graph-dotnet/keys.png)

3. Kopiera ditt **URI** värdet från hello-portalen och gör den hello värdet för hello Endpoint nyckeln i App.config. Du kan använda hello kopieringsknappen enligt hello föregående skärmbild toocopy hello värde.

    `<add key="Endpoint" value="https://FILLME.documents.azure.com:443" />`

4. Kopiera ditt **PRIMÄRNYCKEL** värdet från hello-portalen och gör den hello värdet för hello auktoriseringsnyckel nyckeln i App.config och sedan spara ändringarna. 

    `<add key="AuthKey" value="FILLME" />`

Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB. 

## <a name="run-hello-console-app"></a>Kör hello-konsolprogram

1. I Visual Studio högerklickar du på hello **GraphGetStarted** projektet i **Solution Explorer** och klicka sedan på **hantera NuGet-paket**. 

2. I hello NuGet **Bläddra** skriver *Microsoft.Azure.Graphs* och kontrollera hello **innehåller förhandsversion** rutan. 

3. Hello resultat för att installera hello **Microsoft.Azure.Graphs** bibliotek. Detta installerar hello Azure Cosmos DB graph filnamnstillägget biblioteket paket och alla beroenden.

    Om du får ett meddelande om att granska ändringar toohello lösning, klickar du på **OK**. Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.

4. Klicka på CTRL + F5 toorun hello program.

   hello konsolfönstret visar hello brytpunkter och kanter som läggs till toohello diagram. När hello skriptet har slutförts, tryck på RETUR två gånger tooclose hello konsolfönstret. 

## <a name="browse-using-hello-data-explorer"></a>Bläddra med hello Data Explorer

Du kan nu gå tillbaka tooData Explorer i hello Azure-portalen och bläddra och fråga din nya diagramdata.

1. I Data Explorer visas hello ny databas hello diagram i fönstret. Expandera **graphdb**, **graphcollz** och klicka på **Diagram**.

2. Klicka på hello **Använd Filter** knappen toouse hello standard fråga tooview alla hello verticies i hello graph. hello-data som genereras av hello exempelapp visas hello diagram i fönstret.

    Du kan zooma in och ut hello diagram, kan du expandera hello diagrammet skärmutrymmet, lägga till ytterligare verticies och flytta verticies på hello visa yta.

    ![Visa hello diagram i Data Explorer hello Azure-portalen](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-hello-azure-portal"></a>Granska SLA: er i hello Azure-portalen

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg: 

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto och skapa diagram med hello Data Explorer och kör en app i denna Snabbstart. Nu kan du skapa mer komplexa frågor och implementera kraftfull logik för grafbläddring med Gremlin. 

> [!div class="nextstepaction"]
> [Fråga med hjälp av Gremlin](tutorial-query-graph.md)

