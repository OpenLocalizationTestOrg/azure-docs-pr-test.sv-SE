---
title: "aaaWhat är Azure Search | Microsoft Docs"
description: "Azure Search är en fullständigt hanterad värdbaserade moln söktjänst. Läs mer i översikt över den här funktionen."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 50bed849-b716-4cc9-bbbc-b5b34e2c6153
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/26/2017
ms.author: ashmaka
ms.openlocfilehash: b38a7e93a7f0e0d5f8a3779858032271b3883778
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-search"></a>Vad är Azure Search?
Azure Search är en molnlösning för sökning som en tjänst som ger utvecklare API: er och verktyg för att lägga till en omfattande sökinställningar över dina data i webb-, mobil- och enterprise-program.

Funktioner som exponeras via en enkel [REST API](/rest/api/searchservice/) eller [.NET SDK](search-howto-dotnet-sdk.md) som döljer hello inbyggd komplexiteten i sökteknik. I tillägg tooAPIs ger hello Azure-portalen administrations- och prototyper. Tillgänglighet för infrastruktur och hanteras av Microsoft.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Funktioner – sammanfattning

| Kategori | Funktioner |
|----------|----------|
|Fulltextsökning och analys av text | [**Fulltextsökning** ](search-lucene-query-architecture.md) är en primär användningsfall för de flesta Sökbaserade appar. Frågor kan utformas med en stöds syntax: <br/><br/>[**Enkel frågesyntaxen**](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search), som innehåller logiska operatorer, frasen sökoperatorer, suffix operatorer, prioritet operatörer.<br/><br/>[**Lucene frågesyntaxen** ](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) erbjuder alla enkla frågeterm för support plus fuzzy sökning, närhet sökning, den och reguljära uttryck.| 
| Dataintegrering | Azure Search index ta emot data från alla datakällor under förutsättning att den skickas som en JSON-datastruktur. <br/><br/> Du kan också för datakällor som stöds i Azure, kan du använda [ **indexerare** ](search-indexer-overview.md) tooautomatically crawl [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-documentdb.md), eller [Azure Blob storage](search-howto-indexing-azure-blob-storage.md) toosync search-index är innehåll med din primära dataarkivet. Azure Blob-indexerare kan utföra *dokumentet knäcka* för [indexering större filformat](search-howto-indexing-azure-blob-storage.md), inklusive Microsoft Office, PDF- och HTML-dokument. |
| Sök analytics | [**Anpassade lexikala analyzers** ](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) är tillgängliga för avancerade sökningar med fonetisk matchande och vanliga uttryck. |
| Språkstöd | [**Språkanalys** ](https://docs.microsoft.com/rest/api/searchservice/language-support) från Lucene som Microsoft har naturligt språk processorer som är tillgängliga i 56 olika språk toointelligently referensen språkspecifika linguistics inklusive verbtempus, kön, oregelbundna plural substantiv (till exempel ' mus' eller 'möss'), word Frigör sammanslagning, radbrytning (för språk utan blanksteg) och mycket mer. |
| GEO-sökning | Azure Search Intelligent bearbetar, filter, och visar geografiska platser. Det låter användare tooexplore data baserat på hello närhet av en sökning resultatet tooa fysisk plats. [Det här videoklippet](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) eller [granska det här exemplet](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) toolearn mer. |
| Användarens upplevelse funktioner | [**Sökförslag** ](https://docs.microsoft.com/rest/api/searchservice/suggesters) kan aktiveras för typ-ahead frågor i ett sökfält. Faktiska dokument i ditt index föreslås som användare ange partiell sökning indata. <br/><br/>[**Fasetterad navigering** ](https://docs.microsoft.com/azure/search/search-faceted-navigation) aktiveras genom en enda frågeparameter. Azure Search returnerar en fasetterad navigeringsstruktur som du kan använda som hello koden bakom en lista med kategorier, för automatisk dirigerad filtrering (till exempel toofilter katalogobjekt av pris-intervallet eller varumärken). <br/><br/> [**Filter** ](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) kan vara används tooincorporate fasetterad navigering i programmets användargränssnitt, förbättra frågan formulering och filtrera baserat på eller developer-angivna villkor. Skapa filter med hello OData-syntax.<br/><br/> [**Träffar syntaxmarkering** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) gäller visual formuläret atting tooa matchande nyckelord i sökresultaten. Du kan välja vilka fält returnera markerade kodavsnitt.<br/><br/>[**Sortering** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) är erbjuds för flera fält via hello indexeringsschema och sedan växlas när databasfrågan med en enda sökparameter.<br/><br/> [**Växling** ](search-pagination-page-layout.md) och begränsa sökresultaten är enkelt med hello buffertstorleken justerade kontroll som Azure Search erbjuder över sökresultat.  
| Relevans | [**Enkel resultatfunktioner** ](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) är en fördel med Azure Search. Bedömningsprofil profiler är används toomodel relevans som en funktion för värdena i hello dokument sig själva. Till exempel du kanske vill nyare produkter eller rabatterad produkter tooappear högre upp i hello sökresultat. Du kan också skapa bedömningsprofil profiler med hjälp av taggar för anpassade poäng utifrån kundens sökinställningar du spåras och lagrats separat. |
| Övervakning och rapportering | [**Sök trafik analytics** ](search-traffic-analytics.md) är som samlas in och analyseras toounlock insikter från vad användare skriver i sökrutan hello. <br/><br/>Mått på frågor per sekund, svarstid och begränsning fångas in och rapporteras i portalens sidor utan ytterligare konfiguration krävs. Du kan också enkelt övervakaren index och dokumentet räknar så att du kan justera kapacitet vid behov. Mer information finns i [tjänsten administration](search-manage.md) |
| Verktyg för prototyper och kontroll | I hello-portalen kan du använda hello [ **guiden Importera data** ](search-import-data-portal.md) tooconfigure indexerare index designer toostand upp ett index och [ **Sök explorer** ](search-explorer.md) tootest frågar och förfina bedömningsprofil profiler. Du kan också öppna alla index tooview dess schema. |
| Infrastruktur | Hej **högtillgänglig plattform** garanterar en mycket tillförlitlig sökinställningar för tjänsten. När skalas korrekt [Azure Search erbjuder ett SLA för 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> **Fullständigt hanterad och skalbar** som en lösning för slutpunkt till slutpunkt, kräver Azure Search absolut inga infrastrukturhantering. Tjänsten kan vara skräddarsydda tooyour behov genom att skala i två dimensioner toohandle mer dokumentlagring, högre belastning för frågan eller båda.

## <a name="how-it-works"></a>Hur det fungerar
### <a name="step-1-provision-service"></a>Steg 1: Etablera tjänsten
Du kan få igång en Azure Search-tjänst i hello [Azure-portalen](https://portal.azure.com/) eller via hello [Azure Resource Management API](/rest/api/searchmanagement/). Du kan välja antingen hello ledigt service som delas med andra prenumeranter, eller en [betald nivå](https://azure.microsoft.com/pricing/details/search/) som dedikerar resurser som bara används av tjänsten. Du kan skala en tjänst i två dimensioner för betald nivåer: 

- Lägg till repliker toogrow kapacitet toohandle tunga frågan läses in.   
- Lägg till partitioner toogrow lagring för flera dokument. 

Genom att hantera dokument lagrings- och genomströmning separat kalibrera du resourcing baserat på kraven för produktion.

### <a name="step-2-create-index"></a>Steg 2: Skapa index
Innan du kan överföra sökbara innehåll, måste du först definiera ett Azure Search-index. Ett index är som en databastabell som innehåller dina data och kan acceptera sökfrågor. Du definierar hello index schemat toomap tooreflect hello struktur hello dokument som du vill toosearch, liknande toofields i en databas.

Ett schema kan skapas i hello Azure portal eller programmässigt med hello [.NET SDK](search-howto-dotnet-sdk.md) eller [REST API](/rest/api/searchservice/).

### <a name="step-3-index-data"></a>Steg 3: Indexinformationen
När du har definierat ett index är du redo tooupload innehåll. Du kan använda en sändning eller hämtning modell.

hello pull modellen hämtar data från externa datakällor. Den stöds via *indexerare* att förenkla och automatisera aspekter av datapåfyllning som ansluter till, läser och serialisering. [Indexerare](/rest/api/searchservice/Indexer-operations) är tillgängliga för Azure Cosmos DB, Azure SQL Database, Azure Blob Storage och SQL Server finns i en Azure VM. Du kan konfigurera en indexerare för på begäran eller enligt schemalagda datauppdatering.

push-modell för hello tillhandahålls via hello SDK eller REST API: er, skickar uppdaterade dokument tooan index. Du kan skicka data från valfri dataset med hello JSON-format. Se [Lägg till, uppdatera eller ta bort dokument](/rest/api/searchservice/addupdate-or-delete-documents) eller [hur toouse hello .NET SDK)](search-howto-dotnet-sdk.md) anvisningar om att läsa in data.

### <a name="step-4-search"></a>Steg 4: Sök
Efter att fylla i ett index, kan du [utfärda sökfrågor](/rest/api/searchservice/Search-Documents) tooyour tjänstslutpunkten med enkel HTTP-begäranden med REST API eller hello .NET SDK.

## <a name="how-it-compares"></a>Jämförelse

Kunder ofta fråga hur [fulltextsökning i Azure Search](search-lucene-query-architecture.md) Jämför med [fulltextsökning](https://en.wikipedia.org/wiki/Full_text_search) i sin Databasprodukt. Vår svaret är att Azure Search språk funktioner är bättre och mer flexibelt med stöd för Lucene frågor, språkanalys från Lucene och Microsoft anpassade analyzers för fonetisk eller andra särskilda indata och hello möjlighet toomerge data från flera källor i hello sökindex. 

En annan är angripits att en lösning för sökning adresser hello hela sökinställningar. Du kan exempelvis anpassade poängsättning av resultat, fasetterad navigering för automatisk dirigerad filtrering, träffar syntaxmarkering och typeahead frågeförslag. 

Verktyg för övervakning och förståelse av frågeaktivitet kan också räkna i Sök lösningen beslut. Azure Search stöder till exempel [söka trafik analytics](search-traffic-analytics.md) mått på klickningar upp söker, söker utan klickningar och så vidare. I produkter där sökningen är ett tillägg, du är inte troligt toofind dessa funktioner.

Resursanvändningen är ett annat övervägande. Sökning med naturligt språk är ofta beräkningsmässigt intensiv. Några av våra kunder avläst Sök operations tooAzure sökning som en sätt toopreserve datorresurser för transaktionsbearbetning. Du kan enkelt ändra skala toomatch frågan volym filernas sökningen.

När du har bestämt toogo med dedikerade search är ditt nästa val mellan en molnbaserad tjänst eller en lokal server. En tjänst i molnet är hello rätt val om du vill att en [nyckelfärdig lösning med minimal belastning och underhåll och justerbara skala](#cloud-service-advantage).

Inom hello molnet paradigmet erbjuder flera providers jämförbara grundläggande funktioner med fulltextsökning, geo-sökning och hello möjlighet toohandle andelen tvetydighet i Sök-indata. Normalt den har en [specialiserade funktionen](#feature-drilldown), eller hello enkel och övergripande enkelhet av API: er, verktyg och hantering som bestämmer hello bäst.

Molntjänstleverantörer är Azure Search starkaste för fulltext-sökning arbetsbelastningar över innehållslager och databaser på Azure, för appar som förlitar sig främst på söka efter både hämtning av information och innehåll navigering. Viktiga fördelar är:

+ Azure dataintegrering (robotar) på hello indexering lager
+ Azure portal för central hantering
+ Azure skalbarhet, tillförlitlighet och världsklass tillgänglighet
+ Språkliga och anpassade analys med analyzers för fasta fulltextsökning i 56 språk
+ [Grundläggande funktioner vanliga toosearch till Central appar](#feature-drilldown): resultatfunktioner, faceting, förslag, synonymer, geo-sökning och mycket mer.

> [!Note]
> toobe tydliga, Azure-datakällor stöds fullt ut, men förlita sig på en mer kod-intensiva push-metod i stället för indexerare. Använda våra API: er kan skicka du ett JSON-dokument samling tooan Azure Search index.

Bland våra kunder är dessa kan tooleverage hello bredast utbud av funktioner i Azure Search onlinekataloger, line-of-business-program och program för identifiering av dokumentet.

## <a name="rest-api--net-sdk"></a>REST API | .net SDK

När många aktiviteter kan utföras i hello-portalen, är Azure Search avsedd för utvecklare som vill toointegrate sökfunktionen i befintliga program. hello följande programmeringsgränssnitt är tillgängliga.

|Plattform |Beskrivning |
|-----|------------|
|[REST](/rest/api/searchservice/) | HTTP-kommandon som stöds av alla programming plattformar och språk, inklusive Xamarin, Java och JavaScript|
|[.NET SDK](search-howto-dotnet-sdk.md) | .NET-Omslutning för hello REST API ger effektiv kodning i C# och andra språk för förvaltad kod som mål hello .NET Framework |

## <a name="free-trial"></a>Kostnadsfri utvärderingsversion
Azure-prenumeranter kan [etablera en tjänst i hello kostnadsfria nivån](search-create-service-portal.md).

Om du inte är en prenumerant kan du [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Du får kredit för att pröva Azure-betaltjänster. När du använt, kan du hålla hello-konto och använda [kostnadsfria Azure-tjänster](https://azure.microsoft.com/free/). Ditt kreditkort debiteras aldrig såvida du inte uttryckligen ändrar dina inställningar och be toobe debiteras.

Du kan också [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): din MSDN-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster. 

## <a name="how-tooget-started"></a>Hur tooget igång

1. Skapa en tjänst i hello [kostnadsfria nivån](search-create-service-portal.md).

2. Gå igenom en eller flera av följande kurser hello. 

  + [Hur toouse hello .NET SDK](search-howto-dotnet-sdk.md) visar hello huvudsakliga steg i förvaltad kod.  
  + [Kom igång med hello REST API](https://github.com/Azure-Samples/search-rest-api-getting-started) visar hello samma steg med hello REST API.  
  + [Skapa ditt första index i hello portal](search-get-started-portal.md) visar hello portal inbyggda indexering och prototyp funktioner.   

Sökmotorer är hello vanliga drivrutiner för hämtning av information i mobilappar, på hello web och i företagets datalager. Azure Search ger dig verktyg för att skapa en liknande toothose för search-upplevelse på stora kommersiella webbplatser.

Lär dig hur integrera en sökmotor kan göra din app i den här 9-minuters videon från Programhanteraren Liam Cavanagh. Kort demonstrationer beskriver viktiga funktioner i Azure Search och hur ett typiskt arbetsflöde ser ut. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minuter omfattar viktiga funktioner och användningsfall.
+ 3-4 minuter täcker service etablering. 
+ 4-6 minuter omfattar importera Data guiden används toocreate ett index med hello inbyggda fastigheter dataset.
+ 6-9 minuter omfattar Sök Explorer och olika frågor.


