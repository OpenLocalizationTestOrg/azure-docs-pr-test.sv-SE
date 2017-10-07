---
title: aaaExpire data i Azure Cosmos DB med tiden toolive | Microsoft Docs
description: "Med TTL tillhandahåller Microsoft Azure Cosmos DB hello möjlighet toohave dokument automatiskt rensas bort från hello systemet efter en viss tidsperiod."
services: cosmos-db
documentationcenter: 
keywords: tid toolive
author: arramac
manager: jhubbard
editor: 
ms.assetid: 25fcbbda-71f7-414a-bf57-d8671358ca3f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: 51d8ec46add72c9624457316a4ccd1e23fb83ad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-toolive"></a>Data i Azure Cosmos DB samlingar automatiskt med tiden toolive att gälla
Program kan skapa och lagra stora mängder data. Vissa av dessa data, t.ex. datorn genereras data, loggar och användaren händelsesessionen information är bara användbara för en bestämd tidsperiod. När hello data blir överflödiga toohello behov av programmet hello är säker toopurge dessa data och minska hello lagringsbehov för ett program.

Med ”time toolive” eller TTL tillhandahåller Microsoft Azure Cosmos DB hello möjlighet toohave dokument automatiskt bort från hello databasen efter en viss tidsperiod. hello standardtid toolive kan ange på samlingsnivå hello och åsidosätts på grundval av per dokument. När TTL-värde har angetts som standard samlingen eller på en dokumentnivå bort Cosmos DB automatiskt dokument som finns efter den tid i sekunder, eftersom de senast ändrades.

Tid toolive i Cosmos-databasen använder en förskjutning mot när hello dokumentet senast ändrades. toodo den här den använder hello `_ts` fält som finns på alla dokument. Hej _ts fältet är en unix-format epok tidsstämpel som representerar hello datum och tid. Hej `_ts` varje gång ett dokument ändras uppdateras fältet. 

## <a name="ttl-behavior"></a>TTL-beteende
hello TTL-funktionen styrs av TTL-egenskaper på två nivåer - hello samlingsnivå och hello för dokumentet. hello värden anges i sekunder och behandlas som en lista från hello `_ts` hello dokumentet har ändrades senast.

1. DefaultTTL för hello samling
   
   * Om saknas (eller uppsättning toonull) dokument tas inte bort automatiskt.
   * Om den finns och är-hello värdet ”1” = oändlig – dokument inte ut som standard
   * Om den finns och hello värde är ett nummer (”n”) – dokument gälla ”n” sekunder efter senast ändrades
2. TTL-värde för hello dokument: 
   
   * Egenskapen gäller endast om DefaultTTL finns för hello överordnade samlingen.
   * Åsidosätter hello DefaultTTL för hello överordnade samlingen.

Så snart hello dokumentet har upphört att gälla (`ttl`  +  `_ts` > = aktuella servertiden), hello dokumentet har markerats som ”upphört att gälla”. Ingen åtgärd ska tillåtas på dessa dokument efter den tidpunkten och de kommer att uteslutas från hello resultatet av alla frågor som utförs. hello dokument fysiskt tas bort i hello system och tas bort i bakgrunden hello tillfälligt vid ett senare tillfälle. Detta inte tar upp någon [begära enheter (RUs)](request-units.md) från hello samling budget.

hello ovan logik kan visas i följande matris hello:

|  | DefaultTTL saknas ej på hello samling | DefaultTTL = -1 på en samling | DefaultTTL = ”n” på en samling |
| --- |:--- |:--- |:--- |
| TTL-värde saknas i dokumentet |Inget toooverride på dokumentnivå eftersom både hello dokumentet och samling har något begrepp om TTL-värde. |Inga dokument i den här samlingen upphör att gälla. |hello dokument i den här samlingen upphör att gälla efter intervall n tiden. |
| TTL = -1 i dokumentet |Inget toooverride nivån hello dokument eftersom hello samlingen inte definierar hello DefaultTTL egenskap som ett dokument kan åsidosätta. TTL-värde i ett dokument är icke tolkad hello-systemet. |Inga dokument i den här samlingen upphör att gälla. |upphör aldrig att gälla hello dokument med TTL =-1 i den här samlingen. Alla dokument upphör att gälla efter ”n” intervall. |
| TTL = n i dokumentet |Inget toooverride på hello dokumentnivå. TTL-värde på ett dokument i icke tolkad hello-systemet. |hello dokument med TTL = n upphör att gälla efter intervall n, i sekunder. Andra dokument kommer att ärva intervallet-1 och aldrig går ut. |hello dokument med TTL = n upphör att gälla efter intervall n, i sekunder. Andra dokument ärver ”n” intervall från hello samling. |

## <a name="configuring-ttl"></a>Konfigurera TTL-värde
Tid toolive är inaktiverad som standard i alla Cosmos DB samlingar och på alla dokument som standard.

## <a name="enabling-ttl"></a>Aktivera TTL-värde
tooenable TTL-värde på en samling eller hello dokument i en samling behöver du tooset hello DefaultTTL egenskap för en samling tooeither -1 eller ett positivt tal som inte är noll. Ange hello DefaultTTL för-1 innebär att som standard alla dokument i hello samlingen kommer live alltid men hello Cosmos-DB-tjänsten bör du övervaka den här samlingen för dokument som har åsidosatt den här standardinställningen.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>Konfigurera standard TTL-värde på en samling
Du kan kan tooconfigure en toolive standardtid på en samlingsnivå. tooset hello TTL-värde på en samling måste tooprovide ett positivt tal som är noll som visar hello period i sekunder, tooexpire alla dokument i hello samlingen när hello senast ändrad tidsstämpeln för hello dokumentet (`_ts`). Alternativt kan du ange hello standard för-1, vilket innebär att alla dokument som infogas i toohello samlingen kommer live på obestämd tid som standard.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>Inställningen TTL-värde på ett dokument
Dessutom toosetting standard TTL-värde för en samling du kan ange specifika TTL-värde på dokumentnivå. Detta åsidosätter hello standard hello mängden.

* tooset hello TTL-värde på ett dokument, behöver du tooprovide ett positivt tal som är noll vilket betyder hello period i sekunder, tooexpire hello dokument när hello senast ändrad tidsstämpeln för hello dokumentet (`_ts`).
* Om ett dokument har inget TTL-fält hello standardvärdet hello samling gäller.
* Om TTL-värdet är inaktiverat på samlingsnivå hello ignoreras hello TTL-fältet för hello dokument tills TTL aktiveras igen på hello samling.

Här är ett kodfragment som visar hur tooset hello TTL upphör att gälla på ett dokument:

    // Include a property that serializes too"ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used tooset expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set hello value toohello expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a>Utöka TTL-värde på ett befintligt dokument
Du kan återställa hello TTL-värde i ett dokument genom att göra någon skrivåtgärd i hello dokumentet. Detta kommer att ange hello `_ts` toohello aktuell tid och hello nedräkning toohello dokumentet upphör att gälla, som angetts av hello `ttl`, påbörjas igen. Om du inte vill toochange hello `ttl` i ett dokument, kan du uppdatera hello fält som du kan göra med något annat går fält.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time toolive
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a>Ta bort TTL-värde från ett dokument
Om ett TTL-värde har angetts för ett dokument och du inte längre vill att dokumentet tooexpire, sedan du kan hämta hello dokument, ta bort hello TTL-fältet och ersätta hello dokument på hello-servern. När hello TTL-fältet tas bort från hello dokument kommer hello standardvärdet hello samlingen att tillämpas. toostop ett dokument upphör att gälla och inte ärver från hello samling måste tooset hello TTL-värdet för-1.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit hello default TTL of hello collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a>Inaktivera TTL-värde
toodisable TTL helt på en samling och stoppa hello bakgrund bearbeta från söker efter utgångna dokument hello DefaultTTL egenskapen på hello samling ska tas bort. Ta bort den här egenskapen skiljer sig från att ange värdet 1 för. Inställningen för-1 innebär nya dokument lägga toohello samlingen kommer alltid live men du kan åsidosätta detta på specifika dokument i hello samling. Ta bort den här egenskapen helt från hello samling innebär att inga dokument upphör att gälla, även om det finns dokument som tidigare har uttryckligen åsidosätts.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
**Vad TTL kostar mig?**

Det finns inga extra kostnad toosetting ett TTL-värde i ett dokument.

**Hur lång tid tar det toodelete dokumentet när hello TTL-värde som är igång?**

hello dokument har upphört att gälla omedelbart när hello TTL-värde som är igång och kan inte nås via CRUD eller fråga API: er. 

**TTL-värde i ett dokument har någon effekt på RU avgifter?**

Nej, det är ingen inverkan på RU kostnader för borttagningar av utgångna dokument via TTL-värde i Cosmos-databasen.

**Hello TTL-funktionen endast gäller tooentire dokument eller kan jag upphöra att gälla egenskapsvärden för enskilda dokument?**

TTL-värde gäller toohello hela dokumentet. Om du vill att tooexpire rekommenderas bara en del av ett dokument, då du extrahera hello del från hello dokument i tooa separat ”länkade” dokument och sedan använda TTL-värde med det extraherade dokumentet.

**Har hello TTL-funktionen eventuella särskilda krav för fulltextindexering?**

Ja. hello samlingen måste innehålla [indexering principen](indexing-policies.md) tooeither konsekvent eller Lazy. Försök tooset DefaultTTL på en samling med indexering set tooNone resulterar i ett fel som kommer försök tooturn av indexering på en samling som har en DefaultTTL som redan angetts.

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Cosmos DB finns toohello service [ *dokumentationen* ](https://azure.microsoft.com/documentation/services/cosmos-db/) sidan.

