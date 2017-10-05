---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Preliminär dokumentation för funktionen synonymer (förhandsgranskning), visas i Azure Search REST API."
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
ms.openlocfilehash: 739a0ad77c68ea74ec25bc80c7539ac8b3f18201
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="synonyms-in-azure-search-preview"></a><span data-ttu-id="edcac-102">Synonymer i Azure Search (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="edcac-102">Synonyms in Azure Search (preview)</span></span>

<span data-ttu-id="edcac-103">Synonymer i sökmotorer associera motsvarande termer som implicit expanderar omfattningen av en fråga, utan att användaren behöver ange faktiskt termen.</span><span class="sxs-lookup"><span data-stu-id="edcac-103">Synonyms in search engines associate equivalent terms that implicitly expand the scope of a query, without the user having to actually provide the term.</span></span> <span data-ttu-id="edcac-104">Till exempel ska angivna termen ”hund” och synonymen kopplingarna ”hunddjur” och ”Hundvalp” alla dokument som innehåller ”hund”, ”hunddjur” eller ”Hundvalp” omfattas av frågan.</span><span class="sxs-lookup"><span data-stu-id="edcac-104">For example, given the term "dog" and synonym associations of "canine" and "puppy", any documents containing "dog", "canine" or "puppy" will fall within the scope of the query.</span></span>

<span data-ttu-id="edcac-105">I Azure Search görs synonymen expansion när databasfrågan.</span><span class="sxs-lookup"><span data-stu-id="edcac-105">In Azure Search, synonym expansion is done at query time.</span></span> <span data-ttu-id="edcac-106">Du kan lägga till synonymen mappar till en tjänst utan några avbrott befintliga drift.</span><span class="sxs-lookup"><span data-stu-id="edcac-106">You can add synonym maps to a service with no disruption to existing operations.</span></span> <span data-ttu-id="edcac-107">Du kan lägga till en **synonymMaps** egenskapen till en fältdefinition av utan att behöva återskapa indexet.</span><span class="sxs-lookup"><span data-stu-id="edcac-107">You can add a  **synonymMaps** property to a field definition without having to rebuild the index.</span></span> <span data-ttu-id="edcac-108">Mer information finns i [uppdatera Index](https://docs.microsoft.com/rest/api/searchservice/update-index).</span><span class="sxs-lookup"><span data-stu-id="edcac-108">For more information, see [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index).</span></span>

## <a name="feature-availability"></a><span data-ttu-id="edcac-109">Funktionstillgänglighet</span><span class="sxs-lookup"><span data-stu-id="edcac-109">Feature availability</span></span>

<span data-ttu-id="edcac-110">Funktionen synonymer är för närvarande under förhandsgranskning och stöds bara i den senaste api-förhandsversionen (api-version = 2016-09-01-Preview).</span><span class="sxs-lookup"><span data-stu-id="edcac-110">The synonyms feature is currently in preview and only supported in the latest preview api-version (api-version=2016-09-01-Preview).</span></span> <span data-ttu-id="edcac-111">Funktionen stöds för närvarande inte på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="edcac-111">There is no Azure portal support at this time.</span></span> <span data-ttu-id="edcac-112">Eftersom API-version anges i begäran, är det möjligt att kombinera allmänt tillgänglig (GA) och förhandsgranska API: er i samma app.</span><span class="sxs-lookup"><span data-stu-id="edcac-112">Because the API version is specified on the request, it's possible to combine generally available (GA) and preview APIs in the same app.</span></span> <span data-ttu-id="edcac-113">Dock kan preview API: er inte är under SLA och funktioner ändras, så vi inte rekommenderar att använda dem i program i produktion.</span><span class="sxs-lookup"><span data-stu-id="edcac-113">However, preview APIs are not under SLA and features may change, so we do not recommend using them in production applications.</span></span>

## <a name="how-to-use-synonyms-in-azure-search"></a><span data-ttu-id="edcac-114">Hur du använder synonymer i Azure search</span><span class="sxs-lookup"><span data-stu-id="edcac-114">How to use synonyms in Azure search</span></span>

<span data-ttu-id="edcac-115">I Azure Search baseras synonymen stöd på synonymen mappningar som du definierar och ladda upp till din tjänst.</span><span class="sxs-lookup"><span data-stu-id="edcac-115">In Azure Search, synonym support is based on synonym maps that you define and upload to your service.</span></span> <span data-ttu-id="edcac-116">Dessa mappningar utgör en oberoende resurs (till exempel index eller datakällor) och kan användas av alla sökbara fält i ett index i din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="edcac-116">These maps constitute an independent resource (like indexes or data sources), and can be used by any searchable field in any index in your search service.</span></span>

<span data-ttu-id="edcac-117">Synonymen mappar och index hanteras oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="edcac-117">Synonym maps and indexes are maintained independently.</span></span> <span data-ttu-id="edcac-118">När du definierar en synonym karta och överföra den till din tjänst kan du aktivera funktionen synonymen på ett fält genom att lägga till den nya egenskapen **synonymMaps** i fältdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="edcac-118">Once you define a synonym map and upload it to your service, you can enable the synonym feature on a field by adding a new property called **synonymMaps** in the field definition.</span></span> <span data-ttu-id="edcac-119">Skapa, uppdatera och ta bort en synonym karta är alltid en åtgärd för hela dokumentet, vilket innebär att du inte går att skapa, uppdatera eller ta bort delar av synonymen kartan inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="edcac-119">Creating, updating, and deleting a synonym map is always a whole-document operation, meaning that you cannot create, update or delete parts of the synonym map incrementally.</span></span> <span data-ttu-id="edcac-120">Uppdatering av även en enda post kräver en Läs in igen.</span><span class="sxs-lookup"><span data-stu-id="edcac-120">Updating even a single entry requires a reload.</span></span>

<span data-ttu-id="edcac-121">Integrera synonymer i tillämpningsprogrammet search är en tvåstegsprocess:</span><span class="sxs-lookup"><span data-stu-id="edcac-121">Incorporating synonyms into your search application is a two-step process:</span></span>

1.  <span data-ttu-id="edcac-122">Lägga till en synonym karta till din söktjänst via API: er nedan.</span><span class="sxs-lookup"><span data-stu-id="edcac-122">Add a synonym map to your search service through the APIs below.</span></span>  

2.  <span data-ttu-id="edcac-123">Konfigurera ett sökbara fält om du vill använda synonymen kartan i indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="edcac-123">Configure a searchable field to use the synonym map in the index definition.</span></span>

### <a name="synonymmaps-resource-apis"></a><span data-ttu-id="edcac-124">SynonymMaps Resource API: er</span><span class="sxs-lookup"><span data-stu-id="edcac-124">SynonymMaps Resource APIs</span></span>

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a><span data-ttu-id="edcac-125">Lägga till eller uppdatera en synonym karta under din tjänst, med hjälp av bokför eller PLACERA.</span><span class="sxs-lookup"><span data-stu-id="edcac-125">Add or update a synonym map under your service, using POST or PUT.</span></span>

<span data-ttu-id="edcac-126">Synonymen maps överförs till tjänsten via POST eller PUT.</span><span class="sxs-lookup"><span data-stu-id="edcac-126">Synonym maps are uploaded to the service via POST or PUT.</span></span> <span data-ttu-id="edcac-127">Varje regel måste avgränsas med ett tecken för ny rad (\n).</span><span class="sxs-lookup"><span data-stu-id="edcac-127">Each rule must be delimited by the new line character ('\n').</span></span> <span data-ttu-id="edcac-128">Du kan ange upp till 5 000 regler per synonymen kartan i en kostnadsfri tjänst och 10 000 regler i alla andra SKU: er.</span><span class="sxs-lookup"><span data-stu-id="edcac-128">You can define up to 5,000 rules per synonym map in a free service and 10,000 rules in all other SKUs.</span></span> <span data-ttu-id="edcac-129">Varje regel kan ha upp till 20 expansion.</span><span class="sxs-lookup"><span data-stu-id="edcac-129">Each rule can have up to 20 expansions.</span></span>

<span data-ttu-id="edcac-130">I den här förhandsgranskningen måste synonymen mappningar vara i formatet Apache Solr som förklaras nedan.</span><span class="sxs-lookup"><span data-stu-id="edcac-130">In this preview, synonym maps must be in the Apache Solr format which is explained below.</span></span> <span data-ttu-id="edcac-131">Om du har en befintlig synonymen ordlista i ett annat format och vill använda den direkt, meddela oss på [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="edcac-131">If you have an existing synonym dictionary in a different format and want to use it directly, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

<span data-ttu-id="edcac-132">Du kan skapa en ny synonymen karta med hjälp av HTTP POST, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="edcac-132">You can create a new synonym map using HTTP POST, as in the following example:</span></span>

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

<span data-ttu-id="edcac-133">Du kan också använda PUT och ange mappningsnamn synonymen på URI: N.</span><span class="sxs-lookup"><span data-stu-id="edcac-133">Alternatively, you can use PUT and specify the synonym map name on the URI.</span></span> <span data-ttu-id="edcac-134">Om synonymen mappningen inte finns, kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="edcac-134">If the synonym map does not exist, it will be created.</span></span>

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a><span data-ttu-id="edcac-135">Apache Solr synonymen format</span><span class="sxs-lookup"><span data-stu-id="edcac-135">Apache Solr synonym format</span></span>

<span data-ttu-id="edcac-136">Solr format stöder motsvarande och explicit synonymen mappningar.</span><span class="sxs-lookup"><span data-stu-id="edcac-136">The Solr format supports equivalent and explicit synonym mappings.</span></span> <span data-ttu-id="edcac-137">Mappningsregler följa öppen källkod synonymen filter specificering av Apache Solr, beskrivs i detta dokument: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span><span class="sxs-lookup"><span data-stu-id="edcac-137">Mapping rules adhere to the open source synonym filter specification of Apache Solr, described in this document: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span></span> <span data-ttu-id="edcac-138">Nedan finns en Exempelregel för motsvarande synonymer.</span><span class="sxs-lookup"><span data-stu-id="edcac-138">Below is a sample rule for equivalent synonyms.</span></span>
```
              USA, United States, United States of America
```

<span data-ttu-id="edcac-139">Med regeln ovan, en sökfråga utökas ”USA” till ”USA” eller ”USA” eller ”USA”.</span><span class="sxs-lookup"><span data-stu-id="edcac-139">With the rule above, a search query "USA" will expand to "USA" OR "United States" OR "United States of America".</span></span>

<span data-ttu-id="edcac-140">Explicit mappning markeras med en pil ”= >”.</span><span class="sxs-lookup"><span data-stu-id="edcac-140">Explicit mapping is denoted by an arrow "=>".</span></span> <span data-ttu-id="edcac-141">Om du angett en term sekvens med en sökfråga som matchar den vänstra sidan av ”= >” ersätts med alternativen på höger sida.</span><span class="sxs-lookup"><span data-stu-id="edcac-141">When specified, a term sequence of a search query that matches the left hand side of "=>" will be replaced with the alternatives on the right hand side.</span></span> <span data-ttu-id="edcac-142">Angivna regel nedan sökfrågor ”Washington”, ”Wash.”</span><span class="sxs-lookup"><span data-stu-id="edcac-142">Given the rule below, search queries "Washington", "Wash."</span></span> <span data-ttu-id="edcac-143">eller ”WA” kommer alla skrivas till ”WA”.</span><span class="sxs-lookup"><span data-stu-id="edcac-143">or "WA" will all be rewritten to "WA".</span></span> <span data-ttu-id="edcac-144">Explicit mappning endast gäller i riktningen som anges och skriv om inte frågan ”WA” till ”Washington” i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="edcac-144">Explicit mapping only applies in the direction specified and does not rewrite the query "WA" to "Washington" in this case.</span></span>
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a><span data-ttu-id="edcac-145">Lista synonymen mappar under din tjänst.</span><span class="sxs-lookup"><span data-stu-id="edcac-145">List synonym maps under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a><span data-ttu-id="edcac-146">Få en synonym karta under din tjänst.</span><span class="sxs-lookup"><span data-stu-id="edcac-146">Get a synonym map under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a><span data-ttu-id="edcac-147">Ta bort en synonymer karta under din tjänst.</span><span class="sxs-lookup"><span data-stu-id="edcac-147">Delete a synonyms map under your service.</span></span>

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a><span data-ttu-id="edcac-148">Konfigurera ett sökbara fält om du vill använda synonymen kartan i indexdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="edcac-148">Configure a searchable field to use the synonym map in the index definition.</span></span>

<span data-ttu-id="edcac-149">En ny fältegenskap **synonymMaps** kan användas för att ange en synonym karta ska användas i en sökbara fält.</span><span class="sxs-lookup"><span data-stu-id="edcac-149">A new field property **synonymMaps** can be used to specify a synonym map to use for a searchable field.</span></span> <span data-ttu-id="edcac-150">Synonymen maps är tjänsten Utjämna resurser och kan refereras till av varje fält i ett index under tjänsten.</span><span class="sxs-lookup"><span data-stu-id="edcac-150">Synonym maps are service level resources and can be referenced by any field of an index under the service.</span></span>

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

<span data-ttu-id="edcac-151">**synonymMaps** kan anges för sökbara fält av typen 'Edm.String' eller 'Collection(Edm.String)'.</span><span class="sxs-lookup"><span data-stu-id="edcac-151">**synonymMaps** can be specified for searchable fields of the type 'Edm.String' or 'Collection(Edm.String)'.</span></span>

> [!NOTE]
> <span data-ttu-id="edcac-152">Du kan bara ha en synonym mappa per fält i den här förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="edcac-152">In this preview, you can only have one synonym map per field.</span></span> <span data-ttu-id="edcac-153">Om du vill använda flera synonymen maps berätta på [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="edcac-153">If you want to use multiple synonym maps, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

## <a name="impact-of-synonyms-on-other-search-features"></a><span data-ttu-id="edcac-154">Effekten av synonymer på andra sökfunktioner</span><span class="sxs-lookup"><span data-stu-id="edcac-154">Impact of synonyms on other search features</span></span>

<span data-ttu-id="edcac-155">Funktionen synonymer skrivs den ursprungliga frågan med synonymer med OR-operator.</span><span class="sxs-lookup"><span data-stu-id="edcac-155">The synonyms feature rewrites the original query with synonyms with the OR operator.</span></span> <span data-ttu-id="edcac-156">Därför hanterar träffar syntaxmarkering och bedömningen profiler du de ursprungliga termen och synonymer som likvärdiga.</span><span class="sxs-lookup"><span data-stu-id="edcac-156">For this reason, hit highlighting and scoring profiles treat the original term and synonyms as equivalent.</span></span>

<span data-ttu-id="edcac-157">Synonymen funktionen gäller för att söka efter frågor och gäller inte för filter eller facets.</span><span class="sxs-lookup"><span data-stu-id="edcac-157">Synonym feature applies to search queries and does not apply to filters or facets.</span></span> <span data-ttu-id="edcac-158">På liknande sätt baseras förslag endast på den ursprungliga sikt. synonymen matchar visas inte i svaret.</span><span class="sxs-lookup"><span data-stu-id="edcac-158">Similarly, suggestions are based only on the original term; synonym matches do not appear in the response.</span></span>

<span data-ttu-id="edcac-159">Synonymen expansion gäller inte för jokertecken söktermer; prefix, fuzzy, och villkor för regex är inte expanderas.</span><span class="sxs-lookup"><span data-stu-id="edcac-159">Synonym expansions do not apply to wildcard search terms; prefix, fuzzy, and regex terms aren't expanded.</span></span>

## <a name="tips-for-building-a-synonym-map"></a><span data-ttu-id="edcac-160">Tips för att skapa en synonym karta</span><span class="sxs-lookup"><span data-stu-id="edcac-160">Tips for building a synonym map</span></span>

- <span data-ttu-id="edcac-161">En kortfattad, väl utformad synonymen karta är effektivare än en komplett lista över möjliga matchningar.</span><span class="sxs-lookup"><span data-stu-id="edcac-161">A concise, well-designed synonym map is more efficient than an exhaustive list of possible matches.</span></span> <span data-ttu-id="edcac-162">Alltför stora eller komplexa ordlistor ta längre tid att tolka och påverkar svarstiden fråga om frågan utökas till många synonymer.</span><span class="sxs-lookup"><span data-stu-id="edcac-162">Excessively large or complex dictionaries take longer to parse and affect the query latency if the query expands to many synonyms.</span></span> <span data-ttu-id="edcac-163">I stället för att gissa då villkoren kan användas, kan du faktiskt villkoren via en [söka trafik analysrapporten](search-traffic-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="edcac-163">Rather than guess at which terms might be used, you can get the actual terms via a [search traffic analysis report](search-traffic-analytics.md).</span></span>

- <span data-ttu-id="edcac-164">Som både en preliminär och validering utöva aktivera och sedan använda rapporten för att exakt avgöra vilka villkor ska dra nytta av en synonym matchning och sedan fortsätta att använda den som verifiering synonymen kartan ger ett bättre resultat.</span><span class="sxs-lookup"><span data-stu-id="edcac-164">As both a preliminary and validation exercise, enable and then use this report to precisely determine which terms will benefit from a synonym match, and then continue to use it as validation that your synonym map is producing a better outcome.</span></span> <span data-ttu-id="edcac-165">I rapporten fördefinierade ger paneler ”vanligaste sökfrågor” och ”noll resultatet sökfrågor” dig informationen som behövs.</span><span class="sxs-lookup"><span data-stu-id="edcac-165">In the predefined report, the tiles "Most common search queries" and "Zero-result search queries" will give you the necessary information.</span></span>

- <span data-ttu-id="edcac-166">Du kan skapa flera synonymen mappningar för search-program (till exempel efter språk om programmet stöder en flerspråkig kundbas).</span><span class="sxs-lookup"><span data-stu-id="edcac-166">You can create multiple synonym maps for your search application (for example, by language if your application supports a multi-lingual customer base).</span></span> <span data-ttu-id="edcac-167">Ett fält kan för närvarande kan bara använda en av dem.</span><span class="sxs-lookup"><span data-stu-id="edcac-167">Currently, a field can only use one of them.</span></span> <span data-ttu-id="edcac-168">Du kan uppdatera synonymMaps-egenskapen för ett fält när som helst.</span><span class="sxs-lookup"><span data-stu-id="edcac-168">You can update a field's synonymMaps property at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edcac-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="edcac-169">Next Steps</span></span>

- <span data-ttu-id="edcac-170">Om du har ett befintligt index i en utvecklingsmiljö (icke-produktion) kan du experimentera med en liten ordlista nådde Markera om du vill se hur för att lägga till synonymer ändras sökinställningar, inklusive påverkan på bedömningen profiler och förslag.</span><span class="sxs-lookup"><span data-stu-id="edcac-170">If you have an existing index in a development (non-production) environment, experiment with a small dictionary to see how the addition of synonyms changes the search experience, including impact on scoring profiles, hit highlighting, and suggestions.</span></span>

- <span data-ttu-id="edcac-171">[Aktivera sökning trafik analytics](search-traffic-analytics.md) och använda fördefinierade Power BI-rapport för att veta vilka villkor som används mest och vilka som returnerar noll dokument.</span><span class="sxs-lookup"><span data-stu-id="edcac-171">[Enable search traffic analytics](search-traffic-analytics.md) and use the predefined Power BI report to learn which terms are used the most, and which ones return zero documents.</span></span> <span data-ttu-id="edcac-172">Tillsammans med dessa insikter, revidera ordlistan om du vill inkludera synonymer för improduktiva frågor som ska matchas till dokument i ditt index.</span><span class="sxs-lookup"><span data-stu-id="edcac-172">Armed with these insights, revise the dictionary to include synonyms for unproductive queries that should be resolving to documents in your index.</span></span>
