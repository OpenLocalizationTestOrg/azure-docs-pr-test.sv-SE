---
title: 'Azure Cosmos DB: Skapa en webbapp med .NET och hello DocumentDB API | Microsoft Docs'
description: "Anger en .NET-kodexempel som du kan använda tooconnect tooand query hello Azure Cosmos DB DocumentDB API"
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
ms.openlocfilehash: 35517e35d80c48662a51a99814652ffa1121fc5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-hello-azure-portal"></a>Azure DB Cosmos: Skapa en DocumentDB-API-webbapp med .NET och hello Azure-portalen

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen. Du sedan skapa och distribuera en todo lista-webbapp som bygger på hello [DocumentDB .NET API](documentdb-sdk-dotnet.md)som visas i följande skärmbild hello. 

![Att göra-app med exempeldata](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a>Krav

Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Skapa ett databaskonto

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a>Lägga till en samling

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Lägg till exempeldata

Du kan nu lägga till tooyour nya insamling av data med hjälp av Data Explorer.

1. I Data Explorer visas hello ny databas i hello samlingar rutan. Expandera hello **uppgifter** databas, expandera hello **objekt** samlingen, klickar du på **dokument**, och klicka sedan på **nya dokument**. 

   ![Skapa nya dokument i Data Explorer i hello Azure-portalen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Lägg till en dokumentsamling toohello med följande struktur hello.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. När du har lagt till hello json toohello **dokument** klickar du på **spara**.

    ![Kopiera i json-data och klicka på Spara i Data Explorer i hello Azure-portalen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  Skapa och spara en mer dokument där du infoga ett unikt värde för hello `id` egenskap och hello andra egenskaper som du vill ändra. Dina nya dokument kan ha vilken struktur du vill eftersom Azure Cosmos DB inte kräver något schema för dina data.

     Du kan nu använda frågor i Data Explorer tooretrieve dina data. Som standard använder Data Explorer `SELECT * FROM c` tooretrieve alla dokument i hello samlingen, men du kan ändra den olika tooa [SQL-frågan](documentdb-sql-query.md), som `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn alla hello dokument i fallande ordning utifrån tidsstämpel.
 
     Du kan också använda Data Explorer toocreate lagrade procedurer, UDF: er och utlösare tooperform serversidan affärslogik samt som skala genomflöde. Data Explorer visar alla hello inbyggda programmatisk åtkomst till data i hello API: er, men ger enkel åtkomst tooyour data i hello Azure-portalen.

## <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Nu ska vi växla tooworking med kod. Vi ska klona en DocumentDB-API-app från GitHub, ange hello anslutningssträngen och kör den. Du ser hur enkelt som det är att toowork med data programmässigt. 

1. Öppna ett git terminalfönster, till exempel git bash och `CD` tooa arbetskatalogen.  

2. Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. Öppna sedan hello todo lösningsfilen i Visual Studio. 

## <a name="review-hello-code"></a>Granska hello kod

Låt oss göra en snabb genomgång av vad som händer i hello app. Öppna hello DocumentDBRepository.cs fil- och du hittar att dessa rader med kod skapar hello Azure Cosmos DB resurser. 

* Hej DocumentClient har initierats på rad 73.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* En ny databas skapas på rad 88.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* En ny samling skapas på rad 107.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.

1. I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**. I hello web.config-fil i hello nästa steg ska du använda hello kopiera knappar hello höger på hello skärmen toocopy hello URI och primärnyckel.

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-dotnet/keys.png)

2. Öppna hello web.config-filen i Visual Studio 2017. 

3. Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello endpoint nyckeln i web.config. 

    `<add key="endpoint" value="FILLME" />`

4. Kopiera värdet för PRIMÄRNYCKELN från hello-portalen och gör det hello värdet för hello auktoriseringsnyckel i web.config. Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB. 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-hello-web-app"></a>Köra hello-webbprogram
1. Högerklicka på hello-projekt i Visual Studio **Solution Explorer** och klicka sedan på **hantera NuGet-paket**. 

2. I hello NuGet **Bläddra** skriver *DocumentDB*.

3. Hello resultat för att installera hello **Microsoft.Azure.DocumentDB** bibliotek. Detta installerar hello Microsoft.Azure.DocumentDB paket samt alla beroenden.

4. Klicka på CTRL + F5 toorun hello program. Appen visas i din webbläsare. 

5. Klicka på **Skapa nytt** i hello webbläsare och skapa några nya aktiviteter i din att göra-app.

   ![Att göra-app med exempeldata](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

Du kan nu gå tillbaka tooData Explorer Se fråga, ändra och arbeta med dessa nya data. 

## <a name="review-slas-in-hello-azure-portal"></a>Granska SLA: er i hello Azure-portalen

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en samling med hello Data Explorer och kör ett webbprogram i denna Snabbstart. Nu kan du importera ytterligare data tooyour Cosmos-DB-konto. 

> [!div class="nextstepaction"]
> [Importera data till Azure Cosmos DB](import-data.md)


