---
title: "hello Team av vetenskapliga data i praktiken: använda SQL Data Warehouse | Microsoft Docs"
description: "Processen för avancerade analyser och teknik i åtgärd"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 88ba8e28-0bd7-49fe-8320-5dfa83b65724
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;hangzh;weig
ms.openlocfilehash: b1b6371583a023d32e33db59464cafd8c3b767d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-data-warehouse"></a><span data-ttu-id="c80a0-103">hello Team av vetenskapliga data i praktiken: använda SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c80a0-103">hello Team Data Science Process in action: using SQL Data Warehouse</span></span>
<span data-ttu-id="c80a0-104">I den här självstudiekursen vi beskriver hur du skapar och distribuerar en maskininlärningsmodell med hjälp av SQL Data Warehouse (SQL DW) för en offentligt tillgängliga dataset--hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset.</span><span class="sxs-lookup"><span data-stu-id="c80a0-104">In this tutorial, we walk you through building and deploying a machine learning model using SQL Data Warehouse (SQL DW) for a publicly available dataset -- hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="c80a0-105">hello binär klassificering modellen konstrueras beräknar huruvida ett tips betalat för en resa och modeller för multiklass-baserad klassificering och regression beskrivs också som förutsäga hello distribution för hello tips belopp betald.</span><span class="sxs-lookup"><span data-stu-id="c80a0-105">hello binary classification model constructed predicts whether or not a tip is paid for a trip, and models for multiclass classification and regression are also discussed that predict hello distribution for hello tip amounts paid.</span></span>

<span data-ttu-id="c80a0-106">hello proceduren följer hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="c80a0-106">hello procedure follows hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) workflow.</span></span> <span data-ttu-id="c80a0-107">Visar vi hur toosetup vetenskap datamiljö, hur tooload hello data till SQL DW och hur använda SQL DW eller en bärbar dator IPython tooexplore hello data och tekniker funktioner toomodel.</span><span class="sxs-lookup"><span data-stu-id="c80a0-107">We show how toosetup a data science environment, how tooload hello data into SQL DW, and how use either SQL DW or an IPython Notebook tooexplore hello data and engineer features toomodel.</span></span> <span data-ttu-id="c80a0-108">Sedan visar vi hur toobuild och distribuera en modell med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c80a0-108">We then show how toobuild and deploy a model with Azure Machine Learning.</span></span>

## <span data-ttu-id="c80a0-109"><a name="dataset"></a>hello NYC Taxi resor dataset</span><span class="sxs-lookup"><span data-stu-id="c80a0-109"><a name="dataset"></a>hello NYC Taxi Trips dataset</span></span>
<span data-ttu-id="c80a0-110">hello NYC Taxi resa data består av cirka 20GB komprimerat CSV-filer (~ 48GB okomprimerade), registrera mer än 173 miljoner enskilda resor och hello priser för varje resa.</span><span class="sxs-lookup"><span data-stu-id="c80a0-110">hello NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="c80a0-111">Varje resa post omfattar hello hämtning och Samlingsbibliotek platser och gånger anonym hacka () körkortsnummer och hello medallion (taxi's unikt id) nummer.</span><span class="sxs-lookup"><span data-stu-id="c80a0-111">Each trip record includes hello pickup and drop-off locations and times, anonymized hack (driver's) license number, and hello medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="c80a0-112">hello data omfattar alla resor hello år 2013 och finns i följande två datamängder för varje månad hello:</span><span class="sxs-lookup"><span data-stu-id="c80a0-112">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="c80a0-113">Hej **trip_data.csv** filen innehåller resa information, till exempel antal passagerare, hämtning och dropoff, resa varaktighet och resa längd.</span><span class="sxs-lookup"><span data-stu-id="c80a0-113">hello **trip_data.csv** file contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="c80a0-114">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="c80a0-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="c80a0-115">Hej **trip_fare.csv** filen innehåller information om hello avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar och hello totalbelopp betald.</span><span class="sxs-lookup"><span data-stu-id="c80a0-115">hello **trip_fare.csv** file contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="c80a0-116">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="c80a0-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="c80a0-117">Hej **Unik nyckel** används toojoin resa\_data och resa\_avgiften består av följande tre fält hello:</span><span class="sxs-lookup"><span data-stu-id="c80a0-117">hello **unique key** used toojoin trip\_data and trip\_fare is composed of hello following three fields:</span></span>

* <span data-ttu-id="c80a0-118">medallion,</span><span class="sxs-lookup"><span data-stu-id="c80a0-118">medallion,</span></span>
* <span data-ttu-id="c80a0-119">hacka\_licens och</span><span class="sxs-lookup"><span data-stu-id="c80a0-119">hack\_license and</span></span>
* <span data-ttu-id="c80a0-120">hämtning\_datetime.</span><span class="sxs-lookup"><span data-stu-id="c80a0-120">pickup\_datetime.</span></span>

## <span data-ttu-id="c80a0-121"><a name="mltasks"></a>Åtgärda tre typer av förutsägelse uppgifter</span><span class="sxs-lookup"><span data-stu-id="c80a0-121"><a name="mltasks"></a>Address three types of prediction tasks</span></span>
<span data-ttu-id="c80a0-122">Vi formulera tre förutsägelse problem utifrån hello *tips\_belopp* tooillustrate tre typer av modeling uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c80a0-122">We formulate three prediction problems based on hello *tip\_amount* tooillustrate three kinds of modeling tasks:</span></span>

1. <span data-ttu-id="c80a0-123">**Binär klassificering**: toopredict om huruvida ett tips har betalat för en resa, d.v.s. en *tips\_belopp* som är större än 0 är ett positivt exempel, när en *tips\_belopp* $0 är ett exempel på negativt.</span><span class="sxs-lookup"><span data-stu-id="c80a0-123">**Binary classification**: toopredict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="c80a0-124">**Multiklass-baserad klassificering**: toopredict hello mängd tips betalat för hello resa.</span><span class="sxs-lookup"><span data-stu-id="c80a0-124">**Multiclass classification**: toopredict hello range of tip paid for hello trip.</span></span> <span data-ttu-id="c80a0-125">Vi dela hello *tips\_belopp* i fem lagerplatser eller klasser:</span><span class="sxs-lookup"><span data-stu-id="c80a0-125">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="c80a0-126">**Regression uppgiften**: toopredict hello tips betalats för en transport.</span><span class="sxs-lookup"><span data-stu-id="c80a0-126">**Regression task**: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="c80a0-127"><a name="setup"></a>Konfigurera hello Azure datavetenskap miljö för avancerade analyser</span><span class="sxs-lookup"><span data-stu-id="c80a0-127"><a name="setup"></a>Set up hello Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="c80a0-128">tooset konfigurera Azure datavetenskap miljön, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="c80a0-128">tooset up your Azure Data Science environment, follow these steps.</span></span>

<span data-ttu-id="c80a0-129">**Skapa din egen Azure blob storage-konto**</span><span class="sxs-lookup"><span data-stu-id="c80a0-129">**Create your own Azure blob storage account**</span></span>

* <span data-ttu-id="c80a0-130">När du konfigurerar din egen Azure-blobblagring Välj geoplats för din Azure-blobblagring i eller så mycket som möjligt för**södra centrala USA**, vilket är där hello NYC Taxi data lagras.</span><span class="sxs-lookup"><span data-stu-id="c80a0-130">When you provision your own Azure blob storage, choose a geo-location for your Azure blob storage in or as close as possible too**South Central US**, which is where hello NYC Taxi data is stored.</span></span> <span data-ttu-id="c80a0-131">hello data kopieras med hjälp av AzCopy från hello offentliga behållare tooa blobblagringsbehållare i ditt eget lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c80a0-131">hello data will be copied using AzCopy from hello public blob storage container tooa container in your own storage account.</span></span> <span data-ttu-id="c80a0-132">hello närmare din Azure blob storage är tooSouth centrala USA, hello snabbare (steg 4) kommer att slutföra uppgiften.</span><span class="sxs-lookup"><span data-stu-id="c80a0-132">hello closer your Azure blob storage is tooSouth Central US, hello faster this task (Step 4) will be completed.</span></span>
* <span data-ttu-id="c80a0-133">toocreate din egen Azure storage-konto, följ hello stegen som beskrivs i [om Azure storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="c80a0-133">toocreate your own Azure storage account, follow hello steps outlined at [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="c80a0-134">Vara säker på att toomake not hello värden för följande lagringskontouppgifter som de behövs längre fram i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="c80a0-134">Be sure toomake notes on hello values for following storage account credentials as they will be needed later in this walkthrough.</span></span>
  
  * <span data-ttu-id="c80a0-135">**Lagringskontonamnet**</span><span class="sxs-lookup"><span data-stu-id="c80a0-135">**Storage Account Name**</span></span>
  * <span data-ttu-id="c80a0-136">**Lagringskontonyckel**</span><span class="sxs-lookup"><span data-stu-id="c80a0-136">**Storage Account Key**</span></span>
  * <span data-ttu-id="c80a0-137">**Behållarens namn** (som du vill hello data toobe lagras i hello Azure blob storage)</span><span class="sxs-lookup"><span data-stu-id="c80a0-137">**Container Name** (which you want hello data toobe stored in hello Azure blob storage)</span></span>

<span data-ttu-id="c80a0-138">**Etablera din Azure SQL DW-instans.**</span><span class="sxs-lookup"><span data-stu-id="c80a0-138">**Provision your Azure SQL DW instance.**</span></span>
<span data-ttu-id="c80a0-139">Följ hello dokumentation på [skapa ett SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision en instans av SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c80a0-139">Follow hello documentation at [Create a SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision a SQL Data Warehouse instance.</span></span> <span data-ttu-id="c80a0-140">Kontrollera att du gör här ska på hello följande SQL Data Warehouse-autentiseringsuppgifter som ska användas i senare steg.</span><span class="sxs-lookup"><span data-stu-id="c80a0-140">Make sure that you make notations on hello following SQL Data Warehouse credentials which will be used in later steps.</span></span>

* <span data-ttu-id="c80a0-141">**Servernamnet**: <server Name>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="c80a0-141">**Server Name**: <server Name>.database.windows.net</span></span>
* <span data-ttu-id="c80a0-142">**Namn på SQLDW (Database)**</span><span class="sxs-lookup"><span data-stu-id="c80a0-142">**SQLDW (Database) Name**</span></span>
* <span data-ttu-id="c80a0-143">**Användarnamn**</span><span class="sxs-lookup"><span data-stu-id="c80a0-143">**Username**</span></span>
* <span data-ttu-id="c80a0-144">**Lösenord**</span><span class="sxs-lookup"><span data-stu-id="c80a0-144">**Password**</span></span>

<span data-ttu-id="c80a0-145">**Installera Visual Studio och SQL Server Data Tools.**</span><span class="sxs-lookup"><span data-stu-id="c80a0-145">**Install Visual Studio and SQL Server Data Tools.**</span></span> <span data-ttu-id="c80a0-146">Instruktioner finns i [installera Visual Studio 2015 och SSDT (SQL Server Data Tools) för SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="c80a0-146">For instructions, see [Install Visual Studio 2015 and/or SSDT (SQL Server Data Tools) for SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span></span>

<span data-ttu-id="c80a0-147">**Ansluta tooyour Azure SQL DW med Visual Studio.**</span><span class="sxs-lookup"><span data-stu-id="c80a0-147">**Connect tooyour Azure SQL DW with Visual Studio.**</span></span> <span data-ttu-id="c80a0-148">Instruktioner finns i steg 1 och 2 i [ansluta tooAzure SQL Data Warehouse med Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c80a0-148">For instructions, see steps 1 & 2 in [Connect tooAzure SQL Data Warehouse with Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c80a0-149">Kör hello följande SQL-frågan på hello databasen du skapade i ditt SQL Data Warehouse (i stället för hello frågan som angavs i steg 3 i hello ansluta avsnittet) för**skapa en huvudnyckel**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-149">Run hello following SQL query on hello database you created in your SQL Data Warehouse (instead of hello query provided in step 3 of hello connect topic,) too**create a master key**.</span></span>
> 
> 

    BEGIN TRY
           --Try toocreate hello master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If hello master key exists, do nothing
    END CATCH;

<span data-ttu-id="c80a0-150">**Skapa en Azure Machine Learning-arbetsytan under din Azure-prenumeration.**</span><span class="sxs-lookup"><span data-stu-id="c80a0-150">**Create an Azure Machine Learning workspace under your Azure subscription.**</span></span> <span data-ttu-id="c80a0-151">Instruktioner finns i [skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="c80a0-151">For instructions, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

## <span data-ttu-id="c80a0-152"><a name="getdata"></a>Läs in hello data till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c80a0-152"><a name="getdata"></a>Load hello data into SQL Data Warehouse</span></span>
<span data-ttu-id="c80a0-153">Öppna en Windows PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="c80a0-153">Open a Windows PowerShell command console.</span></span> <span data-ttu-id="c80a0-154">Kör hello följande PowerShell-kommandon till toodownload hello exempel SQL skriptfiler som vi dela med dig på GitHub tooa lokal katalog som du anger med hello parametern *- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="c80a0-154">Run hello following PowerShell commands toodownload hello example SQL script files that we share with you on GitHub tooa local directory that you specify with hello parameter *-DestDir*.</span></span> <span data-ttu-id="c80a0-155">Du kan ändra hello värdet för parametern *- DestDir* tooany lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="c80a0-155">You can change hello value of parameter *-DestDir* tooany local directory.</span></span> <span data-ttu-id="c80a0-156">Om *- DestDir* finns inte, kommer att skapas av hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="c80a0-156">If *-DestDir* does not exist, it will be created by hello PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="c80a0-157">Du kan behöva för**kör som administratör** när du kör följande PowerShell-skript om hello din *DestDir* directory behöver administratören behörighet toocreate eller toowrite tooit.</span><span class="sxs-lookup"><span data-stu-id="c80a0-157">You might need too**Run as Administrator** when executing hello following PowerShell script if your *DestDir* directory needs Administrator privilege toocreate or toowrite tooit.</span></span>
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

<span data-ttu-id="c80a0-158">När du har körts, ändras den aktuella arbetskatalogen för*- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="c80a0-158">After successful execution, your current working directory changes too*-DestDir*.</span></span> <span data-ttu-id="c80a0-159">Du bör kunna toosee skärmen nedan:</span><span class="sxs-lookup"><span data-stu-id="c80a0-159">You should be able toosee screen like below:</span></span>

![][19]

<span data-ttu-id="c80a0-160">I din *- DestDir*, köra följande PowerShell-skript i administratörsläge hello:</span><span class="sxs-lookup"><span data-stu-id="c80a0-160">In your *-DestDir*, execute hello following PowerShell script in administrator mode:</span></span>

    ./SQLDW_Data_Import.ps1

<span data-ttu-id="c80a0-161">När hello PowerShell-skript körs för hello första gången blir du ombedd tooinput hello information från din Azure SQL DW och Azure blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c80a0-161">When hello PowerShell script runs for hello first time, you will be asked tooinput hello information from your Azure SQL DW and your Azure blob storage account.</span></span> <span data-ttu-id="c80a0-162">När det här PowerShell-skriptet har slutförts kommer körs första gången, hello autentiseringsuppgifter för hello du indata har skrivits tooa konfigurationsfilen SQLDW.conf i hello finns arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="c80a0-162">When this PowerShell script completes running for hello first time, hello credentials you input will have been written tooa configuration file SQLDW.conf in hello present working directory.</span></span> <span data-ttu-id="c80a0-163">hello har framtida körning av PowerShell-skriptfil hello alternativet tooread alla nödvändiga parametrar från konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="c80a0-163">hello future run of this PowerShell script file has hello option tooread all needed parameters from this configuration file.</span></span> <span data-ttu-id="c80a0-164">Om du behöver toochange vissa parametrar, kan du välja tooinput hello parametrar på hello-skärmen vid uppmaning om att ta bort den här konfigurationsfilen och mata in hello parametervärdena som efterfrågas eller toochange hello parametervärden genom att redigera hello SQLDW.conf filen i din *- DestDir* directory.</span><span class="sxs-lookup"><span data-stu-id="c80a0-164">If you need toochange some parameters, you can choose tooinput hello parameters on hello screen upon prompt by deleting this configuration file and inputting hello parameters values as prompted or toochange hello parameter values by editing hello SQLDW.conf file in your *-DestDir* directory.</span></span>

> [!NOTE]
> <span data-ttu-id="c80a0-165">I ordning tooavoid schema namnet står i konflikt med de som redan finns i din Azure SQL DW vid läsning av parametrar direkt från hello SQLDW.conf fil, läggs ett 3-siffriga slumptal toohello schemanamnet från hello SQLDW.conf fil som hello schemat för standardnamnet för varje körning.</span><span class="sxs-lookup"><span data-stu-id="c80a0-165">In order tooavoid schema name conflicts with those that already exist in your Azure SQL DW, when reading parameters directly from hello SQLDW.conf file, a 3-digit random number is added toohello schema name from hello SQLDW.conf file as hello default schema name for each run.</span></span> <span data-ttu-id="c80a0-166">hello PowerShell-skript kan efterfrågas ett schemanamn: hello namn kan anges efter användaren gottfinnande.</span><span class="sxs-lookup"><span data-stu-id="c80a0-166">hello PowerShell script may prompt you for a schema name: hello name may be specified at user discretion.</span></span>
> 
> 

<span data-ttu-id="c80a0-167">Detta **PowerShell-skript** filen Slutför hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c80a0-167">This **PowerShell script** file completes hello following tasks:</span></span>

* <span data-ttu-id="c80a0-168">**Hämtar och installerar AzCopy**om AzCopy inte redan är installerad</span><span class="sxs-lookup"><span data-stu-id="c80a0-168">**Downloads and installs AzCopy**, if AzCopy is not already installed</span></span>
  
        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
               Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }
* <span data-ttu-id="c80a0-169">**Kopierar data tooyour privata blob storage-konto** från hello offentlig blob med AzCopy</span><span class="sxs-lookup"><span data-stu-id="c80a0-169">**Copies data tooyour private blob storage account** from hello public blob with AzCopy</span></span>
  
        Write-Host "AzCopy is copying data from public blob tooyo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account tooverify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob tooyour storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* <span data-ttu-id="c80a0-170">**Läser in data med Polybase (genom att köra LoadDataToSQLDW.sql) tooyour Azure SQL DW** från privata blob storage-konto med hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="c80a0-170">**Loads data using Polybase (by executing LoadDataToSQLDW.sql) tooyour Azure SQL DW** from your private blob storage account with hello following commands.</span></span>
  
  * <span data-ttu-id="c80a0-171">Skapa ett schema</span><span class="sxs-lookup"><span data-stu-id="c80a0-171">Create a schema</span></span>
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * <span data-ttu-id="c80a0-172">Skapa en databasbegränsade autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="c80a0-172">Create a database scoped credential</span></span>
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * <span data-ttu-id="c80a0-173">Skapa en extern datakälla för ett Azure storage-blob</span><span class="sxs-lookup"><span data-stu-id="c80a0-173">Create an external data source for an Azure storage blob</span></span>
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
  * <span data-ttu-id="c80a0-174">Skapa ett externt filformat för en csv-fil.</span><span class="sxs-lookup"><span data-stu-id="c80a0-174">Create an external file format for a csv file.</span></span> <span data-ttu-id="c80a0-175">Fälten avgränsas med hello vertikalstrecket att data är okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="c80a0-175">Data is uncompressed and fields are separated with hello pipe character.</span></span>
    
          CREATE EXTERNAL FILE FORMAT {csv_file_format}
          WITH
          (   
              FORMAT_TYPE = DELIMITEDTEXT,
              FORMAT_OPTIONS  
              (
                  FIELD_TERMINATOR ='','',
                  USE_TYPE_DEFAULT = TRUE
              )
          )
          ;
  * <span data-ttu-id="c80a0-176">Skapa extern avgiften och resa tabeller för NYC taxi dataset i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c80a0-176">Create external fare and trip tables for NYC taxi dataset in Azure blob storage.</span></span>
    
          CREATE EXTERNAL TABLE {external_nyctaxi_fare}
          (
              medallion varchar(50) not null,
              hack_license varchar(50) not null,
              vendor_id char(3),
              pickup_datetime datetime not null,
              payment_type char(3),
              fare_amount float,
              surcharge float,
              mta_tax float,
              tip_amount float,
              tolls_amount float,
              total_amount float
          )
          with (
              LOCATION    = ''/nyctaxifare/'',
              DATA_SOURCE = {nyctaxi_fare_storage},
              FILE_FORMAT = {csv_file_format},
              REJECT_TYPE = VALUE,
              REJECT_VALUE = 12     
          )  

            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                   medallion varchar(50) not null,
                   hack_license varchar(50)  not null,
                   vendor_id char(3),
                   rate_code char(3),
                   store_and_fwd_flag char(3),
                   pickup_datetime datetime  not null,
                   dropoff_datetime datetime,
                   passenger_count int,
                   trip_time_in_secs bigint,
                   trip_distance float,
                   pickup_longitude varchar(30),
                   pickup_latitude varchar(30),
                   dropoff_longitude varchar(30),
                   dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - <span data-ttu-id="c80a0-177">Läs in data från externa tabeller i Azure blob storage tooSQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c80a0-177">Load data from external tables in Azure blob storage tooSQL Data Warehouse</span></span>

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - <span data-ttu-id="c80a0-178">Skapa data exempeltabell (NYCTaxi_Sample) och infoga data tooit från att välja SQL-frågor för hello resa och avgiften tabeller.</span><span class="sxs-lookup"><span data-stu-id="c80a0-178">Create a sample data table (NYCTaxi_Sample) and insert data tooit from selecting SQL queries on hello trip and fare tables.</span></span> <span data-ttu-id="c80a0-179">(Vissa steg i den här genomgången måste toouse denna exempeltabell.)</span><span class="sxs-lookup"><span data-stu-id="c80a0-179">(Some steps of this walkthrough needs toouse this sample table.)</span></span>

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

<span data-ttu-id="c80a0-180">hello geografisk plats för dina lagringskonton påverkar inläsningstiden.</span><span class="sxs-lookup"><span data-stu-id="c80a0-180">hello geographic location of your storage accounts affects load times.</span></span>

> [!NOTE]
> <span data-ttu-id="c80a0-181">Beroende på hello geografiska placeringen för privata blob storage-konto, hello processen att kopiera data från en offentlig blob tooyour privata storage-konto kan tar ungefär 15 minuter eller ännu längre och hello bearbeta inläsning av data från ditt lagringskonto tooyour Azure SQL DW ta 20 minuter eller längre.</span><span class="sxs-lookup"><span data-stu-id="c80a0-181">Depending on hello geographical location of your private blob storage account, hello process of copying data from a public blob tooyour private storage account can take about 15 minutes, or even longer,and hello process of loading data from your storage account tooyour Azure SQL DW could take 20 minutes or longer.</span></span>  
> 
> 

<span data-ttu-id="c80a0-182">Du måste toodecide vad gör om du har dubbla källa och målfiler.</span><span class="sxs-lookup"><span data-stu-id="c80a0-182">You will have toodecide what do if you have duplicate source and destination files.</span></span>

> [!NOTE]
> <span data-ttu-id="c80a0-183">Om hello CSV-filer toobe kopieras från hello offentlig blob storage tooyour privata blob storage-konto redan finns i ditt privata blob storage-konto, AzCopy tillfrågas du om du vill att toooverwrite dem.</span><span class="sxs-lookup"><span data-stu-id="c80a0-183">If hello .csv files toobe copied from hello public blob storage tooyour private blob storage account already exist in your private blob storage account, AzCopy will ask you whether you want toooverwrite them.</span></span> <span data-ttu-id="c80a0-184">Om du inte vill toooverwrite dem, inkommande  **n**  när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="c80a0-184">If you do not want toooverwrite them, input **n** when prompted.</span></span> <span data-ttu-id="c80a0-185">Om du vill toooverwrite **alla** av dem, ange **en** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="c80a0-185">If you want toooverwrite **all** of them, input **a** when prompted.</span></span> <span data-ttu-id="c80a0-186">Du kan också ange **y** toooverwrite CSV-filer separat.</span><span class="sxs-lookup"><span data-stu-id="c80a0-186">You can also input **y** toooverwrite .csv files individually.</span></span>
> 
> 

![Rita #21][21]

<span data-ttu-id="c80a0-188">Du kan använda dina egna data.</span><span class="sxs-lookup"><span data-stu-id="c80a0-188">You can use your own data.</span></span> <span data-ttu-id="c80a0-189">Om data finns i den lokala datorn i ditt program i verkligheten kan använda du fortfarande AzCopy tooupload lokala data tooyour privat Azure-blobblagring.</span><span class="sxs-lookup"><span data-stu-id="c80a0-189">If your data is in your on-premises machine in your real life application, you can still use AzCopy tooupload on-premises data tooyour private Azure blob storage.</span></span> <span data-ttu-id="c80a0-190">Behöver du bara toochange hello **källa** plats, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, i hello AzCopy-kommandot för hello PowerShell-skript fil toohello lokala katalog som innehåller dina data.</span><span class="sxs-lookup"><span data-stu-id="c80a0-190">You only need toochange hello **Source** location, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in hello AzCopy command of hello PowerShell script file toohello local directory that contains your data.</span></span>

> [!TIP]
> <span data-ttu-id="c80a0-191">Om data finns redan i din privata Azure blob storage i verkligheten programmet kan du hoppa över hello AzCopy steg i hello PowerShell-skript och direkt överföra hello data tooAzure SQL DW.</span><span class="sxs-lookup"><span data-stu-id="c80a0-191">If your data is already in your private Azure blob storage in your real life application, you can skip hello AzCopy step in hello PowerShell script and directly upload hello data tooAzure SQL DW.</span></span> <span data-ttu-id="c80a0-192">Detta kräver ytterligare redigerar av hello skriptet tootailor det toohello formatet för dina data.</span><span class="sxs-lookup"><span data-stu-id="c80a0-192">This will require additional edits of hello script tootailor it toohello format of your data.</span></span>
> 
> 

<span data-ttu-id="c80a0-193">Detta Powershell-skript ansluts också hello Azure SQL DW information till hello utforskning exempel datafiler SQLDW_Explorations.sql, SQLDW_Explorations.ipynb och SQLDW_Explorations_Scripts.py så att dessa tre filer är klar toobe försökte omedelbart När hello PowerShell-skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c80a0-193">This Powershell script also plugs in hello Azure SQL DW information into hello data exploration example files SQLDW_Explorations.sql, SQLDW_Explorations.ipynb, and SQLDW_Explorations_Scripts.py so that these three files are ready toobe tried out instantly after hello PowerShell script completes.</span></span>

<span data-ttu-id="c80a0-194">När du har körts, visas skärmen som nedan:</span><span class="sxs-lookup"><span data-stu-id="c80a0-194">After a successful execution, you will see screen like below:</span></span>

![][20]

## <span data-ttu-id="c80a0-195"><a name="dbexplore"></a>Datagranskning och funktionen teknikerna i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c80a0-195"><a name="dbexplore"></a>Data exploration and feature engineering in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="c80a0-196">I det här avsnittet vi utföra data från kartläggning av naturresurser och funktionen generation genom att köra SQL-frågor mot Azure SQL DW direkt med **Visual Studio Data Tools**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-196">In this section, we perform data exploration and feature generation by running SQL queries against Azure SQL DW directly using **Visual Studio Data Tools**.</span></span> <span data-ttu-id="c80a0-197">Alla SQL-frågor som används i det här avsnittet hittar du i hello exempelskript med namnet *SQLDW_Explorations.sql*.</span><span class="sxs-lookup"><span data-stu-id="c80a0-197">All SQL queries used in this section can be found in hello sample script named *SQLDW_Explorations.sql*.</span></span> <span data-ttu-id="c80a0-198">Den här filen har redan använts hämtade tooyour lokal katalog med hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="c80a0-198">This file has already been downloaded tooyour local directory by hello PowerShell script.</span></span> <span data-ttu-id="c80a0-199">Du kan också hämta det från [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span><span class="sxs-lookup"><span data-stu-id="c80a0-199">You can also retrieve it from [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span></span> <span data-ttu-id="c80a0-200">Men hello-filen i GitHub har inte hello Azure SQL DW information inkopplad.</span><span class="sxs-lookup"><span data-stu-id="c80a0-200">But hello file in GitHub does not have hello Azure SQL DW information plugged in.</span></span>

<span data-ttu-id="c80a0-201">Ansluta tooyour Azure SQL DW Visual Studio med hello SQL DW-inloggningsnamnet och lösenordet och öppna hello **SQL Object Explorer** tooconfirm hello-databasen och tabeller har importerats.</span><span class="sxs-lookup"><span data-stu-id="c80a0-201">Connect tooyour Azure SQL DW using Visual Studio with hello SQL DW login name and password and open up hello **SQL Object Explorer** tooconfirm hello database and tables have been imported.</span></span> <span data-ttu-id="c80a0-202">Hämta hello *SQLDW_Explorations.sql* fil.</span><span class="sxs-lookup"><span data-stu-id="c80a0-202">Retrieve hello *SQLDW_Explorations.sql* file.</span></span>

> [!NOTE]
> <span data-ttu-id="c80a0-203">tooopen en Parallel Data Warehouse (PDW) frågeredigeraren använda hello **ny fråga** kommandot när din PDW har valts i hello **SQL Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-203">tooopen a Parallel Data Warehouse (PDW) query editor, use hello **New Query** command while your PDW is selected in hello **SQL Object Explorer**.</span></span> <span data-ttu-id="c80a0-204">hello SQL-frågan standardredigeraren stöds inte av PDW.</span><span class="sxs-lookup"><span data-stu-id="c80a0-204">hello standard SQL query editor is not supported by PDW.</span></span>
> 
> 

<span data-ttu-id="c80a0-205">Här följer hello typ av data från kartläggning av naturresurser och funktionen generation uppgifter som utförs i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="c80a0-205">Here are hello type of data exploration and feature generation tasks performed in this section:</span></span>

* <span data-ttu-id="c80a0-206">Utforska data distributioner av ett fåtal fält i olika tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="c80a0-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="c80a0-207">Undersök data quality hello longitud och latitud fält.</span><span class="sxs-lookup"><span data-stu-id="c80a0-207">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="c80a0-208">Generera binära och multiklass-baserad klassificeringsetiketter baserat på hello **tips\_belopp**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-208">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="c80a0-209">Generera funktioner och beräkning eller jämförelse resa avstånd.</span><span class="sxs-lookup"><span data-stu-id="c80a0-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="c80a0-210">Koppla hello två tabeller och extrahera ett slumpmässigt prov som kommer att använda toobuild modeller.</span><span class="sxs-lookup"><span data-stu-id="c80a0-210">Join hello two tables and extract a random sample that will be used toobuild models.</span></span>

### <a name="data-import-verification"></a><span data-ttu-id="c80a0-211">Importera dataverifieringen</span><span class="sxs-lookup"><span data-stu-id="c80a0-211">Data import verification</span></span>
<span data-ttu-id="c80a0-212">De här frågorna ger en snabb kontroll av hello antalet rader och kolumner i hello tabeller fylls tidigare med Polybases parallella bulk importera</span><span class="sxs-lookup"><span data-stu-id="c80a0-212">These queries provide a quick verification of hello number of rows and columns in hello tables populated earlier using Polybase's parallel bulk import,</span></span>

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

<span data-ttu-id="c80a0-213">**Utdata:** du bör få 173,179,759 rader och 14 kolumner.</span><span class="sxs-lookup"><span data-stu-id="c80a0-213">**Output:** You should get 173,179,759 rows and 14 columns.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="c80a0-214">Undersökning: Resa distribution av medallion</span><span class="sxs-lookup"><span data-stu-id="c80a0-214">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="c80a0-215">Den här exempelfråga identifierar hello medallions (taxi numbers) som slutförda mer än 100 resor inom en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="c80a0-215">This example query identifies hello medallions (taxi numbers) that completed more than 100 trips within a specified time period.</span></span> <span data-ttu-id="c80a0-216">hello-frågan skulle dra nytta av hello partitionerad tabellåtkomst eftersom den är villkorad av hello partitionsschemat för **hämtning\_datetime**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-216">hello query would benefit from hello partitioned table access since it is conditioned by hello partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="c80a0-217">Frågar hello fullständiga datauppsättningen blir också användning av hello partitionerad tabell och/eller index-sökning.</span><span class="sxs-lookup"><span data-stu-id="c80a0-217">Querying hello full dataset will also make use of hello partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

<span data-ttu-id="c80a0-218">**Utdata:** hello frågan ska returnera en tabell med rader att ange hello 13,369 medallions (taxibilar) och hello antalet resa slutförts av dem i 2013.</span><span class="sxs-lookup"><span data-stu-id="c80a0-218">**Output:** hello query should return a table with rows specifying hello 13,369 medallions (taxis) and hello number of trip completed by them in 2013.</span></span> <span data-ttu-id="c80a0-219">hello sista kolumnen innehåller hello antal hello antalet turer som slutförts.</span><span class="sxs-lookup"><span data-stu-id="c80a0-219">hello last column contains hello count of hello number of trips completed.</span></span>

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="c80a0-220">Undersökning: Resa distribution av medallion och hack_license</span><span class="sxs-lookup"><span data-stu-id="c80a0-220">Exploration: Trip distribution by medallion and hack_license</span></span>
<span data-ttu-id="c80a0-221">Det här exemplet identifierar hello medallions (taxi numbers) och hack_license siffror (drivrutiner) som slutförda mer än 100 resor inom en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="c80a0-221">This example identifies hello medallions (taxi numbers) and hack_license numbers (drivers) that completed more than 100 trips within a specified time period.</span></span>

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

<span data-ttu-id="c80a0-222">**Utdata:** hello frågan ska returnera en tabell med 13,369 rader att ange hello 13,369 car och drivrutin ID: N som har slutfört mer som 100 resor i 2013.</span><span class="sxs-lookup"><span data-stu-id="c80a0-222">**Output:** hello query should return a table with 13,369 rows specifying hello 13,369 car/driver IDs that have completed more that 100 trips in 2013.</span></span> <span data-ttu-id="c80a0-223">hello sista kolumnen innehåller hello antal hello antalet turer som slutförts.</span><span class="sxs-lookup"><span data-stu-id="c80a0-223">hello last column contains hello count of hello number of trips completed.</span></span>

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="c80a0-224">Data utvärdering: Verifiera poster med felaktigt longituden eller latituden</span><span class="sxs-lookup"><span data-stu-id="c80a0-224">Data quality assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="c80a0-225">Det här exemplet undersöker om någon av hello longituden eller latituden fält antingen innehåller ett ogiltigt värde (radian grader ska vara mellan-90 och 90), eller ha (0, 0) koordinater.</span><span class="sxs-lookup"><span data-stu-id="c80a0-225">This example investigates if any of hello longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

<span data-ttu-id="c80a0-226">**Utdata:** hello frågan returnerar 837,467 resor som har ogiltiga longituden eller latituden fält.</span><span class="sxs-lookup"><span data-stu-id="c80a0-226">**Output:** hello query returns 837,467 trips that have invalid longitude and/or latitude fields.</span></span>

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="c80a0-227">Undersökning: Lutad kontra inte lutad resor distribution</span><span class="sxs-lookup"><span data-stu-id="c80a0-227">Exploration: Tipped vs. not tipped trips distribution</span></span>
<span data-ttu-id="c80a0-228">Det här exemplet hittar hello antalet turer som har lutad kontra hello tal som inte har lutad i en angiven tidsperiod (eller hello fullständiga datauppsättningen om täcker hello fullständig år som det ställs in här).</span><span class="sxs-lookup"><span data-stu-id="c80a0-228">This example finds hello number of trips that were tipped vs. hello number that were not tipped in a specified time period (or in hello full dataset if covering hello full year as it is set up here).</span></span> <span data-ttu-id="c80a0-229">Den här distributionen visar hello binära etikett distribution toobe senare används för binär klassificering modellering.</span><span class="sxs-lookup"><span data-stu-id="c80a0-229">This distribution reflects hello binary label distribution toobe later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

<span data-ttu-id="c80a0-230">**Utdata:** hello frågan ska returnera hello följande tips frekvenser för hello år 2013: 90,447,622 lutad och 82,264,709 inte lutad.</span><span class="sxs-lookup"><span data-stu-id="c80a0-230">**Output:** hello query should return hello following tip frequencies for hello year 2013: 90,447,622 tipped and 82,264,709 not-tipped.</span></span>

### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="c80a0-231">Undersökning: Tips klass-intervallet distribution</span><span class="sxs-lookup"><span data-stu-id="c80a0-231">Exploration: Tip class/range distribution</span></span>
<span data-ttu-id="c80a0-232">Det här exemplet beräknar hello distribution av tips intervall i en given tidpunkt tidsperiod (eller i hello fullständiga datauppsättningen om täcker hello fullständig år).</span><span class="sxs-lookup"><span data-stu-id="c80a0-232">This example computes hello distribution of tip ranges in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="c80a0-233">Detta är hello distribution av hello etikett klasser som ska användas senare för multiklass-baserad klassificering modellering.</span><span class="sxs-lookup"><span data-stu-id="c80a0-233">This is hello distribution of hello label classes that will be used later for multiclass classification modeling.</span></span>

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

<span data-ttu-id="c80a0-234">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="c80a0-234">**Output:**</span></span>

| <span data-ttu-id="c80a0-235">tip_class</span><span class="sxs-lookup"><span data-stu-id="c80a0-235">tip_class</span></span> | <span data-ttu-id="c80a0-236">tip_freq</span><span class="sxs-lookup"><span data-stu-id="c80a0-236">tip_freq</span></span> |
| --- | --- |
| <span data-ttu-id="c80a0-237">1</span><span class="sxs-lookup"><span data-stu-id="c80a0-237">1</span></span> |<span data-ttu-id="c80a0-238">82230915</span><span class="sxs-lookup"><span data-stu-id="c80a0-238">82230915</span></span> |
| <span data-ttu-id="c80a0-239">2</span><span class="sxs-lookup"><span data-stu-id="c80a0-239">2</span></span> |<span data-ttu-id="c80a0-240">6198803</span><span class="sxs-lookup"><span data-stu-id="c80a0-240">6198803</span></span> |
| <span data-ttu-id="c80a0-241">3</span><span class="sxs-lookup"><span data-stu-id="c80a0-241">3</span></span> |<span data-ttu-id="c80a0-242">1932223</span><span class="sxs-lookup"><span data-stu-id="c80a0-242">1932223</span></span> |
| <span data-ttu-id="c80a0-243">0</span><span class="sxs-lookup"><span data-stu-id="c80a0-243">0</span></span> |<span data-ttu-id="c80a0-244">82264625</span><span class="sxs-lookup"><span data-stu-id="c80a0-244">82264625</span></span> |
| <span data-ttu-id="c80a0-245">4</span><span class="sxs-lookup"><span data-stu-id="c80a0-245">4</span></span> |<span data-ttu-id="c80a0-246">85765</span><span class="sxs-lookup"><span data-stu-id="c80a0-246">85765</span></span> |

### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="c80a0-247">Undersökning: Beräkna och jämföra resa avstånd</span><span class="sxs-lookup"><span data-stu-id="c80a0-247">Exploration: Compute and compare trip distance</span></span>
<span data-ttu-id="c80a0-248">Det här exemplet konverterar hello hämtning och Samlingsbibliotek longitud och latitud tooSQL geografi pekar beräknar hello resa avståndet med SQL geografi punkter skillnaden och returnerar ett slumpvist urval av hello resultat för jämförelse.</span><span class="sxs-lookup"><span data-stu-id="c80a0-248">This example converts hello pickup and drop-off longitude and latitude tooSQL geography points, computes hello trip distance using SQL geography points difference, and returns a random sample of hello results for comparison.</span></span> <span data-ttu-id="c80a0-249">hello exempel begränsar hello resultat toovalid samordnar bara använder hello kvalitet assessment datafrågor omfattas tidigare.</span><span class="sxs-lookup"><span data-stu-id="c80a0-249">hello example limits hello results toovalid coordinates only using hello data quality assessment query covered earlier.</span></span>

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function toocalculate hello direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a><span data-ttu-id="c80a0-250">Funktionen tekniker med hjälp av SQL-funktioner</span><span class="sxs-lookup"><span data-stu-id="c80a0-250">Feature engineering using SQL functions</span></span>
<span data-ttu-id="c80a0-251">SQL-funktioner kan ibland vara ett effektivt alternativ för funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="c80a0-251">Sometimes SQL functions can be an efficient option for feature engineering.</span></span> <span data-ttu-id="c80a0-252">Vi har definierat ett SQL-funktionen toocalculate hello direkt avstånd mellan hello hämtning och dropoff platser i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="c80a0-252">In this walkthrough, we defined a SQL function toocalculate hello direct distance between hello pickup and dropoff locations.</span></span> <span data-ttu-id="c80a0-253">Du kan köra följande SQL-skript i hello **Visual Studio Data Tools**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-253">You can run hello following SQL scripts in **Visual Studio Data Tools**.</span></span>

<span data-ttu-id="c80a0-254">Här är hello SQL-skript som definierar hello avståndet funktion.</span><span class="sxs-lookup"><span data-stu-id="c80a0-254">Here is hello SQL script that defines hello distance function.</span></span>

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate hello direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

<span data-ttu-id="c80a0-255">Här är ett exempel toocall denna funktion toogenerate funktioner i SQL-frågan:</span><span class="sxs-lookup"><span data-stu-id="c80a0-255">Here is an example toocall this function toogenerate features in your SQL query:</span></span>

    -- Sample query toocall hello function toocreate features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

<span data-ttu-id="c80a0-256">**Utdata:** den här frågan genererar en tabell (med 2,803,538 rader) med hämtning och dropoff latituderna och longitudes och hello motsvarande direkt avstånd i mil.</span><span class="sxs-lookup"><span data-stu-id="c80a0-256">**Output:** This query generates a table (with 2,803,538 rows) with pickup and dropoff latitudes and longitudes and hello corresponding direct distances in miles.</span></span> <span data-ttu-id="c80a0-257">Här följer hello resultat för första 3 rader:</span><span class="sxs-lookup"><span data-stu-id="c80a0-257">Here are hello results for first 3 rows:</span></span>

|  | <span data-ttu-id="c80a0-258">pickup_latitude</span><span class="sxs-lookup"><span data-stu-id="c80a0-258">pickup_latitude</span></span> | <span data-ttu-id="c80a0-259">pickup_longitude</span><span class="sxs-lookup"><span data-stu-id="c80a0-259">pickup_longitude</span></span> | <span data-ttu-id="c80a0-260">dropoff_latitude</span><span class="sxs-lookup"><span data-stu-id="c80a0-260">dropoff_latitude</span></span> | <span data-ttu-id="c80a0-261">dropoff_longitude</span><span class="sxs-lookup"><span data-stu-id="c80a0-261">dropoff_longitude</span></span> | <span data-ttu-id="c80a0-262">DirectDistance</span><span class="sxs-lookup"><span data-stu-id="c80a0-262">DirectDistance</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="c80a0-263">1</span><span class="sxs-lookup"><span data-stu-id="c80a0-263">1</span></span> |<span data-ttu-id="c80a0-264">40.731804</span><span class="sxs-lookup"><span data-stu-id="c80a0-264">40.731804</span></span> |<span data-ttu-id="c80a0-265">-74.001083</span><span class="sxs-lookup"><span data-stu-id="c80a0-265">-74.001083</span></span> |<span data-ttu-id="c80a0-266">40.736622</span><span class="sxs-lookup"><span data-stu-id="c80a0-266">40.736622</span></span> |<span data-ttu-id="c80a0-267">-73.988953</span><span class="sxs-lookup"><span data-stu-id="c80a0-267">-73.988953</span></span> |<span data-ttu-id="c80a0-268">.7169601222</span><span class="sxs-lookup"><span data-stu-id="c80a0-268">.7169601222</span></span> |
| <span data-ttu-id="c80a0-269">2</span><span class="sxs-lookup"><span data-stu-id="c80a0-269">2</span></span> |<span data-ttu-id="c80a0-270">40.715794</span><span class="sxs-lookup"><span data-stu-id="c80a0-270">40.715794</span></span> |<span data-ttu-id="c80a0-271">-74,010635</span><span class="sxs-lookup"><span data-stu-id="c80a0-271">-74,010635</span></span> |<span data-ttu-id="c80a0-272">40.725338</span><span class="sxs-lookup"><span data-stu-id="c80a0-272">40.725338</span></span> |<span data-ttu-id="c80a0-273">-74.00399</span><span class="sxs-lookup"><span data-stu-id="c80a0-273">-74.00399</span></span> |<span data-ttu-id="c80a0-274">.7448343721</span><span class="sxs-lookup"><span data-stu-id="c80a0-274">.7448343721</span></span> |
| <span data-ttu-id="c80a0-275">3</span><span class="sxs-lookup"><span data-stu-id="c80a0-275">3</span></span> |<span data-ttu-id="c80a0-276">40.761456</span><span class="sxs-lookup"><span data-stu-id="c80a0-276">40.761456</span></span> |<span data-ttu-id="c80a0-277">-73.999886</span><span class="sxs-lookup"><span data-stu-id="c80a0-277">-73.999886</span></span> |<span data-ttu-id="c80a0-278">40.766544</span><span class="sxs-lookup"><span data-stu-id="c80a0-278">40.766544</span></span> |<span data-ttu-id="c80a0-279">-73.988228</span><span class="sxs-lookup"><span data-stu-id="c80a0-279">-73.988228</span></span> |<span data-ttu-id="c80a0-280">0.7037227967</span><span class="sxs-lookup"><span data-stu-id="c80a0-280">0.7037227967</span></span> |

### <a name="prepare-data-for-model-building"></a><span data-ttu-id="c80a0-281">Förbered data för modellskapandet</span><span class="sxs-lookup"><span data-stu-id="c80a0-281">Prepare data for model building</span></span>
<span data-ttu-id="c80a0-282">hello följande fråga kopplingar hello **nyctaxi\_resa** och **nyctaxi\_avgiften** tabeller, genererar en binär klassificering etikett **lutad**, flera klassen klassificering etikett **tips\_klassen**, och extraherar ett sampel från hello fullständigt sammanfogade dataset.</span><span class="sxs-lookup"><span data-stu-id="c80a0-282">hello following query joins hello **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a sample from hello full joined dataset.</span></span> <span data-ttu-id="c80a0-283">hello provtagning görs genom att hämta en delmängd av hello resor baserat på pickup tid.</span><span class="sxs-lookup"><span data-stu-id="c80a0-283">hello sampling is done by retrieving a subset of hello trips based on pickup time.</span></span>  <span data-ttu-id="c80a0-284">Den här frågan kan kopieras och klistras in direkt i hello [Azure Machine Learning Studio](https://studio.azureml.net) [importera Data] [ import-data] -modulen för direkt datapåfyllning från hello SQL-databasinstans i Azure.</span><span class="sxs-lookup"><span data-stu-id="c80a0-284">This query can be copied then pasted directly in hello [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from hello SQL database instance in Azure.</span></span> <span data-ttu-id="c80a0-285">hello frågan utesluter poster med fel (0, 0) koordinater.</span><span class="sxs-lookup"><span data-stu-id="c80a0-285">hello query excludes records with incorrect (0, 0) coordinates.</span></span>

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

<span data-ttu-id="c80a0-286">När du är klar tooproceed tooAzure Machine Learning, kan du antingen:</span><span class="sxs-lookup"><span data-stu-id="c80a0-286">When you are ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="c80a0-287">Spara hello slutliga SQL tooextract och exempel hello data och kopiera / klistra in hello fråga direkt till en [importera Data] [ import-data] modul i Azure Machine Learning, eller</span><span class="sxs-lookup"><span data-stu-id="c80a0-287">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="c80a0-288">Kvarstår hello provtagning och bakåtkompilerade data som du planerar toouse för modellen bygga i en ny SQL DW tabell och använda hello ny tabell i hello [importera Data] [ import-data] modul i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c80a0-288">Persist hello sampled and engineered data you plan toouse for model building in a new SQL DW table and use hello new table in hello [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="c80a0-289">hello PowerShell-skript i tidigare steg har gjort det åt dig.</span><span class="sxs-lookup"><span data-stu-id="c80a0-289">hello PowerShell script in earlier step has done this for you.</span></span> <span data-ttu-id="c80a0-290">Du kan läsa direkt från den här tabellen i hello importera Data modul.</span><span class="sxs-lookup"><span data-stu-id="c80a0-290">You can read directly from this table in hello Import Data module.</span></span>

## <span data-ttu-id="c80a0-291"><a name="ipnb"></a>Datagranskning och funktionen tekniker i IPython anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="c80a0-291"><a name="ipnb"></a>Data exploration and feature engineering in IPython notebook</span></span>
<span data-ttu-id="c80a0-292">Vi kommer att utföra datagranskning och funktionen generation använder båda Python och SQL-frågor mot hello SQL DW skapade tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c80a0-292">In this section, we will perform data exploration and feature generation using both Python and SQL queries against hello SQL DW created earlier.</span></span> <span data-ttu-id="c80a0-293">En exempel IPython bärbar dator med namnet **SQLDW_Explorations.ipynb** och en Python-skriptfil **SQLDW_Explorations_Scripts.py** har hämtat tooyour lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="c80a0-293">A sample IPython notebook named **SQLDW_Explorations.ipynb** and a Python script file **SQLDW_Explorations_Scripts.py** have been downloaded tooyour local directory.</span></span> <span data-ttu-id="c80a0-294">De är också tillgängliga på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span><span class="sxs-lookup"><span data-stu-id="c80a0-294">They are also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span></span> <span data-ttu-id="c80a0-295">Dessa två filer är identiska i Python-skript.</span><span class="sxs-lookup"><span data-stu-id="c80a0-295">These two files are identical in Python scripts.</span></span> <span data-ttu-id="c80a0-296">hello Python skriptfilen tillhandahålls tooyou om du inte har en bärbar dator IPython-server.</span><span class="sxs-lookup"><span data-stu-id="c80a0-296">hello Python script file is provided tooyou in case you do not have an IPython Notebook server.</span></span> <span data-ttu-id="c80a0-297">Dessa två exempel Python filer är utformade under **Python 2.7**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-297">These two sample Python files are designed under **Python 2.7**.</span></span>

<span data-ttu-id="c80a0-298">hello Azure SQL DW-information som behövs i hello prov IPython anteckningsboken och hello Python skriptet filen hämtade tooyour lokala datorn har anslutits med hello PowerShell-skript tidigare.</span><span class="sxs-lookup"><span data-stu-id="c80a0-298">hello needed Azure SQL DW information in hello sample IPython Notebook and hello Python script file downloaded tooyour local machine has been plugged in by hello PowerShell script previously.</span></span> <span data-ttu-id="c80a0-299">De är körbara utan några ändringar.</span><span class="sxs-lookup"><span data-stu-id="c80a0-299">They are executable without any modification.</span></span>

<span data-ttu-id="c80a0-300">Om du redan har installerat en AzureML-arbetsyta, du direkt överför hello exempel IPython anteckningsboken toohello AzureML IPython anteckningsboken service och starta körs.</span><span class="sxs-lookup"><span data-stu-id="c80a0-300">If you have already set up an AzureML workspace, you can directly upload hello sample IPython Notebook toohello AzureML IPython Notebook service and start running it.</span></span> <span data-ttu-id="c80a0-301">Här följer hello steg tooupload tooAzureML IPython anteckningsboken tjänsten:</span><span class="sxs-lookup"><span data-stu-id="c80a0-301">Here are hello steps tooupload tooAzureML IPython Notebook service:</span></span>

1. <span data-ttu-id="c80a0-302">Logga in tooyour AzureML arbetsytan, klickar du på ”Studio” hello överst och klicka på ”bärbara datorer” hello vänster på webbsidan hello.</span><span class="sxs-lookup"><span data-stu-id="c80a0-302">Log in tooyour AzureML workspace, click "Studio" at hello top, and click "NOTEBOOKS" on hello left side of hello web page.</span></span>
   
    ![Rita #22][22]
2. <span data-ttu-id="c80a0-304">Klicka på ”ny” i hello nedre vänstra hörnet på webbsidan hello och välj ”Python 2”.</span><span class="sxs-lookup"><span data-stu-id="c80a0-304">Click "NEW" on hello left bottom corner of hello web page, and select "Python 2".</span></span> <span data-ttu-id="c80a0-305">Sedan anger en bärbar dator toohello namn och klickar på hello markerat toocreate hello nya tomma IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="c80a0-305">Then, provide a name toohello notebook and click hello check mark toocreate hello new blank IPython Notebook.</span></span>
   
    ![Rita #23][23]
3. <span data-ttu-id="c80a0-307">Klicka på symbolen för hello ”Jupyter” på hello översta till vänster i hello ny IPython anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="c80a0-307">Click hello "Jupyter" symbol on hello left top corner of hello new IPython Notebook.</span></span>
   
    ![Rita #24][24]
4. <span data-ttu-id="c80a0-309">Dra och släpp hello exempel IPython anteckningsboken toohello **träd** i din AzureML IPython anteckningsboken tjänsten och klickar på **överför**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-309">Drag and drop hello sample IPython Notebook toohello **tree** page of your AzureML IPython Notebook service, and click **Upload**.</span></span> <span data-ttu-id="c80a0-310">Hello exempel IPython anteckningsboken kommer sedan att överförda toohello AzureML IPython anteckningsboken service.</span><span class="sxs-lookup"><span data-stu-id="c80a0-310">Then, hello sample IPython Notebook will be uploaded toohello AzureML IPython Notebook service.</span></span>
   
    ![Rita #25][25]

<span data-ttu-id="c80a0-312">I ordning toorun hello exempel IPython bärbar dator eller Hej Python skriptfilen, hello följande Python-paket som behövs.</span><span class="sxs-lookup"><span data-stu-id="c80a0-312">In order toorun hello sample IPython Notebook or hello Python script file, hello following Python packages are needed.</span></span> <span data-ttu-id="c80a0-313">Om du använder hello AzureML IPython anteckningsboken service har paketen installerats före.</span><span class="sxs-lookup"><span data-stu-id="c80a0-313">If you are using hello AzureML IPython Notebook service, these packages have been pre-installed.</span></span>

    - <span data-ttu-id="c80a0-314">pandas</span><span class="sxs-lookup"><span data-stu-id="c80a0-314">pandas</span></span>
    - <span data-ttu-id="c80a0-315">numpy</span><span class="sxs-lookup"><span data-stu-id="c80a0-315">numpy</span></span>
    - <span data-ttu-id="c80a0-316">matplotlib</span><span class="sxs-lookup"><span data-stu-id="c80a0-316">matplotlib</span></span>
    - <span data-ttu-id="c80a0-317">pyodbc</span><span class="sxs-lookup"><span data-stu-id="c80a0-317">pyodbc</span></span>
    - <span data-ttu-id="c80a0-318">PyTables</span><span class="sxs-lookup"><span data-stu-id="c80a0-318">PyTables</span></span>

<span data-ttu-id="c80a0-319">hello rekommenderas sekvens när du skapar avancerade analytiska lösningar på AzureML med stora mängder data är hello följande:</span><span class="sxs-lookup"><span data-stu-id="c80a0-319">hello recommended sequence when building advanced analytical solutions on AzureML with large data is hello following:</span></span>

* <span data-ttu-id="c80a0-320">Läs i ett litet antal hello data till en ram i minnet data.</span><span class="sxs-lookup"><span data-stu-id="c80a0-320">Read in a small sample of hello data into an in-memory data frame.</span></span>
* <span data-ttu-id="c80a0-321">Utföra vissa visualiseringar och explorations med hello samplade data.</span><span class="sxs-lookup"><span data-stu-id="c80a0-321">Perform some visualizations and explorations using hello sampled data.</span></span>
* <span data-ttu-id="c80a0-322">Experiment med funktionen tekniker med hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="c80a0-322">Experiment with feature engineering using hello sampled data.</span></span>
* <span data-ttu-id="c80a0-323">För större datagranskning datamanipulering och funktionen konstruktion, kan du använda Python tooissue SQL-frågor direkt mot hello SQL DW.</span><span class="sxs-lookup"><span data-stu-id="c80a0-323">For larger data exploration, data manipulation and feature engineering, use Python tooissue SQL Queries directly against hello SQL DW.</span></span>
* <span data-ttu-id="c80a0-324">Bestäm hello exempel storlek toobe lämpar sig för Azure Machine Learning modellskapandet.</span><span class="sxs-lookup"><span data-stu-id="c80a0-324">Decide hello sample size toobe suitable for Azure Machine Learning model building.</span></span>

<span data-ttu-id="c80a0-325">hello följande är några datagranskning datavisualisering exempel, och funktionen engineering.</span><span class="sxs-lookup"><span data-stu-id="c80a0-325">hello followings are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="c80a0-326">Mer data explorations kan hittas i hello exemplet IPython anteckningsboken och hello Python exempelfilen.</span><span class="sxs-lookup"><span data-stu-id="c80a0-326">More data explorations can be found in hello sample IPython Notebook and hello sample Python script file.</span></span>

### <a name="initialize-database-credentials"></a><span data-ttu-id="c80a0-327">Initiera Databasautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="c80a0-327">Initialize database credentials</span></span>
<span data-ttu-id="c80a0-328">Initiera databasen anslutningsinställningarna i hello följande variabler:</span><span class="sxs-lookup"><span data-stu-id="c80a0-328">Initialize your database connection settings in hello following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a><span data-ttu-id="c80a0-329">Skapa databasanslutning</span><span class="sxs-lookup"><span data-stu-id="c80a0-329">Create database connection</span></span>
<span data-ttu-id="c80a0-330">Här är hello anslutningssträngen som skapar hello toohello databas.</span><span class="sxs-lookup"><span data-stu-id="c80a0-330">Here is hello connection string that creates hello connection toohello database.</span></span>

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="c80a0-331">Rapporten antalet rader och kolumner i tabellen < nyctaxi_trip ></span><span class="sxs-lookup"><span data-stu-id="c80a0-331">Report number of rows and columns in table <nyctaxi_trip></span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="c80a0-332">Totalt antal rader = 173179759</span><span class="sxs-lookup"><span data-stu-id="c80a0-332">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="c80a0-333">Totalt antal kolumner = 14</span><span class="sxs-lookup"><span data-stu-id="c80a0-333">Total number of columns = 14</span></span>

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a><span data-ttu-id="c80a0-334">Rapporten antalet rader och kolumner i tabellen < nyctaxi_fare ></span><span class="sxs-lookup"><span data-stu-id="c80a0-334">Report number of rows and columns in table <nyctaxi_fare></span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="c80a0-335">Totalt antal rader = 173179759</span><span class="sxs-lookup"><span data-stu-id="c80a0-335">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="c80a0-336">Totalt antal kolumner = 11</span><span class="sxs-lookup"><span data-stu-id="c80a0-336">Total number of columns = 11</span></span>

### <a name="read-in-a-small-data-sample-from-hello-sql-data-warehouse-database"></a><span data-ttu-id="c80a0-337">Läs i ett litet datasampel från hello SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="c80a0-337">Read-in a small data sample from hello SQL Data Warehouse Database</span></span>
    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="c80a0-338">Tid tooread hello exempeltabell är 14.096495 sekunder.</span><span class="sxs-lookup"><span data-stu-id="c80a0-338">Time tooread hello sample table is 14.096495 seconds.</span></span>  
<span data-ttu-id="c80a0-339">Antal rader och kolumner hämtas = (1000, 21).</span><span class="sxs-lookup"><span data-stu-id="c80a0-339">Number of rows and columns retrieved = (1000, 21).</span></span>

### <a name="descriptive-statistics"></a><span data-ttu-id="c80a0-340">Beskrivande statistik</span><span class="sxs-lookup"><span data-stu-id="c80a0-340">Descriptive statistics</span></span>
<span data-ttu-id="c80a0-341">Nu är du redo tooexplore hello provtagning data.</span><span class="sxs-lookup"><span data-stu-id="c80a0-341">Now you are ready tooexplore hello sampled data.</span></span> <span data-ttu-id="c80a0-342">Vi börjar med att titta på vissa beskrivande statistik för hello **resa\_avstånd** (eller andra fält som du väljer toospecify).</span><span class="sxs-lookup"><span data-stu-id="c80a0-342">We start with looking at some descriptive statistics for hello **trip\_distance** (or any other fields you choose toospecify).</span></span>

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a><span data-ttu-id="c80a0-343">Visualiseringen: Exempel på ritytans</span><span class="sxs-lookup"><span data-stu-id="c80a0-343">Visualization: Box plot example</span></span>
<span data-ttu-id="c80a0-344">Vi titta på hello Låddiagram för hello resa avståndet toovisualize hello quantiles.</span><span class="sxs-lookup"><span data-stu-id="c80a0-344">Next we look at hello box plot for hello trip distance toovisualize hello quantiles.</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Rita #1][1]

### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="c80a0-346">Visualiseringen: Distribution ritytans exempel</span><span class="sxs-lookup"><span data-stu-id="c80a0-346">Visualization: Distribution plot example</span></span>
<span data-ttu-id="c80a0-347">Områden som visualiserar hello distribution och ett histogram för hello prov resa avstånd.</span><span class="sxs-lookup"><span data-stu-id="c80a0-347">Plots that visualize hello distribution and a histogram for hello sampled trip distances.</span></span>

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Rita #2][2]

### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="c80a0-349">Visualiseringen: Menyraden och rad områden</span><span class="sxs-lookup"><span data-stu-id="c80a0-349">Visualization: Bar and line plots</span></span>
<span data-ttu-id="c80a0-350">I det här exemplet vi bin hello resa avståndet i fem lagerplatser och visualisera hello diskretisering resultat.</span><span class="sxs-lookup"><span data-stu-id="c80a0-350">In this example, we bin hello trip distance into five bins and visualize hello binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="c80a0-351">Vi kan ritas hello ovan bin distribution i ett fält eller rad ritytans med:</span><span class="sxs-lookup"><span data-stu-id="c80a0-351">We can plot hello above bin distribution in a bar or line plot with:</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Rita #3][3]

<span data-ttu-id="c80a0-353">och</span><span class="sxs-lookup"><span data-stu-id="c80a0-353">and</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Rita #4][4]

### <a name="visualization-scatterplot-examples"></a><span data-ttu-id="c80a0-355">Visualiseringen: Scatterplot exempel</span><span class="sxs-lookup"><span data-stu-id="c80a0-355">Visualization: Scatterplot examples</span></span>
<span data-ttu-id="c80a0-356">Visar vi punktdiagram ritytans mellan **resa\_tid\_i\_sek** och **resa\_avstånd** toosee om det finns några korrelation</span><span class="sxs-lookup"><span data-stu-id="c80a0-356">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** toosee if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Rita #6][6]

<span data-ttu-id="c80a0-358">På liknande sätt kan vi Kontrollera hello förhållandet mellan **hastighet\_kod** och **resa\_avstånd**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-358">Similarly we can check hello relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Rita #8][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="c80a0-360">Datagranskning på samplade data med hjälp av SQL-frågor i IPython anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="c80a0-360">Data exploration on sampled data using SQL queries in IPython notebook</span></span>
<span data-ttu-id="c80a0-361">I det här avsnittet förklarar vi data distributioner med hello provtagning data som sparas i hello ny tabell som vi skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="c80a0-361">In this section, we explore data distributions using hello sampled data which is persisted in hello new table we created above.</span></span> <span data-ttu-id="c80a0-362">Observera att liknande explorations kan utföras med hjälp av hello ursprungliga tabeller.</span><span class="sxs-lookup"><span data-stu-id="c80a0-362">Note that similar explorations can be performed using hello original tables.</span></span>

#### <a name="exploration-report-number-of-rows-and-columns-in-hello-sampled-table"></a><span data-ttu-id="c80a0-363">Undersökning: Rapporten antalet rader och kolumner i hello provtagning tabell</span><span class="sxs-lookup"><span data-stu-id="c80a0-363">Exploration: Report number of rows and columns in hello sampled table</span></span>
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a><span data-ttu-id="c80a0-364">Undersökning: Lutad ej utlöses Distribution</span><span class="sxs-lookup"><span data-stu-id="c80a0-364">Exploration: Tipped/not tripped Distribution</span></span>
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a><span data-ttu-id="c80a0-365">Undersökning: Tips klassen distribution</span><span class="sxs-lookup"><span data-stu-id="c80a0-365">Exploration: Tip class distribution</span></span>
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-hello-tip-distribution-by-class"></a><span data-ttu-id="c80a0-366">Undersökning: Rita hello tips distribution av klassen</span><span class="sxs-lookup"><span data-stu-id="c80a0-366">Exploration: Plot hello tip distribution by class</span></span>
    tip_class_dist['tip_freq'].plot(kind='bar')

![Rita #26][26]

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="c80a0-368">Undersökning: Dagliga distribution av resor</span><span class="sxs-lookup"><span data-stu-id="c80a0-368">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="c80a0-369">Undersökning: Resa distribution per medallion</span><span class="sxs-lookup"><span data-stu-id="c80a0-369">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a><span data-ttu-id="c80a0-370">Undersökning: Resa distribution av medallion och hackare licens</span><span class="sxs-lookup"><span data-stu-id="c80a0-370">Exploration: Trip distribution by medallion and hack license</span></span>
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a><span data-ttu-id="c80a0-371">Undersökning: Fördelning av resa</span><span class="sxs-lookup"><span data-stu-id="c80a0-371">Exploration: Trip time distribution</span></span>
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a><span data-ttu-id="c80a0-372">Undersökning: Resa avståndet distribution</span><span class="sxs-lookup"><span data-stu-id="c80a0-372">Exploration: Trip distance distribution</span></span>
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a><span data-ttu-id="c80a0-373">Undersökning: Betalning typen distribution</span><span class="sxs-lookup"><span data-stu-id="c80a0-373">Exploration: Payment type distribution</span></span>
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a><span data-ttu-id="c80a0-374">Kontrollera hello slutliga form av hello featurized tabell</span><span class="sxs-lookup"><span data-stu-id="c80a0-374">Verify hello final form of hello featurized table</span></span>
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <span data-ttu-id="c80a0-375"><a name="mlmodel"></a>Bygga modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c80a0-375"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="c80a0-376">Vi är nu redo tooproceed toomodel byggnad och distribution av modellen i [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="c80a0-376">We are now ready tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="c80a0-377">hello data är klar toobe som används i någon av hello förutsägelse problem som konstaterats tidigare, nämligen:</span><span class="sxs-lookup"><span data-stu-id="c80a0-377">hello data is ready toobe used in any of hello prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="c80a0-378">**Binär klassificering**: toopredict om huruvida ett tips har betalat för en resa.</span><span class="sxs-lookup"><span data-stu-id="c80a0-378">**Binary classification**: toopredict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="c80a0-379">**Multiklass-baserad klassificering**: toopredict hello mängd tips betald, enligt toohello tidigare definierade klasserna.</span><span class="sxs-lookup"><span data-stu-id="c80a0-379">**Multiclass classification**: toopredict hello range of tip paid, according toohello previously defined classes.</span></span>
3. <span data-ttu-id="c80a0-380">**Regression uppgiften**: toopredict hello tips betalats för en transport.</span><span class="sxs-lookup"><span data-stu-id="c80a0-380">**Regression task**: toopredict hello amount of tip paid for a trip.</span></span>  

<span data-ttu-id="c80a0-381">toobegin Hej modeling Övning, logga in tooyour **Azure Machine Learning** arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="c80a0-381">toobegin hello modeling exercise, log in tooyour **Azure Machine Learning** workspace.</span></span> <span data-ttu-id="c80a0-382">Om du inte har skapat machine learning-arbetsytan finns [skapa en arbetsyta för Azure ML](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="c80a0-382">If you have not yet created a machine learning workspace, see [Create an Azure ML workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="c80a0-383">tooget igång med Azure Machine Learning finns [vad är Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="c80a0-383">tooget started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="c80a0-384">Logga in för[Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="c80a0-384">Log in too[Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="c80a0-385">startsidan för hello Studio innehåller en mängd information, videor, självstudier, länkar toohello moduler referens och andra resurser.</span><span class="sxs-lookup"><span data-stu-id="c80a0-385">hello Studio Home page provides a wealth of information, videos, tutorials, links toohello Modules Reference, and other resources.</span></span> <span data-ttu-id="c80a0-386">Mer information om Azure Machine Learning finns hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="c80a0-386">For more information about Azure Machine Learning, consult hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="c80a0-387">En typisk träningsexperiment består av hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c80a0-387">A typical training experiment consists of hello following steps:</span></span>

1. <span data-ttu-id="c80a0-388">Skapa en **+ ny** experiment.</span><span class="sxs-lookup"><span data-stu-id="c80a0-388">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="c80a0-389">Hämta hello data till Azure ML.</span><span class="sxs-lookup"><span data-stu-id="c80a0-389">Get hello data into Azure ML.</span></span>
3. <span data-ttu-id="c80a0-390">Förbearbeta, transformera och manipulera hello data efter behov.</span><span class="sxs-lookup"><span data-stu-id="c80a0-390">Pre-process, transform and manipulate hello data as needed.</span></span>
4. <span data-ttu-id="c80a0-391">Generera funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="c80a0-391">Generate features as needed.</span></span>
5. <span data-ttu-id="c80a0-392">Dela upp hello data i utbildning / / verifieringstesterna datauppsättningar (eller ha separata datauppsättningar för varje).</span><span class="sxs-lookup"><span data-stu-id="c80a0-392">Split hello data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="c80a0-393">Välj en eller flera maskininlärningsalgoritmer beroende på hello learning problemet toosolve.</span><span class="sxs-lookup"><span data-stu-id="c80a0-393">Select one or more machine learning algorithms depending on hello learning problem toosolve.</span></span> <span data-ttu-id="c80a0-394">T.ex. binär klassificering, multiklass-baserad klassificering, regression.</span><span class="sxs-lookup"><span data-stu-id="c80a0-394">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="c80a0-395">Träna en eller flera modeller som använder hello utbildning dataset.</span><span class="sxs-lookup"><span data-stu-id="c80a0-395">Train one or more models using hello training dataset.</span></span>
8. <span data-ttu-id="c80a0-396">Poängsätta hello validering dataset med hello tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="c80a0-396">Score hello validation dataset using hello trained model(s).</span></span>
9. <span data-ttu-id="c80a0-397">Utvärdera hello modeller toocompute hello relevanta mätvärden för hello learning problem.</span><span class="sxs-lookup"><span data-stu-id="c80a0-397">Evaluate hello model(s) toocompute hello relevant metrics for hello learning problem.</span></span>
10. <span data-ttu-id="c80a0-398">Finjustera hello modeller och välj hello bästa modellen toodeploy.</span><span class="sxs-lookup"><span data-stu-id="c80a0-398">Fine tune hello model(s) and select hello best model toodeploy.</span></span>

<span data-ttu-id="c80a0-399">I den här övningen har vi redan utforskade och utformad hello data i SQL Data Warehouse och valt hello exempel storlek tooingest i Azure ML.</span><span class="sxs-lookup"><span data-stu-id="c80a0-399">In this exercise, we have already explored and engineered hello data in SQL Data Warehouse, and decided on hello sample size tooingest in Azure ML.</span></span> <span data-ttu-id="c80a0-400">Här är hello proceduren toobuild en eller flera av hello förutsägelse modeller:</span><span class="sxs-lookup"><span data-stu-id="c80a0-400">Here is hello procedure toobuild one or more of hello prediction models:</span></span>

1. <span data-ttu-id="c80a0-401">Hämta hello data till Azure ML med hello [importera Data] [ import-data] modulen är tillgängliga i hello **Data ingående och utgående** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c80a0-401">Get hello data into Azure ML using hello [Import Data][import-data] module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="c80a0-402">Mer information finns i hello [importera Data] [ import-data] modulsida referens.</span><span class="sxs-lookup"><span data-stu-id="c80a0-402">For more information, see hello [Import Data][import-data] module reference page.</span></span>
   
    ![Azure ML-importera Data][17]
2. <span data-ttu-id="c80a0-404">Välj **Azure SQL Database** som hello **datakällan** i hello **egenskaper** panelen.</span><span class="sxs-lookup"><span data-stu-id="c80a0-404">Select **Azure SQL Database** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="c80a0-405">Ange hello DNS-namn i hello **Databasservernamnet** fältet.</span><span class="sxs-lookup"><span data-stu-id="c80a0-405">Enter hello database DNS name in hello **Database server name** field.</span></span> <span data-ttu-id="c80a0-406">Format:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="c80a0-406">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="c80a0-407">Ange hello **databasnamnet** i hello motsvarande fält.</span><span class="sxs-lookup"><span data-stu-id="c80a0-407">Enter hello **Database name** in hello corresponding field.</span></span>
5. <span data-ttu-id="c80a0-408">Ange hello *användarnamn för SQL* i hello **Server användarkontonamnet**, och hello *lösenord* i hello **serverlösenord**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-408">Enter hello *SQL user name* in hello **Server user account name**, and hello *password* in hello **Server user account password**.</span></span>
6. <span data-ttu-id="c80a0-409">Kontrollera hello **acceptera alla servercertifikat** alternativet.</span><span class="sxs-lookup"><span data-stu-id="c80a0-409">Check hello **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="c80a0-410">I hello **databasfrågan** redigera textområde, klistra in vilka extrakt hello nödvändigt databasfält (inklusive eventuella beräknade fält, till exempel hello etiketter) och ned exempel hello data toohello önskad provtagning hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="c80a0-410">In hello **Database query** edit text area, paste hello query which extracts hello necessary database fields (including any computed fields such as hello labels) and down samples hello data toohello desired sample size.</span></span>

<span data-ttu-id="c80a0-411">Ett exempel på en binär klassificering experiment läsning av data direkt från hello SQL Data Warehouse-databas är i hello figuren nedan (Kom ihåg tooreplace hello namnen nyctaxi_trip och nyctaxi_fare av hello schemat namn och hello tabellnamn som du använde i din genomgång).</span><span class="sxs-lookup"><span data-stu-id="c80a0-411">An example of a binary classification experiment reading data directly from hello SQL Data Warehouse database is in hello figure below (remember tooreplace hello table names nyctaxi_trip and nyctaxi_fare by hello schema name and hello table names you used in your walkthrough).</span></span> <span data-ttu-id="c80a0-412">Liknande experiment kan konstrueras för multiklass-baserad klassificering och regressionsproblem.</span><span class="sxs-lookup"><span data-stu-id="c80a0-412">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Azure ML Train][10]

> [!IMPORTANT]
> <span data-ttu-id="c80a0-414">I hello modellering extrahering av data och provtagning frågan exempel finns i föregående avsnitt **alla etiketter för hello tre modellering övningarna tas med i hello fråga**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-414">In hello modeling data extraction and sampling query examples provided in previous sections, **all labels for hello three modeling exercises are included in hello query**.</span></span> <span data-ttu-id="c80a0-415">Är ett viktigt steg i (obligatoriskt) i varje hello modeling övningarna för**undanta** hello onödiga etiketter för hello andra två problem och andra **mål minnesläckor**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-415">An important (required) step in each of hello modeling exercises is too**exclude** hello unnecessary labels for hello other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="c80a0-416">Till exempel när du använder binär klassificering, använda hello etikett **lutad** och utelämna hello fält **tips\_klassen**, **tips\_belopp**, och **totala\_belopp**.</span><span class="sxs-lookup"><span data-stu-id="c80a0-416">For example, when using binary classification, use hello label **tipped** and exclude hello fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="c80a0-417">hello senare är målet minnesläckor eftersom de innebär hello tips betalda.</span><span class="sxs-lookup"><span data-stu-id="c80a0-417">hello latter are target leaks since they imply hello tip paid.</span></span>
> 
> <span data-ttu-id="c80a0-418">tooexclude alla onödiga kolumner eller mål minnesläckor du kan använda hello [Välj kolumner i datauppsättning] [ select-columns] modul eller hello [redigera Metadata] [ edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="c80a0-418">tooexclude any unnecessary columns or target leaks, you may use hello [Select Columns in Dataset][select-columns] module or hello [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="c80a0-419">Mer information finns i [Välj kolumner i datauppsättning] [ select-columns] och [redigera Metadata] [ edit-metadata] referera sidor.</span><span class="sxs-lookup"><span data-stu-id="c80a0-419">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="c80a0-420"><a name="mldeploy"></a>Distribuera modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c80a0-420"><a name="mldeploy"></a>Deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="c80a0-421">När modellen är klar, kan du enkelt distribuera det som en webbtjänst direkt från hello experiment.</span><span class="sxs-lookup"><span data-stu-id="c80a0-421">When your model is ready, you can easily deploy it as a web service directly from hello experiment.</span></span> <span data-ttu-id="c80a0-422">Mer information om hur du distribuerar Azure ML web services finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="c80a0-422">For more information about deploying Azure ML web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="c80a0-423">toodeploy en ny webbtjänst, måste du:</span><span class="sxs-lookup"><span data-stu-id="c80a0-423">toodeploy a new web service, you need to:</span></span>

1. <span data-ttu-id="c80a0-424">Skapa ett bedömningsprofil experiment.</span><span class="sxs-lookup"><span data-stu-id="c80a0-424">Create a scoring experiment.</span></span>
2. <span data-ttu-id="c80a0-425">Distribuera hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="c80a0-425">Deploy hello web service.</span></span>

<span data-ttu-id="c80a0-426">toocreate poängsättning av ett experiment från en **avslutad** utbildning experiment, klickar du på **skapa BEDÖMNINGEN EXPERIMENTERA** i hello lägre Åtgärdsfältet.</span><span class="sxs-lookup"><span data-stu-id="c80a0-426">toocreate a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in hello lower action bar.</span></span>

![Azure bedömningen][18]

<span data-ttu-id="c80a0-428">Azure Machine Learning försöker toocreate ett bedömningsprofil experiment baserat på hello komponenter i hello träningsexperiment.</span><span class="sxs-lookup"><span data-stu-id="c80a0-428">Azure Machine Learning will attempt toocreate a scoring experiment based on hello components of hello training experiment.</span></span> <span data-ttu-id="c80a0-429">I synnerhet att:</span><span class="sxs-lookup"><span data-stu-id="c80a0-429">In particular, it will:</span></span>

1. <span data-ttu-id="c80a0-430">Spara hello tränad modell och ta bort hello modellen utbildningsmoduler.</span><span class="sxs-lookup"><span data-stu-id="c80a0-430">Save hello trained model and remove hello model training modules.</span></span>
2. <span data-ttu-id="c80a0-431">Identifiera en logisk **inkommande port** toorepresent hello förväntades indata schemat.</span><span class="sxs-lookup"><span data-stu-id="c80a0-431">Identify a logical **input port** toorepresent hello expected input data schema.</span></span>
3. <span data-ttu-id="c80a0-432">Identifiera en logisk **utgående port** toorepresent hello förväntade web service utdataschema.</span><span class="sxs-lookup"><span data-stu-id="c80a0-432">Identify a logical **output port** toorepresent hello expected web service output schema.</span></span>

<span data-ttu-id="c80a0-433">När hello bedömningen experiment skapas, granska den och göra justera efter behov.</span><span class="sxs-lookup"><span data-stu-id="c80a0-433">When hello scoring experiment is created, review it and make adjust as needed.</span></span> <span data-ttu-id="c80a0-434">En typisk justering är tooreplace hello inkommande dataset och/eller fråga med någon vilket utesluter etikett fält, eftersom dessa inte är tillgänglig när tjänsten hello kallas.</span><span class="sxs-lookup"><span data-stu-id="c80a0-434">A typical adjustment is tooreplace hello input dataset and/or query with one which excludes label fields, as these will not be available when hello service is called.</span></span> <span data-ttu-id="c80a0-435">Det är också en bra idé tooreduce hello storlek på hello indata dataset-och/eller tooa några poster, bara tillräckligt med tooindicate hello ingående schema.</span><span class="sxs-lookup"><span data-stu-id="c80a0-435">It is also a good practice tooreduce hello size of hello input dataset and/or query tooa few records, just enough tooindicate hello input schema.</span></span> <span data-ttu-id="c80a0-436">För hello utdataporten den gemensamma tooexclude alla inmatningsfält och bara innehålla hello **poängsatta etiketter** och **bedömas sannolikhet** i hello utdata med hello [Välj kolumner i datauppsättning ] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="c80a0-436">For hello output port, it is common tooexclude all input fields and only include hello **Scored Labels** and **Scored Probabilities** in hello output using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="c80a0-437">Ett exempel bedömningen experiment finns i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="c80a0-437">A sample scoring experiment is provided in hello figure below.</span></span> <span data-ttu-id="c80a0-438">När klar toodeploy, klicka på hello **publicera WEBBTJÄNSTEN** knapp i hello lägre Åtgärdsfältet.</span><span class="sxs-lookup"><span data-stu-id="c80a0-438">When ready toodeploy, click hello **PUBLISH WEB SERVICE** button in hello lower action bar.</span></span>

![Azure ML publicera][11]

## <a name="summary"></a><span data-ttu-id="c80a0-440">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c80a0-440">Summary</span></span>
<span data-ttu-id="c80a0-441">toorecap vad vi har gjort i den här genomgången självstudiekursen, du har skapat ett Azure datavetenskap miljö, arbetat med en stor offentliga datauppsättning tar genom hello Team vetenskap av data, alla hello sätt från data förvärv toomodel utbildning och sedan toohello distribution av en Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="c80a0-441">toorecap what we have done in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset, taking it through hello Team Data Science Process, all hello way from data acquisition toomodel training, and then toohello deployment of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="c80a0-442">Licensinformationen</span><span class="sxs-lookup"><span data-stu-id="c80a0-442">License information</span></span>
<span data-ttu-id="c80a0-443">Den här genomgången exempel och dess tillhörande skript och IPython notebook(s) delas av Microsoft under hello MIT-licens.</span><span class="sxs-lookup"><span data-stu-id="c80a0-443">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="c80a0-444">Kontrollera filen LICENSE.txt för hello i hello directory hello exempelkoden på GitHub för mer information.</span><span class="sxs-lookup"><span data-stu-id="c80a0-444">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="c80a0-445">Referenser</span><span class="sxs-lookup"><span data-stu-id="c80a0-445">References</span></span>
<span data-ttu-id="c80a0-446">• [Andrés Monroy NYC Taxi resor hämtningssidan](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="c80a0-446">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="c80a0-447">• [FOILing NYC Taxitransport resa Data av Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="c80a0-447">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="c80a0-448">• [NYC Taxi och Limousine kommissionens forskning och statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="c80a0-448">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
