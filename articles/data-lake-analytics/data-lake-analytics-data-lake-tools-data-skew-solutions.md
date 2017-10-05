---
title: "Lösa problem med en förskjutning av data med hjälp av Azure Data Lake-verktyg för Visual Studio | Microsoft Docs"
description: "Felsökning av möjliga lösningar för data-förskjutning av problem med Azure Data Lake-verktyg för Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/16/2016
ms.author: yanacai
ms.openlocfilehash: 9b284ef33be4b935569fc368d81ddf040b2c2b7d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="e3fe8-103">Lösa problem med en förskjutning av data med hjälp av Azure Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3fe8-103">Resolve data-skew problems by using Azure Data Lake Tools for Visual Studio</span></span>

## <a name="what-is-data-skew"></a><span data-ttu-id="e3fe8-104">Vad är data skeva?</span><span class="sxs-lookup"><span data-stu-id="e3fe8-104">What is data skew?</span></span>

<span data-ttu-id="e3fe8-105">Kort anges är data förskjutning ett mycket representeras värde.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-105">Briefly stated, data skew is an over-represented value.</span></span> <span data-ttu-id="e3fe8-106">Anta att du har tilldelat 50 skatt granskarna att granska deklarationer, en förarprövaren för varje USA tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-106">Imagine that you have assigned 50 tax examiners to audit tax returns, one examiner for each US state.</span></span> <span data-ttu-id="e3fe8-107">Granskaren Wyoming har eftersom det populationen är liten, liten eller göra.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-107">The Wyoming examiner, because the population there is small, has little to do.</span></span> <span data-ttu-id="e3fe8-108">I California, men sparas granskaren hårt på grund av stora ifyllning av det tillståndet.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-108">In California, however, the examiner is kept very busy because of the state's large population.</span></span>
    <span data-ttu-id="e3fe8-109">![Förskjutning av data problemet exempel](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span><span class="sxs-lookup"><span data-stu-id="e3fe8-109">![Data-skew problem example](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span></span>

<span data-ttu-id="e3fe8-110">I vårt scenario, är data ojämnt fördelad över alla skatt granskare, vilket innebär att vissa granskare måste arbeta mer än andra.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-110">In our scenario, the data is unevenly distributed across all tax examiners, which means that some examiners must work more than others.</span></span> <span data-ttu-id="e3fe8-111">Situationer som exempel skatt förarprövaren här uppstår ofta i dina egna jobbet.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-111">In your own job, you frequently experience situations like the tax-examiner example here.</span></span> <span data-ttu-id="e3fe8-112">Hämtar mycket mer data än dess peer-datorer för ett hörn i mer tekniska villkor, en situation som gör vertex arbeta mer än andra och som slutligen långsammare ett hela jobb.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-112">In more technical terms, one vertex gets much more data than its peers, a situation that makes the vertex work more than the others and that eventually slows down an entire job.</span></span> <span data-ttu-id="e3fe8-113">Vad är försämras kan jobbet misslyckas eftersom formhörnen kanske till exempel en begränsning av 5 timmar runtime och en begränsning med 6 GB minne.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-113">What's worse, the job might fail, because vertices might have, for example, a 5-hour runtime limitation and a 6-GB memory limitation.</span></span>

## <a name="resolving-data-skew-problems"></a><span data-ttu-id="e3fe8-114">Lösa problem för förskjutning av data</span><span class="sxs-lookup"><span data-stu-id="e3fe8-114">Resolving data-skew problems</span></span>

<span data-ttu-id="e3fe8-115">Azure Data Lake-verktyg för Visual Studio kan hjälpa dig att upptäcka om jobbet har problem med en förskjutning av data.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-115">Azure Data Lake Tools for Visual Studio can help detect whether your job has a data-skew problem.</span></span> <span data-ttu-id="e3fe8-116">Om det finns ett problem, kan du lösa det. genom lösningarna i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-116">If a problem exists, you can resolve it by trying the solutions in this section.</span></span>

## <a name="solution-1-improve-table-partitioning"></a><span data-ttu-id="e3fe8-117">Lösning 1: Förbättra Tabellpartitionering</span><span class="sxs-lookup"><span data-stu-id="e3fe8-117">Solution 1: Improve table partitioning</span></span>

### <a name="option-1-filter-the-skewed-key-value-in-advance"></a><span data-ttu-id="e3fe8-118">Alternativ 1: Filtrera skeva nyckelvärdet i förväg</span><span class="sxs-lookup"><span data-stu-id="e3fe8-118">Option 1: Filter the skewed key value in advance</span></span>

<span data-ttu-id="e3fe8-119">Om det inte påverkar affärslogik, kan du filtrera högre frekvens värden i förväg.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-119">If it does not affect your business logic, you can filter the higher-frequency values in advance.</span></span> <span data-ttu-id="e3fe8-120">Om det finns en mängd 000 000 000 i kolumnen GUID, kan du t.ex, inte vill att sammanställa värdet.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-120">For example, if there are a lot of 000-000-000 in column GUID, you might not want to aggregate that value.</span></span> <span data-ttu-id="e3fe8-121">Innan du sammanställd, du kan skriva ”var GUID! =” 000 – 000 000 ”” till filtervärde hög frekvens.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-121">Before you aggregate, you can write “WHERE GUID != “000-000-000”” to filter the high-frequency value.</span></span>

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a><span data-ttu-id="e3fe8-122">Alternativ 2: Välj en annan partition eller distribution nyckel</span><span class="sxs-lookup"><span data-stu-id="e3fe8-122">Option 2: Pick a different partition or distribution key</span></span>

<span data-ttu-id="e3fe8-123">I föregående exempel, om du vill bara kontrollera skatt audit arbetsbelastningen över hela landet, kan du förbättra Datadistributionen genom att välja ett ID-nummer som din nyckel.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-123">In the preceding example, if you want only to check the tax-audit workload all over the country, you can improve the data distribution by selecting the ID number as your key.</span></span> <span data-ttu-id="e3fe8-124">Välja en annan partition eller distribution tangent kan ibland distribuera data jämnare, men du måste se till att detta val inte påverkar din affärslogik.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-124">Picking a different partition or distribution key can sometimes distribute the data more evenly, but you need to make sure that this choice doesn’t affect your business logic.</span></span> <span data-ttu-id="e3fe8-125">Till exempel för att beräkna moms summan för respektive tillstånd, kanske du vill ange _tillstånd_ som partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-125">For instance, to calculate the tax sum for each state, you might want to designate _State_ as the partition key.</span></span> <span data-ttu-id="e3fe8-126">Om du fortsätter att problemet kvarstår kan du prova att använda alternativ 3.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-126">If you continue to experience this problem, try using Option 3.</span></span>

### <a name="option-3-add-more-partition-or-distribution-keys"></a><span data-ttu-id="e3fe8-127">Alternativ 3: Lägg till flera nycklar för partitionen eller distribution</span><span class="sxs-lookup"><span data-stu-id="e3fe8-127">Option 3: Add more partition or distribution keys</span></span>

<span data-ttu-id="e3fe8-128">I stället för endast _tillstånd_ som en partitionsnyckel kan du använda mer än en nyckel för partitionering.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-128">Instead of using only _State_ as a partition key, you can use more than one key for partitioning.</span></span> <span data-ttu-id="e3fe8-129">Anta till exempel att lägga till _postnummer_ som en ytterligare partitionsnyckel att minska data partitionsstorlek och distribuera data mer jämnt.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-129">For example, consider adding _ZIP Code_ as an additional partition key to reduce data-partition sizes and distribute the data more evenly.</span></span>

### <a name="option-4-use-round-robin-distribution"></a><span data-ttu-id="e3fe8-130">Alternativ 4: Använd resursallokering</span><span class="sxs-lookup"><span data-stu-id="e3fe8-130">Option 4: Use round-robin distribution</span></span>

<span data-ttu-id="e3fe8-131">Om du inte hittar en lämplig nyckel för partition och distribution, kan du försöka använda resursallokering.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-131">If you cannot find an appropriate key for partition and distribution, you can try to use round-robin distribution.</span></span> <span data-ttu-id="e3fe8-132">Resursallokering behandlas alla rader lika och placerar slumpmässigt dem i motsvarande buckets.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-132">Round-robin distribution treats all rows equally and randomly puts them into corresponding buckets.</span></span> <span data-ttu-id="e3fe8-133">Hämtar data jämnt fördelad, men det förlorar ort information nackdelen som kan också minska jobbet prestandan för vissa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-133">The data gets evenly distributed, but it loses locality information, a drawback that can also reduce job performance for some operations.</span></span> <span data-ttu-id="e3fe8-134">Om du gör sammanställning för nyckeln skeva ändå, behålls dessutom problemet förskjutning av data.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-134">Additionally, if you are doing aggregation for the skewed key anyway, the data-skew problem will persist.</span></span> <span data-ttu-id="e3fe8-135">Mer information om resursallokering finns i distributioner för U-SQL-tabellen i [CREATE TABLE (U-SQL): skapa en tabell med schemat](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span><span class="sxs-lookup"><span data-stu-id="e3fe8-135">To learn more about round-robin distribution, see the U-SQL Table Distributions section in [CREATE TABLE (U-SQL): Creating a Table with Schema](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span></span>

## <a name="solution-2-improve-the-query-plan"></a><span data-ttu-id="e3fe8-136">Lösning 2: Förbättra frågeplanen</span><span class="sxs-lookup"><span data-stu-id="e3fe8-136">Solution 2: Improve the query plan</span></span>

### <a name="option-1-use-the-create-statistics-statement"></a><span data-ttu-id="e3fe8-137">Alternativ 1: Använd instruktionen CREATE STATISTICS</span><span class="sxs-lookup"><span data-stu-id="e3fe8-137">Option 1: Use the CREATE STATISTICS statement</span></span>

<span data-ttu-id="e3fe8-138">U-SQL ger instruktionen CREATE STATISTICS på tabeller.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-138">U-SQL provides the CREATE STATISTICS statement on tables.</span></span> <span data-ttu-id="e3fe8-139">Den här instruktionen ger mer information till Frågeoptimeringen om, dataegenskaper som till exempel värdet distribution, som lagras i en tabell.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-139">This statement gives more information to the query optimizer about the data characteristics, such as value distribution, that are stored in a table.</span></span> <span data-ttu-id="e3fe8-140">För de flesta frågor genererar Frågeoptimeringen redan nödvändiga statistik för en frågeplan med hög kvalitet.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-140">For most queries, the query optimizer already generates the necessary statistics for a high-quality query plan.</span></span> <span data-ttu-id="e3fe8-141">Ibland kan behöva du förbättra frågeprestanda genom att skapa ytterligare statistik med CREATE STATISTICS eller genom att ändra frågans design.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-141">Occasionally, you might need to improve query performance by creating additional statistics with CREATE STATISTICS or by modifying the query design.</span></span> <span data-ttu-id="e3fe8-142">Mer information finns i [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-142">For more information, see the [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) page.</span></span>

<span data-ttu-id="e3fe8-143">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e3fe8-143">Code example:</span></span>

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
><span data-ttu-id="e3fe8-144">Statistikinformation uppdateras inte automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-144">Statistics information is not updated automatically.</span></span> <span data-ttu-id="e3fe8-145">Om du uppdaterar data i en tabell utan att återskapa statistik kan frågeprestandan neka.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-145">If you update the data in a table without re-creating the statistics, the query performance might decline.</span></span>

### <a name="option-2-use-skewfactor"></a><span data-ttu-id="e3fe8-146">Alternativ 2: Använd SKEWFACTOR</span><span class="sxs-lookup"><span data-stu-id="e3fe8-146">Option 2: Use SKEWFACTOR</span></span>

<span data-ttu-id="e3fe8-147">Om du vill beräkna moms för varje tillstånd måste du använda GROUP BY tillstånd, en metod som inte undviker problemet förskjutning av data.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-147">If you want to sum the tax for each state, you must use GROUP BY state, an approach that doesn't avoid the data-skew problem.</span></span> <span data-ttu-id="e3fe8-148">Men kan du ange en ledtråd för data i din fråga att identifiera data förskjutning i nycklar så att optimering kan förbereda en plan för körning av du.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-148">However, you can provide a data hint in your query to identify data skew in keys so that the optimizer can prepare an execution plan for you.</span></span>

<span data-ttu-id="e3fe8-149">Vanligtvis kan du ange parametern som 0,5 och 1, med 0,5 vilket innebär inte mycket skeva och 1 betydelse tunga förskjutning.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-149">Usually, you can set the parameter as 0.5 and 1, with 0.5 meaning not much skew and 1 meaning heavy skew.</span></span> <span data-ttu-id="e3fe8-150">Eftersom tipset påverkar åtgärdsplan optimering för den aktuella instruktionen och alla underordnade instruktioner, måste du lägga till tipset innan risken förvrängd key-wise aggregering.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-150">Because the hint affects execution-plan optimization for the current statement and all downstream statements, be sure to add the hint before the potential skewed key-wise aggregation.</span></span>

    SKEWFACTOR (columns) = x

    Provides a hint that the given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

<span data-ttu-id="e3fe8-151">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e3fe8-151">Code example:</span></span>

    //Add a SKEWFACTOR hint.
    @Impressions =
        SELECT * FROM
        searchDM.SML.PageView(@start, @end) AS PageView
        OPTION(SKEWFACTOR(Query)=0.5)
        ;

    //Query 1 for key: Query, ClientId
    @Sessions =
        SELECT
            ClientId,
            Query,
            SUM(PageClicks) AS Clicks
        FROM
            @Impressions
        GROUP BY
            Query, ClientId
        ;

    //Query 2 for Key: Query
    @Display =
        SELECT * FROM @Sessions
            INNER JOIN @Campaigns
                ON @Sessions.Query == @Campaigns.Query
        ;   

### <a name="option-3-use-rowcount"></a><span data-ttu-id="e3fe8-152">Alternativ 3: Använd ROWCOUNT</span><span class="sxs-lookup"><span data-stu-id="e3fe8-152">Option 3: Use ROWCOUNT</span></span>  
<span data-ttu-id="e3fe8-153">Förutom SKEWFACTOR ser för specifika förvrängd nyckeln koppling fall om du vet att den andra uppsättningen för domänanslutna raden är liten, du optimering genom att lägga till ett RADANTAL tips i U-SQL-instruktionen innan koppling.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-153">In addition to SKEWFACTOR, for specific skewed-key join cases, if you know that the other joined row set is small, you can tell the optimizer by adding a ROWCOUNT hint in the U-SQL statement before JOIN.</span></span> <span data-ttu-id="e3fe8-154">Det här sättet optimering kan välja en broadcast join-strategi för att förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-154">This way, optimizer can choose a broadcast join strategy to help improve performance.</span></span> <span data-ttu-id="e3fe8-155">Tänk på att ROWCOUNT inte löser problemet förskjutning av data, men den kan erbjuda ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-155">Be aware that ROWCOUNT does not resolve the data-skew problem, but it can offer some additional help.</span></span>

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

<span data-ttu-id="e3fe8-156">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e3fe8-156">Code example:</span></span>

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information to determine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-the-user-defined-reducer-and-combiner"></a><span data-ttu-id="e3fe8-157">Lösning 3: Förbättra användardefinierade reducer och combiner</span><span class="sxs-lookup"><span data-stu-id="e3fe8-157">Solution 3: Improve the user-defined reducer and combiner</span></span>

<span data-ttu-id="e3fe8-158">Ibland kan du skriva en användardefinierad operatorn för att hantera komplicerad Processlogik och en bra reducer och combiner kan minska problem förskjutning av data i vissa fall.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-158">You can sometimes write a user-defined operator to deal with complicated process logic, and a well-written reducer and combiner might mitigate a data-skew problem in some cases.</span></span>

### <a name="option-1-use-a-recursive-reducer-if-possible"></a><span data-ttu-id="e3fe8-159">Alternativ 1: Använd om möjligt en rekursiv reducer</span><span class="sxs-lookup"><span data-stu-id="e3fe8-159">Option 1: Use a recursive reducer, if possible</span></span>

<span data-ttu-id="e3fe8-160">Som standard körs en användardefinierad reducer i icke-rekursiva läge, vilket innebär att minskar arbete för en nyckel har distribuerats till en enda nod.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-160">By default, a user-defined reducer runs in non-recursive mode, which means that reduce work for a key is distributed into a single vertex.</span></span> <span data-ttu-id="e3fe8-161">Men om dina data är förvrängd stora datauppsättningar kan bearbetas i en enda nod och kör under lång tid.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-161">But if your data is skewed, the huge data sets might be processed in a single vertex and run for a long time.</span></span>

<span data-ttu-id="e3fe8-162">Du kan lägga till ett attribut i koden för att definiera reducer för rekursiv läget för att förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-162">To improve performance, you can add an attribute in your code to define reducer to run in recursive mode.</span></span> <span data-ttu-id="e3fe8-163">Stora datauppsättningar kan sedan distribueras till flera formhörnen och köras parallellt, vilket förbättrar ditt jobb.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-163">Then, the huge data sets can be distributed to multiple vertices and run in parallel, which speeds up your job.</span></span>

<span data-ttu-id="e3fe8-164">Om du vill ändra en icke-rekursiva reducer rekursiva, måste du se till att din algoritmen är kopplat.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-164">To change a non-recursive reducer to recursive, you need to make sure that your algorithm is associative.</span></span> <span data-ttu-id="e3fe8-165">Till exempel summan är kopplat och medianvärdet är inte.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-165">For example, the sum is associative, and the median is not.</span></span> <span data-ttu-id="e3fe8-166">Du måste också kontrollera att indata och utdata för reducer behåller samma schema.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-166">You also need to make sure that the input and output for reducer keep the same schema.</span></span>

<span data-ttu-id="e3fe8-167">Attributet för rekursiv reducer:</span><span class="sxs-lookup"><span data-stu-id="e3fe8-167">Attribute of recursive reducer:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]

<span data-ttu-id="e3fe8-168">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e3fe8-168">Code example:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a><span data-ttu-id="e3fe8-169">Alternativ 2: Använd om möjligt radnivå combiner läge</span><span class="sxs-lookup"><span data-stu-id="e3fe8-169">Option 2: Use row-level combiner mode, if possible</span></span>

<span data-ttu-id="e3fe8-170">Liknar ROWCOUNT tips för specifika förvrängd nyckeln koppling fall, combiner läge försöker distribuera stora förvrängd nyckel värde anger att flera formhörnen så att arbetet som kan köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-170">Similar to the ROWCOUNT hint for specific skewed-key join cases, combiner mode tries to distribute huge skewed-key value sets to multiple vertices so that the work can be executed concurrently.</span></span> <span data-ttu-id="e3fe8-171">Combiner läge inte kan lösa problem med förskjutning av data, men den kan erbjuda för stora förvrängd nyckel värde anger ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-171">Combiner mode can’t resolve data-skew issues, but it can offer some additional help for huge skewed-key value sets.</span></span>

<span data-ttu-id="e3fe8-172">Som standard är combiner läget Full, vilket innebär att den raden vänstra och högra raden uppsättningen inte kan skiljas.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-172">By default, the combiner mode is Full, which means that the left row set and right row set cannot be separated.</span></span> <span data-ttu-id="e3fe8-173">Anger läget som inre-vänster/höger kan radnivå koppling.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-173">Setting the mode as Left/Right/Inner enables row-level join.</span></span> <span data-ttu-id="e3fe8-174">Systemet avgränsar motsvarande rad uppsättningar och distribuerar dem till flera formhörnen som körs parallellt.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-174">The system separates the corresponding row sets and distributes them into multiple vertices that run in parallel.</span></span> <span data-ttu-id="e3fe8-175">Innan du konfigurerar combiner läge, Tänk på att säkerställa att motsvarande rad uppsättningar kan delas.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-175">However, before you configure the combiner mode, be careful to ensure that the corresponding row sets can be separated.</span></span>

<span data-ttu-id="e3fe8-176">Följande exempel visar en uppsättning åtskilda vänstra raden.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-176">The example that follows shows a separated left row set.</span></span> <span data-ttu-id="e3fe8-177">Varje rad i utdata beror på en inkommande rad från vänster, och eventuellt på alla rader från höger med samma nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-177">Each output row depends on a single input row from the left, and it potentially depends on all rows from the right with the same key value.</span></span> <span data-ttu-id="e3fe8-178">Om du anger combiner-läge som vänster systemet avgränsar enorma vänster-raden i små och tilldelar dem till flera formhörnen.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-178">If you set the combiner mode as left, the system separates the huge left-row set into small ones and assigns them to multiple vertices.</span></span>

![Combiner läge bild](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
><span data-ttu-id="e3fe8-180">Om du har angett fel combiner läget kombinationen är mindre effektivt och resultaten kan vara felaktig.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-180">If you set the wrong combiner mode, the combination is less efficient, and the results might be wrong.</span></span>

<span data-ttu-id="e3fe8-181">Attribut för combiner läge:</span><span class="sxs-lookup"><span data-stu-id="e3fe8-181">Attributes of combiner mode:</span></span>

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all the input rows from left and right with the same key value.

- <span data-ttu-id="e3fe8-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Varje rad i utdata beror på en rad från vänster (och potentiellt alla rader från höger med samma nyckelvärde) som indata.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Every output row depends on a single input row from the left (and potentially all rows from the right with the same key value).</span></span>

- <span data-ttu-id="e3fe8-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): varje rad i utdata är beroende av en rad från höger (och potentiellt alla rader från vänster med samma nyckelvärde) som indata.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): Every output row depends on a single input row from the right (and potentially all rows from the left with the same key value).</span></span>

- <span data-ttu-id="e3fe8-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Varje rad i utdata beror på en rad från vänster och höger med samma värde som indata.</span><span class="sxs-lookup"><span data-stu-id="e3fe8-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Every output row depends on a single input row from the left and the right with the same value.</span></span>

<span data-ttu-id="e3fe8-185">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e3fe8-185">Code example:</span></span>

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
