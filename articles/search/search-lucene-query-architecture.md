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
# <a name="how-full-text-search-works-in-azure-search"></a>Hur full textsökning fungerar i Azure Search

Den här artikeln är för utvecklare som behöver en bättre förståelse av hur Lucene fulltextsökning fungerar i Azure Search. Azure Search levererar sömlöst förväntat resultat i de flesta scenarier för textfrågor, men ibland kan du få ett resultat som verkar vara ”off” på något sätt. I dessa fall har en bakgrund i hello fyra faser i Lucene Frågekörningen (fråga tolkning lexikala analys, dokumentera matchar bedömningen) kan hjälpa dig att identifiera ändringar tooquery parametrar eller index-konfiguration som levererar hello önskat utfall. 

> [!Note] 
> Azure Search använder Lucene för textsökning men Lucene integrering är inte komplett. Vi selektivt exponera och utöka Lucene funktioner tooenable hello scenarier viktiga tooAzure sökning. 

## <a name="architecture-overview-and-diagram"></a>Översikt över arkitektur och diagram

Bearbetning av en fulltext-sökning startar med parsning hello frågan text tooextract sökvillkoren. hello sökmotor använder ett index tooretrieve dokument med matchande villkor. Enskilda sökord uppdelade ibland och färdigställts till nya formulär toocast ett bredare net över vad kan betraktas som en möjlig matchning. Resultatet sorteras sedan efter ett relevans poäng tilldelade tooeach enskilda matchande dokument. De hello överst i hello rangordnas listan returneras toohello anropande programmet.

Frågekörningen har räknas, fyra steg: 

1. Frågan parsning 
2. Lexikaliskt analys 
3. Hämta dokument 
4. Resultatfunktioner 

hello diagrammet nedan visar hello komponenter som används tooprocess en sökbegäran. 

 ![Arkitekturdiagram för Lucene frågan i Azure Search][1]


| Nyckelkomponenter | Funktionsbeskrivning | 
|----------------|------------------------|
|**Frågan Parser** | Separata sökord från frågeoperatorer och skapa hello frågan struktur (en fråga trädet) toobe skickas toohello sökmotor. |
|**Analyzers** | Analysera lexikala sökord. Den här processen kan omfatta Omforma, ta bort eller expandering av sökord. |
|**Index** | En effektiv datastruktur används toostore och organisera sökbara villkoren som har extraherats från indexerade dokument. |
|**Sökmotor** | Hämtar och resultat som matchar dokument baserat på hello innehållet i hello inverterad index. |

## <a name="anatomy-of-a-search-request"></a>En sökbegäran uppbyggnad

En sökbegäran är en fullständig specifikation av vad som ska returneras i en resultatuppsättning. I enklaste form är en tom fråga med några villkor av något slag. En mer realistisk exempel innehåller parametrar, flera fråga villkor, kanske begränsade toocertain fälten, möjligen filteruttrycket och ordning regler.  

hello följande exempel är en sökbegäran som du kan skicka tooAzure Sök med hjälp av hello [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).  

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

För denna begäran hello sökmotor hello följande:

1. Filtrerar ut dokument där hello pris är minst $60 och mindre än 300 USD.
2. Kör hello fråga. I det här exemplet hello sökfråga består av fraser och villkor: `"Spacious, air-condition* +\"Ocean view\""` (användarna vanligtvis inte ange skiljetecken, men även i hello exempel kan vi tooexplain hur analyzers hantera). För den här frågan söker hello sökmotor hello beskrivning och rubrikfält anges i `searchFields` för dokument som innehåller ”oceanen vyn”, och dessutom på hello termen ”stora”, eller på villkor som börjar med hello prefix ”air-condition”. Hej `searchMode` parametern är används toomatch på något villkor (standard) eller alla, i de fall där en term som inte krävs uttryckligen (`+`).
3. Order hello hotell uppsättning av närhet tooa på geografisk plats, och sedan returnerade toohello anropande programmet. 

hello merparten av den här artikeln handlar om bearbetningen av hello *sökfråga*: `"Spacious, air-condition* +\"Ocean view\""`. Filtrera och sortera ligger utanför omfånget. Mer information finns i hello [Sök API-referensdokumentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a>Steg 1: Frågan parsning 

Enligt nedanstående är hello frågesträngen hello första raden i hello begäran: 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

hello frågan parsern avgränsar operatorer (som `*` och `+` i exemplet hello) från sökning termer och deconstructs hello sökfråga i *underfrågor* av en typ som stöds: 

+ *Termen frågan* för fristående villkor (till exempel stora)
+ *frasen frågan* för angiven villkor (till exempel visa oceanen)
+ *prefixet frågan* för termer följt av ett prefix-operatorn `*` (t.ex. air-condition)

En fullständig lista över stöds frågetyper finns [Lucene fråga sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

Operatörer som är associerade med en underfråga avgöra om hello frågan ”måste vara” eller ”ska vara” nöjd för ett dokument toobe anses vara en matchning. Till exempel `+"Ocean view"` ”måste” på grund av toohello `+` operator. 

Hej frågeparsern omstrukturerar hello underfrågor i en *frågan trädet* (en intern struktur som representerar hello fråga) överförs på toohello sökmotor. I hello första steget av frågan parsning hello frågan trädet ser ut så här.  

 ![Boolesk fråga searchmode alla][2]

### <a name="supported-parsers-simple-and-full-lucene"></a>Stöd för Parser: enkelt och fullständig Lucene 

 Azure Search visar två olika frågespråk `simple` (standard) och `full`. Genom att ange hello `queryType` parameter med din begäran, anger du hello frågeparsern vilka frågespråket du väljer så att den vet hur toointerpret hello operatorer och syntax. Hej [enkel fråga språk](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) intuitivt och säkert, ofta lämplig toointerpret användarindata som-är utan klientsidan bearbetning. Det stöder frågeoperatorer bekant från sökmotorer. Hej [fullständig Lucene frågespråk](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), som du får genom att ange `queryType=full`, utökar hello standardspråk för enkel fråga genom att lägga till stöd för flera operatorer och frågetyper som jokertecken, fuzzy regex och fältet omfång frågor. Till exempel skulle ett reguljärt uttryck som skickas i enkla frågesyntaxen tolkas som en frågesträng och inte ett uttryck. Hej exempelbegäran i den här artikeln använder hello fullständig Lucene frågespråk.

### <a name="impact-of-searchmode-on-hello-parser"></a>Effekten av searchMode i hello tolken 

En annan begäran sökparameter som påverkar parsning är hello `searchMode` parameter. Den styr hello standard operator för booleska frågor: en (standard) eller alla.  

När `searchMode=any`, som är standard hello hello utrymme avgränsare mellan stora och air-condition är eller (`||`), gör hello exempel frågetexten motsvarar: 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

Explicit operatorer som `+` i `+"Ocean view"`, är entydigt i Boolesk fråga konstruktion (hello termen *måste* matchar). Lika självklara är hur toointerpret hello återstående villkor: stora och air-condition. Bör hello sökmotor hitta matchningar för oceanen vyn *och* stora *och* air-condition? Eller bör hitta oceanen visa plus *antingen en* av hello återstående villkoren? 

Som standard (`searchMode=any`), hello sökmotor förutsätter hello bredare tolkning. Antingen fältet *bör* matchas, reflektion ”eller” semantik. hello första fråga trädet illustreras tidigare med hello två ”bör” åtgärder, visar hello standard.  

Anta att vi nu ställer `searchMode=all`. I det här fallet tolkas hello utrymme som en ”och”-åtgärd. Var och en av hello återstående villkor måste båda vara finns i hello dokumentet tooqualify som en matchning. hello resulterande exempelfråga skulle tolkas enligt följande: 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

Ett ändrade frågan träd för den här frågan skulle vara följande, där ett matchande dokument är hello skärningspunkten för alla tre underfrågor: 

 ![Boolesk fråga searchmode alla][3]

> [!Note] 
> Om du väljer `searchMode=any` över `searchMode=all` beslut bästa erhålls genom att köra representativt frågor. Användare som är sannolikt tooinclude operatorer (common när sökning dokumentet lagrar) kan vara resultatet intuitivt om `searchMode=all` informerar Boolesk fråga konstruktioner. Mer information om hello samspelet mellan `searchMode` och operatorer, se [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a>Steg 2: Lexikaliskt analys 

Lexikaliskt analyzers processen *term frågor* och *fras frågor* när hello frågan trädet är strukturerad. En analyzer accepterar hello text indata som angetts tooit av hello parsern processer hello text och sedan skickar tillbaka tokeniserad villkoren toobe införlivas i hello frågan trädet. 

hello vanligaste formen av lexikala analys är *språkliga analysis* som omvandlar sökord baserat på regler för specifika tooa angivna språk: 

* Minska en fråga termen toohello rot form av ett ord 
* Ta bort irrelevanta ord (stoppord, till exempel ”i” eller ”och” på engelska) 
* Dela upp ett sammansatt ord i komponenter 
* Lägre skiftläge ett ord med versal 

Alla dessa åtgärder tenderar tooerase skillnaderna mellan hello textinmatning som tillhandahålls av hello användar- och hello villkoren som lagras i hello index. Dessa åtgärder utöver text bearbetning och kräver avancerade kunskaper hello-språket sig själv. tooadd detta skikt av språkliga medvetenhet Azure Search har stöd för en lång lista med [språkanalys](https://docs.microsoft.com/rest/api/searchservice/language-support) från både Lucene och Microsoft.

> [!Note]
> Analys-kraven kan variera mellan minimal tooelaborate beroende på ditt scenario. Du kan styra komplexitet lexikala analys genom att välja ett av hello fördefinierade analyzers hello eller skapa egna [anpassade analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search). Analyzers är begränsade toosearchable fält som har angetts som en del av en fältdefinition av. Detta ger dig toovary lexikala analys på grundval av per fält. Inget hello *standard* Lucene analyzer används.

I vårt exempel tidigare tooanalysis hello första fråga trädet har hello termen ”Spacious” med versaler ”S” och kommatecken som hello frågeparsern tolkas som en del av hello frågeterm (kommatecken inte anses vara ansvarig query language).  

När hello standard analyzer bearbetar hello har löpt ut, kommer den gemener ”oceanen view” och ”stora” och ta bort hello kommatecken tecken. hello ändrade frågan trädet ska se ut så här: 

 ![Boolesk fråga med analyserade villkor][4]

### <a name="testing-analyzer-behaviors"></a>Testa analyzer beteenden 

hello beteendet för en analyzer kan testas med hello [analysera API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer). Ange hello text som du vill tooanalyze toosee vilka villkor som anges analyzer genererar. Till exempel toosee hur hello standard analyzer skulle bearbeta hello text ”air-condition”, kan du utfärda hello följande begäran:

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

hello standard analyzer bryter hello-indata till hello följande två token, lägga till anteckningar med attribut som start och end förskjutningarna (används för träffar syntaxmarkering) samt deras position (används för frasen matchar):

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

### <a name="exceptions-toolexical-analysis"></a>Undantag toolexical analys 

Lexikaliskt analys gäller endast tooquery typer som kräver fullständiga villkor – en term fråga eller en frasfråga. Den gäller inte tooquery typer med ofullständiga villkor – prefix frågan jokertecken frågan, regex frågan – eller tooa fuzzy frågan. De fråga typer, inklusive hello prefix frågan med termen *air-condition\**  i vårt exempel läggs direkt toohello frågan trädet kringgå hello analys steget. hello endast omvandling utförs på sökord på dessa typer lowercasing.

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a>Steg 3: Hämta för dokument 

Hämta dokument refererar toofinding dokument med matchande villkoren i hello index. Det här steget är att förstå bäst genom ett exempel. Låt oss börja med ett hotell index med hello följande enkla schemat: 

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

Anta vidare att indexet innehåller hello följande fyra dokument: 

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

**Hur villkoren indexeras**

hämtning av toounderstand hjälper tooknow några grunderna om indexering. hello är lagring ett inverterad index, en för varje sökbara fält. Är en sorterad lista över alla villkor från alla dokument i ett inverterad index. Varje term mappar toohello lista över dokument som det uppstår, som ses i hello exemplet nedan.

tooproduce hello villkor i ett inverterad index hello sökmotor utför lexikala analys över hello innehållet i dokumenten, liknande toowhat sker under frågebearbetningen. Text indata skickas tooan analyzer lägre-alltid demontering av skiljetecken och så vidare, beroende på hello analyzer konfiguration. Det är vanligt, men krävs inte, toouse hello samma analyzers för sökning och indexering åtgärder så att sökord påminner mer villkoren i hello index.

> [!Note]
> Azure Search kan du ange olika analyzers för indexering och söka via ytterligare `indexAnalyzer` och `searchAnalyzer` fältet parametrar. Om inget anges hello analyzer med hello `analyzer` egenskapen används för både indexering och sökning.  

**Omvända index för dokument**

Returnerar tooour exempel hello **rubrik** fältet hello inverterad index ser ut så här:

| Period | Listan över |
|------|---------------|
| atman | 1 |
| stranden | 2 |
| hotell | 1, 3 |
| oceanen | 4  |
| playa | 3 |
| utväg | 3 |
| Återställ format | 4 |

I hello rubrikfält, endast *hotell* visas i två dokument: 1, 3.

För hello **beskrivning** fältet hello index är följande:

| Period | Listan över |
|------|---------------|
| luften | 3
| och | 4
| stranden | 1
| villkor | 3
| fria | 3
| Avstånd | 1
| dataö | 2
| kauaʻi | 2
| finns | 2
| Nord | 2
| oceanen | 1, 2, 3
| av | 2
| på |2
| Tyst | 4
| lokaler  | 1, 3
| secluded | 4
| land | 2
| stora | 1
| Hej | 1, 2
| för| 1
| vy | 1, 2, 3
| Gå | 1
| med | 3


**Matchande sökord mot indexerade ord**

Hello inverterad index ovan, vi returnera toohello exempelfråga och se hur matchande dokument hittades för våra exempelfråga. Återkalla hello slutlig trädet ser ut så här: 

 ![Boolesk fråga med analyserade villkor][4]

Vid körning av fråga körs enskilda frågor mot hello sökbara fält oberoende av varandra. 

+ Hej TermQuery, matchar ”stora” dokument 1 (hotell Atman). 

+ Hej PrefixQuery ”, air-condition *”, matchar inte några dokument. 

  Det här är ett beteende som ibland confuses utvecklare. Även om hello termen luftkonditionerad finns i hello dokumentet, är den uppdelad i två termer av hello standard analyzer. Återkalla inte analyseras prefix frågor som innehåller partiella villkoren. Därför villkor med prefixet ”air-condition” slås upp i hello inverterad index och det gick inte att hitta.

+ Hej PhraseQuery, ”oceanen visa-letar upp hello villkor” oceanen ”och” view ”och kontrollerar hello närhet av villkoren i hello originaldokumentet. Den här frågan i beskrivningsfält hello överensstämmer dokument 1, 2 och 3. Meddelande dokumentet 4 har hello termen oceanen i hello avdelning men anses inte vara en matchning vi söker hello ”oceanen visa-fras i stället för enskilda ord. 

> [!Note]
> En sökning körs oberoende mot alla sökbara fält i hello Azure Search index om du inte begränsar hello fält med hello `searchFields` parameter, enligt beskrivningen i hello exempel sökbegäran. Dokument som matchar i någon av hello valt fält returneras. 

Hello-dokument som matchar är hello hela för hello frågan i fråga, 1, 2, 3. 

## <a name="stage-4-scoring"></a>Steg 4: poängsättning  

Alla dokument i ett sökresultat tilldelas en relevans poäng. hello funktionen av hello relevans poäng är toorank högre dessa dokument att bäst besvara en användare fråga som uttrycks genom hello sökfråga. hello poäng beräknas utifrån statistiska egenskaper för termer som matchas. Hello är kärnan i hello bedömningen formeln [TF/IDF (termen frekvens inversen dokumentet frekvens)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf). I frågor som innehåller sällsynt och gemensamma villkor, främjar TF/IDF resultat som innehåller hello sällsynta termen. Exempelvis i ett hypotetiskt index med alla Wikipedia artiklar från dokument som matchade hello frågar *hello VD*, matchning på dokument *VD* betraktas som relevanta mer än dokument som matchar på *i*.


### <a name="scoring-example"></a>Bedömningsprofil exempel

Återkalla hello tre dokument som matchade våra exempelfråga:
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

Dokumentera 1 matchade hello frågan bäst eftersom både hello termen *stora* och hello krävs frasen *oceanen visa* uppstå i beskrivningsfält hello. hello två dokument matchar endast hello frasen *oceanen visa*. Det kan oönskad som hello relevans poäng för dokument 2 och 3 är olika, även om de matchade hello frågan i hello samma sätt. Det beror på att hello bedömningen formeln har fler komponenter än bara TF/IDF. I det här fallet har dokumentet 3 tilldelats en något högre poäng eftersom dess beskrivning är kortare. Lär dig mer om [Lucenes praktiska bedömningen formeln](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) toounderstand hur fältlängden och andra faktorer kan påverka hello relevans poäng.

Vissa fråga typer (med jokertecken, prefix, regex) alltid bidra med en konstant poäng toohello övergripande dokumentera poäng. Detta gör att matchningar via frågan expansion toobe ingår i hello resultat, men utan att påverka hello rangordning. 

Ett exempel visar varför det är viktigt. Jokertecken, inklusive prefix sökningar är tvetydig per definition eftersom hello-indata är en partiell sträng med potentiella matchar på ett mycket stort antal olika villkor (överväga indata av ”rundtur *” med matchningar hittades för ”visningar”, ”tourettes”, och ” tourmaline ”). Eftersom hello de här resultaten returneras, går det inte härleda tooreasonably vilket är mer värdefull än andra. Därför bör Ignorera vi termen frekvenser när poängberäkningen resultat i frågor för typer med jokertecken, prefix och regex. I en flerdelade sökbegäran som innehåller begränsade och fullständiga termer ingår resultat från hello partiella indata med ett konstant poängsätta tooavoid rikta mot potentiellt oväntat matchar.

### <a name="score-tuning"></a>Poäng justera

Det finns två sätt tootune relevans resultat i Azure Search:

1. **Bedömningen profiler** befordra dokument i hello rangordnas listan över resultat baserat på en uppsättning regler. I vårt exempel kan vi anser att dokument som matchas i hello rubrikfält mer relevant än de dokument som matchas i beskrivningsfält hello. Om indexet har ett fält för varje hotell, kan vi dessutom befordra dokument med lägre pris. Mer information hur för[Lägg till bedömningen profiler tooa sökindex.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)
2. **Term den** (endast tillgängligt i hello fullständig Lucene frågesyntaxen) innehåller en operatorn som `^` som kan vara tillämpas tooany en del av hello frågan träd. I vårt exempel, i stället för att söka på hello prefixet *air-condition*\*, en kan söka efter antingen hello exakta termen *air-condition* hello prefix, men dokument som matchar på hello exakt Termen rankas högre genom att använda förstärkningen toohello termen fråga: *luften villkoret ^ 2. AIR-condition**. Lär dig mer om [term den](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).


### <a name="scoring-in-a-distributed-index"></a>Poängberäkningen i en distribuerad index

Alla index i Azure Search är automatiskt dela i flera delar, vilket gör att oss tooquickly distribuera hello index bland flera noder under tjänsten skala upp eller ned. När en sökbegäran utfärdas mot varje Fragmentera oberoende av varandra. hello resultat från varje Fragmentera sedan samman och sorterade efter poäng (om ingen ordning har definierats). Det är viktigt tooknow som hello bedömningsprofil funktionen vikterna frågan termen frekvens mot inverterade dokumentet frekvensen i alla dokument i hello Fragmentera inte över alla shards!

Detta innebär en relevans poäng *kan* vara olika för identiska dokument om de finns på olika delar. Lyckligtvis tenderar skillnaderna toodisappear som hello antalet dokument i hello index växer på grund av toomore även termen distribution. Det är inte möjligt tooassume på vilka Fragmentera alla dokumentet kommer att placeras. Dock förutsatt en Dokumentnyckel inte ändras alltid tilldelas toohello samma Fragmentera.

Dokumentet poäng är i allmänhet inte hello bästa attribut för ordning dokument om stabiliteten för ordning är viktig. Till exempel anges två dokument med en identisk poäng, det är inte säkert vilken visas först i efterföljande körningar av hello samma fråga. Dokumentet poäng bör bara ge en allmän uppfattning om dokumentet relevans relativa tooother dokument i hello resultatmängd.

## <a name="conclusion"></a>Slutsats

hello genomförandet av sökmotorer på internet har signalerat förväntningar för textsökning över privata data. För nästan alla typer av sökinställningar nu planerar vi hello motorn toounderstand våra avsikt, även när villkoren är felstavat eller är ofullständig. Vi tror även matchar baserat på nära motsvarande uttryck eller synonymer som vi aldrig faktiskt har angetts.

Textsökning är mycket komplex som kräver avancerad språkliga analys och en systematiskt tooprocessing på ett sätt som destillera, expandera och transformera frågan villkoren toodeliver relevanta resultat från en teknisk synvinkel. Angivna hello inbyggd svårigheter, finns det många faktorer som kan påverka hello resultatet av en fråga. Därför erbjuder investera hello tid toounderstand hello säkerhetsnivån fulltextsökning faktiska fördelar vid toowork via oväntade resultat.  

Den här artikeln utforskade fulltextsökning i hello kontexten för Azure Search. Vi hoppas att du får tillräckligt bakgrund toorecognize möjliga orsaker och lösningar för adressering vanliga problem med frågan. 

## <a name="next-steps"></a>Nästa steg

+ Skapa hello exempel index, prova olika frågor och granska resultatet. Instruktioner finns i [bygga och fråga ett index i hello portal](search-get-started-portal.md#query-index).

+ Försök ytterligare frågesyntaxen från hello [Sök dokument](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) exempel avsnittet eller från [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) i Sök explorer i hello-portalen.

+ Granska [bedömningen profiler](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) om du vill tootune rangordning i sökprogram.

+ Lär dig hur tooapply [språkspecifika lexikala analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Konfigurera anpassade analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) för minimal bearbetning eller särskilda bearbetning på specifika fält.

+ [Jämför standard och engelska analyzers](http://alice.unearth.ai/)) sida vid sida på den här demo-webbplatsen. 

## <a name="see-also"></a>Se även

[Sök dokument REST-API](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[Enkel frågesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[Fullständig Lucene frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[Hantera sökresultat](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
