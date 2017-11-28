---
title: "aaaAnalyze svarta fördröjning data med Hadoop i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse en Windows PowerShell-skript toocreate ett HDInsight-kluster kör ett Hive-jobb, köra ett jobb för Sqoop och ta bort hello klustret."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00e26aa9-82fb-4dbe-b87d-ffe8e39a5412
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6ebaee65d9b270e5dc2141dd1265011d372f497d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a><span data-ttu-id="a03b5-103">Analysera svarta fördröjning data med hjälp av Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="a03b5-103">Analyze flight delay data by using Hive in HDInsight</span></span>
<span data-ttu-id="a03b5-104">Hive ger dig möjlighet att köra Hadoop MapReduce jobb via en SQL-liknande skriptspråk som kallas * [HiveQL][hadoop-hiveql]*, som kan användas mot sammanfattning, fråga och analys av stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="a03b5-104">Hive provides a means of running Hadoop MapReduce jobs through an SQL-like scripting language called *[HiveQL][hadoop-hiveql]*, which can be applied towards summarizing, querying, and analyzing large volumes of data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a03b5-105">hello kräver stegen i det här dokumentet ett Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a03b5-105">hello steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="a03b5-106">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a03b5-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a03b5-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a03b5-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="a03b5-108">Åtgärder för att arbeta med ett Linux-baserade kluster, se [analysera svarta fördröjning data med hjälp av Hive i HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a03b5-108">For steps that work with a Linux-based cluster, see [Analyze flight delay data by using Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).</span></span>

<span data-ttu-id="a03b5-109">En av hello viktiga fördelar med Azure HDInsight är hello uppdelning av datalagring och beräkning.</span><span class="sxs-lookup"><span data-stu-id="a03b5-109">One of hello major benefits of Azure HDInsight is hello separation of data storage and compute.</span></span> <span data-ttu-id="a03b5-110">HDInsight använder Azure Blob storage för lagring av data.</span><span class="sxs-lookup"><span data-stu-id="a03b5-110">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="a03b5-111">En typisk jobbet innefattar tre delar:</span><span class="sxs-lookup"><span data-stu-id="a03b5-111">A typical job involves three parts:</span></span>

1. <span data-ttu-id="a03b5-112">**Lagra data i Azure Blob storage.**</span><span class="sxs-lookup"><span data-stu-id="a03b5-112">**Store data in Azure Blob storage.**</span></span>  <span data-ttu-id="a03b5-113">Till exempel väder data, sensordata, webbloggar, och i det här fallet svarta fördröjning data sparas i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a03b5-113">For example, weather data, sensor data, web logs, and in this case, flight delay data are saved into Azure Blob storage.</span></span>
2. <span data-ttu-id="a03b5-114">**Kör jobb.**</span><span class="sxs-lookup"><span data-stu-id="a03b5-114">**Run jobs.**</span></span> <span data-ttu-id="a03b5-115">När det är tid tooprocess hello data kan du köra en Windows PowerShell-skript (eller ett klientprogram) toocreate ett HDInsight-kluster, köra jobb och ta bort hello klustret.</span><span class="sxs-lookup"><span data-stu-id="a03b5-115">When it is time tooprocess hello data, you run a Windows PowerShell script (or a client application) toocreate an HDInsight cluster, run jobs, and delete hello cluster.</span></span> <span data-ttu-id="a03b5-116">hello jobb spara utdata data tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a03b5-116">hello jobs save output data tooAzure Blob storage.</span></span> <span data-ttu-id="a03b5-117">hello utdata bevaras även efter hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="a03b5-117">hello output data is retained even after hello cluster is deleted.</span></span> <span data-ttu-id="a03b5-118">På så sätt kan du betalar bara vad du har förbrukat.</span><span class="sxs-lookup"><span data-stu-id="a03b5-118">This way, you pay for only what you have consumed.</span></span>
3. <span data-ttu-id="a03b5-119">**Hämta hello utdata från Azure Blob storage**, eller exportera hello data tooan Azure SQL-databas i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a03b5-119">**Retrieve hello output from Azure Blob storage**, or in this tutorial, export hello data tooan Azure SQL database.</span></span>

<span data-ttu-id="a03b5-120">hello följande diagram illustrerar hello scenariot och hello strukturen för den här kursen:</span><span class="sxs-lookup"><span data-stu-id="a03b5-120">hello following diagram illustrates hello scenario and hello structure of this tutorial:</span></span>

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

<span data-ttu-id="a03b5-122">Observera att hello siffrorna i hello diagram visar toohello avsnittsrubriker.</span><span class="sxs-lookup"><span data-stu-id="a03b5-122">Note that hello numbers in hello diagram correspond toohello section titles.</span></span> <span data-ttu-id="a03b5-123">**M** står för hello huvudsakliga processen.</span><span class="sxs-lookup"><span data-stu-id="a03b5-123">**M** stands for hello main process.</span></span> <span data-ttu-id="a03b5-124">**En** står för hello innehållet i hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="a03b5-124">**A** stands for hello content in hello appendix.</span></span>

<span data-ttu-id="a03b5-125">hello största delen av hello kursen visar hur toouse en Windows PowerShell-skript tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="a03b5-125">hello main portion of hello tutorial shows you how toouse one Windows PowerShell script tooperform hello following tasks:</span></span>

* <span data-ttu-id="a03b5-126">Skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a03b5-126">Create an HDInsight cluster.</span></span>
* <span data-ttu-id="a03b5-127">Köra en Hive-jobb på hello klustret toocalculate genomsnittlig fördröjningar vid flygplatser.</span><span class="sxs-lookup"><span data-stu-id="a03b5-127">Run a Hive job on hello cluster toocalculate average delays at airports.</span></span> <span data-ttu-id="a03b5-128">hello svarta fördröjning data lagras i ett Azure Blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a03b5-128">hello flight delay data is stored in an Azure Blob storage account.</span></span>
* <span data-ttu-id="a03b5-129">Kör en Sqoop jobbet tooexport hello Hive-jobbet utdata tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a03b5-129">Run a Sqoop job tooexport hello Hive job output tooan Azure SQL database.</span></span>
* <span data-ttu-id="a03b5-130">Ta bort hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a03b5-130">Delete hello HDInsight cluster.</span></span>

<span data-ttu-id="a03b5-131">I hello bilagorna, kan du hitta hello anvisningar för överföringen svarta fördröjning data, skapa/ladda upp en Hive-frågesträng och förberedde hello Sqoop jobbet hello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a03b5-131">In hello appendixes, you can find hello instructions for uploading flight delay data, creating/uploading a Hive query string, and preparing hello Azure SQL database for hello Sqoop job.</span></span>

> [!NOTE]
> <span data-ttu-id="a03b5-132">hello stegen i det här dokumentet är specifika tooWindows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a03b5-132">hello steps in this document are specific tooWindows-based HDInsight clusters.</span></span> <span data-ttu-id="a03b5-133">Åtgärder för att arbeta med ett Linux-baserade kluster, se [analysera svarta fördröjning data med Hive i HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)</span><span class="sxs-lookup"><span data-stu-id="a03b5-133">For steps that work with a Linux-based cluster, see [Analyze flight delay data using Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)</span></span>

### <a name="prerequisites"></a><span data-ttu-id="a03b5-134">Krav</span><span class="sxs-lookup"><span data-stu-id="a03b5-134">Prerequisites</span></span>
<span data-ttu-id="a03b5-135">Innan du påbörjar den här självstudien måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a03b5-135">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="a03b5-136">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="a03b5-136">**An Azure subscription**.</span></span> <span data-ttu-id="a03b5-137">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a03b5-137">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="a03b5-138">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="a03b5-138">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a03b5-139">Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017.</span><span class="sxs-lookup"><span data-stu-id="a03b5-139">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="a03b5-140">hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a03b5-140">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="a03b5-141">Följ stegen hello i [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a03b5-141">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="a03b5-142">Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a03b5-142">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

<span data-ttu-id="a03b5-143">**Filer som används i den här självstudiekursen**</span><span class="sxs-lookup"><span data-stu-id="a03b5-143">**Files used in this tutorial**</span></span>

<span data-ttu-id="a03b5-144">Den här kursen använder hello-time-prestanda för flygbolag svarta data från [forskning och innovativa tekniken Administration, Bureau transport statistik eller RITA][rita-website].</span><span class="sxs-lookup"><span data-stu-id="a03b5-144">This tutorial uses hello on-time performance of airline flight data from [Research and Innovative Technology Administration, Bureau of Transportation Statistics or RITA][rita-website].</span></span>
<span data-ttu-id="a03b5-145">En kopia av hello data har överfört tooan Azure Blob storage-behållare med hello offentlig Blob-behörighet.</span><span class="sxs-lookup"><span data-stu-id="a03b5-145">A copy of hello data has been uploaded tooan Azure Blob storage container with hello Public Blob access permission.</span></span>
<span data-ttu-id="a03b5-146">En del av PowerShell-skript kopierar hello data från hello offentlig blob-behållaren toohello standardbehållaren på klustret.</span><span class="sxs-lookup"><span data-stu-id="a03b5-146">A part of your PowerShell script copies hello data from hello public blob container toohello default blob container of your cluster.</span></span> <span data-ttu-id="a03b5-147">Hej HiveQL skript är också kopieras toohello samma Blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="a03b5-147">hello HiveQL script is also copied toohello same Blob container.</span></span>
<span data-ttu-id="a03b5-148">Om du vill toolearn hur tooget/överför hello data tooyour äger Storage-konto och hur toocreate/överför hello HiveQL skriptfil, se [bilaga A](#appendix-a) och [bilaga B](#appendix-b).</span><span class="sxs-lookup"><span data-stu-id="a03b5-148">If you want toolearn how tooget/upload hello data tooyour own Storage account, and how toocreate/upload hello HiveQL script file, see [Appendix A](#appendix-a) and [Appendix B](#appendix-b).</span></span>

<span data-ttu-id="a03b5-149">hello visas följande tabell hello-filer som används i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="a03b5-149">hello following table lists hello files used in this tutorial:</span></span>

<table border="1">
<tr><th><span data-ttu-id="a03b5-150">Filer</span><span class="sxs-lookup"><span data-stu-id="a03b5-150">Files</span></span></th><th><span data-ttu-id="a03b5-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a03b5-151">Description</span></span></th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td><span data-ttu-id="a03b5-152">Hej HiveQL skriptfilen som används av hello Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="a03b5-152">hello HiveQL script file used by hello Hive job.</span></span> <span data-ttu-id="a03b5-153">Det här skriptet har överförda tooan Azure Blob storage-konto med hello offentlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a03b5-153">This script has been uploaded tooan Azure Blob storage account with hello public access.</span></span> <span data-ttu-id="a03b5-154"><a href="#appendix-b">Bilaga B</a> har instruktioner för att förbereda och ladda upp den här filen tooyour egna Azure Blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a03b5-154"><a href="#appendix-b">Appendix B</a> has instructions on preparing and uploading this file tooyour own Azure Blob storage account.</span></span></td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td><span data-ttu-id="a03b5-155">Indata för hello Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="a03b5-155">Input data for hello Hive job.</span></span> <span data-ttu-id="a03b5-156">hello data har överförts tooan Azure Blob storage-konto med hello offentlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a03b5-156">hello data has been uploaded tooan Azure Blob storage account with hello public access.</span></span> <span data-ttu-id="a03b5-157"><a href="#appendix-a">Bilaga A</a> innehåller instruktioner hämtning av hello data och överför hello data tooyour egna Azure Blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a03b5-157"><a href="#appendix-a">Appendix A</a> has instructions on getting hello data and uploading hello data tooyour own Azure Blob storage account.</span></span></td></tr>
<tr><td><span data-ttu-id="a03b5-158">\tutorials\flightdelays\output</span><span class="sxs-lookup"><span data-stu-id="a03b5-158">\tutorials\flightdelays\output</span></span></td><td><span data-ttu-id="a03b5-159">hello Utdatasökväg för hello Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="a03b5-159">hello output path for hello Hive job.</span></span> <span data-ttu-id="a03b5-160">hello standardbehållaren används för att lagra hello utdata.</span><span class="sxs-lookup"><span data-stu-id="a03b5-160">hello default container is used for storing hello output data.</span></span></td></tr>
<tr><td><span data-ttu-id="a03b5-161">\tutorials\flightdelays\jobstatus</span><span class="sxs-lookup"><span data-stu-id="a03b5-161">\tutorials\flightdelays\jobstatus</span></span></td><td><span data-ttu-id="a03b5-162">hello Hive-jobbet status mapp på hello standardbehållaren.</span><span class="sxs-lookup"><span data-stu-id="a03b5-162">hello Hive job status folder on hello default container.</span></span></td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a><span data-ttu-id="a03b5-163">Skapa kluster och köra Hive/Sqoop jobb</span><span class="sxs-lookup"><span data-stu-id="a03b5-163">Create cluster and run Hive/Sqoop jobs</span></span>
<span data-ttu-id="a03b5-164">Hadoop MapReduce är batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="a03b5-164">Hadoop MapReduce is batch processing.</span></span> <span data-ttu-id="a03b5-165">hello de flesta kostnadseffektivt sätt toorun ett Hive-jobb är toocreate ett kluster för hello jobb och tar bort hello jobbet när hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a03b5-165">hello most cost-effective way toorun a Hive job is toocreate a cluster for hello job, and delete hello job after hello job is completed.</span></span> <span data-ttu-id="a03b5-166">hello innehåller följande skript hello hela processen.</span><span class="sxs-lookup"><span data-stu-id="a03b5-166">hello following script covers hello whole process.</span></span>
<span data-ttu-id="a03b5-167">Mer information om hur du skapar ett HDInsight-kluster och köra Hive-jobb finns [skapa Hadoop-kluster i HDInsight] [ hdinsight-provision] och [använda Hive med HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="a03b5-167">For more information on creating an HDInsight cluster and running Hive jobs, see [Create Hadoop clusters in HDInsight][hdinsight-provision] and [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

<span data-ttu-id="a03b5-168">**toorun hello Hive-frågor med Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a03b5-168">**toorun hello Hive queries by Azure PowerShell**</span></span>

1. <span data-ttu-id="a03b5-169">Skapa en Azure SQL-databasen och hello tabell för hello Sqoop jobbutdata med hello instruktionerna i [bilaga C](#appendix-c).</span><span class="sxs-lookup"><span data-stu-id="a03b5-169">Create an Azure SQL database and hello table for hello Sqoop job output by using hello instructions in [Appendix C](#appendix-c).</span></span>
2. <span data-ttu-id="a03b5-170">Öppna Windows PowerShell ISE och kör följande skript hello:</span><span class="sxs-lookup"><span data-stu-id="a03b5-170">Open Windows PowerShell ISE, and run hello following script:</span></span>

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure hello follwing variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on hello version, hello folder can be different

    ###########################################
    # (Optional) configure hello following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter hello Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use hello cluster name

    $existingSqlDatabaseTableName = "AvgDelays"
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"

    $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql"

    $jobStatusFolder = "/tutorials/flightdelays/jobstatus"

    ###########################################
    # Login
    ###########################################
    try{
        $acct = Get-AzureRmSubscription
    }
    catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionID $subscriptionID

    ###########################################
    # Create a new HDInsight cluster
    ###########################################

    # Create ARM group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create hello default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

    # Create hello HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $existingDefaultBlobContainerName

    ###########################################
    # Prepare hello HiveQL script and source data
    ###########################################

    # Create hello temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download hello sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data toodefault container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit hello Hive job
    ###########################################
    Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
    $response = Invoke-AzureRmHDInsightHiveJob `
                    -Files $hqlScriptFile `
                    -DefaultContainer $defaultBlobContainerName `
                    -DefaultStorageAccountName $defaultStorageAccountName `
                    -DefaultStorageAccountKey $defaultStorageAccountKey `
                    -StatusFolder $jobStatusFolder

    write-Host $response

    ###########################################
    # Submit hello Sqoop job
    ###########################################
    $exportDir = "wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                    -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ResourceGroupName $resourceGroupName `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -HttpCredential $httpCredential `
        -WaitTimeoutInSeconds 3600 `
        -Job $sqoopJob.JobId

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -DefaultContainer $existingDefaultBlobContainerName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    ###########################################
    # Delete hello cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. <span data-ttu-id="a03b5-171">Ansluta tooyour SQL-databas och se genomsnittlig svarta fördröjningar efter ort i hello AvgDelays tabell:</span><span class="sxs-lookup"><span data-stu-id="a03b5-171">Connect tooyour SQL database and see average flight delays by city in hello AvgDelays table:</span></span>

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <span data-ttu-id="a03b5-173"><a id="appendix-a"></a>Bilaga A – överför svarta fördröjning data tooAzure Blob storage</span><span class="sxs-lookup"><span data-stu-id="a03b5-173"><a id="appendix-a"></a>Appendix A - Upload flight delay data tooAzure Blob storage</span></span>
<span data-ttu-id="a03b5-174">Överför hello-datafilen och hello HiveQL skriptfiler (se [bilaga B](#appendix-b)) kräver lite planering.</span><span class="sxs-lookup"><span data-stu-id="a03b5-174">Uploading hello data file and hello HiveQL script files (see [Appendix B](#appendix-b)) requires some planning.</span></span> <span data-ttu-id="a03b5-175">hello idé är toostore hello-datafiler och hello HiveQL filen innan du skapar ett HDInsight-kluster och köra hello Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="a03b5-175">hello idea is toostore hello data files and hello HiveQL file before creating an HDInsight cluster and running hello Hive job.</span></span> <span data-ttu-id="a03b5-176">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="a03b5-176">You have two options:</span></span>

* <span data-ttu-id="a03b5-177">**Använd hello samma Azure Storage-konto som ska användas av hello HDInsight-kluster som hello standardfilsystem.**</span><span class="sxs-lookup"><span data-stu-id="a03b5-177">**Use hello same Azure Storage account that will be used by hello HDInsight cluster as hello default file system.**</span></span> <span data-ttu-id="a03b5-178">Eftersom hello HDInsight-kluster har hello åtkomstnyckeln för Lagringskontot, behöver du inte toomake några ytterligare ändringar.</span><span class="sxs-lookup"><span data-stu-id="a03b5-178">Because hello HDInsight cluster will have hello Storage account access key, you don't need toomake any additional changes.</span></span>
* <span data-ttu-id="a03b5-179">**Använd ett annat Azure Storage-konto från hello standardfilsystem för HDInsight-kluster.**</span><span class="sxs-lookup"><span data-stu-id="a03b5-179">**Use a different Azure Storage account from hello HDInsight cluster default file system.**</span></span> <span data-ttu-id="a03b5-180">Om så är fallet hello måste du ändra hello skapas en del av hello Windows PowerShell-skript finns i [skapa HDInsight-kluster och kör Hive/Sqoop jobb](#runjob) toolink hello Storage-konto som ett ytterligare Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a03b5-180">If this is hello case, you must modify hello creation part of hello Windows PowerShell script found in [Create HDInsight cluster and run Hive/Sqoop jobs](#runjob) toolink hello Storage account as an additional Storage account.</span></span> <span data-ttu-id="a03b5-181">Instruktioner finns i [skapa Hadoop-kluster i HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="a03b5-181">For instructions, see [Create Hadoop clusters in HDInsight][hdinsight-provision].</span></span> <span data-ttu-id="a03b5-182">Hej HDInsight-kluster känner sedan hello åtkomstnyckeln för hello Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a03b5-182">hello HDInsight cluster then knows hello access key for hello Storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="a03b5-183">Hej sökvägen för Blob-lagring för hello datafilen är hårddisken kodad i hello HiveQL skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="a03b5-183">hello Blob storage path for hello data file is hard coded in hello HiveQL script file.</span></span> <span data-ttu-id="a03b5-184">Du måste uppdatera därefter.</span><span class="sxs-lookup"><span data-stu-id="a03b5-184">You must update it accordingly.</span></span>

<span data-ttu-id="a03b5-185">**toodownload hello svarta data**</span><span class="sxs-lookup"><span data-stu-id="a03b5-185">**toodownload hello flight data**</span></span>

1. <span data-ttu-id="a03b5-186">Bläddra för[forskning och innovativa tekniken Administration, Bureau transport statistik][rita-website].</span><span class="sxs-lookup"><span data-stu-id="a03b5-186">Browse too[Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>
2. <span data-ttu-id="a03b5-187">Hello på sidan Välj hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="a03b5-187">On hello page, select hello following values:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="a03b5-188">Namn</span><span class="sxs-lookup"><span data-stu-id="a03b5-188">Name</span></span></th><th><span data-ttu-id="a03b5-189">Värde</span><span class="sxs-lookup"><span data-stu-id="a03b5-189">Value</span></span></th></tr>
    <tr><td><span data-ttu-id="a03b5-190">Filtrera år</span><span class="sxs-lookup"><span data-stu-id="a03b5-190">Filter Year</span></span></td><td><span data-ttu-id="a03b5-191">2013</span><span class="sxs-lookup"><span data-stu-id="a03b5-191">2013</span></span> </td></tr>
    <tr><td><span data-ttu-id="a03b5-192">Filtrera Period</span><span class="sxs-lookup"><span data-stu-id="a03b5-192">Filter Period</span></span></td><td><span data-ttu-id="a03b5-193">Januari</span><span class="sxs-lookup"><span data-stu-id="a03b5-193">January</span></span></td></tr>
    <tr><td><span data-ttu-id="a03b5-194">Fält</span><span class="sxs-lookup"><span data-stu-id="a03b5-194">Fields</span></span></td><td><span data-ttu-id="a03b5-195">*År*, *FlightDate*, *UniqueCarrier*, *operatör*, *FlightNum*, *OriginAirportID*, *ursprung*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, * NASDelay*, *SecurityDelay*, *LateAircraftDelay* (rensa alla andra fält)</span><span class="sxs-lookup"><span data-stu-id="a03b5-195">*Year*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (clear all other fields)</span></span></td></tr>
    </table><span data-ttu-id="a03b5-196">
3.Klicka på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="a03b5-196">
3. Click **Download**.</span></span>
<span data-ttu-id="a03b5-197">4.</span><span class="sxs-lookup"><span data-stu-id="a03b5-197">4.</span></span> <span data-ttu-id="a03b5-198">Packa upp hello filen toohello **C:\Tutorials\FlightDelay\2013Data** mapp.</span><span class="sxs-lookup"><span data-stu-id="a03b5-198">Unzip hello file toohello **C:\Tutorials\FlightDelay\2013Data** folder.</span></span> <span data-ttu-id="a03b5-199">Varje fil är en CSV-fil och är ungefär 60GB i storlek.</span><span class="sxs-lookup"><span data-stu-id="a03b5-199">Each file is a CSV file and is approximately 60GB in size.</span></span>
<span data-ttu-id="a03b5-200">5.</span><span class="sxs-lookup"><span data-stu-id="a03b5-200">5.</span></span> <span data-ttu-id="a03b5-201">Byt namn på hello toohello filnamn hello månad som den innehåller data för.</span><span class="sxs-lookup"><span data-stu-id="a03b5-201">Rename hello file toohello name of hello month that it contains data for.</span></span> <span data-ttu-id="a03b5-202">Till exempel hello-fil som innehåller hello januari data namnet *January.csv*.</span><span class="sxs-lookup"><span data-stu-id="a03b5-202">For example, hello file containing hello January data would be named *January.csv*.</span></span>
<span data-ttu-id="a03b5-203">6.</span><span class="sxs-lookup"><span data-stu-id="a03b5-203">6.</span></span> <span data-ttu-id="a03b5-204">Upprepa steg 2 och 5 toodownload en fil för varje hello 12 månader i 2013.</span><span class="sxs-lookup"><span data-stu-id="a03b5-204">Repeat steps 2 and 5 toodownload a file for each of hello 12 months in 2013.</span></span> <span data-ttu-id="a03b5-205">Du behöver minst en fil toorun hello kursen.</span><span class="sxs-lookup"><span data-stu-id="a03b5-205">You will need a minimum of one file toorun hello tutorial.</span></span>

<span data-ttu-id="a03b5-206">**tooupload hello svarta fördröjning data tooAzure Blob storage**</span><span class="sxs-lookup"><span data-stu-id="a03b5-206">**tooupload hello flight delay data tooAzure Blob storage**</span></span>

1. <span data-ttu-id="a03b5-207">Förbered hello parametrar:</span><span class="sxs-lookup"><span data-stu-id="a03b5-207">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="a03b5-208">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="a03b5-208">Variable Name</span></span></th><th><span data-ttu-id="a03b5-209">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a03b5-209">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="a03b5-210">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="a03b5-210">$storageAccountName</span></span></td><td><span data-ttu-id="a03b5-211">hello Azure Storage-konto där du vill att tooupload hello data.</span><span class="sxs-lookup"><span data-stu-id="a03b5-211">hello Azure Storage account where you want tooupload hello data to.</span></span></td></tr>
    <tr><td><span data-ttu-id="a03b5-212">$blobContainerName</span><span class="sxs-lookup"><span data-stu-id="a03b5-212">$blobContainerName</span></span></td><td><span data-ttu-id="a03b5-213">hello Blob-behållaren där du vill att tooupload hello data.</span><span class="sxs-lookup"><span data-stu-id="a03b5-213">hello Blob container where you want tooupload hello data to.</span></span></td></tr>
    </table>
2. <span data-ttu-id="a03b5-214">Öppna Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="a03b5-214">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="a03b5-215">3.</span><span class="sxs-lookup"><span data-stu-id="a03b5-215">3.</span></span> <span data-ttu-id="a03b5-216">Klistra in följande skript i hello Skriptfönster hello:</span><span class="sxs-lookup"><span data-stu-id="a03b5-216">Paste hello following script into hello script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # hello source folder
    $destFolder = "tutorials/flightdelay/2013data"     #hello blob name prefix for hello files toobe uploaded
    #EndRegion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy hello file from local workstation tooAzure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName too$blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "hello source folder on hello workstation doesn't exist" -ForegroundColor Red
    }

    # List hello uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. <span data-ttu-id="a03b5-217">Tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="a03b5-217">Press **F5** toorun hello script.</span></span>

<span data-ttu-id="a03b5-218">Om du väljer toouse en annan metod för att överföra hello filer, kontrollera hello filsökvägen är självstudier-flightdelay-data.</span><span class="sxs-lookup"><span data-stu-id="a03b5-218">If you choose toouse a different method for uploading hello files, please make sure hello file path is tutorials/flightdelay/data.</span></span> <span data-ttu-id="a03b5-219">hello-syntaxen för att komma åt hello filer är:</span><span class="sxs-lookup"><span data-stu-id="a03b5-219">hello syntax for accessing hello files is:</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

<span data-ttu-id="a03b5-220">hello sökväg självstudier/flightdelay/data är hello virtuell mapp du skapade när du har överfört hello-filer.</span><span class="sxs-lookup"><span data-stu-id="a03b5-220">hello path tutorials/flightdelay/data is hello virtual folder you created when you uploaded hello files.</span></span> <span data-ttu-id="a03b5-221">Kontrollera att det finns 12 filer, en för varje månad.</span><span class="sxs-lookup"><span data-stu-id="a03b5-221">Verify that there are 12 files, one for each month.</span></span>

> [!NOTE]
> <span data-ttu-id="a03b5-222">Du måste uppdatera hello Hive-fråga tooread från hello ny plats.</span><span class="sxs-lookup"><span data-stu-id="a03b5-222">You must update hello Hive query tooread from hello new location.</span></span>
>
> <span data-ttu-id="a03b5-223">Du måste konfigurera hello behållaren åtkomst behörighet toobe offentliga eller binda hello Storage-konto toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a03b5-223">You must either configure hello container access permission toobe public or bind hello Storage account toohello HDInsight cluster.</span></span> <span data-ttu-id="a03b5-224">Annars blir inte hello Hive frågesträngen kan tooaccess hello-datafiler.</span><span class="sxs-lookup"><span data-stu-id="a03b5-224">Otherwise, hello Hive query string will not be able tooaccess hello data files.</span></span>

- - -

## <span data-ttu-id="a03b5-225"><a id="appendix-b"></a>Bilaga B – skapa och ladda upp en HiveQL-skript</span><span class="sxs-lookup"><span data-stu-id="a03b5-225"><a id="appendix-b"></a>Appendix B - Create and upload a HiveQL script</span></span>
<span data-ttu-id="a03b5-226">Med Azure PowerShell kan köra du flera HiveQL-instruktioner som vid en tidpunkt eller paketet hello HiveQL-instruktionen i en skriptfil.</span><span class="sxs-lookup"><span data-stu-id="a03b5-226">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package hello HiveQL statement into a script file.</span></span> <span data-ttu-id="a03b5-227">Det här avsnittet visar hur toocreate HiveQL-skript och överför hello skript tooAzure Blob storage med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a03b5-227">This section shows you how toocreate a HiveQL script and upload hello script tooAzure Blob storage by using Azure PowerShell.</span></span> <span data-ttu-id="a03b5-228">Hive kräver hello HiveQL-skript toobe lagras i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a03b5-228">Hive requires hello HiveQL scripts toobe stored in Azure Blob storage.</span></span>

<span data-ttu-id="a03b5-229">Hej HiveQL-skript utför hello följande:</span><span class="sxs-lookup"><span data-stu-id="a03b5-229">hello HiveQL script will perform hello following:</span></span>

1. <span data-ttu-id="a03b5-230">**Släppa hello delays_raw tabellen**, om hello tabellen finns redan.</span><span class="sxs-lookup"><span data-stu-id="a03b5-230">**Drop hello delays_raw table**, in case hello table already exists.</span></span>
2. <span data-ttu-id="a03b5-231">**Skapa hello delays_raw externa Hive-tabell** pekar toohello Blob-lagringsplats med hello svarta fördröjning filer.</span><span class="sxs-lookup"><span data-stu-id="a03b5-231">**Create hello delays_raw external Hive table** pointing toohello Blob storage location with hello flight delay files.</span></span> <span data-ttu-id="a03b5-232">Den här frågan anger att fälten avgränsas med ””, och att rader avslutas med ”\n”.</span><span class="sxs-lookup"><span data-stu-id="a03b5-232">This query specifies that fields are delimited by "," and that lines are terminated by "\n".</span></span> <span data-ttu-id="a03b5-233">Det kan medföra problem när fältvärden innehålla kommatecken eftersom Hive kan skilja mellan ett kommatecken som är en fältavgränsaren och en som är en del av ett fältvärde (vilket är det hello i fältvärden för URSPRUNGET\_Stad\_namnet och MÅLET\_ Stad\_namn).</span><span class="sxs-lookup"><span data-stu-id="a03b5-233">This poses a problem when field values contain commas because Hive cannot differentiate between a comma that is a field delimiter and a one that is part of a field value (which is hello case in field values for ORIGIN\_CITY\_NAME and DEST\_CITY\_NAME).</span></span> <span data-ttu-id="a03b5-234">tooaddress detta, hello frågan skapar TEMP kolumner toohold data som felaktigt delas upp i kolumner.</span><span class="sxs-lookup"><span data-stu-id="a03b5-234">tooaddress this, hello query creates TEMP columns toohold data that is incorrectly split into columns.</span></span>
3. <span data-ttu-id="a03b5-235">**Släppa hello fördröjningar tabellen**, om hello tabellen finns redan.</span><span class="sxs-lookup"><span data-stu-id="a03b5-235">**Drop hello delays table**, in case hello table already exists.</span></span>
4. <span data-ttu-id="a03b5-236">**Skapa hello fördröjningar tabell**.</span><span class="sxs-lookup"><span data-stu-id="a03b5-236">**Create hello delays table**.</span></span> <span data-ttu-id="a03b5-237">Det är bra tooclean hello data innan vidare bearbetning.</span><span class="sxs-lookup"><span data-stu-id="a03b5-237">It is helpful tooclean up hello data before further processing.</span></span> <span data-ttu-id="a03b5-238">Den här frågan skapar en ny tabell *fördröjningar*, från hello delays_raw tabell.</span><span class="sxs-lookup"><span data-stu-id="a03b5-238">This query creates a new table, *delays*, from hello delays_raw table.</span></span> <span data-ttu-id="a03b5-239">Observera att inte kopieras hello TEMP kolumner (som tidigare nämnts) och den hello **delsträngen** funktionen är används tooremove citattecken från hello data.</span><span class="sxs-lookup"><span data-stu-id="a03b5-239">Note that hello TEMP columns (as mentioned previously) are not copied, and that hello **substring** function is used tooremove quotation marks from hello data.</span></span>
5. <span data-ttu-id="a03b5-240">**Beräkna hello genomsnittliga väderlek fördröjning och grupper hello resultat av Ortnamn.**</span><span class="sxs-lookup"><span data-stu-id="a03b5-240">**Compute hello average weather delay and groups hello results by city name.**</span></span> <span data-ttu-id="a03b5-241">Det kommer också utdata hello resultat tooBlob lagring.</span><span class="sxs-lookup"><span data-stu-id="a03b5-241">It will also output hello results tooBlob storage.</span></span> <span data-ttu-id="a03b5-242">Observera hello frågan tar bort apostrofer från hello data och utesluta rader där hello värde för **weather_delay** är null.</span><span class="sxs-lookup"><span data-stu-id="a03b5-242">Note that hello query will remove apostrophes from hello data and will exclude rows where hello value for **weather_delay** is null.</span></span> <span data-ttu-id="a03b5-243">Detta är nödvändigt eftersom Sqoop, används senare i den här kursen kan inte hantera dessa värden avslutas som standard.</span><span class="sxs-lookup"><span data-stu-id="a03b5-243">This is necessary because Sqoop, used later in this tutorial, doesn't handle those values gracefully by default.</span></span>

<span data-ttu-id="a03b5-244">En fullständig lista över hello HiveQL kommandon finns [Hive Data Definition Language][hadoop-hiveql].</span><span class="sxs-lookup"><span data-stu-id="a03b5-244">For a full list of hello HiveQL commands, see [Hive Data Definition Language][hadoop-hiveql].</span></span> <span data-ttu-id="a03b5-245">Varje kommando HiveQL måste sluta med ett semikolon.</span><span class="sxs-lookup"><span data-stu-id="a03b5-245">Each HiveQL command must terminate with a semicolon.</span></span>

<span data-ttu-id="a03b5-246">**toocreate en HiveQL-skriptfil**</span><span class="sxs-lookup"><span data-stu-id="a03b5-246">**toocreate a HiveQL script file**</span></span>

1. <span data-ttu-id="a03b5-247">Förbered hello parametrar:</span><span class="sxs-lookup"><span data-stu-id="a03b5-247">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="a03b5-248">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="a03b5-248">Variable Name</span></span></th><th><span data-ttu-id="a03b5-249">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a03b5-249">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="a03b5-250">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="a03b5-250">$storageAccountName</span></span></td><td><span data-ttu-id="a03b5-251">hello Azure Storage-konto där du vill tooupload hello HiveQL skript.</span><span class="sxs-lookup"><span data-stu-id="a03b5-251">hello Azure Storage account where you want tooupload hello HiveQL script to.</span></span></td></tr>
    <tr><td><span data-ttu-id="a03b5-252">$blobContainerName</span><span class="sxs-lookup"><span data-stu-id="a03b5-252">$blobContainerName</span></span></td><td><span data-ttu-id="a03b5-253">hello Blob-behållaren där du vill tooupload hello HiveQL skript.</span><span class="sxs-lookup"><span data-stu-id="a03b5-253">hello Blob container where you want tooupload hello HiveQL script to.</span></span></td></tr>
    </table>
2. <span data-ttu-id="a03b5-254">Öppna Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="a03b5-254">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="a03b5-255">3.</span><span class="sxs-lookup"><span data-stu-id="a03b5-255">3.</span></span> <span data-ttu-id="a03b5-256">Kopiera och klistra in följande skript i hello Skriptfönster hello:</span><span class="sxs-lookup"><span data-stu-id="a03b5-256">Copy and paste hello following script into hello script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # hello HiveQL script file is exported as this file before it's uploaded tooBlob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # hello HiveQL script file will be uploaded tooBlob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by hello HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate hello file and file path

    # Check if a file with hello same file name already exists on hello workstation
    Write-Host "`nvalidating hello folder structure on hello workstation for saving hello HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'hello file, ' $hqlLocalFileName ', exists.  Do you want toooverwirte it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create hello folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write hello Hive script into a local file
    Write-Host "`nWriting hello Hive script into a file on your workstation ..." `
                -ForegroundColor Green

    $hqlDropDelaysRaw = "DROP TABLE delays_raw;"

    $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
            "YEAR string, " +
            "FL_DATE string, " +
            "UNIQUE_CARRIER string, " +
            "CARRIER string, " +
            "FL_NUM string, " +
            "ORIGIN_AIRPORT_ID string, " +
            "ORIGIN string, " +
            "ORIGIN_CITY_NAME string, " +
            "ORIGIN_CITY_NAME_TEMP string, " +
            "ORIGIN_STATE_ABR string, " +
            "DEST_AIRPORT_ID string, " +
            "DEST string, " +
            "DEST_CITY_NAME string, " +
            "DEST_CITY_NAME_TEMP string, " +
            "DEST_STATE_ABR string, " +
            "DEP_DELAY_NEW float, " +
            "ARR_DELAY_NEW float, " +
            "CARRIER_DELAY float, " +
            "WEATHER_DELAY float, " +
            "NAS_DELAY float, " +
            "SECURITY_DELAY float, " +
            "LATE_AIRCRAFT_DELAY float) " +
        "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
        "LINES TERMINATED BY '\n' " +
        "STORED AS TEXTFILE " +
        "LOCATION 'wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"

    $hqlDropDelays = "DROP TABLE delays;"

    $hqlCreateDelays = "CREATE TABLE delays AS " +
        "SELECT YEAR AS year, " +
            "FL_DATE AS flight_date, " +
            "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
            "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
            "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
            "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
            "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
            "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
            "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
            "DEST_AIRPORT_ID AS dest_airport_id, " +
            "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
            "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
            "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
            "DEP_DELAY_NEW AS dep_delay_new, " +
            "ARR_DELAY_NEW AS arr_delay_new, " +
            "CARRIER_DELAY AS carrier_delay, " +
            "WEATHER_DELAY AS weather_delay, " +
            "NAS_DELAY AS nas_delay, " +
            "SECURITY_DELAY AS security_delay, " +
            "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
        "FROM delays_raw;"

    $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
        "SELECT regexp_replace(origin_city_name, '''', ''), " +
            "avg(weather_delay) " +
        "FROM delays " +
        "WHERE weather_delay IS NOT NULL " +
        "GROUP BY origin_city_name;"

    $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal

    $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
    #endregion

    #region - Upload hello Hive script toohello default Blob container
    Write-Host "`nUploading hello Hive script toohello default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload hello file from local workstation tooBlob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

    <span data-ttu-id="a03b5-257">Här följer hello variabler i hello skript:</span><span class="sxs-lookup"><span data-stu-id="a03b5-257">Here are hello variables used in hello script:</span></span>

   * <span data-ttu-id="a03b5-258">**$hqlLocalFileName** -hello skript sparar hello HiveQL skriptfilen lokalt innan den skickas tooBlob lagring.</span><span class="sxs-lookup"><span data-stu-id="a03b5-258">**$hqlLocalFileName** - hello script saves hello HiveQL script file locally before uploading it tooBlob storage.</span></span> <span data-ttu-id="a03b5-259">Detta är hello filnamn.</span><span class="sxs-lookup"><span data-stu-id="a03b5-259">This is hello file name.</span></span> <span data-ttu-id="a03b5-260">hello standardvärdet är <u>C:\tutorials\flightdelay\flightdelays.hql</u>.</span><span class="sxs-lookup"><span data-stu-id="a03b5-260">hello default value is <u>C:\tutorials\flightdelay\flightdelays.hql</u>.</span></span>
   * <span data-ttu-id="a03b5-261">**$hqlBlobName** -hello HiveQL-skript blob filnamn används i hello Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a03b5-261">**$hqlBlobName** - This is hello HiveQL script file blob name used in hello Azure Blob storage.</span></span> <span data-ttu-id="a03b5-262">hello standardvärdet är tutorials/flightdelay/flightdelays.hql.</span><span class="sxs-lookup"><span data-stu-id="a03b5-262">hello default value is tutorials/flightdelay/flightdelays.hql.</span></span> <span data-ttu-id="a03b5-263">Eftersom hello fil ska skrivas direkt tooAzure Blob storage, finns det inte en ”/” hello början av hello blob-namnet.</span><span class="sxs-lookup"><span data-stu-id="a03b5-263">Because hello file will be written directly tooAzure Blob storage, there is NOT a "/" at hello beginning of hello blob name.</span></span> <span data-ttu-id="a03b5-264">Om du vill att tooaccess hello filen från Blob storage måste tooadd ”/” hello början av hello filnamn.</span><span class="sxs-lookup"><span data-stu-id="a03b5-264">If you want tooaccess hello file from Blob storage, you will need tooadd a "/" at hello beginning of hello file name.</span></span>
   * <span data-ttu-id="a03b5-265">**$srcDataFolder** och **$dstDataFolder** -= ”data-självstudier/flightdelay” = ”självstudier/flightdelay/utdata”</span><span class="sxs-lookup"><span data-stu-id="a03b5-265">**$srcDataFolder** and **$dstDataFolder** - = "tutorials/flightdelay/data" = "tutorials/flightdelay/output"</span></span>

- - -
## <span data-ttu-id="a03b5-266"><a id="appendix-c"></a>Bilaga C – förbereda en Azure SQL database för hello Sqoop jobbutdata</span><span class="sxs-lookup"><span data-stu-id="a03b5-266"><a id="appendix-c"></a>Appendix C - Prepare an Azure SQL database for hello Sqoop job output</span></span>
<span data-ttu-id="a03b5-267">**tooprepare hello SQL-databas (sammanfoga det med hello Sqoop skript)**</span><span class="sxs-lookup"><span data-stu-id="a03b5-267">**tooprepare hello SQL database (merge this with hello Sqoop script)**</span></span>

1. <span data-ttu-id="a03b5-268">Förbered hello parametrar:</span><span class="sxs-lookup"><span data-stu-id="a03b5-268">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="a03b5-269">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="a03b5-269">Variable Name</span></span></th><th><span data-ttu-id="a03b5-270">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a03b5-270">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="a03b5-271">$sqlDatabaseServerName</span><span class="sxs-lookup"><span data-stu-id="a03b5-271">$sqlDatabaseServerName</span></span></td><td><span data-ttu-id="a03b5-272">hello namnet på hello Azure SQL database-server.</span><span class="sxs-lookup"><span data-stu-id="a03b5-272">hello name of hello Azure SQL database server.</span></span> <span data-ttu-id="a03b5-273">Ange inget toocreate en ny server.</span><span class="sxs-lookup"><span data-stu-id="a03b5-273">Enter nothing toocreate a new server.</span></span></td></tr>
    <tr><td><span data-ttu-id="a03b5-274">$sqlDatabaseUsername</span><span class="sxs-lookup"><span data-stu-id="a03b5-274">$sqlDatabaseUsername</span></span></td><td><span data-ttu-id="a03b5-275">hello inloggningsnamnet för hello Azure SQL database-server.</span><span class="sxs-lookup"><span data-stu-id="a03b5-275">hello login name for hello Azure SQL database server.</span></span> <span data-ttu-id="a03b5-276">Om $sqlDatabaseServerName är en befintlig server, är hello inloggningsnamn och inloggningslösenord används tooauthenticate med hello-servern.</span><span class="sxs-lookup"><span data-stu-id="a03b5-276">If $sqlDatabaseServerName is an existing server, hello login and login password are used tooauthenticate with hello server.</span></span> <span data-ttu-id="a03b5-277">De är annars används toocreate en ny server.</span><span class="sxs-lookup"><span data-stu-id="a03b5-277">Otherwise they are used toocreate a new server.</span></span></td></tr>
    <tr><td><span data-ttu-id="a03b5-278">$sqlDatabasePassword</span><span class="sxs-lookup"><span data-stu-id="a03b5-278">$sqlDatabasePassword</span></span></td><td><span data-ttu-id="a03b5-279">hello inloggningslösenordet för hello Azure SQL database-servern.</span><span class="sxs-lookup"><span data-stu-id="a03b5-279">hello login password for hello Azure SQL database server.</span></span></td></tr>
    <tr><td><span data-ttu-id="a03b5-280">$sqlDatabaseLocation</span><span class="sxs-lookup"><span data-stu-id="a03b5-280">$sqlDatabaseLocation</span></span></td><td><span data-ttu-id="a03b5-281">Det här värdet används endast när du skapar en ny Azure databasserver.</span><span class="sxs-lookup"><span data-stu-id="a03b5-281">This value is used only when you're creating a new Azure database server.</span></span></td></tr>
    <tr><td><span data-ttu-id="a03b5-282">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="a03b5-282">$sqlDatabaseName</span></span></td><td><span data-ttu-id="a03b5-283">hello SQL-databasen används toocreate hello AvgDelays tabellen för hello Sqoop jobb.</span><span class="sxs-lookup"><span data-stu-id="a03b5-283">hello SQL database used toocreate hello AvgDelays table for hello Sqoop job.</span></span> <span data-ttu-id="a03b5-284">Lämna det tomt skapar en databas som heter HDISqoop.</span><span class="sxs-lookup"><span data-stu-id="a03b5-284">Leaving it blank will create a database called HDISqoop.</span></span> <span data-ttu-id="a03b5-285">hello tabellnamnet för hello Sqoop jobbutdata är AvgDelays.</span><span class="sxs-lookup"><span data-stu-id="a03b5-285">hello table name for hello Sqoop job output is AvgDelays.</span></span> </td></tr>
    </table>
2. <span data-ttu-id="a03b5-286">Öppna Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="a03b5-286">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="a03b5-287">3.</span><span class="sxs-lookup"><span data-stu-id="a03b5-287">3.</span></span> <span data-ttu-id="a03b5-288">Kopiera och klistra in följande skript i hello Skriptfönster hello:</span><span class="sxs-lookup"><span data-stu-id="a03b5-288">Copy and paste hello following script into hello script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello region toocreate hello Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify hello database name if you have one created. Otherwise use "" toohave hello script create one for you.
    )

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Constants and variables

    # IP address REST service used for retrieving external IP address and creating firewall rules
    [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
    [String]$fireWallRuleName = "FlightDelay"

    # SQL database variables
    [String]$sqlDatabaseMaxSizeGB = 10

    #SQL query string for creating AvgDelays table
    [String]$sqlDatabaseTableName = "AvgDelays"
    [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [origin_city_name] [nvarchar](50) NOT NULL,
                [weather_delay] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
                [origin_city_name] ASC
            )
            )"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resouce group
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
    }

    #EndRegion

    #region - Create and validate Azure SQL database server
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database

    try {
        Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region -  Execute an SQL command toocreate hello AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]
   > <span data-ttu-id="a03b5-289">hello-skript som använder en representational tillståndstjänsten för transfer (REST), http://bot.whatismyipaddress.com, tooretrieve extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a03b5-289">hello script uses a representational state transfer (REST) service, http://bot.whatismyipaddress.com, tooretrieve your external IP address.</span></span> <span data-ttu-id="a03b5-290">hello IP-adress används för att skapa en brandväggsregel för SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="a03b5-290">hello IP address is used for creating a firewall rule for your SQL database server.</span></span>

    <span data-ttu-id="a03b5-291">Här följer några variabler som används i hello skript:</span><span class="sxs-lookup"><span data-stu-id="a03b5-291">Here are some variables used in hello script:</span></span>

   * <span data-ttu-id="a03b5-292">**$ipAddressRestService** -hello standardvärdet är http://bot.whatismyipaddress.com. Det är en offentlig IP-adress REST-tjänst för att hämta extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a03b5-292">**$ipAddressRestService** - hello default value is http://bot.whatismyipaddress.com. It is a public IP address REST service for getting your external IP address.</span></span> <span data-ttu-id="a03b5-293">Du kan använda andra tjänster om du vill.</span><span class="sxs-lookup"><span data-stu-id="a03b5-293">You can use other services if you want.</span></span> <span data-ttu-id="a03b5-294">hello extern IP-adress som hämtats via hello-tjänsten kommer att använda toocreate en brandväggsregel för din Azure SQL database-server så att du kan komma åt hello databasen från din arbetsstation (med hjälp av Windows PowerShell-skript).</span><span class="sxs-lookup"><span data-stu-id="a03b5-294">hello external IP address retrieved through hello service will be used toocreate a firewall rule for your Azure SQL database server, so that you can access hello database from your workstation (by using a Windows PowerShell script).</span></span>
   * <span data-ttu-id="a03b5-295">**$fireWallRuleName** -är hello namnet på hello brandväggsregel för hello Azure SQL database-servern.</span><span class="sxs-lookup"><span data-stu-id="a03b5-295">**$fireWallRuleName** - This is hello name of hello firewall rule for hello Azure SQL database server.</span></span> <span data-ttu-id="a03b5-296">hello standardnamnet är <u>FlightDelay</u>.</span><span class="sxs-lookup"><span data-stu-id="a03b5-296">hello default name is <u>FlightDelay</u>.</span></span> <span data-ttu-id="a03b5-297">Du kan byta namn på den om du vill.</span><span class="sxs-lookup"><span data-stu-id="a03b5-297">You can rename it if you want.</span></span>
   * <span data-ttu-id="a03b5-298">**$sqlDatabaseMaxSizeGB** -det här värdet används endast när du skapar en ny Azure SQL database-server.</span><span class="sxs-lookup"><span data-stu-id="a03b5-298">**$sqlDatabaseMaxSizeGB** - This value is used only when you're creating a new Azure SQL database server.</span></span> <span data-ttu-id="a03b5-299">hello standardvärdet är 10GB.</span><span class="sxs-lookup"><span data-stu-id="a03b5-299">hello default value is 10GB.</span></span> <span data-ttu-id="a03b5-300">10GB är tillräcklig för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a03b5-300">10GB is sufficient for this tutorial.</span></span>
   * <span data-ttu-id="a03b5-301">**$sqlDatabaseName** -det här värdet används endast när du skapar en ny Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a03b5-301">**$sqlDatabaseName** - This value is used only when you're creating a new Azure SQL database.</span></span> <span data-ttu-id="a03b5-302">hello standardvärdet är HDISqoop.</span><span class="sxs-lookup"><span data-stu-id="a03b5-302">hello default value is HDISqoop.</span></span> <span data-ttu-id="a03b5-303">Om du byter namn måste du uppdatera hello Sqoop Windows PowerShell-skript i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="a03b5-303">If you rename it, you must update hello Sqoop Windows PowerShell script accordingly.</span></span>
4. <span data-ttu-id="a03b5-304">Tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="a03b5-304">Press **F5** toorun hello script.</span></span>
5. <span data-ttu-id="a03b5-305">Validera hello utdata för skriptet.</span><span class="sxs-lookup"><span data-stu-id="a03b5-305">Validate hello script output.</span></span> <span data-ttu-id="a03b5-306">Kontrollera att hello skriptet har körts.</span><span class="sxs-lookup"><span data-stu-id="a03b5-306">Make sure hello script ran successfully.</span></span>

## <span data-ttu-id="a03b5-307"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a03b5-307"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="a03b5-308">Nu när du förstår hur tooupload fil-tooAzure Blob storage, hur toopopulate en Hive-tabell med hello data från Azure Blob storage hur toorun Hive frågor, och hur toouse Sqoop tooexport data från HDFS tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a03b5-308">Now you understand how tooupload a file tooAzure Blob storage, how toopopulate a Hive table by using hello data from Azure Blob storage, how toorun Hive queries, and how toouse Sqoop tooexport data from HDFS tooan Azure SQL database.</span></span> <span data-ttu-id="a03b5-309">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a03b5-309">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="a03b5-310">[Komma igång med HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="a03b5-310">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="a03b5-311">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="a03b5-311">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="a03b5-312">[Använda Oozie med HDInsight][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="a03b5-312">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="a03b5-313">[Använda Sqoop med HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="a03b5-313">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="a03b5-314">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="a03b5-314">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="a03b5-315">[Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="a03b5-315">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
