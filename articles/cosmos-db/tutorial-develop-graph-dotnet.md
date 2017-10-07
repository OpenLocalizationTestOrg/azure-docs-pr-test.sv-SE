---
title: 'Azure Cosmos DB: Utveckla med hello Graph API i .NET | Microsoft Docs'
description: "Lär dig hur toodevelop med Azure Cosmos DB DocumentDB-API med hjälp av .NET"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.custom: mvc
ms.openlocfilehash: 12e435d8169aeee6e818dac4a3b66c7a0ec5f2d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-graph-api-in-net"></a>Azure Cosmos DB: Utveckla med hello Graph API i .NET
Azure Cosmos-DB är Microsofts globalt distribuerade flera modellen database-tjänsten. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här kursen visar hur toocreate ett Azure DB som Cosmos-kontot med hello Azure-portalen och toocreate ett diagram databas och en behållare. sedan hello program skapar ett enkelt sociala nätverk med fyra personer som använder hello [Graph API](graph-sdk-dotnet.md) (förhandsversion) och sedan passerar och frågar hello diagram med hjälp av Gremlin.

Den här kursen ingår hello följande uppgifter:

> [!div class="checklist"]
> * Skapa ett Azure Cosmos DB-konto 
> * Skapa ett diagram databas och en behållare
> * Serialisera hörn och kanter too.NET objekt
> * Lägg till hörn och kanter
> * Frågan hello diagram med hjälp av Gremlin

## <a name="graphs-in-azure-cosmos-db"></a>Diagram i Azure Cosmos DB
Du kan använda Azure Cosmos DB toocreate-, update- och fråga diagram med hjälp av hello [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) bibliotek. Hej Microsoft.Azure.Graph-bibliotek innehåller en enda metod `CreateGremlinQuery<T>` ovanpå hello `DocumentClient` klassen tooexecute Gremlin frågor.

Gremlin är en funktionell programmeringsspråk som stöder skriva åtgärder (DML) och fråga och traversal-åtgärder. Vi igenom några exempel i den här artikeln tooget din igång med Gremlin. Se [Gremlin frågor](gremlin-support.md) en detaljerad genomgång av Gremlin av funktionerna i Azure Cosmos DB. 

## <a name="prerequisites"></a>Krav
Kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/). 
    * Du kan också använda hello [Azure DocumentDB-emulatorn](local-emulator.md) för den här självstudiekursen.
* [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Skapa konto

Börja med att skapa ett Azure DB som Cosmos-konto i hello Azure-portalen.  

> [!TIP]
> * Redan har ett Azure DB som Cosmos-konto? I så fall, kan du gå vidare för[konfigurera Visual Studio-lösning](#SetupVS)
> * Hade du ett Azure DocumentDB-konto? Om ditt konto är nu en Azure DB som Cosmos-konto och du gå vidare för så[konfigurera Visual Studio-lösning](#SetupVS).  
> * Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[ställa in din Visual Studio-lösning](#SetupVS). 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Konfigurera din Visual Studio-lösning
1. Öppna **Visual Studio** på datorn.
2. På hello **filen** väljer du **ny**, och välj sedan **projekt**.
3. I hello **nytt projekt** markerar **mallar** / **Visual C#** / **Konsolapp (.NET Framework)**namnge projektet och klicka sedan på **OK**.
4. I hello **Solution Explorer**, högerklicka på den nya konsolappen, som är under din Visual Studio-lösning, och klicka sedan på **hantera NuGet-paket...**
5. I hello **NuGet** klickar du på **Bläddra**, och skriv **Microsoft.Azure.Graphs** i sökrutan hello och kontrollera hello **inkluderar förhandsversioner**.
6. I hello resultat hitta **Microsoft.Azure.Graphs** och på **installera**.
   
   Om du får ett meddelande om att granska ändringar toohello lösning, klickar du på **OK**. Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.
   
    Hej `Microsoft.Azure.Graphs` -bibliotek innehåller en enda metod `CreateGremlinQuery<T>` för att köra Gremlin åtgärder. Gremlin är en funktionell programmeringsspråk som stöder skriva åtgärder (DML) och fråga och traversal-åtgärder. Vi igenom några exempel i den här artikeln tooget din igång med Gremlin. [Gremlin frågor](gremlin-support.md) har en detaljerad genomgång av Gremlin funktioner i Azure Cosmos DB.

## <a id="add-references"></a>Anslut appen

Lägg till nedanstående två konstanter och dina *klienten* variabeln i ditt program. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
Därefter head tillbaka toohello [Azure-portalen](https://portal.azure.com) tooretrieve din slutpunkts-URL och primärnyckel. hello slutpunkts-URL och primärnyckel behövs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning. 

I hello Azure portal, navigerar tooyour Azure Cosmos DB konto, klickar på **nycklar**, och klicka sedan på **skrivskyddad nycklar**. 

Kopiera hello URI från hello-portalen och via `Endpoint` i hello slutpunktsegenskapen ovan. Och sedan kopiera hello PRIMÄRNYCKEL från hello-portalen och klistra in den i hello `AuthKey` egenskapen ovan. 

! [Skärmbild som visar hello Azure-portalen som används av hello självstudiekursen toocreate C#-program. Visar en Azure Cosmos DB konto hello knappen nycklar markerad i hello Azure Cosmos DB navigering och hello URI och PRIMÄRNYCKEL-värden som är markerade i hello bladet nycklar] [nycklar] 
 
## <a id="instantiate"></a>Skapa en instans av hello DocumentClient 
Skapa sedan en ny instans av hello **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Skapa en databas 

Nu ska du skapa en Azure-Cosmos-DB [databasen](documentdb-resources.md#databases) med hjälp av hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metod eller [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metod för hello  **DocumentClient** klass från hello [.NET DocumentDB SDK](documentdb-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Skapa ett diagram 

Skapa sedan en behållare för diagram med hjälp av hello med hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metod eller [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metod för hello **DocumentClient**  klass. En samling är en behållare för diagram entiteter. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <a id="serializing"></a>Serialisera hörn och kanter too.NET objekt
Använder Azure Cosmos-DB hello [GraphSON kabelformat](gremlin-support.md), som definierar en JSON-schema för formhörnen, kanter och egenskaper. hello Azure Cosmos DB .NET SDK innehåller JSON.NET som ett beroende och detta gör att vi tooserialize/deserialisering GraphSON i .NET-objekt som vi kan arbeta med kod.

Exempelvis vi arbeta med ett enkelt sociala nätverk med fyra personer. Vi titta på hur toocreate `Person` formhörnen, lägga till `Knows` relationer mellan dem, och sedan fråga och passerar hello diagram toofind ”vän vän” relationer. 

Hej `Microsoft.Azure.Graphs.Elements` namnområde ger `Vertex`, `Edge`, `Property` och `VertexProperty` klasser för avserialisering av GraphSON svar toowell användardefinierade .NET objekt.

## <a name="run-gremlin-using-creategremlinquery"></a>Kör Gremlin med CreateGremlinQuery
Gremlin som SQL, stöder Läs-, Skriv- och frågor. Till exempel hello följande utdrag visar hur toocreate formhörnen, kanter, utföra vissa exempelfrågor som använder `CreateGremlinQuery<T>`, och asynkront gå igenom dessa resultat med hjälp av `ExecuteNextAsync` och ' HasMoreResults.

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // hello CreateGremlinQuery method extensions allow you tooexecute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a>Lägg till hörn och kanter

Nu ska vi titta på hello Gremlin rapporterna visas i föregående avsnitt hello detalj. Första vi vissa formhörnen med Gremlin's `addV` metod. Till exempel skapas hello följande kodavsnitt en ”Thomas Andersen” nod av typen ”Person”, med egenskaper för förnamn, efternamn och ålder.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Vi skapa vissa kanter mellan dessa formhörnen med Gremlin's `addE` metod. 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

Vi kan uppdatera en befintlig vertex med `properties` steg i Gremlin. Vi hoppar över hello anropet tooexecute hello frågan via `HasMoreResults` och `ExecuteNextAsync` för hello resten av hello exempel.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

Du kan släppa kanter och formhörnen med Gremlin's `drop` steg. Här är ett kodfragment som visar hur toodelete en nod och en kant. Observera att släppa en nod utför borttagningsåtgärder av hello associerade kanter.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-hello-graph"></a>Frågan hello diagram

Du kan utföra frågor och traversals som också använder Gremlin. Hello följande fragment visar till exempel hur toocount hello antalet formhörnen i hello diagram:

```cs
// Run a query toocount vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
Du kan utföra filter med Gremlin's `has` och `hasLabel` steg och kombinera dem med hjälp av `and`, `or`, och `not` toobuild mer komplexa filter:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

Du kan projicera vissa egenskaper i hello frågeresultat med hello `values` steg:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Hittills har sett vi endast frågeoperatorer som fungerar i en databas. Diagram är snabb och effektiv för traversal åtgärder när du behöver toonavigate toorelated kanter och formhörnen. Vi ska hitta alla vänner till Thomas. Vi kan göra detta med hjälp av Gremlin's `outE` steg toofind alla hello kanter ut från Thomas och sedan gå igenom toohello i formhörnen från dessa kanter med Gremlin's `inV` steg:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

hello nästa frågan utför två hopp toofind alla Thomas' ”vänner till vänner”, genom att anropa `outE` och `inV` två gånger. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Du kan skapa mer komplexa frågor och implementera kraftfulla diagrammet traversal logik med Gremlin, inklusive blandningen filter uttryck, utför slingor med hello `loop` steg och implementera villkorlig navigeringen med hello `choose` steg. Mer information om vad du kan göra med [Gremlin stöd](gremlin-support.md)!

Det, självstudierna Azure Cosmos DB är klar! 

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, använda hello följande steg toodelete alla resurser som skapats av den här kursen i hello Azure-portalen.  

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Skapa ett Azure DB som Cosmos-konto 
> * Skapa ett diagram databas och en behållare
> * Serialiserade hörn och kanter too.NET objekt
> * Tillagda hörn och kanter
> * Efterfrågade hello diagram med hjälp av Gremlin

Nu kan du skapa mer komplexa frågor och implementera kraftfull logik för grafbläddring med Gremlin. 

> [!div class="nextstepaction"]
> [Fråga med hjälp av Gremlin](tutorial-query-graph.md)
