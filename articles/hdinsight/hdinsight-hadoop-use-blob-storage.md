---
title: "Fråga efter data i HDFS-kompatibelt Azure-lagringsutrymme - Azure HDInsight | Microsoft Docs"
description: "Lär dig mer om hur du frågar efter data från Azure Storage och Azure Data Lake Store för att lagra resultatet av dina analyser."
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
ms.openlocfilehash: a44c2b363f7ebb593b9a9c5bd9e0d4fc3b4c31bb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a><span data-ttu-id="d44b3-104">Använda Azure-lagring med Azure HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="d44b3-104">Use Azure storage with Azure HDInsight clusters</span></span>

<span data-ttu-id="d44b3-105">För att analysera data i HDInsight-klustret kan du lagra data i antingen Azure Storage, Azure Data Lake Store eller bådadera.</span><span class="sxs-lookup"><span data-stu-id="d44b3-105">To analyze data in HDInsight cluster, you can store the data either in Azure Storage, Azure Data Lake Store, or both.</span></span> <span data-ttu-id="d44b3-106">Båda lagringsalternativen låter dig ta bort HDInsight-kluster som används för beräkning utan att förlora användardata.</span><span class="sxs-lookup"><span data-stu-id="d44b3-106">Both storage options enable you to safely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="d44b3-107">Hadoop stöder begreppet standardfilsystem.</span><span class="sxs-lookup"><span data-stu-id="d44b3-107">Hadoop supports a notion of the default file system.</span></span> <span data-ttu-id="d44b3-108">Standardfilsystemet kräver att ett standardschema och en utfärdare används.</span><span class="sxs-lookup"><span data-stu-id="d44b3-108">The default file system implies a default scheme and authority.</span></span> <span data-ttu-id="d44b3-109">Det kan också användas för att matcha relativa sökvägar.</span><span class="sxs-lookup"><span data-stu-id="d44b3-109">It can also be used to resolve relative paths.</span></span> <span data-ttu-id="d44b3-110">Du kan ange en blobbehållare i Azure Storage som standardfilsystemet när du skapar HDInsight-kluster. Med HDInsight 3.5 kan du välja antingen Azure Storage eller Azure Data Lake Store som standardfilsystem med ett fåtal undantag.</span><span class="sxs-lookup"><span data-stu-id="d44b3-110">During the HDInsight cluster creation process, you can specify a blob container in Azure Storage as the default file system, or with HDInsight 3.5, you can select either Azure Storage or Azure Data Lake Store as the default files system with a few exceptions.</span></span> <span data-ttu-id="d44b3-111">Support för att använda Data Lake Store som både standardwebbplatsen och den länkade storage finns [tillgången för HDInsight-kluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="d44b3-111">For the supportability of using Data Lake Store as both the default and linked storage, see [Availabilities for HDInsight cluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span></span>

<span data-ttu-id="d44b3-112">I den här artikeln får du lära dig hur Azure Storage fungerar med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d44b3-112">In this article, you learn how Azure Storage works with HDInsight clusters.</span></span> <span data-ttu-id="d44b3-113">Läs mer om hur Data Lake Store fungerar med HDInsight-kluster i [Använd Azure Data Lake Store med Azure HDInsight-kluster](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="d44b3-113">To learn how Data Lake Store works with HDInsight clusters, see [Use Azure Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> <span data-ttu-id="d44b3-114">Mer information om hur du skapar ett HDInsight-kluster finns i [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Skapa Hadoop-kluster i HDInsight).</span><span class="sxs-lookup"><span data-stu-id="d44b3-114">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="d44b3-115">Azure Storage är en robust lagringslösning för allmänna ändamål som smidigt kan integreras med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d44b3-115">Azure storage is a robust, general-purpose storage solution that integrates seamlessly with HDInsight.</span></span> <span data-ttu-id="d44b3-116">HDInsight kan använda en blobbehållare i Azure Storage som standardfilsystem för klustret.</span><span class="sxs-lookup"><span data-stu-id="d44b3-116">HDInsight can use a blob container in Azure Storage as the default file system for the cluster.</span></span> <span data-ttu-id="d44b3-117">Genom ett gränssnitt för Hadoop-distribuerat filsystem (HDFS) kan alla komponenter i HDInsight tillämpas direkt på strukturerade eller ostrukturerade data som har lagrats som blobar.</span><span class="sxs-lookup"><span data-stu-id="d44b3-117">Through a Hadoop distributed file system (HDFS) interface, the full set of components in HDInsight can operate directly on structured or unstructured data stored as blobs.</span></span>

> [!WARNING]
> <span data-ttu-id="d44b3-118">Det finns flera tillgängliga alternativ när du skapar ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d44b3-118">There are several options available when creating an Azure Storage account.</span></span> <span data-ttu-id="d44b3-119">I följande tabell finns information om vilka alternativ som stöds med HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d44b3-119">The following table provides information on what options are supported with HDInsight:</span></span>
> 
> | <span data-ttu-id="d44b3-120">Storage Account-typ</span><span class="sxs-lookup"><span data-stu-id="d44b3-120">Storage account type</span></span> | <span data-ttu-id="d44b3-121">Lagringsnivå</span><span class="sxs-lookup"><span data-stu-id="d44b3-121">Storage tier</span></span> | <span data-ttu-id="d44b3-122">Stöds av HDInsight</span><span class="sxs-lookup"><span data-stu-id="d44b3-122">Supported with HDInsight</span></span> |
> | ------- | ------- | ------- |
> | <span data-ttu-id="d44b3-123">Allmänna lagringskonton</span><span class="sxs-lookup"><span data-stu-id="d44b3-123">General-purpose Storage Account</span></span> | <span data-ttu-id="d44b3-124">Standard</span><span class="sxs-lookup"><span data-stu-id="d44b3-124">Standard</span></span> | <span data-ttu-id="d44b3-125">__Ja__</span><span class="sxs-lookup"><span data-stu-id="d44b3-125">__Yes__</span></span> |
> | &nbsp; | <span data-ttu-id="d44b3-126">Premium</span><span class="sxs-lookup"><span data-stu-id="d44b3-126">Premium</span></span> | <span data-ttu-id="d44b3-127">Nej</span><span class="sxs-lookup"><span data-stu-id="d44b3-127">No</span></span> |
> | <span data-ttu-id="d44b3-128">Blob Storage-konto</span><span class="sxs-lookup"><span data-stu-id="d44b3-128">Blob Storage Account</span></span> | <span data-ttu-id="d44b3-129">Frekvent</span><span class="sxs-lookup"><span data-stu-id="d44b3-129">Hot</span></span> | <span data-ttu-id="d44b3-130">Nej</span><span class="sxs-lookup"><span data-stu-id="d44b3-130">No</span></span> |
> | &nbsp; | <span data-ttu-id="d44b3-131">Lågfrekvent</span><span class="sxs-lookup"><span data-stu-id="d44b3-131">Cool</span></span> | <span data-ttu-id="d44b3-132">Nej</span><span class="sxs-lookup"><span data-stu-id="d44b3-132">No</span></span> |

<span data-ttu-id="d44b3-133">Vi rekommenderar att du inte använder standardbehållaren för att lagra företagsdata.</span><span class="sxs-lookup"><span data-stu-id="d44b3-133">We do not recommend that you use the default blob container for storing business data.</span></span> <span data-ttu-id="d44b3-134">Ta bort standardbehållaren efter varje användning för att minska lagringskostnaden.</span><span class="sxs-lookup"><span data-stu-id="d44b3-134">Deleting the default blob container after each use to reduce storage cost is a good practice.</span></span> <span data-ttu-id="d44b3-135">Observera att standardbehållaren innehåller program- och systemloggar.</span><span class="sxs-lookup"><span data-stu-id="d44b3-135">Note that the default container contains application and system logs.</span></span> <span data-ttu-id="d44b3-136">Se till att hämta loggarna innan du tar bort behållaren.</span><span class="sxs-lookup"><span data-stu-id="d44b3-136">Make sure to retrieve the logs before deleting the container.</span></span>

<span data-ttu-id="d44b3-137">Det går inte att dela en blobbehållare över flera kluster.</span><span class="sxs-lookup"><span data-stu-id="d44b3-137">Sharing one blob container for multiple clusters is not supported.</span></span>

## <a name="hdinsight-storage-architecture"></a><span data-ttu-id="d44b3-138">Lagringsarkitekturen i HDInsight</span><span class="sxs-lookup"><span data-stu-id="d44b3-138">HDInsight storage architecture</span></span>
<span data-ttu-id="d44b3-139">Följande diagram visar en abstrakt vy av lagringsarkitekturen i HDInsight med Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="d44b3-139">The following diagram provides an abstract view of the HDInsight storage architecture of using Azure Storage:</span></span>

<span data-ttu-id="d44b3-140">![Hadoop-kluster använder HDFS API för att komma åt och lagra strukturerade och ostrukturerade data i Blob Storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage-arkitektur")</span><span class="sxs-lookup"><span data-stu-id="d44b3-140">![Hadoop clusters use the HDFS API to access and store structured and unstructured data in Blob storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")</span></span>

<span data-ttu-id="d44b3-141">HDInsight ger tillgång till det distribuerade filsystemet som är lokalt anslutet till beräkningsnoderna.</span><span class="sxs-lookup"><span data-stu-id="d44b3-141">HDInsight provides access to the distributed file system that is locally attached to the compute nodes.</span></span> <span data-ttu-id="d44b3-142">Detta filsystem kan nås med hjälp av den fullständigt kvalificerade URI-strängen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="d44b3-142">This file system can be accessed by using the fully qualified URI, for example:</span></span>

    hdfs://<namenodehost>/<path>

<span data-ttu-id="d44b3-143">Dessutom ger HDInsight möjlighet att komma åt data som är lagrade i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d44b3-143">In addition, HDInsight allows you to access data that is stored in Azure Storage.</span></span> <span data-ttu-id="d44b3-144">Syntax:</span><span class="sxs-lookup"><span data-stu-id="d44b3-144">The syntax is:</span></span>

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

<span data-ttu-id="d44b3-145">Här är några saker att tänka på när du använder Azure Storage-konton med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d44b3-145">Here are some considerations when using Azure Storage account with HDInsight clusters.</span></span>

* <span data-ttu-id="d44b3-146">**Behållare på de lagringskonton som är anslutna till ett kluster:** Eftersom kontonamnet och nyckeln associeras med klustret när det skapas har du full tillgång till blobarna i dessa behållare.</span><span class="sxs-lookup"><span data-stu-id="d44b3-146">**Containers in the storage accounts that are connected to a cluster:** Because the account name and key are associated with the cluster during creation, you have full access to the blobs in those containers.</span></span>

* <span data-ttu-id="d44b3-147">**Offentliga behållare eller offentliga blobar på lagringskonton som INTE är anslutna till något kluster:** Du har läsbehörighet till blobarna i dessa behållare.</span><span class="sxs-lookup"><span data-stu-id="d44b3-147">**Public containers or public blobs in storage accounts that are NOT connected to a cluster:** You have read-only permission to the blobs in the containers.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="d44b3-148">Offentliga behållare låter dig hämta en lista över alla blobar som är tillgängliga i behållaren samt hämta metadata för behållaren.</span><span class="sxs-lookup"><span data-stu-id="d44b3-148">Public containers allow you to get a list of all blobs that are available in that container and get container metadata.</span></span> <span data-ttu-id="d44b3-149">Du kan endast komma åt offentliga blobar om du känner till den exakta webbadressen.</span><span class="sxs-lookup"><span data-stu-id="d44b3-149">Public blobs allow you to access the blobs only if you know the exact URL.</span></span> <span data-ttu-id="d44b3-150">Mer information finns i <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Begränsa åtkomsten till behållare och blobar</a>.</span><span class="sxs-lookup"><span data-stu-id="d44b3-150">For more information, see <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access to containers and blobs</a>.</span></span>
  > 
  > 
* <span data-ttu-id="d44b3-151">**Privata behållare på lagringskonton som INTE är anslutna till något kluster:** Du kan inte komma åt blobarna i behållarna om du inte definierar lagringskontot när du skickar WebHCat-jobben.</span><span class="sxs-lookup"><span data-stu-id="d44b3-151">**Private containers in storage accounts that are NOT connected to a cluster:** You can't access the blobs in the containers unless you define the storage account when you submit the WebHCat jobs.</span></span> <span data-ttu-id="d44b3-152">Detta beskrivs senare i artikeln.</span><span class="sxs-lookup"><span data-stu-id="d44b3-152">This is explained later in this article.</span></span>

<span data-ttu-id="d44b3-153">De lagringskonton som definieras under skapandeprocessen och deras nycklar lagras i %HADOOP_HOME%/conf/core-site.xml i klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="d44b3-153">The storage accounts that are defined in the creation process and their keys are stored in %HADOOP_HOME%/conf/core-site.xml on the cluster nodes.</span></span> <span data-ttu-id="d44b3-154">Standardbeteendet för HDInsight är att använda de lagringskonton som definieras i filen core-site.xml.</span><span class="sxs-lookup"><span data-stu-id="d44b3-154">The default behavior of HDInsight is to use the storage accounts defined in the core-site.xml file.</span></span> <span data-ttu-id="d44b3-155">Du kan ändra den här inställningen med [Ambari](./hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="d44b3-155">You can modify this setting using [Ambari](./hdinsight-hadoop-manage-ambari.md)</span></span>

<span data-ttu-id="d44b3-156">Flera WebHCat-jobb, inklusive Hive, MapReduce, Hadoop-strömning och Pig, kan ha med sig en beskrivning av lagringskonton och metadata.</span><span class="sxs-lookup"><span data-stu-id="d44b3-156">Multiple WebHCat jobs, including Hive, MapReduce, Hadoop streaming, and Pig, can carry a description of storage accounts and metadata with them.</span></span> <span data-ttu-id="d44b3-157">(För närvarande fungerar för Pig med lagringskonton, men inte för metadata.) Mer information finns i [Använda ett HDInsight-kluster med alternativa lagringskonton och metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span><span class="sxs-lookup"><span data-stu-id="d44b3-157">(This currently works for Pig with storage accounts, but not for metadata.) For more information, see [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span></span>

<span data-ttu-id="d44b3-158">Blobar kan användas för strukturerade och ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="d44b3-158">Blobs can be used for structured and unstructured data.</span></span> <span data-ttu-id="d44b3-159">I blob-behållare förvaras data som nyckel/värde-par och det finns ingen kataloghierarki.</span><span class="sxs-lookup"><span data-stu-id="d44b3-159">Blob containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="d44b3-160">Däremot kan snedstreck (/) användas i nyckelnamnet så att det visas som om det vore en fil som lagrats i en katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="d44b3-160">However the slash character ( / ) can be used within the key name to make it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="d44b3-161">Nyckeln för en blob kan till exempel vara *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="d44b3-161">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="d44b3-162">Det finns ingen faktisk katalog för *indata*, men eftersom det finns ett snedstreck i nyckelnamnet ser namnet ut som en filsökväg.</span><span class="sxs-lookup"><span data-stu-id="d44b3-162">No actual *input* directory exists, but due to the presence of the slash character in the key name, it has the appearance of a file path.</span></span>

## <span data-ttu-id="d44b3-163"><a id="benefits"></a>Fördelar med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d44b3-163"><a id="benefits"></a>Benefits of Azure Storage</span></span>
<span data-ttu-id="d44b3-164">Den medförande prestandakostnaden för att inte samplacera beräkningskluster och lagringsresurser minskar, på grund av sättet på vilket beräkningsklustren skapas nära lagringskontoresurserna i Azure-regionen, där höghastighetsnätverket gör att beräkningsnoderna effektivt kan komma åt data i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d44b3-164">The implied performance cost of not co-locating compute clusters and storage resources is mitigated by the way the compute clusters are created close to the storage account resources inside the Azure region, where the high-speed network makes it efficient for the compute nodes to access the data inside Azure storage.</span></span>

<span data-ttu-id="d44b3-165">Det finns flera fördelar med att lagra data i Azure Storage i stället för i HDFS:</span><span class="sxs-lookup"><span data-stu-id="d44b3-165">There are several benefits associated with storing the data in Azure storage instead of HDFS:</span></span>

* <span data-ttu-id="d44b3-166">**Återanvändning och delning av data:** Data i HDFS lagras inuti beräkningsklustren.</span><span class="sxs-lookup"><span data-stu-id="d44b3-166">**Data reuse and sharing:** The data in HDFS is located inside the compute cluster.</span></span> <span data-ttu-id="d44b3-167">Endast de program som har åtkomst till beräkningsklustren kan använda dessa data med hjälp av HDFS-API:er.</span><span class="sxs-lookup"><span data-stu-id="d44b3-167">Only the applications that have access to the compute cluster can use the data by using HDFS APIs.</span></span> <span data-ttu-id="d44b3-168">Data i Azure Storage kan antingen nås via HDFS-API:erna eller via [REST-API:erna för Blob Storage][blob-storage-restAPI].</span><span class="sxs-lookup"><span data-stu-id="d44b3-168">The data in Azure storage can be accessed either through the HDFS APIs or through the [Blob Storage REST APIs][blob-storage-restAPI].</span></span> <span data-ttu-id="d44b3-169">Därmed kan en större grupp program (inklusive andra HDInsight-kluster) och verktyg användas för att skapa och använda data.</span><span class="sxs-lookup"><span data-stu-id="d44b3-169">Thus, a larger set of applications (including other HDInsight clusters) and tools can be used to produce and consume the data.</span></span>
* <span data-ttu-id="d44b3-170">**Dataarkivering:** Om data lagras i Azure Storage kan de HDInsight-kluster som används för beräkning tryggt tas bort utan att användardata går förlorade.</span><span class="sxs-lookup"><span data-stu-id="d44b3-170">**Data archiving:** Storing data in Azure storage enables the HDInsight clusters used for computation to be safely deleted without losing user data.</span></span>
* <span data-ttu-id="d44b3-171">**Kostnad för datalagring:** Att lagra data långsiktigt i DFS är dyrare än att lagra data i Azure Storage eftersom kostnaden för ett beräkningskluster är högre än kostnaden för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d44b3-171">**Data storage cost:** Storing data in DFS for the long term is more costly than storing the data in Azure storage because the cost of a compute cluster is higher than the cost of Azure storage.</span></span> <span data-ttu-id="d44b3-172">Eftersom data inte behöver läsas in på nytt för varje generation av beräkningskluster sparar du dessutom kostnader för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="d44b3-172">In addition, because the data does not have to be reloaded for every compute cluster generation, you are also saving data loading costs.</span></span>
* <span data-ttu-id="d44b3-173">**Elastisk utskalning:** Även om HDFS ger dig ett utskalat filsystem bestäms skalan av antalet noder som du skapar för klustret.</span><span class="sxs-lookup"><span data-stu-id="d44b3-173">**Elastic scale-out:** Although HDFS provides you with a scaled-out file system, the scale is determined by the number of nodes that you create for your cluster.</span></span> <span data-ttu-id="d44b3-174">Att ändra skala med HDFS kan vara en mer komplicerad process än att använda de elastiska skalningsfunktionerna som du automatiskt har tillgång till i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d44b3-174">Changing the scale can become a more complicated process than relying on the elastic scaling capabilities that you get automatically in Azure storage.</span></span>
* <span data-ttu-id="d44b3-175">**Geo-replikering:** Din Azure Storage kan geo-replikeras.</span><span class="sxs-lookup"><span data-stu-id="d44b3-175">**Geo-replication:** Your Azure storage can be geo-replicated.</span></span> <span data-ttu-id="d44b3-176">Även om detta ger dig geografisk återställning och dataredundans, innebär en växling till den geo-replikerade platsen en avsevärd försämring av prestandan och kan medföra ytterligare kostnader.</span><span class="sxs-lookup"><span data-stu-id="d44b3-176">Although this gives you geographic recovery and data redundancy, a failover to the geo-replicated location severely impacts your performance, and it may incur additional costs.</span></span> <span data-ttu-id="d44b3-177">Vår rekommendation är därför att tänka efter när du väljer geo-replikering och bara göra det om värdet i informationen motiverar den extra kostnaden.</span><span class="sxs-lookup"><span data-stu-id="d44b3-177">So our recommendation is to choose the geo-replication wisely and only if the value of the data is worth the additional cost.</span></span>

<span data-ttu-id="d44b3-178">Vissa MapReduce-jobb och -paket kan skapa mellanresultat som du inte egentligen vill lagra i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d44b3-178">Certain MapReduce jobs and packages may create intermediate results that you don't really want to store in Azure storage.</span></span> <span data-ttu-id="d44b3-179">I så fall kan du välja att lagra data i ditt lokala HDFS.</span><span class="sxs-lookup"><span data-stu-id="d44b3-179">In that case, you can elect to store the data in the local HDFS.</span></span> <span data-ttu-id="d44b3-180">Faktum är att HDInsight använder DFS för flera av dessa mellanresultat i Hive-jobb och andra processer.</span><span class="sxs-lookup"><span data-stu-id="d44b3-180">In fact, HDInsight uses DFS for several of these intermediate results in Hive jobs and other processes.</span></span>

> [!NOTE]
> <span data-ttu-id="d44b3-181">De flesta HDFS-kommandon (till exempel <b>ls</b>, <b>copyFromLocal</b> och <b>mkdir</b>) fungerar fortfarande som förväntat.</span><span class="sxs-lookup"><span data-stu-id="d44b3-181">Most HDFS commands (for example, <b>ls</b>, <b>copyFromLocal</b> and <b>mkdir</b>) still work as expected.</span></span> <span data-ttu-id="d44b3-182">Endast de kommandon som är specifika för den interna HDFS-implementeringen (så kallade DFS-kommandon) som <b>fschk</b> och <b>dfsadmin</b> har ett avvikande beteende i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d44b3-182">Only the commands that are specific to the native HDFS implementation (which is referred to as DFS), such as <b>fschk</b> and <b>dfsadmin</b>, show different behavior in Azure storage.</span></span>
> 
> 

## <a name="create-blob-containers"></a><span data-ttu-id="d44b3-183">Skapa blob-behållare</span><span class="sxs-lookup"><span data-stu-id="d44b3-183">Create Blob containers</span></span>
<span data-ttu-id="d44b3-184">Om du vill använda blobar måste du först skapa ett [Azure Storage-konto][azure-storage-create].</span><span class="sxs-lookup"><span data-stu-id="d44b3-184">To use blobs, you first create an [Azure Storage account][azure-storage-create].</span></span> <span data-ttu-id="d44b3-185">Som en del av detta anger du en Azure-region där lagringskontot ska skapas.</span><span class="sxs-lookup"><span data-stu-id="d44b3-185">As part of this, you specify an Azure region where the storage account is created.</span></span> <span data-ttu-id="d44b3-186">Klustret och lagringskontot måste finnas i samma region.</span><span class="sxs-lookup"><span data-stu-id="d44b3-186">The cluster and the storage account must be hosted in the same region.</span></span> <span data-ttu-id="d44b3-187">SQL Server-databasen för Hive metastore och SQL Server-databasen för Oozie metastore måste också finnas i samma region.</span><span class="sxs-lookup"><span data-stu-id="d44b3-187">The Hive metastore SQL Server database and Oozie metastore SQL Server database must also be located in the same region.</span></span>

<span data-ttu-id="d44b3-188">Oavsett var den finns tillhör varje blob som du skapar en behållare på ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d44b3-188">Wherever it lives, each blob you create belongs to a container in your Azure Storage account.</span></span> <span data-ttu-id="d44b3-189">Den här behållaren kan vara en befintlig blob som skapats utanför HDInsight eller en behållare som skapats för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d44b3-189">This container may be an existing blob that was created outside of HDInsight, or it may be a container that is created for an HDInsight cluster.</span></span>

<span data-ttu-id="d44b3-190">Standardbehållaren lagrar klusterspecifik information, till exempel jobbhistorik och loggar.</span><span class="sxs-lookup"><span data-stu-id="d44b3-190">The default Blob container stores cluster-specific information such as job history and logs.</span></span> <span data-ttu-id="d44b3-191">Låt inte flera HDInsight-kluster dela en standardblob-behållare.</span><span class="sxs-lookup"><span data-stu-id="d44b3-191">Don't share a default Blob container with multiple HDInsight clusters.</span></span> <span data-ttu-id="d44b3-192">Detta kan skada jobbets historik.</span><span class="sxs-lookup"><span data-stu-id="d44b3-192">This might corrupt job history.</span></span> <span data-ttu-id="d44b3-193">Du rekommenderas att använda en annan behållare för varje kluster och publicera delade data på ett konto för länkad lagring som anges vid distributionen av alla relevanta kluster snarare än på standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="d44b3-193">It is recommended to use a different container for each cluster and put shared data on a linked storage account specified in deployment of all relevant clusters rather than the default storage account.</span></span> <span data-ttu-id="d44b3-194">Mer information om hur du konfigurerar länkade lagringskonton finns i [Skapa HDInsight-kluster][hdinsight-creation].</span><span class="sxs-lookup"><span data-stu-id="d44b3-194">For more information on configuring linked storage accounts, see [Create HDInsight clusters][hdinsight-creation].</span></span> <span data-ttu-id="d44b3-195">Du kan emellertid återanvända en standardbehållare för lagring när det ursprungliga HDInsight-klustret har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="d44b3-195">However you can reuse a default storage container after the original HDInsight cluster has been deleted.</span></span> <span data-ttu-id="d44b3-196">När det gäller HBase-kluster kan du faktiskt behålla HBase-tabellschemat och data genom att skapa en ny blob-behållare som standard som använts av ett HBase-kluster som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="d44b3-196">For HBase clusters, you can actually retain the HBase table schema and data by creating a new HBase cluster using the default blob container that is used by an HBase cluster that has been deleted.</span></span>

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a><span data-ttu-id="d44b3-197">Använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d44b3-197">Use the Azure portal</span></span>
<span data-ttu-id="d44b3-198">När du skapar ett HDInsight-kluster från portalen kan du använda ett befintligt lagringskonto (enligt nedan) och ange uppgifterna för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="d44b3-198">When creating an HDInsight cluster from the Portal, you have the options (as shown below) to provide the storage account details.</span></span> <span data-ttu-id="d44b3-199">Du kan även ange om du vill att ett konto med ytterligare lagringsutrymme som är associerade med klustret och välj i så fall från Data Lake Store eller en annan Azure Storage-blob som ytterligare lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="d44b3-199">You can also specify whether you want an additional storage account associated with the cluster, and if so, choose from Data Lake Store or another Azure Storage blob as the additional storage.</span></span>

![HDInsight hadoop, skapa datakälla](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> <span data-ttu-id="d44b3-201">Du kan inte använda ett annat lagringskonto på en annan plats än HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="d44b3-201">Using an additional storage account in a different location than the HDInsight cluster is not supported.</span></span>


### <a name="use-azure-powershell"></a><span data-ttu-id="d44b3-202">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d44b3-202">Use Azure PowerShell</span></span>
<span data-ttu-id="d44b3-203">Om du har [installerat och konfigurerat Azure PowerShell][powershell-install] kan du använda följande från Azure PowerShell-prompten för att skapa ett lagringskonto och en behållare:</span><span class="sxs-lookup"><span data-stu-id="d44b3-203">If you [installed and configured Azure PowerShell][powershell-install], you can use the following from the Azure PowerShell prompt to create a storage account and container:</span></span>

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

### <a name="use-azure-cli"></a><span data-ttu-id="d44b3-204">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d44b3-204">Use Azure CLI</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

<span data-ttu-id="d44b3-205">Om du har [installerat och konfigurerat Azure CLI](../cli-install-nodejs.md) kan du använda följande kommando för att skapa ett lagringskonto och en behållare.</span><span class="sxs-lookup"><span data-stu-id="d44b3-205">If you have [installed and configured the Azure CLI](../cli-install-nodejs.md), the following command can be used to a storage account and container.</span></span>

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> <span data-ttu-id="d44b3-206">Parametern `--type` anger hur lagringskontot kommer att replikeras.</span><span class="sxs-lookup"><span data-stu-id="d44b3-206">The `--type` parameter indicates how the storage account is replicated.</span></span> <span data-ttu-id="d44b3-207">Mer information finns i [Azure Storage-replikering](../storage/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="d44b3-207">For more information, see [Azure Storage Replication](../storage/storage-redundancy.md).</span></span> <span data-ttu-id="d44b3-208">Använd inte ZRS eftersom ZRS inte stöder sidblobbar, filer, tabeller eller köer.</span><span class="sxs-lookup"><span data-stu-id="d44b3-208">Don't use ZRS as ZRS doesn't support page blob, file, table, or queue.</span></span>
> 
> 

<span data-ttu-id="d44b3-209">Du kommer att bli ombedd att ange den geografiska region som lagringskontot kommer att finnas i.</span><span class="sxs-lookup"><span data-stu-id="d44b3-209">You are prompted to specify the geographic region that the storage account is created in.</span></span> <span data-ttu-id="d44b3-210">Du bör skapa lagringskontot i den region där du planerar att skapa ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d44b3-210">You should create the storage account in the same region that you plan on creating your HDInsight cluster.</span></span>

<span data-ttu-id="d44b3-211">När du har skapat lagringskontot använder du följande kommando för att hämta nycklar till lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="d44b3-211">Once the storage account is created, use the following command to retrieve the storage account keys:</span></span>

    azure storage account keys list <storageaccountname>

<span data-ttu-id="d44b3-212">Om du vill skapa en behållare använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d44b3-212">To create a container, use the following command:</span></span>

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a><span data-ttu-id="d44b3-213">Adressera filer i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d44b3-213">Address files in Azure storage</span></span>
<span data-ttu-id="d44b3-214">Följande URI-schema används för att komma åt filer i Azure Storage från HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d44b3-214">The URI scheme for accessing files in Azure storage from HDInsight is:</span></span>

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

<span data-ttu-id="d44b3-215">URI-schemat ger okrypterad åtkomst (med prefixet *wasb:*) och SSL-krypterad åtkomst (med *wasbs*).</span><span class="sxs-lookup"><span data-stu-id="d44b3-215">The URI scheme provides unencrypted access (with the *wasb:* prefix) and SSL encrypted access (with *wasbs*).</span></span> <span data-ttu-id="d44b3-216">Vi rekommenderar att du använder *wasbs* när det är möjligt, även för åtkomst till data som finns i samma region i Azure.</span><span class="sxs-lookup"><span data-stu-id="d44b3-216">We recommend using *wasbs* wherever possible, even when accessing data that lives inside the same region in Azure.</span></span>

<span data-ttu-id="d44b3-217">&lt;BlobStorageContainerName&gt; identifierar namnet på blobbehållaren i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d44b3-217">The &lt;BlobStorageContainerName&gt; identifies the name of the blob container in Azure storage.</span></span>
<span data-ttu-id="d44b3-218">&lt;StorageAccountName&gt; identifierar namnet på Azure Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="d44b3-218">The &lt;StorageAccountName&gt; identifies the Azure Storage account name.</span></span> <span data-ttu-id="d44b3-219">Ett fullständigt kvalificerat domännamn (FQDN) krävs.</span><span class="sxs-lookup"><span data-stu-id="d44b3-219">A fully qualified domain name (FQDN) is required.</span></span>

<span data-ttu-id="d44b3-220">Om varken &lt;BlobStorageContainerName&gt; eller &lt;StorageAccountName&gt; har angetts används standardfilsystemet.</span><span class="sxs-lookup"><span data-stu-id="d44b3-220">If neither &lt;BlobStorageContainerName&gt; nor &lt;StorageAccountName&gt; has been specified, the default file system is used.</span></span> <span data-ttu-id="d44b3-221">För filer i filsystemet kan du använda en relativ sökväg eller en absolut sökväg.</span><span class="sxs-lookup"><span data-stu-id="d44b3-221">For the files on the default file system, you can use a relative path or an absolute path.</span></span> <span data-ttu-id="d44b3-222">Till exempel kan du referera till den *hadoop-mapreduce-examples.jar*-fil som medföljer HDInsight-kluster på något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d44b3-222">For example, the *hadoop-mapreduce-examples.jar* file that comes with HDInsight clusters can be referred to by using one of the following:</span></span>

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="d44b3-223">Filnamnet är <i>hadoop examples.jar</i> i HDInsight-kluster av version 2.1 och 1.6.</span><span class="sxs-lookup"><span data-stu-id="d44b3-223">The file name is <i>hadoop-examples.jar</i> in HDInsight versions 2.1 and 1.6 clusters.</span></span>
> 
> 

<span data-ttu-id="d44b3-224">&lt;Sökvägen&gt; är filens eller katalogens HDFS-sökvägsnamn.</span><span class="sxs-lookup"><span data-stu-id="d44b3-224">The &lt;path&gt; is the file or directory HDFS path name.</span></span> <span data-ttu-id="d44b3-225">Eftersom behållare i Azure Storage helt enkelt är lagringsplatser för nyckel-/värdepar finns det inte något faktiskt hierarkiskt filsystem.</span><span class="sxs-lookup"><span data-stu-id="d44b3-225">Because containers in Azure storage are simply key-value stores, there is no true hierarchical file system.</span></span> <span data-ttu-id="d44b3-226">Ett snedstreck (/) i en blob-nyckel tolkas som en katalogavgränsare.</span><span class="sxs-lookup"><span data-stu-id="d44b3-226">A slash character ( / ) inside a blob key is interpreted as a directory separator.</span></span> <span data-ttu-id="d44b3-227">Blob-namnet för *hadoop-mapreduce-examples.jar* är till exempel:</span><span class="sxs-lookup"><span data-stu-id="d44b3-227">For example, the blob name for *hadoop-mapreduce-examples.jar* is:</span></span>

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="d44b3-228">När du arbetar med blobar utanför HDInsight kan de flesta verktyg inte identifiera WASB-formatet utan förväntar sig i stället ett grundläggande sökvägsformat som `example/jars/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="d44b3-228">When working with blobs outside of HDInsight, most utilities do not recognize the WASB format and instead expect a basic path format, such as `example/jars/hadoop-mapreduce-examples.jar`.</span></span>
> 
> 

## <a name="access-blobs"></a><span data-ttu-id="d44b3-229">Få åtkomst till blobbar</span><span class="sxs-lookup"><span data-stu-id="d44b3-229">Access blobs</span></span> 


### <span data-ttu-id="d44b3-230"><a name="access-blobs-using-azure-powershell"></a>Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d44b3-230"><a name="access-blobs-using-azure-powershell"></a> Use Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="d44b3-231">Kommandona i det här avsnittet ger grundläggande exempel på hur du använder PowerShell för att komma åt data som är lagrade i blobar.</span><span class="sxs-lookup"><span data-stu-id="d44b3-231">The commands in this section provide a basic example of using PowerShell to access data stored in blobs.</span></span> <span data-ttu-id="d44b3-232">Om du vill ha ett mer komplett exempel som är anpassat för att arbeta med HDInsight, se [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="d44b3-232">For a more full-featured example that is customized for working with HDInsight, see the [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span></span>
> 
> 

<span data-ttu-id="d44b3-233">Använd följande kommando för att visa en lista över blob-relaterade cmdlets:</span><span class="sxs-lookup"><span data-stu-id="d44b3-233">Use the following command to list the blob-related cmdlets:</span></span>

    Get-Command *blob*

![Lista över blob-relaterade PowerShell-cmdlets.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a><span data-ttu-id="d44b3-235">Överföra filer</span><span class="sxs-lookup"><span data-stu-id="d44b3-235">Upload files</span></span>
<span data-ttu-id="d44b3-236">Mer information finns i [Överföra data till HDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="d44b3-236">See [Upload data to HDInsight][hdinsight-upload-data].</span></span>

#### <a name="download-files"></a><span data-ttu-id="d44b3-237">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="d44b3-237">Download files</span></span>
<span data-ttu-id="d44b3-238">Följande skript hämtar en blockblob till den aktuella mappen.</span><span class="sxs-lookup"><span data-stu-id="d44b3-238">The following script downloads a block blob to the current folder.</span></span> <span data-ttu-id="d44b3-239">Innan du kör skriptet ändrar du katalogen till en mapp där du har skrivbehörighet.</span><span class="sxs-lookup"><span data-stu-id="d44b3-239">Before running the script, change the directory to a folder where you have write permissions.</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

<span data-ttu-id="d44b3-240">Om du anger resursgruppens namn och klusternamnet kan du använda följande kod:</span><span class="sxs-lookup"><span data-stu-id="d44b3-240">Providing the resource group name and the cluster name, you can use the following code:</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a><span data-ttu-id="d44b3-241">Ta bort filer</span><span class="sxs-lookup"><span data-stu-id="d44b3-241">Delete files</span></span>
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a><span data-ttu-id="d44b3-242">Lista filer</span><span class="sxs-lookup"><span data-stu-id="d44b3-242">List files</span></span>
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a><span data-ttu-id="d44b3-243">Köra Hive-frågor med ett odefinierat lagringskonto</span><span class="sxs-lookup"><span data-stu-id="d44b3-243">Run Hive queries using an undefined storage account</span></span>
<span data-ttu-id="d44b3-244">Det här exemplet visar hur du listar en mapp från lagringskontot som inte har definierats under skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="d44b3-244">This example shows how to list a folder from storage account that is not defined during the creating process.</span></span>
<span data-ttu-id="d44b3-245">$clusterName = "<HDInsightClusterName>"</span><span class="sxs-lookup"><span data-stu-id="d44b3-245">$clusterName = "<HDInsightClusterName>"</span></span>

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a><span data-ttu-id="d44b3-246">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d44b3-246">Use Azure CLI</span></span>
<span data-ttu-id="d44b3-247">Använd följande kommando för att visa en lista över blob-relaterade kommandon:</span><span class="sxs-lookup"><span data-stu-id="d44b3-247">Use the following command to list the blob-related commands:</span></span>

    azure storage blob

<span data-ttu-id="d44b3-248">**Exempel på hur du använder Azure CLI för att överföra en fil**</span><span class="sxs-lookup"><span data-stu-id="d44b3-248">**Example of using Azure CLI to upload a file**</span></span>

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="d44b3-249">**Exempel på hur du använder Azure CLI för att hämta en fil**</span><span class="sxs-lookup"><span data-stu-id="d44b3-249">**Example of using Azure CLI to download a file**</span></span>

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="d44b3-250">**Exempel på hur du använder Azure CLI för att ta bort en fil**</span><span class="sxs-lookup"><span data-stu-id="d44b3-250">**Example of using Azure CLI to delete a file**</span></span>

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="d44b3-251">**Exempel på hur du använder Azure CLI för att lista filer**</span><span class="sxs-lookup"><span data-stu-id="d44b3-251">**Example of using Azure CLI to list files**</span></span>

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a><span data-ttu-id="d44b3-252">Använda ytterligare lagringskonton</span><span class="sxs-lookup"><span data-stu-id="d44b3-252">Use additional storage accounts</span></span>

<span data-ttu-id="d44b3-253">När du skapar ett HDInsight-kluster kan du ange ett Azure Storage-konto som du vill koppla det till.</span><span class="sxs-lookup"><span data-stu-id="d44b3-253">While creating an HDInsight cluster, you specify the Azure Storage account you want to associate with it.</span></span> <span data-ttu-id="d44b3-254">Förutom det här lagringskontot kan du lägga till ytterligare lagringskonton från samma Azure-prenumeration eller andra Azure-prenumerationer under skapandeprocessen eller efter att ett kluster har skapats.</span><span class="sxs-lookup"><span data-stu-id="d44b3-254">In addition to this storage account, you can add additional storage accounts from the same Azure subscription or different Azure subscriptions during the creation process or after a cluster has been created.</span></span> <span data-ttu-id="d44b3-255">Mer information om hur du lägger till ytterligare lagringskonton finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="d44b3-255">For instructions about adding additional storage accounts, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!WARNING]
> <span data-ttu-id="d44b3-256">Du kan inte använda ett annat lagringskonto på en annan plats än HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="d44b3-256">Using an additional storage account in a different location than the HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d44b3-257">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d44b3-257">Next steps</span></span>
<span data-ttu-id="d44b3-258">I den här artikeln fick du lära dig hur du använder det HDFS-kompatibla Azure Storage med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d44b3-258">In this article, you learned how to use HDFS-compatible Azure storage with HDInsight.</span></span> <span data-ttu-id="d44b3-259">Du kan skapa skalbara, långsiktiga lösningar för arkivering av insamlade data samt HDInsight för att få tillgång till informationen i lagrade strukturerade och ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="d44b3-259">This allows you to build scalable, long-term, archiving data acquisition solutions and use HDInsight to unlock the information inside the stored structured and unstructured data.</span></span>

<span data-ttu-id="d44b3-260">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="d44b3-260">For more information, see:</span></span>

* <span data-ttu-id="d44b3-261">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="d44b3-261">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="d44b3-262">Kom igång med Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d44b3-262">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="d44b3-263">[Överföra data till HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="d44b3-263">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="d44b3-264">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="d44b3-264">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="d44b3-265">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="d44b3-265">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="d44b3-266">[Använda signaturer för delad åtkomst i Azure Storage för att begränsa åtkomsten till data med HDInsight][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="d44b3-266">[Use Azure Storage Shared Access Signatures to restrict access to data with HDInsight][hdinsight-use-sas]</span></span>

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
