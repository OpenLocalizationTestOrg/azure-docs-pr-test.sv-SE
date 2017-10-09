---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Preliminär dokumentation för hello synonymer (förhandsgranskning)-funktionen som exponeras i hello Azure Search REST API."
services: search
documentationCenter: 
authors: mhko
manager: pablocas
editor: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/07/2016
ms.author: nateko
ms.openlocfilehash: 2695139d2b298fa2e7c1814715fdf96729f594ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="synonyms-in-azure-search-preview"></a><span data-ttu-id="40de1-102">Synonymer i Azure Search (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="40de1-102">Synonyms in Azure Search (preview)</span></span>

<span data-ttu-id="40de1-103">Synonymer i sökmotorer associera motsvarande termer som utökar implicit hello omfattningen av en fråga utan hello användare med tooactually ange hello termen.</span><span class="sxs-lookup"><span data-stu-id="40de1-103">Synonyms in search engines associate equivalent terms that implicitly expand hello scope of a query, without hello user having tooactually provide hello term.</span></span> <span data-ttu-id="40de1-104">Till exempel kommer angivna hello termen ”hund” och synonymen associationer i ”hunddjur” och ”Hundvalp”, alla dokument som innehåller ”hund”, ”hunddjur” eller ”Hundvalp” ligga inom hello omfånget för hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="40de1-104">For example, given hello term "dog" and synonym associations of "canine" and "puppy", any documents containing "dog", "canine" or "puppy" will fall within hello scope of hello query.</span></span>

<span data-ttu-id="40de1-105">I Azure Search görs synonymen expansion när databasfrågan.</span><span class="sxs-lookup"><span data-stu-id="40de1-105">In Azure Search, synonym expansion is done at query time.</span></span> <span data-ttu-id="40de1-106">Du kan lägga till synonymen maps tooa tjänsten med inga avbrott tooexisting åtgärder.</span><span class="sxs-lookup"><span data-stu-id="40de1-106">You can add synonym maps tooa service with no disruption tooexisting operations.</span></span> <span data-ttu-id="40de1-107">Du kan lägga till en **synonymMaps** egenskapsdefinition tooa fält utan toorebuild hello index.</span><span class="sxs-lookup"><span data-stu-id="40de1-107">You can add a  **synonymMaps** property tooa field definition without having toorebuild hello index.</span></span> <span data-ttu-id="40de1-108">Mer information finns i [uppdatera Index](https://docs.microsoft.com/rest/api/searchservice/update-index).</span><span class="sxs-lookup"><span data-stu-id="40de1-108">For more information, see [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index).</span></span>

## <a name="feature-availability"></a><span data-ttu-id="40de1-109">Funktionstillgänglighet</span><span class="sxs-lookup"><span data-stu-id="40de1-109">Feature availability</span></span>

<span data-ttu-id="40de1-110">hello synonymer funktionen är för närvarande i Förhandsgranska och stöds i endast hello senaste api-förhandsversionen (api-version = 2016-09-01-Preview).</span><span class="sxs-lookup"><span data-stu-id="40de1-110">hello synonyms feature is currently in preview and only supported in hello latest preview api-version (api-version=2016-09-01-Preview).</span></span> <span data-ttu-id="40de1-111">Funktionen stöds för närvarande inte på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="40de1-111">There is no Azure portal support at this time.</span></span> <span data-ttu-id="40de1-112">Eftersom hello API-version anges i hello begäran, är det möjligt toocombine allmänt tillgänglig (GA) och förhandsgranska API: er i hello samma app.</span><span class="sxs-lookup"><span data-stu-id="40de1-112">Because hello API version is specified on hello request, it's possible toocombine generally available (GA) and preview APIs in hello same app.</span></span> <span data-ttu-id="40de1-113">Dock kan preview API: er inte är under SLA och funktioner ändras, så vi inte rekommenderar att använda dem i program i produktion.</span><span class="sxs-lookup"><span data-stu-id="40de1-113">However, preview APIs are not under SLA and features may change, so we do not recommend using them in production applications.</span></span>

## <a name="how-toouse-synonyms-in-azure-search"></a><span data-ttu-id="40de1-114">Hur söka toouse synonymer i Azure</span><span class="sxs-lookup"><span data-stu-id="40de1-114">How toouse synonyms in Azure search</span></span>

<span data-ttu-id="40de1-115">I Azure Search baseras synonymen stöd på synonymen maps du definierar och ladda upp tooyour service.</span><span class="sxs-lookup"><span data-stu-id="40de1-115">In Azure Search, synonym support is based on synonym maps that you define and upload tooyour service.</span></span> <span data-ttu-id="40de1-116">Dessa mappningar utgör en oberoende resurs (till exempel index eller datakällor) och kan användas av alla sökbara fält i ett index i din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="40de1-116">These maps constitute an independent resource (like indexes or data sources), and can be used by any searchable field in any index in your search service.</span></span>

<span data-ttu-id="40de1-117">Synonymen mappar och index hanteras oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="40de1-117">Synonym maps and indexes are maintained independently.</span></span> <span data-ttu-id="40de1-118">När du definierar en synonym karta och överföra den tooyour service kan du kan aktivera hello synonymen funktionen på ett fält genom att lägga till den nya egenskapen **synonymMaps** i hello fältdefinition.</span><span class="sxs-lookup"><span data-stu-id="40de1-118">Once you define a synonym map and upload it tooyour service, you can enable hello synonym feature on a field by adding a new property called **synonymMaps** in hello field definition.</span></span> <span data-ttu-id="40de1-119">Skapa, uppdatera och ta bort en synonym karta är alltid en åtgärd för hela dokumentet, vilket innebär att du inte kan skapa, uppdatera eller ta bort delar av hello synonymen kartan inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="40de1-119">Creating, updating, and deleting a synonym map is always a whole-document operation, meaning that you cannot create, update or delete parts of hello synonym map incrementally.</span></span> <span data-ttu-id="40de1-120">Uppdatering av även en enda post kräver en Läs in igen.</span><span class="sxs-lookup"><span data-stu-id="40de1-120">Updating even a single entry requires a reload.</span></span>

<span data-ttu-id="40de1-121">Integrera synonymer i tillämpningsprogrammet search är en tvåstegsprocess:</span><span class="sxs-lookup"><span data-stu-id="40de1-121">Incorporating synonyms into your search application is a two-step process:</span></span>

1.  <span data-ttu-id="40de1-122">Lägg till en synonym kartan tooyour search-tjänsten via hello API: er nedan.</span><span class="sxs-lookup"><span data-stu-id="40de1-122">Add a synonym map tooyour search service through hello APIs below.</span></span>  

2.  <span data-ttu-id="40de1-123">Konfigurera en sökbar toouse hello synonymen mappning i hello indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="40de1-123">Configure a searchable field toouse hello synonym map in hello index definition.</span></span>

### <a name="synonymmaps-resource-apis"></a><span data-ttu-id="40de1-124">SynonymMaps Resource API: er</span><span class="sxs-lookup"><span data-stu-id="40de1-124">SynonymMaps Resource APIs</span></span>

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a><span data-ttu-id="40de1-125">Lägga till eller uppdatera en synonym karta under din tjänst, med hjälp av bokför eller PLACERA.</span><span class="sxs-lookup"><span data-stu-id="40de1-125">Add or update a synonym map under your service, using POST or PUT.</span></span>

<span data-ttu-id="40de1-126">Synonymen maps överförs toohello tjänsten via POST eller PUT.</span><span class="sxs-lookup"><span data-stu-id="40de1-126">Synonym maps are uploaded toohello service via POST or PUT.</span></span> <span data-ttu-id="40de1-127">Varje regel måste avgränsas med hello radbrytning (\n).</span><span class="sxs-lookup"><span data-stu-id="40de1-127">Each rule must be delimited by hello new line character ('\n').</span></span> <span data-ttu-id="40de1-128">Du kan definiera too5, 000 regler per synonymen kartan i en kostnadsfri tjänst och 10 000 regler i alla andra SKU: er.</span><span class="sxs-lookup"><span data-stu-id="40de1-128">You can define up too5,000 rules per synonym map in a free service and 10,000 rules in all other SKUs.</span></span> <span data-ttu-id="40de1-129">Varje regel kan ha upp too20 expansion.</span><span class="sxs-lookup"><span data-stu-id="40de1-129">Each rule can have up too20 expansions.</span></span>

<span data-ttu-id="40de1-130">I den här förhandsgranskningen måste synonymen maps ha formatet hello Apache Solr som förklaras nedan.</span><span class="sxs-lookup"><span data-stu-id="40de1-130">In this preview, synonym maps must be in hello Apache Solr format which is explained below.</span></span> <span data-ttu-id="40de1-131">Om du har en befintlig synonymen ordlista i ett annat format och vill toouse den direkt, meddela oss på [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="40de1-131">If you have an existing synonym dictionary in a different format and want toouse it directly, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

<span data-ttu-id="40de1-132">Du kan skapa en ny synonymen karta med hjälp av HTTP POST, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="40de1-132">You can create a new synonym map using HTTP POST, as in hello following example:</span></span>

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

<span data-ttu-id="40de1-133">Du kan också använda PUT och ange hello synonymen mappningsnamn på hello URI.</span><span class="sxs-lookup"><span data-stu-id="40de1-133">Alternatively, you can use PUT and specify hello synonym map name on hello URI.</span></span> <span data-ttu-id="40de1-134">Om hello synonymen mappningen inte finns, kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="40de1-134">If hello synonym map does not exist, it will be created.</span></span>

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a><span data-ttu-id="40de1-135">Apache Solr synonymen format</span><span class="sxs-lookup"><span data-stu-id="40de1-135">Apache Solr synonym format</span></span>

<span data-ttu-id="40de1-136">Hej Solr format stöder motsvarande och explicit synonymen mappningar.</span><span class="sxs-lookup"><span data-stu-id="40de1-136">hello Solr format supports equivalent and explicit synonym mappings.</span></span> <span data-ttu-id="40de1-137">Mappningsregler följa toohello öppen källkod synonymen filter specificering av Apache Solr, beskrivs i detta dokument: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span><span class="sxs-lookup"><span data-stu-id="40de1-137">Mapping rules adhere toohello open source synonym filter specification of Apache Solr, described in this document: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span></span> <span data-ttu-id="40de1-138">Nedan finns en Exempelregel för motsvarande synonymer.</span><span class="sxs-lookup"><span data-stu-id="40de1-138">Below is a sample rule for equivalent synonyms.</span></span>
```
              USA, United States, United States of America
```

<span data-ttu-id="40de1-139">Med hello regel ovan, utökar en sökning frågan ”USA” för ”USA” eller ”USA” eller ”USA”.</span><span class="sxs-lookup"><span data-stu-id="40de1-139">With hello rule above, a search query "USA" will expand too"USA" OR "United States" OR "United States of America".</span></span>

<span data-ttu-id="40de1-140">Explicit mappning markeras med en pil ”= >”.</span><span class="sxs-lookup"><span data-stu-id="40de1-140">Explicit mapping is denoted by an arrow "=>".</span></span> <span data-ttu-id="40de1-141">Om du angett en term sekvens med en sökfråga som matchar hello vänster sida av ”= >” ersätts med hello alternativ hello höger.</span><span class="sxs-lookup"><span data-stu-id="40de1-141">When specified, a term sequence of a search query that matches hello left hand side of "=>" will be replaced with hello alternatives on hello right hand side.</span></span> <span data-ttu-id="40de1-142">Angivna hello regel nedan sökfrågor ”Washington”, ”Wash.”</span><span class="sxs-lookup"><span data-stu-id="40de1-142">Given hello rule below, search queries "Washington", "Wash."</span></span> <span data-ttu-id="40de1-143">eller ”WA” kommer alla skrivas för ”WA”.</span><span class="sxs-lookup"><span data-stu-id="40de1-143">or "WA" will all be rewritten too"WA".</span></span> <span data-ttu-id="40de1-144">Explicit mappning endast gäller i hello riktningen som anges och inte omarbetning hello frågan ”WA” för ”Washington” i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="40de1-144">Explicit mapping only applies in hello direction specified and does not rewrite hello query "WA" too"Washington" in this case.</span></span>
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a><span data-ttu-id="40de1-145">Lista synonymen mappar under din tjänst.</span><span class="sxs-lookup"><span data-stu-id="40de1-145">List synonym maps under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a><span data-ttu-id="40de1-146">Få en synonym karta under din tjänst.</span><span class="sxs-lookup"><span data-stu-id="40de1-146">Get a synonym map under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a><span data-ttu-id="40de1-147">Ta bort en synonymer karta under din tjänst.</span><span class="sxs-lookup"><span data-stu-id="40de1-147">Delete a synonyms map under your service.</span></span>

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-toouse-hello-synonym-map-in-hello-index-definition"></a><span data-ttu-id="40de1-148">Konfigurera en sökbar toouse hello synonymen mappning i hello indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="40de1-148">Configure a searchable field toouse hello synonym map in hello index definition.</span></span>

<span data-ttu-id="40de1-149">En ny fältegenskap **synonymMaps** kan vara används toospecify en synonym kartan toouse för ett sökbara fält.</span><span class="sxs-lookup"><span data-stu-id="40de1-149">A new field property **synonymMaps** can be used toospecify a synonym map toouse for a searchable field.</span></span> <span data-ttu-id="40de1-150">Synonymen maps är tjänsten Utjämna resurser och kan refereras till av varje fält i ett index under hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="40de1-150">Synonym maps are service level resources and can be referenced by any field of an index under hello service.</span></span>

    POST https://[servicename].search.windows.net/indexes?api-version=2016-09-01-Preview
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

<span data-ttu-id="40de1-151">**synonymMaps** kan anges för sökbara fält av hello typ 'Edm.String' eller 'Collection(Edm.String)'.</span><span class="sxs-lookup"><span data-stu-id="40de1-151">**synonymMaps** can be specified for searchable fields of hello type 'Edm.String' or 'Collection(Edm.String)'.</span></span>

> [!NOTE]
> <span data-ttu-id="40de1-152">Du kan bara ha en synonym mappa per fält i den här förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="40de1-152">In this preview, you can only have one synonym map per field.</span></span> <span data-ttu-id="40de1-153">Om du vill toouse flera synonymen maps, meddela oss på [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="40de1-153">If you want toouse multiple synonym maps, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

## <a name="impact-of-synonyms-on-other-search-features"></a><span data-ttu-id="40de1-154">Effekten av synonymer på andra sökfunktioner</span><span class="sxs-lookup"><span data-stu-id="40de1-154">Impact of synonyms on other search features</span></span>

<span data-ttu-id="40de1-155">hello synonymer funktionen skriver om hello ursprungliga frågan med synonymer med hello OR-operator.</span><span class="sxs-lookup"><span data-stu-id="40de1-155">hello synonyms feature rewrites hello original query with synonyms with hello OR operator.</span></span> <span data-ttu-id="40de1-156">Därför träffar syntaxmarkering och bedömningen profiler hello ursprungliga termen och behandlas synonymer som likvärdiga.</span><span class="sxs-lookup"><span data-stu-id="40de1-156">For this reason, hit highlighting and scoring profiles treat hello original term and synonyms as equivalent.</span></span>

<span data-ttu-id="40de1-157">Synonymen funktion gäller toosearch frågor och gäller inte toofilters eller facets.</span><span class="sxs-lookup"><span data-stu-id="40de1-157">Synonym feature applies toosearch queries and does not apply toofilters or facets.</span></span> <span data-ttu-id="40de1-158">På liknande sätt baseras förslag endast på hello ursprungliga sikt. synonymen matchar visas inte i hello svar.</span><span class="sxs-lookup"><span data-stu-id="40de1-158">Similarly, suggestions are based only on hello original term; synonym matches do not appear in hello response.</span></span>

<span data-ttu-id="40de1-159">Synonymen expansion gäller inte toowildcard söktermer; prefix, fuzzy, och villkor för regex är inte expanderas.</span><span class="sxs-lookup"><span data-stu-id="40de1-159">Synonym expansions do not apply toowildcard search terms; prefix, fuzzy, and regex terms aren't expanded.</span></span>

## <a name="tips-for-building-a-synonym-map"></a><span data-ttu-id="40de1-160">Tips för att skapa en synonym karta</span><span class="sxs-lookup"><span data-stu-id="40de1-160">Tips for building a synonym map</span></span>

- <span data-ttu-id="40de1-161">En kortfattad, väl utformad synonymen karta är effektivare än en komplett lista över möjliga matchningar.</span><span class="sxs-lookup"><span data-stu-id="40de1-161">A concise, well-designed synonym map is more efficient than an exhaustive list of possible matches.</span></span> <span data-ttu-id="40de1-162">Alltför stora eller komplexa ordlistor ta längre tooparse och påverka hello svarstid om hello frågan utökas toomany synonymer.</span><span class="sxs-lookup"><span data-stu-id="40de1-162">Excessively large or complex dictionaries take longer tooparse and affect hello query latency if hello query expands toomany synonyms.</span></span> <span data-ttu-id="40de1-163">I stället för att gissa då villkoren kan användas, kan du hello faktiska villkoren via en [söka trafik analysrapporten](search-traffic-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="40de1-163">Rather than guess at which terms might be used, you can get hello actual terms via a [search traffic analysis report](search-traffic-analytics.md).</span></span>

- <span data-ttu-id="40de1-164">Som en preliminär och validering övningen aktivera och sedan använda den här rapporten tooprecisely bestämma vilka villkor ska dra nytta av en synonym matchning och fortsätt sedan toouse den som verifiering synonymen kartan ger ett bättre resultat.</span><span class="sxs-lookup"><span data-stu-id="40de1-164">As both a preliminary and validation exercise, enable and then use this report tooprecisely determine which terms will benefit from a synonym match, and then continue toouse it as validation that your synonym map is producing a better outcome.</span></span> <span data-ttu-id="40de1-165">I hello fördefinierade rapporten hello panelerna ”vanligaste sökfrågor” och ”noll resultatet sökfrågor” ger dig hello nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="40de1-165">In hello predefined report, hello tiles "Most common search queries" and "Zero-result search queries" will give you hello necessary information.</span></span>

- <span data-ttu-id="40de1-166">Du kan skapa flera synonymen mappningar för search-program (till exempel efter språk om programmet stöder en flerspråkig kundbas).</span><span class="sxs-lookup"><span data-stu-id="40de1-166">You can create multiple synonym maps for your search application (for example, by language if your application supports a multi-lingual customer base).</span></span> <span data-ttu-id="40de1-167">Ett fält kan för närvarande kan bara använda en av dem.</span><span class="sxs-lookup"><span data-stu-id="40de1-167">Currently, a field can only use one of them.</span></span> <span data-ttu-id="40de1-168">Du kan uppdatera synonymMaps-egenskapen för ett fält när som helst.</span><span class="sxs-lookup"><span data-stu-id="40de1-168">You can update a field's synonymMaps property at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="40de1-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="40de1-169">Next Steps</span></span>

- <span data-ttu-id="40de1-170">Om du har ett befintligt index i en utvecklingsmiljö (icke-produktion) experimentera med en liten ordlista toosee hur hello lägga till synonymer ändras hello sökinställningar, inklusive påverkan på bedömningen profiler, träffar syntaxmarkering och förslag.</span><span class="sxs-lookup"><span data-stu-id="40de1-170">If you have an existing index in a development (non-production) environment, experiment with a small dictionary toosee how hello addition of synonyms changes hello search experience, including impact on scoring profiles, hit highlighting, and suggestions.</span></span>

- <span data-ttu-id="40de1-171">[Aktivera sökning trafik analytics](search-traffic-analytics.md) och Använd hello fördefinierade Power BI-rapport toolearn vilka villkor används i de flesta och som de som returnerar hello noll dokument.</span><span class="sxs-lookup"><span data-stu-id="40de1-171">[Enable search traffic analytics](search-traffic-analytics.md) and use hello predefined Power BI report toolearn which terms are used hello most, and which ones return zero documents.</span></span> <span data-ttu-id="40de1-172">Tillsammans med dessa insikter, revidera hello ordlista tooinclude synonymer för improduktiva frågor som ska matchas toodocuments i ditt index.</span><span class="sxs-lookup"><span data-stu-id="40de1-172">Armed with these insights, revise hello dictionary tooinclude synonyms for unproductive queries that should be resolving toodocuments in your index.</span></span>
