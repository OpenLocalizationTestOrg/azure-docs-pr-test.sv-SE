---
title: "Fullständig text search engine (Lucene)-arkitekturen i Azure Search | Microsoft Docs"
description: "Förklaring av Lucene frågan bearbetning och dokumentet hämtning begrepp för textsökning som rör Azure Search."
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
ms.openlocfilehash: 9b7adf78271407963ed1d4b34a7760d707b5fc3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a><span data-ttu-id="1b753-103">Hur full textsökning fungerar i Azure Search</span><span class="sxs-lookup"><span data-stu-id="1b753-103">How full text search works in Azure Search</span></span>

<span data-ttu-id="1b753-104">Den här artikeln är för utvecklare som behöver en bättre förståelse av hur Lucene fulltextsökning fungerar i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1b753-104">This article is for developers who need a deeper understanding of how Lucene full text search works in Azure Search.</span></span> <span data-ttu-id="1b753-105">Azure Search levererar sömlöst förväntat resultat i de flesta scenarier för textfrågor, men ibland kan du få ett resultat som verkar vara ”off” på något sätt.</span><span class="sxs-lookup"><span data-stu-id="1b753-105">For text queries, Azure Search will seamlessly deliver expected results in most scenarios, but occasionally you might get a result that seems "off" somehow.</span></span> <span data-ttu-id="1b753-106">I dessa fall har en bakgrund i fyra faser i Lucene Frågekörningen (fråga tolkning lexikala analys, dokumentera matchar bedömningen) kan hjälpa dig att identifiera specifika ändringar av frågeparametrar eller Indexkonfigurationen som ger det önskade resultatet.</span><span class="sxs-lookup"><span data-stu-id="1b753-106">In these situations, having a background in the four stages of Lucene query execution (query parsing, lexical analysis, document matching, scoring) can help you identify specific changes to query parameters or index configuration that will deliver the desired outcome.</span></span> 

> [!Note] 
> <span data-ttu-id="1b753-107">Azure Search använder Lucene för textsökning men Lucene integrering är inte komplett.</span><span class="sxs-lookup"><span data-stu-id="1b753-107">Azure Search uses Lucene for full text search, but Lucene integration is not exhaustive.</span></span> <span data-ttu-id="1b753-108">Vi selektivt exponera och utöka Lucene funktioner för att aktivera scenarierna som är viktigt att Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1b753-108">We selectively expose and extend Lucene functionality to enable the scenarios important to Azure Search.</span></span> 

## <a name="architecture-overview-and-diagram"></a><span data-ttu-id="1b753-109">Översikt över arkitektur och diagram</span><span class="sxs-lookup"><span data-stu-id="1b753-109">Architecture overview and diagram</span></span>

<span data-ttu-id="1b753-110">Bearbetning av en fulltext-sökning börjar med texten frågan om du vill extrahera sökvillkoren.</span><span class="sxs-lookup"><span data-stu-id="1b753-110">Processing a full text search query starts with parsing the query text to extract search terms.</span></span> <span data-ttu-id="1b753-111">Sökmotorn använder ett index för att hämta dokument med matchande villkor.</span><span class="sxs-lookup"><span data-stu-id="1b753-111">The search engine uses an index to retrieve documents with matching terms.</span></span> <span data-ttu-id="1b753-112">Enskilda sökord är ibland uppdelade och färdigställts till nya formulär för att omvandla ett bredare net över vad kan betraktas som en möjlig matchning.</span><span class="sxs-lookup"><span data-stu-id="1b753-112">Individual query terms are sometimes broken down and reconstituted into new forms to cast a broader net over what could be considered as a potential match.</span></span> <span data-ttu-id="1b753-113">Resultatet sorteras sedan efter en relevans poäng som tilldelats varje enskilt matchande dokument.</span><span class="sxs-lookup"><span data-stu-id="1b753-113">A result set is then sorted by a relevance score assigned to each individual matching document.</span></span> <span data-ttu-id="1b753-114">De överst i rankningslista returneras till det anropande programmet.</span><span class="sxs-lookup"><span data-stu-id="1b753-114">Those at the top of the ranked list are returned to the calling application.</span></span>

<span data-ttu-id="1b753-115">Frågekörningen har räknas, fyra steg:</span><span class="sxs-lookup"><span data-stu-id="1b753-115">Restated, query execution has four stages:</span></span> 

1. <span data-ttu-id="1b753-116">Frågan parsning</span><span class="sxs-lookup"><span data-stu-id="1b753-116">Query parsing</span></span> 
2. <span data-ttu-id="1b753-117">Lexikaliskt analys</span><span class="sxs-lookup"><span data-stu-id="1b753-117">Lexical analysis</span></span> 
3. <span data-ttu-id="1b753-118">Hämta dokument</span><span class="sxs-lookup"><span data-stu-id="1b753-118">Document retrieval</span></span> 
4. <span data-ttu-id="1b753-119">Resultatfunktioner</span><span class="sxs-lookup"><span data-stu-id="1b753-119">Scoring</span></span> 

<span data-ttu-id="1b753-120">Diagrammet nedan illustrerar de komponenter som används för att bearbeta en sökbegäran.</span><span class="sxs-lookup"><span data-stu-id="1b753-120">The diagram below illustrates the components used to process a search request.</span></span> 

 ![Arkitekturdiagram för Lucene frågan i Azure Search][1]


| <span data-ttu-id="1b753-122">Nyckelkomponenter</span><span class="sxs-lookup"><span data-stu-id="1b753-122">Key components</span></span> | <span data-ttu-id="1b753-123">Funktionsbeskrivning</span><span class="sxs-lookup"><span data-stu-id="1b753-123">Functional description</span></span> | 
|----------------|------------------------|
|<span data-ttu-id="1b753-124">**Frågan Parser**</span><span class="sxs-lookup"><span data-stu-id="1b753-124">**Query parsers**</span></span> | <span data-ttu-id="1b753-125">Separata sökord från frågeoperatorer och skapa fråga-struktur (en fråga trädet) som ska skickas till sökmotorn.</span><span class="sxs-lookup"><span data-stu-id="1b753-125">Separate query terms from query operators and create the query structure (a query tree) to be sent to the search engine.</span></span> |
|<span data-ttu-id="1b753-126">**Analyzers**</span><span class="sxs-lookup"><span data-stu-id="1b753-126">**Analyzers**</span></span> | <span data-ttu-id="1b753-127">Analysera lexikala sökord.</span><span class="sxs-lookup"><span data-stu-id="1b753-127">Perform lexical analysis on query terms.</span></span> <span data-ttu-id="1b753-128">Den här processen kan omfatta Omforma, ta bort eller expandering av sökord.</span><span class="sxs-lookup"><span data-stu-id="1b753-128">This process can involve transforming, removing, or expanding of query terms.</span></span> |
|<span data-ttu-id="1b753-129">**Index**</span><span class="sxs-lookup"><span data-stu-id="1b753-129">**Index**</span></span> | <span data-ttu-id="1b753-130">En effektiv datastruktur som används för att lagra och organisera sökbara villkoren som har extraherats från indexerade dokument.</span><span class="sxs-lookup"><span data-stu-id="1b753-130">An efficient data structure used to store and organize searchable terms extracted from indexed documents.</span></span> |
|<span data-ttu-id="1b753-131">**Sökmotor**</span><span class="sxs-lookup"><span data-stu-id="1b753-131">**Search engine**</span></span> | <span data-ttu-id="1b753-132">Hämtar och poäng matchande dokument baserat på innehållet i det omvända indexet.</span><span class="sxs-lookup"><span data-stu-id="1b753-132">Retrieves and scores matching documents based on the contents of the inverted index.</span></span> |

## <a name="anatomy-of-a-search-request"></a><span data-ttu-id="1b753-133">En sökbegäran uppbyggnad</span><span class="sxs-lookup"><span data-stu-id="1b753-133">Anatomy of a search request</span></span>

<span data-ttu-id="1b753-134">En sökbegäran är en fullständig specifikation av vad som ska returneras i en resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="1b753-134">A search request is a complete specification of what should be returned in a result set.</span></span> <span data-ttu-id="1b753-135">I enklaste form är en tom fråga med några villkor av något slag.</span><span class="sxs-lookup"><span data-stu-id="1b753-135">In simplest form, it is an empty query with no criteria of any kind.</span></span> <span data-ttu-id="1b753-136">En mer realistisk exempel innehåller parametrar, flera sökord kanske är begränsade till vissa fält med eventuellt filteruttrycket och ordning regler.</span><span class="sxs-lookup"><span data-stu-id="1b753-136">A more realistic example includes parameters, several query terms, perhaps scoped to certain fields, with possibly a filter expression and ordering rules.</span></span>  

<span data-ttu-id="1b753-137">Följande exempel är en sökbegäran som du kan skicka till Azure Search med hjälp av den [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="1b753-137">The following example is a search request you might send to Azure Search using the [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>  

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

<span data-ttu-id="1b753-138">För denna begäran gör sökmotorn följande:</span><span class="sxs-lookup"><span data-stu-id="1b753-138">For this request, the search engine does the following:</span></span>

1. <span data-ttu-id="1b753-139">Filtrerar ut dokument där priset är minst $60 och mindre än 300 USD.</span><span class="sxs-lookup"><span data-stu-id="1b753-139">Filters out documents where the price is at least $60 and less than $300.</span></span>
2. <span data-ttu-id="1b753-140">Kör frågan.</span><span class="sxs-lookup"><span data-stu-id="1b753-140">Executes the query.</span></span> <span data-ttu-id="1b753-141">I det här exemplet sökfrågan består av fraser och villkor: `"Spacious, air-condition* +\"Ocean view\""` (användarna vanligtvis inte ange skiljetecken, men inklusive exempel kan vi förklarar hur analyzers hantera).</span><span class="sxs-lookup"><span data-stu-id="1b753-141">In this example, the search query consists of phrases and terms: `"Spacious, air-condition* +\"Ocean view\""` (users typically don't enter punctuation, but including it in the example allows us to explain how analyzers handle it).</span></span> <span data-ttu-id="1b753-142">För den här frågan sökmotorn igenom beskrivningen och rubrikfält anges i `searchFields` för dokument som innehåller ”oceanen vyn”, och dessutom på termen ”stora”, eller på villkor som börjar med prefixet ”air-condition”.</span><span class="sxs-lookup"><span data-stu-id="1b753-142">For this query, the search engine scans the description and title fields specified in `searchFields` for documents that contain "Ocean view", and additionally on the term "spacious", or on terms that start with the prefix "air-condition".</span></span> <span data-ttu-id="1b753-143">Den `searchMode` används för att matcha något villkor (standard) eller alla, i de fall där en term som inte krävs uttryckligen (`+`).</span><span class="sxs-lookup"><span data-stu-id="1b753-143">The `searchMode` parameter is used to match on any term (default) or all of them, for cases where a term is not explicitly required (`+`).</span></span>
3. <span data-ttu-id="1b753-144">Sorterar den resulterande uppsättningen hotell av närhet till en viss geografisk plats och sedan tillbaka till det anropande programmet.</span><span class="sxs-lookup"><span data-stu-id="1b753-144">Orders the resulting set of hotels by proximity to a given geography location, and then returned to the calling application.</span></span> 

<span data-ttu-id="1b753-145">Merparten av den här artikeln handlar om bearbetningen av den *sökfråga*: `"Spacious, air-condition* +\"Ocean view\""`.</span><span class="sxs-lookup"><span data-stu-id="1b753-145">The majority of this article is about processing of the *search query*: `"Spacious, air-condition* +\"Ocean view\""`.</span></span> <span data-ttu-id="1b753-146">Filtrera och sortera ligger utanför omfånget.</span><span class="sxs-lookup"><span data-stu-id="1b753-146">Filtering and ordering are out of scope.</span></span> <span data-ttu-id="1b753-147">Mer information finns i [Sök API-referensdokumentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="1b753-147">For more information, see the [Search API reference documentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a><span data-ttu-id="1b753-148">Steg 1: Frågan parsning</span><span class="sxs-lookup"><span data-stu-id="1b753-148">Stage 1: Query parsing</span></span> 

<span data-ttu-id="1b753-149">Enligt nedanstående är frågesträngen den första raden för begäran:</span><span class="sxs-lookup"><span data-stu-id="1b753-149">As noted, the query string is the first line of the request:</span></span> 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

<span data-ttu-id="1b753-150">Frågeparsern avgränsar operatorer (t.ex `*` och `+` i exemplet) från sökning termer och deconstructs frågan i *underfrågor* av en typ som stöds:</span><span class="sxs-lookup"><span data-stu-id="1b753-150">The query parser separates operators (such as `*` and `+` in the example) from search terms, and deconstructs the search query into *subqueries* of a supported type:</span></span> 

+ <span data-ttu-id="1b753-151">*Termen frågan* för fristående villkor (till exempel stora)</span><span class="sxs-lookup"><span data-stu-id="1b753-151">*term query* for standalone terms (like spacious)</span></span>
+ <span data-ttu-id="1b753-152">*frasen frågan* för angiven villkor (till exempel visa oceanen)</span><span class="sxs-lookup"><span data-stu-id="1b753-152">*phrase query* for quoted terms (like ocean view)</span></span>
+ <span data-ttu-id="1b753-153">*prefixet frågan* för termer följt av ett prefix-operatorn `*` (t.ex. air-condition)</span><span class="sxs-lookup"><span data-stu-id="1b753-153">*prefix query* for terms followed by a prefix operator `*` (like air-condition)</span></span>

<span data-ttu-id="1b753-154">En fullständig lista över stöds frågetyper finns [Lucene fråga sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span><span class="sxs-lookup"><span data-stu-id="1b753-154">For a full list of supported query types see [Lucene query sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span></span>

<span data-ttu-id="1b753-155">Operatörer som är associerade med en underfråga avgöra om frågan ”måste vara” eller ”ska vara” uppfyllas för att ett dokument för att ses som en matchning.</span><span class="sxs-lookup"><span data-stu-id="1b753-155">Operators associated with a subquery determine whether the query "must be" or "should be" satisfied in order for a document to be considered a match.</span></span> <span data-ttu-id="1b753-156">Till exempel `+"Ocean view"` är ”måste” på grund av att den `+` operator.</span><span class="sxs-lookup"><span data-stu-id="1b753-156">For example, `+"Ocean view"` is "must" due to the `+` operator.</span></span> 

<span data-ttu-id="1b753-157">Frågeparsern omstrukturerar underfrågor i en *frågan trädet* (en intern struktur som representerar frågan) förmedlar sökmotor.</span><span class="sxs-lookup"><span data-stu-id="1b753-157">The query parser restructures the subqueries into a *query tree* (an internal structure representing the query) it passes on to the search engine.</span></span> <span data-ttu-id="1b753-158">I det första steget i frågan parsning trädet frågan ser ut så här.</span><span class="sxs-lookup"><span data-stu-id="1b753-158">In the first stage of query parsing, the query tree looks like this.</span></span>  

 ![Boolesk fråga searchmode alla][2]

### <a name="supported-parsers-simple-and-full-lucene"></a><span data-ttu-id="1b753-160">Stöd för Parser: enkelt och fullständig Lucene</span><span class="sxs-lookup"><span data-stu-id="1b753-160">Supported parsers: Simple and Full Lucene</span></span> 

 <span data-ttu-id="1b753-161">Azure Search visar två olika frågespråk `simple` (standard) och `full`.</span><span class="sxs-lookup"><span data-stu-id="1b753-161">Azure Search exposes two different query languages, `simple` (default) and `full`.</span></span> <span data-ttu-id="1b753-162">Genom att ange den `queryType` parameter med din begäran, anger du frågeparsern vilka frågespråket du väljer så att den vet hur du tolkar operatorer och syntax.</span><span class="sxs-lookup"><span data-stu-id="1b753-162">By setting the `queryType` parameter with your search request, you tell the query parser which query language you choose so that it knows how to interpret the operators and syntax.</span></span> <span data-ttu-id="1b753-163">Den [enkel fråga språk](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) är intuitivt och stabil, ofta lämpligt att tolka indata från användaren som-är utan klientsidan bearbetning.</span><span class="sxs-lookup"><span data-stu-id="1b753-163">The [Simple query langauge](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) is intuitive and robust, often suitable to interpret user input as-is without client-side processing.</span></span> <span data-ttu-id="1b753-164">Det stöder frågeoperatorer bekant från sökmotorer.</span><span class="sxs-lookup"><span data-stu-id="1b753-164">It supports query operators familiar from web search engines.</span></span> <span data-ttu-id="1b753-165">Den [fullständig Lucene frågespråk](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), som du får genom att ange `queryType=full`, utökar standardspråk för enkel fråga genom att lägga till stöd för flera operatorer och frågetyper som jokertecken, fuzzy regex och fältet omfång frågor.</span><span class="sxs-lookup"><span data-stu-id="1b753-165">The [Full Lucene query language](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), which you get by setting `queryType=full`, extends the default Simple query language by adding support for more operators and query types like wildcard, fuzzy, regex, and field-scoped queries.</span></span> <span data-ttu-id="1b753-166">Till exempel skulle ett reguljärt uttryck som skickas i enkla frågesyntaxen tolkas som en frågesträng och inte ett uttryck.</span><span class="sxs-lookup"><span data-stu-id="1b753-166">For example, a regular expression sent in Simple query syntax would be interpreted as a query string and not an expression.</span></span> <span data-ttu-id="1b753-167">Exempelbegäran i den här artikeln använder frågespråket fullständig Lucene.</span><span class="sxs-lookup"><span data-stu-id="1b753-167">The example request in this article uses the Full Lucene query language.</span></span>

### <a name="impact-of-searchmode-on-the-parser"></a><span data-ttu-id="1b753-168">Effekten av searchMode i tolken</span><span class="sxs-lookup"><span data-stu-id="1b753-168">Impact of searchMode on the parser</span></span> 

<span data-ttu-id="1b753-169">En annan begäran sökparameter som påverkar parsning är den `searchMode` parameter.</span><span class="sxs-lookup"><span data-stu-id="1b753-169">Another search request parameter that affects parsing is the `searchMode` parameter.</span></span> <span data-ttu-id="1b753-170">Den styr operatorn standard för booleskt frågor: en (standard) eller alla.</span><span class="sxs-lookup"><span data-stu-id="1b753-170">It controls the default operator for Boolean queries: any (default) or all.</span></span>  

<span data-ttu-id="1b753-171">När `searchMode=any`, som är standard, utrymme avgränsare mellan stora och air-condition är eller (`||`), gör frågan exempeltexten motsvarar:</span><span class="sxs-lookup"><span data-stu-id="1b753-171">When `searchMode=any`, which is the default, the space delimiter between spacious and air-condition is OR (`||`), making the sample query text equivalent to:</span></span> 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

<span data-ttu-id="1b753-172">Explicit operatorer som `+` i `+"Ocean view"`, är entydigt i Boolesk fråga konstruktion (termen *måste* matchar).</span><span class="sxs-lookup"><span data-stu-id="1b753-172">Explicit operators, such as `+` in `+"Ocean view"`, are unambiguous in boolean query construction (the term *must* match).</span></span> <span data-ttu-id="1b753-173">Lika självklara är hur du tolkar återstående villkoren: stora och air-condition.</span><span class="sxs-lookup"><span data-stu-id="1b753-173">Less obvious is how to interpret the remaining terms: spacious and air-condition.</span></span> <span data-ttu-id="1b753-174">Bör sökfunktionen hitta matchningar för oceanen vyn *och* stora *och* air-condition?</span><span class="sxs-lookup"><span data-stu-id="1b753-174">Should the search engine find matches on ocean view *and* spacious *and* air-condition?</span></span> <span data-ttu-id="1b753-175">Eller bör hitta oceanen visa plus *antingen en* återstående villkor?</span><span class="sxs-lookup"><span data-stu-id="1b753-175">Or should it find ocean view plus *either one* of the remaining terms?</span></span> 

<span data-ttu-id="1b753-176">Som standard (`searchMode=any`), sökmotorn förutsätter bredare tolkning.</span><span class="sxs-lookup"><span data-stu-id="1b753-176">By default (`searchMode=any`), the search engine assumes the broader interpretation.</span></span> <span data-ttu-id="1b753-177">Antingen fältet *bör* matchas, reflektion ”eller” semantik.</span><span class="sxs-lookup"><span data-stu-id="1b753-177">Either field *should* be matched, reflecting "or" semantics.</span></span> <span data-ttu-id="1b753-178">Första fråga trädet visas tidigare, med två ”bör” operations, visas standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="1b753-178">The initial query tree illustrated previously, with the two "should" operations, shows the default.</span></span>  

<span data-ttu-id="1b753-179">Anta att vi nu ställer `searchMode=all`.</span><span class="sxs-lookup"><span data-stu-id="1b753-179">Suppose that we now set `searchMode=all`.</span></span> <span data-ttu-id="1b753-180">I det här fallet tolkas området som en ”och”-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="1b753-180">In this case, the space is interpreted as an "and" operation.</span></span> <span data-ttu-id="1b753-181">Var och en av de återstående villkor måste båda vara finns i dokumentet så att en matchning.</span><span class="sxs-lookup"><span data-stu-id="1b753-181">Each of the remaining terms must both be present in the document to qualify as a match.</span></span> <span data-ttu-id="1b753-182">Den resulterande exempelfråga skulle tolkas enligt följande:</span><span class="sxs-lookup"><span data-stu-id="1b753-182">The resulting sample query would be interpreted as follows:</span></span> 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

<span data-ttu-id="1b753-183">Ett ändrade frågan träd för den här frågan skulle vara följande, där en matchande dokumentet är skärningspunkten för alla tre underfrågor:</span><span class="sxs-lookup"><span data-stu-id="1b753-183">A modified query tree for this query would be as follows, where a matching document is the intersection of all three subqueries:</span></span> 

 ![Boolesk fråga searchmode alla][3]

> [!Note] 
> <span data-ttu-id="1b753-185">Om du väljer `searchMode=any` över `searchMode=all` beslut bästa erhålls genom att köra representativt frågor.</span><span class="sxs-lookup"><span data-stu-id="1b753-185">Choosing `searchMode=any` over `searchMode=all` is a decision best arrived at by running representative queries.</span></span> <span data-ttu-id="1b753-186">Användare som är sannolikt att innefatta operatorer (common när sökning dokumentet lagrar) kan vara resultatet intuitivt om `searchMode=all` informerar Boolesk fråga konstruktioner.</span><span class="sxs-lookup"><span data-stu-id="1b753-186">Users who are likely to include operators (common when searching document stores) might find results more intuitive if `searchMode=all` informs boolean query constructs.</span></span> <span data-ttu-id="1b753-187">Mer information om samspelet mellan `searchMode` och operatorer, se [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span><span class="sxs-lookup"><span data-stu-id="1b753-187">For more about the interplay between `searchMode` and operators, see [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span></span>

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a><span data-ttu-id="1b753-188">Steg 2: Lexikaliskt analys</span><span class="sxs-lookup"><span data-stu-id="1b753-188">Stage 2: Lexical analysis</span></span> 

<span data-ttu-id="1b753-189">Lexikaliskt analyzers processen *term frågor* och *fras frågor* när frågan trädet är strukturerad.</span><span class="sxs-lookup"><span data-stu-id="1b753-189">Lexical analyzers process *term queries* and *phrase queries* after the query tree is structured.</span></span> <span data-ttu-id="1b753-190">En analyzer accepterar text indata som anges av tolken, bearbetar text och sedan skickar tillbaka tokeniserad villkoren ingå i trädet frågan.</span><span class="sxs-lookup"><span data-stu-id="1b753-190">An analyzer accepts the text inputs given to it by the parser, processes the text, and then sends back tokenized terms to be incorporated into the query tree.</span></span> 

<span data-ttu-id="1b753-191">Den vanligaste formen av lexikala analys är *språkliga analysis* som transformeringar fråga villkor baserat på regler som är specifika för ett visst språk:</span><span class="sxs-lookup"><span data-stu-id="1b753-191">The most common form of lexical analysis is *linguistic analysis* which transforms query terms based on rules specific to a given language:</span></span> 

* <span data-ttu-id="1b753-192">Att minska en frågeterm rot form av ett ord.</span><span class="sxs-lookup"><span data-stu-id="1b753-192">Reducing a query term to the root form of a word</span></span> 
* <span data-ttu-id="1b753-193">Ta bort irrelevanta ord (stoppord, till exempel ”i” eller ”och” på engelska)</span><span class="sxs-lookup"><span data-stu-id="1b753-193">Removing non-essential words (stopwords, such as "the" or "and" in English)</span></span> 
* <span data-ttu-id="1b753-194">Dela upp ett sammansatt ord i komponenter</span><span class="sxs-lookup"><span data-stu-id="1b753-194">Breaking a composite word into component parts</span></span> 
* <span data-ttu-id="1b753-195">Lägre skiftläge ett ord med versal</span><span class="sxs-lookup"><span data-stu-id="1b753-195">Lower casing an upper case word</span></span> 

<span data-ttu-id="1b753-196">Alla dessa åtgärder tenderar att radera skillnader mellan text indata som angetts av användaren och villkoren lagras i indexet.</span><span class="sxs-lookup"><span data-stu-id="1b753-196">All of these operations tend to erase differences between the text input provided by the user and the terms stored in the index.</span></span> <span data-ttu-id="1b753-197">Dessa åtgärder utöver text bearbetning och kräver avancerade kunskaper om språket sig själv.</span><span class="sxs-lookup"><span data-stu-id="1b753-197">Such operations go beyond text processing and require in-depth knowledge of the language itself.</span></span> <span data-ttu-id="1b753-198">Om du vill lägga till det här lagret av språkliga medvetenhet Azure Search har stöd för en lång lista med [språkanalys](https://docs.microsoft.com/rest/api/searchservice/language-support) från både Lucene och Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1b753-198">To add this layer of linguistic awareness, Azure Search supports a long list of [language analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support) from both Lucene and Microsoft.</span></span>

> [!Note]
> <span data-ttu-id="1b753-199">Analys-kraven kan variera mellan minimal till avancerade beroende på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="1b753-199">Analysis requirements can range from minimal to elaborate depending on your scenario.</span></span> <span data-ttu-id="1b753-200">Du kan styra komplexitet lexikala analys genom att välja något av de fördefinierade analyzers eller skapa egna [anpassade analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="1b753-200">You can control complexity of lexical analysis by the selecting one of the predefined analyzers or by creating your own [custom analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span></span> <span data-ttu-id="1b753-201">Analyzers är begränsade till sökbara fält och anges som en del av en fältdefinition av.</span><span class="sxs-lookup"><span data-stu-id="1b753-201">Analyzers are scoped to searchable fields and are specified as part of a field definition.</span></span> <span data-ttu-id="1b753-202">På så sätt kan du variera lexikala analys på grundval av per fält.</span><span class="sxs-lookup"><span data-stu-id="1b753-202">This allows you to vary lexical analysis on a per-field basis.</span></span> <span data-ttu-id="1b753-203">Måste den *standard* Lucene analyzer används.</span><span class="sxs-lookup"><span data-stu-id="1b753-203">Unspecified, the *standard* Lucene analyzer is used.</span></span>

<span data-ttu-id="1b753-204">I vårt exempel före analys, har trädet första fråga termen ”Spacious” med versaler ”S” och ett kommatecken frågeparsern tolkas som en del av frågeterm (kommatecken inte anses vara ansvarig query language).</span><span class="sxs-lookup"><span data-stu-id="1b753-204">In our example, prior to analysis, the initial query tree has the term "Spacious," with an uppercase "S" and a comma that the query parser interprets as a part of the query term (a comma is not considered a query language operator).</span></span>  

<span data-ttu-id="1b753-205">När standard analyzer bearbetar termen, kommer den gemener ”oceanen view” och ”stora” och ta bort tecknet kommatecken.</span><span class="sxs-lookup"><span data-stu-id="1b753-205">When the default analyzer processes the term, it will lowercase "ocean view" and "spacious", and remove the comma character.</span></span> <span data-ttu-id="1b753-206">Trädet ändrade frågan ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="1b753-206">The modified query tree will look as follows:</span></span> 

 ![Boolesk fråga med analyserade villkor][4]

### <a name="testing-analyzer-behaviors"></a><span data-ttu-id="1b753-208">Testa analyzer beteenden</span><span class="sxs-lookup"><span data-stu-id="1b753-208">Testing analyzer behaviors</span></span> 

<span data-ttu-id="1b753-209">Beteendet för en analyzer kan testas med hjälp av den [analysera API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span><span class="sxs-lookup"><span data-stu-id="1b753-209">The behavior of an analyzer can be tested using the [Analyze API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span></span> <span data-ttu-id="1b753-210">Ange den text som du vill analysera för att se villkoren som angetts analyzer genererar.</span><span class="sxs-lookup"><span data-stu-id="1b753-210">Provide the text you want to analyze to see what terms given analyzer will generate.</span></span> <span data-ttu-id="1b753-211">Om du vill se hur standard analyzer skulle bearbetas i texten ”air-condition”, till exempel kan du utfärda följande begäran:</span><span class="sxs-lookup"><span data-stu-id="1b753-211">For example, to see how the standard analyzer would process the text "air-condition", you can issue the following request:</span></span>

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

<span data-ttu-id="1b753-212">Standard analyzer delar indatatexten i följande två variabler, lägga till anteckningar med attribut som start och end förskjutningarna (används för träffar syntaxmarkering) samt deras position (används för frasen matchar):</span><span class="sxs-lookup"><span data-stu-id="1b753-212">The standard analyzer breaks the input text into the following two tokens, annotating them with attributes like start and end offsets (used for hit highlighting) as well as their position (used for phrase matching):</span></span>

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

### <a name="exceptions-to-lexical-analysis"></a><span data-ttu-id="1b753-213">Undantag till lexikala analys</span><span class="sxs-lookup"><span data-stu-id="1b753-213">Exceptions to lexical analysis</span></span> 

<span data-ttu-id="1b753-214">Lexikaliskt analys gäller enbart för frågetyper som kräver fullständiga villkor – en term fråga eller en frasfråga.</span><span class="sxs-lookup"><span data-stu-id="1b753-214">Lexical analysis applies only to query types that require complete terms – either a term query or a phrase query.</span></span> <span data-ttu-id="1b753-215">Den gäller inte frågetyper med ofullständiga villkor – prefix fråga, jokerteckenfråga med, regex frågan – eller en fuzzy fråga.</span><span class="sxs-lookup"><span data-stu-id="1b753-215">It doesn’t apply to query types with incomplete terms – prefix query, wildcard query, regex query – or to a fuzzy query.</span></span> <span data-ttu-id="1b753-216">De fråga typer, inklusive prefix frågan med termen *air-condition\**  i vårt exempel läggs direkt till trädet frågan kringgå fasen analys.</span><span class="sxs-lookup"><span data-stu-id="1b753-216">Those query types, including the prefix query with term *air-condition\** in our example, are added directly to the query tree, bypassing the analysis stage.</span></span> <span data-ttu-id="1b753-217">Endast omvandling utförs på sökord dessa typer av lowercasing.</span><span class="sxs-lookup"><span data-stu-id="1b753-217">The only transformation performed on query terms of those types is lowercasing.</span></span>

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a><span data-ttu-id="1b753-218">Steg 3: Hämta för dokument</span><span class="sxs-lookup"><span data-stu-id="1b753-218">Stage 3: Document retrieval</span></span> 

<span data-ttu-id="1b753-219">Hämta dokument refererar till att söka efter dokument med matchande termer i indexet.</span><span class="sxs-lookup"><span data-stu-id="1b753-219">Document retrieval refers to finding documents with matching terms in the index.</span></span> <span data-ttu-id="1b753-220">Det här steget är att förstå bäst genom ett exempel.</span><span class="sxs-lookup"><span data-stu-id="1b753-220">This stage is understood best through an example.</span></span> <span data-ttu-id="1b753-221">Låt oss börja med ett hotell index med följande enkla schema:</span><span class="sxs-lookup"><span data-stu-id="1b753-221">Let's start with a hotels index having the following simple schema:</span></span> 

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

<span data-ttu-id="1b753-222">Anta vidare att indexet innehåller följande fyra dokument:</span><span class="sxs-lookup"><span data-stu-id="1b753-222">Further assume that this index contains the following four documents:</span></span> 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance to the beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on the north shore of the island of Kauaʻi. Ocean view."     
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

<span data-ttu-id="1b753-223">**Hur villkoren indexeras**</span><span class="sxs-lookup"><span data-stu-id="1b753-223">**How terms are indexed**</span></span>

<span data-ttu-id="1b753-224">För att förstå hämtning, hjälper det att du känner till några grunderna om indexering.</span><span class="sxs-lookup"><span data-stu-id="1b753-224">To understand retrieval, it helps to know a few basics about indexing.</span></span> <span data-ttu-id="1b753-225">Lagringsenheten är en inverterad index, en för varje sökbara fält.</span><span class="sxs-lookup"><span data-stu-id="1b753-225">The unit of storage is an inverted index, one for each searchable field.</span></span> <span data-ttu-id="1b753-226">Är en sorterad lista över alla villkor från alla dokument i ett inverterad index.</span><span class="sxs-lookup"><span data-stu-id="1b753-226">Within an inverted index is a sorted list of all terms from all documents.</span></span> <span data-ttu-id="1b753-227">Varje term mappar i listan över dokument som det uppstår, som tydligt i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="1b753-227">Each term maps to the list of documents in which it occurs, as evident in the example below.</span></span>

<span data-ttu-id="1b753-228">För att skapa villkoren i inverterad index utför sökmotorn lexikala analys över innehållet i dokument som liknar vad som händer under frågebearbetningen.</span><span class="sxs-lookup"><span data-stu-id="1b753-228">To produce the terms in an inverted index, the search engine performs lexical analysis over the content of documents, similar to what happens during query processing.</span></span> <span data-ttu-id="1b753-229">Text indata skickas till en analyzer lägre-alltid demontering av skiljetecken och så vidare, beroende på hur analyzer.</span><span class="sxs-lookup"><span data-stu-id="1b753-229">Text inputs are passed to an analyzer, lower-cased, stripped of punctuation, and so forth, depending on the analyzer configuration.</span></span> <span data-ttu-id="1b753-230">Det är vanligt, men krävs inte, att använda samma analyzers för sökning och indexering åtgärder så att sökord påminner mer villkoren i indexet.</span><span class="sxs-lookup"><span data-stu-id="1b753-230">It's common, but not required, to use the same analyzers for search and indexing operations so that query terms look more like terms inside the index.</span></span>

> [!Note]
> <span data-ttu-id="1b753-231">Azure Search kan du ange olika analyzers för indexering och söka via ytterligare `indexAnalyzer` och `searchAnalyzer` fältet parametrar.</span><span class="sxs-lookup"><span data-stu-id="1b753-231">Azure Search lets you specify different analyzers for indexing and search via additional `indexAnalyzer` and `searchAnalyzer` field parameters.</span></span> <span data-ttu-id="1b753-232">Om inget anges analyzer anges med den `analyzer` egenskapen används för både indexering och sökning.</span><span class="sxs-lookup"><span data-stu-id="1b753-232">If unspecified, the analyzer set with the `analyzer` property is used for both indexing and searching.</span></span>  

<span data-ttu-id="1b753-233">**Omvända index för dokument**</span><span class="sxs-lookup"><span data-stu-id="1b753-233">**Inverted index for example documents**</span></span>

<span data-ttu-id="1b753-234">Gå tillbaka till våra exempel, för den **rubrik** fältet inverterad indexet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="1b753-234">Returning to our example, for the **title** field, the inverted index looks like this:</span></span>

| <span data-ttu-id="1b753-235">Period</span><span class="sxs-lookup"><span data-stu-id="1b753-235">Term</span></span> | <span data-ttu-id="1b753-236">Listan över</span><span class="sxs-lookup"><span data-stu-id="1b753-236">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="1b753-237">atman</span><span class="sxs-lookup"><span data-stu-id="1b753-237">atman</span></span> | <span data-ttu-id="1b753-238">1</span><span class="sxs-lookup"><span data-stu-id="1b753-238">1</span></span> |
| <span data-ttu-id="1b753-239">stranden</span><span class="sxs-lookup"><span data-stu-id="1b753-239">beach</span></span> | <span data-ttu-id="1b753-240">2</span><span class="sxs-lookup"><span data-stu-id="1b753-240">2</span></span> |
| <span data-ttu-id="1b753-241">hotell</span><span class="sxs-lookup"><span data-stu-id="1b753-241">hotel</span></span> | <span data-ttu-id="1b753-242">1, 3</span><span class="sxs-lookup"><span data-stu-id="1b753-242">1, 3</span></span> |
| <span data-ttu-id="1b753-243">oceanen</span><span class="sxs-lookup"><span data-stu-id="1b753-243">ocean</span></span> | <span data-ttu-id="1b753-244">4</span><span class="sxs-lookup"><span data-stu-id="1b753-244">4</span></span>  |
| <span data-ttu-id="1b753-245">playa</span><span class="sxs-lookup"><span data-stu-id="1b753-245">playa</span></span> | <span data-ttu-id="1b753-246">3</span><span class="sxs-lookup"><span data-stu-id="1b753-246">3</span></span> |
| <span data-ttu-id="1b753-247">utväg</span><span class="sxs-lookup"><span data-stu-id="1b753-247">resort</span></span> | <span data-ttu-id="1b753-248">3</span><span class="sxs-lookup"><span data-stu-id="1b753-248">3</span></span> |
| <span data-ttu-id="1b753-249">Återställ format</span><span class="sxs-lookup"><span data-stu-id="1b753-249">retreat</span></span> | <span data-ttu-id="1b753-250">4</span><span class="sxs-lookup"><span data-stu-id="1b753-250">4</span></span> |

<span data-ttu-id="1b753-251">I rubrikfältet, endast *hotell* visas i två dokument: 1, 3.</span><span class="sxs-lookup"><span data-stu-id="1b753-251">In the title field, only *hotel* shows up in two documents: 1, 3.</span></span>

<span data-ttu-id="1b753-252">För den **beskrivning** fältet indexet är följande:</span><span class="sxs-lookup"><span data-stu-id="1b753-252">For the **description** field, the index is as follows:</span></span>

| <span data-ttu-id="1b753-253">Period</span><span class="sxs-lookup"><span data-stu-id="1b753-253">Term</span></span> | <span data-ttu-id="1b753-254">Listan över</span><span class="sxs-lookup"><span data-stu-id="1b753-254">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="1b753-255">luften</span><span class="sxs-lookup"><span data-stu-id="1b753-255">air</span></span> | <span data-ttu-id="1b753-256">3</span><span class="sxs-lookup"><span data-stu-id="1b753-256">3</span></span>
| <span data-ttu-id="1b753-257">och</span><span class="sxs-lookup"><span data-stu-id="1b753-257">and</span></span> | <span data-ttu-id="1b753-258">4</span><span class="sxs-lookup"><span data-stu-id="1b753-258">4</span></span>
| <span data-ttu-id="1b753-259">stranden</span><span class="sxs-lookup"><span data-stu-id="1b753-259">beach</span></span> | <span data-ttu-id="1b753-260">1</span><span class="sxs-lookup"><span data-stu-id="1b753-260">1</span></span>
| <span data-ttu-id="1b753-261">villkor</span><span class="sxs-lookup"><span data-stu-id="1b753-261">conditioned</span></span> | <span data-ttu-id="1b753-262">3</span><span class="sxs-lookup"><span data-stu-id="1b753-262">3</span></span>
| <span data-ttu-id="1b753-263">fria</span><span class="sxs-lookup"><span data-stu-id="1b753-263">comfortable</span></span> | <span data-ttu-id="1b753-264">3</span><span class="sxs-lookup"><span data-stu-id="1b753-264">3</span></span>
| <span data-ttu-id="1b753-265">Avstånd</span><span class="sxs-lookup"><span data-stu-id="1b753-265">distance</span></span> | <span data-ttu-id="1b753-266">1</span><span class="sxs-lookup"><span data-stu-id="1b753-266">1</span></span>
| <span data-ttu-id="1b753-267">dataö</span><span class="sxs-lookup"><span data-stu-id="1b753-267">island</span></span> | <span data-ttu-id="1b753-268">2</span><span class="sxs-lookup"><span data-stu-id="1b753-268">2</span></span>
| <span data-ttu-id="1b753-269">kauaʻi</span><span class="sxs-lookup"><span data-stu-id="1b753-269">kauaʻi</span></span> | <span data-ttu-id="1b753-270">2</span><span class="sxs-lookup"><span data-stu-id="1b753-270">2</span></span>
| <span data-ttu-id="1b753-271">finns</span><span class="sxs-lookup"><span data-stu-id="1b753-271">located</span></span> | <span data-ttu-id="1b753-272">2</span><span class="sxs-lookup"><span data-stu-id="1b753-272">2</span></span>
| <span data-ttu-id="1b753-273">Nord</span><span class="sxs-lookup"><span data-stu-id="1b753-273">north</span></span> | <span data-ttu-id="1b753-274">2</span><span class="sxs-lookup"><span data-stu-id="1b753-274">2</span></span>
| <span data-ttu-id="1b753-275">oceanen</span><span class="sxs-lookup"><span data-stu-id="1b753-275">ocean</span></span> | <span data-ttu-id="1b753-276">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="1b753-276">1, 2, 3</span></span>
| <span data-ttu-id="1b753-277">av</span><span class="sxs-lookup"><span data-stu-id="1b753-277">of</span></span> | <span data-ttu-id="1b753-278">2</span><span class="sxs-lookup"><span data-stu-id="1b753-278">2</span></span>
| <span data-ttu-id="1b753-279">på</span><span class="sxs-lookup"><span data-stu-id="1b753-279">on</span></span> |<span data-ttu-id="1b753-280">2</span><span class="sxs-lookup"><span data-stu-id="1b753-280">2</span></span>
| <span data-ttu-id="1b753-281">Tyst</span><span class="sxs-lookup"><span data-stu-id="1b753-281">quiet</span></span> | <span data-ttu-id="1b753-282">4</span><span class="sxs-lookup"><span data-stu-id="1b753-282">4</span></span>
| <span data-ttu-id="1b753-283">lokaler</span><span class="sxs-lookup"><span data-stu-id="1b753-283">rooms</span></span>  | <span data-ttu-id="1b753-284">1, 3</span><span class="sxs-lookup"><span data-stu-id="1b753-284">1, 3</span></span>
| <span data-ttu-id="1b753-285">secluded</span><span class="sxs-lookup"><span data-stu-id="1b753-285">secluded</span></span> | <span data-ttu-id="1b753-286">4</span><span class="sxs-lookup"><span data-stu-id="1b753-286">4</span></span>
| <span data-ttu-id="1b753-287">land</span><span class="sxs-lookup"><span data-stu-id="1b753-287">shore</span></span> | <span data-ttu-id="1b753-288">2</span><span class="sxs-lookup"><span data-stu-id="1b753-288">2</span></span>
| <span data-ttu-id="1b753-289">stora</span><span class="sxs-lookup"><span data-stu-id="1b753-289">spacious</span></span> | <span data-ttu-id="1b753-290">1</span><span class="sxs-lookup"><span data-stu-id="1b753-290">1</span></span>
| <span data-ttu-id="1b753-291">den</span><span class="sxs-lookup"><span data-stu-id="1b753-291">the</span></span> | <span data-ttu-id="1b753-292">1, 2</span><span class="sxs-lookup"><span data-stu-id="1b753-292">1, 2</span></span>
| <span data-ttu-id="1b753-293">till</span><span class="sxs-lookup"><span data-stu-id="1b753-293">to</span></span> | <span data-ttu-id="1b753-294">1</span><span class="sxs-lookup"><span data-stu-id="1b753-294">1</span></span>
| <span data-ttu-id="1b753-295">vy</span><span class="sxs-lookup"><span data-stu-id="1b753-295">view</span></span> | <span data-ttu-id="1b753-296">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="1b753-296">1, 2, 3</span></span>
| <span data-ttu-id="1b753-297">Gå</span><span class="sxs-lookup"><span data-stu-id="1b753-297">walking</span></span> | <span data-ttu-id="1b753-298">1</span><span class="sxs-lookup"><span data-stu-id="1b753-298">1</span></span>
| <span data-ttu-id="1b753-299">med</span><span class="sxs-lookup"><span data-stu-id="1b753-299">with</span></span> | <span data-ttu-id="1b753-300">3</span><span class="sxs-lookup"><span data-stu-id="1b753-300">3</span></span>


<span data-ttu-id="1b753-301">**Matchande sökord mot indexerade ord**</span><span class="sxs-lookup"><span data-stu-id="1b753-301">**Matching query terms against indexed terms**</span></span>

<span data-ttu-id="1b753-302">Omvända indexen ovan, vi gå tillbaka till exempel frågan och se hur matchande dokument hittades för våra exempelfråga.</span><span class="sxs-lookup"><span data-stu-id="1b753-302">Given the inverted indices above, let’s return to the sample query and see how matching documents are found for our example query.</span></span> <span data-ttu-id="1b753-303">Kom ihåg att trädet slutlig ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="1b753-303">Recall that the final query tree looks like this:</span></span> 

 ![Boolesk fråga med analyserade villkor][4]

<span data-ttu-id="1b753-305">Vid körning av fråga körs enskilda frågor mot sökbara fält oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="1b753-305">During query execution, individual queries are executed against the searchable fields independently.</span></span> 

+ <span data-ttu-id="1b753-306">TermQuery, ”stora” matchar dokumentera 1 (hotell Atman).</span><span class="sxs-lookup"><span data-stu-id="1b753-306">The TermQuery, "spacious", matches document 1 (Hotel Atman).</span></span> 

+ <span data-ttu-id="1b753-307">PrefixQuery ”, air-condition *”, matchar inte några dokument.</span><span class="sxs-lookup"><span data-stu-id="1b753-307">The PrefixQuery, "air-condition*", doesn't match any documents.</span></span> 

  <span data-ttu-id="1b753-308">Det här är ett beteende som ibland confuses utvecklare.</span><span class="sxs-lookup"><span data-stu-id="1b753-308">This is a behavior that sometimes confuses developers.</span></span> <span data-ttu-id="1b753-309">Även om termen luftkonditionerad finns i dokumentet, är den uppdelad i två termer som standard analyzer.</span><span class="sxs-lookup"><span data-stu-id="1b753-309">Although the term air-conditioned exists in the document, it is split into two terms by the default analyzer.</span></span> <span data-ttu-id="1b753-310">Återkalla inte analyseras prefix frågor som innehåller partiella villkoren.</span><span class="sxs-lookup"><span data-stu-id="1b753-310">Recall that prefix queries, which contain partial terms, are not analyzed.</span></span> <span data-ttu-id="1b753-311">Därför med prefixet ”air-condition” letas upp i inverterad indexet och det gick inte att hitta.</span><span class="sxs-lookup"><span data-stu-id="1b753-311">Therefore terms with prefix "air-condition" are looked up in the inverted index and not found.</span></span>

+ <span data-ttu-id="1b753-312">PhraseQuery, ”oceanen vyn”, letar upp villkoren ”oceanen” och ”visa” och kontrollerar närheten av villkoren i det ursprungliga dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1b753-312">The PhraseQuery, "ocean view", looks up the terms "ocean" and "view" and checks the proximity of terms in the original document.</span></span> <span data-ttu-id="1b753-313">Den här frågan i beskrivningsfältet överensstämmer dokument 1, 2 och 3.</span><span class="sxs-lookup"><span data-stu-id="1b753-313">Documents 1, 2 and 3 match this query in the description field.</span></span> <span data-ttu-id="1b753-314">Lägg märke till dokumentet 4 har termen oceanen i rubriken men inte anses vara en matchning som vi letar efter frasen ”oceanen view” i stället för enskilda ord.</span><span class="sxs-lookup"><span data-stu-id="1b753-314">Notice document 4 has the term ocean in the title but isn’t considered a match, as we're looking for the "ocean view" phrase rather than individual words.</span></span> 

> [!Note]
> <span data-ttu-id="1b753-315">En sökning körs oberoende mot alla sökbara fält i Azure Search-index om du inte begränsar de fält som anges med den `searchFields` parameter, enligt beskrivningen i sökbegäran exempel.</span><span class="sxs-lookup"><span data-stu-id="1b753-315">A search query is executed independently against all searchable fields in the Azure Search index unless you limit the fields set with the `searchFields` parameter, as illustrated in the example search request.</span></span> <span data-ttu-id="1b753-316">Dokument som matchar i någon av de markerade fälten returneras.</span><span class="sxs-lookup"><span data-stu-id="1b753-316">Documents that match in any of the selected fields are returned.</span></span> 

<span data-ttu-id="1b753-317">På hela är dokument som matchar för den aktuella frågan 1, 2, 3.</span><span class="sxs-lookup"><span data-stu-id="1b753-317">On the whole, for the query in question, the documents that match are 1, 2, 3.</span></span> 

## <a name="stage-4-scoring"></a><span data-ttu-id="1b753-318">Steg 4: poängsättning</span><span class="sxs-lookup"><span data-stu-id="1b753-318">Stage 4: Scoring</span></span>  

<span data-ttu-id="1b753-319">Alla dokument i ett sökresultat tilldelas en relevans poäng.</span><span class="sxs-lookup"><span data-stu-id="1b753-319">Every document in a search result set is assigned a relevance score.</span></span> <span data-ttu-id="1b753-320">Funktionen för den relevanta poängen är att högre de dokument som bäst svarar på en fråga användaren återgavs av frågan.</span><span class="sxs-lookup"><span data-stu-id="1b753-320">The function of the relevance score is to rank higher those documents that best answer a user question as expressed by the search query.</span></span> <span data-ttu-id="1b753-321">Poängsättningen beräknas utifrån statistiska egenskaper för termer som matchas.</span><span class="sxs-lookup"><span data-stu-id="1b753-321">The score is computed based on statistical properties of terms that matched.</span></span> <span data-ttu-id="1b753-322">Kärnan i bedömningsprofil formeln är [TF/IDF (termen frekvens inversen dokumentet frekvens)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span><span class="sxs-lookup"><span data-stu-id="1b753-322">At the core of the scoring formula is [TF/IDF (term frequency-inverse document frequency)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span></span> <span data-ttu-id="1b753-323">I frågor som innehåller sällsynt och gemensamma villkor, främjar TF/IDF resultat som innehåller sällsynta termen.</span><span class="sxs-lookup"><span data-stu-id="1b753-323">In queries containing rare and common terms, TF/IDF promotes results containing the rare term.</span></span> <span data-ttu-id="1b753-324">Till exempel i ett hypotetiskt index med alla Wikipedia artiklar från dokument som matchade frågan *VD*, dokument som matchar på *VD* anses vara mer relevant än dokumenten matchar på *i*.</span><span class="sxs-lookup"><span data-stu-id="1b753-324">For example, in a hypothetical index with all Wikipedia articles, from documents that matched the query *the president*, documents matching on *president* are considered more relevant than documents matching on *the*.</span></span>


### <a name="scoring-example"></a><span data-ttu-id="1b753-325">Bedömningsprofil exempel</span><span class="sxs-lookup"><span data-stu-id="1b753-325">Scoring example</span></span>

<span data-ttu-id="1b753-326">Återkalla tre dokument som matchade våra exempelfråga:</span><span class="sxs-lookup"><span data-stu-id="1b753-326">Recall the three documents that matched our example query:</span></span>
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
      "description": "Spacious rooms, ocean view, walking distance to the beach."
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
      "description": "Located on a cliff on the north shore of the island of Kauai. Ocean view."
    }
  ]
}
~~~~

<span data-ttu-id="1b753-327">Dokumentet 1 matchar bäst frågan eftersom båda termen *stora* och nödvändiga frasen *oceanen visa* uppstå i beskrivningsfältet.</span><span class="sxs-lookup"><span data-stu-id="1b753-327">Document 1 matched the query best because both the term *spacious* and the required phrase *ocean view* occur in the description field.</span></span> <span data-ttu-id="1b753-328">Följande två dokument matchar endast frasen *oceanen visa*.</span><span class="sxs-lookup"><span data-stu-id="1b753-328">The next two documents match only the phrase *ocean view*.</span></span> <span data-ttu-id="1b753-329">Du kanske konstigt att relevanta poängsättningen för dokument 2 och 3 är olika trots att de matchar frågan på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="1b753-329">It might be surprising that the relevance score for document 2 and 3 is different even though they matched the query in the same way.</span></span> <span data-ttu-id="1b753-330">Det beror på att bedömningsprofil formeln har fler komponenter än bara TF/IDF.</span><span class="sxs-lookup"><span data-stu-id="1b753-330">It's because the scoring formula has more components than just TF/IDF.</span></span> <span data-ttu-id="1b753-331">I det här fallet har dokumentet 3 tilldelats en något högre poäng eftersom dess beskrivning är kortare.</span><span class="sxs-lookup"><span data-stu-id="1b753-331">In this case, document 3 was assigned a slightly higher score because its description is shorter.</span></span> <span data-ttu-id="1b753-332">Lär dig mer om [Lucenes praktiska bedömningen formeln](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) att förstå hur fältlängden och andra faktorer kan påverka poängsättningen betydelse.</span><span class="sxs-lookup"><span data-stu-id="1b753-332">Learn about [Lucene's Practical Scoring Formula](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) to understand how field length and other factors can influence the relevance score.</span></span>

<span data-ttu-id="1b753-333">Vissa fråga typer (med jokertecken, prefix, regex) alltid bidra med en konstant poäng att hela dokumentet poängen.</span><span class="sxs-lookup"><span data-stu-id="1b753-333">Some query types (wildcard, prefix, regex) always contribute a constant score to the overall document score.</span></span> <span data-ttu-id="1b753-334">Detta gör att matchningar via frågan expansion ska tas med i resultatet, men utan att påverka rangordning.</span><span class="sxs-lookup"><span data-stu-id="1b753-334">This allows matches found through query expansion to be included in the results, but without affecting the ranking.</span></span> 

<span data-ttu-id="1b753-335">Ett exempel visar varför det är viktigt.</span><span class="sxs-lookup"><span data-stu-id="1b753-335">An example illustrates why this matters.</span></span> <span data-ttu-id="1b753-336">Jokertecken, inklusive prefix sökningar är tvetydig per definition eftersom indata är en partiell sträng med potentiella matchar på ett mycket stort antal olika villkor (överväga indata av ”rundtur *” med matchningar hittades för ”visningar”, ”tourettes”, och ” tourmaline ”).</span><span class="sxs-lookup"><span data-stu-id="1b753-336">Wildcard searches, including prefix searches, are ambiguous by definition because the input is a partial string with potential matches on a very large number of disparate terms (consider an input of "tour*", with matches found on “tours”, “tourettes”, and “tourmaline”).</span></span> <span data-ttu-id="1b753-337">Eftersom de här resultaten returneras, går det inte att härleda rimligen vilket är mer värdefull än andra.</span><span class="sxs-lookup"><span data-stu-id="1b753-337">Given the nature of these results, there is no way to reasonably infer which terms are more valuable than others.</span></span> <span data-ttu-id="1b753-338">Därför bör Ignorera vi termen frekvenser när poängberäkningen resultat i frågor för typer med jokertecken, prefix och regex.</span><span class="sxs-lookup"><span data-stu-id="1b753-338">For this reason, we ignore term frequencies when scoring results in queries of types wildcard, prefix and regex.</span></span> <span data-ttu-id="1b753-339">I en flerdelade sökbegäran som innehåller begränsade och fullständiga termer inbyggda resultat från partiella indata med en konstant poäng att undvika att rikta mot potentiellt oväntat matchar.</span><span class="sxs-lookup"><span data-stu-id="1b753-339">In a multi-part search request that includes partial and complete terms, results from the partial input are incorporated with a constant score to avoid bias towards potentially unexpected matches.</span></span>

### <a name="score-tuning"></a><span data-ttu-id="1b753-340">Poäng justera</span><span class="sxs-lookup"><span data-stu-id="1b753-340">Score tuning</span></span>

<span data-ttu-id="1b753-341">Det finns två sätt att justera relevans resultat i Azure Search:</span><span class="sxs-lookup"><span data-stu-id="1b753-341">There are two ways to tune relevance scores in Azure Search:</span></span>

1. <span data-ttu-id="1b753-342">**Bedömningen profiler** befordra dokument i rankningslista över resultat baserat på en uppsättning regler.</span><span class="sxs-lookup"><span data-stu-id="1b753-342">**Scoring profiles** promote documents in the ranked list of results based on a set of rules.</span></span> <span data-ttu-id="1b753-343">I vårt exempel kan vi anser att dokument som matchade mer relevant än de dokument som matchas i beskrivningsfältet fält.</span><span class="sxs-lookup"><span data-stu-id="1b753-343">In our example, we could consider documents that matched in the title field more relevant than documents that matched in the description field.</span></span> <span data-ttu-id="1b753-344">Om indexet har ett fält för varje hotell, kan vi dessutom befordra dokument med lägre pris.</span><span class="sxs-lookup"><span data-stu-id="1b753-344">Additionally, if our index had a price field for each hotel, we could promote documents with lower price.</span></span> <span data-ttu-id="1b753-345">Lär dig mer [lägga till bedömningen profiler i en sökindex.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span><span class="sxs-lookup"><span data-stu-id="1b753-345">Learn more how to [add Scoring Profiles to a search index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span></span>
2. <span data-ttu-id="1b753-346">**Term den** (endast tillgängligt i frågesyntaxen fullständig Lucene) innehåller en operatorn som `^` som kan tillämpas på någon del av trädet frågan.</span><span class="sxs-lookup"><span data-stu-id="1b753-346">**Term boosting** (available only in the Full Lucene query syntax) provides a boosting operator `^` that can be applied to any part of the query tree.</span></span> <span data-ttu-id="1b753-347">I vårt exempel, i stället för att söka i prefixet *air-condition*\*, en kan söka efter antingen exakta termen *air-condition* eller prefix, men dokument som matchar exakt termen är rangordnas högre genom att använda förstärkningen frågan termen: *luften villkoret ^ 2. AIR-condition**.</span><span class="sxs-lookup"><span data-stu-id="1b753-347">In our example, instead of searching on the prefix *air-condition*\*, one could search for either the exact term *air-condition* or the prefix, but documents that match on the exact term are ranked higher by applying boost to the term query: *air-condition^2||air-condition**.</span></span> <span data-ttu-id="1b753-348">Lär dig mer om [term den](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span><span class="sxs-lookup"><span data-stu-id="1b753-348">Learn more about [term boosting](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span></span>


### <a name="scoring-in-a-distributed-index"></a><span data-ttu-id="1b753-349">Poängberäkningen i en distribuerad index</span><span class="sxs-lookup"><span data-stu-id="1b753-349">Scoring in a distributed index</span></span>

<span data-ttu-id="1b753-350">Alla index i Azure Search automatiskt är uppdelade i flera delar, så att oss att snabbt distribuera index mellan flera noder under tjänsten skala upp eller ned.</span><span class="sxs-lookup"><span data-stu-id="1b753-350">All indexes in Azure Search are automatically split into multiple shards, allowing us to quickly distribute the index among multiple nodes during service scale up or scale down.</span></span> <span data-ttu-id="1b753-351">När en sökbegäran utfärdas mot varje Fragmentera oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="1b753-351">When a search request is issued, it’s issued against each shard independently.</span></span> <span data-ttu-id="1b753-352">Resultaten från varje Fragmentera sedan samman och sorterade efter poäng (om ingen ordning har definierats).</span><span class="sxs-lookup"><span data-stu-id="1b753-352">The results from each shard are then merged and ordered by score (if no other ordering is defined).</span></span> <span data-ttu-id="1b753-353">Det är viktigt att veta att bedömningsprofil funktionen vikterna fråga termen frekvens mot inverterade dokumentet frekvensen i alla dokument i Fragmentera, inte på alla shards!</span><span class="sxs-lookup"><span data-stu-id="1b753-353">It is important to know that the scoring function weights query term frequency against its inverse document frequency in all documents within the shard, not across all shards!</span></span>

<span data-ttu-id="1b753-354">Detta innebär en relevans poäng *kan* vara olika för identiska dokument om de finns på olika delar.</span><span class="sxs-lookup"><span data-stu-id="1b753-354">This means a relevance score *could* be different for identical documents if they reside on different shards.</span></span> <span data-ttu-id="1b753-355">Lyckligtvis tenderar skillnaderna att försvinna när antalet dokument i indexet växer på grund av flera termen fördelas jämnt.</span><span class="sxs-lookup"><span data-stu-id="1b753-355">Fortunately, such differences tend to disappear as the number of documents in the index grows due to more even term distribution.</span></span> <span data-ttu-id="1b753-356">Det går inte att anta på vilka Fragmentera alla dokumentet kommer att placeras.</span><span class="sxs-lookup"><span data-stu-id="1b753-356">It’s not possible to assume on which shard any given document will be placed.</span></span> <span data-ttu-id="1b753-357">Dock tilldelas förutsatt en Dokumentnyckel inte ändras den alltid samma Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="1b753-357">However, assuming a document key doesn't change, it will always be assigned to the same shard.</span></span>

<span data-ttu-id="1b753-358">I allmänhet är dokumentet poäng inte det bästa attributet för ordning dokument om stabiliteten för ordning är viktig.</span><span class="sxs-lookup"><span data-stu-id="1b753-358">In general, document score is not the best attribute for ordering documents if order stability is important.</span></span> <span data-ttu-id="1b753-359">Till exempel är anges två dokument med en identisk poäng, det inte säkert vilken visas först i efterföljande körningar av samma fråga.</span><span class="sxs-lookup"><span data-stu-id="1b753-359">For example, given two document with an identical score, there is no guarantee which one appears first in subsequent runs of the same query.</span></span> <span data-ttu-id="1b753-360">Dokumentet poäng bör bara ge en allmän uppfattning om dokumentet betydelse i förhållande till andra dokument i resultatuppsättningen av.</span><span class="sxs-lookup"><span data-stu-id="1b753-360">Document score should only give a general sense of document relevance relative to other documents in the results set.</span></span>

## <a name="conclusion"></a><span data-ttu-id="1b753-361">Slutsats</span><span class="sxs-lookup"><span data-stu-id="1b753-361">Conclusion</span></span>

<span data-ttu-id="1b753-362">Genomförandet av sökmotorer på internet har signalerat förväntningar för textsökning över privata data.</span><span class="sxs-lookup"><span data-stu-id="1b753-362">The success of internet search engines has raised expectations for full text search over private data.</span></span> <span data-ttu-id="1b753-363">För nästan alla typer av sökinställningar planerar vi nu motorn att förstå våra avsikt, även när villkoren är felstavat eller är ofullständig.</span><span class="sxs-lookup"><span data-stu-id="1b753-363">For almost any kind of search experience, we now expect the engine to understand our intent, even when terms are misspelled or incomplete.</span></span> <span data-ttu-id="1b753-364">Vi tror även matchar baserat på nära motsvarande uttryck eller synonymer som vi aldrig faktiskt har angetts.</span><span class="sxs-lookup"><span data-stu-id="1b753-364">We might even expect matches based on near equivalent terms or synonyms that we never actually specified.</span></span>

<span data-ttu-id="1b753-365">Fulltextsökning är mycket komplex som kräver avancerad språkliga analys och systematiskt för bearbetning på ett sätt som destillera, expandera och transformera sökord att leverera relevant resultat från en teknisk synvinkel.</span><span class="sxs-lookup"><span data-stu-id="1b753-365">From a technical standpoint, full text search is highly complex, requiring sophisticated linguistic analysis and a systematic approach to processing in ways that distill, expand, and transform query terms to deliver a relevant result.</span></span> <span data-ttu-id="1b753-366">Inbyggd svårigheter får finns det många faktorer som kan påverka resultatet av en fråga.</span><span class="sxs-lookup"><span data-stu-id="1b753-366">Given the inherent complexities, there are a lot of factors that can affect the outcome of a query.</span></span> <span data-ttu-id="1b753-367">Därför erbjuder investera reda på säkerhetsnivån fulltextsökning faktiska fördelar när du försöker använda oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="1b753-367">For this reason, investing the time to understand the mechanics of full text search offers tangible benefits when trying to work through unexpected results.</span></span>  

<span data-ttu-id="1b753-368">Den här artikeln utforskade fulltextsökning i samband med Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1b753-368">This article explored full text search in the context of Azure Search.</span></span> <span data-ttu-id="1b753-369">Vi hoppas att du får tillräckligt bakgrund att identifiera möjliga orsaker och lösningar för adressering vanliga problem med frågan.</span><span class="sxs-lookup"><span data-stu-id="1b753-369">We hope it gives you sufficient background to recognize potential causes and resolutions for addressing common query problems.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1b753-370">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b753-370">Next steps</span></span>

+ <span data-ttu-id="1b753-371">Skapa exempel index, prova olika frågor och granska resultatet.</span><span class="sxs-lookup"><span data-stu-id="1b753-371">Build the sample index, try out different queries and review results.</span></span> <span data-ttu-id="1b753-372">Instruktioner finns i [bygga och fråga ett index i portalen](search-get-started-portal.md#query-index).</span><span class="sxs-lookup"><span data-stu-id="1b753-372">For instructions, see [Build and query an index in the portal](search-get-started-portal.md#query-index).</span></span>

+ <span data-ttu-id="1b753-373">Försök ytterligare frågesyntaxen från den [Sök dokument](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) exempel avsnittet eller från [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) i Sök explorer i portalen.</span><span class="sxs-lookup"><span data-stu-id="1b753-373">Try additional query syntax from the [Search Documents](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) example section or from [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in the portal.</span></span>

+ <span data-ttu-id="1b753-374">Granska [bedömningen profiler](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) om du vill justera rangordning i sökprogram.</span><span class="sxs-lookup"><span data-stu-id="1b753-374">Review [scoring profiles](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) if you want to tune ranking in your search application.</span></span>

+ <span data-ttu-id="1b753-375">Lär dig hur du använder [språkspecifika lexikala analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).</span><span class="sxs-lookup"><span data-stu-id="1b753-375">Learn how to apply [language-specific lexical analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).</span></span>

+ <span data-ttu-id="1b753-376">[Konfigurera anpassade analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) för minimal bearbetning eller särskilda bearbetning på specifika fält.</span><span class="sxs-lookup"><span data-stu-id="1b753-376">[Configure custom analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) for either minimal processing or specialized processing on specific fields.</span></span>

+ <span data-ttu-id="1b753-377">[Jämför standard och engelska analyzers](http://alice.unearth.ai/)) sida vid sida på den här demo-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="1b753-377">[Compare standard and English analyzers](http://alice.unearth.ai/)) side-by-side on this demo web site.</span></span> 

## <a name="see-also"></a><span data-ttu-id="1b753-378">Se även</span><span class="sxs-lookup"><span data-stu-id="1b753-378">See also</span></span>

[<span data-ttu-id="1b753-379">Sök dokument REST-API</span><span class="sxs-lookup"><span data-stu-id="1b753-379">Search Documents REST API</span></span>](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[<span data-ttu-id="1b753-380">Enkel frågesyntax</span><span class="sxs-lookup"><span data-stu-id="1b753-380">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[<span data-ttu-id="1b753-381">Fullständig Lucene frågesyntaxen</span><span class="sxs-lookup"><span data-stu-id="1b753-381">Full Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[<span data-ttu-id="1b753-382">Hantera sökresultat</span><span class="sxs-lookup"><span data-stu-id="1b753-382">Handle search results</span></span>](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
