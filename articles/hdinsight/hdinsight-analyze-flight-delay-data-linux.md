---
title: "Analysera svarta fördröjning data med Hive i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du använder Hive för att analysera data rör sig på Linux-baserade HDInsight och sedan exportera data till SQL Database med Sqoop."
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
ms.openlocfilehash: 8cdc19ac8a517b6d8eefabb5476a686aa252a332
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a><span data-ttu-id="73004-103">Analysera svarta fördröjning data med hjälp av Hive på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="73004-103">Analyze flight delay data by using Hive on Linux-based HDInsight</span></span>

<span data-ttu-id="73004-104">Lär dig att analysera svarta fördröjning data med Hive på Linux-baserade HDInsight och sedan exportera data till Azure SQL Database med Sqoop.</span><span class="sxs-lookup"><span data-stu-id="73004-104">Learn how to analyze flight delay data using Hive on Linux-based HDInsight then export the data to Azure SQL Database using Sqoop.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="73004-105">Stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="73004-105">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="73004-106">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="73004-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="73004-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="73004-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="73004-108">Krav</span><span class="sxs-lookup"><span data-stu-id="73004-108">Prerequisites</span></span>

* <span data-ttu-id="73004-109">**Ett HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="73004-109">**An HDInsight cluster**.</span></span> <span data-ttu-id="73004-110">Se [komma igång med Hadoop med Hive i HDInsight på Linux](hdinsight-hadoop-linux-tutorial-get-started.md) anvisningar om hur du skapar ett nytt Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="73004-110">See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md) for steps on creating a new Linux-based HDInsight cluster.</span></span>

* <span data-ttu-id="73004-111">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="73004-111">**Azure SQL Database**.</span></span> <span data-ttu-id="73004-112">Du kan använda en Azure SQL database som ett dataarkiv som mål.</span><span class="sxs-lookup"><span data-stu-id="73004-112">You use an Azure SQL database as a destination data store.</span></span> <span data-ttu-id="73004-113">Om du inte redan har en SQL-databas finns [SQL Database-Självstudier: skapa en SQL-databas i minuter](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="73004-113">If you do not have a SQL Database already, see [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span>

* <span data-ttu-id="73004-114">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="73004-114">**Azure CLI**.</span></span> <span data-ttu-id="73004-115">Om du inte har installerat Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) fler steg.</span><span class="sxs-lookup"><span data-stu-id="73004-115">If you have not installed the Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) for more steps.</span></span>

## <a name="download-the-flight-data"></a><span data-ttu-id="73004-116">Ladda ned data rör sig</span><span class="sxs-lookup"><span data-stu-id="73004-116">Download the flight data</span></span>

1. <span data-ttu-id="73004-117">Bläddra till [forskning och innovativa tekniken Administration, som administreras av transport statistik][rita-website].</span><span class="sxs-lookup"><span data-stu-id="73004-117">Browse to [Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>

2. <span data-ttu-id="73004-118">På sidan Välj följande värden:</span><span class="sxs-lookup"><span data-stu-id="73004-118">On the page, select the following values:</span></span>

   | <span data-ttu-id="73004-119">Namn</span><span class="sxs-lookup"><span data-stu-id="73004-119">Name</span></span> | <span data-ttu-id="73004-120">Värde</span><span class="sxs-lookup"><span data-stu-id="73004-120">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="73004-121">Filtrera år</span><span class="sxs-lookup"><span data-stu-id="73004-121">Filter Year</span></span> |<span data-ttu-id="73004-122">2013</span><span class="sxs-lookup"><span data-stu-id="73004-122">2013</span></span> |
   | <span data-ttu-id="73004-123">Filtrera Period</span><span class="sxs-lookup"><span data-stu-id="73004-123">Filter Period</span></span> |<span data-ttu-id="73004-124">Januari</span><span class="sxs-lookup"><span data-stu-id="73004-124">January</span></span> |
   | <span data-ttu-id="73004-125">Fält</span><span class="sxs-lookup"><span data-stu-id="73004-125">Fields</span></span> |<span data-ttu-id="73004-126">År, FlightDate, UniqueCarrier, operatör, FlightNum, OriginAirportID, ursprung, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span><span class="sxs-lookup"><span data-stu-id="73004-126">Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span></span> <span data-ttu-id="73004-127">Rensa alla fält</span><span class="sxs-lookup"><span data-stu-id="73004-127">Clear all other fields</span></span> |

3. <span data-ttu-id="73004-128">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="73004-128">Click **Download**.</span></span>

## <a name="upload-the-data"></a><span data-ttu-id="73004-129">Överföra data</span><span class="sxs-lookup"><span data-stu-id="73004-129">Upload the data</span></span>

1. <span data-ttu-id="73004-130">Använd följande kommando för att ladda upp zip-filen till HDInsight-klustrets huvudnod:</span><span class="sxs-lookup"><span data-stu-id="73004-130">Use the following command to upload the zip file to the HDInsight cluster head node:</span></span>

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="73004-131">Ersätt **FILENAME** med namn som zip-filen.</span><span class="sxs-lookup"><span data-stu-id="73004-131">Replace **FILENAME** with the name of the zip file.</span></span> <span data-ttu-id="73004-132">Ersätt **användarnamn** med SSH-inloggning för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="73004-132">Replace **USERNAME** with the SSH login for the HDInsight cluster.</span></span> <span data-ttu-id="73004-133">Ersätt KLUSTERNAMN med namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="73004-133">Replace CLUSTERNAME with the name of the HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="73004-134">Om du använder ett lösenord för att autentisera SSH-inloggning, tillfrågas du om lösenordet.</span><span class="sxs-lookup"><span data-stu-id="73004-134">If you use a password to authenticate your SSH login, you are prompted for the password.</span></span> <span data-ttu-id="73004-135">Om du använder en offentlig nyckel, kan du behöva använda de `-i` parametern och ange sökvägen till motsvarande privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="73004-135">If you used a public key, you may need to use the `-i` parameter and specify the path to the matching private key.</span></span> <span data-ttu-id="73004-136">Till exempel `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="73004-136">For example, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="73004-137">När överföringen är klar att ansluta till klustret med SSH:</span><span class="sxs-lookup"><span data-stu-id="73004-137">Once the upload has completed, connect to the cluster using SSH:</span></span>

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    <span data-ttu-id="73004-138">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="73004-138">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="73004-139">När du är ansluten, Använd följande för att packa upp ZIP-filen:</span><span class="sxs-lookup"><span data-stu-id="73004-139">Once connected, use the following to unzip the .zip file:</span></span>

    ```
    unzip FILENAME.zip
    ```

    <span data-ttu-id="73004-140">Det här kommandot extraheras en CSV-fil som är ungefär 60 MB.</span><span class="sxs-lookup"><span data-stu-id="73004-140">This command extracts a .csv file that is roughly 60 MB.</span></span>

4. <span data-ttu-id="73004-141">Använder du följande kommando för att skapa en katalog på HDInsight lagring och kopiera filen till katalogen:</span><span class="sxs-lookup"><span data-stu-id="73004-141">Use the following command to create a directory on HDInsight storage, and then copy the file to the directory:</span></span>

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-the-hiveql"></a><span data-ttu-id="73004-142">Skapa och köra HiveQL</span><span class="sxs-lookup"><span data-stu-id="73004-142">Create and run the HiveQL</span></span>

<span data-ttu-id="73004-143">Använd följande steg för att importera data från CSV-filen till en Hive-tabell med namnet **fördröjningar**.</span><span class="sxs-lookup"><span data-stu-id="73004-143">Use the following steps to import data from the CSV file into a Hive table named **Delays**.</span></span>

1. <span data-ttu-id="73004-144">Använd följande kommando för att skapa och redigera en ny fil med namnet **flightdelays.hql**:</span><span class="sxs-lookup"><span data-stu-id="73004-144">Use the following command to create and edit a new file named **flightdelays.hql**:</span></span>

    ```
    nano flightdelays.hql
    ```

    <span data-ttu-id="73004-145">Använd följande text som innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="73004-145">Use the following text as the contents of this file:</span></span>

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
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
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
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

2. <span data-ttu-id="73004-146">Om du vill spara filen, Använd **Ctrl + X**, sedan **Y** .</span><span class="sxs-lookup"><span data-stu-id="73004-146">To save the file, use **Ctrl + X**, then **Y** .</span></span>

3. <span data-ttu-id="73004-147">Att starta Hive och köra den **flightdelays.hql** fil, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="73004-147">To start Hive and run the **flightdelays.hql** file, use the following command:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > <span data-ttu-id="73004-148">I det här exemplet `localhost` används eftersom du är ansluten till HDInsight-kluster som körs där HiveServer2 huvudnod.</span><span class="sxs-lookup"><span data-stu-id="73004-148">In this example, `localhost` is used since you are connected to the head node of the HDInsight cluster, which is where HiveServer2 is running.</span></span>

4. <span data-ttu-id="73004-149">En gång i __flightdelays.hql__ skriptet har slutförts körs, Använd följande kommando för att öppna en interaktiv session Beeline:</span><span class="sxs-lookup"><span data-stu-id="73004-149">Once the __flightdelays.hql__ script finishes running, use the following command to open an interactive Beeline session:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. <span data-ttu-id="73004-150">När du får den `jdbc:hive2://localhost:10001/>` uppmanar, Använd följande fråga för att hämta data från importerade svarta fördröjning data.</span><span class="sxs-lookup"><span data-stu-id="73004-150">When you receive the `jdbc:hive2://localhost:10001/>` prompt, use the following query to retrieve data from the imported flight delay data.</span></span>

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    <span data-ttu-id="73004-151">Den här frågan returnerar en lista över orter som uppfattade väder fördröjningar, samt den genomsnittliga fördröjningen och spara den till `/tutorials/flightdelays/output`.</span><span class="sxs-lookup"><span data-stu-id="73004-151">This query retrieves a list of cities that experienced weather delays, along with the average delay time, and save it to `/tutorials/flightdelays/output`.</span></span> <span data-ttu-id="73004-152">Senare, Sqoop läser data från den här platsen och exportera dem till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="73004-152">Later, Sqoop reads the data from this location and export it to Azure SQL Database.</span></span>

6. <span data-ttu-id="73004-153">Om du vill avsluta Beeline ange `!quit` i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="73004-153">To exit Beeline, enter `!quit` at the prompt.</span></span>

## <a name="create-a-sql-database"></a><span data-ttu-id="73004-154">Skapa en SQL Database</span><span class="sxs-lookup"><span data-stu-id="73004-154">Create a SQL Database</span></span>

<span data-ttu-id="73004-155">Om du redan har en SQL-databas måste du hämta namnet på servern.</span><span class="sxs-lookup"><span data-stu-id="73004-155">If you already have a SQL Database, you must get the server name.</span></span> <span data-ttu-id="73004-156">Du kan hitta servernamnet i den [Azure-portalen](https://portal.azure.com) genom att välja **SQL-databaser**, och sedan filtrering på namnet på den databas du vill använda.</span><span class="sxs-lookup"><span data-stu-id="73004-156">You can find the server name in the [Azure portal](https://portal.azure.com) by selecting **SQL Databases**, and then filtering on the name of the database you wish to use.</span></span> <span data-ttu-id="73004-157">Namnet på server som ingår i den **SERVER** kolumn.</span><span class="sxs-lookup"><span data-stu-id="73004-157">The server name is listed in the **SERVER** column.</span></span>

<span data-ttu-id="73004-158">Om du inte redan har en SQL-databas kan du använda informationen i [SQL Database-Självstudier: skapa en SQL-databas i minuter](../sql-database/sql-database-get-started.md) att skapa en.</span><span class="sxs-lookup"><span data-stu-id="73004-158">If you do not already have a SQL Database, use the information in [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md) to create one.</span></span> <span data-ttu-id="73004-159">Spara namnet på servern som används för databasen.</span><span class="sxs-lookup"><span data-stu-id="73004-159">Save the server name used for the database.</span></span>

## <a name="create-a-sql-database-table"></a><span data-ttu-id="73004-160">Skapa en SQL-databastabell</span><span class="sxs-lookup"><span data-stu-id="73004-160">Create a SQL Database table</span></span>

> [!NOTE]
> <span data-ttu-id="73004-161">Det finns många sätt att ansluta till SQL-databas och skapa en tabell.</span><span class="sxs-lookup"><span data-stu-id="73004-161">There are many ways to connect to SQL Database and create a table.</span></span> <span data-ttu-id="73004-162">Följande steg används [FreeTDS](http://www.freetds.org/) från HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="73004-162">The following steps use [FreeTDS](http://www.freetds.org/) from the HDInsight cluster.</span></span>


1. <span data-ttu-id="73004-163">Använda SSH för att ansluta till Linux-baserat HDInsight-kluster och kör följande steg från SSH-session.</span><span class="sxs-lookup"><span data-stu-id="73004-163">Use SSH to connect to the Linux-based HDInsight cluster, and run the following steps from the SSH session.</span></span>

2. <span data-ttu-id="73004-164">Använd följande kommando för att installera FreeTDS:</span><span class="sxs-lookup"><span data-stu-id="73004-164">Use the following command to install FreeTDS:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. <span data-ttu-id="73004-165">När installationen är klar använder du följande kommando för att ansluta till SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="73004-165">Once the install completes, use the following command to connect to the SQL Database server.</span></span> <span data-ttu-id="73004-166">Ersätt **serverName** med SQL Database-servernamn.</span><span class="sxs-lookup"><span data-stu-id="73004-166">Replace **serverName** with the SQL Database server name.</span></span> <span data-ttu-id="73004-167">Ersätt **adminLogin** och **adminPassword** med inloggningen för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="73004-167">Replace **adminLogin** and **adminPassword** with the login for SQL Database.</span></span> <span data-ttu-id="73004-168">Ersätt **databaseName** med namnet på databasen.</span><span class="sxs-lookup"><span data-stu-id="73004-168">Replace **databaseName** with the database name.</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="73004-169">Visas utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="73004-169">You receive output similar to the following text:</span></span>

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. <span data-ttu-id="73004-170">På den `1>` uppmanar, ange följande rader:</span><span class="sxs-lookup"><span data-stu-id="73004-170">At the `1>` prompt, enter the following lines:</span></span>

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    <span data-ttu-id="73004-171">När den `GO` uttryck har angetts, tidigare rapporter utvärderas.</span><span class="sxs-lookup"><span data-stu-id="73004-171">When the `GO` statement is entered, the previous statements are evaluated.</span></span> <span data-ttu-id="73004-172">Den här frågan skapar en tabell med namnet **fördröjningar**, med ett grupperat index.</span><span class="sxs-lookup"><span data-stu-id="73004-172">This query creates a table named **delays**, with a clustered index.</span></span>

    <span data-ttu-id="73004-173">Använd följande fråga för att kontrollera att tabellen har skapats:</span><span class="sxs-lookup"><span data-stu-id="73004-173">Use the following query to verify that the table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="73004-174">De utdata som genereras liknar följande text:</span><span class="sxs-lookup"><span data-stu-id="73004-174">The output is similar to the following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. <span data-ttu-id="73004-175">Ange `exit` på den `1>` prompten för att avsluta tsql-verktyget.</span><span class="sxs-lookup"><span data-stu-id="73004-175">Enter `exit` at the `1>` prompt to exit the tsql utility.</span></span>

## <a name="export-data-with-sqoop"></a><span data-ttu-id="73004-176">Exportera data med Sqoop</span><span class="sxs-lookup"><span data-stu-id="73004-176">Export data with Sqoop</span></span>

1. <span data-ttu-id="73004-177">Använd följande kommando för att kontrollera att Sqoop kan se din SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="73004-177">Use the following command to verify that Sqoop can see your SQL Database:</span></span>

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    <span data-ttu-id="73004-178">Det här kommandot returnerar en lista över databaser, inklusive den databas som du skapade tidigare tabellen fördröjningar i.</span><span class="sxs-lookup"><span data-stu-id="73004-178">This command returns a list of databases, including the database that you created the delays table in earlier.</span></span>

2. <span data-ttu-id="73004-179">Använd följande kommando för att exportera data från hivesampletable till tabellen mobiledata:</span><span class="sxs-lookup"><span data-stu-id="73004-179">Use the following command to export data from hivesampletable to the mobiledata table:</span></span>

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="73004-180">Sqoop ansluter till den databas som innehåller tabellen fördröjningar och exporterar data från den `/tutorials/flightdelays/output` katalogen till tabellen fördröjningar.</span><span class="sxs-lookup"><span data-stu-id="73004-180">Sqoop connects to the database containing the delays table, and exports data from the `/tutorials/flightdelays/output` directory to the delays table.</span></span>

3. <span data-ttu-id="73004-181">När kommandot har slutförts kan du använda följande för att ansluta till databasen med TSQL:</span><span class="sxs-lookup"><span data-stu-id="73004-181">After the command completes, use the following to connect to the database using TSQL:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="73004-182">När du är ansluten, kan du använda följande instruktioner för att verifiera att data har exporterats till tabellen mobiledata:</span><span class="sxs-lookup"><span data-stu-id="73004-182">Once connected, use the following statements to verify that the data was exported to the mobiledata table:</span></span>

    ```
    SELECT * FROM delays
    GO
    ```

    <span data-ttu-id="73004-183">Du bör se en lista över data i tabellen.</span><span class="sxs-lookup"><span data-stu-id="73004-183">You should see a listing of data in the table.</span></span> <span data-ttu-id="73004-184">Typen `exit` avsluta tsql-verktyget.</span><span class="sxs-lookup"><span data-stu-id="73004-184">Type `exit` to exit the tsql utility.</span></span>

## <span data-ttu-id="73004-185"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73004-185"><a id="nextsteps"></a> Next steps</span></span>

<span data-ttu-id="73004-186">Om du vill lära dig fler sätt att arbeta med data i HDInsight finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="73004-186">To learn more ways to work with data in HDInsight, see the following documents:</span></span>

* <span data-ttu-id="73004-187">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="73004-187">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="73004-188">[Använda Oozie med HDInsight][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="73004-188">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="73004-189">[Använda Sqoop med HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="73004-189">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="73004-190">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="73004-190">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="73004-191">[Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="73004-191">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>
* <span data-ttu-id="73004-192">[Utveckla Python Hadoop-strömning program för HDInsight][hdinsight-develop-streaming]</span><span class="sxs-lookup"><span data-stu-id="73004-192">[Develop Python Hadoop streaming programs for HDInsight][hdinsight-develop-streaming]</span></span>

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
