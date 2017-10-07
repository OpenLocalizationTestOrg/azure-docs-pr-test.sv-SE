---
title: "aaaFind data med loggen söker i Azure Log Analytics | Microsoft Docs"
description: "Loggen sökningar kan du toocombine och korrelera datorn data från flera källor i din miljö."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: 1161857a0027f05726492417362cb24a8fe21ef8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-data-using-log-searches-in-log-analytics"></a><span data-ttu-id="b875c-103">Hitta data med hjälp av loggen sökningar i logganalys</span><span class="sxs-lookup"><span data-stu-id="b875c-103">Find data using log searches in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="b875c-104">Den här artikeln beskriver loggen sökningar med hello aktuella frågespråket i logganalys.</span><span class="sxs-lookup"><span data-stu-id="b875c-104">This article describes log searches using hello current query language in Log Analytics.</span></span>  <span data-ttu-id="b875c-105">Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), läser du bör för[förstå loggen söker i logganalys (nya)](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="b875c-105">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should refer too[Understanding log searches in Log Analytics (new)](log-analytics-log-search-new.md).</span></span>


<span data-ttu-id="b875c-106">Hello kärnan i logganalys är hello loggen sökfunktionen där du toocombine och korrelera datorn data från flera källor i din miljö.</span><span class="sxs-lookup"><span data-stu-id="b875c-106">At hello core of Log Analytics is hello log search feature which allows you toocombine and correlate any machine data from multiple sources within your environment.</span></span> <span data-ttu-id="b875c-107">Lösningar är också drivs av loggen Sök toobring du mått pivoteras runt ett specifikt problem.</span><span class="sxs-lookup"><span data-stu-id="b875c-107">Solutions are also powered by log search toobring you metrics pivoted around a particular problem area.</span></span>

<span data-ttu-id="b875c-108">Du kan skapa en fråga på hello söksidan och sedan när du söker kan du filtrera hello resultat med hjälp av aspekten kontroller.</span><span class="sxs-lookup"><span data-stu-id="b875c-108">On hello Search page, you can create a query, and then when you search, you can filter hello results by using facet controls.</span></span> <span data-ttu-id="b875c-109">Du kan också skapa avancerade frågor tootransform, filter och rapporten på dina resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-109">You can also create advanced queries tootransform, filter, and report on your results.</span></span>

<span data-ttu-id="b875c-110">Vanliga loggen sökfrågor visas på de flesta sidor för lösningen.</span><span class="sxs-lookup"><span data-stu-id="b875c-110">Common log search queries appear on most solution pages.</span></span> <span data-ttu-id="b875c-111">I hela hello OMS-konsolen kan du klickar paneler eller öka detaljnivån i tooother objekt tooview detaljer om hello objekt genom att söka i loggen.</span><span class="sxs-lookup"><span data-stu-id="b875c-111">Throughout hello OMS console, you can click tiles or drill in tooother items tooview details about hello item by using log search.</span></span>

<span data-ttu-id="b875c-112">I den här självstudiekursen kommer går vi igenom exempel toocover alla hello grunderna när du använder loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="b875c-112">In this tutorial, we'll walk through examples toocover all hello basics when you use log search.</span></span>

<span data-ttu-id="b875c-113">Vi börjar med enkla, praktiska exempel och bygga på dem så att du kan få en förståelse av praktiska användningsområden toouse hello syntax tooextract hello insikter du hur från hello data.</span><span class="sxs-lookup"><span data-stu-id="b875c-113">We'll start with simple, practical examples and then build on them so that you can get an understanding of practical use cases about how toouse hello syntax tooextract hello insights you want from hello data.</span></span>

<span data-ttu-id="b875c-114">När du är bekant med sökmetoder kan du läsa hello [logganalys logga Sök referens](log-analytics-search-reference.md).</span><span class="sxs-lookup"><span data-stu-id="b875c-114">After you've familiar with search techniques, you can review hello [Log Analytics log search reference](log-analytics-search-reference.md).</span></span>

## <a name="use-basic-filters"></a><span data-ttu-id="b875c-115">Grundläggande filter</span><span class="sxs-lookup"><span data-stu-id="b875c-115">Use basic filters</span></span>
<span data-ttu-id="b875c-116">hello först öppna tooknow är den första hello-delen av en sökfråga innan någon ”|” lodräta vertikalstrecket är alltid en *filter*.</span><span class="sxs-lookup"><span data-stu-id="b875c-116">hello first thing tooknow is that hello first part of a search query, before any "|" vertical pipe character, is always a *filter*.</span></span> <span data-ttu-id="b875c-117">Du kan se den som en WHERE-sats i TSQL--den avgör *vad* delmängd av data toopull utanför hello OMS-datalagret.</span><span class="sxs-lookup"><span data-stu-id="b875c-117">You can think of it as a WHERE clause in TSQL--it determines *what* subset of data toopull out of hello OMS data store.</span></span> <span data-ttu-id="b875c-118">Söker i hello datalagret är i stort sett information om hur du anger hello egenskaper hello data som du vill tooextract, så är det naturligt att en fråga börjar med hello WHERE-satsen.</span><span class="sxs-lookup"><span data-stu-id="b875c-118">Searching in hello data store is largely about specifying hello characteristics of hello data that you want tooextract, so it is natural that a query would start with hello WHERE clause.</span></span>

<span data-ttu-id="b875c-119">hello mest grundläggande filter som du kan använda är *nyckelord*, till exempel 'fel' eller 'timeout- eller ett datornamn.</span><span class="sxs-lookup"><span data-stu-id="b875c-119">hello most basic filters you can use are *keywords*, such as ‘error’ or ‘timeout’, or a computer name.</span></span> <span data-ttu-id="b875c-120">Dessa typer av enkla frågor returnerar vanligtvis olika former av data i hello samma resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b875c-120">These types of simple queries generally return diverse shapes of data within hello same result set.</span></span> <span data-ttu-id="b875c-121">Detta beror på att Log Analytics har olika *typer* av data i hello system.</span><span class="sxs-lookup"><span data-stu-id="b875c-121">This is because Log Analytics has different *types* of data in hello system.</span></span>

### <a name="tooconduct-a-simple-search"></a><span data-ttu-id="b875c-122">tooconduct en enkel sökning</span><span class="sxs-lookup"><span data-stu-id="b875c-122">tooconduct a simple search</span></span>
1. <span data-ttu-id="b875c-123">I hello OMS-portalen klickar du på **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="b875c-123">In hello OMS portal, click **Log Search**.</span></span>  
    <span data-ttu-id="b875c-124">![Sök sida vid sida](./media/log-analytics-log-searches/oms-overview-log-search.png)</span><span class="sxs-lookup"><span data-stu-id="b875c-124">![search tile](./media/log-analytics-log-searches/oms-overview-log-search.png)</span></span>
2. <span data-ttu-id="b875c-125">Skriv i hello frågan `error` och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="b875c-125">In hello query field, type `error` and then click **Search**.</span></span>  
    <span data-ttu-id="b875c-126">![sökningsfel](./media/log-analytics-log-searches/oms-search-error.png)</span><span class="sxs-lookup"><span data-stu-id="b875c-126">![search error](./media/log-analytics-log-searches/oms-search-error.png)</span></span>  
    <span data-ttu-id="b875c-127">Till exempel hello frågan för `error` i hello returnerade följande bild 100 000 **händelse** poster (som samlas in av loggen Management), 18 **ConfigurationAlert** poster (som genereras av konfiguration Bedömning) och 12 **ConfigurationChange** poster (avbildas hello ändringsspårning).</span><span class="sxs-lookup"><span data-stu-id="b875c-127">For example, hello query for `error` in hello following image returned 100,000 **Event** records (collected by Log Management), 18 **ConfigurationAlert** records (generated by Configuration Assessment) and 12 **ConfigurationChange** records (captured by hello Change Tracking).</span></span>   
    <span data-ttu-id="b875c-128">![sökresultat](./media/log-analytics-log-searches/oms-search-results01.png)</span><span class="sxs-lookup"><span data-stu-id="b875c-128">![search results](./media/log-analytics-log-searches/oms-search-results01.png)</span></span>  

<span data-ttu-id="b875c-129">Dessa filter är inte riktigt typer/objektklasser.</span><span class="sxs-lookup"><span data-stu-id="b875c-129">These filters are not really object types/classes.</span></span> <span data-ttu-id="b875c-130">*Typen* är bara en tagg eller en egenskap eller en sträng/namn/kategori, som är bifogat tooa datablock.</span><span class="sxs-lookup"><span data-stu-id="b875c-130">*Type* is just a tag, or a property, or a string/name/category, that is attached tooa piece of data.</span></span> <span data-ttu-id="b875c-131">Vissa dokument i hello system märks **typ: ConfigurationAlert** och vissa märks **typ: Perf**, eller **typ: händelsen**och så vidare.</span><span class="sxs-lookup"><span data-stu-id="b875c-131">Some documents in hello system are tagged as **Type:ConfigurationAlert** and some are tagged as **Type:Perf**, or **Type:Event**, and so on.</span></span> <span data-ttu-id="b875c-132">Varje sökresultat, dokument, post eller post visas alla hello rådata egenskaper och deras värden för var och en av de olika delarna av data och du kan använda dessa fält namn toospecify i hello filter om du vill tooretrieve endast hello poster där hello fält har som anges värde.</span><span class="sxs-lookup"><span data-stu-id="b875c-132">Each search result, document, record, or entry displays all hello raw properties and their values for each of those pieces of data, and you can use those field names toospecify in hello filter when you want tooretrieve only hello records where hello field has that given value.</span></span>

<span data-ttu-id="b875c-133">*Typen* är egentligen bara ett fält som alla poster har det inte skiljer sig från ett annat fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-133">*Type* is really just a field that all records have, it is not different from any other field.</span></span> <span data-ttu-id="b875c-134">Det har upprättats utifrån hello värdet för hello Type-fältet.</span><span class="sxs-lookup"><span data-stu-id="b875c-134">This was established based on hello value of hello Type field.</span></span> <span data-ttu-id="b875c-135">Posten har ett annat formen eller formulär.</span><span class="sxs-lookup"><span data-stu-id="b875c-135">That record will have a different shape or form.</span></span> <span data-ttu-id="b875c-136">Tillfälligtvis, **typ = Perf**, eller **typ = händelse** är också hello-syntax som du behöver toolearn tooquery för prestandadata eller händelser.</span><span class="sxs-lookup"><span data-stu-id="b875c-136">Incidentally, **Type=Perf**, or **Type=Event** is also hello syntax that you need toolearn tooquery for performance data or events.</span></span>

<span data-ttu-id="b875c-137">Du kan använda ett kolon (:) eller ett likhetstecken (=) efter hello fältnamn och före hello värde.</span><span class="sxs-lookup"><span data-stu-id="b875c-137">You can use either a colon (:) or an equal sign (=) after hello field name and before hello value.</span></span> <span data-ttu-id="b875c-138">**Typ: händelsen** och **typ = händelse** har samma betydelse, kan du välja hello format du föredrar.</span><span class="sxs-lookup"><span data-stu-id="b875c-138">**Type:Event** and **Type=Event** are equivalent in meaning, you can choose hello style you prefer.</span></span>

<span data-ttu-id="b875c-139">Så om hello skriver = Perf poster har ett fält med namnet 'CounterName, och du kan skriva en fråga som liknar `Type=Perf CounterName="% Processor Time"`.</span><span class="sxs-lookup"><span data-stu-id="b875c-139">So, if hello Type=Perf records have a field called 'CounterName', then you can write a query resembling `Type=Perf CounterName="% Processor Time"`.</span></span>

<span data-ttu-id="b875c-140">Detta ger dig endast hello prestandadata där hello namn på prestandaräknare är ”% processortid”.</span><span class="sxs-lookup"><span data-stu-id="b875c-140">This will give you only hello performance data where hello performance counter name is "% Processor Time".</span></span>

### <a name="toosearch-for-processor-time-performance-data"></a><span data-ttu-id="b875c-141">toosearch för prestandadata tid för processor</span><span class="sxs-lookup"><span data-stu-id="b875c-141">toosearch for processor time performance data</span></span>
* <span data-ttu-id="b875c-142">Skriv i hello Sök fråga`Type=Perf CounterName="% Processor Time"`</span><span class="sxs-lookup"><span data-stu-id="b875c-142">In hello search query field, type `Type=Perf CounterName="% Processor Time"`</span></span>

<span data-ttu-id="b875c-143">Du kan också vara mer specifik och använda **InstanceName = _ ”totalt'** i hello fråga, vilket är en Windows-prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="b875c-143">You can also be more specific and use **InstanceName=_'Total'** in hello query, which is a Windows performance counter.</span></span> <span data-ttu-id="b875c-144">Du kan också välja en begränsningsaspekt och en annan **fältvärdet:**.</span><span class="sxs-lookup"><span data-stu-id="b875c-144">You can also select a facet and another **field:value**.</span></span> <span data-ttu-id="b875c-145">hello filter läggs automatiskt tooyour filter i hello fråga fältet.</span><span class="sxs-lookup"><span data-stu-id="b875c-145">hello filter is automatically added tooyour filter in hello query bar.</span></span> <span data-ttu-id="b875c-146">Du kan se dessa i hello följande bild.</span><span class="sxs-lookup"><span data-stu-id="b875c-146">You can see this in hello following image.</span></span> <span data-ttu-id="b875c-147">Den visar där tooclick tooadd **instansnamn: ”_Total”** toohello fråga utan att ange något.</span><span class="sxs-lookup"><span data-stu-id="b875c-147">It shows you where tooclick tooadd **InstanceName:’_Total’** toohello query without typing anything.</span></span>

![Sök aspekten](./media/log-analytics-log-searches/oms-search-facet.png)

<span data-ttu-id="b875c-149">Frågan blir nu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span><span class="sxs-lookup"><span data-stu-id="b875c-149">Your query now becomes `Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span></span>

<span data-ttu-id="b875c-150">I det här exemplet inte toospecify **typ = Perf** tooget toothis resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-150">In this example, you don't have toospecify **Type=Perf** tooget toothis result.</span></span> <span data-ttu-id="b875c-151">Eftersom det finns endast hello fälten CounterName och instansnamn för poster av typen = Perf hello frågan är specifikt nog tooreturn hello samma resultat som hello längre, föregående en:</span><span class="sxs-lookup"><span data-stu-id="b875c-151">Because hello fields CounterName and InstanceName only exist for records of Type=Perf, hello query is specific enough tooreturn hello same results as hello longer, previous one:</span></span>

```
CounterName="% Processor Time" InstanceName="_Total"
```

<span data-ttu-id="b875c-152">Detta beror på att alla hello filter i hello fråga utvärderas som *och* med varandra.</span><span class="sxs-lookup"><span data-stu-id="b875c-152">This is because all hello filters in hello query are evaluated as being in *AND* with each other.</span></span> <span data-ttu-id="b875c-153">Effektivt, hello fler fält som du lägger till toohello villkor du får mindre, mer specifikt och förfinad resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-153">Effectively, hello more fields you add toohello criteria, you get less, more specific and refined results.</span></span>

<span data-ttu-id="b875c-154">Till exempel hello fråga `Type=Event EventLog="Windows PowerShell"` är identiska för`Type=Event AND EventLog="Windows PowerShell"`.</span><span class="sxs-lookup"><span data-stu-id="b875c-154">For example, hello query `Type=Event EventLog="Windows PowerShell"` is identical too`Type=Event AND EventLog="Windows PowerShell"`.</span></span> <span data-ttu-id="b875c-155">Den returnerar alla händelser som har loggat in och samlas in från hello Windows PowerShell-händelseloggen.</span><span class="sxs-lookup"><span data-stu-id="b875c-155">It returns all events that were logged in and collected from hello Windows PowerShell event log.</span></span> <span data-ttu-id="b875c-156">Om du lägger till ett filter flera gånger genom att välja hello samma aspekten, sedan hello problemet är rent kosmetiskt--kanske det tar upp plats hello sökfältet, men fortfarande returneras hello samma resultat eftersom hello implicit och-operatorn alltid finns det flera gånger.</span><span class="sxs-lookup"><span data-stu-id="b875c-156">If you add a filter multiple times by repeatedly selecting hello same facet, then hello issue is purely cosmetic--it might clutter hello Search bar, but it still returns hello same results because hello implicit AND operator is always there.</span></span>

<span data-ttu-id="b875c-157">Du kan enkelt ångra hello implicit och-operator med hjälp av en operatorn inte uttryckligen.</span><span class="sxs-lookup"><span data-stu-id="b875c-157">You can easily reverse hello implicit AND operator by using a NOT operator explicitly.</span></span> <span data-ttu-id="b875c-158">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-158">For example:</span></span>

<span data-ttu-id="b875c-159">`Type:Event NOT(EventLog:"Windows PowerShell")`eller motsvarande `Type=Event EventLog!="Windows PowerShell"` returnera alla händelser från andra loggar som inte är hello Windows PowerShell loggfilen.</span><span class="sxs-lookup"><span data-stu-id="b875c-159">`Type:Event NOT(EventLog:"Windows PowerShell")` or its equivalent `Type=Event EventLog!="Windows PowerShell"` return all events from all other logs that are NOT hello Windows PowerShell log.</span></span>

<span data-ttu-id="b875c-160">Du kan också använda andra booleska operatorn som 'Eller'.</span><span class="sxs-lookup"><span data-stu-id="b875c-160">Or, you can use other Boolean operator such as ‘OR’.</span></span> <span data-ttu-id="b875c-161">hello följande fråga returnerar poster för vilka hello EventLog är antingen program eller System.</span><span class="sxs-lookup"><span data-stu-id="b875c-161">hello following query returns records for which hello EventLog is either Application OR System.</span></span>

```
EventLog=Application OR EventLog=System
```

<span data-ttu-id="b875c-162">Med hello ovan frågan får du poster för båda loggar i hello samma resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b875c-162">Using hello above query, you’ll get entries for both logs in hello same result set.</span></span>

<span data-ttu-id="b875c-163">Men om du tar bort hello eller genom att låta hello implicit och i drift genererar sedan hello följande fråga inte något resultat eftersom det inte finns en post i händelseloggen som tillhör tooBOTH loggar.</span><span class="sxs-lookup"><span data-stu-id="b875c-163">However, if you remove hello OR by leaving hello implicit AND in place, then hello following query will not produce any results because there isn’t an event log entry that belongs tooBOTH logs.</span></span> <span data-ttu-id="b875c-164">Varje post i händelseloggen skrevs tooonly något av två hello-loggar.</span><span class="sxs-lookup"><span data-stu-id="b875c-164">Each event log entry was written tooonly one of hello two logs.</span></span>

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a><span data-ttu-id="b875c-165">Använda ytterligare filter</span><span class="sxs-lookup"><span data-stu-id="b875c-165">Use additional filters</span></span>
<span data-ttu-id="b875c-166">hello returnerar följande fråga poster för 2 händelseloggar för alla hello-datorer som har skickat data.</span><span class="sxs-lookup"><span data-stu-id="b875c-166">hello following query returns entries for 2 event logs for all hello computers that have sent data.</span></span>

```
EventLog=Application OR EventLog=System
```

![sökresultat](./media/log-analytics-log-searches/oms-search-results03.png)

<span data-ttu-id="b875c-168">Att välja ett av hello fält eller filter ska begränsa hello frågan tooa specifika dator, förutom alla de.</span><span class="sxs-lookup"><span data-stu-id="b875c-168">Selecting one of hello fields or filters will narrow hello query tooa specific computer, excluding all other ones.</span></span> <span data-ttu-id="b875c-169">hello resulterande frågan liknar hello följande.</span><span class="sxs-lookup"><span data-stu-id="b875c-169">hello resulting query would resemble hello following.</span></span>

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

<span data-ttu-id="b875c-170">Vilket är likvärdiga toohello följande, på grund av hello implicit och.</span><span class="sxs-lookup"><span data-stu-id="b875c-170">Which is equivalent toohello following, because of hello implicit AND.</span></span>

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="b875c-171">Varje fråga har utvärderats i hello explicit ordning.</span><span class="sxs-lookup"><span data-stu-id="b875c-171">Each query is evaluated in hello following explicit order.</span></span> <span data-ttu-id="b875c-172">Obs hello parentes.</span><span class="sxs-lookup"><span data-stu-id="b875c-172">Note hello parenthesis.</span></span>

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="b875c-173">Precis som hello händelseloggen fält, kan du hämta data endast för en uppsättning specifika datorer genom att lägga till eller.</span><span class="sxs-lookup"><span data-stu-id="b875c-173">Just like hello event log field, you can retrieve data only for a set of specific computers by adding OR.</span></span> <span data-ttu-id="b875c-174">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-174">For example:</span></span>

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

<span data-ttu-id="b875c-175">På liknande sätt kan den här hello följande fråga returnerar **% processortid** för hello valda två datorer.</span><span class="sxs-lookup"><span data-stu-id="b875c-175">Similarly, this hello following query return **% CPU Time** for hello selected two computers only.</span></span>

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a><span data-ttu-id="b875c-176">Fälttyper</span><span class="sxs-lookup"><span data-stu-id="b875c-176">Field types</span></span>
<span data-ttu-id="b875c-177">När du skapar filter, bör du förstå hello skillnader i att arbeta med olika typer av fält som returneras av loggen sökningar.</span><span class="sxs-lookup"><span data-stu-id="b875c-177">When creating filters, you should understand hello differences in working with different types of fields returned by log searches.</span></span>

<span data-ttu-id="b875c-178">**Sökbara fält** visas i blått i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="b875c-178">**Searchable fields** show in blue in search results.</span></span>  <span data-ttu-id="b875c-179">Du kan använda sökbara fält i sökfältet villkor specifika toohello, till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="b875c-179">You can use searchable fields in search conditions specific toohello field such as hello following:</span></span>

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

<span data-ttu-id="b875c-180">**Kostnadsfri text sökbara fält** visas i grått i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="b875c-180">**Free text searchable fields** are shown in grey in search results.</span></span>  <span data-ttu-id="b875c-181">De kan inte användas med sökfältet villkor specifika toohello som sökbara fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-181">They cannot be used with search conditions specific toohello field like searchable fields.</span></span>  <span data-ttu-id="b875c-182">De genomsöks endast när du utför en fråga i alla fält, till exempel hello följande.</span><span class="sxs-lookup"><span data-stu-id="b875c-182">They are only searched when performing a query across all fields such as hello following.</span></span>

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a><span data-ttu-id="b875c-183">Booleska operatorer</span><span class="sxs-lookup"><span data-stu-id="b875c-183">Boolean operators</span></span>
<span data-ttu-id="b875c-184">Med datetime och numeriska fält, kan du söka efter värden med *större än*, *mindre än*, och *mindre än eller lika med*.</span><span class="sxs-lookup"><span data-stu-id="b875c-184">With datetime and numeric fields, you can search for values using *greater than*, *lesser than*, and *lesser than or equal*.</span></span> <span data-ttu-id="b875c-185">Du kan använda enkla operatorer som >, <>, =, < =,! = hello frågan sökfältet.</span><span class="sxs-lookup"><span data-stu-id="b875c-185">You can use simple operators such as >, < , >=, <= , != in hello query search bar.</span></span>

<span data-ttu-id="b875c-186">Du kan fråga en specifik händelselogg för en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="b875c-186">You can query a specific event log for a specific period of time.</span></span> <span data-ttu-id="b875c-187">Till exempel uttrycks hello senaste 24 timmarna med hello följande mnemoteknisk uttryck.</span><span class="sxs-lookup"><span data-stu-id="b875c-187">For example, hello last 24 hours is expressed with hello following mnemonic expression.</span></span>

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="toosearch-using-a-boolean-operator"></a><span data-ttu-id="b875c-188">toosearch med booleska operatorer</span><span class="sxs-lookup"><span data-stu-id="b875c-188">toosearch using a boolean operator</span></span>
* <span data-ttu-id="b875c-189">Skriv i hello Sök fråga`EventLog=System TimeGenerated>NOW-24HOURS`</span><span class="sxs-lookup"><span data-stu-id="b875c-189">In hello search query field, type `EventLog=System TimeGenerated>NOW-24HOURS`</span></span>  
    <span data-ttu-id="b875c-190">![söka med booleskt värde](./media/log-analytics-log-searches/oms-search-boolean.png)</span><span class="sxs-lookup"><span data-stu-id="b875c-190">![search with boolean](./media/log-analytics-log-searches/oms-search-boolean.png)</span></span>

<span data-ttu-id="b875c-191">Även om du kan styra hello tidsintervall grafiskt och de flesta gånger kanske du vill toodo att det finns fördelar tooincluding ett tidsfilter direkt till hello fråga.</span><span class="sxs-lookup"><span data-stu-id="b875c-191">Although you can control hello time interval graphically, and most times you might want toodo that, there are advantages tooincluding a time filter directly into hello query.</span></span> <span data-ttu-id="b875c-192">T.ex, detta passar utmärkt instrumentpaneler där du kan åsidosätta hello tid för varje bricka oavsett hello *globala* tid selector på hello instrumentpanelssida.</span><span class="sxs-lookup"><span data-stu-id="b875c-192">For example, this works great with dashboards where you can override hello time for each tile, regardless of hello *global* time selector on hello dashboard page.</span></span> <span data-ttu-id="b875c-193">Mer information finns i [tid frågor i instrumentpanelen](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span><span class="sxs-lookup"><span data-stu-id="b875c-193">For more information, see [Time Matters in Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span></span>

<span data-ttu-id="b875c-194">När du filtrerar efter tid, Kom ihåg att du får resultat för hello *skärningspunkten* av hello två tidsperioder: hello som har angetts i hello OMS-portalen (S1) och hello som har angetts i frågan hello (S2).</span><span class="sxs-lookup"><span data-stu-id="b875c-194">When filtering by time, keep in mind that you get results for hello *intersection* of hello two time periods: hello one specified in hello OMS portal (S1) and hello one specified in hello query (S2).</span></span>

![skärningspunkten](./media/log-analytics-log-searches/oms-search-intersection.png)

<span data-ttu-id="b875c-196">Detta innebär att, om hello tidsperioder inte överlappa, till exempel i hello OMS-portalen där du väljer **den här veckan** och i hello fråga där du definierar **förra veckan**, det finns inga skärningspunkten och du kommer inte ta emot några resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-196">This means, if hello time periods don’t intersect, for example in hello OMS portal where you choose **This week** and in hello query where you define **last week**, then there is no intersection and you won't receive any results.</span></span>

<span data-ttu-id="b875c-197">Jämförelseoperatorer som används för hello TimeGenerated fältet är också användbara i andra situationer.</span><span class="sxs-lookup"><span data-stu-id="b875c-197">Comparison operators used for hello TimeGenerated field are also useful in other situations.</span></span> <span data-ttu-id="b875c-198">Till exempel med numeriska fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-198">For example, with numeric fields.</span></span>

<span data-ttu-id="b875c-199">Till exempel anges att konfigurationen Assessment aviseringar har hello följande allvarlighetsgrader:</span><span class="sxs-lookup"><span data-stu-id="b875c-199">For example, given that Configuration Assessment’s alerts have hello following severity values:</span></span>

* <span data-ttu-id="b875c-200">0 = information</span><span class="sxs-lookup"><span data-stu-id="b875c-200">0 = Information</span></span>
* <span data-ttu-id="b875c-201">1 = varning</span><span class="sxs-lookup"><span data-stu-id="b875c-201">1 = Warning</span></span>
* <span data-ttu-id="b875c-202">2 = kritisk</span><span class="sxs-lookup"><span data-stu-id="b875c-202">2 = Critical</span></span>

<span data-ttu-id="b875c-203">Du kan fråga efter både varningar och kritiska aviseringar och utesluta information de med hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="b875c-203">You can query for both warning and critical alerts and also exclude informational ones with hello following query:</span></span>

```
Type=ConfigurationAlert  Severity>=1
```


<span data-ttu-id="b875c-204">Du kan också använda intervallet frågor.</span><span class="sxs-lookup"><span data-stu-id="b875c-204">You can also use range queries.</span></span> <span data-ttu-id="b875c-205">Det innebär att du kan ange hello början och slutet värdeintervallet i en sekvens.</span><span class="sxs-lookup"><span data-stu-id="b875c-205">This means that you can provide hello beginning and end range of values in a sequence.</span></span> <span data-ttu-id="b875c-206">Till exempel om du vill händelser från hello Operations Manager-händelseloggen om hello händelse-ID är större än eller lika med too2100 men inte mer än 2199, och sedan hello följande fråga returnerar dem.</span><span class="sxs-lookup"><span data-stu-id="b875c-206">For example, if you want events from hello Operations Manager event log where hello EventID is greater than or equal too2100 but not greater than 2199, then hello following query would return them.</span></span>

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> <span data-ttu-id="b875c-207">hello intervallet syntaxen måste du använda är hello kolon (:) fältvärdet: avgränsare och *inte* hello likhetstecken (=).</span><span class="sxs-lookup"><span data-stu-id="b875c-207">hello range syntax you must use is hello colon (:) field:value separator and *not* hello equal sign (=).</span></span> <span data-ttu-id="b875c-208">Hello lägre och övre intervallslut hello omges av hakparenteser och avgränsa dem med två punkter (.).</span><span class="sxs-lookup"><span data-stu-id="b875c-208">Enclose hello lower and upper end of hello range in square brackets and separate them with two periods (..).</span></span>
>
>

## <a name="manipulate-search-results"></a><span data-ttu-id="b875c-209">Hantera sökresultat</span><span class="sxs-lookup"><span data-stu-id="b875c-209">Manipulate search results</span></span>
<span data-ttu-id="b875c-210">När du söker efter data du vill toorefine sökningen och har en bra kontroll över hello resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-210">When you're searching for data, you'll want toorefine your search query and have a good level of control over hello results.</span></span> <span data-ttu-id="b875c-211">När resultaten hämtas, kan du använda kommandon tootransform dem.</span><span class="sxs-lookup"><span data-stu-id="b875c-211">When results are retrieved, you can apply commands tootransform them.</span></span>

<span data-ttu-id="b875c-212">Kommandon i logganalys sökningar *måste* följer efter hello lodräta vertikalstreck (|).</span><span class="sxs-lookup"><span data-stu-id="b875c-212">Commands in Log Analytics searches *must* follow after hello vertical pipe character (|).</span></span> <span data-ttu-id="b875c-213">Ett filter måste alltid vara hello första delen av en frågesträng.</span><span class="sxs-lookup"><span data-stu-id="b875c-213">A filter must always be hello first part of a query string.</span></span> <span data-ttu-id="b875c-214">Den definierar du arbetar med hello datauppsättning och sedan ”kommer” resultaten i ett kommando.</span><span class="sxs-lookup"><span data-stu-id="b875c-214">It defines hello data set you're working with and then "pipes" those results into a command.</span></span> <span data-ttu-id="b875c-215">Du kan sedan använda hello pipe tooadd ytterligare kommandon.</span><span class="sxs-lookup"><span data-stu-id="b875c-215">You can then use hello pipe tooadd additional commands.</span></span> <span data-ttu-id="b875c-216">Detta är löst liknande toohello Windows PowerShell-pipeline.</span><span class="sxs-lookup"><span data-stu-id="b875c-216">This is loosely similar toohello Windows PowerShell pipeline.</span></span>

<span data-ttu-id="b875c-217">I allmänhet hello logganalys sökspråk försöker toofollow PowerShell format och riktlinjer toomake it liknande toohello IT-proffs och tooease hello inlärningskurvan.</span><span class="sxs-lookup"><span data-stu-id="b875c-217">In general, hello Log Analytics search language tries toofollow PowerShell style and guidelines toomake it similar toohello IT pros, and tooease hello learning curve.</span></span>

<span data-ttu-id="b875c-218">Kommandon har namn med verb, så att du enkelt kan se vad de gör.</span><span class="sxs-lookup"><span data-stu-id="b875c-218">Commands have names of verbs so you can easily tell what they do.</span></span>  

### <a name="sort"></a><span data-ttu-id="b875c-219">Sortera</span><span class="sxs-lookup"><span data-stu-id="b875c-219">Sort</span></span>
<span data-ttu-id="b875c-220">hello sortera kommandot kan du toodefine hello sorteringsordning av ett eller flera fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-220">hello sort command allows you toodefine hello sorting order by one or multiple fields.</span></span> <span data-ttu-id="b875c-221">Även om du inte använder den, som standard, tillämpas taget fallande ordning.</span><span class="sxs-lookup"><span data-stu-id="b875c-221">Even if you don’t use it, by default, a time descending order is enforced.</span></span> <span data-ttu-id="b875c-222">hello senaste resultat är alltid hello överst i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="b875c-222">hello most recent results are always at hello top of search results.</span></span> <span data-ttu-id="b875c-223">Detta innebär att när du kör en sökning med `Type=Event EventID=1234` vad verkligen körs du är:</span><span class="sxs-lookup"><span data-stu-id="b875c-223">This means that when you run a search, with `Type=Event EventID=1234` what really is executed for you is:</span></span>

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

<span data-ttu-id="b875c-224">Det beror på att den är hello typ av du är bekant med i loggarna.</span><span class="sxs-lookup"><span data-stu-id="b875c-224">That's because it is hello type of experience you are familiar with in logs.</span></span> <span data-ttu-id="b875c-225">Till exempel i hello Windows Loggboken.</span><span class="sxs-lookup"><span data-stu-id="b875c-225">For example, in hello Windows Event Viewer.</span></span>

<span data-ttu-id="b875c-226">Du kan använda sortera toochange hello sätt resultat returneras.</span><span class="sxs-lookup"><span data-stu-id="b875c-226">You can use Sort toochange hello way results are returned.</span></span> <span data-ttu-id="b875c-227">hello följande exempel visar hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="b875c-227">hello following examples show how this works.</span></span>

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


<span data-ttu-id="b875c-228">hello enkla exempel ovan visar hur kommandon fungerar--de ändra hello resultat som hello filtret returnerade hello form.</span><span class="sxs-lookup"><span data-stu-id="b875c-228">hello simple examples above show you how commands work--they change hello shape of hello results that hello filter returned.</span></span>

### <a name="limit-and-top"></a><span data-ttu-id="b875c-229">Gränsen och upp</span><span class="sxs-lookup"><span data-stu-id="b875c-229">Limit and top</span></span>
<span data-ttu-id="b875c-230">Ett annat mindre kända kommando är GRÄNSEN.</span><span class="sxs-lookup"><span data-stu-id="b875c-230">Another less known command is LIMIT.</span></span> <span data-ttu-id="b875c-231">Gränsen är ett PowerShell-liknande verb.</span><span class="sxs-lookup"><span data-stu-id="b875c-231">Limit is a PowerShell-like verb.</span></span> <span data-ttu-id="b875c-232">Gränsen är funktionellt identiska toohello TOP-kommando.</span><span class="sxs-lookup"><span data-stu-id="b875c-232">Limit is functionally identical toohello TOP command.</span></span> <span data-ttu-id="b875c-233">hello följande frågor returnera hello samma resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-233">hello following queries return hello same results.</span></span>

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="toosearch-using-top"></a><span data-ttu-id="b875c-234">toosearch använder upp</span><span class="sxs-lookup"><span data-stu-id="b875c-234">toosearch using top</span></span>
* <span data-ttu-id="b875c-235">Skriv i hello Sök fråga`Type=Event EventID=600 | Top 1` </span><span class="sxs-lookup"><span data-stu-id="b875c-235">In hello search query field, type `Type=Event EventID=600 | Top 1` </span></span>  
    <span data-ttu-id="b875c-236">![Sök upp](./media/log-analytics-log-searches/oms-search-top.png)</span><span class="sxs-lookup"><span data-stu-id="b875c-236">![search top](./media/log-analytics-log-searches/oms-search-top.png)</span></span>

<span data-ttu-id="b875c-237">Hello bilden ovan, finns det 358 tusen poster med händelse-ID = 600.</span><span class="sxs-lookup"><span data-stu-id="b875c-237">In hello image above, there are 358 thousand records with EventID=600.</span></span> <span data-ttu-id="b875c-238">Hej fält, facets och filter för hello lämnas alltid visa information om resultaten hello *av hello filter del* av hello fråga, som ingår i hello innan alla vertikalstrecket.</span><span class="sxs-lookup"><span data-stu-id="b875c-238">hello fields, facets, and filters on hello left always show information about hello results returned *by hello filter portion* of hello query, which is hello part before any pipe character.</span></span> <span data-ttu-id="b875c-239">Hej **resultat** fönstret returnerar endast hello senaste 1 resultat, eftersom hello Exempelkommando Formats och omvandlas hello resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-239">hello **Results** pane only returns hello most recent 1 result, because hello example command shaped and transformed hello results.</span></span>

### <a name="select"></a><span data-ttu-id="b875c-240">Välj</span><span class="sxs-lookup"><span data-stu-id="b875c-240">Select</span></span>
<span data-ttu-id="b875c-241">hello SELECT-kommandot fungerar som Select-Object i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b875c-241">hello SELECT command behaves like Select-Object in PowerShell.</span></span> <span data-ttu-id="b875c-242">Returnerar den filtrerade resultat som inte har alla sina ursprungliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b875c-242">It returns filtered results that do not have all of their original properties.</span></span> <span data-ttu-id="b875c-243">I stället väljs endast hello-egenskaper som du anger.</span><span class="sxs-lookup"><span data-stu-id="b875c-243">Instead, it selects only hello properties that you specify.</span></span>

#### <a name="toorun-a-search-using-hello-select-command"></a><span data-ttu-id="b875c-244">toorun en sökning med hello select-kommando</span><span class="sxs-lookup"><span data-stu-id="b875c-244">toorun a search using hello select command</span></span>
1. <span data-ttu-id="b875c-245">Skriv i Sök `Type=Event` och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="b875c-245">In Search, type `Type=Event` and then click **Search**.</span></span>
2. <span data-ttu-id="b875c-246">Klicka på **+ visa fler** i något av hello resultat tooview alla hello egenskaper som hello resultat har.</span><span class="sxs-lookup"><span data-stu-id="b875c-246">Click **+ show more** in one of hello results tooview all hello properties that hello results have.</span></span>
3. <span data-ttu-id="b875c-247">Välj några av de uttryckligen och hello ändras frågan för`Type=Event | Select Computer,EventID,RenderedDescription`.</span><span class="sxs-lookup"><span data-stu-id="b875c-247">Select some of those explicitly, and hello query changes too`Type=Event | Select Computer,EventID,RenderedDescription`.</span></span>  
    <span data-ttu-id="b875c-248">![sökningens](./media/log-analytics-log-searches/oms-search-select.png)</span><span class="sxs-lookup"><span data-stu-id="b875c-248">![search select](./media/log-analytics-log-searches/oms-search-select.png)</span></span>

<span data-ttu-id="b875c-249">Det här kommandot är särskilt användbart när toocontrol Sök utdata och välj endast hello delar av data som verkligen är viktiga för din undersökning, vilket ofta inte hello fullständig post.</span><span class="sxs-lookup"><span data-stu-id="b875c-249">This command is particularly useful when you want toocontrol search output and choose only hello portions of data that really matter for your exploration, which often isn’t hello full record.</span></span> <span data-ttu-id="b875c-250">Detta är också användbart när poster av olika typer har *vissa* gemensamma egenskaper, men inte *alla* är gemensamma för deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b875c-250">This is also useful when records of different types have *some* common properties, but not *all* of their properties are common.</span></span> <span data-ttu-id="b875c-251">Du kan generera utdata som ser ut mer naturligt som en tabell eller fungera bra när exporteras tooa CSV-fil och sedan massaged i Excel.</span><span class="sxs-lookup"><span data-stu-id="b875c-251">The, you can generate output that looks more naturally like a table, or work well when exported tooa CSV file and then massaged in Excel.</span></span>

## <a name="use-hello-measure-command"></a><span data-ttu-id="b875c-252">Kommandot hello-mått</span><span class="sxs-lookup"><span data-stu-id="b875c-252">Use hello measure command</span></span>
<span data-ttu-id="b875c-253">MÅTT är ett av hello mest flexibla kommandon i logganalys-sökningar.</span><span class="sxs-lookup"><span data-stu-id="b875c-253">MEASURE is one of hello most versatile commands in Log Analytics searches.</span></span> <span data-ttu-id="b875c-254">Det gör att du tooapply statistiska *funktioner* tooyour data och mängdresultat grupperade efter ett visst fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-254">It allows you tooapply statistical *functions* tooyour data and aggregate results grouped by a given field.</span></span> <span data-ttu-id="b875c-255">Det finns flera statistiska funktioner som har stöd för måttet.</span><span class="sxs-lookup"><span data-stu-id="b875c-255">There are multiple statistical functions that Measure supports.</span></span>

### <a name="measure-count"></a><span data-ttu-id="b875c-256">Måttet count()</span><span class="sxs-lookup"><span data-stu-id="b875c-256">Measure count()</span></span>
<span data-ttu-id="b875c-257">Hej första statistiska funktionen toowork med och en av hello enklaste toounderstand är hello *count()* funktion.</span><span class="sxs-lookup"><span data-stu-id="b875c-257">hello first statistical function toowork with, and one of hello simplest toounderstand is hello *count()* function.</span></span>

<span data-ttu-id="b875c-258">Resultat från en sökfråga som `Type=Event`, visa filter som kallas även aspekter på vänster sida av sökresultat hello.</span><span class="sxs-lookup"><span data-stu-id="b875c-258">Results from any search query such as `Type=Event`, show filters also called facets on hello left side of search results.</span></span> <span data-ttu-id="b875c-259">hello filter Visar en fördelning av värden med ett visst fält för hello resultat i hello sökning gjordes.</span><span class="sxs-lookup"><span data-stu-id="b875c-259">hello filters show a distribution of values by a given field for hello results in hello search executed.</span></span>

![Sök mått Antal](./media/log-analytics-log-searches/oms-search-measure-count01.png)

<span data-ttu-id="b875c-261">Till exempel i hello bilden ovan du ser hello **datorn** och visar att inom hello nästan 739 tusen händelser i hello resultat, vissa 68 unika och skilda värden för hello **datorn** fält i dessa poster.</span><span class="sxs-lookup"><span data-stu-id="b875c-261">For example, in hello image above you'll see hello **Computer** field and it shows that within hello almost 739 thousand events in hello results, there are 68 unique and distinct values for hello **Computer** field in those records.</span></span> <span data-ttu-id="b875c-262">hello panelen visar hello övre 5, som är hello vanligaste 5 värden som är skrivna i hello **datorn** fält), sorterade efter hello antal dokument som innehåller specifika värdet i fältet.</span><span class="sxs-lookup"><span data-stu-id="b875c-262">hello tile only shows hello top 5, which are hello most common 5 values that are written in hello **Computer** fields), sorted by hello number of documents that contain that specific value in that field.</span></span> <span data-ttu-id="b875c-263">Hello bild ser du att – 90 tusen bland de nästan 369 tusen händelserna – komma från hello OpsInsights04.contoso.com dator, 83 tusen från hello DB03.contoso.com dator, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="b875c-263">In hello image you can see that – among those almost 369 thousand events – 90 thousand come from hello OpsInsights04.contoso.com computer, 83 thousand from hello DB03.contoso.com computer, and so on.</span></span>

<span data-ttu-id="b875c-264">Om du vill att toosee alla värden eftersom hello panelen visar endast hello topp 5?</span><span class="sxs-lookup"><span data-stu-id="b875c-264">What if you want toosee all values, since hello tile only shows only hello top 5?</span></span>

<span data-ttu-id="b875c-265">Som är vilka hello mått kommando kan göra med hello count()-funktionen.</span><span class="sxs-lookup"><span data-stu-id="b875c-265">That’s what hello measure command can do with hello count() function.</span></span> <span data-ttu-id="b875c-266">Den här funktionen används inte några parametrar.</span><span class="sxs-lookup"><span data-stu-id="b875c-266">This function doesn't use any parameters.</span></span> <span data-ttu-id="b875c-267">Du bara ange hello fält som du vill använda toogroup av – hello **datorn** fältet i det här fallet:</span><span class="sxs-lookup"><span data-stu-id="b875c-267">You just specify hello field by which you want toogroup by – hello **Computer** field in this case:</span></span>

`Type=Event | Measure count() by Computer`

![Sök mått Antal](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

<span data-ttu-id="b875c-269">Dock **datorn** är bara ett fält används *i* varje datadel – det finns inga relationsdatabaser inblandade och det finns ingen separat **datorn** objekt var som helst.</span><span class="sxs-lookup"><span data-stu-id="b875c-269">However, **Computer** is just a field used *in* each piece of data – there are no relational databases involved and there is no separate **Computer** object anywhere.</span></span> <span data-ttu-id="b875c-270">Bara hello värden *i* hello data beskrivs vilken entitet genererade dem, och ett antal andra egenskaper och aspekter av hello-data – därför hello termen *aspekten*.</span><span class="sxs-lookup"><span data-stu-id="b875c-270">Just hello values *in* hello data can describe which entity generated them, and a number of other characteristics and aspects of hello data – hence hello term *facet*.</span></span> <span data-ttu-id="b875c-271">Du kan bara också gruppera efter andra fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-271">However, you can just as well group by other fields.</span></span> <span data-ttu-id="b875c-272">Eftersom hello ursprungliga resultaten av nästan 739 tusen händelser som skickas till hello mått kommando har också ett fält med namnet **EventID**, kan du använda hello samma teknik toogroup av fältet och få ett antal händelser efter händelse-ID:</span><span class="sxs-lookup"><span data-stu-id="b875c-272">Because hello original results of almost 739 thousand events that are piped into hello measure command also have a field called **EventID**, you can apply hello same technique toogroup by that field and get a count of events by EventID:</span></span>

```
Type=Event | Measure count() by EventID
```

<span data-ttu-id="b875c-273">Om du inte är intresserad av hello antal faktiska poster som innehåller ett visst värde, men i stället om du bara vill en lista över hello värden själva, kan du lägga till en *Välj* kommandot hello slutet av den och bara väljer hello första kolumnen:</span><span class="sxs-lookup"><span data-stu-id="b875c-273">If you're not interested in hello actual record count that contain a specific value, but instead if you only want a list of hello values themselves, you can add a *Select* command at hello end of it and just select hello first column:</span></span>

```
Type=Event | Measure count() by EventID | Select EventID
```

<span data-ttu-id="b875c-274">Sedan kan du få mer komplicerat och sortera hello resulterar i hello fråga, eller klicka bara hello kolumner i hello rutnät för.</span><span class="sxs-lookup"><span data-stu-id="b875c-274">Then you can get more intricate and pre-sort hello results in hello query, or you can just click hello columns in hello grid, too.</span></span>

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="toosearch-using-measure-count"></a><span data-ttu-id="b875c-275">toosearch använder mått Antal</span><span class="sxs-lookup"><span data-stu-id="b875c-275">toosearch using measure count</span></span>
* <span data-ttu-id="b875c-276">Skriv i hello Sök fråga`Type=Event | Measure count() by EventID`</span><span class="sxs-lookup"><span data-stu-id="b875c-276">In hello search query field, type `Type=Event | Measure count() by EventID`</span></span>
* <span data-ttu-id="b875c-277">Lägg till `| Select EventID` toohello slutet av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="b875c-277">Append `| Select EventID` toohello end of hello query.</span></span>
* <span data-ttu-id="b875c-278">Lägg slutligen till `| Sort EventID asc` toohello slutet av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="b875c-278">Finally, append `| Sort EventID asc` toohello end of hello query.</span></span>

<span data-ttu-id="b875c-279">Det finns några viktiga punkter toonotice och betona:</span><span class="sxs-lookup"><span data-stu-id="b875c-279">There are a couple important points toonotice and emphasize:</span></span>

<span data-ttu-id="b875c-280">Först är hello resultat som du ser inte hello ursprungliga rådata resultat längre.</span><span class="sxs-lookup"><span data-stu-id="b875c-280">First, hello results you see are not hello original raw results anymore.</span></span> <span data-ttu-id="b875c-281">De är i stället sammanlagda resultat – i princip grupper av resultaten.</span><span class="sxs-lookup"><span data-stu-id="b875c-281">Instead, they are aggregated results – essentially groups of results.</span></span> <span data-ttu-id="b875c-282">Detta är inte ett problem, men bör du känna till att du interagerar med en mycket annan form av data som skiljer sig från hello ursprungliga rådata formen som skapas på hello Lägg till följd av aggregering statistiska hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="b875c-282">This isn't a problem, but you should understand that you're interacting with a very different shape of data that differs from hello original raw shape that gets created on hello fly as a result of hello aggregation/statistical function.</span></span>

<span data-ttu-id="b875c-283">Andra, **mäta antalet** för närvarande returnerar endast hello översta 100 distinkta resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-283">Second, **Measure count** currently returns only hello top 100 distinct results.</span></span> <span data-ttu-id="b875c-284">Den här begränsningen gäller inte toohello andra statistiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="b875c-284">This limit does not apply toohello other statistical functions.</span></span> <span data-ttu-id="b875c-285">Därför måste vanligtvis toouse en mer exakt första filtret toosearch för specifika objekt innan du installerar mått count().</span><span class="sxs-lookup"><span data-stu-id="b875c-285">So, you'll usually need toouse a more precise filter first toosearch for specific items before you apply measure count().</span></span>

## <a name="use-hello-max-and-min-functions-with-hello-measure-command"></a><span data-ttu-id="b875c-286">Använd Hej max och min funktioner med hello mått</span><span class="sxs-lookup"><span data-stu-id="b875c-286">Use hello max and min functions with hello measure command</span></span>
<span data-ttu-id="b875c-287">Det finns olika scenarier där **mått Max()** och **mått Min()** är användbara.</span><span class="sxs-lookup"><span data-stu-id="b875c-287">There are various scenarios where **Measure Max()** and **Measure Min()** are useful.</span></span> <span data-ttu-id="b875c-288">Men eftersom varje funktion är motsatt av varandra, kommer vi illustrerar Max() och du kan experimentera med Min() på egen hand.</span><span class="sxs-lookup"><span data-stu-id="b875c-288">However, since each function is opposite of each other, we'll illustrate Max() and you can experiment with Min() on your own.</span></span>

<span data-ttu-id="b875c-289">Om du frågar efter säkerhetshändelser som de har en **nivå** egenskap som kan variera.</span><span class="sxs-lookup"><span data-stu-id="b875c-289">If you query for security events, they have a **Level** property that can vary.</span></span> <span data-ttu-id="b875c-290">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-290">For example:</span></span>

```
Type=SecurityEvent
```

![Sök mått Antal start](./media/log-analytics-log-searches/oms-search-measure-max01.png)

<span data-ttu-id="b875c-292">Om du vill tooview hello högsta värde för alla hello säkerhet kan händelser ges en gemensam dator hello Gruppera efter fält, du använda</span><span class="sxs-lookup"><span data-stu-id="b875c-292">If you want tooview hello highest value for all of hello security events given a common Computer, hello group by field, you can use</span></span>

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![söka mått max dator](./media/log-analytics-log-searches/oms-search-measure-max02.png)

<span data-ttu-id="b875c-294">Visas som för hello-datorer som hade **nivå** poster, de flesta av dem har minst nivå 8 många hade en nivå av 16.</span><span class="sxs-lookup"><span data-stu-id="b875c-294">It will display that for hello computers that had **Level** records, most of them have at least level 8, many had a level of 16.</span></span>

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![max söktiden för måttet genereras dator](./media/log-analytics-log-searches/oms-search-measure-max03.png)

<span data-ttu-id="b875c-296">Den här funktionen fungerar bra med siffror, men den fungerar även med DateTime-fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-296">This function works well with numbers, but it also works with DateTime fields.</span></span> <span data-ttu-id="b875c-297">Det är användbart toocheck för hello senast eller den senaste tidsstämpeln för ett datablock indexerade för varje dator.</span><span class="sxs-lookup"><span data-stu-id="b875c-297">It is useful toocheck for hello last or most recent time stamp for any piece of data indexed for each computer.</span></span> <span data-ttu-id="b875c-298">Exempel: när hello senaste säkerhetshändelse rapporterades för varje dator?</span><span class="sxs-lookup"><span data-stu-id="b875c-298">For example: When was hello most recent security event reported for each machine?</span></span>

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-hello-avg-function-with-hello-measure-command"></a><span data-ttu-id="b875c-299">Använd hello avg-funktionen med hello mått</span><span class="sxs-lookup"><span data-stu-id="b875c-299">Use hello avg function with hello measure command</span></span>
<span data-ttu-id="b875c-300">Hej Avg() statistisk funktion som används med mått kan du toocalculate hello medelvärdet för vissa fält och grupp resultat av hello samma eller andra fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-300">hello Avg() statistical function used with measure allows you toocalculate hello average value for some field, and group results by hello same or other field.</span></span> <span data-ttu-id="b875c-301">Detta är användbart i en mängd fall, till exempel prestandadata.</span><span class="sxs-lookup"><span data-stu-id="b875c-301">This is useful in a variety of cases, such as performance data.</span></span>

<span data-ttu-id="b875c-302">Vi börjar med prestandadata.</span><span class="sxs-lookup"><span data-stu-id="b875c-302">We'll start with performance data.</span></span> <span data-ttu-id="b875c-303">Observera att OMS för närvarande samlas in prestandaräknare för både Windows- och Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="b875c-303">Note that OMS currently collects performance counters for both Windows and Linux machines.</span></span>

<span data-ttu-id="b875c-304">toosearch för *alla* prestandadata, mest grundläggande hello-frågan är:</span><span class="sxs-lookup"><span data-stu-id="b875c-304">toosearch for *all* performance data, hello most basic query is:</span></span>

```
Type=Perf
```

![Sök avg start](./media/log-analytics-log-searches/oms-search-avg01.png)

<span data-ttu-id="b875c-306">hello första märke är att visar logganalys tre perspektiv: listan som visas som visar hello faktiska poster bakom hello bubbeldiagram. Tabell som visar en tabellvy prestandaräknardata; och mått som visar diagram för hello prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="b875c-306">hello first thing you'll notice is that Log Analytics shows you three perspectives: List, which shows you which shows hello actual records behind hello charts; Table, which shows a tabular view of performance counter data; and Metrics, which shows charts for hello performance counters.</span></span>

<span data-ttu-id="b875c-307">Det finns två uppsättningar med fält som har markerats som visar hello följande i hello bilden ovan:</span><span class="sxs-lookup"><span data-stu-id="b875c-307">In hello image above, there are two sets of fields marked that indicate hello following:</span></span>

* <span data-ttu-id="b875c-308">hello första uppsättningen identifierar Windows namn på prestandaräknare, objektnamn och instansnamnet i hello frågefilter.</span><span class="sxs-lookup"><span data-stu-id="b875c-308">hello first set identifies Windows Performance Counter Name, Object Name, and Instance Name in hello query filter.</span></span> <span data-ttu-id="b875c-309">Dessa är hello fält du förmodligen oftast används som facets-filter</span><span class="sxs-lookup"><span data-stu-id="b875c-309">These are hello fields you probably will most commonly use as facets/filters</span></span>
* <span data-ttu-id="b875c-310">**CounterValue** är hello faktiska värdet för hello räknaren.</span><span class="sxs-lookup"><span data-stu-id="b875c-310">**CounterValue** is hello actual value of hello counter.</span></span> <span data-ttu-id="b875c-311">I det här exemplet hello-värdet är *75*.</span><span class="sxs-lookup"><span data-stu-id="b875c-311">In this example, hello value is *75*.</span></span>
* <span data-ttu-id="b875c-312">**TimeGenerated** är 12:51 i 24-timmarsformat.</span><span class="sxs-lookup"><span data-stu-id="b875c-312">**TimeGenerated** is 12:51, in 24-hour time format.</span></span>

<span data-ttu-id="b875c-313">Här är en vy över hello mått i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="b875c-313">Here's a view of hello metrics in a graph.</span></span>

![Sök avg start](./media/log-analytics-log-searches/oms-search-avg02.png)

<span data-ttu-id="b875c-315">Du kan använda den här typen av numeriska data mått Avg() tooaggregate när du läsa mer om hello Perf post form och läst om andra metoder för sökning.</span><span class="sxs-lookup"><span data-stu-id="b875c-315">After reading about hello Perf record shape, and having read about other search techniques, you can use measure Avg() tooaggregate this type of numerical data.</span></span>

<span data-ttu-id="b875c-316">Här är ett enkelt exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-316">Here's a simple example:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Sök avg samplevalue](./media/log-analytics-log-searches/oms-search-avg03.png)

<span data-ttu-id="b875c-318">I det här exemplet väljer du hello prestandaräknare för totala CPU-tid och genomsnitt per dator.</span><span class="sxs-lookup"><span data-stu-id="b875c-318">In this example, you select hello CPU Total Time performance counter and average by Computer.</span></span> <span data-ttu-id="b875c-319">Om du vill toonarrow ned din resultat tooonly hello vara 6 timmar, kan du använda hello tid filterkontrollen eller ange i frågan på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b875c-319">If you want toonarrow down your results tooonly hello last 6 hours, you can either use hello time filter control or specify in your query as follows:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="toosearch-using-hello-avg-function-with-hello-measure-command"></a><span data-ttu-id="b875c-320">toosearch med hello mått kommandot hello avg-funktionen</span><span class="sxs-lookup"><span data-stu-id="b875c-320">toosearch using hello avg function with hello measure command</span></span>
* <span data-ttu-id="b875c-321">Skriv i hello Sök frågan `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span><span class="sxs-lookup"><span data-stu-id="b875c-321">In hello Search query box, type `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span></span>

<span data-ttu-id="b875c-322">Du kan aggregera och korrelera data *över* datorer.</span><span class="sxs-lookup"><span data-stu-id="b875c-322">You can aggregate and correlate data *across* computers.</span></span> <span data-ttu-id="b875c-323">Anta exempelvis att du har en uppsättning värdar i någon form av servergruppen där varje nod är lika tooany andra och de göra bara alla hello samma typ av arbete och belastningen ska balanseras ungefär.</span><span class="sxs-lookup"><span data-stu-id="b875c-323">For example, imagine that you have a set of hosts in some sort of farm where each node is equal tooany other one and they just do all hello same type of work and load should be roughly balanced.</span></span> <span data-ttu-id="b875c-324">Du kan få räknare i en Gå med hello följande fråga efter och hämta medelvärden för hello hela servergruppen.</span><span class="sxs-lookup"><span data-stu-id="b875c-324">You could get their counters all in one go with hello following query and get averages for hello entire farm.</span></span> <span data-ttu-id="b875c-325">Du kan starta genom att välja hello datorer med hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-325">You can start by choosing hello computers with hello following example:</span></span>

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="b875c-326">Nu när du har hello datorer måste du bara vill tooselect två nyckeltal (KPI: er): % CPU-användning och % ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="b875c-326">Now that you have hello computers, you also only want tooselect two key performance indicators (KPIs): % CPU Usage and % Free Disk Space.</span></span> <span data-ttu-id="b875c-327">Därför blir som en del av hello fråga:</span><span class="sxs-lookup"><span data-stu-id="b875c-327">So, that part of hello query becomes:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

<span data-ttu-id="b875c-328">Du kan nu lägga till datorer och räknare med hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-328">Now you can add computers and counters with hello following example:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="b875c-329">Eftersom du har en särskild markering hello **mäta Avg()** kommando kan returnera hello medelvärde inte av datorn, men över hello-grupp genom att gruppera efter CounterName.</span><span class="sxs-lookup"><span data-stu-id="b875c-329">Because you have a very specific selection, hello **measure Avg()** command can return hello average not by computer, but across hello farm, simply by grouping by CounterName.</span></span> <span data-ttu-id="b875c-330">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-330">For example:</span></span>

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

<span data-ttu-id="b875c-331">Detta ger dig en användbar komprimerad vy i ett par KPI: er i din miljö.</span><span class="sxs-lookup"><span data-stu-id="b875c-331">This gives you a useful compact view of a couple of your environment's KPIs.</span></span>

![Sök avg gruppering](./media/log-analytics-log-searches/oms-search-avg04.png)

<span data-ttu-id="b875c-333">Du kan enkelt använda hello sökfråga på en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="b875c-333">You can easily use hello search query in a dashboard.</span></span> <span data-ttu-id="b875c-334">T.ex, du kan spara hello sökfråga och skapa en instrumentpanel i den namngivna *Web servergruppen KPI: er*.</span><span class="sxs-lookup"><span data-stu-id="b875c-334">For example, you could save hello search query and create a dashboard from it named *Web Farm KPIs*.</span></span> <span data-ttu-id="b875c-335">toolearn mer information om hur du använder instrumentpaneler, se [skapa en anpassad instrumentpanel i logganalys](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="b875c-335">toolearn more about using dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span>

![Sök avg instrumentpanelen](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-hello-sum-function-with-hello-measure-command"></a><span data-ttu-id="b875c-337">Använd hello sum-funktionen med hello mått</span><span class="sxs-lookup"><span data-stu-id="b875c-337">Use hello sum function with hello measure command</span></span>
<span data-ttu-id="b875c-338">hello sum-funktionen är liknande tooother funktioner hello mått-kommando.</span><span class="sxs-lookup"><span data-stu-id="b875c-338">hello sum function is similar tooother functions of hello measure command.</span></span> <span data-ttu-id="b875c-339">Du kan se ett exempel på hur hur toouse hello sum-funktionen på [W3C IIS loggar sökning i Microsoft Azure åtgärdsinformation](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="b875c-339">You can see an example about how toouse hello sum function at [W3C IIS Logs Search in Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span></span>

<span data-ttu-id="b875c-340">Du kan använda Max() och Min() med siffror, datum och textsträngar.</span><span class="sxs-lookup"><span data-stu-id="b875c-340">You can use Max() and Min() with numbers, date times and text strings.</span></span> <span data-ttu-id="b875c-341">Med textsträngar, de sorteras alfabetiskt och du hämta första och sista.</span><span class="sxs-lookup"><span data-stu-id="b875c-341">With text strings, they are sorted alphabetically and you get first and last.</span></span>

<span data-ttu-id="b875c-342">Du kan dock använda SUMMA() med något annat än numeriska fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-342">However, you cannot use Sum() with anything other than numerical fields.</span></span> <span data-ttu-id="b875c-343">Detta gäller även tooAvg().</span><span class="sxs-lookup"><span data-stu-id="b875c-343">This also applies tooAvg().</span></span>

### <a name="use-hello-percentile-function-with-hello-measure-command"></a><span data-ttu-id="b875c-344">Använd hello percentilfunktionen med hello mått</span><span class="sxs-lookup"><span data-stu-id="b875c-344">Use hello percentile function with hello measure command</span></span>
<span data-ttu-id="b875c-345">Hej percentilfunktionen är liknande tooAvg() och SUMMA() i som kan endast använda för numeriska fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-345">hello percentile function is similar tooAvg() and Sum() in that you can only use it for numerical fields.</span></span> <span data-ttu-id="b875c-346">Du kan använda alla percentil mellan 1 too99 på ett numeriskt fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-346">You can use any percentile between 1 too99 on a numeric field.</span></span> <span data-ttu-id="b875c-347">Du kan också använda båda **percentil** och **pct** kommandon.</span><span class="sxs-lookup"><span data-stu-id="b875c-347">You can also use both **percentile** and **pct** commands.</span></span> <span data-ttu-id="b875c-348">Här följer några exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-348">Here are few examples:</span></span>  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-hello-where-command"></a><span data-ttu-id="b875c-349">Använd hello där kommandot</span><span class="sxs-lookup"><span data-stu-id="b875c-349">Use hello where command</span></span>
<span data-ttu-id="b875c-350">Hej där kommandot fungerar som ett filter, men den kan användas i hello pipeline toofurther filter samman resultat som har tillverkats av ett mått kommando – som skillnad från tooraw resultat som är filtrerade hello början av en fråga.</span><span class="sxs-lookup"><span data-stu-id="b875c-350">hello where command works like a filter, but it can be applied in hello pipeline toofurther filter aggregated results that have been produced by a Measure command – as opposed tooraw results that are filtered at hello beginning of a query.</span></span>

<span data-ttu-id="b875c-351">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-351">For example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

<span data-ttu-id="b875c-352">Du kan lägga till en annan pipe ”|” tecken och hello där kommandot tooonly få datorer vars Genomsnittlig CPU är över 80 procent med hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-352">You can add another pipe "|" character and hello Where command tooonly get computers whose average CPU is above 80%, with hello following example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

<span data-ttu-id="b875c-353">Om du är bekant med Microsoft System Center - Operations Manager, du kan tänka dig hello där kommandot i management pack villkor.</span><span class="sxs-lookup"><span data-stu-id="b875c-353">If you're familiar with Microsoft System Center - Operations Manager, you can think of hello where command in management pack terms.</span></span> <span data-ttu-id="b875c-354">En regel om hello exempel, är hello första delen av frågan hello hello datakälla och hello där skulle kommandot se ut hello villkorsidentifiering.</span><span class="sxs-lookup"><span data-stu-id="b875c-354">If hello example were a rule, hello first part of hello query would be hello data source and hello where command would be hello condition detection.</span></span>

<span data-ttu-id="b875c-355">Du kan använda hello frågan som en panel i **min instrumentpanel**, som en Övervakare av sorteringar, toosee när datorn processorer är överutnyttjade.</span><span class="sxs-lookup"><span data-stu-id="b875c-355">You can use hello query as a tile in **My Dashboard**, as a monitor of sorts, toosee when computer CPUs are over-utilized.</span></span> <span data-ttu-id="b875c-356">toolearn mer om instrumentpaneler, se [skapa en anpassad instrumentpanel i logganalys](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="b875c-356">toolearn more about dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span> <span data-ttu-id="b875c-357">Du kan också skapa och använda instrumentpaneler med hjälp av hello mobila appar.</span><span class="sxs-lookup"><span data-stu-id="b875c-357">You can also create and use dashboards using hello mobile app.</span></span> <span data-ttu-id="b875c-358">Mer information finns i [OMS Mobilapp ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span><span class="sxs-lookup"><span data-stu-id="b875c-358">For more information, see [OMS Mobile App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span></span> <span data-ttu-id="b875c-359">I hello ned två paneler för hello följande bild, kan du se hello Övervakare visas en lista och som ett tal.</span><span class="sxs-lookup"><span data-stu-id="b875c-359">In hello bottom two tiles of hello following image, you can see hello monitor displayed a list and as a number.</span></span> <span data-ttu-id="b875c-360">I princip du alltid vill hello nummer toobe noll och hello listan toobe tom.</span><span class="sxs-lookup"><span data-stu-id="b875c-360">Essentially, you always want hello number toobe zero and hello list toobe empty.</span></span> <span data-ttu-id="b875c-361">Annars visar en varning villkor.</span><span class="sxs-lookup"><span data-stu-id="b875c-361">Otherwise, it indicates an alert condition.</span></span> <span data-ttu-id="b875c-362">Om det behövs kan använda du den tootake en titt på vilka datorer som är under hög belastning.</span><span class="sxs-lookup"><span data-stu-id="b875c-362">If needed, you can use it tootake a look at which machines are under pressure.</span></span>

![mobila instrumentpanelen](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-hello-in-operator"></a><span data-ttu-id="b875c-364">Använd hello i operatorn</span><span class="sxs-lookup"><span data-stu-id="b875c-364">Use hello in operator</span></span>
<span data-ttu-id="b875c-365">Hej *IN* -operator, tillsammans med *NOT IN* kan du toouse subsearches som sökningar som inkluderar en ny sökning som argument.</span><span class="sxs-lookup"><span data-stu-id="b875c-365">hello *IN* operator, along with *NOT IN* allows you toouse subsearches, which are searches that include another search as an argument.</span></span> <span data-ttu-id="b875c-366">De finns i klammerparenteserna {} i en annan *primära* eller *yttre* sökning.</span><span class="sxs-lookup"><span data-stu-id="b875c-366">They are contained in braces {} within another *primary* or *outer* search.</span></span> <span data-ttu-id="b875c-367">hello resultatet av en subsearch ofta en lista över olika resultat används sedan som ett argument i dess primära sökningen.</span><span class="sxs-lookup"><span data-stu-id="b875c-367">hello result of a subsearch, often a list of distinct results, is then used as an argument in its primary search.</span></span>

<span data-ttu-id="b875c-368">Du kan använda subsearches toomatch delmängder av data som du inte kan beskriva direkt i ett sökuttryck, men som kan genereras från en sökning.</span><span class="sxs-lookup"><span data-stu-id="b875c-368">You can use subsearches toomatch subsets of your data that you cannot describe directly in a search expression, but which can be generated from a search.</span></span> <span data-ttu-id="b875c-369">Om du är intresserad av att använda en sökning toofind alla händelser från exempelvis *datorer som saknar säkerhetsuppdateringar*, måste du toodesign en subsearch som identifierar som först *datorer som saknar säkerhetsuppdateringar*  innan påträffas händelser som tillhör toothose värdar.</span><span class="sxs-lookup"><span data-stu-id="b875c-369">For example, if you’re interested in using one search toofind all events from *computers missing security updates*, then you need toodesign a subsearch that first identifies that *computers missing security updates* before it finds events belonging toothose hosts.</span></span>

<span data-ttu-id="b875c-370">Så kan du express *datorer som för närvarande saknar nödvändiga säkerhetsuppdateringar* med hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="b875c-370">So, you could express *computers currently missing required security updates* with hello following query:</span></span>

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![I Sök-exempel](./media/log-analytics-log-searches/oms-search-in01-revised.png)

<span data-ttu-id="b875c-372">När du har hello lista använda du hello sökning som en inre Sök toofeed hello lista över datorer i en yttre (primära) sökning som söker efter händelser för dessa datorer.</span><span class="sxs-lookup"><span data-stu-id="b875c-372">Once you have hello list, you can use hello search as an inner search toofeed hello list of computers into an outer (primary) search that will look for events for those computers.</span></span> <span data-ttu-id="b875c-373">Du gör detta genom omslutande hello inre Sök i klammerparenteser och mata resultaten som möjliga värden för ett filter/fält i hello yttre sökning med hello i-operatorn.</span><span class="sxs-lookup"><span data-stu-id="b875c-373">You do this by enclosing hello inner search in braces and feeding its results as possible values for a filter/field in hello outer search using hello IN operator.</span></span> <span data-ttu-id="b875c-374">hello frågan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="b875c-374">hello query would resemble:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![I Sök-exempel](./media/log-analytics-log-searches/oms-search-in02-revised.png)

<span data-ttu-id="b875c-376">Meddelande hello tid filter som används i hello inre sökning eftersom hello System Update Assessment tar också en ögonblicksbild av alla datorer var 24: e timme.</span><span class="sxs-lookup"><span data-stu-id="b875c-376">Also notice hello time filter used in hello inner search because hello System Update Assessment takes a snapshot of all computers every 24 hours.</span></span> <span data-ttu-id="b875c-377">Du kan göra hello inre frågan lightweight och mer exakt genom att bara söka efter en dag.</span><span class="sxs-lookup"><span data-stu-id="b875c-377">You can make hello inner query more lightweight and precise by only searching for a day.</span></span> <span data-ttu-id="b875c-378">hello yttre Sök används i stället hello valet av tid i hello användargränssnittet hämtning av händelser från hello senaste 7 dagarna.</span><span class="sxs-lookup"><span data-stu-id="b875c-378">hello outer search instead uses hello time selection in hello user interface, retrieving events from hello last 7 days.</span></span> <span data-ttu-id="b875c-379">Se [booleska operatorer](#boolean-operators) mer information om tid operatörer.</span><span class="sxs-lookup"><span data-stu-id="b875c-379">See [Boolean operators](#boolean-operators) for more information about time operators.</span></span>

<span data-ttu-id="b875c-380">Eftersom du bara använda hello resultaten av inre hello som värde för filtret Hej yttre en, kan du fortfarande använda kommandon i hello yttre sökningen.</span><span class="sxs-lookup"><span data-stu-id="b875c-380">Because you really only use hello results of hello inner search as a filter value for hello outer one, you can still apply commands in hello outer search.</span></span> <span data-ttu-id="b875c-381">Du kan till exempel fortfarande grupp hello ovan händelser med ett annat mått-kommando:</span><span class="sxs-lookup"><span data-stu-id="b875c-381">For example, you can still group hello above events with another measure command:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![I Sök-exempel](./media/log-analytics-log-searches/oms-search-in03-revised.png)

<span data-ttu-id="b875c-383">I allmänhet kan du inre frågan-tooexecute snabbt eftersom logganalys har tjänsten på klientsidan tidsgränser för dem och även tooreturn en liten mängd resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-383">Generally, you want your inner query tooexecute quickly because Log Analytics has service-side timeouts for it and also tooreturn a small amount of results.</span></span> <span data-ttu-id="b875c-384">Om hello inre frågan returnerar fler resultat, hämtar hello resultatlistan trunkerad, vilket kan medföra hello yttre Sök tooreturn felaktiga resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-384">If hello inner query returns more results, hello result list gets truncated, which could potentially cause hello outer search tooreturn incorrect results.</span></span>

<span data-ttu-id="b875c-385">En annan regel är hello inre sökningen för närvarande måste tooprovide *aggregerade* resultat.</span><span class="sxs-lookup"><span data-stu-id="b875c-385">Another rule is that hello inner search currently needs tooprovide *aggregated* results.</span></span> <span data-ttu-id="b875c-386">Med andra ord, det måste innehålla en *mått* kommandot; du kan för närvarande feed rådata resultat i en yttre sökning.</span><span class="sxs-lookup"><span data-stu-id="b875c-386">In other words, it must contain a *measure* command; you cannot currently feed raw results into an outer search.</span></span>

<span data-ttu-id="b875c-387">Dessutom kan det finnas en enda IN-operatorn och den måste vara hello senaste filter i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="b875c-387">Also, there can be only one IN operator and it must be hello last filter in hello query.</span></span> <span data-ttu-id="b875c-388">Flera IN operatorer får inte vara eller skulle – det i praktiken innebär att köra flera subsearches: hello viktigt är endast en sub/inre sökningen är möjligt för varje yttre sökning.</span><span class="sxs-lookup"><span data-stu-id="b875c-388">Multiple IN operators cannot be OR’d – this essentially prevents running multiple subsearches: hello important point is that only one sub/inner search is possible for each outer search.</span></span>

<span data-ttu-id="b875c-389">Även om dessa begränsningar i gör många typer av korrelerade sökningar och att du toodefine liknande toogroups, till exempel datorer, användare eller filer – oavsett hello fält i dina data innehåller.</span><span class="sxs-lookup"><span data-stu-id="b875c-389">Even with these limits, IN enables many kinds of correlated searches, and allows you toodefine something similar toogroups such as computers, users, or files – whatever hello fields in your data contain.</span></span> <span data-ttu-id="b875c-390">Här följer fler exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-390">Here are more examples:</span></span>

<span data-ttu-id="b875c-391">**Alla uppdateringar som saknas från datorer där inställningen för automatiska uppdateringar är inaktiverad**</span><span class="sxs-lookup"><span data-stu-id="b875c-391">**All updates missing from computers where Automatic Update setting is disabled**</span></span>

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

<span data-ttu-id="b875c-392">**Alla fel-händelser från datorer som kör SQL Server (= där SQL-bedömning har körts)**</span><span class="sxs-lookup"><span data-stu-id="b875c-392">**All error events from computers running SQL Server (=where SQL Assessment has run)**</span></span>

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

<span data-ttu-id="b875c-393">**Alla säkerhetshändelser från datorer som är domänkontrollanter (= där AD-bedömning har körts)**</span><span class="sxs-lookup"><span data-stu-id="b875c-393">**All security events from computers that are Domain Controllers (=where AD Assessment has run)**</span></span>

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

<span data-ttu-id="b875c-394">**Som andra konton har loggat in toohello samma datorer där kontot BACONLAND\jochan har loggat in?**</span><span class="sxs-lookup"><span data-stu-id="b875c-394">**Which other accounts have logged on toohello same computers where account BACONLAND\jochan has logged on?**</span></span>

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-hello-distinct-command"></a><span data-ttu-id="b875c-395">Kommandot hello distinkta</span><span class="sxs-lookup"><span data-stu-id="b875c-395">Use hello distinct command</span></span>
<span data-ttu-id="b875c-396">Det här kommandot ger en lista över distinkta värden för ett fält som hello namnet.</span><span class="sxs-lookup"><span data-stu-id="b875c-396">As hello name suggests, this command provides a list of distinct values for a field.</span></span> <span data-ttu-id="b875c-397">Det är förstås enkla men ganska användbart.</span><span class="sxs-lookup"><span data-stu-id="b875c-397">It's surprisingly simple but quite useful.</span></span> <span data-ttu-id="b875c-398">hello samma kan uppnås med måttet count() kommandot samt, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="b875c-398">hello same could be achieved with measure count() command as well, as shown below.</span></span>

```
Type=Event | Measure count() by Computer
```

![DISTINKTA Sök exemplet](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

<span data-ttu-id="b875c-400">Men om du är intresserad av bara en lista över distinkta värden och inte hello antal dokument som har som värden, kan sedan DISTINCT ge tydligare och lättare tooread utdata och kortare syntax som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b875c-400">However, if all you're interested in is just a list of distinct values and not hello count of documents that have that values, then DISTINCT can provide cleaner and easier tooread output, and shorter syntax, as shown below.</span></span>

```
Type=Event | Distinct Computer
```
![DISTINKTA Sök exemplet](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-hello-countdistinct-function-with-hello-measure-command"></a><span data-ttu-id="b875c-402">Använd hello countdistinct funktionen med hello mått kommando</span><span class="sxs-lookup"><span data-stu-id="b875c-402">Use hello countdistinct function with hello measure command</span></span>
<span data-ttu-id="b875c-403">Hej countdistinct funktionen räknar hello antalet distinkta värden inom varje grupp.</span><span class="sxs-lookup"><span data-stu-id="b875c-403">hello countdistinct function counts hello number of distinct values within each group.</span></span> <span data-ttu-id="b875c-404">Det kan till exempel vara används toocount hello antal unika datorer som rapporterar för varje typ:</span><span class="sxs-lookup"><span data-stu-id="b875c-404">For example, it could be used toocount hello number of unique computers reporting for each Type:</span></span>

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-hello-measure-interval-command"></a><span data-ttu-id="b875c-406">Kommandot hello mått intervall</span><span class="sxs-lookup"><span data-stu-id="b875c-406">Use hello measure interval command</span></span>
<span data-ttu-id="b875c-407">Med nästan realtid prestandadatainsamling, kan du samla in och visualisera alla prestandaräknare i logganalys.</span><span class="sxs-lookup"><span data-stu-id="b875c-407">With near real-time performance data collection, you can collect and visualize any performance counter in Log Analytics.</span></span> <span data-ttu-id="b875c-408">Bara att ange hello frågan **typ: Perf** returnerar tusentals mått diagram baserat på hello antal räknare och servrar i logganalys-miljö.</span><span class="sxs-lookup"><span data-stu-id="b875c-408">Simply entering hello query **Type:Perf** will return thousands of metric graphs based on hello number of counters and servers in your Log Analytics environment.</span></span> <span data-ttu-id="b875c-409">Med på begäran mått aggregering, kan du titta på hello övergripande mått i din miljö vid en hög nivå och ingående till mer detaljerade data som du behöver.</span><span class="sxs-lookup"><span data-stu-id="b875c-409">With on-demand metric aggregation, you can look at hello overall metrics in your environment at a high level, and deep dive into more granular data as you need to.</span></span>

<span data-ttu-id="b875c-410">Anta att du vill att tooknow vad är hello Genomsnittlig CPU på alla datorer.</span><span class="sxs-lookup"><span data-stu-id="b875c-410">Let’s say that you want tooknow what is hello average CPU across all your computers.</span></span> <span data-ttu-id="b875c-411">Titta på hello Genomsnittlig CPU för varje dator kanske inte praktiskt eftersom resultat kan hämta jämnas ut. toolook till mer information du kan aggregera dina resultat i en mindre tid fönstret blocken och leta till en tidsserie på olika dimensioner.</span><span class="sxs-lookup"><span data-stu-id="b875c-411">Looking at hello average CPU for every computer might not be helpful because results may get smoothed out. toolook into more details, you can aggregate your result in a smaller time window chunks, and look into a time series across different dimensions.</span></span> <span data-ttu-id="b875c-412">T.ex, kan du utföra hello timvis medelvärdet av CPU-användning på alla datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b875c-412">For example, you can perform hello hourly average of CPU usage across all your computers as follows:</span></span>

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![måttet genomsnittlig intervall](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

<span data-ttu-id="b875c-414">Som standard visas resultaten i interaktiva linjediagram med flera serier.</span><span class="sxs-lookup"><span data-stu-id="b875c-414">By default these results will be displayed in a multi-series interactive line chart.</span></span>  <span data-ttu-id="b875c-415">Det här diagrammet stöder serien växla (med y-axeln rescaling), Zooma och placera markören.</span><span class="sxs-lookup"><span data-stu-id="b875c-415">This chart supports series toggling (with y-axis rescaling), zooming, and hovering.</span></span>  <span data-ttu-id="b875c-416">hello Visningsalternativ för tabellen finns kvar för att visa hello rådata om det behövs.</span><span class="sxs-lookup"><span data-stu-id="b875c-416">hello table display option is still available for viewing hello raw data if necessary.</span></span>

<span data-ttu-id="b875c-417">Du kan också gruppera efter andra fält.</span><span class="sxs-lookup"><span data-stu-id="b875c-417">You can also group by other fields.</span></span> <span data-ttu-id="b875c-418">Jag tittar på alla hello % räknare för en specifik dator i det här exemplet, och jag vill tooknow vad är hello varje timme 70 percentiler för varje räknare:</span><span class="sxs-lookup"><span data-stu-id="b875c-418">In this example, I am looking at all hello % counters for one specific computer, and I want tooknow what is hello hourly 70 percentiles of every counter:</span></span>

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
<span data-ttu-id="b875c-419">En sak toonote är att frågorna inte är begränsad tooperformance räknare.</span><span class="sxs-lookup"><span data-stu-id="b875c-419">One thing toonote is that these queries are not limited tooperformance counters.</span></span> <span data-ttu-id="b875c-420">Du kan använda dem tooany mått.</span><span class="sxs-lookup"><span data-stu-id="b875c-420">You can apply them tooany metric.</span></span> <span data-ttu-id="b875c-421">I det här exemplet tittar jag på W3C IIS-loggar.</span><span class="sxs-lookup"><span data-stu-id="b875c-421">In this example, I’m looking at W3C IIS logs.</span></span> <span data-ttu-id="b875c-422">Jag vill tooknow vad är hello maximal tid tar det över 5-minutersintervall för bearbetning av varje förfrågan:</span><span class="sxs-lookup"><span data-stu-id="b875c-422">I want tooknow what is hello maximum time it takes over a 5-minute interval for processing each request:</span></span>

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a><span data-ttu-id="b875c-423">Använda flera aggregeringar i en fråga</span><span class="sxs-lookup"><span data-stu-id="b875c-423">Use multiple aggregates in one query</span></span>
<span data-ttu-id="b875c-424">Du kan ange flera sammanställd satser i kommandot mått.</span><span class="sxs-lookup"><span data-stu-id="b875c-424">You can specify multiple aggregate clauses in a measure command.</span></span>  <span data-ttu-id="b875c-425">Alias kan vara oberoende av var och en.</span><span class="sxs-lookup"><span data-stu-id="b875c-425">Each one can be aliased independently.</span></span>  <span data-ttu-id="b875c-426">Om inte anges som ett alias hello fältnamn blir hello mängdfunktion som användes (d.v.s. ”avg(CounterValue)” för avg(CounterValue)).</span><span class="sxs-lookup"><span data-stu-id="b875c-426">If it is not given an alias hello resulting field name will be hello aggregate function that was used (i.e. "avg(CounterValue)" for avg(CounterValue)).</span></span>

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

<span data-ttu-id="b875c-428">Här är ett annat exempel:</span><span class="sxs-lookup"><span data-stu-id="b875c-428">Here is another example:</span></span>

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a><span data-ttu-id="b875c-429">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b875c-429">Next steps</span></span>
<span data-ttu-id="b875c-430">Mer information om loggen sökningar finns:</span><span class="sxs-lookup"><span data-stu-id="b875c-430">For additional information about log searches, see:</span></span>

* <span data-ttu-id="b875c-431">Använd [anpassade fält i logganalys](log-analytics-custom-fields.md) tooextend loggen sökningar.</span><span class="sxs-lookup"><span data-stu-id="b875c-431">Use [Custom fields in Log Analytics](log-analytics-custom-fields.md) tooextend log searches.</span></span>
* <span data-ttu-id="b875c-432">Granska hello [logganalys logga Sök referens](log-analytics-search-reference.md) tooview alla hello söka fält och tillgängliga i logganalys-facets.</span><span class="sxs-lookup"><span data-stu-id="b875c-432">Review hello [Log Analytics log search reference](log-analytics-search-reference.md) tooview all of hello search fields and facets available in Log Analytics.</span></span>
