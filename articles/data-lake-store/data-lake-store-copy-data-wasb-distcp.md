---
title: "Kopiera data till och från WASB till Data Lake Store med Distcp | Microsoft Docs"
description: "Använd Distcp för att kopiera data till och från Azure Storage-Blobbar till Data Lake Store"
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
ms.openlocfilehash: 356a93d330e9e8ce3eb3c6c982fc5c2e087846a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a><span data-ttu-id="73b76-103">Använd Distcp för att kopiera data mellan Azure Storage-blobbar och Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73b76-103">Use Distcp to copy data between Azure Storage Blobs and Data Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="73b76-104">Använda DistCp</span><span class="sxs-lookup"><span data-stu-id="73b76-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="73b76-105">Använda AdlCopy</span><span class="sxs-lookup"><span data-stu-id="73b76-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="73b76-106">När du har skapat ett HDInsight-kluster som har åtkomst till ett Data Lake Store-konto kan du använda Hadoop-ekosystemet verktyg som Distcp för att kopiera data **till och från** ett HDInsight-kluster storage (WASB) till ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="73b76-106">Once you have created an HDInsight cluster that has access to a Data Lake Store account, you can use Hadoop ecosystem tools like Distcp to copy data **to and from** an HDInsight cluster storage (WASB) into a Data Lake Store account.</span></span> <span data-ttu-id="73b76-107">Den här artikeln innehåller instruktioner om hur du uppnår detta.</span><span class="sxs-lookup"><span data-stu-id="73b76-107">This article provides instructions on how to achieve this.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73b76-108">Krav</span><span class="sxs-lookup"><span data-stu-id="73b76-108">Prerequisites</span></span>
<span data-ttu-id="73b76-109">Innan du påbörjar den här artikeln måste du ha:</span><span class="sxs-lookup"><span data-stu-id="73b76-109">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="73b76-110">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="73b76-110">**An Azure subscription**.</span></span> <span data-ttu-id="73b76-111">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="73b76-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="73b76-112">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="73b76-112">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="73b76-113">Anvisningar om hur du skapar en finns [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="73b76-113">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="73b76-114">**Azure HDInsight-kluster** med åtkomst till ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="73b76-114">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="73b76-115">Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="73b76-115">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="73b76-116">Kontrollera att du kan aktivera Fjärrskrivbord för klustret.</span><span class="sxs-lookup"><span data-stu-id="73b76-116">Make sure you enable Remote Desktop for the cluster.</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="73b76-117">Lär du dig snabbt med videor?</span><span class="sxs-lookup"><span data-stu-id="73b76-117">Do you learn fast with videos?</span></span>
<span data-ttu-id="73b76-118">[Det här videoklippet](https://mix.office.com/watch/1liuojvdx6sie) om hur du kopierar data mellan Azure Storage-Blobbar och Data Lake Store med hjälp av DistCp.</span><span class="sxs-lookup"><span data-stu-id="73b76-118">[Watch this video](https://mix.office.com/watch/1liuojvdx6sie) on how to copy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a><span data-ttu-id="73b76-119">Använd Distcp från ett kluster i HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="73b76-119">Use Distcp from an HDInsight Linux cluster</span></span>

<span data-ttu-id="73b76-120">Ett HDInsight-kluster medföljer verktyget Distcp som kan användas för att kopiera data från olika källor till ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="73b76-120">An HDInsight cluster comes with the Distcp utility, which can be used to copy data from different sources into an HDInsight cluster.</span></span> <span data-ttu-id="73b76-121">Om du har konfigurerat HDInsight-klustret för att använda Data Lake Store som ett ytterligare lagringsutrymme, att verktyget Distcp använda out-of-the-box att kopiera data till och från ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="73b76-121">If you have configured the HDInsight cluster to use Data Lake Store as an additional storage, the Distcp utility can be used out-of-the-box to copy data to and from a Data Lake Store account as well.</span></span> <span data-ttu-id="73b76-122">I det här avsnittet titta vi på hur du använder verktyget Distcp.</span><span class="sxs-lookup"><span data-stu-id="73b76-122">In this section we look at how to use the Distcp utility.</span></span>

1. <span data-ttu-id="73b76-123">Använd SSH för att ansluta till klustret från ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="73b76-123">From your desktop, use SSH to connect to the cluster.</span></span> <span data-ttu-id="73b76-124">Se [Anslut till ett Linux-baserat HDInsight-kluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="73b76-124">See [Connect to a Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="73b76-125">Kör kommandon från SSH-prompten.</span><span class="sxs-lookup"><span data-stu-id="73b76-125">Run the commands from the SSH prompt.</span></span>

2. <span data-ttu-id="73b76-126">Kontrollera om du har åtkomst till Azure Storage BLOB (WASB).</span><span class="sxs-lookup"><span data-stu-id="73b76-126">Verify whether you can access the Azure Storage Blobs (WASB).</span></span> <span data-ttu-id="73b76-127">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="73b76-127">Run the following command:</span></span>

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    <span data-ttu-id="73b76-128">Detta bör ge en lista över innehållet i blob storage.</span><span class="sxs-lookup"><span data-stu-id="73b76-128">This should provide a list of contents in the storage blob.</span></span>
3. <span data-ttu-id="73b76-129">På liknande sätt kan kontrollera om du har åtkomst till Data Lake Store-konto från klustret.</span><span class="sxs-lookup"><span data-stu-id="73b76-129">Similarly, verify whether you can access the Data Lake Store account from the cluster.</span></span> <span data-ttu-id="73b76-130">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="73b76-130">Run the following command:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    <span data-ttu-id="73b76-131">Detta bör ge en lista över filer/mappar i Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="73b76-131">This should provide a list of files/folders in the Data Lake Store account.</span></span>
4. <span data-ttu-id="73b76-132">Använd Distcp för att kopiera data från WASB till ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="73b76-132">Use Distcp to copy data from WASB to a Data Lake Store account.</span></span>

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    <span data-ttu-id="73b76-133">Detta kommer att kopiera innehållet i den **/exempel/data/gutenberg/** mapp i WASB till **/myfolder** i Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="73b76-133">This will copy the contents of the **/example/data/gutenberg/** folder in WASB to **/myfolder** in the Data Lake Store account.</span></span>
5. <span data-ttu-id="73b76-134">Använd Distcp på samma sätt för att kopiera data från Data Lake Store-konto till WASB.</span><span class="sxs-lookup"><span data-stu-id="73b76-134">Similarly, use Distcp to copy data from Data Lake Store account to WASB.</span></span>

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    <span data-ttu-id="73b76-135">Detta kommer att kopiera innehållet i **/myfolder** Data Lake Store-konto som **/exempel/data/gutenberg/** mapp i WASB.</span><span class="sxs-lookup"><span data-stu-id="73b76-135">This will copy the contents of **/myfolder** in the Data Lake Store account to **/example/data/gutenberg/** folder in WASB.</span></span>

## <a name="performance-considerations-while-using-distcp"></a><span data-ttu-id="73b76-136">Prestandaöverväganden när du använder DistCp</span><span class="sxs-lookup"><span data-stu-id="73b76-136">Performance considerations while using DistCp</span></span>

<span data-ttu-id="73b76-137">Eftersom Distcps lägsta Granularitet är en enskild fil, är ange det maximala antalet samtidiga kopior den viktigaste parametern att optimera mot Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="73b76-137">Because DistCp’s lowest granularity is a single file, setting the maximum number of simultaneous copies is the most important parameter to optimize it against Data Lake Store.</span></span> <span data-ttu-id="73b76-138">Detta styrs genom att ange antalet mappers ('M ') parametern på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="73b76-138">This is controlled by setting the number of mappers (‘m’) parameter on the command line.</span></span> <span data-ttu-id="73b76-139">Den här parametern anger det maximala antalet mappers som ska användas för att kopiera data.</span><span class="sxs-lookup"><span data-stu-id="73b76-139">This parameter specifies the maximum number of mappers that will be used to copy data.</span></span> <span data-ttu-id="73b76-140">Standardvärdet är 20.</span><span class="sxs-lookup"><span data-stu-id="73b76-140">Default value is 20.</span></span>

<span data-ttu-id="73b76-141">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="73b76-141">**Example**</span></span>

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a><span data-ttu-id="73b76-142">Hur tar jag reda på antalet mappers ska användas?</span><span class="sxs-lookup"><span data-stu-id="73b76-142">How do I determine the number of mappers to use?</span></span>

<span data-ttu-id="73b76-143">Här är några riktlinjer som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="73b76-143">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="73b76-144">**Steg 1: Välj den totala YARN minnet** -det första steget är att avgöra YARN-minne i klustret där du kör jobbet DistCp.</span><span class="sxs-lookup"><span data-stu-id="73b76-144">**Step 1: Determine total YARN memory** - The first step is to determine the YARN memory available to the cluster where you run the DistCp job.</span></span> <span data-ttu-id="73b76-145">Den här informationen är tillgänglig i Ambari-portalen som associeras med klustret.</span><span class="sxs-lookup"><span data-stu-id="73b76-145">This information is available in the Ambari portal associated with the cluster.</span></span> <span data-ttu-id="73b76-146">Navigera till YARN och visar fliken konfigurationerna för att visa YARN-minne.</span><span class="sxs-lookup"><span data-stu-id="73b76-146">Navigate to YARN and view the Configs tab to see the YARN memory.</span></span> <span data-ttu-id="73b76-147">För att få det totala minnet YARN, multiplicera YARN minne per nod med antalet noder som du har i klustret.</span><span class="sxs-lookup"><span data-stu-id="73b76-147">To get the total YARN memory, multiply the YARN memory per node with the number of nodes you have in your cluster.</span></span>

* <span data-ttu-id="73b76-148">**Steg 2: Beräkna antalet mappers** -värdet för **m** motsvarar kvoten av totalt YARN minne delat med storleken för YARN-behållare.</span><span class="sxs-lookup"><span data-stu-id="73b76-148">**Step 2: Calculate the number of mappers** - The value of **m** is equal to the quotient of total YARN memory divided by the YARN container size.</span></span> <span data-ttu-id="73b76-149">YARN behållaren Storleksinformation finns i Ambari-portalen.</span><span class="sxs-lookup"><span data-stu-id="73b76-149">The YARN container size information is available in the Ambari portal as well.</span></span> <span data-ttu-id="73b76-150">Navigera till YARN och visar fliken konfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="73b76-150">Navigate to YARN and view the Configs tab.</span></span> <span data-ttu-id="73b76-151">YARN behållaren storleken visas i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="73b76-151">The YARN container size is displayed in this window.</span></span> <span data-ttu-id="73b76-152">Formeln att få fram antalet mappers (**m**) är</span><span class="sxs-lookup"><span data-stu-id="73b76-152">The equation to arrive at the number of mappers (**m**) is</span></span>

        m = (number of nodes * YARN memory for each node) / YARN container size

<span data-ttu-id="73b76-153">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="73b76-153">**Example**</span></span>

<span data-ttu-id="73b76-154">Anta att du har en 4 D14v2s-noder i klustret och du försöker överföra 10TB data från 10 olika mappar.</span><span class="sxs-lookup"><span data-stu-id="73b76-154">Let’s assume that you have a 4 D14v2s nodes in the cluster and you are trying to transfer 10TB of data from 10 different folders.</span></span> <span data-ttu-id="73b76-155">Alla mappar innehålla olika mängder data och filstorlekarna i mappen är olika.</span><span class="sxs-lookup"><span data-stu-id="73b76-155">Each of the folders contain varying amounts of data and the file sizes within each folder are different.</span></span>

* <span data-ttu-id="73b76-156">Total YARN-minne, från Ambari portal YARN-minne ligger 96GB för en D14-nod.</span><span class="sxs-lookup"><span data-stu-id="73b76-156">Total YARN memory - From the Ambari portal you determine that the YARN memory is 96GB for a D14 node.</span></span> <span data-ttu-id="73b76-157">Så är den totala YARN minnet för kluster med 4 noder:</span><span class="sxs-lookup"><span data-stu-id="73b76-157">So, total YARN memory for 4 node cluster is:</span></span> 

        YARN memory = 4 * 96GB = 384GB

* <span data-ttu-id="73b76-158">Antal mappers - från Ambari portal du bestämma att YARN behållare storleken är 3072 för D14 klusternoder.</span><span class="sxs-lookup"><span data-stu-id="73b76-158">Number of mappers - From the Ambari portal you determine that the YARN container size is 3072 for a D14 cluster node.</span></span> <span data-ttu-id="73b76-159">Det är antal mappers:</span><span class="sxs-lookup"><span data-stu-id="73b76-159">So, number of mappers is:</span></span>

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

<span data-ttu-id="73b76-160">Om andra program använder minne, kan du välja att bara använda en del av ditt kluster YARN minne för DistCp.</span><span class="sxs-lookup"><span data-stu-id="73b76-160">If other applications are using memory, then you can choose to only use a portion of your cluster’s YARN memory for DistCp.</span></span>

### <a name="copying-large-datasets"></a><span data-ttu-id="73b76-161">Kopiera stora datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="73b76-161">Copying large datasets</span></span>

<span data-ttu-id="73b76-162">När storleken på datamängden som ska flyttas är mycket stora (t.ex. > 1TB) eller om du har många olika mappar bör du använda flera DistCp jobb.</span><span class="sxs-lookup"><span data-stu-id="73b76-162">When the size of the dataset to be moved is very large (e.g. >1TB) or if you have many different folders, you should consider using multiple DistCp jobs.</span></span> <span data-ttu-id="73b76-163">Det är förmodligen inga prestandafördelar, men den att sprida ut jobb så att om något jobb misslyckas, kommer du bara behöver starta om det specifika jobbet i stället för hela jobbet.</span><span class="sxs-lookup"><span data-stu-id="73b76-163">There is likely no performance gain, but it will spread out the jobs so that if any job fails, you will only need to restart that specific job rather than the entire job.</span></span>

### <a name="limitations"></a><span data-ttu-id="73b76-164">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="73b76-164">Limitations</span></span>

* <span data-ttu-id="73b76-165">DistCp försöker skapa mappers med liknande storlek för att optimera prestanda.</span><span class="sxs-lookup"><span data-stu-id="73b76-165">DistCp tries to create mappers that are similar in size to optimize performance.</span></span> <span data-ttu-id="73b76-166">Öka antalet mappers kanske inte alltid att öka prestanda.</span><span class="sxs-lookup"><span data-stu-id="73b76-166">Increasing the number of mappers may not always increase performance.</span></span>

* <span data-ttu-id="73b76-167">DistCp är begränsad till endast en mappning per fil.</span><span class="sxs-lookup"><span data-stu-id="73b76-167">DistCp is limited to only one mapper per file.</span></span> <span data-ttu-id="73b76-168">Du bör därför inte ha flera mappers än vad du har filer.</span><span class="sxs-lookup"><span data-stu-id="73b76-168">Therefore, you should not have more mappers than you have files.</span></span> <span data-ttu-id="73b76-169">Eftersom DistCp kan bara tilldela en mappning till en fil, begränsar detta mängden samtidighet som kan användas för att kopiera stora filer.</span><span class="sxs-lookup"><span data-stu-id="73b76-169">Since DistCp can only assign one mapper to a file, this limits the amount of concurrency that can be used to copy large files.</span></span>

* <span data-ttu-id="73b76-170">Om du har ett litet antal stora filer, bör du dela dem i 256MB filsegment att ge mer potentiella samtidighet.</span><span class="sxs-lookup"><span data-stu-id="73b76-170">If you have a small number of large files, then you should split them into 256MB file chunks to give you more potential concurrency.</span></span> 
 
* <span data-ttu-id="73b76-171">Om du kopierar från ett Azure Blob Storage-konto, att kopiera-projekt begränsas på blob storage-sida.</span><span class="sxs-lookup"><span data-stu-id="73b76-171">If you are copying from an Azure Blob Storage account, your copy job may be throttled on the blob storage side.</span></span> <span data-ttu-id="73b76-172">Detta försämrar prestanda för Kopiera projekt.</span><span class="sxs-lookup"><span data-stu-id="73b76-172">This will degrade the performance of your copy job.</span></span> <span data-ttu-id="73b76-173">Mer information om gränserna för Azure Blob Storage finns Azure Lagringsgränser på [Azure-prenumeration och tjänstbegränsningarna](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="73b76-173">To learn more about the limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="73b76-174">Se även</span><span class="sxs-lookup"><span data-stu-id="73b76-174">See also</span></span>
* [<span data-ttu-id="73b76-175">Kopiera data från Azure Storage-Blobbar till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73b76-175">Copy data from Azure Storage Blobs to Data Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="73b76-176">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73b76-176">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="73b76-177">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73b76-177">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="73b76-178">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="73b76-178">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
