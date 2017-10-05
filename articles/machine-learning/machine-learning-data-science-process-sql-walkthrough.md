---
title: "Skapa och distribuera en maskininlärningsmodell som använder SQL Server på en virtuell dator i Azure | Microsoft Docs"
description: "Processen för avancerade analyser och teknik i åtgärd"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6066b083-262c-4453-a712-a5c05acc3df8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: fashah;bradsev
ms.openlocfilehash: 6c5361c7e47209c8eb4d5630b44b3dcfeedeaf01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="the-team-data-science-process-in-action-using-sql-server"></a><span data-ttu-id="1badd-103">Team vetenskap av data i praktiken: använder SQL Server</span><span class="sxs-lookup"><span data-stu-id="1badd-103">The Team Data Science Process in action: using SQL Server</span></span>
<span data-ttu-id="1badd-104">I den här kursen går igenom processen att skapa och distribuera en maskininlärningsmodell med hjälp av SQL Server och ett offentligt tillgängliga dataset – [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset.</span><span class="sxs-lookup"><span data-stu-id="1badd-104">In this tutorial, you walk through the process of building and deploying a machine learning model using SQL Server and a publicly available dataset -- the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="1badd-105">Förfarandet som följer en standard datavetenskap arbetsflödet: infognings- och utforska data, tekniker funktioner för att underlätta learning, och sedan skapa och distribuera en modell.</span><span class="sxs-lookup"><span data-stu-id="1badd-105">The procedure follows a standard data science workflow: ingest and explore the data, engineer features to facilitate learning, then build and deploy a model.</span></span>

## <span data-ttu-id="1badd-106"><a name="dataset"></a>NYC Taxi resor Dataset-beskrivning</span><span class="sxs-lookup"><span data-stu-id="1badd-106"><a name="dataset"></a>NYC Taxi Trips Dataset Description</span></span>
<span data-ttu-id="1badd-107">NYC Taxi resa data är cirka 20GB komprimerat CSV-filer (~ 48GB okomprimerade), som består av fler än 173 miljoner enskilda resor och priser betalat för varje resa.</span><span class="sxs-lookup"><span data-stu-id="1badd-107">The NYC Taxi Trip data is about 20GB of compressed CSV files (~48GB uncompressed), comprising more than 173 million individual trips and the fares paid for each trip.</span></span> <span data-ttu-id="1badd-108">Hämtning och Samlingsbibliotek plats och tid, anonymiserade hackare (drivrutin) licensnummer och medallion (taxi's unikt id) nummer innehåller varje resa-post.</span><span class="sxs-lookup"><span data-stu-id="1badd-108">Each trip record includes the pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="1badd-109">Data omfattar alla resor år 2013 och finns i följande två datauppsättningar för varje månad:</span><span class="sxs-lookup"><span data-stu-id="1badd-109">The data covers all trips in the year 2013 and is provided in the following two datasets for each month:</span></span>

1. <span data-ttu-id="1badd-110">Trip_data CSV innehåller resa information, till exempel antal passagerare, hämtning och dropoff punkter, resa varaktighet och resa längd.</span><span class="sxs-lookup"><span data-stu-id="1badd-110">The 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="1badd-111">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="1badd-111">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="1badd-112">Trip_fare CSV innehåller information om avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar, och det totala betald.</span><span class="sxs-lookup"><span data-stu-id="1badd-112">The 'trip_fare' CSV contains details of the fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and the total amount paid.</span></span> <span data-ttu-id="1badd-113">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="1badd-113">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="1badd-114">Unik nyckel för att ansluta till resa\_data och resa\_avgiften består av fälten: medallion hackare\_licensen och hämtning\_datetime.</span><span class="sxs-lookup"><span data-stu-id="1badd-114">The unique key to join trip\_data and trip\_fare is composed of the fields: medallion, hack\_licence and pickup\_datetime.</span></span>

## <span data-ttu-id="1badd-115"><a name="mltasks"></a>Exempel på förutsägelse uppgifter</span><span class="sxs-lookup"><span data-stu-id="1badd-115"><a name="mltasks"></a>Examples of Prediction Tasks</span></span>
<span data-ttu-id="1badd-116">Vi kommer att formulera tre förutsägelse problem utifrån den *tips\_belopp*, nämligen:</span><span class="sxs-lookup"><span data-stu-id="1badd-116">We will formulate three prediction problems based on the *tip\_amount*, namely:</span></span>

1. <span data-ttu-id="1badd-117">Binär klassificering: förutsäga oavsett betalats ett tips för en resa i d.v.s. en *tips\_belopp* som är större än 0 är ett positivt exempel, när en *tips\_belopp* $0 är en negativt exempel.</span><span class="sxs-lookup"><span data-stu-id="1badd-117">Binary classification: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="1badd-118">Multiklass-baserad klassificering: att förutsäga intervallet för betald för resan-tips.</span><span class="sxs-lookup"><span data-stu-id="1badd-118">Multiclass classification: To predict the range of tip paid for the trip.</span></span> <span data-ttu-id="1badd-119">Vi dela den *tips\_belopp* i fem lagerplatser eller klasser:</span><span class="sxs-lookup"><span data-stu-id="1badd-119">We divide the *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="1badd-120">Regression uppgiften: att förutsäga mängden tips för en resa.</span><span class="sxs-lookup"><span data-stu-id="1badd-120">Regression task: To predict the amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="1badd-121"><a name="setup"></a>Ställa in Azure datavetenskap miljö för avancerade analyser</span><span class="sxs-lookup"><span data-stu-id="1badd-121"><a name="setup"></a>Setting Up the Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="1badd-122">Som du kan se den [planera din miljö](machine-learning-data-science-plan-your-environment.md) guide, det finns flera alternativ för att arbeta med NYC Taxi resor datauppsättningen i Azure:</span><span class="sxs-lookup"><span data-stu-id="1badd-122">As you can see from the [Plan Your Environment](machine-learning-data-science-plan-your-environment.md) guide, there are several options to work with the NYC Taxi Trips dataset in Azure:</span></span>

* <span data-ttu-id="1badd-123">Arbeta med data i Azure BLOB sedan modellen i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="1badd-123">Work with the data in Azure blobs then model in Azure Machine Learning</span></span>
* <span data-ttu-id="1badd-124">Läsa in data i en SQL Server-databasen och sedan modellen i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="1badd-124">Load the data into a SQL Server database then model in Azure Machine Learning</span></span>

<span data-ttu-id="1badd-125">I den här kursen visar vi parallella massimport av data till en SQL Server datagranskning, funktion tekniker och ned provtagning med hjälp av SQL Server Management Studio som använder IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="1badd-125">In this tutorial we will demonstrate parallel bulk import of the data to a SQL Server, data exploration, feature engineering and down sampling using SQL Server Management Studio as well as using IPython Notebook.</span></span> <span data-ttu-id="1badd-126">[Exempel på skript](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) och [IPython anteckningsböcker](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) delas i GitHub.</span><span class="sxs-lookup"><span data-stu-id="1badd-126">[Sample scripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) and [IPython notebooks](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) are shared in GitHub.</span></span> <span data-ttu-id="1badd-127">Det finns också en bärbar dator IPython exempel att arbeta med data i Azure BLOB på samma plats.</span><span class="sxs-lookup"><span data-stu-id="1badd-127">A sample IPython notebook to work with the data in Azure blobs is also available in the same location.</span></span>

<span data-ttu-id="1badd-128">Att ställa in Azure datavetenskap miljön:</span><span class="sxs-lookup"><span data-stu-id="1badd-128">To set up your Azure Data Science environment:</span></span>

1. [<span data-ttu-id="1badd-129">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="1badd-129">Create a storage account</span></span>](../storage/common/storage-create-storage-account.md)
2. [<span data-ttu-id="1badd-130">Skapa en arbetsyta för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="1badd-130">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
3. <span data-ttu-id="1badd-131">[Etablera en virtuell dator med datavetenskap](machine-learning-data-science-setup-sql-server-virtual-machine.md), som innehåller en SQL-Server och en bärbar dator IPython-server.</span><span class="sxs-lookup"><span data-stu-id="1badd-131">[Provision a Data Science Virtual Machine](machine-learning-data-science-setup-sql-server-virtual-machine.md), which provides a SQL Server and an IPython Notebook server.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1badd-132">Exempel på skript och IPython bärbara datorer kommer att hämtas till den virtuella datorn datavetenskap under installationen.</span><span class="sxs-lookup"><span data-stu-id="1badd-132">The sample scripts and IPython notebooks will be downloaded to your Data Science virtual machine during the setup process.</span></span> <span data-ttu-id="1badd-133">När skriptet VM efter installationen är klar blir exemplen i den Virtuella datorns dokumentbibliotek:</span><span class="sxs-lookup"><span data-stu-id="1badd-133">When the VM post-installation script completes, the samples will be in your VM's Documents library:</span></span>  
   > 
   > * <span data-ttu-id="1badd-134">Exempel skript:`C:\Users\<user_name>\Documents\Data Science Scripts`</span><span class="sxs-lookup"><span data-stu-id="1badd-134">Sample Scripts: `C:\Users\<user_name>\Documents\Data Science Scripts`</span></span>  
   > * <span data-ttu-id="1badd-135">Exempel IPython bärbara datorer:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span><span class="sxs-lookup"><span data-stu-id="1badd-135">Sample IPython Notebooks: `C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span></span>  
   >   <span data-ttu-id="1badd-136">där `<user_name>` är den Virtuella datorns Windows-inloggningsnamn.</span><span class="sxs-lookup"><span data-stu-id="1badd-136">where `<user_name>` is your VM's Windows login name.</span></span> <span data-ttu-id="1badd-137">Vi kommer att referera till exempel mappar som **exempelskript** och **exempel IPython anteckningsböcker**.</span><span class="sxs-lookup"><span data-stu-id="1badd-137">We will refer to the sample folders as **Sample Scripts** and **Sample IPython Notebooks**.</span></span>
   > 
   > 

<span data-ttu-id="1badd-138">Baserat på datamängden storlek, datakällplats och valda Azure målmiljön, det här scenariot liknar [scenariot \#5: stora datauppsättning i lokala filer, mål SQL Server i Azure VM](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span><span class="sxs-lookup"><span data-stu-id="1badd-138">Based on the dataset size, data source location, and the selected Azure target environment, this scenario is similar to [Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span></span>

## <span data-ttu-id="1badd-139"><a name="getdata"></a>Hämta Data från offentliga källa</span><span class="sxs-lookup"><span data-stu-id="1badd-139"><a name="getdata"></a>Get the Data from Public Source</span></span>
<span data-ttu-id="1badd-140">Att hämta den [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset från dess offentlig plats, kan du använda någon av metoderna som beskrivs i [flytta Data till och från Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) att kopiera data till din nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1badd-140">To get the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of the methods described in [Move Data to and from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) to copy the data to your new virtual machine.</span></span>

<span data-ttu-id="1badd-141">Att kopiera data med hjälp av AzCopy:</span><span class="sxs-lookup"><span data-stu-id="1badd-141">To copy the data using AzCopy:</span></span>

1. <span data-ttu-id="1badd-142">Logga in på den virtuella datorn (VM)</span><span class="sxs-lookup"><span data-stu-id="1badd-142">Log in to your virtual machine (VM)</span></span>
2. <span data-ttu-id="1badd-143">Skapa en ny katalog i datadisk för den virtuella datorn (Obs: Använd inte tillfällig disken som innehåller den virtuella datorn som en datadisk).</span><span class="sxs-lookup"><span data-stu-id="1badd-143">Create a new directory in the VM's data disk (Note: Do not use the Temporary Disk which comes with the VM as a Data Disk).</span></span>
3. <span data-ttu-id="1badd-144">Kör följande Azcopy kommandoraden ersätta < path_to_data_folder > med datamappen som skapats i (2) i ett kommandotolksfönster:</span><span class="sxs-lookup"><span data-stu-id="1badd-144">In a Command Prompt window, run the following Azcopy command line, replacing <path_to_data_folder> with your data folder created in (2):</span></span>
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    <span data-ttu-id="1badd-145">När AzCopy är klar totalt av 24 zippade CSV-filer (12 för resa\_data och 12 för resa\_avgiften) ska vara i datamappen.</span><span class="sxs-lookup"><span data-stu-id="1badd-145">When the AzCopy completes, a total of 24 zipped CSV files (12 for trip\_data and 12 for trip\_fare) should be in the data folder.</span></span>
4. <span data-ttu-id="1badd-146">Packa upp de hämtade filerna.</span><span class="sxs-lookup"><span data-stu-id="1badd-146">Unzip the downloaded files.</span></span> <span data-ttu-id="1badd-147">Kom ihåg vilken mapp där de okomprimerade filerna finns.</span><span class="sxs-lookup"><span data-stu-id="1badd-147">Note the folder where the uncompressed files reside.</span></span> <span data-ttu-id="1badd-148">Den här mappen kommer kallas den < sökväg\_till\_data\_filer\>.</span><span class="sxs-lookup"><span data-stu-id="1badd-148">This folder will be referred to as the <path\_to\_data\_files\>.</span></span>

## <span data-ttu-id="1badd-149"><a name="dbload"></a>Massinläsning importera Data till SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="1badd-149"><a name="dbload"></a>Bulk Import Data into SQL Server Database</span></span>
<span data-ttu-id="1badd-150">Prestanda för inläsning/överföra stora mängder data till en SQL-databas och efterföljande frågor kan förbättras genom att använda *partitionerade tabeller och vyer*.</span><span class="sxs-lookup"><span data-stu-id="1badd-150">The performance of loading/transferring large amounts of data to an SQL database and subsequent queries can be improved by using *Partitioned Tables and Views*.</span></span> <span data-ttu-id="1badd-151">I det här avsnittet kommer vi följer instruktionerna i [parallella Bulk Import med hjälp av SQL-Partition datatabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) att skapa en ny databas och läsa in data i partitionerade tabeller parallellt.</span><span class="sxs-lookup"><span data-stu-id="1badd-151">In this section, we will follow the instructions described in [Parallel Bulk Data Import Using SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) to create a new database and load the data into partitioned tables in parallel.</span></span>

1. <span data-ttu-id="1badd-152">Logga in den virtuella datorn, starta **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="1badd-152">While logged in to your VM, start **SQL Server Management Studio**.</span></span>
2. <span data-ttu-id="1badd-153">Ansluta med Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="1badd-153">Connect using Windows Authentication.</span></span>
   
    ![SSMS Anslut][12]
3. <span data-ttu-id="1badd-155">Om du har ännu inte ändra SQL Server-autentiseringsläget och skapa en ny SQL-inloggning, öppna skriptfilen med namnet **ändra\_auth.sql** i den **exempelskript** mapp.</span><span class="sxs-lookup"><span data-stu-id="1badd-155">If you have not yet changed the SQL Server authentication mode and created a new SQL login user, open the script file named **change\_auth.sql** in the **Sample Scripts** folder.</span></span> <span data-ttu-id="1badd-156">Ändra standardanvändarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="1badd-156">Change the  default user name and password.</span></span> <span data-ttu-id="1badd-157">Klicka på **! Köra** i verktygsfältet för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="1badd-157">Click **!Execute** in the toolbar to run the script.</span></span>
   
    ![Kör skript][13]
4. <span data-ttu-id="1badd-159">Kontrollera och/eller ändra SQL Server standard databasen och loggfilerna mapparna så som nyligen skapats databaser kommer att lagras i en datadisk.</span><span class="sxs-lookup"><span data-stu-id="1badd-159">Verify and/or change the SQL Server default database and log folders to ensure that newly created databases will be stored in a Data Disk.</span></span> <span data-ttu-id="1badd-160">SQL Server-VM-avbildning som är optimerad för datawarehousing belastningar är förkonfigurerad med data och loggfilen diskar.</span><span class="sxs-lookup"><span data-stu-id="1badd-160">The SQL Server VM image that is optimized for datawarehousing loads is pre-configured with data and log disks.</span></span> <span data-ttu-id="1badd-161">Om den virtuella datorn inte innehöll en datadisk och du har lagt till nya virtuella hårddiskar under installationen VM, ändra standardmappar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1badd-161">If your VM did not include a Data Disk and you added new virtual hard disks during the VM setup process, change the default folders as follows:</span></span>
   
   * <span data-ttu-id="1badd-162">Högerklicka på SQL Server-namnet i den vänstra rutan och klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1badd-162">Right-click the SQL Server name in the left panel and click **Properties**.</span></span>
     
       ![Egenskaper för SQL Server][14]
   * <span data-ttu-id="1badd-164">Välj **databasinställningar** från den **Välj en sida** listan till vänster.</span><span class="sxs-lookup"><span data-stu-id="1badd-164">Select **Database Settings** from the **Select a page** list to the left.</span></span>
   * <span data-ttu-id="1badd-165">Kontrollera och/eller ändra den **databasen standardplatserna** till den **datadisk** platser för ditt val.</span><span class="sxs-lookup"><span data-stu-id="1badd-165">Verify and/or change the **Database default locations** to the **Data Disk** locations of your choice.</span></span> <span data-ttu-id="1badd-166">Detta är där nya databaser finns om skapas med standardinställningar för platsen.</span><span class="sxs-lookup"><span data-stu-id="1badd-166">This is where new databases reside if created with the default location settings.</span></span>
     
       ![Standardvärdet för SQL-databasen][15]  
5. <span data-ttu-id="1badd-168">Öppna exempelskript för att skapa en ny databas och en uppsättning filgrupper för partitionerade tabeller, **skapa\_db\_default.sql**.</span><span class="sxs-lookup"><span data-stu-id="1badd-168">To create a new database and a set of filegroups to hold the partitioned tables, open the sample script **create\_db\_default.sql**.</span></span> <span data-ttu-id="1badd-169">Skriptet skapar en ny databas med namnet **TaxiNYC** och 12 filgrupper på standardplatsen för data.</span><span class="sxs-lookup"><span data-stu-id="1badd-169">The script will create a new database named **TaxiNYC** and 12 filegroups in the default data location.</span></span> <span data-ttu-id="1badd-170">Varje filgrupp innehåller en månad resa\_data och resa\_färdavgiften data.</span><span class="sxs-lookup"><span data-stu-id="1badd-170">Each filegroup will hold one month of trip\_data and trip\_fare data.</span></span> <span data-ttu-id="1badd-171">Ändra namnet på databasen, om så önskas.</span><span class="sxs-lookup"><span data-stu-id="1badd-171">Modify the database name, if desired.</span></span> <span data-ttu-id="1badd-172">Klicka på **! Köra** att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="1badd-172">Click **!Execute** to run the script.</span></span>
6. <span data-ttu-id="1badd-173">Skapa sedan två partitionstabeller, en för resan\_data och en för resan\_avgiften.</span><span class="sxs-lookup"><span data-stu-id="1badd-173">Next, create two partition tables, one for the trip\_data and another for the trip\_fare.</span></span> <span data-ttu-id="1badd-174">Öppna exempelskriptet **skapa\_partitionerade\_table.sql**, som:</span><span class="sxs-lookup"><span data-stu-id="1badd-174">Open the sample script **create\_partitioned\_table.sql**, which will:</span></span>
   
   * <span data-ttu-id="1badd-175">Skapa en partitionsfunktion om du vill dela data per månad.</span><span class="sxs-lookup"><span data-stu-id="1badd-175">Create a partition function to split the data by month.</span></span>
   * <span data-ttu-id="1badd-176">Skapa ett partitionsschema mappa varje månad data till en annan filgrupp.</span><span class="sxs-lookup"><span data-stu-id="1badd-176">Create a partition scheme to map each month's data to a different filegroup.</span></span>
   * <span data-ttu-id="1badd-177">Skapa två partitionerade tabeller som är mappad till partitionsschemat: **nyctaxi\_resa** innehåller resan\_data och **nyctaxi\_avgiften** innehåller resan\_färdavgiften data.</span><span class="sxs-lookup"><span data-stu-id="1badd-177">Create two partitioned tables mapped to the partition scheme: **nyctaxi\_trip** will hold the trip\_data and **nyctaxi\_fare** will hold the trip\_fare data.</span></span>
     
     <span data-ttu-id="1badd-178">Klicka på **! Köra** att köra skriptet och skapa partitionerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="1badd-178">Click **!Execute** to run the script and create the partitioned tables.</span></span>
7. <span data-ttu-id="1badd-179">I den **exempelskript** mapp, det finns två exempel PowerShell-skript som demonstrerar parallella bulk import av data till SQL Server-tabeller.</span><span class="sxs-lookup"><span data-stu-id="1badd-179">In the **Sample Scripts** folder, there are two sample PowerShell scripts provided to demonstrate parallel bulk imports of data to SQL Server tables.</span></span>
   
   * <span data-ttu-id="1badd-180">**BCP\_parallella\_generic.ps1** är ett Allmänt skript till parallella bulk importera data till en tabell.</span><span class="sxs-lookup"><span data-stu-id="1badd-180">**bcp\_parallel\_generic.ps1** is a generic script to parallel bulk import data into a table.</span></span> <span data-ttu-id="1badd-181">Ändra det här skriptet för att ange indata- och variabler som anges i kommentarrader i skriptet.</span><span class="sxs-lookup"><span data-stu-id="1badd-181">Modify this script to set the input and target variables as indicated in the comment lines in the script.</span></span>
   * <span data-ttu-id="1badd-182">**BCP\_parallella\_nyctaxi.ps1** är en förkonfigurerad version av skript och kan användas till att läsa in båda tabellerna för NYC Taxi resor data.</span><span class="sxs-lookup"><span data-stu-id="1badd-182">**bcp\_parallel\_nyctaxi.ps1** is a pre-configured version of the generic script and can be used to to load both tables for the NYC Taxi Trips data.</span></span>  
8. <span data-ttu-id="1badd-183">Högerklicka på den **bcp\_parallella\_nyctaxi.ps1** skriptets namn och klicka på **redigera** att öppna den i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1badd-183">Right-click the **bcp\_parallel\_nyctaxi.ps1** script name and click **Edit** to open it in PowerShell.</span></span> <span data-ttu-id="1badd-184">Granska de förinställda variablerna och ändra enligt din valda databasens namn, mappen inkommande data, log målmappen och sökvägar till exempel format-filer **nyctaxi_trip.xml** och **nyctaxi\_fare.xml** (anges i den **exempelskript** mapp).</span><span class="sxs-lookup"><span data-stu-id="1badd-184">Review the preset variables and modify according to your selected database name, input data folder, target log folder, and paths to the  sample format files **nyctaxi_trip.xml** and **nyctaxi\_fare.xml** (provided in the **Sample Scripts** folder).</span></span>
   
    ![Massinläsning importera Data][16]
   
    <span data-ttu-id="1badd-186">Du kan också markera autentiseringsläget, standardvärdet är Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="1badd-186">You may also select the authentication mode, default is Windows Authentication.</span></span> <span data-ttu-id="1badd-187">Klicka på den gröna pilen i verktygsfältet för att köra.</span><span class="sxs-lookup"><span data-stu-id="1badd-187">Click the green arrow in the toolbar to run.</span></span> <span data-ttu-id="1badd-188">Skriptet startas 24 import massåtgärder parallell 12 för varje partitionerade tabellen.</span><span class="sxs-lookup"><span data-stu-id="1badd-188">The script will launch 24 bulk import operations in parallel, 12 for each partitioned table.</span></span> <span data-ttu-id="1badd-189">Du kan övervaka data import förloppet genom att öppna SQL Server data standardmappen som ovan.</span><span class="sxs-lookup"><span data-stu-id="1badd-189">You may monitor the data import progress by opening the SQL Server default data folder as set above.</span></span>
9. <span data-ttu-id="1badd-190">PowerShell-skriptet rapporterar start- och sluttid.</span><span class="sxs-lookup"><span data-stu-id="1badd-190">The PowerShell script reports the starting and ending times.</span></span> <span data-ttu-id="1badd-191">När alla Massredigera importen slutförts rapporteras sluttiden.</span><span class="sxs-lookup"><span data-stu-id="1badd-191">When all bulk imports complete, the ending time is reported.</span></span> <span data-ttu-id="1badd-192">Kontrollera loggen målmappen för att kontrollera att flesta importerar lyckades, dvs, inga fel rapporteras i målmappen för loggen.</span><span class="sxs-lookup"><span data-stu-id="1badd-192">Check the target log folder to verify that the bulk imports were successful, i.e., no errors reported in the target log folder.</span></span>
10. <span data-ttu-id="1badd-193">Databasen är nu redo för undersökning, funktionen tekniker och andra åtgärder efter behov.</span><span class="sxs-lookup"><span data-stu-id="1badd-193">Your database is now ready for exploration, feature engineering, and other operations as desired.</span></span> <span data-ttu-id="1badd-194">Eftersom tabellerna partitioneras enligt den **hämtning\_datetime** fältet frågor som omfattar **hämtning\_datetime** villkor i den **där** satsen drar nytta av partitionsschemat.</span><span class="sxs-lookup"><span data-stu-id="1badd-194">Since the tables are partitioned according to the **pickup\_datetime** field, queries which include **pickup\_datetime** conditions in the **WHERE** clause will benefit from the partition scheme.</span></span>
11. <span data-ttu-id="1badd-195">I **SQL Server Management Studio**, utforska det tillhandahållna exempelskriptet **exempel\_queries.sql**.</span><span class="sxs-lookup"><span data-stu-id="1badd-195">In **SQL Server Management Studio**, explore the provided sample script **sample\_queries.sql**.</span></span> <span data-ttu-id="1badd-196">Om du vill köra någon av exempelfrågor fokusera på linjerna i fråga och klicka sedan på **! Köra** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="1badd-196">To run any of the sample queries, highlight the query lines then click **!Execute** in the toolbar.</span></span>
12. <span data-ttu-id="1badd-197">NYC Taxi resor data har lästs in i två olika tabeller.</span><span class="sxs-lookup"><span data-stu-id="1badd-197">The NYC Taxi Trips data is loaded in two separate tables.</span></span> <span data-ttu-id="1badd-198">För att förbättra kopplingsåtgärder, rekommenderas att indexera tabellerna.</span><span class="sxs-lookup"><span data-stu-id="1badd-198">To improve join operations, it is highly recommended to index the tables.</span></span> <span data-ttu-id="1badd-199">Exempelskriptet **skapa\_partitionerade\_index.sql** skapar partitionerade index i sammansatta koppling nyckeln **medallion hackare\_licens och hämtning\_ datetime**.</span><span class="sxs-lookup"><span data-stu-id="1badd-199">The sample script **create\_partitioned\_index.sql** creates partitioned indexes on the composite join key **medallion, hack\_license, and pickup\_datetime**.</span></span>

## <span data-ttu-id="1badd-200"><a name="dbexplore"></a>Datagranskning och funktionen tekniker i SQLServer</span><span class="sxs-lookup"><span data-stu-id="1badd-200"><a name="dbexplore"></a>Data Exploration and Feature Engineering in SQL Server</span></span>
<span data-ttu-id="1badd-201">I det här avsnittet ska vi utföra data från kartläggning av naturresurser och funktionen generation genom att köra SQL-frågor direkt i den **SQL Server Management Studio** använda SQL Server-databasen som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="1badd-201">In this section, we will perform data exploration and feature generation by running SQL queries directly in the **SQL Server Management Studio** using the SQL Server database created earlier.</span></span> <span data-ttu-id="1badd-202">Ett exempelskript som heter **exempel\_queries.sql** har angetts i den **exempelskript** mapp.</span><span class="sxs-lookup"><span data-stu-id="1badd-202">A sample script named **sample\_queries.sql** is provided in the **Sample Scripts** folder.</span></span> <span data-ttu-id="1badd-203">Ändra skriptet för att ändra namnet på databasen, om det skiljer sig från standard: **TaxiNYC**.</span><span class="sxs-lookup"><span data-stu-id="1badd-203">Modify the script to change the database name, if it is different from the default: **TaxiNYC**.</span></span>

<span data-ttu-id="1badd-204">I den här övningen ska du:</span><span class="sxs-lookup"><span data-stu-id="1badd-204">In this exercise, we will:</span></span>

* <span data-ttu-id="1badd-205">Ansluta till **SQL Server Management Studio** med hjälp av antingen Windows-autentisering eller SQL-autentisering och SQL-inloggningsnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="1badd-205">Connect to **SQL Server Management Studio** using either Windows Authentication or using SQL Authentication and the SQL login name and password.</span></span>
* <span data-ttu-id="1badd-206">Utforska data distributioner av ett fåtal fält i olika tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="1badd-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="1badd-207">Undersök data quality longitud och latitud fält.</span><span class="sxs-lookup"><span data-stu-id="1badd-207">Investigate data quality of the longitude and latitude fields.</span></span>
* <span data-ttu-id="1badd-208">Generera binära och multiklass-baserad klassificeringsetiketter baserat på de **tips\_belopp**.</span><span class="sxs-lookup"><span data-stu-id="1badd-208">Generate binary and multiclass classification labels based on the **tip\_amount**.</span></span>
* <span data-ttu-id="1badd-209">Generera funktioner och beräkning eller jämförelse resa avstånd.</span><span class="sxs-lookup"><span data-stu-id="1badd-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="1badd-210">Koppla två tabeller och extrahera ett slumpmässigt prov som ska användas för att bygga modeller.</span><span class="sxs-lookup"><span data-stu-id="1badd-210">Join the two tables and extract a random sample that will be used to build models.</span></span>

<span data-ttu-id="1badd-211">När du är redo att fortsätta till Azure Machine Learning, kan du antingen:</span><span class="sxs-lookup"><span data-stu-id="1badd-211">When you are ready to proceed to Azure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="1badd-212">Spara slutliga SQL-frågan om du vill extrahera den exempeldata och kopiera / klistra in frågan direkt i en [importera Data] [ import-data] modul i Azure Machine Learning, eller</span><span class="sxs-lookup"><span data-stu-id="1badd-212">Save the final SQL query to extract and sample the data and copy-paste the query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="1badd-213">Spara den provade och bakåtkompilerade data som du planerar att använda för modellen bygga i en ny databas tabell och använda den nya tabellen i den [importera Data] [ import-data] modul i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="1badd-213">Persist the sampled and engineered data you plan to use for model building in a new database table and use the new table in the [Import Data][import-data] module in Azure Machine Learning.</span></span>

<span data-ttu-id="1badd-214">I det här avsnittet sparar vi sista frågan för att extrahera och den exempeldata.</span><span class="sxs-lookup"><span data-stu-id="1badd-214">In this section we will save the final query to extract and sample the data.</span></span> <span data-ttu-id="1badd-215">Den andra metoden visas i den [Datagranskning och funktionen Engineering i IPython anteckningsbok](#ipnb) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1badd-215">The second method is demonstrated in the [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span>

<span data-ttu-id="1badd-216">En snabb kontroll av antalet rader och kolumner i tabellerna fyllts i tidigare med hjälp av parallella massimport</span><span class="sxs-lookup"><span data-stu-id="1badd-216">For a quick verification of the number of rows and columns in the tables populated earlier using parallel bulk import,</span></span>

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="1badd-217">Undersökning: Resa distribution av medallion</span><span class="sxs-lookup"><span data-stu-id="1badd-217">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="1badd-218">Det här exemplet identifierar medallion (taxi numbers) med mer än 100 resor inom en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="1badd-218">This example identifies the medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="1badd-219">Frågan skulle dra nytta av partitionerad tabellåtkomst eftersom den är villkorad av partitionsschemat för **hämtning\_datetime**.</span><span class="sxs-lookup"><span data-stu-id="1badd-219">The query would benefit from the partitioned table access since it is conditioned by the partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="1badd-220">Frågar den fullständiga datauppsättningen blir också användning av partitionerad tabell och/eller index-sökning.</span><span class="sxs-lookup"><span data-stu-id="1badd-220">Querying the full dataset will also make use of the partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="1badd-221">Undersökning: Resa distribution av medallion och hack_license</span><span class="sxs-lookup"><span data-stu-id="1badd-221">Exploration: Trip distribution by medallion and hack_license</span></span>
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="1badd-222">Data utvärdering: Verifiera poster med felaktigt longituden eller latituden</span><span class="sxs-lookup"><span data-stu-id="1badd-222">Data Quality Assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="1badd-223">Det här exemplet undersöker om något av fälten longituden eller latituden antingen innehåller ett ogiltigt värde (radian grader ska vara mellan-90 och 90), eller ha (0, 0) koordinater.</span><span class="sxs-lookup"><span data-stu-id="1badd-223">This example investigates if any of the longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="1badd-224">Undersökning: Lutad jämfört med Inte lutad resor distribution</span><span class="sxs-lookup"><span data-stu-id="1badd-224">Exploration: Tipped vs. Not Tipped Trips distribution</span></span>
<span data-ttu-id="1badd-225">Det här exemplet hittar antalet turer som har lutad kontra lutad inte i en given tidpunkt tidsperiod (eller i den fullständiga datauppsättningen om täcker hela året).</span><span class="sxs-lookup"><span data-stu-id="1badd-225">This example finds the number of trips that were tipped vs. not tipped in a given time period (or in the full dataset if covering the full year).</span></span> <span data-ttu-id="1badd-226">Den här distributionen visar binära etikett distributionen senare användas för binär klassificering modellering.</span><span class="sxs-lookup"><span data-stu-id="1badd-226">This distribution reflects the binary label distribution to be later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="1badd-227">Undersökning: Tips klass-intervallet Distribution</span><span class="sxs-lookup"><span data-stu-id="1badd-227">Exploration: Tip Class/Range Distribution</span></span>
<span data-ttu-id="1badd-228">Det här exemplet beräknar fördelningen av tips intervall i en viss tidsperiod (eller den fullständiga datauppsättningen om täcker hela året).</span><span class="sxs-lookup"><span data-stu-id="1badd-228">This example computes the distribution of tip ranges in a given time period (or in the full dataset if covering the full year).</span></span> <span data-ttu-id="1badd-229">Detta är distributionen av klasserna etiketten som ska användas senare för multiklass-baserad klassificering modellering.</span><span class="sxs-lookup"><span data-stu-id="1badd-229">This is the distribution of the label classes that will be used later for multiclass classification modeling.</span></span>

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="1badd-230">Undersökning: Beräkna och jämföra resa avstånd</span><span class="sxs-lookup"><span data-stu-id="1badd-230">Exploration: Compute and Compare Trip Distance</span></span>
<span data-ttu-id="1badd-231">Det här exemplet konverterar hämtning och Samlingsbibliotek longitud och latitud till SQL geografi pekar beräknar resa avståndet med SQL geografi punkter skillnaden och returnerar ett slumpvist urval av resultaten för jämförelse.</span><span class="sxs-lookup"><span data-stu-id="1badd-231">This example converts the pickup and drop-off longitude and latitude to SQL geography points, computes the trip distance using SQL geography points difference, and returns a random sample of the results for comparison.</span></span> <span data-ttu-id="1badd-232">I exempel begränsar resultat till giltiga koordinater endast med data quality assessment frågan omfattas tidigare.</span><span class="sxs-lookup"><span data-stu-id="1badd-232">The example limits the results to valid coordinates only using the data quality assessment query covered earlier.</span></span>

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a><span data-ttu-id="1badd-233">Funktionen teknikerna i SQL-frågor</span><span class="sxs-lookup"><span data-stu-id="1badd-233">Feature Engineering in SQL Queries</span></span>
<span data-ttu-id="1badd-234">Etikett generation och geografi konvertering utforskning frågorna kan också användas för att generera etiketter och funktioner genom att ta bort cykliska.</span><span class="sxs-lookup"><span data-stu-id="1badd-234">The label generation and geography conversion exploration queries can also be used to generate labels/features by removing the counting part.</span></span> <span data-ttu-id="1badd-235">Ytterligare funktionen engineering SQL-exempel finns i den [Datagranskning och funktionen Engineering i IPython anteckningsbok](#ipnb) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1badd-235">Additional feature engineering SQL examples are provided in the [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span> <span data-ttu-id="1badd-236">Det är mer effektivt att köra funktionen generation frågor i den fullständiga datauppsättningen eller en stor del av den med hjälp av SQL-frågor som körs direkt på den SQL Server-databasinstansen.</span><span class="sxs-lookup"><span data-stu-id="1badd-236">It is more efficient to run the feature generation queries on the full dataset or a large subset of it using SQL queries which run directly on the SQL Server database instance.</span></span> <span data-ttu-id="1badd-237">Frågor kan köras i **SQL Server Management Studio**, IPython bärbar dator eller en verktyget/utvecklingsmiljö som har åtkomst till databasen lokalt eller fjärranslutet.</span><span class="sxs-lookup"><span data-stu-id="1badd-237">The queries may be executed in **SQL Server Management Studio**, IPython Notebook or any development tool/environment which can access the database locally or remotely.</span></span>

#### <a name="preparing-data-for-model-building"></a><span data-ttu-id="1badd-238">Förbereder Data för Modellskapandet</span><span class="sxs-lookup"><span data-stu-id="1badd-238">Preparing Data for Model Building</span></span>
<span data-ttu-id="1badd-239">Följande fråga kopplingar i **nyctaxi\_resa** och **nyctaxi\_avgiften** tabeller, genererar en binär klassificering etikett **lutad**, flera klassen klassificering etikett **tips\_klassen**, och extraherar ett slumpmässigt prov 1% från den kopplade fullständiga datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="1badd-239">The following query joins the **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a 1% random sample from the full joined dataset.</span></span> <span data-ttu-id="1badd-240">Den här frågan kan kopieras och klistras in direkt i den [Azure Machine Learning Studio](https://studio.azureml.net) [importera Data] [ import-data] -modulen för direkt datapåfyllning från SQL Server-databasen instans i Azure.</span><span class="sxs-lookup"><span data-stu-id="1badd-240">This query can be copied then pasted directly in the [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from the SQL Server database instance in Azure.</span></span> <span data-ttu-id="1badd-241">Frågan utesluter poster med fel (0, 0) koordinater.</span><span class="sxs-lookup"><span data-stu-id="1badd-241">The query excludes records with incorrect (0, 0) coordinates.</span></span>

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <span data-ttu-id="1badd-242"><a name="ipnb"></a>Datagranskning och funktionen tekniker i IPython anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="1badd-242"><a name="ipnb"></a>Data Exploration and Feature Engineering in IPython Notebook</span></span>
<span data-ttu-id="1badd-243">I det här avsnittet ska vi utföra datagranskning och funktion som genereras med hjälp av Python- och SQL-frågor mot SQL Server-databasen som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="1badd-243">In this section, we will perform data exploration and feature generation using both Python and SQL queries against the SQL Server database created earlier.</span></span> <span data-ttu-id="1badd-244">En exempel IPython bärbar dator med namnet **machine-Learning-data-science-process-sql-story.ipynb** har angetts i den **exempel IPython anteckningsböcker** mapp.</span><span class="sxs-lookup"><span data-stu-id="1badd-244">A sample IPython notebook named **machine-Learning-data-science-process-sql-story.ipynb** is provided in the **Sample IPython Notebooks** folder.</span></span> <span data-ttu-id="1badd-245">Den här anteckningsboken finns också på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span><span class="sxs-lookup"><span data-stu-id="1badd-245">This notebook is also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span></span>

<span data-ttu-id="1badd-246">Den rekommenderade sekvensen när du arbetar med stordata är följande:</span><span class="sxs-lookup"><span data-stu-id="1badd-246">The recommended sequence when working with big data is the following:</span></span>

* <span data-ttu-id="1badd-247">Läs i ett litet antal data till en ram i minnet data.</span><span class="sxs-lookup"><span data-stu-id="1badd-247">Read in a small sample of the data into an in-memory data frame.</span></span>
* <span data-ttu-id="1badd-248">Utföra vissa visualiseringar och explorations med hjälp av samplade data.</span><span class="sxs-lookup"><span data-stu-id="1badd-248">Perform some visualizations and explorations using the sampled data.</span></span>
* <span data-ttu-id="1badd-249">Experimentera med funktionen tekniker med hjälp av samplade data.</span><span class="sxs-lookup"><span data-stu-id="1badd-249">Experiment with feature engineering using the sampled data.</span></span>
* <span data-ttu-id="1badd-250">För större datagranskning, databehandling och funktionen tekniker Använd Python för att utfärda SQL-frågor direkt mot SQL Server-databas i Azure-VM.</span><span class="sxs-lookup"><span data-stu-id="1badd-250">For larger data exploration, data manipulation and feature engineering, use Python to issue SQL Queries directly against the SQL Server database in the Azure VM.</span></span>
* <span data-ttu-id="1badd-251">Bestäm provtagning för Azure Machine Learning modellskapandet.</span><span class="sxs-lookup"><span data-stu-id="1badd-251">Decide the sample size to use for Azure Machine Learning model building.</span></span>

<span data-ttu-id="1badd-252">När det är dags att gå vidare till Azure Machine Learning, kan du antingen:</span><span class="sxs-lookup"><span data-stu-id="1badd-252">When ready to proceed to Azure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="1badd-253">Spara slutliga SQL-frågan om du vill extrahera den exempeldata och kopiera / klistra in frågan direkt i en [importera Data] [ import-data] modul i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="1badd-253">Save the final SQL query to extract and sample the data and copy-paste the query directly into a [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="1badd-254">Den här metoden visas i den [bygga modeller i Azure Machine Learning](#mlmodel) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1badd-254">This method is demonstrated in the [Building Models in Azure Machine Learning](#mlmodel) section.</span></span>    
2. <span data-ttu-id="1badd-255">Spara provade och bakåtkompilerade data som du planerar att använda för modellen bygga i en ny databastabell och sedan använda den nya tabellen i den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="1badd-255">Persist the sampled and engineered data you plan to use for model building in a new database table, then use the new table in the [Import Data][import-data] module.</span></span>

<span data-ttu-id="1badd-256">Följande är några datagranskning datavisualisering och funktionen engineering exempel.</span><span class="sxs-lookup"><span data-stu-id="1badd-256">The following are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="1badd-257">Fler exempel finns i exemplet SQL IPython anteckningsboken i den **exempel IPython anteckningsböcker** mapp.</span><span class="sxs-lookup"><span data-stu-id="1badd-257">For more examples, see the sample SQL IPython notebook in the **Sample IPython Notebooks** folder.</span></span>

#### <a name="initialize-database-credentials"></a><span data-ttu-id="1badd-258">Initiera Databasautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1badd-258">Initialize Database Credentials</span></span>
<span data-ttu-id="1badd-259">Initiera anslutningsinställningarna databasen i följande variabler:</span><span class="sxs-lookup"><span data-stu-id="1badd-259">Initialize your database connection settings in the following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a><span data-ttu-id="1badd-260">Skapa databasanslutning</span><span class="sxs-lookup"><span data-stu-id="1badd-260">Create Database Connection</span></span>
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="1badd-261">Rapporten antalet rader och kolumner i tabellen nyctaxi_trip</span><span class="sxs-lookup"><span data-stu-id="1badd-261">Report number of rows and columns in table nyctaxi_trip</span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="1badd-262">Totalt antal rader = 173179759</span><span class="sxs-lookup"><span data-stu-id="1badd-262">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="1badd-263">Totalt antal kolumner = 14</span><span class="sxs-lookup"><span data-stu-id="1badd-263">Total number of columns = 14</span></span>

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a><span data-ttu-id="1badd-264">Läs i ett litet datasampel från SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="1badd-264">Read-in a small data sample from the SQL Server Database</span></span>
    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="1badd-265">Tid för att läsa exempeltabell är 6.492000 sekunder</span><span class="sxs-lookup"><span data-stu-id="1badd-265">Time to read the sample table is 6.492000 seconds</span></span>  
<span data-ttu-id="1badd-266">Antal rader och kolumner hämtas = (84952, 21)</span><span class="sxs-lookup"><span data-stu-id="1badd-266">Number of rows and columns retrieved = (84952, 21)</span></span>

#### <a name="descriptive-statistics"></a><span data-ttu-id="1badd-267">Beskrivande statistik</span><span class="sxs-lookup"><span data-stu-id="1badd-267">Descriptive Statistics</span></span>
<span data-ttu-id="1badd-268">Är nu redo att utforska samplade data.</span><span class="sxs-lookup"><span data-stu-id="1badd-268">Now are ready to explore the sampled data.</span></span> <span data-ttu-id="1badd-269">Vi börjar med att titta på beskrivande statistik för den **resa\_avstånd** (eller andra) nyckelfälten:</span><span class="sxs-lookup"><span data-stu-id="1badd-269">We start with looking at descriptive statistics for the **trip\_distance** (or any other) field(s):</span></span>

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a><span data-ttu-id="1badd-270">Visualiseringen: Exempel på ritytans</span><span class="sxs-lookup"><span data-stu-id="1badd-270">Visualization: Box Plot Example</span></span>
<span data-ttu-id="1badd-271">Vi titta på Låddiagram för resa avståndet visualisera quantiles</span><span class="sxs-lookup"><span data-stu-id="1badd-271">Next we look at the box plot for the trip distance to visualize the quantiles</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Rita #1][1]

#### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="1badd-273">Visualiseringen: Distribution ritytans exempel</span><span class="sxs-lookup"><span data-stu-id="1badd-273">Visualization: Distribution Plot Example</span></span>
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Rita #2][2]

#### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="1badd-275">Visualisering: Fältet och rad områden</span><span class="sxs-lookup"><span data-stu-id="1badd-275">Visualization: Bar and Line Plots</span></span>
<span data-ttu-id="1badd-276">I det här exemplet vi bin resa avståndet i fem lagerplatser och visualisera binning resultat.</span><span class="sxs-lookup"><span data-stu-id="1badd-276">In this example, we bin the trip distance into five bins and visualize the binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="1badd-277">Vi kan ritas ovan bin-distribution i ett fält eller rad ritytans enligt nedan</span><span class="sxs-lookup"><span data-stu-id="1badd-277">We can plot the above bin distribution in a bar or line plot as below</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Rita #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Rita #4][4]

#### <a name="visualization-scatterplot-example"></a><span data-ttu-id="1badd-280">Visualiseringen: Scatterplot exempel</span><span class="sxs-lookup"><span data-stu-id="1badd-280">Visualization: Scatterplot Example</span></span>
<span data-ttu-id="1badd-281">Visar vi punktdiagram ritytans mellan **resa\_tid\_i\_sek** och **resa\_avstånd** om det finns några korrelation</span><span class="sxs-lookup"><span data-stu-id="1badd-281">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** to see if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Rita #6][6]

<span data-ttu-id="1badd-283">På liknande sätt kan vi kontrollerar relationen mellan **hastighet\_kod** och **resa\_avstånd**.</span><span class="sxs-lookup"><span data-stu-id="1badd-283">Similarly we can check the relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Rita #8][8]

### <a name="sub-sampling-the-data-in-sql"></a><span data-ttu-id="1badd-285">Underordnad Datasampling i SQL</span><span class="sxs-lookup"><span data-stu-id="1badd-285">Sub-Sampling the Data in SQL</span></span>
<span data-ttu-id="1badd-286">När du förbereder data för modellen bygga i [Azure Machine Learning Studio](https://studio.azureml.net), kan du antingen välja på den **SQL-frågan ska använda direkt i modulen importera Data** eller spara bakåtkompilerade och samplade data i en ny tabellen som du kan använda i den [importera Data] [ import-data] modulen med en enkel **Välj * FROM < din\_nya\_tabell\_namn >** .</span><span class="sxs-lookup"><span data-stu-id="1badd-286">When preparing data for model building in [Azure Machine Learning Studio](https://studio.azureml.net), you may either decide on the **SQL query to use directly in the Import Data module** or persist the engineered and sampled data in a new table, which you could use in the [Import Data][import-data] module with a simple **SELECT * FROM <your\_new\_table\_name>**.</span></span>

<span data-ttu-id="1badd-287">I det här avsnittet skapar vi en ny tabell för att lagra data provade och bakåtkompilerade.</span><span class="sxs-lookup"><span data-stu-id="1badd-287">In this section we will create a new table to hold the sampled and engineered data.</span></span> <span data-ttu-id="1badd-288">Ett exempel på en direkt SQL-fråga för skapande av modellen som har angetts i den [Datagranskning och funktionen Engineering i SQL Server](#dbexplore) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1badd-288">An example of a direct SQL query for model building is provided in the [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a><span data-ttu-id="1badd-289">Skapa ett exempel på en tabell och fylla med 1% av de kopplade tabellerna.</span><span class="sxs-lookup"><span data-stu-id="1badd-289">Create a Sample Table and Populate with 1% of the Joined Tables.</span></span> <span data-ttu-id="1badd-290">Ta bort den första tabellen om den finns.</span><span class="sxs-lookup"><span data-stu-id="1badd-290">Drop Table First if it Exists.</span></span>
<span data-ttu-id="1badd-291">I det här avsnittet vi koppla tabellerna **nyctaxi\_resa** och **nyctaxi\_avgiften**Extrahera ett slumpmässigt prov 1% och bevara samplade data i ett nytt tabellnamn  **nyctaxi\_en\_procent**:</span><span class="sxs-lookup"><span data-stu-id="1badd-291">In this section, we join the tables **nyctaxi\_trip** and **nyctaxi\_fare**, extract a 1% random sample, and persist the sampled data in a new table name **nyctaxi\_one\_percent**:</span></span>

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="1badd-292">Datagranskning med hjälp av SQL-frågor i IPython anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="1badd-292">Data Exploration using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="1badd-293">I det här avsnittet förklarar vi data distributioner med hjälp av 1% provtagning data som sparas i den nya tabellen som vi skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="1badd-293">In this section, we explore data distributions using the 1% sampled data which is persisted in the new table we created above.</span></span> <span data-ttu-id="1badd-294">Observera att liknande explorations kan utföras med hjälp av de ursprungliga tabeller, vid behov kan **TABLESAMPLE** att begränsa utforskning exempel eller genom att begränsa resultaten till en given tidpunkt period med hjälp av den **hämtning\_datetime** partitioner, enligt beskrivningen i den [Datagranskning och funktionen Engineering i SQL Server](#dbexplore) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1badd-294">Note that similar explorations can be performed using the original tables, optionally using **TABLESAMPLE** to limit the exploration sample or by limiting the results to a given time period using the **pickup\_datetime** partitions, as illustrated in the [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="1badd-295">Undersökning: Dagliga distribution av resor</span><span class="sxs-lookup"><span data-stu-id="1badd-295">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="1badd-296">Undersökning: Resa distribution per medallion</span><span class="sxs-lookup"><span data-stu-id="1badd-296">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="1badd-297">Funktionen Generation med hjälp av SQL-frågor i IPython anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="1badd-297">Feature Generation Using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="1badd-298">I det här avsnittet kommer vi att generera nya etiketter och funktioner som direkt med SQL-frågor körs på 1% exempeltabell vi skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1badd-298">In this section we will generate new labels and features directly using SQL queries, operating on the 1% sample table we created in the previous section.</span></span>

#### <a name="label-generation-generate-class-labels"></a><span data-ttu-id="1badd-299">Etikettgenerering: Skapa klassen etiketter</span><span class="sxs-lookup"><span data-stu-id="1badd-299">Label Generation: Generate Class Labels</span></span>
<span data-ttu-id="1badd-300">I följande exempel skapar vi två uppsättningar med etiketter ska användas för modellering:</span><span class="sxs-lookup"><span data-stu-id="1badd-300">In the following example, we generate two sets of labels to use for modeling:</span></span>

1. <span data-ttu-id="1badd-301">Binär klassen etiketter **lutad** (förutsäga om ett tips ges)</span><span class="sxs-lookup"><span data-stu-id="1badd-301">Binary Class Labels **tipped** (predicting if a tip will be given)</span></span>
2. <span data-ttu-id="1badd-302">Multiklass-baserad etiketter **tips\_klassen** (förutsäga tips bin eller intervall)</span><span class="sxs-lookup"><span data-stu-id="1badd-302">Multiclass Labels **tip\_class** (predicting the tip bin or range)</span></span>
   
        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''
   
        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()
   
        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''
   
        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a><span data-ttu-id="1badd-303">Funktionen tekniker: Antal funktioner för Kategoriska kolumner</span><span class="sxs-lookup"><span data-stu-id="1badd-303">Feature Engineering: Count Features for Categorical Columns</span></span>
<span data-ttu-id="1badd-304">Det här exemplet transformerar ett kategoriska fält i ett numeriskt fält genom att ersätta varje kategori med antalet dess förekomster i data.</span><span class="sxs-lookup"><span data-stu-id="1badd-304">This example transforms a categorical field into a numeric field by replacing each category with the count of its occurrences in the data.</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a><span data-ttu-id="1badd-305">Funktionen tekniker: Bin funktioner för numeriska kolumner</span><span class="sxs-lookup"><span data-stu-id="1badd-305">Feature Engineering: Bin features for Numerical Columns</span></span>
<span data-ttu-id="1badd-306">Det här exemplet omvandlar en kontinuerlig numeriskt fält till förinställda kategori intervall, d.v.s. transformeringen numeriskt fält i ett kategoriska fält.</span><span class="sxs-lookup"><span data-stu-id="1badd-306">This example transforms a continuous numeric field into preset category ranges, i.e., transform numeric field into a categorical field.</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a><span data-ttu-id="1badd-307">Funktionen tekniker: Extrahera plats funktioner från Decimal latitud/longitud</span><span class="sxs-lookup"><span data-stu-id="1badd-307">Feature Engineering: Extract Location Features from Decimal Latitude/Longitude</span></span>
<span data-ttu-id="1badd-308">Det här exemplet uppdelad decimal-representation av ett latitud och longitud fält till flera region fält i olika detaljnivå som, land, ort, staden, block och så vidare. Observera att nya geo-fält inte har mappats till faktiska platser.</span><span class="sxs-lookup"><span data-stu-id="1badd-308">This example breaks down the decimal representation of a latitude and/or longitude field into multiple region fields of different granularity, such as, country, city, town, block, etc. Note that the new geo-fields are not mapped to actual locations.</span></span> <span data-ttu-id="1badd-309">Information om geocode kartläggning finns [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).</span><span class="sxs-lookup"><span data-stu-id="1badd-309">For information on mapping geocode locations, see [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a><span data-ttu-id="1badd-310">Kontrollera den slutliga utformning av tabellen featurized</span><span class="sxs-lookup"><span data-stu-id="1badd-310">Verify the final form of the featurized table</span></span>
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

<span data-ttu-id="1badd-311">Vi är nu redo att fortsätta att modellskapandet och distribution av modellen i [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="1badd-311">We are now ready to proceed to model building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="1badd-312">Data är redo för någon av de förutsägelse problem som konstaterats tidigare, nämligen:</span><span class="sxs-lookup"><span data-stu-id="1badd-312">The data is ready for any of the prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="1badd-313">Binär klassificering: för att förutsäga om huruvida ett tips har betalat för en resa.</span><span class="sxs-lookup"><span data-stu-id="1badd-313">Binary classification: To predict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="1badd-314">Multiklass-baserad klassificering: att förutsäga intervallet för tips betald enligt de tidigare definierade klasserna.</span><span class="sxs-lookup"><span data-stu-id="1badd-314">Multiclass classification: To predict the range of tip paid, according to the previously defined classes.</span></span>
3. <span data-ttu-id="1badd-315">Regression uppgiften: att förutsäga mängden tips för en resa.</span><span class="sxs-lookup"><span data-stu-id="1badd-315">Regression task: To predict the amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="1badd-316"><a name="mlmodel"></a>Skapa modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="1badd-316"><a name="mlmodel"></a>Building Models in Azure Machine Learning</span></span>
<span data-ttu-id="1badd-317">Om du vill börja modellering du logga in på Azure Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="1badd-317">To begin the modeling exercise, log in to your Azure Machine Learning workspace.</span></span> <span data-ttu-id="1badd-318">Om du inte har skapat machine learning-arbetsytan finns [skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="1badd-318">If you have not yet created a machine learning workspace, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="1badd-319">Om du vill komma igång med Azure Machine Learning finns [vad är Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="1badd-319">To get started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="1badd-320">Logga in på [Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="1badd-320">Log in to [Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="1badd-321">Sidan Studio innehåller en mängd information, videor, självstudier, länkar till moduler referens och andra resurser.</span><span class="sxs-lookup"><span data-stu-id="1badd-321">The Studio Home page provides a wealth of information, videos, tutorials, links to the Modules Reference, and other resources.</span></span> <span data-ttu-id="1badd-322">Mer information om Azure Machine Learning finns i [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="1badd-322">Fore more information about Azure Machine Learning, consult the [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="1badd-323">En typisk träningsexperiment består av följande:</span><span class="sxs-lookup"><span data-stu-id="1badd-323">A typical training experiment consists of the following:</span></span>

1. <span data-ttu-id="1badd-324">Skapa en **+ ny** experiment.</span><span class="sxs-lookup"><span data-stu-id="1badd-324">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="1badd-325">Hämta data till Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="1badd-325">Get the data to Azure Machine Learning.</span></span>
3. <span data-ttu-id="1badd-326">Förbearbeta, transformera och manipulera data efter behov.</span><span class="sxs-lookup"><span data-stu-id="1badd-326">Pre-process, transform and manipulate the data as needed.</span></span>
4. <span data-ttu-id="1badd-327">Generera funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="1badd-327">Generate features as needed.</span></span>
5. <span data-ttu-id="1badd-328">Dela data i utbildning / / verifieringstesterna datauppsättningar (eller ha separata datauppsättningar för varje).</span><span class="sxs-lookup"><span data-stu-id="1badd-328">Split the data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="1badd-329">Välj en eller flera maskininlärningsalgoritmer beroende på learning problemet att lösa.</span><span class="sxs-lookup"><span data-stu-id="1badd-329">Select one or more machine learning algorithms depending on the learning problem to solve.</span></span> <span data-ttu-id="1badd-330">T.ex. binär klassificering, multiklass-baserad klassificering, regression.</span><span class="sxs-lookup"><span data-stu-id="1badd-330">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="1badd-331">Träna en eller flera modeller med hjälp av utbildning dataset.</span><span class="sxs-lookup"><span data-stu-id="1badd-331">Train one or more models using the training dataset.</span></span>
8. <span data-ttu-id="1badd-332">Poängsätta validering datauppsättningen med den tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="1badd-332">Score the validation dataset using the trained model(s).</span></span>
9. <span data-ttu-id="1badd-333">Utvärdera modeller för att beräkna relevanta mätvärden för learning problemet.</span><span class="sxs-lookup"><span data-stu-id="1badd-333">Evaluate the model(s) to compute the relevant metrics for the learning problem.</span></span>
10. <span data-ttu-id="1badd-334">Bra finjustera modeller och välj den bästa modellen för distribution.</span><span class="sxs-lookup"><span data-stu-id="1badd-334">Fine tune the model(s) and select the best model to deploy.</span></span>

<span data-ttu-id="1badd-335">I den här övningen har vi redan utforskade och utformad data i SQL Server och valt exempelstorleken att mata in i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="1badd-335">In this exercise, we have already explored and engineered the data in SQL Server, and decided on the sample size to ingest in Azure Machine Learning.</span></span> <span data-ttu-id="1badd-336">Att skapa en eller flera av förutsägelse vi valt:</span><span class="sxs-lookup"><span data-stu-id="1badd-336">To build one or more of the prediction models we decided:</span></span>

1. <span data-ttu-id="1badd-337">Hämta data till Azure Machine Learning med hjälp av den [importera Data] [ import-data] modulen är tillgängliga i den **Data ingående och utgående** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1badd-337">Get the data to Azure Machine Learning using the [Import Data][import-data] module, available in the **Data Input and Output** section.</span></span> <span data-ttu-id="1badd-338">Mer information finns i [importera Data] [ import-data] modulsida referens.</span><span class="sxs-lookup"><span data-stu-id="1badd-338">For more information, see the [Import Data][import-data] module reference page.</span></span>
   
    ![Azure Machine Learning importera Data][17]
2. <span data-ttu-id="1badd-340">Välj **Azure SQL Database** som den **datakällan** i den **egenskaper** panelen.</span><span class="sxs-lookup"><span data-stu-id="1badd-340">Select **Azure SQL Database** as the **Data source** in the **Properties** panel.</span></span>
3. <span data-ttu-id="1badd-341">Ange DNS-namnet för databasen i den **Databasservernamnet** fältet.</span><span class="sxs-lookup"><span data-stu-id="1badd-341">Enter the database DNS name in the **Database server name** field.</span></span> <span data-ttu-id="1badd-342">Format:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="1badd-342">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="1badd-343">Ange den **databasnamnet** i motsvarande fält.</span><span class="sxs-lookup"><span data-stu-id="1badd-343">Enter the **Database name** in the corresponding field.</span></span>
5. <span data-ttu-id="1badd-344">Ange den **användarnamn för SQL** i den ** aqccount användarnamnet och lösenordet i den **serverlösenord**.</span><span class="sxs-lookup"><span data-stu-id="1badd-344">Enter the **SQL user name** in the **Server user aqccount name, and the password in the **Server user account password**.</span></span>
6. <span data-ttu-id="1badd-345">Kontrollera **acceptera alla servercertifikat** alternativet.</span><span class="sxs-lookup"><span data-stu-id="1badd-345">Check **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="1badd-346">I den **databasfrågan** redigera texten, klistrar in frågan som extraherar nödvändiga databasfält (inklusive eventuella beräknade fält, till exempel etiketter) och ned exempel data till den önskade provtagning.</span><span class="sxs-lookup"><span data-stu-id="1badd-346">In the **Database query** edit text area, paste the query which extracts the necessary database fields (including any computed fields such as the labels) and down samples the data to the desired sample size.</span></span>

<span data-ttu-id="1badd-347">Ett exempel på en binär klassificering experiment läsning av data direkt från SQL Server-databasen är i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="1badd-347">An example of a binary classification experiment reading data directly from the SQL Server database is in the figure below.</span></span> <span data-ttu-id="1badd-348">Liknande experiment kan konstrueras för multiklass-baserad klassificering och regressionsproblem.</span><span class="sxs-lookup"><span data-stu-id="1badd-348">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Azure Machine Learning Train][10]

> [!IMPORTANT]
> <span data-ttu-id="1badd-350">För modellering data extrahering och samlar frågan exempel som i föregående avsnitt **alla etiketter för de tre modellering övningarna ingår i frågan**.</span><span class="sxs-lookup"><span data-stu-id="1badd-350">In the modeling data extraction and sampling query examples provided in previous sections, **all labels for the three modeling exercises are included in the query**.</span></span> <span data-ttu-id="1badd-351">Ett viktigt steg i (obligatoriskt) i varje modellering övningarna är att **undanta** onödiga etiketterna för de andra två problem och andra **mål minnesläckor**.</span><span class="sxs-lookup"><span data-stu-id="1badd-351">An important (required) step in each of the modeling exercises is to **exclude** the unnecessary labels for the other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="1badd-352">För t.ex. när du använder binär klassificering, använda etiketten **lutad** och utelämna fälten **tips\_klassen**, **tips\_belopp**, och **totala\_belopp**.</span><span class="sxs-lookup"><span data-stu-id="1badd-352">For e.g., when using binary classification, use the label **tipped** and exclude the fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="1badd-353">Dessa är målet minnesläckor eftersom de innebär tips betald.</span><span class="sxs-lookup"><span data-stu-id="1badd-353">The latter are target leaks since they imply the tip paid.</span></span>
> 
> <span data-ttu-id="1badd-354">Om du vill exkludera onödiga kolumner och/eller mål minnesläckor, kan du använda den [Välj kolumner i datauppsättning] [ select-columns] modul eller [redigera Metadata][edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="1badd-354">To exclude unnecessary columns and/or target leaks, you may use the [Select Columns in Dataset][select-columns] module or the [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="1badd-355">Mer information finns i [Välj kolumner i datauppsättning] [ select-columns] och [redigera Metadata] [ edit-metadata] referera sidor.</span><span class="sxs-lookup"><span data-stu-id="1badd-355">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="1badd-356"><a name="mldeploy"></a>Distribuera modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="1badd-356"><a name="mldeploy"></a>Deploying Models in Azure Machine Learning</span></span>
<span data-ttu-id="1badd-357">När modellen är klar, kan du enkelt distribuera det som en webbtjänst direkt från experimentet.</span><span class="sxs-lookup"><span data-stu-id="1badd-357">When your model is ready, you can easily deploy it as a web service directly from the experiment.</span></span> <span data-ttu-id="1badd-358">Mer information om hur du distribuerar Azure Machine Learning-webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="1badd-358">For more information about deploying Azure Machine Learning web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="1badd-359">Om du vill distribuera en ny webbtjänst, måste du:</span><span class="sxs-lookup"><span data-stu-id="1badd-359">To deploy a new web service, you need to:</span></span>

1. <span data-ttu-id="1badd-360">Skapa ett bedömningsprofil experiment.</span><span class="sxs-lookup"><span data-stu-id="1badd-360">Create a scoring experiment.</span></span>
2. <span data-ttu-id="1badd-361">Distribuera webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="1badd-361">Deploy the web service.</span></span>

<span data-ttu-id="1badd-362">Så här skapar du ett bedömningsprofil experiment från en **avslutad** utbildning experiment, klickar du på **skapa BEDÖMNINGEN EXPERIMENTERA** i Åtgärdsfältet lägre.</span><span class="sxs-lookup"><span data-stu-id="1badd-362">To create a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in the lower action bar.</span></span>

![Azure bedömningen][18]

<span data-ttu-id="1badd-364">Azure Machine Learning försöker skapa ett bedömningsprofil experiment som bygger på komponenterna för utbildning experimentet.</span><span class="sxs-lookup"><span data-stu-id="1badd-364">Azure Machine Learning will attempt to create a scoring experiment based on the components of the training experiment.</span></span> <span data-ttu-id="1badd-365">I synnerhet att:</span><span class="sxs-lookup"><span data-stu-id="1badd-365">In particular, it will:</span></span>

1. <span data-ttu-id="1badd-366">Spara den tränade modellen och ta bort modellen utbildningsmoduler.</span><span class="sxs-lookup"><span data-stu-id="1badd-366">Save the trained model and remove the model training modules.</span></span>
2. <span data-ttu-id="1badd-367">Identifiera en logisk **inkommande port** som representerar det förväntade indata-schemat.</span><span class="sxs-lookup"><span data-stu-id="1badd-367">Identify a logical **input port** to represent the expected input data schema.</span></span>
3. <span data-ttu-id="1badd-368">Identifiera en logisk **utgående port** att representera utdataschema förväntade web service.</span><span class="sxs-lookup"><span data-stu-id="1badd-368">Identify a logical **output port** to represent the expected web service output schema.</span></span>

<span data-ttu-id="1badd-369">När bedömningsprofil experiment skapas, granska den och justera efter behov.</span><span class="sxs-lookup"><span data-stu-id="1badd-369">When the scoring experiment is created, review it and adjust as needed.</span></span> <span data-ttu-id="1badd-370">En typisk justering är att ersätta inkommande dataset och/eller fråga med en vilket utesluter etikett fält, eftersom dessa inte är tillgänglig när tjänsten har anropats.</span><span class="sxs-lookup"><span data-stu-id="1badd-370">A typical adjustment is to replace the input dataset and/or query with one which excludes label fields, as these will not be available when the service is called.</span></span> <span data-ttu-id="1badd-371">Det är också en bra idé att minska storleken på inkommande dataset och/eller fråga till några poster bara tillräckligt för att ange det inkommande schemat.</span><span class="sxs-lookup"><span data-stu-id="1badd-371">It is also a good practice to reduce the size of the input dataset and/or query to a few records, just enough to indicate the input schema.</span></span> <span data-ttu-id="1badd-372">För den utgående porten, är det vanligt att undanta alla inmatningsfält och bara ta med den **poängsatta etiketter** och **bedömas sannolikhet** i utdata med den [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="1badd-372">For the output port, it is common to exclude all input fields and only include the **Scored Labels** and **Scored Probabilities** in the output using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="1badd-373">Ett exempel på en bedömningen experiment är i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="1badd-373">A sample scoring experiment is in the figure below.</span></span> <span data-ttu-id="1badd-374">När du är klar att distribuera klickar du på den **publicera WEBBTJÄNSTEN** knappen i det nedre Åtgärdsfältet.</span><span class="sxs-lookup"><span data-stu-id="1badd-374">When ready to deploy, click the **PUBLISH WEB SERVICE** button in the lower action bar.</span></span>

![Publicera Azure Machine Learning][11]

<span data-ttu-id="1badd-376">Om du vill Sammanfattningsvis i den här genomgången självstudiekursen har du skapat ett Azure datavetenskap miljö, arbetat med en stor offentliga datauppsättning allt från datainsamling modellera träning och distributionen av en Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="1badd-376">To recap, in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset all the way from data acquisition to model training and deploying of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="1badd-377">Licensinformationen</span><span class="sxs-lookup"><span data-stu-id="1badd-377">License Information</span></span>
<span data-ttu-id="1badd-378">Den här genomgången exempel och dess tillhörande skript och IPython notebook(s) delas av Microsoft under MIT-licensen.</span><span class="sxs-lookup"><span data-stu-id="1badd-378">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under the MIT license.</span></span> <span data-ttu-id="1badd-379">Kontrollera filen LICENSE.txt i katalogen exempelkoden på GitHub för mer information.</span><span class="sxs-lookup"><span data-stu-id="1badd-379">Please check the LICENSE.txt file in in the directory of the sample code on GitHub for more details.</span></span>

### <a name="references"></a><span data-ttu-id="1badd-380">Referenser</span><span class="sxs-lookup"><span data-stu-id="1badd-380">References</span></span>
<span data-ttu-id="1badd-381">• [Andrés Monroy NYC Taxi resor hämtningssidan](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="1badd-381">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="1badd-382">• [FOILing NYC Taxitransport resa Data av Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="1badd-382">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="1badd-383">• [NYC Taxi och Limousine kommissionens forskning och statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="1badd-383">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
