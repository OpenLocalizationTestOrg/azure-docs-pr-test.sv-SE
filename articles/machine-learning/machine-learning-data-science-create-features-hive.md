---
title: "aaaCreate funktioner för data i ett Hadoop-kluster med hjälp av Hive-frågor | Microsoft Docs"
description: "Exempel på Hive-frågor som genererar funktioner i data som lagras i ett Azure HDInsight Hadoop-kluster."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e8a94c71-979b-4707-b8fd-85b47d309a30
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 686282bf0fb84ea82758d3c5b7de2bd90f0cf159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Skapa funktioner för data i ett Hadoop-kluster med hjälp av Hive-frågor
Det här dokumentet beskrivs hur toocreate funktioner för data som lagras i ett Azure HDInsight Hadoop-kluster med hjälp av Hive-frågor. Dessa Hive-frågor använder inbäddade Hive användardefinierade funktioner (UDF), hello skript som tillhandahålls.

hello åtgärder som krävs för toocreate funktioner kan vara minnesintensiva. hello prestanda för Hive-frågor blir mer kritiska i sådana fall och kan förbättras genom att justera vissa parametrar. hello finjustering av dessa parametrar beskrivs i hello sista delen.

Exempel på hello frågor som visas är specifik toohello [NYC Taxi resa Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarier finns också i [GitHub-lagringsplatsen](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Dessa frågor som redan har angivna dataschemat och är redo toobe som skickats toorun. Under hello slutliga beskrivs också parametrar som användare kan justera så att hello prestanda för Hive-frågor kan förbättras.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Detta **menyn** länkar tootopics som beskriver hur toocreate funktioner för data i olika miljöer. Den här uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har:

* Skapa ett Azure storage-konto. Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Etablera ett anpassat Hadoop-kluster med hello HDInsight-tjänst.  Om du behöver mer information, se [anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).
* hello data har laddats upp tooHive tabeller i Azure HDInsight Hadoop-kluster. Om den inte har följer [skapa och läsa in tooHive datatabeller](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tabeller först.
* Aktivera fjärråtkomst toohello klustret. Om du behöver mer information, se [åtkomst hello Head-nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="hive-featureengineering"></a>Funktionen Generation
I det här avsnittet beskrivs flera exempel på hello sätt där funktioner kan genereras med hjälp av Hive-frågor. När du har genererat ytterligare funktioner, kan du lägga till dem som kolumner toohello befintlig tabell eller skapa en ny tabell med hello ytterligare funktioner och primärnyckeln, som sedan kan förenas med hello ursprungliga tabellen. Här följer hello exempel visas:

1. [Frekvens baserat funktionen Generation](#hive-frequencyfeature)
2. [Riskerna med Kategoriska variabler i binär klassificering](#hive-riskfeature)
3. [Extrahera funktioner från Datetime Field](#hive-datefeatures)
4. [Extrahera funktioner från textfält](#hive-textfeatures)
5. [Beräkna avståndet mellan GPS koordinater](#hive-gpsdistance)

### <a name="hive-frequencyfeature"></a>Frekvens baserat funktionen Generation
Det är ofta användbara toocalculate hello frekvenser hello nivåer av en kategoriska variabel eller hello frekvenser för vissa kombinationer av nivåerna från flera kategoriska variabler. Användare kan använda följande skript toocalculate hello dessa frekvenser:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <a name="hive-riskfeature"></a>Riskerna med Kategoriska variabler i binär klassificering
I binär klassificering behöver vi tooconvert icke-numeriska kategoriska variabler i numeriska funktioner när hello modeller används bara ta numeriska funktioner. Detta görs genom att ersätta alla icke-numeriska nivå med numeriska risk. I det här avsnittet visar vi några allmänna Hive-frågor som beräknar hello riskvärden (loggen oddsen) för en kategoriska variabel.

        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

I det här exemplet variabler `smooth_param1` och `smooth_param2` beräknas toosmooth hello riskvärden från hello data. Intervallet mellan -Inf och Inf är risker. Risker > 0 anger att hello sannolikheten att hello mål är lika too1 är större än 0,5.

När hello risk tabell beräknas tilldela användare risk värden tooa tabell genom att anslutas med hello risk tabell. anslutande hello Hive-frågan har angetts i föregående avsnitt.

### <a name="hive-datefeatures"></a>Extrahera funktioner från Datetime-Fields
Hive levereras med en uppsättning UDF: er för bearbetning av datetime-fält. I Hive, hello standardformatet för datum/tid är ”åååå-MM-dd 00:00:00 ' ('1970-01-01 12:21:32, till exempel). I det här avsnittet visar vi exempel som extraheras hello dagen i en månad, hello månad från ett datetime-fält och andra exempel som konverteras en datetime-sträng i formatet än hello standard format tooa datetime-sträng i formatet.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Den här Hive-fråga förutsätter att hello *&#60; datetime-fält >* i hello standard datetime-format.

Om ett datetime-fält är inte i hello standardformat, du behöver tooconvert hello datetime-fält i Unix tidsstämpel först och sedan konvertera hello Unix tid stämpel tooa datetime-sträng som är i hello standard format. När hello datetime är i formatet, kan användare använda hello inbäddade datetime UDF: er tooextract funktioner.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of hello datetime field>'))
        from <databasename>.<tablename>;

I den här frågan om hello *&#60; datetime-fält >* har hello mönster som *03/26/2015 12:04:39*, hello *' &#60; mönstret för hello datetime-fält >'* ska vara `'MM/dd/yyyy HH:mm:ss'`. tootest, användare kan köra

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Hej *hivesampletable* i den här frågan är förinstallerat på alla Azure HDInsight Hadoop-kluster som standard när hello kluster etableras.

### <a name="hive-textfeatures"></a>Extrahera funktioner från textfält
När hello Hive-tabellen har ett textfält som innehåller en sträng med ord som är avgränsade med blanksteg, extraheras hello följande fråga hello längd hello sträng och hello antalet ord i hello-sträng.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="hive-gpsdistance"></a>Beräkna avstånd mellan GPS-koordinatuppsättningar
hello-fråga som anges i det här avsnittet kan vara direkt toohello NYC Taxi resa Data. hello syftet med den här frågan är tooshow hur tooapply inbäddad matematiska funktioner i Hive toogenerate funktioner.

hello fält som används i den här frågan är hello GPS-koordinater för hämtning och dropoff platser, med namnet *hämtning\_longitud*, *hämtning\_latitud*,  *dropoff\_longitud*, och *dropoff\_latitud*. hello-frågor som beräknar hello direkt avståndet mellan hello hämtning och dropoff koordinater är:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

hello matematiska formler som beräknar hello avståndet mellan två GPS-koordinater kan hittas på hello <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">flyttbar typen skript</a> plats som skapats av Peter Lapisu. I sin Javascript hello funktionen `toRad()` är bara *lat_or_lon*pi/180 *, som konverterar tooradians grader. Här, *lat_or_lon* är hello latitud och longitud. Eftersom Hive inte ger hello funktionen `atan2`, men ger hello funktionen `atan`, hello `atan2` funktionen implementeras av `atan` funktion i hello ovan Hive-fråga med hello definitionen i <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank"> Wikipedia</a>.

![Skapa arbetsyta](./media/machine-learning-data-science-create-features-hive/atan2new.png)

En fullständig lista över Hive inbäddade UDF: er finns i hello **inbyggda funktioner** avsnittet hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).  

## <a name="tuning"></a>Avancerade alternativ: finjustera Hive parametrar tooImprove frågan hastighet
Hej standardparametern inställningarna för Hive-kluster inte kan vara lämplig för hello Hive-frågor och hello data som bearbetar hello frågor. I det här avsnittet diskuterar vi vissa parametrar som användare kan justera som förbättrar prestanda hello Hive-frågor. Användarna måste tooadd hello parametrar frågor innan hello frågor för bearbetning av data.

1. **Java heap utrymme**: för frågor som rör koppla stora datauppsättningar eller bearbetning långa poster **heap utrymmet börjar ta slut** är en av hello vanliga fel. Detta kan anpassas genom att ange parametrar *mapreduce.map.java.opts* och *mapreduce.task.io.sort.mb* toodesired värden. Här är ett exempel:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Den här parametern allokerar 4GB minne tooJava heap utrymme så att det blir sortering effektivare genom att allokera mer minne för den. Om det inte finns något jobb fel fel relaterade tooheap utrymme är det en bra idé tooplay med dessa allokeringar.

1. **DFS-blockstorlek** : den här parametern anger hello minsta enheten som hello filen system lagrar. Exempelvis om hello DFS blockstorleken är 128MB, lagras sedan några data av storleken och mindre än in too128MB i ett enda block när data som är större än 128MB tilldelas extra block. Om du väljer en liten blockstorlek medför stora kostnaderna i Hadoop eftersom hello namn har tooprocess många fler begäranden toofind hello relevanta block som rör toohello filen. En rekommenderad inställning när behandlar gigabyte (eller större) data är:
   
        set dfs.block.size=128m;
2. **Optimera join-uttrycket i Hive** : Anslut till åtgärder i hello kartan/minska framework vanligtvis sker i hello minska fasen ibland, enorma vinster kan uppnås genom att schemalägga kopplingar i hello kartan fasen (kallas även ”mapjoins”). toodirect Hive toodo detta när det är möjligt, vi kan ange:
   
        set hive.auto.convert.join=true;
3. **Anger hello antal mappers tooHive** : medan Hadoop tillåter hello användaren tooset hello antalet förminskningsapparater, hello antal mappers är vanligtvis inte anges av hello användare. Ett tips som gör att viss mån av kontrollen i det här antalet är toochoose hello Hadoop variabler *mapred.min.split.size* och *mapred.max.split.size* som hello storleken på varje mappning uppgiften avgörs av:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Normalt hello standardvärdet *mapred.min.split.size* är 0, som *mapred.max.split.size* är **Long.MAX** och *dfs.block.size* är 64MB. Som vi ser angivna hello datastorleken justera parametrarna av ”inställningen” dem kan vi tootune hello antalet mappers används.
4. Några fler **avancerade alternativ** för att optimera Hive prestanda anges nedan. Dessa kan du tooset hello minne som allokerats toomap minska uppgifter och kan vara användbar vid modifiera prestanda. Kom ihåg att hello *mapreduce.reduce.memory.mb* får inte vara större än hello storlek för fysiskt minne för varje arbetsnod i hello Hadoop-kluster.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

