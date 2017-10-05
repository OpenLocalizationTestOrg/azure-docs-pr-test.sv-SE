---
title: "Skapa funktioner för data i ett Hadoop-kluster med hjälp av Hive-frågor | Microsoft Docs"
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
ms.openlocfilehash: e027a6ffcb63868be13432870e484c5cbf2eef4b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Skapa funktioner för data i ett Hadoop-kluster med hjälp av Hive-frågor
Det här dokumentet beskrivs hur du skapar funktioner för data som lagras i ett Azure HDInsight Hadoop-kluster med hjälp av Hive-frågor. Dessa Hive-frågor använda inbäddade Hive användardefinierade funktioner (UDF), skript som tillhandahålls.

De åtgärder som behövs för att skapa funktioner kan vara minnesintensiva. Prestanda för Hive-frågor blir mer kritiska i sådana fall och kan förbättras genom att justera vissa parametrar. Justering av dessa parametrar beskrivs i den sista delen.

Exempel på de frågor som presenteras som är specifika för den [NYC Taxi resa Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarier finns också i [GitHub-lagringsplatsen](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). De här frågorna har angetts dataschemat redan och är redo att skickas till kör. I den sista delen beskrivs också parametrar som användare kan justera så att prestanda för Hive-frågor kan förbättras.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Detta **menyn** länkar till avsnitt som beskriver hur du skapar funktioner för data i olika miljöer. Den här uppgiften är ett steg i den [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har:

* Skapa ett Azure storage-konto. Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Etablera ett anpassat Hadoop-kluster med HDInsight-tjänst.  Om du behöver mer information, se [anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).
* Data har överförts till Hive-tabeller i Azure HDInsight Hadoop-kluster. Om den inte har följer [skapa och läsa in data till Hive-tabeller](machine-learning-data-science-move-hive-tables.md) först överföra data till Hive-tabeller.
* Aktivera fjärråtkomst till klustret. Om du behöver mer information, se [komma åt det Head nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="hive-featureengineering"></a>Funktionen Generation
I det här avsnittet beskrivs flera exempel på sätt som funktioner kan genereras med hjälp av Hive-frågor. När du har genererat ytterligare funktioner, kan du lägga till dem som kolumner i den befintliga tabellen eller skapa en ny tabell med ytterligare funktioner och primär nyckel, vilket kan sedan kopplas till den ursprungliga tabellen. Här följer exempel visas:

1. [Frekvens baserat funktionen Generation](#hive-frequencyfeature)
2. [Riskerna med Kategoriska variabler i binär klassificering](#hive-riskfeature)
3. [Extrahera funktioner från Datetime Field](#hive-datefeatures)
4. [Extrahera funktioner från textfält](#hive-textfeatures)
5. [Beräkna avståndet mellan GPS koordinater](#hive-gpsdistance)

### <a name="hive-frequencyfeature"></a>Frekvens baserat funktionen Generation
Det är ofta användbar för att beräkna frekvenserna av en kategoriska variabel eller frekvenser för vissa kombinationer av nivåerna från flera kategoriska variabler. Användare kan använda följande skript för att beräkna dessa frekvenser:

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
Vi behöver konvertera icke-numeriska kategoriska variabler till numeriska funktioner när modeller används bara ta numeriska funktioner i binär klassificering. Detta görs genom att ersätta alla icke-numeriska nivå med numeriska risk. I det här avsnittet visar vi några allmänna Hive-frågor som beräknar riskvärden (loggen oddsen) för en kategoriska variabel.

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

I det här exemplet variabler `smooth_param1` och `smooth_param2` är inställda på att utjämna riskvärden som beräknats med data. Intervallet mellan -Inf och Inf är risker. Risker > 0 anger att sannolikheten att målet är lika med 1 är större än 0,5.

Efter risken beräknade tabell, användare kan tilldela riskvärden till en tabell genom att anslutas med tabellen risk. Anslutande Hive-frågan har angetts i föregående avsnitt.

### <a name="hive-datefeatures"></a>Extrahera funktioner från Datetime-Fields
Hive levereras med en uppsättning UDF: er för bearbetning av datetime-fält. I Hive, standardformatet för datum/tid är ”åååå-MM-dd 00:00:00 ' ('1970-01-01 12:21:32, till exempel). I det här avsnittet visar vi exempel som extraherar dagen i månaden, månaden från ett datetime-fält och andra exempel som konverteras en datetime-sträng i formatet än standardformatet till ett datetime-sträng i formatet.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Den här Hive-fråga förutsätter att den *&#60; datetime-fält >* i standard datetime-format.

Om ett datetime-fält inte är i standardformatet, måste du konvertera datetime-fält till Unix-tidsstämpel först och sedan konvertera tidsstämpeln Unix till en datetime-sträng som är i standardformatet. När datetime är i formatet kan använda användare inbäddade datetime UDF: er att extrahera funktioner.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

I den här frågan, om den *&#60; datetime-fält >* har mönster som *03/26/2015 12:04:39*, *' &#60; mönstret för datetime-fält >'* ska vara `'MM/dd/yyyy HH:mm:ss'`. Om du vill testa den kan användare som köra

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Den *hivesampletable* i den här frågan är förinstallerat på alla Azure HDInsight Hadoop-kluster som standard när klustren etableras.

### <a name="hive-textfeatures"></a>Extrahera funktioner från textfält
När Hive-tabellen har ett textfält som innehåller en sträng med ord som är avgränsade med blanksteg, extraheras följande fråga längden på strängen och antalet ord i strängen.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="hive-gpsdistance"></a>Beräkna avstånd mellan GPS-koordinatuppsättningar
Frågan i det här avsnittet kan tillämpas direkt på NYC Taxi resa Data. Syftet med den här frågan är att visa hur du använder ett inbäddat matematiska funktioner i Hive för att generera funktioner.

De fält som används i den här frågan är GPS-koordinaterna för hämtning och dropoff platser, med namnet *hämtning\_longitud*, *hämtning\_latitud*,  *dropoff\_longitud*, och *dropoff\_latitud*. Frågorna som beräknar direkt avståndet mellan koordinaterna hämtning och dropoff är:

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

Matematiska formler som beräknar avståndet mellan två GPS-koordinater kan hittas på den <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">flyttbar typen skript</a> plats som skapats av Peter Lapisu. I sin Javascript funktionen `toRad()` är bara *lat_or_lon*pi/180 *, som konverterar grader till radianer. Här, *lat_or_lon* är latitud och longitud. Eftersom Hive inte ger funktionen `atan2`, men ger funktionen `atan`, `atan2` funktionen implementeras av `atan` funktion i Hive frågan ovan med definitionen i <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Skapa arbetsyta](./media/machine-learning-data-science-create-features-hive/atan2new.png)

En fullständig lista över Hive inbäddade UDF: er finns i den **inbyggda funktioner** avsnitt på den <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).  

## <a name="tuning"></a>Avancerade alternativ: finjustera Hive parametrar för att förbättra hastighet för frågan
Parametern standardinställningarna för Hive-kluster är kanske inte lämplig för Hive-frågor och de data som bearbetar frågorna. I det här avsnittet diskuterar vi vissa parametrar som användare kan justera som förbättrar prestandan för Hive-frågor. Användare måste lägga till parametern justera frågor innan frågor för bearbetning av data.

1. **Java heap utrymme**: för frågor som rör koppla stora datauppsättningar eller bearbetning långa poster **heap utrymmet börjar ta slut** är ett vanligt fel. Detta kan anpassas genom att ange parametrar *mapreduce.map.java.opts* och *mapreduce.task.io.sort.mb* till önskade värden. Här är ett exempel:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Den här parametern allokerar 4GB minne till Java heap utrymmet och även gör sortering effektivare genom att allokera mer minne för den. Det är en bra idé att spela med dessa allokeringar om det finns jobb felen som rör heap utrymme.

1. **DFS-blockstorlek** : den här parametern anger den minsta enheten av data som lagras i filsystemet. Exempelvis om DFS-blockstorlek är 128MB, sedan några data av storlek mindre än och upp till lagras 128MB i ett enda block när data som är större än 128MB tilldelas extra block. Om du väljer en liten blockstorlek medför stora kostnaderna i Hadoop eftersom noden namn har att bearbeta många fler begäranden för att hitta relevanta block som hör till filen. En rekommenderad inställning när behandlar gigabyte (eller större) data är:
   
        set dfs.block.size=128m;
2. **Optimera join-uttrycket i Hive** : vid anslutning till åtgärder i kartan/minska framework vanligtvis sker minska fas, ibland enorma vinster kan uppnås genom att schemalägga kopplingar i fasen mappning (även kallat ”mapjoins”). Om du vill dirigera Hive för att göra det när det är möjligt, kan vi ange:
   
        set hive.auto.convert.join=true;
3. **Anger antalet mappers till Hive** : medan Hadoop tillåter användaren att ange antalet förminskningsapparater, antal mappers är vanligtvis inte anges av användaren. Ett tips som gör att viss mån av kontrollen i det här antalet är att välja variablerna Hadoop *mapred.min.split.size* och *mapred.max.split.size* som storleken på varje mappning uppgiften avgörs av:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Normalt standardvärdet *mapred.min.split.size* är 0, som *mapred.max.split.size* är **Long.MAX** och *dfs.block.size* är 64MB. Som vi ser anges storleken på data justera parametrarna av ”inställningen” dem gör att vi kan justera antalet mappers används.
4. Några fler **avancerade alternativ** för att optimera Hive prestanda anges nedan. Dessa kan du ange det minne som allokerats för att mappa och minska uppgifter och kan vara användbar vid modifiera prestanda. Kontrollera Tänk på att den *mapreduce.reduce.memory.mb* får inte vara större än storlek för fysiskt minne för varje arbetsnod i Hadoop-kluster.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

