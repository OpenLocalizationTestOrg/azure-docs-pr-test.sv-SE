---
title: "aaaCreate på begäran Hadoop-kluster med hjälp av Data Factory - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate på begäran Hadoop-kluster i HDInsight med hjälp av Azure Data Factory."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: spelluru
ms.openlocfilehash: c869776ac270e37dec710b5fc8d2a792d9263129
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a><span data-ttu-id="ee1e4-103">Skapa på begäran Hadoop-kluster i HDInsight med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="ee1e4-103">Create on-demand Hadoop clusters in HDInsight using Azure Data Factory</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="ee1e4-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) är en molnbaserad integration datatjänst som samordnar och automatiserar hello flytt och transformering av data.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) is a cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="ee1e4-105">Det kan skapa en HDInsight Hadoop-kluster just-in-time tooprocess ett indata-segment och ta bort hello klustret när hello bearbetningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-105">It can create a HDInsight Hadoop cluster just-in-time tooprocess an input data slice and delete hello cluster when hello processing is complete.</span></span> <span data-ttu-id="ee1e4-106">Några av hello fördelarna med att använda ett HDInsight Hadoop-kluster på begäran är:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-106">Some of hello benefits of using an on-demand HDInsight Hadoop cluster are:</span></span>

- <span data-ttu-id="ee1e4-107">Du endast betala för hello jobbet körs på hello HDInsight Hadoop-kluster (plus en kort konfigurerbara inaktivitetstid).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-107">You only pay for hello time job is running on hello HDInsight Hadoop cluster (plus a brief configurable idle time).</span></span> <span data-ttu-id="ee1e4-108">hello fakturering för HDInsight-kluster sker proportionerligt per minut, oavsett om du använder dem eller inte.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-108">hello billing for HDInsight clusters is pro-rated per minute, whether you are using them or not.</span></span> <span data-ttu-id="ee1e4-109">När du använder en på-begäran länkad HDInsight-tjänst i Data Factory skapas hello kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-109">When you use an on-demand HDInsight linked service in Data Factory, hello clusters are created on-demand.</span></span> <span data-ttu-id="ee1e4-110">Och hello kluster tas bort automatiskt när hello jobben har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-110">And hello clusters are deleted automatically when hello jobs are completed.</span></span> <span data-ttu-id="ee1e4-111">Därför kan betalar du bara för hello jobbet körs och hello kort inaktiv tid (time to live-inställning).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-111">Therefore, you only pay for hello job running time and hello brief idle time (time-to-live setting).</span></span>
- <span data-ttu-id="ee1e4-112">Du kan skapa ett arbetsflöde med Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-112">You can create a workflow using a Data Factory pipeline.</span></span> <span data-ttu-id="ee1e4-113">Du kan till exempel ha hello pipeline toocopy data från en lokal SQL Server-tooan Azure blob storage, processen hello data genom att köra en Hive-skript och Pig-skriptet på en HDInsight Hadoop-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-113">For example, you can have hello pipeline toocopy data from an on-premises SQL Server tooan Azure blob storage, process hello data by running a Hive script and a Pig script on an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="ee1e4-114">Kopiera sedan hello resultatet data tooan Azure SQL Data Warehouse för BI program tooconsume.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-114">Then, copy hello result data tooan Azure SQL Data Warehouse for BI applications tooconsume.</span></span>
- <span data-ttu-id="ee1e4-115">Du kan schemalägga hello arbetsflöde toorun regelbundet (varje timme, varje dag, vecka, månad, etc.).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-115">You can schedule hello workflow toorun periodically (hourly, daily, weekly, monthly, etc.).</span></span>

<span data-ttu-id="ee1e4-116">En datafabrik kan ha en eller flera data pipelines i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-116">In Azure Data Factory, a data factory can have one or more data pipelines.</span></span> <span data-ttu-id="ee1e4-117">En data-pipeline har en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-117">A data pipeline has one or more activities.</span></span> <span data-ttu-id="ee1e4-118">Det finns två typer av aktiviteter: [Data Movement aktiviteter](../data-factory/data-factory-data-movement-activities.md) och [Data Transformation aktiviteter](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-118">There are two types of activities: [Data Movement Activities](../data-factory/data-factory-data-movement-activities.md) and [Data Transformation Activities](../data-factory/data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="ee1e4-119">Du kan använda dataflytt aktiviteter (för närvarande endast Kopieringsaktivitet) toomove data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-119">You use data movement activities (currently, only Copy Activity) toomove data from a source data store tooa destination data store.</span></span> <span data-ttu-id="ee1e4-120">Du kan använda data transformation aktiviteter tootransform/bearbetning av data.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-120">You use data transformation activities tootransform/process data.</span></span> <span data-ttu-id="ee1e4-121">HDInsight Hive aktivitet är en av hello omvandling av aktiviteter som stöds av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-121">HDInsight Hive Activity is one of hello transformation activities supported by Data Factory.</span></span> <span data-ttu-id="ee1e4-122">Du kan använda hello Hive omvandling aktiviteten i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-122">You use hello Hive transformation activity in this tutorial.</span></span>

<span data-ttu-id="ee1e4-123">Du kan konfigurera en hive aktiviteten toouse egna HDInsight Hadoop-kluster eller en HDInsight Hadoop-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-123">You can configure a hive activity toouse your own HDInsight Hadoop cluster or an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="ee1e4-124">I den här kursen är hello Hive aktivitet i hello data factory-pipelinen konfigurerade toouse ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-124">In this tutorial, hello Hive activity in hello data factory pipeline is configured toouse an on-demand HDInsight cluster.</span></span> <span data-ttu-id="ee1e4-125">Därför när hello aktiviteten körs tooprocess en datasektorn är här vad som händer:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-125">Therefore, when hello activity runs tooprocess a data slice, here is what happens:</span></span>

1. <span data-ttu-id="ee1e4-126">Ett HDInsight Hadoop-kluster skapas automatiskt för du just-in-time tooprocess hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-126">A HDInsight Hadoop cluster is automatically created for you just-in-time tooprocess hello slice.</span></span>  
2. <span data-ttu-id="ee1e4-127">hello indata bearbetas genom att köra en HiveQL-skript på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-127">hello input data is processed by running a HiveQL script on hello cluster.</span></span>
3. <span data-ttu-id="ee1e4-128">Hej HDInsight Hadoop-kluster tas bort när hello bearbetningen är klar och hello klustret är inaktiv under hello konfigurerad tidsperiod (timeToLive-inställning).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-128">hello HDInsight Hadoop cluster is deleted after hello processing is complete and hello cluster is idle for hello configured amount of time (timeToLive setting).</span></span> <span data-ttu-id="ee1e4-129">Om hello nästa datasektorn är tillgängliga för bearbetning med i den här timeToLive väntetid är hello samma kluster används tooprocess hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-129">If hello next data slice is available for processing with in this timeToLive idle time, hello same cluster is used tooprocess hello slice.</span></span>  

<span data-ttu-id="ee1e4-130">I den här självstudiekursen utför hello HiveQL-skript som är associerade med hello hive aktiviteten hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-130">In this tutorial, hello HiveQL script associated with hello hive activity performs hello following actions:</span></span>

1. <span data-ttu-id="ee1e4-131">Skapar en extern tabell refererar hello rådata web loggdata som lagras i ett Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-131">Creates an external table that references hello raw web log data stored in an Azure Blob storage.</span></span>
2. <span data-ttu-id="ee1e4-132">Partitioner hello rådata per år och månad.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-132">Partitions hello raw data by year and month.</span></span>
3. <span data-ttu-id="ee1e4-133">Lagrar hello partitionerade data i hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-133">Stores hello partitioned data in hello Azure blob storage.</span></span>

<span data-ttu-id="ee1e4-134">I den här självstudiekursen skapar hello HiveQL-skript som är associerade med hello hive aktiviteten en extern tabell refererar hello rådata web loggdata som lagras i hello Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-134">In this tutorial, hello HiveQL script associated with hello hive activity creates an external table that references hello raw web log data stored in hello Azure Blob Storage.</span></span> <span data-ttu-id="ee1e4-135">Här följer hello exempel rader för varje månad i hello indatafilen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-135">Here are hello sample rows for each month in hello input file.</span></span>

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

<span data-ttu-id="ee1e4-136">Hej HiveQL-skript partitioner hello rådata per år och månad.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-136">hello HiveQL script partitions hello raw data by year and month.</span></span> <span data-ttu-id="ee1e4-137">Den skapar tre utdatamappar baserat på hello tidigare indata.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-137">It creates three output folders based on hello previous input.</span></span> <span data-ttu-id="ee1e4-138">Varje mapp innehåller en fil med poster från varje månad.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-138">Each folder contains a file with entries from each month.</span></span>

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

<span data-ttu-id="ee1e4-139">En lista över Data Factory data transformation aktiviteter i tillägg tooHive aktivitet finns [transformera och analysera med hjälp av Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-139">For a list of Data Factory data transformation activities in addition tooHive activity, see [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="ee1e4-140">För närvarande kan du bara skapa HDInsight-kluster av version 3.2 från Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-140">Currently, you can only create HDInsight cluster version 3.2 from Azure Data Factory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee1e4-141">Krav</span><span class="sxs-lookup"><span data-stu-id="ee1e4-141">Prerequisites</span></span>
<span data-ttu-id="ee1e4-142">Innan du börjar hello anvisningarna i den här artikeln, måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-142">Before you begin hello instructions in this article, you must have hello following items:</span></span>

* <span data-ttu-id="ee1e4-143">[Azure-prenumeration](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-143">[Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="ee1e4-144">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-144">Azure PowerShell.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a><span data-ttu-id="ee1e4-145">Förbereda storage-konto</span><span class="sxs-lookup"><span data-stu-id="ee1e4-145">Prepare storage account</span></span>
<span data-ttu-id="ee1e4-146">Du kan använda upp toothree storage-konton i det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-146">You can use up toothree storage accounts in this scenario:</span></span>

- <span data-ttu-id="ee1e4-147">standardkontot för lagring för hello HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="ee1e4-147">default storage account for hello HDInsight cluster</span></span>
- <span data-ttu-id="ee1e4-148">lagringskontot för hello indata</span><span class="sxs-lookup"><span data-stu-id="ee1e4-148">storage account for hello input data</span></span>
- <span data-ttu-id="ee1e4-149">Storage-konto för hello-utdata</span><span class="sxs-lookup"><span data-stu-id="ee1e4-149">storage account for hello output data</span></span>

<span data-ttu-id="ee1e4-150">toosimplify hello självstudier, använda en storage-konto tooserve hello tre funktioner.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-150">toosimplify hello tutorial, you use one storage account tooserve hello three purposes.</span></span> <span data-ttu-id="ee1e4-151">hello Azure PowerShell-exempelskript finns i det här avsnittet utför hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-151">hello Azure PowerShell sample script found in this section performs hello following tasks:</span></span>

1. <span data-ttu-id="ee1e4-152">Logga in tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-152">Log in tooAzure.</span></span>
2. <span data-ttu-id="ee1e4-153">Skapa en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-153">Create an Azure resource group.</span></span>
3. <span data-ttu-id="ee1e4-154">Skapa ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-154">Create an Azure Storage account.</span></span>
4. <span data-ttu-id="ee1e4-155">Skapa en blobbbehållare i hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="ee1e4-155">Create a Blob container in hello storage account</span></span>
5. <span data-ttu-id="ee1e4-156">Kopiera hello följande två filer toohello Blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-156">Copy hello following two files toohello Blob container:</span></span>

   * <span data-ttu-id="ee1e4-157">Indatafilen: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span><span class="sxs-lookup"><span data-stu-id="ee1e4-157">Input data file: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span></span>
   * <span data-ttu-id="ee1e4-158">HiveQL-skript: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span><span class="sxs-lookup"><span data-stu-id="ee1e4-158">HiveQL script: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span></span>

     <span data-ttu-id="ee1e4-159">Filer som lagras i en offentlig Blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-159">Both files are stored in a public Blob container.</span></span>


<span data-ttu-id="ee1e4-160">**tooprepare hello lagring och kopiera hello filer med hjälp av Azure PowerShell:**</span><span class="sxs-lookup"><span data-stu-id="ee1e4-160">**tooprepare hello storage and copy hello files using Azure PowerShell:**</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ee1e4-161">Ange namn för hello Azure-resursgrupp och hello Azure storage-konto som skapas av hello-skriptet.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-161">Specify names for hello Azure resource group and hello Azure storage account that will be created by hello script.</span></span>
> <span data-ttu-id="ee1e4-162">Skriv ned **resursgruppnamn**, **lagringskontonamnet**, och **lagringskontonyckel** utdata av hello skript.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-162">Write down **resource group name**, **storage account name**, and **storage account key** outputted by hello script.</span></span> <span data-ttu-id="ee1e4-163">Du behöver dem i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-163">You need them in hello next section.</span></span>

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect tooAzure
####################################
#region - Connect tooAzure subscription
Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use hello following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

<span data-ttu-id="ee1e4-164">Om du behöver hjälp med hello PowerShell-skript finns [Using hello Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-164">If you need help with hello PowerShell script, see [Using hello Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span> <span data-ttu-id="ee1e4-165">Om du toouse Azure CLI Läs hello [bilaga](#appendix) avsnittet hello Azure CLI-skript.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-165">If you like toouse Azure CLI instead, see hello [Appendix](#appendix) section for hello Azure CLI script.</span></span>

<span data-ttu-id="ee1e4-166">**tooexamine hello storage-konto och hello innehållet**</span><span class="sxs-lookup"><span data-stu-id="ee1e4-166">**tooexamine hello storage account and hello contents**</span></span>

1. <span data-ttu-id="ee1e4-167">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-167">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ee1e4-168">Klicka på **resursgrupper** hello vänster.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-168">Click **Resource groups** on hello left pane.</span></span>
3. <span data-ttu-id="ee1e4-169">Dubbelklicka på hello resursgruppens namn du skapade i PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-169">Double-click hello resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="ee1e4-170">Använda hello filtret om du har för många resursgrupper i listan.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-170">Use hello filter if you have too many resource groups listed.</span></span>
4. <span data-ttu-id="ee1e4-171">På hello **resurser** sida vid sida, har du en resurs i listan om du delar hello resursgrupp med andra projekt.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-171">On hello **Resources** tile, you shall have one resource listed unless you share hello resource group with other projects.</span></span> <span data-ttu-id="ee1e4-172">Den här resursen är hello lagringskonto med hello namn du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-172">That resource is hello storage account with hello name you specified earlier.</span></span> <span data-ttu-id="ee1e4-173">Klicka på hello lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-173">Click hello storage account name.</span></span>
5. <span data-ttu-id="ee1e4-174">Klicka på hello **Blobbar** paneler.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-174">Click hello **Blobs** tiles.</span></span>
6. <span data-ttu-id="ee1e4-175">Klicka på hello **adfgetstarted** behållare.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-175">Click hello **adfgetstarted** container.</span></span> <span data-ttu-id="ee1e4-176">Du ser två mappar: **inputdata** och **skriptet**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-176">You see two folders: **inputdata** and **script**.</span></span>
7. <span data-ttu-id="ee1e4-177">Öppna mappen hello och kontrollera hello filer i hello mappar.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-177">Open hello folder and check hello files in hello folders.</span></span> <span data-ttu-id="ee1e4-178">Hej inputdata innehåller hello input.log filen med indata hello skript mappen innehåller hello HiveQL skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-178">hello inputdata contains hello input.log file with input data and hello script folder contains hello HiveQL script file.</span></span>

## <a name="create-a-data-factory-using-resource-manager-template"></a><span data-ttu-id="ee1e4-179">Skapa en datafabrik med hjälp av Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="ee1e4-179">Create a data factory using Resource Manager template</span></span>
<span data-ttu-id="ee1e4-180">Med hello lagringskonto, hello indata och hello förberedd HiveQL-skript, är du redo toocreate ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-180">With hello storage account, hello input data, and hello HiveQL script prepared, you are ready toocreate an Azure data factory.</span></span> <span data-ttu-id="ee1e4-181">Det finns flera metoder för att skapa datafabriken.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-181">There are several methods for creating data factory.</span></span> <span data-ttu-id="ee1e4-182">I kursen får skapa du en datafabrik genom att distribuera en Azure Resource Manager-mallen med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-182">In this tutorial, you create a data factory by deploying an Azure Resource Manager template using hello Azure portal.</span></span> <span data-ttu-id="ee1e4-183">Du kan också distribuera en Resource Manager-mall med hjälp av [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) och [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-183">You can also deploy a Resource Manager template by using [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) and [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span> <span data-ttu-id="ee1e4-184">Andra data factory-metoder, se [Självstudier: skapa din första data factory](../data-factory/data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-184">For other data factory creation methods, see [Tutorial: Build your first data factory](../data-factory/data-factory-build-your-first-pipeline.md).</span></span>

1. <span data-ttu-id="ee1e4-185">Klicka på hello efter bilden toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-185">Click hello following image toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span> <span data-ttu-id="ee1e4-186">hello-mallen finns på https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-186">hello template is located at https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span></span> <span data-ttu-id="ee1e4-187">Se hello [Data Factory-entiteter i hello mallen](#data-factory-entities-in-the-template) finns detaljerad information om enheter som har definierats i mallen för hello.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-187">See hello [Data Factory entities in hello template](#data-factory-entities-in-the-template) section for detailed information about entities defined in hello template.</span></span> 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="ee1e4-188">Välj **Använd befintliga** alternativ för hello **resursgruppen** inställningen och välj hello namnet på hello resursgrupp som du skapade i föregående steg i hello (med PowerShell-skript).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-188">Select **Use existing** option for hello **Resource group** setting, and select hello name of hello resource group you created in hello previous step (using PowerShell script).</span></span>
3. <span data-ttu-id="ee1e4-189">Ange ett namn för hello data factory (**Datafabriksnamnet**).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-189">Enter a name for hello data factory (**Data Factory Name**).</span></span> <span data-ttu-id="ee1e4-190">Det här namnet måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-190">This name must be globally unique.</span></span>
4. <span data-ttu-id="ee1e4-191">Ange hello **lagringskontonamnet** och **lagringskontonyckel** du noterade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-191">Enter hello **storage account name** and **storage account key** you wrote down in hello previous step.</span></span>
5. <span data-ttu-id="ee1e4-192">Välj **jag accepterar villkoren toohello** anges ovan när du har läst **villkor**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-192">Select **I agree toohello terms and conditions** stated above after reading through **terms and conditions**.</span></span>
6. <span data-ttu-id="ee1e4-193">Välj **PIN-kod toodashboard** alternativet.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-193">Select **Pin toodashboard** option.</span></span>
6. <span data-ttu-id="ee1e4-194">Klicka på **inköp/skapa**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-194">Click **Purchase/Create**.</span></span> <span data-ttu-id="ee1e4-195">Du ser en panel på instrumentpanelen kallas hello **skicka malldistribution**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-195">You see a tile on hello Dashboard called **Deploying Template deployment**.</span></span> <span data-ttu-id="ee1e4-196">Vänta tills hello **resursgruppen** öppnas bladet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-196">Wait until hello **Resource group** blade for your resource group opens.</span></span> <span data-ttu-id="ee1e4-197">Du kan också klicka på hello sida vid sida med titeln som din grupp namnet tooopen hello resurs blad för resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-197">You can also click hello tile titled as your resource group name tooopen hello resource group blade.</span></span>
6. <span data-ttu-id="ee1e4-198">Klicka på hello panelen tooopen hello resursgrupp om hello blad för resursgrupp inte redan är öppen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-198">Click hello tile tooopen hello resource group if hello resource group blade is not already open.</span></span> <span data-ttu-id="ee1e4-199">Nu visas en mer data factory-resurs visas dessutom toohello lagringsresurs för kontot.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-199">Now you shall see one more data factory resource listed in addition toohello storage account resource.</span></span>
7. <span data-ttu-id="ee1e4-200">Klicka på hello namnet på din data factory (värdet du angav för hello **Datafabriksnamnet** parametern).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-200">Click hello name of your data factory (value you specified for hello **Data Factory Name** parameter).</span></span>
8. <span data-ttu-id="ee1e4-201">Hello Data Factory-bladet, klickar du på hello **Diagram** panelen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-201">In hello Data Factory blade, click hello **Diagram** tile.</span></span> <span data-ttu-id="ee1e4-202">hello diagram visar en aktivitet med en inkommande datauppsättning och en datamängd för utdata:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-202">hello diagram shows one activity with an input dataset, and an output dataset:</span></span>

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline diagram](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    <span data-ttu-id="ee1e4-204">hello namn har definierats i hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-204">hello names are defined in hello Resource Manager template.</span></span>
9. <span data-ttu-id="ee1e4-205">Dubbelklicka på **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-205">Double-click **AzureBlobOutput**.</span></span>
10. <span data-ttu-id="ee1e4-206">På hello **senaste uppdaterade segment**, visas ett segment.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-206">On hello **Recent updated slices**, you shall see one slice.</span></span> <span data-ttu-id="ee1e4-207">Om hello status är **pågår**, vänta tills den har ändrats för**klar**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-207">If hello status is **In progress**, wait until it is changed too**Ready**.</span></span> <span data-ttu-id="ee1e4-208">Det tar vanligtvis om **20 minuter** toocreate ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-208">It usually takes about **20 minutes** toocreate an HDInsight cluster.</span></span>

### <a name="check-hello-data-factory-output"></a><span data-ttu-id="ee1e4-209">Kontrollera hello data factory-utdata</span><span class="sxs-lookup"><span data-stu-id="ee1e4-209">Check hello data factory output</span></span>

1. <span data-ttu-id="ee1e4-210">Använd hello samma procedur i hello senaste sessionen toocheck hello behållare för hello adfgetstarted behållare.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-210">Use hello same procedure in hello last session toocheck hello containers of hello adfgetstarted container.</span></span> <span data-ttu-id="ee1e4-211">Det finns två nya behållare dessutom för**adfgetsarted**:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-211">There are two new containers in addition too**adfgetsarted**:</span></span>

   * <span data-ttu-id="ee1e4-212">En behållare med namn som följer hello mönster: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-212">A container with name that follows hello pattern: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span></span> <span data-ttu-id="ee1e4-213">Den här behållaren är hello standardbehållaren för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-213">This container is hello default container for hello HDInsight cluster.</span></span>
   * <span data-ttu-id="ee1e4-214">adfjobs: den här behållaren är hello behållare för hello ADF jobbloggar.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-214">adfjobs: This container is hello container for hello ADF job logs.</span></span>

     <span data-ttu-id="ee1e4-215">hello data factory utdata lagras i **afgetstarted** som du konfigurerade i hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-215">hello data factory output is stored in **afgetstarted** as you configured in hello Resource Manager template.</span></span>
2. <span data-ttu-id="ee1e4-216">Klicka på **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-216">Click **adfgetstarted**.</span></span>
3. <span data-ttu-id="ee1e4-217">Dubbelklicka på **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-217">Double-click **partitioneddata**.</span></span> <span data-ttu-id="ee1e4-218">Du ser en **år 2014 =** mappen eftersom alla hello webbloggar funktionsdokumentationen år 2014.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-218">You see a **year=2014** folder because all hello web logs are dated in year 2014.</span></span>

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline utdata](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    <span data-ttu-id="ee1e4-220">Om du detaljnivån hello lista visas tre mappar för januari, februari och mars.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-220">If you drill down hello list, you shall see three folders for January, February, and March.</span></span> <span data-ttu-id="ee1e4-221">Och det finns en logg för varje månad.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-221">And there is a log for each month.</span></span>

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline utdata](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="ee1e4-223">Data Factory-entiteter i hello mall</span><span class="sxs-lookup"><span data-stu-id="ee1e4-223">Data Factory entities in hello template</span></span>
<span data-ttu-id="ee1e4-224">Här är att hello översta Resource Manager-mall för en datafabrik som ser ut som:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-224">Here is how hello top-level Resource Manager template for a data factory looks like:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```

### <a name="define-data-factory"></a><span data-ttu-id="ee1e4-225">Definiera en datafabrik</span><span class="sxs-lookup"><span data-stu-id="ee1e4-225">Define data factory</span></span>
<span data-ttu-id="ee1e4-226">Du kan definiera en datafabrik i hello Resource Manager-mall som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-226">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
<span data-ttu-id="ee1e4-227">Hej dataFactoryName är hello datafabriken som du anger när du distribuerar mallen hello hello namn.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-227">hello dataFactoryName is hello name of hello data factory you specify when you deploy hello template.</span></span> <span data-ttu-id="ee1e4-228">Datafabriken är för närvarande stöds endast i hello östra USA, västra USA och Norra Europa regioner.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-228">Data factory is currently only supported in hello East US, West US, and North Europe regions.</span></span>

### <a name="defining-entities-within-hello-data-factory"></a><span data-ttu-id="ee1e4-229">Definiera enheter inom hello data factory</span><span class="sxs-lookup"><span data-stu-id="ee1e4-229">Defining entities within hello data factory</span></span>
<span data-ttu-id="ee1e4-230">hello har följande Data Factory-enheter definierats i hello JSON-mall:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-230">hello following Data Factory entities are defined in hello JSON template:</span></span>

* [<span data-ttu-id="ee1e4-231">Länkad Azure Storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="ee1e4-231">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="ee1e4-232">Länkad tjänst för HDInsight på begäran</span><span class="sxs-lookup"><span data-stu-id="ee1e4-232">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="ee1e4-233">Indatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="ee1e4-233">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="ee1e4-234">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="ee1e4-234">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="ee1e4-235">Datapipeline med en kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="ee1e4-235">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="ee1e4-236">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="ee1e4-236">Azure Storage linked service</span></span>
<span data-ttu-id="ee1e4-237">hello Azure Storage länkade tjänsten länkar till din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-237">hello Azure Storage linked service links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="ee1e4-238">I den här självstudiekursen används hello samma lagringskonto som hello HDInsight standardkontot för lagring, lagring av indata och utdata datalagring.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-238">In this tutorial, hello same storage account is used as hello default HDInsight storage account, input data storage, and output data storage.</span></span> <span data-ttu-id="ee1e4-239">Därför kan definiera du endast en Azure Storage länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-239">Therefore, you define only one Azure Storage linked service.</span></span> <span data-ttu-id="ee1e4-240">I hello länkade tjänstdefinitionen ange hello namn och nyckel för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-240">In hello linked service definition, you specify hello name and key of your Azure storage account.</span></span> <span data-ttu-id="ee1e4-241">Se [Azure länkade lagringstjänsten](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om toodefine för JSON-egenskaper som används för ett Azure Storage länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-241">See [Azure Storage linked service](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span>

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
<span data-ttu-id="ee1e4-242">Hej **connectionString** använder hello storageAccountName och storageAccountKey parametrar.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-242">hello **connectionString** uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="ee1e4-243">Du anger värden för dessa parametrar när du distribuerar hello mall.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-243">You specify values for these parameters while deploying hello template.</span></span>  

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="ee1e4-244">HDInsight on-demand linked service (Länkad tjänst för HDInsight på begäran)</span><span class="sxs-lookup"><span data-stu-id="ee1e4-244">HDInsight on-demand linked service</span></span>
<span data-ttu-id="ee1e4-245">Hello på begäran HDInsight länkade tjänstdefinitionen kan du ange värden för konfigurationsparametrar som används av hello Data Factory-tjänsten toocreate en HDInsight Hadoop-kluster vid körning.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-245">In hello on-demand HDInsight linked service definition, you specify values for configuration parameters that are used by hello Data Factory service toocreate a HDInsight Hadoop cluster at runtime.</span></span> <span data-ttu-id="ee1e4-246">Se [Compute länkade tjänster](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) artikeln för information om toodefine för JSON-egenskaper som används för en länkad tjänst för HDInsight-på begäran.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-246">See [Compute linked services](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used toodefine an HDInsight on-demand linked service.</span></span>  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
<span data-ttu-id="ee1e4-247">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-247">Note hello following points:</span></span>

* <span data-ttu-id="ee1e4-248">hello Data Factory skapar en **Linux-baserade** HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-248">hello Data Factory creates a **Linux-based** HDInsight cluster for you.</span></span>
* <span data-ttu-id="ee1e4-249">Hej HDInsight Hadoop-kluster skapas i hello samma region som hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-249">hello HDInsight Hadoop cluster is created in hello same region as hello storage account.</span></span>
* <span data-ttu-id="ee1e4-250">Meddelande hello *timeToLive* inställningen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-250">Notice hello *timeToLive* setting.</span></span> <span data-ttu-id="ee1e4-251">Hej datafabriken tar bort hello klustret automatiskt när hello klustret är att ha varit inaktiv i 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-251">hello data factory deletes hello cluster automatically after hello cluster is being idle for 30 minutes.</span></span>
* <span data-ttu-id="ee1e4-252">Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-252">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="ee1e4-253">HDInsight tar inte bort den här behållaren när hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-253">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="ee1e4-254">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-254">This behavior is by design.</span></span> <span data-ttu-id="ee1e4-255">Med länkad HDInsight-tjänsten på begäran, ett HDInsight-kluster skapas varje gång ett segment måste toobe bearbetas såvida det inte finns ett befintligt live kluster (**timeToLive**) och tas bort när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-255">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>

<span data-ttu-id="ee1e4-256">Se [HDInsight-länkad tjänst på begäran](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-256">See [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee1e4-257">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-257">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="ee1e4-258">Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-258">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="ee1e4-259">hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-259">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="ee1e4-260">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-260">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="ee1e4-261">Indatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="ee1e4-261">Azure blob input dataset</span></span>
<span data-ttu-id="ee1e4-262">Ange hello namnen på blob-behållare, mapp och fil som innehåller hello indata i hello inkommande datauppsättningsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-262">In hello input dataset definition, you specify hello names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="ee1e4-263">Se [Azure Blob-egenskaper för datamängd](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-263">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}

```

<span data-ttu-id="ee1e4-264">Observera hello efter specifika inställningar i hello JSON-definitionen:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-264">Notice hello following specific settings in hello JSON definition:</span></span>

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="ee1e4-265">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="ee1e4-265">Azure Blob output dataset</span></span>
<span data-ttu-id="ee1e4-266">Ange hello namnen på blob-behållaren och mappen som innehåller hello utdata i datauppsättningsdefinitionen för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-266">In hello output dataset definition, you specify hello names of blob container and folder that holds hello output data.</span></span> <span data-ttu-id="ee1e4-267">Se [Azure Blob-egenskaper för datamängd](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-267">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

<span data-ttu-id="ee1e4-268">hello folderPath anger hello toohello sökväg som innehåller hello utdata:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-268">hello folderPath specifies hello path toohello folder that holds hello output data:</span></span>

```json
"folderPath": "adfgetstarted/partitioneddata",
```

<span data-ttu-id="ee1e4-269">Hej [dataset tillgänglighet](../data-factory/data-factory-create-datasets.md#dataset-availability) inställningen är följande:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-269">hello [dataset availability](../data-factory/data-factory-create-datasets.md#dataset-availability) setting is as follows:</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

<span data-ttu-id="ee1e4-270">Utdata dataset tillgänglighet enheter hello pipeline i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-270">In Azure Data Factory, output dataset availability drives hello pipeline.</span></span> <span data-ttu-id="ee1e4-271">I det här exemplet skapas hello sektorn månad på hello sista dagen i månaden (EndOfInterval).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-271">In this example, hello slice is produced monthly on hello last day of month (EndOfInterval).</span></span> <span data-ttu-id="ee1e4-272">Mer information finns i [Data Factory schemaläggning och körning av](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-272">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

#### <a name="data-pipeline"></a><span data-ttu-id="ee1e4-273">Datapipeline</span><span class="sxs-lookup"><span data-stu-id="ee1e4-273">Data pipeline</span></span>
<span data-ttu-id="ee1e4-274">Du kan definiera en pipeline som omvandlar data genom att köra Hive-skript på en Azure HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-274">You define a pipeline that transforms data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="ee1e4-275">Se [Pipeline-JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) beskrivningar av JSON-element som används toodefine en pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-275">See [Pipeline JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span>

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="ee1e4-276">hello pipelinen innehåller en aktivitet, HDInsightHive aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-276">hello pipeline contains one activity, HDInsightHive activity.</span></span> <span data-ttu-id="ee1e4-277">Som både start- och slutdatum är i januari 2016 data för endast en månad (ett segment) har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-277">As both start and end dates are in January 2016, data for only one month (a slice) is processed.</span></span> <span data-ttu-id="ee1e4-278">Båda *starta* och *end* hello aktivitet har en tidigare tidpunkt, så hello Data Factory bearbetar data i hello månad omedelbart.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-278">Both *start* and *end* of hello activity have a past date, so hello Data Factory processes data for hello month immediately.</span></span> <span data-ttu-id="ee1e4-279">Om hello end är ett datum i framtiden, skapar hello data factory ett annat segment när hello tid kommer.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-279">If hello end is a future date, hello data factory creates another slice when hello time comes.</span></span> <span data-ttu-id="ee1e4-280">Mer information finns i [Data Factory schemaläggning och körning av](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-280">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

## <a name="clean-up-hello-tutorial"></a><span data-ttu-id="ee1e4-281">Rensa hello självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="ee1e4-281">Clean up hello tutorial</span></span>

### <a name="delete-hello-blob-containers-created-by-on-demand-hdinsight-cluster"></a><span data-ttu-id="ee1e4-282">Ta bort hello blob-behållare som skapats av HDInsight-kluster på begäran</span><span class="sxs-lookup"><span data-stu-id="ee1e4-282">Delete hello blob containers created by on-demand HDInsight cluster</span></span>
<span data-ttu-id="ee1e4-283">Med länkad HDInsight-tjänsten på begäran skapas ett HDInsight-kluster varje gång ett segment måste toobe bearbetas såvida det inte finns ett befintligt live kluster (timeToLive) och hello klustret tas bort när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-283">With on-demand HDInsight linked service, an HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (timeToLive); and hello cluster is deleted when hello processing is done.</span></span> <span data-ttu-id="ee1e4-284">För varje kluster skapar Azure Data Factory en blob-behållare i hello Azure blob-lagring som används som hello stroage standardkontot för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-284">For each cluster, Azure Data Factory creates a blob container in hello Azure blob storage used as hello default stroage account for hello cluster.</span></span> <span data-ttu-id="ee1e4-285">Även om HDInsight-kluster tas bort hello standard blob storage-behållare och hello associerade lagringskontot tas inte bort.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-285">Even though HDInsight cluster is deleted, hello default blob storage container and hello associated storage account are not deleted.</span></span> <span data-ttu-id="ee1e4-286">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-286">This behavior is by design.</span></span> <span data-ttu-id="ee1e4-287">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-287">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="ee1e4-288">Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-288">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="ee1e4-289">hello namnen på de här behållarna följer ett mönster: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-289">hello names of these containers follow a pattern: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span></span>

<span data-ttu-id="ee1e4-290">Ta bort hello **adfjobs** och **adfyourdatafactoryname-linkedservicename-datetimestamp** mappar.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-290">Delete hello **adfjobs** and **adfyourdatafactoryname-linkedservicename-datetimestamp** folders.</span></span> <span data-ttu-id="ee1e4-291">Hej adfjobs behållaren innehåller jobbloggar från Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-291">hello adfjobs container contains job logs from Azure Data Factory.</span></span>

### <a name="delete-hello-resource-group"></a><span data-ttu-id="ee1e4-292">Ta bort hello resursgruppen</span><span class="sxs-lookup"><span data-stu-id="ee1e4-292">Delete hello resource group</span></span>
<span data-ttu-id="ee1e4-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) är används toodeploy, hantera och övervaka din lösning som en grupp.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) is used toodeploy, manage, and monitor your solution as a group.</span></span>  <span data-ttu-id="ee1e4-294">Tar bort en resursgrupp alla hello komponenter inuti hello grupp.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-294">Deleting a resource group deletes all hello components inside hello group.</span></span>  

1. <span data-ttu-id="ee1e4-295">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-295">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ee1e4-296">Klicka på **resursgrupper** hello vänster.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-296">Click **Resource groups** on hello left pane.</span></span>
3. <span data-ttu-id="ee1e4-297">Klicka på hello resursgruppens namn du skapade i PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-297">Click hello resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="ee1e4-298">Använda hello filtret om du har för många resursgrupper i listan.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-298">Use hello filter if you have too many resource groups listed.</span></span> <span data-ttu-id="ee1e4-299">Hello resursgrupp öppnas i ett nytt blad.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-299">It opens hello resource group in a new blade.</span></span>
4. <span data-ttu-id="ee1e4-300">På hello **resurser** sida vid sida, har du hello standardkontot för lagring och hello data factory om du delar hello resursgrupp med andra projekt i listan.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-300">On hello **Resources** tile, you shall have hello default storage account and hello data factory listed unless you share hello resource group with other projects.</span></span>
5. <span data-ttu-id="ee1e4-301">Klicka på **ta bort** hello längst upp på bladet hello.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-301">Click **Delete** on hello top of hello blade.</span></span> <span data-ttu-id="ee1e4-302">Detta tar bort hello storage-konto och hello data som lagras i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-302">Doing so deletes hello storage account and hello data stored in hello storage account.</span></span>
6. <span data-ttu-id="ee1e4-303">Ange hello resurs grupp namnet tooconfirm togs bort och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-303">Enter hello resource group name tooconfirm deletion, and then click **Delete**.</span></span>

<span data-ttu-id="ee1e4-304">Om du inte vill toodelete hello storage-konto när du tar bort hello resursgrupp, Överväg hello följande arkitektur genom att avgränsa hello affärsdata från hello standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-304">In case you don't want toodelete hello storage account when you delete hello resource group, consider hello following architecture by separating hello business data from hello default storage account.</span></span> <span data-ttu-id="ee1e4-305">I så fall måste du har en resursgrupp för hello storage-konto med hello affärsdata och hello andra resursgruppen för hello standardkontot för lagring för HDInsight länkade tjänsten och hello data factory.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-305">In this case, you have one resource group for hello storage account with hello business data, and hello other resource group for hello default storage account for HDInsight linked service and hello data factory.</span></span> <span data-ttu-id="ee1e4-306">När du tar bort hello andra resursgruppen påverkar inte hello business data storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-306">When you delete hello second resource group, it does not impact hello business data storage account.</span></span> <span data-ttu-id="ee1e4-307">toodo så:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-307">toodo so:</span></span>

* <span data-ttu-id="ee1e4-308">Lägg till hello följande toohello översta resursgruppen tillsammans med hello Microsoft.DataFactory/datafactories resurs i Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-308">Add hello following toohello top-level resource group along with hello Microsoft.DataFactory/datafactories resource in your Resource Manager template.</span></span> <span data-ttu-id="ee1e4-309">Den skapar ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-309">It creates a storage account:</span></span>

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* <span data-ttu-id="ee1e4-310">Lägg till ett nytt länkade tjänsten punkt toohello nya lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-310">Add a new linked service point toohello new storage account:</span></span>

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* <span data-ttu-id="ee1e4-311">Konfigurera hello HDInsight ondemand länkad tjänst med en ytterligare dependsOn och en additionalLinkedServiceNames:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-311">Configure hello HDInsight ondemand linked service with an additional dependsOn and an additionalLinkedServiceNames:</span></span>

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a><span data-ttu-id="ee1e4-312">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee1e4-312">Next steps</span></span>
<span data-ttu-id="ee1e4-313">I den här artikeln har du lärt dig hur toouse Azure Data Factory toocreate på begäran HDInsight-kluster tooprocess Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-313">In this article, you have learned how toouse Azure Data Factory toocreate on-demand HDInsight cluster tooprocess Hive jobs.</span></span> <span data-ttu-id="ee1e4-314">tooread mer:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-314">tooread more:</span></span>

* [<span data-ttu-id="ee1e4-315">Hadoop-vägledning: komma igång med Linux-baserade Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee1e4-315">Hadoop tutorial: Get started using Linux-based Hadoop in HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="ee1e4-316">Skapa Linux-baserade Hadoop-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee1e4-316">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="ee1e4-317">HDInsight-dokumentation</span><span class="sxs-lookup"><span data-stu-id="ee1e4-317">HDInsight documentation</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="ee1e4-318">Data factory-dokumentation</span><span class="sxs-lookup"><span data-stu-id="ee1e4-318">Data factory documentation</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a><span data-ttu-id="ee1e4-319">Bilaga</span><span class="sxs-lookup"><span data-stu-id="ee1e4-319">Appendix</span></span>

### <a name="azure-cli-script"></a><span data-ttu-id="ee1e4-320">Azure CLI-skript</span><span class="sxs-lookup"><span data-stu-id="ee1e4-320">Azure CLI script</span></span>
<span data-ttu-id="ee1e4-321">Du kan använda Azure CLI istället för att använda Azure PowerShell toodo hello kursen.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-321">You can use Azure CLI instead of using Azure PowerShell toodo hello tutorial.</span></span> <span data-ttu-id="ee1e4-322">först installera toouse Azure CLI, Azure CLI enligt instruktionerna hello:</span><span class="sxs-lookup"><span data-stu-id="ee1e4-322">toouse Azure CLI, first install Azure CLI as per hello following instructions:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-tooprepare-hello-storage-and-copy-hello-files"></a><span data-ttu-id="ee1e4-323">Använda Azure CLI tooprepare hello lagring och kopiera hello-filer</span><span class="sxs-lookup"><span data-stu-id="ee1e4-323">Use Azure CLI tooprepare hello storage and copy hello files</span></span>

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

<span data-ttu-id="ee1e4-324">hello behållarens namn är *adfgetstarted*.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-324">hello container name is *adfgetstarted*.</span></span> <span data-ttu-id="ee1e4-325">Se till att den som den är.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-325">Keep it as it is.</span></span> <span data-ttu-id="ee1e4-326">Annars måste tooupdate hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="ee1e4-326">Otherwise you need tooupdate hello Resource Manager template.</span></span> <span data-ttu-id="ee1e4-327">Om du behöver hjälp med skriptet CLI, se [Using hello Azure CLI med Azure Storage](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ee1e4-327">If you need help with this CLI script, see [Using hello Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>
