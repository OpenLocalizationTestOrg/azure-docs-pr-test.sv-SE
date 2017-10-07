---
title: aaaUse Data Lake Store med Hadoop i Azure HDInsight | Microsoft Docs
description: "Lär dig hur tooquery data från Azure Data Lake Store och toostore resultatet av dina analyser."
keywords: blob storage,hdfs,structured data,unstructured data, data lake store
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: jgao
ms.openlocfilehash: 89633218a37a2fe05043e05d61199dcc0252d7f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a><span data-ttu-id="6cf79-104">Använda Data Lake Store med Azure HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="6cf79-104">Use Data Lake Store with Azure HDInsight clusters</span></span>

<span data-ttu-id="6cf79-105">tooanalyze data i HDInsight-kluster, du kan lagra data hello antingen i [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), eller båda.</span><span class="sxs-lookup"><span data-stu-id="6cf79-105">tooanalyze data in HDInsight cluster, you can store hello data either in [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), or both.</span></span> <span data-ttu-id="6cf79-106">Båda lagringsalternativ aktivera toosafely ta bort HDInsight-kluster som används för beräkning utan att förlora användardata.</span><span class="sxs-lookup"><span data-stu-id="6cf79-106">Both storage options enable you toosafely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="6cf79-107">I den här artikeln får du lära dig hur Data Lake Store fungerar med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="6cf79-107">In this article, you learn how Data Lake Store works with HDInsight clusters.</span></span> <span data-ttu-id="6cf79-108">toolearn hur Azure Storage fungerar med HDInsight-kluster finns i [använda Azure Storage med Azure HDInsight-kluster](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="6cf79-108">toolearn how Azure Storage works with HDInsight clusters, see [Use Azure Storage with Azure HDInsight clusters](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="6cf79-109">Mer information om hur du skapar ett HDInsight-kluster finns i [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Skapa Hadoop-kluster i HDInsight).</span><span class="sxs-lookup"><span data-stu-id="6cf79-109">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6cf79-110">Data Lake Store alltid är tillgängligt via en säker kanal så det finns inget `adls`-filsystemsschemanamn.</span><span class="sxs-lookup"><span data-stu-id="6cf79-110">Data Lake Store is always accessed through a secure channel, so there is no `adls` filesystem scheme name.</span></span> <span data-ttu-id="6cf79-111">Du använder alltid `adl`.</span><span class="sxs-lookup"><span data-stu-id="6cf79-111">You always use `adl`.</span></span>
> 
> 

## <a name="availabilities-for-hdinsight-clusters"></a><span data-ttu-id="6cf79-112">Tillgänglighet för HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="6cf79-112">Availabilities for HDInsight clusters</span></span>

<span data-ttu-id="6cf79-113">Hadoop stöder begreppet standardfilsystem hello.</span><span class="sxs-lookup"><span data-stu-id="6cf79-113">Hadoop supports a notion of hello default file system.</span></span> <span data-ttu-id="6cf79-114">Hej standardfilsystemet kräver ett standardschema och utfärdare.</span><span class="sxs-lookup"><span data-stu-id="6cf79-114">hello default file system implies a default scheme and authority.</span></span> <span data-ttu-id="6cf79-115">Det kan också vara används tooresolve relativa sökvägar.</span><span class="sxs-lookup"><span data-stu-id="6cf79-115">It can also be used tooresolve relative paths.</span></span> <span data-ttu-id="6cf79-116">Under skapandeprocessen hello HDInsight-kluster, kan du ange en blob-behållare i Azure Storage som hello standardfilsystem eller med HDInsight 3.5 och nyare versioner, kan du välja Azure Storage eller Azure Data Lake Store som hello standard filer system med en få undantag.</span><span class="sxs-lookup"><span data-stu-id="6cf79-116">During hello HDInsight cluster creation process, you can specify a blob container in Azure Storage as hello default file system, or with HDInsight 3.5 and newer versions, you can select either Azure Storage or Azure Data Lake Store as hello default files system with a few exceptions.</span></span> 

<span data-ttu-id="6cf79-117">HDInsight-kluster kan använda Data Lake Store på två sätt:</span><span class="sxs-lookup"><span data-stu-id="6cf79-117">HDInsight clusters can use Data Lake Store in two ways:</span></span>

* <span data-ttu-id="6cf79-118">Som standard hello</span><span class="sxs-lookup"><span data-stu-id="6cf79-118">As hello default storage</span></span>
* <span data-ttu-id="6cf79-119">Som ytterligare lagringsutrymme med Azure Storage Blob som standardlagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="6cf79-119">As additional storage, with Azure Storage Blob as default storage.</span></span>

<span data-ttu-id="6cf79-120">Från och med nu endast en del av hello HDInsight klusterstöd typer-versioner med Data Lake Store som standardlagring och ytterligare storage-konton:</span><span class="sxs-lookup"><span data-stu-id="6cf79-120">As of now, only some of hello HDInsight cluster types/versions support using Data Lake Store as default storage and additional storage accounts:</span></span>

| <span data-ttu-id="6cf79-121">Typ av HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="6cf79-121">HDInsight cluster type</span></span> | <span data-ttu-id="6cf79-122">Data Lake Store som standardlagring</span><span class="sxs-lookup"><span data-stu-id="6cf79-122">Data Lake Store as default storage</span></span> | <span data-ttu-id="6cf79-123">Data Lake Store som ytterligare lagring</span><span class="sxs-lookup"><span data-stu-id="6cf79-123">Data Lake Store as additional storage</span></span>| <span data-ttu-id="6cf79-124">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="6cf79-124">Notes</span></span> |
|------------------------|------------------------------------|---------------------------------------|------|
| <span data-ttu-id="6cf79-125">HDInsight version 3.6</span><span class="sxs-lookup"><span data-stu-id="6cf79-125">HDInsight version 3.6</span></span> | <span data-ttu-id="6cf79-126">Ja</span><span class="sxs-lookup"><span data-stu-id="6cf79-126">Yes</span></span> | <span data-ttu-id="6cf79-127">Ja</span><span class="sxs-lookup"><span data-stu-id="6cf79-127">Yes</span></span> | |
| <span data-ttu-id="6cf79-128">HDInsight version 3.5</span><span class="sxs-lookup"><span data-stu-id="6cf79-128">HDInsight version 3.5</span></span> | <span data-ttu-id="6cf79-129">Ja</span><span class="sxs-lookup"><span data-stu-id="6cf79-129">Yes</span></span> | <span data-ttu-id="6cf79-130">Ja</span><span class="sxs-lookup"><span data-stu-id="6cf79-130">Yes</span></span> | <span data-ttu-id="6cf79-131">Med undantag för hello av HBase</span><span class="sxs-lookup"><span data-stu-id="6cf79-131">With hello exception of HBase</span></span>|
| <span data-ttu-id="6cf79-132">HDInsight version 3.4</span><span class="sxs-lookup"><span data-stu-id="6cf79-132">HDInsight version 3.4</span></span> | <span data-ttu-id="6cf79-133">Nej</span><span class="sxs-lookup"><span data-stu-id="6cf79-133">No</span></span> | <span data-ttu-id="6cf79-134">Ja</span><span class="sxs-lookup"><span data-stu-id="6cf79-134">Yes</span></span> | |
| <span data-ttu-id="6cf79-135">HDInsight version 3.3</span><span class="sxs-lookup"><span data-stu-id="6cf79-135">HDInsight version 3.3</span></span> | <span data-ttu-id="6cf79-136">Nej</span><span class="sxs-lookup"><span data-stu-id="6cf79-136">No</span></span> | <span data-ttu-id="6cf79-137">Nej</span><span class="sxs-lookup"><span data-stu-id="6cf79-137">No</span></span> | |
| <span data-ttu-id="6cf79-138">HDInsight version 3.2</span><span class="sxs-lookup"><span data-stu-id="6cf79-138">HDInsight version 3.2</span></span> | <span data-ttu-id="6cf79-139">Nej</span><span class="sxs-lookup"><span data-stu-id="6cf79-139">No</span></span> | <span data-ttu-id="6cf79-140">Ja</span><span class="sxs-lookup"><span data-stu-id="6cf79-140">Yes</span></span> | |
| <span data-ttu-id="6cf79-141">HDInsight Premium (nivå)</span><span class="sxs-lookup"><span data-stu-id="6cf79-141">HDInsight Premium (tier)</span></span>| <span data-ttu-id="6cf79-142">Nej</span><span class="sxs-lookup"><span data-stu-id="6cf79-142">No</span></span> | <span data-ttu-id="6cf79-143">Nej</span><span class="sxs-lookup"><span data-stu-id="6cf79-143">No</span></span> | |
| <span data-ttu-id="6cf79-144">Storm</span><span class="sxs-lookup"><span data-stu-id="6cf79-144">Storm</span></span> | | |<span data-ttu-id="6cf79-145">Du kan använda Data Lake Store toowrite data från en Storm-topologi.</span><span class="sxs-lookup"><span data-stu-id="6cf79-145">You can use Data Lake Store toowrite data from a Storm topology.</span></span> <span data-ttu-id="6cf79-146">Du kan också använda Data Lake Store för referensdata som sedan kan läsas av en Storm-topologi.</span><span class="sxs-lookup"><span data-stu-id="6cf79-146">You can also use Data Lake Store for reference data that can then be read by a Storm topology.</span></span>|

<span data-ttu-id="6cf79-147">Med Data Lake Store som ett ytterligare storage-konto inte påverka prestanda eller hello möjlighet tooread eller skriva tooAzure lagring från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="6cf79-147">Using Data Lake Store as an additional storage account does not affect performance or hello ability tooread or write tooAzure storage from hello cluster.</span></span>


## <a name="use-data-lake-store-as-default-storage"></a><span data-ttu-id="6cf79-148">Använda Data Lake Store som standardlagring</span><span class="sxs-lookup"><span data-stu-id="6cf79-148">Use Data Lake Store as default storage</span></span>

<span data-ttu-id="6cf79-149">När HDInsight har distribuerats med Data Lake Store som standardlagring, lagras hello kluster-relaterade filer i Data Lake Store i hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="6cf79-149">When HDInsight is deployed with Data Lake Store as default storage, hello cluster-related files are stored in Data Lake Store in hello following location:</span></span>

    adl://mydatalakestore/<cluster_root_path>/

<span data-ttu-id="6cf79-150">där `<cluster_root_path>` är hello namnet på en mapp som du skapar i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6cf79-150">where `<cluster_root_path>` is hello name of a folder you create in Data Lake Store.</span></span> <span data-ttu-id="6cf79-151">Genom att ange en rotsökväg för varje kluster kan du använda hello samma Data Lake Store-konto för flera kluster.</span><span class="sxs-lookup"><span data-stu-id="6cf79-151">By specifying a root path for each cluster, you can use hello same Data Lake Store account for more than one cluster.</span></span> <span data-ttu-id="6cf79-152">Så du kan ha en konfiguration enligt följande:</span><span class="sxs-lookup"><span data-stu-id="6cf79-152">So, you can have a setup where:</span></span>

* <span data-ttu-id="6cf79-153">Cluster1 kan använda hello sökväg`adl://mydatalakestore/cluster1storage`</span><span class="sxs-lookup"><span data-stu-id="6cf79-153">Cluster1 can use hello path `adl://mydatalakestore/cluster1storage`</span></span>
* <span data-ttu-id="6cf79-154">Cluster2 kan använda hello sökväg`adl://mydatalakestore/cluster2storage`</span><span class="sxs-lookup"><span data-stu-id="6cf79-154">Cluster2 can use hello path `adl://mydatalakestore/cluster2storage`</span></span>

<span data-ttu-id="6cf79-155">Meddelande både hello kluster Använd hello samma Data Lake Store-konto **mydatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="6cf79-155">Notice that both hello clusters use hello same Data Lake Store account **mydatalakestore**.</span></span> <span data-ttu-id="6cf79-156">Varje kluster har åtkomst tooits äger rot filsystem i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6cf79-156">Each cluster has access tooits own root filesystem in Data Lake Store.</span></span> <span data-ttu-id="6cf79-157">hello Azure portal distributionsupplevelse särskilt efterfrågar toouse ett mappnamn som **/clusters/\<klusternamn >** för hello rotsökvägen.</span><span class="sxs-lookup"><span data-stu-id="6cf79-157">hello Azure portal deployment experience in particular prompts you toouse a folder name such as **/clusters/\<clustername>** for hello root path.</span></span>

<span data-ttu-id="6cf79-158">toobe kan toouse ett Data Lake Store som standardlagring, måste du bevilja hello service principal åtkomst toohello följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="6cf79-158">toobe able toouse a Data Lake Store as default storage, you must grant hello service principal access toohello following paths:</span></span>

- <span data-ttu-id="6cf79-159">hello roten för Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="6cf79-159">hello Data Lake Store account root.</span></span>  <span data-ttu-id="6cf79-160">Till exempel: adl://mydatalakestore/.</span><span class="sxs-lookup"><span data-stu-id="6cf79-160">For example: adl://mydatalakestore/.</span></span>
- <span data-ttu-id="6cf79-161">hello mapp för alla mappar för klustret.</span><span class="sxs-lookup"><span data-stu-id="6cf79-161">hello folder for all cluster folders.</span></span>  <span data-ttu-id="6cf79-162">Till exempel: adl://mydatalakestore/clusters.</span><span class="sxs-lookup"><span data-stu-id="6cf79-162">For example: adl://mydatalakestore/clusters.</span></span>
- <span data-ttu-id="6cf79-163">hello mapp för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="6cf79-163">hello folder for hello cluster.</span></span>  <span data-ttu-id="6cf79-164">Till exempel: adl://mydatalakestore/clusters/cluster1storage.</span><span class="sxs-lookup"><span data-stu-id="6cf79-164">For example: adl://mydatalakestore/clusters/cluster1storage.</span></span>

<span data-ttu-id="6cf79-165">Mer information om att skapa tjänstens huvudnamn och bevilja åtkomst finns i [Konfigurera åtkomst till Data Lake Store](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="6cf79-165">For more information for creating service principal and grant access, see [Configure Data Lake store access](#configure-data-lake-store-access).</span></span>


## <a name="use-data-lake-store-as-additional-storage"></a><span data-ttu-id="6cf79-166">Använda Data Lake Store som ytterligare lagring</span><span class="sxs-lookup"><span data-stu-id="6cf79-166">Use Data Lake Store as additional storage</span></span>

<span data-ttu-id="6cf79-167">Du kan använda Data Lake Store som ytterligare lagringsutrymme för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="6cf79-167">You can use Data Lake Store as additional storage for hello cluster as well.</span></span> <span data-ttu-id="6cf79-168">I sådana fall kan hello standard klusterlagringen vara en Azure Storage Blob eller ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="6cf79-168">In such cases, hello cluster default storage can either be an Azure Storage Blob or a Data Lake Store account.</span></span> <span data-ttu-id="6cf79-169">Om du använder HDInsight jobb mot hello data som lagras i Data Lake Store som ytterligare lagringsutrymme måste du använda hello fullständig sökväg toohello filer.</span><span class="sxs-lookup"><span data-stu-id="6cf79-169">If you are running HDInsight jobs against hello data stored in Data Lake Store as additional storage, you must use hello fully-qualified path toohello files.</span></span> <span data-ttu-id="6cf79-170">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6cf79-170">For example:</span></span>

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

<span data-ttu-id="6cf79-171">Observera att det finns inga **cluster_root_path** i nu hello-URL.</span><span class="sxs-lookup"><span data-stu-id="6cf79-171">Note that there's no **cluster_root_path** in hello URL now.</span></span> <span data-ttu-id="6cf79-172">Det beror på att Data Lake Store inte är en standardlagring i det här fallet så behöver du toodo hello sökvägen toohello filer.</span><span class="sxs-lookup"><span data-stu-id="6cf79-172">That's because Data Lake Store is not a default storage in this case so all you need toodo is provide hello path toohello files.</span></span>

<span data-ttu-id="6cf79-173">toobe kan toouse ett Data Lake Store som ytterligare lagringsutrymme, behöver du bara toogrant hello service principal åtkomst toohello sökvägar där filerna lagras.</span><span class="sxs-lookup"><span data-stu-id="6cf79-173">toobe able toouse a Data Lake Store as additional storage, you only need toogrant hello service principal access toohello paths where your files are stored.</span></span>  <span data-ttu-id="6cf79-174">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6cf79-174">For example:</span></span>

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

<span data-ttu-id="6cf79-175">Mer information om att skapa tjänstens huvudnamn och bevilja åtkomst finns i [Konfigurera åtkomst till Data Lake Store](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="6cf79-175">For more information for creating service principal and grant access, see [Configure Data Lake store access](#configure-data-lake-store-access).</span></span>


## <a name="use-more-than-one-data-lake-store-accounts"></a><span data-ttu-id="6cf79-176">Använda fler än ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="6cf79-176">Use more than one Data Lake Store accounts</span></span>

<span data-ttu-id="6cf79-177">Lägger till ett Data Lake Store-konto som ytterligare och lägga till fler än en Data Lake Store erhålla konton genom att ge behörighet för hello HDInsight-kluster på data i en eller flera datasjölagerkonton.</span><span class="sxs-lookup"><span data-stu-id="6cf79-177">Adding a Data Lake Store account as additional and adding more than one Data Lake Store accounts are accomplished by giving hello HDInsight cluster permission on data in one ore more Data Lake Store accounts.</span></span> <span data-ttu-id="6cf79-178">Läs mer i [Konfigurera åtkomst till Data Lake Store](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="6cf79-178">See [Configure Data Lake Store access](#configure-data-lake-store-access).</span></span>

## <a name="configure-data-lake-store-access"></a><span data-ttu-id="6cf79-179">Konfigurera åtkomst till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6cf79-179">Configure Data Lake store access</span></span>

<span data-ttu-id="6cf79-180">Du måste ha en Azure Active directory (AD Azure) tjänstens huvudnamn tooconfigure Data Lake store-åtkomst från ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="6cf79-180">tooconfigure Data Lake store access from your HDInsight cluster, you must have an Azure Active directory (Azure AD) service principal.</span></span> <span data-ttu-id="6cf79-181">Det är bara Azure AD-administratörer som kan vara tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="6cf79-181">Only an Azure AD administrator can create a service principal.</span></span> <span data-ttu-id="6cf79-182">hello tjänstens huvudnamn måste skapas med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="6cf79-182">hello service principal must be created with a certificate.</span></span> <span data-ttu-id="6cf79-183">Mer information finns i [Konfigurera åtkomst till Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access) och [Create service principal with self-signed-certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate) (Skapa tjänstens huvudnamn med självsignerat certifikat).</span><span class="sxs-lookup"><span data-stu-id="6cf79-183">For more information, see [Configure Data Lake Store access](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access), and [Create service principal with self-signed-certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).</span></span>

> [!NOTE]
> <span data-ttu-id="6cf79-184">Om du ska toouse Azure Data Lake Store som ytterligare lagringsutrymme för HDInsight-kluster, rekommenderar vi att du gör detta när du skapar klustret hello som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6cf79-184">If you are going toouse Azure Data Lake Store as additional storage for HDInsight cluster, we strongly recommend that you do this while you create hello cluster as described in this article.</span></span> <span data-ttu-id="6cf79-185">Lägga till Azure Data Lake Store som ytterligare lagringsutrymme tooan är befintligt HDInsight-kluster en komplicerad process och felbenägna tooerrors.</span><span class="sxs-lookup"><span data-stu-id="6cf79-185">Adding Azure Data Lake Store as additional storage tooan existing HDInsight cluster is a complicated process and prone tooerrors.</span></span>
>

## <a name="access-files-from-hello-cluster"></a><span data-ttu-id="6cf79-186">Komma åt filer från hello kluster</span><span class="sxs-lookup"><span data-stu-id="6cf79-186">Access files from hello cluster</span></span>

<span data-ttu-id="6cf79-187">Det finns flera sätt som du kan komma åt hello filer i Data Lake Store från ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="6cf79-187">There are several ways you can access hello files in Data Lake Store from an HDInsight cluster.</span></span>

* <span data-ttu-id="6cf79-188">**Hello fullständigt kvalificerade namnet**.</span><span class="sxs-lookup"><span data-stu-id="6cf79-188">**Using hello fully qualified name**.</span></span> <span data-ttu-id="6cf79-189">Med den här metoden ger du hello fullständig sökväg toohello filen som du vill tooaccess.</span><span class="sxs-lookup"><span data-stu-id="6cf79-189">With this approach, you provide hello full path toohello file that you want tooaccess.</span></span>

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* <span data-ttu-id="6cf79-190">**Formatet för hello kortare sökväg**.</span><span class="sxs-lookup"><span data-stu-id="6cf79-190">**Using hello shortened path format**.</span></span> <span data-ttu-id="6cf79-191">Med den här metoden ersätts hello sökväg in toohello kluster roten adl: / / /.</span><span class="sxs-lookup"><span data-stu-id="6cf79-191">With this approach, you replace hello path up toohello cluster root with adl:///.</span></span> <span data-ttu-id="6cf79-192">Så i hello-exemplet ovan kan du ersätta `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` med `adl:///`.</span><span class="sxs-lookup"><span data-stu-id="6cf79-192">So, in hello example above, you can replace `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` with `adl:///`.</span></span>

        adl:///<file path>

* <span data-ttu-id="6cf79-193">**Med hjälp av hello relativa sökvägen**.</span><span class="sxs-lookup"><span data-stu-id="6cf79-193">**Using hello relative path**.</span></span> <span data-ttu-id="6cf79-194">Med den här metoden du bara ange hello relativ sökväg toohello filen som du vill tooaccess.</span><span class="sxs-lookup"><span data-stu-id="6cf79-194">With this approach, you only provide hello relative path toohello file that you want tooaccess.</span></span> <span data-ttu-id="6cf79-195">Om exempelvis hello fullständig sökväg toohello filen är:</span><span class="sxs-lookup"><span data-stu-id="6cf79-195">For example, if hello complete path toohello file is:</span></span>

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    <span data-ttu-id="6cf79-196">Du kan komma åt hello samma sample.log-fil med hjälp av den här relativ sökväg istället.</span><span class="sxs-lookup"><span data-stu-id="6cf79-196">You can access hello same sample.log file by using this relative path instead.</span></span>

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-toodata-lake-store"></a><span data-ttu-id="6cf79-197">Skapa HDInsight-kluster med att komma åt tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="6cf79-197">Create HDInsight clusters with access tooData Lake Store</span></span>

<span data-ttu-id="6cf79-198">Använd hello efter länkar till detaljerade instruktioner om hur toocreate HDInsight-kluster med åt tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6cf79-198">Use hello following links for detailed instructions on how toocreate HDInsight clusters with access tooData Lake Store.</span></span>

* [<span data-ttu-id="6cf79-199">Använda portalen</span><span class="sxs-lookup"><span data-stu-id="6cf79-199">Using Portal</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [<span data-ttu-id="6cf79-200">Använda PowerShell (med Data Lake Store som standardlagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="6cf79-200">Using PowerShell (with Data Lake Store as default storage)</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [<span data-ttu-id="6cf79-201">Använda PowerShell (med Data Lake Store som ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="6cf79-201">Using PowerShell (with Data Lake Store as additional storage)</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [<span data-ttu-id="6cf79-202">Använda Azure-mallar</span><span class="sxs-lookup"><span data-stu-id="6cf79-202">Using Azure templates</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a><span data-ttu-id="6cf79-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6cf79-203">Next steps</span></span>
<span data-ttu-id="6cf79-204">I den här artikeln har du lärt dig hur toouse HDFS-kompatibla Azure Data Lake Store med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6cf79-204">In this article, you learned how toouse HDFS-compatible Azure Data Lake Store with HDInsight.</span></span> <span data-ttu-id="6cf79-205">Detta gör att du toobuild skalbara, långsiktiga, arkivering lösningar för datainsamling och använda HDInsight toounlock hello information i hello lagrade strukturerade och Ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="6cf79-205">This allows you toobuild scalable, long-term, archiving data acquisition solutions and use HDInsight toounlock hello information inside hello stored structured and unstructured data.</span></span>

<span data-ttu-id="6cf79-206">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="6cf79-206">For more information, see:</span></span>

* <span data-ttu-id="6cf79-207">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="6cf79-207">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="6cf79-208">Kom igång med Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6cf79-208">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="6cf79-209">[Ladda upp data tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="6cf79-209">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="6cf79-210">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="6cf79-210">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="6cf79-211">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="6cf79-211">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="6cf79-212">[Använda Azure Storage signaturer för delad åtkomst toorestrict åtkomst toodata med HDInsight][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="6cf79-212">[Use Azure Storage Shared Access Signatures toorestrict access toodata with HDInsight][hdinsight-use-sas]</span></span>

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
