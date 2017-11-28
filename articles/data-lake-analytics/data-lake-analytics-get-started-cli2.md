---
title: "aaaGet igång med Azure Data Lake Analytics med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toouse hello Azure kommandoradsgränssnittet 2.0 toocreate ett Data Lake Analytics-konto, skapa ett Data Lake Analytics-jobb med hjälp av U-SQL och skicka hello-jobbet. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: jgao
ms.openlocfilehash: c4e91c0d3526e4932c2948c0a326d4cedc985791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a><span data-ttu-id="ef3a8-103">Kom igång med Azure Data Lake Analytics med hjälp av Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ef3a8-103">Get started with Azure Data Lake Analytics using Azure CLI 2.0</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="ef3a8-104">I de här självstudierna utvecklar du ett jobb som läser en fil med tabbavgränsade värden (TVS) och konverterar den till en fil med kommaavgränsade värden (CSV).</span><span class="sxs-lookup"><span data-stu-id="ef3a8-104">In this tutorial, you develop a job that reads a tab separated values (TSV) file and converts it into a comma-separated values (CSV) file.</span></span> <span data-ttu-id="ef3a8-105">toogo via hello stöds samma självstudier med andra verktyg, Använd hello listrutan hello längst upp i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-105">toogo through hello same tutorial using other supported tools, use hello dropdown list on hello top of this section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef3a8-106">Krav</span><span class="sxs-lookup"><span data-stu-id="ef3a8-106">Prerequisites</span></span>
<span data-ttu-id="ef3a8-107">Innan du påbörjar den här självstudien måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-107">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="ef3a8-108">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-108">**An Azure subscription**.</span></span> <span data-ttu-id="ef3a8-109">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ef3a8-109">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ef3a8-110">**Azure CLI 2.0**.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-110">**Azure CLI 2.0**.</span></span> <span data-ttu-id="ef3a8-111">Se [Installera och konfigurera Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ef3a8-111">See [Install and configure Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="ef3a8-112">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="ef3a8-112">Log in tooAzure</span></span>

<span data-ttu-id="ef3a8-113">toolog i tooyour Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-113">toolog in tooyour Azure subscription:</span></span>

```
azurecli
az login
```

<span data-ttu-id="ef3a8-114">Du har begärt toobrowse tooa URL och ange en Autentiseringskod.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-114">You are requested toobrowse tooa URL, and enter an authentication code.</span></span>  <span data-ttu-id="ef3a8-115">Och följ hello instruktioner tooenter dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-115">And then follow hello instructions tooenter your credentials.</span></span>

<span data-ttu-id="ef3a8-116">När du har loggat in visar hello inloggningen kommando dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-116">Once you have logged in, hello login command lists your subscriptions.</span></span>

<span data-ttu-id="ef3a8-117">toouse en viss prenumeration:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-117">toouse a specific subscription:</span></span>

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="ef3a8-118">Skapa ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="ef3a8-118">Create Data Lake Analytics account</span></span>
<span data-ttu-id="ef3a8-119">Du måste ha ett Data Lake Analytics-konto innan du kan köra några jobb.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-119">You need a Data Lake Analytics account before you can run any jobs.</span></span> <span data-ttu-id="ef3a8-120">toocreate ett Data Lake Analytics-konto, måste du ange hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-120">toocreate a Data Lake Analytics account, you must specify hello following items:</span></span>

* <span data-ttu-id="ef3a8-121">**Azure-resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-121">**Azure Resource Group**.</span></span> <span data-ttu-id="ef3a8-122">Ett Data Lake Analytics-konto måste skapas i en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-122">A Data Lake Analytics account must be created within an Azure Resource group.</span></span> <span data-ttu-id="ef3a8-123">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kan du toowork med hello resurser i ditt program som en grupp.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-123">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) enables you toowork with hello resources in your application as a group.</span></span> <span data-ttu-id="ef3a8-124">Du kan distribuera, uppdatera eller ta bort alla hello resurser i ditt program i en enda, samordnad åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-124">You can deploy, update, or delete all of hello resources for your application in a single, coordinated operation.</span></span>  

<span data-ttu-id="ef3a8-125">toolist hello befintliga resursgrupper i din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-125">toolist hello existing resource groups under your subscription:</span></span>

```
az group list
```

<span data-ttu-id="ef3a8-126">toocreate en ny resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-126">toocreate a new resource group:</span></span>

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* <span data-ttu-id="ef3a8-127">**Data Lake Analytics-kontonamn**.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-127">**Data Lake Analytics account name**.</span></span> <span data-ttu-id="ef3a8-128">Varje Data Lake Analytics-konto har ett namn.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-128">Each Data Lake Analytics account has a name.</span></span>
* <span data-ttu-id="ef3a8-129">**Plats**.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-129">**Location**.</span></span> <span data-ttu-id="ef3a8-130">Använd någon av hello Azure-datacenter som stöder Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-130">Use one of hello Azure data centers that supports Data Lake Analytics.</span></span>
* <span data-ttu-id="ef3a8-131">**Data Lake Store-standardkonto**: Varje Data Lake Analytics-konto har ett Data Lake Store-standardkonto.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-131">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span>

<span data-ttu-id="ef3a8-132">toolist hello befintliga Data Lake Store-konto:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-132">toolist hello existing Data Lake Store account:</span></span>

```
az dls account list
```

<span data-ttu-id="ef3a8-133">toocreate ett nytt Data Lake Store-konto:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-133">toocreate a new Data Lake Store account:</span></span>

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

<span data-ttu-id="ef3a8-134">Använd följande syntax toocreate ett Data Lake Analytics-konto hello:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-134">Use hello following syntax toocreate a Data Lake Analytics account:</span></span>

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

<span data-ttu-id="ef3a8-135">Du kan använda följande kommandon toolist hello konton hello och visa kontoinformation när du har skapat ett konto:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-135">After creating an account, you can use hello following commands toolist hello accounts and show account details:</span></span>

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-toodata-lake-store"></a><span data-ttu-id="ef3a8-136">Överför tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="ef3a8-136">Upload data tooData Lake Store</span></span>
<span data-ttu-id="ef3a8-137">I den här självstudien bearbetar du vissa sökloggar.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-137">In this tutorial, you process some search logs.</span></span>  <span data-ttu-id="ef3a8-138">Hej sökloggen kan lagras i Data Lake store eller Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-138">hello search log can be stored in either Data Lake store or Azure Blob storage.</span></span>

<span data-ttu-id="ef3a8-139">hello Azure-portalen innehåller ett användargränssnitt för att kopiera vissa exempel data filer toohello Data Lake Store standardkontot, bland annat en sökloggfil.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-139">hello Azure portal provides a user interface for copying some sample data files toohello default Data Lake Store account, which include a search log file.</span></span> <span data-ttu-id="ef3a8-140">Se [förbereda källdata](data-lake-analytics-get-started-portal.md) tooupload hello data toohello Data Lake Store-standardkontot.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-140">See [Prepare source data](data-lake-analytics-get-started-portal.md) tooupload hello data toohello default Data Lake Store account.</span></span>

<span data-ttu-id="ef3a8-141">tooupload filer med hjälp av CLI 2.0, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-141">tooupload files using CLI 2.0, use hello following commands:</span></span>

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

<span data-ttu-id="ef3a8-142">Data Lake Analytics kan också använda Azure Blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-142">Data Lake Analytics can also access Azure Blob storage.</span></span>  <span data-ttu-id="ef3a8-143">Ladda upp data tooAzure Blob storage finns [Using hello Azure CLI med Azure Storage](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ef3a8-143">For uploading data tooAzure Blob storage, see [Using hello Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>

## <a name="submit-data-lake-analytics-jobs"></a><span data-ttu-id="ef3a8-144">Skicka Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="ef3a8-144">Submit Data Lake Analytics jobs</span></span>
<span data-ttu-id="ef3a8-145">hello Data Lake Analytics-jobb skrivs på hello U-SQL-språket.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-145">hello Data Lake Analytics jobs are written in hello U-SQL language.</span></span> <span data-ttu-id="ef3a8-146">toolearn mer om U-SQL finns [Kom igång med U-SQL-språket](data-lake-analytics-u-sql-get-started.md) och [U-SQL-språket eence](http://go.microsoft.com/fwlink/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="ef3a8-146">toolearn more about U-SQL, see [Get started with U-SQL language](data-lake-analytics-u-sql-get-started.md) and [U-SQL language eence](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>

<span data-ttu-id="ef3a8-147">**toocreate Data Lake Analytics-jobbskript**</span><span class="sxs-lookup"><span data-stu-id="ef3a8-147">**toocreate a Data Lake Analytics job script**</span></span>

<span data-ttu-id="ef3a8-148">Skapa en textfil med följande U-SQL-skript och spara hello text filen tooyour arbetsstation:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-148">Create a text file with following U-SQL script, and save hello text file tooyour workstation:</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

<span data-ttu-id="ef3a8-149">U-SQL-skriptet läser hello källa data filen med hjälp av **Extractors.Tsv()**, och skapar sedan en CSV-fil med hjälp av **Outputters.Csv()**.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-149">This U-SQL script reads hello source data file using **Extractors.Tsv()**, and then creates a csv file using **Outputters.Csv()**.</span></span>

<span data-ttu-id="ef3a8-150">Ändra inte hello två sökvägarna om du kopierar hello källfilen till en annan plats.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-150">Don't modify hello two paths unless you copy hello source file into a different location.</span></span>  <span data-ttu-id="ef3a8-151">Data Lake Analytics skapar Utdatamappen hello om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-151">Data Lake Analytics creates hello output folder if it doesn't exist.</span></span>

<span data-ttu-id="ef3a8-152">Det är enklare toouse relativa sökvägar för filer lagrade i Data Lake Store standardkonton.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-152">It is simpler toouse relative paths for files stored in default Data Lake Store accounts.</span></span> <span data-ttu-id="ef3a8-153">Du kan också använda absoluta sökvägar.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-153">You can also use absolute paths.</span></span>  <span data-ttu-id="ef3a8-154">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-154">For example:</span></span>

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

<span data-ttu-id="ef3a8-155">Du måste använda absoluta sökvägar tooaccess filer i länkade Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-155">You must use absolute paths tooaccess files in linked Storage accounts.</span></span>  <span data-ttu-id="ef3a8-156">hello syntaxen för filer som lagras i länkade Azure Storage-konto är:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-156">hello syntax for files stored in linked Azure Storage account is:</span></span>

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> <span data-ttu-id="ef3a8-157">Azure Blob-behållare med offentliga blobbar stöds inte.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-157">Azure Blob container with public blobs are not supported.</span></span>      
> <span data-ttu-id="ef3a8-158">Azure Blob-behållare med offentliga behållare stöds inte.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-158">Azure Blob container with public containers are not supported.</span></span>      
>

<span data-ttu-id="ef3a8-159">**toosubmit jobb**</span><span class="sxs-lookup"><span data-stu-id="ef3a8-159">**toosubmit jobs**</span></span>

<span data-ttu-id="ef3a8-160">Använd följande syntax toosubmit ett jobb hello.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-160">Use hello following syntax toosubmit a job.</span></span>

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

<span data-ttu-id="ef3a8-161">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-161">For example:</span></span>

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

<span data-ttu-id="ef3a8-162">**toolist jobb och visa jobbinformation**</span><span class="sxs-lookup"><span data-stu-id="ef3a8-162">**toolist jobs and show job details**</span></span>

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

<span data-ttu-id="ef3a8-163">**toocancel jobb**</span><span class="sxs-lookup"><span data-stu-id="ef3a8-163">**toocancel jobs**</span></span>

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a><span data-ttu-id="ef3a8-164">Hämta jobbresultat</span><span class="sxs-lookup"><span data-stu-id="ef3a8-164">Retrieve job results</span></span>

<span data-ttu-id="ef3a8-165">När ett jobb har slutförts kan du använda hello följande kommandon toolist hello utgående filer och hämta hello filer:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-165">After a job is completed, you can use hello following commands toolist hello output files, and download hello files:</span></span>

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

<span data-ttu-id="ef3a8-166">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ef3a8-166">For example:</span></span>

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="pipelines-and-recurrences"></a><span data-ttu-id="ef3a8-167">Pipelines och upprepningar</span><span class="sxs-lookup"><span data-stu-id="ef3a8-167">Pipelines and recurrences</span></span>

<span data-ttu-id="ef3a8-168">**Hämta information om pipelines och upprepningar**</span><span class="sxs-lookup"><span data-stu-id="ef3a8-168">**Get information about pipelines and recurrences**</span></span>

<span data-ttu-id="ef3a8-169">Använd hello `az dla job pipeline` kommandon toosee hello pipeline-information skickats tidigare jobb.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-169">Use hello `az dla job pipeline` commands toosee hello pipeline information previously submitted jobs.</span></span>

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

<span data-ttu-id="ef3a8-170">Använd hello `az dla job recurrence` kommandon toosee hello återkommande information för tidigare skickade jobb.</span><span class="sxs-lookup"><span data-stu-id="ef3a8-170">Use hello `az dla job recurrence` commands toosee hello recurrence information for previously submitted jobs.</span></span>

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a><span data-ttu-id="ef3a8-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef3a8-171">Next steps</span></span>

* <span data-ttu-id="ef3a8-172">toosee hello Data Lake Analytics CLI 2.0 Referensdokumentet finns [Datasjöanalys](https://docs.microsoft.com/cli/azure/dla).</span><span class="sxs-lookup"><span data-stu-id="ef3a8-172">toosee hello Data Lake Analytics CLI 2.0 reference document, see [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).</span></span>
* <span data-ttu-id="ef3a8-173">toosee hello Data Lake Store CLI 2.0 Referensdokumentet finns [Datasjölager](https://docs.microsoft.com/cli/azure/dls).</span><span class="sxs-lookup"><span data-stu-id="ef3a8-173">toosee hello Data Lake Store CLI 2.0 reference document, see [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).</span></span>
* <span data-ttu-id="ef3a8-174">toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="ef3a8-174">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
