---
title: "Kopiera data från Azure Storage-Blobbar till Data Lake Store | Microsoft Docs"
description: "Använd AdlCopy för att kopiera data från Azure Storage-Blobbar till Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 68f44991432a76c2ef1c79ec6dffdea4c62bcb17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a><span data-ttu-id="ca969-103">Kopiera data från Azure Storage-blobar till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ca969-103">Copy data from Azure Storage Blobs to Data Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ca969-104">Använda DistCp</span><span class="sxs-lookup"><span data-stu-id="ca969-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="ca969-105">Använda AdlCopy</span><span class="sxs-lookup"><span data-stu-id="ca969-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="ca969-106">Azure Data Lake Store ger ett kommandoradsverktyg [AdlCopy](http://aka.ms/downloadadlcopy), för att kopiera data från följande källor:</span><span class="sxs-lookup"><span data-stu-id="ca969-106">Azure Data Lake Store provides a command line tool, [AdlCopy](http://aka.ms/downloadadlcopy), to copy data from the following sources:</span></span>

* <span data-ttu-id="ca969-107">Från Azure Storage BLOB till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ca969-107">From Azure Storage Blobs into Data Lake Store.</span></span> <span data-ttu-id="ca969-108">Du kan inte använda AdlCopy för att kopiera data från Data Lake Store till Azure Storage BLOB.</span><span class="sxs-lookup"><span data-stu-id="ca969-108">You cannot use AdlCopy to copy data from Data Lake Store to Azure Storage blobs.</span></span>
* <span data-ttu-id="ca969-109">Mellan två Azure Data Lake Store-konton.</span><span class="sxs-lookup"><span data-stu-id="ca969-109">Between two Azure Data Lake Store accounts.</span></span>

<span data-ttu-id="ca969-110">Du kan också använda verktyget AdlCopy i två olika lägen:</span><span class="sxs-lookup"><span data-stu-id="ca969-110">Also, you can use the AdlCopy tool in two different modes:</span></span>

* <span data-ttu-id="ca969-111">**Fristående**, där verktyget använder Data Lake Store-resurser för att utföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ca969-111">**Standalone**, where the tool uses Data Lake Store resources to perform the task.</span></span>
* <span data-ttu-id="ca969-112">**Med ett Data Lake Analytics-konto**, där enheter som tilldelats till ditt Data Lake Analytics-konto som används för att utföra kopieringen.</span><span class="sxs-lookup"><span data-stu-id="ca969-112">**Using a Data Lake Analytics account**, where the units assigned to your Data Lake Analytics account are used to perform the copy operation.</span></span> <span data-ttu-id="ca969-113">Du kanske vill använda det här alternativet när du behöver utföra uppgifter kopiera förutsägbart.</span><span class="sxs-lookup"><span data-stu-id="ca969-113">You might want to use this option when you are looking to perform the copy tasks in a predictable manner.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca969-114">Krav</span><span class="sxs-lookup"><span data-stu-id="ca969-114">Prerequisites</span></span>
<span data-ttu-id="ca969-115">Innan du påbörjar den här artikeln måste du ha:</span><span class="sxs-lookup"><span data-stu-id="ca969-115">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="ca969-116">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="ca969-116">**An Azure subscription**.</span></span> <span data-ttu-id="ca969-117">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca969-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ca969-118">**Azure Storage-Blobbar** behållare med vissa data.</span><span class="sxs-lookup"><span data-stu-id="ca969-118">**Azure Storage Blobs** container with some data.</span></span>
* <span data-ttu-id="ca969-119">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="ca969-119">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="ca969-120">Anvisningar om hur du skapar en finns [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ca969-120">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="ca969-121">**Azure Data Lake Analytics-kontot (valfritt)** -finns [Kom igång med Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) anvisningar om hur du skapar ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-121">**Azure Data Lake Analytics account (optional)** - See [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) for instructions on how to create a Data Lake Store account.</span></span>
* <span data-ttu-id="ca969-122">**AdlCopy verktyget**.</span><span class="sxs-lookup"><span data-stu-id="ca969-122">**AdlCopy tool**.</span></span> <span data-ttu-id="ca969-123">Installera verktyget AdlCopy från [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span><span class="sxs-lookup"><span data-stu-id="ca969-123">Install the AdlCopy tool from [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span></span>

## <a name="syntax-of-the-adlcopy-tool"></a><span data-ttu-id="ca969-124">Syntaxen för verktyget AdlCopy</span><span class="sxs-lookup"><span data-stu-id="ca969-124">Syntax of the AdlCopy tool</span></span>
<span data-ttu-id="ca969-125">Använd följande syntax för att arbeta med verktyget AdlCopy</span><span class="sxs-lookup"><span data-stu-id="ca969-125">Use the following syntax to work with the AdlCopy tool</span></span>

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

<span data-ttu-id="ca969-126">Parametrarna i syntaxen beskrivs nedan:</span><span class="sxs-lookup"><span data-stu-id="ca969-126">The parameters in the syntax are described below:</span></span>

| <span data-ttu-id="ca969-127">Alternativ</span><span class="sxs-lookup"><span data-stu-id="ca969-127">Option</span></span> | <span data-ttu-id="ca969-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ca969-128">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ca969-129">Källa</span><span class="sxs-lookup"><span data-stu-id="ca969-129">Source</span></span> |<span data-ttu-id="ca969-130">Anger platsen för datakällan i Azure storage blob.</span><span class="sxs-lookup"><span data-stu-id="ca969-130">Specifies the location of the source data in the Azure storage blob.</span></span> <span data-ttu-id="ca969-131">Källan kan vara en blob-behållare, en blobb eller en annan Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-131">The source can be a blob container, a blob, or another Data Lake Store account.</span></span> |
| <span data-ttu-id="ca969-132">Målet</span><span class="sxs-lookup"><span data-stu-id="ca969-132">Dest</span></span> |<span data-ttu-id="ca969-133">Anger målet att kopiera till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ca969-133">Specifies the Data Lake Store destination to copy to.</span></span> |
| <span data-ttu-id="ca969-134">SourceKey</span><span class="sxs-lookup"><span data-stu-id="ca969-134">SourceKey</span></span> |<span data-ttu-id="ca969-135">Anger lagringsåtkomstnyckel för Azure storage blob-källa.</span><span class="sxs-lookup"><span data-stu-id="ca969-135">Specifies the storage access key for the Azure storage blob source.</span></span> <span data-ttu-id="ca969-136">Detta krävs endast om källan är en blob-behållare eller en blob.</span><span class="sxs-lookup"><span data-stu-id="ca969-136">This is required only if the source is a blob container or a blob.</span></span> |
| <span data-ttu-id="ca969-137">Konto</span><span class="sxs-lookup"><span data-stu-id="ca969-137">Account</span></span> |<span data-ttu-id="ca969-138">**Valfritt**.</span><span class="sxs-lookup"><span data-stu-id="ca969-138">**Optional**.</span></span> <span data-ttu-id="ca969-139">Använd det här alternativet om du vill använda Azure Data Lake Analytics-konto för att köra jobbet kopia.</span><span class="sxs-lookup"><span data-stu-id="ca969-139">Use this if you want to use Azure Data Lake Analytics account to run the copy job.</span></span> <span data-ttu-id="ca969-140">Om du använder alternativet /Account syntaxen men inte anger ett Data Lake Analytics-konto, använder AdlCopy ett standardkonto för att köra jobbet.</span><span class="sxs-lookup"><span data-stu-id="ca969-140">If you use the /Account option in the syntax but do not specify a Data Lake Analytics account, AdlCopy uses a default account to run the job.</span></span> <span data-ttu-id="ca969-141">Även om du använder det här alternativet måste du lägga källa (Azure Storage Blob) och mål (Azure Data Lake Store) som datakällor för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="ca969-141">Also, if you use this option, you must add the source (Azure Storage Blob) and destination (Azure Data Lake Store) as data sources for your Data Lake Analytics account.</span></span> |
| <span data-ttu-id="ca969-142">Enheter</span><span class="sxs-lookup"><span data-stu-id="ca969-142">Units</span></span> |<span data-ttu-id="ca969-143">Anger antalet Data Lake Analytics-enheter som ska användas för Kopiera projekt.</span><span class="sxs-lookup"><span data-stu-id="ca969-143">Specifies the number of Data Lake Analytics units that will be used for the copy job.</span></span> <span data-ttu-id="ca969-144">Det här alternativet är obligatoriskt om du använder den **/kontot** alternativet för att ange Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="ca969-144">This option is mandatory if you use the **/Account** option to specify the Data Lake Analytics account.</span></span> |
| <span data-ttu-id="ca969-145">Mönstret</span><span class="sxs-lookup"><span data-stu-id="ca969-145">Pattern</span></span> |<span data-ttu-id="ca969-146">Anger ett regex-mönster som anger vilka blobbar eller filer som ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="ca969-146">Specifies a regex pattern that indicates which blobs or files to copy.</span></span> <span data-ttu-id="ca969-147">AdlCopy använder skiftlägeskänsliga matchar.</span><span class="sxs-lookup"><span data-stu-id="ca969-147">AdlCopy uses case-sensitive matching.</span></span> <span data-ttu-id="ca969-148">Det Standardmönster som används när inga mönster har angetts används för att kopiera alla objekt.</span><span class="sxs-lookup"><span data-stu-id="ca969-148">The default pattern used when no pattern is specified is to copy all items.</span></span> <span data-ttu-id="ca969-149">Ange flera filen mönster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="ca969-149">Specifying multiple file patterns is not supported.</span></span> |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a><span data-ttu-id="ca969-150">Använd AdlCopy (som fristående) för att kopiera data från ett Azure Storage-blob</span><span class="sxs-lookup"><span data-stu-id="ca969-150">Use AdlCopy (as standalone) to copy data from an Azure Storage blob</span></span>
1. <span data-ttu-id="ca969-151">Öppna en kommandotolk och gå till den katalog där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="ca969-151">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="ca969-152">Kör följande kommando för att kopiera en specifik blobb från käll-behållare till ett Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="ca969-152">Run the following command to copy a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="ca969-153">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ca969-153">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] <span data-ttu-id="ca969-154">Ovanstående syntax anger den fil som ska kopieras till en mapp i Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-154">The syntax above specifies the file to be copied to a folder in the Data Lake Store account.</span></span> <span data-ttu-id="ca969-155">AdlCopy verktyget skapar en mapp om namnet på angivna mappen inte finns.</span><span class="sxs-lookup"><span data-stu-id="ca969-155">AdlCopy tool creates a folder if the specified folder name does not exist.</span></span>

    <span data-ttu-id="ca969-156">Du uppmanas att ange autentiseringsuppgifter för Azure-prenumerationen som du har ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-156">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="ca969-157">Du kommer se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="ca969-157">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. <span data-ttu-id="ca969-158">Du kan också kopiera alla blobbar från en behållare till Data Lake Store-konto med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca969-158">You can also copy all the blobs from one container to the Data Lake Store account using the following command:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    <span data-ttu-id="ca969-159">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ca969-159">For example:</span></span>

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a><span data-ttu-id="ca969-160">Saker att tänka på gällande prestanda</span><span class="sxs-lookup"><span data-stu-id="ca969-160">Performance considerations</span></span>

<span data-ttu-id="ca969-161">Om du kopierar från ett Azure Blob Storage-konto kan du begränsas vid kopiering av blob storage-sida.</span><span class="sxs-lookup"><span data-stu-id="ca969-161">If you are copying from an Azure Blob Storage account, you may be throttled during copy on the blob storage side.</span></span> <span data-ttu-id="ca969-162">Detta försämrar prestanda för Kopiera projekt.</span><span class="sxs-lookup"><span data-stu-id="ca969-162">This will degrade the performance of your copy job.</span></span> <span data-ttu-id="ca969-163">Mer information om gränserna för Azure Blob Storage finns Azure Lagringsgränser på [Azure-prenumeration och tjänstbegränsningarna](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="ca969-163">To learn more about the limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a><span data-ttu-id="ca969-164">Använd AdlCopy (som fristående) för att kopiera data från en annan Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="ca969-164">Use AdlCopy (as standalone) to copy data from another Data Lake Store account</span></span>
<span data-ttu-id="ca969-165">Du kan också använda AdlCopy för att kopiera data mellan två Data Lake Store-konton.</span><span class="sxs-lookup"><span data-stu-id="ca969-165">You can also use AdlCopy to copy data between two Data Lake Store accounts.</span></span>

1. <span data-ttu-id="ca969-166">Öppna en kommandotolk och gå till den katalog där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="ca969-166">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="ca969-167">Kör följande kommando för att kopiera en fil från ett Data Lake Store-konto till en annan.</span><span class="sxs-lookup"><span data-stu-id="ca969-167">Run the following command to copy a specific file from one Data Lake Store account to another.</span></span>

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    <span data-ttu-id="ca969-168">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ca969-168">For example:</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > <span data-ttu-id="ca969-169">Ovanstående syntax anger den fil som ska kopieras till en mapp i målet Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-169">The syntax above specifies the file to be copied to a folder in the destination Data Lake Store account.</span></span> <span data-ttu-id="ca969-170">AdlCopy verktyget skapar en mapp om namnet på angivna mappen inte finns.</span><span class="sxs-lookup"><span data-stu-id="ca969-170">AdlCopy tool creates a folder if the specified folder name does not exist.</span></span>
   >
   >

    <span data-ttu-id="ca969-171">Du uppmanas att ange autentiseringsuppgifter för Azure-prenumerationen som du har ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-171">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="ca969-172">Du kommer se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="ca969-172">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. <span data-ttu-id="ca969-173">Följande kommando kopierar alla filer i en mapp i Data Lake Store-konto för källan till en mapp i målet Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-173">The following command copies all files from a specific folder in the source Data Lake Store account to a folder in the destination Data Lake Store account.</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a><span data-ttu-id="ca969-174">Saker att tänka på gällande prestanda</span><span class="sxs-lookup"><span data-stu-id="ca969-174">Performance considerations</span></span>

<span data-ttu-id="ca969-175">När du använder AdlCopy som ett fristående verktyg kopian körs på delade, Azure hanterade resurser.</span><span class="sxs-lookup"><span data-stu-id="ca969-175">When using AdlCopy as a standalone tool, the copy is run on shared, Azure managed resources.</span></span> <span data-ttu-id="ca969-176">Prestanda kan du få i den här miljön är beroende av systembelastning och tillgängliga resurser.</span><span class="sxs-lookup"><span data-stu-id="ca969-176">The performance you may get in this environment depends on system load and available resources.</span></span> <span data-ttu-id="ca969-177">Det här läget är bäst för mindre överföringar på ad hoc-basis.</span><span class="sxs-lookup"><span data-stu-id="ca969-177">This mode is best used for small transfers on an ad hoc basis.</span></span> <span data-ttu-id="ca969-178">Inga parametrar behöver justeras när du använder AdlCopy som ett fristående verktyg.</span><span class="sxs-lookup"><span data-stu-id="ca969-178">No parameters need to be tuned when using AdlCopy as a standalone tool.</span></span>

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a><span data-ttu-id="ca969-179">Använd AdlCopy (med Data Lake Analytics-konto) för att kopiera data</span><span class="sxs-lookup"><span data-stu-id="ca969-179">Use AdlCopy (with Data Lake Analytics account) to copy data</span></span>
<span data-ttu-id="ca969-180">Du kan också använda ditt Data Lake Analytics-konto för att köra jobbet AdlCopy för att kopiera data från Azure storage BLOB till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ca969-180">You can also use your Data Lake Analytics account to run the AdlCopy job to copy data from Azure storage blobs to Data Lake Store.</span></span> <span data-ttu-id="ca969-181">Du använder normalt bara det här alternativet när data flyttas är inom räckhåll gigabyte och terabyte och du vill bättre och förutsägbar prestanda genomflöde.</span><span class="sxs-lookup"><span data-stu-id="ca969-181">You would typically use this option when the data to be moved is in the range of gigabytes and terabytes, and you want better and predictable performance throughput.</span></span>

<span data-ttu-id="ca969-182">Om du vill använda ditt Data Lake Analytics-konto med AdlCopy för att kopiera från en Azure Storage Blob, läggas källan (Azure Storage Blob) till som en datakälla för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="ca969-182">To use your Data Lake Analytics account with AdlCopy to copy from an Azure Storage Blob, the source (Azure Storage Blob) must be added as a data source for your Data Lake Analytics account.</span></span> <span data-ttu-id="ca969-183">Anvisningar för att lägga till ytterligare datakällor i Data Lake Analytics-kontot finns [hantera Data Lake Analytics-kontot datakällor](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span><span class="sxs-lookup"><span data-stu-id="ca969-183">For instructions on adding additional data sources to your Data Lake Analytics account, see [Manage Data Lake Analytics account data sources](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span></span>

> [!NOTE]
> <span data-ttu-id="ca969-184">Om du kopierar från ett Azure Data Lake Store-konto som källa med ett Data Lake Analytics-konto behöver du inte associera Data Lake Store-kontot med Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-184">If you are copying from an Azure Data Lake Store account as the source using a Data Lake Analytics account, you do not need to associate the Data Lake Store account with the Data Lake Analytics account.</span></span> <span data-ttu-id="ca969-185">Kravet på att associera arkivet källa med Data Lake Analytics-kontot är endast om källan är ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ca969-185">The requirement to associate the source store with the Data Lake Analytics account is only when the source is an Azure Storage account.</span></span>
>
>

<span data-ttu-id="ca969-186">Kör följande kommando för att kopiera från en Azure Storage blob till ett Data Lake Store-konto med Data Lake Analytics-konto:</span><span class="sxs-lookup"><span data-stu-id="ca969-186">Run the following command to copy from an Azure Storage blob to a Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

<span data-ttu-id="ca969-187">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ca969-187">For example:</span></span>

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

<span data-ttu-id="ca969-188">På liknande sätt, kör följande kommando för att kopiera från en Azure Storage blob till ett Data Lake Store-konto med Data Lake Analytics-konto:</span><span class="sxs-lookup"><span data-stu-id="ca969-188">Similarly, run the following command to copy from an Azure Storage blob to a Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a><span data-ttu-id="ca969-189">Saker att tänka på gällande prestanda</span><span class="sxs-lookup"><span data-stu-id="ca969-189">Performance considerations</span></span>

<span data-ttu-id="ca969-190">När du kopierar data i intervallet terabyte ger med din egen Azure Data Lake Analytics-konto AdlCopy bättre och mer förutsägbar prestanda.</span><span class="sxs-lookup"><span data-stu-id="ca969-190">When copying data in the range of terabytes, using AdlCopy with your own Azure Data Lake Analytics account provides better and more predictable performance.</span></span> <span data-ttu-id="ca969-191">Parametern som ska justeras är antalet Azure Data Lake Analytics-enheter ska användas för Kopiera projekt.</span><span class="sxs-lookup"><span data-stu-id="ca969-191">The parameter that should be tuned is the number of Azure Data Lake Analytics Units to use for the copy job.</span></span> <span data-ttu-id="ca969-192">Öka antalet enheter kommer att öka prestanda för Kopiera projekt.</span><span class="sxs-lookup"><span data-stu-id="ca969-192">Increasing the number of units will increase the performance of your copy job.</span></span> <span data-ttu-id="ca969-193">Varje fil som ska kopieras kan använda maximalt en enhet.</span><span class="sxs-lookup"><span data-stu-id="ca969-193">Each file to be copied can use maximum one unit.</span></span> <span data-ttu-id="ca969-194">Ange fler enheter än antalet filer som kopieras kommer inte att öka prestanda.</span><span class="sxs-lookup"><span data-stu-id="ca969-194">Specifying more units than the number of files being copied will not increase performance.</span></span>

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a><span data-ttu-id="ca969-195">Använd AdlCopy för att kopiera data med hjälp av mönstermatchning</span><span class="sxs-lookup"><span data-stu-id="ca969-195">Use AdlCopy to copy data using pattern matching</span></span>
<span data-ttu-id="ca969-196">I det här avsnittet kan du lära dig hur du använder AdlCopy för att kopiera data från en källa (i vårt exempel nedan använder vi Azure Storage Blob) till ett mål Data Lake Store-konto med hjälp av mönstermatchning.</span><span class="sxs-lookup"><span data-stu-id="ca969-196">In this section, you learn how to use AdlCopy to copy data from a source (in our example below we use Azure Storage Blob) to a destination Data Lake Store account using pattern matching.</span></span> <span data-ttu-id="ca969-197">Du kan till exempel använda instruktionerna nedan för att kopiera alla filer med filnamnstillägget .csv från blob för källan till målet.</span><span class="sxs-lookup"><span data-stu-id="ca969-197">For example, you can use the steps below to copy all files with .csv extension from the source blob to the destination.</span></span>

1. <span data-ttu-id="ca969-198">Öppna en kommandotolk och gå till den katalog där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="ca969-198">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="ca969-199">Kör följande kommando för att kopiera alla filer med filnamnstillägget *.csv från en specifik blobb från käll-behållare till ett Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="ca969-199">Run the following command to copy all files with *.csv extension from a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    <span data-ttu-id="ca969-200">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ca969-200">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a><span data-ttu-id="ca969-201">Fakturering</span><span class="sxs-lookup"><span data-stu-id="ca969-201">Billing</span></span>
* <span data-ttu-id="ca969-202">Om du använder verktyget AdlCopy som fristående kommer du att debiteras för kostnader för nätverksegress för att flytta data, om källan Azure Storage-konto inte är i samma region som Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ca969-202">If you use the AdlCopy tool as standalone you will be billed for egress costs for moving data, if the source Azure Storage account is not in the same region as the Data Lake Store.</span></span>
* <span data-ttu-id="ca969-203">Om du använder verktyget AdlCopy med Data Lake Analytics-kontot, standard [Data Lake Analytics fakturering priser](https://azure.microsoft.com/pricing/details/data-lake-analytics/) gäller.</span><span class="sxs-lookup"><span data-stu-id="ca969-203">If you use the AdlCopy tool with your Data Lake Analytics account, standard [Data Lake Analytics billing rates](https://azure.microsoft.com/pricing/details/data-lake-analytics/) will apply.</span></span>

## <a name="considerations-for-using-adlcopy"></a><span data-ttu-id="ca969-204">Överväganden för att använda AdlCopy</span><span class="sxs-lookup"><span data-stu-id="ca969-204">Considerations for using AdlCopy</span></span>
* <span data-ttu-id="ca969-205">AdlCopy (för version 1.0.5), har stöd för kopiering av data från källor som gemensamt har mer än tusentals filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="ca969-205">AdlCopy (for version 1.0.5), supports copying data from sources that collectively have more than thousands of files and folders.</span></span> <span data-ttu-id="ca969-206">Om det uppstår problem kopiera en stor datauppsättning kan du dock distribuera filer/mappar till olika undermappar och Använd sökvägen till de underordnade mapparna som källa i stället.</span><span class="sxs-lookup"><span data-stu-id="ca969-206">However, if you encounter issues copying a large dataset, you can distribute the files/folders into different sub-folders and use the path to those sub-folders as the source instead.</span></span>

## <a name="performance-considerations-for-using-adlcopy"></a><span data-ttu-id="ca969-207">Prestandaöverväganden för att använda AdlCopy</span><span class="sxs-lookup"><span data-stu-id="ca969-207">Performance considerations for using AdlCopy</span></span>

<span data-ttu-id="ca969-208">AdlCopy har stöd för kopiering av data som innehåller tusentals filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="ca969-208">AdlCopy supports copying data containing thousands of files and folders.</span></span> <span data-ttu-id="ca969-209">Om du får problem med kopiera en stor datauppsättning kan du distribuera filer/mappar till mindre underordnade mappar.</span><span class="sxs-lookup"><span data-stu-id="ca969-209">However, if you encounter issues copying a large dataset, you can distribute the files/folders into smaller sub-folders.</span></span> <span data-ttu-id="ca969-210">AdlCopy skapades för ad hoc-kopior.</span><span class="sxs-lookup"><span data-stu-id="ca969-210">AdlCopy was built for ad hoc copies.</span></span> <span data-ttu-id="ca969-211">Om du vill kopiera data regelbundet, bör du använda [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) som ger fullständig hantering runt åtgärderna kopia.</span><span class="sxs-lookup"><span data-stu-id="ca969-211">If you are trying to copy data on a recurring basis, you should consider using [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) that provides full management around the copy operations.</span></span>

## <a name="release-notes"></a><span data-ttu-id="ca969-212">Viktig information</span><span class="sxs-lookup"><span data-stu-id="ca969-212">Release notes</span></span>
* <span data-ttu-id="ca969-213">1.0.13 - om du kopierar data till samma Azure Data Lake Store-konto över flera adlcopy kommandon du behöver inte ange dina autentiseringsuppgifter för varje kör längre.</span><span class="sxs-lookup"><span data-stu-id="ca969-213">1.0.13 - If you are copying data to the same Azure Data Lake Store account across multiple adlcopy commands, you do not need to reenter your credentials for each run anymore.</span></span> <span data-ttu-id="ca969-214">Adlcopy nu cachelagras informationen över flera körs.</span><span class="sxs-lookup"><span data-stu-id="ca969-214">Adlcopy will now cache that information across multiple runs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca969-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca969-215">Next steps</span></span>
* [<span data-ttu-id="ca969-216">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ca969-216">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="ca969-217">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ca969-217">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="ca969-218">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ca969-218">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
