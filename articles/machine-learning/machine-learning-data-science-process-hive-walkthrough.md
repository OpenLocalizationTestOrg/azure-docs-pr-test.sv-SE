---
title: Utforska data i ett Hadoop-kluster och skapa modeller i Azure Machine Learning | Microsoft Docs
description: "Med hjälp av Team datavetenskap Process för en slutpunkt till slutpunkt-scenario med HDInsight Hadoop-kluster för att skapa och distribuera en modell med en offentligt tillgängliga dataset."
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
ms.openlocfilehash: e48d59ca467e3e7fd772389e6e48a2d81726f859
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a><span data-ttu-id="392f9-103">Team vetenskap av data i praktiken: Använd Azure HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="392f9-103">The Team Data Science Process in action: Use Azure HDInsight Hadoop clusters</span></span>
<span data-ttu-id="392f9-104">I den här genomgången ska vi använda den [Team Data vetenskap processen (TDSP)](data-science-process-overview.md) i ett scenario för slutpunkt till slutpunkt med hjälp av en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) för att lagra, utforska och funktion tekniker data från den offentligt tillgängliga [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset, och exempeldata nedåt.</span><span class="sxs-lookup"><span data-stu-id="392f9-104">In this walkthrough, we use the [Team Data Science Process (TDSP)](data-science-process-overview.md) in an end-to-end scenario using an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) to store, explore and feature engineer data from the publicly available [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset, and to down sample the data.</span></span> <span data-ttu-id="392f9-105">Modeller av data skapas med Azure Machine Learning att hantera binära och multiklass-baserad klassificering och regression förutsägande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="392f9-105">Models of the data are built with Azure Machine Learning to handle binary and multiclass classification and regression predictive tasks.</span></span>

<span data-ttu-id="392f9-106">En genomgång som visar hur du hanterar en större datamängd (1 terabyte) för en liknande scenario med HDInsight Hadoop-kluster för databearbetning finns [Team datavetenskap Process, med hjälp av Azure HDInsight Hadoop-kluster i en dataset 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md) .</span><span class="sxs-lookup"><span data-stu-id="392f9-106">For a walkthrough that shows how to handle a larger (1 terabyte) dataset for a similar scenario using HDInsight Hadoop clusters for data processing, see [Team Data Science Process - Using Azure HDInsight Hadoop Clusters on a 1 TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).</span></span>

<span data-ttu-id="392f9-107">Det är också möjligt att använda en bärbar dator IPython för att utföra aktiviteter visas den här genomgången använder 1 TB dataset.</span><span class="sxs-lookup"><span data-stu-id="392f9-107">It is also possible to use an IPython notebook to accomplish the tasks presented the walkthrough using the 1 TB dataset.</span></span> <span data-ttu-id="392f9-108">Användare som vill använda den här metoden bör kontakta den [Criteo genomgången använder en Hive ODBC-anslutning](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="392f9-108">Users who would like to try this approach should consult the [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="392f9-109"><a name="dataset"></a>NYC Taxi resor Dataset-beskrivning</span><span class="sxs-lookup"><span data-stu-id="392f9-109"><a name="dataset"></a>NYC Taxi Trips Dataset description</span></span>
<span data-ttu-id="392f9-110">NYC Taxi resa data är cirka 20GB komprimerad fil med kommaavgränsade värden (CSV)-filer (~ 48GB okomprimerade), som består av fler än 173 miljoner enskilda resor och priser betalat för varje resa.</span><span class="sxs-lookup"><span data-stu-id="392f9-110">The NYC Taxi Trip data is about 20GB of compressed comma-separated values (CSV) files (~48GB uncompressed), comprising more than 173 million individual trips and the fares paid for each trip.</span></span> <span data-ttu-id="392f9-111">Hämtning och Samlingsbibliotek plats och tid, anonymiserade hackare (drivrutin) licensnummer och medallion (taxi's unikt id) nummer innehåller varje resa-post.</span><span class="sxs-lookup"><span data-stu-id="392f9-111">Each trip record includes the pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="392f9-112">Data omfattar alla resor år 2013 och finns i följande två datauppsättningar för varje månad:</span><span class="sxs-lookup"><span data-stu-id="392f9-112">The data covers all trips in the year 2013 and is provided in the following two datasets for each month:</span></span>

1. <span data-ttu-id="392f9-113">'Trip_data' CSV-filer innehåller resa information, till exempel antal passagerare, hämtning och dropoff punkter, resa varaktighet och resa längd.</span><span class="sxs-lookup"><span data-stu-id="392f9-113">The 'trip_data' CSV files contain trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="392f9-114">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="392f9-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="392f9-115">'Trip_fare' CSV-filer innehåller information om avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar, och det totala betald.</span><span class="sxs-lookup"><span data-stu-id="392f9-115">The 'trip_fare' CSV files contain details of the fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and the total amount paid.</span></span> <span data-ttu-id="392f9-116">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="392f9-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="392f9-117">Unik nyckel för att ansluta till resa\_data och resa\_avgiften består av fälten: medallion hackare\_licensen och hämtning\_datetime.</span><span class="sxs-lookup"><span data-stu-id="392f9-117">The unique key to join trip\_data and trip\_fare is composed of the fields: medallion, hack\_licence and pickup\_datetime.</span></span>

<span data-ttu-id="392f9-118">För att få alla detaljer relevanta för en viss resa, det räcker att ansluta med tre nycklar: ”medallion” ”, hacka\_licens” och ”hämtning\_datetime”.</span><span class="sxs-lookup"><span data-stu-id="392f9-118">To get all of the details relevant to a particular trip, it is sufficient to join with three keys: the "medallion", "hack\_license" and "pickup\_datetime".</span></span>

<span data-ttu-id="392f9-119">Vi beskriver vissa mer information om data när vi lagrar dem i Hive-tabeller inom kort.</span><span class="sxs-lookup"><span data-stu-id="392f9-119">We describe some more details of the data when we store them into Hive tables shortly.</span></span>

## <span data-ttu-id="392f9-120"><a name="mltasks"></a>Exempel på förutsägelse uppgifter</span><span class="sxs-lookup"><span data-stu-id="392f9-120"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="392f9-121">När närmar sig data som fastställer typ av du vill göra förutsägelser baserat på dess analys hjälper till att klargöra de uppgifter som du behöver ta med i processen.</span><span class="sxs-lookup"><span data-stu-id="392f9-121">When approaching data, determining the kind of predictions you want to make based on its analysis helps clarify the tasks that you will need to include in your process.</span></span>
<span data-ttu-id="392f9-122">Här följer tre exempel på förutsägelse problem som vi adressen i den här genomgången vars formulering baseras på den *tips\_belopp*:</span><span class="sxs-lookup"><span data-stu-id="392f9-122">Here are three examples of prediction problems that we address in this walkthrough whose formulation is based on the *tip\_amount*:</span></span>

1. <span data-ttu-id="392f9-123">**Binär klassificering**: förutsäga oavsett betalats ett tips för en resa i d.v.s. en *tips\_belopp* som är större än 0 är ett positivt exempel, när en *tips\_belopp* $0 är ett exempel på negativt.</span><span class="sxs-lookup"><span data-stu-id="392f9-123">**Binary classification**: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0
2. <span data-ttu-id="392f9-124">**Multiklass-baserad klassificering**: att förutsäga intervallet av tips för resan.</span><span class="sxs-lookup"><span data-stu-id="392f9-124">**Multiclass classification**: To predict the range of tip amounts paid for the trip.</span></span> <span data-ttu-id="392f9-125">Vi dela den *tips\_belopp* i fem lagerplatser eller klasser:</span><span class="sxs-lookup"><span data-stu-id="392f9-125">We divide the *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="392f9-126">**Regression uppgiften**: att förutsäga mängden tips för en resa.</span><span class="sxs-lookup"><span data-stu-id="392f9-126">**Regression task**: To predict the amount of the tip paid for a trip.</span></span>  

## <span data-ttu-id="392f9-127"><a name="setup"></a>Konfigurera en HDInsight Hadoop-kluster för avancerade analyser</span><span class="sxs-lookup"><span data-stu-id="392f9-127"><a name="setup"></a>Set up an HDInsight Hadoop cluster for advanced analytics</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-128">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-128">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="392f9-129">Du kan konfigurera en Azure-miljö för avancerade analyser som använder ett HDInsight-kluster i tre steg:</span><span class="sxs-lookup"><span data-stu-id="392f9-129">You can set up an Azure environment for advanced analytics that employs an HDInsight cluster in three steps:</span></span>

1. <span data-ttu-id="392f9-130">[Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md): det här lagringskontot används för att lagra data i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="392f9-130">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used for storing data in Azure Blob Storage.</span></span> <span data-ttu-id="392f9-131">De data som används i HDInsight-kluster finns också här.</span><span class="sxs-lookup"><span data-stu-id="392f9-131">The data used in HDInsight clusters also resides here.</span></span>
2. <span data-ttu-id="392f9-132">[Anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics Process- och teknikbehov](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="392f9-132">[Customize Azure HDInsight Hadoop clusters for the Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md).</span></span> <span data-ttu-id="392f9-133">Det här steget skapar ett Azure HDInsight Hadoop-kluster med 64-bitars Anaconda Python 2.7 installerad på alla noder.</span><span class="sxs-lookup"><span data-stu-id="392f9-133">This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="392f9-134">Det finns två viktiga steg för att komma ihåg vid anpassning av ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="392f9-134">There are two important steps to remember while customizing your HDInsight cluster.</span></span>
   
   * <span data-ttu-id="392f9-135">Kom ihåg att länka storage-konto som skapades i steg 1 med ditt HDInsight-kluster när du skapar den.</span><span class="sxs-lookup"><span data-stu-id="392f9-135">Remember to link the storage account created in step 1 with your HDInsight cluster when creating it.</span></span> <span data-ttu-id="392f9-136">Det här lagringskontot används för att komma åt data som bearbetas i klustret.</span><span class="sxs-lookup"><span data-stu-id="392f9-136">This storage account is used to access data that is processed within the cluster.</span></span>
   * <span data-ttu-id="392f9-137">När klustret har skapats kan du aktivera fjärråtkomst till huvudnod i klustret.</span><span class="sxs-lookup"><span data-stu-id="392f9-137">After the cluster is created, enable Remote Access to the head node of the cluster.</span></span> <span data-ttu-id="392f9-138">Navigera till den **Configuration** och klicka på **aktivera fjärråtkomst**.</span><span class="sxs-lookup"><span data-stu-id="392f9-138">Navigate to the **Configuration** tab and click **Enable Remote**.</span></span> <span data-ttu-id="392f9-139">Det här steget anger de autentiseringsuppgifter som används för fjärrinloggning.</span><span class="sxs-lookup"><span data-stu-id="392f9-139">This step specifies the user credentials used for remote login.</span></span>
3. <span data-ttu-id="392f9-140">[Skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md): den här Azure Machine Learning-arbetsytan används för att skapa machine learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="392f9-140">[Create an Azure Machine Learning workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used to build machine learning models.</span></span> <span data-ttu-id="392f9-141">Den här uppgiften riktar sig när du har slutfört en första datagranskning och ned med hjälp av HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="392f9-141">This task is addressed after completing an initial data exploration and down sampling using the HDInsight cluster.</span></span>

## <span data-ttu-id="392f9-142"><a name="getdata"></a>Hämta data från en offentlig källa</span><span class="sxs-lookup"><span data-stu-id="392f9-142"><a name="getdata"></a>Get the data from a public source</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-143">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-143">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="392f9-144">Att hämta den [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset från dess offentlig plats, kan du använda någon av metoderna som beskrivs i [flytta Data till och från Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) att kopiera data till din dator.</span><span class="sxs-lookup"><span data-stu-id="392f9-144">To get the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of the methods described in [Move Data to and from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) to copy the data to your machine.</span></span>

<span data-ttu-id="392f9-145">Här beskrivs hur använder AzCopy för att överföra filer som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="392f9-145">Here we describe how use AzCopy to transfer the files containing data.</span></span> <span data-ttu-id="392f9-146">Hämta och installera AzCopy följer du anvisningarna [komma igång med kommandoradsverktyget Azcopy](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="392f9-146">To download and install AzCopy follow the instructions at [Getting Started with the AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

1. <span data-ttu-id="392f9-147">Från Kommandotolken, utfärda följande AzCopy kommandona, ersätter *< path_to_data_folder >* med önskat mål:</span><span class="sxs-lookup"><span data-stu-id="392f9-147">From a Command Prompt window, issue the following AzCopy commands, replacing *<path_to_data_folder>* with the desired destination:</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. <span data-ttu-id="392f9-148">När kopieringen är klar är totalt av 24 komprimerade filer i datamappen valt.</span><span class="sxs-lookup"><span data-stu-id="392f9-148">When the copy completes, a total of 24 zipped files are in the data folder chosen.</span></span> <span data-ttu-id="392f9-149">Packa upp de hämtade filerna i samma katalog på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="392f9-149">Unzip the downloaded files to the same directory on your local machine.</span></span> <span data-ttu-id="392f9-150">Anteckna den mapp där de okomprimerade filerna finns.</span><span class="sxs-lookup"><span data-stu-id="392f9-150">Make a note of the folder where the uncompressed files reside.</span></span> <span data-ttu-id="392f9-151">Den här mappen kommer kallas den *< sökväg\_till\_unzipped_data\_filer\>*  är det som följer.</span><span class="sxs-lookup"><span data-stu-id="392f9-151">This folder will be referred to as the *<path\_to\_unzipped_data\_files\>* is what follows.</span></span>

## <span data-ttu-id="392f9-152"><a name="upload"></a>Ladda upp data till standardbehållaren för Azure HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="392f9-152"><a name="upload"></a>Upload the data to the default container of Azure HDInsight Hadoop cluster</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-153">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-153">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="392f9-154">Ersätt följande parametrar i följande AzCopy-kommandon med de faktiska värdena som du angav när du skapar Hadoop-kluster och packa upp datafilerna.</span><span class="sxs-lookup"><span data-stu-id="392f9-154">In the following AzCopy commands, replace the following parameters with the actual values that you specified when creating the Hadoop cluster and unzipping the data files.</span></span>

* <span data-ttu-id="392f9-155">***&#60; path_to_data_folder >*** katalogen (tillsammans med sökväg) på datorn som innehåller de uppackade datafilerna</span><span class="sxs-lookup"><span data-stu-id="392f9-155">***&#60;path_to_data_folder>*** the directory (along with path) on your machine that contain the unzipped data files</span></span>  
* <span data-ttu-id="392f9-156">***&#60; lagringskontonamnet av Hadoop-kluster >*** storage-konto som är kopplad till ditt HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="392f9-156">***&#60;storage account name of Hadoop cluster>*** the storage account associated with your HDInsight cluster</span></span>
* <span data-ttu-id="392f9-157">***&#60; standardbehållaren för Hadoop-kluster >*** standardbehållaren som används av klustret.</span><span class="sxs-lookup"><span data-stu-id="392f9-157">***&#60;default container of Hadoop cluster>*** the default container used by your cluster.</span></span> <span data-ttu-id="392f9-158">Observera att namnet på standardbehållaren är vanligtvis samma namn som själva klustret.</span><span class="sxs-lookup"><span data-stu-id="392f9-158">Note that the name of the default container is usually the same name as the cluster itself.</span></span> <span data-ttu-id="392f9-159">Om klustret kallas ”abc123.azurehdinsight.net”, är standardbehållaren abc123.</span><span class="sxs-lookup"><span data-stu-id="392f9-159">For example, if the cluster is called "abc123.azurehdinsight.net", the default container is abc123.</span></span>
* <span data-ttu-id="392f9-160">***&#60; lagringskontonyckel >*** nyckel för storage-konto som används av klustret</span><span class="sxs-lookup"><span data-stu-id="392f9-160">***&#60;storage account key>*** the key for the storage account used by your cluster</span></span>

<span data-ttu-id="392f9-161">Kör följande två AzCopy-kommandon från en kommandotolk eller ett Windows PowerShell-fönster på datorn.</span><span class="sxs-lookup"><span data-stu-id="392f9-161">From a Command Prompt or a Windows PowerShell window in your machine, run the following two AzCopy commands.</span></span>

<span data-ttu-id="392f9-162">Det här kommandot överför data resa till ***nyctaxitripraw*** katalog i standardbehållaren för Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="392f9-162">This command uploads the trip data to ***nyctaxitripraw*** directory in the default container of the Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

<span data-ttu-id="392f9-163">Det här kommandot överför data till avgiften ***nyctaxifareraw*** katalog i standardbehållaren för Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="392f9-163">This command uploads the fare data to ***nyctaxifareraw*** directory in the default container of the Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

<span data-ttu-id="392f9-164">Data bör nu i Azure Blob Storage och redo att användas i HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="392f9-164">The data should now in Azure Blob Storage and ready to be consumed within the HDInsight cluster.</span></span>

## <span data-ttu-id="392f9-165"><a name="#download-hql-files"></a>Logga in på huvudnod Hadoop-kluster och och förbereda för undersökande dataanalys</span><span class="sxs-lookup"><span data-stu-id="392f9-165"><a name="#download-hql-files"></a>Log into the head node of Hadoop cluster and and prepare for exploratory data analysis</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-166">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-166">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="392f9-167">För att komma åt huvudnod i klustret för undersökande dataanalys och ned sampling av data, följer du proceduren som beskrivs i [komma åt det Head nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="392f9-167">To access the head node of the cluster for exploratory data analysis and down sampling of the data, follow the procedure outlined in [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

<span data-ttu-id="392f9-168">I den här genomgången ska vi i första hand använder frågor som skrivits i [Hive](https://hive.apache.org/), en SQL-liknande query language att utföra preliminära data explorations.</span><span class="sxs-lookup"><span data-stu-id="392f9-168">In this walkthrough, we primarily use queries written in [Hive](https://hive.apache.org/), a SQL-like query language, to perform preliminary data explorations.</span></span> <span data-ttu-id="392f9-169">Hive-frågor som lagras i .hql filer.</span><span class="sxs-lookup"><span data-stu-id="392f9-169">The Hive queries are stored in .hql files.</span></span> <span data-ttu-id="392f9-170">Vi sedan exempel ned på dessa data som ska användas i Azure Machine Learning för att skapa modeller.</span><span class="sxs-lookup"><span data-stu-id="392f9-170">We then down sample this data to be used within Azure Machine Learning for building models.</span></span>

<span data-ttu-id="392f9-171">För att förbereda klustret för undersökande dataanalys, vi hämtar .hql filer som innehåller de relevanta Hive-skript från [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) till en lokal katalog (C:\temp) i huvudnod.</span><span class="sxs-lookup"><span data-stu-id="392f9-171">To prepare the cluster for exploratory data analysis, we download the .hql files containing the relevant Hive scripts from [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) to a local directory (C:\temp) on the head node.</span></span> <span data-ttu-id="392f9-172">Det gör du genom att öppna den **kommandotolk** från inom huvudnod i klustret och utfärda följande två kommandon:</span><span class="sxs-lookup"><span data-stu-id="392f9-172">To do this, open the **Command Prompt** from within the head node of the cluster and issue the following two commands:</span></span>

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

<span data-ttu-id="392f9-173">Dessa två kommandon hämtas alla .hql filer som behövs i den här genomgången till den lokala katalogen ***C:\temp &#92;*** i huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="392f9-173">These two commands will download all .hql files needed in this walkthrough to the local directory ***C:\temp&#92;*** in the head node.</span></span>

## <span data-ttu-id="392f9-174"><a name="#hive-db-tables"></a>Skapa Hive-databasen och tabeller som är partitionerad per månad</span><span class="sxs-lookup"><span data-stu-id="392f9-174"><a name="#hive-db-tables"></a>Create Hive database and tables partitioned by month</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-175">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-175">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="392f9-176">Vi är nu redo att skapa Hive-tabeller för våra NYC taxi dataset.</span><span class="sxs-lookup"><span data-stu-id="392f9-176">We are now ready to create Hive tables for our NYC taxi dataset.</span></span>
<span data-ttu-id="392f9-177">Öppna i huvudnod i Hadoop-kluster i ***Hadoop kommandoraden*** på skrivbordet för huvudnoden, och ange Hive-katalogen genom att ange kommandot</span><span class="sxs-lookup"><span data-stu-id="392f9-177">In the head node of the Hadoop cluster, open the ***Hadoop Command Line*** on the desktop of the head node, and enter the Hive directory by entering the command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="392f9-178">**Kör alla Hive-kommandon i den här genomgången från ovan Hive bin / directory-fråga. Detta hand tar om eventuella problem med sökväg automatiskt. Vi använder termer ”Hive directory prompt” ”, Hive bin / directory prompt”, och ”Hadoop kommandoraden” synonymt i den här genomgången.**</span><span class="sxs-lookup"><span data-stu-id="392f9-178">**Run all Hive commands in this walkthrough from the above Hive bin/ directory prompt. This will take care of any path issues automatically. We use the terms "Hive directory prompt", "Hive bin/ directory prompt",  and "Hadoop Command Line" interchangeably in this walkthrough.**</span></span>
> 
> 

<span data-ttu-id="392f9-179">Ange följande kommando i Hadoop kommandoraden för huvudnoden att skicka Hive-fråga för att skapa Hive-databasen och tabeller från Hive directory prompten:</span><span class="sxs-lookup"><span data-stu-id="392f9-179">From the Hive directory prompt, enter the following command in Hadoop Command Line of the head node to submit the Hive query to create Hive database and tables:</span></span>

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

<span data-ttu-id="392f9-180">Här är innehållet i den ***C:\temp\sample\_hive\_skapa\_db\_och\_tables.hql*** -fil som skapar Hive databas ***nyctaxidb*** och tabeller ***resa*** och ***avgiften***.</span><span class="sxs-lookup"><span data-stu-id="392f9-180">Here is the content of the ***C:\temp\sample\_hive\_create\_db\_and\_tables.hql*** file which creates Hive database ***nyctaxidb*** and tables ***trip*** and ***fare***.</span></span>

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

<span data-ttu-id="392f9-181">Den här Hive-skript skapar två tabeller:</span><span class="sxs-lookup"><span data-stu-id="392f9-181">This Hive script creates two tables:</span></span>

* <span data-ttu-id="392f9-182">tabellen ”resa” innehåller resa information om varje resa (drivrutinsinformation, pickup tid, resa avstånd och gånger)</span><span class="sxs-lookup"><span data-stu-id="392f9-182">the "trip" table contains trip details of each ride (driver details, pickup time, trip distance and times)</span></span>
* <span data-ttu-id="392f9-183">tabellen ”avgiften” innehåller information om avgiften (avgiften, tips belopp, vägtullar och tillägg).</span><span class="sxs-lookup"><span data-stu-id="392f9-183">the "fare" table contains fare details (fare amount, tip amount, tolls and surcharges).</span></span>

<span data-ttu-id="392f9-184">Om du behöver ytterligare hjälp med dessa procedurer eller vill undersöka de alternativ som finns i avsnittet [skicka Hive-frågor direkt från kommandoraden i Hadoop ](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="392f9-184">If you need any additional assistance with these procedures or want to investigate alternative ones, see the section [Submit Hive queries directly from the Hadoop Command Line ](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="392f9-185"><a name="#load-data"></a>Läs in Data till Hive-tabeller av partitioner</span><span class="sxs-lookup"><span data-stu-id="392f9-185"><a name="#load-data"></a>Load Data to Hive tables by partitions</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-186">Detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-186">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="392f9-187">NYC taxi datamängden har en naturlig partitionering per månad som vi använder för att aktivera snabbare bearbetning och fråga.</span><span class="sxs-lookup"><span data-stu-id="392f9-187">The NYC taxi dataset has a natural partitioning by month, which we use to enable faster processing and query times.</span></span> <span data-ttu-id="392f9-188">PowerShell-kommandona nedan (utfärdats från en Hive directory med hjälp av den **Hadoop kommandoraden**) att läsa in data ”resa” och ”avgiften” Hive tabeller partitionerad per månad.</span><span class="sxs-lookup"><span data-stu-id="392f9-188">The PowerShell commands below (issued from the Hive directory using the **Hadoop Command Line**) load data to the "trip" and "fare" Hive tables partitioned by month.</span></span>

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

<span data-ttu-id="392f9-189">Den *exempel\_hive\_ladda\_data\_av\_partitions.hql* filen innehåller följande **LADDA** kommandon.</span><span class="sxs-lookup"><span data-stu-id="392f9-189">The *sample\_hive\_load\_data\_by\_partitions.hql* file contains the following **LOAD** commands.</span></span>

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

<span data-ttu-id="392f9-190">Observera att ett antal Hive-frågor som vi använder här pågående utforskning innebär att bara en enda partition eller på bara några partitioner.</span><span class="sxs-lookup"><span data-stu-id="392f9-190">Note that a number of Hive queries we use here in the exploration process involve looking at just a single partition or at only a couple of partitions.</span></span> <span data-ttu-id="392f9-191">Men dessa frågor kan köras över hela data.</span><span class="sxs-lookup"><span data-stu-id="392f9-191">But these queries could be run across the entire data.</span></span>

### <span data-ttu-id="392f9-192"><a name="#show-db"></a>Visa databaser i HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="392f9-192"><a name="#show-db"></a>Show databases in the HDInsight Hadoop cluster</span></span>
<span data-ttu-id="392f9-193">Om du vill visa de databaser som skapas i HDInsight Hadoop-kluster i Hadoop kommandoradsfönster, kör du följande kommando i Hadoop kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="392f9-193">To show the databases created in HDInsight Hadoop cluster inside the Hadoop Command Line window, run the following command in Hadoop Command Line:</span></span>

    hive -e "show databases;"

### <span data-ttu-id="392f9-194"><a name="#show-tables"></a>Visa Hive-tabeller i databasen nyctaxidb</span><span class="sxs-lookup"><span data-stu-id="392f9-194"><a name="#show-tables"></a>Show the Hive tables in the nyctaxidb database</span></span>
<span data-ttu-id="392f9-195">Om du vill visa tabellerna i databasen nyctaxidb, kör du följande kommando i Hadoop kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="392f9-195">To show the tables in the nyctaxidb database, run the following command in Hadoop Command Line:</span></span>

    hive -e "show tables in nyctaxidb;"

<span data-ttu-id="392f9-196">Vi kan bekräfta att partitioneras tabellerna genom att utfärda kommandot nedan:</span><span class="sxs-lookup"><span data-stu-id="392f9-196">We can confirm that the tables are partitioned by issuing the command below:</span></span>

    hive -e "show partitions nyctaxidb.trip;"

<span data-ttu-id="392f9-197">Förväntad utdata visas nedan:</span><span class="sxs-lookup"><span data-stu-id="392f9-197">The expected output is shown below:</span></span>

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

<span data-ttu-id="392f9-198">På liknande sätt kan vi se till att avgiften tabellen är partitionerad genom att utfärda kommandot nedan:</span><span class="sxs-lookup"><span data-stu-id="392f9-198">Similarly, we can ensure that the fare table is partitioned by issuing the command below:</span></span>

    hive -e "show partitions nyctaxidb.fare;"

<span data-ttu-id="392f9-199">Förväntad utdata visas nedan:</span><span class="sxs-lookup"><span data-stu-id="392f9-199">The expected output is shown below:</span></span>

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

## <span data-ttu-id="392f9-200"><a name="#explore-hive"></a>Datagranskning och funktionen teknikerna i Hive</span><span class="sxs-lookup"><span data-stu-id="392f9-200"><a name="#explore-hive"></a>Data exploration and feature engineering in Hive</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-201">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-201">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-202">Funktionen tekniska uppgifter för de data som läses in i Hive-tabeller och undersökning av data kan utföras med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="392f9-202">The data exploration and feature engineering tasks for the data loaded into the Hive tables can be accomplished using Hive queries.</span></span> <span data-ttu-id="392f9-203">Här följer exempel på sådana uppgifter att vi dig igenom i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="392f9-203">Here are examples of such tasks that we walk you through in this section:</span></span>

* <span data-ttu-id="392f9-204">Visa översta 10 posterna i båda tabellerna.</span><span class="sxs-lookup"><span data-stu-id="392f9-204">View the top 10 records in both tables.</span></span>
* <span data-ttu-id="392f9-205">Utforska data distributioner av ett fåtal fält i olika tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="392f9-205">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="392f9-206">Undersök data quality longitud och latitud fält.</span><span class="sxs-lookup"><span data-stu-id="392f9-206">Investigate data quality of the longitude and latitude fields.</span></span>
* <span data-ttu-id="392f9-207">Generera binära och multiklass-baserad klassificeringsetiketter baserat på de **tips\_belopp**.</span><span class="sxs-lookup"><span data-stu-id="392f9-207">Generate binary and multiclass classification labels based on the **tip\_amount**.</span></span>
* <span data-ttu-id="392f9-208">Generera funktioner genom att beräkna direkt kommunikation avstånd.</span><span class="sxs-lookup"><span data-stu-id="392f9-208">Generate features by computing the direct trip distances.</span></span>

### <a name="exploration-view-the-top-10-records-in-table-trip"></a><span data-ttu-id="392f9-209">Undersökning: Visa de översta 10 posterna i tabellen resa</span><span class="sxs-lookup"><span data-stu-id="392f9-209">Exploration: View the top 10 records in table trip</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-210">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-210">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-211">Om du vill se hur data ser ut, granska vi 10 poster från varje tabell.</span><span class="sxs-lookup"><span data-stu-id="392f9-211">To see what the data looks like, we examine 10 records from each table.</span></span> <span data-ttu-id="392f9-212">Kör följande två frågor separat från Hive directory-fråga i kommandoraden för Hadoop-konsolen för att kontrollera posterna.</span><span class="sxs-lookup"><span data-stu-id="392f9-212">Run the following two queries separately from the Hive directory prompt in the Hadoop Command Line console to inspect the records.</span></span>

<span data-ttu-id="392f9-213">Hämta de översta 10 posterna i tabellen ”resa” från den första månaden:</span><span class="sxs-lookup"><span data-stu-id="392f9-213">To get the top 10 records in the table "trip" from the first month:</span></span>

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

<span data-ttu-id="392f9-214">Hämta de översta 10 posterna i tabellen ”avgiften” från den första månaden:</span><span class="sxs-lookup"><span data-stu-id="392f9-214">To get the top 10 records in the table "fare" from the first month:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

<span data-ttu-id="392f9-215">Det vara bra att spara poster till en fil för lämplig visning.</span><span class="sxs-lookup"><span data-stu-id="392f9-215">It is often useful to save the records to a file for convenient viewing.</span></span> <span data-ttu-id="392f9-216">En mindre ändring i ovanstående frågan åstadkommer detta:</span><span class="sxs-lookup"><span data-stu-id="392f9-216">A small change to the above query accomplishes this:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a><span data-ttu-id="392f9-217">Undersökning: Visa antalet poster i var och en av de 12 partitionerna</span><span class="sxs-lookup"><span data-stu-id="392f9-217">Exploration: View the number of records in each of the 12 partitions</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-218">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-218">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-219">Intressanta är hur antalet turer varierar under kalenderåret.</span><span class="sxs-lookup"><span data-stu-id="392f9-219">Of interest is the how the number of trips varies during the calendar year.</span></span> <span data-ttu-id="392f9-220">Gruppera efter månad kan vi se hur den här distributionen av resor ser ut.</span><span class="sxs-lookup"><span data-stu-id="392f9-220">Grouping by month allows us to see what this distribution of trips looks like.</span></span>

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

<span data-ttu-id="392f9-221">Detta ger oss utdata:</span><span class="sxs-lookup"><span data-stu-id="392f9-221">This gives us the output :</span></span>

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

<span data-ttu-id="392f9-222">Här är den första kolumnen är månaden och andra antalet turer för den månaden.</span><span class="sxs-lookup"><span data-stu-id="392f9-222">Here, the first column is the month and the second is the number of trips for that month.</span></span>

<span data-ttu-id="392f9-223">Vi kan även räkna antalet poster i vår datauppsättning för kommunikation med följande kommando i Kommandotolken Hive directory.</span><span class="sxs-lookup"><span data-stu-id="392f9-223">We can also count the total number of records in our trip data set by issuing the following command at the Hive directory prompt.</span></span>

    hive -e "select count(*) from nyctaxidb.trip;"

<span data-ttu-id="392f9-224">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="392f9-224">This yields:</span></span>

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

<span data-ttu-id="392f9-225">Med kommandon som liknar de som visas för datauppsättningen resan, kan vi utfärda Hive-frågor från Hive directory prompten för datauppsättningen avgiften att validera antalet poster.</span><span class="sxs-lookup"><span data-stu-id="392f9-225">Using commands similar to those shown for the trip data set, we can issue Hive queries from the Hive directory prompt for the fare data set to validate the number of records.</span></span>

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

<span data-ttu-id="392f9-226">Detta ger oss utdata:</span><span class="sxs-lookup"><span data-stu-id="392f9-226">This gives us the output:</span></span>

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

<span data-ttu-id="392f9-227">Observera att exakt samma antal turer månaden returneras för både datamängder.</span><span class="sxs-lookup"><span data-stu-id="392f9-227">Note that the exact same number of trips per month is returned for both data sets.</span></span> <span data-ttu-id="392f9-228">Detta ger första verifieringen att data har lästs in korrekt.</span><span class="sxs-lookup"><span data-stu-id="392f9-228">This provides the first validation that the data has been loaded correctly.</span></span>

<span data-ttu-id="392f9-229">Räknar antalet poster i datauppsättningen avgiften kan göras med kommandot nedan i Hive directory-fråga:</span><span class="sxs-lookup"><span data-stu-id="392f9-229">Counting the total number of records in the fare data set can be done using the command below from the Hive directory prompt:</span></span>

    hive -e "select count(*) from nyctaxidb.fare;"

<span data-ttu-id="392f9-230">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="392f9-230">This yields :</span></span>

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

<span data-ttu-id="392f9-231">Det totala antalet poster i båda tabellerna är också samma.</span><span class="sxs-lookup"><span data-stu-id="392f9-231">The total number of records in both tables is also the same.</span></span> <span data-ttu-id="392f9-232">Detta ger en andra verifiering att data har lästs in korrekt.</span><span class="sxs-lookup"><span data-stu-id="392f9-232">This provides a second validation that the data has been loaded correctly.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="392f9-233">Undersökning: Resa distribution av medallion</span><span class="sxs-lookup"><span data-stu-id="392f9-233">Exploration: Trip distribution by medallion</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-234">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-234">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-235">Det här exemplet identifierar medallion (taxi numbers) med mer än 100 resor inom en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="392f9-235">This example identifies the medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="392f9-236">Frågan fördelar från partitionerad tabellåtkomst eftersom den är villkorad av variabeln partition **månad**.</span><span class="sxs-lookup"><span data-stu-id="392f9-236">The query benefits from the partitioned table access since it is conditioned by the partition variable **month**.</span></span> <span data-ttu-id="392f9-237">Frågeresultatet skrivs till en lokal fil queryoutput.tsv i `C:\temp` i huvudnod.</span><span class="sxs-lookup"><span data-stu-id="392f9-237">The query results are written to a local file queryoutput.tsv in `C:\temp` on the head node.</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="392f9-238">Här är innehållet *exempel\_hive\_resa\_antal\_av\_medallion.hql* filen för inspektion.</span><span class="sxs-lookup"><span data-stu-id="392f9-238">Here is the content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="392f9-239">Medallion i datauppsättningen NYC taxi identifierar en unik CAB-fil.</span><span class="sxs-lookup"><span data-stu-id="392f9-239">The medallion in the NYC taxi data set identifies a unique cab.</span></span> <span data-ttu-id="392f9-240">Vi kan identifiera vilka CAB-filer är ”upptagna” genom att fråga vilka som gjorts mer än ett visst antal turer under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="392f9-240">We can identify which cabs are "busy" by asking which ones made more than a certain number of trips in a particular time period.</span></span> <span data-ttu-id="392f9-241">I följande exempel identifierar CAB-filer som gjorts flera hundra resor i den första tre månader och sparar frågan resultatet till en lokal fil, C:\temp\queryoutput.tsv.</span><span class="sxs-lookup"><span data-stu-id="392f9-241">The following example identifies cabs that made more than a hundred trips in the first three months, and saves the query results to a local file, C:\temp\queryoutput.tsv.</span></span>

<span data-ttu-id="392f9-242">Här är innehållet *exempel\_hive\_resa\_antal\_av\_medallion.hql* filen för inspektion.</span><span class="sxs-lookup"><span data-stu-id="392f9-242">Here is the content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="392f9-243">Utfärda kommandot nedan Hive directory-prompten:</span><span class="sxs-lookup"><span data-stu-id="392f9-243">From the Hive directory prompt, issue the command below :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="392f9-244">Undersökning: Resa distribution av medallion och hack_license</span><span class="sxs-lookup"><span data-stu-id="392f9-244">Exploration: Trip distribution by medallion and hack_license</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-245">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-245">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-246">När du utforskar en datamängd, vill vi ofta undersöka antalet co-förekomster av grupper av värden.</span><span class="sxs-lookup"><span data-stu-id="392f9-246">When exploring a dataset, we frequently want to examine the number of co-occurences of groups of values.</span></span> <span data-ttu-id="392f9-247">Det här avsnittet ger ett exempel på hur du gör detta för CAB-filer och drivrutiner.</span><span class="sxs-lookup"><span data-stu-id="392f9-247">This section provide an example of how to do this for cabs and drivers.</span></span>

<span data-ttu-id="392f9-248">Den *exempel\_hive\_resa\_antal\_av\_medallion\_license.hql* filgrupper avgiften datamängden på ”medallion” och ”hack_license” och Returnerar antal varje kombination.</span><span class="sxs-lookup"><span data-stu-id="392f9-248">The *sample\_hive\_trip\_count\_by\_medallion\_license.hql* file groups the fare data set on "medallion" and "hack_license" and returns counts of each combination.</span></span> <span data-ttu-id="392f9-249">Nedan visas dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="392f9-249">Below are its contents.</span></span>

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

<span data-ttu-id="392f9-250">Den här frågan returnerar drivrutinen kombinationer sorterade efter fallande antalet turer och CAB-filen.</span><span class="sxs-lookup"><span data-stu-id="392f9-250">This query returns cab and particular driver combinations ordered by descending number of trips.</span></span>

<span data-ttu-id="392f9-251">Från Kommandotolken Hive directory kör du:</span><span class="sxs-lookup"><span data-stu-id="392f9-251">From the Hive directory prompt, run :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="392f9-252">Frågeresultatet skrivs till en lokal fil C:\temp\queryoutput.tsv.</span><span class="sxs-lookup"><span data-stu-id="392f9-252">The query results are written to a local file C:\temp\queryoutput.tsv.</span></span>

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a><span data-ttu-id="392f9-253">Undersökning: Utvärdera data quality genom att söka efter ogiltig longitud/latitud poster</span><span class="sxs-lookup"><span data-stu-id="392f9-253">Exploration: Assessing data quality by checking for invalid longitude/latitude records</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-254">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-254">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-255">Ett av undersökande dataanalys vanliga mål är att filtrera ut ogiltiga eller felaktiga poster.</span><span class="sxs-lookup"><span data-stu-id="392f9-255">A common objective of exploratory data analysis is to weed out invalid or bad records.</span></span> <span data-ttu-id="392f9-256">Exemplet i det här avsnittet anger om fälten longituden eller latituden innehålla ett värde långt utanför området NYC.</span><span class="sxs-lookup"><span data-stu-id="392f9-256">The example in this section determines whether either the longitude or latitude fields contain a value far outside the NYC area.</span></span> <span data-ttu-id="392f9-257">Eftersom det är troligt att dessa poster har en felaktig longitud-latitudvärden som vi vill ta bort dem från alla data som ska användas för modellering.</span><span class="sxs-lookup"><span data-stu-id="392f9-257">Since it is likely that such records have an erroneous longitude-latitude values, we want to eliminate them from any data that is to be used for modeling.</span></span>

<span data-ttu-id="392f9-258">Här är innehållet *exempel\_hive\_kvalitet\_assessment.hql* filen för inspektion.</span><span class="sxs-lookup"><span data-stu-id="392f9-258">Here is the content of *sample\_hive\_quality\_assessment.hql* file for inspection.</span></span>

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


<span data-ttu-id="392f9-259">Från Kommandotolken Hive directory kör du:</span><span class="sxs-lookup"><span data-stu-id="392f9-259">From the Hive directory prompt, run :</span></span>

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

<span data-ttu-id="392f9-260">Den *-S* argumentet som ingår i det här kommandot förhindrar status skärmen utskriften Hive kartan/minska jobb.</span><span class="sxs-lookup"><span data-stu-id="392f9-260">The *-S* argument included in this command suppresses the status screen printout of the Hive Map/Reduce jobs.</span></span> <span data-ttu-id="392f9-261">Detta är användbart eftersom det gör skärmen utskrift av Hive-frågan mer lättläst.</span><span class="sxs-lookup"><span data-stu-id="392f9-261">This is useful because it makes the screen print of the Hive query output more readable.</span></span>

### <a name="exploration-binary-class-distributions-of-trip-tips"></a><span data-ttu-id="392f9-262">Undersökning: Binära klassen distributioner av resa tips</span><span class="sxs-lookup"><span data-stu-id="392f9-262">Exploration: Binary class distributions of trip tips</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-263">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-263">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-264">För binära klassificeringsproblem som beskrivs i den [exempel på uppgifter som förutsägelse](machine-learning-data-science-process-hive-walkthrough.md#mltasks) avsnittet är det bra att känna av om ett tips gavs eller inte.</span><span class="sxs-lookup"><span data-stu-id="392f9-264">For the binary classification problem outlined in the [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section, it is useful to know whether a tip was given or not.</span></span> <span data-ttu-id="392f9-265">Den här distributionen av tips är binär:</span><span class="sxs-lookup"><span data-stu-id="392f9-265">This distribution of tips is binary:</span></span>

* <span data-ttu-id="392f9-266">Tips angivna (klass 1, tips\_belopp > 0)</span><span class="sxs-lookup"><span data-stu-id="392f9-266">tip given(Class 1, tip\_amount > $0)</span></span>  
* <span data-ttu-id="392f9-267">Inget tips (0-klassen, tips\_belopp = 0).</span><span class="sxs-lookup"><span data-stu-id="392f9-267">no tip (Class 0, tip\_amount = $0).</span></span>

<span data-ttu-id="392f9-268">Den *exempel\_hive\_lutad\_frequencies.hql* -exempelfilen nedan gör detta.</span><span class="sxs-lookup"><span data-stu-id="392f9-268">The *sample\_hive\_tipped\_frequencies.hql* file shown below does this.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

<span data-ttu-id="392f9-269">Från Kommandotolken Hive directory kör du:</span><span class="sxs-lookup"><span data-stu-id="392f9-269">From the Hive directory prompt, run:</span></span>

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a><span data-ttu-id="392f9-270">Undersökning: Klassen distributioner i inställningen för multiklass-baserad</span><span class="sxs-lookup"><span data-stu-id="392f9-270">Exploration: Class distributions in the multiclass setting</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-271">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-271">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-272">För multiklass-baserad klassificering problemet som beskrivs i den [exempel på uppgifter som förutsägelse](machine-learning-data-science-process-hive-walkthrough.md#mltasks) avsnittet datamängden också lämpar sig för en fysisk klassificering där vi vill förutsäga mängden tips angivna.</span><span class="sxs-lookup"><span data-stu-id="392f9-272">For the multiclass classification problem outlined in the [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section this data set also lends itself to a natural classification where we would like to predict the amount of the tips given.</span></span> <span data-ttu-id="392f9-273">Vi kan använda lagerplatser för att definiera tips intervall i frågan.</span><span class="sxs-lookup"><span data-stu-id="392f9-273">We can use bins to define tip ranges in the query.</span></span> <span data-ttu-id="392f9-274">Om du vill hämta klassen distributioner för de olika tips intervall, vi använder den *exempel\_hive\_tips\_intervallet\_frequencies.hql* fil.</span><span class="sxs-lookup"><span data-stu-id="392f9-274">To get the class distributions for the various tip ranges, we use the *sample\_hive\_tip\_range\_frequencies.hql* file.</span></span> <span data-ttu-id="392f9-275">Nedan visas dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="392f9-275">Below are its contents.</span></span>

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

<span data-ttu-id="392f9-276">Kör följande kommando från kommandoraden för Hadoop-konsolen:</span><span class="sxs-lookup"><span data-stu-id="392f9-276">Run the following command from Hadoop Command Line console:</span></span>

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a><span data-ttu-id="392f9-277">Undersökning: Compute direkt avståndet mellan två longitud latitud-platser</span><span class="sxs-lookup"><span data-stu-id="392f9-277">Exploration: Compute Direct Distance Between Two Longitude-Latitude Locations</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-278">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-278">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-279">Om du har ett mått på direkt avståndet kan oss att ta reda på avvikelse mellan det och det faktiska resa avståndet.</span><span class="sxs-lookup"><span data-stu-id="392f9-279">Having a measure of the direct distance allows us to find out the discrepancy between it and the actual trip distance.</span></span> <span data-ttu-id="392f9-280">Vi motivera funktionen genom att peka att passagerare kanske mindre troligt att tips om de ta reda på att drivrutinen avsiktligt vidtagit dem med en mycket längre väg.</span><span class="sxs-lookup"><span data-stu-id="392f9-280">We motivate this feature by pointing out that a passenger might be less likely to tip if they figure out that the driver has intentionally taken them by a much longer route.</span></span>

<span data-ttu-id="392f9-281">Se en jämförelse mellan faktiska resa avstånd och [Haversine avstånd](http://en.wikipedia.org/wiki/Haversine_formula) mellan två longitud latitud punkter (”” storcirkelavståndet), vi använda trigonometriska funktioner som finns i Hive, därför:</span><span class="sxs-lookup"><span data-stu-id="392f9-281">To see the comparison between actual trip distance and the [Haversine distance](http://en.wikipedia.org/wiki/Haversine_formula) between two longitude-latitude points (the "great circle" distance), we use the trigonometric functions available within Hive, thus :</span></span>

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

<span data-ttu-id="392f9-282">R är radius jordens i mil i frågan ovan och pi konverteras till radianer.</span><span class="sxs-lookup"><span data-stu-id="392f9-282">In the query above, R is the radius of the Earth in miles, and pi is converted to radians.</span></span> <span data-ttu-id="392f9-283">Observera att longitud latitud punkter ”filtreras” för att ta bort värden som är långt från området NYC.</span><span class="sxs-lookup"><span data-stu-id="392f9-283">Note that the longitude-latitude points are "filtered" to remove values that are far from the NYC area.</span></span>

<span data-ttu-id="392f9-284">I det här fallet skriva vi våra resultat till en katalog med namnet ”queryoutputdir”.</span><span class="sxs-lookup"><span data-stu-id="392f9-284">In this case, we write our results to a directory called "queryoutputdir".</span></span> <span data-ttu-id="392f9-285">De kommandon som visas nedan skapar den här målkatalogen först och kör sedan kommandot Hive.</span><span class="sxs-lookup"><span data-stu-id="392f9-285">The sequence of commands shown below first creates this output directory, and then runs the Hive command.</span></span>

<span data-ttu-id="392f9-286">Från Kommandotolken Hive directory kör du:</span><span class="sxs-lookup"><span data-stu-id="392f9-286">From the Hive directory prompt, run:</span></span>

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


<span data-ttu-id="392f9-287">Frågeresultatet skrivs till 9 Azure BLOB ***queryoutputdir/000000\_0*** till ***queryoutputdir/000008\_0*** under standardbehållaren för Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="392f9-287">The query results are written to 9 Azure blobs ***queryoutputdir/000000\_0*** to  ***queryoutputdir/000008\_0*** under the default container of the Hadoop cluster.</span></span>

<span data-ttu-id="392f9-288">Om du vill visa storleken på enskilda blobbar kör vi följande kommando från katalogen Hive-prompten:</span><span class="sxs-lookup"><span data-stu-id="392f9-288">To see the size of the individual blobs, we run the following command from the Hive directory prompt :</span></span>

    hdfs dfs -ls wasb:///queryoutputdir

<span data-ttu-id="392f9-289">Om du vill visa innehållet i en viss fil, säg 000000\_0, använder vi Hadoop's `copyToLocal` kommandot därför.</span><span class="sxs-lookup"><span data-stu-id="392f9-289">To see the contents of a given file, say 000000\_0, we use Hadoop's `copyToLocal` command, thus.</span></span>

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> <span data-ttu-id="392f9-290">`copyToLocal`rekommenderas inte för användning med dem och kan ta mycket lång tid för stora filer.</span><span class="sxs-lookup"><span data-stu-id="392f9-290">`copyToLocal` can be very slow for large files, and is not recommended for use with them.</span></span>  
> 
> 

<span data-ttu-id="392f9-291">En stor fördel med dessa data finns i en Azure blob är att vi kan utforska data i Azure Machine Learning med hjälp av den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="392f9-291">A key advantage of having this data reside in an Azure blob is that we may explore the data within Azure Machine Learning using the [Import Data][import-data] module.</span></span>

## <span data-ttu-id="392f9-292"><a name="#downsample"></a>Ned exempel data och bygga modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="392f9-292"><a name="#downsample"></a>Down sample data and build models in Azure Machine Learning</span></span>
> [!NOTE]
> <span data-ttu-id="392f9-293">Detta är vanligtvis en **Data forskare** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="392f9-293">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="392f9-294">När analysfasen undersökande data är vi nu redo att ned exempel data för att skapa modeller i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="392f9-294">After the exploratory data analysis phase, we are now ready to down sample the data for building models in Azure Machine Learning.</span></span> <span data-ttu-id="392f9-295">I det här avsnittet beskrivs hur du använder en Hive-fråga till exempel nedåt data, som sedan kan nås från den [importera Data] [ import-data] modul i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="392f9-295">In this section, we show how to use a Hive query to down sample the data, which is then accessed from the [Import Data][import-data] module in Azure Machine Learning.</span></span>

### <a name="down-sampling-the-data"></a><span data-ttu-id="392f9-296">Ned provtagning data</span><span class="sxs-lookup"><span data-stu-id="392f9-296">Down sampling the data</span></span>
<span data-ttu-id="392f9-297">Det finns två steg i den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="392f9-297">There are two steps in this procedure.</span></span> <span data-ttu-id="392f9-298">Först vi ansluta den **nyctaxidb.trip** och **nyctaxidb.fare** tabeller på tre nycklar som finns i alla poster: ”medallion” ”, hacka\_licens”, och ”hämtning\_datetime”.</span><span class="sxs-lookup"><span data-stu-id="392f9-298">First we join the **nyctaxidb.trip** and **nyctaxidb.fare** tables on three keys that are present in all records : "medallion", "hack\_license", and "pickup\_datetime".</span></span> <span data-ttu-id="392f9-299">Vi sedan generera en binär klassificering etikett **lutad** och en etikett för flera klassen klassificering **tips\_klassen**.</span><span class="sxs-lookup"><span data-stu-id="392f9-299">We then generate a binary classification label **tipped** and a multi-class classification label **tip\_class**.</span></span>

<span data-ttu-id="392f9-300">För att kunna använda ned exempeldata direkt från den [importera Data] [ import-data] modul i Azure Machine Learning är det nödvändigt att lagra resultaten av ovanstående fråga till en intern Hive-tabell.</span><span class="sxs-lookup"><span data-stu-id="392f9-300">To be able to use the down sampled data directly from the [Import Data][import-data] module in Azure Machine Learning, it is necessary to store the results of the above query to an internal Hive table.</span></span> <span data-ttu-id="392f9-301">I följande, vi skapa en intern Hive-tabell och fylla i dess innehåll med den sammanfogade och ned samplade data.</span><span class="sxs-lookup"><span data-stu-id="392f9-301">In what follows, we create an internal Hive table and populate its contents with the joined and down sampled data.</span></span>

<span data-ttu-id="392f9-302">Frågan använder Hive standardfunktioner direkt om du vill generera timme på dagen, veckan på året, veckodag (1 står för måndag och 7 står för söndag) från den ”hämtning\_datetime” fältet och direkt avståndet mellan platser för hämtning och dropoff.</span><span class="sxs-lookup"><span data-stu-id="392f9-302">The query applies standard Hive functions directly to generate the hour of day, week of year, weekday (1 stands for Monday, and 7 stands for Sunday) from the "pickup\_datetime" field,  and the direct distance between the pickup and dropoff locations.</span></span> <span data-ttu-id="392f9-303">Användare kan referera till [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) för en fullständig lista över dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="392f9-303">Users can refer to [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) for a complete list of such functions.</span></span>

<span data-ttu-id="392f9-304">Frågan sedan exempel ned data så att frågeresultatet passar in i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="392f9-304">The query then down samples the data so that the query results can fit into the Azure Machine Learning Studio.</span></span> <span data-ttu-id="392f9-305">Endast ca 1% av den ursprungliga datauppsättningen har importerats till Studio.</span><span class="sxs-lookup"><span data-stu-id="392f9-305">Only about 1% of the original dataset is imported into the Studio.</span></span>

<span data-ttu-id="392f9-306">Nedan visas innehållet i *exempel\_hive\_förbereda\_för\_aml\_full.hql* filen som förbereder data för modellen skapar i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="392f9-306">Below are the contents of *sample\_hive\_prepare\_for\_aml\_full.hql* file that prepares data for model building in Azure Machine Learning.</span></span>

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

        --- now insert contents of the join into the above internal table

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

<span data-ttu-id="392f9-307">Att köra den här frågan från Hive directory prompten:</span><span class="sxs-lookup"><span data-stu-id="392f9-307">To run this query, from the Hive directory prompt :</span></span>

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

<span data-ttu-id="392f9-308">Nu har vi en intern tabell ”nyctaxidb.nyctaxi_downsampled_dataset” som kan nås med hjälp av den [importera Data] [ import-data] modul från Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="392f9-308">We now have an internal table "nyctaxidb.nyctaxi_downsampled_dataset" which can be accessed using the [Import Data][import-data] module from Azure Machine Learning.</span></span> <span data-ttu-id="392f9-309">Vi kan dessutom använda denna dataset för att skapa Machine Learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="392f9-309">Furthermore, we may use this dataset for building Machine Learning models.</span></span>  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a><span data-ttu-id="392f9-310">Komma åt nedåt samplade data med modulen importera Data i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="392f9-310">Use the Import Data module in Azure Machine Learning to access the down sampled data</span></span>
<span data-ttu-id="392f9-311">Som krav för utfärdande av Hive frågor i den [importera Data] [ import-data] modulen för Azure Machine Learning vi behöver åtkomst till en Azure Machine Learning arbetsytan och åtkomst till autentiseringsuppgifterna för klustret och dess associerat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="392f9-311">As prerequisites for issuing Hive queries in the [Import Data][import-data] module of Azure Machine Learning, we need access to an Azure Machine Learning workspace and access to the credentials of the cluster and its associated storage account.</span></span>

<span data-ttu-id="392f9-312">Vissa detaljer på den [importera Data] [ import-data] modulen och parametrar för att ange:</span><span class="sxs-lookup"><span data-stu-id="392f9-312">Some details on the [Import Data][import-data] module and the parameters to input :</span></span>

<span data-ttu-id="392f9-313">**HCatalog server URI**: Om klusternamnet är abc123, så det är bara: https://abc123.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="392f9-313">**HCatalog server URI**: If the cluster name is abc123, then this is simply : https://abc123.azurehdinsight.net</span></span>

<span data-ttu-id="392f9-314">**Hadoop användarkontonamnet** : användarnamnet som valts för klustret (**inte** användarnamnet fjärråtkomst)</span><span class="sxs-lookup"><span data-stu-id="392f9-314">**Hadoop user account name** : The user name chosen for the cluster (**not** the remote access user name)</span></span>

<span data-ttu-id="392f9-315">**Kontolösenord för Hadoop ser** : lösenordet som valts för klustret (**inte** remote access-lösenordet)</span><span class="sxs-lookup"><span data-stu-id="392f9-315">**Hadoop ser account password** : The password chosen for the cluster (**not** the remote access password)</span></span>

<span data-ttu-id="392f9-316">**Platsen för utdata** : detta väljs ska Azure.</span><span class="sxs-lookup"><span data-stu-id="392f9-316">**Location of output data** : This is chosen to be Azure.</span></span>

<span data-ttu-id="392f9-317">**Azure lagringskontonamnet** : namnet på standardkontot för lagring som är associerade med klustret.</span><span class="sxs-lookup"><span data-stu-id="392f9-317">**Azure storage account name** : Name of the default storage account associated with the cluster.</span></span>

<span data-ttu-id="392f9-318">**Azure behållarnamn** : Detta är behållare för standardnamnet för klustret och är vanligtvis samma som klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="392f9-318">**Azure container name** : This is the default container name for the cluster, and is typically the same as the cluster name.</span></span> <span data-ttu-id="392f9-319">Detta är bara abc123 för ett kluster som heter ”abc123”.</span><span class="sxs-lookup"><span data-stu-id="392f9-319">For a cluster called "abc123", this is just abc123.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="392f9-320">**Alla tabeller som vi vill fråga med hjälp av den [importera Data] [ import-data] modul i Azure Machine Learning måste vara en intern tabell.**</span><span class="sxs-lookup"><span data-stu-id="392f9-320">**Any table we wish to query using the [Import Data][import-data] module in Azure Machine Learning must be an internal table.**</span></span> <span data-ttu-id="392f9-321">Ett tips för att avgöra om en tabell T i en databas D.db är en intern tabell är som följer.</span><span class="sxs-lookup"><span data-stu-id="392f9-321">A tip for determining if a table T in a database D.db is an internal table is as follows.</span></span>
> 
> 

<span data-ttu-id="392f9-322">Utfärda kommandot Hive directory-prompten:</span><span class="sxs-lookup"><span data-stu-id="392f9-322">From the Hive directory prompt, issue the command :</span></span>

    hdfs dfs -ls wasb:///D.db/T

<span data-ttu-id="392f9-323">Om tabellen är en intern tabell och det fylls, måste innehållet visas här.</span><span class="sxs-lookup"><span data-stu-id="392f9-323">If the table is an internal table and it is populated, its contents must show here.</span></span> <span data-ttu-id="392f9-324">Ett annat sätt att avgöra om en tabell är en intern tabell är att använda Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="392f9-324">Another way to determine whether a table is an internal table is to use the Azure Storage Explorer.</span></span> <span data-ttu-id="392f9-325">Använd den för att navigera till behållaren för standardnamnet på klustret och sedan filtrera efter tabellens namn.</span><span class="sxs-lookup"><span data-stu-id="392f9-325">Use it to navigate to the default container name of the cluster, and then filter by the table name.</span></span> <span data-ttu-id="392f9-326">Det här bekräftar att den är en intern tabell om tabellen och dess innehåll visas.</span><span class="sxs-lookup"><span data-stu-id="392f9-326">If the table and its contents show up, this confirms that it is an internal table.</span></span>

<span data-ttu-id="392f9-327">Här är en ögonblicksbild av Hive-fråga och [importera Data] [ import-data] modulen:</span><span class="sxs-lookup"><span data-stu-id="392f9-327">Here is a snapshot of the Hive query and the [Import Data][import-data] module:</span></span>

![Hive-fråga för modulen importera Data](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

<span data-ttu-id="392f9-329">Observera att eftersom vår ned samplade data finns i standardbehållaren resulterande Hive-frågan från Azure Machine Learning är mycket enkelt och bara en ”Välj * från nyctaxidb.nyctaxi\_upplösning\_data”.</span><span class="sxs-lookup"><span data-stu-id="392f9-329">Note that since our down sampled data resides in the default container, the resulting Hive query from Azure Machine Learning is very simple and is just a "SELECT * FROM nyctaxidb.nyctaxi\_downsampled\_data".</span></span>

<span data-ttu-id="392f9-330">Dataset kan nu användas som utgångspunkt för att skapa Machine Learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="392f9-330">The dataset may now be used as the starting point for building Machine Learning models.</span></span>

### <span data-ttu-id="392f9-331"><a name="mlmodel"></a>Bygga modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="392f9-331"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="392f9-332">Vi kan nu fortsätta till modellskapandet och distribution av modellen i [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="392f9-332">We are now able to proceed to model building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="392f9-333">Data är redo för att vi ska använda i adressering förutsägelse problemen som anges ovan:</span><span class="sxs-lookup"><span data-stu-id="392f9-333">The data is ready for us to use in addressing the prediction problems identified above:</span></span>

<span data-ttu-id="392f9-334">**1. Binär klassificering**: för att förutsäga om huruvida ett tips har betalat för en resa.</span><span class="sxs-lookup"><span data-stu-id="392f9-334">**1. Binary classification**: To predict whether or not a tip was paid for a trip.</span></span>

<span data-ttu-id="392f9-335">**Deltagaren används:** Two-class logistic regression</span><span class="sxs-lookup"><span data-stu-id="392f9-335">**Learner used:** Two-class logistic regression</span></span>

<span data-ttu-id="392f9-336">a.</span><span class="sxs-lookup"><span data-stu-id="392f9-336">a.</span></span> <span data-ttu-id="392f9-337">För det här problemet är våra mål (eller klassen) etiketten ”lutad”.</span><span class="sxs-lookup"><span data-stu-id="392f9-337">For this problem, our target (or class) label is "tipped".</span></span> <span data-ttu-id="392f9-338">Vår ursprungliga ned provtagning datauppsättning har några kolumner som är mål minnesläckor för klassificering experimentet.</span><span class="sxs-lookup"><span data-stu-id="392f9-338">Our original down-sampled dataset has a few columns that are target leaks for this classification experiment.</span></span> <span data-ttu-id="392f9-339">I synnerhet: tips\_klassen, tips\_belopp och totalt\_belopp avslöjar information om det mål som inte är tillgänglig vid testning tid.</span><span class="sxs-lookup"><span data-stu-id="392f9-339">In particular : tip\_class, tip\_amount, and total\_amount reveal information about the target label that is not available at testing time.</span></span> <span data-ttu-id="392f9-340">Vi ta bort dessa kolumner från beräkningen som använder den [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="392f9-340">We remove these columns from consideration using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="392f9-341">Ögonblicksbilden nedan visar vårt experiment att förutsäga huruvida ett tips har betalat för en given resa.</span><span class="sxs-lookup"><span data-stu-id="392f9-341">The snapshot below shows our experiment to predict whether or not a tip was paid for a given trip.</span></span>

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

<span data-ttu-id="392f9-343">b.</span><span class="sxs-lookup"><span data-stu-id="392f9-343">b.</span></span> <span data-ttu-id="392f9-344">Det här experimentet har våra mål etikett distributioner ungefär 1:1.</span><span class="sxs-lookup"><span data-stu-id="392f9-344">For this experiment, our target label distributions were roughly 1:1.</span></span>

<span data-ttu-id="392f9-345">Ögonblicksbilden nedan visar fördelningen av tips klassen etiketter för binära klassificeringsproblem.</span><span class="sxs-lookup"><span data-stu-id="392f9-345">The snapshot below shows the distribution of tip class labels for the binary classification problem.</span></span>

![Distribution av tips klassen etiketter](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

<span data-ttu-id="392f9-347">Därför kan får vi en AUC av 0.987 som visas i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="392f9-347">As a result, we obtain an AUC of 0.987 as shown in the figure below.</span></span>

![AUC värde](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

<span data-ttu-id="392f9-349">**2. Multiklass-baserad klassificering**: att förutsäga intervallet av tips för resan, med hjälp av de tidigare definierade klasserna.</span><span class="sxs-lookup"><span data-stu-id="392f9-349">**2. Multiclass classification**: To predict the range of tip amounts paid for the trip, using the previously defined classes.</span></span>

<span data-ttu-id="392f9-350">**Deltagaren används:** multiklass-baserad logistic regression</span><span class="sxs-lookup"><span data-stu-id="392f9-350">**Learner used:** Multiclass logistic regression</span></span>

<span data-ttu-id="392f9-351">a.</span><span class="sxs-lookup"><span data-stu-id="392f9-351">a.</span></span> <span data-ttu-id="392f9-352">För det här problemet är vårt mål (eller klassen) etiketten ”tips\_klass” vilket kan ta en av fem värden (0,1,2,3,4).</span><span class="sxs-lookup"><span data-stu-id="392f9-352">For this problem, our target (or class) label is "tip\_class" which can take one of five values (0,1,2,3,4).</span></span> <span data-ttu-id="392f9-353">Som i fallet med binär klassificering har vi några kolumner som är mål minnesläckor för experimentet.</span><span class="sxs-lookup"><span data-stu-id="392f9-353">As in the binary classification case, we have a few columns that are target leaks for this experiment.</span></span> <span data-ttu-id="392f9-354">I synnerhet: lutad, tips\_totala angivet\_belopp avslöjar information om det mål som inte är tillgänglig vid testning tid.</span><span class="sxs-lookup"><span data-stu-id="392f9-354">In particular : tipped, tip\_amount, total\_amount reveal information about the target label that is not available at testing time.</span></span> <span data-ttu-id="392f9-355">Vi ta bort dessa kolumner med hjälp av den [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="392f9-355">We remove these columns using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="392f9-356">Ögonblicksbilden nedan visar vårt experiment att förutsäga ett tips är troligt att hamna i vilka bin (klass 0: tips = 0, klass 1: tips > 0 och tips < = $5, klass 2: tips > 5 och tips < = $10, klass 3: tips > 10 och tips < = $20 Klass 4: tips > 20)</span><span class="sxs-lookup"><span data-stu-id="392f9-356">The snapshot below shows our experiment to predict in which bin a tip is likely to fall ( Class 0: tip = $0, class 1 : tip > $0 and tip <= $5, Class 2 : tip > $5 and tip <= $10, Class 3 : tip > $10 and tip <= $20, Class 4 : tip > $20)</span></span>

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

<span data-ttu-id="392f9-358">Nu visas hur våra faktiska test klassen distribution ser ut.</span><span class="sxs-lookup"><span data-stu-id="392f9-358">We now show what our actual test class distribution looks like.</span></span> <span data-ttu-id="392f9-359">Vi finns att medan klassen 0 och 1 för klassen är vanliga, andra klasser sällsynta.</span><span class="sxs-lookup"><span data-stu-id="392f9-359">We see that while Class 0 and Class 1 are prevalent, the other classes are rare.</span></span>

![Testa klassen distribution](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

<span data-ttu-id="392f9-361">b.</span><span class="sxs-lookup"><span data-stu-id="392f9-361">b.</span></span> <span data-ttu-id="392f9-362">Det här experimentet använder vi en förvirring matris för att titta på vår prognosens noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="392f9-362">For this experiment, we use a confusion matrix to look at our prediction accuracies.</span></span> <span data-ttu-id="392f9-363">Detta visas nedan.</span><span class="sxs-lookup"><span data-stu-id="392f9-363">This is shown below.</span></span>

![Förvirring matris](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

<span data-ttu-id="392f9-365">Observera att vår klass noggrannhet på vanliga klasser är ganska bra, modellen utför inte bra på ”learning” på sällsynta klasser.</span><span class="sxs-lookup"><span data-stu-id="392f9-365">Note that while our class accuracies on the prevalent classes is quite good, the model does not do a good job of "learning" on the rarer classes.</span></span>

<span data-ttu-id="392f9-366">**3. Regression uppgiften**: att förutsäga mängden tips för en resa.</span><span class="sxs-lookup"><span data-stu-id="392f9-366">**3. Regression task**: To predict the amount of tip paid for a trip.</span></span>

<span data-ttu-id="392f9-367">**Deltagaren används:** Boosted beslutsträdet</span><span class="sxs-lookup"><span data-stu-id="392f9-367">**Learner used:** Boosted decision tree</span></span>

<span data-ttu-id="392f9-368">a.</span><span class="sxs-lookup"><span data-stu-id="392f9-368">a.</span></span> <span data-ttu-id="392f9-369">För det här problemet är vårt mål (eller klassen) etiketten ”tips\_belopp”.</span><span class="sxs-lookup"><span data-stu-id="392f9-369">For this problem, our target (or class) label is "tip\_amount".</span></span> <span data-ttu-id="392f9-370">I det här fallet är vårt mål minnesläckor: lutad, tips\_klass, totalt antal\_belopp; dessa variabler avslöja information om hur tips som är vanligtvis inte tillgängliga vid testning tid.</span><span class="sxs-lookup"><span data-stu-id="392f9-370">Our target leaks in this case are : tipped, tip\_class, total\_amount ; all these variables reveal information about the tip amount that is typically unavailable at testing time.</span></span> <span data-ttu-id="392f9-371">Vi ta bort dessa kolumner med hjälp av den [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="392f9-371">We remove these columns using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="392f9-372">Ögonblicksbild belows visar vårt experiment att förutsäga mängden angivna tips.</span><span class="sxs-lookup"><span data-stu-id="392f9-372">The snapshot belows shows our experiment to predict the amount of the given tip.</span></span>

![Experiment ögonblicksbild](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

<span data-ttu-id="392f9-374">b.</span><span class="sxs-lookup"><span data-stu-id="392f9-374">b.</span></span> <span data-ttu-id="392f9-375">Vi mäter noggrannhet med vår prognoser för genom att titta på kvadratfel i förutsägelser, bestämningskoefficienten och liknande för regressionsproblem.</span><span class="sxs-lookup"><span data-stu-id="392f9-375">For regression problems, we measure the accuracies of our prediction by looking at the squared error in the predictions, the coefficient of determination, and the like.</span></span> <span data-ttu-id="392f9-376">Vi visas dessa nedan.</span><span class="sxs-lookup"><span data-stu-id="392f9-376">We show these below.</span></span>

![Förutsägelse statistik](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

<span data-ttu-id="392f9-378">Vi se att om bestämningskoefficienten är 0.709 förklaras innebär ungefär 71% varians med vår modell koefficienter.</span><span class="sxs-lookup"><span data-stu-id="392f9-378">We see that about the coefficient of determination is 0.709, implying about 71% of the variance is explained by our model coefficients.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="392f9-379">Om du vill veta mer om Azure Machine Learning och hur du använder den, se [vad är Machine Learning?](machine-learning-what-is-machine-learning.md).</span><span class="sxs-lookup"><span data-stu-id="392f9-379">To learn more about Azure Machine Learning and how to access and use it, please refer to [What's Machine Learning?](machine-learning-what-is-machine-learning.md).</span></span> <span data-ttu-id="392f9-380">En resurs som är användbar för att spela upp med en massa Machine Learning-experiment i Azure Machine Learning är den [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="392f9-380">A very useful resource for playing with a bunch of Machine Learning experiments on Azure Machine Learning is the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span></span> <span data-ttu-id="392f9-381">Galleriet omfattar en alla experiment och ger en omfattande introduktion till nya funktioner i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="392f9-381">The Gallery covers a gamut of experiments and provides a thorough introduction into the range of capabilities of Azure Machine Learning.</span></span>
> 
> 

## <a name="license-information"></a><span data-ttu-id="392f9-382">Licensinformationen</span><span class="sxs-lookup"><span data-stu-id="392f9-382">License Information</span></span>
<span data-ttu-id="392f9-383">Den här genomgången exempel och dess tillhörande skript som delas av Microsoft under MIT-licensen.</span><span class="sxs-lookup"><span data-stu-id="392f9-383">This sample walkthrough and its accompanying scripts are shared by Microsoft under the MIT license.</span></span> <span data-ttu-id="392f9-384">Kontrollera filen LICENSE.txt i katalogen exempelkoden på GitHub för mer information.</span><span class="sxs-lookup"><span data-stu-id="392f9-384">Please check the LICENSE.txt file in in the directory of the sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="392f9-385">Referenser</span><span class="sxs-lookup"><span data-stu-id="392f9-385">References</span></span>
<span data-ttu-id="392f9-386">• [Andrés Monroy NYC Taxi resor hämtningssidan](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="392f9-386">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="392f9-387">• [FOILing NYC Taxitransport resa Data av Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="392f9-387">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="392f9-388">• [NYC Taxi och Limousine kommissionens forskning och statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="392f9-388">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
