---
title: "aaaUsing hello loggen Sök portal i Azure Log Analytics | Microsoft Docs"
description: "Den här artikeln innehåller en genomgång som beskriver hur toocreate logga sökningar och analysera data som lagras i logganalys-arbetsytan med hjälp av hello loggen Sök portal.  hello självstudier innehåller och köra några enkla frågor tooreturn olika typer av data och analysera resultat."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 2e6633d548bb508edc0c650d11d2c32fc6ee536c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-hello-log-search-portal"></a><span data-ttu-id="417db-104">Skapa loggen sökningar i Azure Log Analytics med hjälp av hello loggen Sök portal</span><span class="sxs-lookup"><span data-stu-id="417db-104">Create log searches in Azure Log Analytics using hello Log Search portal</span></span>

> [!NOTE]
> <span data-ttu-id="417db-105">Den här artikeln beskriver hello loggen Sök portal i Azure Log Analytics med hjälp av hello nya frågespråk.</span><span class="sxs-lookup"><span data-stu-id="417db-105">This article describes hello Log Search portal in Azure Log Analytics using hello new query language.</span></span>  <span data-ttu-id="417db-106">Du lär dig mer om hello nytt språk och få hello proceduren tooupgrade ditt arbetsområde på [uppgradera sökningen Azure logganalys arbetsytan toonew loggen](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="417db-106">You can learn more about hello new language and get hello procedure tooupgrade your workspace at [Upgrade your Azure Log Analytics workspace toonew log search](log-analytics-log-search-upgrade.md).</span></span>  
>
> <span data-ttu-id="417db-107">Om ditt arbetsområde inte har varit uppgraderade toohello nya frågespråk, bör du läsa för[söka efter data med hjälp av loggen sökningar i logganalys](log-analytics-log-searches.md) information om hello nuvarande version av hello loggen Sök-portalen.</span><span class="sxs-lookup"><span data-stu-id="417db-107">If your workspace hasn't been upgraded toohello new query language, you should refer too[Find data using log searches in Log Analytics](log-analytics-log-searches.md) for information on hello current version of hello Log Search portal.</span></span>

<span data-ttu-id="417db-108">Den här artikeln innehåller en genomgång som beskriver hur toocreate logga sökningar och analysera data som lagras i logganalys-arbetsytan med hjälp av hello loggen Sök portal.</span><span class="sxs-lookup"><span data-stu-id="417db-108">This article includes a tutorial that describes how toocreate log searches and analyze data stored in your Log Analytics workspace using hello Log Search portal.</span></span>  <span data-ttu-id="417db-109">hello självstudier innehåller och köra några enkla frågor tooreturn olika typer av data och analysera resultat.</span><span class="sxs-lookup"><span data-stu-id="417db-109">hello tutorial includes running some simple queries tooreturn different types of data and analyzing results.</span></span>  <span data-ttu-id="417db-110">Den fokuserar på funktioner i hello loggen Sök portal för att ändra hello frågan i stället för att ändra direkt.</span><span class="sxs-lookup"><span data-stu-id="417db-110">It focuses on features in hello Log Search portal for modifying hello query rather than modifying it directly.</span></span>  <span data-ttu-id="417db-111">Mer information om direkt redigera hello frågan finns hello [Query Language referens](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="417db-111">For details on directly editing hello query, see hello [Query Language reference](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>

<span data-ttu-id="417db-112">toocreate söker i hello Advanced Analytics-portalen i stället för hello loggen Sök portal finns [komma igång med hello Analytics-portalen](https://go.microsoft.com/fwlink/?linkid=856587).</span><span class="sxs-lookup"><span data-stu-id="417db-112">toocreate searches in hello Advanced Analytics portal instead of hello Log Search portal, see [Getting Started with hello Analytics Portal](https://go.microsoft.com/fwlink/?linkid=856587).</span></span>  <span data-ttu-id="417db-113">Båda portaler använda hello samma fråga språk tooaccess hello samma data i hello logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="417db-113">Both portals use hello same query language tooaccess hello same data in hello Log Analytics workspace.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="417db-114">Krav</span><span class="sxs-lookup"><span data-stu-id="417db-114">Prerequisites</span></span>
<span data-ttu-id="417db-115">Den här kursen förutsätter att du redan har en logganalys-arbetsytan med minst en ansluten datakälla som genererar data för hello frågor tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="417db-115">This tutorial assumes that you already have a Log Analytics workspace with at least one connected source that generates data for hello queries tooanalyze.</span></span>  

- <span data-ttu-id="417db-116">Om du inte har en arbetsyta, du kan skapa en kostnadsfri hello sätt på [komma igång med en logganalys-arbetsytan](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="417db-116">If you don't have a workspace, you can create a free one using hello procedure at [Get started with a Log Analytics workspace](log-analytics-get-started.md).</span></span>
- <span data-ttu-id="417db-117">Ansluta minst ett [Windows-agenten](log-analytics-windows-agents.md) eller en [Linux-agenten](log-analytics-linux-agents.md) toohello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="417db-117">Connect least one [Windows agent](log-analytics-windows-agents.md) or one [Linux agent](log-analytics-linux-agents.md) toohello workspace.</span></span>  

## <a name="open-hello-log-search-portal"></a><span data-ttu-id="417db-118">Öppna hello loggen Sök portal</span><span class="sxs-lookup"><span data-stu-id="417db-118">Open hello Log Search portal</span></span>
<span data-ttu-id="417db-119">Starta genom att öppna hello loggen Sök-portalen.</span><span class="sxs-lookup"><span data-stu-id="417db-119">Start by opening hello Log Search portal.</span></span>  <span data-ttu-id="417db-120">Du kan komma åt den i hello Azure-portalen eller hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="417db-120">You can access it in either hello Azure portal or hello OMS portal.</span></span>

1. <span data-ttu-id="417db-121">Öppna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="417db-121">Open hello Azure portal.</span></span>
2. <span data-ttu-id="417db-122">Navigera tooLog Analytics och markera arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="417db-122">Navigate tooLog Analytics and select your workspace.</span></span>
3. <span data-ttu-id="417db-123">Välj antingen **loggen Sök** toostay i hello Azure-portalen eller starta hello OMS-portalen genom att välja **OMS-portalen** och sedan klicka på sökknappen för hello loggen.</span><span class="sxs-lookup"><span data-stu-id="417db-123">Either select **Log Search** toostay in hello Azure portal or launch hello OMS portal by selecting **OMS Portal** and then clicking hello Log Search button.</span></span>

![Logga sökknappen](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a><span data-ttu-id="417db-125">Skapa en enkel sökning</span><span class="sxs-lookup"><span data-stu-id="417db-125">Create a simple search</span></span>
<span data-ttu-id="417db-126">hello snabbaste sättet tooretrieve vissa data toowork med är en enkel fråga som returnerar alla poster i tabellen.</span><span class="sxs-lookup"><span data-stu-id="417db-126">hello quickest way tooretrieve some data toowork with is a simple query that returns all records in table.</span></span>  <span data-ttu-id="417db-127">Om du har en Windows- eller Linux-klienter anslutna tooyour arbetsyta har du data i antingen hello händelse (Windows) eller Syslog (Linux) tabell.</span><span class="sxs-lookup"><span data-stu-id="417db-127">If you have any Windows or Linux clients connected tooyour workspace, then you'll have data in either hello Event (Windows) or Syslog (Linux) table.</span></span>

<span data-ttu-id="417db-128">Skriv en hello följande frågor i hello sökrutan och klicka hello sökning.</span><span class="sxs-lookup"><span data-stu-id="417db-128">Type one hello following queries in hello search box and click hello search button.</span></span>  

```
Event
```
```
Syslog
```

<span data-ttu-id="417db-129">Data returneras i listvyn för hello standard och du kan se hur många Totalt antal poster returnerades.</span><span class="sxs-lookup"><span data-stu-id="417db-129">Data is returned in hello default list view, and you can see how many total records were returned.</span></span>

![Enkel fråga](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

<span data-ttu-id="417db-131">Endast visas hello första några egenskaper för varje post.</span><span class="sxs-lookup"><span data-stu-id="417db-131">Only hello first few properties of each record are displayed.</span></span>  <span data-ttu-id="417db-132">Klicka på **visa fler** toodisplay alla egenskaper för en viss post.</span><span class="sxs-lookup"><span data-stu-id="417db-132">Click **show more** toodisplay all properties for a particular record.</span></span>

![Registrera information](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-hello-time-scope"></a><span data-ttu-id="417db-134">Ange hello tid scope</span><span class="sxs-lookup"><span data-stu-id="417db-134">Set hello time scope</span></span>
<span data-ttu-id="417db-135">Alla poster som samlas in av logganalys har en **TimeGenerated** egenskap som innehåller hello datum och tid hello posten skapades.</span><span class="sxs-lookup"><span data-stu-id="417db-135">Every record collected by Log Analytics has a **TimeGenerated** property that contains hello date and time that hello record was created.</span></span>  <span data-ttu-id="417db-136">En fråga i hello loggen Sök portal returnerar bara poster med en **TimeGenerated** omfattas hello tid som visas på vänster sida av skärmen hello hello.</span><span class="sxs-lookup"><span data-stu-id="417db-136">A query in hello Log Search portal only returns records with a **TimeGenerated** within hello time scope that's displayed on hello left side of hello screen.</span></span>  

<span data-ttu-id="417db-137">Du kan ändra hello tidsfiltret genom att välja hello dropdown eller genom att ändra hello skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="417db-137">You can change hello time filter either by selecting hello dropdown or by modifying hello slider.</span></span>  <span data-ttu-id="417db-138">hello skjutreglaget visar ett stapeldiagram som visar hello relativa antal poster för varje gång segment inom hello intervallet.</span><span class="sxs-lookup"><span data-stu-id="417db-138">hello slider displays a bar graph that shows hello relative number of records for each time segment within hello range.</span></span>  <span data-ttu-id="417db-139">Det här segmentet varierar beroende på hello intervall.</span><span class="sxs-lookup"><span data-stu-id="417db-139">This segment will vary depending on hello range.</span></span>

<span data-ttu-id="417db-140">hello standard tid omfånget är **1 dag**.</span><span class="sxs-lookup"><span data-stu-id="417db-140">hello default time scope is **1 day**.</span></span>  <span data-ttu-id="417db-141">Ändra värdet för**7 dagar**, och öka hello Totalt antal poster.</span><span class="sxs-lookup"><span data-stu-id="417db-141">Change this value too**7 days**, and hello total number of records should increase.</span></span>

![Datum tid omfång](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-hello-query"></a><span data-ttu-id="417db-143">Filtrera resultatet av hello fråga</span><span class="sxs-lookup"><span data-stu-id="417db-143">Filter results of hello query</span></span>
<span data-ttu-id="417db-144">På hello är vänster hello-skärmen hello filterfönstret där du tooadd filtrering toohello fråga utan att ändra den direkt.</span><span class="sxs-lookup"><span data-stu-id="417db-144">On hello left side of hello screen is hello filter pane which allows you tooadd filtering toohello query without modifying it directly.</span></span>  <span data-ttu-id="417db-145">Flera egenskaper för hello poster returneras visas med deras översta tio värden med antal poster.</span><span class="sxs-lookup"><span data-stu-id="417db-145">Several properties of hello records returned are displayed with their top ten values with their record count.</span></span>

<span data-ttu-id="417db-146">Om du arbetar med **händelse**, Välj hello för kryssrutan bredvid**fel** under **EVENTLEVELNAME**.</span><span class="sxs-lookup"><span data-stu-id="417db-146">If you're working with **Event**, select hello checkbox next too**Error** under **EVENTLEVELNAME**.</span></span>   <span data-ttu-id="417db-147">Om du arbetar med **Syslog**, Välj hello för kryssrutan bredvid**err** under **SEVERITYLEVEL**.</span><span class="sxs-lookup"><span data-stu-id="417db-147">If you're working with **Syslog**, select hello checkbox next too**err** under **SEVERITYLEVEL**.</span></span>  <span data-ttu-id="417db-148">Detta ändrar hello fråga tooone av hello följande toolimit hello resulterar tooerror händelser.</span><span class="sxs-lookup"><span data-stu-id="417db-148">This changes hello query tooone of hello following toolimit hello results tooerror events.</span></span>

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filter](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

<span data-ttu-id="417db-150">Lägg till egenskaper toohello filterfönstret genom att välja **lägga till toofilters** hello egenskapen-menyn på en av hello poster.</span><span class="sxs-lookup"><span data-stu-id="417db-150">Add properties toohello filter pane by selecting **Add toofilters** from hello property menu on one of hello records.</span></span>

![Toofilter menyn Lägg till](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

<span data-ttu-id="417db-152">Du kan ange samma filter genom att välja hello **Filter** hello egenskapen menyn för en post med hello värde du vill toofilter.</span><span class="sxs-lookup"><span data-stu-id="417db-152">You can set hello same filter by selecting **Filter** from hello property menu for a record with hello value you want toofilter.</span></span>  

<span data-ttu-id="417db-153">Du har bara hello **Filter** alternativ för egenskaper med deras namn i blått.</span><span class="sxs-lookup"><span data-stu-id="417db-153">You only have hello **Filter** option for properties with their name in blue.</span></span>  <span data-ttu-id="417db-154">Dessa är *sökbara* fält som indexeras för sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="417db-154">These are *searchable* fields which are indexed for search conditions.</span></span>  <span data-ttu-id="417db-155">Fält i grått *fritext sökbara* fält som endast har hello **visa referenser** alternativet.</span><span class="sxs-lookup"><span data-stu-id="417db-155">Fields in grey are *free text searchable* fields which only have hello **Show references** option.</span></span>  <span data-ttu-id="417db-156">Det här alternativet returnerar poster som har detta värde i en egenskap.</span><span class="sxs-lookup"><span data-stu-id="417db-156">This option returns records that have that value in any property.</span></span>

![Filter-menyn](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

<span data-ttu-id="417db-158">Du kan gruppera hello resultat på en enskild egenskap genom att välja hello **Gruppera efter** alternativ i hello post-menyn.</span><span class="sxs-lookup"><span data-stu-id="417db-158">You can group hello results on a single property by selecting hello **Group by** option in hello record menu.</span></span>  <span data-ttu-id="417db-159">Detta lägger till en [sammanfatta](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operatorn tooyour fråga som visar hello resultatet i ett diagram.</span><span class="sxs-lookup"><span data-stu-id="417db-159">This will add a [summarize](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operator tooyour query that displays hello results in a chart.</span></span>  <span data-ttu-id="417db-160">Du kan gruppera på mer än en egenskap, men du behöver tooedit hello frågan direkt.</span><span class="sxs-lookup"><span data-stu-id="417db-160">You can group on more than one property, but you would need tooedit hello query directly.</span></span>  <span data-ttu-id="417db-161">Välj hello poster menyn nästa hello hello **datorn** egenskapen och välj **Gruppera efter ”dator”**.</span><span class="sxs-lookup"><span data-stu-id="417db-161">Select hello record menu next hello hello **Computer** property and select **Group by 'Computer'**.</span></span>  

![Gruppera efter dator](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a><span data-ttu-id="417db-163">Arbeta med resultat</span><span class="sxs-lookup"><span data-stu-id="417db-163">Work with results</span></span>
<span data-ttu-id="417db-164">hello loggen Sök portalen innehåller en mängd funktioner för att arbeta med hello resultatet av en fråga.</span><span class="sxs-lookup"><span data-stu-id="417db-164">hello Log Search portal has a variety of features for working with hello results of a query.</span></span>  <span data-ttu-id="417db-165">Du kan sortera, filtrera och gruppera resulterar tooanalyze hello data utan att ändra hello faktiska fråga.</span><span class="sxs-lookup"><span data-stu-id="417db-165">You can sort, filter, and group results tooanalyze hello data without modifying hello actual query.</span></span>  <span data-ttu-id="417db-166">Resultatet av en fråga sorteras inte som standard.</span><span class="sxs-lookup"><span data-stu-id="417db-166">Results of a query are not sorted by default.</span></span>

<span data-ttu-id="417db-167">tooview hello data i tabellformat som tillhandahåller ytterligare alternativ för att filtrera och sortera, klicka på **tabellen**.</span><span class="sxs-lookup"><span data-stu-id="417db-167">tooview hello data in table form which provides additional options for filtering and sorting, click **Table**.</span></span>  

![Tabellvy](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

<span data-ttu-id="417db-169">Klicka på pilen för hello genom ett poster tooview hello information för den posten.</span><span class="sxs-lookup"><span data-stu-id="417db-169">Click hello arrow by a record tooview hello details for that record.</span></span>

![Sortera resultat](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

<span data-ttu-id="417db-171">Sortera efter något fält genom att klicka på dess kolumnrubriken.</span><span class="sxs-lookup"><span data-stu-id="417db-171">Sort on any field by clicking on its column header.</span></span>

![Sortera resultat](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

<span data-ttu-id="417db-173">Filtrera hello resultaten på ett specifikt värde i kolumnen hello genom att klicka hello filter och tillhandahåller ett filtervillkor.</span><span class="sxs-lookup"><span data-stu-id="417db-173">Filter hello results on a specific value in hello column by clicking hello filter button and providing a filter condition.</span></span>

![Filtrera resultat](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

<span data-ttu-id="417db-175">Gruppera efter en kolumn genom att dra dess kolumnen huvud toohello överkant hello resultat.</span><span class="sxs-lookup"><span data-stu-id="417db-175">Group on a column by dragging its column header toohello top of hello results.</span></span>  <span data-ttu-id="417db-176">Du kan gruppera på flera fält genom att dra flera kolumner toohello upp.</span><span class="sxs-lookup"><span data-stu-id="417db-176">You can group on multiple fields by dragging multiple columns toohello top.</span></span>

![Gruppera resultat](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a><span data-ttu-id="417db-178">Arbeta med prestandadata</span><span class="sxs-lookup"><span data-stu-id="417db-178">Work with performance data</span></span>
<span data-ttu-id="417db-179">Prestandadata för både Windows och Linux-agenter lagras i hello logganalys-arbetsytan i hello **Perf** tabell.</span><span class="sxs-lookup"><span data-stu-id="417db-179">Performance data for both Windows and Linux agents is stored in hello Log Analytics workspace in hello **Perf** table.</span></span>  <span data-ttu-id="417db-180">Uppgifter som ser ut precis som andra poster och vi kan skriva en enkel fråga som returnerar alla uppgifter på samma sätt som med händelser.</span><span class="sxs-lookup"><span data-stu-id="417db-180">Performance records look just like any other record, and we can write a simple query that returns all performance records just like with events.</span></span>

```
Perf
```

![Prestandadata](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

<span data-ttu-id="417db-182">Returnerar miljontals poster för alla prestandaobjekt och räknare men är inte användbar.</span><span class="sxs-lookup"><span data-stu-id="417db-182">Returning millions of records for all performance objects and counters though isn't very useful.</span></span>  <span data-ttu-id="417db-183">Du kan använda samma metoder som du använt ovanför toofilter hello data eller bara skriver hello följande fråga hello direkt i sökrutan för hello loggen.</span><span class="sxs-lookup"><span data-stu-id="417db-183">You can use hello same methods you used above toofilter hello data or just type hello following query directly into hello log search box.</span></span>  <span data-ttu-id="417db-184">Detta returnerar endast processor användning poster för både Windows och Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="417db-184">This returns only processor utilization records for both Windows and Linux computers.</span></span>

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Processorbelastning](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

<span data-ttu-id="417db-186">Detta begränsar hello data tooa räknaren, men det fortfarande Placera inte den i ett formulär som är särskilt användbart.</span><span class="sxs-lookup"><span data-stu-id="417db-186">This limits hello data tooa particular counter, but it still doesn't put it in a form that's particularly useful.</span></span>  <span data-ttu-id="417db-187">Du kan visa hello data i ett linjediagram, men måste först toogroup den av dator- och TimeGenerated.</span><span class="sxs-lookup"><span data-stu-id="417db-187">You can display hello data in a line chart, but first need toogroup it by Computer and TimeGenerated.</span></span>  <span data-ttu-id="417db-188">toogroup på flera fält måste toomodify hello frågan direkt, så att ändra hello frågan toohello följande.</span><span class="sxs-lookup"><span data-stu-id="417db-188">toogroup on multiple fields, you need toomodify hello query directly, so modify hello query toohello following.</span></span>  <span data-ttu-id="417db-189">Här används hello [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) fungerar på hello **CounterValue** egenskapen toocalculate hello genomsnittligt värde över varje timme.</span><span class="sxs-lookup"><span data-stu-id="417db-189">This uses hello [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) function on hello **CounterValue** property toocalculate hello average value over each hour.</span></span>

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Data prestandadiagrammet](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

<span data-ttu-id="417db-191">Nu när hello data är lämpligt grupperade kan du visa det i ett visual diagram genom att lägga till hello [återge](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operator.</span><span class="sxs-lookup"><span data-stu-id="417db-191">Now that hello data is suitably grouped, you can display it in a visual chart by adding hello [render](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operator.</span></span>  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Linjediagram](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a><span data-ttu-id="417db-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="417db-193">Next steps</span></span>

- <span data-ttu-id="417db-194">Mer information om hello Log Analytics-frågespråket i [komma igång med hello Analytics-portalen](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="417db-194">Learn more about hello Log Analytics query language at [Getting Started with hello Analytics Portal](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>
- <span data-ttu-id="417db-195">Gå igenom en självstudiekurs med hello [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) vilket gör att du toorun hello samma frågor och åtkomst hello samma data som hello loggen Sök-portalen.</span><span class="sxs-lookup"><span data-stu-id="417db-195">Walk through a tutorial using hello [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) which allows you toorun hello same queries and access hello same data as hello Log Search portal.</span></span>
