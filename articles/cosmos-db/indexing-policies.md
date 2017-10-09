---
title: aaaAzure Cosmos DB indexering principer | Microsoft Docs
description: "Förstå hur indexering fungerar i Azure Cosmos-databasen. Lär dig hur tooconfigure och ändrar hello indexprincip för automatisk indexering och bättre prestanda."
keywords: hur indexering fungerar automatisk indexering, indexering databas
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/17/2017
ms.author: arramac
ms.openlocfilehash: 4f77b352b89382aa3352136038cb0e95c7588aac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Hur fungerar Azure Cosmos DB indexinformationen?

Alla Azure Cosmos DB data indexeras som standard. Och många kunder är Grattis toolet Azure Cosmos DB automatiskt hantera alla aspekter av indexering, Azure Cosmos DB också stöd för att ange en anpassad **indexering princip** för samlingar när du skapar. Indexering principer i Azure Cosmos DB är mer flexibel och kraftfull än sekundärindex som erbjuds i andra databasplattformar,, eftersom de låter dig utforma och anpassa hello form hello index utan att kompromissa flexibelt schema. toolearn hur indexering fungerar i Azure Cosmos DB, måste du förstå som du kan göra detaljerade kompromisser mellan Omkostnad för indexlagring, Skriv- och fråga genomflöde och fråga konsekvens genom att hantera indexprincip.  

Vi titta Stäng på Azure Cosmos DB indexering principer, hur du kan anpassa indexprincip och hello associerade avvägningarna i den här artikeln. 

När du har läst den här artikeln att du kunna tooanswer hello följande frågor:

* Hur kan åsidosätta hello egenskaper tooinclude eller undantas från att indexera?
* Hur konfigurerar hello index för eventuell uppdateringar?
* Hur konfigurerar indexering tooperform Order By- eller intervallet frågor?
* Hur gör ändringar tooa samling indexprincip?
* Hur jag jämföra lagrings- och prestandakrav för olika indexering principer?

## <a id="CustomizingIndexingPolicy"></a>Anpassa hello indexprincip i en samling
Utvecklare kan anpassa hello avvägningarna mellan lagring, Skriv-/ frågeprestanda och fråga konsekvens genom att åsidosätta hello-standardprincipen indexering på en samling Azure Cosmos DB och konfigurera hello följande aspekter.

* **Inklusive/exklusive dokument och sökvägar till/från index**. Utvecklare kan välja vissa dokument toobe undantagna eller ingår i hello index när hello infoga eller ersätta dem toohello samling. Utvecklare kan också välja tooinclude eller kallas även undanta vissa JSON-egenskaper sökvägar (inklusive mönster med jokertecken) toobe indexeras mellan dokument som ingår i ett index.
* **Konfigurera olika indexera typer**. Utvecklare kan också ange hello typ av index som de kräver över en samling baserat på deras data och förväntade frågan arbetsbelastning och hello numeriska/strängen ”precision” för varje sökväg för varje hello ingår sökvägar.
* **Konfigurera Index Update lägen**. Azure Cosmos-DB stöder tre indexering lägen som kan konfigureras via hello indexering princip på en samling Azure Cosmos DB: konsekvent, Lazy och None. 

Hej efter .NET kodfragment visas hur tooset en anpassad indexprincip under hello skapandet av en samling. Här anger vi hello principen med områdesindex för strängar och tal på hello högsta precision. Den här principen kan vi köra Order By-frågor mot strängar.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> hello JSON-schema för indexprincip ändrades med hello-versionen av REST API-version 2015-06-03 toosupport intervallet index mot strängar. .NET SDK 1.2.0 och Java, Python och Node.js SDK 1.1.0 stöd för hello nya princip schemat. Äldre SDK använder hello REST API-version 2015-04-08 och stöder hello äldre schemat för indexering av principen.
> 
> Som standard indexerar Azure Cosmos DB alla egenskaper för anslutningssträngen i dokument konsekvent med ett Hash-index och numeriska egenskaper med ett index för intervallet.  
> 
> 

### <a name="customizing-hello-indexing-policy-using-hello-portal"></a>Anpassa hello indexprincip med hello-portalen

Du kan ändra hello indexering princip i en samling med hello Azure-portalen. Öppna Azure Cosmos DB kontot i hello Azure portal, väljer din samling i hello vänstra navigeringsfönstret och klicka **inställningar**, och klicka sedan på **indexering princip**. I hello **indexering princip** bladet ändra indexprincip och klicka sedan på **OK** toosave ändringarna. 

### <a id="indexing-modes"></a>Databasen indexering lägen
Azure Cosmos-DB stöder tre indexering lägen som kan konfigureras via hello indexering princip på en samling Azure Cosmos DB – konsekvent, Lazy och None.

**Konsekvent**: om en samling Azure Cosmos DB principen anges som ”konsekvent” hello frågor för en viss Azure Cosmos DB samling Följ hello samma konsekvensnivå som angetts för hello punkt-läsningar (d.v.s. stark, begränsat föråldrad, sessionen eller senare). hello indexet uppdateras synkront som en del av hello dokumentet uppdateringen (d.v.s. insert-, Ersätt, uppdatering och borttagning av ett dokument i en samling Azure Cosmos DB).  Konsekvent indexering stöder konsekvent frågor på hello kostnaden för eventuell minskning i genomströmning för skrivning. Denna minskning är en funktion av hello unika sökvägar som behöver toobe indexerade och hello ”konsekvensnivå”. Konsekvent indexering läge är utformad för ”skriva snabbt fråga omedelbart” arbetsbelastningar.

**Lazy**: tooallow maximala dokumentet införandet dataflöde, en samling Azure Cosmos DB kan konfigureras med lazy konsekvenskontroll; betydelse frågor är överensstämmelse. hello indexet uppdateras asynkront när en samling Azure Cosmos DB är overksamt d.v.s. när hello samling genomflödeskapaciteten inte är fullt utnyttjade tooserve användarförfrågningar. För ”mata in nu fråga senare” arbetsbelastningar som kräver röra sig obehindrat dokumentet införandet är ”lazy” indexering läge lämpligt.

**Ingen**: en samling som markerats med index ”None” har inget index som är kopplade till den. Detta är vanligt om Azure Cosmos DB används som en nyckel / värde-lagring och dokument används endast av deras ID-egenskap. 

> [!NOTE]
> Konfigurera hello indexering princip med ”None” har hello sidoeffekt av släppa alla befintliga indexet. Använd det här alternativet om ditt åtkomstmönster bara behöver ”id” och/eller ”automatisk länka”.
> 
> 

hello som följande exempel visar hur skapa en Azure DB som Cosmos-samling med hello .NET SDK konsekvent automatisk indexering på alla dokument infogningar.

hello följande tabell visar hello konsekvens för frågor baserat på hello indexering läge (konsekvent och Lazy konfigurerats för hello insamling och hello konsekvensnivå angetts för hello-fråga). Detta gäller tooqueries som gjorts med alla gränssnitt - REST API-SDK: er eller inifrån lagrade procedurer och utlösare. 

|Konsekvens|Indexering läge: konsekvent|Indexering läge: Lazy|
|---|---|---|
|Stark|Stark|Eventuell|
|Begränsad föråldring|Begränsad föråldring|Eventuell|
|Session|Session|Eventuell|
|Eventuell|Eventuell|Eventuell|

Azure Cosmos-DB Returnerar ett fel för frågor som gjorts på samlingar med ingen indexering läge. Frågor kan fortfarande köras som genomsökningar via hello explicit `x-ms-documentdb-enable-scan` huvudet i hello REST API eller hello `EnableScanInQuery` begäran med hjälp av alternativet hello .NET SDK. Vissa frågefunktioner som ORDER BY stöds inte som sökningar med `EnableScanInQuery`.

hello följande tabell visar hello konsekvens för frågor baserat på hello indexerings-läge (konsekvent Lazy och None) när EnableScanInQuery har angetts.

|Konsekvens|Indexering läge: konsekvent|Indexering läge: Lazy|Indexering läge: ingen|
|---|---|---|---|
|Stark|Stark|Eventuell|Stark|
|Begränsad föråldring|Begränsad föråldring|Eventuell|Begränsad föråldring|
|Session|Session|Eventuell|Session|
|Eventuell|Eventuell|Eventuell|Eventuell|

hello som följande exempel visar hur skapa en Azure DB som Cosmos-samling med indexering konsekvent på alla dokument infogningar av hello .NET SDK.

     // Default collection creates a hash index for all string fields and a range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Index sökvägar
Azure Cosmos-DB modeller JSON-dokument och hello index som träd och låter dig tootune toopolicies för sökvägar i hello-trädet. Dokument, kan du välja vilka sökvägar måste inkluderas eller uteslutas från indexering. Detta kan erbjuda bättre skrivprestanda och lägre index lagring för scenarier när hello frågemönster är kända i förväg.

Index sökvägar inledas med hello rot (/) och vanligtvis avslutas med hello? jokertecken operator, som anger att det finns flera möjliga värden för hello prefix. Till exempel tooserve Välj * från familjer F var F.familyName = ”Andersen” måste du inkludera ett index sökvägen för /familyName/? i hello samling index princip.

Index sökvägar kan också använda hello * jokertecken operatorn toospecify hello beteende för sökvägar rekursivt under hello prefixet. Till exempel nyttolast / * kan vara att använda tooexclude allt under hello nyttolast egenskap från indexering.

Här följer hello vanliga mönster för att ange index sökvägar:

| Sökväg                | Beskrivning/användningsfall                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Standardsökvägen för samlingen. Rekursiva och tillämpar toowhole dokumentets träd.                                                                                                                                                                                                                                   |
| / prop /?             | Index sökväg krävs tooserve frågor som följande hello (typer respektive med hash- eller intervall):<br><br>Välj från samlingen-c WHERE c.prop = ”värde”<br><br>Välj från samlingen-c WHERE c.prop > 5<br><br>Välj samling c ORDER BY c.prop                                                                       |
| / prop / *             | Index sökvägen för alla sökvägar under hello angivna etiketten. Fungerar med hello följande frågor<br><br>Välj från samlingen-c WHERE c.prop = ”värde”<br><br>Välj från samlingen-c WHERE c.prop.subprop > 5<br><br>Välj från samlingen-c WHERE c.prop.subprop.nextprop = ”värde”<br><br>Välj samling c ORDER BY c.prop         |
| [] / sammanställer / /?         | Index sökväg måste tooserve iteration och JOIN-frågor mot matriser av skalärer som [”a”, ”b”, ”c”]:<br><br>Välj taggen från tagg i collection.props var taggen = ”värde”<br><br>Välj tagg från samlingen c JOIN-tagg i c.props där tagga > 5                                                                         |
| /Props/ [] /subprop/? | Index sökväg krävs tooserve iteration och JOIN-frågor mot matriser av objekt som [{subprop: ”a”}, {subprop: ”b”}]:<br><br>Välj taggen från tagg i collection.props var tag.subprop = ”värde”<br><br>Välj taggen från samlingen c JOIN-tagg i c.props var tag.subprop = ”värde”                                  |
| / prop/subprop /?     | Index sökväg krävs tooserve frågor (typer respektive med hash- eller intervall):<br><br>Välj från samlingen-c WHERE c.prop.subprop = ”värde”<br><br>Välj från samlingen-c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Anpassat index sökvägar, när du är obligatoriska toospecify hello indexering Standardregeln för hello hela dokumentträdet med hello Särskild sökväg ”/ *”. 
> 
> 

hello konfigurerar följande exempel en specifik sökväg med intervallet indexering och anpassade Precisionvärdet 20 byte:

    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);


### <a name="index-data-types-kinds-and-precisions"></a>Index-datatyper, typer och Precision-datorerna
Nu när vi har valt en titt på hur toospecify sökvägar nu ska vi titta på hello alternativ vi kan använda tooconfigure hello indexprincip för en sökväg. Du kan ange en eller flera indexering definitioner för varje sökväg:

* Datatyp: **sträng**, **nummer**, **punkt**, **Polygon**, eller **LineString** (kan endast innehålla en post per datatyp per sökväg)
* Index-typ: **Hash** (likhetsfrågor) **intervallet** (likhetsfrågor, intervall eller Order By-frågor) eller **Spatial** (spatial frågor) 
* Precision: För hash-index Detta skiljer sig från 1 too8 för både strängar och tal med en standard som 3. För områdesindex det här värdet vara 1 (maximal precision) och variera mellan 1-100 (maximal precision) för sträng eller numeriska värden.

#### <a name="index-kind"></a>Typ av index
Azure Cosmos-DB stöder Hash och intervallet index typer för varje sökväg (som kan konfigureras för strängar, siffror eller båda).

* **Hash** stöder effektiv likhetsfrågor och JOIN-frågor. Hash-index behöver inte en högre precision än hello standardvärdet 3 byte för de flesta användningsfall. Datatyp kan vara en sträng eller en siffra.
* **Intervallet** stöder effektiv likhetsfrågor intervallet frågor (med hjälp av >, <>, =, < =,! =), och Order By-frågor. Order By-frågor som standard även kräva maximala index precision (-1). Datatyp kan vara en sträng eller en siffra.

Azure Cosmos-DB stöder också hello rumsindexet typ för varje sökväg som kan anges för hello punkt, Polygon eller LineString datatyper. hello värdet på angivna hello-sökvägen måste vara ett giltigt GeoJSON-fragment som `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Spatial** stöder effektiv spatial (inom och avstånd) frågor. Datatyp kan vara punkt, Polygon eller LineString.

> [!NOTE]
> Azure Cosmos-DB har stöd för automatisk indexering av punkter och polygoner LineStrings.
> 
> 

Här följer hello stöds index typer och exempel på att de kan använda tooserve frågor:

| Typ av index | Beskrivning/användningsfall                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hash       | Hash-över/prop /? (eller /) kan vara används tooserve hello följande frågor effektivt:<br><br>Välj från samlingen-c WHERE c.prop = ”värde”<br><br>Hash över/sammanställer / [] /? (eller / eller/sammanställer /) kan vara används tooserve hello följande frågor effektivt:<br><br>Välj taggen från samlingen c JOIN-tagg i c.props var taggen = 5                                                                                                                       |
| intervallet      | Intervallet över/prop /? (eller /) kan vara används tooserve hello följande frågor effektivt:<br><br>Välj från samlingen-c WHERE c.prop = ”värde”<br><br>Välj från samlingen-c WHERE c.prop > 5<br><br>Välj samling c ORDER BY c.prop                                                                                                                                                                                                              |
| Spatial     | Intervallet över/prop /? (eller /) kan vara används tooserve hello följande frågor effektivt:<br><br>Välj från samlingen c<br><br>VAR ST_DISTANCE (c.prop, {”typ”: ”plats”, ”coordinates”: [0.0, 10.0]}) < 40<br><br>Välj från samlingen c där ST_WITHIN(c.prop, {"type": "Polygon",...})--med indexering punkter aktiverad<br><br>Välj från samlingen c där ST_WITHIN({"type": "Point",...}, c.prop)--med indexering på polygoner aktiverad              |

Som standard returneras ett fel för frågor med intervallet operatorer som > = om det finns inga intervall index (för alla precision) i ordning toosignal som en genomsökning kan vara nödvändiga tooserve hello frågan. Intervallet frågor kan utföras utan ett intervall-index med hello x-ms-documentdb-enable-genomsökning huvudet i hello REST API eller hello EnableScanInQuery begäran alternativet med hello .NET SDK. Om det finns andra filter i hello-fråga som Azure Cosmos DB kan använda hello index toofilter mot, returneras inget fel.

hello samma regler gäller för spatial frågor. Som standard returneras ett fel för spatial frågor om det finns inget spatial index och det finns inga filter som kan hanteras från hello index. De kan utföras som en genomsökning med x-ms-documentdb-enable-genomsökning/EnableScanInQuery.

#### <a name="index-precision"></a>Index precision
Index precision kan du väger index lagring omkostnader och prestanda för frågor. För tal bör du använda hello precision standardkonfigurationen 1 (”högsta”). Eftersom siffror är 8 byte i JSON, är detta likvärdiga tooa konfiguration på 8 byte. Du väljer ett lägre värde för precision, till exempel 1-7, indexposten innebär att värden i vissa områden kartan toohello samma. Därför minskar du index lagringsutrymme, men Frågekörningen kan ha tooprocess fler dokument och därför förbruka mer genomströmning d.v.s. enheter för programbegäran.

Index precision konfigurationen har mer praktiska program med strängen intervall. Eftersom strängar kan vara en godtycklig längd, kan hello valet av hello index precision prestanda påverkas hello sträng intervallet frågor och påverkan hello mängden index lagringsutrymme som krävs. Strängen intervallet index kan konfigureras med 1-100 eller -1 (”högsta”). Om du vill tooperform Order By-frågor mot egenskaperna för anslutningssträngen måste du ange en noggrannhet på 1 för hello motsvarande sökvägar.

Rumsindex alltid använda hello standard index precision för alla typer (poäng, LineStrings och polygoner) och kan inte ändras. 

hello som följande exempel visar hur tooincrease hello precision för intervallet index i en samling med hello .NET SDK. 

**Skapa en samling med ett anpassat index precision**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override hello default policy for Strings toorange indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos-DB Returnerar ett fel när en fråga som använder Order By men har inte en områdesindex mot hello efterfrågade sökväg med hello maximala precision. 
> 
> 

På liknande sätt kan sökvägar inte helt uteslutas från indexering. hello nästa exempel visas hur tooexclude ett helt avsnitt med hello dokument (kallas även ett underträd) från att indexera med hello ”*” jokertecken.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opting-in-and-opting-out-of-indexing"></a>Börjar och väljer bort indexering
Du kan välja om du vill hello Mängdindexets tooautomatically alla dokument. Som standard alla dokument indexeras automatiskt, men du kan välja tooturn av. När indexering är avstängd dokument kan nås bara via deras självlänkar eller av frågor med ID: t.

Med automatisk indexering avstängd, kan du fortfarande selektivt lägga till endast specifika dokument toohello index. Däremot kan du lämna automatisk indexering på och selektivt väljer tooexclude specifika dokument. Indexering på/av konfigurationer är användbara när du har bara en del av dokument som behöver toobe efterfrågas.

Till exempel hello följande exempel visar hur tooinclude ett dokument som uttryckligen med hjälp av hello [DocumentDB API .NET SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/documentdb-sdk-dotnet) och hello [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) egenskapen.

    // If you want toooverride hello default collection behavior tooeither
    // exclude (or include) a Document from indexing,
    // use hello RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modifying-hello-indexing-policy-of-a-collection"></a>Ändra hello indexprincip i en samling
Azure Cosmos-DB tillåter toomake ändringar toohello indexprincip i en samling i hello direkt. En ändring i indexering princip på en samling Azure Cosmos DB kan leda tooa ändring i form av hello indexets hello inklusive hello sökvägar kan indexeras, deras precision, samt hello konsekvent modell av hello index sig själv. Därmed kan en ändring i indexprincip, effektivt kräver en transformering av hello gammalt index till en ny. 

**Online Index omvandlingar**

![Hur indexering fungerar – Azure Cosmos DB online index omvandlingar](./media/indexing-policies/index-transformations.png)

Index transformationer görs online, vilket innebär att hello indexerade per hello gamla princip effektivt omvandlas per hello ny princip **utan att påverka hello skrivåtgärder tillgänglighet eller hello etablerat dataflöde** av hello-samling. hello konsekvens för läsning och skrivåtgärder som skapats med hjälp av hello REST API SDK eller från inom lagrade procedurer och utlösare påverkas inte under omvandling av indexet. Det innebär att det finns inga prestanda försämras eller driftstopp tooyour appar när du gör en indexprincip ändra.

Under hello tid för omvandling av indexet pågår, är dock frågor överensstämmelse oavsett hello indexering konfiguration (konsekvent eller Lazy). Detta gäller tooqueries från alla gränssnitt – REST API SDK: er, och inifrån lagrade procedurer och utlösare. Precis som med Lazy indexering, utförs omvandling av indexet asynkront i hello bakgrunden på hello repliker med hello extra resurser som är tillgängliga för en given replikering. 

Index transformationer görs också **i situ** (på plats), d.v.s. Azure Cosmos DB inte har två kopior av hello index och växlingen hello gammalt index ut med hello ny. Detta innebär att inga ytterligare diskutrymme krävs eller förbrukas i samlingar när du utför index transformationer.

När du ändrar indexprincip hur hello ändras tillämpade toomove från hello gamla index toohello nya en beror främst på hello indexering läge konfigurationer mer så att andra värden än hello som är inkluderad/exkluderad sökvägar, index-typer och Precision-datorerna. Om både de gamla och nya principerna använder konsekvent indexering, utför Azure Cosmos DB en onlineåtgärd transformation. Du kan inte använda en annan indexering principändring konsekvent indexering läge medan hello omvandling pågår.

Du kan dock flytta tooLazy eller ingen indexering läge under en transformering pågår. 

* När du flyttar tooLazy hello index ändring görs gällande omedelbart och Azure Cosmos DB börjar återskapa indexet hello asynkront. 
* När du flyttar tooNone sedan hello index släpps gälla omedelbart. Flytta tooNone är användbart om du vill toocancel en pågående omvandling och starta ny med en annan indexprincip. 

Om du använder hello .NET SDK kan startar du en indexering principändring använda hello nya **ReplaceDocumentCollectionAsync** metod och spåra hello procentandel fortskrider hello index omvandlingen med hello  **IndexTransformationProgress** svar egenskap från en **ReadDocumentCollectionAsync** anropa. Andra SDK: er och hello REST API stöd för motsvarande egenskaper och metoder för att göra ändringar av indexerings-principer.

Här är ett kodfragment som visar hur toomodify en samling indexering principen från konsekvent indexering läge tooLazy.

**Ändra indexering princip från konsekvent tooLazy**

    // Switch toolazy indexing.
    Console.WriteLine("Changing from Default tooLazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);


Du kan kontrollera hello förloppet för en index-transformation genom att anropa ReadDocumentCollectionAsync, till exempel som visas nedan.

**Spåra förloppet för omvandling av Index**

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

Du kan släppa hello index för en samling genom att flytta toohello ingen indexering läge. Det kan vara användbart operativa om du vill toocancel en pågående omvandling och starta en ny direkt.

**Släppa hello indexet för en samling**

    // Switch toolazy indexing.
    Console.WriteLine("Dropping index by changing tootoohello None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

När du blir indexering principändringar tooyour Azure Cosmos DB samlingar? hello följande är hello vanligaste användningsområden:

* Hantera enhetliga resultat under normal drift, men återgår toolazy indexering under bulk data import
* Börja använda nya indexering funktioner på din aktuella Azure Cosmos DB samlingar, t.ex. som geospatiala fråga som kräver hello rumsindexet typ eller Order By / sträng intervallet frågor som kräver hello sträng intervallet index typ
* Sidan Välj hello egenskaper toobe indexerade och ändra dem över tid
* Finjustera indexering precision tooimprove frågeprestanda eller minska lagringsutrymme som förbrukas

> [!NOTE]
> toomodify indexering principen med ReplaceDocumentCollectionAsync kan du behöva version > = 1.3.0 av hello .NET SDK
> 
> För index omvandling toocomplete, du måste se till att det finns tillräckligt med ledigt lagringsutrymme på hello samling. Om hello samling når sin lagringskvot, pausades hello index omvandling. Index omvandling återupptas automatiskt när lagringsutrymmet är tillgängligt, t.ex. Om du tar bort vissa dokument.
> 
> 

## <a name="performance-tuning"></a>Prestandajustering
Hej DocumentDB APIs innehåller information om prestandamått som hello index lagringsenheter som används och hello genomströmning kostnad (frågeenheter) för varje åtgärd. Den här informationen kan vara används toocompare olika principer för indexering och för prestandajustering.

lagringskvoten för toocheck hello användning av en samling kör en HEAD eller GET-begäran mot hello samling resurs och inspektera hello x-ms-begäran-quota och hello x-ms-begäran-användning rubriker. I hello .NET SDK, hello [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) och [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) egenskaper i [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) innehåller dessa motsvarande värden .

     // Measure hello document size usage (which includes hello index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


toomeasure hello arbetet med att indexera i varje skrivåtgärd (skapa, uppdatera eller ta bort) inspektera hello x-ms-begäran-kostnad huvud (eller motsvarande hello [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) egenskap i [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) i hello .NET SDK) toomeasure hello antalet frågeenheter som används av dessa åtgärder.

     // Measure hello performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure hello performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-toohello-indexing-policy-specification"></a>Ändringar toohello indexering principspecifikationen
En ändring i hello schemat för indexprincip introducerades 7 juli 2015 med REST API-version 2015-06-03. hello motsvarande klasser i hello SDK-versioner har nya implementeringar toomatch hello schema. 

hello följande ändringar har införts i hello JSON-specifikationen:

* Indexering princip stöder intervallet index för strängar
* Varje sökväg kan ha flera index definitioner, ett för varje datatyp
* Indexering precision har stöd för 1 – 8 för tal 1-100 för strängar och -1 (maximal precision)
* Sökvägar segment kräver inte ett citattecken tooescape varje sökväg. Exempelvis kan du lägga till en sökväg för/rubrik /? i stället för / ”title” /?
* hello rotsökvägen som representerar ”alla sökvägar” kan representeras som / * (förutom för /)

Om du har kod som etablerar samlingar med en anpassad indexprincip som skrivits med version 1.1.0 av hello .NET SDK eller senare, behöver du toochange tillämpningsprogrammet code toohandle ändringarna i ordning toomove tooSDK version 1.2.0. Om du inte har kod som konfigurerar indexprincip eller planera toocontinue använder en äldre version av SDK, krävs några ändringar.

En praktisk jämförelse är här en anpassad indexprincip som skrivits med hello REST API-version 2015-06-03 exempel samt hello tidigare version 2015-04-08.

**Tidigare Indexprincip JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }

**Aktuella Indexprincip JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }

## <a name="next-steps"></a>Nästa steg
Följ hello länkarna nedan för index policy management exemplen och toolearn mer om Azure Cosmos DB frågespråk.

1. [DocumentDB API .NET indexhantering kodexempel](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
2. [Åtgärder för insamling av DocumentDB API REST](https://msdn.microsoft.com/library/azure/dn782195.aspx)
3. [Fråga med SQL](documentdb-sql-query.md)

