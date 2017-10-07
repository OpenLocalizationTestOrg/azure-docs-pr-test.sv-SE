---
title: "aaaRequest enheter & uppskattning genomströmning - Azure Cosmos DB | Microsoft Docs"
description: "Läs mer om hur toounderstand, ange och beräkna begäran enhet krav i Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 13c4e7aeb6222fa14ef982e238716e15a0159fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-in-azure-cosmos-db"></a>Enheter för programbegäran i Azure Cosmos DB
Nu tillgängligt: Azure Cosmos-DB [begäran enhet Kalkylatorn](https://www.documentdb.com/capacityplanner). Läs mer i [uppskatta dina genomströmning måste](request-units.md#estimating-throughput-needs).

![Genomströmning Kalkylatorn][5]

## <a name="introduction"></a>Introduktion
[Azure Cosmos-DB](https://azure.microsoft.com/services/cosmos-db/) är Microsofts globalt distribuerad flera modellen databas. Du inte har toorent virtuella datorer, distribuera programvara eller övervaka databaser med Azure Cosmos DB. Azure Cosmos-DB drivs och övervakas kontinuerligt av Microsoft översta engineers toodeliver world klassen tillgänglighet, prestanda och data protection. Du kan komma åt dina data med hjälp av API: er för ditt val som [DocumentDB SQL](documentdb-sql-query.md) (dokument), MongoDB (dokument), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (nyckelvärde) och [Gremlin](https://tinkerpop.apache.org/gremlin.html) (kurva) alla Inbyggt stöd för. hello valuta Azure Cosmos DB är hello begära enhet (RU). Med RUs behöver du inte tooreserve läsning och skrivning kapacitet eller etablera CPU, minne och IOPS.

Azure Cosmos-DB stöder ett antal API: er med olika åtgärder, från enkla läser och skriver toocomplex graph-frågor. Eftersom inte alla begäranden som är lika med de tilldelas en normaliserade mängd **programbegäran** baserat på hello mängden beräkning krävs tooserve hello begäran. Du kan spåra hello antalet frågeenheter som används av alla åtgärder i Azure Cosmos DB via en svarshuvud hello antal begäran för en åtgärd är deterministiska. 

tooprovide förutsägbar prestanda, behöver du tooreserve dataflöde i enheter av 100 RU per sekund. 

När du har läst den här artikeln att du kunna tooanswer hello följande frågor:  

* Vad är programbegäran och begära avgifter?
* Hur jag för att ange begäran enhet kapacitet för en samling?
* Hur jag beräkna måste mitt program begäran enhet?
* Vad händer om jag överskrider begäran enhet kapacitet för en samling?

Azure Cosmos DB är en databas med flera olika modeller, är det viktigt toonote att tooa samling dokument kallas för ett dokument API, diagram/nod för graph API och tabellen/entiteten för tabell-API. Genomströmning dokumentet kommer vi generalisera toohello begreppet behållare/artikel.

## <a name="request-units-and-request-charges"></a>Frågeenheter och kostnader för begäran
Azure Cosmos-DB ger snabb och förutsägbar prestanda av *reservera* resurser toosatisfy genomströmning för ditt program behöver.  Eftersom program läsa in och komma åt mönster ändras över tiden, Azure Cosmos DB kan du tooeasily öka eller minska hello reserverat dataflöde tillgängliga tooyour program.

Med Azure Cosmos DB anges reserverat dataflöde som frågeenheter som bearbetades per sekund. Du kan se frågeenheter som dataflöde valuta, där du *reservera* en mängd garanteras frågeenheter tillgängliga tooyour program på grundval av per sekund.  Varje åtgärd i Azure DB som Cosmos - skriva ett dokument, utför en fråga, uppdatera ett dokument - förbrukar CPU, minne och IOPS.  Det vill säga varje åtgärd har en *begära kostnad*, som uttrycks i *programbegäran*.  Förstå hello faktorer som påverkar begäran enhet kostnader, tillsammans med ditt program genomströmning krav, kan du toorun tillämpningsprogrammet som kostnad effektiv som möjligt. Hej frågeutforskaren är också en fantastiska verktyget tootest hello kärnan i en fråga.

Vi rekommenderar att komma igång genom att titta på hello följande video, där Aravind Ramachandran förklarar frågeenheter och förutsägbar prestanda med Azure Cosmos DB.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a>Ange kapacitet för begäran-enhet i Azure Cosmos DB
När du startar en ny samling, tabell eller diagram, kan du ange hello många frågeenheter per sekund (RU per sekund) som du vill reserverade. Baserat på etablerat dataflöde för hello Azure Cosmos DB allokerar fysiska partitioner toohost samlingen och delningar/rebalances data över partitioner när det växer.

Azure Cosmos-DB kräver en nyckel toobe partition anges när en samling är etablerad med 2 500 frågeenheter eller högre. En partitionsnyckel är också nödvändig tooscale din samling genomflöde utöver 2 500 frågeenheter i hello framtida. Därför rekommenderas tooconfigure en [partitionsnyckel](partition-data.md) när du skapar en behållare oavsett din första genomflöde. Eftersom dina data kan ha toobe dela upp på flera partitioner, är det nödvändigt toopick en partitionsnyckel som har en hög kardinalitet (100 toomillions distinkta värden) så att dina graph-samling/tabell och begäranden kan skalas enhetligt med Azure Cosmos DB. 

> [!NOTE]
> En partitionsnyckel är en logisk gräns och inte en fysisk. Därför behöver inte toolimit hello antalet distinkta partitionsnyckelvärden. Det är i själva verket bättre toohave tydligare partitions nyckelvärden än mindre, eftersom Azure Cosmos DB har flera alternativ för belastningsutjämning.

Här är ett kodfragment för att skapa en samling med 3 000 begäran enheter per andra med hello .NET SDK:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Azure Cosmos-DB fungerar på en modell för reservation på genomflöde. Det vill säga du debiteras för hello mängden genomströmning *reserverade*, oavsett hur mycket av den genomströmningen är aktivt *används*. Hej mängden reserverad RUs via SDK: er som ditt program load, data och användning mönster du kan enkelt skala upp eller ned eller med hjälp av hello [Azure Portal](https://portal.azure.com).

Varje samling/tabellen/diagram mappas tooan `Offer` resurs i Azure Cosmos DB som innehåller metadata om hello etablerat dataflöde. Du kan ändra hello allokerade genomströmning genom att slå upp hello motsvarande erbjudande resurs för en behållare och sedan uppdaterar med hello nya genomströmning värdet. Här är ett kodfragment för att ändra hello genomströmningen i en samling too5, 000 frågeenheter per andra med hello .NET SDK:

```csharp
// Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set hello throughput too5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

Det finns ingen inverkan toohello tillgänglighet för din behållare när du ändrar hello genomflöde. Hello nya reserverat dataflöde är vanligtvis effektiva i sekunder för programmet hello nya genomströmning.

## <a name="request-unit-considerations"></a>Överväganden för begäran-enhet
När du uppskattar hello antalet begäran enheter tooreserve för Azure DB som Cosmos-behållaren är viktiga tootake hello följande variabler i beräkningen:

* **Objektet storlek**. Då ökar storleken hello enheter används tooread eller skriva hello data ökar också.
* **Objektet antal egenskaper**. Under förutsättning att standard indexering av alla egenskaper, hello enheter används toowrite dokument/nod/ntity ökar när hello egenskapen Antal ökar.
* **Datakonsekvens**. När du använder data konsekvensnivåer av starka eller begränsas föråldrad, kommer ytterligare enheter att förbrukade tooread objekt.
* **Indexerade egenskaper**. En princip för index varje behållare avgör vilka egenskaper som indexeras som standard. Du kan minska konsumtion din begäran enheten genom att begränsa hello antal indexerade egenskaper eller genom att aktivera lazy indexering.
* **Dokumentindexering**. Som standard varje objekt indexeras automatiskt kommer du att använda färre frågeenheter om du väljer inte tooindex några av dina objekt.
* **Fråga mönster**. hello komplexiteten i en fråga påverkar hur många enheter som begär förbrukas för en åtgärd. hello antalet predikat, uppbyggnad hello predikat, projektioner, antalet UDF: er och hello storleken på hello källa datauppsättning alla påverka hello kostnaden för fråga åtgärder.
* **Script användning**.  Precis som med frågor, lagrade procedurer och utlösare kan du använda frågeenheter utifrån hello komplext hello åtgärder som utförs. När du utvecklar programmet inspektera hello begäran kostnad huvud toobetter förstå hur varje åtgärd förbrukar begäran enhet kapacitet.

## <a name="estimating-throughput-needs"></a>Uppskattning behov för genomströmning
En begäran-enhet är ett normaliserat mått på kostnaden för begäranden. En enskild begäran enhet representerar hello bearbetning kapacitet som krävs tooread (via self länk eller id) en enda 1KB artikel bestående av 10 unika värden (exklusive Systemegenskaper). En begäran toocreate (insert), ersätta eller ta bort hello samma objekt förbrukar mer bearbetning från hello-tjänsten och därmed mer programbegäran.   

> [!NOTE]
> hello baslinje för begäran om 1 enhet för en 1KB objekt motsvarar tooa enkelt få self länk eller hello objekt-id.
> 
> 

Här är till exempel en tabell som visar hur många begära tooprovision för enheter på tre olika objekt-storlekar (1KB 4KB och 64KB) och två olika prestandanivåer (500 läsningar per sekund + 100 skrivningar per sekund och 500 läsningar per sekund + 500 skrivningar per sekund). hello datakonsekvens konfigurerades på sessionen och hello indexering princip har angetts tooNone.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Objektets storlek</strong></p></td>
            <td valign="top"><p><strong>Läsningar per sekund</strong></p></td>
            <td valign="top"><p><strong>Skrivningar per sekund</strong></p></td>
            <td valign="top"><p><strong>Enheter för programbegäran</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1 000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1) + (500 * 5) = 3 000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1.3) + (100 * 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1.3) + (500 * 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 kB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 10) + (100 * 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 kB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 10) + (500 * 48) = 29,000 RU/s</p></td>
        </tr>
    </tbody>
</table>

### <a name="use-hello-request-unit-calculator"></a>Använda hello begäran enhet Kalkylatorn
toohelp kunder bra justera sina genomströmning uppskattningar, finns det en webbaserad [begäran enhet Kalkylatorn](https://www.documentdb.com/capacityplanner) toohelp uppskattning hello begäran enhet krav för vanliga åtgärder, inklusive:

* Objektet skapar (skriver)
* Läsningar av objektet
* Tar bort objekt
* Objektet uppdateringar

hello verktyget omfattar också stöd för att uppskatta lagringsbehov baserat på hello exempelobjekt som du anger.

Verktyget hello är enkel:

1. Ladda upp en eller flera representativa objekt.
   
    ![Överför objekt toohello begäran enhet Kalkylatorn][2]
2. krav för datalagring tooestimate, ange hello Totalt antal objekt du förväntar dig toostore.
3. Ange hello antal objekt skapa, läsa, uppdatera och delete-åtgärder som du behöver (på grundval av per sekund). tooestimate hello begäran enhet avgifter för objektet uppdateringsåtgärder överför en kopia av hello exempel objekt från steg 1 ovan som innehåller vanliga fältuppdateringar.  Till exempel om objektet uppdateringar ändra vanligtvis två egenskaper med namnet lastLogin och userVisits och sedan enkelt kopiera hello exempel objekt, uppdatera hello värden för dessa två egenskaper och överför hello kopieras objektet.
   
    ![Ange kraven för genomflöde i hello begäran enhet Kalkylatorn][3]
4. Klicka på Beräkna och undersöka hello resultat.
   
    ![Begära enhet Kalkylatorn resultat][4]

> [!NOTE]
> Om du har objekttyper som varierar kraftigt vad gäller storlek och hello antal indexerade egenskaper, sedan överföra ett exempel på varje *typen* för vanliga objekt toohello verktyget och sedan beräkna hello resultat.
> 
> 

### <a name="use-hello-azure-cosmos-db-request-charge-response-header"></a>Använd hello Azure Cosmos DB begäran kostnad svarshuvud
Varje svar från hello Azure Cosmos DB tjänsten omfattar en anpassad rubrik (`x-ms-request-charge`) som innehåller hello frågeenheter som används för hello-begäran. Det här sidhuvudet är också tillgängligt via hello Azure Cosmos DB SDK: er. I hello .NET SDK är RequestCharge en egenskap hos hello ResourceResponse objekt.  För frågor visar hello Azure Cosmos DB Frågeutforskaren i hello Azure-portalen begäran kostnad information om utförs frågor.

![Undersöka RU tillägg i hello Frågeutforskaren][1]

Med detta i åtanke, en metod för att uppskatta reserverat dataflöde som krävs av programmet hello mycket kostar toorecord hello begäran enhet som är associerade med vanliga åtgärder som körs mot ett representativt objekt som används av ditt program och sedan uppskatta hello antal åtgärder vill du utföra varje sekund.  Vara säker på att toomeasure och innehåller vanliga frågor och Azure Cosmos DB skript samt användning.

> [!NOTE]
> Om du har objekttyper som varierar kraftigt vad gäller storlek och hello antal indexerade egenskaper kan sedan registrera hello tillämpliga åtgärden begäran enhetsavgift som associeras med varje *typen* för vanliga objekt.
> 
> 

Exempel:

1. Registrera hello begäran enhet kostnad för att skapa (Infoga) en typisk objektet. 
2. Registrera hello begäran enhet kostnad läsa vanliga objekt.
3. Registrera hello begäran enhet kostnad för att uppdatera en typisk objektet.
4. Registrera hello begäran enhetsavgift för vanliga, vanliga artikel frågor.
5. Registrera hello begäran enhet kostnad för alla anpassade skript (lagrade procedurer, utlösare, användardefinierade funktioner) kan användas av programmet hello
6. Beräkna hello krävs begäran enheter som anges hello Uppskattat antal åtgärder du förutse toorun varje sekund.

### <a id="GetLastRequestStatistics"></a>Använd API för Mongodb's GetLastRequestStatistics kommando
API: et för MongoDB stöder ett anpassat kommando *getLastRequestStatistics*, för att hämta hello begäran kostnad för angivna åtgärder.

Till exempel köra hello åtgärden som du vill tooverify hello begäran kostnad för i hello Mongo-gränssnittet.
```
> db.sample.find()
```

Sedan köra kommandot hello *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Med detta i åtanke, en metod för att uppskatta reserverat dataflöde som krävs av programmet hello mycket kostar toorecord hello begäran enhet som är associerade med vanliga åtgärder som körs mot ett representativt objekt som används av ditt program och sedan uppskatta hello antal åtgärder vill du utföra varje sekund.

> [!NOTE]
> Om du har objekttyper som varierar kraftigt vad gäller storlek och hello antal indexerade egenskaper kan sedan registrera hello tillämpliga åtgärden begäran enhetsavgift som associeras med varje *typen* för vanliga objekt.
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a>Använd API för Mongodb's portal mätvärden
Hej enklaste sättet tooget en bra uppskattning av begäran enhet avgifter för ditt API för MongoDB-databas är toouse hello [Azure-portalen](https://portal.azure.com) mått. Med hello *antal begäranden* och *begäran kostnad* diagram, du kan få en uppskattning av hur många frågeenheter varje åtgärd är förbrukar och hur många frågeenheter de förbruka relativa tooone en annan.

![API för MongoDB portal mått][6]

## <a name="a-request-unit-estimation-example"></a>Exempel på beräkning av en begäran
Överväg följande ~ 1KB dokumentet hello:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Dokument är minified i Azure Cosmos DB så hello system beräknas hello dokument ovan är något mindre än 1KB.
> 
> 

hello följande tabell visas ungefärlig begäran enhet kostnader för vanliga åtgärder på det här objektet (hello ungefärliga begäran enhet kostnad förutsätter att hello kontonivå konsekvenskontroll har angetts för ”Session” och att alla objekt som indexeras automatiskt):

| Åtgärd | Begäran enhet kostnad |
| --- | --- |
| Skapa objekt |~ 15 RU |
| Läs objekt |~ 1 RU |
| Frågan objekt-ID: t |~2.5 RU |

Den här tabellen visas dessutom ungefärliga begäran enhet kostnader för vanliga frågor som används i programmet hello:

| Fråga | Begäran enhet kostnad | antal returnerade artiklar |
| --- | --- | --- |
| Välj mat-ID: t |~2.5 RU |1 |
| Välj livsmedel per tillverkare |~ 7 RU |7 |
| Välj av mat gruppen och ordning baserat på vikt |~ 70 RU |100 |
| Välj översta 10 livsmedel i en Matgrupp |~ 10 RU |10 |

> [!NOTE]
> RU avgifter variera beroende på hello antal objekt som returneras.
> 
> 

Med den här informationen kan uppskattar vi hello RU krav för det här programmet får hello antal åtgärder och frågor som vi räknar per sekund:

| Åtgärden-fråga | Uppskattat antal per sekund | Nödvändiga RUs |
| --- | --- | --- |
| Skapa objekt |10 |150 |
| Läs objekt |100 |100 |
| Välj livsmedel per tillverkare |25 |175 |
| Välj av Matgrupp |10 |700 |
| Välj Topp 10 |15 |150 totalt |

I det här fallet räknar vi med en genomsnittlig genomströmning krav på 1,275 RU/s.  Vi vill etablera 1 300 RU/s för det här programmet samling avrundning toohello närmast 100.

## <a id="RequestRateTooLarge"></a>Reserverat dataflöde överskreds i Azure Cosmos DB
Kom ihåg att konsumtion av begäran enheten utvärderas som en sats per sekund om hello budget är tom. För program som överskrider hello begär etablerad enhet förfrågningar för en behållare toothat samlingen kommer att begränsas förrän hello frekvensen sjunker under hello reserverade nivå. När en begränsning inträffar avslutas hello server förebyggande syfte hello begäran med RequestRateTooLargeException (HTTP-statuskod 429) och returnerar hello x-ms-retry-efter-ms-huvud som anger hello lång tid i millisekunder som hello användare måste vänta innan ett nytt försök hello begäran.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Om du använder hello klient-SDK för .NET och LINQ-frågor och de flesta hello tid du aldrig har toodeal med det här undantaget som hello nuvarande version av hello .NET klient-SDK fångar implicit svaret värnar hello server angetts försök igen efter rubrik och begäran om återförsök hello. Om ditt konto används samtidigt av flera klienter, lyckas hello nästa försök.

Om du har mer än en klient kumulativt drift ovan hello förfrågningar hello försök standardbeteendet finns tillräckligt inte och hello klienten genereras en DocumentClientException med status kod 429 toohello program. I sådana fall, kan du hantera försök beteende och logik i ditt program fel hantering rutiner eller att öka hello reserverat dataflöde för hello behållare.

## <a id="RequestRateTooLargeAPIforMongoDB"></a>Överskrider reserverat dataflöde gränser i API för MongoDB
Program som överskrider hello etableras frågeenheter för en samling kommer att begränsas förrän hello frekvensen sjunker under hello reserverade nivå. När en begränsning inträffar hello backend förebyggande syfte avslutas hello begäran med en *16500* felkoden - *för många begäranden*. Som standard API för MongoDB kommer automatiskt att försöka in too10 gånger innan det returneras en *för många begäranden* felkoden. Om du tar emot många *för många begäranden* felkoder, kan du antingen lägga till försök beteende i ditt program felhantering rutiner eller [öka hello reserverat dataflöde för hello samling](set-throughput.md).

## <a name="next-steps"></a>Nästa steg
toolearn mer om reserverat dataflöde med Azure Cosmos DB databaser utforska gärna dessa resurser:

* [Azure DB Cosmos-priser](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Partitionering data i Azure Cosmos DB](partition-data.md)

toolearn mer om Azure Cosmos DB finns hello Azure Cosmos DB [dokumentationen](https://azure.microsoft.com/documentation/services/cosmos-db/). 

tooget igång med skalning och prestandatester med Azure Cosmos DB finns [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md).

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
