---
title: "aaaManage Azure Data Lake Analytics med hjälp av Azure-kommandoradsgränssnittet | Microsoft Docs"
description: "Lär dig hur toomanage Data Lake Analytics-konton, datakällor, jobb och användare som använder Azure CLI"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 0af1f89080739b39f6980989b7694734cc135715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a><span data-ttu-id="e61cc-103">Hantera Azure Data Lake Analytics med hjälp av Azure-kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="e61cc-103">Manage Azure Data Lake Analytics using Azure Command-line Interface (CLI)</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="e61cc-104">Lär dig hur toomanage Azure Data Lake Analytics-konton, datakällor, användare och jobb med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e61cc-104">Learn how toomanage Azure Data Lake Analytics accounts, data sources, users, and jobs using hello Azure CLI.</span></span> <span data-ttu-id="e61cc-105">toosee management information med andra verktyg klickar du på hello flikväljaren ovan.</span><span class="sxs-lookup"><span data-stu-id="e61cc-105">toosee management topics using other tools, click hello tab select above.</span></span>


<span data-ttu-id="e61cc-106">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="e61cc-106">**Prerequisites**</span></span>

<span data-ttu-id="e61cc-107">Innan du påbörjar den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="e61cc-107">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="e61cc-108">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="e61cc-108">**An Azure subscription**.</span></span> <span data-ttu-id="e61cc-109">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e61cc-109">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e61cc-110">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="e61cc-110">**Azure CLI**.</span></span> <span data-ttu-id="e61cc-111">Se [Installera och konfigurera Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="e61cc-111">See [Install and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="e61cc-112">Hämta och installera hello **förhandsversionen** [Azure CLI-verktygen](https://github.com/MicrosoftBigData/AzureDataLake/releases) ordning toocomplete i den här demon.</span><span class="sxs-lookup"><span data-stu-id="e61cc-112">Download and install hello **pre-release** [Azure CLI tools](https://github.com/MicrosoftBigData/AzureDataLake/releases) in order toocomplete this demo.</span></span>
* <span data-ttu-id="e61cc-113">**Autentisering**med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e61cc-113">**Authentication**, using hello following command:</span></span>
  
        azure login
    <span data-ttu-id="e61cc-114">Mer information om autentisering med ett arbets- eller skolkonto konto finns [ansluta tooan Azure-prenumeration från hello Azure CLI](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="e61cc-114">For more information on authenticating using a work or school account, see [Connect tooan Azure subscription from hello Azure CLI](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="e61cc-115">**Växla toohello Azure Resource Manager läge**med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e61cc-115">**Switch toohello Azure Resource Manager mode**, using hello following command:</span></span>
  
        azure config mode arm

<span data-ttu-id="e61cc-116">**toolist hello Data Lake Store och Data Lake Analytics-kommandon:**</span><span class="sxs-lookup"><span data-stu-id="e61cc-116">**toolist hello Data Lake Store and Data Lake Analytics commands:**</span></span>

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a><span data-ttu-id="e61cc-117">Hantera konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-117">Manage accounts</span></span>
<span data-ttu-id="e61cc-118">Du måste ha ett Data Lake Analytics-konto innan du kör alla Data Lake Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="e61cc-118">Before running any Data Lake Analytics jobs, you must have a Data Lake Analytics account.</span></span> <span data-ttu-id="e61cc-119">Till skillnad från Azure HDInsight betalar inte du för Analytics-konto när ett jobb inte körs.</span><span class="sxs-lookup"><span data-stu-id="e61cc-119">Unlike Azure HDInsight, you don't pay for an Analytics account when it is not running a job.</span></span>  <span data-ttu-id="e61cc-120">Du betalar bara för hello tid när den körs ett jobb.</span><span class="sxs-lookup"><span data-stu-id="e61cc-120">You only pay for hello time when it is running a job.</span></span>  <span data-ttu-id="e61cc-121">Mer information finns i [översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e61cc-121">For more information, see [Azure Data Lake Analytics Overview](data-lake-analytics-overview.md).</span></span>  

### <a name="create-accounts"></a><span data-ttu-id="e61cc-122">Skapa konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-122">Create accounts</span></span>
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a><span data-ttu-id="e61cc-123">Uppdatera konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-123">Update accounts</span></span>
<span data-ttu-id="e61cc-124">hello uppdaterar följande kommando hello egenskaper för ett befintligt Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="e61cc-124">hello following command updates hello properties of an existing Data Lake Analytics Account</span></span>

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a><span data-ttu-id="e61cc-125">Lista över konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-125">List accounts</span></span>
<span data-ttu-id="e61cc-126">Lista över Data Lake Analytics-konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-126">List Data Lake Analytics accounts</span></span> 

    azure datalake analytics account list

<span data-ttu-id="e61cc-127">Lista över Data Lake Analytics-konton inom en viss resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e61cc-127">List Data Lake Analytics accounts within a specific resource group</span></span>

    azure datalake analytics account list -g "<Azure Resource Group Name>"

<span data-ttu-id="e61cc-128">Hämta information om ett visst Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="e61cc-128">Get details of a specific Data Lake Analytics account</span></span>

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a><span data-ttu-id="e61cc-129">Ta bort Data Lake Analytics-konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-129">Delete Data Lake Analytics accounts</span></span>
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a><span data-ttu-id="e61cc-130">Hantera datakällor för kontot</span><span class="sxs-lookup"><span data-stu-id="e61cc-130">Manage account data sources</span></span>
<span data-ttu-id="e61cc-131">Data Lake Analytics stöder för närvarande hello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="e61cc-131">Data Lake Analytics currently supports hello following data sources:</span></span>

* [<span data-ttu-id="e61cc-132">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e61cc-132">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="e61cc-133">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e61cc-133">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="e61cc-134">När du skapar ett Analytics-konto, anger du ett Azure Data Lake Storage-konto toobe hello standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="e61cc-134">When you create an Analytics account, you must designate an Azure Data Lake Storage account toobe hello default storage account.</span></span> <span data-ttu-id="e61cc-135">hello ADL standardkontot för lagring används toostore jobbet metadata och jobbet granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="e61cc-135">hello default ADL storage account is used toostore job metadata and job audit logs.</span></span> <span data-ttu-id="e61cc-136">När du har skapat ett Analytics-konto kan du lägga till ytterligare Data Lake-lagringskontona och/eller Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e61cc-136">After you have created an Analytics account, you can add additional Data Lake Storage accounts and/or Azure Storage account.</span></span> 

### <a name="find-hello-default-adl-storage-account"></a><span data-ttu-id="e61cc-137">Hitta hello ADL standardkontot för lagring</span><span class="sxs-lookup"><span data-stu-id="e61cc-137">Find hello default ADL storage account</span></span>
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

<span data-ttu-id="e61cc-138">hello värde visas under egenskaper: datalakeStoreAccount:name.</span><span class="sxs-lookup"><span data-stu-id="e61cc-138">hello value is listed under properties:datalakeStoreAccount:name.</span></span>

### <a name="add-additional-azure-blob-storage-accounts"></a><span data-ttu-id="e61cc-139">Lägg till ytterligare Azure Blob storage-konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-139">Add additional Azure Blob storage accounts</span></span>
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> <span data-ttu-id="e61cc-140">Endast Blob storage kortnamn stöds.</span><span class="sxs-lookup"><span data-stu-id="e61cc-140">Only Blob storage short names are supported.</span></span>  <span data-ttu-id="e61cc-141">Använd inte FQDN, till exempel ”myblob.blob.core.windows.net”.</span><span class="sxs-lookup"><span data-stu-id="e61cc-141">Don't use FQDN, for example "myblob.blob.core.windows.net".</span></span>
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a><span data-ttu-id="e61cc-142">Lägga till ytterligare Data Lake Store-konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-142">Add additional Data Lake Store accounts</span></span>
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

<span data-ttu-id="e61cc-143">[-d] är en valfri växel tooindicate om hello Data Lake som läggs till är hello Data Lake-standardkontot.</span><span class="sxs-lookup"><span data-stu-id="e61cc-143">[-d] is an optional switch tooindicate whether hello Data Lake being added is hello default Data Lake account.</span></span> 

### <a name="update-existing-data-source"></a><span data-ttu-id="e61cc-144">Uppdatera befintlig datakälla</span><span class="sxs-lookup"><span data-stu-id="e61cc-144">Update existing data source</span></span>
<span data-ttu-id="e61cc-145">tooset standard toobe-hello en befintlig Data Lake Store-konto:</span><span class="sxs-lookup"><span data-stu-id="e61cc-145">tooset an existing Data Lake Store account toobe hello default:</span></span>

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

<span data-ttu-id="e61cc-146">tooupdate en befintlig nyckel för Blob storage-konto:</span><span class="sxs-lookup"><span data-stu-id="e61cc-146">tooupdate an existing Blob storage account key:</span></span>

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a><span data-ttu-id="e61cc-147">Lista över datakällor:</span><span class="sxs-lookup"><span data-stu-id="e61cc-147">List data sources:</span></span>
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![Datakälla för data Lake Analytics-lista](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a><span data-ttu-id="e61cc-149">Ta bort datakällor:</span><span class="sxs-lookup"><span data-stu-id="e61cc-149">Delete data sources:</span></span>
<span data-ttu-id="e61cc-150">toodelete ett Data Lake Store-konto:</span><span class="sxs-lookup"><span data-stu-id="e61cc-150">toodelete a Data Lake Store account:</span></span>

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

<span data-ttu-id="e61cc-151">toodelete Blob storage-kontot:</span><span class="sxs-lookup"><span data-stu-id="e61cc-151">toodelete a Blob storage account:</span></span>

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a><span data-ttu-id="e61cc-152">Hantera jobb</span><span class="sxs-lookup"><span data-stu-id="e61cc-152">Manage jobs</span></span>
<span data-ttu-id="e61cc-153">Du måste ha ett Data Lake Analytics-konto innan du kan skapa ett jobb.</span><span class="sxs-lookup"><span data-stu-id="e61cc-153">You must have a Data Lake Analytics account before you can create a job.</span></span>  <span data-ttu-id="e61cc-154">Mer information finns i [hantera Data Lake Analytics-konton](#manage-accounts).</span><span class="sxs-lookup"><span data-stu-id="e61cc-154">For more information, see [Manage Data Lake Analytics accounts](#manage-accounts).</span></span>

### <a name="list-jobs"></a><span data-ttu-id="e61cc-155">Lista över jobb</span><span class="sxs-lookup"><span data-stu-id="e61cc-155">List jobs</span></span>
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Datakälla för data Lake Analytics-lista](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a><span data-ttu-id="e61cc-157">Hämta jobbinformation</span><span class="sxs-lookup"><span data-stu-id="e61cc-157">Get job details</span></span>
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a><span data-ttu-id="e61cc-158">Skicka jobb</span><span class="sxs-lookup"><span data-stu-id="e61cc-158">Submit jobs</span></span>
> [!NOTE]
> <span data-ttu-id="e61cc-159">hello standardprioritet för ett jobb är 1000 och hello standard grad av parallellitet för ett jobb är 1.</span><span class="sxs-lookup"><span data-stu-id="e61cc-159">hello default priority of a job is 1000, and hello default degree of parallelism for a job is 1.</span></span>
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a><span data-ttu-id="e61cc-160">Avbryta jobb</span><span class="sxs-lookup"><span data-stu-id="e61cc-160">Cancel jobs</span></span>
<span data-ttu-id="e61cc-161">Använd hello listan kommandot toofind hello jobb-id och använder sedan Avbryt toocancel hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="e61cc-161">Use hello list command toofind hello job id, and then use cancel toocancel hello job.</span></span>

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a><span data-ttu-id="e61cc-162">Hantera katalog</span><span class="sxs-lookup"><span data-stu-id="e61cc-162">Manage catalog</span></span>
<span data-ttu-id="e61cc-163">hello U-SQL-katalogen är används toostructure data och kod så att de kan delas av U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="e61cc-163">hello U-SQL catalog is used toostructure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="e61cc-164">hello katalog kan hello högsta prestanda möjligt med data i Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="e61cc-164">hello catalog enables hello highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="e61cc-165">Mer information finns i [Använd U-SQL-katalogen](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="e61cc-165">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-catalog-items"></a><span data-ttu-id="e61cc-166">Lista katalogartiklar</span><span class="sxs-lookup"><span data-stu-id="e61cc-166">List catalog items</span></span>
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

<span data-ttu-id="e61cc-167">hello typer är databasen, schemat, sammansättningen, extern datakälla, tabell, Tabellvärdesfunktionen eller tabellstatistik.</span><span class="sxs-lookup"><span data-stu-id="e61cc-167">hello types include database, schema, assembly, external data source, table, table valued function or table statistics.</span></span>

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a><span data-ttu-id="e61cc-168">Använda grupper för ARM</span><span class="sxs-lookup"><span data-stu-id="e61cc-168">Use ARM groups</span></span>
<span data-ttu-id="e61cc-169">Program består vanligtvis av flera komponenter, till exempel en webbapp, databas, databasserver, lagring och tredjepartstjänster.</span><span class="sxs-lookup"><span data-stu-id="e61cc-169">Applications are typically made up of many components, for example a web app, database, database server, storage, and 3rd party services.</span></span> <span data-ttu-id="e61cc-170">Azure Resource Manager (ARM) kan du toowork med hello resurser i ditt program som en grupp, som anges tooas Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e61cc-170">Azure Resource Manager (ARM) enables you toowork with hello resources in your application as a group, referred tooas an Azure Resource Group.</span></span> <span data-ttu-id="e61cc-171">Du kan distribuera, uppdatera, övervaka eller ta bort alla hello resurser i ditt program i en enda, samordnad åtgärd.</span><span class="sxs-lookup"><span data-stu-id="e61cc-171">You can deploy, update, monitor or delete all of hello resources for your application in a single, coordinated operation.</span></span> <span data-ttu-id="e61cc-172">Du använder en mall för distributionen. Mallen kan användas i olika miljöer, till exempel för testning, mellanlagring och produktion.</span><span class="sxs-lookup"><span data-stu-id="e61cc-172">You use a template for deployment and that template can work for different environments such as testing, staging and production.</span></span> <span data-ttu-id="e61cc-173">Du kan tydliggöra fakturering för din organisation genom att visa hello upplyfta kostnader för hello hela gruppen.</span><span class="sxs-lookup"><span data-stu-id="e61cc-173">You can clarify billing for your organization by viewing hello rolled-up costs for hello entire group.</span></span> <span data-ttu-id="e61cc-174">Mer information finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e61cc-174">For more information, see [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span> 

<span data-ttu-id="e61cc-175">Data Lake Analytics-tjänsten kan omfatta hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="e61cc-175">A Data Lake Analytics service can include hello following components:</span></span>

* <span data-ttu-id="e61cc-176">Azure Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="e61cc-176">Azure Data Lake Analytics account</span></span>
* <span data-ttu-id="e61cc-177">Nödvändiga standardkontot för lagring av Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="e61cc-177">Required default Azure Data Lake Storage account</span></span>
* <span data-ttu-id="e61cc-178">Ytterligare Azure Data Lake Storage-konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-178">Additional Azure Data Lake Storage accounts</span></span>
* <span data-ttu-id="e61cc-179">Ytterligare Azure Storage-konton</span><span class="sxs-lookup"><span data-stu-id="e61cc-179">Additional Azure Storage accounts</span></span>

<span data-ttu-id="e61cc-180">Du kan skapa alla dessa komponenter under en ARM grupp toomake dem lättare toomanage.</span><span class="sxs-lookup"><span data-stu-id="e61cc-180">You can create all these components under one ARM group toomake them easier toomanage.</span></span>

![Azure Data Lake Analytics-konto och lagring](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

<span data-ttu-id="e61cc-182">Ett Data Lake Analytics-konto och hello beroende storage-konton måste placeras i hello samma Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="e61cc-182">A Data Lake Analytics account and hello dependent storage accounts must be placed in hello same Azure data center.</span></span>
<span data-ttu-id="e61cc-183">hello ARM-grupp kan dock finnas i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="e61cc-183">hello ARM group however can be located in a different data center.</span></span>  

## <a name="see-also"></a><span data-ttu-id="e61cc-184">Se även</span><span class="sxs-lookup"><span data-stu-id="e61cc-184">See also</span></span>
* [<span data-ttu-id="e61cc-185">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="e61cc-185">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="e61cc-186">Kom igång med Data Lake Analytics med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e61cc-186">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="e61cc-187">Hantera Azure Data Lake Analytics med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e61cc-187">Manage Azure Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-manage-use-portal.md)
* [<span data-ttu-id="e61cc-188">Övervaka och felsök Azure Data Lake Analytics-jobb med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e61cc-188">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure Portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

