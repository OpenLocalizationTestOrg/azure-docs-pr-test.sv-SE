---
title: "Azure CosmosDB: Programbegäran per minut (RU/m) | Microsoft Docs"
description: "Lär dig att minska kostnaderna genom att använda frågeenheter per minut."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 72d60d5656ad664e42a848fc9b372cb09e49b888
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a>Frågeenheter per minut i Azure Cosmos DB

Azure Cosmos-DB är utformat för att hjälpa dig att uppnå en snabb och förutsägbar prestanda och skalning sömlöst tillsammans med ditt program tillväxt. Du kan etablera dataflödet på en Cosmos-DB-behållare på båda, per sekund och granulariteter per minut (RU/m). Det etablerade dataflödet på minutprecision används för att hantera oväntade toppar i arbetsbelastning som sker med en per sekund-precision. 

Den här artikeln innehåller en översikt över hur etableringen av Frågeenhet per minut (RU/m) fungerar. Målet i åtanke med etablering av RU/m är att tillhandahålla en förutsägbar prestanda runt oförutsägbart behov (särskilt om du behöver köra analytics ovanpå operational data) och spiky arbetsbelastningar. Vi vill att kunderna förbruka mer genomströmning som de konfigurerar så att de kan skalas snabbt med sinnesro.

När du har läst den här artikeln kommer du att kunna svara på följande frågor:

* Hur fungerar en Frågeenhet per minut?
* Vad är skillnaden mellan Frågeenhet per minut och Frågeenhet per sekund?
* Hur du etablerar RU/m?
* Under vilket scenario beakta jag etablering Frågeenhet per minut?
* Hur du använder portalen mått för att optimera min kostnad och prestanda?
* Definiera vilken typ av begäran kan använda din RU/m budget?

## <a name="provisioning-request-units-per-minute-rum"></a>Etablering av frågeenheter per minut (RU/m)

När du etablerar Azure Cosmos DB på andra Granulariteten (RU/s) kan du hämta garanti för att din begäran lyckas på en låg latens om din genomströmning inte har överskridit den kapacitet som tilldelats i den andra. Med RU/m är Granulariteten minut med garanti för att begäran slutförs inom den minuten. Jämfört med burst-överföring system, kontrollera vi att du får prestanda är förutsägbar och kan du planera på den.

Hur per minut etablering fungerar är enkel:

* RU/m faktureras timvis och utöver RU/s. Mer information finns i Azure Cosmos DB [sida med priser](https://aka.ms/acdbpricing).
* RU/m kan aktiveras på samlingsnivå. Som kan göras via SDK: er (Node.js, Java eller .net) eller via portalen (också inkludera MongoDB API arbetsbelastningar)
* När RU/m är aktiverat för varje 100 RU/s etableras, också få 1 000 RU/m etablerats (förhållandet är 10 x)
* Vid en given andra förbrukar en begäran enhet din RU/m etablering endast om du har överskridit din per andra etablering i den andra
* När 60-sekunders tid (UTC) slutar den per minut etablering är fyllas
* RU/m kan aktiveras endast för samlingar med en maximal etablering av 5 000 RU/s per partition. Om du skala dina behov av genomflöde och har en hög nivå av etablering per partition, får du ett varningsmeddelande

Nedan ett konkreta exempel, där en kund kan etablera 10kRU/s med 100kRU/m, sparar 73% kostnaden mot etablering för belastning (på 50kRU per sekund) via en 90 sekunder period på en samling som har 10 000 RU/s och 100 000 RU/m etableras:

* 1 sekund: RU/m budget är inställt på 100 000
* 3: e sekund: under den andra förbrukningen av Frågeenhet var 11,010 RUs, 1,010 RUs ovan RU/s etableringen. Därför dras 1,010 RUs från RU/m budget. 98,990 RUs är tillgängliga nästa 57 sekunder i budget RU/m
* 29: e sekund: under den andra stora topp har hänt (> 4 x som är högre än etablering per sekund) och förbrukningen av Frågeenhet var 46,920 RUs. 36,920 RUs dras från RU/m budget bort från 92,323 RUs (28 sekund) till 55,403 RUs (29: e sekund)
* 61st sekund: RU/m budget ändras till 100 000 RUs.
 
![Diagrammet visar förbrukningen och etablering av Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a>Ange kapacitet för begäran-enhet med RU/m

När du skapar en Azure DB som Cosmos-samling kan du ange hur många frågeenheter per sekund (RU per sekund) som du vill reserverade för samlingen. Du kan också bestämma om du vill lägga till RU per minut. Detta kan göras via portalen eller SDK. 

### <a name="through-the-portal"></a>Via portalen

Aktivera eller inaktivera RU per minut kräver bara ett klick vid etablering av en samling. 

 ![Skärmbild som visar hur du ställer in RU/m i Azure-portalen](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-the-sdk"></a>Via SDK
Först, är det viktigt att notera RU/m är endast tillgängligt för följande SDK:

* .NET 1.14.0
* Java 1.11.0
* Node.js 1.12.0
* Python 2.2.0

Här är ett kodfragment för att skapa en samling med 3 000 frågeenheter per andra och 30 000 frågeenheter per minut med .NET SDK:

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set the throughput to 3,000 request units per second which will give you 30,000 request units per minute as the RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

Här är ett kodfragment för att ändra genomflödet av en samling till 5 000 frågeenheter per sekund utan etablering RU per minut med .NET SDK:

```csharp
// Get the current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to 5000 request units per second without RU/m enabled (the last parameter to OfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a>Bra passa scenarier

I det här avsnittet ger vi en översikt över scenarier som passar bra för att aktivera frågeenheter per minut.

**Miljö för utveckling och testning:** passar bra. Under utveckling-steget om du vill testa ditt program med olika arbetsbelastningar ange RU/m flexibilitet i det här skedet. När den [emulatorn](local-emulator.md) är en bra kostnadsfria verktyg för att testa Azure Cosmos DB. Men om du vill starta i en molnmiljö behöver en stor flexibilitet med RU/m för dina behov för ad hoc-prestanda. Du kan göra fler inställningar som utvecklar mindre oroa prestandabehov först. Vi rekommenderar att du börjar etablering av minsta RU/s och aktivera RU/m.

**Oväntade, spiky, minut granularitet måste:** passar bra – besparingar: 25 75%. Vi har sett stora improvement från RU/m och de flesta scenarier med produktion finns i den gruppen. Om du har en IoT-arbetsbelastning som har topp några gånger i en minut, om du har frågor som körs när systemet gör masslagring insert samtidigt behöver extra kapacitet för handeling spiky behov. Vi rekommenderar att optimera din resursbehov genom att använda vår metod för steg-för-steg nedan.

 ![Diagram som visar begäran förbrukning i 5 minuter granularitet](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 *Bild - RU förbrukning prestandamått*

**Sinnesro:** passar bra – besparingar: 10-20%. Ibland kan du vill bara ha sinnesro och bry dig inte om potentiella toppar och begränsning. Den här funktionen är rätt för dig. I så fall rekommenderar vi att aktivera RU/m och något lägre din per andra etablering. Det här fallet skiljer sig från ovanstående som du inte försöka optimera aggressivt din etablering. Det här är mer av ett ”noll begränsning” tänkesätt du befinner dig i.

Kritiska åtgärder med ad hoc behov: ibland rekommenderas så att endast kritiska åtgärder åtkomst RU/m budget så budget inte får använda av ad hoc eller mindre viktiga åtgärder. Som kan definieras enkelt i följande avsnitt.

## <a name="using-the-portal-metrics-to-optimize-cost-and-performance"></a>Använda portalen mått för att optimera kostnad och prestanda

**Inom några veckor kommer vi ytterligare utveckla innehållet runt övervakning RUs minut förbrukningen för att optimera dina behov för genomströmning.**

Du kan se hur mycket vanlig RU sekunder du använder jämfört med RU minuter via portalen mått. Övervaka de här måtten hjälper dig att optimera din etablering. 

Vi rekommenderar en steg-för-steg-metod för hur du använder RU/m till din fördel. För varje steg du bör ha en översikt över förbrukningen RU som representerar en fullständig cykel för din arbetsbelastning (det kan vara timmar, dagar, eller även veckor) och få insikter om användningen av du etablera.

Principen bakom den här metoden är att din genomströmning etablering så nära som möjligt till en allokering som matchar sökvillkoren prestanda. 

![Diagram som visar begäran förbrukning i 5 minuter granularitet](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
För att förstå den optimala etablering punkten för din arbetsbelastning, måste du förstå:

* Förbrukning mönster: Nej, ovanligt eller varaktigt toppar? Liten (2 x medelvärde), medel eller stor (> 10 x medelvärde) toppar?
* Procent av begäranden för begränsad: känner du föredrar att om du har lite begränsning? I så fall, med hur mycket? 

När du har identifierat dina mål är kan kommer du att kunna hämta närmast optimal etableringen.

För att hjälpa dig som vi vill ge ett övergripande riktlinjer för hur du optimerar dina etablering enligt RU/m användning. Den här vägledningen gäller inte för alla typer av arbetsbelastningar men baserat på kunskap privat förhandsversion. Vi kan ändra dessa baslinjer som vi Läs mer:

|RU/m % användning|Grad av användning av RU/m|Rekommenderade åtgärder för etablering|
|---|---|---|
|0-1%|Under användning|Lägre RU/s förbruka mer RU/m|
|1-10%|Felfri användning|Behåll samma etablering nivå|
|Över 10%|Överbelastning|Öka RU/s för att förlita sig mindre på RU/m|

## <a name="select-which-operations-can-consume-the-rum-budget"></a>Välj vilka aktiviteter kan förbruka budget RU/m

På begäran-nivå, du kan också aktivera och inaktivera RU/m budget för att hantera begäran oavsett åtgärd. Om används reguljära etablerade RUs per sekund budget och begäran kan inte använda RU/m budget, kommer att begränsas denna begäran. Som standard är varje begäran hanteras av RU/m budget om RU/m genomströmning budget är aktiverad. 

Här är ett kodfragment för att inaktivera RU/m budget med DocumentDB-API för CRUD- och fråga.

```csharp
// In order to disable any CRUD request for RU/m, set DisableRUPerMinuteUsage to true in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order to disable any query request for RU/m, set DisableRUPerMinuteOnRequest to true in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a>Nästa steg

Vi har i den här artikeln beskrivs hur partitionering fungerar i Azure Cosmos DB, hur du kan skapa partitionerade samlingar och hur du kan välja en bra partitionsnyckel för ditt program.

* Utföra skalnings- och prestandatester med Azure Cosmos DB. Se [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md) ett exempel.
* Komma igång med programmeringen med den [SDK](documentdb-sdk-dotnet.md) eller [REST API](/rest/api/documentdb/).
* Lär dig mer om [etablerat dataflöde](request-units.md) i Azure Cosmos DB 

