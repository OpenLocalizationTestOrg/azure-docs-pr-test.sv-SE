---
title: "Team vetenskap av data i praktiken - med ett Azure HDInsight Hadoop-kluster på en datamängd för 1 TB | Microsoft Docs"
description: "Med hjälp av Team datavetenskap Process för en slutpunkt till slutpunkt-scenario med HDInsight Hadoop-kluster för att skapa och distribuera en modell med en stor (1 TB) offentligt tillgängliga datauppsättning"
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
ms.openlocfilehash: 8e6143bca819c9a0484221f8b4feb319aaaa73f5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="the-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>Team vetenskap av data i praktiken - med ett Azure HDInsight Hadoop-kluster på en datamängd för 1 TB

I den här genomgången visar med Team av vetenskapliga data i ett scenario för slutpunkt till slutpunkt med ett [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) för att lagra, utforska funktion tekniker och ned exempeldata från en av de offentligt tillgängliga [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datauppsättningar. Vi använder Azure Machine Learning för att skapa en binär klassificering modell för den här informationen. Vi visar även hur du publicerar en av dessa modeller som en webbtjänst.

Det är också möjligt att använda en bärbar dator IPython för att utföra uppgifter som visas i den här genomgången. Användare som vill använda den här metoden bör kontakta den [Criteo genomgången använder en Hive ODBC-anslutning](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) avsnittet.

## <a name="dataset"></a>Criteo Dataset-beskrivning
Criteo data är en Klicka förutsägelse datamängd som är cirka 370GB gzip komprimerade TVS-filer (~1.3TB okomprimerade), som består av fler än 4.3 miljard poster. Det hämtas från 24 dagar i klickar du på data som har gjorts tillgängliga av [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). För att underlätta för datavetare vi har uppackade tillgängliga för oss att experimentera med data.

Varje post i den här datauppsättningen innehåller 40 kolumner:

* den första kolumnen är en etikett-kolumn som anger om en användare klickar på en **lägga till** (värdet 1) eller inte på en (värde 0)
* Nästa 13 kolumner är numeriska, och
* senaste 26 är kategoriska kolumner

Kolumnerna är anonym och använder en serie av uppräknade namn: ”Kol1” (för etikettkolumnen) till ' Col40 ”(för den sista kolumnen kategoriska).            

Här är ett utdrag av två observationer (rader) från den här datauppsättningen först 20 kolumner:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Det finns värden som saknas i båda numeriska kategoriska kolumnerna och i denna dataset. Vi beskriver en enkel metod för att hantera värden som saknas. Ytterligare information om data beskrivs när vi lagrar dem i Hive-tabeller.

**Definition:** *klickningar (CTR):* detta är procentandelen av klick i data. I denna dataset för Criteo är CTR ungefär 3.3% eller 0.033.

## <a name="mltasks"></a>Exempel på förutsägelse uppgifter
Två exempel förutsägelse problem åtgärdas i den här genomgången:

1. **Binär klassificering**: beräknar om en användare klickar på en Lägg till:
   
   * Klass 0: Ingen klickar du på
   * Klass 1: Klicka på
2. **Regression**: beräknar sannolikheten för en ad Klicka på funktioner för användaren.

## <a name="setup"></a>Ange ett HDInsight Hadoop-kluster för datavetenskap
**Obs:** detta är vanligtvis en **Admin** aktivitet.

Ställ in Azure datavetenskap miljön för att skapa förutsägelseanalyslösningar med HDInsight-kluster i tre steg:

1. [Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md): det här lagringskontot används för att lagra data i Azure Blob Storage. Här lagras data som används i HDInsight-kluster.
2. [Anpassa Azure HDInsight Hadoop-kluster för datavetenskap](machine-learning-data-science-customize-hadoop-cluster.md): det här steget skapar ett Azure HDInsight Hadoop-kluster med 64-bitars Anaconda Python 2.7 installerad på alla noder. Det finns två viktiga steg (beskrivs i det här avsnittet) för att slutföra när du anpassar HDInsight-klustret.
   
   * Du måste länka storage-konto som skapades i steg 1 med ditt HDInsight-kluster när den skapas. Det här lagringskontot används för att komma åt data som kan bearbetas i klustret.
   * Du måste aktivera fjärråtkomst till huvudnod i klustret när den har skapats. Kom ihåg autentiseringsuppgifterna för fjärråtkomst som du anger här (skiljer sig från de som angetts för klustret när skapades): du behöver dem för att slutföra följande procedurer.
3. [Skapa en arbetsyta för Azure ML](machine-learning-create-workspace.md): den här Azure Machine Learning-arbetsytan används för att skapa machine learning-modeller efter en inledande datagranskning och ned provtagning på HDInsight-kluster.

## <a name="getdata"></a>Hämta och använda data från en offentlig källa
Den [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset kan nås genom att klicka på länken acceptera användningsvillkoren och tillhandahåller ett namn. En ögonblicksbild av hur det ser ut visas här:

![Acceptera Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Klicka på **Fortsätt om du vill hämta** du kan läsa mer om datauppsättningen och dess tillgänglighet.

Data som finns i en offentlig [Azure-blobblagring](../storage/blobs/storage-dotnet-how-to-use-blobs.md) plats: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. ”Wasb” refererar till Azure Blob Storage-plats. 

1. Data i den här offentliga blob storage består av tre undermappar uppackade data.
   
   1. Undermappen *raw /-beräkning/* innehåller de första 21 dagarna data - från dag\_00 dag\_20
   2. Undermappen *raw-tåg/* består av en dag av data, dag\_21
   3. Undermappen *rådata/test/* består av två dagars data, dag\_22 och dag\_23
2. För de som du vill börja med raw gzip-data, de är också tillgängliga i mappen huvudsakliga *rådata /* som day_NN.gz, där NN går mellan 00 och 23.

En annan metod för att komma åt, utforska och modellen dessa data som inte kräver någon lokal hämtningar beskrivs senare i den här genomgången när vi skapar Hive-tabeller.

## <a name="login"></a>Logga in på klustret headnode
Om du vill logga in på headnode i klustret, Använd den [Azure-portalen](https://ms.portal.azure.com) att hitta klustret. Klicka på ikonen HDInsight Elefant till vänster och dubbelklicka sedan på namnet på klustret. Navigera till den **Configuration** fliken, dubbelklicka på ikonen Anslut längst ned på sidan och ange dina autentiseringsuppgifter för fjärråtkomst när du tillfrågas. Då kommer du att headnode i klustret.

Här är en typisk första inloggningen till klustret headnode ser ut:

![Logga in på kluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)

Vi kan se ”Hadoop kommandoraden”, som våra bestämmer hög grad för undersökning av data till vänster. Vi kan också se två användbara webbadresserna - ”Hadoop Yarn Status” och ”Hadoop namn nod”. URL: en yarn status visar jobbförloppet och den nod URL: ger information om klusterkonfigurationen.

Nu vi har ställts in och är redo för att börja första delen av genomgången: datagranskning med Hive och förbereda data för Azure Machine Learning.

## <a name="hive-db-tables"></a>Skapa Hive-databasen och tabeller
Om du vill skapa Hive-tabeller för våra Criteo dataset, öppna den ***Hadoop kommandoraden*** på skrivbordet för huvudnoden, och ange Hive-katalogen genom att ange kommandot

    cd %hive_home%\bin

> [!NOTE]
> Kör alla Hive-kommandon i den här genomgången från Hive bin / directory-fråga. Detta hand tar om eventuella problem med sökväg automatiskt. Vi använder termer ”Hive directory prompt” ”, Hive bin / directory prompt”, och ”Hadoop-kommandoraden” synonymt.
> 
> [!NOTE]
> Om du vill köra Hive-fråga använda en alltid följande kommandon:
> 
> 

        cd %hive_home%\bin
        hive

När Hive REPL visas med en ”hive >” Logga, bara klipp ut och klistra in frågan för att köra den.

Följande kod skapar en databas ”criteo” och sedan genererar 4 tabeller:

* en *tabell för generering av antal* bygger på dagar dag\_00 dag\_20,
* en *tabell som ska användas som tåget datauppsättningen* bygger på dag\_21, och
* två *tabeller som test datauppsättningar* bygger på dag\_22 och dag\_23 respektive.

Vi dela vår testdata i två olika tabeller eftersom en av dagarna är helgdagar, och vi vill att avgöra om modellen identifiera skillnader mellan en helgdag och icke-helgdagar från klickningar.

Skriptet [exempel &#95; hive &#95; Skapa &#95; criteo &#95; databas &#95; och &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) visas här av praktiska skäl:

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

Vi Observera att dessa tabeller är externa som vi helt enkelt pekar på Azure Blob Storage (wasb)-platser.

**Det finns två sätt att köra alla Hive-fråga som vi nämnt nu.**

1. **Med den Hive REPL kommandoradsverktyget**: först är att utfärda en ”hive” kommandot och kopiera och klistra in en fråga på den Hive REPL kommandoraden. Om du vill göra det, gör du:
   
        cd %hive_home%\bin
        hive
   
     Nu på REPL kommandoradsverktyg kör kopiera och klistra in frågan den.
2. **Spara frågor till en fil och kommandot**: andra är att spara frågor till en .hql-fil ([exempel &#95; hive &#95; Skapa &#95; criteo &#95; databas &#95; och &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) och sedan kör du följande kommando för att köra frågan:
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Bekräfta databas och tabell skapas
Därefter vi bekräfta att databasen med följande kommando från Hive bin / directory prompten:

        hive -e "show databases;"

Detta ger:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Det här bekräftar att skapa den nya databasen ”criteo”.

Om du vill se vilka tabeller som vi skapade vi bara utfärda kommandot här från Hive bin / directory prompten:

        hive -e "show tables in criteo;"

Vi kan sedan se följande utdata:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="exploration"></a>Datagranskning i Hive
Vi är nu redo att göra vissa grundläggande datagranskning i Hive. Vi börjar med att räkna antalet exempel i tåget och testa datatabeller.

### <a name="number-of-train-examples"></a>Antal train-exempel
Innehållet i [exempel &#95; hive &#95; antal &#95; train &#95; tabellen &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) visas här:

        SELECT COUNT(*) FROM criteo.criteo_train;

Detta ger:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Alternativt kan en även kör du följande kommando från lagerplatsen Hive / directory prompten:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Antal test exemplen i de två test datauppsättningarna
Vi nu räkna antalet exempel i två test datauppsättningar. Innehållet i [exempel &#95; hive &#95; antal &#95; criteo &#95; test &#95; dag &#95; 22 &#95; tabellen &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) finns här:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Detta ger:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Vi kan som vanligt också anropa skriptet från Hive bin / directory kommandoprompt genom att utfärda kommandot:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Slutligen vi undersöka antalet test exemplen i testdata baserat på dag\_23.

Kommandot för att göra detta liknar den som bara visas (se [exempel &#95; hive &#95; antal &#95; criteo &#95; test &#95; dag &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Detta ger:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Etikett distribution i train datauppsättningen
Distributionen av etikett i train datamängden är av intresse. Om du vill se den här vi visa innehållet i [exempel &#95; hive &#95; criteo &#95; etikett &#95; distri &#95; train &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Detta ger etikett-distribution:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Observera att procentandelen positivt etiketter är ungefär 3.3% (konsekvent med den ursprungliga datauppsättningen).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogram distributioner av vissa numeriska variablerna i datauppsättningen train
Vi kan använda registreringsdata intern ”histogram\_numeriska” funktionen för att ta reda på hur distributionen av variablerna numeriska ser ut. Här är innehållet i [exempel &#95; hive &#95; criteo &#95; histogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Detta ger följande:

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

LATERALA Visa - Expandera kombination i Hive används för att skapa en SQL-liknande utdata i stället för vanliga listan. Observera att i den här tabellen, den första kolumnen motsvarar bin center och andra för bin frekvens.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Ungefärlig percentiler av vissa numeriska variablerna i datauppsättningen train
Är också beräkning av ungefärliga percentiler intressanta med numeriska variabler. Hive datorns inbyggda ”percentil\_ungefärlig” matchar det för oss. Innehållet i [exempel &#95; hive &#95; criteo &#95; ungefärliga &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) är:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Detta ger:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Vi Markera att distributionen av percentiler är nära relaterade till histogram distribution av en numerisk variabel vanligtvis.         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Räkna antalet unika värden för vissa kategoriska kolumner i datauppsättningen train
Fortsätter datagranskning finns vi nu, för vissa kategoriska kolumner, antalet unika värden som de vidtar. Detta gör vi visa innehållet i [exempel &#95; hive &#95; criteo &#95; unika &#95; värden &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Detta ger:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Vi Observera att Col15 19M unika värden! Använda naïve tekniker som ”en-hot kodning” är om du vill koda sådana hög endimensionell kategoriska variabler omöjligt. I synnerhet Vi förklarar och visar en kraftfull, robust teknik som kallas [inlärning med antal](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) för att effektivt hantera problemet.

Vi avbryta den här underavsnittet genom att titta på antalet unika värden för vissa andra kategoriska kolumner samt. Innehållet i [exempel &#95; hive &#95; criteo &#95; unika &#95; värden &#95; flera &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) är:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Detta ger:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Igen Se vi att de andra kolumnerna förutom Col20, har många unika värden.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Samtidigt förekomsten räknar par kategoriska variablerna i datauppsättningen train

Samtidigt förekomsten antal par kategoriska variabler är också av intresse. Detta kan fastställas med hjälp av koden i [exempel &#95; hive &#95; criteo &#95; parad &#95; kategoriska &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Vi omvänd ordna antal efter deras förekomst och titta på högsta 15 i det här fallet. Detta ger oss:

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

## <a name="downsample"></a>Ned exempel datamängder för Azure Machine Learning
Med utforskade datauppsättningar och visas hur vi kan göra den här typen av alla variabler (inklusive kombinationer), vi nu ned exempel exploatera datauppsättningar så att vi kan bygga modeller i Azure Machine Learning. Kom ihåg att vi fokusera på problemet är: en uppsättning exempel attribut (funktionen värden från Col2 - Col40) får vi förutsäga om Kol1 är 0 (ingen klicka) eller 1 (klicka).

För att ned exempel våra tåg och testa datauppsättningar till 1% av den ursprungliga storleken, funktionen vi registreringsdata interna RAND(). Skriptet nästa [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; train &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) gör detta för tåget datauppsättningen:

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

Skriptet [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; test &#95; dag &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) matchar för testdata, dag\_22:

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


Slutligen skriptet [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; test &#95; dag &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) matchar för testdata, dag\_23:

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

Med detta kan är vi redo att använda våra nedåt provade tåg och testa datauppsättningar för att skapa modeller i Azure Machine Learning.

Det finns en sista viktig komponent innan vi vidare till Azure Machine Learning, vilket är av säkerhetsskäl tabellen antal. I nästa underavsnitt diskuterar vi detta i viss detalj.

## <a name="count"></a>En kort beskrivning på tabellen antal
Som vi såg har flera kategoriska variabler en mycket hög dimensionalitet. I vår genomgången presenterar vi en kraftfull teknik som kallas [inlärning med antal](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) att koda dessa variabler på ett effektivt och robust sätt. Mer information om den här tekniken finns i länken.

[!NOTE]
>I den här genomgången ska fokusera vi på använder antal tabeller för att skapa compact representationer av hög endimensionell kategoriska funktioner. Detta är inte det enda sättet att koda kategoriska funktioner; Mer information om andra metoder berörda användare kan checka ut [en-hot encoding](http://en.wikipedia.org/wiki/One-hot) och [hash-funktionen](http://en.wikipedia.org/wiki/Feature_hashing).
>

För att skapa antal tabeller på antalet data använder vi informationen i mappen rådata/antal. I avsnittet modellering vi Visa användare skapa dessa antal tabeller för kategoriska funktioner från början, eller också använda en förskapad antal tabell för sina explorations. I vilka sätt när vi refererar till ”förskapad antal tabeller”, menar vi med antalet tabeller som vi tillhandahåller. Detaljerade anvisningar om hur du kommer åt dessa tabeller finns i nästa avsnitt.

## <a name="aml"></a>Skapa en modell med Azure Machine Learning
Vår modell processen i Azure Machine Learning för att bygga följer de här stegen:

1. [Hämta data från Hive-tabeller i Azure Machine Learning](#step1)
2. [Skapa experimentet: rensa data och featurize med antal tabeller](#step2)
3. [Skapa, träna och betygsätta modellen](#step3)
4. [Utvärdera modellen](#step4)
5. [Publicera modellen som en webbtjänst](#step5)

Vi är nu redo att bygga modeller i Azure Machine Learning studio. Vår nedåt samplade data sparas som Hive-tabeller i klustret. Vi använder Azure Machine Learning **importera Data** modulen att läsa informationen. Autentiseringsuppgifter för åtkomst till lagringskontot för det här klustret finns i följande.

### <a name="step1"></a>Steg 1: Hämta data från Hive-tabeller i Azure Machine Learning med hjälp av modulen importera Data och markera ett maskininlärningsexperiment
Starta genom att välja en **+ ny** -> **EXPERIMENT** -> **tomt Experiment**. Sedan från den **Sök** rutan längst upp till vänster, Sök efter ”importera Data”. Dra och släpp den **importera Data** modulen in experimentet arbetsytan (mellersta delen av skärmen) ska kunna använda modulen för dataåtkomst.

Det här är vad den **importera Data** ser ut som att hämta data från Hive-tabell:

![Importera Data hämtar data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

För den **importera Data** modulen värdena för parametrarna som tillhandahålls i bilden är bara exempel på sorteringen av värden som du behöver ange. Här är några allmänna riktlinjer för hur du fyller i parameteruppsättning för de **importera Data** modul.

1. Välj ”Hive-frågan” för **datakälla**
2. I den **Hive databasfrågan** , en enkel, MARKERAR du kryssrutan * FROM < din\_databasen\_name.your\_tabell\_namn >-räcker.
3. **Hcatalog server URI**: om klustret är ”abc” och sedan det här är bara: https://abc.azurehdinsight.net
4. **Hadoop användarkontonamnet**: användarnamnet som valts vid tidpunkten för idriftsättning klustret. (Inte fjärråtkomst användarnamnet!)
5. **Hadoop lösenord**: lösenordet för användarnamnet som valts vid tidpunkten för idriftsättning klustret. (Inte fjärråtkomst lösenordet!)
6. **Platsen för utdata**: Välj ”Azure”
7. **Azure lagringskontonamnet**: lagringskonto som associeras med klustret
8. **Azure lagringskontonyckel**: nyckeln för lagringskontot som är associerade med klustret.
9. **Azure behållarnamn**: Om klusternamnet är ”abc”, så det är helt enkelt ”abc”, vanligtvis.

En gång i **importera Data** har avslutats hämtning av data (visas grönt skalstreck i modulen), spara informationen som en datamängd (med ett valfritt namn). Det ser ut:

![Importera Data spara data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Högerklicka på utdataporten för den **importera Data** modul. Nu visas en **Spara som dataset** alternativet och en **visualisera** alternativet. Den **visualisera** alternativet om du visar 100 rader med data, tillsammans med en högra panelen som är användbar för vissa sammanfattande statistik. För att spara data, markerar du bara **Spara som dataset** och följ instruktionerna.

Om du vill välja den sparade datamängden för användning i ett machine learning-experiment, hitta datauppsättningar med hjälp av den **Sök** rutan som visas i följande bild. Sedan skriver ut namnet du gav dataset delvis vill komma åt den och dra datauppsättningen till åtgärdsfönstervärdens. Släppa på åtgärdsfönstervärdens markeras den för användning i machine learning modellering.

![Drage dataset till åtgärdsfönstervärdens](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> Gör detta för både tåg och testa datauppsättningar. Dessutom Kom ihåg att använda databasnamnet och tabellnamn som du gav för detta ändamål. De värden som används i bilden är endast för bilden purposes.* *
> 
> 

### <a name="step2"></a>Steg 2: Skapa ett enkelt experiment i Azure Machine Learning att förutsäga klick / några klick
Vårt Azure ML-experiment ser ut så här:

![Machine Learning-experiment](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Vi nu undersöka huvudkomponenterna i experimentet. Som en påminnelse om behöver vi dra våra sparade tåg och testa datauppsättningar in våra experimentet först.

#### <a name="clean-missing-data"></a>Rensa Data som saknas
Den **Rensa Data som saknas** modulen har namnet antyder: data som saknas på ett sätt som kan vara användardefinierade rensas. Titta på den här modulen finns vi här:

![Rensa data som saknas](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Vi valde här, ersätter alla värden som saknas med 0. Det finns andra alternativ, vilket kan ses genom att titta på de nedrullningsbara listorna i modulen.

#### <a name="feature-engineering-on-the-data"></a>Egenskapsval på data
Det kan finnas miljontals unika värden för vissa kategoriska funktioner i stora datauppsättningar. Med hjälp av naïve metoder, till exempel en hot kodning för att representera hög endimensionell kategoriska funktioner är helt unfeasible. I den här genomgången visar vi hur du använder antal funktioner med hjälp av inbyggda Azure Machine Learning-moduler för att generera compact representationer av dessa hög endimensionell kategoriska variabler. Slutresultatet blir mindre modellen, snabbare utbildning och resultatmått som är helt jämförbar med andra metoder.

##### <a name="building-counting-transforms"></a>Skapa inventering transformeringar
För att skapa antalet funktioner som vi använder den **skapa inventering transformera** modul som finns tillgängliga i Azure Machine Learning. Modulen ser ut så här:

![Skapa inventering transformera modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![skapa inventering transformera modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT] 
> I den **antal kolumner** vi rutan Ange de kolumner som vi vill utföra antal på. Dessa normalt (som tidigare nämnts) hög endimensionell kategoriska kolumner. I början, som vi nämnt att Criteo datamängden har 26 kategoriska kolumner: från Col15 till Col40. Här kan vi räkna på dem alla och ge sina index (från 15 till 40 avgränsade med kommatecken enligt).
> 

Använda modul i MapReduce-läge (lämplig för stora datauppsättningar), vi behöver åtkomst till ett HDInsight Hadoop-kluster (den som används för funktionen utforskning kan återanvändas för detta ändamål samt) och dess autentiseringsuppgifter. Tidigare siffror visar vilka ifyllda värdena se ut (ersätter värdena för jämförelseändamål med de som är relevanta för dina egna användningsfall).

![Modulparametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

I bilden ovan visar vi hur du anger inkommande blobbplats. Den här platsen har reserverats för antalet tabeller som bygger på data.

När den här modulen är klar vi spara transformeringen för senare genom att högerklicka på modulen och välja den **Spara som transformeringen** alternativ:

![Alternativet ”Spara som transformeringen”](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

I vårt experiment arkitektur som visas ovan motsvarar dataset ”ytransform2” exakt en sparad antal transformering. I resten av experimentet vi antar att läsaren som en **skapa inventering transformera** modul på vissa data att generera inventeringar och kan sedan använda dessa antal om du vill generera antal funktioner på tåg och testa datauppsättningar.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Välja vilka antal funktioner för att inkludera som en del av tåg och testa datauppsättningar
När vi har ett antal transformera redo användaren kan välja vilka funktioner som ska inkluderas i sina tåg och testa datauppsättningar med hjälp av den **ändra antalet tabellen parametrar** modul. Vi bara att visa den här modulen här för fullständighetens skull, men i enkelhetens inte utnyttjar den i vårt experiment.

![Ändra antalet tabellen parametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

I det här fallet som kan ses har vi valt att använda bara log-oddsen och ignorera på baksidan av kolumn. Vi kan också ange parametrar som skräp bin tröskelvärdet, hur många startvärden föregående exempel för att lägga till för Utjämning och om du vill använda alla Laplacian brus eller inte. Alla dessa avancerade funktioner och det är att märka att standardvärdena är en bra utgångspunkt för användare som är nya för den här typen av funktionen.

##### <a name="data-transformation-before-generating-the-count-features"></a>DTS innan du genererar antal funktioner
Nu vi fokusera på en viktig aspekt Omforma våra tåg och testa data innan du genererar faktiskt antal funktioner. Observera att det finns två **köra R-skriptet** moduler som används innan vi använda antal transformeringen i våra data.

![Köra R-skriptet moduler](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Här är första R-skriptet:

![Första R-skriptet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)

I det här R-skriptet kan vi Byt namn på vår kolumner till namn ”Kol1” till ”Col40”. Det beror på att antalet transformeringen förväntar namnen på det här formatet.

I andra R-skriptet vi Utjämna distributionen mellan positiva och negativa klasser (klasserna 1 och 0 respektive) av nedsampling klassen negativt. R-skriptet här visas hur du gör detta:

![Andra R-skriptet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

I det här enkla R-skriptet kan vi använda ”pos\_neg\_förhållandet” ange mängden balans mellan positiva och negativa klasser. Detta är viktigt att utföra eftersom förbättra klassen obalans vanligtvis har prestandafördelarna för klassificering problem där distributionen av klassen är förvrängd (återkallning att i vårt fall har vi 3.3% positivt klassen och 96.7% negativt klassen).

##### <a name="applying-the-count-transformation-on-our-data"></a>Tillämpa count-transformation på våra data
Slutligen kan vi använda den **gäller omvandling** modul som ska tillämpa antal transformeringar för våra tåg och testa datauppsättningar. Den här modulen tar sparade count-transformeringen som en inmatning och tåg eller test datauppsättningar som andra indata och returnerar data med antal funktioner. Den visas här:

![Tillämpa omvandling av modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Det ser ut som ett utdrag vilka antal funktioner
Det är nyttigt att se hur antalet funktioner som ser ut i vårt fall. Här beskrivs ett utdrag ur detta:

![Antal funktioner](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

Detta utdrag visar att för de kolumner som vi räknas på vi få fram de och loggar oddsen utöver eventuella relevanta backoffs.

Vi är nu redo att skapa en Azure Machine Learning-modell med hjälp av dessa transformerade data. I nästa avsnitt visar vi hur detta kan göras.

### <a name="step3"></a>Steg 3: Skapa, träna och betygsätta modellen

#### <a name="choice-of-learner"></a>Valet av deltagaren
Vi måste först välja en deltagaren. Vi kommer att använda ett två klassen förstärkta beslutsträd som våra deltagaren. Här är alternativ för den här deltagaren:

![Två-Tvåklassförhöjt beslutsträd parametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

För vårt experiment ska vi välja standardvärden. Vi Observera att standardvärdena är vanligtvis beskrivande och ett bra sätt att få snabb baslinjer för prestanda. Du kan förbättra prestanda genom omfattande parametrar om du väljer när du har en baslinje.

#### <a name="train-the-model"></a>Träna modellen
För träning, vi bara anropa en **Träningsmodell** modul. De två indatavärdena till den är två-Tvåklassförhöjt beslutsträd deltagaren och vår train datauppsättning. Detta visas här:

![Modulen för Train-modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-the-model"></a>Poängsätt modellen
När vi har en tränad modell är vi redo att poängsätta mot testdatauppsättningen och utvärdera dess prestanda. Vi kan göra detta med hjälp av den **Poängmodell** modulen som visas i följande bild, tillsammans med en **utvärdera modell** modulen:

![Modulen Poängsätta modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)

### <a name="step4"></a>Steg 4: Utvärdera modellen
Slutligen vill vi analysera modellen prestanda. Vanligtvis är två klass (binär) klassificering problem ett bra mått på AUC. Om du vill visualisera detta vi koppla samman den **Poängmodell** modulen till en **utvärdera modell** -modul för detta. Klicka på **visualisera** på den **utvärdera modell** modulen ger en bild som den följande:

![Utvärdera modulen BDT modellen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

I binär (eller två klassen) klassificering problem, ett bra mått på förutsägelsefunktionen är det område i kurvan (AUC). I följande, visar vi våra resultat med hjälp av den här modellen på vår testdata. För att få detta, högerklickar du på utdataporten för den **utvärdera modell** modulen och sedan **visualisera**.

![Visualisera modulen utvärdera modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step5"></a>Steg 5: Publicera modellen som en webbtjänst
Möjligheten att publicera en Azure Machine Learning-modell som webbtjänster med minsta möjliga ansträngning är en viktig funktion för att göra den allmänt tillgänglig. När det är klart, göra vem som helst anrop till webbtjänsten med indata att de behöver förutsägelser för och webbtjänsten använder modellen för att returnera dessa förutsägelser.

Detta gör spara vi först vår tränad modell som en Trained Model-objektet. Detta görs genom att högerklicka på den **Träningsmodell** modulen och använder den **Spara som Trained Model** alternativet.

Därefter behöver vi skapa inkommande och utgående portar för våra webbtjänst:

* en port hämtar data i samma formulär som data som vi behöver förutsägelser för
* en utdataporten returnerar poängsatta etiketter och associerade troliga.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Välj ett fåtal rader med data för den inkommande porten
Är det praktiskt att använda en **gäller SQL omvandling** modulen att välja bara 10 rader att fungera som indataport data. Välj bara dessa rader med data för våra indataport använda SQL-fråga som visas här:

![Indataport data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webbtjänst
Vi är nu redo att köra ett litet experiment som kan användas för att publicera våra webbtjänsten.

#### <a name="generate-input-data-for-webservice"></a>Generera indata för webbtjänsten
Som ett zeroth steg eftersom tabellen antal är stort, vi tar några rader testdata och generera utdata från det antal funktioner. Detta kan fungera som indata för våra webservice. Detta visas här:

![Skapa BDT indata](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> För formatet indata vi nu använda utdata från den **antal Featurizer** modul. När detta experimentera är klar kör spara utdata från den **antal Featurizer** modulen som en datamängd. Den här datauppsättningen används för indata i den här webbtjänsten.
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a>Bedömningen experiment för publishing webbtjänsten
Först visar vi hur det ser ut. Den grundläggande strukturen är en **Poängmodell** modul som accepterar våra trained model-objektet och några få rader av indata som vi skapade i föregående steg med hjälp av den **antal Featurizer** modul. Vi använder ”Välj kolumner i datauppsättning” för projektet Scored etiketter och troliga poäng.

![Välj kolumner i datauppsättning](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Observera hur **Välj kolumner i datauppsättning** modul kan användas för ”filtrera' data från en datamängd. Vi visa innehållet här:

![Välj kolumner i datauppsättning modulen-filtrering](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

För att få blå ingående och utgående portar, klickar du på **förbereda webservice** längst ned rätt. Kör experimentet också möjlighet att publicera webbtjänsten: Klicka på den **publicera WEBBTJÄNSTEN** längst ned rätt, visas här:

![Publicera Web service](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)

När webbtjänsten har publicerats kan hämta vi dirigeras till en sida som ser ut därför:

![Web service-instrumentpanelen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Det finns två länkar webservices på vänster sida:

* Den **frågor och svar** Service (eller Resursposter) är avsedd för enskild förutsägelser och är vad vi utnyttjar i den här workshopen.
* Den **BATCH EXECUTION** Service BES-används för batch förutsägelser och kräver att indata används för att göra förutsägelser som finns i Azure Blob Storage.

Klicka på länken **frågor och svar** tar vi en sida som ger oss före burk koden i C#, python och R. Den här koden kan användas för att göra anrop till webbtjänsten enkelt. Observera att API-nyckel på den här sidan måste användas för autentisering.

Det är praktiskt att kopiera koden python över till en ny cell i IPython anteckningsboken.

Här visar vi en del av python-kod med rätt API-nyckel.

![Python-kod](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)

Observera att vi ersatt standard API-nyckel med vår webservices API-nyckel. Klicka på **kör** för den här cellen i en IPython anteckningsboken ger följande svar:

![IPython svar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Vi kan se att två test exempel vi frågade om (i JSON-ramverket för python-skriptet), vi få tillbaka svar i formatet ”Scored etiketterna, Scored sannolikhet”. Observera att i det här fallet vi valde standardvärdena som fördefinierad kod ger (0 för alla numeriska kolumner och strängen ”värde” för alla kategoriska kolumner).

Detta avslutar våra slutpunkt till slutpunkt genomgången visar hur du hanterar storskaliga dataset med hjälp av Azure Machine Learning. Vi igång med en terabyte data, skapas en förutsägelse modell och distribueras som en webbtjänst i molnet.

