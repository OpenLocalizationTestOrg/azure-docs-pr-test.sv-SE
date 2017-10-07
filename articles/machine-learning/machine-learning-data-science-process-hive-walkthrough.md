---
title: aaaExplore data i ett Hadoop-kluster och skapa modeller i Azure Machine Learning | Microsoft Docs
description: "Med hello Team datavetenskap Process för en slutpunkt till slutpunkt-scenario med en HDInsight Hadoop kluster toobuild och distribuerar en modell med en offentligt tillgängliga dataset."
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: a371032e356ffc366af0d6fbe364af281b6efd19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>hello Team av vetenskapliga data i praktiken: Använd Azure HDInsight Hadoop-kluster
I den här genomgången ska vi använda hello [Team Data vetenskap processen (TDSP)](data-science-process-overview.md) i ett scenario för slutpunkt till slutpunkt med hjälp av en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) toostore, utforska och funktion tekniker data från hello offentligt tillgängliga [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset och toodown exempeldata hello. Modeller av hello data skapas med Azure Machine Learning toohandle binära och multiklass-baserad klassificering och regression förutsägande uppgifter.

En genomgång som visar hur toohandle en större datamängd (1 terabyte) liknande scenarier med HDInsight Hadoop-kluster för databearbetning finns [Team datavetenskap Process, med hjälp av Azure HDInsight Hadoop-kluster på 1 TB-dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Det är också möjligt toouse en IPython anteckningsboken tooaccomplish hello uppgifter presenterades hello genomgången använder hello 1 TB dataset. Användare som vill som den här metoden bör kontakta tootry hello [Criteo genomgången använder en Hive ODBC-anslutning](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) avsnittet.

## <a name="dataset"></a>NYC Taxi resor Dataset-beskrivning
hello NYC Taxi resa data är cirka 20GB komprimerad fil med kommaavgränsade värden (CSV)-filer (~ 48GB okomprimerade), som består av fler än 173 miljoner enskilda resor och hello priser för varje resa. Hello hämtning och Samlingsbibliotek plats och tid, anonymiserade hackare (drivrutin) licensnummer och medallion (taxi's unikt id) nummer innehåller varje resa-post. hello data omfattar alla resor hello år 2013 och finns i följande två datamängder för varje månad hello:

1. hello 'trip_data' CSV-filer innehåller resa information, till exempel antal passagerare, hämtning och dropoff punkter, resa varaktighet och resa längd. Här följer några Exempelposter:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. hello 'trip_fare' CSV-filer innehåller information om hello avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar och hello totalbelopp betald. Här följer några Exempelposter:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

hello Unik nyckel toojoin resa\_data och resa\_avgiften består av hello fält: medallion hackare\_licensen och hämtning\_datetime.

alla tooget hello information relevanta tooa viss resan, den är tillräckligt toojoin med tre nycklar: hello ”medallion” ”, hacka\_licens” och ”hämtning\_datetime”.

Vi beskriver vissa mer information om hello data när vi lagrar dem i Hive-tabeller inom kort.

## <a name="mltasks"></a>Exempel på förutsägelse uppgifter
När närmar sig data kan fastställa hello slags förutsägelser som du vill toomake baserat på dess analysis tydliggöra hello uppgifter du behöver tooinclude i processen.
Här följer tre exempel på förutsägelse problem som vi adressen i den här genomgången vars formulering baseras på hello *tips\_belopp*:

1. **Binär klassificering**: förutsäga oavsett betalats ett tips för en resa i d.v.s. en *tips\_belopp* som är större än 0 är ett positivt exempel, när en *tips\_belopp* $0 är ett exempel på negativt.
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0
2. **Multiklass-baserad klassificering**: toopredict hello mängd tips belopp betalat för hello resa. Vi dela hello *tips\_belopp* i fem lagerplatser eller klasser:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regression uppgiften**: toopredict hello hello tips betalats för en transport.  

## <a name="setup"></a>Konfigurera en HDInsight Hadoop-kluster för avancerade analyser
> [!NOTE]
> Detta är vanligtvis en **Admin** aktivitet.
> 
> 

Du kan konfigurera en Azure-miljö för avancerade analyser som använder ett HDInsight-kluster i tre steg:

1. [Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md): det här lagringskontot används för att lagra data i Azure Blob Storage. hello-data som används i HDInsight-kluster finns också här.
2. [Anpassa Azure HDInsight Hadoop-kluster för hello Advanced Analytics processen och tekniken](machine-learning-data-science-customize-hadoop-cluster.md). Det här steget skapar ett Azure HDInsight Hadoop-kluster med 64-bitars Anaconda Python 2.7 installerad på alla noder. Det finns två viktiga steg tooremember vid anpassning av ditt HDInsight-kluster.
   
   * Kom ihåg toolink hello storage-konto som skapades i steg 1 med ditt HDInsight-kluster när du skapar den. Det här lagringskontot är används tooaccess data som bearbetas i hello kluster.
   * När hello klustret har skapats kan du aktivera fjärråtkomst toohello huvudnod hello-klustret. Navigera toohello **Configuration** och klicka på **aktivera fjärråtkomst**. Det här steget anger hello autentiseringsuppgifter används för fjärrinloggning.
3. [Skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md): den här Azure Machine Learning-arbetsytan har använt toobuild machine learning-modeller. Den här uppgiften riktar sig när du har slutfört en första datagranskning och ned med hjälp av hello HDInsight-kluster.

## <a name="getdata"></a>Hämta hello data från en offentlig källa
> [!NOTE]
> Detta är vanligtvis en **Admin** aktivitet.
> 
> 

tooget hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset från dess offentlig plats, kan du använda någon av hello-metoder som beskrivs i [tooand flytta Data från Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour datorn.

Här beskrivs hur använda AzCopy tootransfer hello filer som innehåller data. toodownload och installera AzCopy följer hello instruktionerna på [komma igång med kommandoradsverktyget Azcopy hello](../storage/common/storage-use-azcopy.md).

1. Från Kommandotolken, utfärda hello följande AzCopy kommandona, ersätter *< path_to_data_folder >* med hello önskat mål:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. När hello kopia är klar finns totalt av 24 komprimerade filer i hello-datamappen valt. Packa upp hello hämtade filer toohello samma katalog på den lokala datorn. Anteckna hello mappen där hello okomprimerade filerna finns. Den här mappen blir refererad tooas hello *< sökväg\_till\_unzipped_data\_filer\>*  är det som följer.

## <a name="upload"></a>Ladda upp data hello toohello standardbehållaren för Azure HDInsight Hadoop-kluster
> [!NOTE]
> Detta är vanligtvis en **Admin** aktivitet.
> 
> 

Följande kommandon för AzCopy och Ersätt i hello hello följande parametrar med hello faktiska värden som du angav när du skapar hello Hadoop-kluster och packa upp hello datafiler.

* ***&#60; path_to_data_folder >*** hello katalog (tillsammans med sökväg) på datorn som innehåller hello uppackade datafiler  
* ***&#60; lagringskontonamnet av Hadoop-kluster >*** hello storage-konto som är kopplad till ditt HDInsight-kluster
* ***&#60; standardbehållaren för Hadoop-kluster >*** hello standardbehållaren som används av klustret. Observera att hello namn hello default-behållaren är vanligtvis hello samma namn som själva hello-klustret. Om hello kluster kallas ”abc123.azurehdinsight.net”, är hello standardbehållaren abc123.
* ***&#60; lagringskontonyckel >*** hello nyckel för hello storage-konto som används av klustret

Kör hello följande två kommandon för AzCopy från en kommandotolk eller ett Windows PowerShell-fönster på datorn.

Det här kommandot överför hello resa data för***nyctaxitripraw*** katalog i hello standardbehållaren för hello Hadoop-kluster.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Det här kommandot överför hello avgiften data för***nyctaxifareraw*** katalog i hello standardbehållaren för hello Hadoop-kluster.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

hello data bör nu i Azure Blob Storage och redo toobe förbrukas inom hello HDInsight-kluster.

## <a name="#download-hql-files"></a>Logga in på hello huvudnod i Hadoop-kluster och och förbereda för undersökande dataanalys
> [!NOTE]
> Detta är vanligtvis en **Admin** aktivitet.
> 
> 

tooaccess hello huvudnod hello klustret för undersökande dataanalys och ned hello dataurval, följ hello proceduren som beskrivs i [åtkomst hello Head-nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

I den här genomgången ska vi i första hand använder frågor som skrivits i [Hive](https://hive.apache.org/), ett frågespråk för SQL-liknande, tooperform preliminära data explorations. hello Hive-frågor som lagras i .hql filer. Vi sedan exempel ned på den här toobe för data som används i Azure Machine Learning för att skapa modeller.

tooprepare hello kluster för undersökande dataanalys, vi hämtar hello .hql filer som innehåller hello relevanta Hive-skript från [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa lokal katalog (C:\temp) på hello huvudnod. toodo detta, öppna hello **kommandotolk** inifrån hello huvudnod hello klustret och problemet hello följande två kommandon:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Dessa två kommandon hämtas alla .hql filer som behövs i den här genomgången toohello lokal katalog ***C:\temp &#92;*** i hello huvudnod.

## <a name="#hive-db-tables"></a>Skapa Hive-databasen och tabeller som är partitionerad per månad
> [!NOTE]
> Detta är vanligtvis en **Admin** aktivitet.
> 
> 

Vi är nu redo toocreate Hive-tabeller för våra NYC taxi dataset.
Öppna hello i hello huvudnod hello Hadoop-kluster, ***Hadoop kommandoraden*** på hello skrivbord hello huvudnod och ange hello Hive katalogen genom att ange hello kommando

    cd %hive_home%\bin

> [!NOTE]
> **Kör alla Hive-kommandon i den här genomgången från hello ovan Hive bin / directory-fråga. Detta hand tar om eventuella problem med sökväg automatiskt. Vi använder hello termer ”Hive directory prompt” ”, Hive bin / directory prompt”, och ”Hadoop kommandoraden” synonymt i den här genomgången.**
> 
> 

Ange följande kommando i Hadoop kommandoraden för hello huvudnod toosubmit hello Hive-fråga toocreate Hive-databasen och tabeller hello hello Hive directory prompten:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Här är hello innehållet i hello ***C:\temp\sample\_hive\_skapa\_db\_och\_tables.hql*** -fil som skapar Hive databas ***nyctaxidb *** och tabeller ***resa*** och ***avgiften***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Den här Hive-skript skapar två tabeller:

* Hej ”resa” tabellen innehåller resa information om varje resa (drivrutinsinformation, pickup tid, resa avstånd och gånger)
* Hej ”avgiften” tabell innehåller information om avgiften (avgiften, tips belopp, vägtullar och tillägg).

Om du behöver ytterligare hjälp med dessa procedurer eller vill tooinvestigate alternativa de avsnittet hello [skicka Hive-frågor direkt från hello Hadoop kommandoraden ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Läsa in tooHive datatabeller av partitioner
> [!NOTE]
> Detta är vanligtvis en **Admin** aktivitet.
> 
> 

hello NYC taxi datamängden har en naturlig partitionering per månad som vi använder tooenable bearbetningen och frågeprestanda snabbare. Hej PowerShell-kommandona nedan (utfärdats från hello Hive-katalogen med hjälp av hello **Hadoop kommandoraden**) läsa in data toohello ”resa” och ”avgiften” Hive-tabeller partitionerad per månad.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

Hej *exempel\_hive\_ladda\_data\_av\_partitions.hql* filen innehåller följande hello **LADDA** kommandon.

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Observera att ett antal Hive-frågor som vi använder här hello utforskning pågående innebär att bara en enda partition eller på bara några partitioner. Men dessa frågor kan köras över hela hello-data.

### <a name="#show-db"></a>Visa databaser i hello HDInsight Hadoop-kluster
tooshow hello databaser som skapats i HDInsight Hadoop-kluster i hello Hadoop kommandoradsfönster, kör följande kommando i Hadoop kommandoraden hello:

    hive -e "show databases;"

### <a name="#show-tables"></a>Visa hello Hive-tabeller i hello nyctaxidb databas
tooshow hello tabeller i hello nyctaxidb databas, kör följande kommando i Hadoop kommandoraden hello:

    hive -e "show tables in nyctaxidb;"

Vi kan bekräfta att partitioneras hello tabeller genom att utfärda hello kommandot nedan:

    hive -e "show partitions nyctaxidb.trip;"

hello förväntad utdata visas nedan:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

På liknande sätt kan vi Kontrollera hello avgiften tabellen är partitionerad genom att utfärda hello kommandot nedan:

    hive -e "show partitions nyctaxidb.fare;"

hello förväntad utdata visas nedan:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Datagranskning och funktionen teknikerna i Hive
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

Hej datagranskning och funktion tekniska uppgifter för hello data lästs in i hello Hive-tabeller kan utföras med hjälp av Hive-frågor. Här följer exempel på sådana uppgifter att vi dig igenom i det här avsnittet:

* Visa översta 10 hello-poster i båda tabellerna.
* Utforska data distributioner av ett fåtal fält i olika tidsfönster.
* Undersök data quality hello longitud och latitud fält.
* Generera binära och multiklass-baserad klassificeringsetiketter baserat på hello **tips\_belopp**.
* Generera funktioner genom att beräkna hello direkt kommunikation avstånd.

### <a name="exploration-view-hello-top-10-records-in-table-trip"></a>Undersökning: Visa hello översta 10 poster i tabellen resa
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

toosee vilka hello data ser ut, vi undersöka 10 poster från varje tabell. Kör hello separat följande två frågor från hello Hive directory efterfrågas i hello Hadoop kommandoraden konsolen tooinspect hello poster.

tooget hello översta 10 poster i hello tabell ”resa” från hello första månad:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

tooget hello översta 10 poster i hello tabell ”avgiften” från hello första månad:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Det är ofta användbara toosave hello poster tooa fil för lämplig visning. En mindre ändring toohello ovan frågan åstadkommer detta:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-hello-number-of-records-in-each-of-hello-12-partitions"></a>Undersökning: Visa hello antal poster i varje hello 12 partitioner
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

Är hello hur hello antalet turer varierar under kalenderåret hello av intresse. Gruppera efter månad kan vi toosee hur den här distributionen av resor ser ut.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Detta ger oss hello utdata:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Här, hello första kolumnen är hello månad och hello är andra hello antalet turer för den månaden.

Vi kan även räkna hello Totalt antal poster i vår datauppsättning resa genom att utfärda hello följande kommando i Kommandotolken hello Hive directory.

    hive -e "select count(*) from nyctaxidb.trip;"

Detta ger:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Med kommandon liknande toothose visas för hello resa datauppsättning, kan vi utfärda Hive-frågor från hello Hive directory prompten hello avgiften data toovalidate hello antal poster.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Detta ger oss hello utdata:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Observera att hello exakt samma antal turer månaden returneras för både datamängder. Detta ger hello första verifiering som hello data har lästs in korrekt.

Cyklisk hello Totalt antal poster i hello avgiften datauppsättning kan göras med hjälp av hello-kommandot nedan från hello Hive directory prompten:

    hive -e "select count(*) from nyctaxidb.fare;"

Detta ger:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

också är hello samma hello Totalt antal poster i båda tabellerna. Detta ger en andra verifiering som hello data har lästs in korrekt.

### <a name="exploration-trip-distribution-by-medallion"></a>Undersökning: Resa distribution av medallion
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

Det här exemplet identifierar hello medallion (taxi numbers) med mer än 100 resor inom en viss tidsperiod. hello frågan fördelarna hello partitionerad tabellåtkomst eftersom den är villkorad av hello partition variabeln **månad**. hello frågeresultatet skrivs tooa lokal fil queryoutput.tsv i `C:\temp` i hello huvudnod.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Här är hello innehållet i *exempel\_hive\_resa\_antal\_av\_medallion.hql* filen för inspektion.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Hej medallion i hello NYC taxi datamängd identifierar en unik CAB-fil. Vi kan identifiera vilka CAB-filer är ”upptagna” genom att fråga vilka som gjorts mer än ett visst antal turer under en viss tidsperiod. hello följande exempel identifierar CAB-filer som gjorts flera hundra resor i hello första tre månader och sparar hello resultat tooa lokala frågefilen, C:\temp\queryoutput.tsv.

Här är hello innehållet i *exempel\_hive\_resa\_antal\_av\_medallion.hql* filen för inspektion.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Från hello Hive directory prompten problemet hello kommandot nedan:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Undersökning: Resa distribution av medallion och hack_license
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

När du utforskar en datamängd, vill vi ofta tooexamine hello antalet co-förekomster av grupper av värden. Det här avsnittet innehåller ett exempel på hur toodo för CAB-filer och drivrutiner.

Hej *exempel\_hive\_resa\_antal\_av\_medallion\_license.hql* filgrupper hello avgiften data är inställd på ”medallion” och ”hack_license” och returnerar antalet varje kombination. Nedan visas dess innehåll.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Den här frågan returnerar drivrutinen kombinationer sorterade efter fallande antalet turer och CAB-filen.

Från hello Hive directory kommandotolk, kör:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

hello frågeresultatet skrivs tooa lokal fil C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Undersökning: Utvärdera data quality genom att söka efter ogiltig longitud/latitud poster
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

En gemensam undersökande dataanalys syftar tooweed ut ogiltiga eller felaktiga poster. hello anger exemplet i det här avsnittet om antingen hello longituden eller latituden fält innehåller ett värde långt utanför hello NYC område. Eftersom det är troligt att dessa poster har en felaktig longitud-latitudvärden vill vi tooeliminate dem från alla data som är toobe används för förutsägelsemodellering.

Här är hello innehållet i *exempel\_hive\_kvalitet\_assessment.hql* filen för inspektion.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Från hello Hive directory kommandotolk, kör:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Hej *-S* argumentet som ingår i det här kommandot förhindrar hello status skärmen utskrift av hello Hive kartan/minska jobb. Detta är användbart eftersom det gör hello skärmen utskrifts hello Hive mer lättläst frågeresultatet.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Undersökning: Binära klassen distributioner av resa tips
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

För hello binära klassificeringsproblem beskrivs i hello [exempel på uppgifter som förutsägelse](machine-learning-data-science-process-hive-walkthrough.md#mltasks) avsnitt, är det användbart tooknow om ett tips gavs eller inte. Den här distributionen av tips är binär:

* Tips angivna (klass 1, tips\_belopp > 0)  
* Inget tips (0-klassen, tips\_belopp = 0).

Hej *exempel\_hive\_lutad\_frequencies.hql* -exempelfilen nedan gör detta.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Från hello Hive directory kommandotolk, kör:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-hello-multiclass-setting"></a>Undersökning: Klassen distributioner i hello multiklass-baserad inställningen
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

Hej multiklass-baserad klassificering problemet som beskrivs i hello [exempel på uppgifter som förutsägelse](machine-learning-data-science-process-hive-walkthrough.md#mltasks) avsnittet datamängden lämpar sig också tooa fysiska klassificering som toopredict hello mängden hello tips angivna. Vi kan använda lagerplatser toodefine tips adressintervall i hello-frågan. tooget hello klassen distributioner för hello olika tips intervall använder vi hello *exempel\_hive\_tips\_intervallet\_frequencies.hql* fil. Nedan visas dess innehåll.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Kör följande kommando från kommandoraden för Hadoop-konsolen hello:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Undersökning: Compute direkt avståndet mellan två longitud latitud-platser
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

Om du har ett mått på hello direkt avståndet kan oss toofind ut hello avvikelse mellan den och hello faktiska utlösas avstånd. Vi motivera funktionen genom att peka som en passagerare kan vara mindre troligt tootip om de ta reda på hello drivrutinen vidtagit dem avsiktligt av en mycket längre väg.

toosee hello jämförelse mellan faktiska resa avstånd och hello [Haversine avstånd](http://en.wikipedia.org/wiki/Haversine_formula) mellan två longitud latitud punkter (hello ”” storcirkelavståndet), vi använda hello trigonometrifunktioner som är tillgängliga i Hive, därför:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

R är hello radien för hello Earth i mil i hello frågan ovan, och pi är konverterade tooradians. Observera att ”filtrerade” tooremove värden som är långt från hello NYC området hello longitud latitud punkter.

I det här fallet skriva vi vårt resultat tooa katalog med namnet ”queryoutputdir”. hello kommandosekvens nedan skapar den här målkatalogen först och kör sedan hello Hive-kommando.

Från hello Hive directory kommandotolk, kör:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


hello frågeresultatet skrivs too9 Azure-blobbar ***queryoutputdir/000000\_0*** för ***queryoutputdir/000008\_0*** under hello standardbehållaren för hello Hadoop-kluster.

toosee hello storleken på hello enskilda blobbar, vi kör följande kommando från hello Hive directory prompten hello:

    hdfs dfs -ls wasb:///queryoutputdir

toosee hello innehållet i en viss fil, säg 000000\_0, använder vi Hadoop's `copyToLocal` kommandot därför.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`rekommenderas inte för användning med dem och kan ta mycket lång tid för stora filer.  
> 
> 

En stor fördel med dessa data finns i en Azure blob är att vi kan utforska hello data i Azure Machine Learning med hello [importera Data] [ import-data] modul.

## <a name="#downsample"></a>Ned exempel data och bygga modeller i Azure Machine Learning
> [!NOTE]
> Detta är vanligtvis en **Data forskare** aktivitet.
> 
> 

Efter hello undersökande data analysfasen är vi nu redo toodown hello exempeldata för att skapa modeller i Azure Machine Learning. I det här avsnittet visar vi hur toouse registreringsdata fråga toodown hello exempeldata, som sedan öppnas från hello [importera Data] [ import-data] modul i Azure Machine Learning.

### <a name="down-sampling-hello-data"></a>Ned hello datasampling
Det finns två steg i den här proceduren. Först vi ansluta hello **nyctaxidb.trip** och **nyctaxidb.fare** tabeller på tre nycklar som finns i alla poster: ”medallion” ”, hacka\_licens”, och ”hämtning\_datetime”. Vi sedan generera en binär klassificering etikett **lutad** och en etikett för flera klassen klassificering **tips\_klassen**.

toobe kan toouse hello ned samplade data direkt från hello [importera Data] [ import-data] modul i Azure Machine Learning är det nödvändigt toostore hello resultaten av hello ovan frågan tooan interna Hive-tabell. I följande, vi skapa en intern Hive-tabell och fylla i dess innehåll med hello ansluten och ned samplade data.

hello fråga gäller Hive standardfunktioner direkt toogenerate hello timme på dagen och veckan på året, veckodag (1 står för måndag och 7 står för söndag) från hello ”hämtning\_datetime” fältet och hello direkt avståndet mellan hello hämtning och dropoff platser. Användare kan se för[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) för en fullständig lista över dessa funktioner.

Hej fråga sedan ned exempel hello data så att hello frågeresultat passar in i hello Azure Machine Learning Studio. Endast ca 1% av hello ursprungliga datauppsättningen importeras till hello Studio.

Nedan visas hello innehållet i *exempel\_hive\_förbereda\_för\_aml\_full.hql* filen som förbereder data för modellen skapar i Azure Machine Learning.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of hello join into hello above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

toorun fråga för den här frågan från hello Hive directory:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Nu har vi en intern tabell ”nyctaxidb.nyctaxi_downsampled_dataset” som kan nås med hjälp av hello [importera Data] [ import-data] modul från Azure Machine Learning. Vi kan dessutom använda denna dataset för att skapa Machine Learning-modeller.  

### <a name="use-hello-import-data-module-in-azure-machine-learning-tooaccess-hello-down-sampled-data"></a>Använd hello importera Data modul i Azure Machine Learning tooaccess hello ned samplade data
Som krav för utfärdande av Hive-frågor i hello [importera Data] [ import-data] modulen för Azure Machine Learning vi behöver komma åt tooan Azure Machine Learning-arbetsytan och komma åt toohello autentiseringsuppgifterna för hello klustret och dess associerade lagringskontot.

Vissa detaljer på hello [importera Data] [ import-data] tooinput modulen och hello parametrar:

**HCatalog server URI**: om hello klusternamnet är abc123, så det är bara: https://abc123.azurehdinsight.net

**Hadoop användarkontonamnet** : hello användarnamn som valts för klustret hello (**inte** hello användarnamnet)

**Kontolösenord för Hadoop ser** : hello lösenord du valde för hello kluster (**inte** hello remote access-lösenordet)

**Platsen för utdata** : detta väljs toobe Azure.

**Azure lagringskontonamnet** : namnet på hello standardkontot för lagring som associerats med hello kluster.

**Azure behållarnamn** : Detta är hello behållare för standardnamnet för hello kluster och är vanligtvis hello samma som hello klusternamnet. Detta är bara abc123 för ett kluster som heter ”abc123”.

> [!IMPORTANT]
> **Alla tabeller som vi vill tooquery med hello [importera Data] [ import-data] modul i Azure Machine Learning måste vara en intern tabell.** Ett tips för att avgöra om en tabell T i en databas D.db är en intern tabell är som följer.
> 
> 

Hive directory prompten problemet hello kommandot från hello:

    hdfs dfs -ls wasb:///D.db/T

Om hello tabell är en intern tabell och det fylls, måste innehållet visas här. Ett annat sätt toodetermine om en tabell är en intern tabell är toouse hello Azure Lagringsutforskaren. Använda det toonavigate toohello behållare standardnamnet hello klustret och sedan filtrera efter hello tabellnamn. Om hello tabell och dess innehåll visas, bekräftar det att det är en intern tabell.

Här är en ögonblicksbild av hello Hive-fråga och hello [importera Data] [ import-data] modulen:

![Hive-fråga för modulen importera Data](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Observera att sedan vår ned samplade data finns i standardbehållaren hello hello resulterande Hive-fråga från Azure Machine Learning är väldigt enkelt och bara en ”Välj * från nyctaxidb.nyctaxi\_upplösning\_data”.

hello dataset kan nu användas som hello som startpunkt för att skapa Machine Learning-modeller.

### <a name="mlmodel"></a>Bygga modeller i Azure Machine Learning
Vi kan nu kan tooproceed toomodel byggnad och distribution av modellen i [Azure Machine Learning](https://studio.azureml.net). hello data är klar för oss toouse i adressering hello förutsägelse problem som anges ovan:

**1. Binär klassificering**: toopredict om huruvida ett tips har betalat för en resa.

**Deltagaren används:** Two-class logistic regression

a. För det här problemet är våra mål (eller klassen) etiketten ”lutad”. Vår ursprungliga ned provtagning datauppsättning har några kolumner som är mål minnesläckor för klassificering experimentet. I synnerhet: tips\_klassen, tips\_belopp och totalt\_belopp avslöjar information om hello måletikett som inte är tillgänglig vid testning tid. Vi ta bort dessa kolumner från behandling med hello [Välj kolumner i datauppsättning] [ select-columns] modul.

hello ögonblicksbild nedan visar vår experiment toopredict huruvida ett tips har betalat för en given resa.

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Det här experimentet har våra mål etikett distributioner ungefär 1:1.

hello ögonblicksbild nedan visar hello distributionen av tips klassen etiketter för hello binära klassificeringsproblem.

![Distribution av tips klassen etiketter](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Därför kan får vi en AUC av 0.987 enligt hello bilden nedan.

![AUC värde](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. Multiklass-baserad klassificering**: toopredict hello många av tips för hello resan, med hjälp av hello tidigare definierade klasser.

**Deltagaren används:** multiklass-baserad logistic regression

a. För det här problemet är vårt mål (eller klassen) etiketten ”tips\_klass” vilket kan ta en av fem värden (0,1,2,3,4). Vi har några kolumner som är mål minnesläckor för experimentet som hello binär klassificering fallet. I synnerhet: lutad, tips\_totala angivet\_belopp avslöjar information om hello måletikett som inte är tillgänglig vid testning tid. Vi ta bort dessa kolumner med hello [Välj kolumner i datauppsättning] [ select-columns] modul.

hello ögonblicksbild nedan visar vår experiment toopredict i vilka bin ett tips är sannolikt toofall (klass 0: tips = 0, klass 1: tips > 0 och tips < = $5, klass 2: tips > 5 och tips < = $10, klass 3: tips > 10 och tips < = $20 Klass 4: tips > 20)

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Nu visas hur våra faktiska test klassen distribution ser ut. Vi se att klassen 0 och 1 för klassen är vanliga, hello andra klasser är ovanligt.

![Testa klassen distribution](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Det här experimentet använder vi en förvirring matrisen toolook på vår prognosens noggrannhet. Detta visas nedan.

![Förvirring matris](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Observera att vår klass noggrannhet på hello vanliga klasser är ganska bra, hello modellen utför inte bra på ”learning” på hello sällsynta klasser.

**3. Regression uppgiften**: toopredict hello tips betalats för en transport.

**Deltagaren används:** Boosted beslutsträdet

a. För det här problemet är vårt mål (eller klassen) etiketten ”tips\_belopp”. I det här fallet är vårt mål minnesläckor: lutad, tips\_klass, totalt antal\_belopp; dessa variabler avslöja information om hello tips belopp som är vanligtvis inte tillgängliga vid testning tid. Vi ta bort dessa kolumner med hello [Välj kolumner i datauppsättning] [ select-columns] modul.

hello ögonblicksbild belows visar vår experiment toopredict hello hello ges tips.

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Regression problem vi mäter hello noggrannhet med vår prognoser genom att titta på hello kvadrat fel i hello förutsägelser hello bestämningskoefficienten och hello som. Vi visas dessa nedan.

![Förutsägelse statistik](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Vi kan se att bestämningskoefficienten om hello är 0.709, innebär ungefär 71% hello variationen förklaras med vår modell koefficienter.

> [!IMPORTANT]
> Mer om Azure Machine Learning toolearn och hur tooaccess och använder den, se för[vad är Machine Learning?](machine-learning-what-is-machine-learning.md). En resurs som är användbar för att spela upp med en massa Machine Learning-experiment i Azure Machine Learning är hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/). hello galleriet omfattar en alla experiment och ger en omfattande introduktion till hello nya funktioner i Azure Machine Learning.
> 
> 

## <a name="license-information"></a>Licensinformationen
Den här genomgången exempel och dess tillhörande skript som delas av Microsoft under hello MIT-licens. Kontrollera filen LICENSE.txt för hello i hello directory hello exempelkoden på GitHub för mer information.

## <a name="references"></a>Referenser
• [Andrés Monroy NYC Taxi resor hämtningssidan](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxitransport resa Data av Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi och Limousine kommissionens forskning och statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
