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
# <a name="hello-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a><span data-ttu-id="52ae6-103">hello Team av vetenskapliga data i praktiken: Använd Azure HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="52ae6-103">hello Team Data Science Process in action: Use Azure HDInsight Hadoop clusters</span></span>
<span data-ttu-id="52ae6-104">I den här genomgången ska vi använda hello [Team Data vetenskap processen (TDSP)](data-science-process-overview.md) i ett scenario för slutpunkt till slutpunkt med hjälp av en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) toostore, utforska och funktion tekniker data från hello offentligt tillgängliga [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset och toodown exempeldata hello.</span><span class="sxs-lookup"><span data-stu-id="52ae6-104">In this walkthrough, we use hello [Team Data Science Process (TDSP)](data-science-process-overview.md) in an end-to-end scenario using an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore and feature engineer data from hello publicly available [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset, and toodown sample hello data.</span></span> <span data-ttu-id="52ae6-105">Modeller av hello data skapas med Azure Machine Learning toohandle binära och multiklass-baserad klassificering och regression förutsägande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="52ae6-105">Models of hello data are built with Azure Machine Learning toohandle binary and multiclass classification and regression predictive tasks.</span></span>

<span data-ttu-id="52ae6-106">En genomgång som visar hur toohandle en större datamängd (1 terabyte) liknande scenarier med HDInsight Hadoop-kluster för databearbetning finns [Team datavetenskap Process, med hjälp av Azure HDInsight Hadoop-kluster på 1 TB-dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="52ae6-106">For a walkthrough that shows how toohandle a larger (1 terabyte) dataset for a similar scenario using HDInsight Hadoop clusters for data processing, see [Team Data Science Process - Using Azure HDInsight Hadoop Clusters on a 1 TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).</span></span>

<span data-ttu-id="52ae6-107">Det är också möjligt toouse en IPython anteckningsboken tooaccomplish hello uppgifter presenterades hello genomgången använder hello 1 TB dataset.</span><span class="sxs-lookup"><span data-stu-id="52ae6-107">It is also possible toouse an IPython notebook tooaccomplish hello tasks presented hello walkthrough using hello 1 TB dataset.</span></span> <span data-ttu-id="52ae6-108">Användare som vill som den här metoden bör kontakta tootry hello [Criteo genomgången använder en Hive ODBC-anslutning](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-108">Users who would like tootry this approach should consult hello [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="52ae6-109"><a name="dataset"></a>NYC Taxi resor Dataset-beskrivning</span><span class="sxs-lookup"><span data-stu-id="52ae6-109"><a name="dataset"></a>NYC Taxi Trips Dataset description</span></span>
<span data-ttu-id="52ae6-110">hello NYC Taxi resa data är cirka 20GB komprimerad fil med kommaavgränsade värden (CSV)-filer (~ 48GB okomprimerade), som består av fler än 173 miljoner enskilda resor och hello priser för varje resa.</span><span class="sxs-lookup"><span data-stu-id="52ae6-110">hello NYC Taxi Trip data is about 20GB of compressed comma-separated values (CSV) files (~48GB uncompressed), comprising more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="52ae6-111">Hello hämtning och Samlingsbibliotek plats och tid, anonymiserade hackare (drivrutin) licensnummer och medallion (taxi's unikt id) nummer innehåller varje resa-post.</span><span class="sxs-lookup"><span data-stu-id="52ae6-111">Each trip record includes hello pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="52ae6-112">hello data omfattar alla resor hello år 2013 och finns i följande två datamängder för varje månad hello:</span><span class="sxs-lookup"><span data-stu-id="52ae6-112">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="52ae6-113">hello 'trip_data' CSV-filer innehåller resa information, till exempel antal passagerare, hämtning och dropoff punkter, resa varaktighet och resa längd.</span><span class="sxs-lookup"><span data-stu-id="52ae6-113">hello 'trip_data' CSV files contain trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="52ae6-114">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="52ae6-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="52ae6-115">hello 'trip_fare' CSV-filer innehåller information om hello avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar och hello totalbelopp betald.</span><span class="sxs-lookup"><span data-stu-id="52ae6-115">hello 'trip_fare' CSV files contain details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="52ae6-116">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="52ae6-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="52ae6-117">hello Unik nyckel toojoin resa\_data och resa\_avgiften består av hello fält: medallion hackare\_licensen och hämtning\_datetime.</span><span class="sxs-lookup"><span data-stu-id="52ae6-117">hello unique key toojoin trip\_data and trip\_fare is composed of hello fields: medallion, hack\_licence and pickup\_datetime.</span></span>

<span data-ttu-id="52ae6-118">alla tooget hello information relevanta tooa viss resan, den är tillräckligt toojoin med tre nycklar: hello ”medallion” ”, hacka\_licens” och ”hämtning\_datetime”.</span><span class="sxs-lookup"><span data-stu-id="52ae6-118">tooget all of hello details relevant tooa particular trip, it is sufficient toojoin with three keys: hello "medallion", "hack\_license" and "pickup\_datetime".</span></span>

<span data-ttu-id="52ae6-119">Vi beskriver vissa mer information om hello data när vi lagrar dem i Hive-tabeller inom kort.</span><span class="sxs-lookup"><span data-stu-id="52ae6-119">We describe some more details of hello data when we store them into Hive tables shortly.</span></span>

## <span data-ttu-id="52ae6-120"><a name="mltasks"></a>Exempel på förutsägelse uppgifter</span><span class="sxs-lookup"><span data-stu-id="52ae6-120"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="52ae6-121">När närmar sig data kan fastställa hello slags förutsägelser som du vill toomake baserat på dess analysis tydliggöra hello uppgifter du behöver tooinclude i processen.</span><span class="sxs-lookup"><span data-stu-id="52ae6-121">When approaching data, determining hello kind of predictions you want toomake based on its analysis helps clarify hello tasks that you will need tooinclude in your process.</span></span>
<span data-ttu-id="52ae6-122">Här följer tre exempel på förutsägelse problem som vi adressen i den här genomgången vars formulering baseras på hello *tips\_belopp*:</span><span class="sxs-lookup"><span data-stu-id="52ae6-122">Here are three examples of prediction problems that we address in this walkthrough whose formulation is based on hello *tip\_amount*:</span></span>

1. <span data-ttu-id="52ae6-123">**Binär klassificering**: förutsäga oavsett betalats ett tips för en resa i d.v.s. en *tips\_belopp* som är större än 0 är ett positivt exempel, när en *tips\_belopp* $0 är ett exempel på negativt.</span><span class="sxs-lookup"><span data-stu-id="52ae6-123">**Binary classification**: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0
2. <span data-ttu-id="52ae6-124">**Multiklass-baserad klassificering**: toopredict hello mängd tips belopp betalat för hello resa.</span><span class="sxs-lookup"><span data-stu-id="52ae6-124">**Multiclass classification**: toopredict hello range of tip amounts paid for hello trip.</span></span> <span data-ttu-id="52ae6-125">Vi dela hello *tips\_belopp* i fem lagerplatser eller klasser:</span><span class="sxs-lookup"><span data-stu-id="52ae6-125">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="52ae6-126">**Regression uppgiften**: toopredict hello hello tips betalats för en transport.</span><span class="sxs-lookup"><span data-stu-id="52ae6-126">**Regression task**: toopredict hello amount of hello tip paid for a trip.</span></span>  

## <span data-ttu-id="52ae6-127"><a name="setup"></a>Konfigurera en HDInsight Hadoop-kluster för avancerade analyser</span><span class="sxs-lookup"><span data-stu-id="52ae6-127"><a name="setup"></a>Set up an HDInsight Hadoop cluster for advanced analytics</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-128">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-128">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-129">Du kan konfigurera en Azure-miljö för avancerade analyser som använder ett HDInsight-kluster i tre steg:</span><span class="sxs-lookup"><span data-stu-id="52ae6-129">You can set up an Azure environment for advanced analytics that employs an HDInsight cluster in three steps:</span></span>

1. <span data-ttu-id="52ae6-130">[Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md): det här lagringskontot används för att lagra data i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="52ae6-130">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used for storing data in Azure Blob Storage.</span></span> <span data-ttu-id="52ae6-131">hello-data som används i HDInsight-kluster finns också här.</span><span class="sxs-lookup"><span data-stu-id="52ae6-131">hello data used in HDInsight clusters also resides here.</span></span>
2. <span data-ttu-id="52ae6-132">[Anpassa Azure HDInsight Hadoop-kluster för hello Advanced Analytics processen och tekniken](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="52ae6-132">[Customize Azure HDInsight Hadoop clusters for hello Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md).</span></span> <span data-ttu-id="52ae6-133">Det här steget skapar ett Azure HDInsight Hadoop-kluster med 64-bitars Anaconda Python 2.7 installerad på alla noder.</span><span class="sxs-lookup"><span data-stu-id="52ae6-133">This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="52ae6-134">Det finns två viktiga steg tooremember vid anpassning av ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-134">There are two important steps tooremember while customizing your HDInsight cluster.</span></span>
   
   * <span data-ttu-id="52ae6-135">Kom ihåg toolink hello storage-konto som skapades i steg 1 med ditt HDInsight-kluster när du skapar den.</span><span class="sxs-lookup"><span data-stu-id="52ae6-135">Remember toolink hello storage account created in step 1 with your HDInsight cluster when creating it.</span></span> <span data-ttu-id="52ae6-136">Det här lagringskontot är används tooaccess data som bearbetas i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-136">This storage account is used tooaccess data that is processed within hello cluster.</span></span>
   * <span data-ttu-id="52ae6-137">När hello klustret har skapats kan du aktivera fjärråtkomst toohello huvudnod hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="52ae6-137">After hello cluster is created, enable Remote Access toohello head node of hello cluster.</span></span> <span data-ttu-id="52ae6-138">Navigera toohello **Configuration** och klicka på **aktivera fjärråtkomst**.</span><span class="sxs-lookup"><span data-stu-id="52ae6-138">Navigate toohello **Configuration** tab and click **Enable Remote**.</span></span> <span data-ttu-id="52ae6-139">Det här steget anger hello autentiseringsuppgifter används för fjärrinloggning.</span><span class="sxs-lookup"><span data-stu-id="52ae6-139">This step specifies hello user credentials used for remote login.</span></span>
3. <span data-ttu-id="52ae6-140">[Skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md): den här Azure Machine Learning-arbetsytan har använt toobuild machine learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="52ae6-140">[Create an Azure Machine Learning workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used toobuild machine learning models.</span></span> <span data-ttu-id="52ae6-141">Den här uppgiften riktar sig när du har slutfört en första datagranskning och ned med hjälp av hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-141">This task is addressed after completing an initial data exploration and down sampling using hello HDInsight cluster.</span></span>

## <span data-ttu-id="52ae6-142"><a name="getdata"></a>Hämta hello data från en offentlig källa</span><span class="sxs-lookup"><span data-stu-id="52ae6-142"><a name="getdata"></a>Get hello data from a public source</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-143">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-143">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-144">tooget hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset från dess offentlig plats, kan du använda någon av hello-metoder som beskrivs i [tooand flytta Data från Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour datorn.</span><span class="sxs-lookup"><span data-stu-id="52ae6-144">tooget hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of hello methods described in [Move Data tooand from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour machine.</span></span>

<span data-ttu-id="52ae6-145">Här beskrivs hur använda AzCopy tootransfer hello filer som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="52ae6-145">Here we describe how use AzCopy tootransfer hello files containing data.</span></span> <span data-ttu-id="52ae6-146">toodownload och installera AzCopy följer hello instruktionerna på [komma igång med kommandoradsverktyget Azcopy hello](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="52ae6-146">toodownload and install AzCopy follow hello instructions at [Getting Started with hello AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

1. <span data-ttu-id="52ae6-147">Från Kommandotolken, utfärda hello följande AzCopy kommandona, ersätter *< path_to_data_folder >* med hello önskat mål:</span><span class="sxs-lookup"><span data-stu-id="52ae6-147">From a Command Prompt window, issue hello following AzCopy commands, replacing *<path_to_data_folder>* with hello desired destination:</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. <span data-ttu-id="52ae6-148">När hello kopia är klar finns totalt av 24 komprimerade filer i hello-datamappen valt.</span><span class="sxs-lookup"><span data-stu-id="52ae6-148">When hello copy completes, a total of 24 zipped files are in hello data folder chosen.</span></span> <span data-ttu-id="52ae6-149">Packa upp hello hämtade filer toohello samma katalog på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="52ae6-149">Unzip hello downloaded files toohello same directory on your local machine.</span></span> <span data-ttu-id="52ae6-150">Anteckna hello mappen där hello okomprimerade filerna finns.</span><span class="sxs-lookup"><span data-stu-id="52ae6-150">Make a note of hello folder where hello uncompressed files reside.</span></span> <span data-ttu-id="52ae6-151">Den här mappen blir refererad tooas hello *< sökväg\_till\_unzipped_data\_filer\>*  är det som följer.</span><span class="sxs-lookup"><span data-stu-id="52ae6-151">This folder will be referred tooas hello *<path\_to\_unzipped_data\_files\>* is what follows.</span></span>

## <span data-ttu-id="52ae6-152"><a name="upload"></a>Ladda upp data hello toohello standardbehållaren för Azure HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="52ae6-152"><a name="upload"></a>Upload hello data toohello default container of Azure HDInsight Hadoop cluster</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-153">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-153">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-154">Följande kommandon för AzCopy och Ersätt i hello hello följande parametrar med hello faktiska värden som du angav när du skapar hello Hadoop-kluster och packa upp hello datafiler.</span><span class="sxs-lookup"><span data-stu-id="52ae6-154">In hello following AzCopy commands, replace hello following parameters with hello actual values that you specified when creating hello Hadoop cluster and unzipping hello data files.</span></span>

* <span data-ttu-id="52ae6-155">***&#60; path_to_data_folder >*** hello katalog (tillsammans med sökväg) på datorn som innehåller hello uppackade datafiler</span><span class="sxs-lookup"><span data-stu-id="52ae6-155">***&#60;path_to_data_folder>*** hello directory (along with path) on your machine that contain hello unzipped data files</span></span>  
* <span data-ttu-id="52ae6-156">***&#60; lagringskontonamnet av Hadoop-kluster >*** hello storage-konto som är kopplad till ditt HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="52ae6-156">***&#60;storage account name of Hadoop cluster>*** hello storage account associated with your HDInsight cluster</span></span>
* <span data-ttu-id="52ae6-157">***&#60; standardbehållaren för Hadoop-kluster >*** hello standardbehållaren som används av klustret.</span><span class="sxs-lookup"><span data-stu-id="52ae6-157">***&#60;default container of Hadoop cluster>*** hello default container used by your cluster.</span></span> <span data-ttu-id="52ae6-158">Observera att hello namn hello default-behållaren är vanligtvis hello samma namn som själva hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="52ae6-158">Note that hello name of hello default container is usually hello same name as hello cluster itself.</span></span> <span data-ttu-id="52ae6-159">Om hello kluster kallas ”abc123.azurehdinsight.net”, är hello standardbehållaren abc123.</span><span class="sxs-lookup"><span data-stu-id="52ae6-159">For example, if hello cluster is called "abc123.azurehdinsight.net", hello default container is abc123.</span></span>
* <span data-ttu-id="52ae6-160">***&#60; lagringskontonyckel >*** hello nyckel för hello storage-konto som används av klustret</span><span class="sxs-lookup"><span data-stu-id="52ae6-160">***&#60;storage account key>*** hello key for hello storage account used by your cluster</span></span>

<span data-ttu-id="52ae6-161">Kör hello följande två kommandon för AzCopy från en kommandotolk eller ett Windows PowerShell-fönster på datorn.</span><span class="sxs-lookup"><span data-stu-id="52ae6-161">From a Command Prompt or a Windows PowerShell window in your machine, run hello following two AzCopy commands.</span></span>

<span data-ttu-id="52ae6-162">Det här kommandot överför hello resa data för***nyctaxitripraw*** katalog i hello standardbehållaren för hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-162">This command uploads hello trip data too***nyctaxitripraw*** directory in hello default container of hello Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

<span data-ttu-id="52ae6-163">Det här kommandot överför hello avgiften data för***nyctaxifareraw*** katalog i hello standardbehållaren för hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-163">This command uploads hello fare data too***nyctaxifareraw*** directory in hello default container of hello Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

<span data-ttu-id="52ae6-164">hello data bör nu i Azure Blob Storage och redo toobe förbrukas inom hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-164">hello data should now in Azure Blob Storage and ready toobe consumed within hello HDInsight cluster.</span></span>

## <span data-ttu-id="52ae6-165"><a name="#download-hql-files"></a>Logga in på hello huvudnod i Hadoop-kluster och och förbereda för undersökande dataanalys</span><span class="sxs-lookup"><span data-stu-id="52ae6-165"><a name="#download-hql-files"></a>Log into hello head node of Hadoop cluster and and prepare for exploratory data analysis</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-166">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-166">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-167">tooaccess hello huvudnod hello klustret för undersökande dataanalys och ned hello dataurval, följ hello proceduren som beskrivs i [åtkomst hello Head-nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="52ae6-167">tooaccess hello head node of hello cluster for exploratory data analysis and down sampling of hello data, follow hello procedure outlined in [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

<span data-ttu-id="52ae6-168">I den här genomgången ska vi i första hand använder frågor som skrivits i [Hive](https://hive.apache.org/), ett frågespråk för SQL-liknande, tooperform preliminära data explorations.</span><span class="sxs-lookup"><span data-stu-id="52ae6-168">In this walkthrough, we primarily use queries written in [Hive](https://hive.apache.org/), a SQL-like query language, tooperform preliminary data explorations.</span></span> <span data-ttu-id="52ae6-169">hello Hive-frågor som lagras i .hql filer.</span><span class="sxs-lookup"><span data-stu-id="52ae6-169">hello Hive queries are stored in .hql files.</span></span> <span data-ttu-id="52ae6-170">Vi sedan exempel ned på den här toobe för data som används i Azure Machine Learning för att skapa modeller.</span><span class="sxs-lookup"><span data-stu-id="52ae6-170">We then down sample this data toobe used within Azure Machine Learning for building models.</span></span>

<span data-ttu-id="52ae6-171">tooprepare hello kluster för undersökande dataanalys, vi hämtar hello .hql filer som innehåller hello relevanta Hive-skript från [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa lokal katalog (C:\temp) på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="52ae6-171">tooprepare hello cluster for exploratory data analysis, we download hello .hql files containing hello relevant Hive scripts from [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa local directory (C:\temp) on hello head node.</span></span> <span data-ttu-id="52ae6-172">toodo detta, öppna hello **kommandotolk** inifrån hello huvudnod hello klustret och problemet hello följande två kommandon:</span><span class="sxs-lookup"><span data-stu-id="52ae6-172">toodo this, open hello **Command Prompt** from within hello head node of hello cluster and issue hello following two commands:</span></span>

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

<span data-ttu-id="52ae6-173">Dessa två kommandon hämtas alla .hql filer som behövs i den här genomgången toohello lokal katalog ***C:\temp &#92;*** i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="52ae6-173">These two commands will download all .hql files needed in this walkthrough toohello local directory ***C:\temp&#92;*** in hello head node.</span></span>

## <span data-ttu-id="52ae6-174"><a name="#hive-db-tables"></a>Skapa Hive-databasen och tabeller som är partitionerad per månad</span><span class="sxs-lookup"><span data-stu-id="52ae6-174"><a name="#hive-db-tables"></a>Create Hive database and tables partitioned by month</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-175">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-175">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-176">Vi är nu redo toocreate Hive-tabeller för våra NYC taxi dataset.</span><span class="sxs-lookup"><span data-stu-id="52ae6-176">We are now ready toocreate Hive tables for our NYC taxi dataset.</span></span>
<span data-ttu-id="52ae6-177">Öppna hello i hello huvudnod hello Hadoop-kluster, ***Hadoop kommandoraden*** på hello skrivbord hello huvudnod och ange hello Hive katalogen genom att ange hello kommando</span><span class="sxs-lookup"><span data-stu-id="52ae6-177">In hello head node of hello Hadoop cluster, open hello ***Hadoop Command Line*** on hello desktop of hello head node, and enter hello Hive directory by entering hello command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="52ae6-178">**Kör alla Hive-kommandon i den här genomgången från hello ovan Hive bin / directory-fråga. Detta hand tar om eventuella problem med sökväg automatiskt. Vi använder hello termer ”Hive directory prompt” ”, Hive bin / directory prompt”, och ”Hadoop kommandoraden” synonymt i den här genomgången.**</span><span class="sxs-lookup"><span data-stu-id="52ae6-178">**Run all Hive commands in this walkthrough from hello above Hive bin/ directory prompt. This will take care of any path issues automatically. We use hello terms "Hive directory prompt", "Hive bin/ directory prompt",  and "Hadoop Command Line" interchangeably in this walkthrough.**</span></span>
> 
> 

<span data-ttu-id="52ae6-179">Ange följande kommando i Hadoop kommandoraden för hello huvudnod toosubmit hello Hive-fråga toocreate Hive-databasen och tabeller hello hello Hive directory prompten:</span><span class="sxs-lookup"><span data-stu-id="52ae6-179">From hello Hive directory prompt, enter hello following command in Hadoop Command Line of hello head node toosubmit hello Hive query toocreate Hive database and tables:</span></span>

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

<span data-ttu-id="52ae6-180">Här är hello innehållet i hello ***C:\temp\sample\_hive\_skapa\_db\_och\_tables.hql*** -fil som skapar Hive databas ***nyctaxidb *** och tabeller ***resa*** och ***avgiften***.</span><span class="sxs-lookup"><span data-stu-id="52ae6-180">Here is hello content of hello ***C:\temp\sample\_hive\_create\_db\_and\_tables.hql*** file which creates Hive database ***nyctaxidb*** and tables ***trip*** and ***fare***.</span></span>

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

<span data-ttu-id="52ae6-181">Den här Hive-skript skapar två tabeller:</span><span class="sxs-lookup"><span data-stu-id="52ae6-181">This Hive script creates two tables:</span></span>

* <span data-ttu-id="52ae6-182">Hej ”resa” tabellen innehåller resa information om varje resa (drivrutinsinformation, pickup tid, resa avstånd och gånger)</span><span class="sxs-lookup"><span data-stu-id="52ae6-182">hello "trip" table contains trip details of each ride (driver details, pickup time, trip distance and times)</span></span>
* <span data-ttu-id="52ae6-183">Hej ”avgiften” tabell innehåller information om avgiften (avgiften, tips belopp, vägtullar och tillägg).</span><span class="sxs-lookup"><span data-stu-id="52ae6-183">hello "fare" table contains fare details (fare amount, tip amount, tolls and surcharges).</span></span>

<span data-ttu-id="52ae6-184">Om du behöver ytterligare hjälp med dessa procedurer eller vill tooinvestigate alternativa de avsnittet hello [skicka Hive-frågor direkt från hello Hadoop kommandoraden ](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="52ae6-184">If you need any additional assistance with these procedures or want tooinvestigate alternative ones, see hello section [Submit Hive queries directly from hello Hadoop Command Line ](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="52ae6-185"><a name="#load-data"></a>Läsa in tooHive datatabeller av partitioner</span><span class="sxs-lookup"><span data-stu-id="52ae6-185"><a name="#load-data"></a>Load Data tooHive tables by partitions</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-186">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-186">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-187">hello NYC taxi datamängden har en naturlig partitionering per månad som vi använder tooenable bearbetningen och frågeprestanda snabbare.</span><span class="sxs-lookup"><span data-stu-id="52ae6-187">hello NYC taxi dataset has a natural partitioning by month, which we use tooenable faster processing and query times.</span></span> <span data-ttu-id="52ae6-188">Hej PowerShell-kommandona nedan (utfärdats från hello Hive-katalogen med hjälp av hello **Hadoop kommandoraden**) läsa in data toohello ”resa” och ”avgiften” Hive-tabeller partitionerad per månad.</span><span class="sxs-lookup"><span data-stu-id="52ae6-188">hello PowerShell commands below (issued from hello Hive directory using hello **Hadoop Command Line**) load data toohello "trip" and "fare" Hive tables partitioned by month.</span></span>

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

<span data-ttu-id="52ae6-189">Hej *exempel\_hive\_ladda\_data\_av\_partitions.hql* filen innehåller följande hello **LADDA** kommandon.</span><span class="sxs-lookup"><span data-stu-id="52ae6-189">hello *sample\_hive\_load\_data\_by\_partitions.hql* file contains hello following **LOAD** commands.</span></span>

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

<span data-ttu-id="52ae6-190">Observera att ett antal Hive-frågor som vi använder här hello utforskning pågående innebär att bara en enda partition eller på bara några partitioner.</span><span class="sxs-lookup"><span data-stu-id="52ae6-190">Note that a number of Hive queries we use here in hello exploration process involve looking at just a single partition or at only a couple of partitions.</span></span> <span data-ttu-id="52ae6-191">Men dessa frågor kan köras över hela hello-data.</span><span class="sxs-lookup"><span data-stu-id="52ae6-191">But these queries could be run across hello entire data.</span></span>

### <span data-ttu-id="52ae6-192"><a name="#show-db"></a>Visa databaser i hello HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="52ae6-192"><a name="#show-db"></a>Show databases in hello HDInsight Hadoop cluster</span></span>
<span data-ttu-id="52ae6-193">tooshow hello databaser som skapats i HDInsight Hadoop-kluster i hello Hadoop kommandoradsfönster, kör följande kommando i Hadoop kommandoraden hello:</span><span class="sxs-lookup"><span data-stu-id="52ae6-193">tooshow hello databases created in HDInsight Hadoop cluster inside hello Hadoop Command Line window, run hello following command in Hadoop Command Line:</span></span>

    hive -e "show databases;"

### <span data-ttu-id="52ae6-194"><a name="#show-tables"></a>Visa hello Hive-tabeller i hello nyctaxidb databas</span><span class="sxs-lookup"><span data-stu-id="52ae6-194"><a name="#show-tables"></a>Show hello Hive tables in hello nyctaxidb database</span></span>
<span data-ttu-id="52ae6-195">tooshow hello tabeller i hello nyctaxidb databas, kör följande kommando i Hadoop kommandoraden hello:</span><span class="sxs-lookup"><span data-stu-id="52ae6-195">tooshow hello tables in hello nyctaxidb database, run hello following command in Hadoop Command Line:</span></span>

    hive -e "show tables in nyctaxidb;"

<span data-ttu-id="52ae6-196">Vi kan bekräfta att partitioneras hello tabeller genom att utfärda hello kommandot nedan:</span><span class="sxs-lookup"><span data-stu-id="52ae6-196">We can confirm that hello tables are partitioned by issuing hello command below:</span></span>

    hive -e "show partitions nyctaxidb.trip;"

<span data-ttu-id="52ae6-197">hello förväntad utdata visas nedan:</span><span class="sxs-lookup"><span data-stu-id="52ae6-197">hello expected output is shown below:</span></span>

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

<span data-ttu-id="52ae6-198">På liknande sätt kan vi Kontrollera hello avgiften tabellen är partitionerad genom att utfärda hello kommandot nedan:</span><span class="sxs-lookup"><span data-stu-id="52ae6-198">Similarly, we can ensure that hello fare table is partitioned by issuing hello command below:</span></span>

    hive -e "show partitions nyctaxidb.fare;"

<span data-ttu-id="52ae6-199">hello förväntad utdata visas nedan:</span><span class="sxs-lookup"><span data-stu-id="52ae6-199">hello expected output is shown below:</span></span>

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

## <span data-ttu-id="52ae6-200"><a name="#explore-hive"></a>Datagranskning och funktionen teknikerna i Hive</span><span class="sxs-lookup"><span data-stu-id="52ae6-200"><a name="#explore-hive"></a>Data exploration and feature engineering in Hive</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-201">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-201">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-202">Hej datagranskning och funktion tekniska uppgifter för hello data lästs in i hello Hive-tabeller kan utföras med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="52ae6-202">hello data exploration and feature engineering tasks for hello data loaded into hello Hive tables can be accomplished using Hive queries.</span></span> <span data-ttu-id="52ae6-203">Här följer exempel på sådana uppgifter att vi dig igenom i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="52ae6-203">Here are examples of such tasks that we walk you through in this section:</span></span>

* <span data-ttu-id="52ae6-204">Visa översta 10 hello-poster i båda tabellerna.</span><span class="sxs-lookup"><span data-stu-id="52ae6-204">View hello top 10 records in both tables.</span></span>
* <span data-ttu-id="52ae6-205">Utforska data distributioner av ett fåtal fält i olika tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-205">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="52ae6-206">Undersök data quality hello longitud och latitud fält.</span><span class="sxs-lookup"><span data-stu-id="52ae6-206">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="52ae6-207">Generera binära och multiklass-baserad klassificeringsetiketter baserat på hello **tips\_belopp**.</span><span class="sxs-lookup"><span data-stu-id="52ae6-207">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="52ae6-208">Generera funktioner genom att beräkna hello direkt kommunikation avstånd.</span><span class="sxs-lookup"><span data-stu-id="52ae6-208">Generate features by computing hello direct trip distances.</span></span>

### <a name="exploration-view-hello-top-10-records-in-table-trip"></a><span data-ttu-id="52ae6-209">Undersökning: Visa hello översta 10 poster i tabellen resa</span><span class="sxs-lookup"><span data-stu-id="52ae6-209">Exploration: View hello top 10 records in table trip</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-210">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-210">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-211">toosee vilka hello data ser ut, vi undersöka 10 poster från varje tabell.</span><span class="sxs-lookup"><span data-stu-id="52ae6-211">toosee what hello data looks like, we examine 10 records from each table.</span></span> <span data-ttu-id="52ae6-212">Kör hello separat följande två frågor från hello Hive directory efterfrågas i hello Hadoop kommandoraden konsolen tooinspect hello poster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-212">Run hello following two queries separately from hello Hive directory prompt in hello Hadoop Command Line console tooinspect hello records.</span></span>

<span data-ttu-id="52ae6-213">tooget hello översta 10 poster i hello tabell ”resa” från hello första månad:</span><span class="sxs-lookup"><span data-stu-id="52ae6-213">tooget hello top 10 records in hello table "trip" from hello first month:</span></span>

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

<span data-ttu-id="52ae6-214">tooget hello översta 10 poster i hello tabell ”avgiften” från hello första månad:</span><span class="sxs-lookup"><span data-stu-id="52ae6-214">tooget hello top 10 records in hello table "fare" from hello first month:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

<span data-ttu-id="52ae6-215">Det är ofta användbara toosave hello poster tooa fil för lämplig visning.</span><span class="sxs-lookup"><span data-stu-id="52ae6-215">It is often useful toosave hello records tooa file for convenient viewing.</span></span> <span data-ttu-id="52ae6-216">En mindre ändring toohello ovan frågan åstadkommer detta:</span><span class="sxs-lookup"><span data-stu-id="52ae6-216">A small change toohello above query accomplishes this:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-hello-number-of-records-in-each-of-hello-12-partitions"></a><span data-ttu-id="52ae6-217">Undersökning: Visa hello antal poster i varje hello 12 partitioner</span><span class="sxs-lookup"><span data-stu-id="52ae6-217">Exploration: View hello number of records in each of hello 12 partitions</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-218">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-218">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-219">Är hello hur hello antalet turer varierar under kalenderåret hello av intresse.</span><span class="sxs-lookup"><span data-stu-id="52ae6-219">Of interest is hello how hello number of trips varies during hello calendar year.</span></span> <span data-ttu-id="52ae6-220">Gruppera efter månad kan vi toosee hur den här distributionen av resor ser ut.</span><span class="sxs-lookup"><span data-stu-id="52ae6-220">Grouping by month allows us toosee what this distribution of trips looks like.</span></span>

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

<span data-ttu-id="52ae6-221">Detta ger oss hello utdata:</span><span class="sxs-lookup"><span data-stu-id="52ae6-221">This gives us hello output :</span></span>

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

<span data-ttu-id="52ae6-222">Här, hello första kolumnen är hello månad och hello är andra hello antalet turer för den månaden.</span><span class="sxs-lookup"><span data-stu-id="52ae6-222">Here, hello first column is hello month and hello second is hello number of trips for that month.</span></span>

<span data-ttu-id="52ae6-223">Vi kan även räkna hello Totalt antal poster i vår datauppsättning resa genom att utfärda hello följande kommando i Kommandotolken hello Hive directory.</span><span class="sxs-lookup"><span data-stu-id="52ae6-223">We can also count hello total number of records in our trip data set by issuing hello following command at hello Hive directory prompt.</span></span>

    hive -e "select count(*) from nyctaxidb.trip;"

<span data-ttu-id="52ae6-224">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="52ae6-224">This yields:</span></span>

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

<span data-ttu-id="52ae6-225">Med kommandon liknande toothose visas för hello resa datauppsättning, kan vi utfärda Hive-frågor från hello Hive directory prompten hello avgiften data toovalidate hello antal poster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-225">Using commands similar toothose shown for hello trip data set, we can issue Hive queries from hello Hive directory prompt for hello fare data set toovalidate hello number of records.</span></span>

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

<span data-ttu-id="52ae6-226">Detta ger oss hello utdata:</span><span class="sxs-lookup"><span data-stu-id="52ae6-226">This gives us hello output:</span></span>

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

<span data-ttu-id="52ae6-227">Observera att hello exakt samma antal turer månaden returneras för både datamängder.</span><span class="sxs-lookup"><span data-stu-id="52ae6-227">Note that hello exact same number of trips per month is returned for both data sets.</span></span> <span data-ttu-id="52ae6-228">Detta ger hello första verifiering som hello data har lästs in korrekt.</span><span class="sxs-lookup"><span data-stu-id="52ae6-228">This provides hello first validation that hello data has been loaded correctly.</span></span>

<span data-ttu-id="52ae6-229">Cyklisk hello Totalt antal poster i hello avgiften datauppsättning kan göras med hjälp av hello-kommandot nedan från hello Hive directory prompten:</span><span class="sxs-lookup"><span data-stu-id="52ae6-229">Counting hello total number of records in hello fare data set can be done using hello command below from hello Hive directory prompt:</span></span>

    hive -e "select count(*) from nyctaxidb.fare;"

<span data-ttu-id="52ae6-230">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="52ae6-230">This yields :</span></span>

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

<span data-ttu-id="52ae6-231">också är hello samma hello Totalt antal poster i båda tabellerna.</span><span class="sxs-lookup"><span data-stu-id="52ae6-231">hello total number of records in both tables is also hello same.</span></span> <span data-ttu-id="52ae6-232">Detta ger en andra verifiering som hello data har lästs in korrekt.</span><span class="sxs-lookup"><span data-stu-id="52ae6-232">This provides a second validation that hello data has been loaded correctly.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="52ae6-233">Undersökning: Resa distribution av medallion</span><span class="sxs-lookup"><span data-stu-id="52ae6-233">Exploration: Trip distribution by medallion</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-234">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-234">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-235">Det här exemplet identifierar hello medallion (taxi numbers) med mer än 100 resor inom en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="52ae6-235">This example identifies hello medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="52ae6-236">hello frågan fördelarna hello partitionerad tabellåtkomst eftersom den är villkorad av hello partition variabeln **månad**.</span><span class="sxs-lookup"><span data-stu-id="52ae6-236">hello query benefits from hello partitioned table access since it is conditioned by hello partition variable **month**.</span></span> <span data-ttu-id="52ae6-237">hello frågeresultatet skrivs tooa lokal fil queryoutput.tsv i `C:\temp` i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="52ae6-237">hello query results are written tooa local file queryoutput.tsv in `C:\temp` on hello head node.</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="52ae6-238">Här är hello innehållet i *exempel\_hive\_resa\_antal\_av\_medallion.hql* filen för inspektion.</span><span class="sxs-lookup"><span data-stu-id="52ae6-238">Here is hello content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="52ae6-239">Hej medallion i hello NYC taxi datamängd identifierar en unik CAB-fil.</span><span class="sxs-lookup"><span data-stu-id="52ae6-239">hello medallion in hello NYC taxi data set identifies a unique cab.</span></span> <span data-ttu-id="52ae6-240">Vi kan identifiera vilka CAB-filer är ”upptagna” genom att fråga vilka som gjorts mer än ett visst antal turer under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="52ae6-240">We can identify which cabs are "busy" by asking which ones made more than a certain number of trips in a particular time period.</span></span> <span data-ttu-id="52ae6-241">hello följande exempel identifierar CAB-filer som gjorts flera hundra resor i hello första tre månader och sparar hello resultat tooa lokala frågefilen, C:\temp\queryoutput.tsv.</span><span class="sxs-lookup"><span data-stu-id="52ae6-241">hello following example identifies cabs that made more than a hundred trips in hello first three months, and saves hello query results tooa local file, C:\temp\queryoutput.tsv.</span></span>

<span data-ttu-id="52ae6-242">Här är hello innehållet i *exempel\_hive\_resa\_antal\_av\_medallion.hql* filen för inspektion.</span><span class="sxs-lookup"><span data-stu-id="52ae6-242">Here is hello content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="52ae6-243">Från hello Hive directory prompten problemet hello kommandot nedan:</span><span class="sxs-lookup"><span data-stu-id="52ae6-243">From hello Hive directory prompt, issue hello command below :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="52ae6-244">Undersökning: Resa distribution av medallion och hack_license</span><span class="sxs-lookup"><span data-stu-id="52ae6-244">Exploration: Trip distribution by medallion and hack_license</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-245">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-245">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-246">När du utforskar en datamängd, vill vi ofta tooexamine hello antalet co-förekomster av grupper av värden.</span><span class="sxs-lookup"><span data-stu-id="52ae6-246">When exploring a dataset, we frequently want tooexamine hello number of co-occurences of groups of values.</span></span> <span data-ttu-id="52ae6-247">Det här avsnittet innehåller ett exempel på hur toodo för CAB-filer och drivrutiner.</span><span class="sxs-lookup"><span data-stu-id="52ae6-247">This section provide an example of how toodo this for cabs and drivers.</span></span>

<span data-ttu-id="52ae6-248">Hej *exempel\_hive\_resa\_antal\_av\_medallion\_license.hql* filgrupper hello avgiften data är inställd på ”medallion” och ”hack_license” och returnerar antalet varje kombination.</span><span class="sxs-lookup"><span data-stu-id="52ae6-248">hello *sample\_hive\_trip\_count\_by\_medallion\_license.hql* file groups hello fare data set on "medallion" and "hack_license" and returns counts of each combination.</span></span> <span data-ttu-id="52ae6-249">Nedan visas dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="52ae6-249">Below are its contents.</span></span>

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

<span data-ttu-id="52ae6-250">Den här frågan returnerar drivrutinen kombinationer sorterade efter fallande antalet turer och CAB-filen.</span><span class="sxs-lookup"><span data-stu-id="52ae6-250">This query returns cab and particular driver combinations ordered by descending number of trips.</span></span>

<span data-ttu-id="52ae6-251">Från hello Hive directory kommandotolk, kör:</span><span class="sxs-lookup"><span data-stu-id="52ae6-251">From hello Hive directory prompt, run :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="52ae6-252">hello frågeresultatet skrivs tooa lokal fil C:\temp\queryoutput.tsv.</span><span class="sxs-lookup"><span data-stu-id="52ae6-252">hello query results are written tooa local file C:\temp\queryoutput.tsv.</span></span>

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a><span data-ttu-id="52ae6-253">Undersökning: Utvärdera data quality genom att söka efter ogiltig longitud/latitud poster</span><span class="sxs-lookup"><span data-stu-id="52ae6-253">Exploration: Assessing data quality by checking for invalid longitude/latitude records</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-254">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-254">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-255">En gemensam undersökande dataanalys syftar tooweed ut ogiltiga eller felaktiga poster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-255">A common objective of exploratory data analysis is tooweed out invalid or bad records.</span></span> <span data-ttu-id="52ae6-256">hello anger exemplet i det här avsnittet om antingen hello longituden eller latituden fält innehåller ett värde långt utanför hello NYC område.</span><span class="sxs-lookup"><span data-stu-id="52ae6-256">hello example in this section determines whether either hello longitude or latitude fields contain a value far outside hello NYC area.</span></span> <span data-ttu-id="52ae6-257">Eftersom det är troligt att dessa poster har en felaktig longitud-latitudvärden vill vi tooeliminate dem från alla data som är toobe används för förutsägelsemodellering.</span><span class="sxs-lookup"><span data-stu-id="52ae6-257">Since it is likely that such records have an erroneous longitude-latitude values, we want tooeliminate them from any data that is toobe used for modeling.</span></span>

<span data-ttu-id="52ae6-258">Här är hello innehållet i *exempel\_hive\_kvalitet\_assessment.hql* filen för inspektion.</span><span class="sxs-lookup"><span data-stu-id="52ae6-258">Here is hello content of *sample\_hive\_quality\_assessment.hql* file for inspection.</span></span>

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


<span data-ttu-id="52ae6-259">Från hello Hive directory kommandotolk, kör:</span><span class="sxs-lookup"><span data-stu-id="52ae6-259">From hello Hive directory prompt, run :</span></span>

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

<span data-ttu-id="52ae6-260">Hej *-S* argumentet som ingår i det här kommandot förhindrar hello status skärmen utskrift av hello Hive kartan/minska jobb.</span><span class="sxs-lookup"><span data-stu-id="52ae6-260">hello *-S* argument included in this command suppresses hello status screen printout of hello Hive Map/Reduce jobs.</span></span> <span data-ttu-id="52ae6-261">Detta är användbart eftersom det gör hello skärmen utskrifts hello Hive mer lättläst frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-261">This is useful because it makes hello screen print of hello Hive query output more readable.</span></span>

### <a name="exploration-binary-class-distributions-of-trip-tips"></a><span data-ttu-id="52ae6-262">Undersökning: Binära klassen distributioner av resa tips</span><span class="sxs-lookup"><span data-stu-id="52ae6-262">Exploration: Binary class distributions of trip tips</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-263">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-263">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-264">För hello binära klassificeringsproblem beskrivs i hello [exempel på uppgifter som förutsägelse](machine-learning-data-science-process-hive-walkthrough.md#mltasks) avsnitt, är det användbart tooknow om ett tips gavs eller inte.</span><span class="sxs-lookup"><span data-stu-id="52ae6-264">For hello binary classification problem outlined in hello [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section, it is useful tooknow whether a tip was given or not.</span></span> <span data-ttu-id="52ae6-265">Den här distributionen av tips är binär:</span><span class="sxs-lookup"><span data-stu-id="52ae6-265">This distribution of tips is binary:</span></span>

* <span data-ttu-id="52ae6-266">Tips angivna (klass 1, tips\_belopp > 0)</span><span class="sxs-lookup"><span data-stu-id="52ae6-266">tip given(Class 1, tip\_amount > $0)</span></span>  
* <span data-ttu-id="52ae6-267">Inget tips (0-klassen, tips\_belopp = 0).</span><span class="sxs-lookup"><span data-stu-id="52ae6-267">no tip (Class 0, tip\_amount = $0).</span></span>

<span data-ttu-id="52ae6-268">Hej *exempel\_hive\_lutad\_frequencies.hql* -exempelfilen nedan gör detta.</span><span class="sxs-lookup"><span data-stu-id="52ae6-268">hello *sample\_hive\_tipped\_frequencies.hql* file shown below does this.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

<span data-ttu-id="52ae6-269">Från hello Hive directory kommandotolk, kör:</span><span class="sxs-lookup"><span data-stu-id="52ae6-269">From hello Hive directory prompt, run:</span></span>

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-hello-multiclass-setting"></a><span data-ttu-id="52ae6-270">Undersökning: Klassen distributioner i hello multiklass-baserad inställningen</span><span class="sxs-lookup"><span data-stu-id="52ae6-270">Exploration: Class distributions in hello multiclass setting</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-271">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-271">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-272">Hej multiklass-baserad klassificering problemet som beskrivs i hello [exempel på uppgifter som förutsägelse](machine-learning-data-science-process-hive-walkthrough.md#mltasks) avsnittet datamängden lämpar sig också tooa fysiska klassificering som toopredict hello mängden hello tips angivna.</span><span class="sxs-lookup"><span data-stu-id="52ae6-272">For hello multiclass classification problem outlined in hello [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section this data set also lends itself tooa natural classification where we would like toopredict hello amount of hello tips given.</span></span> <span data-ttu-id="52ae6-273">Vi kan använda lagerplatser toodefine tips adressintervall i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="52ae6-273">We can use bins toodefine tip ranges in hello query.</span></span> <span data-ttu-id="52ae6-274">tooget hello klassen distributioner för hello olika tips intervall använder vi hello *exempel\_hive\_tips\_intervallet\_frequencies.hql* fil.</span><span class="sxs-lookup"><span data-stu-id="52ae6-274">tooget hello class distributions for hello various tip ranges, we use hello *sample\_hive\_tip\_range\_frequencies.hql* file.</span></span> <span data-ttu-id="52ae6-275">Nedan visas dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="52ae6-275">Below are its contents.</span></span>

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

<span data-ttu-id="52ae6-276">Kör följande kommando från kommandoraden för Hadoop-konsolen hello:</span><span class="sxs-lookup"><span data-stu-id="52ae6-276">Run hello following command from Hadoop Command Line console:</span></span>

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a><span data-ttu-id="52ae6-277">Undersökning: Compute direkt avståndet mellan två longitud latitud-platser</span><span class="sxs-lookup"><span data-stu-id="52ae6-277">Exploration: Compute Direct Distance Between Two Longitude-Latitude Locations</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-278">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-278">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-279">Om du har ett mått på hello direkt avståndet kan oss toofind ut hello avvikelse mellan den och hello faktiska utlösas avstånd.</span><span class="sxs-lookup"><span data-stu-id="52ae6-279">Having a measure of hello direct distance allows us toofind out hello discrepancy between it and hello actual trip distance.</span></span> <span data-ttu-id="52ae6-280">Vi motivera funktionen genom att peka som en passagerare kan vara mindre troligt tootip om de ta reda på hello drivrutinen vidtagit dem avsiktligt av en mycket längre väg.</span><span class="sxs-lookup"><span data-stu-id="52ae6-280">We motivate this feature by pointing out that a passenger might be less likely tootip if they figure out that hello driver has intentionally taken them by a much longer route.</span></span>

<span data-ttu-id="52ae6-281">toosee hello jämförelse mellan faktiska resa avstånd och hello [Haversine avstånd](http://en.wikipedia.org/wiki/Haversine_formula) mellan två longitud latitud punkter (hello ”” storcirkelavståndet), vi använda hello trigonometrifunktioner som är tillgängliga i Hive, därför:</span><span class="sxs-lookup"><span data-stu-id="52ae6-281">toosee hello comparison between actual trip distance and hello [Haversine distance](http://en.wikipedia.org/wiki/Haversine_formula) between two longitude-latitude points (hello "great circle" distance), we use hello trigonometric functions available within Hive, thus :</span></span>

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

<span data-ttu-id="52ae6-282">R är hello radien för hello Earth i mil i hello frågan ovan, och pi är konverterade tooradians.</span><span class="sxs-lookup"><span data-stu-id="52ae6-282">In hello query above, R is hello radius of hello Earth in miles, and pi is converted tooradians.</span></span> <span data-ttu-id="52ae6-283">Observera att ”filtrerade” tooremove värden som är långt från hello NYC området hello longitud latitud punkter.</span><span class="sxs-lookup"><span data-stu-id="52ae6-283">Note that hello longitude-latitude points are "filtered" tooremove values that are far from hello NYC area.</span></span>

<span data-ttu-id="52ae6-284">I det här fallet skriva vi vårt resultat tooa katalog med namnet ”queryoutputdir”.</span><span class="sxs-lookup"><span data-stu-id="52ae6-284">In this case, we write our results tooa directory called "queryoutputdir".</span></span> <span data-ttu-id="52ae6-285">hello kommandosekvens nedan skapar den här målkatalogen först och kör sedan hello Hive-kommando.</span><span class="sxs-lookup"><span data-stu-id="52ae6-285">hello sequence of commands shown below first creates this output directory, and then runs hello Hive command.</span></span>

<span data-ttu-id="52ae6-286">Från hello Hive directory kommandotolk, kör:</span><span class="sxs-lookup"><span data-stu-id="52ae6-286">From hello Hive directory prompt, run:</span></span>

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


<span data-ttu-id="52ae6-287">hello frågeresultatet skrivs too9 Azure-blobbar ***queryoutputdir/000000\_0*** för ***queryoutputdir/000008\_0*** under hello standardbehållaren för hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-287">hello query results are written too9 Azure blobs ***queryoutputdir/000000\_0*** too ***queryoutputdir/000008\_0*** under hello default container of hello Hadoop cluster.</span></span>

<span data-ttu-id="52ae6-288">toosee hello storleken på hello enskilda blobbar, vi kör följande kommando från hello Hive directory prompten hello:</span><span class="sxs-lookup"><span data-stu-id="52ae6-288">toosee hello size of hello individual blobs, we run hello following command from hello Hive directory prompt :</span></span>

    hdfs dfs -ls wasb:///queryoutputdir

<span data-ttu-id="52ae6-289">toosee hello innehållet i en viss fil, säg 000000\_0, använder vi Hadoop's `copyToLocal` kommandot därför.</span><span class="sxs-lookup"><span data-stu-id="52ae6-289">toosee hello contents of a given file, say 000000\_0, we use Hadoop's `copyToLocal` command, thus.</span></span>

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> <span data-ttu-id="52ae6-290">`copyToLocal`rekommenderas inte för användning med dem och kan ta mycket lång tid för stora filer.</span><span class="sxs-lookup"><span data-stu-id="52ae6-290">`copyToLocal` can be very slow for large files, and is not recommended for use with them.</span></span>  
> 
> 

<span data-ttu-id="52ae6-291">En stor fördel med dessa data finns i en Azure blob är att vi kan utforska hello data i Azure Machine Learning med hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="52ae6-291">A key advantage of having this data reside in an Azure blob is that we may explore hello data within Azure Machine Learning using hello [Import Data][import-data] module.</span></span>

## <span data-ttu-id="52ae6-292"><a name="#downsample"></a>Ned exempel data och bygga modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="52ae6-292"><a name="#downsample"></a>Down sample data and build models in Azure Machine Learning</span></span>
> [!NOTE]
> <span data-ttu-id="52ae6-293">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-293">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="52ae6-294">Efter hello undersökande data analysfasen är vi nu redo toodown hello exempeldata för att skapa modeller i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="52ae6-294">After hello exploratory data analysis phase, we are now ready toodown sample hello data for building models in Azure Machine Learning.</span></span> <span data-ttu-id="52ae6-295">I det här avsnittet visar vi hur toouse registreringsdata fråga toodown hello exempeldata, som sedan öppnas från hello [importera Data] [ import-data] modul i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="52ae6-295">In this section, we show how toouse a Hive query toodown sample hello data, which is then accessed from hello [Import Data][import-data] module in Azure Machine Learning.</span></span>

### <a name="down-sampling-hello-data"></a><span data-ttu-id="52ae6-296">Ned hello datasampling</span><span class="sxs-lookup"><span data-stu-id="52ae6-296">Down sampling hello data</span></span>
<span data-ttu-id="52ae6-297">Det finns två steg i den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="52ae6-297">There are two steps in this procedure.</span></span> <span data-ttu-id="52ae6-298">Först vi ansluta hello **nyctaxidb.trip** och **nyctaxidb.fare** tabeller på tre nycklar som finns i alla poster: ”medallion” ”, hacka\_licens”, och ”hämtning\_datetime”.</span><span class="sxs-lookup"><span data-stu-id="52ae6-298">First we join hello **nyctaxidb.trip** and **nyctaxidb.fare** tables on three keys that are present in all records : "medallion", "hack\_license", and "pickup\_datetime".</span></span> <span data-ttu-id="52ae6-299">Vi sedan generera en binär klassificering etikett **lutad** och en etikett för flera klassen klassificering **tips\_klassen**.</span><span class="sxs-lookup"><span data-stu-id="52ae6-299">We then generate a binary classification label **tipped** and a multi-class classification label **tip\_class**.</span></span>

<span data-ttu-id="52ae6-300">toobe kan toouse hello ned samplade data direkt från hello [importera Data] [ import-data] modul i Azure Machine Learning är det nödvändigt toostore hello resultaten av hello ovan frågan tooan interna Hive-tabell.</span><span class="sxs-lookup"><span data-stu-id="52ae6-300">toobe able toouse hello down sampled data directly from hello [Import Data][import-data] module in Azure Machine Learning, it is necessary toostore hello results of hello above query tooan internal Hive table.</span></span> <span data-ttu-id="52ae6-301">I följande, vi skapa en intern Hive-tabell och fylla i dess innehåll med hello ansluten och ned samplade data.</span><span class="sxs-lookup"><span data-stu-id="52ae6-301">In what follows, we create an internal Hive table and populate its contents with hello joined and down sampled data.</span></span>

<span data-ttu-id="52ae6-302">hello fråga gäller Hive standardfunktioner direkt toogenerate hello timme på dagen och veckan på året, veckodag (1 står för måndag och 7 står för söndag) från hello ”hämtning\_datetime” fältet och hello direkt avståndet mellan hello hämtning och dropoff platser.</span><span class="sxs-lookup"><span data-stu-id="52ae6-302">hello query applies standard Hive functions directly toogenerate hello hour of day, week of year, weekday (1 stands for Monday, and 7 stands for Sunday) from hello "pickup\_datetime" field,  and hello direct distance between hello pickup and dropoff locations.</span></span> <span data-ttu-id="52ae6-303">Användare kan se för[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) för en fullständig lista över dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="52ae6-303">Users can refer too[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) for a complete list of such functions.</span></span>

<span data-ttu-id="52ae6-304">Hej fråga sedan ned exempel hello data så att hello frågeresultat passar in i hello Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="52ae6-304">hello query then down samples hello data so that hello query results can fit into hello Azure Machine Learning Studio.</span></span> <span data-ttu-id="52ae6-305">Endast ca 1% av hello ursprungliga datauppsättningen importeras till hello Studio.</span><span class="sxs-lookup"><span data-stu-id="52ae6-305">Only about 1% of hello original dataset is imported into hello Studio.</span></span>

<span data-ttu-id="52ae6-306">Nedan visas hello innehållet i *exempel\_hive\_förbereda\_för\_aml\_full.hql* filen som förbereder data för modellen skapar i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="52ae6-306">Below are hello contents of *sample\_hive\_prepare\_for\_aml\_full.hql* file that prepares data for model building in Azure Machine Learning.</span></span>

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

<span data-ttu-id="52ae6-307">toorun fråga för den här frågan från hello Hive directory:</span><span class="sxs-lookup"><span data-stu-id="52ae6-307">toorun this query, from hello Hive directory prompt :</span></span>

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

<span data-ttu-id="52ae6-308">Nu har vi en intern tabell ”nyctaxidb.nyctaxi_downsampled_dataset” som kan nås med hjälp av hello [importera Data] [ import-data] modul från Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="52ae6-308">We now have an internal table "nyctaxidb.nyctaxi_downsampled_dataset" which can be accessed using hello [Import Data][import-data] module from Azure Machine Learning.</span></span> <span data-ttu-id="52ae6-309">Vi kan dessutom använda denna dataset för att skapa Machine Learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="52ae6-309">Furthermore, we may use this dataset for building Machine Learning models.</span></span>  

### <a name="use-hello-import-data-module-in-azure-machine-learning-tooaccess-hello-down-sampled-data"></a><span data-ttu-id="52ae6-310">Använd hello importera Data modul i Azure Machine Learning tooaccess hello ned samplade data</span><span class="sxs-lookup"><span data-stu-id="52ae6-310">Use hello Import Data module in Azure Machine Learning tooaccess hello down sampled data</span></span>
<span data-ttu-id="52ae6-311">Som krav för utfärdande av Hive-frågor i hello [importera Data] [ import-data] modulen för Azure Machine Learning vi behöver komma åt tooan Azure Machine Learning-arbetsytan och komma åt toohello autentiseringsuppgifterna för hello klustret och dess associerade lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="52ae6-311">As prerequisites for issuing Hive queries in hello [Import Data][import-data] module of Azure Machine Learning, we need access tooan Azure Machine Learning workspace and access toohello credentials of hello cluster and its associated storage account.</span></span>

<span data-ttu-id="52ae6-312">Vissa detaljer på hello [importera Data] [ import-data] tooinput modulen och hello parametrar:</span><span class="sxs-lookup"><span data-stu-id="52ae6-312">Some details on hello [Import Data][import-data] module and hello parameters tooinput :</span></span>

<span data-ttu-id="52ae6-313">**HCatalog server URI**: om hello klusternamnet är abc123, så det är bara: https://abc123.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="52ae6-313">**HCatalog server URI**: If hello cluster name is abc123, then this is simply : https://abc123.azurehdinsight.net</span></span>

<span data-ttu-id="52ae6-314">**Hadoop användarkontonamnet** : hello användarnamn som valts för klustret hello (**inte** hello användarnamnet)</span><span class="sxs-lookup"><span data-stu-id="52ae6-314">**Hadoop user account name** : hello user name chosen for hello cluster (**not** hello remote access user name)</span></span>

<span data-ttu-id="52ae6-315">**Kontolösenord för Hadoop ser** : hello lösenord du valde för hello kluster (**inte** hello remote access-lösenordet)</span><span class="sxs-lookup"><span data-stu-id="52ae6-315">**Hadoop ser account password** : hello password chosen for hello cluster (**not** hello remote access password)</span></span>

<span data-ttu-id="52ae6-316">**Platsen för utdata** : detta väljs toobe Azure.</span><span class="sxs-lookup"><span data-stu-id="52ae6-316">**Location of output data** : This is chosen toobe Azure.</span></span>

<span data-ttu-id="52ae6-317">**Azure lagringskontonamnet** : namnet på hello standardkontot för lagring som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="52ae6-317">**Azure storage account name** : Name of hello default storage account associated with hello cluster.</span></span>

<span data-ttu-id="52ae6-318">**Azure behållarnamn** : Detta är hello behållare för standardnamnet för hello kluster och är vanligtvis hello samma som hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-318">**Azure container name** : This is hello default container name for hello cluster, and is typically hello same as hello cluster name.</span></span> <span data-ttu-id="52ae6-319">Detta är bara abc123 för ett kluster som heter ”abc123”.</span><span class="sxs-lookup"><span data-stu-id="52ae6-319">For a cluster called "abc123", this is just abc123.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="52ae6-320">**Alla tabeller som vi vill tooquery med hello [importera Data] [ import-data] modul i Azure Machine Learning måste vara en intern tabell.**</span><span class="sxs-lookup"><span data-stu-id="52ae6-320">**Any table we wish tooquery using hello [Import Data][import-data] module in Azure Machine Learning must be an internal table.**</span></span> <span data-ttu-id="52ae6-321">Ett tips för att avgöra om en tabell T i en databas D.db är en intern tabell är som följer.</span><span class="sxs-lookup"><span data-stu-id="52ae6-321">A tip for determining if a table T in a database D.db is an internal table is as follows.</span></span>
> 
> 

<span data-ttu-id="52ae6-322">Hive directory prompten problemet hello kommandot från hello:</span><span class="sxs-lookup"><span data-stu-id="52ae6-322">From hello Hive directory prompt, issue hello command :</span></span>

    hdfs dfs -ls wasb:///D.db/T

<span data-ttu-id="52ae6-323">Om hello tabell är en intern tabell och det fylls, måste innehållet visas här.</span><span class="sxs-lookup"><span data-stu-id="52ae6-323">If hello table is an internal table and it is populated, its contents must show here.</span></span> <span data-ttu-id="52ae6-324">Ett annat sätt toodetermine om en tabell är en intern tabell är toouse hello Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="52ae6-324">Another way toodetermine whether a table is an internal table is toouse hello Azure Storage Explorer.</span></span> <span data-ttu-id="52ae6-325">Använda det toonavigate toohello behållare standardnamnet hello klustret och sedan filtrera efter hello tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="52ae6-325">Use it toonavigate toohello default container name of hello cluster, and then filter by hello table name.</span></span> <span data-ttu-id="52ae6-326">Om hello tabell och dess innehåll visas, bekräftar det att det är en intern tabell.</span><span class="sxs-lookup"><span data-stu-id="52ae6-326">If hello table and its contents show up, this confirms that it is an internal table.</span></span>

<span data-ttu-id="52ae6-327">Här är en ögonblicksbild av hello Hive-fråga och hello [importera Data] [ import-data] modulen:</span><span class="sxs-lookup"><span data-stu-id="52ae6-327">Here is a snapshot of hello Hive query and hello [Import Data][import-data] module:</span></span>

![Hive-fråga för modulen importera Data](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

<span data-ttu-id="52ae6-329">Observera att sedan vår ned samplade data finns i standardbehållaren hello hello resulterande Hive-fråga från Azure Machine Learning är väldigt enkelt och bara en ”Välj * från nyctaxidb.nyctaxi\_upplösning\_data”.</span><span class="sxs-lookup"><span data-stu-id="52ae6-329">Note that since our down sampled data resides in hello default container, hello resulting Hive query from Azure Machine Learning is very simple and is just a "SELECT * FROM nyctaxidb.nyctaxi\_downsampled\_data".</span></span>

<span data-ttu-id="52ae6-330">hello dataset kan nu användas som hello som startpunkt för att skapa Machine Learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="52ae6-330">hello dataset may now be used as hello starting point for building Machine Learning models.</span></span>

### <span data-ttu-id="52ae6-331"><a name="mlmodel"></a>Bygga modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="52ae6-331"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="52ae6-332">Vi kan nu kan tooproceed toomodel byggnad och distribution av modellen i [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="52ae6-332">We are now able tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="52ae6-333">hello data är klar för oss toouse i adressering hello förutsägelse problem som anges ovan:</span><span class="sxs-lookup"><span data-stu-id="52ae6-333">hello data is ready for us toouse in addressing hello prediction problems identified above:</span></span>

<span data-ttu-id="52ae6-334">**1. Binär klassificering**: toopredict om huruvida ett tips har betalat för en resa.</span><span class="sxs-lookup"><span data-stu-id="52ae6-334">**1. Binary classification**: toopredict whether or not a tip was paid for a trip.</span></span>

<span data-ttu-id="52ae6-335">**Deltagaren används:** Two-class logistic regression</span><span class="sxs-lookup"><span data-stu-id="52ae6-335">**Learner used:** Two-class logistic regression</span></span>

<span data-ttu-id="52ae6-336">a.</span><span class="sxs-lookup"><span data-stu-id="52ae6-336">a.</span></span> <span data-ttu-id="52ae6-337">För det här problemet är våra mål (eller klassen) etiketten ”lutad”.</span><span class="sxs-lookup"><span data-stu-id="52ae6-337">For this problem, our target (or class) label is "tipped".</span></span> <span data-ttu-id="52ae6-338">Vår ursprungliga ned provtagning datauppsättning har några kolumner som är mål minnesläckor för klassificering experimentet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-338">Our original down-sampled dataset has a few columns that are target leaks for this classification experiment.</span></span> <span data-ttu-id="52ae6-339">I synnerhet: tips\_klassen, tips\_belopp och totalt\_belopp avslöjar information om hello måletikett som inte är tillgänglig vid testning tid.</span><span class="sxs-lookup"><span data-stu-id="52ae6-339">In particular : tip\_class, tip\_amount, and total\_amount reveal information about hello target label that is not available at testing time.</span></span> <span data-ttu-id="52ae6-340">Vi ta bort dessa kolumner från behandling med hello [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="52ae6-340">We remove these columns from consideration using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="52ae6-341">hello ögonblicksbild nedan visar vår experiment toopredict huruvida ett tips har betalat för en given resa.</span><span class="sxs-lookup"><span data-stu-id="52ae6-341">hello snapshot below shows our experiment toopredict whether or not a tip was paid for a given trip.</span></span>

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

<span data-ttu-id="52ae6-343">b.</span><span class="sxs-lookup"><span data-stu-id="52ae6-343">b.</span></span> <span data-ttu-id="52ae6-344">Det här experimentet har våra mål etikett distributioner ungefär 1:1.</span><span class="sxs-lookup"><span data-stu-id="52ae6-344">For this experiment, our target label distributions were roughly 1:1.</span></span>

<span data-ttu-id="52ae6-345">hello ögonblicksbild nedan visar hello distributionen av tips klassen etiketter för hello binära klassificeringsproblem.</span><span class="sxs-lookup"><span data-stu-id="52ae6-345">hello snapshot below shows hello distribution of tip class labels for hello binary classification problem.</span></span>

![Distribution av tips klassen etiketter](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

<span data-ttu-id="52ae6-347">Därför kan får vi en AUC av 0.987 enligt hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="52ae6-347">As a result, we obtain an AUC of 0.987 as shown in hello figure below.</span></span>

![AUC värde](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

<span data-ttu-id="52ae6-349">**2. Multiklass-baserad klassificering**: toopredict hello många av tips för hello resan, med hjälp av hello tidigare definierade klasser.</span><span class="sxs-lookup"><span data-stu-id="52ae6-349">**2. Multiclass classification**: toopredict hello range of tip amounts paid for hello trip, using hello previously defined classes.</span></span>

<span data-ttu-id="52ae6-350">**Deltagaren används:** multiklass-baserad logistic regression</span><span class="sxs-lookup"><span data-stu-id="52ae6-350">**Learner used:** Multiclass logistic regression</span></span>

<span data-ttu-id="52ae6-351">a.</span><span class="sxs-lookup"><span data-stu-id="52ae6-351">a.</span></span> <span data-ttu-id="52ae6-352">För det här problemet är vårt mål (eller klassen) etiketten ”tips\_klass” vilket kan ta en av fem värden (0,1,2,3,4).</span><span class="sxs-lookup"><span data-stu-id="52ae6-352">For this problem, our target (or class) label is "tip\_class" which can take one of five values (0,1,2,3,4).</span></span> <span data-ttu-id="52ae6-353">Vi har några kolumner som är mål minnesläckor för experimentet som hello binär klassificering fallet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-353">As in hello binary classification case, we have a few columns that are target leaks for this experiment.</span></span> <span data-ttu-id="52ae6-354">I synnerhet: lutad, tips\_totala angivet\_belopp avslöjar information om hello måletikett som inte är tillgänglig vid testning tid.</span><span class="sxs-lookup"><span data-stu-id="52ae6-354">In particular : tipped, tip\_amount, total\_amount reveal information about hello target label that is not available at testing time.</span></span> <span data-ttu-id="52ae6-355">Vi ta bort dessa kolumner med hello [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="52ae6-355">We remove these columns using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="52ae6-356">hello ögonblicksbild nedan visar vår experiment toopredict i vilka bin ett tips är sannolikt toofall (klass 0: tips = 0, klass 1: tips > 0 och tips < = $5, klass 2: tips > 5 och tips < = $10, klass 3: tips > 10 och tips < = $20 Klass 4: tips > 20)</span><span class="sxs-lookup"><span data-stu-id="52ae6-356">hello snapshot below shows our experiment toopredict in which bin a tip is likely toofall ( Class 0: tip = $0, class 1 : tip > $0 and tip <= $5, Class 2 : tip > $5 and tip <= $10, Class 3 : tip > $10 and tip <= $20, Class 4 : tip > $20)</span></span>

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

<span data-ttu-id="52ae6-358">Nu visas hur våra faktiska test klassen distribution ser ut.</span><span class="sxs-lookup"><span data-stu-id="52ae6-358">We now show what our actual test class distribution looks like.</span></span> <span data-ttu-id="52ae6-359">Vi se att klassen 0 och 1 för klassen är vanliga, hello andra klasser är ovanligt.</span><span class="sxs-lookup"><span data-stu-id="52ae6-359">We see that while Class 0 and Class 1 are prevalent, hello other classes are rare.</span></span>

![Testa klassen distribution](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

<span data-ttu-id="52ae6-361">b.</span><span class="sxs-lookup"><span data-stu-id="52ae6-361">b.</span></span> <span data-ttu-id="52ae6-362">Det här experimentet använder vi en förvirring matrisen toolook på vår prognosens noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="52ae6-362">For this experiment, we use a confusion matrix toolook at our prediction accuracies.</span></span> <span data-ttu-id="52ae6-363">Detta visas nedan.</span><span class="sxs-lookup"><span data-stu-id="52ae6-363">This is shown below.</span></span>

![Förvirring matris](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

<span data-ttu-id="52ae6-365">Observera att vår klass noggrannhet på hello vanliga klasser är ganska bra, hello modellen utför inte bra på ”learning” på hello sällsynta klasser.</span><span class="sxs-lookup"><span data-stu-id="52ae6-365">Note that while our class accuracies on hello prevalent classes is quite good, hello model does not do a good job of "learning" on hello rarer classes.</span></span>

<span data-ttu-id="52ae6-366">**3. Regression uppgiften**: toopredict hello tips betalats för en transport.</span><span class="sxs-lookup"><span data-stu-id="52ae6-366">**3. Regression task**: toopredict hello amount of tip paid for a trip.</span></span>

<span data-ttu-id="52ae6-367">**Deltagaren används:** Boosted beslutsträdet</span><span class="sxs-lookup"><span data-stu-id="52ae6-367">**Learner used:** Boosted decision tree</span></span>

<span data-ttu-id="52ae6-368">a.</span><span class="sxs-lookup"><span data-stu-id="52ae6-368">a.</span></span> <span data-ttu-id="52ae6-369">För det här problemet är vårt mål (eller klassen) etiketten ”tips\_belopp”.</span><span class="sxs-lookup"><span data-stu-id="52ae6-369">For this problem, our target (or class) label is "tip\_amount".</span></span> <span data-ttu-id="52ae6-370">I det här fallet är vårt mål minnesläckor: lutad, tips\_klass, totalt antal\_belopp; dessa variabler avslöja information om hello tips belopp som är vanligtvis inte tillgängliga vid testning tid.</span><span class="sxs-lookup"><span data-stu-id="52ae6-370">Our target leaks in this case are : tipped, tip\_class, total\_amount ; all these variables reveal information about hello tip amount that is typically unavailable at testing time.</span></span> <span data-ttu-id="52ae6-371">Vi ta bort dessa kolumner med hello [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="52ae6-371">We remove these columns using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="52ae6-372">hello ögonblicksbild belows visar vår experiment toopredict hello hello ges tips.</span><span class="sxs-lookup"><span data-stu-id="52ae6-372">hello snapshot belows shows our experiment toopredict hello amount of hello given tip.</span></span>

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

<span data-ttu-id="52ae6-374">b.</span><span class="sxs-lookup"><span data-stu-id="52ae6-374">b.</span></span> <span data-ttu-id="52ae6-375">Regression problem vi mäter hello noggrannhet med vår prognoser genom att titta på hello kvadrat fel i hello förutsägelser hello bestämningskoefficienten och hello som.</span><span class="sxs-lookup"><span data-stu-id="52ae6-375">For regression problems, we measure hello accuracies of our prediction by looking at hello squared error in hello predictions, hello coefficient of determination, and hello like.</span></span> <span data-ttu-id="52ae6-376">Vi visas dessa nedan.</span><span class="sxs-lookup"><span data-stu-id="52ae6-376">We show these below.</span></span>

![Förutsägelse statistik](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

<span data-ttu-id="52ae6-378">Vi kan se att bestämningskoefficienten om hello är 0.709, innebär ungefär 71% hello variationen förklaras med vår modell koefficienter.</span><span class="sxs-lookup"><span data-stu-id="52ae6-378">We see that about hello coefficient of determination is 0.709, implying about 71% of hello variance is explained by our model coefficients.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="52ae6-379">Mer om Azure Machine Learning toolearn och hur tooaccess och använder den, se för[vad är Machine Learning?](machine-learning-what-is-machine-learning.md).</span><span class="sxs-lookup"><span data-stu-id="52ae6-379">toolearn more about Azure Machine Learning and how tooaccess and use it, please refer too[What's Machine Learning?](machine-learning-what-is-machine-learning.md).</span></span> <span data-ttu-id="52ae6-380">En resurs som är användbar för att spela upp med en massa Machine Learning-experiment i Azure Machine Learning är hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="52ae6-380">A very useful resource for playing with a bunch of Machine Learning experiments on Azure Machine Learning is hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span></span> <span data-ttu-id="52ae6-381">hello galleriet omfattar en alla experiment och ger en omfattande introduktion till hello nya funktioner i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="52ae6-381">hello Gallery covers a gamut of experiments and provides a thorough introduction into hello range of capabilities of Azure Machine Learning.</span></span>
> 
> 

## <a name="license-information"></a><span data-ttu-id="52ae6-382">Licensinformationen</span><span class="sxs-lookup"><span data-stu-id="52ae6-382">License Information</span></span>
<span data-ttu-id="52ae6-383">Den här genomgången exempel och dess tillhörande skript som delas av Microsoft under hello MIT-licens.</span><span class="sxs-lookup"><span data-stu-id="52ae6-383">This sample walkthrough and its accompanying scripts are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="52ae6-384">Kontrollera filen LICENSE.txt för hello i hello directory hello exempelkoden på GitHub för mer information.</span><span class="sxs-lookup"><span data-stu-id="52ae6-384">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="52ae6-385">Referenser</span><span class="sxs-lookup"><span data-stu-id="52ae6-385">References</span></span>
<span data-ttu-id="52ae6-386">• [Andrés Monroy NYC Taxi resor hämtningssidan](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="52ae6-386">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="52ae6-387">• [FOILing NYC Taxitransport resa Data av Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="52ae6-387">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="52ae6-388">• [NYC Taxi och Limousine kommissionens forskning och statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="52ae6-388">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
