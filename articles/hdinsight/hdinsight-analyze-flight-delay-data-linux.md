---
title: "aaaAnalyze svarta fördröjning data med Hive i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse Hive tooanalyze svarta data på Linux-baserat HDInsight och sedan exportera hello data tooSQL databasen med Sqoop."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0c23a079-981a-4079-b3f7-ad147b4609e5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 7830457a7100880dff1c647dde1b4d203bfea3c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a>Analysera svarta fördröjning data med hjälp av Hive på Linux-baserat HDInsight

Lär dig hur tooanalyze svarta fördröjning data med hjälp av Hive på Linux-baserade HDInsight sedan exportera hello data tooAzure SQL-databas med hjälp av Sqoop.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="prerequisites"></a>Krav

* **Ett HDInsight-kluster**. Se [komma igång med Hadoop med Hive i HDInsight på Linux](hdinsight-hadoop-linux-tutorial-get-started.md) anvisningar om hur du skapar ett nytt Linux-baserat HDInsight-kluster.

* **Azure SQL Database**. Du kan använda en Azure SQL database som ett dataarkiv som mål. Om du inte redan har en SQL-databas finns [SQL Database-Självstudier: skapa en SQL-databas i minuter](../sql-database/sql-database-get-started.md).

* **Azure CLI**. Om du inte har installerat hello Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) fler steg.

## <a name="download-hello-flight-data"></a>Ladda ned hello svarta data

1. Bläddra för[forskning och innovativa tekniken Administration, Bureau transport statistik][rita-website].

2. Hello på sidan Välj hello följande värden:

   | Namn | Värde |
   | --- | --- |
   | Filtrera år |2013 |
   | Filtrera Period |Januari |
   | Fält |År, FlightDate, UniqueCarrier, operatör, FlightNum, OriginAirportID, ursprung, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Rensa alla fält |

3. Klicka på **Hämta**.

## <a name="upload-hello-data"></a>Ladda upp hello data

1. Använd följande kommando tooupload hello zip-filen toohello HDInsight-klustrets huvudnod hello:

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    Ersätt **FILENAME** med hello namnet hello zip-filen. Ersätt **användarnamn** med hello SSH-inloggning för hello HDInsight-kluster. Ersätt KLUSTERNAMN med hello namnet på hello HDInsight-kluster.

   > [!NOTE]
   > Om du använder ett lösenord tooauthenticate SSH-inloggning kan ombeds du hello lösenord. Om du använder en offentlig nyckel, måste du kanske toouse hello `-i` parametern och ange hello sökvägen toohello matchar privat nyckel. Till exempel `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. När hello överföringen är klar ansluter du toohello kluster med SSH:

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

3. När du är ansluten, Använd hello följande toounzip hello ZIP-filen:

    ```
    unzip FILENAME.zip
    ```

    Det här kommandot extraheras en CSV-fil som är ungefär 60 MB.

4. Använd hello efter kommandot toocreate en katalog på HDInsight lagring och kopiera hello toohello katalog:

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-hello-hiveql"></a>Skapa och köra hello HiveQL

Använd hello följande steg tooimport data från hello CSV-fil till en Hive-tabell med namnet **fördröjningar**.

1. Använd hello följande kommando toocreate och redigera en ny fil med namnet **flightdelays.hql**:

    ```
    nano flightdelays.hql
    ```

    Använd hello följande text som hello innehållet i den här filen:

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over hello csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- hello following lines describe hello format and location of hello file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop hello delays table if it exists
    DROP TABLE delays;
    -- Create hello delays table and populate it with data
    -- pulled in from hello CSV file (via hello external table defined previously)
    CREATE TABLE delays AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

2. toosave hello-fil, Använd **Ctrl + X**, sedan **Y** .

3. toostart Hive och kör hello **flightdelays.hql** fil ska du använda hello följande kommando:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > I det här exemplet `localhost` används eftersom du är ansluten toohello huvudnod hello HDInsight-kluster, vilket är där HiveServer2 körs.

4. En gång hello __flightdelays.hql__ skriptet har slutförts kör Använd hello följande kommando tooopen en interaktiv Beeline session:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. När du får hello `jdbc:hive2://localhost:10001/>` uppmanar, Använd följande fråga tooretrieve data från hello importeras svarta fördröjning data hello.

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Den här frågan returnerar en lista över orter att erfarna väder fördröjningar, samt hello medelvärde fördröjning tid och spara den för`/tutorials/flightdelays/output`. Senare, Sqoop läser hello data från den här platsen och exportera det tooAzure SQL-databas.

6. tooexit Beeline, ange `!quit` hello i Kommandotolken.

## <a name="create-a-sql-database"></a>Skapa en SQL Database

Om du redan har en SQL-databas måste du hämta hello servernamn. Du hittar hello servernamnet i hello [Azure-portalen](https://portal.azure.com) genom att välja **SQL-databaser**, och sedan filtrering på hello namn för hello databasen du vill toouse. hello servernamn visas i hello **SERVER** kolumn.

Om du inte redan har en SQL-databas att använda hello information i [SQL Database-Självstudier: skapa en SQL-databas i minuter](../sql-database/sql-database-get-started.md) toocreate en. Spara hello servernamn som används för hello-databasen.

## <a name="create-a-sql-database-table"></a>Skapa en SQL-databastabell

> [!NOTE]
> Det finns många sätt tooconnect tooSQL databas och skapa en tabell. Hej följande steg används [FreeTDS](http://www.freetds.org/) från hello HDInsight-kluster.


1. Använda SSH tooconnect toohello Linux-baserade HDInsight-kluster och kör hello följande steg från hello SSH-session.

2. Använd följande kommando tooinstall FreeTDS hello:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. När hello-installationen har slutförts kan du använda hello efter kommandot tooconnect toohello SQL Database-server. Ersätt **serverName** med hello SQL Database-servernamn. Ersätt **adminLogin** och **adminPassword** med hello inloggningen för SQL-databas. Ersätt **databaseName** med hello databasnamn.

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    Du får utdata liknande toohello följande text:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set toosqooptest
    1>
    ```

4. Vid hello `1>` uppmanar, ange hello följande rader:

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    När hello `GO` uttryck har angetts, hello tidigare rapporter utvärderas. Den här frågan skapar en tabell med namnet **fördröjningar**, med ett grupperat index.

    Använd hello följande fråga tooverify som hello tabellen har skapats:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    hello utdata är liknande toohello följande text:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. Ange `exit` på hello `1>` fråga tooexit hello tsql-verktyget.

## <a name="export-data-with-sqoop"></a>Exportera data med Sqoop

1. Använd hello följande kommando tooverify att Sqoop kan se din SQL-databas:

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    Det här kommandot returnerar en lista över databaser, inklusive hello-databasen som du skapade tidigare hello fördröjningar tabellen i.

2. Använd följande kommando tooexport data från hivesampletable toohello mobiledata tabell hello:

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop ansluter toohello databas som innehåller hello fördröjningar tabell och exporterar data från hello `/tutorials/flightdelays/output` katalogtabellen toohello fördröjningar.

3. När hello-kommandot har slutförts kan du använda följande tooconnect toohello databasen med TSQL hello:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    När du är ansluten, Använd hello följande instruktioner tooverify att hello data var exporterade toohello mobiledata tabell:

    ```
    SELECT * FROM delays
    GO
    ```

    Du bör se en lista över data i hello tabell. Typen `exit` tooexit hello tsql-verktyget.

## <a id="nextsteps"></a>Nästa steg

toolearn mer sätt toowork med data i HDInsight, se hello följande dokument:

* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Oozie med HDInsight][hdinsight-use-oozie]
* [Använda Sqoop med HDInsight][hdinsight-use-sqoop]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-mapreduce]
* [Utveckla Python Hadoop-strömning program för HDInsight][hdinsight-develop-streaming]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
