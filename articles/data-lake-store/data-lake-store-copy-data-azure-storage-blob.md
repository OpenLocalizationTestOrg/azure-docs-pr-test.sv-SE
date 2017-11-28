---
title: "aaaCopy data från Azure Storage-Blobbar i Data Lake Store | Microsoft Docs"
description: "Använda AdlCopy verktyget toocopy data från Azure Storage BLOB tooData Datasjölager"
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
ms.openlocfilehash: a3d4172eaefe7395cdef2fff72691bd70f642b78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-from-azure-storage-blobs-toodata-lake-store"></a><span data-ttu-id="674f0-103">Kopiera data från Azure Storage BLOB tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="674f0-103">Copy data from Azure Storage Blobs tooData Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="674f0-104">Använda DistCp</span><span class="sxs-lookup"><span data-stu-id="674f0-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="674f0-105">Använda AdlCopy</span><span class="sxs-lookup"><span data-stu-id="674f0-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="674f0-106">Azure Data Lake Store ger ett kommandoradsverktyg [AdlCopy](http://aka.ms/downloadadlcopy), toocopy data från hello följande källor:</span><span class="sxs-lookup"><span data-stu-id="674f0-106">Azure Data Lake Store provides a command line tool, [AdlCopy](http://aka.ms/downloadadlcopy), toocopy data from hello following sources:</span></span>

* <span data-ttu-id="674f0-107">Från Azure Storage BLOB till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="674f0-107">From Azure Storage Blobs into Data Lake Store.</span></span> <span data-ttu-id="674f0-108">Du kan inte använda AdlCopy toocopy data från Data Lake Store tooAzure Storage-blobbar.</span><span class="sxs-lookup"><span data-stu-id="674f0-108">You cannot use AdlCopy toocopy data from Data Lake Store tooAzure Storage blobs.</span></span>
* <span data-ttu-id="674f0-109">Mellan två Azure Data Lake Store-konton.</span><span class="sxs-lookup"><span data-stu-id="674f0-109">Between two Azure Data Lake Store accounts.</span></span>

<span data-ttu-id="674f0-110">Du kan också använda verktyget för hello AdlCopy i två olika lägen:</span><span class="sxs-lookup"><span data-stu-id="674f0-110">Also, you can use hello AdlCopy tool in two different modes:</span></span>

* <span data-ttu-id="674f0-111">**Fristående**, där hello verktyget använder Data Lake Store resurser tooperform hello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="674f0-111">**Standalone**, where hello tool uses Data Lake Store resources tooperform hello task.</span></span>
* <span data-ttu-id="674f0-112">**Med ett Data Lake Analytics-konto**, där hello-enheter som tilldelats tooyour Data Lake Analytics-konto är används tooperform hello kopieringen.</span><span class="sxs-lookup"><span data-stu-id="674f0-112">**Using a Data Lake Analytics account**, where hello units assigned tooyour Data Lake Analytics account are used tooperform hello copy operation.</span></span> <span data-ttu-id="674f0-113">Du kanske vill toouse det här alternativet när du söker tooperform hello kopiera aktiviteter förutsägbart.</span><span class="sxs-lookup"><span data-stu-id="674f0-113">You might want toouse this option when you are looking tooperform hello copy tasks in a predictable manner.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="674f0-114">Krav</span><span class="sxs-lookup"><span data-stu-id="674f0-114">Prerequisites</span></span>
<span data-ttu-id="674f0-115">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="674f0-115">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="674f0-116">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="674f0-116">**An Azure subscription**.</span></span> <span data-ttu-id="674f0-117">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="674f0-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="674f0-118">**Azure Storage-Blobbar** behållare med vissa data.</span><span class="sxs-lookup"><span data-stu-id="674f0-118">**Azure Storage Blobs** container with some data.</span></span>
* <span data-ttu-id="674f0-119">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="674f0-119">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="674f0-120">Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="674f0-120">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="674f0-121">**Azure Data Lake Analytics-kontot (valfritt)** -finns [Kom igång med Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) anvisningar för hur toocreate ett Data Lake lagrar konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-121">**Azure Data Lake Analytics account (optional)** - See [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) for instructions on how toocreate a Data Lake Store account.</span></span>
* <span data-ttu-id="674f0-122">**AdlCopy verktyget**.</span><span class="sxs-lookup"><span data-stu-id="674f0-122">**AdlCopy tool**.</span></span> <span data-ttu-id="674f0-123">Installera verktyget för hello AdlCopy från [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span><span class="sxs-lookup"><span data-stu-id="674f0-123">Install hello AdlCopy tool from [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span></span>

## <a name="syntax-of-hello-adlcopy-tool"></a><span data-ttu-id="674f0-124">Syntaxen för verktyget för hello AdlCopy</span><span class="sxs-lookup"><span data-stu-id="674f0-124">Syntax of hello AdlCopy tool</span></span>
<span data-ttu-id="674f0-125">Använd följande syntax toowork med hello AdlCopy verktyget hello</span><span class="sxs-lookup"><span data-stu-id="674f0-125">Use hello following syntax toowork with hello AdlCopy tool</span></span>

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

<span data-ttu-id="674f0-126">hello parametrar i hello syntaxen beskrivs nedan:</span><span class="sxs-lookup"><span data-stu-id="674f0-126">hello parameters in hello syntax are described below:</span></span>

| <span data-ttu-id="674f0-127">Alternativ</span><span class="sxs-lookup"><span data-stu-id="674f0-127">Option</span></span> | <span data-ttu-id="674f0-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="674f0-128">Description</span></span> |
| --- | --- |
| <span data-ttu-id="674f0-129">Källa</span><span class="sxs-lookup"><span data-stu-id="674f0-129">Source</span></span> |<span data-ttu-id="674f0-130">Anger hello plats hello källdata i hello Azure storage blob.</span><span class="sxs-lookup"><span data-stu-id="674f0-130">Specifies hello location of hello source data in hello Azure storage blob.</span></span> <span data-ttu-id="674f0-131">hello källan kan vara en blob-behållare, en blobb eller en annan Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-131">hello source can be a blob container, a blob, or another Data Lake Store account.</span></span> |
| <span data-ttu-id="674f0-132">Målet</span><span class="sxs-lookup"><span data-stu-id="674f0-132">Dest</span></span> |<span data-ttu-id="674f0-133">Anger hello Data Lake Store mål toocopy till.</span><span class="sxs-lookup"><span data-stu-id="674f0-133">Specifies hello Data Lake Store destination toocopy to.</span></span> |
| <span data-ttu-id="674f0-134">SourceKey</span><span class="sxs-lookup"><span data-stu-id="674f0-134">SourceKey</span></span> |<span data-ttu-id="674f0-135">Anger hello lagringsåtkomstnyckel för hello Azure storage blob källa.</span><span class="sxs-lookup"><span data-stu-id="674f0-135">Specifies hello storage access key for hello Azure storage blob source.</span></span> <span data-ttu-id="674f0-136">Detta krävs endast om hello källan är en blob-behållare eller en blob.</span><span class="sxs-lookup"><span data-stu-id="674f0-136">This is required only if hello source is a blob container or a blob.</span></span> |
| <span data-ttu-id="674f0-137">Konto</span><span class="sxs-lookup"><span data-stu-id="674f0-137">Account</span></span> |<span data-ttu-id="674f0-138">**Valfritt**.</span><span class="sxs-lookup"><span data-stu-id="674f0-138">**Optional**.</span></span> <span data-ttu-id="674f0-139">Använd det här alternativet om du vill toouse Azure Data Lake Analytics-konto toorun hello kopieringsjobbet.</span><span class="sxs-lookup"><span data-stu-id="674f0-139">Use this if you want toouse Azure Data Lake Analytics account toorun hello copy job.</span></span> <span data-ttu-id="674f0-140">Om du använder hello /Account alternativet i hello syntax men inte anger ett Data Lake Analytics-konto, använder AdlCopy ett standard konto toorun hello jobb.</span><span class="sxs-lookup"><span data-stu-id="674f0-140">If you use hello /Account option in hello syntax but do not specify a Data Lake Analytics account, AdlCopy uses a default account toorun hello job.</span></span> <span data-ttu-id="674f0-141">Även om du använder det här alternativet måste du lägga hello källa (Azure Storage Blob) och mål (Azure Data Lake Store) som datakällor för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="674f0-141">Also, if you use this option, you must add hello source (Azure Storage Blob) and destination (Azure Data Lake Store) as data sources for your Data Lake Analytics account.</span></span> |
| <span data-ttu-id="674f0-142">Enheter</span><span class="sxs-lookup"><span data-stu-id="674f0-142">Units</span></span> |<span data-ttu-id="674f0-143">Anger hello antalet Data Lake Analytics-enheter som ska användas för hello kopieringsjobbet.</span><span class="sxs-lookup"><span data-stu-id="674f0-143">Specifies hello number of Data Lake Analytics units that will be used for hello copy job.</span></span> <span data-ttu-id="674f0-144">Det här alternativet är obligatoriskt om du använder hello **/kontot** alternativet toospecify hello Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-144">This option is mandatory if you use hello **/Account** option toospecify hello Data Lake Analytics account.</span></span> |
| <span data-ttu-id="674f0-145">Mönstret</span><span class="sxs-lookup"><span data-stu-id="674f0-145">Pattern</span></span> |<span data-ttu-id="674f0-146">Anger ett regex-mönster som anger vilka filer eller BLOB toocopy.</span><span class="sxs-lookup"><span data-stu-id="674f0-146">Specifies a regex pattern that indicates which blobs or files toocopy.</span></span> <span data-ttu-id="674f0-147">AdlCopy använder skiftlägeskänsliga matchar.</span><span class="sxs-lookup"><span data-stu-id="674f0-147">AdlCopy uses case-sensitive matching.</span></span> <span data-ttu-id="674f0-148">hello Standardmönster som används när inga mönster anges är toocopy alla objekt.</span><span class="sxs-lookup"><span data-stu-id="674f0-148">hello default pattern used when no pattern is specified is toocopy all items.</span></span> <span data-ttu-id="674f0-149">Ange flera filen mönster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="674f0-149">Specifying multiple file patterns is not supported.</span></span> |

## <a name="use-adlcopy-as-standalone-toocopy-data-from-an-azure-storage-blob"></a><span data-ttu-id="674f0-150">Använda AdlCopy (som fristående) toocopy data från en Azure Storage-blob</span><span class="sxs-lookup"><span data-stu-id="674f0-150">Use AdlCopy (as standalone) toocopy data from an Azure Storage blob</span></span>
1. <span data-ttu-id="674f0-151">Öppna en kommandotolk och navigera toohello directory där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="674f0-151">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="674f0-152">Kör följande kommando toocopy hello en specifik blobb från hello källa behållaren tooa Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="674f0-152">Run hello following command toocopy a specific blob from hello source container tooa Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="674f0-153">Exempel:</span><span class="sxs-lookup"><span data-stu-id="674f0-153">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] <span data-ttu-id="674f0-154">hello ovanstående syntax anger hello toobe kopierade tooa mapp i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-154">hello syntax above specifies hello file toobe copied tooa folder in hello Data Lake Store account.</span></span> <span data-ttu-id="674f0-155">AdlCopy verktyget skapar en mapp om hello angivna mappen inte finns.</span><span class="sxs-lookup"><span data-stu-id="674f0-155">AdlCopy tool creates a folder if hello specified folder name does not exist.</span></span>

    <span data-ttu-id="674f0-156">Du kan ange tooenter hello autentiseringsuppgifter för hello Azure-prenumeration som du har ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-156">You will be prompted tooenter hello credentials for hello Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="674f0-157">En liknande toohello följande i utdata visas:</span><span class="sxs-lookup"><span data-stu-id="674f0-157">You will see an output similar toohello following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. <span data-ttu-id="674f0-158">Du kan också kopiera alla hello BLOB från en behållare toohello Data Lake Store-konto med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="674f0-158">You can also copy all hello blobs from one container toohello Data Lake Store account using hello following command:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    <span data-ttu-id="674f0-159">Exempel:</span><span class="sxs-lookup"><span data-stu-id="674f0-159">For example:</span></span>

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a><span data-ttu-id="674f0-160">Saker att tänka på gällande prestanda</span><span class="sxs-lookup"><span data-stu-id="674f0-160">Performance considerations</span></span>

<span data-ttu-id="674f0-161">Om du kopierar från ett Azure Blob Storage-konto kan du begränsas under kopia på hello blob-lagring på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="674f0-161">If you are copying from an Azure Blob Storage account, you may be throttled during copy on hello blob storage side.</span></span> <span data-ttu-id="674f0-162">Hello prestanda kopiera jobbet kommer att försämras.</span><span class="sxs-lookup"><span data-stu-id="674f0-162">This will degrade hello performance of your copy job.</span></span> <span data-ttu-id="674f0-163">toolearn mer om hello gränserna för Azure Blob Storage finns i Azure Storage gränser med [Azure-prenumeration och tjänstbegränsningarna](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="674f0-163">toolearn more about hello limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="use-adlcopy-as-standalone-toocopy-data-from-another-data-lake-store-account"></a><span data-ttu-id="674f0-164">Använda AdlCopy (som fristående) toocopy data från en annan Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="674f0-164">Use AdlCopy (as standalone) toocopy data from another Data Lake Store account</span></span>
<span data-ttu-id="674f0-165">Du kan också använda AdlCopy toocopy data mellan två Data Lake Store-konton.</span><span class="sxs-lookup"><span data-stu-id="674f0-165">You can also use AdlCopy toocopy data between two Data Lake Store accounts.</span></span>

1. <span data-ttu-id="674f0-166">Öppna en kommandotolk och navigera toohello directory där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="674f0-166">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="674f0-167">Kör följande kommando toocopy hello en viss fil från ett Data Lake Store-konto tooanother.</span><span class="sxs-lookup"><span data-stu-id="674f0-167">Run hello following command toocopy a specific file from one Data Lake Store account tooanother.</span></span>

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    <span data-ttu-id="674f0-168">Exempel:</span><span class="sxs-lookup"><span data-stu-id="674f0-168">For example:</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > <span data-ttu-id="674f0-169">hello ovanstående syntax anger hello toobe kopierade tooa mapp i hello mål Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-169">hello syntax above specifies hello file toobe copied tooa folder in hello destination Data Lake Store account.</span></span> <span data-ttu-id="674f0-170">AdlCopy verktyget skapar en mapp om hello angivna mappen inte finns.</span><span class="sxs-lookup"><span data-stu-id="674f0-170">AdlCopy tool creates a folder if hello specified folder name does not exist.</span></span>
   >
   >

    <span data-ttu-id="674f0-171">Du kan ange tooenter hello autentiseringsuppgifter för hello Azure-prenumeration som du har ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-171">You will be prompted tooenter hello credentials for hello Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="674f0-172">En liknande toohello följande i utdata visas:</span><span class="sxs-lookup"><span data-stu-id="674f0-172">You will see an output similar toohello following:</span></span>

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. <span data-ttu-id="674f0-173">hello kopierar följande kommando alla filer i en mapp i hello Data Lake Store-konto tooa källmapp i hello målet Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-173">hello following command copies all files from a specific folder in hello source Data Lake Store account tooa folder in hello destination Data Lake Store account.</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a><span data-ttu-id="674f0-174">Saker att tänka på gällande prestanda</span><span class="sxs-lookup"><span data-stu-id="674f0-174">Performance considerations</span></span>

<span data-ttu-id="674f0-175">När du använder AdlCopy som ett fristående verktyg hello kopia körs på delade, Azure hanterade resurser.</span><span class="sxs-lookup"><span data-stu-id="674f0-175">When using AdlCopy as a standalone tool, hello copy is run on shared, Azure managed resources.</span></span> <span data-ttu-id="674f0-176">hello prestanda du kan få i den här miljön är beroende av systembelastning och tillgängliga resurser.</span><span class="sxs-lookup"><span data-stu-id="674f0-176">hello performance you may get in this environment depends on system load and available resources.</span></span> <span data-ttu-id="674f0-177">Det här läget är bäst för mindre överföringar på ad hoc-basis.</span><span class="sxs-lookup"><span data-stu-id="674f0-177">This mode is best used for small transfers on an ad hoc basis.</span></span> <span data-ttu-id="674f0-178">Inga parametrar måste toobe justerade när du använder AdlCopy som ett fristående verktyg.</span><span class="sxs-lookup"><span data-stu-id="674f0-178">No parameters need toobe tuned when using AdlCopy as a standalone tool.</span></span>

## <a name="use-adlcopy-with-data-lake-analytics-account-toocopy-data"></a><span data-ttu-id="674f0-179">Använda AdlCopy (med Data Lake Analytics-konto) toocopy data</span><span class="sxs-lookup"><span data-stu-id="674f0-179">Use AdlCopy (with Data Lake Analytics account) toocopy data</span></span>
<span data-ttu-id="674f0-180">Du kan också använda Data Lake Analytics konto toorun hello AdlCopy toocopy jobbdata från Azure storage-blobbar tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="674f0-180">You can also use your Data Lake Analytics account toorun hello AdlCopy job toocopy data from Azure storage blobs tooData Lake Store.</span></span> <span data-ttu-id="674f0-181">Normalt använder du det här alternativet när hello data toobe flyttas är i intervallet hello gigabyte och terabyte och du vill ha bättre och förutsägbar prestanda, genomströmning.</span><span class="sxs-lookup"><span data-stu-id="674f0-181">You would typically use this option when hello data toobe moved is in hello range of gigabytes and terabytes, and you want better and predictable performance throughput.</span></span>

<span data-ttu-id="674f0-182">toouse ditt Data Lake Analytics-konto med AdlCopy toocopy från ett Azure Storage Blob, hello källa (Azure Storage Blob) måste läggas till som en datakälla för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="674f0-182">toouse your Data Lake Analytics account with AdlCopy toocopy from an Azure Storage Blob, hello source (Azure Storage Blob) must be added as a data source for your Data Lake Analytics account.</span></span> <span data-ttu-id="674f0-183">Anvisningar för att lägga till ytterligare datakällor tooyour Data Lake Analytics-konto finns [hantera Data Lake Analytics-kontot datakällor](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span><span class="sxs-lookup"><span data-stu-id="674f0-183">For instructions on adding additional data sources tooyour Data Lake Analytics account, see [Manage Data Lake Analytics account data sources](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span></span>

> [!NOTE]
> <span data-ttu-id="674f0-184">Om du kopierar från ett Azure Data Lake Store-konto som hello-datakälla med hjälp av ett Data Lake Analytics-konto behöver du inte tooassociate hello Data Lake Store-konto med hello Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-184">If you are copying from an Azure Data Lake Store account as hello source using a Data Lake Analytics account, you do not need tooassociate hello Data Lake Store account with hello Data Lake Analytics account.</span></span> <span data-ttu-id="674f0-185">hello krav tooassociate hello källa store med hello Data Lake Analytics-konto är bara när hello källa är ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="674f0-185">hello requirement tooassociate hello source store with hello Data Lake Analytics account is only when hello source is an Azure Storage account.</span></span>
>
>

<span data-ttu-id="674f0-186">Kör följande kommando toocopy från ett Azure Storage blob tooa Data Lake Store-konto med hjälp av Data Lake Analytics-konto hello:</span><span class="sxs-lookup"><span data-stu-id="674f0-186">Run hello following command toocopy from an Azure Storage blob tooa Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

<span data-ttu-id="674f0-187">Exempel:</span><span class="sxs-lookup"><span data-stu-id="674f0-187">For example:</span></span>

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

<span data-ttu-id="674f0-188">På liknande sätt, kör hello efter kommandot toocopy från ett Azure Storage blob tooa Data Lake Store-konto med hjälp av Data Lake Analytics-konto:</span><span class="sxs-lookup"><span data-stu-id="674f0-188">Similarly, run hello following command toocopy from an Azure Storage blob tooa Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a><span data-ttu-id="674f0-189">Saker att tänka på gällande prestanda</span><span class="sxs-lookup"><span data-stu-id="674f0-189">Performance considerations</span></span>

<span data-ttu-id="674f0-190">När du kopierar data i intervallet hello terabyte ger AdlCopy med din egen Azure Data Lake Analytics-kontot bättre och mer förutsägbar prestanda.</span><span class="sxs-lookup"><span data-stu-id="674f0-190">When copying data in hello range of terabytes, using AdlCopy with your own Azure Data Lake Analytics account provides better and more predictable performance.</span></span> <span data-ttu-id="674f0-191">hello-parameter som ska justeras är hello Azure Data Lake Analytics enheter toouse för hello kopieringsjobbet.</span><span class="sxs-lookup"><span data-stu-id="674f0-191">hello parameter that should be tuned is hello number of Azure Data Lake Analytics Units toouse for hello copy job.</span></span> <span data-ttu-id="674f0-192">Öka hello antalet enheter ökar hello prestanda för Kopiera projekt.</span><span class="sxs-lookup"><span data-stu-id="674f0-192">Increasing hello number of units will increase hello performance of your copy job.</span></span> <span data-ttu-id="674f0-193">Varje fil toobe kopieras kan använda maximalt en enhet.</span><span class="sxs-lookup"><span data-stu-id="674f0-193">Each file toobe copied can use maximum one unit.</span></span> <span data-ttu-id="674f0-194">Ange fler enheter än hello antalet filer som kopieras kommer inte att öka prestanda.</span><span class="sxs-lookup"><span data-stu-id="674f0-194">Specifying more units than hello number of files being copied will not increase performance.</span></span>

## <a name="use-adlcopy-toocopy-data-using-pattern-matching"></a><span data-ttu-id="674f0-195">Använd AdlCopy toocopy data med hjälp av mönstermatchning</span><span class="sxs-lookup"><span data-stu-id="674f0-195">Use AdlCopy toocopy data using pattern matching</span></span>
<span data-ttu-id="674f0-196">I det här avsnittet lär du dig hur toouse AdlCopy toocopy data från en källa (i vårt exempel nedan använder vi Azure Storage Blob) tooa mål Data Lake Store-konto med hjälp av mönstermatchning.</span><span class="sxs-lookup"><span data-stu-id="674f0-196">In this section, you learn how toouse AdlCopy toocopy data from a source (in our example below we use Azure Storage Blob) tooa destination Data Lake Store account using pattern matching.</span></span> <span data-ttu-id="674f0-197">Du kan till exempel använda hello stegen nedan toocopy alla filer med CSV-fil från hello källa blob toohello mål.</span><span class="sxs-lookup"><span data-stu-id="674f0-197">For example, you can use hello steps below toocopy all files with .csv extension from hello source blob toohello destination.</span></span>

1. <span data-ttu-id="674f0-198">Öppna en kommandotolk och navigera toohello directory där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="674f0-198">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="674f0-199">Kör följande kommando toocopy hello alla filer med *.csv tillägg från en specifik blobb från hello källa behållaren tooa Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="674f0-199">Run hello following command toocopy all files with *.csv extension from a specific blob from hello source container tooa Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    <span data-ttu-id="674f0-200">Exempel:</span><span class="sxs-lookup"><span data-stu-id="674f0-200">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a><span data-ttu-id="674f0-201">Fakturering</span><span class="sxs-lookup"><span data-stu-id="674f0-201">Billing</span></span>
* <span data-ttu-id="674f0-202">Om du använder hello AdlCopy verktyget som fristående du debiteras för kostnader för nätverksegress för att flytta hello data om hello källa Azure Storage-konto inte är i samma region som hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="674f0-202">If you use hello AdlCopy tool as standalone you will be billed for egress costs for moving data, if hello source Azure Storage account is not in hello same region as hello Data Lake Store.</span></span>
* <span data-ttu-id="674f0-203">Om du använder hello AdlCopy verktyget med Data Lake Analytics-kontot, standard [Data Lake Analytics fakturering priser](https://azure.microsoft.com/pricing/details/data-lake-analytics/) gäller.</span><span class="sxs-lookup"><span data-stu-id="674f0-203">If you use hello AdlCopy tool with your Data Lake Analytics account, standard [Data Lake Analytics billing rates](https://azure.microsoft.com/pricing/details/data-lake-analytics/) will apply.</span></span>

## <a name="considerations-for-using-adlcopy"></a><span data-ttu-id="674f0-204">Överväganden för att använda AdlCopy</span><span class="sxs-lookup"><span data-stu-id="674f0-204">Considerations for using AdlCopy</span></span>
* <span data-ttu-id="674f0-205">AdlCopy (för version 1.0.5), har stöd för kopiering av data från källor som gemensamt har mer än tusentals filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="674f0-205">AdlCopy (for version 1.0.5), supports copying data from sources that collectively have more than thousands of files and folders.</span></span> <span data-ttu-id="674f0-206">Om det uppstår problem kopiera en stor datauppsättning kan du dock distribuera hello filer/mappar till olika undermappar och Använd hello sökvägen toothose undermappar som hello källa i stället.</span><span class="sxs-lookup"><span data-stu-id="674f0-206">However, if you encounter issues copying a large dataset, you can distribute hello files/folders into different sub-folders and use hello path toothose sub-folders as hello source instead.</span></span>

## <a name="performance-considerations-for-using-adlcopy"></a><span data-ttu-id="674f0-207">Prestandaöverväganden för att använda AdlCopy</span><span class="sxs-lookup"><span data-stu-id="674f0-207">Performance considerations for using AdlCopy</span></span>

<span data-ttu-id="674f0-208">AdlCopy har stöd för kopiering av data som innehåller tusentals filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="674f0-208">AdlCopy supports copying data containing thousands of files and folders.</span></span> <span data-ttu-id="674f0-209">Om du får problem med kopiera en stor datauppsättning kan du distribuera hello filer/mappar till mindre underordnade mappar.</span><span class="sxs-lookup"><span data-stu-id="674f0-209">However, if you encounter issues copying a large dataset, you can distribute hello files/folders into smaller sub-folders.</span></span> <span data-ttu-id="674f0-210">AdlCopy skapades för ad hoc-kopior.</span><span class="sxs-lookup"><span data-stu-id="674f0-210">AdlCopy was built for ad hoc copies.</span></span> <span data-ttu-id="674f0-211">Om du försöker toocopy data regelbundet, bör du använda [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) som ger fullständig hantering runt hello kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="674f0-211">If you are trying toocopy data on a recurring basis, you should consider using [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) that provides full management around hello copy operations.</span></span>

## <a name="release-notes"></a><span data-ttu-id="674f0-212">Viktig information</span><span class="sxs-lookup"><span data-stu-id="674f0-212">Release notes</span></span>
* <span data-ttu-id="674f0-213">1.0.13 - om du kopierar data toohello samma Azure Data Lake Store-konto över flera adlcopy kommandon du inte behöver tooreenter dina autentiseringsuppgifter för varje köras längre.</span><span class="sxs-lookup"><span data-stu-id="674f0-213">1.0.13 - If you are copying data toohello same Azure Data Lake Store account across multiple adlcopy commands, you do not need tooreenter your credentials for each run anymore.</span></span> <span data-ttu-id="674f0-214">Adlcopy nu cachelagras informationen över flera körs.</span><span class="sxs-lookup"><span data-stu-id="674f0-214">Adlcopy will now cache that information across multiple runs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="674f0-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="674f0-215">Next steps</span></span>
* [<span data-ttu-id="674f0-216">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="674f0-216">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="674f0-217">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="674f0-217">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="674f0-218">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="674f0-218">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
