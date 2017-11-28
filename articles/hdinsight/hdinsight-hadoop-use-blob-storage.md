---
title: "aaaQuery data från HDFS-kompatibla Azure storage - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur tooquery data från Azure storage och Azure Data Lake Store toostore resultat av dina analyser."
keywords: blob storage,hdfs,structured data,unstructured data,data lake store,Hadoop input,Hadoop output, hadoop storage, hdfs input,hdfs output,hdfs storage,wasb azure
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: jgao
ms.openlocfilehash: 1032d60424b65e3c0c54a25c7c15970b017a788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a><span data-ttu-id="4a08d-104">Använda Azure-lagring med Azure HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="4a08d-104">Use Azure storage with Azure HDInsight clusters</span></span>

<span data-ttu-id="4a08d-105">tooanalyze data i HDInsight-kluster, du kan lagra hello data i Azure Storage, Azure Data Lake Store eller båda.</span><span class="sxs-lookup"><span data-stu-id="4a08d-105">tooanalyze data in HDInsight cluster, you can store hello data either in Azure Storage, Azure Data Lake Store, or both.</span></span> <span data-ttu-id="4a08d-106">Båda lagringsalternativ aktivera toosafely ta bort HDInsight-kluster som används för beräkning utan att förlora användardata.</span><span class="sxs-lookup"><span data-stu-id="4a08d-106">Both storage options enable you toosafely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="4a08d-107">Hadoop stöder begreppet standardfilsystem hello.</span><span class="sxs-lookup"><span data-stu-id="4a08d-107">Hadoop supports a notion of hello default file system.</span></span> <span data-ttu-id="4a08d-108">Hej standardfilsystemet kräver ett standardschema och utfärdare.</span><span class="sxs-lookup"><span data-stu-id="4a08d-108">hello default file system implies a default scheme and authority.</span></span> <span data-ttu-id="4a08d-109">Det kan också vara används tooresolve relativa sökvägar.</span><span class="sxs-lookup"><span data-stu-id="4a08d-109">It can also be used tooresolve relative paths.</span></span> <span data-ttu-id="4a08d-110">Du kan ange en blob-behållare i Azure Storage som hello standardfilsystem under hello processen för HDInsight-kluster, eller med HDInsight 3.5, kan du välja Azure Storage eller Azure Data Lake Store som hello standard filer system med några få undantag.</span><span class="sxs-lookup"><span data-stu-id="4a08d-110">During hello HDInsight cluster creation process, you can specify a blob container in Azure Storage as hello default file system, or with HDInsight 3.5, you can select either Azure Storage or Azure Data Lake Store as hello default files system with a few exceptions.</span></span> <span data-ttu-id="4a08d-111">Hello när du kan använda Data Lake Store som både hello standard och länkad lagring, se [tillgången för HDInsight-kluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="4a08d-111">For hello supportability of using Data Lake Store as both hello default and linked storage, see [Availabilities for HDInsight cluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span></span>

<span data-ttu-id="4a08d-112">I den här artikeln får du lära dig hur Azure Storage fungerar med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4a08d-112">In this article, you learn how Azure Storage works with HDInsight clusters.</span></span> <span data-ttu-id="4a08d-113">toolearn hur Data Lake Store fungerar med HDInsight-kluster finns i [Använd Azure Data Lake Store med Azure HDInsight-kluster](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="4a08d-113">toolearn how Data Lake Store works with HDInsight clusters, see [Use Azure Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> <span data-ttu-id="4a08d-114">Mer information om hur du skapar ett HDInsight-kluster finns i [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Skapa Hadoop-kluster i HDInsight).</span><span class="sxs-lookup"><span data-stu-id="4a08d-114">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="4a08d-115">Azure Storage är en robust lagringslösning för allmänna ändamål som smidigt kan integreras med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4a08d-115">Azure storage is a robust, general-purpose storage solution that integrates seamlessly with HDInsight.</span></span> <span data-ttu-id="4a08d-116">HDInsight kan du använda en blob-behållare i Azure Storage som hello standardfilsystem för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="4a08d-116">HDInsight can use a blob container in Azure Storage as hello default file system for hello cluster.</span></span> <span data-ttu-id="4a08d-117">Via Hadoop distributed file system (HDFS) gränssnittet tillämpas hello fullständig uppsättning komponenter i HDInsight direkt på strukturerade eller Ostrukturerade data som lagras som blobar.</span><span class="sxs-lookup"><span data-stu-id="4a08d-117">Through a Hadoop distributed file system (HDFS) interface, hello full set of components in HDInsight can operate directly on structured or unstructured data stored as blobs.</span></span>

> [!WARNING]
> <span data-ttu-id="4a08d-118">Det finns flera tillgängliga alternativ när du skapar ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4a08d-118">There are several options available when creating an Azure Storage account.</span></span> <span data-ttu-id="4a08d-119">hello följande tabell innehåller information om vilka alternativ som stöds med HDInsight:</span><span class="sxs-lookup"><span data-stu-id="4a08d-119">hello following table provides information on what options are supported with HDInsight:</span></span>
> 
> | <span data-ttu-id="4a08d-120">Storage Account-typ</span><span class="sxs-lookup"><span data-stu-id="4a08d-120">Storage account type</span></span> | <span data-ttu-id="4a08d-121">Lagringsnivå</span><span class="sxs-lookup"><span data-stu-id="4a08d-121">Storage tier</span></span> | <span data-ttu-id="4a08d-122">Stöds av HDInsight</span><span class="sxs-lookup"><span data-stu-id="4a08d-122">Supported with HDInsight</span></span> |
> | ------- | ------- | ------- |
> | <span data-ttu-id="4a08d-123">Allmänna lagringskonton</span><span class="sxs-lookup"><span data-stu-id="4a08d-123">General-purpose Storage Account</span></span> | <span data-ttu-id="4a08d-124">Standard</span><span class="sxs-lookup"><span data-stu-id="4a08d-124">Standard</span></span> | <span data-ttu-id="4a08d-125">__Ja__</span><span class="sxs-lookup"><span data-stu-id="4a08d-125">__Yes__</span></span> |
> | &nbsp; | <span data-ttu-id="4a08d-126">Premium</span><span class="sxs-lookup"><span data-stu-id="4a08d-126">Premium</span></span> | <span data-ttu-id="4a08d-127">Nej</span><span class="sxs-lookup"><span data-stu-id="4a08d-127">No</span></span> |
> | <span data-ttu-id="4a08d-128">Blob Storage-konto</span><span class="sxs-lookup"><span data-stu-id="4a08d-128">Blob Storage Account</span></span> | <span data-ttu-id="4a08d-129">Frekvent</span><span class="sxs-lookup"><span data-stu-id="4a08d-129">Hot</span></span> | <span data-ttu-id="4a08d-130">Nej</span><span class="sxs-lookup"><span data-stu-id="4a08d-130">No</span></span> |
> | &nbsp; | <span data-ttu-id="4a08d-131">Lågfrekvent</span><span class="sxs-lookup"><span data-stu-id="4a08d-131">Cool</span></span> | <span data-ttu-id="4a08d-132">Nej</span><span class="sxs-lookup"><span data-stu-id="4a08d-132">No</span></span> |

<span data-ttu-id="4a08d-133">Vi rekommenderar inte att du använder hello standardbehållaren för att lagra företagets data.</span><span class="sxs-lookup"><span data-stu-id="4a08d-133">We do not recommend that you use hello default blob container for storing business data.</span></span> <span data-ttu-id="4a08d-134">Ta bort hello standardbehållaren efter varje Använd tooreduce är lagringskostnaden bra.</span><span class="sxs-lookup"><span data-stu-id="4a08d-134">Deleting hello default blob container after each use tooreduce storage cost is a good practice.</span></span> <span data-ttu-id="4a08d-135">Observera att hello standardbehållaren innehåller program- och loggar.</span><span class="sxs-lookup"><span data-stu-id="4a08d-135">Note that hello default container contains application and system logs.</span></span> <span data-ttu-id="4a08d-136">Se till att tooretrieve hello loggar innan du tar bort hello behållare.</span><span class="sxs-lookup"><span data-stu-id="4a08d-136">Make sure tooretrieve hello logs before deleting hello container.</span></span>

<span data-ttu-id="4a08d-137">Det går inte att dela en blobbehållare över flera kluster.</span><span class="sxs-lookup"><span data-stu-id="4a08d-137">Sharing one blob container for multiple clusters is not supported.</span></span>

## <a name="hdinsight-storage-architecture"></a><span data-ttu-id="4a08d-138">Lagringsarkitekturen i HDInsight</span><span class="sxs-lookup"><span data-stu-id="4a08d-138">HDInsight storage architecture</span></span>
<span data-ttu-id="4a08d-139">hello följande diagram ger en abstrakt vy av hello lagringsarkitekturen i HDInsight för att använda Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="4a08d-139">hello following diagram provides an abstract view of hello HDInsight storage architecture of using Azure Storage:</span></span>

<span data-ttu-id="4a08d-140">![Hadoop-kluster använder hello HDFS API tooaccess och lagra strukturerade och Ostrukturerade data i Blob storage. ] (./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Lagringsarkitekturen i HDInsight")</span><span class="sxs-lookup"><span data-stu-id="4a08d-140">![Hadoop clusters use hello HDFS API tooaccess and store structured and unstructured data in Blob storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")</span></span>

<span data-ttu-id="4a08d-141">HDInsight ger åtkomst toohello distribuerade filsystemet som är lokalt anslutna toohello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4a08d-141">HDInsight provides access toohello distributed file system that is locally attached toohello compute nodes.</span></span> <span data-ttu-id="4a08d-142">Detta filsystem kan nås med hjälp av hello fullständigt kvalificerade URI, till exempel:</span><span class="sxs-lookup"><span data-stu-id="4a08d-142">This file system can be accessed by using hello fully qualified URI, for example:</span></span>

    hdfs://<namenodehost>/<path>

<span data-ttu-id="4a08d-143">HDInsight kan dessutom tooaccess data som lagras i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="4a08d-143">In addition, HDInsight allows you tooaccess data that is stored in Azure Storage.</span></span> <span data-ttu-id="4a08d-144">hello-syntaxen är:</span><span class="sxs-lookup"><span data-stu-id="4a08d-144">hello syntax is:</span></span>

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

<span data-ttu-id="4a08d-145">Här är några saker att tänka på när du använder Azure Storage-konton med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4a08d-145">Here are some considerations when using Azure Storage account with HDInsight clusters.</span></span>

* <span data-ttu-id="4a08d-146">**Behållare på hello lagringskonton som är anslutna tooa kluster:** eftersom hello kontonamnet och nyckeln associeras med hello kluster under skapande, har du fullständig åtkomst toohello blobarna i dessa behållare.</span><span class="sxs-lookup"><span data-stu-id="4a08d-146">**Containers in hello storage accounts that are connected tooa cluster:** Because hello account name and key are associated with hello cluster during creation, you have full access toohello blobs in those containers.</span></span>

* <span data-ttu-id="4a08d-147">**Offentliga behållare eller offentliga blobar på lagringskonton som inte är ansluten tooa kluster:** du har läsbehörighet toohello blobarna i hello behållare.</span><span class="sxs-lookup"><span data-stu-id="4a08d-147">**Public containers or public blobs in storage accounts that are NOT connected tooa cluster:** You have read-only permission toohello blobs in hello containers.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="4a08d-148">Offentliga behållare låter dig tooget en lista över alla blobar som är tillgängliga i behållaren samt hämta metadata för behållaren.</span><span class="sxs-lookup"><span data-stu-id="4a08d-148">Public containers allow you tooget a list of all blobs that are available in that container and get container metadata.</span></span> <span data-ttu-id="4a08d-149">Offentliga blobar tooaccess hello blobbar endast om du vet hello exakta URL.</span><span class="sxs-lookup"><span data-stu-id="4a08d-149">Public blobs allow you tooaccess hello blobs only if you know hello exact URL.</span></span> <span data-ttu-id="4a08d-150">Mer information finns i <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">begränsa åtkomst toocontainers och blobbar</a>.</span><span class="sxs-lookup"><span data-stu-id="4a08d-150">For more information, see <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access toocontainers and blobs</a>.</span></span>
  > 
  > 
* <span data-ttu-id="4a08d-151">**Privata behållare på lagringskonton som inte är ansluten tooa kluster:** hello blobarna i hello behållare kan inte komma åt såvida du inte definierar hello storage-konto när du skickar hello WebHCat-jobb.</span><span class="sxs-lookup"><span data-stu-id="4a08d-151">**Private containers in storage accounts that are NOT connected tooa cluster:** You can't access hello blobs in hello containers unless you define hello storage account when you submit hello WebHCat jobs.</span></span> <span data-ttu-id="4a08d-152">Detta beskrivs senare i artikeln.</span><span class="sxs-lookup"><span data-stu-id="4a08d-152">This is explained later in this article.</span></span>

<span data-ttu-id="4a08d-153">Hej lagringskonton som definieras i hello skapandeprocessen och deras nycklar lagras i %HADOOP_HOME%/conf/core-site.xml i klusternoderna hello.</span><span class="sxs-lookup"><span data-stu-id="4a08d-153">hello storage accounts that are defined in hello creation process and their keys are stored in %HADOOP_HOME%/conf/core-site.xml on hello cluster nodes.</span></span> <span data-ttu-id="4a08d-154">hello standardbeteendet för HDInsight är toouse hello lagringskonton som definieras i hello core-site.xml-filen.</span><span class="sxs-lookup"><span data-stu-id="4a08d-154">hello default behavior of HDInsight is toouse hello storage accounts defined in hello core-site.xml file.</span></span> <span data-ttu-id="4a08d-155">Du kan ändra den här inställningen med [Ambari](./hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="4a08d-155">You can modify this setting using [Ambari](./hdinsight-hadoop-manage-ambari.md)</span></span>

<span data-ttu-id="4a08d-156">Flera WebHCat-jobb, inklusive Hive, MapReduce, Hadoop-strömning och Pig, kan ha med sig en beskrivning av lagringskonton och metadata.</span><span class="sxs-lookup"><span data-stu-id="4a08d-156">Multiple WebHCat jobs, including Hive, MapReduce, Hadoop streaming, and Pig, can carry a description of storage accounts and metadata with them.</span></span> <span data-ttu-id="4a08d-157">(För närvarande fungerar för Pig med lagringskonton, men inte för metadata.) Mer information finns i [Använda ett HDInsight-kluster med alternativa lagringskonton och metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a08d-157">(This currently works for Pig with storage accounts, but not for metadata.) For more information, see [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span></span>

<span data-ttu-id="4a08d-158">Blobar kan användas för strukturerade och ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="4a08d-158">Blobs can be used for structured and unstructured data.</span></span> <span data-ttu-id="4a08d-159">I blob-behållare förvaras data som nyckel/värde-par och det finns ingen kataloghierarki.</span><span class="sxs-lookup"><span data-stu-id="4a08d-159">Blob containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="4a08d-160">Men hello snedstreck (/) kan användas i hello nyckelnamn toomake visas den som om en fil lagras i en katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="4a08d-160">However hello slash character ( / ) can be used within hello key name toomake it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="4a08d-161">Nyckeln för en blob kan till exempel vara *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="4a08d-161">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="4a08d-162">Ingen faktisk *inkommande* katalog finns, men på grund av toohello förekomst av hello snedstreck i nyckelnamnet hello, har hello utseendet på en sökväg till filen.</span><span class="sxs-lookup"><span data-stu-id="4a08d-162">No actual *input* directory exists, but due toohello presence of hello slash character in hello key name, it has hello appearance of a file path.</span></span>

## <span data-ttu-id="4a08d-163"><a id="benefits"></a>Fördelar med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="4a08d-163"><a id="benefits"></a>Benefits of Azure Storage</span></span>
<span data-ttu-id="4a08d-164">hello underförstådd hello sätt hello beräkningsklustren skapas nära toohello lagringskontoresurserna i hello Azure-regionen där hello höghastighetsnätverket gör att hindra kostnaden för inte samordnar beräkningskluster och lagringsresurser effektivt hello datornoderna tooaccess hello data i Azure storage.</span><span class="sxs-lookup"><span data-stu-id="4a08d-164">hello implied performance cost of not co-locating compute clusters and storage resources is mitigated by hello way hello compute clusters are created close toohello storage account resources inside hello Azure region, where hello high-speed network makes it efficient for hello compute nodes tooaccess hello data inside Azure storage.</span></span>

<span data-ttu-id="4a08d-165">Det finns flera fördelar med att lagra hello data i Azure storage i stället för HDFS:</span><span class="sxs-lookup"><span data-stu-id="4a08d-165">There are several benefits associated with storing hello data in Azure storage instead of HDFS:</span></span>

* <span data-ttu-id="4a08d-166">**Data återanvändning och delning:** hello data i HDFS finns inuti hello beräkningskluster.</span><span class="sxs-lookup"><span data-stu-id="4a08d-166">**Data reuse and sharing:** hello data in HDFS is located inside hello compute cluster.</span></span> <span data-ttu-id="4a08d-167">Endast hello-program som har åtkomst toohello beräkna klustret kan använda hello data med hjälp av HDFS-API: er.</span><span class="sxs-lookup"><span data-stu-id="4a08d-167">Only hello applications that have access toohello compute cluster can use hello data by using HDFS APIs.</span></span> <span data-ttu-id="4a08d-168">hello data i Azure storage kan nås via hello HDFS-API: er eller hello [Blob Storage REST API: er][blob-storage-restAPI].</span><span class="sxs-lookup"><span data-stu-id="4a08d-168">hello data in Azure storage can be accessed either through hello HDFS APIs or through hello [Blob Storage REST APIs][blob-storage-restAPI].</span></span> <span data-ttu-id="4a08d-169">Därför kan en större grupp program (inklusive andra HDInsight-kluster) och verktyg att använda tooproduce och använda hello data.</span><span class="sxs-lookup"><span data-stu-id="4a08d-169">Thus, a larger set of applications (including other HDInsight clusters) and tools can be used tooproduce and consume hello data.</span></span>
* <span data-ttu-id="4a08d-170">**Dataarkivering:** lagra data i Azure-lagring kan hello HDInsight-kluster som används för beräkning toobe tas bort utan att förlora användardata.</span><span class="sxs-lookup"><span data-stu-id="4a08d-170">**Data archiving:** Storing data in Azure storage enables hello HDInsight clusters used for computation toobe safely deleted without losing user data.</span></span>
* <span data-ttu-id="4a08d-171">**Kostnad för datalagring:** lagra data i DFS för hello långsiktiga är dyrare än att lagra hello data i Azure-lagring eftersom hello kostnaden för ett beräkningskluster är högre än hello kostnaden för Azure storage.</span><span class="sxs-lookup"><span data-stu-id="4a08d-171">**Data storage cost:** Storing data in DFS for hello long term is more costly than storing hello data in Azure storage because hello cost of a compute cluster is higher than hello cost of Azure storage.</span></span> <span data-ttu-id="4a08d-172">Dessutom hello data har inte lästs in på nytt för varje generation av beräkningskluster toobe, sparar du kostnader för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="4a08d-172">In addition, because hello data does not have toobe reloaded for every compute cluster generation, you are also saving data loading costs.</span></span>
* <span data-ttu-id="4a08d-173">**Elastisk utskalning:** även om HDFS ger dig ett utskalat filsystem, hello skala bestäms av hello antalet noder som du skapar för klustret.</span><span class="sxs-lookup"><span data-stu-id="4a08d-173">**Elastic scale-out:** Although HDFS provides you with a scaled-out file system, hello scale is determined by hello number of nodes that you create for your cluster.</span></span> <span data-ttu-id="4a08d-174">Ändra hello skala kan bli en mer komplicerad process än att använda hello elastiska skalningsfunktionerna som du automatiskt i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="4a08d-174">Changing hello scale can become a more complicated process than relying on hello elastic scaling capabilities that you get automatically in Azure storage.</span></span>
* <span data-ttu-id="4a08d-175">**Geo-replikering:** Din Azure Storage kan geo-replikeras.</span><span class="sxs-lookup"><span data-stu-id="4a08d-175">**Geo-replication:** Your Azure storage can be geo-replicated.</span></span> <span data-ttu-id="4a08d-176">Även om detta ger dig geografisk återställning och dataredundans, en växling vid fel toohello geo-replikerade platsen en avsevärd försämring av prestanda och det kan innebära ytterligare kostnader.</span><span class="sxs-lookup"><span data-stu-id="4a08d-176">Although this gives you geographic recovery and data redundancy, a failover toohello geo-replicated location severely impacts your performance, and it may incur additional costs.</span></span> <span data-ttu-id="4a08d-177">Vår rekommendation är klokt toochoose hello geo-replikering och bara om hello-värde för hello är värda hello extra kostnad.</span><span class="sxs-lookup"><span data-stu-id="4a08d-177">So our recommendation is toochoose hello geo-replication wisely and only if hello value of hello data is worth hello additional cost.</span></span>

<span data-ttu-id="4a08d-178">Vissa MapReduce-jobb och paket kan skapa mellanresultat som du inte verkligen vill toostore i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="4a08d-178">Certain MapReduce jobs and packages may create intermediate results that you don't really want toostore in Azure storage.</span></span> <span data-ttu-id="4a08d-179">I den fallet kan du välja toostore hello data i hello lokala HDFS.</span><span class="sxs-lookup"><span data-stu-id="4a08d-179">In that case, you can elect toostore hello data in hello local HDFS.</span></span> <span data-ttu-id="4a08d-180">Faktum är att HDInsight använder DFS för flera av dessa mellanresultat i Hive-jobb och andra processer.</span><span class="sxs-lookup"><span data-stu-id="4a08d-180">In fact, HDInsight uses DFS for several of these intermediate results in Hive jobs and other processes.</span></span>

> [!NOTE]
> <span data-ttu-id="4a08d-181">De flesta HDFS-kommandon (till exempel <b>ls</b>, <b>copyFromLocal</b> och <b>mkdir</b>) fungerar fortfarande som förväntat.</span><span class="sxs-lookup"><span data-stu-id="4a08d-181">Most HDFS commands (for example, <b>ls</b>, <b>copyFromLocal</b> and <b>mkdir</b>) still work as expected.</span></span> <span data-ttu-id="4a08d-182">Endast hello kommandon som är specifika toohello interna HDFS-implementeringen (vilket är refererad tooas DFS), till exempel <b>fschk</b> och <b>dfsadmin</b>, visa olika beteenden i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="4a08d-182">Only hello commands that are specific toohello native HDFS implementation (which is referred tooas DFS), such as <b>fschk</b> and <b>dfsadmin</b>, show different behavior in Azure storage.</span></span>
> 
> 

## <a name="create-blob-containers"></a><span data-ttu-id="4a08d-183">Skapa blob-behållare</span><span class="sxs-lookup"><span data-stu-id="4a08d-183">Create Blob containers</span></span>
<span data-ttu-id="4a08d-184">toouse blobbar du först skapa en [Azure Storage-konto][azure-storage-create].</span><span class="sxs-lookup"><span data-stu-id="4a08d-184">toouse blobs, you first create an [Azure Storage account][azure-storage-create].</span></span> <span data-ttu-id="4a08d-185">Som en del av det här kan ange du en Azure-region där hello storage-konto har skapats.</span><span class="sxs-lookup"><span data-stu-id="4a08d-185">As part of this, you specify an Azure region where hello storage account is created.</span></span> <span data-ttu-id="4a08d-186">hello-kluster och hello storage-kontot måste finnas i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="4a08d-186">hello cluster and hello storage account must be hosted in hello same region.</span></span> <span data-ttu-id="4a08d-187">hello Hive metastore SQL Server-databas och Oozie metastore SQL Server-databas måste även finnas i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="4a08d-187">hello Hive metastore SQL Server database and Oozie metastore SQL Server database must also be located in hello same region.</span></span>

<span data-ttu-id="4a08d-188">Oavsett var den finns tillhör varje blob som du skapar tooa behållare i Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4a08d-188">Wherever it lives, each blob you create belongs tooa container in your Azure Storage account.</span></span> <span data-ttu-id="4a08d-189">Den här behållaren kan vara en befintlig blob som skapats utanför HDInsight eller en behållare som skapats för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4a08d-189">This container may be an existing blob that was created outside of HDInsight, or it may be a container that is created for an HDInsight cluster.</span></span>

<span data-ttu-id="4a08d-190">hello standardbehållaren lagrar klusterspecifika information, till exempel jobbhistorik och loggar.</span><span class="sxs-lookup"><span data-stu-id="4a08d-190">hello default Blob container stores cluster-specific information such as job history and logs.</span></span> <span data-ttu-id="4a08d-191">Låt inte flera HDInsight-kluster dela en standardblob-behållare.</span><span class="sxs-lookup"><span data-stu-id="4a08d-191">Don't share a default Blob container with multiple HDInsight clusters.</span></span> <span data-ttu-id="4a08d-192">Detta kan skada jobbets historik.</span><span class="sxs-lookup"><span data-stu-id="4a08d-192">This might corrupt job history.</span></span> <span data-ttu-id="4a08d-193">Det rekommenderas toouse en annan behållare för varje kluster och publicera delade data på ett konto för länkad lagring som anges i distribution av alla relevanta kluster snarare än hello standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="4a08d-193">It is recommended toouse a different container for each cluster and put shared data on a linked storage account specified in deployment of all relevant clusters rather than hello default storage account.</span></span> <span data-ttu-id="4a08d-194">Mer information om hur du konfigurerar länkade lagringskonton finns i [Skapa HDInsight-kluster][hdinsight-creation].</span><span class="sxs-lookup"><span data-stu-id="4a08d-194">For more information on configuring linked storage accounts, see [Create HDInsight clusters][hdinsight-creation].</span></span> <span data-ttu-id="4a08d-195">Du kan emellertid återanvända en standardbehållare för lagring när hello ursprungliga HDInsight-klustret har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="4a08d-195">However you can reuse a default storage container after hello original HDInsight cluster has been deleted.</span></span> <span data-ttu-id="4a08d-196">För HBase-kluster kan du faktiskt behålla hello HBase-tabellschemat och data genom att skapa en ny HBase-kluster med hjälp av hello standardbehållaren som används av ett HBase-kluster som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="4a08d-196">For HBase clusters, you can actually retain hello HBase table schema and data by creating a new HBase cluster using hello default blob container that is used by an HBase cluster that has been deleted.</span></span>

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-hello-azure-portal"></a><span data-ttu-id="4a08d-197">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4a08d-197">Use hello Azure portal</span></span>
<span data-ttu-id="4a08d-198">När du skapar ett HDInsight-kluster från hello Portal kan ha hello alternativ (som visas nedan) tooprovide hello lagringskontouppgifter.</span><span class="sxs-lookup"><span data-stu-id="4a08d-198">When creating an HDInsight cluster from hello Portal, you have hello options (as shown below) tooprovide hello storage account details.</span></span> <span data-ttu-id="4a08d-199">Du kan också ange om du vill att ett konto för ytterligare lagringsutrymme som associerats med hello kluster och i så fall, Välj Data Lake Store eller en annan Azure Storage blob som hello ytterligare lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="4a08d-199">You can also specify whether you want an additional storage account associated with hello cluster, and if so, choose from Data Lake Store or another Azure Storage blob as hello additional storage.</span></span>

![HDInsight hadoop, skapa datakälla](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> <span data-ttu-id="4a08d-201">Med ett konto för ytterligare lagringsutrymme på en annan plats än hello HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="4a08d-201">Using an additional storage account in a different location than hello HDInsight cluster is not supported.</span></span>


### <a name="use-azure-powershell"></a><span data-ttu-id="4a08d-202">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a08d-202">Use Azure PowerShell</span></span>
<span data-ttu-id="4a08d-203">Om du [installerat och konfigurerat Azure PowerShell][powershell-install], du kan använda hello följande från hello Azure PowerShell-prompt toocreate ett lagringskonto och en behållare:</span><span class="sxs-lookup"><span data-stu-id="4a08d-203">If you [installed and configured Azure PowerShell][powershell-install], you can use hello following from hello Azure PowerShell prompt toocreate a storage account and container:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="use-azure-cli"></a><span data-ttu-id="4a08d-204">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4a08d-204">Use Azure CLI</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

<span data-ttu-id="4a08d-205">Om du har [installerat och konfigurerat hello Azure CLI](../cli-install-nodejs.md), hello följande kommando kan vara används tooa storage-konto och en behållare.</span><span class="sxs-lookup"><span data-stu-id="4a08d-205">If you have [installed and configured hello Azure CLI](../cli-install-nodejs.md), hello following command can be used tooa storage account and container.</span></span>

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> <span data-ttu-id="4a08d-206">Hej `--type` parameter anger hur hello storage-konto replikeras.</span><span class="sxs-lookup"><span data-stu-id="4a08d-206">hello `--type` parameter indicates how hello storage account is replicated.</span></span> <span data-ttu-id="4a08d-207">Mer information finns i [Azure Storage-replikering](../storage/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="4a08d-207">For more information, see [Azure Storage Replication](../storage/storage-redundancy.md).</span></span> <span data-ttu-id="4a08d-208">Använd inte ZRS eftersom ZRS inte stöder sidblobbar, filer, tabeller eller köer.</span><span class="sxs-lookup"><span data-stu-id="4a08d-208">Don't use ZRS as ZRS doesn't support page blob, file, table, or queue.</span></span>
> 
> 

<span data-ttu-id="4a08d-209">Du kan ange toospecify hello geografiska region som hello lagringskonto skapas i.</span><span class="sxs-lookup"><span data-stu-id="4a08d-209">You are prompted toospecify hello geographic region that hello storage account is created in.</span></span> <span data-ttu-id="4a08d-210">Du bör skapa hello storage-konto i hello samma region som du planerar att skapa ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4a08d-210">You should create hello storage account in hello same region that you plan on creating your HDInsight cluster.</span></span>

<span data-ttu-id="4a08d-211">När hello storage-konto har skapats använder du följande kommando tooretrieve hello lagringskontonycklar hello:</span><span class="sxs-lookup"><span data-stu-id="4a08d-211">Once hello storage account is created, use hello following command tooretrieve hello storage account keys:</span></span>

    azure storage account keys list <storageaccountname>

<span data-ttu-id="4a08d-212">toocreate en behållare använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4a08d-212">toocreate a container, use hello following command:</span></span>

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a><span data-ttu-id="4a08d-213">Adressera filer i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="4a08d-213">Address files in Azure storage</span></span>
<span data-ttu-id="4a08d-214">hello URI-schemat för att komma åt filer i Azure storage från HDInsight är:</span><span class="sxs-lookup"><span data-stu-id="4a08d-214">hello URI scheme for accessing files in Azure storage from HDInsight is:</span></span>

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

<span data-ttu-id="4a08d-215">hello URI-schemat ger okrypterad åtkomst (med hello *wasb:* prefix) och SSL-krypterad åtkomst (med *wasbs*).</span><span class="sxs-lookup"><span data-stu-id="4a08d-215">hello URI scheme provides unencrypted access (with hello *wasb:* prefix) and SSL encrypted access (with *wasbs*).</span></span> <span data-ttu-id="4a08d-216">Vi rekommenderar att du använder *wasbs* där det är möjligt, även om det inte att komma åt data som finns i hello samma region i Azure.</span><span class="sxs-lookup"><span data-stu-id="4a08d-216">We recommend using *wasbs* wherever possible, even when accessing data that lives inside hello same region in Azure.</span></span>

<span data-ttu-id="4a08d-217">Hej &lt;BlobStorageContainerName&gt; identifierar hello namnet på hello blob-behållaren i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="4a08d-217">hello &lt;BlobStorageContainerName&gt; identifies hello name of hello blob container in Azure storage.</span></span>
<span data-ttu-id="4a08d-218">Hej &lt;StorageAccountName&gt; identifierar hello Azure Storage-kontonamnet.</span><span class="sxs-lookup"><span data-stu-id="4a08d-218">hello &lt;StorageAccountName&gt; identifies hello Azure Storage account name.</span></span> <span data-ttu-id="4a08d-219">Ett fullständigt kvalificerat domännamn (FQDN) krävs.</span><span class="sxs-lookup"><span data-stu-id="4a08d-219">A fully qualified domain name (FQDN) is required.</span></span>

<span data-ttu-id="4a08d-220">Om varken &lt;BlobStorageContainerName&gt; eller &lt;StorageAccountName&gt; har angetts används hello standardfilsystem.</span><span class="sxs-lookup"><span data-stu-id="4a08d-220">If neither &lt;BlobStorageContainerName&gt; nor &lt;StorageAccountName&gt; has been specified, hello default file system is used.</span></span> <span data-ttu-id="4a08d-221">För hello filer i filsystemet för hello standard kan använda du en relativ sökväg eller en absolut sökväg.</span><span class="sxs-lookup"><span data-stu-id="4a08d-221">For hello files on hello default file system, you can use a relative path or an absolute path.</span></span> <span data-ttu-id="4a08d-222">Till exempel hello *hadoop-mapreduce-examples.jar* fil som medföljer HDInsight-kluster kan vara hänvisade tooby med hjälp av en av följande hello:</span><span class="sxs-lookup"><span data-stu-id="4a08d-222">For example, hello *hadoop-mapreduce-examples.jar* file that comes with HDInsight clusters can be referred tooby using one of hello following:</span></span>

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="4a08d-223">hello-filnamnet är <i>hadoop examples.jar</i> i HDInsight-kluster av version 2.1 och 1.6.</span><span class="sxs-lookup"><span data-stu-id="4a08d-223">hello file name is <i>hadoop-examples.jar</i> in HDInsight versions 2.1 and 1.6 clusters.</span></span>
> 
> 

<span data-ttu-id="4a08d-224">Hej &lt;sökväg&gt; är hello filen eller katalogen HDFS-sökvägsnamn.</span><span class="sxs-lookup"><span data-stu-id="4a08d-224">hello &lt;path&gt; is hello file or directory HDFS path name.</span></span> <span data-ttu-id="4a08d-225">Eftersom behållare i Azure Storage helt enkelt är lagringsplatser för nyckel-/värdepar finns det inte något faktiskt hierarkiskt filsystem.</span><span class="sxs-lookup"><span data-stu-id="4a08d-225">Because containers in Azure storage are simply key-value stores, there is no true hierarchical file system.</span></span> <span data-ttu-id="4a08d-226">Ett snedstreck (/) i en blob-nyckel tolkas som en katalogavgränsare.</span><span class="sxs-lookup"><span data-stu-id="4a08d-226">A slash character ( / ) inside a blob key is interpreted as a directory separator.</span></span> <span data-ttu-id="4a08d-227">Till exempel hello blob-namnet för *hadoop-mapreduce-examples.jar* är:</span><span class="sxs-lookup"><span data-stu-id="4a08d-227">For example, hello blob name for *hadoop-mapreduce-examples.jar* is:</span></span>

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="4a08d-228">När du arbetar med blobar utanför HDInsight kan de flesta verktyg inte identifiera WASB-formatet hello och förväntar sig i stället ett grundläggande sökvägsformat som `example/jars/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="4a08d-228">When working with blobs outside of HDInsight, most utilities do not recognize hello WASB format and instead expect a basic path format, such as `example/jars/hadoop-mapreduce-examples.jar`.</span></span>
> 
> 

## <a name="access-blobs"></a><span data-ttu-id="4a08d-229">Få åtkomst till blobbar</span><span class="sxs-lookup"><span data-stu-id="4a08d-229">Access blobs</span></span> 


### <span data-ttu-id="4a08d-230"><a name="access-blobs-using-azure-powershell"></a>Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a08d-230"><a name="access-blobs-using-azure-powershell"></a> Use Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="4a08d-231">hello kommandona i det här avsnittet ger grundläggande exempel på hur du använder PowerShell tooaccess data som lagras i BLOB.</span><span class="sxs-lookup"><span data-stu-id="4a08d-231">hello commands in this section provide a basic example of using PowerShell tooaccess data stored in blobs.</span></span> <span data-ttu-id="4a08d-232">En mer komplett exempel som är anpassad för att arbeta med HDInsight finns hello [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="4a08d-232">For a more full-featured example that is customized for working with HDInsight, see hello [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span></span>
> 
> 

<span data-ttu-id="4a08d-233">Använd följande kommando toolist hello blob-relaterade cmdlets hello:</span><span class="sxs-lookup"><span data-stu-id="4a08d-233">Use hello following command toolist hello blob-related cmdlets:</span></span>

    Get-Command *blob*

![Lista över blob-relaterade PowerShell-cmdlets.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a><span data-ttu-id="4a08d-235">Överföra filer</span><span class="sxs-lookup"><span data-stu-id="4a08d-235">Upload files</span></span>
<span data-ttu-id="4a08d-236">Se [överför data tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="4a08d-236">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

#### <a name="download-files"></a><span data-ttu-id="4a08d-237">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="4a08d-237">Download files</span></span>
<span data-ttu-id="4a08d-238">hello hämtar följande skript en block-blob toohello aktuella mappen.</span><span class="sxs-lookup"><span data-stu-id="4a08d-238">hello following script downloads a block blob toohello current folder.</span></span> <span data-ttu-id="4a08d-239">Ändra hello directory tooa mapp där du har behörighet att skriva innan köra hello skriptet.</span><span class="sxs-lookup"><span data-stu-id="4a08d-239">Before running hello script, change hello directory tooa folder where you have write permissions.</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # hello storage account used for hello default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # hello default file system container has hello same name as hello cluster.
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    # Use Add-AzureAccount if you haven't connected tooyour Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List hello downloaded file ..." -ForegroundColor Green
    cat "./$blob"

<span data-ttu-id="4a08d-240">Att erbjuda hello resursgruppens namn och hello klusternamnet, kan du använda hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="4a08d-240">Providing hello resource group name and hello cluster name, you can use hello following code:</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a><span data-ttu-id="4a08d-241">Ta bort filer</span><span class="sxs-lookup"><span data-stu-id="4a08d-241">Delete files</span></span>
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a><span data-ttu-id="4a08d-242">Lista filer</span><span class="sxs-lookup"><span data-stu-id="4a08d-242">List files</span></span>
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a><span data-ttu-id="4a08d-243">Köra Hive-frågor med ett odefinierat lagringskonto</span><span class="sxs-lookup"><span data-stu-id="4a08d-243">Run Hive queries using an undefined storage account</span></span>
<span data-ttu-id="4a08d-244">Det här exemplet illustrerar hur hello toolist en mapp från lagringskontot som inte har definierats under skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="4a08d-244">This example shows how toolist a folder from storage account that is not defined during hello creating process.</span></span>
<span data-ttu-id="4a08d-245">$clusterName = "<HDInsightClusterName>"</span><span class="sxs-lookup"><span data-stu-id="4a08d-245">$clusterName = "<HDInsightClusterName>"</span></span>

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a><span data-ttu-id="4a08d-246">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4a08d-246">Use Azure CLI</span></span>
<span data-ttu-id="4a08d-247">Använd följande kommando toolist hello blob-relaterade kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="4a08d-247">Use hello following command toolist hello blob-related commands:</span></span>

    azure storage blob

<span data-ttu-id="4a08d-248">**Exempel på användning av Azure CLI tooupload en fil**</span><span class="sxs-lookup"><span data-stu-id="4a08d-248">**Example of using Azure CLI tooupload a file**</span></span>

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="4a08d-249">**Exempel på användning av Azure CLI toodownload en fil**</span><span class="sxs-lookup"><span data-stu-id="4a08d-249">**Example of using Azure CLI toodownload a file**</span></span>

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="4a08d-250">**Exempel på användning av Azure CLI toodelete en fil**</span><span class="sxs-lookup"><span data-stu-id="4a08d-250">**Example of using Azure CLI toodelete a file**</span></span>

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="4a08d-251">**Exempel på användning av Azure CLI toolist filer**</span><span class="sxs-lookup"><span data-stu-id="4a08d-251">**Example of using Azure CLI toolist files**</span></span>

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a><span data-ttu-id="4a08d-252">Använda ytterligare lagringskonton</span><span class="sxs-lookup"><span data-stu-id="4a08d-252">Use additional storage accounts</span></span>

<span data-ttu-id="4a08d-253">När du skapar ett HDInsight-kluster måste ange du hello Azure Storage-konto som du vill tooassociate med den.</span><span class="sxs-lookup"><span data-stu-id="4a08d-253">While creating an HDInsight cluster, you specify hello Azure Storage account you want tooassociate with it.</span></span> <span data-ttu-id="4a08d-254">Dessutom toothis storage-konto, du kan lägga till ytterligare lagringskonton från hello samma Azure-prenumeration eller andra Azure-prenumerationer under skapandeprocessen hello eller ett kluster har skapats.</span><span class="sxs-lookup"><span data-stu-id="4a08d-254">In addition toothis storage account, you can add additional storage accounts from hello same Azure subscription or different Azure subscriptions during hello creation process or after a cluster has been created.</span></span> <span data-ttu-id="4a08d-255">Mer information om hur du lägger till ytterligare lagringskonton finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="4a08d-255">For instructions about adding additional storage accounts, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!WARNING]
> <span data-ttu-id="4a08d-256">Med ett konto för ytterligare lagringsutrymme på en annan plats än hello HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="4a08d-256">Using an additional storage account in a different location than hello HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a08d-257">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a08d-257">Next steps</span></span>
<span data-ttu-id="4a08d-258">I den här artikeln har du lärt dig hur toouse HDFS-kompatibla Azure storage med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4a08d-258">In this article, you learned how toouse HDFS-compatible Azure storage with HDInsight.</span></span> <span data-ttu-id="4a08d-259">Detta gör att du toobuild skalbara, långsiktiga, arkivering lösningar för datainsamling och använda HDInsight toounlock hello information i hello lagrade strukturerade och Ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="4a08d-259">This allows you toobuild scalable, long-term, archiving data acquisition solutions and use HDInsight toounlock hello information inside hello stored structured and unstructured data.</span></span>

<span data-ttu-id="4a08d-260">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="4a08d-260">For more information, see:</span></span>

* <span data-ttu-id="4a08d-261">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="4a08d-261">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="4a08d-262">Kom igång med Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4a08d-262">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="4a08d-263">[Ladda upp data tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="4a08d-263">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="4a08d-264">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="4a08d-264">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="4a08d-265">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="4a08d-265">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="4a08d-266">[Använda Azure Storage signaturer för delad åtkomst toorestrict åtkomst toodata med HDInsight][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="4a08d-266">[Use Azure Storage Shared Access Signatures toorestrict access toodata with HDInsight][hdinsight-use-sas]</span></span>

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
