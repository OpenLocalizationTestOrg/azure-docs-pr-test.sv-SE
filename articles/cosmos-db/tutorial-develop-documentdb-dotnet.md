---
title: 'Azure Cosmos DB: Utveckla med hello DocumentDB API i .NET | Microsoft Docs'
description: "Lär dig hur toodevelop med Azure Cosmos DB DocumentDB-API med hjälp av .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 0d3d17afa782054c8fdf3cbac421e5a5d0a6800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmosdb-develop-with-hello-documentdb-api-in-net"></a>Azure CosmosDB: Utveckla med hello DocumentDB API i .NET

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här kursen visar hur toocreate ett Azure DB som Cosmos-kontot med hello Azure-portalen och sedan skapa ett dokument databas och samling med en [partitionsnyckel](documentdb-partition-data.md#partition-keys) med hello [DocumentDB .NET API](documentdb-introduction.md). Genom att definiera en partitionsnyckel när du skapar en samling kan förbereds programmet tooscale utan problem när dina data växer. 

Den här självstudiekursen omfattar hello följande uppgifter med hjälp av hello [DocumentDB .NET API](documentdb-sdk-dotnet.md):

> [!div class="checklist"]
> * Skapa ett Azure Cosmos DB-konto
> * Skapa en databas och samling med en partitionsnyckel
> * Skapa JSON-dokument
> * Uppdatera ett dokument
> * Fråga partitionerade samlingar
> * Köra lagrade procedurer
> * Ta bort ett dokument
> * Ta bort en databas

## <a name="prerequisites"></a>Krav
Kontrollera att du har hello följande:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/). 
    * Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen om du vill att toouse en lokal miljö som emulerar hello Azure DocumentDB-tjänsten för utveckling.
* [Visual Studio](http://www.visualstudio.com/).

## <a name="create-an-azure-cosmos-db-account"></a>Skapa ett Azure Cosmos DB-konto

Börja med att skapa ett Azure DB som Cosmos-konto i hello Azure-portalen.

> [!TIP]
> * Redan har ett Azure DB som Cosmos-konto? I så fall, kan du gå vidare för[konfigurera Visual Studio-lösning](#SetupVS)
> * Hade du ett Azure DocumentDB-konto? Om ditt konto är nu en Azure DB som Cosmos-konto och du gå vidare för så[konfigurera Visual Studio-lösning](#SetupVS).  
> * Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[ställa in din Visual Studio-lösning](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Konfigurera din Visual Studio-lösning
1. Öppna **Visual Studio** på datorn.
2. På hello **filen** väljer du **ny**, och välj sedan **projekt**.
3. I hello **nytt projekt** markerar **mallar** / **Visual C#** / **Konsolapp (.NET Framework)**namnge projektet och klicka sedan på **OK**.
   ![Skärmbild som visar hello-fönstret nytt projekt](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)

4. I hello **Solution Explorer**, högerklicka på den nya konsolappen, som är under din Visual Studio-lösning, och klicka sedan på **hantera NuGet-paket...**
    
    ![Skärmbild som visar hello höger Clicked menyn för hello projekt](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. I hello **NuGet** klickar du på **Bläddra**, och skriv **documentdb** i hello sökrutan.
<!---stopped here--->
6. I hello resultat hitta **Microsoft.Azure.DocumentDB** och på **installera**.
   hello paket-ID för hello Azure Cosmos DB-klientbiblioteket är [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Skärmbild som visar hello NuGet-menyn för att hitta Azure Cosmos DB klient-SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Om du får ett meddelande om att granska ändringar toohello lösning, klickar du på **OK**. Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.

## <a id="Connect"></a>Lägga till referenser tooyour projekt
hello återstående steg i den här självstudiekursen ange hello DocumentDB API kod kodavsnitt toocreate och uppdatera Azure Cosmos DB resurser som krävs i projektet.

Lägg först till dessa referenser tooyour program.
<!---These aren't added by default when you install hello pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Anslut appen

Lägg till nedanstående två konstanter och dina *klienten* variabeln i ditt program.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Sedan head tillbaka toohello [Azure-portalen](https://portal.azure.com) tooretrieve din slutpunkts-URL och primärnyckel. hello slutpunkts-URL och primärnyckel behövs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.

I hello Azure portal, navigerar tooyour Azure Cosmos DB konto, klickar på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.

Kopiera hello URI från hello-portalen och via `<your endpoint URL>` i hello program.cs-filen. Kopiera hello PRIMÄRNYCKEL från hello-portalen och klistra in den över `<your primary key>`. Vara säker på att tooremove hello `<` och `>` från värden.

![Skärmbild som visar hello Azure-portalen som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram. Visar ett konto med hello nycklar markerad i hello Azure DB som Cosmos-kontoblad och hello URI och PRIMÄRNYCKEL-värden markerade i bladet nycklar för hello Azure Cosmos DB](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>Skapa en instans av hello DocumentClient

Nu skapa en ny instans av hello **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
```

## <a id="create-database"></a>Skapa en databas

Skapa sedan en Azure-Cosmos-DB [databasen](documentdb-resources.md#databases) med hjälp av hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metod eller [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metod för hello  **DocumentClient** klass från hello [.NET DocumentDB SDK](documentdb-sdk-dotnet.md). En databas är hello logisk behållare för JSON dokumentlagring, partitionerad över samlingarna.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Besluta om en partitionsnyckel 

Samlingar är behållare för att lagra dokument. De logiska resurser och kan [omfattar en eller flera fysiska partitioner](partition-data.md). En [partitionsnyckel](documentdb-partition-data.md) är en egenskap (eller sökväg) i dokument som har använt toodistribute data mellan hello servrar eller partitioner. Alla dokument med samma partitionsnyckel lagras i hello hello samma partition. 

Fastställa en partitionsnyckel är ett viktigt beslut toomake innan du skapar en samling. Partitionsnycklar är en egenskap (sökväg) i dokument som kan vara används av Azure Cosmos DB toodistribute data mellan flera servrar eller partitioner. Cosmos DB hashar hello partitionsnyckelvärde och använder hello hashas resultatet toodetermine hello partition i vilket toostore hello-dokument. Alla dokument med samma partitionsnyckel lagras i hello hello samma partition och partitionsnycklar kan inte ändras när en samling har skapats. 

Den här självstudiekursen kommer vi tooset hello partitionsnyckel för`/deviceId` så som hello alla hello-data för en enskild enhet lagras i en enda partition. Du vill toochoose en partitionsnyckel som har ett stort antal värden, som används vid om hello samma frekvens tooensure Cosmos DB kan belastningsutjämna när dina data växer och uppnå hello totala genomflödet i hello samling. 

Mer information om partitionering finns [hur toopartition och skala i Azure Cosmos DB?](partition-data.md) 

## <a id="CreateColl"></a>Skapa en samling 

Nu när vi vet våra partitionsnyckel `/deviceId`, kan du skapa en [samling](documentdb-resources.md#collections) med hjälp av hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metod eller [ CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metod för hello **DocumentClient** klass. En samling är en behållare för JSON-dokument och alla associerade JavaScript-programlogik. 

> [!WARNING]
> Skapa en samling har prissättning effekter, som du reserverar genomströmning för hello programmet toocommunicate med Azure Cosmos DB. Mer information finns våra [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

```csharp
// Collection for device telemetry. Here hello JSON property deviceId is used  
// as hello partition key toospread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

Den här metoden gör en REST-API anropa tooAzure Cosmos DB och hello tjänsten tillhandahåller ett antal partitioner baserat på hello begärda genomflöde. Du kan ändra hello genomströmningen i en samling som prestandan måste utvecklas med hjälp av hello SDK eller hello [Azure-portalen](set-throughput.md).

## <a id="CreateDoc"></a>Skapa JSON-dokument
Vi infoga vissa JSON-dokument i Azure Cosmos DB. En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metod för hello **DocumentClient** klass. Dokument är användardefinierat (godtyckligt) JSON-innehåll. Detta exempel på klass innehåller en enhet läsning och en anropet tooCreateDocumentAsync tooinsert en ny enhet som läses in en samling.

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here hello partition key is extracted 
// as "XMS-0001" based on hello collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a>Läsa data

Nu ska vi läsa hello dokumentet av sin partitionsnyckel och Id hello ReadDocumentAsync metoden. Observera att hello läsningar innehåller ett värde för PartitionKey (motsvarande toohello `x-ms-documentdb-partitionkey` huvudet i begäran i hello REST-API).

```csharp
// Read document. Needs hello partition key and hello Id toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Uppdatera data

Nu ska vi uppdatera vissa data hello ReplaceDocumentAsync metoden.

```csharp
// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a>Ta bort data

Nu kan du ta bort ett dokument med partitionsnyckel och id med hjälp av hello DeleteDocumentAsync metod.

```csharp
// Delete a document. hello partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Fråga partitionerade samlingar

När du frågar efter data i partitionerade samlingar, Azure Cosmos DB automatiskt hello vägar toohello frågepartitioner motsvarande toohello partitionsnyckelvärden som angavs i hello filter (om det finns några). Den här frågan är till exempel routade toojust hello partition som innehåller hello partitionsnyckel ”XMS 0001”.

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
hello följande fråga har inte ett filter på hello partitionsnyckel (DeviceId) och fanned ut tooall partitioner där den körs mot hello partition index. Observera att du har toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` i hello REST-API) toohave hello SDK tooexecute en fråga över partitioner.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Parallell frågekörning
hello Azure Cosmos DB DocumentDB SDK: er 1.9.0 och ovan stöd parallell körning frågealternativ, som gör att du tooperform låg latens frågor mot partitionerade samlingar, även när de behöver tootouch ett stort antal partitioner. Följande fråga hello är till exempel konfigurerade toorun parallellt över partitioner.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Du kan hantera parallell frågekörning genom att justera hello följande parametrar:

* Genom att ange `MaxDegreeOfParallelism`, du kan styra hello grad av parallellitet d.v.s. hello högsta tillåtna antalet samtidiga nätverket anslutningar toohello mängdens partitioner. Om du väljer för 1, hanteras hello grad av parallellitet av hello SDK. Om hello `MaxDegreeOfParallelism` har inte angetts eller ange too0, vilket är standardvärdet för hello finns ett enda nätverk anslutning toohello mängdens partitioner.
* Genom att ange `MaxBufferedItemCount`, du kan handlar av frågan latens och klientsidan minnesanvändning. Om du utelämnar den här parametern eller Ställ in för 1 hanteras hello antal objekt som buffras under parallell frågekörning av hello SDK.

Angivna hello samma tillstånd mängden hello en parallell fråga returnerar resultat i hello samma ordning som seriella körning. När du utför fråga cross-partition som innehåller sortering (ORDER BY och/eller TOP), hello DocumentDB SDK skickar hello fråga parallellt över partitioner och sammanfogar delvis sorterade resulterar i hello klienten sida tooproduce globalt sorterade resultat.

## <a name="execute-stored-procedures"></a>Köra lagrade procedurer
Slutligen kan du köra atomiska transaktioner mot dokument med hello samma enhets-ID, t.ex. Om du underhålla mängder eller hello senaste tillståndet för en enhet i ett enskilt dokument genom att lägga till hello följande kod tooyour projekt.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

Och det! de är hello huvudkomponenterna i ett Azure DB som Cosmos-program som använder en partition viktiga tooefficiently skala Datadistribution över partitioner.  

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av den här kursen i hello Azure-portalen med hello följande steg:

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello unika namn hello resurs du skapat. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande: 

> [!div class="checklist"]
> * Skapa ett Azure DB som Cosmos-konto
> * Skapa en databas och samling med en partitionsnyckel
> * Skapade JSON-dokument
> * Uppdatera ett dokument
> * Efterfrågade partitionerade samlingar
> * Körde en lagrad procedur
> * Ta bort ett dokument
> * Ta bort en databas

Du kan nu fortsätta toohello nästa kurs och importera ytterligare data tooyour Cosmos-DB-konto. 

> [!div class="nextstepaction"]
> [Importera data till Azure Cosmos DB](import-data.md)
