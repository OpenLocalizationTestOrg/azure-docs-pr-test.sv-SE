---
title: "problem med aaaResolve förskjutning av data med hjälp av Azure Data Lake-verktyg för Visual Studio | Microsoft Docs"
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
ms.openlocfilehash: 3909fbd89eb40f061268cb7128f7fa84a3c33de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="60bcb-103">Lösa problem med en förskjutning av data med hjälp av Azure Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60bcb-103">Resolve data-skew problems by using Azure Data Lake Tools for Visual Studio</span></span>

## <a name="what-is-data-skew"></a><span data-ttu-id="60bcb-104">Vad är data skeva?</span><span class="sxs-lookup"><span data-stu-id="60bcb-104">What is data skew?</span></span>

<span data-ttu-id="60bcb-105">Kort anges är data förskjutning ett mycket representeras värde.</span><span class="sxs-lookup"><span data-stu-id="60bcb-105">Briefly stated, data skew is an over-represented value.</span></span> <span data-ttu-id="60bcb-106">Anta att du har tilldelat 50 skatt granskare tooaudit deklarationer, en förarprövaren för varje USA tillstånd.</span><span class="sxs-lookup"><span data-stu-id="60bcb-106">Imagine that you have assigned 50 tax examiners tooaudit tax returns, one examiner for each US state.</span></span> <span data-ttu-id="60bcb-107">Hej Wyoming förarprövaren har eftersom det hello population är liten, lite toodo.</span><span class="sxs-lookup"><span data-stu-id="60bcb-107">hello Wyoming examiner, because hello population there is small, has little toodo.</span></span> <span data-ttu-id="60bcb-108">I California, men sparas hello förarprövaren hårt på grund av stora ifyllning av hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="60bcb-108">In California, however, hello examiner is kept very busy because of hello state's large population.</span></span>
    <span data-ttu-id="60bcb-109">![Förskjutning av data problemet exempel](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span><span class="sxs-lookup"><span data-stu-id="60bcb-109">![Data-skew problem example](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span></span>

<span data-ttu-id="60bcb-110">I vårt scenario distribueras hello ojämnt data på alla skatt granskare, vilket innebär att vissa granskare måste arbeta mer än andra.</span><span class="sxs-lookup"><span data-stu-id="60bcb-110">In our scenario, hello data is unevenly distributed across all tax examiners, which means that some examiners must work more than others.</span></span> <span data-ttu-id="60bcb-111">Situationer som hello skatt förarprövaren exempel här uppstår ofta i dina egna jobbet.</span><span class="sxs-lookup"><span data-stu-id="60bcb-111">In your own job, you frequently experience situations like hello tax-examiner example here.</span></span> <span data-ttu-id="60bcb-112">Ett hörn hämtar mycket mer data än dess peer-datorer, en situation som gör hello vertex arbeta mer än andra och som slutligen saktar ned en hela jobbet hello i mer tekniska villkor.</span><span class="sxs-lookup"><span data-stu-id="60bcb-112">In more technical terms, one vertex gets much more data than its peers, a situation that makes hello vertex work more than hello others and that eventually slows down an entire job.</span></span> <span data-ttu-id="60bcb-113">Vad är försämras kan hello jobb misslyckas eftersom formhörnen kanske till exempel en begränsning av 5 timmar runtime och en begränsning med 6 GB minne.</span><span class="sxs-lookup"><span data-stu-id="60bcb-113">What's worse, hello job might fail, because vertices might have, for example, a 5-hour runtime limitation and a 6-GB memory limitation.</span></span>

## <a name="resolving-data-skew-problems"></a><span data-ttu-id="60bcb-114">Lösa problem för förskjutning av data</span><span class="sxs-lookup"><span data-stu-id="60bcb-114">Resolving data-skew problems</span></span>

<span data-ttu-id="60bcb-115">Azure Data Lake-verktyg för Visual Studio kan hjälpa dig att upptäcka om jobbet har problem med en förskjutning av data.</span><span class="sxs-lookup"><span data-stu-id="60bcb-115">Azure Data Lake Tools for Visual Studio can help detect whether your job has a data-skew problem.</span></span> <span data-ttu-id="60bcb-116">Om det finns ett problem, kan du lösa det. genom hello lösningar i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="60bcb-116">If a problem exists, you can resolve it by trying hello solutions in this section.</span></span>

## <a name="solution-1-improve-table-partitioning"></a><span data-ttu-id="60bcb-117">Lösning 1: Förbättra Tabellpartitionering</span><span class="sxs-lookup"><span data-stu-id="60bcb-117">Solution 1: Improve table partitioning</span></span>

### <a name="option-1-filter-hello-skewed-key-value-in-advance"></a><span data-ttu-id="60bcb-118">Alternativ 1: Filter hello förvrängd nyckelvärdet i förväg</span><span class="sxs-lookup"><span data-stu-id="60bcb-118">Option 1: Filter hello skewed key value in advance</span></span>

<span data-ttu-id="60bcb-119">Om det inte påverkar affärslogik, kan du filtrera hello högre frekvens värden i förväg.</span><span class="sxs-lookup"><span data-stu-id="60bcb-119">If it does not affect your business logic, you can filter hello higher-frequency values in advance.</span></span> <span data-ttu-id="60bcb-120">Till exempel om det finns en mängd 000 000 000 i kolumnen GUID kan kanske du inte vill tooaggregate värdet.</span><span class="sxs-lookup"><span data-stu-id="60bcb-120">For example, if there are a lot of 000-000-000 in column GUID, you might not want tooaggregate that value.</span></span> <span data-ttu-id="60bcb-121">Innan du sammanställd, du kan skriva ”var GUID! =” 000 – 000 000 ”” toofilter hello hög frekvens värde.</span><span class="sxs-lookup"><span data-stu-id="60bcb-121">Before you aggregate, you can write “WHERE GUID != “000-000-000”” toofilter hello high-frequency value.</span></span>

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a><span data-ttu-id="60bcb-122">Alternativ 2: Välj en annan partition eller distribution nyckel</span><span class="sxs-lookup"><span data-stu-id="60bcb-122">Option 2: Pick a different partition or distribution key</span></span>

<span data-ttu-id="60bcb-123">Om du vill att endast toocheck hello skatt audit arbetsbelastning alla över hello land i föregående exempel hello kan du förbättra hello Datadistribution genom att markera hello ID-nummer som din nyckel.</span><span class="sxs-lookup"><span data-stu-id="60bcb-123">In hello preceding example, if you want only toocheck hello tax-audit workload all over hello country, you can improve hello data distribution by selecting hello ID number as your key.</span></span> <span data-ttu-id="60bcb-124">Välja en annan partition eller distribution tangent kan ibland distribuera hello data jämnare, men du måste toomake till att detta val inte påverkar din affärslogik.</span><span class="sxs-lookup"><span data-stu-id="60bcb-124">Picking a different partition or distribution key can sometimes distribute hello data more evenly, but you need toomake sure that this choice doesn’t affect your business logic.</span></span> <span data-ttu-id="60bcb-125">Till exempel toocalculate hello skatt summan för respektive tillstånd, kanske du vill toodesignate _tillstånd_ som hello partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="60bcb-125">For instance, toocalculate hello tax sum for each state, you might want toodesignate _State_ as hello partition key.</span></span> <span data-ttu-id="60bcb-126">Om du fortsätter tooexperience det här problemet kan du prova att använda alternativ 3.</span><span class="sxs-lookup"><span data-stu-id="60bcb-126">If you continue tooexperience this problem, try using Option 3.</span></span>

### <a name="option-3-add-more-partition-or-distribution-keys"></a><span data-ttu-id="60bcb-127">Alternativ 3: Lägg till flera nycklar för partitionen eller distribution</span><span class="sxs-lookup"><span data-stu-id="60bcb-127">Option 3: Add more partition or distribution keys</span></span>

<span data-ttu-id="60bcb-128">I stället för endast _tillstånd_ som en partitionsnyckel kan du använda mer än en nyckel för partitionering.</span><span class="sxs-lookup"><span data-stu-id="60bcb-128">Instead of using only _State_ as a partition key, you can use more than one key for partitioning.</span></span> <span data-ttu-id="60bcb-129">Anta till exempel att lägga till _postnummer_ som en ytterligare partition viktiga tooreduce data-partition storlekar och distribuera hello data mer jämnt.</span><span class="sxs-lookup"><span data-stu-id="60bcb-129">For example, consider adding _ZIP Code_ as an additional partition key tooreduce data-partition sizes and distribute hello data more evenly.</span></span>

### <a name="option-4-use-round-robin-distribution"></a><span data-ttu-id="60bcb-130">Alternativ 4: Använd resursallokering</span><span class="sxs-lookup"><span data-stu-id="60bcb-130">Option 4: Use round-robin distribution</span></span>

<span data-ttu-id="60bcb-131">Om du inte hittar en lämplig nyckel för partition och distribution, kan du försöka toouse resursallokering.</span><span class="sxs-lookup"><span data-stu-id="60bcb-131">If you cannot find an appropriate key for partition and distribution, you can try toouse round-robin distribution.</span></span> <span data-ttu-id="60bcb-132">Resursallokering behandlas alla rader lika och placerar slumpmässigt dem i motsvarande buckets.</span><span class="sxs-lookup"><span data-stu-id="60bcb-132">Round-robin distribution treats all rows equally and randomly puts them into corresponding buckets.</span></span> <span data-ttu-id="60bcb-133">hello data hämtar jämnt, men det förlorar ort information nackdelen som kan också minska jobbet prestandan för vissa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="60bcb-133">hello data gets evenly distributed, but it loses locality information, a drawback that can also reduce job performance for some operations.</span></span> <span data-ttu-id="60bcb-134">Om du gör sammanställning för hello skeva nyckeln ändå, behålls dessutom hello data förskjutning problem.</span><span class="sxs-lookup"><span data-stu-id="60bcb-134">Additionally, if you are doing aggregation for hello skewed key anyway, hello data-skew problem will persist.</span></span> <span data-ttu-id="60bcb-135">toolearn mer om resursallokering, se hello U-SQL-tabell distributioner i avsnittet [CREATE TABLE (U-SQL): skapa en tabell med schemat](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span><span class="sxs-lookup"><span data-stu-id="60bcb-135">toolearn more about round-robin distribution, see hello U-SQL Table Distributions section in [CREATE TABLE (U-SQL): Creating a Table with Schema](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span></span>

## <a name="solution-2-improve-hello-query-plan"></a><span data-ttu-id="60bcb-136">Lösning 2: Förbättra hello frågeplan</span><span class="sxs-lookup"><span data-stu-id="60bcb-136">Solution 2: Improve hello query plan</span></span>

### <a name="option-1-use-hello-create-statistics-statement"></a><span data-ttu-id="60bcb-137">Alternativ 1: Använd hello CREATE STATISTICS-uttryck</span><span class="sxs-lookup"><span data-stu-id="60bcb-137">Option 1: Use hello CREATE STATISTICS statement</span></span>

<span data-ttu-id="60bcb-138">U-SQL ger hello CREATE STATISTICS-uttryck på tabeller.</span><span class="sxs-lookup"><span data-stu-id="60bcb-138">U-SQL provides hello CREATE STATISTICS statement on tables.</span></span> <span data-ttu-id="60bcb-139">Den här instruktionen ger mer information toohello frågeoptimeraren om hello dataegenskaper, till exempel värdet distribution, som lagras i en tabell.</span><span class="sxs-lookup"><span data-stu-id="60bcb-139">This statement gives more information toohello query optimizer about hello data characteristics, such as value distribution, that are stored in a table.</span></span> <span data-ttu-id="60bcb-140">För de flesta frågor genererar hello frågeoptimeraren redan hello nödvändiga statistik för en frågeplan med hög kvalitet.</span><span class="sxs-lookup"><span data-stu-id="60bcb-140">For most queries, hello query optimizer already generates hello necessary statistics for a high-quality query plan.</span></span> <span data-ttu-id="60bcb-141">Ibland kan behöva du tooimprove frågeprestanda genom att skapa ytterligare statistik med CREATE STATISTICS eller genom att ändra hello frågans design.</span><span class="sxs-lookup"><span data-stu-id="60bcb-141">Occasionally, you might need tooimprove query performance by creating additional statistics with CREATE STATISTICS or by modifying hello query design.</span></span> <span data-ttu-id="60bcb-142">Mer information finns i hello [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="60bcb-142">For more information, see hello [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) page.</span></span>

<span data-ttu-id="60bcb-143">Exempel:</span><span class="sxs-lookup"><span data-stu-id="60bcb-143">Code example:</span></span>

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
><span data-ttu-id="60bcb-144">Statistikinformation uppdateras inte automatiskt.</span><span class="sxs-lookup"><span data-stu-id="60bcb-144">Statistics information is not updated automatically.</span></span> <span data-ttu-id="60bcb-145">Om du uppdaterar hello data i en tabell utan att återskapa hello statistik kan neka hello frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="60bcb-145">If you update hello data in a table without re-creating hello statistics, hello query performance might decline.</span></span>

### <a name="option-2-use-skewfactor"></a><span data-ttu-id="60bcb-146">Alternativ 2: Använd SKEWFACTOR</span><span class="sxs-lookup"><span data-stu-id="60bcb-146">Option 2: Use SKEWFACTOR</span></span>

<span data-ttu-id="60bcb-147">Om du vill toosum hello skatt för respektive tillstånd, måste du använda GROUP BY tillstånd, en metod som inte undviker hello data förskjutning problem.</span><span class="sxs-lookup"><span data-stu-id="60bcb-147">If you want toosum hello tax for each state, you must use GROUP BY state, an approach that doesn't avoid hello data-skew problem.</span></span> <span data-ttu-id="60bcb-148">Du kan dock ange en ledtråd för data i din fråga tooidentify data skeva i nycklar så att hello optimering kan förbereda en plan för körning av du.</span><span class="sxs-lookup"><span data-stu-id="60bcb-148">However, you can provide a data hint in your query tooidentify data skew in keys so that hello optimizer can prepare an execution plan for you.</span></span>

<span data-ttu-id="60bcb-149">Vanligtvis kan du ange hello-parameter som 0,5 och 1, med 0,5 vilket innebär inte mycket skeva och 1 betydelse tunga förskjutning.</span><span class="sxs-lookup"><span data-stu-id="60bcb-149">Usually, you can set hello parameter as 0.5 and 1, with 0.5 meaning not much skew and 1 meaning heavy skew.</span></span> <span data-ttu-id="60bcb-150">Eftersom hello tipset påverkar åtgärdsplan optimering för hello aktuella instruktionen och alla underordnade instruktioner, vara att tooadd hello tipset innan hello potentiella förvrängd key-wise aggregering.</span><span class="sxs-lookup"><span data-stu-id="60bcb-150">Because hello hint affects execution-plan optimization for hello current statement and all downstream statements, be sure tooadd hello hint before hello potential skewed key-wise aggregation.</span></span>

    SKEWFACTOR (columns) = x

    Provides a hint that hello given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

<span data-ttu-id="60bcb-151">Exempel:</span><span class="sxs-lookup"><span data-stu-id="60bcb-151">Code example:</span></span>

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

### <a name="option-3-use-rowcount"></a><span data-ttu-id="60bcb-152">Alternativ 3: Använd ROWCOUNT</span><span class="sxs-lookup"><span data-stu-id="60bcb-152">Option 3: Use ROWCOUNT</span></span>  
<span data-ttu-id="60bcb-153">Dessutom tooSKEWFACTOR för specifika förvrängd-nyckeln ansluta till fall, om du vet att hello andra domänanslutna raden är liten, berätta hello optimering genom att lägga till ett RADANTAL tips i hello U-SQL-instruktionen innan koppling.</span><span class="sxs-lookup"><span data-stu-id="60bcb-153">In addition tooSKEWFACTOR, for specific skewed-key join cases, if you know that hello other joined row set is small, you can tell hello optimizer by adding a ROWCOUNT hint in hello U-SQL statement before JOIN.</span></span> <span data-ttu-id="60bcb-154">Det här sättet optimering kan välja en broadcast koppling strategi toohelp förbättra prestanda.</span><span class="sxs-lookup"><span data-stu-id="60bcb-154">This way, optimizer can choose a broadcast join strategy toohelp improve performance.</span></span> <span data-ttu-id="60bcb-155">Tänk på att ROWCOUNT matchar inte hello data förskjutning problem, men den kan erbjuda ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="60bcb-155">Be aware that ROWCOUNT does not resolve hello data-skew problem, but it can offer some additional help.</span></span>

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

<span data-ttu-id="60bcb-156">Exempel:</span><span class="sxs-lookup"><span data-stu-id="60bcb-156">Code example:</span></span>

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information toodetermine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-hello-user-defined-reducer-and-combiner"></a><span data-ttu-id="60bcb-157">Lösning 3: Förbättra hello användardefinierade reducer och combiner</span><span class="sxs-lookup"><span data-stu-id="60bcb-157">Solution 3: Improve hello user-defined reducer and combiner</span></span>

<span data-ttu-id="60bcb-158">Ibland kan du skriva en användardefinierad operatorn toodeal med komplicerad Processlogik och en bra reducer och combiner kan minska problem förskjutning av data i vissa fall.</span><span class="sxs-lookup"><span data-stu-id="60bcb-158">You can sometimes write a user-defined operator toodeal with complicated process logic, and a well-written reducer and combiner might mitigate a data-skew problem in some cases.</span></span>

### <a name="option-1-use-a-recursive-reducer-if-possible"></a><span data-ttu-id="60bcb-159">Alternativ 1: Använd om möjligt en rekursiv reducer</span><span class="sxs-lookup"><span data-stu-id="60bcb-159">Option 1: Use a recursive reducer, if possible</span></span>

<span data-ttu-id="60bcb-160">Som standard körs en användardefinierad reducer i icke-rekursiva läge, vilket innebär att minskar arbete för en nyckel har distribuerats till en enda nod.</span><span class="sxs-lookup"><span data-stu-id="60bcb-160">By default, a user-defined reducer runs in non-recursive mode, which means that reduce work for a key is distributed into a single vertex.</span></span> <span data-ttu-id="60bcb-161">Men om dina data är förvrängd hello stora datauppsättningar kan bearbetas i en enda nod och kör under lång tid.</span><span class="sxs-lookup"><span data-stu-id="60bcb-161">But if your data is skewed, hello huge data sets might be processed in a single vertex and run for a long time.</span></span>

<span data-ttu-id="60bcb-162">tooimprove prestanda du kan lägga till ett attribut i koden toodefine reducer toorun i rekursiva läge.</span><span class="sxs-lookup"><span data-stu-id="60bcb-162">tooimprove performance, you can add an attribute in your code toodefine reducer toorun in recursive mode.</span></span> <span data-ttu-id="60bcb-163">Sedan kan hello stora datauppsättningar distribuerade toomultiple formhörnen och köras parallellt, vilket förbättrar ditt jobb.</span><span class="sxs-lookup"><span data-stu-id="60bcb-163">Then, hello huge data sets can be distributed toomultiple vertices and run in parallel, which speeds up your job.</span></span>

<span data-ttu-id="60bcb-164">toochange en icke-rekursiva reducer toorecursive måste toomake till att din algoritmen är kopplat.</span><span class="sxs-lookup"><span data-stu-id="60bcb-164">toochange a non-recursive reducer toorecursive, you need toomake sure that your algorithm is associative.</span></span> <span data-ttu-id="60bcb-165">Till exempel hello summan är kopplat och hello median är inte.</span><span class="sxs-lookup"><span data-stu-id="60bcb-165">For example, hello sum is associative, and hello median is not.</span></span> <span data-ttu-id="60bcb-166">Du måste också till att hello indata och utdata för reducer behåller hello toomake samma schema.</span><span class="sxs-lookup"><span data-stu-id="60bcb-166">You also need toomake sure that hello input and output for reducer keep hello same schema.</span></span>

<span data-ttu-id="60bcb-167">Attributet för rekursiv reducer:</span><span class="sxs-lookup"><span data-stu-id="60bcb-167">Attribute of recursive reducer:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]

<span data-ttu-id="60bcb-168">Exempel:</span><span class="sxs-lookup"><span data-stu-id="60bcb-168">Code example:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a><span data-ttu-id="60bcb-169">Alternativ 2: Använd om möjligt radnivå combiner läge</span><span class="sxs-lookup"><span data-stu-id="60bcb-169">Option 2: Use row-level combiner mode, if possible</span></span>

<span data-ttu-id="60bcb-170">Liknande toohello ROWCOUNT-tipset för specifika förvrängd nyckeln koppling fall combiner läge försöker toodistribute enorma förvrängd nyckel värde anger toomultiple formhörnen så att hello arbete kan köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="60bcb-170">Similar toohello ROWCOUNT hint for specific skewed-key join cases, combiner mode tries toodistribute huge skewed-key value sets toomultiple vertices so that hello work can be executed concurrently.</span></span> <span data-ttu-id="60bcb-171">Combiner läge inte kan lösa problem med förskjutning av data, men den kan erbjuda för stora förvrängd nyckel värde anger ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="60bcb-171">Combiner mode can’t resolve data-skew issues, but it can offer some additional help for huge skewed-key value sets.</span></span>

<span data-ttu-id="60bcb-172">Som standard är hello combiner läge Full, vilket innebär att hello kvar raduppsättningen och inte kan skiljas högra raden set.</span><span class="sxs-lookup"><span data-stu-id="60bcb-172">By default, hello combiner mode is Full, which means that hello left row set and right row set cannot be separated.</span></span> <span data-ttu-id="60bcb-173">Inställningen hello läget som inre-vänster/höger kan radnivå koppling.</span><span class="sxs-lookup"><span data-stu-id="60bcb-173">Setting hello mode as Left/Right/Inner enables row-level join.</span></span> <span data-ttu-id="60bcb-174">hello system avgränsar hello motsvarande rad uppsättningar och distribuerar dem till flera formhörnen som körs parallellt.</span><span class="sxs-lookup"><span data-stu-id="60bcb-174">hello system separates hello corresponding row sets and distributes them into multiple vertices that run in parallel.</span></span> <span data-ttu-id="60bcb-175">Innan du konfigurerar hello combiner läge Tänk tooensure som hello motsvarande rad anger kan delas.</span><span class="sxs-lookup"><span data-stu-id="60bcb-175">However, before you configure hello combiner mode, be careful tooensure that hello corresponding row sets can be separated.</span></span>

<span data-ttu-id="60bcb-176">hello följande exempel visar en uppsättning åtskilda vänstra raden.</span><span class="sxs-lookup"><span data-stu-id="60bcb-176">hello example that follows shows a separated left row set.</span></span> <span data-ttu-id="60bcb-177">Varje rad i utdata beror på en inkommande rad från hello vänster, och eventuellt på alla rader från hello med hello samma nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="60bcb-177">Each output row depends on a single input row from hello left, and it potentially depends on all rows from hello right with hello same key value.</span></span> <span data-ttu-id="60bcb-178">Om du anger hello combiner läge som vänster hello system avgränsar hello enorma vänster-raden i små och tilldelar dem toomultiple formhörnen.</span><span class="sxs-lookup"><span data-stu-id="60bcb-178">If you set hello combiner mode as left, hello system separates hello huge left-row set into small ones and assigns them toomultiple vertices.</span></span>

![Combiner läge bild](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
><span data-ttu-id="60bcb-180">Om du ställer in hello fel combiner läge hello kombination är mindre effektivt och hello resultaten kan vara felaktigt.</span><span class="sxs-lookup"><span data-stu-id="60bcb-180">If you set hello wrong combiner mode, hello combination is less efficient, and hello results might be wrong.</span></span>

<span data-ttu-id="60bcb-181">Attribut för combiner läge:</span><span class="sxs-lookup"><span data-stu-id="60bcb-181">Attributes of combiner mode:</span></span>

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all hello input rows from left and right with hello same key value.

- <span data-ttu-id="60bcb-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Varje rad i utdata beror på en inkommande rad från hello vänster (och potentiellt alla rader från hello hello med samma nyckelvärde).</span><span class="sxs-lookup"><span data-stu-id="60bcb-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Every output row depends on a single input row from hello left (and potentially all rows from hello right with hello same key value).</span></span>

- <span data-ttu-id="60bcb-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): varje rad i utdata beror på en inkommande rad från hello rätt (och potentiellt alla rader från hello vänster med hello samma nyckelvärde).</span><span class="sxs-lookup"><span data-stu-id="60bcb-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): Every output row depends on a single input row from hello right (and potentially all rows from hello left with hello same key value).</span></span>

- <span data-ttu-id="60bcb-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Varje rad i utdata beror på en inkommande rad från vänster hello och hello med hello samma värde.</span><span class="sxs-lookup"><span data-stu-id="60bcb-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Every output row depends on a single input row from hello left and hello right with hello same value.</span></span>

<span data-ttu-id="60bcb-185">Exempel:</span><span class="sxs-lookup"><span data-stu-id="60bcb-185">Code example:</span></span>

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
