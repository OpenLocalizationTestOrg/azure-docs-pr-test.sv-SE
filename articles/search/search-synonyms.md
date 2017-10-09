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
# <a name="synonyms-in-azure-search-preview"></a>Synonymer i Azure Search (förhandsgranskning)

Synonymer i sökmotorer associera motsvarande termer som utökar implicit hello omfattningen av en fråga utan hello användare med tooactually ange hello termen. Till exempel kommer angivna hello termen ”hund” och synonymen associationer i ”hunddjur” och ”Hundvalp”, alla dokument som innehåller ”hund”, ”hunddjur” eller ”Hundvalp” ligga inom hello omfånget för hello-fråga.

I Azure Search görs synonymen expansion när databasfrågan. Du kan lägga till synonymen maps tooa tjänsten med inga avbrott tooexisting åtgärder. Du kan lägga till en **synonymMaps** egenskapsdefinition tooa fält utan toorebuild hello index. Mer information finns i [uppdatera Index](https://docs.microsoft.com/rest/api/searchservice/update-index).

## <a name="feature-availability"></a>Funktionstillgänglighet

hello synonymer funktionen är för närvarande i Förhandsgranska och stöds i endast hello senaste api-förhandsversionen (api-version = 2016-09-01-Preview). Funktionen stöds för närvarande inte på Azure Portal. Eftersom hello API-version anges i hello begäran, är det möjligt toocombine allmänt tillgänglig (GA) och förhandsgranska API: er i hello samma app. Dock kan preview API: er inte är under SLA och funktioner ändras, så vi inte rekommenderar att använda dem i program i produktion.

## <a name="how-toouse-synonyms-in-azure-search"></a>Hur söka toouse synonymer i Azure

I Azure Search baseras synonymen stöd på synonymen maps du definierar och ladda upp tooyour service. Dessa mappningar utgör en oberoende resurs (till exempel index eller datakällor) och kan användas av alla sökbara fält i ett index i din söktjänst.

Synonymen mappar och index hanteras oberoende av varandra. När du definierar en synonym karta och överföra den tooyour service kan du kan aktivera hello synonymen funktionen på ett fält genom att lägga till den nya egenskapen **synonymMaps** i hello fältdefinition. Skapa, uppdatera och ta bort en synonym karta är alltid en åtgärd för hela dokumentet, vilket innebär att du inte kan skapa, uppdatera eller ta bort delar av hello synonymen kartan inkrementellt. Uppdatering av även en enda post kräver en Läs in igen.

Integrera synonymer i tillämpningsprogrammet search är en tvåstegsprocess:

1.  Lägg till en synonym kartan tooyour search-tjänsten via hello API: er nedan.  

2.  Konfigurera en sökbar toouse hello synonymen mappning i hello indexdefinitionen.

### <a name="synonymmaps-resource-apis"></a>SynonymMaps Resource API: er

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Lägga till eller uppdatera en synonym karta under din tjänst, med hjälp av bokför eller PLACERA.

Synonymen maps överförs toohello tjänsten via POST eller PUT. Varje regel måste avgränsas med hello radbrytning (\n). Du kan definiera too5, 000 regler per synonymen kartan i en kostnadsfri tjänst och 10 000 regler i alla andra SKU: er. Varje regel kan ha upp too20 expansion.

I den här förhandsgranskningen måste synonymen maps ha formatet hello Apache Solr som förklaras nedan. Om du har en befintlig synonymen ordlista i ett annat format och vill toouse den direkt, meddela oss på [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

Du kan skapa en ny synonymen karta med hjälp av HTTP POST, som i följande exempel hello:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Du kan också använda PUT och ange hello synonymen mappningsnamn på hello URI. Om hello synonymen mappningen inte finns, kommer att skapas.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr synonymen format

Hej Solr format stöder motsvarande och explicit synonymen mappningar. Mappningsregler följa toohello öppen källkod synonymen filter specificering av Apache Solr, beskrivs i detta dokument: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Nedan finns en Exempelregel för motsvarande synonymer.
```
              USA, United States, United States of America
```

Med hello regel ovan, utökar en sökning frågan ”USA” för ”USA” eller ”USA” eller ”USA”.

Explicit mappning markeras med en pil ”= >”. Om du angett en term sekvens med en sökfråga som matchar hello vänster sida av ”= >” ersätts med hello alternativ hello höger. Angivna hello regel nedan sökfrågor ”Washington”, ”Wash.” eller ”WA” kommer alla skrivas för ”WA”. Explicit mappning endast gäller i hello riktningen som anges och inte omarbetning hello frågan ”WA” för ”Washington” i det här fallet.
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Lista synonymen mappar under din tjänst.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Få en synonym karta under din tjänst.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Ta bort en synonymer karta under din tjänst.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-toouse-hello-synonym-map-in-hello-index-definition"></a>Konfigurera en sökbar toouse hello synonymen mappning i hello indexdefinitionen.

En ny fältegenskap **synonymMaps** kan vara används toospecify en synonym kartan toouse för ett sökbara fält. Synonymen maps är tjänsten Utjämna resurser och kan refereras till av varje fält i ett index under hello-tjänst.

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

**synonymMaps** kan anges för sökbara fält av hello typ 'Edm.String' eller 'Collection(Edm.String)'.

> [!NOTE]
> Du kan bara ha en synonym mappa per fält i den här förhandsgranskningen. Om du vill toouse flera synonymen maps, meddela oss på [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="impact-of-synonyms-on-other-search-features"></a>Effekten av synonymer på andra sökfunktioner

hello synonymer funktionen skriver om hello ursprungliga frågan med synonymer med hello OR-operator. Därför träffar syntaxmarkering och bedömningen profiler hello ursprungliga termen och behandlas synonymer som likvärdiga.

Synonymen funktion gäller toosearch frågor och gäller inte toofilters eller facets. På liknande sätt baseras förslag endast på hello ursprungliga sikt. synonymen matchar visas inte i hello svar.

Synonymen expansion gäller inte toowildcard söktermer; prefix, fuzzy, och villkor för regex är inte expanderas.

## <a name="tips-for-building-a-synonym-map"></a>Tips för att skapa en synonym karta

- En kortfattad, väl utformad synonymen karta är effektivare än en komplett lista över möjliga matchningar. Alltför stora eller komplexa ordlistor ta längre tooparse och påverka hello svarstid om hello frågan utökas toomany synonymer. I stället för att gissa då villkoren kan användas, kan du hello faktiska villkoren via en [söka trafik analysrapporten](search-traffic-analytics.md).

- Som en preliminär och validering övningen aktivera och sedan använda den här rapporten tooprecisely bestämma vilka villkor ska dra nytta av en synonym matchning och fortsätt sedan toouse den som verifiering synonymen kartan ger ett bättre resultat. I hello fördefinierade rapporten hello panelerna ”vanligaste sökfrågor” och ”noll resultatet sökfrågor” ger dig hello nödvändig information.

- Du kan skapa flera synonymen mappningar för search-program (till exempel efter språk om programmet stöder en flerspråkig kundbas). Ett fält kan för närvarande kan bara använda en av dem. Du kan uppdatera synonymMaps-egenskapen för ett fält när som helst.

## <a name="next-steps"></a>Nästa steg

- Om du har ett befintligt index i en utvecklingsmiljö (icke-produktion) experimentera med en liten ordlista toosee hur hello lägga till synonymer ändras hello sökinställningar, inklusive påverkan på bedömningen profiler, träffar syntaxmarkering och förslag.

- [Aktivera sökning trafik analytics](search-traffic-analytics.md) och Använd hello fördefinierade Power BI-rapport toolearn vilka villkor används i de flesta och som de som returnerar hello noll dokument. Tillsammans med dessa insikter, revidera hello ordlista tooinclude synonymer för improduktiva frågor som ska matchas toodocuments i ditt index.
