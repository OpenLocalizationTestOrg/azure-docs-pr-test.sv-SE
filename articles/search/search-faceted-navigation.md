---
title: aaaHow tooimplement fasetterad navigering i Azure Search | Microsoft Docs
description: "Lägg till fasetterad navigering tooapplications som integreras med Azure Search molnbaserade search-tjänsten på Microsoft Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: cdf98fd4-63fd-4b50-b0b0-835cb08ad4d3
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 3/10/2017
ms.author: heidist
ms.openlocfilehash: c1e6bf9dc55d0044525db79e37d35a50ded5a736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-faceted-navigation-in-azure-search"></a>Hur tooimplement fasetterad navigering i Azure Search
Fasetterad navigering är en filtreringsmekanism som ger automatisk dirigerad nedbrytning navigering i sökprogram. hello termen ”fasetterad navigering' kanske inte känner, men du förmodligen har använt det förut. Följande exempel visar hello, är fasetterad navigering inget annat än hello kategorier som används för toofilter resultat.

 ![Azure Search-jobbet Portal Demo][1]

Fasetterad navigering är en alternativ post punkt toosearch. Den erbjuder ett enkelt alternativ tootyping komplexa sökuttryck manuellt. Facets kan hjälpa dig att hitta det du söker efter, samtidigt som du säkerställer att du inte blir noll. Som en utvecklare kan du exponera hello mest användbara sökkriterierna för att navigera din sökning Kristi facets. I online retail-program bygger ofta fasetterad navigering via varumärken, avdelningar (barnens situation), storlek, pris, popularitet och klassificeringar. 

Implementera fasetterad navigering skiljer sig åt mellan Sök tekniker. I Azure Search skapas fasetterad navigering när databasfrågan, med fält som du tidigare attributet i schemat.

-   I hello frågor som programmet skapar en fråga måste skicka *aspekten frågeparametrar* tooget hello tillgängliga aspekten filtervärden för dokumentet resultatuppsättning.

-   tooactually trim hello dokumentet resultatmängden, hello programmet måste också använda en `$filter` uttryck.

I din programutveckling utgör skriva kod som skapar frågor hello bulk hello arbete. Många av hello programfunktioner som du förväntar dig från fasetterad navigering tillhandahålls av hello-tjänsten, inklusive inbyggt stöd för att definiera intervall och hämtar antal för aspekten resultat. hello tjänsten omfattar också sensible standardvärden som hjälper dig undvika svårhanterliga navigeringsstrukturer. 

## <a name="sample-code-and-demo"></a>Exempelkod och demo
Den här artikeln används en jobbet sökportal som exempel. hello exempel implementeras som en ASP.NET MVC-program.

-   Se och testa hello fungerande demo online på [Azure Search-jobbet Portal Demo](http://azjobsdemo.azurewebsites.net/).

-   Hämta hello koden från hello [lagringsplatsen för Azure-exempel på GitHub](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

## <a name="get-started"></a>Kom igång
Om du är ny toosearch utveckling är hello toothink på bästa sätt fasetterad navigering att den visar hello möjligheter för automatisk dirigerad sökning. Det är en typ av nedåt sökinställningar, baserat på fördefinierade filter som används för att begränsa snabbt ned sökresultat via plats och klicka på åtgärder. 

### <a name="interaction-model"></a>Interaktion modellen

hello sökinställningar för fasetterad navigering är iterativ, så vi först förstå som en sekvens av frågor som skapas i toouser responsåtgärder.

hello startpunkt är program som ger fasetterad navigering, vanligtvis placerad hello omkrets. Fasetterad navigering är ofta en trädstruktur med kryssrutorna för varje värde eller en klickbar text. 

1. En fråga skickas tooAzure Sök anger hello fasetterad navigeringsstruktur via en eller flera aspekten Frågeparametrar. Till exempel hello fråga kan innehålla `facet=Rating`, kanske med en `:values` eller `:sort` alternativet toofurther förfina hello presentation.
2. hello presentation layer återger en sida som innehåller fasetterad navigering, med hjälp av hello aspekterna som har angetts på hello-begäran.
3. En fasetterad navigeringsstruktur som innehåller klassificering får du klickar på ”4” tooindicate som ska visas endast produkter med en klassificering på 4 eller senare. 
4. Svar skickar hello en fråga som innehåller`$filter=Rating ge 4` 
5. hello presentation layer hello sidan visar en minskad resultatmängd som innehåller bara de objekt som uppfyller hello villkor (i det här fallet produkter klassificerade 4 och senare).

En begränsningsaspekt är en frågeparameter men Blanda inte ihop med frågan indata. Den används aldrig som urvalskriterier i en fråga. Se i stället aspekten frågeparametrar som indata toohello navigeringsstruktur som kommer tillbaka hello svar. För varje aspekten frågeparameter som du anger, utvärderar Azure Search är hur många dokumenten i hello ofullständiga resultat för varje aspektvärdet.

Meddelande hello `$filter` i steg 4. hello-filter är en viktig del av fasetterad navigeringen. Även om facets och filter är oberoende hello API, måste båda toodeliver hello upplevelse som du avser. 

### <a name="app-design-pattern"></a>Designmönstret för App

I programkoden är hello mönstret toouse aspekten frågans parametrar tooreturn hello fasetterad navigeringsstruktur tillsammans med aspekten resultat, plus ett $filter uttryck.  hello filter uttryck handtag hello klickar du på händelsen i hello aspektvärdet. Se hello `$filter` uttryck som hello koden bakom hello faktiska Trimning av sökresultaten returnerade toohello presentation lager. Få en färger-begränsningsaspekt klickar du på röd färg i hello implementeras via en `$filter` som väljer endast de objekt som har en röd färg. 

### <a name="query-basics"></a>Grunderna i frågan

En begäran har angetts via en eller flera frågeparametrar i Azure Search (se [Sök dokument](http://msdn.microsoft.com/library/azure/dn798927.aspx) en beskrivning av var och en). Ingen frågeparametrar Hej är nödvändig, men du måste ha minst en för att en giltig fråga toobe.

Precision, tolkas som hello möjlighet toofilter ut irrelevanta träffar uppnås genom en eller båda av dessa uttryck.

-   **Sök =**  
    hello värdet på parametern utgör hello sökuttryck. Det kan vara en enda typ av text eller en komplex sökning-uttryck som innehåller flera villkor och operatörer. Ett sökuttryck används för textsökning frågar sökbara fält i hello index för matchar villkoren som returnerar resultat i rank ordning på hello-servern. Om du ställer in `search` toonull, köra frågor är över hello hela indexet (det vill säga `search=*`). In this Case, andra element för hello fråga, exempelvis en `$filter` eller bedömningen profil hello primära faktorer som påverkar vilka dokument som returneras `($filter`) och i vilken ordning (`scoringProfile` eller `$orderby`).

-   **$filter =**  
    Ett filter är en kraftfull mekanism för att begränsa hello storleken på sökresultat som baseras på hello värden för specifika dokumentattribut. En `$filter` utvärderas först, följt av faceting logik som genererar hello tillgängliga värden och motsvarande räknare för varje värde

Komplexa sökuttryck minska hello prestanda för hello frågan. När det är möjligt använda väl avvägt filter uttryck tooincrease precision och förbättra frågeprestanda.

toobetter förstå hur ett filter lägger till mer exakta, jämför en komplex sökning uttryck tooone som innehåller ett filteruttryck:

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Båda frågorna är giltig, men hello andra är högre om du letar efter icke motell med parkering i Seattle.
-   hello första frågan är beroende av dessa specifika ord som nämns eller inte anges i strängfält som namn, beskrivning och ett annat fält som innehåller sökbara data.
-   hello andra frågan söker efter exakt matchar på strukturerade data och är sannolikt toobe mycket mer exakt.

Kontrollera att varje användaråtgärd via en fasetterad navigeringsstruktur åtföljs av en begränsa sökresultaten i program som innehåller fasetterad navigering. toonarrow resultat kan du använda ett filteruttryck.

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>Skapa en app fasetterad navigering
Du kan implementera fasetterad navigering med Azure Search i din programkod som bygger hello sökbegäran. hello fasetterad navigering är beroende av element i din schema som du definierade tidigare.

Fördefinierade i din sökindex är hello `Facetable [true|false]` indexera attribut, ange på markerade fälten tooenable eller inaktivera deras användning i en fasetterad navigeringsstruktur. Utan `"Facetable" = true`, ett fält kan inte användas i aspekten navigering.

hello presentation lager i koden får hello användarupplevelse. Den bör innehålla hello beståndsdelar av hello fasetterad navigering, till exempel hello etikett, värden, kryssrutorna och hello antal. hello Azure Search REST API är plattformsoberoende, så Använd oavsett språk och plattformar som du vill. hello viktiga är tooinclude UI-element som stöder inkrementell med uppdaterade gränssnittstillstånd uppdatera eftersom varje ytterligare aspekten har valts. 

Frågan samtidigt programkoden skapar en begäran som innehåller `facet=[string]`, en begäranparameter som ger hello fältet toofacet av. En fråga kan ha flera aspekter som `&facet=color&facet=category&facet=rating`, var och en avgränsade med ett et-tecken (&).

Programkod måste också skapa ett `$filter` uttryck toohandle hello på händelser i fasetterad navigeringsfältet. En `$filter` minskar hello sökresultat med hello aspektvärdet som filtervillkor.

Azure Search returnerar hello sökresultat, baserat på en eller flera villkor som du anger, och uppdateringar toohello fasetterad navigeringsstruktur. I Azure Search fasetterad navigering är en nivå-konstruktion med aspekten värden och räknar hur många resultat hittades för var och en.

I följande avsnitt hello, vi ta en närmare titt på hur toobuild varje del.

<a name="buildindex"></a>

## <a name="build-hello-index"></a>Skapa hello index
Faceting är aktiverat för fältet av fält i hello index via det här attributet index: `"Facetable": true`.  
Alla typer av fält som eventuellt kan användas i fasetterad navigering `Facetable` som standard. Dessa typer av fältet `Edm.String`, `Edm.DateTimeOffset`, och alla hello numeriskt fälttyper (i princip alla fälttyp är facetable utom `Edm.GeographyPoint`, som inte kan användas i fasetterad navigeringsfältet). 

När du skapar ett index är bästa praxis för fasetterad navigering tooexplicitly Stäng faceting inaktiverat för fält som ska aldrig användas som en säkerhetsåtgärd.  I synnerhet strängfält för singleton-värden, till exempel ett ID eller produkt-namn ska anges för`"Facetable": false` tooprevent sina oavsiktliga (och ineffektiv) används i fasetterad navigeringsfältet. Aktivera faceting av där du inte behöver det hjälper dig att skydda hello storlek på hello index små och vanligtvis förbättrar prestandan.

Följande är en del av hello schemat för hello jobbet Portal Demo sample-appen, trimmade av vissa attribut tooreduce hello storlek:

```json
{
  ...
  "name": "nycjobs",
  "fields": [
    { “name”: "id",                 "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "job_id",             "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "agency",              "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "posting_type",        "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "num_of_positions",    "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "business_title",      "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "civil_service_title", "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "title_code_no",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "level",               "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_from",   "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_to",     "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_frequency",    "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "work_location",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
…
    { “name”: "geo_location",        "type": "Edm.GeographyPoint",     "searchable": false, "filterable": true, ...  "facetable": false, ... },
    { “name”: "tags",                "type": "Collection(Edm.String)", "searchable": true,  "filterable": true, ...  "facetable": true, ...  }
  ],
…
}
```

Som du ser i hello exempel schemat `Facetable` är inaktiverat för strängfält som inte får användas som facets, t.ex ID-värden. Aktivera faceting av där du inte behöver det hjälper dig att skydda hello storlek på hello index små och vanligtvis förbättrar prestandan.

> [!TIP]
> Som bästa praxis, bland annat hello fullständig index attribut för varje fält. Även om `Facetable` är aktiverad som standard för nästan alla fält, ange ändamålsenligt varje attribut kan hjälpa dig att tänka igenom hello följderna av att varje beslut om schemat. 

<a name="checkdata"></a>

## <a name="check-hello-data"></a>Kontrollera hello data
hello kvaliteten på dina data har en direkt inverkan på om hello fasetterad navigeringsstruktur materialiseras som förväntat. Det påverkar också hello enkel konstruera filtren tooreduce hello resultatmängden.

Om du vill toofacet av varumärken eller pris, varje dokument som ska innehålla värden för *BrandName* och *ProductPrice* som är giltig, konsekvent och produktiva som ett filter.

Här följer några påminnelser för vilka tooscrub för:

* För varje fält som du vill toofacet genom att fråga dig själv om den innehåller värden som är lämpliga som filter i själva dirigerad sökning. hello värdena bör vara korta och beskrivande tillräckligt särskiljande toooffer Rensa välja mellan konkurrerande alternativ.
* Stavfel eller nästan matchande värden. Om aspekten färg och fältvärden omfatta Orange och Ornage (felstavat), en begränsningsaspekt baserat på hello färgfältet skulle hämta båda.
* Blandad case text kan också orsaka oreda i fasetterad navigering med orange och Orange visas som två olika värden. 
* Enkel och pluralis versioner av hello samma värde som kan resultera i en separat begränsningsaspekt för varje.

Du kan föreställa dig är omsorg med förbereder hello data en viktig aspekt av effektiva fasetterad navigering.

<a name="presentationlayer"></a>

## <a name="build-hello-ui"></a>Skapa hello UI
Arbeta tillbaka från hello presentation lager kan du upptäcka krav som kan missas annars och förstå vilka funktioner är viktiga toohello hjälp söka upplevelse.

Vad gäller fasetterad navigering webb- eller sidan visar hello fasetterad navigeringsstruktur, identifierar användarindata för hello sidan och infogar hello ändras element. 

För webbprogram används ofta AJAX i hello presentation lager eftersom du toorefresh stegvisa ändringar. Du kan också använda ASP.NET MVC eller andra visualiseringen plattform som kan ansluta tooan Azure Search-tjänsten via HTTP. hello exempelprogrammet hänvisas till i den här artikeln – hello **Azure Search-jobbet Portal Demo** – händer toobe ASP.NET MVC-program.

I exemplet hello är fasetterad navigering inbyggd i hello sökresultatsidan. Hej följande exempel hämtas från hello `index.cshtml` -filen för hello exempelprogrammet visar hello statiska HTML-strukturen för att visa fasetterad navigering på hello search resultatsida. hello lista över facets inbyggda eller byggas dynamiskt om du skickar en sökterm eller markera eller avmarkera en begränsningsaspekt.

```html
<div class="widget sidebar-widget jobs-filter-widget">
  <h5 class="widget-title">Filter Results</h5>
    <p id="filterReset"></p>
    <div class="widget-content">

      <h6 id="businessTitleFacetTitle">Business Title</h6>
      <ul class="filter-list" id="business_title_facets">
      </ul>

      <h6>Location</h6>
      <ul class="filter-list" id="posting_type_facets">
      </ul>

      <h6>Posting Type</h6>
      <ul class="filter-list" id="posting_type_facets"></ul>

      <h6>Minimum Salary</h6>
      <ul class="filter-list" id="salary_range_facets">
      </ul>

  </div>
</div>
```

Hej följande kodavsnitt från hello `index.cshtml` sidan dynamiskt skapa hello HTML toodisplay hello första aspekten, befattning. Liknande funktioner skapa dynamiskt hello HTML för hello andra facets. Varje aspekten har en etikett och ett antal som visar hello antal objekt hittades för aspekten resultatmängden.

```js
function UpdateBusinessTitleFacets(data) {
  var facetResultsHTML = '';
  for (var i = 0; i < data.length; i++) {
    facetResultsHTML += '<li><a href="javascript:void(0)" onclick="ChooseBusinessTitleFacet(\'' + data[i].Value + '\');">' + data[i].Value + ' (' + data[i].Count + ')</span></a></li>';
  }

  $("#business_title_facets").html(facetResultsHTML);
}
```

> [!TIP]
> Kom ihåg tooadd en mekanism för att rensa facets när du utformar hello sökresultatsidan. Om du lägger till kryssrutorna kan du enkelt se hur tooclear hello filtrerar. För andra layouter behöva spåret mönster eller en annan kreativa metod. I hello jobbet sökportal exempelprogrammet, kan du till exempel klicka hello `[X]` efter en valda aspekten tooclear hello-begränsningsaspekt.

<a name="buildquery"></a>

## <a name="build-hello-query"></a>Skapa hello-fråga
hello-kod som du skriver för att skapa frågor ska ange alla delar av en giltig fråga, inklusive sökuttryck, facets, filter, bedömningen profiler – allt används tooformulate en begäran. I det här avsnittet utforska där facets passar in i en fråga och hur filter används med facets toodeliver sänkt resultatuppsättning.

Observera att facets är integrerad i det här exempelprogrammet. hello sökinställningar i hello jobbet Portal Demo är designad runt fasetterad navigering och filter. hello framträdande placering av fasetterad navigering på hello sidan visar dess prioritet. 

Ett exempel är ofta en bra toobegin. Hej följande exempel hämtas från hello `JobsSearch.cs` fil, en begäran som skapar aspekten navigering baserat på befattning, plats, bokföring och minsta lön versioner. 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

Begränsningsaspekten frågeparameter anges tooa fältet och beroende på hello-datatypen, kan ytterligare parameteriseras via kommaavgränsad lista med `count:<integer>`, `sort:<>`, `interval:<integer>`, och `values:<list>`. En värdelista som stöds för numeriska data när du ställer in intervall. Se [Sök dokument (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) för användningsinformation.

Tillsammans med facets, bör hello begäran om hjälp från ditt program också bygga filter toonarrow ned hello uppsättning kandidatdokument baserat på en markering för aspekten värde. För en cykel store fasetterad navigering innehåller ledtrådar tooquestions som *vilka färger, tillverkare och typer av cyklar är tillgängliga?*. Filtrering svar på frågor som *vilka exakt cyklar är röda mountain cyklar i det här pris av intervallet?*. När du klickar på ”Red” tooindicate som ska visas endast röda produkter, hello nästa fråga hello skickar innehåller `$filter=Color eq ‘Red’`.

Hej följande kodavsnitt från hello `JobsSearch.cs` sidan lägger till hello valt befattning toohello filter om du väljer ett värde från hello befattning-begränsningsaspekt.

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>Tips och råd

### <a name="indexing-tips"></a>Indexering tips
**Förbättra effektiviteten för index om du inte använder en kryssruta**

Om programmet använder fasetterad navigering exklusivt (d.v.s. inga sökrutan) kan du markera hello fält som `searchable=false`, `facetable=true` tooproduce ett komprimerat index. Dessutom kan uppstår indexera bara på hela aspekten värden med ingen word radbrytning eller indexering av hello delar av en flera ords-värde.

**Ange vilka fält som kan användas som facets**

Återkalla den hello schemat hello indexet avgör vilka fält som är tillgängliga toouse som en säkerhetsåtgärd. Under förutsättning att ett fält är facetable anger hello frågan vilka fält toofacet av. hello fält som du är faceting ger hello värden som visas under hello etiketten. 

hello-värden som visas under varje etikett hämtas från hello index. Till exempel om hello aspekten är fältet *färg*, hello-värden som är tillgängliga för ytterligare filtrering är hello värdena för fältet - röd, svart och så vidare.

För numeriska och DateTime-värden endast, kan du ange värden på hello aspekten fält (till exempel `facet=Rating,values:1|2|3|4|5`). En värdelista som tillåts för dessa fält typer toosimplify hello avgränsning av aspekten resultat i sammanhängande intervall (antingen intervall baserat på numeriska värden eller tidsperioder). 

**Som standard kan du bara ha en nivå med fasetterad navigering** 

Som nämndes, finns det inget direkt stöd för många kapslade facets i en hierarki. Som standard stöder fasetterad navigering i Azure Search endast en nivå med filter. Dock finns lösningar. Du kan koda en hierarkisk aspekten struktur i en `Collection(Edm.String)` peka per hierarki med en post. Den här lösningen ligger utanför hello i den här artikeln. 

### <a name="querying-tips"></a>Frågar tips
**Validera fält**

Om du bygger hello lista över facets dynamiskt baserat på ej betrodda användarindata kan verifiera att hello fältnamn hello fasetterad är giltiga. Eller undanta hello namn när du skapar URL: er med hjälp av antingen `Uri.EscapeDataString()` i .NET eller hello motsvarande i din plattform föredrar.

### <a name="filtering-tips"></a>Tips för filtrering
**Öka Sök precision med filter**

Använda filter. Om du förlita dig på returneras bara sökuttryck som är enbart härrör kan orsaka en toobe dokument som inte har något av dess fält hello exakt aspektvärdet.

**Öka sökningsprestanda med filter**

Filter begränsa hello uppsättning kandidatdokument för sökning och utesluta rangordning. Om du har ett stort utbud av dokument får med en selektiv begränsningsaspekt nedåt ofta du bättre prestanda.
  
**Filtrera endast hello fasetterad fält**

I fasetterad nedåt, vill du förmodligen tooonly Inkludera dokument som innehåller hello aspektvärdet i ett visst (fasetterad) fält inte någonstans i alla sökbara fält. Lägger till ett filter förstärker hello målfält genom att styra hello service toosearch i hello fasetterad fält för ett motsvarande värde.

**Trim aspekten resultat med flera filter**

Begränsningsaspekten resultat är dokument hittades i hello sökresultat som matchar en term som aspekten. I hello som följande exempel i sökresultat för *molntjänster*, 254 objekt har också *interna specifikationen* som en innehållstyp. Objekt är inte nödvändigtvis ömsesidigt uteslutande. Om ett objekt uppfyller hello villkor för båda filtren, räknas den i var och en. Duplicering är möjligt när faceting på `Collection(Edm.String)` fält, som ofta används tooimplement dokumentet Taggning.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

I allmänhet om du upptäcker att aspekten resultat konsekvent är för stor, rekommenderar vi att du lägger till fler filter toogive användare fler alternativ för att begränsa hello sökning.

### <a name="tips-about-result-count"></a>Tips om resultatantal

**Begränsa hello antalet objekt i hello aspekten navigering**

För varje fasetterad fält i navigeringsträdet hello finns en standardgräns med 10 värden. Den här standardinställningen är meningsfullt för navigeringsstrukturer eftersom den bevarar hello värden listan tooa hanterbar storlek. Du kan åsidosätta hello standard genom att tilldela ett värde toocount.

* `&facet=city,count:5`Anger att endast hello första fem städer hittades i hello övre rangordnas resultat returneras som ett resultat av aspekten. Överväg att en exempelfråga med en sökterm ”flygplats” och 32 matchar. Om hello-frågan anger `&facet=city,count:5`, men endast hello första fem unika orter med hello de flesta dokument i sökresultaten hello ingår i hello aspekten resultat.

Meddelande hello åtskillnad mellan aspekten resultat och sökresultat. Sökresultaten är alla hello-dokument som matchar hello fråga. Begränsningsaspekten resultat är hello matchar för varje aspektvärdet. Sökresultaten innehåller i hello exempel City-namn som inte är i hello aspekten klassificering lista (5 i vårt exempel). Resultat som har filtrerats ut via fasetterad navigering visas när du tar bort facets eller välja andra facets förutom stad. 

> [!NOTE]
> Diskutera `count` när det finns fler än en typ kan vara förvirrande. hello ger följande tabell en kort sammanfattning av hur hello termen används i Azure Search-API, exempelkod och dokumentation. 

* `@colorFacet.count`<br/>
  Du bör se antalet parameter på hello aspekten, används toodisplay hello antalet aspekten resultat i presentation kod. Begränsningsaspekten sökresultat anger antal hello antal dokument som matchar på hello aspekten termen eller intervall.
* `&facet=City,count:12`<br/>
  Du kan ange värdet för antal tooa i en aspekten-fråga.  hello standardvärdet är 10, men du kan ange den högre eller lägre. Ange `count:12` hämtar hello översta 12 matchningar i aspekten resultaten av dokumentantal.
* "`@odata.count`"<br/>
  Det här värdet anger hello antalet matchande objekt i hello sökresultat i hello frågesvar. I genomsnitt den är större än hello summan av alla aspekten resultat i kombination, på grund av toohello förekomsten av objekt som matchar söktermen hello, men har ingen aspekten matchar.

**Få fram i aspekten resultat**

När du lägger till en tooa fasetterad filterfråga du kanske vill tooretain hello aspekten instruktionen (till exempel `facet=Rating&$filter=Rating ge 4`). Tekniskt sett aspekten = klassificering inte krävs, men håller den returnerar hello antal aspekten värden för klassificeringar 4 och senare. Om du klickar på ”4” och hello frågan innehåller exempelvis ett filter för större eller lika för ”4”, antal returneras för varje klassificering som är 4 och senare.  

**Kontrollera att du få fram exakta aspekten**

I vissa fall kanske du upptäcker att aspekten räknas inte matchar hello resultatmängder (se [fasetterad navigering i Azure Search (foruminlägg)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Begränsningsaspekten antal kan vara felaktig på grund av toohello horisontell partitionering arkitektur. Varje sökindex har flera delar, och varje Fragmentera rapporterar hello främsta aspekter av dokumentantal som sedan kombineras till ett enskilt resultat. Om vissa delar har många matchande värden, medan andra har färre hända att vissa aspekten värden saknas eller är under-inventerat i hello resultat.

Men det här beteendet kan ändra när som helst om du stöter på problemet idag, du kan undvika det genom artificiellt lufttryck hello antal:<number> tooa stora antalet tooenforce fullständig från varje Fragmentera-rapportering. Om hello värdet för antal: är större än eller lika toohello tal med unika värden i fältet hello du garanterat korrekta resultat. Men när dokumentantal är hög, det finns en prestandaförsämring, så Använd det här alternativet omdöme.

### <a name="user-interface-tips"></a>Användaren gränssnittet tips
**Lägga till etiketter för varje fält i aspekten navigering**

Etiketter definieras vanligen i hello HTML eller formulär (`index.cshtml` i hello exempelprogram). Det finns inga API i Azure Search för aspekten navigering etiketter eller andra metadata.

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>Filtrera baserat på ett intervall
Faceting över intervall med värden är en gemensam programkrav för sökning. Adressintervall stöds för numeriska data och DateTime-värden. Du kan läsa mer om varje metod i [Sök dokument (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Azure Search förenklar intervallet konstruktionen genom att tillhandahålla två metoder för att beräkna ett intervall. För båda metoderna skapar Azure Search hello lämpliga områden anges hello indata som du har angett. Till exempel om du anger intervallvärden 10 | 20 | 30, skapas automatiskt intervall på 0-10, 20 10, 20 – 30. Programmet kan du ta bort alla intervall som är tomma. 

**Metod 1: Använd hello intervall parametern**  
tooset pris facets i $10 steg, anger du:`&facet=price,interval:10`

**Metod 2: Använda en värdelista med**  
Du kan använda en värdelista för numeriska data.  Överväg att hello aspekten intervall för en `listPrice` -fält ska renderas på följande sätt:

  ![Exempel värdelistan][5]

toospecify ett aspekten intervall som hello i föregående skärmbild som visar hello använda en lista över värden:

    facet=listPrice,values:10|25|100|500|1000|2500

Varje område bygger använder 0 som en startpunkt, ett värde hello listan som en slutpunkt och beskärs sedan hello tidigare intervall toocreate diskreta intervall. Azure Search gör dessa saker som en del av fasetterad navigeringen. Du har inte toowrite koden för att strukturera varje intervall.

### <a name="build-a-filter-for-a-range"></a>Skapa ett filter för ett intervall
toofilter dokument baserat på ett intervall som du väljer, kan du använda hello `"ge"` och `"lt"` filtrera operatorer i ett tvådelat uttryck som definierar hello slutpunkter för hello intervall. Till exempel om du väljer hello intervallet 10-25 för en `listPrice` fältet hello filtret skulle bli `$filter=listPrice ge 10 and listPrice lt 25`. Hello exempelkod hello filteruttrycket använder **priceFrom** och **priceTo** parametrar tooset hello slutpunkter. 

  ![Frågan för ett intervall med värden][6]

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>Filtrera baserat på avstånd
Vanliga toosee filtrerar som hjälper du väljer en butik, restaurang eller baserat på dess aktuella plats närhet tooyour mål. Den här typen av filter kan se ut så fasetterad navigering, men det är bara ett filter. Vi anges det här gäller de av er som specifikt söker efter implementering råd om det specifika design problemet.

Det finns två geospatiala funktioner i Azure Search **geo.distance** och **geo.intersects**.

* Hej **geo.distance** returnerar funktionen hello avståndet i kilometer, mellan två punkter. En punkt är ett fält och andra är en konstant skickas som en del av hello filter. 
* Hej **geo.intersects** funktionen returnerar true om en viss punkten befinner sig inom en viss polygon. hello är ett fält och hello polygon har angetts som en konstant lista över koordinater som skickas som en del av hello filter.

Du kan hitta filter exemplen i [syntaxen för OData-uttryck (Azure Search)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>

## <a name="try-hello-demo"></a>Prova hello demonstrationer
hello Azure Search-jobbet Portal Demo innehåller hello exempel i den här artikeln.

-   Se och testa hello fungerande demo online på [Azure Search-jobbet Portal Demo](http://azjobsdemo.azurewebsites.net/).

-   Hämta hello koden från hello [lagringsplatsen för Azure-exempel på GitHub](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

Titta på hello URL: en för ändringar i Frågekonstruktionen när du arbetar med sökresultaten. Det här programmet sker tooappend facets toohello URI som var och en.

1. toouse hello mappning funktionerna i hello demoappen få en Bing Maps-nyckel från hello [Bing Maps Dev Center](https://www.bingmapsportal.com/). Klistra in den över hello befintlig nyckel i hello `index.cshtml` sidan. Hej `BingApiKey` i hello `Web.config` filen inte används. 

2. Kör hello program. Rundtur hello valfria eller stänga hello-dialogrutan.
   
3. Ange en sökterm, till exempel ”analytiker”, och klicka på sökikonen hello. hello fråga körs snabbt.
   
   En fasetterad navigeringsstruktur returneras även med hello sökresultat. I hello sökresultatsidan innehåller hello fasetterad navigeringsstrukturen för varje aspekten resultat. Inga facets är markerad, så alla matchande resultat returneras.
   
   ![Sökresultat innan du väljer facets][11]

4. Klicka på en befattning, plats eller minsta lön. Facets var null hello inledande sökning, men som de får på värden där objekt som matchar inte längre bort hello sökresultat.
   
   ![Sökresultat när du har valt facets][12]

5. tooclear hello fasetterad frågan så att du kan försöka annan fråga funktioner, klickar du på hello `[X]` när hello valt facets tooclear hello facets.
   
<a name="nextstep"></a>

## <a name="learn-more"></a>Läs mer
Titta på [Azure Search ingående](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). Vid 45:25, det finns en demonstration på hur tooimplement facets.

För mer information om principer för fasetterad navigering rekommenderar vi hello följande länkar:

* [Utformning för fasetterad sökning](http://www.uie.com/articles/faceted_search/)
* [Designmönster: Fasetterad navigering](http://alistapart.com/article/design-patterns-faceted-navigation)


<!--Anchors-->
[How toobuild it]: #howtobuildit
[Build hello presentation layer]: #presentationlayer
[Build hello index]: #buildindex
[Check for data quality]: #checkdata
[Build hello query]: #buildquery
[Tips on how toocontrol faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/azure-search-faceting-example.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png
[11]: ./media/search-faceted-navigation/faceted-search-before-facets.png
[12]: ./media/search-faceted-navigation/faceted-search-after-facets.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

