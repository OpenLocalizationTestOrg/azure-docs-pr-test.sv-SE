---
title: "Azure CosmosDB: Programbegäran per minut (RU/m) | Microsoft Docs"
description: "Lär dig hur tooreduce kostnader genom att använda enheter för programbegäran per minut."
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
ms.openlocfilehash: fcc3a92b9788750a2bfba361c3a9cebdb56eee05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a>Frågeenheter per minut i Azure Cosmos DB

Azure Cosmos-DB är utformad toohelp du få en snabb och förutsägbar prestanda och skalning sömlöst tillsammans med ditt program tillväxt. Du kan etablera dataflödet på en Cosmos-DB-behållare på båda, per sekund och granulariteter per minut (RU/m). hello dataflöde på per minut Granularitet är används toomanage oväntat toppar i hello arbetsbelastning som äger rum med en kornighet per sekund. 

Den här artikeln innehåller en översikt över hur hello etablering av Frågeenhet per minut (RU/m) fungerar. hello mål i åtanke med etablering av RU/m är tooprovide en förutsägbar prestanda runt oförutsägbart behov (särskilt om du behöver toorun analytics ovanpå operational data) och spiky arbetsbelastningar. Vi vill toohave våra kunder förbruka mer hello genomströmning de konfigurerar så att de kan skalas snabbt med sinnesro.

När du har läst den här artikeln kommer du att kunna tooanswer hello följande frågor:

* Hur fungerar en Frågeenhet per minut?
* Vad är hello skillnaden mellan Frågeenhet per minut och Frågeenhet per sekund?
* Hur tooprovision RU/m?
* Under vilket scenario beakta jag etablering Frågeenhet per minut?
* Hur hello toouse portal mått toooptimize min kostnad och prestanda?
* Definiera vilken typ av begäran kan använda din RU/m budget?

## <a name="provisioning-request-units-per-minute-rum"></a>Etablering av frågeenheter per minut (RU/m)

När du etablerar Azure Cosmos DB på hello andra granularitet (RU/s) kan du hämta hello garanti för att din begäran lyckas på en låg latens om din genomströmning inte har överstigit hello kapacitet etablerats i den andra. Med RU/m är hello granularitet på hello minut med hello garanti för att begäran slutförs inom den minuten. Jämfört med toobursting system kan vi se till att hello prestanda som du får är förutsägbar och du kan planera på den.

hello sätt per minut etablering fungerar är enkel:

* RU/m faktureras timvis och tillägg tooRU/s. Mer information finns i Azure Cosmos DB [sida med priser](https://aka.ms/acdbpricing).
* RU/m kan aktiveras på samlingsnivå. Som kan göras via hello SDK: er (Node.js, Java eller .net) eller hello-portal (också inkludera MongoDB API arbetsbelastningar)
* När RU/m är aktiverat för varje 100 RU/s etableras, också få 1 000 RU/m etablerats (hello förhållandet är 10 x)
* Vid en given andra förbrukar en begäran enhet din RU/m etablering endast om du har överskridit din per andra etablering i den andra
* En gång Hej 60-sekunders tid (UTC) avslutas, hello per minut etablering fyllas
* RU/m kan aktiveras endast för samlingar med en maximal etablering av 5 000 RU/s per partition. Om du skala dina behov av genomflöde och har en hög nivå av etablering per partition, får du ett varningsmeddelande

Nedan ett konkreta exempel, där en kund kan etablera 10kRU/s med 100kRU/m, sparar 73% kostnaden mot etablering för belastning (på 50kRU per sekund) via en 90 sekunder period på en samling som har 10 000 RU/s och 100 000 RU/m etableras:

* 1 sekund: hello RU/m budget är inställt på 100 000
* 3: e sekund: under den andra hello förbrukningen av Frågeenhet var 11,010 RUs, 1,010 RUs ovan hello RU/s-etablering. Därför dras 1,010 RUs från hello RU/m budget. 98,990 RUs är tillgängliga för hello nästa 57 sekunder i hello RU/m budget
* 29: e sekund: under den andra stora topp har hänt (> 4 x som är högre än etablering per sekund) och hello förbrukningen av Frågeenhet var 46,920 RUs. 36,920 RUs dras från hello RU/m budget som tagits bort från 92,323 RUs (28 sekund) too55, 403 RUs (29: e sekund)
* 61st sekund: RU/m budget anges tillbaka too100 000 RUs.
 
![Diagram som visar hello förbrukning och etablering av Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a>Ange kapacitet för begäran-enhet med RU/m

När du skapar en Azure DB som Cosmos-samling kan du ange hello många frågeenheter per sekund (RU per sekund) som du vill reserverade för hello samling. Du kan också bestämma om du vill tooadd RU per minut. Detta kan göras via hello portalen eller hello SDK. 

### <a name="through-hello-portal"></a>Via hello Portal

Aktivera eller inaktivera RU per minut kräver bara ett klick vid etablering av en samling. 

 ![Skärmbild som visar hur tooset RU/m i hello Azure-portalen](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-hello-sdk"></a>Via hello SDK
Först, det är viktigt toonote RU/m är endast tillgängligt för följande SDK hello:

* .NET 1.14.0
* Java 1.11.0
* Node.js 1.12.0
* Python 2.2.0

Här är ett kodfragment för att skapa en samling med 3 000 frågeenheter per andra och 30 000 frågeenheter per minut med hello .NET SDK:

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set hello throughput too3,000 request units per second which will give you 30,000 request units per minute as hello RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

Här är ett kodfragment för att ändra hello genomströmningen i en samling too5, 000 frågeenheter per sekund utan etablering RU per minut med hello .NET SDK:

```csharp
// Get hello current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput too5000 request units per second without RU/m enabled (hello last parameter tooOfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a>Bra passa scenarier

I det här avsnittet ger vi en översikt över scenarier som passar bra för att aktivera frågeenheter per minut.

**Miljö för utveckling och testning:** passar bra. Hello development etapp, om du vill testa ditt program med olika arbetsbelastningar ange RU/m hello flexibilitet i det här skedet. När hello [emulatorn](local-emulator.md) är en bra gratisverktyg tootest Azure Cosmos DB. Om du vill toostart i en molnmiljö kan du men har en stor flexibilitet med RU/m för dina behov för ad hoc-prestanda. Du kan göra fler inställningar som utvecklar mindre oroa prestandabehov först. Vi rekommenderar att du börjar med hello minsta RU/s-etablering och aktivera RU/m.

**Oväntade, spiky, minut granularitet måste:** passar bra – besparingar: 25 75%. Vi har sett stora improvement från RU/m och de flesta scenarier med produktion finns i den gruppen. Om du har en IoT-arbetsbelastning som har topp några gånger i en minut, om du har frågor som körs när systemet gör vikt infogningen hello samma tid och du behöver extra kapacitet för handeling spiky måste. Vi rekommenderar att optimera din resursbehov genom att använda vår metod för steg-för-steg nedan.

 ![Diagram som visar begäran förbrukning i 5 minuter granularitet](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 *Bild - RU förbrukning prestandamått*

**Sinnesro:** passar bra – besparingar: 10-20%. Ibland kan du vill bara toohave sinnesro och bry dig inte om potentiella toppar och begränsning. Den här funktionen är hello rätt en för dig. I så fall rekommenderar vi att aktivera RU/m och något lägre din per andra etablering. Det här fallet skiljer sig från hello ovan som du inte försöker toooptimize aggressivt din etablering. Det här är mer av ett ”noll begränsning” tänkesätt du befinner dig i.

Kritiska åtgärder med ad hoc behov: Vi rekommenderar ibland tooonly kan kritiska åtgärder åtkomst RU/m budget så hello budget inte får använda av ad hoc eller mindre viktiga åtgärder. Som kan definieras enkelt i hello nedan.

## <a name="using-hello-portal-metrics-toooptimize-cost-and-performance"></a>Med hjälp av hello portal mått toooptimize kostnad och prestanda

**I hello några veckor, kommer vi ytterligare utveckla hello innehåll runt övervakning RUs minut förbrukning toooptimize din genomströmning måste.**

Via hello portal statistik och, kan du se hur mycket vanlig RU sekunder du använder jämfört med RU minuter. Övervaka de här måtten hjälper dig att optimera din etablering. 

Vi rekommenderar en steg-för-steg-metod på hur toouse RU/m tooyour nytta. För varje steg du bör ha en översikt över hello RU förbrukning som representerar en fullständig cykel för din arbetsbelastning (det kan vara timmar, dagar, eller även veckor) och få insikter om hello användning av du etablera.

hello principen bakom den här metoden är toomake din genomströmning etablering som nära som möjligt tooa etablering som matchar sökvillkoren prestanda. 

![Diagram som visar begäran förbrukning i 5 minuter granularitet](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
toounderstand hello optimala etablering platsen för din arbetsbelastning behöver du toounderstand:

* Förbrukning mönster: Nej, ovanligt eller varaktigt toppar? Liten (2 x medelvärde), medel eller stor (> 10 x medelvärde) toppar?
* Procent av begäranden för begränsad: känner du föredrar att om du har lite begränsning? I så fall, med hur mycket? 

När du har identifierat dina mål är, kommer du att kunna tooget närmare toohello optimala etablering.

tooassist, vi vill tooprovide en allmän vägledning om hur toooptimize din etablering enligt RU/m användning. Den här vägledningen gäller inte tooall typer av arbetsbelastningar men baseras på hello privat förhandsversion kunskap. Vi kan ändra dessa baslinjer som vi Läs mer:

|RU/m % användning|Grad av användning av RU/m|Rekommenderade åtgärder för etablering|
|---|---|---|
|0-1%|Under användning|Lägre RU/s tooconsume mer RU/m|
|1-10%|Felfri användning|Hello Behåll samma etablering nivå|
|Över 10%|Överbelastning|Öka RU/s toorely mindre på RU/m|

## <a name="select-which-operations-can-consume-hello-rum-budget"></a>Välj vilka aktiviteter kan förbruka hello RU/m budget

På begäran-nivå, du kan också aktivera och inaktivera RU/m budget tooserve hello begäran oavsett åtgärd. Om används reguljära etablerade RUs per sekund budget och hello begäran kan inte använda hello RU/m budget, kommer att begränsas denna begäran. Som standard är varje begäran hanteras av RU/m budget om RU/m genomströmning budget är aktiverad. 

Här är ett kodfragment för att inaktivera RU/m budget med hello DocumentDB API för CRUD- och fråga.

```csharp
// In order toodisable any CRUD request for RU/m, set DisableRUPerMinuteUsage tootrue in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order toodisable any query request for RU/m, set DisableRUPerMinuteOnRequest tootrue in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a>Nästa steg

Vi har i den här artikeln beskrivs hur partitionering fungerar i Azure Cosmos DB, hur du kan skapa partitionerade samlingar och hur du kan välja en bra partitionsnyckel för ditt program.

* Utföra skalnings- och prestandatester med Azure Cosmos DB. Se [prestanda och Skalningstester med Azure Cosmos DB](performance-testing.md) ett exempel.
* Komma igång med programmeringen med hello [SDK](documentdb-sdk-dotnet.md) eller hello [REST API](/rest/api/documentdb/).
* Lär dig mer om [etablerat dataflöde](request-units.md) i Azure Cosmos DB 

