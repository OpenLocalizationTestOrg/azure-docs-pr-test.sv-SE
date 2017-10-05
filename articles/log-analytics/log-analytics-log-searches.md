---
title: "Söka efter data med loggen sökningar i Azure Log Analytics | Microsoft Docs"
description: "Loggsökningar låter dig kombinera och korrelera datordata från flera källor i din miljö."
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
ms.openlocfilehash: bf237a837297cb8f1ab3a3340139133adcd2b244
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="find-data-using-log-searches-in-log-analytics"></a><span data-ttu-id="880cd-103">Hitta data med hjälp av loggen sökningar i logganalys</span><span class="sxs-lookup"><span data-stu-id="880cd-103">Find data using log searches in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="880cd-104">Den här artikeln beskriver loggen sökningar med aktuella frågespråket i logganalys.</span><span class="sxs-lookup"><span data-stu-id="880cd-104">This article describes log searches using the current query language in Log Analytics.</span></span>  <span data-ttu-id="880cd-105">Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), läser du bör [förstå loggen söker i logganalys (nya)](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="880cd-105">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should refer to [Understanding log searches in Log Analytics (new)](log-analytics-log-search-new.md).</span></span>


<span data-ttu-id="880cd-106">Kärnan i logganalys är loggen sökfunktionen där du kan kombinera och korrelera datorn data från flera källor i din miljö.</span><span class="sxs-lookup"><span data-stu-id="880cd-106">At the core of Log Analytics is the log search feature which allows you to combine and correlate any machine data from multiple sources within your environment.</span></span> <span data-ttu-id="880cd-107">Lösningar är också drivs av logg-sökning för att sätta mått pivoteras runt ett specifikt problem.</span><span class="sxs-lookup"><span data-stu-id="880cd-107">Solutions are also powered by log search to bring you metrics pivoted around a particular problem area.</span></span>

<span data-ttu-id="880cd-108">Du kan skapa en fråga på sidan Sök efter och sedan när du söker kan du filtrera resultaten genom att använda aspekten kontroller.</span><span class="sxs-lookup"><span data-stu-id="880cd-108">On the Search page, you can create a query, and then when you search, you can filter the results by using facet controls.</span></span> <span data-ttu-id="880cd-109">Du kan också skapa avancerade frågor för att transformera, filter och rapporten på dina resultat.</span><span class="sxs-lookup"><span data-stu-id="880cd-109">You can also create advanced queries to transform, filter, and report on your results.</span></span>

<span data-ttu-id="880cd-110">Vanliga loggen sökfrågor visas på de flesta sidor för lösningen.</span><span class="sxs-lookup"><span data-stu-id="880cd-110">Common log search queries appear on most solution pages.</span></span> <span data-ttu-id="880cd-111">I OMS-konsolen kan du klicka på paneler eller detaljer till andra objekt att visa detaljer om objektet genom att söka i loggen.</span><span class="sxs-lookup"><span data-stu-id="880cd-111">Throughout the OMS console, you can click tiles or drill in to other items to view details about the item by using log search.</span></span>

<span data-ttu-id="880cd-112">I den här självstudiekursen kommer går vi igenom exemplen innehåller alla grundläggande information när du använder loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="880cd-112">In this tutorial, we'll walk through examples to cover all the basics when you use log search.</span></span>

<span data-ttu-id="880cd-113">Vi börjar med enkla, praktiska exempel och bygga på dem så att du kan få en förståelse av praktiska användningsområden om hur du använder syntaxen för att extrahera de insikter som du vill använda från data.</span><span class="sxs-lookup"><span data-stu-id="880cd-113">We'll start with simple, practical examples and then build on them so that you can get an understanding of practical use cases about how to use the syntax to extract the insights you want from the data.</span></span>

<span data-ttu-id="880cd-114">När du är bekant med sökmetoder kan du läsa den [logganalys logga Sök referens](log-analytics-search-reference.md).</span><span class="sxs-lookup"><span data-stu-id="880cd-114">After you've familiar with search techniques, you can review the [Log Analytics log search reference](log-analytics-search-reference.md).</span></span>

## <a name="use-basic-filters"></a><span data-ttu-id="880cd-115">Grundläggande filter</span><span class="sxs-lookup"><span data-stu-id="880cd-115">Use basic filters</span></span>
<span data-ttu-id="880cd-116">Det första du behöver veta är att den första delen av en sökning fråga innan någon ”|” lodräta vertikalstrecket är alltid en *filter*.</span><span class="sxs-lookup"><span data-stu-id="880cd-116">The first thing to know is that the first part of a search query, before any "|" vertical pipe character, is always a *filter*.</span></span> <span data-ttu-id="880cd-117">Du kan se den som en WHERE-sats i TSQL--den avgör *vad* delmängd av data och hämtar utanför OMS-datalagret.</span><span class="sxs-lookup"><span data-stu-id="880cd-117">You can think of it as a WHERE clause in TSQL--it determines *what* subset of data to pull out of the OMS data store.</span></span> <span data-ttu-id="880cd-118">Söka i datalagret är i stort sett information om hur du anger egenskaperna för de data som du vill extrahera, så är det naturligt att en fråga börjar med WHERE-satsen.</span><span class="sxs-lookup"><span data-stu-id="880cd-118">Searching in the data store is largely about specifying the characteristics of the data that you want to extract, so it is natural that a query would start with the WHERE clause.</span></span>

<span data-ttu-id="880cd-119">De mest grundläggande filter som du kan använda *nyckelord*, till exempel 'fel' eller 'timeout- eller ett datornamn.</span><span class="sxs-lookup"><span data-stu-id="880cd-119">The most basic filters you can use are *keywords*, such as ‘error’ or ‘timeout’, or a computer name.</span></span> <span data-ttu-id="880cd-120">Dessa typer av enkla frågor returnerar vanligtvis olika former av data i samma resultatmängden.</span><span class="sxs-lookup"><span data-stu-id="880cd-120">These types of simple queries generally return diverse shapes of data within the same result set.</span></span> <span data-ttu-id="880cd-121">Detta beror på att Log Analytics har olika *typer* av data i systemet.</span><span class="sxs-lookup"><span data-stu-id="880cd-121">This is because Log Analytics has different *types* of data in the system.</span></span>

### <a name="to-conduct-a-simple-search"></a><span data-ttu-id="880cd-122">Att genomföra en enkel sökning</span><span class="sxs-lookup"><span data-stu-id="880cd-122">To conduct a simple search</span></span>
1. <span data-ttu-id="880cd-123">I OMS-portalen klickar du på **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="880cd-123">In the OMS portal, click **Log Search**.</span></span>  
    <span data-ttu-id="880cd-124">![Sök sida vid sida](./media/log-analytics-log-searches/oms-overview-log-search.png)</span><span class="sxs-lookup"><span data-stu-id="880cd-124">![search tile](./media/log-analytics-log-searches/oms-overview-log-search.png)</span></span>
2. <span data-ttu-id="880cd-125">Ange i fältet fråga `error` och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="880cd-125">In the query field, type `error` and then click **Search**.</span></span>  
    <span data-ttu-id="880cd-126">![sökningsfel](./media/log-analytics-log-searches/oms-search-error.png)</span><span class="sxs-lookup"><span data-stu-id="880cd-126">![search error](./media/log-analytics-log-searches/oms-search-error.png)</span></span>  
    <span data-ttu-id="880cd-127">Till exempel fråga om `error` i följande bild returnerade 100 000 **händelse** poster (som samlas in av loggen Management), 18 **ConfigurationAlert** poster (som genereras av Configuration Assessment) och 12 **ConfigurationChange** poster (avbildas av spårning av ändringar).</span><span class="sxs-lookup"><span data-stu-id="880cd-127">For example, the query for `error` in the following image returned 100,000 **Event** records (collected by Log Management), 18 **ConfigurationAlert** records (generated by Configuration Assessment) and 12 **ConfigurationChange** records (captured by the Change Tracking).</span></span>   
    <span data-ttu-id="880cd-128">![sökresultat](./media/log-analytics-log-searches/oms-search-results01.png)</span><span class="sxs-lookup"><span data-stu-id="880cd-128">![search results](./media/log-analytics-log-searches/oms-search-results01.png)</span></span>  

<span data-ttu-id="880cd-129">Dessa filter är inte riktigt typer/objektklasser.</span><span class="sxs-lookup"><span data-stu-id="880cd-129">These filters are not really object types/classes.</span></span> <span data-ttu-id="880cd-130">*Typen* är bara en tagg, eller en egenskap eller en sträng/namn/kategori, som är kopplad till en typ av data.</span><span class="sxs-lookup"><span data-stu-id="880cd-130">*Type* is just a tag, or a property, or a string/name/category, that is attached to a piece of data.</span></span> <span data-ttu-id="880cd-131">Vissa dokument i systemet märks **typ: ConfigurationAlert** och vissa märks **typ: Perf**, eller **typ: händelsen**och så vidare.</span><span class="sxs-lookup"><span data-stu-id="880cd-131">Some documents in the system are tagged as **Type:ConfigurationAlert** and some are tagged as **Type:Perf**, or **Type:Event**, and so on.</span></span> <span data-ttu-id="880cd-132">Varje sökresultat, dokument, post eller post visar rådata egenskaperna och deras värden för var och en av de olika delarna av data och du kan använda dessa fältnamn ange i filtret om du vill bara poster där fältet har det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="880cd-132">Each search result, document, record, or entry displays all the raw properties and their values for each of those pieces of data, and you can use those field names to specify in the filter when you want to retrieve only the records where the field has that given value.</span></span>

<span data-ttu-id="880cd-133">*Typen* är egentligen bara ett fält som alla poster har det inte skiljer sig från ett annat fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-133">*Type* is really just a field that all records have, it is not different from any other field.</span></span> <span data-ttu-id="880cd-134">Det har upprättats baserat på värdet för det typ av fältet.</span><span class="sxs-lookup"><span data-stu-id="880cd-134">This was established based on the value of the Type field.</span></span> <span data-ttu-id="880cd-135">Posten har ett annat formen eller formulär.</span><span class="sxs-lookup"><span data-stu-id="880cd-135">That record will have a different shape or form.</span></span> <span data-ttu-id="880cd-136">Tillfälligtvis, **typ = Perf**, eller **typ = händelse** är också den syntax som du behöver för att lära sig att fråga efter prestandadata och händelser.</span><span class="sxs-lookup"><span data-stu-id="880cd-136">Incidentally, **Type=Perf**, or **Type=Event** is also the syntax that you need to learn to query for performance data or events.</span></span>

<span data-ttu-id="880cd-137">Du kan använda ett kolon (:) eller ett likhetstecken (=) efter fältnamnet och före värdet.</span><span class="sxs-lookup"><span data-stu-id="880cd-137">You can use either a colon (:) or an equal sign (=) after the field name and before the value.</span></span> <span data-ttu-id="880cd-138">**Typ: händelsen** och **typ = händelse** har samma betydelse, du kan välja det format du föredrar.</span><span class="sxs-lookup"><span data-stu-id="880cd-138">**Type:Event** and **Type=Event** are equivalent in meaning, you can choose the style you prefer.</span></span>

<span data-ttu-id="880cd-139">I så fall om typen = Perf poster har ett fält med namnet 'CounterName, och du kan skriva en fråga som liknar `Type=Perf CounterName="% Processor Time"`.</span><span class="sxs-lookup"><span data-stu-id="880cd-139">So, if the Type=Perf records have a field called 'CounterName', then you can write a query resembling `Type=Perf CounterName="% Processor Time"`.</span></span>

<span data-ttu-id="880cd-140">Detta ger dig endast prestandadata där prestandaräknarnamnet är ”% processortid”.</span><span class="sxs-lookup"><span data-stu-id="880cd-140">This will give you only the performance data where the performance counter name is "% Processor Time".</span></span>

### <a name="to-search-for-processor-time-performance-data"></a><span data-ttu-id="880cd-141">Sök efter processor tid prestandadata</span><span class="sxs-lookup"><span data-stu-id="880cd-141">To search for processor time performance data</span></span>
* <span data-ttu-id="880cd-142">Skriv i fältet Sök fråga`Type=Perf CounterName="% Processor Time"`</span><span class="sxs-lookup"><span data-stu-id="880cd-142">In the search query field, type `Type=Perf CounterName="% Processor Time"`</span></span>

<span data-ttu-id="880cd-143">Du kan också vara mer specifik och använda **InstanceName = _ ”totalt'** i frågan, vilket är en Windows-prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="880cd-143">You can also be more specific and use **InstanceName=_'Total'** in the query, which is a Windows performance counter.</span></span> <span data-ttu-id="880cd-144">Du kan också välja en begränsningsaspekt och en annan **fältvärdet:**.</span><span class="sxs-lookup"><span data-stu-id="880cd-144">You can also select a facet and another **field:value**.</span></span> <span data-ttu-id="880cd-145">Filtret läggs automatiskt till ditt filter i frågan-fältet.</span><span class="sxs-lookup"><span data-stu-id="880cd-145">The filter is automatically added to your filter in the query bar.</span></span> <span data-ttu-id="880cd-146">Du kan se dessa i följande bild.</span><span class="sxs-lookup"><span data-stu-id="880cd-146">You can see this in the following image.</span></span> <span data-ttu-id="880cd-147">Den visar var du ska klicka för att lägga till **instansnamn: ”_Total”** i frågan utan att ange något.</span><span class="sxs-lookup"><span data-stu-id="880cd-147">It shows you where to click to add **InstanceName:’_Total’** to the query without typing anything.</span></span>

![Sök aspekten](./media/log-analytics-log-searches/oms-search-facet.png)

<span data-ttu-id="880cd-149">Frågan blir nu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span><span class="sxs-lookup"><span data-stu-id="880cd-149">Your query now becomes `Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span></span>

<span data-ttu-id="880cd-150">I det här exemplet du inte behöver ange **typ = Perf** till resultatet för den här.</span><span class="sxs-lookup"><span data-stu-id="880cd-150">In this example, you don't have to specify **Type=Perf** to get to this result.</span></span> <span data-ttu-id="880cd-151">Eftersom det finns endast fälten CounterName och instansnamn för poster av typen = Perf, frågan är det specifikt nog för att returnera samma resultat som längre, tidigare:</span><span class="sxs-lookup"><span data-stu-id="880cd-151">Because the fields CounterName and InstanceName only exist for records of Type=Perf, the query is specific enough to return the same results as the longer, previous one:</span></span>

```
CounterName="% Processor Time" InstanceName="_Total"
```

<span data-ttu-id="880cd-152">Detta beror på att alla filter i frågan utvärderas som *och* med varandra.</span><span class="sxs-lookup"><span data-stu-id="880cd-152">This is because all the filters in the query are evaluated as being in *AND* with each other.</span></span> <span data-ttu-id="880cd-153">Effektivt, fler fält du lägga till kriterierna du får mindre, mer specifikt och förfinad resultat.</span><span class="sxs-lookup"><span data-stu-id="880cd-153">Effectively, the more fields you add to the criteria, you get less, more specific and refined results.</span></span>

<span data-ttu-id="880cd-154">Till exempel fråga `Type=Event EventLog="Windows PowerShell"` är identisk med `Type=Event AND EventLog="Windows PowerShell"`.</span><span class="sxs-lookup"><span data-stu-id="880cd-154">For example, the query `Type=Event EventLog="Windows PowerShell"` is identical to `Type=Event AND EventLog="Windows PowerShell"`.</span></span> <span data-ttu-id="880cd-155">Den returnerar alla händelser som har loggat in och samlas in från händelseloggen i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="880cd-155">It returns all events that were logged in and collected from the Windows PowerShell event log.</span></span> <span data-ttu-id="880cd-156">Om du lägger till ett filter flera gånger genom att välja samma aspekten flera gånger och sedan problemet är rent kosmetiskt--kanske det tar upp plats sökfältet, men samma resultat returneras fortfarande eftersom operatorn implicit och finns alltid.</span><span class="sxs-lookup"><span data-stu-id="880cd-156">If you add a filter multiple times by repeatedly selecting the same facet, then the issue is purely cosmetic--it might clutter the Search bar, but it still returns the same results because the implicit AND operator is always there.</span></span>

<span data-ttu-id="880cd-157">Du kan enkelt ångra operatorn implicit och genom att använda en inte explicit.</span><span class="sxs-lookup"><span data-stu-id="880cd-157">You can easily reverse the implicit AND operator by using a NOT operator explicitly.</span></span> <span data-ttu-id="880cd-158">Exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-158">For example:</span></span>

<span data-ttu-id="880cd-159">`Type:Event NOT(EventLog:"Windows PowerShell")`eller motsvarande `Type=Event EventLog!="Windows PowerShell"` returnera alla händelser från andra loggar som inte är Windows PowerShell-loggen.</span><span class="sxs-lookup"><span data-stu-id="880cd-159">`Type:Event NOT(EventLog:"Windows PowerShell")` or its equivalent `Type=Event EventLog!="Windows PowerShell"` return all events from all other logs that are NOT the Windows PowerShell log.</span></span>

<span data-ttu-id="880cd-160">Du kan också använda andra booleska operatorn som 'Eller'.</span><span class="sxs-lookup"><span data-stu-id="880cd-160">Or, you can use other Boolean operator such as ‘OR’.</span></span> <span data-ttu-id="880cd-161">Följande fråga returnerar poster där händelseloggen är antingen program eller System.</span><span class="sxs-lookup"><span data-stu-id="880cd-161">The following query returns records for which the EventLog is either Application OR System.</span></span>

```
EventLog=Application OR EventLog=System
```

<span data-ttu-id="880cd-162">Med hjälp av frågan ovan, får du poster för båda loggar i samma resultatet.</span><span class="sxs-lookup"><span data-stu-id="880cd-162">Using the above query, you’ll get entries for both logs in the same result set.</span></span>

<span data-ttu-id="880cd-163">Men om du tar bort eller genom att låta den implicita och i drift genererar sedan följande fråga inte något resultat eftersom det inte finns en post i händelseloggen som hör till båda loggarna.</span><span class="sxs-lookup"><span data-stu-id="880cd-163">However, if you remove the OR by leaving the implicit AND in place, then the following query will not produce any results because there isn’t an event log entry that belongs to BOTH logs.</span></span> <span data-ttu-id="880cd-164">Varje post i händelseloggen skrevs till endast en av två loggar.</span><span class="sxs-lookup"><span data-stu-id="880cd-164">Each event log entry was written to only one of the two logs.</span></span>

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a><span data-ttu-id="880cd-165">Använda ytterligare filter</span><span class="sxs-lookup"><span data-stu-id="880cd-165">Use additional filters</span></span>
<span data-ttu-id="880cd-166">Följande fråga returnerar poster för 2 händelseloggar för alla datorer som har skickat data.</span><span class="sxs-lookup"><span data-stu-id="880cd-166">The following query returns entries for 2 event logs for all the computers that have sent data.</span></span>

```
EventLog=Application OR EventLog=System
```

![sökresultat](./media/log-analytics-log-searches/oms-search-results03.png)

<span data-ttu-id="880cd-168">Att välja ett fält eller filter ska begränsa frågan till en viss dator, förutom alla de.</span><span class="sxs-lookup"><span data-stu-id="880cd-168">Selecting one of the fields or filters will narrow the query to a specific computer, excluding all other ones.</span></span> <span data-ttu-id="880cd-169">Den resulterande frågan liknar följande.</span><span class="sxs-lookup"><span data-stu-id="880cd-169">The resulting query would resemble the following.</span></span>

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

<span data-ttu-id="880cd-170">Vilket motsvarar följande, på grund av implicit och.</span><span class="sxs-lookup"><span data-stu-id="880cd-170">Which is equivalent to the following, because of the implicit AND.</span></span>

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="880cd-171">Varje fråga utvärderas i följande ordning explicit.</span><span class="sxs-lookup"><span data-stu-id="880cd-171">Each query is evaluated in the following explicit order.</span></span> <span data-ttu-id="880cd-172">Observera parentesen.</span><span class="sxs-lookup"><span data-stu-id="880cd-172">Note the parenthesis.</span></span>

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="880cd-173">Precis som fältet händelselogg kan du hämta data endast för en uppsättning specifika datorer genom att lägga till eller.</span><span class="sxs-lookup"><span data-stu-id="880cd-173">Just like the event log field, you can retrieve data only for a set of specific computers by adding OR.</span></span> <span data-ttu-id="880cd-174">Exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-174">For example:</span></span>

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

<span data-ttu-id="880cd-175">På liknande sätt kan detta följande fråga returnerar **% processortid** för de valda två endast datorerna.</span><span class="sxs-lookup"><span data-stu-id="880cd-175">Similarly, this the following query return **% CPU Time** for the selected two computers only.</span></span>

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a><span data-ttu-id="880cd-176">Fälttyper</span><span class="sxs-lookup"><span data-stu-id="880cd-176">Field types</span></span>
<span data-ttu-id="880cd-177">När du skapar filter, bör du förstå skillnader i att arbeta med olika typer av fält som returneras av loggen sökningar.</span><span class="sxs-lookup"><span data-stu-id="880cd-177">When creating filters, you should understand the differences in working with different types of fields returned by log searches.</span></span>

<span data-ttu-id="880cd-178">**Sökbara fält** visas i blått i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="880cd-178">**Searchable fields** show in blue in search results.</span></span>  <span data-ttu-id="880cd-179">Du kan använda sökbara fält i sökvillkor specifika fält, till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="880cd-179">You can use searchable fields in search conditions specific to the field such as the following:</span></span>

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

<span data-ttu-id="880cd-180">**Kostnadsfri text sökbara fält** visas i grått i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="880cd-180">**Free text searchable fields** are shown in grey in search results.</span></span>  <span data-ttu-id="880cd-181">De kan inte användas med sökvillkor som är specifika för fält som sökbara fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-181">They cannot be used with search conditions specific to the field like searchable fields.</span></span>  <span data-ttu-id="880cd-182">De genomsöks endast när du utför en fråga i alla fält till exempel följande.</span><span class="sxs-lookup"><span data-stu-id="880cd-182">They are only searched when performing a query across all fields such as the following.</span></span>

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a><span data-ttu-id="880cd-183">Booleska operatorer</span><span class="sxs-lookup"><span data-stu-id="880cd-183">Boolean operators</span></span>
<span data-ttu-id="880cd-184">Med datetime och numeriska fält, kan du söka efter värden med *större än*, *mindre än*, och *mindre än eller lika med*.</span><span class="sxs-lookup"><span data-stu-id="880cd-184">With datetime and numeric fields, you can search for values using *greater than*, *lesser than*, and *lesser than or equal*.</span></span> <span data-ttu-id="880cd-185">Du kan använda enkla operatorer som >, <>, =, < =,! = i sökfältet frågan.</span><span class="sxs-lookup"><span data-stu-id="880cd-185">You can use simple operators such as >, < , >=, <= , != in the query search bar.</span></span>

<span data-ttu-id="880cd-186">Du kan fråga en specifik händelselogg för en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="880cd-186">You can query a specific event log for a specific period of time.</span></span> <span data-ttu-id="880cd-187">Till exempel uttrycks de senaste 24 timmarna med följande mnemoteknisk uttryck.</span><span class="sxs-lookup"><span data-stu-id="880cd-187">For example, the last 24 hours is expressed with the following mnemonic expression.</span></span>

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a><span data-ttu-id="880cd-188">Att sökningen med booleska operatorer</span><span class="sxs-lookup"><span data-stu-id="880cd-188">To search using a boolean operator</span></span>
* <span data-ttu-id="880cd-189">Skriv i fältet Sök fråga`EventLog=System TimeGenerated>NOW-24HOURS`</span><span class="sxs-lookup"><span data-stu-id="880cd-189">In the search query field, type `EventLog=System TimeGenerated>NOW-24HOURS`</span></span>  
    <span data-ttu-id="880cd-190">![söka med booleskt värde](./media/log-analytics-log-searches/oms-search-boolean.png)</span><span class="sxs-lookup"><span data-stu-id="880cd-190">![search with boolean](./media/log-analytics-log-searches/oms-search-boolean.png)</span></span>

<span data-ttu-id="880cd-191">Även om du kan styra tidsintervallet grafiskt och de flesta gånger kanske du vill göra det, har fördelar inklusive ett tidsfilter direkt i frågan.</span><span class="sxs-lookup"><span data-stu-id="880cd-191">Although you can control the time interval graphically, and most times you might want to do that, there are advantages to including a time filter directly into the query.</span></span> <span data-ttu-id="880cd-192">T.ex, detta passar utmärkt instrumentpaneler där du kan åsidosätta tid för varje bricka oberoende av den *globala* tid val på instrumentpanelssidan.</span><span class="sxs-lookup"><span data-stu-id="880cd-192">For example, this works great with dashboards where you can override the time for each tile, regardless of the *global* time selector on the dashboard page.</span></span> <span data-ttu-id="880cd-193">Mer information finns i [tid frågor i instrumentpanelen](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span><span class="sxs-lookup"><span data-stu-id="880cd-193">For more information, see [Time Matters in Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span></span>

<span data-ttu-id="880cd-194">När du filtrerar efter tid, Tänk på att du får resultat för de *skärningspunkten* av två tidsperioder: den som anges i OMS-portalen (S1) och den som angetts i frågan (S2).</span><span class="sxs-lookup"><span data-stu-id="880cd-194">When filtering by time, keep in mind that you get results for the *intersection* of the two time periods: the one specified in the OMS portal (S1) and the one specified in the query (S2).</span></span>

![skärningspunkten](./media/log-analytics-log-searches/oms-search-intersection.png)

<span data-ttu-id="880cd-196">Detta innebär att, om tidsperioderna inte överlappa, till exempel i OMS-portalen där du väljer **den här veckan** och i frågan där du definierar **förra veckan**, det finns inga skärningspunkten och visas inte några resultat.</span><span class="sxs-lookup"><span data-stu-id="880cd-196">This means, if the time periods don’t intersect, for example in the OMS portal where you choose **This week** and in the query where you define **last week**, then there is no intersection and you won't receive any results.</span></span>

<span data-ttu-id="880cd-197">Jämförelseoperatorer som används för fältet TimeGenerated är också användbara i andra situationer.</span><span class="sxs-lookup"><span data-stu-id="880cd-197">Comparison operators used for the TimeGenerated field are also useful in other situations.</span></span> <span data-ttu-id="880cd-198">Till exempel med numeriska fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-198">For example, with numeric fields.</span></span>

<span data-ttu-id="880cd-199">Till exempel anges att konfigurationen Assessment aviseringar har följande allvarlighetsgrader:</span><span class="sxs-lookup"><span data-stu-id="880cd-199">For example, given that Configuration Assessment’s alerts have the following severity values:</span></span>

* <span data-ttu-id="880cd-200">0 = information</span><span class="sxs-lookup"><span data-stu-id="880cd-200">0 = Information</span></span>
* <span data-ttu-id="880cd-201">1 = varning</span><span class="sxs-lookup"><span data-stu-id="880cd-201">1 = Warning</span></span>
* <span data-ttu-id="880cd-202">2 = kritisk</span><span class="sxs-lookup"><span data-stu-id="880cd-202">2 = Critical</span></span>

<span data-ttu-id="880cd-203">Du kan fråga efter både varningar och kritiska aviseringar och utesluta information de med följande fråga:</span><span class="sxs-lookup"><span data-stu-id="880cd-203">You can query for both warning and critical alerts and also exclude informational ones with the following query:</span></span>

```
Type=ConfigurationAlert  Severity>=1
```


<span data-ttu-id="880cd-204">Du kan också använda intervallet frågor.</span><span class="sxs-lookup"><span data-stu-id="880cd-204">You can also use range queries.</span></span> <span data-ttu-id="880cd-205">Det innebär att du kan ange början och slutet intervallet för värden i en sekvens.</span><span class="sxs-lookup"><span data-stu-id="880cd-205">This means that you can provide the beginning and end range of values in a sequence.</span></span> <span data-ttu-id="880cd-206">Till exempel om du vill ha händelser från Operations Manager-händelseloggen där den händelse-ID är större än eller lika med 2100 men inte mer än 2199 returnerar sedan följande fråga dem.</span><span class="sxs-lookup"><span data-stu-id="880cd-206">For example, if you want events from the Operations Manager event log where the EventID is greater than or equal to 2100 but not greater than 2199, then the following query would return them.</span></span>

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> <span data-ttu-id="880cd-207">Intervallet syntaxen måste du använda är kolon (:) fältvärdet: avgränsare och *inte* likhetstecken (=).</span><span class="sxs-lookup"><span data-stu-id="880cd-207">The range syntax you must use is the colon (:) field:value separator and *not* the equal sign (=).</span></span> <span data-ttu-id="880cd-208">Lägre och övre delen av intervallet omges av hakparenteser och avgränsa dem med två punkter (.).</span><span class="sxs-lookup"><span data-stu-id="880cd-208">Enclose the lower and upper end of the range in square brackets and separate them with two periods (..).</span></span>
>
>

## <a name="manipulate-search-results"></a><span data-ttu-id="880cd-209">Hantera sökresultat</span><span class="sxs-lookup"><span data-stu-id="880cd-209">Manipulate search results</span></span>
<span data-ttu-id="880cd-210">När du söker efter data vill du förfina sökningen och har en bra kontroll över resultatet.</span><span class="sxs-lookup"><span data-stu-id="880cd-210">When you're searching for data, you'll want to refine your search query and have a good level of control over the results.</span></span> <span data-ttu-id="880cd-211">Du kan använda kommandon för att omvandla dem när resultaten hämtas.</span><span class="sxs-lookup"><span data-stu-id="880cd-211">When results are retrieved, you can apply commands to transform them.</span></span>

<span data-ttu-id="880cd-212">Kommandon i logganalys sökningar *måste* följer efter det lodräta vertikalstrecket (|).</span><span class="sxs-lookup"><span data-stu-id="880cd-212">Commands in Log Analytics searches *must* follow after the vertical pipe character (|).</span></span> <span data-ttu-id="880cd-213">Ett filter måste alltid vara den första delen av en frågesträng.</span><span class="sxs-lookup"><span data-stu-id="880cd-213">A filter must always be the first part of a query string.</span></span> <span data-ttu-id="880cd-214">Den definierar vilka data du arbetar med och sedan ”kommer” resultaten i ett kommando.</span><span class="sxs-lookup"><span data-stu-id="880cd-214">It defines the data set you're working with and then "pipes" those results into a command.</span></span> <span data-ttu-id="880cd-215">Du kan sedan använda pipe för att lägga till ytterligare kommandon.</span><span class="sxs-lookup"><span data-stu-id="880cd-215">You can then use the pipe to add additional commands.</span></span> <span data-ttu-id="880cd-216">Detta liknar löst Windows PowerShell-pipeline.</span><span class="sxs-lookup"><span data-stu-id="880cd-216">This is loosely similar to the Windows PowerShell pipeline.</span></span>

<span data-ttu-id="880cd-217">I allmänhet försöker logganalys sökspråk följer PowerShell format och riktlinjer för att göra det liknar IT-proffs och underlättar inlärningskurvan.</span><span class="sxs-lookup"><span data-stu-id="880cd-217">In general, the Log Analytics search language tries to follow PowerShell style and guidelines to make it similar to the IT pros, and to ease the learning curve.</span></span>

<span data-ttu-id="880cd-218">Kommandon har namn med verb, så att du enkelt kan se vad de gör.</span><span class="sxs-lookup"><span data-stu-id="880cd-218">Commands have names of verbs so you can easily tell what they do.</span></span>  

### <a name="sort"></a><span data-ttu-id="880cd-219">Sortera</span><span class="sxs-lookup"><span data-stu-id="880cd-219">Sort</span></span>
<span data-ttu-id="880cd-220">Sortera kan du definiera sorteringsordningen genom att ett eller flera fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-220">The sort command allows you to define the sorting order by one or multiple fields.</span></span> <span data-ttu-id="880cd-221">Även om du inte använder den, som standard, tillämpas taget fallande ordning.</span><span class="sxs-lookup"><span data-stu-id="880cd-221">Even if you don’t use it, by default, a time descending order is enforced.</span></span> <span data-ttu-id="880cd-222">Senaste resultat är alltid högst upp i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="880cd-222">The most recent results are always at the top of search results.</span></span> <span data-ttu-id="880cd-223">Detta innebär att när du kör en sökning med `Type=Event EventID=1234` vad verkligen körs du är:</span><span class="sxs-lookup"><span data-stu-id="880cd-223">This means that when you run a search, with `Type=Event EventID=1234` what really is executed for you is:</span></span>

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

<span data-ttu-id="880cd-224">Det beror på att det är typen av du är bekant med i loggarna.</span><span class="sxs-lookup"><span data-stu-id="880cd-224">That's because it is the type of experience you are familiar with in logs.</span></span> <span data-ttu-id="880cd-225">Till exempel i Loggboken i Windows.</span><span class="sxs-lookup"><span data-stu-id="880cd-225">For example, in the Windows Event Viewer.</span></span>

<span data-ttu-id="880cd-226">Du kan använda sortera så att resultaten returneras.</span><span class="sxs-lookup"><span data-stu-id="880cd-226">You can use Sort to change the way results are returned.</span></span> <span data-ttu-id="880cd-227">I följande exempel visas hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="880cd-227">The following examples show how this works.</span></span>

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


<span data-ttu-id="880cd-228">Enkel exemplen ovan visar hur kommandon fungerar--de ändra formen av de resultat som returneras av filtret.</span><span class="sxs-lookup"><span data-stu-id="880cd-228">The simple examples above show you how commands work--they change the shape of the results that the filter returned.</span></span>

### <a name="limit-and-top"></a><span data-ttu-id="880cd-229">Gränsen och upp</span><span class="sxs-lookup"><span data-stu-id="880cd-229">Limit and top</span></span>
<span data-ttu-id="880cd-230">Ett annat mindre kända kommando är GRÄNSEN.</span><span class="sxs-lookup"><span data-stu-id="880cd-230">Another less known command is LIMIT.</span></span> <span data-ttu-id="880cd-231">Gränsen är ett PowerShell-liknande verb.</span><span class="sxs-lookup"><span data-stu-id="880cd-231">Limit is a PowerShell-like verb.</span></span> <span data-ttu-id="880cd-232">Gränsen är funktionellt identiskt med kommandot ÖVERSTA.</span><span class="sxs-lookup"><span data-stu-id="880cd-232">Limit is functionally identical to the TOP command.</span></span> <span data-ttu-id="880cd-233">Följande frågor ger samma resultat.</span><span class="sxs-lookup"><span data-stu-id="880cd-233">The following queries return the same results.</span></span>

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a><span data-ttu-id="880cd-234">Att söka med överst</span><span class="sxs-lookup"><span data-stu-id="880cd-234">To search using top</span></span>
* <span data-ttu-id="880cd-235">Skriv i fältet Sök fråga`Type=Event EventID=600 | Top 1` </span><span class="sxs-lookup"><span data-stu-id="880cd-235">In the search query field, type `Type=Event EventID=600 | Top 1` </span></span>  
    <span data-ttu-id="880cd-236">![Sök upp](./media/log-analytics-log-searches/oms-search-top.png)</span><span class="sxs-lookup"><span data-stu-id="880cd-236">![search top](./media/log-analytics-log-searches/oms-search-top.png)</span></span>

<span data-ttu-id="880cd-237">I bilden ovan finns 358 tusen poster med händelse-ID = 600.</span><span class="sxs-lookup"><span data-stu-id="880cd-237">In the image above, there are 358 thousand records with EventID=600.</span></span> <span data-ttu-id="880cd-238">Fält, facets och filter till vänster alltid visar information om resultaten *av filter-del* av frågan som är en del innan alla vertikalstrecket.</span><span class="sxs-lookup"><span data-stu-id="880cd-238">The fields, facets, and filters on the left always show information about the results returned *by the filter portion* of the query, which is the part before any pipe character.</span></span> <span data-ttu-id="880cd-239">Den **resultat** fönstret returnerar endast det senaste resultatet 1, eftersom detta kommando Formats och omvandlas resultaten.</span><span class="sxs-lookup"><span data-stu-id="880cd-239">The **Results** pane only returns the most recent 1 result, because the example command shaped and transformed the results.</span></span>

### <a name="select"></a><span data-ttu-id="880cd-240">Välj</span><span class="sxs-lookup"><span data-stu-id="880cd-240">Select</span></span>
<span data-ttu-id="880cd-241">SELECT-kommandot fungerar som Select-Object i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="880cd-241">The SELECT command behaves like Select-Object in PowerShell.</span></span> <span data-ttu-id="880cd-242">Returnerar den filtrerade resultat som inte har alla sina ursprungliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="880cd-242">It returns filtered results that do not have all of their original properties.</span></span> <span data-ttu-id="880cd-243">I stället väljs egenskaper som du anger.</span><span class="sxs-lookup"><span data-stu-id="880cd-243">Instead, it selects only the properties that you specify.</span></span>

#### <a name="to-run-a-search-using-the-select-command"></a><span data-ttu-id="880cd-244">Att köra en sökning med select-kommando</span><span class="sxs-lookup"><span data-stu-id="880cd-244">To run a search using the select command</span></span>
1. <span data-ttu-id="880cd-245">Skriv i Sök `Type=Event` och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="880cd-245">In Search, type `Type=Event` and then click **Search**.</span></span>
2. <span data-ttu-id="880cd-246">Klicka på **+ visa fler** i ett resultat för att visa alla egenskaper som har resultaten.</span><span class="sxs-lookup"><span data-stu-id="880cd-246">Click **+ show more** in one of the results to view all the properties that the results have.</span></span>
3. <span data-ttu-id="880cd-247">Välj några av de uttryckligen och frågan ändras till `Type=Event | Select Computer,EventID,RenderedDescription`.</span><span class="sxs-lookup"><span data-stu-id="880cd-247">Select some of those explicitly, and the query changes to `Type=Event | Select Computer,EventID,RenderedDescription`.</span></span>  
    <span data-ttu-id="880cd-248">![sökningens](./media/log-analytics-log-searches/oms-search-select.png)</span><span class="sxs-lookup"><span data-stu-id="880cd-248">![search select](./media/log-analytics-log-searches/oms-search-select.png)</span></span>

<span data-ttu-id="880cd-249">Det här kommandot är särskilt användbart när du vill styra sökningen utdata och välj endast delar av data som verkligen är viktiga för din undersökning, vilket ofta är inte fullständig posten.</span><span class="sxs-lookup"><span data-stu-id="880cd-249">This command is particularly useful when you want to control search output and choose only the portions of data that really matter for your exploration, which often isn’t the full record.</span></span> <span data-ttu-id="880cd-250">Detta är också användbart när poster av olika typer har *vissa* gemensamma egenskaper, men inte *alla* är gemensamma för deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="880cd-250">This is also useful when records of different types have *some* common properties, but not *all* of their properties are common.</span></span> <span data-ttu-id="880cd-251">Du kan generera utdata som ser ut mer naturligt som en tabell eller fungera bra när exporteras till en CSV-fil och sedan massaged i Excel.</span><span class="sxs-lookup"><span data-stu-id="880cd-251">The, you can generate output that looks more naturally like a table, or work well when exported to a CSV file and then massaged in Excel.</span></span>

## <a name="use-the-measure-command"></a><span data-ttu-id="880cd-252">Använd kommandot mått</span><span class="sxs-lookup"><span data-stu-id="880cd-252">Use the measure command</span></span>
<span data-ttu-id="880cd-253">MÅTTET är något av kommandona mest flexibla i logganalys-sökningar.</span><span class="sxs-lookup"><span data-stu-id="880cd-253">MEASURE is one of the most versatile commands in Log Analytics searches.</span></span> <span data-ttu-id="880cd-254">Det kan du använda statistiska *funktioner* till dina data och mängdresultat grupperade efter ett visst fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-254">It allows you to apply statistical *functions* to your data and aggregate results grouped by a given field.</span></span> <span data-ttu-id="880cd-255">Det finns flera statistiska funktioner som har stöd för måttet.</span><span class="sxs-lookup"><span data-stu-id="880cd-255">There are multiple statistical functions that Measure supports.</span></span>

### <a name="measure-count"></a><span data-ttu-id="880cd-256">Måttet count()</span><span class="sxs-lookup"><span data-stu-id="880cd-256">Measure count()</span></span>
<span data-ttu-id="880cd-257">Den första statistiska funktionen att arbeta med och en av enklast att förstå är den *count()* funktion.</span><span class="sxs-lookup"><span data-stu-id="880cd-257">The first statistical function to work with, and one of the simplest to understand is the *count()* function.</span></span>

<span data-ttu-id="880cd-258">Resultat från en sökfråga som `Type=Event`, visa filter som kallas även aspekter på vänster sida i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="880cd-258">Results from any search query such as `Type=Event`, show filters also called facets on the left side of search results.</span></span> <span data-ttu-id="880cd-259">Filtren visar en fördelning av värden med ett visst fält för resultaten i sökningen utförs.</span><span class="sxs-lookup"><span data-stu-id="880cd-259">The filters show a distribution of values by a given field for the results in the search executed.</span></span>

![Sök mått Antal](./media/log-analytics-log-searches/oms-search-measure-count01.png)

<span data-ttu-id="880cd-261">Till exempel i bilden ovan visas den **datorn** och visar att i nästan 739 tusen-händelser i resultatet vissa 68 unika och skilda värden för den **datorn** i dessa poster.</span><span class="sxs-lookup"><span data-stu-id="880cd-261">For example, in the image above you'll see the **Computer** field and it shows that within the almost 739 thousand events in the results, there are 68 unique and distinct values for the **Computer** field in those records.</span></span> <span data-ttu-id="880cd-262">Panelen visar bara upp 5, som är de vanligaste 5 värdena som har skrivits i den **datorn** fält), sorterade efter antalet dokument som innehåller det specifikt värdet i fältet.</span><span class="sxs-lookup"><span data-stu-id="880cd-262">The tile only shows the top 5, which are the most common 5 values that are written in the **Computer** fields), sorted by the number of documents that contain that specific value in that field.</span></span> <span data-ttu-id="880cd-263">I bilden ser du att – 90 tusen bland de nästan 369 tusen händelserna – komma från datorns OpsInsights04.contoso.com 83 tusen från DB03.contoso.com-datorn och så vidare.</span><span class="sxs-lookup"><span data-stu-id="880cd-263">In the image you can see that – among those almost 369 thousand events – 90 thousand come from the OpsInsights04.contoso.com computer, 83 thousand from the DB03.contoso.com computer, and so on.</span></span>

<span data-ttu-id="880cd-264">Vad händer om du vill se alla värden eftersom panelen endast visar endast överst 5?</span><span class="sxs-lookup"><span data-stu-id="880cd-264">What if you want to see all values, since the tile only shows only the top 5?</span></span>

<span data-ttu-id="880cd-265">Det här är vad kommandot mått kan göra med funktionen count().</span><span class="sxs-lookup"><span data-stu-id="880cd-265">That’s what the measure command can do with the count() function.</span></span> <span data-ttu-id="880cd-266">Den här funktionen används inte några parametrar.</span><span class="sxs-lookup"><span data-stu-id="880cd-266">This function doesn't use any parameters.</span></span> <span data-ttu-id="880cd-267">Du bara ange det fält som du vill gruppera efter – den **datorn** fältet i det här fallet:</span><span class="sxs-lookup"><span data-stu-id="880cd-267">You just specify the field by which you want to group by – the **Computer** field in this case:</span></span>

`Type=Event | Measure count() by Computer`

![Sök mått Antal](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

<span data-ttu-id="880cd-269">Dock **datorn** är bara ett fält används *i* varje datadel – det finns inga relationsdatabaser inblandade och det finns ingen separat **datorn** objekt var som helst.</span><span class="sxs-lookup"><span data-stu-id="880cd-269">However, **Computer** is just a field used *in* each piece of data – there are no relational databases involved and there is no separate **Computer** object anywhere.</span></span> <span data-ttu-id="880cd-270">Bara värdena *i* data beskrivs vilken entitet genererade dem, och ett antal andra egenskaper och aspekter av data och därför termen *aspekten*.</span><span class="sxs-lookup"><span data-stu-id="880cd-270">Just the values *in* the data can describe which entity generated them, and a number of other characteristics and aspects of the data – hence the term *facet*.</span></span> <span data-ttu-id="880cd-271">Du kan bara också gruppera efter andra fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-271">However, you can just as well group by other fields.</span></span> <span data-ttu-id="880cd-272">Eftersom de ursprungliga resultaten av nästan 739 tusen händelser som skickas till kommandot mått har också ett fält med namnet **EventID**, kan du använda samma metod för att gruppera efter fältet och få ett antal händelser efter händelse-ID:</span><span class="sxs-lookup"><span data-stu-id="880cd-272">Because the original results of almost 739 thousand events that are piped into the measure command also have a field called **EventID**, you can apply the same technique to group by that field and get a count of events by EventID:</span></span>

```
Type=Event | Measure count() by EventID
```

<span data-ttu-id="880cd-273">Om du inte är intresserad av att antalet faktiska post som innehåller ett visst värde, men i stället om du bara vill en lista över de faktiska värdena du kan lägga till en *Välj* kommando i slutet av den och markera den första kolumnen:</span><span class="sxs-lookup"><span data-stu-id="880cd-273">If you're not interested in the actual record count that contain a specific value, but instead if you only want a list of the values themselves, you can add a *Select* command at the end of it and just select the first column:</span></span>

```
Type=Event | Measure count() by EventID | Select EventID
```

<span data-ttu-id="880cd-274">Sedan kan du få mer komplicerat och sortera resultaten i frågan, eller klicka bara kolumner i rutnätet för.</span><span class="sxs-lookup"><span data-stu-id="880cd-274">Then you can get more intricate and pre-sort the results in the query, or you can just click the columns in the grid, too.</span></span>

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a><span data-ttu-id="880cd-275">Så här söker du använder mått Antal</span><span class="sxs-lookup"><span data-stu-id="880cd-275">To search using measure count</span></span>
* <span data-ttu-id="880cd-276">Skriv i fältet Sök fråga`Type=Event | Measure count() by EventID`</span><span class="sxs-lookup"><span data-stu-id="880cd-276">In the search query field, type `Type=Event | Measure count() by EventID`</span></span>
* <span data-ttu-id="880cd-277">Lägg till `| Select EventID` till slutet av frågan.</span><span class="sxs-lookup"><span data-stu-id="880cd-277">Append `| Select EventID` to the end of the query.</span></span>
* <span data-ttu-id="880cd-278">Lägg slutligen till `| Sort EventID asc` till slutet av frågan.</span><span class="sxs-lookup"><span data-stu-id="880cd-278">Finally, append `| Sort EventID asc` to the end of the query.</span></span>

<span data-ttu-id="880cd-279">Det finns några viktiga saker att Observera och betona:</span><span class="sxs-lookup"><span data-stu-id="880cd-279">There are a couple important points to notice and emphasize:</span></span>

<span data-ttu-id="880cd-280">Först är resultatet visas inte de ursprungliga rådata resultat längre.</span><span class="sxs-lookup"><span data-stu-id="880cd-280">First, the results you see are not the original raw results anymore.</span></span> <span data-ttu-id="880cd-281">De är i stället sammanlagda resultat – i princip grupper av resultaten.</span><span class="sxs-lookup"><span data-stu-id="880cd-281">Instead, they are aggregated results – essentially groups of results.</span></span> <span data-ttu-id="880cd-282">Detta är inte ett problem, men du bör känna till att du interagerar med en mycket annan form av data som skiljer sig från den ursprungliga rådata formen som skapas direkt på grund av funktionen aggregering för statistisk.</span><span class="sxs-lookup"><span data-stu-id="880cd-282">This isn't a problem, but you should understand that you're interacting with a very different shape of data that differs from the original raw shape that gets created on the fly as a result of the aggregation/statistical function.</span></span>

<span data-ttu-id="880cd-283">Andra, **mäta antalet** returnerar för närvarande endast de 100 främsta distinkta resultat.</span><span class="sxs-lookup"><span data-stu-id="880cd-283">Second, **Measure count** currently returns only the top 100 distinct results.</span></span> <span data-ttu-id="880cd-284">Den här begränsningen gäller inte för de statistiska funktionerna.</span><span class="sxs-lookup"><span data-stu-id="880cd-284">This limit does not apply to the other statistical functions.</span></span> <span data-ttu-id="880cd-285">Så behöver du vanligtvis använda mer exakta filter först att söka efter specifika objekt innan du installerar mått count().</span><span class="sxs-lookup"><span data-stu-id="880cd-285">So, you'll usually need to use a more precise filter first to search for specific items before you apply measure count().</span></span>

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a><span data-ttu-id="880cd-286">Använda max och min med kommandot mått</span><span class="sxs-lookup"><span data-stu-id="880cd-286">Use the max and min functions with the measure command</span></span>
<span data-ttu-id="880cd-287">Det finns olika scenarier där **mått Max()** och **mått Min()** är användbara.</span><span class="sxs-lookup"><span data-stu-id="880cd-287">There are various scenarios where **Measure Max()** and **Measure Min()** are useful.</span></span> <span data-ttu-id="880cd-288">Men eftersom varje funktion är motsatt av varandra, kommer vi illustrerar Max() och du kan experimentera med Min() på egen hand.</span><span class="sxs-lookup"><span data-stu-id="880cd-288">However, since each function is opposite of each other, we'll illustrate Max() and you can experiment with Min() on your own.</span></span>

<span data-ttu-id="880cd-289">Om du frågar efter säkerhetshändelser som de har en **nivå** egenskap som kan variera.</span><span class="sxs-lookup"><span data-stu-id="880cd-289">If you query for security events, they have a **Level** property that can vary.</span></span> <span data-ttu-id="880cd-290">Exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-290">For example:</span></span>

```
Type=SecurityEvent
```

![Sök mått Antal start](./media/log-analytics-log-searches/oms-search-measure-max01.png)

<span data-ttu-id="880cd-292">Om du vill visa det högsta värdet för alla säkerheten kan händelser ges en gemensam dator, gruppera efter fält, du använda</span><span class="sxs-lookup"><span data-stu-id="880cd-292">If you want to view the highest value for all of the security events given a common Computer, the group by field, you can use</span></span>

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![söka mått max dator](./media/log-analytics-log-searches/oms-search-measure-max02.png)

<span data-ttu-id="880cd-294">Visas som för de datorer som hade **nivå** poster, de flesta av dem har minst nivå 8 många hade en nivå av 16.</span><span class="sxs-lookup"><span data-stu-id="880cd-294">It will display that for the computers that had **Level** records, most of them have at least level 8, many had a level of 16.</span></span>

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![max söktiden för måttet genereras dator](./media/log-analytics-log-searches/oms-search-measure-max03.png)

<span data-ttu-id="880cd-296">Den här funktionen fungerar bra med siffror, men den fungerar även med DateTime-fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-296">This function works well with numbers, but it also works with DateTime fields.</span></span> <span data-ttu-id="880cd-297">Det är praktiskt att söka efter senaste eller senaste tidsstämpeln för ett datablock indexerat för varje dator.</span><span class="sxs-lookup"><span data-stu-id="880cd-297">It is useful to check for the last or most recent time stamp for any piece of data indexed for each computer.</span></span> <span data-ttu-id="880cd-298">Exempel: när den senaste säkerhetshändelse rapporterades för varje dator?</span><span class="sxs-lookup"><span data-stu-id="880cd-298">For example: When was the most recent security event reported for each machine?</span></span>

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a><span data-ttu-id="880cd-299">Använda avg-funktionen med kommandot mått</span><span class="sxs-lookup"><span data-stu-id="880cd-299">Use the avg function with the measure command</span></span>
<span data-ttu-id="880cd-300">Avg() statistisk funktionen som används med mått kan du beräkna medelvärdet för vissa fält och gruppera resultat från samma eller andra fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-300">The Avg() statistical function used with measure allows you to calculate the average value for some field, and group results by the same or other field.</span></span> <span data-ttu-id="880cd-301">Detta är användbart i en mängd fall, till exempel prestandadata.</span><span class="sxs-lookup"><span data-stu-id="880cd-301">This is useful in a variety of cases, such as performance data.</span></span>

<span data-ttu-id="880cd-302">Vi börjar med prestandadata.</span><span class="sxs-lookup"><span data-stu-id="880cd-302">We'll start with performance data.</span></span> <span data-ttu-id="880cd-303">Observera att OMS för närvarande samlas in prestandaräknare för både Windows- och Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="880cd-303">Note that OMS currently collects performance counters for both Windows and Linux machines.</span></span>

<span data-ttu-id="880cd-304">Sök efter *alla* prestandadata, mest grundläggande frågan är:</span><span class="sxs-lookup"><span data-stu-id="880cd-304">To search for *all* performance data, the most basic query is:</span></span>

```
Type=Perf
```

![Sök avg start](./media/log-analytics-log-searches/oms-search-avg01.png)

<span data-ttu-id="880cd-306">Det första du märker är att visar logganalys tre perspektiv: listan som visas som visar de faktiska posterna bakom bubbeldiagram. Tabell som visar en tabellvy prestandaräknardata; och mått som visar diagram för prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="880cd-306">The first thing you'll notice is that Log Analytics shows you three perspectives: List, which shows you which shows the actual records behind the charts; Table, which shows a tabular view of performance counter data; and Metrics, which shows charts for the performance counters.</span></span>

<span data-ttu-id="880cd-307">Det finns två uppsättningar med fält som har markerats som visar följande i bilden ovan:</span><span class="sxs-lookup"><span data-stu-id="880cd-307">In the image above, there are two sets of fields marked that indicate the following:</span></span>

* <span data-ttu-id="880cd-308">Den första uppsättningen identifierar Windows namn på prestandaräknare, objektnamn och instansnamnet i fråga filter.</span><span class="sxs-lookup"><span data-stu-id="880cd-308">The first set identifies Windows Performance Counter Name, Object Name, and Instance Name in the query filter.</span></span> <span data-ttu-id="880cd-309">Dessa är de fält du förmodligen oftast används som facets-filter</span><span class="sxs-lookup"><span data-stu-id="880cd-309">These are the fields you probably will most commonly use as facets/filters</span></span>
* <span data-ttu-id="880cd-310">**CounterValue** är det faktiska värdet för räknaren.</span><span class="sxs-lookup"><span data-stu-id="880cd-310">**CounterValue** is the actual value of the counter.</span></span> <span data-ttu-id="880cd-311">I det här exemplet är värdet *75*.</span><span class="sxs-lookup"><span data-stu-id="880cd-311">In this example, the value is *75*.</span></span>
* <span data-ttu-id="880cd-312">**TimeGenerated** är 12:51 i 24-timmarsformat.</span><span class="sxs-lookup"><span data-stu-id="880cd-312">**TimeGenerated** is 12:51, in 24-hour time format.</span></span>

<span data-ttu-id="880cd-313">Här är en vy över mått i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="880cd-313">Here's a view of the metrics in a graph.</span></span>

![Sök avg start](./media/log-analytics-log-searches/oms-search-avg02.png)

<span data-ttu-id="880cd-315">Efter att läsa mer om formen Perf poster och läst om andra sökmetoder kan använda du måttet Avg() för att sammanställa den här typen av numeriska data.</span><span class="sxs-lookup"><span data-stu-id="880cd-315">After reading about the Perf record shape, and having read about other search techniques, you can use measure Avg() to aggregate this type of numerical data.</span></span>

<span data-ttu-id="880cd-316">Här är ett enkelt exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-316">Here's a simple example:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Sök avg samplevalue](./media/log-analytics-log-searches/oms-search-avg03.png)

<span data-ttu-id="880cd-318">I det här exemplet väljer du Total CPU-tid prestanda räknare och genomsnittlig per dator.</span><span class="sxs-lookup"><span data-stu-id="880cd-318">In this example, you select the CPU Total Time performance counter and average by Computer.</span></span> <span data-ttu-id="880cd-319">Om du vill begränsa sökresultaten till endast de senaste 6 timmarna kan du använda filterkontrollen tid eller ange i frågan på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="880cd-319">If you want to narrow down your results to only the last 6 hours, you can either use the time filter control or specify in your query as follows:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a><span data-ttu-id="880cd-320">Att söka med avg-funktionen med kommandot mått</span><span class="sxs-lookup"><span data-stu-id="880cd-320">To search using the avg function with the measure command</span></span>
* <span data-ttu-id="880cd-321">Skriv i sökrutan frågan `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span><span class="sxs-lookup"><span data-stu-id="880cd-321">In the Search query box, type `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span></span>

<span data-ttu-id="880cd-322">Du kan aggregera och korrelera data *över* datorer.</span><span class="sxs-lookup"><span data-stu-id="880cd-322">You can aggregate and correlate data *across* computers.</span></span> <span data-ttu-id="880cd-323">Anta exempelvis att du har en uppsättning värdar i någon form av servergruppen där varje nod är lika med andra någon och de samma skriver arbete och bör vara ungefär belastningsutjämnade.</span><span class="sxs-lookup"><span data-stu-id="880cd-323">For example, imagine that you have a set of hosts in some sort of farm where each node is equal to any other one and they just do all the same type of work and load should be roughly balanced.</span></span> <span data-ttu-id="880cd-324">Du kan få räknare i en Gå med följande fråga och få medelvärden för hela servergruppen.</span><span class="sxs-lookup"><span data-stu-id="880cd-324">You could get their counters all in one go with the following query and get averages for the entire farm.</span></span> <span data-ttu-id="880cd-325">Du kan starta genom att välja datorer med följande exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-325">You can start by choosing the computers with the following example:</span></span>

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="880cd-326">Nu när du har datorer kan du bara vill välja två nyckeltal (KPI: er): % CPU-användning och % ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="880cd-326">Now that you have the computers, you also only want to select two key performance indicators (KPIs): % CPU Usage and % Free Disk Space.</span></span> <span data-ttu-id="880cd-327">Därför blir den delen av frågan:</span><span class="sxs-lookup"><span data-stu-id="880cd-327">So, that part of the query becomes:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

<span data-ttu-id="880cd-328">Nu kan du lägga till datorer och räknare med följande exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-328">Now you can add computers and counters with the following example:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="880cd-329">Eftersom du har en särskild markering av **mäta Avg()** kommando kan returnera medelvärdet inte av datorn, men hela servergruppen, genom att gruppera efter CounterName.</span><span class="sxs-lookup"><span data-stu-id="880cd-329">Because you have a very specific selection, the **measure Avg()** command can return the average not by computer, but across the farm, simply by grouping by CounterName.</span></span> <span data-ttu-id="880cd-330">Exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-330">For example:</span></span>

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

<span data-ttu-id="880cd-331">Detta ger dig en användbar komprimerad vy i ett par KPI: er i din miljö.</span><span class="sxs-lookup"><span data-stu-id="880cd-331">This gives you a useful compact view of a couple of your environment's KPIs.</span></span>

![Sök avg gruppering](./media/log-analytics-log-searches/oms-search-avg04.png)

<span data-ttu-id="880cd-333">Du kan enkelt använda frågan i en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="880cd-333">You can easily use the search query in a dashboard.</span></span> <span data-ttu-id="880cd-334">Exempelvis kan du spara frågan och skapa en instrumentpanel i den namngivna *Web servergruppen KPI: er*.</span><span class="sxs-lookup"><span data-stu-id="880cd-334">For example, you could save the search query and create a dashboard from it named *Web Farm KPIs*.</span></span> <span data-ttu-id="880cd-335">Läs mer om att använda instrumentpaneler i [skapa en anpassad instrumentpanel i logganalys](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="880cd-335">To learn more about using dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span>

![Sök avg instrumentpanelen](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a><span data-ttu-id="880cd-337">Använd sum-funktionen med kommandot mått</span><span class="sxs-lookup"><span data-stu-id="880cd-337">Use the sum function with the measure command</span></span>
<span data-ttu-id="880cd-338">Sum-funktionen liknar andra funktioner i kommandot mått.</span><span class="sxs-lookup"><span data-stu-id="880cd-338">The sum function is similar to other functions of the measure command.</span></span> <span data-ttu-id="880cd-339">Du kan se ett exempel om hur du använder summafunktionen på [W3C IIS loggar sökning i Microsoft Azure åtgärdsinformation](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="880cd-339">You can see an example about how to use the sum function at [W3C IIS Logs Search in Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span></span>

<span data-ttu-id="880cd-340">Du kan använda Max() och Min() med siffror, datum och textsträngar.</span><span class="sxs-lookup"><span data-stu-id="880cd-340">You can use Max() and Min() with numbers, date times and text strings.</span></span> <span data-ttu-id="880cd-341">Med textsträngar, de sorteras alfabetiskt och du hämta första och sista.</span><span class="sxs-lookup"><span data-stu-id="880cd-341">With text strings, they are sorted alphabetically and you get first and last.</span></span>

<span data-ttu-id="880cd-342">Du kan dock använda SUMMA() med något annat än numeriska fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-342">However, you cannot use Sum() with anything other than numerical fields.</span></span> <span data-ttu-id="880cd-343">Detta gäller även för Avg().</span><span class="sxs-lookup"><span data-stu-id="880cd-343">This also applies to Avg().</span></span>

### <a name="use-the-percentile-function-with-the-measure-command"></a><span data-ttu-id="880cd-344">Använd percentilfunktionen med kommandot mått</span><span class="sxs-lookup"><span data-stu-id="880cd-344">Use the percentile function with the measure command</span></span>
<span data-ttu-id="880cd-345">Percentilfunktionen liknar Avg() och SUMMA() i som kan endast använda för numeriska fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-345">The percentile function is similar to Avg() and Sum() in that you can only use it for numerical fields.</span></span> <span data-ttu-id="880cd-346">Du kan använda alla percentil mellan 1 och 99 för ett numeriskt fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-346">You can use any percentile between 1 to 99 on a numeric field.</span></span> <span data-ttu-id="880cd-347">Du kan också använda båda **percentil** och **pct** kommandon.</span><span class="sxs-lookup"><span data-stu-id="880cd-347">You can also use both **percentile** and **pct** commands.</span></span> <span data-ttu-id="880cd-348">Här följer några exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-348">Here are few examples:</span></span>  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a><span data-ttu-id="880cd-349">Använd till kommandot</span><span class="sxs-lookup"><span data-stu-id="880cd-349">Use the where command</span></span>
<span data-ttu-id="880cd-350">Den där kommandot fungerar som ett filter, men den kan användas i pipeline för att filtrera ytterligare sammanlagda resultat som har producerats av måttet kommandot – i stället för raw resultat som är filtrerade i början av en fråga.</span><span class="sxs-lookup"><span data-stu-id="880cd-350">The where command works like a filter, but it can be applied in the pipeline to further filter aggregated results that have been produced by a Measure command – as opposed to raw results that are filtered at the beginning of a query.</span></span>

<span data-ttu-id="880cd-351">Exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-351">For example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

<span data-ttu-id="880cd-352">Du kan lägga till en annan pipe ”|” tecken och Where-kommandot för att bara hämta datorer vars Genomsnittlig CPU är över 80% med följande exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-352">You can add another pipe "|" character and the Where command to only get computers whose average CPU is above 80%, with the following example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

<span data-ttu-id="880cd-353">Om du är bekant med Microsoft System Center - Operations Manager kan du se where kommandot i management pack villkor.</span><span class="sxs-lookup"><span data-stu-id="880cd-353">If you're familiar with Microsoft System Center - Operations Manager, you can think of the where command in management pack terms.</span></span> <span data-ttu-id="880cd-354">En regel om exemplet, den första delen av frågan skulle vara datakällan och var skulle kommandot se ut villkorsidentifiering.</span><span class="sxs-lookup"><span data-stu-id="880cd-354">If the example were a rule, the first part of the query would be the data source and the where command would be the condition detection.</span></span>

<span data-ttu-id="880cd-355">Du kan använda frågan som en panel i **min instrumentpanel**, som en Övervakare av sorteringar och se när datorn processorer är överutnyttjade.</span><span class="sxs-lookup"><span data-stu-id="880cd-355">You can use the query as a tile in **My Dashboard**, as a monitor of sorts, to see when computer CPUs are over-utilized.</span></span> <span data-ttu-id="880cd-356">Läs mer om instrumentpaneler i [skapa en anpassad instrumentpanel i logganalys](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="880cd-356">To learn more about dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span> <span data-ttu-id="880cd-357">Du kan också skapa och använda instrumentpaneler med hjälp av mobilappen.</span><span class="sxs-lookup"><span data-stu-id="880cd-357">You can also create and use dashboards using the mobile app.</span></span> <span data-ttu-id="880cd-358">Mer information finns i [OMS Mobilapp ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span><span class="sxs-lookup"><span data-stu-id="880cd-358">For more information, see [OMS Mobile App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span></span> <span data-ttu-id="880cd-359">I de nedre två panelerna i följande bild visas övervakaren visas en lista och som ett tal.</span><span class="sxs-lookup"><span data-stu-id="880cd-359">In the bottom two tiles of the following image, you can see the monitor displayed a list and as a number.</span></span> <span data-ttu-id="880cd-360">I princip vill du alltid värdet till noll och listan måste vara tomma.</span><span class="sxs-lookup"><span data-stu-id="880cd-360">Essentially, you always want the number to be zero and the list to be empty.</span></span> <span data-ttu-id="880cd-361">Annars visar en varning villkor.</span><span class="sxs-lookup"><span data-stu-id="880cd-361">Otherwise, it indicates an alert condition.</span></span> <span data-ttu-id="880cd-362">Om det behövs kan du använda den för att ta en titt på vilka datorer som är under hög belastning.</span><span class="sxs-lookup"><span data-stu-id="880cd-362">If needed, you can use it to take a look at which machines are under pressure.</span></span>

![mobila instrumentpanelen](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a><span data-ttu-id="880cd-364">Använda operatorn i</span><span class="sxs-lookup"><span data-stu-id="880cd-364">Use the in operator</span></span>
<span data-ttu-id="880cd-365">Den *IN* -operator, tillsammans med *NOT IN* kan du använda subsearches som sökningar som inkluderar en ny sökning som argument.</span><span class="sxs-lookup"><span data-stu-id="880cd-365">The *IN* operator, along with *NOT IN* allows you to use subsearches, which are searches that include another search as an argument.</span></span> <span data-ttu-id="880cd-366">De finns i klammerparenteserna {} i en annan *primära* eller *yttre* sökning.</span><span class="sxs-lookup"><span data-stu-id="880cd-366">They are contained in braces {} within another *primary* or *outer* search.</span></span> <span data-ttu-id="880cd-367">Resultatet av en subsearch ofta en lista över olika resultat används sedan som ett argument i dess primära sökningen.</span><span class="sxs-lookup"><span data-stu-id="880cd-367">The result of a subsearch, often a list of distinct results, is then used as an argument in its primary search.</span></span>

<span data-ttu-id="880cd-368">Du kan använda subsearches för att matcha delmängder av data som du inte kan beskriva direkt i ett sökuttryck, men som kan genereras från en sökning.</span><span class="sxs-lookup"><span data-stu-id="880cd-368">You can use subsearches to match subsets of your data that you cannot describe directly in a search expression, but which can be generated from a search.</span></span> <span data-ttu-id="880cd-369">Om du är intresserad av att använda en sökning efter alla händelser från exempelvis *datorer som saknar säkerhetsuppdateringar*, måste du utforma en subsearch som identifierar som först *datorer som saknar säkerhetsuppdateringar* innan den hittar händelser som hör till de här värdarna.</span><span class="sxs-lookup"><span data-stu-id="880cd-369">For example, if you’re interested in using one search to find all events from *computers missing security updates*, then you need to design a subsearch that first identifies that *computers missing security updates* before it finds events belonging to those hosts.</span></span>

<span data-ttu-id="880cd-370">Så kan du express *datorer som för närvarande saknar nödvändiga säkerhetsuppdateringar* med följande fråga:</span><span class="sxs-lookup"><span data-stu-id="880cd-370">So, you could express *computers currently missing required security updates* with the following query:</span></span>

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![I Sök-exempel](./media/log-analytics-log-searches/oms-search-in01-revised.png)

<span data-ttu-id="880cd-372">När du har i listan kan använda du sökningen som en inre sökning för att mata in en lista med datorer i en yttre (primära) sökning som söker efter händelser för dessa datorer.</span><span class="sxs-lookup"><span data-stu-id="880cd-372">Once you have the list, you can use the search as an inner search to feed the list of computers into an outer (primary) search that will look for events for those computers.</span></span> <span data-ttu-id="880cd-373">Du kan göra detta genom att omsluta den inre sökningen med klammerparenteser och mata resultaten som möjliga värden för yttre sökningen med operatorn i filtret/fältet.</span><span class="sxs-lookup"><span data-stu-id="880cd-373">You do this by enclosing the inner search in braces and feeding its results as possible values for a filter/field in the outer search using the IN operator.</span></span> <span data-ttu-id="880cd-374">Frågan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="880cd-374">The query would resemble:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![I Sök-exempel](./media/log-analytics-log-searches/oms-search-in02-revised.png)

<span data-ttu-id="880cd-376">Även meddelandet tidsfiltret i inre sökningen eftersom systemet Update Assessment tar en ögonblicksbild av alla datorer var 24: e timme.</span><span class="sxs-lookup"><span data-stu-id="880cd-376">Also notice the time filter used in the inner search because the System Update Assessment takes a snapshot of all computers every 24 hours.</span></span> <span data-ttu-id="880cd-377">Du kan göra den inre frågan lightweight och mer exakt genom att bara söka efter en dag.</span><span class="sxs-lookup"><span data-stu-id="880cd-377">You can make the inner query more lightweight and precise by only searching for a day.</span></span> <span data-ttu-id="880cd-378">Yttre sökningen används i stället valet av tid i användargränssnittet, hämta händelser från de senaste 7 dagarna.</span><span class="sxs-lookup"><span data-stu-id="880cd-378">The outer search instead uses the time selection in the user interface, retrieving events from the last 7 days.</span></span> <span data-ttu-id="880cd-379">Se [booleska operatorer](#boolean-operators) mer information om tid operatörer.</span><span class="sxs-lookup"><span data-stu-id="880cd-379">See [Boolean operators](#boolean-operators) for more information about time operators.</span></span>

<span data-ttu-id="880cd-380">Eftersom du verkligen Använd bara resultaten av den inre som ett filter värde för den yttre, kan du fortfarande använda kommandon i yttre sökningen.</span><span class="sxs-lookup"><span data-stu-id="880cd-380">Because you really only use the results of the inner search as a filter value for the outer one, you can still apply commands in the outer search.</span></span> <span data-ttu-id="880cd-381">Du kan till exempel fortfarande gruppera händelserna ovan med ett annat mått kommando:</span><span class="sxs-lookup"><span data-stu-id="880cd-381">For example, you can still group the above events with another measure command:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![I Sök-exempel](./media/log-analytics-log-searches/oms-search-in03-revised.png)

<span data-ttu-id="880cd-383">I allmänhet kan du inre frågan köras snabbt eftersom logganalys har tjänsten på klientsidan tidsgränser för det och att returnera en liten mängd resultat.</span><span class="sxs-lookup"><span data-stu-id="880cd-383">Generally, you want your inner query to execute quickly because Log Analytics has service-side timeouts for it and also to return a small amount of results.</span></span> <span data-ttu-id="880cd-384">Om den inre frågan returnerar fler resultat, hämtar resultatlistan trunkerad, vilket kan medföra yttre sökning för att returnera felaktiga resultat.</span><span class="sxs-lookup"><span data-stu-id="880cd-384">If the inner query returns more results, the result list gets truncated, which could potentially cause the outer search to return incorrect results.</span></span>

<span data-ttu-id="880cd-385">En annan regel är att inre sökningen för närvarande måste ange *aggregerade* resultat.</span><span class="sxs-lookup"><span data-stu-id="880cd-385">Another rule is that the inner search currently needs to provide *aggregated* results.</span></span> <span data-ttu-id="880cd-386">Med andra ord, det måste innehålla en *mått* kommandot; du kan för närvarande feed rådata resultat i en yttre sökning.</span><span class="sxs-lookup"><span data-stu-id="880cd-386">In other words, it must contain a *measure* command; you cannot currently feed raw results into an outer search.</span></span>

<span data-ttu-id="880cd-387">Dessutom kan det finnas en enda IN-operatorn och det måste vara det sista filtret i frågan.</span><span class="sxs-lookup"><span data-stu-id="880cd-387">Also, there can be only one IN operator and it must be the last filter in the query.</span></span> <span data-ttu-id="880cd-388">Flera IN operatorer får inte vara eller skulle – det i praktiken innebär att köra flera subsearches: Det viktiga är att endast en sub/inre sökningen är möjligt för varje yttre sökning.</span><span class="sxs-lookup"><span data-stu-id="880cd-388">Multiple IN operators cannot be OR’d – this essentially prevents running multiple subsearches: the important point is that only one sub/inner search is possible for each outer search.</span></span>

<span data-ttu-id="880cd-389">Även om dessa begränsningar i gör många typer av korrelerade sökningar och kan du definiera något som liknar grupper, till exempel datorer, användare eller filer – oavsett fälten i dina data innehåller.</span><span class="sxs-lookup"><span data-stu-id="880cd-389">Even with these limits, IN enables many kinds of correlated searches, and allows you to define something similar to groups such as computers, users, or files – whatever the fields in your data contain.</span></span> <span data-ttu-id="880cd-390">Här följer fler exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-390">Here are more examples:</span></span>

<span data-ttu-id="880cd-391">**Alla uppdateringar som saknas från datorer där inställningen för automatiska uppdateringar är inaktiverad**</span><span class="sxs-lookup"><span data-stu-id="880cd-391">**All updates missing from computers where Automatic Update setting is disabled**</span></span>

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

<span data-ttu-id="880cd-392">**Alla fel-händelser från datorer som kör SQL Server (= där SQL-bedömning har körts)**</span><span class="sxs-lookup"><span data-stu-id="880cd-392">**All error events from computers running SQL Server (=where SQL Assessment has run)**</span></span>

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

<span data-ttu-id="880cd-393">**Alla säkerhetshändelser från datorer som är domänkontrollanter (= där AD-bedömning har körts)**</span><span class="sxs-lookup"><span data-stu-id="880cd-393">**All security events from computers that are Domain Controllers (=where AD Assessment has run)**</span></span>

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

<span data-ttu-id="880cd-394">**Som andra konton har loggat in till samma datorer där kontot BACONLAND\jochan har loggat in?**</span><span class="sxs-lookup"><span data-stu-id="880cd-394">**Which other accounts have logged on to the same computers where account BACONLAND\jochan has logged on?**</span></span>

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a><span data-ttu-id="880cd-395">Använd kommandot distinkta</span><span class="sxs-lookup"><span data-stu-id="880cd-395">Use the distinct command</span></span>
<span data-ttu-id="880cd-396">Det här kommandot ger en lista över distinkta värden för ett fält som namnet antyder.</span><span class="sxs-lookup"><span data-stu-id="880cd-396">As the name suggests, this command provides a list of distinct values for a field.</span></span> <span data-ttu-id="880cd-397">Det är förstås enkla men ganska användbart.</span><span class="sxs-lookup"><span data-stu-id="880cd-397">It's surprisingly simple but quite useful.</span></span> <span data-ttu-id="880cd-398">Samma kan uppnås med måttet count() kommandot samt, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="880cd-398">The same could be achieved with measure count() command as well, as shown below.</span></span>

```
Type=Event | Measure count() by Computer
```

![DISTINKTA Sök exemplet](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

<span data-ttu-id="880cd-400">Om du är intresserad är dock bara en lista över distinkta värden och inte antalet dokument som innehåller värden och sedan DISTINCT kan ge tydligare och lättare att läsa utdata och kortare syntax som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="880cd-400">However, if all you're interested in is just a list of distinct values and not the count of documents that have that values, then DISTINCT can provide cleaner and easier to read output, and shorter syntax, as shown below.</span></span>

```
Type=Event | Distinct Computer
```
![DISTINKTA Sök exemplet](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a><span data-ttu-id="880cd-402">Använd funktionen countdistinct med kommandot mått</span><span class="sxs-lookup"><span data-stu-id="880cd-402">Use the countdistinct function with the measure command</span></span>
<span data-ttu-id="880cd-403">Funktionen countdistinct räknar antalet distinkta värden inom varje grupp.</span><span class="sxs-lookup"><span data-stu-id="880cd-403">The countdistinct function counts the number of distinct values within each group.</span></span> <span data-ttu-id="880cd-404">Till exempel kan den användas för att räkna antalet unika datorer som rapporterar för varje typ:</span><span class="sxs-lookup"><span data-stu-id="880cd-404">For example, it could be used to count the number of unique computers reporting for each Type:</span></span>

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a><span data-ttu-id="880cd-406">Använd kommandot mått intervall</span><span class="sxs-lookup"><span data-stu-id="880cd-406">Use the measure interval command</span></span>
<span data-ttu-id="880cd-407">Med nästan realtid prestandadatainsamling, kan du samla in och visualisera alla prestandaräknare i logganalys.</span><span class="sxs-lookup"><span data-stu-id="880cd-407">With near real-time performance data collection, you can collect and visualize any performance counter in Log Analytics.</span></span> <span data-ttu-id="880cd-408">Att bara ange frågan **typ: Perf** returnerar tusentals mått diagram baserat på antalet räknare och servrar i logganalys-miljö.</span><span class="sxs-lookup"><span data-stu-id="880cd-408">Simply entering the query **Type:Perf** will return thousands of metric graphs based on the number of counters and servers in your Log Analytics environment.</span></span> <span data-ttu-id="880cd-409">Med på begäran mått aggregering tittar du på den övergripande måtten i din miljö vid en hög nivå och ingående till mer detaljerade data som du behöver.</span><span class="sxs-lookup"><span data-stu-id="880cd-409">With on-demand metric aggregation, you can look at the overall metrics in your environment at a high level, and deep dive into more granular data as you need to.</span></span>

<span data-ttu-id="880cd-410">Anta att du vill veta vad är den genomsnittliga CPU på alla datorer.</span><span class="sxs-lookup"><span data-stu-id="880cd-410">Let’s say that you want to know what is the average CPU across all your computers.</span></span> <span data-ttu-id="880cd-411">Titta på Genomsnittlig CPU för varje dator kanske inte praktiskt eftersom resultat kan hämta jämnas ut.</span><span class="sxs-lookup"><span data-stu-id="880cd-411">Looking at the average CPU for every computer might not be helpful because results may get smoothed out.</span></span> <span data-ttu-id="880cd-412">Om du vill se mer detaljer kan du aggregera dina resultat i en mindre tid fönstret blocken och leta till en tidsserie på olika dimensioner.</span><span class="sxs-lookup"><span data-stu-id="880cd-412">To look into more details, you can aggregate your result in a smaller time window chunks, and look into a time series across different dimensions.</span></span> <span data-ttu-id="880cd-413">T.ex, kan du utföra timvis medelvärdet av CPU-användning på alla datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="880cd-413">For example, you can perform the hourly average of CPU usage across all your computers as follows:</span></span>

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![måttet genomsnittlig intervall](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

<span data-ttu-id="880cd-415">Som standard visas resultaten i interaktiva linjediagram med flera serier.</span><span class="sxs-lookup"><span data-stu-id="880cd-415">By default these results will be displayed in a multi-series interactive line chart.</span></span>  <span data-ttu-id="880cd-416">Det här diagrammet stöder serien växla (med y-axeln rescaling), Zooma och placera markören.</span><span class="sxs-lookup"><span data-stu-id="880cd-416">This chart supports series toggling (with y-axis rescaling), zooming, and hovering.</span></span>  <span data-ttu-id="880cd-417">Visningsalternativ för tabellen finns kvar för att visa rådata om det behövs.</span><span class="sxs-lookup"><span data-stu-id="880cd-417">The table display option is still available for viewing the raw data if necessary.</span></span>

<span data-ttu-id="880cd-418">Du kan också gruppera efter andra fält.</span><span class="sxs-lookup"><span data-stu-id="880cd-418">You can also group by other fields.</span></span> <span data-ttu-id="880cd-419">I det här exemplet jag tittar på alla % räknare för en viss dator och du vill veta vad är varje timme 70 percentiler för varje räknare:</span><span class="sxs-lookup"><span data-stu-id="880cd-419">In this example, I am looking at all the % counters for one specific computer, and I want to know what is the hourly 70 percentiles of every counter:</span></span>

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
<span data-ttu-id="880cd-420">Observera är dessa frågor inte är begränsade till prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="880cd-420">One thing to note is that these queries are not limited to performance counters.</span></span> <span data-ttu-id="880cd-421">Du kan använda dem på alla mått.</span><span class="sxs-lookup"><span data-stu-id="880cd-421">You can apply them to any metric.</span></span> <span data-ttu-id="880cd-422">I det här exemplet tittar jag på W3C IIS-loggar.</span><span class="sxs-lookup"><span data-stu-id="880cd-422">In this example, I’m looking at W3C IIS logs.</span></span> <span data-ttu-id="880cd-423">Jag vill veta vad är den maximala tid det tar över 5-minutersintervall för bearbetning av varje förfrågan:</span><span class="sxs-lookup"><span data-stu-id="880cd-423">I want to know what is the maximum time it takes over a 5-minute interval for processing each request:</span></span>

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a><span data-ttu-id="880cd-424">Använda flera aggregeringar i en fråga</span><span class="sxs-lookup"><span data-stu-id="880cd-424">Use multiple aggregates in one query</span></span>
<span data-ttu-id="880cd-425">Du kan ange flera sammanställd satser i kommandot mått.</span><span class="sxs-lookup"><span data-stu-id="880cd-425">You can specify multiple aggregate clauses in a measure command.</span></span>  <span data-ttu-id="880cd-426">Alias kan vara oberoende av var och en.</span><span class="sxs-lookup"><span data-stu-id="880cd-426">Each one can be aliased independently.</span></span>  <span data-ttu-id="880cd-427">Om den inte ges ett alias blir det resulterande fältnamnet mängdfunktionen som var används (d.v.s. ”avg(CounterValue)” för avg(CounterValue)).</span><span class="sxs-lookup"><span data-stu-id="880cd-427">If it is not given an alias the resulting field name will be the aggregate function that was used (i.e. "avg(CounterValue)" for avg(CounterValue)).</span></span>

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

<span data-ttu-id="880cd-429">Här är ett annat exempel:</span><span class="sxs-lookup"><span data-stu-id="880cd-429">Here is another example:</span></span>

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a><span data-ttu-id="880cd-430">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="880cd-430">Next steps</span></span>
<span data-ttu-id="880cd-431">Mer information om loggen sökningar finns:</span><span class="sxs-lookup"><span data-stu-id="880cd-431">For additional information about log searches, see:</span></span>

* <span data-ttu-id="880cd-432">Använd [anpassade fält i logganalys](log-analytics-custom-fields.md) att utöka loggen sökningar.</span><span class="sxs-lookup"><span data-stu-id="880cd-432">Use [Custom fields in Log Analytics](log-analytics-custom-fields.md) to extend log searches.</span></span>
* <span data-ttu-id="880cd-433">Granska de [logganalys logga Sök referens](log-analytics-search-reference.md) att visa alla sökfält och aspekter som är tillgängliga i logganalys.</span><span class="sxs-lookup"><span data-stu-id="880cd-433">Review the [Log Analytics log search reference](log-analytics-search-reference.md) to view all of the search fields and facets available in Log Analytics.</span></span>
