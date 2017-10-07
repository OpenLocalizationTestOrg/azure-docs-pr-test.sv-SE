---
title: aaaPartitioning och teckenbredden i Azure Cosmos DB | Microsoft Docs
description: "Lär dig mer om hur partitionering fungerar i Azure Cosmos DB hur tooconfigure partitionering och partitionsnycklar och hur toopick hello höger partitions-nyckeln för ditt program."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 87d56db8c4ccc6b94b1650baff0fcfb3db6d1777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopartition-and-scale-in-azure-cosmos-db"></a>Hur toopartition och skala i Azure Cosmos DB

[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) är en global distribuerade och flera olika modeller databasen syftar toohelp du uppnå snabb och förutsägbar prestanda och skalning sömlöst tillsammans med ditt program som de växer. Den här artikeln innehåller en översikt över hur partitionering fungerar för alla data som hello modeller i Azure Cosmos DB och beskriver hur du kan konfigurera Azure Cosmos DB behållare tooeffectively skala ditt program.

Partitionering och partitionsnycklar också beskrivs i den här Azure fredag video med Scott Hanselman och Azure Cosmos DB Principal tekniker Manager, Shireesh Thota.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Partitionering i Azure Cosmos DB
Du kan lagra och fråga schemat mindre data med ordning millisekunder svarstider skaländras i Azure Cosmos-databasen. Cosmos DB innehåller behållare för lagring av data kallas **samlingar (för dokument), diagram och tabeller**. Behållare är logiska nätverksresurser och kan sträcka sig över en eller flera partitioner fysiska servrar. hello antalet partitioner bestäms av Cosmos DB baserat på hello lagringsstorlek och hello etablerat dataflöde för hello behållare. Varje partition i Cosmos-databasen har en fast mängd SSD-baserad lagring som är kopplad till den och replikeras för hög tillgänglighet. Partitionen management hanteras helt av Azure Cosmos DB och du inte har toowrite komplex kod eller hantera din partitioner. Cosmos DB behållare är obegränsad lagring och genomflöde. 

![vågrät](./media/introduction/azure-cosmos-db-partitioning.png) 

Partitionering är transparent tooyour program. Cosmos DB stöder snabb läsning och skrivning, frågor, transaktionella logik, konsekvensnivåer och detaljerad åtkomstkontroll via metoder/API: er tooa enskild behållare resurs. hello tjänsten hanterar distribuera data över partitioner och routning frågan begäranden toohello rätt partition. 

Hur fungerar partitionering? Varje objekt måste ha en partitionsnyckel och en rad, som kan identifieras. Din partitionsnyckel fungerar som en logisk partition för dina data och ger Cosmos DB en naturlig gräns för att distribuera data över partitioner. I korthet är här hur partitionering fungerar i Azure Cosmos DB:

* Du etablerar en Cosmos-DB-behållare med `T` begäranden/s genomströmning
* Hello bakgrunden Cosmos DB tillhandahåller partitioner behövs tooserve `T` begäranden/s. Om `T` är högre än hello maximalt dataflöde per partition `t`, sedan Cosmos DB tillhandahåller `N`  =  `T/t` partitioner
* Cosmos DB allokerar hello nyckel utrymme på partitionen viktiga hashvärden jämnt över hello `N` partitioner. Därför 1 N partition nyckelvärdena (logiska partitioner) är värd för varje partition (fysiska partition)
* När en fysisk partition `p` når sin lagringsgräns, Cosmos DB sömlöst delar `p` till två nya partitioner `p1` och `p2` och distribuerar värden som motsvarar tooroughly halva hello nycklar tooeach av hello partitioner. Den här delade åtgärden är osynliga tooyour program.
* På liknande sätt, när du etablerar genomströmning som är högre än `t*N` dataflöde, Cosmos DB delar upp en eller flera av dina partitioner toosupport hello högre genomströmning

hello semantik för partitionsnycklar är något annorlunda toomatch hello semantiken för varje API, som visas i följande tabell hello:

| API | Partitionsnyckeln | Radnyckel |
| --- | --- | --- |
| DocumentDB | Anpassad partitionering nyckelsökvägen | fast`id` | 
| MongoDB | anpassade Fragmentera nyckel  | fast`_id` | 
| Graph | Anpassad partitionering nyckelegenskapen | fast`id` | 
| Tabell | fast`PartitionKey` | fast`RowKey` | 

Cosmos DB använder hash-baserad partitionering. När du skriver ett objekt Cosmos DB hashar hello partitionsnyckelvärde och Använd hello hashas resultatet toodetermine vilket partition toostore hello objekt i. Cosmos DB lagrar alla artiklar med hello samma partitionsnyckel i hello samma fysiska partition. hello valet av hello partitionsnyckel är ett viktigt beslut som du har toomake i designläge. Du måste välja ett egenskapsnamn som har ett stort antal värden och har även åtkomstmönster.

> [!NOTE]
> Det är en bästa praxis toohave en partitionsnyckel med många distinkta värden (100-tal-1 000-tal minst).
>

Azure DB Cosmos-behållare kan skapas som ”fast” eller ”unlimited”. Fast storlek behållare har en maxgräns på 10 GB och 10 000 RU/s genomströmning. Vissa API: er kan hello partition viktiga toobe utelämnas för behållare med fast storlek. Du måste ange en lägsta dataflöde på 2500 RU/s toocreate en behållare som obegränsade.

## <a name="partitioning-and-provisioned-throughput"></a>Partitionering och etablerat dataflöde
Cosmos DB är utformad för förutsägbar prestanda. När du skapar en behållare kan du reservera genomflöde i  **[programbegäran](request-units.md) (RU) per sekund med en potentiell tillägget för RU per minut**. Varje begäran har tilldelats en avgift för begäran enhet som är proportionell toohello mängden systemresurser som CPU, minne och i/o som används av hello åtgärden. En läsning av ett 1-KB-dokument med sessionskonsekvens förbrukar en begäran enhet. Läs är 1 RU oavsett hello antal objekt som lagras eller hello antalet samtidiga begäranden som körs vid hello samtidigt. Större objekt kräver högre frågeenheter beroende på hello storlek. Om du känner hello storleken på dina enheter och hello antalet läsningar du toosupport för programmet, du kan etablera hello exakta mängden dataflödet som krävs för ditt program är behöver läsa. 

> [!NOTE]
> tooachieve hello totala genomflödet i hello behållare, måste du välja en partitionsnyckel som gör att du tooevenly distribuera begäranden mellan vissa nyckelvärden för olika partition.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-hello-azure-cosmos-db-apis"></a>Arbeta med hello Azure Cosmos DB API: er
Du kan använda hello Azure-portalen eller Azure CLI toocreate behållare och skala dem när som helst. Det här avsnittet visas hur toocreate behållare och ange hello genomflöde och partition definitionen i varje hello stöds API: er.

### <a name="documentdb-api"></a>DocumentDB-API
hello som följande exempel visar hur en behållare (insamling) med hjälp av toocreate hello DocumentDB-API. Du hittar mer information finns i [partitionering med DocumentDB API](partition-data.md).

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Du kan läsa en artikel (dokument) med hello `GET` metod i hello REST API eller med hjälp av `ReadDocumentAsync` i ett hello SDK: er.

```csharp
// Read document. Needs hello partition key and hello ID toobe specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a>MongoDB-API
Du kan skapa ett delat samling via din favorit-verktyget, drivrutiner och SDK med hello MongoDB-API. I det här exemplet använder vi hello Mongo-gränssnittet för att skapa en samling hello.

I hello Mongo Shell:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Resultat:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Tabell-API

Med hello tabell API ange hello genomströmning för tabeller i hello appSettings-konfigurationen för programmet:

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

Sedan kan du skapa en tabell med hello Azure Table storage SDK. hello partitionsnyckel skapas implicit som hello `PartitionKey` värde. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

Du kan hämta en enda entitet med hjälp av följande fragment hello:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Se [utveckling med hello tabell API](tutorial-develop-table-dotnet.md) för mer information.

### <a name="graph-api"></a>Graph API

Du måste använda hello Azure-portalen eller CLI toocreate behållare med hello Graph API. Eftersom Azure Cosmos DB flera modell kan du alternativt med någon av hello andra modeller toocreate och skala graph-behållaren.

Du kan läsa alla hörn eller en kant med hello partitionsnyckel och ID: t i Gremlin. Till exempel för ett diagram med region (”USA”) som hello partitionsnyckel och ”Seattle” som hello radnyckel hittar du en nod med hjälp av hello följande syntax:

```
g.V(['USA', 'Seattle'])
```

På samma sätt som med kanter, kan du referera en kant med hello partitionsnyckel och radnyckel.

```
g.E(['USA', 'I5'])
```

Se [Gremlin stöd för Cosmos-DB](gremlin-support.md) för mer information.


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a>Utformning för partitionering
tooscale effektivt med Azure Cosmos DB måste toopick en bra partitionsnyckel när du skapar en behållare. Det finns två viktiga överväganden för att välja en partitionsnyckel:

* **Gräns för frågan och transaktioner**: valet av partitionsnyckel ska utjämna hello måste tooenable hello användning av transaktioner mot hello krav toodistribute entiteterna mot flera partition nycklar tooensure en skalbar lösning. Du kan ange hello på en extreme samma partitionsnyckel för alla objekt, men detta kan begränsa hello skalbarhet i lösningen. Vid hello andra extreme kan du tilldela en unik partitionsnyckel för varje objekt som skulle vara mycket skalbar, men som skulle kunna hindra att du använder transaktioner mellan dokument via lagrade procedurer och utlösare. En perfekt Partitionsnyckeln är något som gör att du toouse effektiva frågor och som har tillräcklig kardinalitet tooensure din lösning är skalbart. 
* **Inga lagrings- och prestandakrav flaskhalsar**: det är viktigt toopick en egenskap som gör att skriver toobe fördelad över olika distinkta värden. Begäranden toohello samma partitionsnyckel får inte överskrida hello genomströmningen i en enda partition och har begränsats. Det är därför viktigt toopick en partitionsnyckel inte resulterar i ”aktiva punkter” i ditt program. Eftersom alla hello-data för en enda partitionsnyckel måste lagras i en partition bör också tooavoid partitionsnycklar som har stora mängder data för hello samma värde. 

Nu ska vi titta på några verkliga scenarier och bra partitionsnycklar för varje:
* Om du implementerar en serverdel för användarens profil, är ett bra alternativ för partitionsnyckel med hello användar-ID.
* Om du lagrar IoT data till exempel enhetens tillstånd, är enhets-ID ett bra alternativ för partitionsnyckel.
* Om du använder Azure Cosmos DB för loggning av time series-data, hello värdnamn eller process-ID är ett bra alternativ för partitionsnyckel.
* Om du har en arkitektur med flera innehavare är hello klient-ID ett bra alternativ för partitionsnyckel.

I vissa användningsfall som IoT och användarprofiler, hello partitionsnyckel kan vara hello samma som ditt id (Dokumentnyckel). Du kan ha en partitionsnyckel än hello-id i andra som hello tid series-data.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionering och loggning-tidsserier data
En av hello vanliga användningsområden för Cosmos DB är för loggning och telemetri. Det är viktigt toopick en bra partitionsnyckel eftersom du kan behöva tooread/skrivning stora mängder data. hello beror på din läsning och skrivning priser och typer av frågor som du förväntar dig toorun. Här följer några tips på hur toochoose en bra partitionsnyckel.

* Om din användningsfall omfattar en liten andel skriver kan sparas under en längre tid och måste tooquery av intervall med tidsstämplar och filter, och sedan använda en sammanslagning av hello timestamp, till exempel datum som en partitionsnyckel är en bra metod. Detta ger dig tooquery över alla hello-data för ett datum från en enda partition. 
* Om din arbetsbelastning skrivs frekventa som är vanligare, bör du använda en partitionsnyckel som inte baseras på tidsstämpel så att Cosmos DB kan distribuera skrivningar jämnt mellan olika partitioner. Här är ett värdnamn, process-ID, aktivitets-ID eller en annan egenskap med hög kardinalitet ett bra alternativ. 
* Ett tredje sätt är en hybrid en om du har flera behållare, ett för varje dag/månad och hello Partitionsnyckeln är en detaljerad egenskap som värdnamn. Hello förmånen som du kan ange olika genomströmning baserat på hello tidsfönstret har, till exempel hello behållare för hello aktuella månaden har etablerats med högre genomströmning eftersom det används läsningar och skrivningar, medan tidigare månader med lägre genomströmning eftersom de fungerar endast läsningar.

### <a name="partitioning-and-multi-tenancy"></a>Partitionering och flera innehavare
Om du implementerar ett program för flera innehavare med Cosmos DB, finns det två populära mönster – en partitionsnyckel per klient och en behållare per klient. Här följer hello- och nackdelar för varje:

* En partitionsnyckel per klient: I den här modellen klienter som är samordnad inom en enskild behållare. Men frågor och infogningar för objekt inom en enskild klient kan utföras mot en enskild partition. Du kan också implementera transaktionella logik i alla artiklar i en klient. Eftersom flera innehavare delar en behållare kan spara du kostnader för lagring och genomströmning genom poolning resurser för klienter i en enskild behållare i stället för att etablera extra utrymme för varje klient. hello Nackdelen är att du inte har isolering av prestandan per klient. Ökar prestanda/dataflöde gäller toohello hela behållaren vs riktade ökar för klienter.
* En behållare per klient: varje innehavare har sin egen behållare. Du kan reservera prestanda per klient i den här modellen. Den här modellen är mer kostnadseffektivt för program med flera klienter med några innehavare med Cosmos DB ny allokering prismodell.

Du kan också använda en kombination/nivåer metod som collocates små klienter och migrerar större hyresgäster tootheir egen behållare.

## <a name="next-steps"></a>Nästa steg
Vi tillhandahöll en översikt för en översikt över begrepp och bästa praxis för partitionering med Azure Cosmos DB API: er i den här artikeln. 

* Lär dig mer om [etablerat dataflöde i Azure Cosmos DB](request-units.md)
* Lär dig mer om [global distributionsplatsen i Azure Cosmos DB](distribute-data-globally.md)



