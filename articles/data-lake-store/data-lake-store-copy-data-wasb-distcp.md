---
title: "aaaCopy data tooand från WASB till Data Lake Store med hjälp av Distcp | Microsoft Docs"
description: "Använd Distcp verktyget toocopy data tooand från Azure Storage BLOB tooData Datasjölager"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: ec23410bbab0f82449a475412bc3b097c4a8fccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-distcp-toocopy-data-between-azure-storage-blobs-and-data-lake-store"></a><span data-ttu-id="67a7d-103">Använd Distcp toocopy data mellan Azure Storage-Blobbar och Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67a7d-103">Use Distcp toocopy data between Azure Storage Blobs and Data Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="67a7d-104">Använda DistCp</span><span class="sxs-lookup"><span data-stu-id="67a7d-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="67a7d-105">Använda AdlCopy</span><span class="sxs-lookup"><span data-stu-id="67a7d-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="67a7d-106">När du har skapat ett HDInsight-kluster som har åtkomst till tooa Data Lake Store-konto kan du använda Hadoop-ekosystemet verktyg som Distcp toocopy data **tooand från** ett HDInsight-kluster storage (WASB) till ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="67a7d-106">Once you have created an HDInsight cluster that has access tooa Data Lake Store account, you can use Hadoop ecosystem tools like Distcp toocopy data **tooand from** an HDInsight cluster storage (WASB) into a Data Lake Store account.</span></span> <span data-ttu-id="67a7d-107">Den här artikeln innehåller instruktioner om hur tooachieve detta.</span><span class="sxs-lookup"><span data-stu-id="67a7d-107">This article provides instructions on how tooachieve this.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67a7d-108">Krav</span><span class="sxs-lookup"><span data-stu-id="67a7d-108">Prerequisites</span></span>
<span data-ttu-id="67a7d-109">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="67a7d-109">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="67a7d-110">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="67a7d-110">**An Azure subscription**.</span></span> <span data-ttu-id="67a7d-111">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="67a7d-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="67a7d-112">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="67a7d-112">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="67a7d-113">Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="67a7d-113">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="67a7d-114">**Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="67a7d-114">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="67a7d-115">Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="67a7d-115">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="67a7d-116">Kontrollera att du kan aktivera Fjärrskrivbord för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="67a7d-116">Make sure you enable Remote Desktop for hello cluster.</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="67a7d-117">Lär du dig snabbt med videor?</span><span class="sxs-lookup"><span data-stu-id="67a7d-117">Do you learn fast with videos?</span></span>
<span data-ttu-id="67a7d-118">[Det här videoklippet](https://mix.office.com/watch/1liuojvdx6sie) om hur toocopy data mellan Azure Storage-Blobbar och Data Lake Store med hjälp av DistCp.</span><span class="sxs-lookup"><span data-stu-id="67a7d-118">[Watch this video](https://mix.office.com/watch/1liuojvdx6sie) on how toocopy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a><span data-ttu-id="67a7d-119">Använd Distcp från ett kluster i HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="67a7d-119">Use Distcp from an HDInsight Linux cluster</span></span>

<span data-ttu-id="67a7d-120">Ett HDInsight-kluster innehåller hello Distcp verktyg som kan vara används toocopy data från olika källor till ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="67a7d-120">An HDInsight cluster comes with hello Distcp utility, which can be used toocopy data from different sources into an HDInsight cluster.</span></span> <span data-ttu-id="67a7d-121">Om du har konfigurerat hello HDInsight klustret toouse Data Lake Store som ett ytterligare lagringsutrymme, används hello Distcp verktyg kan vara out box toocopy data tooand från ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="67a7d-121">If you have configured hello HDInsight cluster toouse Data Lake Store as an additional storage, hello Distcp utility can be used out-of-the-box toocopy data tooand from a Data Lake Store account as well.</span></span> <span data-ttu-id="67a7d-122">I det här avsnittet titta vi på hur toouse hello Distcp-verktyget.</span><span class="sxs-lookup"><span data-stu-id="67a7d-122">In this section we look at how toouse hello Distcp utility.</span></span>

1. <span data-ttu-id="67a7d-123">Använda SSH tooconnect toohello kluster från ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="67a7d-123">From your desktop, use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="67a7d-124">Se [Anslut tooa Linux-baserade HDInsight-kluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="67a7d-124">See [Connect tooa Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="67a7d-125">Kör hello kommandon från hello SSH-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="67a7d-125">Run hello commands from hello SSH prompt.</span></span>

2. <span data-ttu-id="67a7d-126">Kontrollera om du kan komma åt hello Azure Storage BLOB (WASB).</span><span class="sxs-lookup"><span data-stu-id="67a7d-126">Verify whether you can access hello Azure Storage Blobs (WASB).</span></span> <span data-ttu-id="67a7d-127">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="67a7d-127">Run hello following command:</span></span>

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    <span data-ttu-id="67a7d-128">Detta bör ge en lista över innehållet i hello storage-blob.</span><span class="sxs-lookup"><span data-stu-id="67a7d-128">This should provide a list of contents in hello storage blob.</span></span>
3. <span data-ttu-id="67a7d-129">På liknande sätt kan kontrollera om du kan komma åt hello Data Lake Store-konto från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="67a7d-129">Similarly, verify whether you can access hello Data Lake Store account from hello cluster.</span></span> <span data-ttu-id="67a7d-130">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="67a7d-130">Run hello following command:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    <span data-ttu-id="67a7d-131">Detta bör ge en lista över filer/mappar i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="67a7d-131">This should provide a list of files/folders in hello Data Lake Store account.</span></span>
4. <span data-ttu-id="67a7d-132">Använd Distcp toocopy data från WASB tooa Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="67a7d-132">Use Distcp toocopy data from WASB tooa Data Lake Store account.</span></span>

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    <span data-ttu-id="67a7d-133">Detta kopierar hello innehållet i hello **/exempel/data/gutenberg/** mapp i WASB för**/myfolder** i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="67a7d-133">This will copy hello contents of hello **/example/data/gutenberg/** folder in WASB too**/myfolder** in hello Data Lake Store account.</span></span>
5. <span data-ttu-id="67a7d-134">På liknande sätt, Använd Distcp toocopy data från Data Lake Store-konto tooWASB.</span><span class="sxs-lookup"><span data-stu-id="67a7d-134">Similarly, use Distcp toocopy data from Data Lake Store account tooWASB.</span></span>

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    <span data-ttu-id="67a7d-135">Detta kopierar hello innehållet i **/myfolder** i hello Data Lake Store-konto för**/exempel/data/gutenberg/** mapp i WASB.</span><span class="sxs-lookup"><span data-stu-id="67a7d-135">This will copy hello contents of **/myfolder** in hello Data Lake Store account too**/example/data/gutenberg/** folder in WASB.</span></span>

## <a name="performance-considerations-while-using-distcp"></a><span data-ttu-id="67a7d-136">Prestandaöverväganden när du använder DistCp</span><span class="sxs-lookup"><span data-stu-id="67a7d-136">Performance considerations while using DistCp</span></span>

<span data-ttu-id="67a7d-137">Eftersom DistCp är lägsta Granularitet är en enskild fil är inställningen hello maximalt antal samtidiga kopior hello viktigaste parametern toooptimize den mot Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="67a7d-137">Because DistCp’s lowest granularity is a single file, setting hello maximum number of simultaneous copies is hello most important parameter toooptimize it against Data Lake Store.</span></span> <span data-ttu-id="67a7d-138">Detta styrs genom att ange hello antalet mappers ('M ') parametern på kommandoraden för hello.</span><span class="sxs-lookup"><span data-stu-id="67a7d-138">This is controlled by setting hello number of mappers (‘m’) parameter on hello command line.</span></span> <span data-ttu-id="67a7d-139">Den här parametern anger hello maximalt antal mappers som kommer att använda toocopy data.</span><span class="sxs-lookup"><span data-stu-id="67a7d-139">This parameter specifies hello maximum number of mappers that will be used toocopy data.</span></span> <span data-ttu-id="67a7d-140">Standardvärdet är 20.</span><span class="sxs-lookup"><span data-stu-id="67a7d-140">Default value is 20.</span></span>

<span data-ttu-id="67a7d-141">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="67a7d-141">**Example**</span></span>

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-hello-number-of-mappers-toouse"></a><span data-ttu-id="67a7d-142">Hur vet hello antalet mappers toouse?</span><span class="sxs-lookup"><span data-stu-id="67a7d-142">How do I determine hello number of mappers toouse?</span></span>

<span data-ttu-id="67a7d-143">Här är några riktlinjer som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="67a7d-143">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="67a7d-144">**Steg 1: Välj den totala YARN minnet** -hello första steget är toodetermine hello YARN minne tillgängligt toohello klustret där du kör hello DistCp jobb.</span><span class="sxs-lookup"><span data-stu-id="67a7d-144">**Step 1: Determine total YARN memory** - hello first step is toodetermine hello YARN memory available toohello cluster where you run hello DistCp job.</span></span> <span data-ttu-id="67a7d-145">Den här informationen är tillgänglig i hello Ambari portal som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="67a7d-145">This information is available in hello Ambari portal associated with hello cluster.</span></span> <span data-ttu-id="67a7d-146">Navigera tooYARN och visa hello konfigurationerna fliken toosee hello YARN minne.</span><span class="sxs-lookup"><span data-stu-id="67a7d-146">Navigate tooYARN and view hello Configs tab toosee hello YARN memory.</span></span> <span data-ttu-id="67a7d-147">tooget hello totala YARN-minne, multiplicera hello YARN minne per nod med hello antalet noder som finns i klustret.</span><span class="sxs-lookup"><span data-stu-id="67a7d-147">tooget hello total YARN memory, multiply hello YARN memory per node with hello number of nodes you have in your cluster.</span></span>

* <span data-ttu-id="67a7d-148">**Steg 2: Beräkna hello antalet mappers** -hello värdet för **m** är lika toohello kvoten av den totala YARN minnet dividerat med hello YARN behållarens storlek.</span><span class="sxs-lookup"><span data-stu-id="67a7d-148">**Step 2: Calculate hello number of mappers** - hello value of **m** is equal toohello quotient of total YARN memory divided by hello YARN container size.</span></span> <span data-ttu-id="67a7d-149">Hej YARN behållare Storleksinformation är tillgänglig i hello Ambari samt portal.</span><span class="sxs-lookup"><span data-stu-id="67a7d-149">hello YARN container size information is available in hello Ambari portal as well.</span></span> <span data-ttu-id="67a7d-150">Navigera tooYARN och visa hello konfigurationerna fliken hello YARN behållarens storlek visas i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="67a7d-150">Navigate tooYARN and view hello Configs tab. hello YARN container size is displayed in this window.</span></span> <span data-ttu-id="67a7d-151">Hej ekvationen tooarrive vid hello antalet mappers (**m**) är</span><span class="sxs-lookup"><span data-stu-id="67a7d-151">hello equation tooarrive at hello number of mappers (**m**) is</span></span>

        m = (number of nodes * YARN memory for each node) / YARN container size

<span data-ttu-id="67a7d-152">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="67a7d-152">**Example**</span></span>

<span data-ttu-id="67a7d-153">Anta att du har en 4 D14v2s-noder i klustret hello och du försöker tootransfer 10TB data från 10 olika mappar.</span><span class="sxs-lookup"><span data-stu-id="67a7d-153">Let’s assume that you have a 4 D14v2s nodes in hello cluster and you are trying tootransfer 10TB of data from 10 different folders.</span></span> <span data-ttu-id="67a7d-154">Hello mappar innehålla olika mängder data och hello filstorlekar inom varje mapp är olika.</span><span class="sxs-lookup"><span data-stu-id="67a7d-154">Each of hello folders contain varying amounts of data and hello file sizes within each folder are different.</span></span>

* <span data-ttu-id="67a7d-155">Totala minnet i YARN - från hello Ambari portal du bestämma att hello YARN minne är 96GB för en D14-nod.</span><span class="sxs-lookup"><span data-stu-id="67a7d-155">Total YARN memory - From hello Ambari portal you determine that hello YARN memory is 96GB for a D14 node.</span></span> <span data-ttu-id="67a7d-156">Så är den totala YARN minnet för kluster med 4 noder:</span><span class="sxs-lookup"><span data-stu-id="67a7d-156">So, total YARN memory for 4 node cluster is:</span></span> 

        YARN memory = 4 * 96GB = 384GB

* <span data-ttu-id="67a7d-157">Antal mappers - från hello Ambari portal du bestämma att hello YARN behållarens storlek är 3072 för D14 klusternoder.</span><span class="sxs-lookup"><span data-stu-id="67a7d-157">Number of mappers - From hello Ambari portal you determine that hello YARN container size is 3072 for a D14 cluster node.</span></span> <span data-ttu-id="67a7d-158">Det är antal mappers:</span><span class="sxs-lookup"><span data-stu-id="67a7d-158">So, number of mappers is:</span></span>

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

<span data-ttu-id="67a7d-159">Om andra program använder minne, kan sedan du välja tooonly används en del av ditt kluster YARN minne för DistCp.</span><span class="sxs-lookup"><span data-stu-id="67a7d-159">If other applications are using memory, then you can choose tooonly use a portion of your cluster’s YARN memory for DistCp.</span></span>

### <a name="copying-large-datasets"></a><span data-ttu-id="67a7d-160">Kopiera stora datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="67a7d-160">Copying large datasets</span></span>

<span data-ttu-id="67a7d-161">När hello storleken på hello dataset toobe flyttas är mycket stora (t.ex. > 1TB) eller om du har många olika mappar bör du använda flera DistCp jobb.</span><span class="sxs-lookup"><span data-stu-id="67a7d-161">When hello size of hello dataset toobe moved is very large (e.g. >1TB) or if you have many different folders, you should consider using multiple DistCp jobs.</span></span> <span data-ttu-id="67a7d-162">Det är förmodligen inga prestandafördelar, men den att sprida ut hello jobb så att om något jobb misslyckas, behöver du bara toorestart som jobbets i stället för hela hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="67a7d-162">There is likely no performance gain, but it will spread out hello jobs so that if any job fails, you will only need toorestart that specific job rather than hello entire job.</span></span>

### <a name="limitations"></a><span data-ttu-id="67a7d-163">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="67a7d-163">Limitations</span></span>

* <span data-ttu-id="67a7d-164">DistCp försöker toocreate mappers med liknande storlek toooptimize prestanda.</span><span class="sxs-lookup"><span data-stu-id="67a7d-164">DistCp tries toocreate mappers that are similar in size toooptimize performance.</span></span> <span data-ttu-id="67a7d-165">Öka hello antal mappers kanske inte alltid att öka prestanda.</span><span class="sxs-lookup"><span data-stu-id="67a7d-165">Increasing hello number of mappers may not always increase performance.</span></span>

* <span data-ttu-id="67a7d-166">DistCp är begränsad tooonly en mappning per fil.</span><span class="sxs-lookup"><span data-stu-id="67a7d-166">DistCp is limited tooonly one mapper per file.</span></span> <span data-ttu-id="67a7d-167">Du bör därför inte ha flera mappers än vad du har filer.</span><span class="sxs-lookup"><span data-stu-id="67a7d-167">Therefore, you should not have more mappers than you have files.</span></span> <span data-ttu-id="67a7d-168">Eftersom DistCp kan endast tilldelas en mapper tooa fil, begränsar detta hello samtidighet som kan använda toocopy stora filer.</span><span class="sxs-lookup"><span data-stu-id="67a7d-168">Since DistCp can only assign one mapper tooa file, this limits hello amount of concurrency that can be used toocopy large files.</span></span>

* <span data-ttu-id="67a7d-169">Om du har ett litet antal stora filer, så att du ska dela upp dem i 256MB filen segment toogive du flera potentiella samtidighet.</span><span class="sxs-lookup"><span data-stu-id="67a7d-169">If you have a small number of large files, then you should split them into 256MB file chunks toogive you more potential concurrency.</span></span> 
 
* <span data-ttu-id="67a7d-170">Om du kopierar från ett Azure Blob Storage-konto, att kopiera-projekt begränsas på hello blob storage-sida.</span><span class="sxs-lookup"><span data-stu-id="67a7d-170">If you are copying from an Azure Blob Storage account, your copy job may be throttled on hello blob storage side.</span></span> <span data-ttu-id="67a7d-171">Hello prestanda kopiera jobbet kommer att försämras.</span><span class="sxs-lookup"><span data-stu-id="67a7d-171">This will degrade hello performance of your copy job.</span></span> <span data-ttu-id="67a7d-172">toolearn mer om hello gränserna för Azure Blob Storage finns i Azure Storage gränser med [Azure-prenumeration och tjänstbegränsningarna](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="67a7d-172">toolearn more about hello limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="67a7d-173">Se även</span><span class="sxs-lookup"><span data-stu-id="67a7d-173">See also</span></span>
* [<span data-ttu-id="67a7d-174">Kopiera data från Azure Storage BLOB tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="67a7d-174">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="67a7d-175">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67a7d-175">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="67a7d-176">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67a7d-176">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="67a7d-177">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67a7d-177">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
