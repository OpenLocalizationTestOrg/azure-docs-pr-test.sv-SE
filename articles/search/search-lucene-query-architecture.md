---
title: aaaFull text search engine (Lucene)-arkitekturen i Azure Search | Microsoft Docs
description: "Förklaring av Lucene frågan bearbetning och dokumentet hämtning begrepp för textsökning, som relaterade tooAzure sökning."
services: search
manager: jhubbard
author: yahnoosh
documentationcenter: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/06/2017
ms.author: jlembicz
ms.openlocfilehash: c6d1bea8d40154fd9237b9e44584cdfcd193cbd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a><span data-ttu-id="81ba8-103">Hur full textsökning fungerar i Azure Search</span><span class="sxs-lookup"><span data-stu-id="81ba8-103">How full text search works in Azure Search</span></span>

<span data-ttu-id="81ba8-104">Den här artikeln är för utvecklare som behöver en bättre förståelse av hur Lucene fulltextsökning fungerar i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="81ba8-104">This article is for developers who need a deeper understanding of how Lucene full text search works in Azure Search.</span></span> <span data-ttu-id="81ba8-105">Azure Search levererar sömlöst förväntat resultat i de flesta scenarier för textfrågor, men ibland kan du få ett resultat som verkar vara ”off” på något sätt.</span><span class="sxs-lookup"><span data-stu-id="81ba8-105">For text queries, Azure Search will seamlessly deliver expected results in most scenarios, but occasionally you might get a result that seems "off" somehow.</span></span> <span data-ttu-id="81ba8-106">I dessa fall har en bakgrund i hello fyra faser i Lucene Frågekörningen (fråga tolkning lexikala analys, dokumentera matchar bedömningen) kan hjälpa dig att identifiera ändringar tooquery parametrar eller index-konfiguration som levererar hello önskat utfall.</span><span class="sxs-lookup"><span data-stu-id="81ba8-106">In these situations, having a background in hello four stages of Lucene query execution (query parsing, lexical analysis, document matching, scoring) can help you identify specific changes tooquery parameters or index configuration that will deliver hello desired outcome.</span></span> 

> [!Note] 
> <span data-ttu-id="81ba8-107">Azure Search använder Lucene för textsökning men Lucene integrering är inte komplett.</span><span class="sxs-lookup"><span data-stu-id="81ba8-107">Azure Search uses Lucene for full text search, but Lucene integration is not exhaustive.</span></span> <span data-ttu-id="81ba8-108">Vi selektivt exponera och utöka Lucene funktioner tooenable hello scenarier viktiga tooAzure sökning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-108">We selectively expose and extend Lucene functionality tooenable hello scenarios important tooAzure Search.</span></span> 

## <a name="architecture-overview-and-diagram"></a><span data-ttu-id="81ba8-109">Översikt över arkitektur och diagram</span><span class="sxs-lookup"><span data-stu-id="81ba8-109">Architecture overview and diagram</span></span>

<span data-ttu-id="81ba8-110">Bearbetning av en fulltext-sökning startar med parsning hello frågan text tooextract sökvillkoren.</span><span class="sxs-lookup"><span data-stu-id="81ba8-110">Processing a full text search query starts with parsing hello query text tooextract search terms.</span></span> <span data-ttu-id="81ba8-111">hello sökmotor använder ett index tooretrieve dokument med matchande villkor.</span><span class="sxs-lookup"><span data-stu-id="81ba8-111">hello search engine uses an index tooretrieve documents with matching terms.</span></span> <span data-ttu-id="81ba8-112">Enskilda sökord uppdelade ibland och färdigställts till nya formulär toocast ett bredare net över vad kan betraktas som en möjlig matchning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-112">Individual query terms are sometimes broken down and reconstituted into new forms toocast a broader net over what could be considered as a potential match.</span></span> <span data-ttu-id="81ba8-113">Resultatet sorteras sedan efter ett relevans poäng tilldelade tooeach enskilda matchande dokument.</span><span class="sxs-lookup"><span data-stu-id="81ba8-113">A result set is then sorted by a relevance score assigned tooeach individual matching document.</span></span> <span data-ttu-id="81ba8-114">De hello överst i hello rangordnas listan returneras toohello anropande programmet.</span><span class="sxs-lookup"><span data-stu-id="81ba8-114">Those at hello top of hello ranked list are returned toohello calling application.</span></span>

<span data-ttu-id="81ba8-115">Frågekörningen har räknas, fyra steg:</span><span class="sxs-lookup"><span data-stu-id="81ba8-115">Restated, query execution has four stages:</span></span> 

1. <span data-ttu-id="81ba8-116">Frågan parsning</span><span class="sxs-lookup"><span data-stu-id="81ba8-116">Query parsing</span></span> 
2. <span data-ttu-id="81ba8-117">Lexikaliskt analys</span><span class="sxs-lookup"><span data-stu-id="81ba8-117">Lexical analysis</span></span> 
3. <span data-ttu-id="81ba8-118">Hämta dokument</span><span class="sxs-lookup"><span data-stu-id="81ba8-118">Document retrieval</span></span> 
4. <span data-ttu-id="81ba8-119">Resultatfunktioner</span><span class="sxs-lookup"><span data-stu-id="81ba8-119">Scoring</span></span> 

<span data-ttu-id="81ba8-120">hello diagrammet nedan visar hello komponenter som används tooprocess en sökbegäran.</span><span class="sxs-lookup"><span data-stu-id="81ba8-120">hello diagram below illustrates hello components used tooprocess a search request.</span></span> 

 ![Arkitekturdiagram för Lucene frågan i Azure Search][1]


| <span data-ttu-id="81ba8-122">Nyckelkomponenter</span><span class="sxs-lookup"><span data-stu-id="81ba8-122">Key components</span></span> | <span data-ttu-id="81ba8-123">Funktionsbeskrivning</span><span class="sxs-lookup"><span data-stu-id="81ba8-123">Functional description</span></span> | 
|----------------|------------------------|
|<span data-ttu-id="81ba8-124">**Frågan Parser**</span><span class="sxs-lookup"><span data-stu-id="81ba8-124">**Query parsers**</span></span> | <span data-ttu-id="81ba8-125">Separata sökord från frågeoperatorer och skapa hello frågan struktur (en fråga trädet) toobe skickas toohello sökmotor.</span><span class="sxs-lookup"><span data-stu-id="81ba8-125">Separate query terms from query operators and create hello query structure (a query tree) toobe sent toohello search engine.</span></span> |
|<span data-ttu-id="81ba8-126">**Analyzers**</span><span class="sxs-lookup"><span data-stu-id="81ba8-126">**Analyzers**</span></span> | <span data-ttu-id="81ba8-127">Analysera lexikala sökord.</span><span class="sxs-lookup"><span data-stu-id="81ba8-127">Perform lexical analysis on query terms.</span></span> <span data-ttu-id="81ba8-128">Den här processen kan omfatta Omforma, ta bort eller expandering av sökord.</span><span class="sxs-lookup"><span data-stu-id="81ba8-128">This process can involve transforming, removing, or expanding of query terms.</span></span> |
|<span data-ttu-id="81ba8-129">**Index**</span><span class="sxs-lookup"><span data-stu-id="81ba8-129">**Index**</span></span> | <span data-ttu-id="81ba8-130">En effektiv datastruktur används toostore och organisera sökbara villkoren som har extraherats från indexerade dokument.</span><span class="sxs-lookup"><span data-stu-id="81ba8-130">An efficient data structure used toostore and organize searchable terms extracted from indexed documents.</span></span> |
|<span data-ttu-id="81ba8-131">**Sökmotor**</span><span class="sxs-lookup"><span data-stu-id="81ba8-131">**Search engine**</span></span> | <span data-ttu-id="81ba8-132">Hämtar och resultat som matchar dokument baserat på hello innehållet i hello inverterad index.</span><span class="sxs-lookup"><span data-stu-id="81ba8-132">Retrieves and scores matching documents based on hello contents of hello inverted index.</span></span> |

## <a name="anatomy-of-a-search-request"></a><span data-ttu-id="81ba8-133">En sökbegäran uppbyggnad</span><span class="sxs-lookup"><span data-stu-id="81ba8-133">Anatomy of a search request</span></span>

<span data-ttu-id="81ba8-134">En sökbegäran är en fullständig specifikation av vad som ska returneras i en resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-134">A search request is a complete specification of what should be returned in a result set.</span></span> <span data-ttu-id="81ba8-135">I enklaste form är en tom fråga med några villkor av något slag.</span><span class="sxs-lookup"><span data-stu-id="81ba8-135">In simplest form, it is an empty query with no criteria of any kind.</span></span> <span data-ttu-id="81ba8-136">En mer realistisk exempel innehåller parametrar, flera fråga villkor, kanske begränsade toocertain fälten, möjligen filteruttrycket och ordning regler.</span><span class="sxs-lookup"><span data-stu-id="81ba8-136">A more realistic example includes parameters, several query terms, perhaps scoped toocertain fields, with possibly a filter expression and ordering rules.</span></span>  

<span data-ttu-id="81ba8-137">hello följande exempel är en sökbegäran som du kan skicka tooAzure Sök med hjälp av hello [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="81ba8-137">hello following example is a search request you might send tooAzure Search using hello [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>  

~~~~
POST /indexes/hotels/docs/search?api-version=2016-09-01 
{  
    "search": "Spacious, air-condition* +\"Ocean view\"",  
    "searchFields": "description, title",  
    "searchMode": "any",
    "filter": "price ge 60 and price lt 300",  
    "orderby": "geo.distance(location, geography'POINT(-159.476235 22.227659)')", 
    "queryType": "full" 
 } 
~~~~

<span data-ttu-id="81ba8-138">För denna begäran hello sökmotor hello följande:</span><span class="sxs-lookup"><span data-stu-id="81ba8-138">For this request, hello search engine does hello following:</span></span>

1. <span data-ttu-id="81ba8-139">Filtrerar ut dokument där hello pris är minst $60 och mindre än 300 USD.</span><span class="sxs-lookup"><span data-stu-id="81ba8-139">Filters out documents where hello price is at least $60 and less than $300.</span></span>
2. <span data-ttu-id="81ba8-140">Kör hello fråga.</span><span class="sxs-lookup"><span data-stu-id="81ba8-140">Executes hello query.</span></span> <span data-ttu-id="81ba8-141">I det här exemplet hello sökfråga består av fraser och villkor: `"Spacious, air-condition* +\"Ocean view\""` (användarna vanligtvis inte ange skiljetecken, men även i hello exempel kan vi tooexplain hur analyzers hantera).</span><span class="sxs-lookup"><span data-stu-id="81ba8-141">In this example, hello search query consists of phrases and terms: `"Spacious, air-condition* +\"Ocean view\""` (users typically don't enter punctuation, but including it in hello example allows us tooexplain how analyzers handle it).</span></span> <span data-ttu-id="81ba8-142">För den här frågan söker hello sökmotor hello beskrivning och rubrikfält anges i `searchFields` för dokument som innehåller ”oceanen vyn”, och dessutom på hello termen ”stora”, eller på villkor som börjar med hello prefix ”air-condition”.</span><span class="sxs-lookup"><span data-stu-id="81ba8-142">For this query, hello search engine scans hello description and title fields specified in `searchFields` for documents that contain "Ocean view", and additionally on hello term "spacious", or on terms that start with hello prefix "air-condition".</span></span> <span data-ttu-id="81ba8-143">Hej `searchMode` parametern är används toomatch på något villkor (standard) eller alla, i de fall där en term som inte krävs uttryckligen (`+`).</span><span class="sxs-lookup"><span data-stu-id="81ba8-143">hello `searchMode` parameter is used toomatch on any term (default) or all of them, for cases where a term is not explicitly required (`+`).</span></span>
3. <span data-ttu-id="81ba8-144">Order hello hotell uppsättning av närhet tooa på geografisk plats, och sedan returnerade toohello anropande programmet.</span><span class="sxs-lookup"><span data-stu-id="81ba8-144">Orders hello resulting set of hotels by proximity tooa given geography location, and then returned toohello calling application.</span></span> 

<span data-ttu-id="81ba8-145">hello merparten av den här artikeln handlar om bearbetningen av hello *sökfråga*: `"Spacious, air-condition* +\"Ocean view\""`.</span><span class="sxs-lookup"><span data-stu-id="81ba8-145">hello majority of this article is about processing of hello *search query*: `"Spacious, air-condition* +\"Ocean view\""`.</span></span> <span data-ttu-id="81ba8-146">Filtrera och sortera ligger utanför omfånget.</span><span class="sxs-lookup"><span data-stu-id="81ba8-146">Filtering and ordering are out of scope.</span></span> <span data-ttu-id="81ba8-147">Mer information finns i hello [Sök API-referensdokumentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="81ba8-147">For more information, see hello [Search API reference documentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a><span data-ttu-id="81ba8-148">Steg 1: Frågan parsning</span><span class="sxs-lookup"><span data-stu-id="81ba8-148">Stage 1: Query parsing</span></span> 

<span data-ttu-id="81ba8-149">Enligt nedanstående är hello frågesträngen hello första raden i hello begäran:</span><span class="sxs-lookup"><span data-stu-id="81ba8-149">As noted, hello query string is hello first line of hello request:</span></span> 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

<span data-ttu-id="81ba8-150">hello frågan parsern avgränsar operatorer (som `*` och `+` i exemplet hello) från sökning termer och deconstructs hello sökfråga i *underfrågor* av en typ som stöds:</span><span class="sxs-lookup"><span data-stu-id="81ba8-150">hello query parser separates operators (such as `*` and `+` in hello example) from search terms, and deconstructs hello search query into *subqueries* of a supported type:</span></span> 

+ <span data-ttu-id="81ba8-151">*Termen frågan* för fristående villkor (till exempel stora)</span><span class="sxs-lookup"><span data-stu-id="81ba8-151">*term query* for standalone terms (like spacious)</span></span>
+ <span data-ttu-id="81ba8-152">*frasen frågan* för angiven villkor (till exempel visa oceanen)</span><span class="sxs-lookup"><span data-stu-id="81ba8-152">*phrase query* for quoted terms (like ocean view)</span></span>
+ <span data-ttu-id="81ba8-153">*prefixet frågan* för termer följt av ett prefix-operatorn `*` (t.ex. air-condition)</span><span class="sxs-lookup"><span data-stu-id="81ba8-153">*prefix query* for terms followed by a prefix operator `*` (like air-condition)</span></span>

<span data-ttu-id="81ba8-154">En fullständig lista över stöds frågetyper finns [Lucene fråga sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span><span class="sxs-lookup"><span data-stu-id="81ba8-154">For a full list of supported query types see [Lucene query sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span></span>

<span data-ttu-id="81ba8-155">Operatörer som är associerade med en underfråga avgöra om hello frågan ”måste vara” eller ”ska vara” nöjd för ett dokument toobe anses vara en matchning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-155">Operators associated with a subquery determine whether hello query "must be" or "should be" satisfied in order for a document toobe considered a match.</span></span> <span data-ttu-id="81ba8-156">Till exempel `+"Ocean view"` ”måste” på grund av toohello `+` operator.</span><span class="sxs-lookup"><span data-stu-id="81ba8-156">For example, `+"Ocean view"` is "must" due toohello `+` operator.</span></span> 

<span data-ttu-id="81ba8-157">Hej frågeparsern omstrukturerar hello underfrågor i en *frågan trädet* (en intern struktur som representerar hello fråga) överförs på toohello sökmotor.</span><span class="sxs-lookup"><span data-stu-id="81ba8-157">hello query parser restructures hello subqueries into a *query tree* (an internal structure representing hello query) it passes on toohello search engine.</span></span> <span data-ttu-id="81ba8-158">I hello första steget av frågan parsning hello frågan trädet ser ut så här.</span><span class="sxs-lookup"><span data-stu-id="81ba8-158">In hello first stage of query parsing, hello query tree looks like this.</span></span>  

 ![Boolesk fråga searchmode alla][2]

### <a name="supported-parsers-simple-and-full-lucene"></a><span data-ttu-id="81ba8-160">Stöd för Parser: enkelt och fullständig Lucene</span><span class="sxs-lookup"><span data-stu-id="81ba8-160">Supported parsers: Simple and Full Lucene</span></span> 

 <span data-ttu-id="81ba8-161">Azure Search visar två olika frågespråk `simple` (standard) och `full`.</span><span class="sxs-lookup"><span data-stu-id="81ba8-161">Azure Search exposes two different query languages, `simple` (default) and `full`.</span></span> <span data-ttu-id="81ba8-162">Genom att ange hello `queryType` parameter med din begäran, anger du hello frågeparsern vilka frågespråket du väljer så att den vet hur toointerpret hello operatorer och syntax.</span><span class="sxs-lookup"><span data-stu-id="81ba8-162">By setting hello `queryType` parameter with your search request, you tell hello query parser which query language you choose so that it knows how toointerpret hello operators and syntax.</span></span> <span data-ttu-id="81ba8-163">Hej [enkel fråga språk](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) intuitivt och säkert, ofta lämplig toointerpret användarindata som-är utan klientsidan bearbetning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-163">hello [Simple query langauge](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) is intuitive and robust, often suitable toointerpret user input as-is without client-side processing.</span></span> <span data-ttu-id="81ba8-164">Det stöder frågeoperatorer bekant från sökmotorer.</span><span class="sxs-lookup"><span data-stu-id="81ba8-164">It supports query operators familiar from web search engines.</span></span> <span data-ttu-id="81ba8-165">Hej [fullständig Lucene frågespråk](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), som du får genom att ange `queryType=full`, utökar hello standardspråk för enkel fråga genom att lägga till stöd för flera operatorer och frågetyper som jokertecken, fuzzy regex och fältet omfång frågor.</span><span class="sxs-lookup"><span data-stu-id="81ba8-165">hello [Full Lucene query language](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), which you get by setting `queryType=full`, extends hello default Simple query language by adding support for more operators and query types like wildcard, fuzzy, regex, and field-scoped queries.</span></span> <span data-ttu-id="81ba8-166">Till exempel skulle ett reguljärt uttryck som skickas i enkla frågesyntaxen tolkas som en frågesträng och inte ett uttryck.</span><span class="sxs-lookup"><span data-stu-id="81ba8-166">For example, a regular expression sent in Simple query syntax would be interpreted as a query string and not an expression.</span></span> <span data-ttu-id="81ba8-167">Hej exempelbegäran i den här artikeln använder hello fullständig Lucene frågespråk.</span><span class="sxs-lookup"><span data-stu-id="81ba8-167">hello example request in this article uses hello Full Lucene query language.</span></span>

### <a name="impact-of-searchmode-on-hello-parser"></a><span data-ttu-id="81ba8-168">Effekten av searchMode i hello tolken</span><span class="sxs-lookup"><span data-stu-id="81ba8-168">Impact of searchMode on hello parser</span></span> 

<span data-ttu-id="81ba8-169">En annan begäran sökparameter som påverkar parsning är hello `searchMode` parameter.</span><span class="sxs-lookup"><span data-stu-id="81ba8-169">Another search request parameter that affects parsing is hello `searchMode` parameter.</span></span> <span data-ttu-id="81ba8-170">Den styr hello standard operator för booleska frågor: en (standard) eller alla.</span><span class="sxs-lookup"><span data-stu-id="81ba8-170">It controls hello default operator for Boolean queries: any (default) or all.</span></span>  

<span data-ttu-id="81ba8-171">När `searchMode=any`, som är standard hello hello utrymme avgränsare mellan stora och air-condition är eller (`||`), gör hello exempel frågetexten motsvarar:</span><span class="sxs-lookup"><span data-stu-id="81ba8-171">When `searchMode=any`, which is hello default, hello space delimiter between spacious and air-condition is OR (`||`), making hello sample query text equivalent to:</span></span> 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

<span data-ttu-id="81ba8-172">Explicit operatorer som `+` i `+"Ocean view"`, är entydigt i Boolesk fråga konstruktion (hello termen *måste* matchar).</span><span class="sxs-lookup"><span data-stu-id="81ba8-172">Explicit operators, such as `+` in `+"Ocean view"`, are unambiguous in boolean query construction (hello term *must* match).</span></span> <span data-ttu-id="81ba8-173">Lika självklara är hur toointerpret hello återstående villkor: stora och air-condition.</span><span class="sxs-lookup"><span data-stu-id="81ba8-173">Less obvious is how toointerpret hello remaining terms: spacious and air-condition.</span></span> <span data-ttu-id="81ba8-174">Bör hello sökmotor hitta matchningar för oceanen vyn *och* stora *och* air-condition?</span><span class="sxs-lookup"><span data-stu-id="81ba8-174">Should hello search engine find matches on ocean view *and* spacious *and* air-condition?</span></span> <span data-ttu-id="81ba8-175">Eller bör hitta oceanen visa plus *antingen en* av hello återstående villkoren?</span><span class="sxs-lookup"><span data-stu-id="81ba8-175">Or should it find ocean view plus *either one* of hello remaining terms?</span></span> 

<span data-ttu-id="81ba8-176">Som standard (`searchMode=any`), hello sökmotor förutsätter hello bredare tolkning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-176">By default (`searchMode=any`), hello search engine assumes hello broader interpretation.</span></span> <span data-ttu-id="81ba8-177">Antingen fältet *bör* matchas, reflektion ”eller” semantik.</span><span class="sxs-lookup"><span data-stu-id="81ba8-177">Either field *should* be matched, reflecting "or" semantics.</span></span> <span data-ttu-id="81ba8-178">hello första fråga trädet illustreras tidigare med hello två ”bör” åtgärder, visar hello standard.</span><span class="sxs-lookup"><span data-stu-id="81ba8-178">hello initial query tree illustrated previously, with hello two "should" operations, shows hello default.</span></span>  

<span data-ttu-id="81ba8-179">Anta att vi nu ställer `searchMode=all`.</span><span class="sxs-lookup"><span data-stu-id="81ba8-179">Suppose that we now set `searchMode=all`.</span></span> <span data-ttu-id="81ba8-180">I det här fallet tolkas hello utrymme som en ”och”-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="81ba8-180">In this case, hello space is interpreted as an "and" operation.</span></span> <span data-ttu-id="81ba8-181">Var och en av hello återstående villkor måste båda vara finns i hello dokumentet tooqualify som en matchning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-181">Each of hello remaining terms must both be present in hello document tooqualify as a match.</span></span> <span data-ttu-id="81ba8-182">hello resulterande exempelfråga skulle tolkas enligt följande:</span><span class="sxs-lookup"><span data-stu-id="81ba8-182">hello resulting sample query would be interpreted as follows:</span></span> 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

<span data-ttu-id="81ba8-183">Ett ändrade frågan träd för den här frågan skulle vara följande, där ett matchande dokument är hello skärningspunkten för alla tre underfrågor:</span><span class="sxs-lookup"><span data-stu-id="81ba8-183">A modified query tree for this query would be as follows, where a matching document is hello intersection of all three subqueries:</span></span> 

 ![Boolesk fråga searchmode alla][3]

> [!Note] 
> <span data-ttu-id="81ba8-185">Om du väljer `searchMode=any` över `searchMode=all` beslut bästa erhålls genom att köra representativt frågor.</span><span class="sxs-lookup"><span data-stu-id="81ba8-185">Choosing `searchMode=any` over `searchMode=all` is a decision best arrived at by running representative queries.</span></span> <span data-ttu-id="81ba8-186">Användare som är sannolikt tooinclude operatorer (common när sökning dokumentet lagrar) kan vara resultatet intuitivt om `searchMode=all` informerar Boolesk fråga konstruktioner.</span><span class="sxs-lookup"><span data-stu-id="81ba8-186">Users who are likely tooinclude operators (common when searching document stores) might find results more intuitive if `searchMode=all` informs boolean query constructs.</span></span> <span data-ttu-id="81ba8-187">Mer information om hello samspelet mellan `searchMode` och operatorer, se [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span><span class="sxs-lookup"><span data-stu-id="81ba8-187">For more about hello interplay between `searchMode` and operators, see [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span></span>

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a><span data-ttu-id="81ba8-188">Steg 2: Lexikaliskt analys</span><span class="sxs-lookup"><span data-stu-id="81ba8-188">Stage 2: Lexical analysis</span></span> 

<span data-ttu-id="81ba8-189">Lexikaliskt analyzers processen *term frågor* och *fras frågor* när hello frågan trädet är strukturerad.</span><span class="sxs-lookup"><span data-stu-id="81ba8-189">Lexical analyzers process *term queries* and *phrase queries* after hello query tree is structured.</span></span> <span data-ttu-id="81ba8-190">En analyzer accepterar hello text indata som angetts tooit av hello parsern processer hello text och sedan skickar tillbaka tokeniserad villkoren toobe införlivas i hello frågan trädet.</span><span class="sxs-lookup"><span data-stu-id="81ba8-190">An analyzer accepts hello text inputs given tooit by hello parser, processes hello text, and then sends back tokenized terms toobe incorporated into hello query tree.</span></span> 

<span data-ttu-id="81ba8-191">hello vanligaste formen av lexikala analys är *språkliga analysis* som omvandlar sökord baserat på regler för specifika tooa angivna språk:</span><span class="sxs-lookup"><span data-stu-id="81ba8-191">hello most common form of lexical analysis is *linguistic analysis* which transforms query terms based on rules specific tooa given language:</span></span> 

* <span data-ttu-id="81ba8-192">Minska en fråga termen toohello rot form av ett ord</span><span class="sxs-lookup"><span data-stu-id="81ba8-192">Reducing a query term toohello root form of a word</span></span> 
* <span data-ttu-id="81ba8-193">Ta bort irrelevanta ord (stoppord, till exempel ”i” eller ”och” på engelska)</span><span class="sxs-lookup"><span data-stu-id="81ba8-193">Removing non-essential words (stopwords, such as "the" or "and" in English)</span></span> 
* <span data-ttu-id="81ba8-194">Dela upp ett sammansatt ord i komponenter</span><span class="sxs-lookup"><span data-stu-id="81ba8-194">Breaking a composite word into component parts</span></span> 
* <span data-ttu-id="81ba8-195">Lägre skiftläge ett ord med versal</span><span class="sxs-lookup"><span data-stu-id="81ba8-195">Lower casing an upper case word</span></span> 

<span data-ttu-id="81ba8-196">Alla dessa åtgärder tenderar tooerase skillnaderna mellan hello textinmatning som tillhandahålls av hello användar- och hello villkoren som lagras i hello index.</span><span class="sxs-lookup"><span data-stu-id="81ba8-196">All of these operations tend tooerase differences between hello text input provided by hello user and hello terms stored in hello index.</span></span> <span data-ttu-id="81ba8-197">Dessa åtgärder utöver text bearbetning och kräver avancerade kunskaper hello-språket sig själv.</span><span class="sxs-lookup"><span data-stu-id="81ba8-197">Such operations go beyond text processing and require in-depth knowledge of hello language itself.</span></span> <span data-ttu-id="81ba8-198">tooadd detta skikt av språkliga medvetenhet Azure Search har stöd för en lång lista med [språkanalys](https://docs.microsoft.com/rest/api/searchservice/language-support) från både Lucene och Microsoft.</span><span class="sxs-lookup"><span data-stu-id="81ba8-198">tooadd this layer of linguistic awareness, Azure Search supports a long list of [language analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support) from both Lucene and Microsoft.</span></span>

> [!Note]
> <span data-ttu-id="81ba8-199">Analys-kraven kan variera mellan minimal tooelaborate beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="81ba8-199">Analysis requirements can range from minimal tooelaborate depending on your scenario.</span></span> <span data-ttu-id="81ba8-200">Du kan styra komplexitet lexikala analys genom att välja ett av hello fördefinierade analyzers hello eller skapa egna [anpassade analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="81ba8-200">You can control complexity of lexical analysis by hello selecting one of hello predefined analyzers or by creating your own [custom analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span></span> <span data-ttu-id="81ba8-201">Analyzers är begränsade toosearchable fält som har angetts som en del av en fältdefinition av.</span><span class="sxs-lookup"><span data-stu-id="81ba8-201">Analyzers are scoped toosearchable fields and are specified as part of a field definition.</span></span> <span data-ttu-id="81ba8-202">Detta ger dig toovary lexikala analys på grundval av per fält.</span><span class="sxs-lookup"><span data-stu-id="81ba8-202">This allows you toovary lexical analysis on a per-field basis.</span></span> <span data-ttu-id="81ba8-203">Inget hello *standard* Lucene analyzer används.</span><span class="sxs-lookup"><span data-stu-id="81ba8-203">Unspecified, hello *standard* Lucene analyzer is used.</span></span>

<span data-ttu-id="81ba8-204">I vårt exempel tidigare tooanalysis hello första fråga trädet har hello termen ”Spacious” med versaler ”S” och kommatecken som hello frågeparsern tolkas som en del av hello frågeterm (kommatecken inte anses vara ansvarig query language).</span><span class="sxs-lookup"><span data-stu-id="81ba8-204">In our example, prior tooanalysis, hello initial query tree has hello term "Spacious," with an uppercase "S" and a comma that hello query parser interprets as a part of hello query term (a comma is not considered a query language operator).</span></span>  

<span data-ttu-id="81ba8-205">När hello standard analyzer bearbetar hello har löpt ut, kommer den gemener ”oceanen view” och ”stora” och ta bort hello kommatecken tecken.</span><span class="sxs-lookup"><span data-stu-id="81ba8-205">When hello default analyzer processes hello term, it will lowercase "ocean view" and "spacious", and remove hello comma character.</span></span> <span data-ttu-id="81ba8-206">hello ändrade frågan trädet ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="81ba8-206">hello modified query tree will look as follows:</span></span> 

 ![Boolesk fråga med analyserade villkor][4]

### <a name="testing-analyzer-behaviors"></a><span data-ttu-id="81ba8-208">Testa analyzer beteenden</span><span class="sxs-lookup"><span data-stu-id="81ba8-208">Testing analyzer behaviors</span></span> 

<span data-ttu-id="81ba8-209">hello beteendet för en analyzer kan testas med hello [analysera API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span><span class="sxs-lookup"><span data-stu-id="81ba8-209">hello behavior of an analyzer can be tested using hello [Analyze API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span></span> <span data-ttu-id="81ba8-210">Ange hello text som du vill tooanalyze toosee vilka villkor som anges analyzer genererar.</span><span class="sxs-lookup"><span data-stu-id="81ba8-210">Provide hello text you want tooanalyze toosee what terms given analyzer will generate.</span></span> <span data-ttu-id="81ba8-211">Till exempel toosee hur hello standard analyzer skulle bearbeta hello text ”air-condition”, kan du utfärda hello följande begäran:</span><span class="sxs-lookup"><span data-stu-id="81ba8-211">For example, toosee how hello standard analyzer would process hello text "air-condition", you can issue hello following request:</span></span>

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

<span data-ttu-id="81ba8-212">hello standard analyzer bryter hello-indata till hello följande två token, lägga till anteckningar med attribut som start och end förskjutningarna (används för träffar syntaxmarkering) samt deras position (används för frasen matchar):</span><span class="sxs-lookup"><span data-stu-id="81ba8-212">hello standard analyzer breaks hello input text into hello following two tokens, annotating them with attributes like start and end offsets (used for hit highlighting) as well as their position (used for phrase matching):</span></span>

~~~~
{  
  "tokens": [
    {
      "token": "air",
      "startOffset": 0,
      "endOffset": 3,
      "position": 0
    },
    {
      "token": "condition",
      "startOffset": 4,
      "endOffset": 13,
      "position": 1
    }
  ]
}
~~~~

### <a name="exceptions-toolexical-analysis"></a><span data-ttu-id="81ba8-213">Undantag toolexical analys</span><span class="sxs-lookup"><span data-stu-id="81ba8-213">Exceptions toolexical analysis</span></span> 

<span data-ttu-id="81ba8-214">Lexikaliskt analys gäller endast tooquery typer som kräver fullständiga villkor – en term fråga eller en frasfråga.</span><span class="sxs-lookup"><span data-stu-id="81ba8-214">Lexical analysis applies only tooquery types that require complete terms – either a term query or a phrase query.</span></span> <span data-ttu-id="81ba8-215">Den gäller inte tooquery typer med ofullständiga villkor – prefix frågan jokertecken frågan, regex frågan – eller tooa fuzzy frågan.</span><span class="sxs-lookup"><span data-stu-id="81ba8-215">It doesn’t apply tooquery types with incomplete terms – prefix query, wildcard query, regex query – or tooa fuzzy query.</span></span> <span data-ttu-id="81ba8-216">De fråga typer, inklusive hello prefix frågan med termen *air-condition\**  i vårt exempel läggs direkt toohello frågan trädet kringgå hello analys steget.</span><span class="sxs-lookup"><span data-stu-id="81ba8-216">Those query types, including hello prefix query with term *air-condition\** in our example, are added directly toohello query tree, bypassing hello analysis stage.</span></span> <span data-ttu-id="81ba8-217">hello endast omvandling utförs på sökord på dessa typer lowercasing.</span><span class="sxs-lookup"><span data-stu-id="81ba8-217">hello only transformation performed on query terms of those types is lowercasing.</span></span>

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a><span data-ttu-id="81ba8-218">Steg 3: Hämta för dokument</span><span class="sxs-lookup"><span data-stu-id="81ba8-218">Stage 3: Document retrieval</span></span> 

<span data-ttu-id="81ba8-219">Hämta dokument refererar toofinding dokument med matchande villkoren i hello index.</span><span class="sxs-lookup"><span data-stu-id="81ba8-219">Document retrieval refers toofinding documents with matching terms in hello index.</span></span> <span data-ttu-id="81ba8-220">Det här steget är att förstå bäst genom ett exempel.</span><span class="sxs-lookup"><span data-stu-id="81ba8-220">This stage is understood best through an example.</span></span> <span data-ttu-id="81ba8-221">Låt oss börja med ett hotell index med hello följande enkla schemat:</span><span class="sxs-lookup"><span data-stu-id="81ba8-221">Let's start with a hotels index having hello following simple schema:</span></span> 

~~~~
{   
    "name": "hotels",     
    "fields": [     
        { "name": "id", "type": "Edm.String", "key": true, "searchable": false },     
        { "name": "title", "type": "Edm.String", "searchable": true },     
        { "name": "description", "type": "Edm.String", "searchable": true }
    ] 
} 
~~~~

<span data-ttu-id="81ba8-222">Anta vidare att indexet innehåller hello följande fyra dokument:</span><span class="sxs-lookup"><span data-stu-id="81ba8-222">Further assume that this index contains hello following four documents:</span></span> 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance toohello beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on hello north shore of hello island of Kauaʻi. Ocean view."     
        },       
        {         
            "id": "3",         
            "title": "Playa Hotel",         
            "description": "Comfortable, air-conditioned rooms with ocean view."
        },       
        {         
            "id": "4",         
            "title": "Ocean Retreat",         
            "description": "Quiet and secluded"
        }    
    ]
}
~~~~

<span data-ttu-id="81ba8-223">**Hur villkoren indexeras**</span><span class="sxs-lookup"><span data-stu-id="81ba8-223">**How terms are indexed**</span></span>

<span data-ttu-id="81ba8-224">hämtning av toounderstand hjälper tooknow några grunderna om indexering.</span><span class="sxs-lookup"><span data-stu-id="81ba8-224">toounderstand retrieval, it helps tooknow a few basics about indexing.</span></span> <span data-ttu-id="81ba8-225">hello är lagring ett inverterad index, en för varje sökbara fält.</span><span class="sxs-lookup"><span data-stu-id="81ba8-225">hello unit of storage is an inverted index, one for each searchable field.</span></span> <span data-ttu-id="81ba8-226">Är en sorterad lista över alla villkor från alla dokument i ett inverterad index.</span><span class="sxs-lookup"><span data-stu-id="81ba8-226">Within an inverted index is a sorted list of all terms from all documents.</span></span> <span data-ttu-id="81ba8-227">Varje term mappar toohello lista över dokument som det uppstår, som ses i hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="81ba8-227">Each term maps toohello list of documents in which it occurs, as evident in hello example below.</span></span>

<span data-ttu-id="81ba8-228">tooproduce hello villkor i ett inverterad index hello sökmotor utför lexikala analys över hello innehållet i dokumenten, liknande toowhat sker under frågebearbetningen.</span><span class="sxs-lookup"><span data-stu-id="81ba8-228">tooproduce hello terms in an inverted index, hello search engine performs lexical analysis over hello content of documents, similar toowhat happens during query processing.</span></span> <span data-ttu-id="81ba8-229">Text indata skickas tooan analyzer lägre-alltid demontering av skiljetecken och så vidare, beroende på hello analyzer konfiguration.</span><span class="sxs-lookup"><span data-stu-id="81ba8-229">Text inputs are passed tooan analyzer, lower-cased, stripped of punctuation, and so forth, depending on hello analyzer configuration.</span></span> <span data-ttu-id="81ba8-230">Det är vanligt, men krävs inte, toouse hello samma analyzers för sökning och indexering åtgärder så att sökord påminner mer villkoren i hello index.</span><span class="sxs-lookup"><span data-stu-id="81ba8-230">It's common, but not required, toouse hello same analyzers for search and indexing operations so that query terms look more like terms inside hello index.</span></span>

> [!Note]
> <span data-ttu-id="81ba8-231">Azure Search kan du ange olika analyzers för indexering och söka via ytterligare `indexAnalyzer` och `searchAnalyzer` fältet parametrar.</span><span class="sxs-lookup"><span data-stu-id="81ba8-231">Azure Search lets you specify different analyzers for indexing and search via additional `indexAnalyzer` and `searchAnalyzer` field parameters.</span></span> <span data-ttu-id="81ba8-232">Om inget anges hello analyzer med hello `analyzer` egenskapen används för både indexering och sökning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-232">If unspecified, hello analyzer set with hello `analyzer` property is used for both indexing and searching.</span></span>  

<span data-ttu-id="81ba8-233">**Omvända index för dokument**</span><span class="sxs-lookup"><span data-stu-id="81ba8-233">**Inverted index for example documents**</span></span>

<span data-ttu-id="81ba8-234">Returnerar tooour exempel hello **rubrik** fältet hello inverterad index ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="81ba8-234">Returning tooour example, for hello **title** field, hello inverted index looks like this:</span></span>

| <span data-ttu-id="81ba8-235">Period</span><span class="sxs-lookup"><span data-stu-id="81ba8-235">Term</span></span> | <span data-ttu-id="81ba8-236">Listan över</span><span class="sxs-lookup"><span data-stu-id="81ba8-236">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="81ba8-237">atman</span><span class="sxs-lookup"><span data-stu-id="81ba8-237">atman</span></span> | <span data-ttu-id="81ba8-238">1</span><span class="sxs-lookup"><span data-stu-id="81ba8-238">1</span></span> |
| <span data-ttu-id="81ba8-239">stranden</span><span class="sxs-lookup"><span data-stu-id="81ba8-239">beach</span></span> | <span data-ttu-id="81ba8-240">2</span><span class="sxs-lookup"><span data-stu-id="81ba8-240">2</span></span> |
| <span data-ttu-id="81ba8-241">hotell</span><span class="sxs-lookup"><span data-stu-id="81ba8-241">hotel</span></span> | <span data-ttu-id="81ba8-242">1, 3</span><span class="sxs-lookup"><span data-stu-id="81ba8-242">1, 3</span></span> |
| <span data-ttu-id="81ba8-243">oceanen</span><span class="sxs-lookup"><span data-stu-id="81ba8-243">ocean</span></span> | <span data-ttu-id="81ba8-244">4</span><span class="sxs-lookup"><span data-stu-id="81ba8-244">4</span></span>  |
| <span data-ttu-id="81ba8-245">playa</span><span class="sxs-lookup"><span data-stu-id="81ba8-245">playa</span></span> | <span data-ttu-id="81ba8-246">3</span><span class="sxs-lookup"><span data-stu-id="81ba8-246">3</span></span> |
| <span data-ttu-id="81ba8-247">utväg</span><span class="sxs-lookup"><span data-stu-id="81ba8-247">resort</span></span> | <span data-ttu-id="81ba8-248">3</span><span class="sxs-lookup"><span data-stu-id="81ba8-248">3</span></span> |
| <span data-ttu-id="81ba8-249">Återställ format</span><span class="sxs-lookup"><span data-stu-id="81ba8-249">retreat</span></span> | <span data-ttu-id="81ba8-250">4</span><span class="sxs-lookup"><span data-stu-id="81ba8-250">4</span></span> |

<span data-ttu-id="81ba8-251">I hello rubrikfält, endast *hotell* visas i två dokument: 1, 3.</span><span class="sxs-lookup"><span data-stu-id="81ba8-251">In hello title field, only *hotel* shows up in two documents: 1, 3.</span></span>

<span data-ttu-id="81ba8-252">För hello **beskrivning** fältet hello index är följande:</span><span class="sxs-lookup"><span data-stu-id="81ba8-252">For hello **description** field, hello index is as follows:</span></span>

| <span data-ttu-id="81ba8-253">Period</span><span class="sxs-lookup"><span data-stu-id="81ba8-253">Term</span></span> | <span data-ttu-id="81ba8-254">Listan över</span><span class="sxs-lookup"><span data-stu-id="81ba8-254">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="81ba8-255">luften</span><span class="sxs-lookup"><span data-stu-id="81ba8-255">air</span></span> | <span data-ttu-id="81ba8-256">3</span><span class="sxs-lookup"><span data-stu-id="81ba8-256">3</span></span>
| <span data-ttu-id="81ba8-257">och</span><span class="sxs-lookup"><span data-stu-id="81ba8-257">and</span></span> | <span data-ttu-id="81ba8-258">4</span><span class="sxs-lookup"><span data-stu-id="81ba8-258">4</span></span>
| <span data-ttu-id="81ba8-259">stranden</span><span class="sxs-lookup"><span data-stu-id="81ba8-259">beach</span></span> | <span data-ttu-id="81ba8-260">1</span><span class="sxs-lookup"><span data-stu-id="81ba8-260">1</span></span>
| <span data-ttu-id="81ba8-261">villkor</span><span class="sxs-lookup"><span data-stu-id="81ba8-261">conditioned</span></span> | <span data-ttu-id="81ba8-262">3</span><span class="sxs-lookup"><span data-stu-id="81ba8-262">3</span></span>
| <span data-ttu-id="81ba8-263">fria</span><span class="sxs-lookup"><span data-stu-id="81ba8-263">comfortable</span></span> | <span data-ttu-id="81ba8-264">3</span><span class="sxs-lookup"><span data-stu-id="81ba8-264">3</span></span>
| <span data-ttu-id="81ba8-265">Avstånd</span><span class="sxs-lookup"><span data-stu-id="81ba8-265">distance</span></span> | <span data-ttu-id="81ba8-266">1</span><span class="sxs-lookup"><span data-stu-id="81ba8-266">1</span></span>
| <span data-ttu-id="81ba8-267">dataö</span><span class="sxs-lookup"><span data-stu-id="81ba8-267">island</span></span> | <span data-ttu-id="81ba8-268">2</span><span class="sxs-lookup"><span data-stu-id="81ba8-268">2</span></span>
| <span data-ttu-id="81ba8-269">kauaʻi</span><span class="sxs-lookup"><span data-stu-id="81ba8-269">kauaʻi</span></span> | <span data-ttu-id="81ba8-270">2</span><span class="sxs-lookup"><span data-stu-id="81ba8-270">2</span></span>
| <span data-ttu-id="81ba8-271">finns</span><span class="sxs-lookup"><span data-stu-id="81ba8-271">located</span></span> | <span data-ttu-id="81ba8-272">2</span><span class="sxs-lookup"><span data-stu-id="81ba8-272">2</span></span>
| <span data-ttu-id="81ba8-273">Nord</span><span class="sxs-lookup"><span data-stu-id="81ba8-273">north</span></span> | <span data-ttu-id="81ba8-274">2</span><span class="sxs-lookup"><span data-stu-id="81ba8-274">2</span></span>
| <span data-ttu-id="81ba8-275">oceanen</span><span class="sxs-lookup"><span data-stu-id="81ba8-275">ocean</span></span> | <span data-ttu-id="81ba8-276">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="81ba8-276">1, 2, 3</span></span>
| <span data-ttu-id="81ba8-277">av</span><span class="sxs-lookup"><span data-stu-id="81ba8-277">of</span></span> | <span data-ttu-id="81ba8-278">2</span><span class="sxs-lookup"><span data-stu-id="81ba8-278">2</span></span>
| <span data-ttu-id="81ba8-279">på</span><span class="sxs-lookup"><span data-stu-id="81ba8-279">on</span></span> |<span data-ttu-id="81ba8-280">2</span><span class="sxs-lookup"><span data-stu-id="81ba8-280">2</span></span>
| <span data-ttu-id="81ba8-281">Tyst</span><span class="sxs-lookup"><span data-stu-id="81ba8-281">quiet</span></span> | <span data-ttu-id="81ba8-282">4</span><span class="sxs-lookup"><span data-stu-id="81ba8-282">4</span></span>
| <span data-ttu-id="81ba8-283">lokaler</span><span class="sxs-lookup"><span data-stu-id="81ba8-283">rooms</span></span>  | <span data-ttu-id="81ba8-284">1, 3</span><span class="sxs-lookup"><span data-stu-id="81ba8-284">1, 3</span></span>
| <span data-ttu-id="81ba8-285">secluded</span><span class="sxs-lookup"><span data-stu-id="81ba8-285">secluded</span></span> | <span data-ttu-id="81ba8-286">4</span><span class="sxs-lookup"><span data-stu-id="81ba8-286">4</span></span>
| <span data-ttu-id="81ba8-287">land</span><span class="sxs-lookup"><span data-stu-id="81ba8-287">shore</span></span> | <span data-ttu-id="81ba8-288">2</span><span class="sxs-lookup"><span data-stu-id="81ba8-288">2</span></span>
| <span data-ttu-id="81ba8-289">stora</span><span class="sxs-lookup"><span data-stu-id="81ba8-289">spacious</span></span> | <span data-ttu-id="81ba8-290">1</span><span class="sxs-lookup"><span data-stu-id="81ba8-290">1</span></span>
| <span data-ttu-id="81ba8-291">Hej</span><span class="sxs-lookup"><span data-stu-id="81ba8-291">hello</span></span> | <span data-ttu-id="81ba8-292">1, 2</span><span class="sxs-lookup"><span data-stu-id="81ba8-292">1, 2</span></span>
| <span data-ttu-id="81ba8-293">för</span><span class="sxs-lookup"><span data-stu-id="81ba8-293">too</span></span>| <span data-ttu-id="81ba8-294">1</span><span class="sxs-lookup"><span data-stu-id="81ba8-294">1</span></span>
| <span data-ttu-id="81ba8-295">vy</span><span class="sxs-lookup"><span data-stu-id="81ba8-295">view</span></span> | <span data-ttu-id="81ba8-296">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="81ba8-296">1, 2, 3</span></span>
| <span data-ttu-id="81ba8-297">Gå</span><span class="sxs-lookup"><span data-stu-id="81ba8-297">walking</span></span> | <span data-ttu-id="81ba8-298">1</span><span class="sxs-lookup"><span data-stu-id="81ba8-298">1</span></span>
| <span data-ttu-id="81ba8-299">med</span><span class="sxs-lookup"><span data-stu-id="81ba8-299">with</span></span> | <span data-ttu-id="81ba8-300">3</span><span class="sxs-lookup"><span data-stu-id="81ba8-300">3</span></span>


<span data-ttu-id="81ba8-301">**Matchande sökord mot indexerade ord**</span><span class="sxs-lookup"><span data-stu-id="81ba8-301">**Matching query terms against indexed terms**</span></span>

<span data-ttu-id="81ba8-302">Hello inverterad index ovan, vi returnera toohello exempelfråga och se hur matchande dokument hittades för våra exempelfråga.</span><span class="sxs-lookup"><span data-stu-id="81ba8-302">Given hello inverted indices above, let’s return toohello sample query and see how matching documents are found for our example query.</span></span> <span data-ttu-id="81ba8-303">Återkalla hello slutlig trädet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="81ba8-303">Recall that hello final query tree looks like this:</span></span> 

 ![Boolesk fråga med analyserade villkor][4]

<span data-ttu-id="81ba8-305">Vid körning av fråga körs enskilda frågor mot hello sökbara fält oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="81ba8-305">During query execution, individual queries are executed against hello searchable fields independently.</span></span> 

+ <span data-ttu-id="81ba8-306">Hej TermQuery, matchar ”stora” dokument 1 (hotell Atman).</span><span class="sxs-lookup"><span data-stu-id="81ba8-306">hello TermQuery, "spacious", matches document 1 (Hotel Atman).</span></span> 

+ <span data-ttu-id="81ba8-307">Hej PrefixQuery ”, air-condition *”, matchar inte några dokument.</span><span class="sxs-lookup"><span data-stu-id="81ba8-307">hello PrefixQuery, "air-condition*", doesn't match any documents.</span></span> 

  <span data-ttu-id="81ba8-308">Det här är ett beteende som ibland confuses utvecklare.</span><span class="sxs-lookup"><span data-stu-id="81ba8-308">This is a behavior that sometimes confuses developers.</span></span> <span data-ttu-id="81ba8-309">Även om hello termen luftkonditionerad finns i hello dokumentet, är den uppdelad i två termer av hello standard analyzer.</span><span class="sxs-lookup"><span data-stu-id="81ba8-309">Although hello term air-conditioned exists in hello document, it is split into two terms by hello default analyzer.</span></span> <span data-ttu-id="81ba8-310">Återkalla inte analyseras prefix frågor som innehåller partiella villkoren.</span><span class="sxs-lookup"><span data-stu-id="81ba8-310">Recall that prefix queries, which contain partial terms, are not analyzed.</span></span> <span data-ttu-id="81ba8-311">Därför villkor med prefixet ”air-condition” slås upp i hello inverterad index och det gick inte att hitta.</span><span class="sxs-lookup"><span data-stu-id="81ba8-311">Therefore terms with prefix "air-condition" are looked up in hello inverted index and not found.</span></span>

+ <span data-ttu-id="81ba8-312">Hej PhraseQuery, ”oceanen visa-letar upp hello villkor” oceanen ”och” view ”och kontrollerar hello närhet av villkoren i hello originaldokumentet.</span><span class="sxs-lookup"><span data-stu-id="81ba8-312">hello PhraseQuery, "ocean view", looks up hello terms "ocean" and "view" and checks hello proximity of terms in hello original document.</span></span> <span data-ttu-id="81ba8-313">Den här frågan i beskrivningsfält hello överensstämmer dokument 1, 2 och 3.</span><span class="sxs-lookup"><span data-stu-id="81ba8-313">Documents 1, 2 and 3 match this query in hello description field.</span></span> <span data-ttu-id="81ba8-314">Meddelande dokumentet 4 har hello termen oceanen i hello avdelning men anses inte vara en matchning vi söker hello ”oceanen visa-fras i stället för enskilda ord.</span><span class="sxs-lookup"><span data-stu-id="81ba8-314">Notice document 4 has hello term ocean in hello title but isn’t considered a match, as we're looking for hello "ocean view" phrase rather than individual words.</span></span> 

> [!Note]
> <span data-ttu-id="81ba8-315">En sökning körs oberoende mot alla sökbara fält i hello Azure Search index om du inte begränsar hello fält med hello `searchFields` parameter, enligt beskrivningen i hello exempel sökbegäran.</span><span class="sxs-lookup"><span data-stu-id="81ba8-315">A search query is executed independently against all searchable fields in hello Azure Search index unless you limit hello fields set with hello `searchFields` parameter, as illustrated in hello example search request.</span></span> <span data-ttu-id="81ba8-316">Dokument som matchar i någon av hello valt fält returneras.</span><span class="sxs-lookup"><span data-stu-id="81ba8-316">Documents that match in any of hello selected fields are returned.</span></span> 

<span data-ttu-id="81ba8-317">Hello-dokument som matchar är hello hela för hello frågan i fråga, 1, 2, 3.</span><span class="sxs-lookup"><span data-stu-id="81ba8-317">On hello whole, for hello query in question, hello documents that match are 1, 2, 3.</span></span> 

## <a name="stage-4-scoring"></a><span data-ttu-id="81ba8-318">Steg 4: poängsättning</span><span class="sxs-lookup"><span data-stu-id="81ba8-318">Stage 4: Scoring</span></span>  

<span data-ttu-id="81ba8-319">Alla dokument i ett sökresultat tilldelas en relevans poäng.</span><span class="sxs-lookup"><span data-stu-id="81ba8-319">Every document in a search result set is assigned a relevance score.</span></span> <span data-ttu-id="81ba8-320">hello funktionen av hello relevans poäng är toorank högre dessa dokument att bäst besvara en användare fråga som uttrycks genom hello sökfråga.</span><span class="sxs-lookup"><span data-stu-id="81ba8-320">hello function of hello relevance score is toorank higher those documents that best answer a user question as expressed by hello search query.</span></span> <span data-ttu-id="81ba8-321">hello poäng beräknas utifrån statistiska egenskaper för termer som matchas.</span><span class="sxs-lookup"><span data-stu-id="81ba8-321">hello score is computed based on statistical properties of terms that matched.</span></span> <span data-ttu-id="81ba8-322">Hello är kärnan i hello bedömningen formeln [TF/IDF (termen frekvens inversen dokumentet frekvens)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span><span class="sxs-lookup"><span data-stu-id="81ba8-322">At hello core of hello scoring formula is [TF/IDF (term frequency-inverse document frequency)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span></span> <span data-ttu-id="81ba8-323">I frågor som innehåller sällsynt och gemensamma villkor, främjar TF/IDF resultat som innehåller hello sällsynta termen.</span><span class="sxs-lookup"><span data-stu-id="81ba8-323">In queries containing rare and common terms, TF/IDF promotes results containing hello rare term.</span></span> <span data-ttu-id="81ba8-324">Exempelvis i ett hypotetiskt index med alla Wikipedia artiklar från dokument som matchade hello frågar *hello VD*, matchning på dokument *VD* betraktas som relevanta mer än dokument som matchar på *i*.</span><span class="sxs-lookup"><span data-stu-id="81ba8-324">For example, in a hypothetical index with all Wikipedia articles, from documents that matched hello query *hello president*, documents matching on *president* are considered more relevant than documents matching on *the*.</span></span>


### <a name="scoring-example"></a><span data-ttu-id="81ba8-325">Bedömningsprofil exempel</span><span class="sxs-lookup"><span data-stu-id="81ba8-325">Scoring example</span></span>

<span data-ttu-id="81ba8-326">Återkalla hello tre dokument som matchade våra exempelfråga:</span><span class="sxs-lookup"><span data-stu-id="81ba8-326">Recall hello three documents that matched our example query:</span></span>
~~~~
search=Spacious, air-condition* +"Ocean view"  
~~~~
~~~~
{  
  "value": [
    {
      "@search.score": 0.25610128,
      "id": "1",
      "title": "Hotel Atman",
      "description": "Spacious rooms, ocean view, walking distance toohello beach."
    },
    {
      "@search.score": 0.08951007,
      "id": "3",
      "title": "Playa Hotel",
      "description": "Comfortable, air-conditioned rooms with ocean view."
    },
    {
      "@search.score": 0.05967338,
      "id": "2",
      "title": "Ocean Resort",
      "description": "Located on a cliff on hello north shore of hello island of Kauai. Ocean view."
    }
  ]
}
~~~~

<span data-ttu-id="81ba8-327">Dokumentera 1 matchade hello frågan bäst eftersom både hello termen *stora* och hello krävs frasen *oceanen visa* uppstå i beskrivningsfält hello.</span><span class="sxs-lookup"><span data-stu-id="81ba8-327">Document 1 matched hello query best because both hello term *spacious* and hello required phrase *ocean view* occur in hello description field.</span></span> <span data-ttu-id="81ba8-328">hello två dokument matchar endast hello frasen *oceanen visa*.</span><span class="sxs-lookup"><span data-stu-id="81ba8-328">hello next two documents match only hello phrase *ocean view*.</span></span> <span data-ttu-id="81ba8-329">Det kan oönskad som hello relevans poäng för dokument 2 och 3 är olika, även om de matchade hello frågan i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="81ba8-329">It might be surprising that hello relevance score for document 2 and 3 is different even though they matched hello query in hello same way.</span></span> <span data-ttu-id="81ba8-330">Det beror på att hello bedömningen formeln har fler komponenter än bara TF/IDF.</span><span class="sxs-lookup"><span data-stu-id="81ba8-330">It's because hello scoring formula has more components than just TF/IDF.</span></span> <span data-ttu-id="81ba8-331">I det här fallet har dokumentet 3 tilldelats en något högre poäng eftersom dess beskrivning är kortare.</span><span class="sxs-lookup"><span data-stu-id="81ba8-331">In this case, document 3 was assigned a slightly higher score because its description is shorter.</span></span> <span data-ttu-id="81ba8-332">Lär dig mer om [Lucenes praktiska bedömningen formeln](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) toounderstand hur fältlängden och andra faktorer kan påverka hello relevans poäng.</span><span class="sxs-lookup"><span data-stu-id="81ba8-332">Learn about [Lucene's Practical Scoring Formula](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) toounderstand how field length and other factors can influence hello relevance score.</span></span>

<span data-ttu-id="81ba8-333">Vissa fråga typer (med jokertecken, prefix, regex) alltid bidra med en konstant poäng toohello övergripande dokumentera poäng.</span><span class="sxs-lookup"><span data-stu-id="81ba8-333">Some query types (wildcard, prefix, regex) always contribute a constant score toohello overall document score.</span></span> <span data-ttu-id="81ba8-334">Detta gör att matchningar via frågan expansion toobe ingår i hello resultat, men utan att påverka hello rangordning.</span><span class="sxs-lookup"><span data-stu-id="81ba8-334">This allows matches found through query expansion toobe included in hello results, but without affecting hello ranking.</span></span> 

<span data-ttu-id="81ba8-335">Ett exempel visar varför det är viktigt.</span><span class="sxs-lookup"><span data-stu-id="81ba8-335">An example illustrates why this matters.</span></span> <span data-ttu-id="81ba8-336">Jokertecken, inklusive prefix sökningar är tvetydig per definition eftersom hello-indata är en partiell sträng med potentiella matchar på ett mycket stort antal olika villkor (överväga indata av ”rundtur *” med matchningar hittades för ”visningar”, ”tourettes”, och ” tourmaline ”).</span><span class="sxs-lookup"><span data-stu-id="81ba8-336">Wildcard searches, including prefix searches, are ambiguous by definition because hello input is a partial string with potential matches on a very large number of disparate terms (consider an input of "tour*", with matches found on “tours”, “tourettes”, and “tourmaline”).</span></span> <span data-ttu-id="81ba8-337">Eftersom hello de här resultaten returneras, går det inte härleda tooreasonably vilket är mer värdefull än andra.</span><span class="sxs-lookup"><span data-stu-id="81ba8-337">Given hello nature of these results, there is no way tooreasonably infer which terms are more valuable than others.</span></span> <span data-ttu-id="81ba8-338">Därför bör Ignorera vi termen frekvenser när poängberäkningen resultat i frågor för typer med jokertecken, prefix och regex.</span><span class="sxs-lookup"><span data-stu-id="81ba8-338">For this reason, we ignore term frequencies when scoring results in queries of types wildcard, prefix and regex.</span></span> <span data-ttu-id="81ba8-339">I en flerdelade sökbegäran som innehåller begränsade och fullständiga termer ingår resultat från hello partiella indata med ett konstant poängsätta tooavoid rikta mot potentiellt oväntat matchar.</span><span class="sxs-lookup"><span data-stu-id="81ba8-339">In a multi-part search request that includes partial and complete terms, results from hello partial input are incorporated with a constant score tooavoid bias towards potentially unexpected matches.</span></span>

### <a name="score-tuning"></a><span data-ttu-id="81ba8-340">Poäng justera</span><span class="sxs-lookup"><span data-stu-id="81ba8-340">Score tuning</span></span>

<span data-ttu-id="81ba8-341">Det finns två sätt tootune relevans resultat i Azure Search:</span><span class="sxs-lookup"><span data-stu-id="81ba8-341">There are two ways tootune relevance scores in Azure Search:</span></span>

1. <span data-ttu-id="81ba8-342">**Bedömningen profiler** befordra dokument i hello rangordnas listan över resultat baserat på en uppsättning regler.</span><span class="sxs-lookup"><span data-stu-id="81ba8-342">**Scoring profiles** promote documents in hello ranked list of results based on a set of rules.</span></span> <span data-ttu-id="81ba8-343">I vårt exempel kan vi anser att dokument som matchas i hello rubrikfält mer relevant än de dokument som matchas i beskrivningsfält hello.</span><span class="sxs-lookup"><span data-stu-id="81ba8-343">In our example, we could consider documents that matched in hello title field more relevant than documents that matched in hello description field.</span></span> <span data-ttu-id="81ba8-344">Om indexet har ett fält för varje hotell, kan vi dessutom befordra dokument med lägre pris.</span><span class="sxs-lookup"><span data-stu-id="81ba8-344">Additionally, if our index had a price field for each hotel, we could promote documents with lower price.</span></span> <span data-ttu-id="81ba8-345">Mer information hur för[Lägg till bedömningen profiler tooa sökindex.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span><span class="sxs-lookup"><span data-stu-id="81ba8-345">Learn more how too[add Scoring Profiles tooa search index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span></span>
2. <span data-ttu-id="81ba8-346">**Term den** (endast tillgängligt i hello fullständig Lucene frågesyntaxen) innehåller en operatorn som `^` som kan vara tillämpas tooany en del av hello frågan träd.</span><span class="sxs-lookup"><span data-stu-id="81ba8-346">**Term boosting** (available only in hello Full Lucene query syntax) provides a boosting operator `^` that can be applied tooany part of hello query tree.</span></span> <span data-ttu-id="81ba8-347">I vårt exempel, i stället för att söka på hello prefixet *air-condition*\*, en kan söka efter antingen hello exakta termen *air-condition* hello prefix, men dokument som matchar på hello exakt Termen rankas högre genom att använda förstärkningen toohello termen fråga: *luften villkoret ^ 2. AIR-condition**.</span><span class="sxs-lookup"><span data-stu-id="81ba8-347">In our example, instead of searching on hello prefix *air-condition*\*, one could search for either hello exact term *air-condition* or hello prefix, but documents that match on hello exact term are ranked higher by applying boost toohello term query: *air-condition^2||air-condition**.</span></span> <span data-ttu-id="81ba8-348">Lär dig mer om [term den](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span><span class="sxs-lookup"><span data-stu-id="81ba8-348">Learn more about [term boosting](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span></span>


### <a name="scoring-in-a-distributed-index"></a><span data-ttu-id="81ba8-349">Poängberäkningen i en distribuerad index</span><span class="sxs-lookup"><span data-stu-id="81ba8-349">Scoring in a distributed index</span></span>

<span data-ttu-id="81ba8-350">Alla index i Azure Search är automatiskt dela i flera delar, vilket gör att oss tooquickly distribuera hello index bland flera noder under tjänsten skala upp eller ned.</span><span class="sxs-lookup"><span data-stu-id="81ba8-350">All indexes in Azure Search are automatically split into multiple shards, allowing us tooquickly distribute hello index among multiple nodes during service scale up or scale down.</span></span> <span data-ttu-id="81ba8-351">När en sökbegäran utfärdas mot varje Fragmentera oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="81ba8-351">When a search request is issued, it’s issued against each shard independently.</span></span> <span data-ttu-id="81ba8-352">hello resultat från varje Fragmentera sedan samman och sorterade efter poäng (om ingen ordning har definierats).</span><span class="sxs-lookup"><span data-stu-id="81ba8-352">hello results from each shard are then merged and ordered by score (if no other ordering is defined).</span></span> <span data-ttu-id="81ba8-353">Det är viktigt tooknow som hello bedömningsprofil funktionen vikterna frågan termen frekvens mot inverterade dokumentet frekvensen i alla dokument i hello Fragmentera inte över alla shards!</span><span class="sxs-lookup"><span data-stu-id="81ba8-353">It is important tooknow that hello scoring function weights query term frequency against its inverse document frequency in all documents within hello shard, not across all shards!</span></span>

<span data-ttu-id="81ba8-354">Detta innebär en relevans poäng *kan* vara olika för identiska dokument om de finns på olika delar.</span><span class="sxs-lookup"><span data-stu-id="81ba8-354">This means a relevance score *could* be different for identical documents if they reside on different shards.</span></span> <span data-ttu-id="81ba8-355">Lyckligtvis tenderar skillnaderna toodisappear som hello antalet dokument i hello index växer på grund av toomore även termen distribution.</span><span class="sxs-lookup"><span data-stu-id="81ba8-355">Fortunately, such differences tend toodisappear as hello number of documents in hello index grows due toomore even term distribution.</span></span> <span data-ttu-id="81ba8-356">Det är inte möjligt tooassume på vilka Fragmentera alla dokumentet kommer att placeras.</span><span class="sxs-lookup"><span data-stu-id="81ba8-356">It’s not possible tooassume on which shard any given document will be placed.</span></span> <span data-ttu-id="81ba8-357">Dock förutsatt en Dokumentnyckel inte ändras alltid tilldelas toohello samma Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="81ba8-357">However, assuming a document key doesn't change, it will always be assigned toohello same shard.</span></span>

<span data-ttu-id="81ba8-358">Dokumentet poäng är i allmänhet inte hello bästa attribut för ordning dokument om stabiliteten för ordning är viktig.</span><span class="sxs-lookup"><span data-stu-id="81ba8-358">In general, document score is not hello best attribute for ordering documents if order stability is important.</span></span> <span data-ttu-id="81ba8-359">Till exempel anges två dokument med en identisk poäng, det är inte säkert vilken visas först i efterföljande körningar av hello samma fråga.</span><span class="sxs-lookup"><span data-stu-id="81ba8-359">For example, given two document with an identical score, there is no guarantee which one appears first in subsequent runs of hello same query.</span></span> <span data-ttu-id="81ba8-360">Dokumentet poäng bör bara ge en allmän uppfattning om dokumentet relevans relativa tooother dokument i hello resultatmängd.</span><span class="sxs-lookup"><span data-stu-id="81ba8-360">Document score should only give a general sense of document relevance relative tooother documents in hello results set.</span></span>

## <a name="conclusion"></a><span data-ttu-id="81ba8-361">Slutsats</span><span class="sxs-lookup"><span data-stu-id="81ba8-361">Conclusion</span></span>

<span data-ttu-id="81ba8-362">hello genomförandet av sökmotorer på internet har signalerat förväntningar för textsökning över privata data.</span><span class="sxs-lookup"><span data-stu-id="81ba8-362">hello success of internet search engines has raised expectations for full text search over private data.</span></span> <span data-ttu-id="81ba8-363">För nästan alla typer av sökinställningar nu planerar vi hello motorn toounderstand våra avsikt, även när villkoren är felstavat eller är ofullständig.</span><span class="sxs-lookup"><span data-stu-id="81ba8-363">For almost any kind of search experience, we now expect hello engine toounderstand our intent, even when terms are misspelled or incomplete.</span></span> <span data-ttu-id="81ba8-364">Vi tror även matchar baserat på nära motsvarande uttryck eller synonymer som vi aldrig faktiskt har angetts.</span><span class="sxs-lookup"><span data-stu-id="81ba8-364">We might even expect matches based on near equivalent terms or synonyms that we never actually specified.</span></span>

<span data-ttu-id="81ba8-365">Textsökning är mycket komplex som kräver avancerad språkliga analys och en systematiskt tooprocessing på ett sätt som destillera, expandera och transformera frågan villkoren toodeliver relevanta resultat från en teknisk synvinkel.</span><span class="sxs-lookup"><span data-stu-id="81ba8-365">From a technical standpoint, full text search is highly complex, requiring sophisticated linguistic analysis and a systematic approach tooprocessing in ways that distill, expand, and transform query terms toodeliver a relevant result.</span></span> <span data-ttu-id="81ba8-366">Angivna hello inbyggd svårigheter, finns det många faktorer som kan påverka hello resultatet av en fråga.</span><span class="sxs-lookup"><span data-stu-id="81ba8-366">Given hello inherent complexities, there are a lot of factors that can affect hello outcome of a query.</span></span> <span data-ttu-id="81ba8-367">Därför erbjuder investera hello tid toounderstand hello säkerhetsnivån fulltextsökning faktiska fördelar vid toowork via oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="81ba8-367">For this reason, investing hello time toounderstand hello mechanics of full text search offers tangible benefits when trying toowork through unexpected results.</span></span>  

<span data-ttu-id="81ba8-368">Den här artikeln utforskade fulltextsökning i hello kontexten för Azure Search.</span><span class="sxs-lookup"><span data-stu-id="81ba8-368">This article explored full text search in hello context of Azure Search.</span></span> <span data-ttu-id="81ba8-369">Vi hoppas att du får tillräckligt bakgrund toorecognize möjliga orsaker och lösningar för adressering vanliga problem med frågan.</span><span class="sxs-lookup"><span data-stu-id="81ba8-369">We hope it gives you sufficient background toorecognize potential causes and resolutions for addressing common query problems.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="81ba8-370">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81ba8-370">Next steps</span></span>

+ <span data-ttu-id="81ba8-371">Skapa hello exempel index, prova olika frågor och granska resultatet.</span><span class="sxs-lookup"><span data-stu-id="81ba8-371">Build hello sample index, try out different queries and review results.</span></span> <span data-ttu-id="81ba8-372">Instruktioner finns i [bygga och fråga ett index i hello portal](search-get-started-portal.md#query-index).</span><span class="sxs-lookup"><span data-stu-id="81ba8-372">For instructions, see [Build and query an index in hello portal](search-get-started-portal.md#query-index).</span></span>

+ <span data-ttu-id="81ba8-373">Försök ytterligare frågesyntaxen från hello [Sök dokument](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) exempel avsnittet eller från [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) i Sök explorer i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="81ba8-373">Try additional query syntax from hello [Search Documents](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) example section or from [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in hello portal.</span></span>

+ <span data-ttu-id="81ba8-374">Granska [bedömningen profiler](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) om du vill tootune rangordning i sökprogram.</span><span class="sxs-lookup"><span data-stu-id="81ba8-374">Review [scoring profiles](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) if you want tootune ranking in your search application.</span></span>

+ <span data-ttu-id="81ba8-375">Lär dig hur tooapply [språkspecifika lexikala analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).</span><span class="sxs-lookup"><span data-stu-id="81ba8-375">Learn how tooapply [language-specific lexical analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).</span></span>

+ <span data-ttu-id="81ba8-376">[Konfigurera anpassade analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) för minimal bearbetning eller särskilda bearbetning på specifika fält.</span><span class="sxs-lookup"><span data-stu-id="81ba8-376">[Configure custom analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) for either minimal processing or specialized processing on specific fields.</span></span>

+ <span data-ttu-id="81ba8-377">[Jämför standard och engelska analyzers](http://alice.unearth.ai/)) sida vid sida på den här demo-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="81ba8-377">[Compare standard and English analyzers](http://alice.unearth.ai/)) side-by-side on this demo web site.</span></span> 

## <a name="see-also"></a><span data-ttu-id="81ba8-378">Se även</span><span class="sxs-lookup"><span data-stu-id="81ba8-378">See also</span></span>

[<span data-ttu-id="81ba8-379">Sök dokument REST-API</span><span class="sxs-lookup"><span data-stu-id="81ba8-379">Search Documents REST API</span></span>](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[<span data-ttu-id="81ba8-380">Enkel frågesyntax</span><span class="sxs-lookup"><span data-stu-id="81ba8-380">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[<span data-ttu-id="81ba8-381">Fullständig Lucene frågesyntaxen</span><span class="sxs-lookup"><span data-stu-id="81ba8-381">Full Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[<span data-ttu-id="81ba8-382">Hantera sökresultat</span><span class="sxs-lookup"><span data-stu-id="81ba8-382">Handle search results</span></span>](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
