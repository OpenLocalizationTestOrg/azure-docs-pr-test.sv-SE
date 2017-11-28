---
title: "aaaManaging statistik på tabellerna i SQL Data Warehouse | Microsoft Docs"
description: "Komma igång med statistik på tabellerna i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: c9521dc47891f68d124e77a53e2e15d03275caaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a><span data-ttu-id="2e1de-103">Hantera statistik på tabellerna i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2e1de-103">Managing statistics on tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="2e1de-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="2e1de-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="2e1de-105">[Datatyper][Data Types]</span><span class="sxs-lookup"><span data-stu-id="2e1de-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="2e1de-106">[Distribuera][Distribute]</span><span class="sxs-lookup"><span data-stu-id="2e1de-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="2e1de-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="2e1de-107">[Index][Index]</span></span>
> * <span data-ttu-id="2e1de-108">[Partition][Partition]</span><span class="sxs-lookup"><span data-stu-id="2e1de-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="2e1de-109">[Statistik][Statistics]</span><span class="sxs-lookup"><span data-stu-id="2e1de-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="2e1de-110">[Tillfällig][Temporary]</span><span class="sxs-lookup"><span data-stu-id="2e1de-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="2e1de-111">hello mer SQL Data Warehouse medveten om dina data hello snabbare den kan köra frågor mot dina data.</span><span class="sxs-lookup"><span data-stu-id="2e1de-111">hello more SQL Data Warehouse knows about your data, hello faster it can execute queries against your data.</span></span>  <span data-ttu-id="2e1de-112">hello sätt som du anger SQL Data Warehouse om dina data är genom att samla in statistik om dina data.</span><span class="sxs-lookup"><span data-stu-id="2e1de-112">hello way that you tell SQL Data Warehouse about your data, is by collecting statistics about your data.</span></span>  <span data-ttu-id="2e1de-113">Med statistik om dina data är en av hello viktigaste saker du kan göra toooptimize dina frågor.</span><span class="sxs-lookup"><span data-stu-id="2e1de-113">Having statistics on your data is one of hello most important things you can do toooptimize your queries.</span></span>  <span data-ttu-id="2e1de-114">Statistik kan SQL Data Warehouse skapar hello mest optimala plan för dina frågor.</span><span class="sxs-lookup"><span data-stu-id="2e1de-114">Statistics help SQL Data Warehouse create hello most optimal plan for your queries.</span></span>  <span data-ttu-id="2e1de-115">Det beror på att hello SQL Data Warehouse frågan optimering är en kostnad baserat optimering.</span><span class="sxs-lookup"><span data-stu-id="2e1de-115">This is because hello SQL Data Warehouse query optimizer is a cost based optimizer.</span></span>  <span data-ttu-id="2e1de-116">Det vill säga jämför hello kostnaden för olika frågeplaner och väljer hello plan med hello lägsta kostnad som bör också vara hello-plan som utför hello snabbaste.</span><span class="sxs-lookup"><span data-stu-id="2e1de-116">That is, it compares hello cost of various query plans and then chooses hello plan with hello lowest cost, which should also be hello plan that will execute hello fastest.</span></span>

<span data-ttu-id="2e1de-117">Statistik kan skapas på en kolumn, flera kolumner eller i ett index i en tabell.</span><span class="sxs-lookup"><span data-stu-id="2e1de-117">Statistics can be created on a single column, multiple columns or on an index of a table.</span></span>  <span data-ttu-id="2e1de-118">Statistik lagras i ett histogram som samlar in hello intervall och selektivitet värden.</span><span class="sxs-lookup"><span data-stu-id="2e1de-118">Statistics are stored in a histogram which captures hello range and selectivity of values.</span></span>  <span data-ttu-id="2e1de-119">Detta är särskilt intressanta när hello optimering behöver tooevaluate kopplingar, GROUP BY, HAVING och WHERE-satser i en fråga.</span><span class="sxs-lookup"><span data-stu-id="2e1de-119">This is of particular interest when hello optimizer needs tooevaluate JOINs, GROUP BY, HAVING and WHERE clauses in a query.</span></span>  <span data-ttu-id="2e1de-120">Till exempel hello optimering uppskattar att hello datum du filtrerar i frågan returnerar 1 rad, det kan välja väldigt annorlunda planera än om den beräknar de datum du har valt att returnera 1 miljon rader.</span><span class="sxs-lookup"><span data-stu-id="2e1de-120">For example, if hello optimizer estimates that hello date you are filtering in your query will return 1 row, it may choose a very different plan than if it estimates that they date you have selected will return 1 million rows.</span></span>  <span data-ttu-id="2e1de-121">Även om det är mycket viktigt att statistik skapas, är det lika viktigt att statistik *korrekt* återge hello nuvarande hello tabell.</span><span class="sxs-lookup"><span data-stu-id="2e1de-121">While creating statistics is extremely important, it is equally important that statistics *accurately* reflect hello current state of hello table.</span></span>  <span data-ttu-id="2e1de-122">Uppdaterad statistik försäkrar du dig om att en bra plan är markerade som hello optimering.</span><span class="sxs-lookup"><span data-stu-id="2e1de-122">Having up-to-date statistics ensures that a good plan is selected by hello optimizer.</span></span>  <span data-ttu-id="2e1de-123">hello planer som skapats av hello optimering är bättre hello statistik på dina data.</span><span class="sxs-lookup"><span data-stu-id="2e1de-123">hello plans created by hello optimizer are only as good as hello statistics on your data.</span></span>

<span data-ttu-id="2e1de-124">hello-processen för att skapa och uppdatera statistik för närvarande är en manuell process, men är väldigt enkelt toodo.</span><span class="sxs-lookup"><span data-stu-id="2e1de-124">hello process of creating and updating statistics is currently a manual process, but is very simple toodo.</span></span>  <span data-ttu-id="2e1de-125">Detta skiljer sig från SQL Server som automatiskt skapar och uppdaterar statistik på enskild kolumner och index.</span><span class="sxs-lookup"><span data-stu-id="2e1de-125">This is unlike SQL Server which automatically creates and updates statistics on single columns and indexes.</span></span>  <span data-ttu-id="2e1de-126">Du kan avsevärt automatisera hello hantering av hello statistik på dina data med hjälp av hello informationen nedan.</span><span class="sxs-lookup"><span data-stu-id="2e1de-126">By using hello information below, you can greatly automate hello management of hello statistics on your data.</span></span> 

## <a name="getting-started-with-statistics"></a><span data-ttu-id="2e1de-127">Komma igång med statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-127">Getting started with statistics</span></span>
 <span data-ttu-id="2e1de-128">Provade statistik skapas för varje kolumn är ett enkelt sätt tooget igång med statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-128">Creating sampled statistics on every column is an easy way tooget started with statistics.</span></span>  <span data-ttu-id="2e1de-129">Eftersom det är lika viktigt tookeep statistik uppdaterade, en försiktig metod kan vara tooupdate statistiken varje dag eller efter varje inläsningen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-129">Since it is equally important tookeep statistics up-to-date, a conservative approach may be tooupdate your statistics daily or after each load.</span></span> <span data-ttu-id="2e1de-130">Det finns alltid avvägningarna mellan prestanda och hello kostnaden toocreate och uppdatera statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-130">There are always trade-offs between performance and hello cost toocreate and update statistics.</span></span>  <span data-ttu-id="2e1de-131">Om du hittar det tar för lång toomaintain alla din statistik kan kanske du vill tootry toobe mer noga med vilka kolumner har statistik eller vilka kolumner som behöver uppdateras regelbundet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-131">If you find it is taking too long toomaintain all of your statistics, you may want tootry toobe more selective about which columns have statistics or which columns need frequent updating.</span></span>  <span data-ttu-id="2e1de-132">Du kan till exempel vill tooupdate datumkolumnerna dagliga som nya värden kan läggas till i stället för efter varje inläsningen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-132">For example, you might want tooupdate date columns daily, as new values may be added rather than after every load.</span></span> <span data-ttu-id="2e1de-133">Igen och du kommer att få hello utnyttja genom att låta statistik för kolumner som ingår i kopplingar, GROUP BY, HAVING och WHERE-satser.</span><span class="sxs-lookup"><span data-stu-id="2e1de-133">Again, you will gain hello most benefit by having statistics on columns involved in JOINs, GROUP BY, HAVING and WHERE clauses.</span></span>  <span data-ttu-id="2e1de-134">Om du har en tabell med många kolumner som används endast i hello Markera satsen statistik på dessa kolumner kan inte hjälpa och utgifter lite mer arbete tooidentify endast hello kolumner där statistik hjälper kan minska hello tid toomaintain din statistik .</span><span class="sxs-lookup"><span data-stu-id="2e1de-134">If you have a table with a lot of columns which are only used in hello SELECT clause, statistics on these columns may not help, and spending a little more effort tooidentify only hello columns where statistics will help, can reduce hello time toomaintain your statistics.</span></span>

## <a name="multi-column-statistics"></a><span data-ttu-id="2e1de-135">Flera kolumnstatistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-135">Multi-column statistics</span></span>
<span data-ttu-id="2e1de-136">Dessutom toocreating statistik på enskild kolumner, kan vara att dina frågor att dra nytta av flera kolumnstatistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-136">In addition toocreating statistics on single columns, you may find that your queries will benefit from multi-column statistics.</span></span>  <span data-ttu-id="2e1de-137">Flera kolumnstatistik är statistik skapas på en lista med kolumner.</span><span class="sxs-lookup"><span data-stu-id="2e1de-137">Multi-column statistics are statistics created on a list of columns.</span></span>  <span data-ttu-id="2e1de-138">De inkluderar kolumnstatistik hello första kolumnen i hello lista, plus vissa mellan kolumnen korrelation information kallas densitet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-138">They include single column statistics on hello first column in hello list, plus some cross-column correlation information called densities.</span></span>  <span data-ttu-id="2e1de-139">Till exempel om du har en tabell som ansluter till tooanother på två kolumner kan vara att SQL Data Warehouse bättre kan optimera hello planen om den förstår hello relationen mellan två kolumner.</span><span class="sxs-lookup"><span data-stu-id="2e1de-139">For example, if you have a table that joins tooanother on two columns, you may find that SQL Data Warehouse can better optimize hello plan if it understands hello relationship between two columns.</span></span>   <span data-ttu-id="2e1de-140">Flera kolumnstatistik kan förbättra frågeprestanda för vissa åtgärder, till exempel sammansatta kopplingar och gruppera efter.</span><span class="sxs-lookup"><span data-stu-id="2e1de-140">Multi-column statistics can improve query performance for some operations such as composite joins and group by.</span></span>

## <a name="updating-statistics"></a><span data-ttu-id="2e1de-141">Uppdatera statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-141">Updating statistics</span></span>
<span data-ttu-id="2e1de-142">Uppdaterar statistiken är en viktig del av din databas management rutinen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-142">Updating statistics is an important part of your database management routine.</span></span>  <span data-ttu-id="2e1de-143">När hello fördelning av data i hello-databasen ändras, måste statistik toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="2e1de-143">When hello distribution of data in hello database changes, statistics need toobe updated.</span></span>  <span data-ttu-id="2e1de-144">Inaktuella statistik leder toosub optimala frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="2e1de-144">Out-of-date statistics will lead toosub-optimal query performance.</span></span>

<span data-ttu-id="2e1de-145">Ett bra tips är tooupdate statistik över datumkolumnerna varje dag som nya datum har lagts till.</span><span class="sxs-lookup"><span data-stu-id="2e1de-145">One best practice is tooupdate statistics on date columns each day as new dates are added.</span></span>  <span data-ttu-id="2e1de-146">Varje gång nya rader läses in i datalagret hello, nya belastningen datum eller datum har lagts till.</span><span class="sxs-lookup"><span data-stu-id="2e1de-146">Each time new rows are loaded into hello data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="2e1de-147">Dessa ändra hello datadistribution och se hello statistik inaktuella.</span><span class="sxs-lookup"><span data-stu-id="2e1de-147">These change hello data distribution and make hello statistics out-of-date.</span></span> <span data-ttu-id="2e1de-148">Statistik i ett land-kolumn i en kundtabell behöva däremot aldrig toobe uppdateras, som vanligtvis inte ändras hello fördelningen av värden.</span><span class="sxs-lookup"><span data-stu-id="2e1de-148">Conversely, statistics on a country column in a customer table might never need toobe updated, as hello distribution of values doesn’t generally change.</span></span> <span data-ttu-id="2e1de-149">Under förutsättning att hello distribution är konstant mellan kunder, enkelt lägga till nya rader toohello tabell variation toochange hello Datadistribution.</span><span class="sxs-lookup"><span data-stu-id="2e1de-149">Assuming hello distribution is constant between customers, adding new rows toohello table variation isn't going toochange hello data distribution.</span></span> <span data-ttu-id="2e1de-150">Dock om ditt data warehouse endast innehåller ett land och du hämta data från ett nytt land, måste vilket resulterar i data från flera länder lagras, definitivt tooupdate statistik hello land kolumn.</span><span class="sxs-lookup"><span data-stu-id="2e1de-150">However, if your data warehouse only contains one country and you bring in data from a new country, resulting in data from multiple countries being stored, then you definitely need tooupdate statistics on hello country column.</span></span>

<span data-ttu-id="2e1de-151">En av hello första frågor tooask när felsökning av en fråga är ”är hello statistik uppdaterade”?</span><span class="sxs-lookup"><span data-stu-id="2e1de-151">One of hello first questions tooask when troubleshooting a query is, "Are hello statistics up-to-date?"</span></span>

<span data-ttu-id="2e1de-152">Den här frågan är inte en som kan betjänas av hello åldern på hello data.</span><span class="sxs-lookup"><span data-stu-id="2e1de-152">This question is not one that can be answered by hello age of hello data.</span></span> <span data-ttu-id="2e1de-153">Ett dig toodate statistik objekt kan vara mycket gamla om det har förekommit några väsentlig förändring toohello underliggande data.</span><span class="sxs-lookup"><span data-stu-id="2e1de-153">An up toodate statistics object could be very old if there's been no material change toohello underlying data.</span></span> <span data-ttu-id="2e1de-154">När hello antalet rader som har ändrats betydligt eller så finns det en väsentlig förändring i hello fördelning av värden för en viss kolumn *sedan* är det tooupdate statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-154">When hello number of rows has changed substantially or there is a material change in hello distribution of values for a given column *then* it's time tooupdate statistics.</span></span>  

<span data-ttu-id="2e1de-155">Referens **SQL Server** (inte SQL Data Warehouse) uppdateras automatiskt statistik för dessa situationer:</span><span class="sxs-lookup"><span data-stu-id="2e1de-155">For reference, **SQL Server** (not SQL Data Warehouse) automatically updates statistics for these situations:</span></span>

* <span data-ttu-id="2e1de-156">Om du har noll rader i tabellen hello, när du lägger till rader, får du en automatisk uppdatering av statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-156">If you have zero rows in hello table, when you add rows, you’ll get an automatic update of statistics</span></span>
* <span data-ttu-id="2e1de-157">När du lägger till mer än 500 rader tooa tabell från och med färre än 500 rader (t.ex. vid start du har 499 och Lägg sedan till 500 rader tooa totalt 999 rader), får du en automatisk uppdatering</span><span class="sxs-lookup"><span data-stu-id="2e1de-157">When you add more than 500 rows tooa table starting with less than 500 rows (e.g. at start you have 499 and then add 500 rows tooa total of 999 rows), you’ll get an automatic update</span></span> 
* <span data-ttu-id="2e1de-158">När du är än 500 rader har du tooadd 500 ytterligare rader + 20% av hello storlek hello tabell innan du ser en automatisk uppdatering på hello statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-158">Once you’re over 500 rows you will have tooadd 500 additional rows + 20% of hello size of hello table before you’ll see an automatic update on hello stats</span></span>

<span data-ttu-id="2e1de-159">Eftersom det inte finns några DMV toodetermine om data i hello tabellen har ändrats sedan hello senaste statistik har uppdaterats, kan veta hello ålder på din statistik förse dig med en del av hello bild.</span><span class="sxs-lookup"><span data-stu-id="2e1de-159">Since there is no DMV toodetermine if data within hello table has changed since hello last time statistics were updated, knowing hello age of your statistics can provide you with part of hello picture.</span></span>  <span data-ttu-id="2e1de-160">Du kan använda följande fråga toodetermine hello senast statistiken hello där uppdateras vid varje tabell.</span><span class="sxs-lookup"><span data-stu-id="2e1de-160">You can use hello following query toodetermine hello last time your statistics where updated on each table.</span></span>  

> [!NOTE]
> <span data-ttu-id="2e1de-161">Kom ihåg att om det finns en väsentlig förändring i hello fördelning av värden för en kolumn, bör du uppdatera statistik oavsett hello senast som de har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="2e1de-161">Remember if there is a material change in hello distribution of values for a given column, you should update statistics regardless of hello last time they were updated.</span></span>  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

<span data-ttu-id="2e1de-162">Datumkolumnerna i datalagret, till exempel måste vanligtvis frekventa uppdateringar av statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-162">Date columns in a data warehouse, for example, usually need frequent statistics updates.</span></span> <span data-ttu-id="2e1de-163">Varje gång nya rader läses in i datalagret hello, nya belastningen datum eller datum har lagts till.</span><span class="sxs-lookup"><span data-stu-id="2e1de-163">Each time new rows are loaded into hello data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="2e1de-164">Dessa ändra hello datadistribution och se hello statistik inaktuella.</span><span class="sxs-lookup"><span data-stu-id="2e1de-164">These change hello data distribution and make hello statistics out-of-date.</span></span>  <span data-ttu-id="2e1de-165">Däremot kanske aldrig statistik på kolumnen kön för en kundtabell som toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="2e1de-165">Conversely, statistics on a gender column on a customer table might never need toobe updated.</span></span> <span data-ttu-id="2e1de-166">Under förutsättning att hello distribution är konstant mellan kunder, enkelt lägga till nya rader toohello tabell variation toochange hello Datadistribution.</span><span class="sxs-lookup"><span data-stu-id="2e1de-166">Assuming hello distribution is constant between customers, adding new rows toohello table variation isn't going toochange hello data distribution.</span></span> <span data-ttu-id="2e1de-167">Men om ditt data warehouse bara innehåller en kön och en ny krav resulterar i flera könen måste definitivt tooupdate statistik hello kolumnen för kön.</span><span class="sxs-lookup"><span data-stu-id="2e1de-167">However, if your data warehouse only contains one gender and a new requirement results in multiple genders then you definitely need tooupdate statistics on hello gender column.</span></span>

<span data-ttu-id="2e1de-168">Ytterligare förklaring finns [statistik] [ Statistics] på MSDN.</span><span class="sxs-lookup"><span data-stu-id="2e1de-168">For further explanation, see [Statistics][Statistics] on MSDN.</span></span>

## <a name="implementing-statistics-management"></a><span data-ttu-id="2e1de-169">Implementera hantering av statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-169">Implementing statistics management</span></span>
<span data-ttu-id="2e1de-170">Det är ofta en bra idé tooextend din datainläsning processen tooensure som uppdateras vid hello slutet av hello belastningen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-170">It is often a good idea tooextend your data loading process tooensure that statistics are updated at hello end of hello load.</span></span> <span data-ttu-id="2e1de-171">hello datainläsning är när tabeller ändras oftast deras storlek och/eller distribution av värden.</span><span class="sxs-lookup"><span data-stu-id="2e1de-171">hello data load is when tables most frequently change their size and/or their distribution of values.</span></span> <span data-ttu-id="2e1de-172">Därför är detta en logisk plats tooimplement vissa hanteringsprocesser.</span><span class="sxs-lookup"><span data-stu-id="2e1de-172">Therefore, this is a logical place tooimplement some management processes.</span></span>

<span data-ttu-id="2e1de-173">Vissa riktlinjerna tillhandahålls nedan för att uppdatera statistiken under inläsningen hello:</span><span class="sxs-lookup"><span data-stu-id="2e1de-173">Some guiding principles are provided below for updating your statistics during hello load process:</span></span>

* <span data-ttu-id="2e1de-174">Kontrollera att varje inlästa tabell har minst ett statistik objekt uppdateras.</span><span class="sxs-lookup"><span data-stu-id="2e1de-174">Ensure that each loaded table has at least one statistics object updated.</span></span> <span data-ttu-id="2e1de-175">Den här uppdateringar hello tabeller (radantal och antal sidor) Storleksinformation som en del av hello stats uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-175">This updates hello tables size (row count and page count) information as part of hello stats update.</span></span>
* <span data-ttu-id="2e1de-176">Fokusera på kolumner som ingår i koppling, GROUP BY, ORDER BY och DISTINCT-satser</span><span class="sxs-lookup"><span data-stu-id="2e1de-176">Focus on columns participating in JOIN, GROUP BY, ORDER BY and DISTINCT clauses</span></span>
* <span data-ttu-id="2e1de-177">Överväg att uppdatera ”stigande nyckeln” kolumner som transaktionen datum oftare, eftersom dessa värden inte inkluderas i hello statistik histogram.</span><span class="sxs-lookup"><span data-stu-id="2e1de-177">Consider updating "ascending key" columns such as transaction dates more frequently as these values will not be included in hello statistics histogram.</span></span>
* <span data-ttu-id="2e1de-178">Överväg att uppdatera statiska distributionskolumner mindre ofta.</span><span class="sxs-lookup"><span data-stu-id="2e1de-178">Consider updating static distribution columns less frequently.</span></span>
* <span data-ttu-id="2e1de-179">Kom ihåg att varje statistik objekt uppdateras i serien.</span><span class="sxs-lookup"><span data-stu-id="2e1de-179">Remember each statistic object is updated in series.</span></span> <span data-ttu-id="2e1de-180">Bara implementera `UPDATE STATISTICS <TABLE_NAME>` kanske inte är optimal - särskilt för många tabeller med många statistik objekt.</span><span class="sxs-lookup"><span data-stu-id="2e1de-180">Simply implementing `UPDATE STATISTICS <TABLE_NAME>` may not be ideal - especially for wide tables with lots of statistics objects.</span></span>

> [!NOTE]
> <span data-ttu-id="2e1de-181">Mer information om [stigande nyckeln] finns toohello SQL Server 2014 kardinalitet beräkning av modellen vitbok.</span><span class="sxs-lookup"><span data-stu-id="2e1de-181">For more details on [ascending key] please refer toohello SQL Server 2014 cardinality estimation model whitepaper.</span></span>
> 
> 

<span data-ttu-id="2e1de-182">Ytterligare förklaring finns [kardinalitet uppskattning] [ Cardinality Estimation] på MSDN.</span><span class="sxs-lookup"><span data-stu-id="2e1de-182">For further explanation, see  [Cardinality Estimation][Cardinality Estimation] on MSDN.</span></span>

## <a name="examples-create-statistics"></a><span data-ttu-id="2e1de-183">Exempel: Skapa statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-183">Examples: Create statistics</span></span>
<span data-ttu-id="2e1de-184">Följande exempel visar hur toouse olika alternativ för att skapa statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-184">These examples show how toouse various options for creating statistics.</span></span> <span data-ttu-id="2e1de-185">hello-alternativ som du använder för varje kolumn är beroende av hello egenskaper för dina data och hur hello kolumnen kommer att användas i frågor.</span><span class="sxs-lookup"><span data-stu-id="2e1de-185">hello options that you use for each column depend on hello characteristics of your data and how hello column will be used in queries.</span></span>

### <a name="a-create-single-column-statistics-with-default-options"></a><span data-ttu-id="2e1de-186">A.</span><span class="sxs-lookup"><span data-stu-id="2e1de-186">A.</span></span> <span data-ttu-id="2e1de-187">Skapa statistik för en kolumn med standardalternativ</span><span class="sxs-lookup"><span data-stu-id="2e1de-187">Create single-column statistics with default options</span></span>
<span data-ttu-id="2e1de-188">toocreate statistik om en kolumn, ange bara ett namn för hello statistik objektet och hello hello kolumnens namn.</span><span class="sxs-lookup"><span data-stu-id="2e1de-188">toocreate statistics on a column, simply provide a name for hello statistics object and hello name of hello column.</span></span>

<span data-ttu-id="2e1de-189">Den här syntaxen använder alla hello standardalternativ.</span><span class="sxs-lookup"><span data-stu-id="2e1de-189">This syntax uses all of hello default options.</span></span> <span data-ttu-id="2e1de-190">Som standard prover SQL Data Warehouse 20 procent av hello tabellen när den skapar statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-190">By default, SQL Data Warehouse samples 20 percent of hello table when it creates statistics.</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

<span data-ttu-id="2e1de-191">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2e1de-191">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a><span data-ttu-id="2e1de-192">B.</span><span class="sxs-lookup"><span data-stu-id="2e1de-192">B.</span></span> <span data-ttu-id="2e1de-193">Skapa enkolumns-statistik genom att undersöka varje rad</span><span class="sxs-lookup"><span data-stu-id="2e1de-193">Create single-column statistics by examining every row</span></span>
<span data-ttu-id="2e1de-194">hello standard samplingsfrekvensen 20 procent räcker för de flesta situationer.</span><span class="sxs-lookup"><span data-stu-id="2e1de-194">hello default sampling rate of 20 percent is sufficient for most situations.</span></span> <span data-ttu-id="2e1de-195">Du kan dock justera samplingsfrekvensen hello.</span><span class="sxs-lookup"><span data-stu-id="2e1de-195">However, you can adjust hello sampling rate.</span></span>

<span data-ttu-id="2e1de-196">toosample hello fullständig tabell, använder du följande syntax:</span><span class="sxs-lookup"><span data-stu-id="2e1de-196">toosample hello full table, use this syntax:</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

<span data-ttu-id="2e1de-197">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2e1de-197">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-hello-sample-size"></a><span data-ttu-id="2e1de-198">C.</span><span class="sxs-lookup"><span data-stu-id="2e1de-198">C.</span></span> <span data-ttu-id="2e1de-199">Skapa enkolumns-statistik genom att ange hello provtagning</span><span class="sxs-lookup"><span data-stu-id="2e1de-199">Create single-column statistics by specifying hello sample size</span></span>
<span data-ttu-id="2e1de-200">Du kan också ange hello provtagning procent:</span><span class="sxs-lookup"><span data-stu-id="2e1de-200">Alternatively, you can specify hello sample size as a percent:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-hello-rows"></a><span data-ttu-id="2e1de-201">D.</span><span class="sxs-lookup"><span data-stu-id="2e1de-201">D.</span></span> <span data-ttu-id="2e1de-202">Skapa enkolumns-statistik på endast en del av hello rader</span><span class="sxs-lookup"><span data-stu-id="2e1de-202">Create single-column statistics on only some of hello rows</span></span>
<span data-ttu-id="2e1de-203">Ett annat alternativ du kan skapa statistik på en del av hello rader i tabellen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-203">Another option, you can create statistics on a portion of hello rows in your table.</span></span> <span data-ttu-id="2e1de-204">Detta kallas en filtrerad statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-204">This is called a filtered statistic.</span></span>

<span data-ttu-id="2e1de-205">Du kan till exempel använda filtrerad statistik när du planerar tooquery en specifik partition för en stor partitionerad tabell.</span><span class="sxs-lookup"><span data-stu-id="2e1de-205">For example, you could use filtered statistics when you plan tooquery a specific partition of a large partitioned table.</span></span> <span data-ttu-id="2e1de-206">Genom att skapa statistik på endast hello partition värden, kommer hello riktighet hello statistik förbättra och därför förbättra frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="2e1de-206">By creating statistics on only hello partition values, hello accuracy of hello statistics will improve, and therefore improve query performance.</span></span>

<span data-ttu-id="2e1de-207">Det här exemplet skapar statistik på ett intervall med värden.</span><span class="sxs-lookup"><span data-stu-id="2e1de-207">This example creates statistics on a range of values.</span></span> <span data-ttu-id="2e1de-208">hello-värden kan enkelt definierats toomatch hello värdeintervallet i en partition.</span><span class="sxs-lookup"><span data-stu-id="2e1de-208">hello values could easily be defined toomatch hello range of values in a partition.</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> <span data-ttu-id="2e1de-209">För hello frågan optimering tooconsider använder filtrerad statistik när de väljer hello distribuerade frågeplan, hello frågan måste få plats inuti hello definition av hello statistik objekt.</span><span class="sxs-lookup"><span data-stu-id="2e1de-209">For hello query optimizer tooconsider using filtered statistics when it chooses hello distributed query plan, hello query must fit inside hello definition of hello statistics object.</span></span> <span data-ttu-id="2e1de-210">Med hjälp av hello föregående exempel hello frågans där sats måste toospecify Kol1 värden mellan 2000101 och 20001231.</span><span class="sxs-lookup"><span data-stu-id="2e1de-210">Using hello previous example, hello query's where clause needs toospecify col1 values between 2000101 and 20001231.</span></span>
> 
> 

### <a name="e-create-single-column-statistics-with-all-hello-options"></a><span data-ttu-id="2e1de-211">E.</span><span class="sxs-lookup"><span data-stu-id="2e1de-211">E.</span></span> <span data-ttu-id="2e1de-212">Skapa enkolumns-statistik med alla hello-alternativ</span><span class="sxs-lookup"><span data-stu-id="2e1de-212">Create single-column statistics with all hello options</span></span>
<span data-ttu-id="2e1de-213">Du kan självklart kombinera hello alternativ tillsammans.</span><span class="sxs-lookup"><span data-stu-id="2e1de-213">You can, of course, combine hello options together.</span></span> <span data-ttu-id="2e1de-214">hello exemplet nedan skapar en filtrerad statistik-objekt med en anpassad provtagning:</span><span class="sxs-lookup"><span data-stu-id="2e1de-214">hello example below creates a filtered statistics object with a custom sample size:</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="2e1de-215">Hello fullständiga referenser finns [CREATE STATISTICS] [ CREATE STATISTICS] på MSDN.</span><span class="sxs-lookup"><span data-stu-id="2e1de-215">For hello full reference, see [CREATE STATISTICS][CREATE STATISTICS] on MSDN.</span></span>

### <a name="f-create-multi-column-statistics"></a><span data-ttu-id="2e1de-216">F.</span><span class="sxs-lookup"><span data-stu-id="2e1de-216">F.</span></span> <span data-ttu-id="2e1de-217">Skapa flera kolumner statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-217">Create multi-column statistics</span></span>
<span data-ttu-id="2e1de-218">toocreate flera kolumnstatistik, kan du bara använda hello föregående exempel, men ange fler kolumner.</span><span class="sxs-lookup"><span data-stu-id="2e1de-218">toocreate a multi-column statistics, simply use hello previous examples, but specify more columns.</span></span>

> [!NOTE]
> <span data-ttu-id="2e1de-219">hello histogram som används för tooestimate antalet rader i hello frågeresultat är endast tillgängligt för hello första kolumnen i hello statistik objektdefinition.</span><span class="sxs-lookup"><span data-stu-id="2e1de-219">hello histogram, which is used tooestimate number of rows in hello query result, is only available for hello first column listed in hello statistics object definition.</span></span>
> 
> 

<span data-ttu-id="2e1de-220">I det här exemplet hello histogram finns på *produkten\_kategori*.</span><span class="sxs-lookup"><span data-stu-id="2e1de-220">In this example, hello histogram is on *product\_category*.</span></span> <span data-ttu-id="2e1de-221">Statistik för Cross-kolumnen beräknas på *produkten\_kategori* och *produkten\_sub_c\ategory*:</span><span class="sxs-lookup"><span data-stu-id="2e1de-221">Cross-column statistics are calculated on *product\_category* and *product\_sub_c\ategory*:</span></span>

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="2e1de-222">Eftersom det inte finns en korrelation mellan *produkten\_kategori* och *produkten\_sub\_kategori*, en flera kolumner kan vara användbart om de här kolumnerna kan nås vid hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="2e1de-222">Since there is a correlation between *product\_category* and *product\_sub\_category*, a multi-column stat can be useful if these columns are accessed at hello same time.</span></span>

### <a name="g-create-statistics-on-all-hello-columns-in-a-table"></a><span data-ttu-id="2e1de-223">G.</span><span class="sxs-lookup"><span data-stu-id="2e1de-223">G.</span></span> <span data-ttu-id="2e1de-224">Skapa statistik på alla hello kolumner i en tabell</span><span class="sxs-lookup"><span data-stu-id="2e1de-224">Create statistics on all hello columns in a table</span></span>
<span data-ttu-id="2e1de-225">Enkelriktade toocreate statistik är tooissues CREATE STATISTICS kommandon när du har skapat hello tabell.</span><span class="sxs-lookup"><span data-stu-id="2e1de-225">One way toocreate statistics is tooissues CREATE STATISTICS commands after creating hello table.</span></span>

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-toocreate-statistics-on-all-columns-in-a-database"></a><span data-ttu-id="2e1de-226">H.</span><span class="sxs-lookup"><span data-stu-id="2e1de-226">H.</span></span> <span data-ttu-id="2e1de-227">Använda en lagrad procedur toocreate statistik på alla kolumner i en databas</span><span class="sxs-lookup"><span data-stu-id="2e1de-227">Use a stored procedure toocreate statistics on all columns in a database</span></span>
<span data-ttu-id="2e1de-228">SQL Data Warehouse inte har en motsvarighet på system lagrade proceduren för [sp_create_stats] [] i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2e1de-228">SQL Data Warehouse does not have a system stored procedure equivalent too[sp_create_stats][] in SQL Server.</span></span> <span data-ttu-id="2e1de-229">Den här lagrade proceduren skapar en kolumn statistik-objekt på varje kolumn i hello-databas som inte redan har statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-229">This stored procedure creates a single column statistics object on every column of hello database that doesn't already have statistics.</span></span>

<span data-ttu-id="2e1de-230">Detta hjälper dig att komma igång med din databasdesign.</span><span class="sxs-lookup"><span data-stu-id="2e1de-230">This will help you get started with your database design.</span></span> <span data-ttu-id="2e1de-231">Känna sig fria tooadapt den tooyour måste.</span><span class="sxs-lookup"><span data-stu-id="2e1de-231">Feel free tooadapt it tooyour needs.</span></span>

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

<span data-ttu-id="2e1de-232">toocreate statistik på alla kolumner i tabellen hello med den här proceduren anropa bara hello-procedur.</span><span class="sxs-lookup"><span data-stu-id="2e1de-232">toocreate statistics on all columns in hello table with this procedure, simply call hello procedure.</span></span>

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a><span data-ttu-id="2e1de-233">Exempel: uppdatera statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-233">Examples: update statistics</span></span>
<span data-ttu-id="2e1de-234">tooupdate statistik, kan du:</span><span class="sxs-lookup"><span data-stu-id="2e1de-234">tooupdate statistics, you can:</span></span>

1. <span data-ttu-id="2e1de-235">Uppdatera ett objekt av statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-235">Update one statistics object.</span></span> <span data-ttu-id="2e1de-236">Ange hello statistik objekt du vill tooupdate hello namn.</span><span class="sxs-lookup"><span data-stu-id="2e1de-236">Specify hello name of hello statistics object you wish tooupdate.</span></span>
2. <span data-ttu-id="2e1de-237">Uppdatera alla statistik objekt i en tabell.</span><span class="sxs-lookup"><span data-stu-id="2e1de-237">Update all statistics objects on a table.</span></span> <span data-ttu-id="2e1de-238">Ange hello namn hello tabellen i stället för en viss statistik-objektet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-238">Specify hello name of hello table instead of one specific statistics object.</span></span>

### <a name="a-update-one-specific-statistics-object"></a><span data-ttu-id="2e1de-239">A.</span><span class="sxs-lookup"><span data-stu-id="2e1de-239">A.</span></span> <span data-ttu-id="2e1de-240">Uppdatera en specifik statistik objekt</span><span class="sxs-lookup"><span data-stu-id="2e1de-240">Update one specific statistics object</span></span>
<span data-ttu-id="2e1de-241">Använd hello följande syntax tooupdate en viss statistik-objekt:</span><span class="sxs-lookup"><span data-stu-id="2e1de-241">Use hello following syntax tooupdate a specific statistics object:</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

<span data-ttu-id="2e1de-242">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2e1de-242">For example:</span></span>

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

<span data-ttu-id="2e1de-243">Du kan minimera hello tid och resurser krävs toomanage statistik genom att uppdatera statistik för specifika objekt.</span><span class="sxs-lookup"><span data-stu-id="2e1de-243">By updating specific statistics objects, you can minimize hello time and resources required toomanage statistics.</span></span> <span data-ttu-id="2e1de-244">Detta kräver vissa tänkte, men toochoose hello bästa statistik objekt tooupdate.</span><span class="sxs-lookup"><span data-stu-id="2e1de-244">This requires some thought, though, toochoose hello best statistics objects tooupdate.</span></span>

### <a name="b-update-all-statistics-on-a-table"></a><span data-ttu-id="2e1de-245">B.</span><span class="sxs-lookup"><span data-stu-id="2e1de-245">B.</span></span> <span data-ttu-id="2e1de-246">Uppdatera all statistik för en tabell</span><span class="sxs-lookup"><span data-stu-id="2e1de-246">Update all statistics on a table</span></span>
<span data-ttu-id="2e1de-247">Detta visar en enkel metod för att uppdatera alla hello statistik objekt i en tabell.</span><span class="sxs-lookup"><span data-stu-id="2e1de-247">This shows a simple method for updating all hello statistics objects on a table.</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

<span data-ttu-id="2e1de-248">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2e1de-248">For example:</span></span>

```sql
UPDATE STATISTICS dbo.table1;
```

<span data-ttu-id="2e1de-249">Den här instruktionen är enkelt toouse.</span><span class="sxs-lookup"><span data-stu-id="2e1de-249">This statement is easy toouse.</span></span> <span data-ttu-id="2e1de-250">Kom ihåg detta uppdaterar all statistik för hello tabellen och därför kan utföra mer arbete än nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2e1de-250">Just remember this updates all statistics on hello table, and therefore might perform more work than is necessary.</span></span> <span data-ttu-id="2e1de-251">Om hello prestanda inte är ett problem, men det är definitivt hello enklaste och sätt tooguarantee statistik är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="2e1de-251">If hello performance is not an issue, this is definitely hello easiest and most complete way tooguarantee statistics are up-to-date.</span></span>

> [!NOTE]
> <span data-ttu-id="2e1de-252">När du uppdaterar all statistik för en tabell, stöder SQL Data Warehouse en genomsökning toosample hello tabell för varje statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-252">When updating all statistics on a table, SQL Data Warehouse does a scan toosample hello table for each statistics.</span></span> <span data-ttu-id="2e1de-253">Om hello tabell är stort, har många kolumner och många statistik, kan det vara effektivare tooupdate-enskilda statistik baserat på behov.</span><span class="sxs-lookup"><span data-stu-id="2e1de-253">If hello table is large, has many columns, and many statistics, it might be more efficient tooupdate individual statistics based on need.</span></span>
> 
> 

<span data-ttu-id="2e1de-254">För en implementering av en `UPDATE STATISTICS` proceduren finns hello [temporära tabeller] [ Temporary] artikel.</span><span class="sxs-lookup"><span data-stu-id="2e1de-254">For an implementation of an `UPDATE STATISTICS` procedure please see hello [Temporary Tables][Temporary] article.</span></span> <span data-ttu-id="2e1de-255">hello implementering metoden är något annorlunda toohello `CREATE STATISTICS` proceduren ovan men hello slutresultatet är hello samma.</span><span class="sxs-lookup"><span data-stu-id="2e1de-255">hello implementation method is slightly different toohello `CREATE STATISTICS` procedure above but hello end result is hello same.</span></span>

<span data-ttu-id="2e1de-256">Hello fullständig syntax, se [Update Statistics] [ Update Statistics] på MSDN.</span><span class="sxs-lookup"><span data-stu-id="2e1de-256">For hello full syntax, see [Update Statistics][Update Statistics] on MSDN.</span></span>

## <a name="statistics-metadata"></a><span data-ttu-id="2e1de-257">Statistik metadata</span><span class="sxs-lookup"><span data-stu-id="2e1de-257">Statistics metadata</span></span>
<span data-ttu-id="2e1de-258">Det finns flera systemvy och funktioner som du kan använda toofind information om statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-258">There are several system view and functions that you can use toofind information about statistics.</span></span> <span data-ttu-id="2e1de-259">Du kan till exempel se om ett statistik-objekt kan vara inaktuell med hello stats datum funktionen toosee när statistiken skapades eller uppdaterades senast.</span><span class="sxs-lookup"><span data-stu-id="2e1de-259">For example, you can see if a statistics object might be out-of-date by using hello stats-date function toosee when statistics were last created or updated.</span></span>

### <a name="catalog-views-for-statistics"></a><span data-ttu-id="2e1de-260">Katalogvyer för statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-260">Catalog views for statistics</span></span>
<span data-ttu-id="2e1de-261">Dessa systemvyer innehåller information om statistik:</span><span class="sxs-lookup"><span data-stu-id="2e1de-261">These system views provide information about statistics:</span></span>

| <span data-ttu-id="2e1de-262">Vyn katalog</span><span class="sxs-lookup"><span data-stu-id="2e1de-262">Catalog View</span></span> | <span data-ttu-id="2e1de-263">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2e1de-263">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2e1de-264">[sys.Columns][sys.columns]</span><span class="sxs-lookup"><span data-stu-id="2e1de-264">[sys.columns][sys.columns]</span></span> |<span data-ttu-id="2e1de-265">En rad för varje kolumn.</span><span class="sxs-lookup"><span data-stu-id="2e1de-265">One row for each column.</span></span> |
| <span data-ttu-id="2e1de-266">[sys.Objects][sys.objects]</span><span class="sxs-lookup"><span data-stu-id="2e1de-266">[sys.objects][sys.objects]</span></span> |<span data-ttu-id="2e1de-267">En rad för varje objekt i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-267">One row for each object in hello database.</span></span> |
| <span data-ttu-id="2e1de-268">[sys.schemas][sys.schemas]</span><span class="sxs-lookup"><span data-stu-id="2e1de-268">[sys.schemas][sys.schemas]</span></span> |<span data-ttu-id="2e1de-269">En rad för varje schema i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-269">One row for each schema in hello database.</span></span> |
| <span data-ttu-id="2e1de-270">[sys.stats][sys.stats]</span><span class="sxs-lookup"><span data-stu-id="2e1de-270">[sys.stats][sys.stats]</span></span> |<span data-ttu-id="2e1de-271">En rad för varje objekt i statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-271">One row for each statistics object.</span></span> |
| <span data-ttu-id="2e1de-272">[sys.stats_columns][sys.stats_columns]</span><span class="sxs-lookup"><span data-stu-id="2e1de-272">[sys.stats_columns][sys.stats_columns]</span></span> |<span data-ttu-id="2e1de-273">En rad för varje kolumn i hello statistik för objektet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-273">One row for each column in hello statistics object.</span></span> <span data-ttu-id="2e1de-274">Länkar tillbaka toosys.columns.</span><span class="sxs-lookup"><span data-stu-id="2e1de-274">Links back toosys.columns.</span></span> |
| <span data-ttu-id="2e1de-275">[sys.Tables][sys.tables]</span><span class="sxs-lookup"><span data-stu-id="2e1de-275">[sys.tables][sys.tables]</span></span> |<span data-ttu-id="2e1de-276">En rad för varje tabell (inklusive externa tabeller).</span><span class="sxs-lookup"><span data-stu-id="2e1de-276">One row for each table (includes external tables).</span></span> |
| <span data-ttu-id="2e1de-277">[sys.table_types][sys.table_types]</span><span class="sxs-lookup"><span data-stu-id="2e1de-277">[sys.table_types][sys.table_types]</span></span> |<span data-ttu-id="2e1de-278">En rad för varje datatyp.</span><span class="sxs-lookup"><span data-stu-id="2e1de-278">One row for each data type.</span></span> |

### <a name="system-functions-for-statistics"></a><span data-ttu-id="2e1de-279">Systemfunktioner för statistik</span><span class="sxs-lookup"><span data-stu-id="2e1de-279">System functions for statistics</span></span>
<span data-ttu-id="2e1de-280">Dessa systemfunktioner är användbara för att arbeta med statistik:</span><span class="sxs-lookup"><span data-stu-id="2e1de-280">These system functions are useful for working with statistics:</span></span>

| <span data-ttu-id="2e1de-281">Systemfunktionen</span><span class="sxs-lookup"><span data-stu-id="2e1de-281">System Function</span></span> | <span data-ttu-id="2e1de-282">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2e1de-282">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2e1de-283">[STATS_DATE][STATS_DATE]</span><span class="sxs-lookup"><span data-stu-id="2e1de-283">[STATS_DATE][STATS_DATE]</span></span> |<span data-ttu-id="2e1de-284">Datum hello statistik objekt uppdaterades senast.</span><span class="sxs-lookup"><span data-stu-id="2e1de-284">Date hello statistics object was last updated.</span></span> |
| <span data-ttu-id="2e1de-285">[DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span><span class="sxs-lookup"><span data-stu-id="2e1de-285">[DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span></span> |<span data-ttu-id="2e1de-286">Innehåller översiktlig nivå och detaljerad information om hello fördelningen av värden som tolkas av hello statistik-objektet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-286">Provides summary level and detailed information about hello distribution of values as understood by hello statistics object.</span></span> |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a><span data-ttu-id="2e1de-287">Kombinera statistik kolumner och funktioner i en vy</span><span class="sxs-lookup"><span data-stu-id="2e1de-287">Combine statistics columns and functions into one view</span></span>
<span data-ttu-id="2e1de-288">Den här vyn visar kolumner som rör toostatistics och resultat från hello [STATS_DATE()] [] funktionen tillsammans.</span><span class="sxs-lookup"><span data-stu-id="2e1de-288">This view brings columns that relate toostatistics, and results from hello [STATS_DATE()][]function together.</span></span>

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a><span data-ttu-id="2e1de-289">DBCC SHOW_STATISTICS() exempel</span><span class="sxs-lookup"><span data-stu-id="2e1de-289">DBCC SHOW_STATISTICS() examples</span></span>
<span data-ttu-id="2e1de-290">DBCC SHOW_STATISTICS() visar hello data som lagras i ett statistik-objekt.</span><span class="sxs-lookup"><span data-stu-id="2e1de-290">DBCC SHOW_STATISTICS() shows hello data held within a statistics object.</span></span> <span data-ttu-id="2e1de-291">Dessa data finns i tre delar.</span><span class="sxs-lookup"><span data-stu-id="2e1de-291">This data comes in three parts.</span></span>

1. <span data-ttu-id="2e1de-292">Huvudet</span><span class="sxs-lookup"><span data-stu-id="2e1de-292">Header</span></span>
2. <span data-ttu-id="2e1de-293">Densitet Vector</span><span class="sxs-lookup"><span data-stu-id="2e1de-293">Density Vector</span></span>
3. <span data-ttu-id="2e1de-294">Histogram</span><span class="sxs-lookup"><span data-stu-id="2e1de-294">Histogram</span></span>

<span data-ttu-id="2e1de-295">hello sidhuvud metadata om hello statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-295">hello header metadata about hello statistics.</span></span> <span data-ttu-id="2e1de-296">hello histogram visar hello fördelningen av värden i hello första nyckelkolumn hello statistik för objektet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-296">hello histogram displays hello distribution of values in hello first key column of hello statistics object.</span></span> <span data-ttu-id="2e1de-297">Hej densitet vector åtgärder mellan kolumnen korrelation.</span><span class="sxs-lookup"><span data-stu-id="2e1de-297">hello density vector measures cross-column correlation.</span></span> <span data-ttu-id="2e1de-298">SQLDW beräknar kardinalitet uppskattningar med någon av hello data i hello statistik för objektet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-298">SQLDW computes cardinality estimates with any of hello data in hello statistics object.</span></span>

### <a name="show-header-density-and-histogram"></a><span data-ttu-id="2e1de-299">Visa sidhuvud, densitet och histogram</span><span class="sxs-lookup"><span data-stu-id="2e1de-299">Show header, density, and histogram</span></span>
<span data-ttu-id="2e1de-300">Det här enkla exemplet visar alla tre delar av ett objekt av statistik.</span><span class="sxs-lookup"><span data-stu-id="2e1de-300">This simple example shows all three parts of a statistics object.</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

<span data-ttu-id="2e1de-301">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2e1de-301">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a><span data-ttu-id="2e1de-302">Visa en eller flera delar av DBCC SHOW_STATISTICS();</span><span class="sxs-lookup"><span data-stu-id="2e1de-302">Show one or more parts of DBCC SHOW_STATISTICS();</span></span>
<span data-ttu-id="2e1de-303">Om du bara vill visa specifika delar använder hello `WITH` satsen och ange vilka delar du vill toosee:</span><span class="sxs-lookup"><span data-stu-id="2e1de-303">If you are only interested in viewing specific parts, use hello `WITH` clause and specify which parts you want toosee:</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

<span data-ttu-id="2e1de-304">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2e1de-304">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a><span data-ttu-id="2e1de-305">DBCC SHOW_STATISTICS() skillnader</span><span class="sxs-lookup"><span data-stu-id="2e1de-305">DBCC SHOW_STATISTICS() differences</span></span>
<span data-ttu-id="2e1de-306">DBCC SHOW_STATISTICS() implementeras striktare i SQL Data Warehouse jämfört med tooSQL Server.</span><span class="sxs-lookup"><span data-stu-id="2e1de-306">DBCC SHOW_STATISTICS() is more strictly implemented in SQL Data Warehouse compared tooSQL Server.</span></span>

1. <span data-ttu-id="2e1de-307">Odokumenterade funktioner stöds inte</span><span class="sxs-lookup"><span data-stu-id="2e1de-307">Undocumented features are not supported</span></span>
2. <span data-ttu-id="2e1de-308">Det går inte att använda Stats_stream</span><span class="sxs-lookup"><span data-stu-id="2e1de-308">Cannot use Stats_stream</span></span>
3. <span data-ttu-id="2e1de-309">Kan inte ansluta resultat för specifika delmängder av statistikdata, t.ex. (STAT_HEADER koppling DENSITY_VECTOR)</span><span class="sxs-lookup"><span data-stu-id="2e1de-309">Cannot join results for specific subsets of statistics data e.g. (STAT_HEADER JOIN DENSITY_VECTOR)</span></span>
4. <span data-ttu-id="2e1de-310">NO_INFOMSGS kan inte anges för Undertryckning meddelande</span><span class="sxs-lookup"><span data-stu-id="2e1de-310">NO_INFOMSGS cannot be set for message suppression</span></span>
5. <span data-ttu-id="2e1de-311">Hakparenteser runt statistik namn kan inte användas</span><span class="sxs-lookup"><span data-stu-id="2e1de-311">Square brackets around statistics names cannot be used</span></span>
6. <span data-ttu-id="2e1de-312">Det går inte att använda kolumnen namn tooidentify statistik objekt</span><span class="sxs-lookup"><span data-stu-id="2e1de-312">Cannot use column names tooidentify statistics objects</span></span>
7. <span data-ttu-id="2e1de-313">Anpassade fel 2767 stöds inte</span><span class="sxs-lookup"><span data-stu-id="2e1de-313">Custom error 2767 is not supported</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e1de-314">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e1de-314">Next steps</span></span>
<span data-ttu-id="2e1de-315">Mer information finns i [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] på MSDN.</span><span class="sxs-lookup"><span data-stu-id="2e1de-315">For more details, see [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] on MSDN.</span></span>  <span data-ttu-id="2e1de-316">toolearn finns fler hello artiklar på [tabell översikt][Overview], [Data tabelltyper][Data Types], [distribuerar en tabell] [ Distribute], [Indexering av en tabell][Index], [partitionering en tabell] [ Partition] och [ Temporära tabeller][Temporary].</span><span class="sxs-lookup"><span data-stu-id="2e1de-316">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="2e1de-317">Mer information om metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="2e1de-317">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
