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
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a><span data-ttu-id="5c80d-103">Analysera svarta fördröjning data med hjälp av Hive på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="5c80d-103">Analyze flight delay data by using Hive on Linux-based HDInsight</span></span>

<span data-ttu-id="5c80d-104">Lär dig hur tooanalyze svarta fördröjning data med hjälp av Hive på Linux-baserade HDInsight sedan exportera hello data tooAzure SQL-databas med hjälp av Sqoop.</span><span class="sxs-lookup"><span data-stu-id="5c80d-104">Learn how tooanalyze flight delay data using Hive on Linux-based HDInsight then export hello data tooAzure SQL Database using Sqoop.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c80d-105">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="5c80d-105">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="5c80d-106">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5c80d-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="5c80d-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="5c80d-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5c80d-108">Krav</span><span class="sxs-lookup"><span data-stu-id="5c80d-108">Prerequisites</span></span>

* <span data-ttu-id="5c80d-109">**Ett HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="5c80d-109">**An HDInsight cluster**.</span></span> <span data-ttu-id="5c80d-110">Se [komma igång med Hadoop med Hive i HDInsight på Linux](hdinsight-hadoop-linux-tutorial-get-started.md) anvisningar om hur du skapar ett nytt Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="5c80d-110">See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md) for steps on creating a new Linux-based HDInsight cluster.</span></span>

* <span data-ttu-id="5c80d-111">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="5c80d-111">**Azure SQL Database**.</span></span> <span data-ttu-id="5c80d-112">Du kan använda en Azure SQL database som ett dataarkiv som mål.</span><span class="sxs-lookup"><span data-stu-id="5c80d-112">You use an Azure SQL database as a destination data store.</span></span> <span data-ttu-id="5c80d-113">Om du inte redan har en SQL-databas finns [SQL Database-Självstudier: skapa en SQL-databas i minuter](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="5c80d-113">If you do not have a SQL Database already, see [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span>

* <span data-ttu-id="5c80d-114">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="5c80d-114">**Azure CLI**.</span></span> <span data-ttu-id="5c80d-115">Om du inte har installerat hello Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) fler steg.</span><span class="sxs-lookup"><span data-stu-id="5c80d-115">If you have not installed hello Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) for more steps.</span></span>

## <a name="download-hello-flight-data"></a><span data-ttu-id="5c80d-116">Ladda ned hello svarta data</span><span class="sxs-lookup"><span data-stu-id="5c80d-116">Download hello flight data</span></span>

1. <span data-ttu-id="5c80d-117">Bläddra för[forskning och innovativa tekniken Administration, Bureau transport statistik][rita-website].</span><span class="sxs-lookup"><span data-stu-id="5c80d-117">Browse too[Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>

2. <span data-ttu-id="5c80d-118">Hello på sidan Välj hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="5c80d-118">On hello page, select hello following values:</span></span>

   | <span data-ttu-id="5c80d-119">Namn</span><span class="sxs-lookup"><span data-stu-id="5c80d-119">Name</span></span> | <span data-ttu-id="5c80d-120">Värde</span><span class="sxs-lookup"><span data-stu-id="5c80d-120">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="5c80d-121">Filtrera år</span><span class="sxs-lookup"><span data-stu-id="5c80d-121">Filter Year</span></span> |<span data-ttu-id="5c80d-122">2013</span><span class="sxs-lookup"><span data-stu-id="5c80d-122">2013</span></span> |
   | <span data-ttu-id="5c80d-123">Filtrera Period</span><span class="sxs-lookup"><span data-stu-id="5c80d-123">Filter Period</span></span> |<span data-ttu-id="5c80d-124">Januari</span><span class="sxs-lookup"><span data-stu-id="5c80d-124">January</span></span> |
   | <span data-ttu-id="5c80d-125">Fält</span><span class="sxs-lookup"><span data-stu-id="5c80d-125">Fields</span></span> |<span data-ttu-id="5c80d-126">År, FlightDate, UniqueCarrier, operatör, FlightNum, OriginAirportID, ursprung, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span><span class="sxs-lookup"><span data-stu-id="5c80d-126">Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span></span> <span data-ttu-id="5c80d-127">Rensa alla fält</span><span class="sxs-lookup"><span data-stu-id="5c80d-127">Clear all other fields</span></span> |

3. <span data-ttu-id="5c80d-128">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="5c80d-128">Click **Download**.</span></span>

## <a name="upload-hello-data"></a><span data-ttu-id="5c80d-129">Ladda upp hello data</span><span class="sxs-lookup"><span data-stu-id="5c80d-129">Upload hello data</span></span>

1. <span data-ttu-id="5c80d-130">Använd följande kommando tooupload hello zip-filen toohello HDInsight-klustrets huvudnod hello:</span><span class="sxs-lookup"><span data-stu-id="5c80d-130">Use hello following command tooupload hello zip file toohello HDInsight cluster head node:</span></span>

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="5c80d-131">Ersätt **FILENAME** med hello namnet hello zip-filen.</span><span class="sxs-lookup"><span data-stu-id="5c80d-131">Replace **FILENAME** with hello name of hello zip file.</span></span> <span data-ttu-id="5c80d-132">Ersätt **användarnamn** med hello SSH-inloggning för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="5c80d-132">Replace **USERNAME** with hello SSH login for hello HDInsight cluster.</span></span> <span data-ttu-id="5c80d-133">Ersätt KLUSTERNAMN med hello namnet på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="5c80d-133">Replace CLUSTERNAME with hello name of hello HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5c80d-134">Om du använder ett lösenord tooauthenticate SSH-inloggning kan ombeds du hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="5c80d-134">If you use a password tooauthenticate your SSH login, you are prompted for hello password.</span></span> <span data-ttu-id="5c80d-135">Om du använder en offentlig nyckel, måste du kanske toouse hello `-i` parametern och ange hello sökvägen toohello matchar privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="5c80d-135">If you used a public key, you may need toouse hello `-i` parameter and specify hello path toohello matching private key.</span></span> <span data-ttu-id="5c80d-136">Till exempel `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="5c80d-136">For example, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="5c80d-137">När hello överföringen är klar ansluter du toohello kluster med SSH:</span><span class="sxs-lookup"><span data-stu-id="5c80d-137">Once hello upload has completed, connect toohello cluster using SSH:</span></span>

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    <span data-ttu-id="5c80d-138">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="5c80d-138">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="5c80d-139">När du är ansluten, Använd hello följande toounzip hello ZIP-filen:</span><span class="sxs-lookup"><span data-stu-id="5c80d-139">Once connected, use hello following toounzip hello .zip file:</span></span>

    ```
    unzip FILENAME.zip
    ```

    <span data-ttu-id="5c80d-140">Det här kommandot extraheras en CSV-fil som är ungefär 60 MB.</span><span class="sxs-lookup"><span data-stu-id="5c80d-140">This command extracts a .csv file that is roughly 60 MB.</span></span>

4. <span data-ttu-id="5c80d-141">Använd hello efter kommandot toocreate en katalog på HDInsight lagring och kopiera hello toohello katalog:</span><span class="sxs-lookup"><span data-stu-id="5c80d-141">Use hello following command toocreate a directory on HDInsight storage, and then copy hello file toohello directory:</span></span>

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-hello-hiveql"></a><span data-ttu-id="5c80d-142">Skapa och köra hello HiveQL</span><span class="sxs-lookup"><span data-stu-id="5c80d-142">Create and run hello HiveQL</span></span>

<span data-ttu-id="5c80d-143">Använd hello följande steg tooimport data från hello CSV-fil till en Hive-tabell med namnet **fördröjningar**.</span><span class="sxs-lookup"><span data-stu-id="5c80d-143">Use hello following steps tooimport data from hello CSV file into a Hive table named **Delays**.</span></span>

1. <span data-ttu-id="5c80d-144">Använd hello följande kommando toocreate och redigera en ny fil med namnet **flightdelays.hql**:</span><span class="sxs-lookup"><span data-stu-id="5c80d-144">Use hello following command toocreate and edit a new file named **flightdelays.hql**:</span></span>

    ```
    nano flightdelays.hql
    ```

    <span data-ttu-id="5c80d-145">Använd hello följande text som hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="5c80d-145">Use hello following text as hello contents of this file:</span></span>

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

2. <span data-ttu-id="5c80d-146">toosave hello-fil, Använd **Ctrl + X**, sedan **Y** .</span><span class="sxs-lookup"><span data-stu-id="5c80d-146">toosave hello file, use **Ctrl + X**, then **Y** .</span></span>

3. <span data-ttu-id="5c80d-147">toostart Hive och kör hello **flightdelays.hql** fil ska du använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5c80d-147">toostart Hive and run hello **flightdelays.hql** file, use hello following command:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > <span data-ttu-id="5c80d-148">I det här exemplet `localhost` används eftersom du är ansluten toohello huvudnod hello HDInsight-kluster, vilket är där HiveServer2 körs.</span><span class="sxs-lookup"><span data-stu-id="5c80d-148">In this example, `localhost` is used since you are connected toohello head node of hello HDInsight cluster, which is where HiveServer2 is running.</span></span>

4. <span data-ttu-id="5c80d-149">En gång hello __flightdelays.hql__ skriptet har slutförts kör Använd hello följande kommando tooopen en interaktiv Beeline session:</span><span class="sxs-lookup"><span data-stu-id="5c80d-149">Once hello __flightdelays.hql__ script finishes running, use hello following command tooopen an interactive Beeline session:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. <span data-ttu-id="5c80d-150">När du får hello `jdbc:hive2://localhost:10001/>` uppmanar, Använd följande fråga tooretrieve data från hello importeras svarta fördröjning data hello.</span><span class="sxs-lookup"><span data-stu-id="5c80d-150">When you receive hello `jdbc:hive2://localhost:10001/>` prompt, use hello following query tooretrieve data from hello imported flight delay data.</span></span>

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    <span data-ttu-id="5c80d-151">Den här frågan returnerar en lista över orter att erfarna väder fördröjningar, samt hello medelvärde fördröjning tid och spara den för`/tutorials/flightdelays/output`.</span><span class="sxs-lookup"><span data-stu-id="5c80d-151">This query retrieves a list of cities that experienced weather delays, along with hello average delay time, and save it too`/tutorials/flightdelays/output`.</span></span> <span data-ttu-id="5c80d-152">Senare, Sqoop läser hello data från den här platsen och exportera det tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5c80d-152">Later, Sqoop reads hello data from this location and export it tooAzure SQL Database.</span></span>

6. <span data-ttu-id="5c80d-153">tooexit Beeline, ange `!quit` hello i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="5c80d-153">tooexit Beeline, enter `!quit` at hello prompt.</span></span>

## <a name="create-a-sql-database"></a><span data-ttu-id="5c80d-154">Skapa en SQL Database</span><span class="sxs-lookup"><span data-stu-id="5c80d-154">Create a SQL Database</span></span>

<span data-ttu-id="5c80d-155">Om du redan har en SQL-databas måste du hämta hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="5c80d-155">If you already have a SQL Database, you must get hello server name.</span></span> <span data-ttu-id="5c80d-156">Du hittar hello servernamnet i hello [Azure-portalen](https://portal.azure.com) genom att välja **SQL-databaser**, och sedan filtrering på hello namn för hello databasen du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="5c80d-156">You can find hello server name in hello [Azure portal](https://portal.azure.com) by selecting **SQL Databases**, and then filtering on hello name of hello database you wish toouse.</span></span> <span data-ttu-id="5c80d-157">hello servernamn visas i hello **SERVER** kolumn.</span><span class="sxs-lookup"><span data-stu-id="5c80d-157">hello server name is listed in hello **SERVER** column.</span></span>

<span data-ttu-id="5c80d-158">Om du inte redan har en SQL-databas att använda hello information i [SQL Database-Självstudier: skapa en SQL-databas i minuter](../sql-database/sql-database-get-started.md) toocreate en.</span><span class="sxs-lookup"><span data-stu-id="5c80d-158">If you do not already have a SQL Database, use hello information in [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md) toocreate one.</span></span> <span data-ttu-id="5c80d-159">Spara hello servernamn som används för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="5c80d-159">Save hello server name used for hello database.</span></span>

## <a name="create-a-sql-database-table"></a><span data-ttu-id="5c80d-160">Skapa en SQL-databastabell</span><span class="sxs-lookup"><span data-stu-id="5c80d-160">Create a SQL Database table</span></span>

> [!NOTE]
> <span data-ttu-id="5c80d-161">Det finns många sätt tooconnect tooSQL databas och skapa en tabell.</span><span class="sxs-lookup"><span data-stu-id="5c80d-161">There are many ways tooconnect tooSQL Database and create a table.</span></span> <span data-ttu-id="5c80d-162">Hej följande steg används [FreeTDS](http://www.freetds.org/) från hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="5c80d-162">hello following steps use [FreeTDS](http://www.freetds.org/) from hello HDInsight cluster.</span></span>


1. <span data-ttu-id="5c80d-163">Använda SSH tooconnect toohello Linux-baserade HDInsight-kluster och kör hello följande steg från hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="5c80d-163">Use SSH tooconnect toohello Linux-based HDInsight cluster, and run hello following steps from hello SSH session.</span></span>

2. <span data-ttu-id="5c80d-164">Använd följande kommando tooinstall FreeTDS hello:</span><span class="sxs-lookup"><span data-stu-id="5c80d-164">Use hello following command tooinstall FreeTDS:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. <span data-ttu-id="5c80d-165">När hello-installationen har slutförts kan du använda hello efter kommandot tooconnect toohello SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="5c80d-165">Once hello install completes, use hello following command tooconnect toohello SQL Database server.</span></span> <span data-ttu-id="5c80d-166">Ersätt **serverName** med hello SQL Database-servernamn.</span><span class="sxs-lookup"><span data-stu-id="5c80d-166">Replace **serverName** with hello SQL Database server name.</span></span> <span data-ttu-id="5c80d-167">Ersätt **adminLogin** och **adminPassword** med hello inloggningen för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5c80d-167">Replace **adminLogin** and **adminPassword** with hello login for SQL Database.</span></span> <span data-ttu-id="5c80d-168">Ersätt **databaseName** med hello databasnamn.</span><span class="sxs-lookup"><span data-stu-id="5c80d-168">Replace **databaseName** with hello database name.</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="5c80d-169">Du får utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="5c80d-169">You receive output similar toohello following text:</span></span>

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set toosqooptest
    1>
    ```

4. <span data-ttu-id="5c80d-170">Vid hello `1>` uppmanar, ange hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="5c80d-170">At hello `1>` prompt, enter hello following lines:</span></span>

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    <span data-ttu-id="5c80d-171">När hello `GO` uttryck har angetts, hello tidigare rapporter utvärderas.</span><span class="sxs-lookup"><span data-stu-id="5c80d-171">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="5c80d-172">Den här frågan skapar en tabell med namnet **fördröjningar**, med ett grupperat index.</span><span class="sxs-lookup"><span data-stu-id="5c80d-172">This query creates a table named **delays**, with a clustered index.</span></span>

    <span data-ttu-id="5c80d-173">Använd hello följande fråga tooverify som hello tabellen har skapats:</span><span class="sxs-lookup"><span data-stu-id="5c80d-173">Use hello following query tooverify that hello table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="5c80d-174">hello utdata är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="5c80d-174">hello output is similar toohello following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. <span data-ttu-id="5c80d-175">Ange `exit` på hello `1>` fråga tooexit hello tsql-verktyget.</span><span class="sxs-lookup"><span data-stu-id="5c80d-175">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="export-data-with-sqoop"></a><span data-ttu-id="5c80d-176">Exportera data med Sqoop</span><span class="sxs-lookup"><span data-stu-id="5c80d-176">Export data with Sqoop</span></span>

1. <span data-ttu-id="5c80d-177">Använd hello följande kommando tooverify att Sqoop kan se din SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="5c80d-177">Use hello following command tooverify that Sqoop can see your SQL Database:</span></span>

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    <span data-ttu-id="5c80d-178">Det här kommandot returnerar en lista över databaser, inklusive hello-databasen som du skapade tidigare hello fördröjningar tabellen i.</span><span class="sxs-lookup"><span data-stu-id="5c80d-178">This command returns a list of databases, including hello database that you created hello delays table in earlier.</span></span>

2. <span data-ttu-id="5c80d-179">Använd följande kommando tooexport data från hivesampletable toohello mobiledata tabell hello:</span><span class="sxs-lookup"><span data-stu-id="5c80d-179">Use hello following command tooexport data from hivesampletable toohello mobiledata table:</span></span>

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="5c80d-180">Sqoop ansluter toohello databas som innehåller hello fördröjningar tabell och exporterar data från hello `/tutorials/flightdelays/output` katalogtabellen toohello fördröjningar.</span><span class="sxs-lookup"><span data-stu-id="5c80d-180">Sqoop connects toohello database containing hello delays table, and exports data from hello `/tutorials/flightdelays/output` directory toohello delays table.</span></span>

3. <span data-ttu-id="5c80d-181">När hello-kommandot har slutförts kan du använda följande tooconnect toohello databasen med TSQL hello:</span><span class="sxs-lookup"><span data-stu-id="5c80d-181">After hello command completes, use hello following tooconnect toohello database using TSQL:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="5c80d-182">När du är ansluten, Använd hello följande instruktioner tooverify att hello data var exporterade toohello mobiledata tabell:</span><span class="sxs-lookup"><span data-stu-id="5c80d-182">Once connected, use hello following statements tooverify that hello data was exported toohello mobiledata table:</span></span>

    ```
    SELECT * FROM delays
    GO
    ```

    <span data-ttu-id="5c80d-183">Du bör se en lista över data i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="5c80d-183">You should see a listing of data in hello table.</span></span> <span data-ttu-id="5c80d-184">Typen `exit` tooexit hello tsql-verktyget.</span><span class="sxs-lookup"><span data-stu-id="5c80d-184">Type `exit` tooexit hello tsql utility.</span></span>

## <span data-ttu-id="5c80d-185"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c80d-185"><a id="nextsteps"></a> Next steps</span></span>

<span data-ttu-id="5c80d-186">toolearn mer sätt toowork med data i HDInsight, se hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="5c80d-186">toolearn more ways toowork with data in HDInsight, see hello following documents:</span></span>

* <span data-ttu-id="5c80d-187">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="5c80d-187">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="5c80d-188">[Använda Oozie med HDInsight][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="5c80d-188">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="5c80d-189">[Använda Sqoop med HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="5c80d-189">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="5c80d-190">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="5c80d-190">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="5c80d-191">[Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="5c80d-191">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>
* <span data-ttu-id="5c80d-192">[Utveckla Python Hadoop-strömning program för HDInsight][hdinsight-develop-streaming]</span><span class="sxs-lookup"><span data-stu-id="5c80d-192">[Develop Python Hadoop streaming programs for HDInsight][hdinsight-develop-streaming]</span></span>

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
