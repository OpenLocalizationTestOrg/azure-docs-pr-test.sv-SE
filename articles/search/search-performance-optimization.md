---
title: "aaaAzure Sök prestanda och optimering överväganden | Microsoft Docs"
description: "Finjustera prestanda för Azure Search och konfigurera optimala skala"
services: search
documentationcenter: 
author: LiamCavanagh
manager: pablocas
editor: 
ms.assetid: 4d3cd864-29d2-4921-be0d-a3f1a819de46
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: 725149ba32773ab6b4c9d4b441424aba78db7c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-performance-and-optimization-considerations"></a>Azure Search-prestanda och optimering överväganden
En bra sökinställningar är en nyckel toosuccess för många mobila enheter och webbprogram. Från fastigheter påverkar tooused bilen marknadsplatser tooonline kataloger, Snabbsökning och relevanta resultat hello kundupplevelsen. Det här dokumentet är avsett toohelp du identifiera bästa praxis för hur tooget hello mesta möjliga av Azure Search, särskilt för avancerade scenarier med sofistikerade krav för skalbarhet, stöd för flera språk eller anpassade rangordning.  Dessutom kan det här dokumentet beskrivs internals och täcker metoder fungera effektivt i verkliga kunden appar.

## <a name="performance-and-scale-tuning-for-search-services"></a>Prestanda och skalning justera för Search-tjänster
Vi kan alla använda toosearch motorer, till exempel Bing och Google hello hög prestanda och de erbjuder.  När kunder använder din sökning-aktiverade webb- eller mobila program, kommer de därför förvänta sig liknande prestandaegenskaper.  När du optimerar prestanda för sökning är på bästa sätt för hello toofocus på latens, vilket är hello tid som en fråga tar toocomplete och returnera resultat.  När du optimerar för sökning fördröjning är det viktigt att:

1. Välj en målfördröjning (eller hur lång tid) som en typisk sökbegäran bör vidta toocomplete.
2. Skapa och testa en verklig arbetsbelastning mot din söktjänst med en realistisk dataset toomeasure dessa latens priser.
3. Börja med ett lågt antal frågor per sekund (QPS) och fortsätta tooincrease hello nummer köras i hello test förrän hello frågan latens sjunker under hello definierats målfördröjning.  Det här är ett viktigt benchmark toohelp som du planerar för att skala när programmet växer i användning.
4. Om möjligt återanvända HTTP-anslutningar.  Om du använder hello Azure Search .NET SDK, det innebär att du ska använda en instans eller [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) instans och om du använder Hej REST-API, bör du återanvänder en enda HttpClient.

När du skapar dessa testa arbetsbelastningar finns det vissa egenskaper för Azure Search tookeep i åtanke:

1. Det är möjligt toopush så många Sök frågor i taget, att hello tillgängliga resurser i Azure Search-tjänsten kommer att för många.  När det händer visas HTTP 503 svarskoder.  Därför är det bästa toostart med olika områden av search-begäranden toosee hello skillnaderna i svarstid priser när du lägger till flera search-begäranden.
2. Överföra innehåll tooAzure Sök påverkar hello prestandan och svarstiden för hello Azure Search-tjänsten.  Om du förväntar dig toosend data medan användare utför sökningar, är det viktigt tootake arbetsbelastningen till kontot i dina tester.
3. Inte alla sökfråga utför på hello samma prestanda.  Till exempel utför ett dokument lookup- eller förslag vanligtvis snabbare än en fråga med ett stort antal aspekter och filter.  Det är den bästa tootake hello olika frågor som du förväntar dig toosee till kontot när du skapar dina tester.  
4. Variation av search-begäranden är viktig eftersom om du kör kontinuerligt hello samma söka begäranden, cachelagring av data start toomake prestanda ser bättre än den kan med flera olika fråga.

> [!NOTE]
> [Visual Studio belastningen testning](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) är ett mycket bra sätt tooperform din benchmark testar eftersom du tooexecute HTTP-begäranden som du behöver för att köra frågor mot Azure Search och aktiverar parallellisering för förfrågningar.
> 
> 

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Skala Azure Search för hög frågan priser och begränsas begäranden
När du tar emot för många begäranden för begränsad eller överskrider dina mål latens priser från en ökad frågebelastningen titta toodecrease svarstid priser på något av två sätt:

1. **Öka repliker:** en replik är som en kopia av dina data så att Azure Search tooload belastningsutjämna förfrågningar mot hello flera kopior.  Alla belastningsutjämning och replikering av data över repliker hanteras av Azure Search och du kan ändra hello antal repliker som allokerats för din tjänst när som helst.  Du kan allokera too12 repliker i en standardsöktjänst och 3 repliker i en grundläggande söktjänst. Repliker kan justeras antingen från hello [Azure-portalen](search-create-service-portal.md) eller [PowerShell](search-manage-powershell.md).
2. **Öka Sök nivå:** Azure Search är en [antal nivåer](https://azure.microsoft.com/pricing/details/search/) och var och en av de här nivåerna ger olika nivåer av prestanda.  I vissa fall kanske du så många frågor hello nivån på inte kan tillhandahålla tillräckligt låg latens priser, även när repliker är överutnyttjade ut.  I det här fallet kan du tooconsider utnyttjar en hello senare Sök nivåer, till exempel hello Azure Search S3 nivå som passar bra för scenarier med stora mängder dokument och mycket hög frågan arbetsbelastningar.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Skala Azure Search för långsam enskilda frågor
En annan orsak varför latens priser kan vara långsam är från en enskild fråga tar för lång toocomplete.  I det här fallet förbättrar lägger till repliker inte svarstiden priser.  För det här fallet är finns det två alternativ:

1. **Öka partitioner** en partition är en mekanism för att dela data mellan extra resurser.  Därför när du lägger till en andra partition hämtar data delas in i två.  En tredje partition delar ditt index i tre osv.  Detta har även hello effekt att i vissa fall långsam frågor utför snabbare på grund av toohello parallellisering för beräkning.  Det finns några exempel på där vi sett att detta parallellisering fungerar mycket bra med frågor som har låg selektivitet frågor.  Det här består av frågor som matchar många dokument eller när faceting måste tooprovide räknar över stora mängder dokument.  Eftersom det finns många beräkning behövs tooscore hello relevans hello dokument eller toocount hello antal dokument, lägga till extra partitioner kan hjälpa tooprovide ytterligare beräkning.  
   
   Det kan finnas maximalt 12 partitioner i standardsöktjänst och 1 partition i hello grundläggande search-tjänsten.  Partitioner kan justeras antingen från hello [Azure-portalen](search-create-service-portal.md) eller [PowerShell](search-manage-powershell.md).
2. **Gränsen för hög kardinalitet fält:** en hög kardinalitet fältet består av en facetable eller filtrera fält som har ett stort antal unika värden och har därför mycket resurser toocompute resultat än.   Exempelvis blir anger ett produkt-ID eller beskrivning fält som facetable filtrera för hög kardinalitet eftersom de flesta hello värdena från dokumentet toodocument är unika. Om möjligt begränsa hello antalet hög kardinalitet fält.
3. **Öka Sök nivå:** Flytta upp tooa högre nivå för Azure Search kan vara ett annat sätt tooimprove prestanda för långsamma frågor.  Varje högre nivå ger också snabbare processor och minne som kan ha en positiv inverkan på prestanda för frågor.

## <a name="scaling-for-availability"></a>Skalning för tillgänglighet
Repliker inte bara minska svarstid men kan också tillåta för hög tillgänglighet.  Med en enskild replik du förväntar dig periodiska driftstopp på grund av tooserver omstarter efter programuppdateringar eller för andra underhållshändelser som inträffar.  Därför är det viktigt tooconsider om ditt program kräver hög tillgänglighet för sökningar (frågor) samt skrivningar (indexering händelser).  Azure Search erbjuder SLA alternativ på alla hello betald Sök erbjudanden med hello följande attribut:

* 2 repliker för hög tillgänglighet för skrivskyddade arbetsbelastningar (frågor)
* 3 eller fler repliker för hög tillgänglighet i läsa / skriva-arbetsbelastningar (frågor och indexering)

Mer information om detta finns hello [servicenivåavtal för Azure Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Eftersom repliker är kopior av dina data med flera repliker kan Azure Search toodo datorn startas om och underhåll mot en replik i taget samtidigt som frågor toocontinue toobe körs mot hello repliker.  Därför måste du också tooconsider hur den här driftstörningen kan påverka hello frågor som nu har toobe körs mot en mindre kopia av hello data.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Skalning fördelade arbetsbelastningar och ge geo-redundans
Du hittar att användare finns är långt från hello datacenter där är värd för Azure Search-tjänsten har högre latens priser för fördelade arbetsbelastningar.  Därför är det ofta viktiga toohave flera search-tjänster i områden som är närmare närhet toothese användare.  Azure Search innehåller för närvarande inte en automatiserad metod för geo-replikering Azure Search index över regioner, men det finns några metoder som kan användas som kan göra den här processen enkel tooimplement och hantera. Dessa beskrivs i hello följande avsnitten.

hello målet en uppsättning fördelade search-tjänster är toohave två eller fler index som är tillgängliga i två eller flera regioner där en användare kommer att dirigeras toohello Azure Search-tjänst som tillhandahåller hello lägsta svarstid som visas i det här exemplet:

   ![Cross-fliken i tjänster efter region][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Synkronisera data över flera Azure Search-tjänster
Det finns två alternativ för att hålla dina distribuerade söktjänster synkroniserade som består av antingen med hjälp av hello [Azure sökindexeraren](search-indexer-overview.md) eller hello Push-API (kallas även tooas hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="azure-search-indexers"></a>Azure Search indexerare
Om du använder hello Azure sökindexeraren importerar du redan dataändringar från en central datalagret, till exempel Azure SQL DB- eller Azure Cosmos DB. När du skapar en ny sökning tjänsten du helt enkelt också skapa en ny Azure-sökindexeraren för tjänsten som punkter toothis samma datalager. På så sätt, när nya ändringar träda i hello datalagret, kommer de att hämtas indexeras av hello olika indexerare.  

Här är ett exempel på hur den arkitekturen skulle se ut.

   ![Enskild datakälla med tjänsten kombinationer och distribuerade indexeraren][2]

### <a name="push-api"></a>Push-API
Om du använder hello Azure Search Push-API för[uppdaterar innehållet i ditt Azure Search index](https://docs.microsoft.com/rest/api/searchservice/update-index), du kan synkronisera din olika search-tjänster genom att överföra ändringar tooall search-tjänster när en uppdatering krävs.  När detta är det viktigt toomake att toohandle fall där en uppdatering tooone search-tjänsten misslyckas och en eller flera uppdateringar lyckas.

## <a name="leveraging-azure-traffic-manager"></a>Utnyttja Azure Traffic Manager
[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) kan du tooroute begäranden toomultiple geo finns webbplatser som sedan backas upp av flera Azure Search-tjänster.  En fördel med hello Traffic Manager är att den avsökning Azure Search tooensure att den är tillgänglig och dirigera användarna tooalternate search-tjänster i hello händelse driftstopp.  Dessutom, om du routning search-begäranden via Azure-webbplatser, kan Azure Traffic Manager du tooload saldo fall där hello webbplatsen är igång, men inte Azure Search.  Här är ett exempel på vilka hello-arkitektur som utnyttjar Traffic Manager.

   ![Cross-fliken i tjänster efter region, med central Traffic Manager][3]

## <a name="monitoring-performance"></a>Övervaka prestanda
Azure Search erbjuder hello möjlighet tooanalyze och övervaka hello prestandan för din tjänst via [Sök trafik Analytics (STA)](search-traffic-analytics.md). Via STA, kan du eventuellt logga hello enskilda sökningar samt aggregerade mått tooan Azure Storage-konto som sedan kan bearbetas för analys eller visualiseras i Power BI.  Med hjälp av STA mätvärden, kan du granska prestandastatistik, till exempel Genomsnittligt antal frågor eller svarstider för frågan.  Dessutom kan hello loggning du toodrill detaljer om specifika sökningar.

STA är ett ovärderligt verktyg toounderstand svarstid priser som ur Azure Search.  Eftersom hello frågan prestandamått loggas baseras på hello tid en fråga tar toobe bearbetats helt i Azure Search (från hello tiden är det begärda toowhen skickas ut), är du kan toouse denna toodetermine om problem med nätverkssvarstiden från hello Azure Search-tjänsten sida eller utanför hello tjänst, till exempel från Nätverksfördröjningen.  

## <a name="next-steps"></a>Nästa steg
toolearn mer information om priser nivåer och tjänster gränser för vart och ett hello finns [gränser i Azure Search](search-limits-quotas-capacity.md).

Besök [kapacitetsplanering](search-capacity-planning.md) toolearn mer om kombinationer av partition och repliken.

Titta på följande video hello för flera nedbrytning på prestanda och toosee vissa demonstration av hur tooimplement hello optimeringar som beskrivs i den här artikeln:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
