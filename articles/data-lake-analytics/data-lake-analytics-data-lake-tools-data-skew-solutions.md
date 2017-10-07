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
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a>Lösa problem med en förskjutning av data med hjälp av Azure Data Lake-verktyg för Visual Studio

## <a name="what-is-data-skew"></a>Vad är data skeva?

Kort anges är data förskjutning ett mycket representeras värde. Anta att du har tilldelat 50 skatt granskare tooaudit deklarationer, en förarprövaren för varje USA tillstånd. Hej Wyoming förarprövaren har eftersom det hello population är liten, lite toodo. I California, men sparas hello förarprövaren hårt på grund av stora ifyllning av hello tillstånd.
    ![Förskjutning av data problemet exempel](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)

I vårt scenario distribueras hello ojämnt data på alla skatt granskare, vilket innebär att vissa granskare måste arbeta mer än andra. Situationer som hello skatt förarprövaren exempel här uppstår ofta i dina egna jobbet. Ett hörn hämtar mycket mer data än dess peer-datorer, en situation som gör hello vertex arbeta mer än andra och som slutligen saktar ned en hela jobbet hello i mer tekniska villkor. Vad är försämras kan hello jobb misslyckas eftersom formhörnen kanske till exempel en begränsning av 5 timmar runtime och en begränsning med 6 GB minne.

## <a name="resolving-data-skew-problems"></a>Lösa problem för förskjutning av data

Azure Data Lake-verktyg för Visual Studio kan hjälpa dig att upptäcka om jobbet har problem med en förskjutning av data. Om det finns ett problem, kan du lösa det. genom hello lösningar i det här avsnittet.

## <a name="solution-1-improve-table-partitioning"></a>Lösning 1: Förbättra Tabellpartitionering

### <a name="option-1-filter-hello-skewed-key-value-in-advance"></a>Alternativ 1: Filter hello förvrängd nyckelvärdet i förväg

Om det inte påverkar affärslogik, kan du filtrera hello högre frekvens värden i förväg. Till exempel om det finns en mängd 000 000 000 i kolumnen GUID kan kanske du inte vill tooaggregate värdet. Innan du sammanställd, du kan skriva ”var GUID! =” 000 – 000 000 ”” toofilter hello hög frekvens värde.

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a>Alternativ 2: Välj en annan partition eller distribution nyckel

Om du vill att endast toocheck hello skatt audit arbetsbelastning alla över hello land i föregående exempel hello kan du förbättra hello Datadistribution genom att markera hello ID-nummer som din nyckel. Välja en annan partition eller distribution tangent kan ibland distribuera hello data jämnare, men du måste toomake till att detta val inte påverkar din affärslogik. Till exempel toocalculate hello skatt summan för respektive tillstånd, kanske du vill toodesignate _tillstånd_ som hello partitionsnyckel. Om du fortsätter tooexperience det här problemet kan du prova att använda alternativ 3.

### <a name="option-3-add-more-partition-or-distribution-keys"></a>Alternativ 3: Lägg till flera nycklar för partitionen eller distribution

I stället för endast _tillstånd_ som en partitionsnyckel kan du använda mer än en nyckel för partitionering. Anta till exempel att lägga till _postnummer_ som en ytterligare partition viktiga tooreduce data-partition storlekar och distribuera hello data mer jämnt.

### <a name="option-4-use-round-robin-distribution"></a>Alternativ 4: Använd resursallokering

Om du inte hittar en lämplig nyckel för partition och distribution, kan du försöka toouse resursallokering. Resursallokering behandlas alla rader lika och placerar slumpmässigt dem i motsvarande buckets. hello data hämtar jämnt, men det förlorar ort information nackdelen som kan också minska jobbet prestandan för vissa åtgärder. Om du gör sammanställning för hello skeva nyckeln ändå, behålls dessutom hello data förskjutning problem. toolearn mer om resursallokering, se hello U-SQL-tabell distributioner i avsnittet [CREATE TABLE (U-SQL): skapa en tabell med schemat](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).

## <a name="solution-2-improve-hello-query-plan"></a>Lösning 2: Förbättra hello frågeplan

### <a name="option-1-use-hello-create-statistics-statement"></a>Alternativ 1: Använd hello CREATE STATISTICS-uttryck

U-SQL ger hello CREATE STATISTICS-uttryck på tabeller. Den här instruktionen ger mer information toohello frågeoptimeraren om hello dataegenskaper, till exempel värdet distribution, som lagras i en tabell. För de flesta frågor genererar hello frågeoptimeraren redan hello nödvändiga statistik för en frågeplan med hög kvalitet. Ibland kan behöva du tooimprove frågeprestanda genom att skapa ytterligare statistik med CREATE STATISTICS eller genom att ändra hello frågans design. Mer information finns i hello [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) sidan.

Exempel:

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
>Statistikinformation uppdateras inte automatiskt. Om du uppdaterar hello data i en tabell utan att återskapa hello statistik kan neka hello frågeprestanda.

### <a name="option-2-use-skewfactor"></a>Alternativ 2: Använd SKEWFACTOR

Om du vill toosum hello skatt för respektive tillstånd, måste du använda GROUP BY tillstånd, en metod som inte undviker hello data förskjutning problem. Du kan dock ange en ledtråd för data i din fråga tooidentify data skeva i nycklar så att hello optimering kan förbereda en plan för körning av du.

Vanligtvis kan du ange hello-parameter som 0,5 och 1, med 0,5 vilket innebär inte mycket skeva och 1 betydelse tunga förskjutning. Eftersom hello tipset påverkar åtgärdsplan optimering för hello aktuella instruktionen och alla underordnade instruktioner, vara att tooadd hello tipset innan hello potentiella förvrängd key-wise aggregering.

    SKEWFACTOR (columns) = x

    Provides a hint that hello given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

Exempel:

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

### <a name="option-3-use-rowcount"></a>Alternativ 3: Använd ROWCOUNT  
Dessutom tooSKEWFACTOR för specifika förvrängd-nyckeln ansluta till fall, om du vet att hello andra domänanslutna raden är liten, berätta hello optimering genom att lägga till ett RADANTAL tips i hello U-SQL-instruktionen innan koppling. Det här sättet optimering kan välja en broadcast koppling strategi toohelp förbättra prestanda. Tänk på att ROWCOUNT matchar inte hello data förskjutning problem, men den kan erbjuda ytterligare hjälp.

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

Exempel:

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

## <a name="solution-3-improve-hello-user-defined-reducer-and-combiner"></a>Lösning 3: Förbättra hello användardefinierade reducer och combiner

Ibland kan du skriva en användardefinierad operatorn toodeal med komplicerad Processlogik och en bra reducer och combiner kan minska problem förskjutning av data i vissa fall.

### <a name="option-1-use-a-recursive-reducer-if-possible"></a>Alternativ 1: Använd om möjligt en rekursiv reducer

Som standard körs en användardefinierad reducer i icke-rekursiva läge, vilket innebär att minskar arbete för en nyckel har distribuerats till en enda nod. Men om dina data är förvrängd hello stora datauppsättningar kan bearbetas i en enda nod och kör under lång tid.

tooimprove prestanda du kan lägga till ett attribut i koden toodefine reducer toorun i rekursiva läge. Sedan kan hello stora datauppsättningar distribuerade toomultiple formhörnen och köras parallellt, vilket förbättrar ditt jobb.

toochange en icke-rekursiva reducer toorecursive måste toomake till att din algoritmen är kopplat. Till exempel hello summan är kopplat och hello median är inte. Du måste också till att hello indata och utdata för reducer behåller hello toomake samma schema.

Attributet för rekursiv reducer:

    [SqlUserDefinedReducer(IsRecursive = true)]

Exempel:

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a>Alternativ 2: Använd om möjligt radnivå combiner läge

Liknande toohello ROWCOUNT-tipset för specifika förvrängd nyckeln koppling fall combiner läge försöker toodistribute enorma förvrängd nyckel värde anger toomultiple formhörnen så att hello arbete kan köras samtidigt. Combiner läge inte kan lösa problem med förskjutning av data, men den kan erbjuda för stora förvrängd nyckel värde anger ytterligare hjälp.

Som standard är hello combiner läge Full, vilket innebär att hello kvar raduppsättningen och inte kan skiljas högra raden set. Inställningen hello läget som inre-vänster/höger kan radnivå koppling. hello system avgränsar hello motsvarande rad uppsättningar och distribuerar dem till flera formhörnen som körs parallellt. Innan du konfigurerar hello combiner läge Tänk tooensure som hello motsvarande rad anger kan delas.

hello följande exempel visar en uppsättning åtskilda vänstra raden. Varje rad i utdata beror på en inkommande rad från hello vänster, och eventuellt på alla rader från hello med hello samma nyckelvärde. Om du anger hello combiner läge som vänster hello system avgränsar hello enorma vänster-raden i små och tilldelar dem toomultiple formhörnen.

![Combiner läge bild](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
>Om du ställer in hello fel combiner läge hello kombination är mindre effektivt och hello resultaten kan vara felaktigt.

Attribut för combiner läge:

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all hello input rows from left and right with hello same key value.

- SqlUserDefinedCombiner(Mode=CombinerMode.Left): Varje rad i utdata beror på en inkommande rad från hello vänster (och potentiellt alla rader från hello hello med samma nyckelvärde).

- qlUserDefinedCombiner(Mode=CombinerMode.Right): varje rad i utdata beror på en inkommande rad från hello rätt (och potentiellt alla rader från hello vänster med hello samma nyckelvärde).

- SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Varje rad i utdata beror på en inkommande rad från vänster hello och hello med hello samma värde.

Exempel:

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
