---
title: "hello Team av vetenskapliga data i praktiken - med ett Azure HDInsight Hadoop-kluster på en datamängd för 1 TB | Microsoft Docs"
description: "Med hjälp av hello Team datavetenskap Process för en slutpunkt till slutpunkt-scenario med en HDInsight Hadoop kluster toobuild och distribuera en modell med en stor offentligt tillgängliga (1 TB) datauppsättning"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 59b2af02e7840cb60a4b5b2f2c8ab0611df198ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>hello Team av vetenskapliga data i praktiken - med ett Azure HDInsight Hadoop-kluster på en datamängd för 1 TB

I den här genomgången visar hur hello Team datavetenskap Process i ett scenario för slutpunkt till slutpunkt med ett [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) toostore, utforska funktion tekniker och ned exempeldata från en hello offentligt tillgängliga [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datauppsättningar. Vi kan använda Azure Machine Learning toobuild en binär klassificering modell för den här informationen. Vi kan också visa hur toopublish en av dessa modeller som en webbtjänst.

Det är också möjligt toouse en IPython anteckningsboken tooaccomplish hello aktiviteter visas i den här genomgången. Användare som vill som den här metoden bör kontakta tootry hello [Criteo genomgången använder en Hive ODBC-anslutning](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) avsnittet.

## <a name="dataset"></a>Criteo Dataset-beskrivning
Hej Criteo data är en Klicka förutsägelse datamängd som är cirka 370GB gzip komprimerade TVS-filer (~1.3TB okomprimerade), som består av fler än 4.3 miljard poster. Det hämtas från 24 dagar i klickar du på data som har gjorts tillgängliga av [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). I informationssyfte hello data forskare, vi har uppackade data tillgängliga toous tooexperiment med.

Varje post i den här datauppsättningen innehåller 40 kolumner:

* hello första kolumnen är en etikett-kolumn som anger om en användare klickar på en **lägga till** (värdet 1) eller inte på en (värde 0)
* Nästa 13 kolumner är numeriska, och
* senaste 26 är kategoriska kolumner

hello-kolumner är anonym och använder en serie av uppräknade namn: ”Kol1” (för hello etikett kolumn) för ' Col40 ”(för hello senaste kategoriska kolumn).            

Här är ett utdrag ur hello först 20 kolumner med två observationer (rader) från den här datauppsättningen:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Det finns värden som saknas i båda hello numeriska och kategoriska kolumnerna i denna dataset. Vi beskriver en enkel metod för att hantera hello värden som saknas. Ytterligare information om hello data beskrivs när vi lagrar dem i Hive-tabeller.

**Definition:** *klickningar (CTR):* detta är hello procent klick i hello data. I den här Criteo datauppsättningen är hello CTR ungefär 3.3% eller 0.033.

## <a name="mltasks"></a>Exempel på förutsägelse uppgifter
Två exempel förutsägelse problem åtgärdas i den här genomgången:

1. **Binär klassificering**: beräknar om en användare klickar på en Lägg till:
   
   * Klass 0: Ingen klickar du på
   * Klass 1: Klicka på
2. **Regression**: beräknar hello sannolikheten för en ad Klicka på funktioner för användaren.

## <a name="setup"></a>Ange ett HDInsight Hadoop-kluster för datavetenskap
**Obs:** detta är vanligtvis en **Admin** aktivitet.

Ställ in Azure datavetenskap miljön för att skapa förutsägelseanalyslösningar med HDInsight-kluster i tre steg:

1. [Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md): det här lagringskontot är används toostore data i Azure Blob Storage. Här lagras hello data används i HDInsight-kluster.
2. [Anpassa Azure HDInsight Hadoop-kluster för datavetenskap](machine-learning-data-science-customize-hadoop-cluster.md): det här steget skapar ett Azure HDInsight Hadoop-kluster med 64-bitars Anaconda Python 2.7 installerad på alla noder. Det finns två viktiga steg (beskrivs i det här avsnittet) toocomplete när du anpassar hello HDInsight-kluster.
   
   * Du måste länka hello storage-konto som skapades i steg 1 med ditt HDInsight-kluster när den skapas. Det här lagringskontot används för att komma åt data som kan bearbetas i hello kluster.
   * Du måste aktivera fjärråtkomst toohello huvudnod hello klustret när den har skapats. Kom ihåg hello fjärråtkomst autentiseringsuppgifter du anger här (skiljer sig från de som anges för hello klustret när skapades): du behöver toocomplete hello följande procedurer.
3. [Skapa en arbetsyta för Azure ML](machine-learning-create-workspace.md): den här Azure Machine Learning-arbetsytan används för att skapa machine learning-modeller efter en inledande datagranskning och ned provtagning på hello HDInsight-kluster.

## <a name="getdata"></a>Hämta och använda data från en offentlig källa
Hej [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset kan nås genom att klicka på hello länk, acceptera hello villkor för användning och tillhandahåller ett namn. En ögonblicksbild av hur det ser ut visas här:

![Acceptera Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Klicka på **Fortsätt tooDownload** tooread mer om hello datauppsättningen och dess tillgänglighet.

hello data finns i en offentlig [Azure-blobblagring](../storage/blobs/storage-dotnet-how-to-use-blobs.md) plats: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. Hej ”wasb” refererar tooAzure Blob-lagringsplats. 

1. hello data i den här offentliga blob storage består av tre undermappar uppackade data.
   
   1. Hej undermapp *raw /-beräkning/* innehåller hello första 21 dagars data - från dag\_00 tooday\_20
   2. Hej undermapp *raw-tåg/* består av en dag av data, dag\_21
   3. Hej undermapp *rådata/test/* består av två dagars data, dag\_22 och dag\_23
2. För de som vill toostart med hello rådata gzip data, de är också tillgängliga i hello huvudsakliga mappen *rådata /* som day_NN.gz, där NN går från 00 too23.

En annan metod tooaccess utforska och modellerar data som inte kräver någon lokal hämtningar beskrivs senare i den här genomgången när vi skapar Hive-tabeller.

## <a name="login"></a>Logga in toohello klustret headnode
toolog i toohello headnode i hello kluster, Använd hello [Azure-portalen](https://ms.portal.azure.com) toolocate hello klustret. Klicka på hello HDInsight Elefant ikon på hello kvar och dubbelklicka på hello namnet på klustret. Navigera toohello **Configuration** fliken, klicka på ikonen hello Anslut på hello längst ned på sidan för hello och ange dina autentiseringsuppgifter för fjärråtkomst när du uppmanas. Då kommer du toohello headnode hello-klustret.

Här är en typisk första inloggningen toohello klustret headnode ser ut:

![Logga in toocluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)

Vi kan se hello ”Hadoop kommandoraden”, som våra bestämmer hög grad för hello datagranskning hello vänster. Vi kan också se två användbara webbadresserna - ”Hadoop Yarn Status” och ”Hadoop namn nod”. Hej yarn status URL visar jobbförloppet och hello namn nod URL ger information om hello klusterkonfigurationen.

Nu vi ställs in och klara toobegin första delen av genomgången hello: datagranskning med Hive och förbereda data för Azure Machine Learning.

## <a name="hive-db-tables"></a>Skapa Hive-databasen och tabeller
toocreate Hive-tabeller för våra Criteo dataset, öppna hello ***Hadoop kommandoraden*** på hello skrivbord hello huvudnod och ange hello Hive katalogen genom att ange hello kommando

    cd %hive_home%\bin

> [!NOTE]
> Kör alla Hive-kommandon i den här genomgången från hello Hive bin / directory-fråga. Detta hand tar om eventuella problem med sökväg automatiskt. Vi använder hello termer ”Hive directory prompt” ”, Hive bin / directory prompt”, och ”Hadoop-kommandoraden” synonymt.
> 
> [!NOTE]
> tooexecute Hive-fråga kan alltid använda hello följande kommandon:
> 
> 

        cd %hive_home%\bin
        hive

Efter hello Hive REPL visas med en ”hive >” Logga, bara klippa och klistra in hello frågan tooexecute den.

hello följande kod skapar en databas ”criteo” och sedan genererar 4 tabeller:

* en *tabell för generering av antal* bygger på dagar dag\_00 tooday\_20,
* en *tabell för användning som hello train dataset* bygger på dag\_21, och
* två *tabeller som hello testa datauppsättningar* bygger på dag\_22 och dag\_23 respektive.

Vi dela vår testdata i två olika tabeller eftersom en av hello dagar är helgdagar, och vi vill toodetermine om hello modell kan identifiera skillnader mellan en helgdag och icke-helgdagar från hello klickningar.

hello skript [exempel &#95; hive &#95; Skapa &#95; criteo &#95; databas &#95; och &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) visas här informationssyfte:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Vi Observera att dessa tabeller är externa som vi helt enkelt punkt tooAzure Blob Storage (wasb)-platser.

**Det finns två sätt tooexecute alla Hive-fråga som vi nämnt nu.**

1. **Med hjälp av hello Hive REPL kommandoradsverktyget**: hello först är tooissue en ”hive” kommandot och kopiera och klistra in en fråga på hello Hive REPL-kommandoraden. toodo detta, vill:
   
        cd %hive_home%\bin
        hive
   
     Nu på hello REPL kommandoradsverktyg, kopiera och klistra in körs hello fråga den.
2. **Spara frågor tooa fil- och köra hello kommando**: hello är andra toosave hello frågor tooa .hql fil ([exempel &#95; hive &#95; Skapa &#95; criteo &#95; databas &#95; och &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) och sedan problemet hello följande kommando tooexecute hello fråga:
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Bekräfta databas och tabell skapas
Därefter vi bekräfta hello skapandet av hello databasen med följande kommando från hello Hive bin hello / directory prompten:

        hive -e "show databases;"

Detta ger:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Det här bekräftar hello skapandet av hello ny databas, ”criteo”.

toosee vilka tabeller som vi skapade vi bara utfärda hello kommando här från hello Hive bin / directory prompten:

        hive -e "show tables in criteo;"

Vi kan sedan se hello följande utdata:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="exploration"></a>Datagranskning i Hive
Nu är du redo toodo vissa grundläggande datagranskning i Hive. Vi börjar med inventering hello antalet exempel i hello tåg och testa datatabeller.

### <a name="number-of-train-examples"></a>Antal train-exempel
Hej innehållet i [exempel &#95; hive &#95; antal &#95; train &#95; tabellen &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) visas här:

        SELECT COUNT(*) FROM criteo.criteo_train;

Detta ger:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Alternativt kan en också utfärda hello följande kommando från hello Hive bin / directory prompten:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-hello-two-test-datasets"></a>Antal test exemplen i hello två test datauppsättningar
Vi nu antal hello exemplen i hello två test datauppsättningar. Hej innehållet i [exempel &#95; hive &#95; antal &#95; criteo &#95; test &#95; dag &#95; 22 &#95; tabellen &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) finns här:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Detta ger:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Vi kan som vanligt också anropa hello skript från hello Hive bin / directory fråga genom att utfärda hello-kommando:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Slutligen kan vi se hello många test exemplen i hello testdata baserat på dag\_23.

hello kommandot toodo är liknande toohello som bara visas (se för[exempel &#95; hive &#95; antal &#95; criteo &#95; test &#95; dag &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Detta ger:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-hello-train-dataset"></a>Etikett distribution i hello train datauppsättning
hello etikett distribution i hello train dataset är av intresse. toosee kan vi visa innehållet i [exempel &#95; hive &#95; criteo &#95; etikett &#95; distri &#95; train &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Detta ger hello etikett distribution:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Observera att hello procentandelen positivt etiketter är ungefär 3.3% (konsekvent med hello ursprungliga datauppsättningen).

### <a name="histogram-distributions-of-some-numeric-variables-in-hello-train-dataset"></a>Histogram distributioner av vissa numeriska variabler i hello träna dataset
Vi kan använda registreringsdata intern ”histogram\_numeriska” toofind reda på vilka hello distribution av hello numeriska variabler som ser ut att fungera. Här följer hello innehållet i [exempel &#95; hive &#95; criteo &#95; histogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Detta ger hello följande:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

hello LATERALA Visa - Expandera kombination i Hive fungerar tooproduce en SQL-liknande utdata i stället för vanliga hello-listan. Observera att i hello denna tabell, hello första kolumnen motsvarar toohello bin center och hello andra toohello bin frekvens.

### <a name="approximate-percentiles-of-some-numeric-variables-in-hello-train-dataset"></a>Ungefärlig percentiler av vissa numeriska variabler i hello träna dataset
Är också hello beräkning av ungefärliga percentiler intressanta med numeriska variabler. Hive datorns inbyggda ”percentil\_ungefärlig” matchar det för oss. Hej innehållet i [exempel &#95; hive &#95; criteo &#95; ungefärliga &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) är:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Detta ger:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Vi Markera hello distribution av percentiler är nära relaterade toohello histogram distribution av en numerisk variabel vanligtvis.         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-hello-train-dataset"></a>Hitta antalet unika värden för vissa kategoriska kolumner i hello train datauppsättning
Fortsätter hello datagranskning vi nu finns för vissa kategoriska kolumner hello antalet unika värden som de vidtar. toodo kan vi visa innehållet i [exempel &#95; hive &#95; criteo &#95; unika &#95; värden &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Detta ger:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Vi Observera att Col15 19M unika värden! Använda naïve tekniker som ”en-hot kodning” tooencode sådana hög endimensionell kategoriska variabler är omöjligt. I synnerhet Vi förklarar och visar en kraftfull, robust teknik som kallas [inlärning med antal](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) för att effektivt hantera problemet.

Vi avbryta den här underavsnittet genom att titta på hello antalet unika värden för vissa andra kategoriska kolumner samt. Hej innehållet i [exempel &#95; hive &#95; criteo &#95; unika &#95; värden &#95; flera &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) är:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Detta ger:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Igen ser vi att förutom Col20, alla hello andra kolumner har många unika värden.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-hello-train-dataset"></a>Antal samtidigt förekomsten av par med kategoriska variabler i hello träna dataset

hello samtidigt förekomsten antal par kategoriska variabler är också av intresse. Detta kan fastställas med hjälp av hello koden i [exempel &#95; hive &#95; criteo &#95; parad &#95; kategoriska &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Vi omvänd ordning hello antal av deras förekomst och titta på hello högsta 15 i det här fallet. Detta ger oss:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Ned hello provdatauppsättningar för Azure Machine Learning
Med utforskade hello datauppsättningar och visas hur vi kan göra den här typen av exploatera några variabler (inklusive kombinationer), vi nu ned hello provdatauppsättningar så att vi kan bygga modeller i Azure Machine Learning. Återkalla åtgärdas hello vi fokusera på är: en uppsättning exempel attribut (funktionen värden från Col2 - Col40) får vi förutsäga om Kol1 är 0 (ingen klicka) eller 1 (klicka).

toodown exempel våra tåg och testa datauppsättningar too1% av hello ursprungliga storlek, vi använder registreringsdata interna RAND() funktion. Hej nästa skript [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; train &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) gör detta för hello train datauppsättningen:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Detta ger:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Hej skriptet [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; test &#95; dag &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) matchar för testdata, dag\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Detta ger:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Slutligen hello skriptet [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; test &#95; dag &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) matchar för testdata, dag\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Detta ger:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Med detta kan vi är klar toouse våra ned provtagning träna och testa datauppsättningar för att skapa modeller i Azure Machine Learning.

Det finns en sista viktig komponent innan vi vidare tooAzure Maskininlärning är av säkerhetsskäl hello antal tabell. I hello nästa underavsnitt diskuterar vi detta i viss detalj.

## <a name="count"></a>En kort beskrivning på hello antal tabell
Som vi såg har flera kategoriska variabler en mycket hög dimensionalitet. I vår genomgången presenterar vi en kraftfull teknik som kallas [inlärning med antal](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode dessa variabler på ett effektivt och robust sätt. Mer information om den här tekniken finns i länken hello.

[!NOTE]
>I den här genomgången ska fokusera vi på med antal tabeller tooproduce compact representationer av hög endimensionell kategoriska funktioner. Detta är inte hello endast hur tooencode kategoriska funktionerna; Mer information om andra metoder berörda användare kan checka ut [en-hot encoding](http://en.wikipedia.org/wiki/One-hot) och [hash-funktionen](http://en.wikipedia.org/wiki/Feature_hashing).
>

toobuild antal tabeller på hello antal data, vi använda hello data i hello mappen raw /-beräkning. I hello modeling avsnittet visar vi användarna tabeller hur toobuild dessa antal för kategoriska funktioner från grunden eller alternativt toouse en förskapad antal tabell för sina explorations. I det här när vi refererar för ”inbyggd antal tabeller”, vi menar med hello antalet tabeller som vi tillhandahåller. Detaljerade anvisningar om hur tooaccess dessa tabeller finns i hello nästa avsnitt.

## <a name="aml"></a>Skapa en modell med Azure Machine Learning
Vår modell processen i Azure Machine Learning för att bygga följer de här stegen:

1. [Hämta hello data från Hive-tabeller i Azure Machine Learning](#step1)
2. [Skapa hello experiment: Rensa hello data och featurize med antal tabeller](#step2)
3. [Skapa, tåg och hello poängsätta modell](#step3)
4. [Utvärdera hello modellen](#step4)
5. [Publicera hello modell som en webbtjänst](#step5)

Vi är nu redo toobuild modeller i Azure Machine Learning studio. Vår nedåt samplade data sparas som Hive-tabeller i hello kluster. Vi använder hello Azure Machine Learning **importera Data** modulen tooread dessa data. hello autentiseringsuppgifter tooaccess hello storage-konto för detta kluster finns i vilka sätt.

### <a name="step1"></a>Steg 1: Hämta data från Hive-tabeller i Azure Machine Learning modulen hello importera Data och markera ett maskininlärningsexperiment
Starta genom att välja en **+ ny** -> **EXPERIMENT** -> **tomt Experiment**. Sedan från hello **Sök** rutan hello längst upp till vänster, Sök efter ”importera Data”. Dra och släpp hello **importera Data** modul på toohello experiment arbetsytan (hello mellersta delen av hello-skärmen) toouse hello-modulen för dataåtkomst.

Det här är vad hello **importera Data** ser ut som att hämta data från hello Hive-tabell:

![Importera Data hämtar data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

För hello **importera Data** modulen hello värdena för hello-parametrar som finns angivna i hello bild är bara exempel på hello till värden du behöver tooprovide. Här är några allmänna riktlinjer om hur toofill Utdataparametern hello anges för hello **importera Data** modul.

1. Välj ”Hive-frågan” för **datakälla**
2. I hello **Hive databasfrågan** , en enkel, MARKERAR du kryssrutan * FROM < din\_databasen\_name.your\_tabell\_name >-räcker.
3. **Hcatalog server URI**: om klustret är ”abc” och sedan det här är bara: https://abc.azurehdinsight.net
4. **Hadoop användarkontonamnet**: hello användarnamn valt när hello idriftsättning hello klustret. (Inte hello fjärråtkomst användarnamn!)
5. **Hadoop lösenord**: hello lösenordet för hello användarnamn valt när hello idriftsättning hello klustret. (Inte hello fjärråtkomst lösenord!)
6. **Platsen för utdata**: Välj ”Azure”
7. **Azure lagringskontonamnet**: hello storage-konto som är associerade med hello-kluster
8. **Azure lagringskontonyckel**: hello nyckeln för hello storage-konto som är associerade med hello-kluster.
9. **Azure behållarnamn**: om hello klusternamnet är ”abc”, så det är helt enkelt ”abc”, vanligtvis.

En gång hello **importera Data** har avslutats hämtning av data (visas hello grön skalstreck på hello Module), spara informationen som en datamängd (med ett valfritt namn). Det ser ut:

![Importera Data spara data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Högerklicka på hello utgående port för hello **importera Data** modul. Nu visas en **Spara som dataset** alternativet och en **visualisera** alternativet. Hej **visualisera** alternativet om du visar 100 datarader hello, tillsammans med en högra panelen som är användbar för vissa sammanfattande statistik. toosave data, välja **Spara som dataset** och följ instruktionerna.

tooselect hello sparade dataset för användning i ett machine learning-experiment hitta hello datauppsättningar som använder hello **Sök** rutan som visas i följande bild hello. Sedan bara Skriv ut hello namn du gav hello dataset delvis tooaccess den och dra hello dataset till hello åtgärdsfönstervärdens. Släppa på hello åtgärdsfönstervärdens markeras den för användning i machine learning modellering.

![Drage dataset till hello åtgärdsfönstervärdens](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> Gör detta för både hello tåg och hello test datauppsättningar. Glöm inte heller toouse hello databasens namn och tabellnamn som du gav för detta ändamål. hello-värden som används i hello bild är enbart för bilden purposes.* *
> 
> 

### <a name="step2"></a>Steg 2: Skapa ett enkelt experiment i Azure Machine Learning toopredict klick / några klick
Vårt Azure ML-experiment ser ut så här:

![Machine Learning-experiment](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Vi nu undersöka hello viktiga komponenter av experimentet. Som en påminnelse om behöver vi toodrag våra sparade träna och testa datauppsättningar på tooour experimentet först.

#### <a name="clean-missing-data"></a>Rensa Data som saknas
Hej **Rensa Data som saknas** modulen har namnet antyder: data som saknas på ett sätt som kan vara användardefinierade rensas. Titta på den här modulen finns vi här:

![Rensa data som saknas](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Här kan valde vi tooreplace alla värden som saknas med 0. Det finns även andra alternativ som du kan se genom att titta på hello nedrullningsbara listorna i hello modul.

#### <a name="feature-engineering-on-hello-data"></a>Egenskapsval på hello data
Det kan finnas miljontals unika värden för vissa kategoriska funktioner i stora datauppsättningar. Med hjälp av naïve metoder, till exempel en hot kodning för att representera hög endimensionell kategoriska funktioner är helt unfeasible. I den här genomgången visar hur toouse antal funktioner med hjälp av inbyggda Azure Machine Learning-moduler toogenerate komprimera representationer av dessa hög endimensionell kategoriska variabler. hello-slutresultatet blir mindre modellen, snabbare utbildning och resultatmått som är helt jämförbar toousing andra metoder.

##### <a name="building-counting-transforms"></a>Skapa inventering transformeringar
toobuild antal funktioner, använder vi hello **skapa inventering transformera** modul som finns tillgängliga i Azure Machine Learning. hello modulen ser ut så här:

![Skapa inventering transformera modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![skapa inventering transformera modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT] 
> I hello **antal kolumner** vi rutan Ange de kolumner som vi vill tooperform räknar. Dessa normalt (som tidigare nämnts) hög endimensionell kategoriska kolumner. Hello början anges som hello Criteo datamängden har 26 kategoriska kolumner: från Col15 tooCol40. Här kan vi räkna på dem alla och ge sina index (från 15 too40 avgränsade med kommatecken enligt).
> 

toouse hello-modul i Hej MapReduce-läge (lämplig för stora datauppsättningar), vi behöver komma åt tooan HDInsight Hadoop-kluster (en som används för funktionen utforskning kan återanvändas för detta ändamål samt hello) och dess autentiseringsuppgifter. hello tidigare siffror visar hello ifyllda värdena se ut (ersätta hello värden för jämförelseändamål med de som är relevanta för dina egna användningsfall).

![Modulparametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

I ovanstående hello bild, visar vi hur tooenter hello indata blob plats. Den här platsen har hello data som reserverats för antalet tabeller som bygger på.

När den här modulen är klar vi spara hello-transformering för senare genom att högerklicka på hello modulen och välja hello **Spara som transformeringen** alternativ:

![Alternativet ”Spara som transformeringen”](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

I vårt experiment arkitektur som visas ovan motsvarar hello dataset ”ytransform2” exakt tooa Spara antal transformeringen. För hello resten av experimentet, antar vi att hello läsare användes en **skapa inventering transformera** modul på vissa data toogenerate antal och kan sedan använda dessa antal toogenerate antal funktioner på hello tåg och testa datauppsättningar.

##### <a name="choosing-what-count-features-tooinclude-as-part-of-hello-train-and-test-datasets"></a>Välja vilka antal funktioner tooinclude som en del av hello tåg och testa datauppsättningar
När vi har ett antal transformera redo hello användaren kan välja vilka funktioner tooinclude i sina tåg och testa datauppsättningar som använder hello **ändra antalet tabellen parametrar** modul. Vi bara att visa den här modulen här för fullständighetens skull, men i enkelhetens inte utnyttjar den i vårt experiment.

![Ändra antalet tabellen parametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

I det här fallet som kan ses vi har valt toouse bara hello log-oddsen och tooignore hello tillbaka av kolumn. Vi kan också ange parametrar som till exempel hello skräp bin tröskelvärdet, hur många startvärden föregående exempel tooadd för utjämning, och om toouse alla Laplacian-brus eller inte. Alla dessa avancerade funktioner och det är toobe anges att hello standardvärdena är en bra utgångspunkt för användare som är ny toothis typ av funktionen.

##### <a name="data-transformation-before-generating-hello-count-features"></a>DTS innan du genererar hello antal funktioner
Nu vi fokusera på en viktig aspekt Omforma våra tåg och testa data tidigare tooactually genererar antal funktioner. Observera att det finns två **köra R-skriptet** moduler som används innan vi installerar hello antal omvandla tooour data.

![Köra R-skriptet moduler](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Här är hello första R-skriptet:

![Första R-skriptet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)

I det här R-skriptet kan vi byta namn på vår kolumner toonames ”Kol1” för ”Col40”. Det beror på att hello antal transformeringen förväntar namnen på det här formatet.

I hello andra R-skriptet vi balansera hello distributionen mellan positiva och negativa klasser (klasserna 1 och 0 respektive) av negativt nedsampling hello-klassen. hello R-skript här visas hur toodo detta:

![Andra R-skriptet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

I det här enkla R-skriptet kan vi använda ”pos\_neg\_förhållandet” tooset hello mängden balans mellan hello positiva och negativa hello-klasser. Detta är viktigt toodo eftersom förbättra klassen obalans vanligtvis har prestandafördelar för klassificering problem där hello klassen distribution är förvrängd (återkallning att i vårt fall har vi 3.3% positivt klassen och 96.7% negativt klassen).

##### <a name="applying-hello-count-transformation-on-our-data"></a>Tillämpa hello antal omvandling på våra data
Slutligen kan vi använda hello **gäller omvandling** modulen tooapply hello antal transformeringar på vår tåg och testa datauppsättningar. Den här modulen tar hello Spara antal transformeringen som en indata hello träna och testa datauppsättningar som hello andra indata och returnerar data med antal funktioner. Den visas här:

![Tillämpa omvandling av modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-hello-count-features-look-like"></a>Hämtad hello antal funktioner ser ut
Det är instruktiva toosee vilka hello antal funktioner ser ut i vårt fall. Här beskrivs ett utdrag ur detta:

![Antal funktioner](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

I detta utdrag visar vi att för hello kolumner som vi räknas på, vi få fram hello och logga oddsen i tillägg tooany relevanta backoffs.

Vi är nu redo toobuild en Azure Machine Learning-modell med hjälp av dessa transformerade data. I nästa avsnitt hello visar vi hur detta kan göras.

### <a name="step3"></a>Steg 3: Skapa, träna och betygsätta hello modellen

#### <a name="choice-of-learner"></a>Valet av deltagaren
Vi måste först toochoose en deltagaren. Vi kan gå toouse ett två klassen förstärkta beslutsträd som våra deltagaren. Här följer hello standardalternativ för den här deltagaren:

![Två-Tvåklassförhöjt beslutsträd parametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Vi kan kommer toochoose hello standardvärden för vårt experiment. Vi Observera att hello som standard är vanligtvis beskrivande och ett bra sätt tooget snabb baslinjer på prestanda. Du kan förbättra prestanda genom omfattande parametrar om du väljer tooonce som du har en baslinje.

#### <a name="train-hello-model"></a>Hej träningsmodell
För träning, vi bara anropa en **Träningsmodell** modul. hello två indata tooit är hello två-Tvåklassförhöjt beslutsträd deltagaren och vår train datauppsättning. Detta visas här:

![Modulen för Train-modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-hello-model"></a>Hej poängmodell
När vi har en tränad modell vi är klara tooscore på hello testa dataset och tooevaluate dess prestanda. Vi kan göra detta med hjälp av hello **Poängmodell** modulen som visas i följande bild, tillsammans med hello en **utvärdera modell** modulen:

![Modulen Poängsätta modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)

### <a name="step4"></a>Steg 4: Utvärdera hello modellen
Slutligen vill vi tooanalyze modellen prestanda. Vanligtvis är två klass (binär) klassificering problem ett bra mått hello AUC. toovisualize kan vi koppla samman hello **Poängmodell** modulen tooan **utvärdera modell** -modul för detta. Klicka på **visualisera** på hello **utvärdera modell** modulen ger en bild som hello efter:

![Utvärdera modulen BDT modellen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

I binär (eller två klassen) klassificering problem, ett bra mått på förutsägelsefunktionen är hello Area Under kurvan (AUC). I följande, visar vi våra resultat med hjälp av den här modellen på vår testdata. tooget detta, högerklicka på hello utdataporten för hello **utvärdera modell** modulen och sedan **visualisera**.

![Visualisera modulen utvärdera modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step5"></a>Steg 5: Publicera hello modell som en webbtjänst
hello möjlighet toopublish en Azure Machine Learning-modell som webbtjänster med minsta möjliga ansträngning är en viktig funktion för att göra den allmänt tillgänglig. När det är klart kan vem som helst göra anrop toohello webbtjänst med indata att de behöver förutsägelser för och hello-webbtjänsten använder hello modellen tooreturn dessa förutsägelser.

toodo kan vi spara vår tränad modell som en Trained Model-objektet. Detta görs genom att högerklicka på hello **Träningsmodell** modulen och använder hello **Spara som Trained Model** alternativet.

Nu ska vi behöver toocreate ingående och utgående portar för våra webbtjänst:

* en port hämtar data i hello samma formuläret som hello data som vi behöver förutsägelser för
* en utdataporten returnerar hello poängsatta etiketter och hello associerade sannolikhet.

#### <a name="select-a-few-rows-of-data-for-hello-input-port"></a>Välj ett fåtal rader med data för hello indataport
Det är praktiskt toouse en **gäller SQL omvandling** modulen tooselect bara 10 rader tooserve som hello port indata. Välj bara dessa rader med data för våra indataport använda hello SQL-fråga som visas här:

![Indataport data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webbtjänst
Nu vi är redo toorun ett litet experiment som kan använda toopublish våra webbtjänsten.

#### <a name="generate-input-data-for-webservice"></a>Generera indata för webbtjänsten
Som ett zeroth steg eftersom hello antal tabell är stort, vi tar några rader testdata och generera utdata från det antal funktioner. Detta kan fungera som hello indata format för våra webbtjänsten. Detta visas här:

![Skapa BDT indata](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> Hello indata format vi nu använda hello utdata från hello **antal Featurizer** modul. När detta experimentera är klar kör du spara hello utdata från hello **antal Featurizer** modulen som en datamängd. Den här datauppsättningen används för hello indata i hello-webbtjänsten.
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a>Bedömningen experiment för publishing webbtjänsten
Först visar vi hur det ser ut. grundläggande hello-strukturen är en **Poängmodell** modul som accepterar våra trained model-objektet och några få rader av indata som vi skapade i föregående steg i hello med hello **antal Featurizer** modul. Vi använder ”Välj kolumner i datauppsättning” tooproject hello Scored etiketter och hello poäng sannolikhet.

![Välj kolumner i datauppsättning](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Observera hur hello **Välj kolumner i datauppsättning** modul kan användas för ”filtrera' data från en datamängd. Vi visar hello innehållet här:

![Filtrera med hello Välj kolumner i datauppsättning modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

tooget hello blå indata och utdata portar, klickar du på **förbereda webservice** på hello nedre högra hörnet. Kör experimentet kan också oss toopublish hello-webbtjänsten: Klicka på hello **publicera WEBBTJÄNSTEN** längst hello nedre högra, visas här:

![Publicera Web service](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)

När hello webservice har publicerats kan hämta vi omdirigerade tooa sida som ser ut därför:

![Web service-instrumentpanelen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Det finns två länkar webservices hello vänster:

* Hej **frågor och svar** Service (eller Resursposter) är avsedd för enskild förutsägelser och är vad vi utnyttjar i den här workshopen.
* Hej **BATCH EXECUTION** Service BES-används för batch förutsägelser och kräver att hello indata används toomake förutsägelser finns i Azure Blob Storage.

Klicka på länken hello **frågor och svar** tar tooa sida som ger oss burk före koden i C#, python och R. Den här koden kan användas för att göra anrop toohello webservice enkelt. Observera att hello API-nyckel på den här sidan måste toobe som används för autentisering.

Det är praktiskt toocopy denna python code över tooa nya cell i hello IPython anteckningsbok.

Här visar vi en del av python-kod med hello rätt API-nyckel.

![Python-kod](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)

Observera att vi ersatt hello standard API-nyckel med vår webservices API-nyckel. Klicka på **kör** för den här cellen i en IPython anteckningsboken ger hello efter svar:

![IPython svar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Vi kan se att hello två testa exempel har vi frågade om (i hello JSON-ramverket för hello python-skriptet), vi få tillbaka svar i hello form ”Scored etiketterna, Scored sannolikhet”. Observera att i det här fallet vi valde hello standardvärden hello fördefinierad koden innehåller (0 för alla numeriska kolumner och hello strängen ”värde” för alla kategoriska kolumner).

Detta avslutar våra slutpunkt till slutpunkt genomgången visar hur toohandle storskaliga dataset med hjälp av Azure Machine Learning. Vi igång med en terabyte data, skapas en förutsägelse modell och distribueras som en webbtjänst i hello molnet.

