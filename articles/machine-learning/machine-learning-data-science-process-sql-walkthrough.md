---
title: "aaaBuild och distribuera en maskininlärningsmodell som använder SQL Server på en virtuell dator i Azure | Microsoft Docs"
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
ms.openlocfilehash: 30ba9a9e3cf65f75015e13f9c7876dcbccc5bc47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-server"></a><span data-ttu-id="ef928-103">hello Team av vetenskapliga data i praktiken: använder SQL Server</span><span class="sxs-lookup"><span data-stu-id="ef928-103">hello Team Data Science Process in action: using SQL Server</span></span>
<span data-ttu-id="ef928-104">I den här kursen går igenom hello processen att skapa och distribuera en maskininlärningsmodell med hjälp av SQL Server och ett offentligt tillgängliga dataset--hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset.</span><span class="sxs-lookup"><span data-stu-id="ef928-104">In this tutorial, you walk through hello process of building and deploying a machine learning model using SQL Server and a publicly available dataset -- hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="ef928-105">hello proceduren följer ett standarddata vetenskap arbetsflöde: infognings- och utforska hello data, utveckla funktioner toofacilitate learning och sedan skapa och distribuera en modell.</span><span class="sxs-lookup"><span data-stu-id="ef928-105">hello procedure follows a standard data science workflow: ingest and explore hello data, engineer features toofacilitate learning, then build and deploy a model.</span></span>

## <span data-ttu-id="ef928-106"><a name="dataset"></a>NYC Taxi resor Dataset-beskrivning</span><span class="sxs-lookup"><span data-stu-id="ef928-106"><a name="dataset"></a>NYC Taxi Trips Dataset Description</span></span>
<span data-ttu-id="ef928-107">hello NYC Taxi resa data är cirka 20GB komprimerat CSV-filer (~ 48GB okomprimerade), som består av fler än 173 miljoner enskilda resor och hello priser för varje resa.</span><span class="sxs-lookup"><span data-stu-id="ef928-107">hello NYC Taxi Trip data is about 20GB of compressed CSV files (~48GB uncompressed), comprising more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="ef928-108">Hello hämtning och Samlingsbibliotek plats och tid, anonymiserade hackare (drivrutin) licensnummer och medallion (taxi's unikt id) nummer innehåller varje resa-post.</span><span class="sxs-lookup"><span data-stu-id="ef928-108">Each trip record includes hello pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="ef928-109">hello data omfattar alla resor hello år 2013 och finns i följande två datamängder för varje månad hello:</span><span class="sxs-lookup"><span data-stu-id="ef928-109">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="ef928-110">hello 'trip_data' CSV innehåller resa information, till exempel antal passagerare, hämtning och dropoff punkter, resa varaktighet och resa längd.</span><span class="sxs-lookup"><span data-stu-id="ef928-110">hello 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="ef928-111">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="ef928-111">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="ef928-112">hello 'trip_fare' CSV innehåller information om hello avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar och hello totalbelopp betald.</span><span class="sxs-lookup"><span data-stu-id="ef928-112">hello 'trip_fare' CSV contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="ef928-113">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="ef928-113">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="ef928-114">hello Unik nyckel toojoin resa\_data och resa\_avgiften består av hello fält: medallion hackare\_licensen och hämtning\_datetime.</span><span class="sxs-lookup"><span data-stu-id="ef928-114">hello unique key toojoin trip\_data and trip\_fare is composed of hello fields: medallion, hack\_licence and pickup\_datetime.</span></span>

## <span data-ttu-id="ef928-115"><a name="mltasks"></a>Exempel på förutsägelse uppgifter</span><span class="sxs-lookup"><span data-stu-id="ef928-115"><a name="mltasks"></a>Examples of Prediction Tasks</span></span>
<span data-ttu-id="ef928-116">Vi kommer att formulera tre förutsägelse problem utifrån hello *tips\_belopp*, nämligen:</span><span class="sxs-lookup"><span data-stu-id="ef928-116">We will formulate three prediction problems based on hello *tip\_amount*, namely:</span></span>

1. <span data-ttu-id="ef928-117">Binär klassificering: förutsäga oavsett betalats ett tips för en resa i d.v.s. en *tips\_belopp* som är större än 0 är ett positivt exempel, när en *tips\_belopp* $0 är en negativt exempel.</span><span class="sxs-lookup"><span data-stu-id="ef928-117">Binary classification: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="ef928-118">Multiklass-baserad klassificering: toopredict hello mängd tips betalat för hello resa.</span><span class="sxs-lookup"><span data-stu-id="ef928-118">Multiclass classification: toopredict hello range of tip paid for hello trip.</span></span> <span data-ttu-id="ef928-119">Vi dela hello *tips\_belopp* i fem lagerplatser eller klasser:</span><span class="sxs-lookup"><span data-stu-id="ef928-119">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="ef928-120">Regression uppgiften: toopredict hello tips betalats för en transport.</span><span class="sxs-lookup"><span data-stu-id="ef928-120">Regression task: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="ef928-121"><a name="setup"></a>Ställa in hello Azure vetenskap datamiljö för avancerade analyser</span><span class="sxs-lookup"><span data-stu-id="ef928-121"><a name="setup"></a>Setting Up hello Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="ef928-122">Som du kan se hello [planera din miljö](machine-learning-data-science-plan-your-environment.md) guide, det finns flera alternativ toowork med hello NYC Taxi resor datamängd i Azure:</span><span class="sxs-lookup"><span data-stu-id="ef928-122">As you can see from hello [Plan Your Environment](machine-learning-data-science-plan-your-environment.md) guide, there are several options toowork with hello NYC Taxi Trips dataset in Azure:</span></span>

* <span data-ttu-id="ef928-123">Arbeta med hello data i Azure BLOB sedan modellen i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ef928-123">Work with hello data in Azure blobs then model in Azure Machine Learning</span></span>
* <span data-ttu-id="ef928-124">Läs in hello data till en SQL Server-databasen och sedan modellen i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ef928-124">Load hello data into a SQL Server database then model in Azure Machine Learning</span></span>

<span data-ttu-id="ef928-125">I den här kursen visar vi parallella massimport av hello data tooa SQL Server, datagranskning, funktionen tekniker och ned provtagning med hjälp av SQL Server Management Studio som använder IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="ef928-125">In this tutorial we will demonstrate parallel bulk import of hello data tooa SQL Server, data exploration, feature engineering and down sampling using SQL Server Management Studio as well as using IPython Notebook.</span></span> <span data-ttu-id="ef928-126">[Exempel på skript](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) och [IPython anteckningsböcker](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) delas i GitHub.</span><span class="sxs-lookup"><span data-stu-id="ef928-126">[Sample scripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) and [IPython notebooks](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) are shared in GitHub.</span></span> <span data-ttu-id="ef928-127">Exempel IPython anteckningsboken toowork med hello data i Azure BLOB är också tillgängligt i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="ef928-127">A sample IPython notebook toowork with hello data in Azure blobs is also available in hello same location.</span></span>

<span data-ttu-id="ef928-128">tooset konfigurera Azure datavetenskap miljön:</span><span class="sxs-lookup"><span data-stu-id="ef928-128">tooset up your Azure Data Science environment:</span></span>

1. [<span data-ttu-id="ef928-129">Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="ef928-129">Create a storage account</span></span>](../storage/common/storage-create-storage-account.md)
2. [<span data-ttu-id="ef928-130">Skapa en arbetsyta för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ef928-130">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
3. <span data-ttu-id="ef928-131">[Etablera en virtuell dator med datavetenskap](machine-learning-data-science-setup-sql-server-virtual-machine.md), som innehåller en SQL-Server och en bärbar dator IPython-server.</span><span class="sxs-lookup"><span data-stu-id="ef928-131">[Provision a Data Science Virtual Machine](machine-learning-data-science-setup-sql-server-virtual-machine.md), which provides a SQL Server and an IPython Notebook server.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ef928-132">hello exempelskript och IPython bärbara datorer kommer att hämtade tooyour datavetenskap virtuella datorn under hello installationsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ef928-132">hello sample scripts and IPython notebooks will be downloaded tooyour Data Science virtual machine during hello setup process.</span></span> <span data-ttu-id="ef928-133">När hello VM efterinstallationsskript är klar blir hello-exempel i den Virtuella datorns dokumentbibliotek:</span><span class="sxs-lookup"><span data-stu-id="ef928-133">When hello VM post-installation script completes, hello samples will be in your VM's Documents library:</span></span>  
   > 
   > * <span data-ttu-id="ef928-134">Exempel skript:`C:\Users\<user_name>\Documents\Data Science Scripts`</span><span class="sxs-lookup"><span data-stu-id="ef928-134">Sample Scripts: `C:\Users\<user_name>\Documents\Data Science Scripts`</span></span>  
   > * <span data-ttu-id="ef928-135">Exempel IPython bärbara datorer:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span><span class="sxs-lookup"><span data-stu-id="ef928-135">Sample IPython Notebooks: `C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span></span>  
   >   <span data-ttu-id="ef928-136">där `<user_name>` är den Virtuella datorns Windows-inloggningsnamn.</span><span class="sxs-lookup"><span data-stu-id="ef928-136">where `<user_name>` is your VM's Windows login name.</span></span> <span data-ttu-id="ef928-137">Vi kommer att referera toohello exempel mappar som **exempelskript** och **exempel IPython anteckningsböcker**.</span><span class="sxs-lookup"><span data-stu-id="ef928-137">We will refer toohello sample folders as **Sample Scripts** and **Sample IPython Notebooks**.</span></span>
   > 
   > 

<span data-ttu-id="ef928-138">Utifrån hello dataset storlek, datakällplats och hello valda Azure målmiljön det här scenariot är liknande för[scenariot \#5: stora datauppsättning i lokala filer, mål SQL Server i Azure VM](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span><span class="sxs-lookup"><span data-stu-id="ef928-138">Based on hello dataset size, data source location, and hello selected Azure target environment, this scenario is similar too[Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span></span>

## <span data-ttu-id="ef928-139"><a name="getdata"></a>Hämta hello Data från offentliga källa</span><span class="sxs-lookup"><span data-stu-id="ef928-139"><a name="getdata"></a>Get hello Data from Public Source</span></span>
<span data-ttu-id="ef928-140">tooget hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset från dess offentlig plats, kan du använda någon av hello-metoder som beskrivs i [tooand flytta Data från Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ef928-140">tooget hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of hello methods described in [Move Data tooand from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour new virtual machine.</span></span>

<span data-ttu-id="ef928-141">toocopy hello data med hjälp av AzCopy:</span><span class="sxs-lookup"><span data-stu-id="ef928-141">toocopy hello data using AzCopy:</span></span>

1. <span data-ttu-id="ef928-142">Logga in tooyour virtuell dator (VM)</span><span class="sxs-lookup"><span data-stu-id="ef928-142">Log in tooyour virtual machine (VM)</span></span>
2. <span data-ttu-id="ef928-143">Skapa en ny katalog i hello VM datadisk (Obs: Använd inte hello diskutrymme som medföljer hello VM som en datadisk).</span><span class="sxs-lookup"><span data-stu-id="ef928-143">Create a new directory in hello VM's data disk (Note: Do not use hello Temporary Disk which comes with hello VM as a Data Disk).</span></span>
3. <span data-ttu-id="ef928-144">Kör hello följande kommandorad för Azcopy, ersätta < path_to_data_folder > med datamappen som skapats i (2) i ett kommandotolksfönster:</span><span class="sxs-lookup"><span data-stu-id="ef928-144">In a Command Prompt window, run hello following Azcopy command line, replacing <path_to_data_folder> with your data folder created in (2):</span></span>
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    <span data-ttu-id="ef928-145">När hello AzCopy är klar totalt av 24 zippade CSV-filer (12 för resa\_data och 12 för resa\_avgiften) måste vara i hello data-mappen.</span><span class="sxs-lookup"><span data-stu-id="ef928-145">When hello AzCopy completes, a total of 24 zipped CSV files (12 for trip\_data and 12 for trip\_fare) should be in hello data folder.</span></span>
4. <span data-ttu-id="ef928-146">Packa upp hello hämtade filer.</span><span class="sxs-lookup"><span data-stu-id="ef928-146">Unzip hello downloaded files.</span></span> <span data-ttu-id="ef928-147">Observera hello mappen där hello okomprimerade filerna finns.</span><span class="sxs-lookup"><span data-stu-id="ef928-147">Note hello folder where hello uncompressed files reside.</span></span> <span data-ttu-id="ef928-148">Den här mappen blir refererad tooas Hej < sökväg\_till\_data\_filer\>.</span><span class="sxs-lookup"><span data-stu-id="ef928-148">This folder will be referred tooas hello <path\_to\_data\_files\>.</span></span>

## <span data-ttu-id="ef928-149"><a name="dbload"></a>Massinläsning importera Data till SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="ef928-149"><a name="dbload"></a>Bulk Import Data into SQL Server Database</span></span>
<span data-ttu-id="ef928-150">hello prestanda för inläsning/överföra stora mängder data tooan SQL-databasen och efterföljande frågor kan förbättras genom att använda *partitionerade tabeller och vyer*.</span><span class="sxs-lookup"><span data-stu-id="ef928-150">hello performance of loading/transferring large amounts of data tooan SQL database and subsequent queries can be improved by using *Partitioned Tables and Views*.</span></span> <span data-ttu-id="ef928-151">I det här avsnittet kommer vi följer instruktionerna i hello [parallella Bulk Import med hjälp av SQL-Partition datatabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate en ny databas och Läs in hello data till partitionerade tabeller parallellt.</span><span class="sxs-lookup"><span data-stu-id="ef928-151">In this section, we will follow hello instructions described in [Parallel Bulk Data Import Using SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate a new database and load hello data into partitioned tables in parallel.</span></span>

1. <span data-ttu-id="ef928-152">När du är inloggad i tooyour VM starta **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="ef928-152">While logged in tooyour VM, start **SQL Server Management Studio**.</span></span>
2. <span data-ttu-id="ef928-153">Ansluta med Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="ef928-153">Connect using Windows Authentication.</span></span>
   
    ![SSMS Anslut][12]
3. <span data-ttu-id="ef928-155">Om du har ännu inte ändrats hello SQL Server-autentiseringsläget och skapa en ny SQL-inloggning, öppna hello skriptfilen som heter **ändra\_auth.sql** i hello **exempelskript** mapp.</span><span class="sxs-lookup"><span data-stu-id="ef928-155">If you have not yet changed hello SQL Server authentication mode and created a new SQL login user, open hello script file named **change\_auth.sql** in hello **Sample Scripts** folder.</span></span> <span data-ttu-id="ef928-156">Ändra hello standardanvändarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ef928-156">Change hello  default user name and password.</span></span> <span data-ttu-id="ef928-157">Klicka på **! Köra** i hello verktygsfältet toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="ef928-157">Click **!Execute** in hello toolbar toorun hello script.</span></span>
   
    ![Kör skript][13]
4. <span data-ttu-id="ef928-159">Kontrollera och/eller ändra hello SQL Server standard databasen och loggfilerna mappar tooensure som nyligen skapats databaser kommer att lagras i en datadisk.</span><span class="sxs-lookup"><span data-stu-id="ef928-159">Verify and/or change hello SQL Server default database and log folders tooensure that newly created databases will be stored in a Data Disk.</span></span> <span data-ttu-id="ef928-160">hello SQL Server-VM-avbildning som är optimerad för datawarehousing belastningar är förkonfigurerad med data och loggfilen diskar.</span><span class="sxs-lookup"><span data-stu-id="ef928-160">hello SQL Server VM image that is optimized for datawarehousing loads is pre-configured with data and log disks.</span></span> <span data-ttu-id="ef928-161">Om den virtuella datorn inte innehöll en datadisk och du har lagt till nya virtuella hårddiskar under hello VM installationsprocessen, ändra hello standardmappar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ef928-161">If your VM did not include a Data Disk and you added new virtual hard disks during hello VM setup process, change hello default folders as follows:</span></span>
   
   * <span data-ttu-id="ef928-162">Högerklicka på hello SQL Server-namnet i hello vänster panel och klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ef928-162">Right-click hello SQL Server name in hello left panel and click **Properties**.</span></span>
     
       ![Egenskaper för SQL Server][14]
   * <span data-ttu-id="ef928-164">Välj **databasinställningar** från hello **Välj en sida** listan toohello vänster.</span><span class="sxs-lookup"><span data-stu-id="ef928-164">Select **Database Settings** from hello **Select a page** list toohello left.</span></span>
   * <span data-ttu-id="ef928-165">Kontrollera och/eller ändra hello **databasen standardplatserna** toohello **datadisk** platser för ditt val.</span><span class="sxs-lookup"><span data-stu-id="ef928-165">Verify and/or change hello **Database default locations** toohello **Data Disk** locations of your choice.</span></span> <span data-ttu-id="ef928-166">Detta är där nya databaser finns om skapats med hello standardinställningarna för platsen.</span><span class="sxs-lookup"><span data-stu-id="ef928-166">This is where new databases reside if created with hello default location settings.</span></span>
     
       ![Standardvärdet för SQL-databasen][15]  
5. <span data-ttu-id="ef928-168">toocreate en ny databas och en uppsättning filgrupper toohold Hej partitionerade tabeller, öppna hello exempelskript **skapa\_db\_default.sql**.</span><span class="sxs-lookup"><span data-stu-id="ef928-168">toocreate a new database and a set of filegroups toohold hello partitioned tables, open hello sample script **create\_db\_default.sql**.</span></span> <span data-ttu-id="ef928-169">hello skriptet skapar en ny databas med namnet **TaxiNYC** och 12 filgrupper hello standardplatsen för data.</span><span class="sxs-lookup"><span data-stu-id="ef928-169">hello script will create a new database named **TaxiNYC** and 12 filegroups in hello default data location.</span></span> <span data-ttu-id="ef928-170">Varje filgrupp innehåller en månad resa\_data och resa\_färdavgiften data.</span><span class="sxs-lookup"><span data-stu-id="ef928-170">Each filegroup will hold one month of trip\_data and trip\_fare data.</span></span> <span data-ttu-id="ef928-171">Ändra hello databasens namn, om så önskas.</span><span class="sxs-lookup"><span data-stu-id="ef928-171">Modify hello database name, if desired.</span></span> <span data-ttu-id="ef928-172">Klicka på **! Köra** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="ef928-172">Click **!Execute** toorun hello script.</span></span>
6. <span data-ttu-id="ef928-173">Skapa sedan två partitionstabeller, en för hello resa\_data och en annan för hello resa\_avgiften.</span><span class="sxs-lookup"><span data-stu-id="ef928-173">Next, create two partition tables, one for hello trip\_data and another for hello trip\_fare.</span></span> <span data-ttu-id="ef928-174">Öppna hello exempelskript **skapa\_partitionerade\_table.sql**, som:</span><span class="sxs-lookup"><span data-stu-id="ef928-174">Open hello sample script **create\_partitioned\_table.sql**, which will:</span></span>
   
   * <span data-ttu-id="ef928-175">Skapa en partition funktionen toosplit hello data per månad.</span><span class="sxs-lookup"><span data-stu-id="ef928-175">Create a partition function toosplit hello data by month.</span></span>
   * <span data-ttu-id="ef928-176">Skapa en partition scheme toomap varje månad data tooa annan filgrupp.</span><span class="sxs-lookup"><span data-stu-id="ef928-176">Create a partition scheme toomap each month's data tooa different filegroup.</span></span>
   * <span data-ttu-id="ef928-177">Skapa två partitionerade tabeller mappade toohello partitionsschema: **nyctaxi\_resa** innehåller hello resa\_data och **nyctaxi\_avgiften** innehåller hello resa \_färdavgiften data.</span><span class="sxs-lookup"><span data-stu-id="ef928-177">Create two partitioned tables mapped toohello partition scheme: **nyctaxi\_trip** will hold hello trip\_data and **nyctaxi\_fare** will hold hello trip\_fare data.</span></span>
     
     <span data-ttu-id="ef928-178">Klicka på **! Köra** toorun hello skript och skapa hello partitionerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="ef928-178">Click **!Execute** toorun hello script and create hello partitioned tables.</span></span>
7. <span data-ttu-id="ef928-179">I hello **exempelskript** mapp, det finns två PowerShell-exempelskript tillhandahålls toodemonstrate parallella bulk import av data tooSQL Server-tabeller.</span><span class="sxs-lookup"><span data-stu-id="ef928-179">In hello **Sample Scripts** folder, there are two sample PowerShell scripts provided toodemonstrate parallel bulk imports of data tooSQL Server tables.</span></span>
   
   * <span data-ttu-id="ef928-180">**BCP\_parallella\_generic.ps1** är ett Allmänt skript tooparallel bulk importera data till en tabell.</span><span class="sxs-lookup"><span data-stu-id="ef928-180">**bcp\_parallel\_generic.ps1** is a generic script tooparallel bulk import data into a table.</span></span> <span data-ttu-id="ef928-181">Ändra det här skriptet tooset hello indata- och variabler som anges i hello kommentarer i hello skript.</span><span class="sxs-lookup"><span data-stu-id="ef928-181">Modify this script tooset hello input and target variables as indicated in hello comment lines in hello script.</span></span>
   * <span data-ttu-id="ef928-182">**BCP\_parallella\_nyctaxi.ps1** är en förkonfigurerad version av hello Allmänt skript och kan vara används tootooload båda tabellerna för hello NYC Taxi resor data.</span><span class="sxs-lookup"><span data-stu-id="ef928-182">**bcp\_parallel\_nyctaxi.ps1** is a pre-configured version of hello generic script and can be used tootooload both tables for hello NYC Taxi Trips data.</span></span>  
8. <span data-ttu-id="ef928-183">Högerklicka på hello **bcp\_parallella\_nyctaxi.ps1** skriptets namn och klicka på **redigera** tooopen i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ef928-183">Right-click hello **bcp\_parallel\_nyctaxi.ps1** script name and click **Edit** tooopen it in PowerShell.</span></span> <span data-ttu-id="ef928-184">Granska hello förinställningen variabler och ändra bl.a tooyour valda databasens namn, mappen inkommande data, loggen målmappen och sökvägar toohello exempelfiler format **nyctaxi_trip.xml** och **nyctaxi\_ fare.XML** (anges i hello **exempelskript** mapp).</span><span class="sxs-lookup"><span data-stu-id="ef928-184">Review hello preset variables and modify according tooyour selected database name, input data folder, target log folder, and paths toohello  sample format files **nyctaxi_trip.xml** and **nyctaxi\_fare.xml** (provided in hello **Sample Scripts** folder).</span></span>
   
    ![Massinläsning importera Data][16]
   
    <span data-ttu-id="ef928-186">Du kan också markera hello autentiseringsläge, standardvärdet är Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="ef928-186">You may also select hello authentication mode, default is Windows Authentication.</span></span> <span data-ttu-id="ef928-187">Klicka på hello grön pil i hello verktygsfältet toorun.</span><span class="sxs-lookup"><span data-stu-id="ef928-187">Click hello green arrow in hello toolbar toorun.</span></span> <span data-ttu-id="ef928-188">hello skriptet startas 24 import massåtgärder parallell 12 för varje partitionerade tabellen.</span><span class="sxs-lookup"><span data-stu-id="ef928-188">hello script will launch 24 bulk import operations in parallel, 12 for each partitioned table.</span></span> <span data-ttu-id="ef928-189">Du kan övervaka hello importera förlopp genom att öppna hello SQL Server data standardmappen som ovan.</span><span class="sxs-lookup"><span data-stu-id="ef928-189">You may monitor hello data import progress by opening hello SQL Server default data folder as set above.</span></span>
9. <span data-ttu-id="ef928-190">hello PowerShell-skript rapporter hello start- och sluttid.</span><span class="sxs-lookup"><span data-stu-id="ef928-190">hello PowerShell script reports hello starting and ending times.</span></span> <span data-ttu-id="ef928-191">När alla Massredigera importen slutförts rapporteras hello sluttid.</span><span class="sxs-lookup"><span data-stu-id="ef928-191">When all bulk imports complete, hello ending time is reported.</span></span> <span data-ttu-id="ef928-192">Kontrollera hello mål loggen mappen tooverify hello bulk import, som t.ex. inga fel rapporteras i hello målmappen för loggen.</span><span class="sxs-lookup"><span data-stu-id="ef928-192">Check hello target log folder tooverify that hello bulk imports were successful, i.e., no errors reported in hello target log folder.</span></span>
10. <span data-ttu-id="ef928-193">Databasen är nu redo för undersökning, funktionen tekniker och andra åtgärder efter behov.</span><span class="sxs-lookup"><span data-stu-id="ef928-193">Your database is now ready for exploration, feature engineering, and other operations as desired.</span></span> <span data-ttu-id="ef928-194">Eftersom hello tabeller partitioneras enligt toohello **hämtning\_datetime** fältet frågor som omfattar **hämtning\_datetime** villkor i hello  **DÄR** satsen drar nytta av hello partitionsschema.</span><span class="sxs-lookup"><span data-stu-id="ef928-194">Since hello tables are partitioned according toohello **pickup\_datetime** field, queries which include **pickup\_datetime** conditions in hello **WHERE** clause will benefit from hello partition scheme.</span></span>
11. <span data-ttu-id="ef928-195">I **SQL Server Management Studio**, utforska hello tillhandahålls exempelskript **exempel\_queries.sql**.</span><span class="sxs-lookup"><span data-stu-id="ef928-195">In **SQL Server Management Studio**, explore hello provided sample script **sample\_queries.sql**.</span></span> <span data-ttu-id="ef928-196">toorun någon hello exempelfrågor, markera hello fråga rader och klicka sedan på **! Köra** i hello-verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="ef928-196">toorun any of hello sample queries, highlight hello query lines then click **!Execute** in hello toolbar.</span></span>
12. <span data-ttu-id="ef928-197">hello NYC Taxi resor data har lästs in i två olika tabeller.</span><span class="sxs-lookup"><span data-stu-id="ef928-197">hello NYC Taxi Trips data is loaded in two separate tables.</span></span> <span data-ttu-id="ef928-198">tooimprove kopplingsåtgärder rekommenderas tooindex hello tabeller.</span><span class="sxs-lookup"><span data-stu-id="ef928-198">tooimprove join operations, it is highly recommended tooindex hello tables.</span></span> <span data-ttu-id="ef928-199">Hej exempelskript **skapa\_partitionerade\_index.sql** skapar partitionerade index i hello sammansatta koppling nyckeln **medallion hackare\_licens och hämtning\_datetime**.</span><span class="sxs-lookup"><span data-stu-id="ef928-199">hello sample script **create\_partitioned\_index.sql** creates partitioned indexes on hello composite join key **medallion, hack\_license, and pickup\_datetime**.</span></span>

## <span data-ttu-id="ef928-200"><a name="dbexplore"></a>Datagranskning och funktionen tekniker i SQLServer</span><span class="sxs-lookup"><span data-stu-id="ef928-200"><a name="dbexplore"></a>Data Exploration and Feature Engineering in SQL Server</span></span>
<span data-ttu-id="ef928-201">I det här avsnittet ska vi utföra data från kartläggning av naturresurser och funktionen generation genom att köra SQL-frågor direkt i hello **SQL Server Management Studio** använda hello SQL Server-databas som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="ef928-201">In this section, we will perform data exploration and feature generation by running SQL queries directly in hello **SQL Server Management Studio** using hello SQL Server database created earlier.</span></span> <span data-ttu-id="ef928-202">Ett exempelskript som heter **exempel\_queries.sql** har angetts i hello **exempelskript** mapp.</span><span class="sxs-lookup"><span data-stu-id="ef928-202">A sample script named **sample\_queries.sql** is provided in hello **Sample Scripts** folder.</span></span> <span data-ttu-id="ef928-203">Ändra hello skriptet toochange hello databasens namn, om det skiljer sig från standard hello: **TaxiNYC**.</span><span class="sxs-lookup"><span data-stu-id="ef928-203">Modify hello script toochange hello database name, if it is different from hello default: **TaxiNYC**.</span></span>

<span data-ttu-id="ef928-204">I den här övningen ska du:</span><span class="sxs-lookup"><span data-stu-id="ef928-204">In this exercise, we will:</span></span>

* <span data-ttu-id="ef928-205">Ansluta för**SQL Server Management Studio** med hjälp av antingen Windows-autentisering eller SQL-autentisering och hello SQL-inloggningsnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ef928-205">Connect too**SQL Server Management Studio** using either Windows Authentication or using SQL Authentication and hello SQL login name and password.</span></span>
* <span data-ttu-id="ef928-206">Utforska data distributioner av ett fåtal fält i olika tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="ef928-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="ef928-207">Undersök data quality hello longitud och latitud fält.</span><span class="sxs-lookup"><span data-stu-id="ef928-207">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="ef928-208">Generera binära och multiklass-baserad klassificeringsetiketter baserat på hello **tips\_belopp**.</span><span class="sxs-lookup"><span data-stu-id="ef928-208">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="ef928-209">Generera funktioner och beräkning eller jämförelse resa avstånd.</span><span class="sxs-lookup"><span data-stu-id="ef928-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="ef928-210">Koppla hello två tabeller och extrahera ett slumpmässigt prov som kommer att använda toobuild modeller.</span><span class="sxs-lookup"><span data-stu-id="ef928-210">Join hello two tables and extract a random sample that will be used toobuild models.</span></span>

<span data-ttu-id="ef928-211">När du är klar tooproceed tooAzure Machine Learning, kan du antingen:</span><span class="sxs-lookup"><span data-stu-id="ef928-211">When you are ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="ef928-212">Spara hello slutliga SQL tooextract och exempel hello data och kopiera / klistra in hello fråga direkt till en [importera Data] [ import-data] modul i Azure Machine Learning, eller</span><span class="sxs-lookup"><span data-stu-id="ef928-212">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="ef928-213">Kvarstår hello provtagning och bakåtkompilerade data som du planerar toouse för modellen bygga i en ny databas tabell och använda hello ny tabell i hello [importera Data] [ import-data] modul i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ef928-213">Persist hello sampled and engineered data you plan toouse for model building in a new database table and use hello new table in hello [Import Data][import-data] module in Azure Machine Learning.</span></span>

<span data-ttu-id="ef928-214">I det här avsnittet sparar vi hello slutlig tooextract och exempel hello data.</span><span class="sxs-lookup"><span data-stu-id="ef928-214">In this section we will save hello final query tooextract and sample hello data.</span></span> <span data-ttu-id="ef928-215">hello andra metoden visar hello [Datagranskning och funktionen Engineering i IPython anteckningsbok](#ipnb) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef928-215">hello second method is demonstrated in hello [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span>

<span data-ttu-id="ef928-216">För en snabb kontroll av hello antalet rader och kolumner i hello fylls tabeller tidigare med hjälp av parallella massimport</span><span class="sxs-lookup"><span data-stu-id="ef928-216">For a quick verification of hello number of rows and columns in hello tables populated earlier using parallel bulk import,</span></span>

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="ef928-217">Undersökning: Resa distribution av medallion</span><span class="sxs-lookup"><span data-stu-id="ef928-217">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="ef928-218">Det här exemplet identifierar hello medallion (taxi numbers) med mer än 100 resor inom en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="ef928-218">This example identifies hello medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="ef928-219">hello-frågan skulle dra nytta av hello partitionerad tabellåtkomst eftersom den är villkorad av hello partitionsschemat för **hämtning\_datetime**.</span><span class="sxs-lookup"><span data-stu-id="ef928-219">hello query would benefit from hello partitioned table access since it is conditioned by hello partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="ef928-220">Frågar hello fullständiga datauppsättningen blir också användning av hello partitionerad tabell och/eller index-sökning.</span><span class="sxs-lookup"><span data-stu-id="ef928-220">Querying hello full dataset will also make use of hello partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="ef928-221">Undersökning: Resa distribution av medallion och hack_license</span><span class="sxs-lookup"><span data-stu-id="ef928-221">Exploration: Trip distribution by medallion and hack_license</span></span>
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="ef928-222">Data utvärdering: Verifiera poster med felaktigt longituden eller latituden</span><span class="sxs-lookup"><span data-stu-id="ef928-222">Data Quality Assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="ef928-223">Det här exemplet undersöker om någon av hello longituden eller latituden fält antingen innehåller ett ogiltigt värde (radian grader ska vara mellan-90 och 90), eller ha (0, 0) koordinater.</span><span class="sxs-lookup"><span data-stu-id="ef928-223">This example investigates if any of hello longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="ef928-224">Undersökning: Lutad jämfört med Inte lutad resor distribution</span><span class="sxs-lookup"><span data-stu-id="ef928-224">Exploration: Tipped vs. Not Tipped Trips distribution</span></span>
<span data-ttu-id="ef928-225">Det här exemplet hittar hello antalet turer som har lutad kontra lutad inte i en given tidpunkt tidsperiod (eller i hello fullständiga datauppsättningen om täcker hello fullständig år).</span><span class="sxs-lookup"><span data-stu-id="ef928-225">This example finds hello number of trips that were tipped vs. not tipped in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="ef928-226">Den här distributionen visar hello binära etikett distribution toobe senare används för binär klassificering modellering.</span><span class="sxs-lookup"><span data-stu-id="ef928-226">This distribution reflects hello binary label distribution toobe later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="ef928-227">Undersökning: Tips klass-intervallet Distribution</span><span class="sxs-lookup"><span data-stu-id="ef928-227">Exploration: Tip Class/Range Distribution</span></span>
<span data-ttu-id="ef928-228">Det här exemplet beräknar hello distribution av tips intervall i en given tidpunkt tidsperiod (eller i hello fullständiga datauppsättningen om täcker hello fullständig år).</span><span class="sxs-lookup"><span data-stu-id="ef928-228">This example computes hello distribution of tip ranges in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="ef928-229">Detta är hello distribution av hello etikett klasser som ska användas senare för multiklass-baserad klassificering modellering.</span><span class="sxs-lookup"><span data-stu-id="ef928-229">This is hello distribution of hello label classes that will be used later for multiclass classification modeling.</span></span>

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

#### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="ef928-230">Undersökning: Beräkna och jämföra resa avstånd</span><span class="sxs-lookup"><span data-stu-id="ef928-230">Exploration: Compute and Compare Trip Distance</span></span>
<span data-ttu-id="ef928-231">Det här exemplet konverterar hello hämtning och Samlingsbibliotek longitud och latitud tooSQL geografi pekar beräknar hello resa avståndet med SQL geografi punkter skillnaden och returnerar ett slumpvist urval av hello resultat för jämförelse.</span><span class="sxs-lookup"><span data-stu-id="ef928-231">This example converts hello pickup and drop-off longitude and latitude tooSQL geography points, computes hello trip distance using SQL geography points difference, and returns a random sample of hello results for comparison.</span></span> <span data-ttu-id="ef928-232">hello exempel begränsar hello resultat toovalid samordnar bara använder hello kvalitet assessment datafrågor omfattas tidigare.</span><span class="sxs-lookup"><span data-stu-id="ef928-232">hello example limits hello results toovalid coordinates only using hello data quality assessment query covered earlier.</span></span>

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

#### <a name="feature-engineering-in-sql-queries"></a><span data-ttu-id="ef928-233">Funktionen teknikerna i SQL-frågor</span><span class="sxs-lookup"><span data-stu-id="ef928-233">Feature Engineering in SQL Queries</span></span>
<span data-ttu-id="ef928-234">hello etikett generation och geografi konvertering utforskning frågor kan också vara används toogenerate etiketter och funktioner genom att ta bort hello räknat del.</span><span class="sxs-lookup"><span data-stu-id="ef928-234">hello label generation and geography conversion exploration queries can also be used toogenerate labels/features by removing hello counting part.</span></span> <span data-ttu-id="ef928-235">Ytterligare funktionen engineering SQL-exempel finns i hello [Datagranskning och funktionen Engineering i IPython anteckningsbok](#ipnb) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef928-235">Additional feature engineering SQL examples are provided in hello [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span> <span data-ttu-id="ef928-236">Det är effektivare toorun hello funktionen generation frågor på hello fullständiga datauppsättningen eller en stor del av den med hjälp av SQL-frågor som körs direkt på hello SQL Server-databasinstansen.</span><span class="sxs-lookup"><span data-stu-id="ef928-236">It is more efficient toorun hello feature generation queries on hello full dataset or a large subset of it using SQL queries which run directly on hello SQL Server database instance.</span></span> <span data-ttu-id="ef928-237">hello frågor kan köras i **SQL Server Management Studio**, IPython bärbar dator eller alla verktyg/utvecklingsmiljö kan komma åt hello databas lokalt eller fjärranslutet.</span><span class="sxs-lookup"><span data-stu-id="ef928-237">hello queries may be executed in **SQL Server Management Studio**, IPython Notebook or any development tool/environment which can access hello database locally or remotely.</span></span>

#### <a name="preparing-data-for-model-building"></a><span data-ttu-id="ef928-238">Förbereder Data för Modellskapandet</span><span class="sxs-lookup"><span data-stu-id="ef928-238">Preparing Data for Model Building</span></span>
<span data-ttu-id="ef928-239">hello följande fråga kopplingar hello **nyctaxi\_resa** och **nyctaxi\_avgiften** tabeller, genererar en binär klassificering etikett **lutad**, flera klassen klassificering etikett **tips\_klassen**, och extraherar ett slumpmässigt prov 1% från hello fullständigt sammanfogade dataset.</span><span class="sxs-lookup"><span data-stu-id="ef928-239">hello following query joins hello **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a 1% random sample from hello full joined dataset.</span></span> <span data-ttu-id="ef928-240">Den här frågan kan kopieras och klistras in direkt i hello [Azure Machine Learning Studio](https://studio.azureml.net) [importera Data] [ import-data] -modulen för direkt datapåfyllning från hello SQL Server-databas instans i Azure.</span><span class="sxs-lookup"><span data-stu-id="ef928-240">This query can be copied then pasted directly in hello [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from hello SQL Server database instance in Azure.</span></span> <span data-ttu-id="ef928-241">hello frågan utesluter poster med fel (0, 0) koordinater.</span><span class="sxs-lookup"><span data-stu-id="ef928-241">hello query excludes records with incorrect (0, 0) coordinates.</span></span>

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


## <span data-ttu-id="ef928-242"><a name="ipnb"></a>Datagranskning och funktionen tekniker i IPython anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="ef928-242"><a name="ipnb"></a>Data Exploration and Feature Engineering in IPython Notebook</span></span>
<span data-ttu-id="ef928-243">I det här avsnittet ska vi utföra datagranskning och funktion som genereras med hjälp av Python- och SQL-frågor mot hello SQL Server-databas som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="ef928-243">In this section, we will perform data exploration and feature generation using both Python and SQL queries against hello SQL Server database created earlier.</span></span> <span data-ttu-id="ef928-244">En exempel IPython bärbar dator med namnet **machine-Learning-data-science-process-sql-story.ipynb** har angetts i hello **exempel IPython anteckningsböcker** mapp.</span><span class="sxs-lookup"><span data-stu-id="ef928-244">A sample IPython notebook named **machine-Learning-data-science-process-sql-story.ipynb** is provided in hello **Sample IPython Notebooks** folder.</span></span> <span data-ttu-id="ef928-245">Den här anteckningsboken finns också på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span><span class="sxs-lookup"><span data-stu-id="ef928-245">This notebook is also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span></span>

<span data-ttu-id="ef928-246">hello rekommenderas sekvens när du arbetar med stordata är hello följande:</span><span class="sxs-lookup"><span data-stu-id="ef928-246">hello recommended sequence when working with big data is hello following:</span></span>

* <span data-ttu-id="ef928-247">Läs i ett litet antal hello data till en ram i minnet data.</span><span class="sxs-lookup"><span data-stu-id="ef928-247">Read in a small sample of hello data into an in-memory data frame.</span></span>
* <span data-ttu-id="ef928-248">Utföra vissa visualiseringar och explorations med hello samplade data.</span><span class="sxs-lookup"><span data-stu-id="ef928-248">Perform some visualizations and explorations using hello sampled data.</span></span>
* <span data-ttu-id="ef928-249">Experiment med funktionen tekniker med hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="ef928-249">Experiment with feature engineering using hello sampled data.</span></span>
* <span data-ttu-id="ef928-250">För större datagranskning använder datamanipulering och funktionen tekniker Python tooissue SQL-frågor direkt mot hello SQL Server-databas i hello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="ef928-250">For larger data exploration, data manipulation and feature engineering, use Python tooissue SQL Queries directly against hello SQL Server database in hello Azure VM.</span></span>
* <span data-ttu-id="ef928-251">Bestäm hello exempel storlek toouse för skapande av Azure Machine Learning-modellen.</span><span class="sxs-lookup"><span data-stu-id="ef928-251">Decide hello sample size toouse for Azure Machine Learning model building.</span></span>

<span data-ttu-id="ef928-252">När klar tooproceed tooAzure Machine Learning, du kan antingen:</span><span class="sxs-lookup"><span data-stu-id="ef928-252">When ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="ef928-253">Spara hello slutliga SQL tooextract och exempel hello data och kopiera / klistra in hello fråga direkt till en [importera Data] [ import-data] modul i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ef928-253">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="ef928-254">Den här metoden visar hello [bygga modeller i Azure Machine Learning](#mlmodel) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef928-254">This method is demonstrated in hello [Building Models in Azure Machine Learning](#mlmodel) section.</span></span>    
2. <span data-ttu-id="ef928-255">Bevara hello provtagning och bakåtkompilerade data som du planerar toouse för modellen bygga i en ny databastabell, och Använd sedan hello ny tabell hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="ef928-255">Persist hello sampled and engineered data you plan toouse for model building in a new database table, then use hello new table in hello [Import Data][import-data] module.</span></span>

<span data-ttu-id="ef928-256">hello följande är några datagranskning datavisualisering och funktionen engineering exempel.</span><span class="sxs-lookup"><span data-stu-id="ef928-256">hello following are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="ef928-257">Fler exempel finns hello exempel SQL IPython anteckningsboken i hello **exempel IPython anteckningsböcker** mapp.</span><span class="sxs-lookup"><span data-stu-id="ef928-257">For more examples, see hello sample SQL IPython notebook in hello **Sample IPython Notebooks** folder.</span></span>

#### <a name="initialize-database-credentials"></a><span data-ttu-id="ef928-258">Initiera Databasautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="ef928-258">Initialize Database Credentials</span></span>
<span data-ttu-id="ef928-259">Initiera databasen anslutningsinställningarna i hello följande variabler:</span><span class="sxs-lookup"><span data-stu-id="ef928-259">Initialize your database connection settings in hello following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a><span data-ttu-id="ef928-260">Skapa databasanslutning</span><span class="sxs-lookup"><span data-stu-id="ef928-260">Create Database Connection</span></span>
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="ef928-261">Rapporten antalet rader och kolumner i tabellen nyctaxi_trip</span><span class="sxs-lookup"><span data-stu-id="ef928-261">Report number of rows and columns in table nyctaxi_trip</span></span>
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

* <span data-ttu-id="ef928-262">Totalt antal rader = 173179759</span><span class="sxs-lookup"><span data-stu-id="ef928-262">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="ef928-263">Totalt antal kolumner = 14</span><span class="sxs-lookup"><span data-stu-id="ef928-263">Total number of columns = 14</span></span>

#### <a name="read-in-a-small-data-sample-from-hello-sql-server-database"></a><span data-ttu-id="ef928-264">Läs i ett litet datasampel från hello SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="ef928-264">Read-in a small data sample from hello SQL Server Database</span></span>
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
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="ef928-265">Tid tooread hello exempeltabell är 6.492000 sekunder</span><span class="sxs-lookup"><span data-stu-id="ef928-265">Time tooread hello sample table is 6.492000 seconds</span></span>  
<span data-ttu-id="ef928-266">Antal rader och kolumner hämtas = (84952, 21)</span><span class="sxs-lookup"><span data-stu-id="ef928-266">Number of rows and columns retrieved = (84952, 21)</span></span>

#### <a name="descriptive-statistics"></a><span data-ttu-id="ef928-267">Beskrivande statistik</span><span class="sxs-lookup"><span data-stu-id="ef928-267">Descriptive Statistics</span></span>
<span data-ttu-id="ef928-268">Nu är klar tooexplore hello provtagning data.</span><span class="sxs-lookup"><span data-stu-id="ef928-268">Now are ready tooexplore hello sampled data.</span></span> <span data-ttu-id="ef928-269">Vi börjar med att titta på beskrivande statistik för hello **resa\_avstånd** (eller andra) nyckelfälten:</span><span class="sxs-lookup"><span data-stu-id="ef928-269">We start with looking at descriptive statistics for hello **trip\_distance** (or any other) field(s):</span></span>

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a><span data-ttu-id="ef928-270">Visualiseringen: Exempel på ritytans</span><span class="sxs-lookup"><span data-stu-id="ef928-270">Visualization: Box Plot Example</span></span>
<span data-ttu-id="ef928-271">Vi titta på hello Låddiagram för hello resa avståndet toovisualize hello quantiles</span><span class="sxs-lookup"><span data-stu-id="ef928-271">Next we look at hello box plot for hello trip distance toovisualize hello quantiles</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Rita #1][1]

#### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="ef928-273">Visualiseringen: Distribution ritytans exempel</span><span class="sxs-lookup"><span data-stu-id="ef928-273">Visualization: Distribution Plot Example</span></span>
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Rita #2][2]

#### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="ef928-275">Visualisering: Fältet och rad områden</span><span class="sxs-lookup"><span data-stu-id="ef928-275">Visualization: Bar and Line Plots</span></span>
<span data-ttu-id="ef928-276">I det här exemplet vi bin hello resa avståndet i fem lagerplatser och visualisera hello diskretisering resultat.</span><span class="sxs-lookup"><span data-stu-id="ef928-276">In this example, we bin hello trip distance into five bins and visualize hello binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="ef928-277">Vi kan ritas hello ovan bin distribution i ett fält eller rad ritytans enligt nedan</span><span class="sxs-lookup"><span data-stu-id="ef928-277">We can plot hello above bin distribution in a bar or line plot as below</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Rita #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Rita #4][4]

#### <a name="visualization-scatterplot-example"></a><span data-ttu-id="ef928-280">Visualiseringen: Scatterplot exempel</span><span class="sxs-lookup"><span data-stu-id="ef928-280">Visualization: Scatterplot Example</span></span>
<span data-ttu-id="ef928-281">Visar vi punktdiagram ritytans mellan **resa\_tid\_i\_sek** och **resa\_avstånd** toosee om det finns några korrelation</span><span class="sxs-lookup"><span data-stu-id="ef928-281">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** toosee if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Rita #6][6]

<span data-ttu-id="ef928-283">På liknande sätt kan vi Kontrollera hello förhållandet mellan **hastighet\_kod** och **resa\_avstånd**.</span><span class="sxs-lookup"><span data-stu-id="ef928-283">Similarly we can check hello relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Rita #8][8]

### <a name="sub-sampling-hello-data-in-sql"></a><span data-ttu-id="ef928-285">Underordnade provtagning hello Data i SQL</span><span class="sxs-lookup"><span data-stu-id="ef928-285">Sub-Sampling hello Data in SQL</span></span>
<span data-ttu-id="ef928-286">När du förbereder data för modellen bygga i [Azure Machine Learning Studio](https://studio.azureml.net), kan du antingen välja på hello **SQL-frågan toouse direkt i hello importera Data modulen** eller spara hello utformad och provtagning data i en ny tabell, som du kan använda i hello [importera Data] [ import-data] modul med en enkel **Välj * FROM < din\_nya\_tabell\_namn >**.</span><span class="sxs-lookup"><span data-stu-id="ef928-286">When preparing data for model building in [Azure Machine Learning Studio](https://studio.azureml.net), you may either decide on hello **SQL query toouse directly in hello Import Data module** or persist hello engineered and sampled data in a new table, which you could use in hello [Import Data][import-data] module with a simple **SELECT * FROM <your\_new\_table\_name>**.</span></span>

<span data-ttu-id="ef928-287">I det här avsnittet ska vi skapa en ny tabell toohold hello provtagning och utformad data.</span><span class="sxs-lookup"><span data-stu-id="ef928-287">In this section we will create a new table toohold hello sampled and engineered data.</span></span> <span data-ttu-id="ef928-288">Ett exempel på en direkt SQL-fråga för skapande av modellen som har angetts i hello [Datagranskning och funktionen Engineering i SQL Server](#dbexplore) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef928-288">An example of a direct SQL query for model building is provided in hello [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="create-a-sample-table-and-populate-with-1-of-hello-joined-tables-drop-table-first-if-it-exists"></a><span data-ttu-id="ef928-289">Skapa en exempeltabell och Fyll i med 1% av hello kopplade tabeller.</span><span class="sxs-lookup"><span data-stu-id="ef928-289">Create a Sample Table and Populate with 1% of hello Joined Tables.</span></span> <span data-ttu-id="ef928-290">Ta bort den första tabellen om den finns.</span><span class="sxs-lookup"><span data-stu-id="ef928-290">Drop Table First if it Exists.</span></span>
<span data-ttu-id="ef928-291">I det här avsnittet vi koppla hello tabeller **nyctaxi\_resa** och **nyctaxi\_avgiften**Extrahera ett slumpmässigt prov 1% och bevara hello provtagning data i ett nytt tabellnamn  **nyctaxi\_en\_procent**:</span><span class="sxs-lookup"><span data-stu-id="ef928-291">In this section, we join hello tables **nyctaxi\_trip** and **nyctaxi\_fare**, extract a 1% random sample, and persist hello sampled data in a new table name **nyctaxi\_one\_percent**:</span></span>

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

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="ef928-292">Datagranskning med hjälp av SQL-frågor i IPython anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="ef928-292">Data Exploration using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="ef928-293">I det här avsnittet förklarar vi data distributioner med hello 1% provtagning data som sparas i hello ny tabell som vi skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="ef928-293">In this section, we explore data distributions using hello 1% sampled data which is persisted in hello new table we created above.</span></span> <span data-ttu-id="ef928-294">Observera att liknande explorations kan utföras med hjälp av hello ursprungliga tabeller, vid behov kan **TABLESAMPLE** toolimit hello utforskning exempel eller genom att begränsa hello resulterar tooa angivna tidsperiod med hello **hämtning \_datetime** partitioner, enligt beskrivningen i hello [Datagranskning och funktionen Engineering i SQL Server](#dbexplore) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef928-294">Note that similar explorations can be performed using hello original tables, optionally using **TABLESAMPLE** toolimit hello exploration sample or by limiting hello results tooa given time period using hello **pickup\_datetime** partitions, as illustrated in hello [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="ef928-295">Undersökning: Dagliga distribution av resor</span><span class="sxs-lookup"><span data-stu-id="ef928-295">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="ef928-296">Undersökning: Resa distribution per medallion</span><span class="sxs-lookup"><span data-stu-id="ef928-296">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="ef928-297">Funktionen Generation med hjälp av SQL-frågor i IPython anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="ef928-297">Feature Generation Using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="ef928-298">I det här avsnittet kommer vi att generera nya etiketter och funktioner direkt med SQL-frågor, arbetar hello 1% exempeltabell vi skapade i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef928-298">In this section we will generate new labels and features directly using SQL queries, operating on hello 1% sample table we created in hello previous section.</span></span>

#### <a name="label-generation-generate-class-labels"></a><span data-ttu-id="ef928-299">Etikettgenerering: Skapa klassen etiketter</span><span class="sxs-lookup"><span data-stu-id="ef928-299">Label Generation: Generate Class Labels</span></span>
<span data-ttu-id="ef928-300">I följande exempel hello, skapar vi två uppsättningar med etiketter toouse för modellering:</span><span class="sxs-lookup"><span data-stu-id="ef928-300">In hello following example, we generate two sets of labels toouse for modeling:</span></span>

1. <span data-ttu-id="ef928-301">Binär klassen etiketter **lutad** (förutsäga om ett tips ges)</span><span class="sxs-lookup"><span data-stu-id="ef928-301">Binary Class Labels **tipped** (predicting if a tip will be given)</span></span>
2. <span data-ttu-id="ef928-302">Multiklass-baserad etiketter **tips\_klassen** (förutsäga hello tips bin eller intervall)</span><span class="sxs-lookup"><span data-stu-id="ef928-302">Multiclass Labels **tip\_class** (predicting hello tip bin or range)</span></span>
   
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

#### <a name="feature-engineering-count-features-for-categorical-columns"></a><span data-ttu-id="ef928-303">Funktionen tekniker: Antal funktioner för Kategoriska kolumner</span><span class="sxs-lookup"><span data-stu-id="ef928-303">Feature Engineering: Count Features for Categorical Columns</span></span>
<span data-ttu-id="ef928-304">Det här exemplet transformerar ett kategoriska fält i ett numeriskt fält genom att ersätta varje kategori med hello antal dess förekomster i hello data.</span><span class="sxs-lookup"><span data-stu-id="ef928-304">This example transforms a categorical field into a numeric field by replacing each category with hello count of its occurrences in hello data.</span></span>

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

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a><span data-ttu-id="ef928-305">Funktionen tekniker: Bin funktioner för numeriska kolumner</span><span class="sxs-lookup"><span data-stu-id="ef928-305">Feature Engineering: Bin features for Numerical Columns</span></span>
<span data-ttu-id="ef928-306">Det här exemplet omvandlar en kontinuerlig numeriskt fält till förinställda kategori intervall, d.v.s. transformeringen numeriskt fält i ett kategoriska fält.</span><span class="sxs-lookup"><span data-stu-id="ef928-306">This example transforms a continuous numeric field into preset category ranges, i.e., transform numeric field into a categorical field.</span></span>

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

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a><span data-ttu-id="ef928-307">Funktionen tekniker: Extrahera plats funktioner från Decimal latitud/longitud</span><span class="sxs-lookup"><span data-stu-id="ef928-307">Feature Engineering: Extract Location Features from Decimal Latitude/Longitude</span></span>
<span data-ttu-id="ef928-308">Det här exemplet uppdelad hello decimal representation av ett latitud och longitud fält till flera region fält i olika detaljnivå som, land, ort, staden, block och så vidare. Observera att hello nya geo-fält inte är mappad tooactual platser.</span><span class="sxs-lookup"><span data-stu-id="ef928-308">This example breaks down hello decimal representation of a latitude and/or longitude field into multiple region fields of different granularity, such as, country, city, town, block, etc. Note that hello new geo-fields are not mapped tooactual locations.</span></span> <span data-ttu-id="ef928-309">Information om geocode kartläggning finns [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef928-309">For information on mapping geocode locations, see [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).</span></span>

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

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a><span data-ttu-id="ef928-310">Kontrollera hello slutliga form av hello featurized tabell</span><span class="sxs-lookup"><span data-stu-id="ef928-310">Verify hello final form of hello featurized table</span></span>
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

<span data-ttu-id="ef928-311">Vi är nu redo tooproceed toomodel byggnad och distribution av modellen i [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="ef928-311">We are now ready tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="ef928-312">hello data är redo för någon av hello förutsägelse problem som konstaterats tidigare, nämligen:</span><span class="sxs-lookup"><span data-stu-id="ef928-312">hello data is ready for any of hello prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="ef928-313">Binär klassificering: toopredict huruvida ett tips har betalat för en resa.</span><span class="sxs-lookup"><span data-stu-id="ef928-313">Binary classification: toopredict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="ef928-314">Multiklass-baserad klassificering: toopredict hello mängd tips betald, enligt toohello tidigare definierade klasserna.</span><span class="sxs-lookup"><span data-stu-id="ef928-314">Multiclass classification: toopredict hello range of tip paid, according toohello previously defined classes.</span></span>
3. <span data-ttu-id="ef928-315">Regression uppgiften: toopredict hello tips betalats för en transport.</span><span class="sxs-lookup"><span data-stu-id="ef928-315">Regression task: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="ef928-316"><a name="mlmodel"></a>Skapa modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ef928-316"><a name="mlmodel"></a>Building Models in Azure Machine Learning</span></span>
<span data-ttu-id="ef928-317">toobegin Hej modeling Övning, logga in tooyour Azure Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="ef928-317">toobegin hello modeling exercise, log in tooyour Azure Machine Learning workspace.</span></span> <span data-ttu-id="ef928-318">Om du inte har skapat machine learning-arbetsytan finns [skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="ef928-318">If you have not yet created a machine learning workspace, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="ef928-319">tooget igång med Azure Machine Learning finns [vad är Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="ef928-319">tooget started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="ef928-320">Logga in för[Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="ef928-320">Log in too[Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="ef928-321">startsidan för hello Studio innehåller en mängd information, videor, självstudier, länkar toohello moduler referens och andra resurser.</span><span class="sxs-lookup"><span data-stu-id="ef928-321">hello Studio Home page provides a wealth of information, videos, tutorials, links toohello Modules Reference, and other resources.</span></span> <span data-ttu-id="ef928-322">Mer information om Azure Machine Learning finns hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="ef928-322">Fore more information about Azure Machine Learning, consult hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="ef928-323">En typisk träningsexperiment består av följande hello:</span><span class="sxs-lookup"><span data-stu-id="ef928-323">A typical training experiment consists of hello following:</span></span>

1. <span data-ttu-id="ef928-324">Skapa en **+ ny** experiment.</span><span class="sxs-lookup"><span data-stu-id="ef928-324">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="ef928-325">Hämta hello data tooAzure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ef928-325">Get hello data tooAzure Machine Learning.</span></span>
3. <span data-ttu-id="ef928-326">Förbearbeta, transformera och manipulera hello data efter behov.</span><span class="sxs-lookup"><span data-stu-id="ef928-326">Pre-process, transform and manipulate hello data as needed.</span></span>
4. <span data-ttu-id="ef928-327">Generera funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="ef928-327">Generate features as needed.</span></span>
5. <span data-ttu-id="ef928-328">Dela upp hello data i utbildning / / verifieringstesterna datauppsättningar (eller ha separata datauppsättningar för varje).</span><span class="sxs-lookup"><span data-stu-id="ef928-328">Split hello data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="ef928-329">Välj en eller flera maskininlärningsalgoritmer beroende på hello learning problemet toosolve.</span><span class="sxs-lookup"><span data-stu-id="ef928-329">Select one or more machine learning algorithms depending on hello learning problem toosolve.</span></span> <span data-ttu-id="ef928-330">T.ex. binär klassificering, multiklass-baserad klassificering, regression.</span><span class="sxs-lookup"><span data-stu-id="ef928-330">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="ef928-331">Träna en eller flera modeller som använder hello utbildning dataset.</span><span class="sxs-lookup"><span data-stu-id="ef928-331">Train one or more models using hello training dataset.</span></span>
8. <span data-ttu-id="ef928-332">Poängsätta hello validering dataset med hello tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="ef928-332">Score hello validation dataset using hello trained model(s).</span></span>
9. <span data-ttu-id="ef928-333">Utvärdera hello modeller toocompute hello relevanta mätvärden för hello learning problem.</span><span class="sxs-lookup"><span data-stu-id="ef928-333">Evaluate hello model(s) toocompute hello relevant metrics for hello learning problem.</span></span>
10. <span data-ttu-id="ef928-334">Finjustera hello modeller och välj hello bästa modellen toodeploy.</span><span class="sxs-lookup"><span data-stu-id="ef928-334">Fine tune hello model(s) and select hello best model toodeploy.</span></span>

<span data-ttu-id="ef928-335">I den här övningen har vi redan utforskade och utformad hello data i SQL Server och valt hello exempel storlek tooingest i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ef928-335">In this exercise, we have already explored and engineered hello data in SQL Server, and decided on hello sample size tooingest in Azure Machine Learning.</span></span> <span data-ttu-id="ef928-336">toobuild en eller flera av hello förutsägelse modeller vi valt:</span><span class="sxs-lookup"><span data-stu-id="ef928-336">toobuild one or more of hello prediction models we decided:</span></span>

1. <span data-ttu-id="ef928-337">Hämta hello data tooAzure Machine Learning med hello [importera Data] [ import-data] modulen är tillgängliga i hello **Data ingående och utgående** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef928-337">Get hello data tooAzure Machine Learning using hello [Import Data][import-data] module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="ef928-338">Mer information finns i hello [importera Data] [ import-data] modulsida referens.</span><span class="sxs-lookup"><span data-stu-id="ef928-338">For more information, see hello [Import Data][import-data] module reference page.</span></span>
   
    ![Azure Machine Learning importera Data][17]
2. <span data-ttu-id="ef928-340">Välj **Azure SQL Database** som hello **datakällan** i hello **egenskaper** panelen.</span><span class="sxs-lookup"><span data-stu-id="ef928-340">Select **Azure SQL Database** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="ef928-341">Ange hello DNS-namn i hello **Databasservernamnet** fältet.</span><span class="sxs-lookup"><span data-stu-id="ef928-341">Enter hello database DNS name in hello **Database server name** field.</span></span> <span data-ttu-id="ef928-342">Format:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="ef928-342">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="ef928-343">Ange hello **databasnamnet** i hello motsvarande fält.</span><span class="sxs-lookup"><span data-stu-id="ef928-343">Enter hello **Database name** in hello corresponding field.</span></span>
5. <span data-ttu-id="ef928-344">Ange hello **användarnamn för SQL** i hello ** aqccount användarnamn för Server och hello lösenord i hello **serverlösenord**.</span><span class="sxs-lookup"><span data-stu-id="ef928-344">Enter hello **SQL user name** in hello **Server user aqccount name, and hello password in hello **Server user account password**.</span></span>
6. <span data-ttu-id="ef928-345">Kontrollera **acceptera alla servercertifikat** alternativet.</span><span class="sxs-lookup"><span data-stu-id="ef928-345">Check **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="ef928-346">I hello **databasfrågan** redigera textområde, klistra in vilka extrakt hello nödvändigt databasfält (inklusive eventuella beräknade fält, till exempel hello etiketter) och ned exempel hello data toohello önskad provtagning hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="ef928-346">In hello **Database query** edit text area, paste hello query which extracts hello necessary database fields (including any computed fields such as hello labels) and down samples hello data toohello desired sample size.</span></span>

<span data-ttu-id="ef928-347">Ett exempel på en binär klassificering experiment läsning av data direkt från hello SQL Server-databasen är i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="ef928-347">An example of a binary classification experiment reading data directly from hello SQL Server database is in hello figure below.</span></span> <span data-ttu-id="ef928-348">Liknande experiment kan konstrueras för multiklass-baserad klassificering och regressionsproblem.</span><span class="sxs-lookup"><span data-stu-id="ef928-348">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Azure Machine Learning Train][10]

> [!IMPORTANT]
> <span data-ttu-id="ef928-350">I hello modellering extrahering av data och provtagning frågan exempel finns i föregående avsnitt **alla etiketter för hello tre modellering övningarna tas med i hello fråga**.</span><span class="sxs-lookup"><span data-stu-id="ef928-350">In hello modeling data extraction and sampling query examples provided in previous sections, **all labels for hello three modeling exercises are included in hello query**.</span></span> <span data-ttu-id="ef928-351">Är ett viktigt steg i (obligatoriskt) i varje hello modeling övningarna för**undanta** hello onödiga etiketter för hello andra två problem och andra **mål minnesläckor**.</span><span class="sxs-lookup"><span data-stu-id="ef928-351">An important (required) step in each of hello modeling exercises is too**exclude** hello unnecessary labels for hello other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="ef928-352">För t.ex. Använd när du använder binär klassificering hello etikett **lutad** och utelämna hello fält **tips\_klassen**, **tips\_belopp**, och **totala\_belopp**.</span><span class="sxs-lookup"><span data-stu-id="ef928-352">For e.g., when using binary classification, use hello label **tipped** and exclude hello fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="ef928-353">hello senare är målet minnesläckor eftersom de innebär hello tips betalda.</span><span class="sxs-lookup"><span data-stu-id="ef928-353">hello latter are target leaks since they imply hello tip paid.</span></span>
> 
> <span data-ttu-id="ef928-354">tooexclude onödiga kolumner och/eller målet minnesläckor, kan du använda hello [Välj kolumner i datauppsättning] [ select-columns] modul eller hello [redigera Metadata] [ edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="ef928-354">tooexclude unnecessary columns and/or target leaks, you may use hello [Select Columns in Dataset][select-columns] module or hello [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="ef928-355">Mer information finns i [Välj kolumner i datauppsättning] [ select-columns] och [redigera Metadata] [ edit-metadata] referera sidor.</span><span class="sxs-lookup"><span data-stu-id="ef928-355">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="ef928-356"><a name="mldeploy"></a>Distribuera modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ef928-356"><a name="mldeploy"></a>Deploying Models in Azure Machine Learning</span></span>
<span data-ttu-id="ef928-357">När modellen är klar, kan du enkelt distribuera det som en webbtjänst direkt från hello experiment.</span><span class="sxs-lookup"><span data-stu-id="ef928-357">When your model is ready, you can easily deploy it as a web service directly from hello experiment.</span></span> <span data-ttu-id="ef928-358">Mer information om hur du distribuerar Azure Machine Learning-webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="ef928-358">For more information about deploying Azure Machine Learning web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="ef928-359">toodeploy en ny webbtjänst, måste du:</span><span class="sxs-lookup"><span data-stu-id="ef928-359">toodeploy a new web service, you need to:</span></span>

1. <span data-ttu-id="ef928-360">Skapa ett bedömningsprofil experiment.</span><span class="sxs-lookup"><span data-stu-id="ef928-360">Create a scoring experiment.</span></span>
2. <span data-ttu-id="ef928-361">Distribuera hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef928-361">Deploy hello web service.</span></span>

<span data-ttu-id="ef928-362">toocreate poängsättning av ett experiment från en **avslutad** utbildning experiment, klickar du på **skapa BEDÖMNINGEN EXPERIMENTERA** i hello lägre Åtgärdsfältet.</span><span class="sxs-lookup"><span data-stu-id="ef928-362">toocreate a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in hello lower action bar.</span></span>

![Azure bedömningen][18]

<span data-ttu-id="ef928-364">Azure Machine Learning försöker toocreate ett bedömningsprofil experiment baserat på hello komponenter i hello träningsexperiment.</span><span class="sxs-lookup"><span data-stu-id="ef928-364">Azure Machine Learning will attempt toocreate a scoring experiment based on hello components of hello training experiment.</span></span> <span data-ttu-id="ef928-365">I synnerhet att:</span><span class="sxs-lookup"><span data-stu-id="ef928-365">In particular, it will:</span></span>

1. <span data-ttu-id="ef928-366">Spara hello tränad modell och ta bort hello modellen utbildningsmoduler.</span><span class="sxs-lookup"><span data-stu-id="ef928-366">Save hello trained model and remove hello model training modules.</span></span>
2. <span data-ttu-id="ef928-367">Identifiera en logisk **inkommande port** toorepresent hello förväntades indata schemat.</span><span class="sxs-lookup"><span data-stu-id="ef928-367">Identify a logical **input port** toorepresent hello expected input data schema.</span></span>
3. <span data-ttu-id="ef928-368">Identifiera en logisk **utgående port** toorepresent hello förväntade web service utdataschema.</span><span class="sxs-lookup"><span data-stu-id="ef928-368">Identify a logical **output port** toorepresent hello expected web service output schema.</span></span>

<span data-ttu-id="ef928-369">När hello bedömningen experiment skapas, granska den och justera efter behov.</span><span class="sxs-lookup"><span data-stu-id="ef928-369">When hello scoring experiment is created, review it and adjust as needed.</span></span> <span data-ttu-id="ef928-370">En typisk justering är tooreplace hello inkommande dataset och/eller fråga med någon vilket utesluter etikett fält, eftersom dessa inte är tillgänglig när tjänsten hello kallas.</span><span class="sxs-lookup"><span data-stu-id="ef928-370">A typical adjustment is tooreplace hello input dataset and/or query with one which excludes label fields, as these will not be available when hello service is called.</span></span> <span data-ttu-id="ef928-371">Det är också en bra idé tooreduce hello storlek på hello indata dataset-och/eller tooa några poster, bara tillräckligt med tooindicate hello ingående schema.</span><span class="sxs-lookup"><span data-stu-id="ef928-371">It is also a good practice tooreduce hello size of hello input dataset and/or query tooa few records, just enough tooindicate hello input schema.</span></span> <span data-ttu-id="ef928-372">För hello utdataporten den gemensamma tooexclude alla inmatningsfält och bara innehålla hello **poängsatta etiketter** och **bedömas sannolikhet** i hello utdata med hello [Välj kolumner i datauppsättning ] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="ef928-372">For hello output port, it is common tooexclude all input fields and only include hello **Scored Labels** and **Scored Probabilities** in hello output using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="ef928-373">Ett exempel på en bedömningen experiment har hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="ef928-373">A sample scoring experiment is in hello figure below.</span></span> <span data-ttu-id="ef928-374">När klar toodeploy, klicka på hello **publicera WEBBTJÄNSTEN** knapp i hello lägre Åtgärdsfältet.</span><span class="sxs-lookup"><span data-stu-id="ef928-374">When ready toodeploy, click hello **PUBLISH WEB SERVICE** button in hello lower action bar.</span></span>

![Publicera Azure Machine Learning][11]

<span data-ttu-id="ef928-376">toorecap, i den här genomgången kursen har du skapat ett Azure datavetenskap miljö, arbetat med en stor offentliga datauppsättning alla hello sätt från data förvärv toomodel träning och distributionen av en Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ef928-376">toorecap, in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset all hello way from data acquisition toomodel training and deploying of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="ef928-377">Licensinformationen</span><span class="sxs-lookup"><span data-stu-id="ef928-377">License Information</span></span>
<span data-ttu-id="ef928-378">Den här genomgången exempel och dess tillhörande skript och IPython notebook(s) delas av Microsoft under hello MIT-licens.</span><span class="sxs-lookup"><span data-stu-id="ef928-378">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="ef928-379">Kontrollera filen LICENSE.txt för hello i hello directory hello exempelkoden på GitHub för mer information.</span><span class="sxs-lookup"><span data-stu-id="ef928-379">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

### <a name="references"></a><span data-ttu-id="ef928-380">Referenser</span><span class="sxs-lookup"><span data-stu-id="ef928-380">References</span></span>
<span data-ttu-id="ef928-381">• [Andrés Monroy NYC Taxi resor hämtningssidan](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="ef928-381">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="ef928-382">• [FOILing NYC Taxitransport resa Data av Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="ef928-382">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="ef928-383">• [NYC Taxi och Limousine kommissionens forskning och statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="ef928-383">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

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
