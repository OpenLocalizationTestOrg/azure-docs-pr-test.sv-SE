---
title: "Skapa på begäran Hadoop-kluster med hjälp av Data Factory - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du skapar på begäran Hadoop-kluster i HDInsight med hjälp av Azure Data Factory."
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
ms.openlocfilehash: e68f1d72965d9516e0552c84d03d234c21739390
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a><span data-ttu-id="8bec7-103">Skapa på begäran Hadoop-kluster i HDInsight med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="8bec7-103">Create on-demand Hadoop clusters in HDInsight using Azure Data Factory</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="8bec7-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) är en molnbaserad integration datatjänst som samordnar och automatiserar flytt och transformering av data.</span><span class="sxs-lookup"><span data-stu-id="8bec7-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) is a cloud-based data integration service that orchestrates and automates the movement and transformation of data.</span></span> <span data-ttu-id="8bec7-105">Det kan skapa en HDInsight Hadoop-kluster just-in-time för att bearbeta en inkommande datasektorn och tar bort klustret när bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="8bec7-105">It can create a HDInsight Hadoop cluster just-in-time to process an input data slice and delete the cluster when the processing is complete.</span></span> <span data-ttu-id="8bec7-106">Några av fördelarna med att använda ett HDInsight Hadoop-kluster på begäran är:</span><span class="sxs-lookup"><span data-stu-id="8bec7-106">Some of the benefits of using an on-demand HDInsight Hadoop cluster are:</span></span>

- <span data-ttu-id="8bec7-107">Du endast betala för tid jobbet körs på HDInsight Hadoop-kluster (plus en kort konfigurerbara inaktivitetstid).</span><span class="sxs-lookup"><span data-stu-id="8bec7-107">You only pay for the time job is running on the HDInsight Hadoop cluster (plus a brief configurable idle time).</span></span> <span data-ttu-id="8bec7-108">Fakturering för HDInsight-kluster sker proportionerligt per minut, oavsett om du använder dem eller inte.</span><span class="sxs-lookup"><span data-stu-id="8bec7-108">The billing for HDInsight clusters is pro-rated per minute, whether you are using them or not.</span></span> <span data-ttu-id="8bec7-109">När du använder en på-begäran länkad HDInsight-tjänst i Data Factory skapas kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="8bec7-109">When you use an on-demand HDInsight linked service in Data Factory, the clusters are created on-demand.</span></span> <span data-ttu-id="8bec7-110">Och kluster tas bort automatiskt när jobben har slutförts.</span><span class="sxs-lookup"><span data-stu-id="8bec7-110">And the clusters are deleted automatically when the jobs are completed.</span></span> <span data-ttu-id="8bec7-111">Därför kan betalar du bara för jobbet kör tid och kort ledig tid (time to live-inställning).</span><span class="sxs-lookup"><span data-stu-id="8bec7-111">Therefore, you only pay for the job running time and the brief idle time (time-to-live setting).</span></span>
- <span data-ttu-id="8bec7-112">Du kan skapa ett arbetsflöde med Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-112">You can create a workflow using a Data Factory pipeline.</span></span> <span data-ttu-id="8bec7-113">Du kan till exempel ha rörledning för att kopiera data från en lokal SQL Server till ett Azure blob storage, bearbeta data genom att köra en Hive-skript och Pig-skriptet på en HDInsight Hadoop-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="8bec7-113">For example, you can have the pipeline to copy data from an on-premises SQL Server to an Azure blob storage, process the data by running a Hive script and a Pig script on an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="8bec7-114">Kopiera sedan Resultatdata till en Azure SQL Data Warehouse BI program ska använda.</span><span class="sxs-lookup"><span data-stu-id="8bec7-114">Then, copy the result data to an Azure SQL Data Warehouse for BI applications to consume.</span></span>
- <span data-ttu-id="8bec7-115">Du kan schemalägga arbetsflödet ska köras regelbundet (varje timme, varje dag, vecka, månad, etc.).</span><span class="sxs-lookup"><span data-stu-id="8bec7-115">You can schedule the workflow to run periodically (hourly, daily, weekly, monthly, etc.).</span></span>

<span data-ttu-id="8bec7-116">En datafabrik kan ha en eller flera data pipelines i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="8bec7-116">In Azure Data Factory, a data factory can have one or more data pipelines.</span></span> <span data-ttu-id="8bec7-117">En data-pipeline har en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="8bec7-117">A data pipeline has one or more activities.</span></span> <span data-ttu-id="8bec7-118">Det finns två typer av aktiviteter: [Data Movement aktiviteter](../data-factory/data-factory-data-movement-activities.md) och [Data Transformation aktiviteter](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="8bec7-118">There are two types of activities: [Data Movement Activities](../data-factory/data-factory-data-movement-activities.md) and [Data Transformation Activities](../data-factory/data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="8bec7-119">Du kan använda data movement aktiviteter (för närvarande endast Kopieringsaktivitet) för att flytta data från ett dataarkiv som källa till ett dataarkiv som mål.</span><span class="sxs-lookup"><span data-stu-id="8bec7-119">You use data movement activities (currently, only Copy Activity) to move data from a source data store to a destination data store.</span></span> <span data-ttu-id="8bec7-120">Du kan använda data transformation aktiviteter för att transformera/bearbeta data.</span><span class="sxs-lookup"><span data-stu-id="8bec7-120">You use data transformation activities to transform/process data.</span></span> <span data-ttu-id="8bec7-121">HDInsight Hive aktivitet är en omvandling av aktiviteter som stöds av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="8bec7-121">HDInsight Hive Activity is one of the transformation activities supported by Data Factory.</span></span> <span data-ttu-id="8bec7-122">Du använder Hive omvandling aktiviteten i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-122">You use the Hive transformation activity in this tutorial.</span></span>

<span data-ttu-id="8bec7-123">Du kan konfigurera en hive-aktivitet för att använda ditt eget HDInsight Hadoop-kluster eller en HDInsight Hadoop-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="8bec7-123">You can configure a hive activity to use your own HDInsight Hadoop cluster or an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="8bec7-124">I de här självstudierna har Hive-aktiviteten i data factory-pipelinen konfigurerats för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="8bec7-124">In this tutorial, the Hive activity in the data factory pipeline is configured to use an on-demand HDInsight cluster.</span></span> <span data-ttu-id="8bec7-125">Därför när aktiviteten körs för att bearbeta en datasektorn är här vad som händer:</span><span class="sxs-lookup"><span data-stu-id="8bec7-125">Therefore, when the activity runs to process a data slice, here is what happens:</span></span>

1. <span data-ttu-id="8bec7-126">Ett HDInsight Hadoop-kluster skapas automatiskt för du just-in-time bearbeta sektorn.</span><span class="sxs-lookup"><span data-stu-id="8bec7-126">A HDInsight Hadoop cluster is automatically created for you just-in-time to process the slice.</span></span>  
2. <span data-ttu-id="8bec7-127">Indata bearbetas av en HiveQL-skript körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="8bec7-127">The input data is processed by running a HiveQL script on the cluster.</span></span>
3. <span data-ttu-id="8bec7-128">HDInsight Hadoop-kluster tas bort när bearbetningen är klar och klustret är inaktiv under den konfigurerade tidsperiod (timeToLive-inställning).</span><span class="sxs-lookup"><span data-stu-id="8bec7-128">The HDInsight Hadoop cluster is deleted after the processing is complete and the cluster is idle for the configured amount of time (timeToLive setting).</span></span> <span data-ttu-id="8bec7-129">Om nästa datasektorn är tillgängliga för bearbetning med i den här timeToLive väntetid används samma kluster att bearbeta sektorn.</span><span class="sxs-lookup"><span data-stu-id="8bec7-129">If the next data slice is available for processing with in this timeToLive idle time, the same cluster is used to process the slice.</span></span>  

<span data-ttu-id="8bec7-130">I den här självstudiekursen utför HiveQL-skript som är associerade med hive aktiviteten följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="8bec7-130">In this tutorial, the HiveQL script associated with the hive activity performs the following actions:</span></span>

1. <span data-ttu-id="8bec7-131">Skapar en extern tabell som refererar till rå web logga data som lagras i ett Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="8bec7-131">Creates an external table that references the raw web log data stored in an Azure Blob storage.</span></span>
2. <span data-ttu-id="8bec7-132">Partitioner rådata per år och månad.</span><span class="sxs-lookup"><span data-stu-id="8bec7-132">Partitions the raw data by year and month.</span></span>
3. <span data-ttu-id="8bec7-133">Lagrar den partitionerade data i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="8bec7-133">Stores the partitioned data in the Azure blob storage.</span></span>

<span data-ttu-id="8bec7-134">I den här självstudiekursen skapar HiveQL-skript som är associerad med aktiviteten hive en extern tabell som refererar till rå web logga data som lagras i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8bec7-134">In this tutorial, the HiveQL script associated with the hive activity creates an external table that references the raw web log data stored in the Azure Blob Storage.</span></span> <span data-ttu-id="8bec7-135">Här följer exempel rader för varje månad i indatafilen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-135">Here are the sample rows for each month in the input file.</span></span>

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

<span data-ttu-id="8bec7-136">HiveQL-skript partitionerar rådata per år och månad.</span><span class="sxs-lookup"><span data-stu-id="8bec7-136">The HiveQL script partitions the raw data by year and month.</span></span> <span data-ttu-id="8bec7-137">Den skapar tre utdatamappar baserat på tidigare indata.</span><span class="sxs-lookup"><span data-stu-id="8bec7-137">It creates three output folders based on the previous input.</span></span> <span data-ttu-id="8bec7-138">Varje mapp innehåller en fil med poster från varje månad.</span><span class="sxs-lookup"><span data-stu-id="8bec7-138">Each folder contains a file with entries from each month.</span></span>

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

<span data-ttu-id="8bec7-139">En lista över Data Factory data transformation aktiviteter utöver Hive aktivitet finns [transformera och analysera med hjälp av Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="8bec7-139">For a list of Data Factory data transformation activities in addition to Hive activity, see [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="8bec7-140">För närvarande kan du bara skapa HDInsight-kluster av version 3.2 från Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="8bec7-140">Currently, you can only create HDInsight cluster version 3.2 from Azure Data Factory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8bec7-141">Krav</span><span class="sxs-lookup"><span data-stu-id="8bec7-141">Prerequisites</span></span>
<span data-ttu-id="8bec7-142">Innan du börjar med instruktionerna i den här artikeln måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="8bec7-142">Before you begin the instructions in this article, you must have the following items:</span></span>

* <span data-ttu-id="8bec7-143">[Azure-prenumeration](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="8bec7-143">[Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="8bec7-144">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8bec7-144">Azure PowerShell.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a><span data-ttu-id="8bec7-145">Förbereda storage-konto</span><span class="sxs-lookup"><span data-stu-id="8bec7-145">Prepare storage account</span></span>
<span data-ttu-id="8bec7-146">Du kan använda upp till tre storage-konton i det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="8bec7-146">You can use up to three storage accounts in this scenario:</span></span>

- <span data-ttu-id="8bec7-147">standardkontot för lagring för HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="8bec7-147">default storage account for the HDInsight cluster</span></span>
- <span data-ttu-id="8bec7-148">lagringskontot för indata</span><span class="sxs-lookup"><span data-stu-id="8bec7-148">storage account for the input data</span></span>
- <span data-ttu-id="8bec7-149">lagringskontot för utdata</span><span class="sxs-lookup"><span data-stu-id="8bec7-149">storage account for the output data</span></span>

<span data-ttu-id="8bec7-150">För att förenkla kursen, använder du ett lagringskonto för att de tre syften.</span><span class="sxs-lookup"><span data-stu-id="8bec7-150">To simplify the tutorial, you use one storage account to serve the three purposes.</span></span> <span data-ttu-id="8bec7-151">Azure PowerShell-exempelskript finns i det här avsnittet utför följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8bec7-151">The Azure PowerShell sample script found in this section performs the following tasks:</span></span>

1. <span data-ttu-id="8bec7-152">Logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="8bec7-152">Log in to Azure.</span></span>
2. <span data-ttu-id="8bec7-153">Skapa en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8bec7-153">Create an Azure resource group.</span></span>
3. <span data-ttu-id="8bec7-154">Skapa ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8bec7-154">Create an Azure Storage account.</span></span>
4. <span data-ttu-id="8bec7-155">Skapa en blobbbehållare i storage-konto</span><span class="sxs-lookup"><span data-stu-id="8bec7-155">Create a Blob container in the storage account</span></span>
5. <span data-ttu-id="8bec7-156">Kopiera följande två filer till Blob-behållare:</span><span class="sxs-lookup"><span data-stu-id="8bec7-156">Copy the following two files to the Blob container:</span></span>

   * <span data-ttu-id="8bec7-157">Indatafilen: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span><span class="sxs-lookup"><span data-stu-id="8bec7-157">Input data file: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span></span>
   * <span data-ttu-id="8bec7-158">HiveQL-skript: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span><span class="sxs-lookup"><span data-stu-id="8bec7-158">HiveQL script: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span></span>

     <span data-ttu-id="8bec7-159">Filer som lagras i en offentlig Blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="8bec7-159">Both files are stored in a public Blob container.</span></span>


<span data-ttu-id="8bec7-160">**Förbered lagring och kopiera filerna med hjälp av Azure PowerShell:**</span><span class="sxs-lookup"><span data-stu-id="8bec7-160">**To prepare the storage and copy the files using Azure PowerShell:**</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8bec7-161">Ange namn för Azure-resursgrupp och Azure storage-konto som skapas av skriptet.</span><span class="sxs-lookup"><span data-stu-id="8bec7-161">Specify names for the Azure resource group and the Azure storage account that will be created by the script.</span></span>
> <span data-ttu-id="8bec7-162">Skriv ned **resursgruppnamn**, **lagringskontonamnet**, och **lagringskontonyckel** utdata av skriptet.</span><span class="sxs-lookup"><span data-stu-id="8bec7-162">Write down **resource group name**, **storage account name**, and **storage account key** outputted by the script.</span></span> <span data-ttu-id="8bec7-163">Du behöver dem i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8bec7-163">You need them in the next section.</span></span>

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
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

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

<span data-ttu-id="8bec7-164">Om du behöver hjälp med PowerShell-skript finns [med hjälp av Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="8bec7-164">If you need help with the PowerShell script, see [Using the Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span> <span data-ttu-id="8bec7-165">Om du vill använda Azure CLI i stället se den [bilaga](#appendix) avsnittet för Azure CLI-skript.</span><span class="sxs-lookup"><span data-stu-id="8bec7-165">If you like to use Azure CLI instead, see the [Appendix](#appendix) section for the Azure CLI script.</span></span>

<span data-ttu-id="8bec7-166">**Granska lagringskontot och innehållet**</span><span class="sxs-lookup"><span data-stu-id="8bec7-166">**To examine the storage account and the contents**</span></span>

1. <span data-ttu-id="8bec7-167">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8bec7-167">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8bec7-168">Klicka på **resursgrupper** i det vänstra fönstret.</span><span class="sxs-lookup"><span data-stu-id="8bec7-168">Click **Resource groups** on the left pane.</span></span>
3. <span data-ttu-id="8bec7-169">Dubbelklicka på resursgruppens namn du skapade i PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="8bec7-169">Double-click the resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="8bec7-170">Med filtret om du har för många resursgrupper i listan.</span><span class="sxs-lookup"><span data-stu-id="8bec7-170">Use the filter if you have too many resource groups listed.</span></span>
4. <span data-ttu-id="8bec7-171">På den **resurser** sida vid sida, har du en resurs i listan om du delar resursgruppen med andra projekt.</span><span class="sxs-lookup"><span data-stu-id="8bec7-171">On the **Resources** tile, you shall have one resource listed unless you share the resource group with other projects.</span></span> <span data-ttu-id="8bec7-172">Den här resursen är lagringskontot med det namn du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="8bec7-172">That resource is the storage account with the name you specified earlier.</span></span> <span data-ttu-id="8bec7-173">Klicka på namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="8bec7-173">Click the storage account name.</span></span>
5. <span data-ttu-id="8bec7-174">Klicka på den **Blobbar** paneler.</span><span class="sxs-lookup"><span data-stu-id="8bec7-174">Click the **Blobs** tiles.</span></span>
6. <span data-ttu-id="8bec7-175">Klicka på den **adfgetstarted** behållare.</span><span class="sxs-lookup"><span data-stu-id="8bec7-175">Click the **adfgetstarted** container.</span></span> <span data-ttu-id="8bec7-176">Du ser två mappar: **inputdata** och **skriptet**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-176">You see two folders: **inputdata** and **script**.</span></span>
7. <span data-ttu-id="8bec7-177">Öppna mappen och kontrollera filer i mappar.</span><span class="sxs-lookup"><span data-stu-id="8bec7-177">Open the folder and check the files in the folders.</span></span> <span data-ttu-id="8bec7-178">Inputdata innehåller input.log filen med indata och mappen skript innehåller filen HiveQL-skript.</span><span class="sxs-lookup"><span data-stu-id="8bec7-178">The inputdata contains the input.log file with input data and the script folder contains the HiveQL script file.</span></span>

## <a name="create-a-data-factory-using-resource-manager-template"></a><span data-ttu-id="8bec7-179">Skapa en datafabrik med hjälp av Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="8bec7-179">Create a data factory using Resource Manager template</span></span>
<span data-ttu-id="8bec7-180">När lagringskontot indata och HiveQL-skript som har förberetts, är du redo att skapa ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="8bec7-180">With the storage account, the input data, and the HiveQL script prepared, you are ready to create an Azure data factory.</span></span> <span data-ttu-id="8bec7-181">Det finns flera metoder för att skapa datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8bec7-181">There are several methods for creating data factory.</span></span> <span data-ttu-id="8bec7-182">I kursen får skapa du en datafabrik genom att distribuera en Azure Resource Manager-mallen med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-182">In this tutorial, you create a data factory by deploying an Azure Resource Manager template using the Azure portal.</span></span> <span data-ttu-id="8bec7-183">Du kan också distribuera en Resource Manager-mall med hjälp av [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) och [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="8bec7-183">You can also deploy a Resource Manager template by using [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) and [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span> <span data-ttu-id="8bec7-184">Andra data factory-metoder, se [Självstudier: skapa din första data factory](../data-factory/data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="8bec7-184">For other data factory creation methods, see [Tutorial: Build your first data factory](../data-factory/data-factory-build-your-first-pipeline.md).</span></span>

1. <span data-ttu-id="8bec7-185">Klicka på följande bild för att logga in på Azure och öppna Resource Manager-mallen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="8bec7-185">Click the following image to sign in to Azure and open the Resource Manager template in the Azure portal.</span></span> <span data-ttu-id="8bec7-186">Mallen finns i https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span><span class="sxs-lookup"><span data-stu-id="8bec7-186">The template is located at https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span></span> <span data-ttu-id="8bec7-187">Finns det [Data Factory-entiteter i mallen](#data-factory-entities-in-the-template) finns detaljerad information om enheter som definierats i mallen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-187">See the [Data Factory entities in the template](#data-factory-entities-in-the-template) section for detailed information about entities defined in the template.</span></span> 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. <span data-ttu-id="8bec7-188">Välj **Använd befintliga** för den **resursgruppen** inställningen och välj namnet på resursgruppen som du skapade i föregående steg (med PowerShell-skript).</span><span class="sxs-lookup"><span data-stu-id="8bec7-188">Select **Use existing** option for the **Resource group** setting, and select the name of the resource group you created in the previous step (using PowerShell script).</span></span>
3. <span data-ttu-id="8bec7-189">Ange ett namn för data factory (**Datafabriksnamnet**).</span><span class="sxs-lookup"><span data-stu-id="8bec7-189">Enter a name for the data factory (**Data Factory Name**).</span></span> <span data-ttu-id="8bec7-190">Det här namnet måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="8bec7-190">This name must be globally unique.</span></span>
4. <span data-ttu-id="8bec7-191">Ange den **lagringskontonamnet** och **lagringskontonyckel** du noterade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="8bec7-191">Enter the **storage account name** and **storage account key** you wrote down in the previous step.</span></span>
5. <span data-ttu-id="8bec7-192">Välj **jag samtycker till villkoren** anges ovan när du har läst **villkor**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-192">Select **I agree to the terms and conditions** stated above after reading through **terms and conditions**.</span></span>
6. <span data-ttu-id="8bec7-193">Välj **fäst på instrumentpanelen** alternativet.</span><span class="sxs-lookup"><span data-stu-id="8bec7-193">Select **Pin to dashboard** option.</span></span>
6. <span data-ttu-id="8bec7-194">Klicka på **inköp/skapa**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-194">Click **Purchase/Create**.</span></span> <span data-ttu-id="8bec7-195">Du ser en panel på instrumentpanelen kallas **skicka malldistribution**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-195">You see a tile on the Dashboard called **Deploying Template deployment**.</span></span> <span data-ttu-id="8bec7-196">Vänta tills den **resursgruppen** öppnas bladet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-196">Wait until the **Resource group** blade for your resource group opens.</span></span> <span data-ttu-id="8bec7-197">Du kan också klicka på panelen med titeln som din resursgruppens namn att öppna bladet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-197">You can also click the tile titled as your resource group name to open the resource group blade.</span></span>
6. <span data-ttu-id="8bec7-198">Klicka på panelen om du vill öppna resursgruppen om bladet för resursgruppen inte är redan öppen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-198">Click the tile to open the resource group if the resource group blade is not already open.</span></span> <span data-ttu-id="8bec7-199">Du bör nu se en mer data factory-resurs visas förutom kontot lagringsresurs.</span><span class="sxs-lookup"><span data-stu-id="8bec7-199">Now you shall see one more data factory resource listed in addition to the storage account resource.</span></span>
7. <span data-ttu-id="8bec7-200">Klicka på namnet på din data factory (värde som du angav för den **Datafabriksnamnet** parametern).</span><span class="sxs-lookup"><span data-stu-id="8bec7-200">Click the name of your data factory (value you specified for the **Data Factory Name** parameter).</span></span>
8. <span data-ttu-id="8bec7-201">I bladet Data Factory klickar du på den **Diagram** panelen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-201">In the Data Factory blade, click the **Diagram** tile.</span></span> <span data-ttu-id="8bec7-202">Diagrammet visar en aktivitet med en inkommande datauppsättning och en datamängd för utdata:</span><span class="sxs-lookup"><span data-stu-id="8bec7-202">The diagram shows one activity with an input dataset, and an output dataset:</span></span>

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline diagram](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    <span data-ttu-id="8bec7-204">Namn har definierats i Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-204">The names are defined in the Resource Manager template.</span></span>
9. <span data-ttu-id="8bec7-205">Dubbelklicka på **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-205">Double-click **AzureBlobOutput**.</span></span>
10. <span data-ttu-id="8bec7-206">På den **senaste uppdaterade segment**, visas ett segment.</span><span class="sxs-lookup"><span data-stu-id="8bec7-206">On the **Recent updated slices**, you shall see one slice.</span></span> <span data-ttu-id="8bec7-207">Om statusen är **pågår**, vänta tills den har ändrats till **klar**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-207">If the status is **In progress**, wait until it is changed to **Ready**.</span></span> <span data-ttu-id="8bec7-208">Det tar vanligtvis om **20 minuter** att skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bec7-208">It usually takes about **20 minutes** to create an HDInsight cluster.</span></span>

### <a name="check-the-data-factory-output"></a><span data-ttu-id="8bec7-209">Kontrollera data factory-utdata</span><span class="sxs-lookup"><span data-stu-id="8bec7-209">Check the data factory output</span></span>

1. <span data-ttu-id="8bec7-210">Använd samma procedur på den senaste sessionen för att söka behållaren adfgetstarted behållare.</span><span class="sxs-lookup"><span data-stu-id="8bec7-210">Use the same procedure in the last session to check the containers of the adfgetstarted container.</span></span> <span data-ttu-id="8bec7-211">Det finns två nya behållare förutom **adfgetsarted**:</span><span class="sxs-lookup"><span data-stu-id="8bec7-211">There are two new containers in addition to **adfgetsarted**:</span></span>

   * <span data-ttu-id="8bec7-212">En behållare med namn som följer mönstret: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="8bec7-212">A container with name that follows the pattern: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span></span> <span data-ttu-id="8bec7-213">Den här behållaren är standardbehållaren för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bec7-213">This container is the default container for the HDInsight cluster.</span></span>
   * <span data-ttu-id="8bec7-214">adfjobs: den här behållaren är behållare för ADF jobbloggar.</span><span class="sxs-lookup"><span data-stu-id="8bec7-214">adfjobs: This container is the container for the ADF job logs.</span></span>

     <span data-ttu-id="8bec7-215">Data factory-utdata är lagrad i **afgetstarted** som du konfigurerade i Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-215">The data factory output is stored in **afgetstarted** as you configured in the Resource Manager template.</span></span>
2. <span data-ttu-id="8bec7-216">Klicka på **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-216">Click **adfgetstarted**.</span></span>
3. <span data-ttu-id="8bec7-217">Dubbelklicka på **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-217">Double-click **partitioneddata**.</span></span> <span data-ttu-id="8bec7-218">Du ser en **år 2014 =** mappen eftersom alla webbloggar funktionsdokumentationen år 2014.</span><span class="sxs-lookup"><span data-stu-id="8bec7-218">You see a **year=2014** folder because all the web logs are dated in year 2014.</span></span>

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline utdata](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    <span data-ttu-id="8bec7-220">Om du detaljnivån i listan visas tre mappar för januari, februari och mars.</span><span class="sxs-lookup"><span data-stu-id="8bec7-220">If you drill down the list, you shall see three folders for January, February, and March.</span></span> <span data-ttu-id="8bec7-221">Och det finns en logg för varje månad.</span><span class="sxs-lookup"><span data-stu-id="8bec7-221">And there is a log for each month.</span></span>

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline utdata](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-the-template"></a><span data-ttu-id="8bec7-223">Data Factory-entiteter i mallen</span><span class="sxs-lookup"><span data-stu-id="8bec7-223">Data Factory entities in the template</span></span>
<span data-ttu-id="8bec7-224">Här är att den översta Resource Manager-mallen för en datafabrik som ser ut som:</span><span class="sxs-lookup"><span data-stu-id="8bec7-224">Here is how the top-level Resource Manager template for a data factory looks like:</span></span>

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

### <a name="define-data-factory"></a><span data-ttu-id="8bec7-225">Definiera en datafabrik</span><span class="sxs-lookup"><span data-stu-id="8bec7-225">Define data factory</span></span>
<span data-ttu-id="8bec7-226">Du definierar en datafabrik i Resource Manager-mallen enligt följande exempel:</span><span class="sxs-lookup"><span data-stu-id="8bec7-226">You define a data factory in the Resource Manager template as shown in the following sample:</span></span>  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
<span data-ttu-id="8bec7-227">DataFactoryName är namnet på datafabriken som du anger när du distribuerar mallen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-227">The dataFactoryName is the name of the data factory you specify when you deploy the template.</span></span> <span data-ttu-id="8bec7-228">Datafabriken är för närvarande stöds endast i östra USA, västra USA och Norra Europa regioner.</span><span class="sxs-lookup"><span data-stu-id="8bec7-228">Data factory is currently only supported in the East US, West US, and North Europe regions.</span></span>

### <a name="defining-entities-within-the-data-factory"></a><span data-ttu-id="8bec7-229">Definiera enheter inom datafabriken</span><span class="sxs-lookup"><span data-stu-id="8bec7-229">Defining entities within the data factory</span></span>
<span data-ttu-id="8bec7-230">Följande Data Factory-entiteter har definierats i JSON-mallen:</span><span class="sxs-lookup"><span data-stu-id="8bec7-230">The following Data Factory entities are defined in the JSON template:</span></span>

* [<span data-ttu-id="8bec7-231">Länkad Azure Storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="8bec7-231">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="8bec7-232">Länkad tjänst för HDInsight på begäran</span><span class="sxs-lookup"><span data-stu-id="8bec7-232">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="8bec7-233">Indatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="8bec7-233">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="8bec7-234">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="8bec7-234">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="8bec7-235">Datapipeline med en kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="8bec7-235">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="8bec7-236">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="8bec7-236">Azure Storage linked service</span></span>
<span data-ttu-id="8bec7-237">AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8bec7-237">The Azure Storage linked service links your Azure storage account to the data factory.</span></span> <span data-ttu-id="8bec7-238">I den här kursen används samma lagringskonto som HDInsight standardkontot för lagring, lagring av indata och utdata datalagring.</span><span class="sxs-lookup"><span data-stu-id="8bec7-238">In this tutorial, the same storage account is used as the default HDInsight storage account, input data storage, and output data storage.</span></span> <span data-ttu-id="8bec7-239">Därför kan definiera du endast en Azure Storage länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8bec7-239">Therefore, you define only one Azure Storage linked service.</span></span> <span data-ttu-id="8bec7-240">Länkad tjänst-definition anger du namn och nyckeln för ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8bec7-240">In the linked service definition, you specify the name and key of your Azure storage account.</span></span> <span data-ttu-id="8bec7-241">Se [Länkad Azure Storage-tjänst](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) om du vill ha information om JSON-egenskaper som används för att definiera en länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8bec7-241">See [Azure Storage linked service](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used to define an Azure Storage linked service.</span></span>

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
<span data-ttu-id="8bec7-242">**connectionString** använder parametrarna storageAccountName och storageAccountKey.</span><span class="sxs-lookup"><span data-stu-id="8bec7-242">The **connectionString** uses the storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="8bec7-243">Du kan ange värden för dessa parametrar när mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="8bec7-243">You specify values for these parameters while deploying the template.</span></span>  

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="8bec7-244">HDInsight on-demand linked service (Länkad tjänst för HDInsight på begäran)</span><span class="sxs-lookup"><span data-stu-id="8bec7-244">HDInsight on-demand linked service</span></span>
<span data-ttu-id="8bec7-245">I tjänstdefinitionen på begäran länkad HDInsight kan du ange värden för konfigurationsparametrar som används av tjänsten Data Factory för att skapa ett HDInsight Hadoop-kluster vid körning.</span><span class="sxs-lookup"><span data-stu-id="8bec7-245">In the on-demand HDInsight linked service definition, you specify values for configuration parameters that are used by the Data Factory service to create a HDInsight Hadoop cluster at runtime.</span></span> <span data-ttu-id="8bec7-246">Läs mer i artikeln [Beräkna länkade tjänster](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) om JSON-egenskaper som används för att definiera en länkad tjänst för HDInsight på begäran.</span><span class="sxs-lookup"><span data-stu-id="8bec7-246">See [Compute linked services](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used to define an HDInsight on-demand linked service.</span></span>  

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
<span data-ttu-id="8bec7-247">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="8bec7-247">Note the following points:</span></span>

* <span data-ttu-id="8bec7-248">Datafabriken skapar en **Linux-baserade** HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bec7-248">The Data Factory creates a **Linux-based** HDInsight cluster for you.</span></span>
* <span data-ttu-id="8bec7-249">HDInsight Hadoop-kluster skapas i samma region som lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="8bec7-249">The HDInsight Hadoop cluster is created in the same region as the storage account.</span></span>
* <span data-ttu-id="8bec7-250">Observera den *timeToLive* inställningen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-250">Notice the *timeToLive* setting.</span></span> <span data-ttu-id="8bec7-251">Datafabriken tar bort klustret automatiskt när klustret är att ha varit inaktiv i 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="8bec7-251">The data factory deletes the cluster automatically after the cluster is being idle for 30 minutes.</span></span>
* <span data-ttu-id="8bec7-252">HDInsight-klustret skapar en **standardbehållare** i den blobblagring som du angav i JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="8bec7-252">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (**linkedServiceName**).</span></span> <span data-ttu-id="8bec7-253">HDInsight tar inte bort den här behållaren när klustret tas bort.</span><span class="sxs-lookup"><span data-stu-id="8bec7-253">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="8bec7-254">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="8bec7-254">This behavior is by design.</span></span> <span data-ttu-id="8bec7-255">Med en HDInsight-länkad tjänst på begäran skapas ett HDInsight-kluster varje gång en sektor behöver bearbetas, såvida det inte finns ett befintligt livekluster (**timeToLive**). Det raderas när bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="8bec7-255">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs to be processed unless there is an existing live cluster (**timeToLive**) and is deleted when the processing is done.</span></span>

<span data-ttu-id="8bec7-256">Se [HDInsight-länkad tjänst på begäran](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8bec7-256">See [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8bec7-257">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8bec7-257">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="8bec7-258">Om du inte behöver dem för att felsöka jobb, kan du ta bort dem för att minska lagringskostnaderna.</span><span class="sxs-lookup"><span data-stu-id="8bec7-258">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="8bec7-259">Namnen på de här behållarna följer ett mönster: ”adf**datafabrikensnamn**-**denlänkadetjänstensnamn**-datumtidsstämpel”.</span><span class="sxs-lookup"><span data-stu-id="8bec7-259">The names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="8bec7-260">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) till att ta bort behållare i din Azure Blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="8bec7-260">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="8bec7-261">Indatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="8bec7-261">Azure blob input dataset</span></span>
<span data-ttu-id="8bec7-262">Ange namnen på blob-behållare, mapp och fil som innehåller indata i definitionen för inkommande datamängden.</span><span class="sxs-lookup"><span data-stu-id="8bec7-262">In the input dataset definition, you specify the names of blob container, folder, and file that contains the input data.</span></span> <span data-ttu-id="8bec7-263">Se [Egenskaper för Azure-blobbdatauppsättning](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) om du vill ha information om JSON-egenskaper som används för att definiera en Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="8bec7-263">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span>

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

<span data-ttu-id="8bec7-264">Observera följande specifika inställningar i JSON-definitionen:</span><span class="sxs-lookup"><span data-stu-id="8bec7-264">Notice the following specific settings in the JSON definition:</span></span>

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="8bec7-265">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="8bec7-265">Azure Blob output dataset</span></span>
<span data-ttu-id="8bec7-266">Ange namnen på blob-behållaren och mappen som innehåller utdata i datauppsättningsdefinitionen utdata.</span><span class="sxs-lookup"><span data-stu-id="8bec7-266">In the output dataset definition, you specify the names of blob container and folder that holds the output data.</span></span> <span data-ttu-id="8bec7-267">Se [Egenskaper för Azure-blobbdatauppsättning](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) om du vill ha information om JSON-egenskaper som används för att definiera en Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="8bec7-267">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span>  

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

<span data-ttu-id="8bec7-268">Mappsökvägen anger sökvägen till den mapp som innehåller utdata:</span><span class="sxs-lookup"><span data-stu-id="8bec7-268">The folderPath specifies the path to the folder that holds the output data:</span></span>

```json
"folderPath": "adfgetstarted/partitioneddata",
```

<span data-ttu-id="8bec7-269">Den [dataset tillgänglighet](../data-factory/data-factory-create-datasets.md#dataset-availability) inställningen är följande:</span><span class="sxs-lookup"><span data-stu-id="8bec7-269">The [dataset availability](../data-factory/data-factory-create-datasets.md#dataset-availability) setting is as follows:</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

<span data-ttu-id="8bec7-270">I Azure Data Factory enheter utdata dataset tillgänglighet pipeline.</span><span class="sxs-lookup"><span data-stu-id="8bec7-270">In Azure Data Factory, output dataset availability drives the pipeline.</span></span> <span data-ttu-id="8bec7-271">I det här exemplet producerade sektorn månad på den sista dagen i månaden (EndOfInterval).</span><span class="sxs-lookup"><span data-stu-id="8bec7-271">In this example, the slice is produced monthly on the last day of month (EndOfInterval).</span></span> <span data-ttu-id="8bec7-272">Mer information finns i [Data Factory schemaläggning och körning av](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="8bec7-272">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

#### <a name="data-pipeline"></a><span data-ttu-id="8bec7-273">Datapipeline</span><span class="sxs-lookup"><span data-stu-id="8bec7-273">Data pipeline</span></span>
<span data-ttu-id="8bec7-274">Du kan definiera en pipeline som omvandlar data genom att köra Hive-skript på en Azure HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="8bec7-274">You define a pipeline that transforms data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="8bec7-275">Se [Pipeline-JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) för beskrivningar av JSON-element som används för att definiera en pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="8bec7-275">See [Pipeline JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used to define a pipeline in this example.</span></span>

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

<span data-ttu-id="8bec7-276">Pipelinen innehåller en aktivitet, HDInsightHive aktivitet.</span><span class="sxs-lookup"><span data-stu-id="8bec7-276">The pipeline contains one activity, HDInsightHive activity.</span></span> <span data-ttu-id="8bec7-277">Som både start- och slutdatum är i januari 2016 data för endast en månad (ett segment) har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="8bec7-277">As both start and end dates are in January 2016, data for only one month (a slice) is processed.</span></span> <span data-ttu-id="8bec7-278">Båda *starta* och *end* för aktiviteten har ett tidigare datum, så Data Factory bearbetar data i månad omedelbart.</span><span class="sxs-lookup"><span data-stu-id="8bec7-278">Both *start* and *end* of the activity have a past date, so the Data Factory processes data for the month immediately.</span></span> <span data-ttu-id="8bec7-279">Om slutet är ett datum i framtiden, skapar ett annat segment i datafabriken när dessa.</span><span class="sxs-lookup"><span data-stu-id="8bec7-279">If the end is a future date, the data factory creates another slice when the time comes.</span></span> <span data-ttu-id="8bec7-280">Mer information finns i [Data Factory schemaläggning och körning av](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="8bec7-280">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

## <a name="clean-up-the-tutorial"></a><span data-ttu-id="8bec7-281">Rensa vägledningen</span><span class="sxs-lookup"><span data-stu-id="8bec7-281">Clean up the tutorial</span></span>

### <a name="delete-the-blob-containers-created-by-on-demand-hdinsight-cluster"></a><span data-ttu-id="8bec7-282">Ta bort blob-behållare som skapats av HDInsight-kluster på begäran</span><span class="sxs-lookup"><span data-stu-id="8bec7-282">Delete the blob containers created by on-demand HDInsight cluster</span></span>
<span data-ttu-id="8bec7-283">Med länkad HDInsight-tjänsten på begäran skapas ett HDInsight-kluster varje gång ett segment måste bearbetas såvida det inte finns ett befintligt live kluster (timeToLive) och klustret tas bort när bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="8bec7-283">With on-demand HDInsight linked service, an HDInsight cluster is created every time a slice needs to be processed unless there is an existing live cluster (timeToLive); and the cluster is deleted when the processing is done.</span></span> <span data-ttu-id="8bec7-284">För varje kluster skapar Azure Data Factory blob-behållaren i Azure blob storage som används som standard stroage för klustret.</span><span class="sxs-lookup"><span data-stu-id="8bec7-284">For each cluster, Azure Data Factory creates a blob container in the Azure blob storage used as the default stroage account for the cluster.</span></span> <span data-ttu-id="8bec7-285">Även om HDInsight-kluster har tagits bort, standard blob storage-behållare och det associerade lagringskontot tas inte bort.</span><span class="sxs-lookup"><span data-stu-id="8bec7-285">Even though HDInsight cluster is deleted, the default blob storage container and the associated storage account are not deleted.</span></span> <span data-ttu-id="8bec7-286">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="8bec7-286">This behavior is by design.</span></span> <span data-ttu-id="8bec7-287">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8bec7-287">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="8bec7-288">Om du inte behöver dem för att felsöka jobb, kan du ta bort dem för att minska lagringskostnaderna.</span><span class="sxs-lookup"><span data-stu-id="8bec7-288">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="8bec7-289">Namnen på de här behållarna följer ett mönster: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="8bec7-289">The names of these containers follow a pattern: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span></span>

<span data-ttu-id="8bec7-290">Ta bort den **adfjobs** och **adfyourdatafactoryname-linkedservicename-datetimestamp** mappar.</span><span class="sxs-lookup"><span data-stu-id="8bec7-290">Delete the **adfjobs** and **adfyourdatafactoryname-linkedservicename-datetimestamp** folders.</span></span> <span data-ttu-id="8bec7-291">Behållaren adfjobs innehåller jobbloggar från Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="8bec7-291">The adfjobs container contains job logs from Azure Data Factory.</span></span>

### <a name="delete-the-resource-group"></a><span data-ttu-id="8bec7-292">Ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-292">Delete the resource group</span></span>
<span data-ttu-id="8bec7-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) används för att distribuera, hantera och övervaka din lösning som en grupp.</span><span class="sxs-lookup"><span data-stu-id="8bec7-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) is used to deploy, manage, and monitor your solution as a group.</span></span>  <span data-ttu-id="8bec7-294">Tar bort en resursgrupp alla komponenter inuti gruppen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-294">Deleting a resource group deletes all the components inside the group.</span></span>  

1. <span data-ttu-id="8bec7-295">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8bec7-295">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8bec7-296">Klicka på **resursgrupper** i det vänstra fönstret.</span><span class="sxs-lookup"><span data-stu-id="8bec7-296">Click **Resource groups** on the left pane.</span></span>
3. <span data-ttu-id="8bec7-297">Klicka på resursgruppens namn du skapade i PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="8bec7-297">Click the resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="8bec7-298">Med filtret om du har för många resursgrupper i listan.</span><span class="sxs-lookup"><span data-stu-id="8bec7-298">Use the filter if you have too many resource groups listed.</span></span> <span data-ttu-id="8bec7-299">Resursgruppen öppnas i ett nytt blad.</span><span class="sxs-lookup"><span data-stu-id="8bec7-299">It opens the resource group in a new blade.</span></span>
4. <span data-ttu-id="8bec7-300">På den **resurser** sida vid sida, har du standardkontot för lagring och datafabriken visas om du delar resursgruppen med andra projekt.</span><span class="sxs-lookup"><span data-stu-id="8bec7-300">On the **Resources** tile, you shall have the default storage account and the data factory listed unless you share the resource group with other projects.</span></span>
5. <span data-ttu-id="8bec7-301">Klicka på **ta bort** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="8bec7-301">Click **Delete** on the top of the blade.</span></span> <span data-ttu-id="8bec7-302">Detta tar bort lagringskontot och data som lagras i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="8bec7-302">Doing so deletes the storage account and the data stored in the storage account.</span></span>
6. <span data-ttu-id="8bec7-303">Ange Resursgruppnamnet Bekräfta borttagning och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="8bec7-303">Enter the resource group name to confirm deletion, and then click **Delete**.</span></span>

<span data-ttu-id="8bec7-304">Om du inte vill ta bort lagringskontot när du tar bort resursgruppen, Överväg följande arkitekturen genom att avgränsa affärsdata från standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="8bec7-304">In case you don't want to delete the storage account when you delete the resource group, consider the following architecture by separating the business data from the default storage account.</span></span> <span data-ttu-id="8bec7-305">I det här fallet du har en resursgruppen för lagringskontot med affärsdata och andra resursgruppen för standardkontot för lagring för HDInsight länkade tjänst och datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8bec7-305">In this case, you have one resource group for the storage account with the business data, and the other resource group for the default storage account for HDInsight linked service and the data factory.</span></span> <span data-ttu-id="8bec7-306">När du tar bort andra resursgruppen påverkar inte data storage-konto för företag.</span><span class="sxs-lookup"><span data-stu-id="8bec7-306">When you delete the second resource group, it does not impact the business data storage account.</span></span> <span data-ttu-id="8bec7-307">Gör så här:</span><span class="sxs-lookup"><span data-stu-id="8bec7-307">To do so:</span></span>

* <span data-ttu-id="8bec7-308">Lägg till följande översta resursgruppen tillsammans med Microsoft.DataFactory/datafactories resurs i Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="8bec7-308">Add the following to the top-level resource group along with the Microsoft.DataFactory/datafactories resource in your Resource Manager template.</span></span> <span data-ttu-id="8bec7-309">Den skapar ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="8bec7-309">It creates a storage account:</span></span>

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
* <span data-ttu-id="8bec7-310">Lägg till en ny länkade tjänst i det nya lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="8bec7-310">Add a new linked service point to the new storage account:</span></span>

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
* <span data-ttu-id="8bec7-311">Konfigurera HDInsight ondemand länkad tjänst med en ytterligare dependsOn och en additionalLinkedServiceNames:</span><span class="sxs-lookup"><span data-stu-id="8bec7-311">Configure the HDInsight ondemand linked service with an additional dependsOn and an additionalLinkedServiceNames:</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="8bec7-312">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8bec7-312">Next steps</span></span>
<span data-ttu-id="8bec7-313">I den här artikeln har du lärt dig hur du använder Azure Data Factory för att skapa HDInsight-kluster på begäran för att bearbeta Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="8bec7-313">In this article, you have learned how to use Azure Data Factory to create on-demand HDInsight cluster to process Hive jobs.</span></span> <span data-ttu-id="8bec7-314">Mer information:</span><span class="sxs-lookup"><span data-stu-id="8bec7-314">To read more:</span></span>

* [<span data-ttu-id="8bec7-315">Hadoop-vägledning: komma igång med Linux-baserade Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8bec7-315">Hadoop tutorial: Get started using Linux-based Hadoop in HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="8bec7-316">Skapa Linux-baserade Hadoop-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8bec7-316">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="8bec7-317">HDInsight-dokumentation</span><span class="sxs-lookup"><span data-stu-id="8bec7-317">HDInsight documentation</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="8bec7-318">Data factory-dokumentation</span><span class="sxs-lookup"><span data-stu-id="8bec7-318">Data factory documentation</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a><span data-ttu-id="8bec7-319">Bilaga</span><span class="sxs-lookup"><span data-stu-id="8bec7-319">Appendix</span></span>

### <a name="azure-cli-script"></a><span data-ttu-id="8bec7-320">Azure CLI-skript</span><span class="sxs-lookup"><span data-stu-id="8bec7-320">Azure CLI script</span></span>
<span data-ttu-id="8bec7-321">Du kan använda Azure CLI i stället för att använda Azure PowerShell för att göra kursen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-321">You can use Azure CLI instead of using Azure PowerShell to do the tutorial.</span></span> <span data-ttu-id="8bec7-322">Om du vill använda Azure CLI, installera Azure CLI enligt följande anvisningar:</span><span class="sxs-lookup"><span data-stu-id="8bec7-322">To use Azure CLI, first install Azure CLI as per the following instructions:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-to-prepare-the-storage-and-copy-the-files"></a><span data-ttu-id="8bec7-323">Använda Azure CLI för att förbereda lagring och kopiera filer</span><span class="sxs-lookup"><span data-stu-id="8bec7-323">Use Azure CLI to prepare the storage and copy the files</span></span>

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

<span data-ttu-id="8bec7-324">Behållarens namn är *adfgetstarted*.</span><span class="sxs-lookup"><span data-stu-id="8bec7-324">The container name is *adfgetstarted*.</span></span> <span data-ttu-id="8bec7-325">Se till att den som den är.</span><span class="sxs-lookup"><span data-stu-id="8bec7-325">Keep it as it is.</span></span> <span data-ttu-id="8bec7-326">Annat behöver du uppdatera Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-326">Otherwise you need to update the Resource Manager template.</span></span> <span data-ttu-id="8bec7-327">Om du behöver hjälp med skriptet CLI, se [med hjälp av Azure CLI med Azure Storage](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8bec7-327">If you need help with this CLI script, see [Using the Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>
