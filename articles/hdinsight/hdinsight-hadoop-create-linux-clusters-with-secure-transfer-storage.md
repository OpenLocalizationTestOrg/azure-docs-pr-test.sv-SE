---
title: "aaaCreate Hadoop-kluster med säker överföring storage-konton i Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate HDInsight-kluster med säker överföring aktiverat Azure storage-konton."
keywords: hadoop getting started,hadoop linux,hadoop quickstart,secure transfer,azure storage account
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: jgao
ms.openlocfilehash: 0acb8814ad0d5d5b5652d930b2e3da90f9d7978d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a><span data-ttu-id="2466d-104">Skapa ett Hadoop-kluster med lagringskonton som använder säker överföring i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="2466d-104">Create Hadoop cluster with secure transfer storage accounts in Azure HDInsight</span></span>

<span data-ttu-id="2466d-105">Hej [säker överföring krävs](../storage/common/storage-require-secure-transfer.md) funktionen förbättrar hello säkerheten för din Azure Storage-konto genom att tillämpa alla begäranden tooyour kontot via en säker anslutning.</span><span class="sxs-lookup"><span data-stu-id="2466d-105">hello [Secure transfer required](../storage/common/storage-require-secure-transfer.md) feature enhances hello security of your Azure Storage account by enforcing all requests tooyour account through a secure connection.</span></span> <span data-ttu-id="2466d-106">Den här funktionen och hello wasbs schemat stöds endast av HDInsight-kluster av version 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2466d-106">This feature and hello wasbs scheme are only supported by HDInsight cluster version 3.6 or newer.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2466d-107">Krav</span><span class="sxs-lookup"><span data-stu-id="2466d-107">Prerequisites</span></span>
<span data-ttu-id="2466d-108">Innan du börjar den här vägledningen måste du ha:</span><span class="sxs-lookup"><span data-stu-id="2466d-108">Before you begin this tutorial, you must have:</span></span>

* <span data-ttu-id="2466d-109">**Azure-prenumeration**: toocreate ett kostnadsfritt en månad konto, bläddra för[azure.microsoft.com/free](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="2466d-109">**Azure subscription**: toocreate a free one-month trial account, browse too[azure.microsoft.com/free](https://azure.microsoft.com/free).</span></span>
* <span data-ttu-id="2466d-110">**Ett Azure Storage-konto med säker överföring aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="2466d-110">**An Azure Storage account with secure transfer enabled**.</span></span> <span data-ttu-id="2466d-111">Hello instruktioner finns i [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) och [kräver säker överföring](../storage/common/storage-require-secure-transfer.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-111">For hello instructions, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) and [Require secure transfer](../storage/common/storage-require-secure-transfer.md).</span></span>
* <span data-ttu-id="2466d-112">**En Blob-behållare på hello lagringskontot**.</span><span class="sxs-lookup"><span data-stu-id="2466d-112">**A Blob container on hello storage account**.</span></span> 
## <a name="create-cluster"></a><span data-ttu-id="2466d-113">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="2466d-113">Create cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


<span data-ttu-id="2466d-114">I det här avsnittet skapar du ett Hadoop-kluster i HDInsight med en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-114">In this section, you create a Hadoop cluster in HDInsight using an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="2466d-115">hello-mallen finns i [Gibhub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/).</span><span class="sxs-lookup"><span data-stu-id="2466d-115">hello template is located in [Gibhub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/).</span></span> <span data-ttu-id="2466d-116">Du behöver inte ha någon erfarenhet av Resource Manager-mallar för att kunna följa de här självstudierna.</span><span class="sxs-lookup"><span data-stu-id="2466d-116">Resource Manager template experience is not required for following this tutorial.</span></span> <span data-ttu-id="2466d-117">För andra metoder för att skapa kluster och förstå hello egenskaper som används i den här självstudiekursen, se [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-117">For other cluster creation methods and understanding hello properties used in this tutorial, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="2466d-118">Klicka på hello efter bilden toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2466d-118">Click hello following image toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="2466d-119">Följ hello instruktioner toocreate hello kluster med hello följande specifikationer:</span><span class="sxs-lookup"><span data-stu-id="2466d-119">Follow hello instructions toocreate hello cluster with hello following specifications:</span></span> 

    - <span data-ttu-id="2466d-120">Ange HDInsight version 3.6.</span><span class="sxs-lookup"><span data-stu-id="2466d-120">Specify HDInsight version 3.6.</span></span>  <span data-ttu-id="2466d-121">hello standardversionen är 3.5.</span><span class="sxs-lookup"><span data-stu-id="2466d-121">hello default version is 3.5.</span></span> <span data-ttu-id="2466d-122">Version 3.6 eller senare krävs.</span><span class="sxs-lookup"><span data-stu-id="2466d-122">Version 3.6 or newer is required.</span></span>
    - <span data-ttu-id="2466d-123">Ange ett lagringskonto som använder säker överföring.</span><span class="sxs-lookup"><span data-stu-id="2466d-123">Specify a secure transfer enabled storage account.</span></span>
    - <span data-ttu-id="2466d-124">Använd ett kort namn för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2466d-124">Use short name for hello storage account.</span></span>
    - <span data-ttu-id="2466d-125">Både hello storage-konto och hello blob-behållaren måste skapas i förväg.</span><span class="sxs-lookup"><span data-stu-id="2466d-125">Both hello storage account and hello blob container must be created beforehand.</span></span> 

    <span data-ttu-id="2466d-126">Hello instruktioner finns i [Skapa kluster](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="2466d-126">For hello instructions, see [Create cluster](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span> 

<span data-ttu-id="2466d-127">Om du använder skriptet åtgärd tooprovide egna konfigurationsfiler, måste du använda wasbs i hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="2466d-127">If you use script action tooprovide your own configuration files, you must use wasbs in hello following settings:</span></span>

- <span data-ttu-id="2466d-128">fs.defaultFS (core-site)</span><span class="sxs-lookup"><span data-stu-id="2466d-128">fs.defaultFS (core-site)</span></span>
- <span data-ttu-id="2466d-129">spark.eventLog.dir</span><span class="sxs-lookup"><span data-stu-id="2466d-129">spark.eventLog.dir</span></span> 
- <span data-ttu-id="2466d-130">spark.history.fs.logDirectory</span><span class="sxs-lookup"><span data-stu-id="2466d-130">spark.history.fs.logDirectory</span></span>

## <a name="add-additional-storage-accounts"></a><span data-ttu-id="2466d-131">Lägga till fler lagringskonton</span><span class="sxs-lookup"><span data-stu-id="2466d-131">Add additional storage accounts</span></span>

<span data-ttu-id="2466d-132">Det finns flera alternativ tooadd ytterligare säker överföring aktiverat storage-konton:</span><span class="sxs-lookup"><span data-stu-id="2466d-132">There are several options tooadd additional secure transfer enabled storage accounts:</span></span>

- <span data-ttu-id="2466d-133">Ändra hello Azure Resource Manager-mall i hello sista avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2466d-133">Modify hello Azure Resource Manager template in hello last section.</span></span>
- <span data-ttu-id="2466d-134">Skapa ett kluster med hello [Azure-portalen](https://portal.azure.com) och ange länkade storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2466d-134">Create a cluster using hello [Azure portal](https://portal.azure.com) and specify linked storage account.</span></span>
- <span data-ttu-id="2466d-135">Använd skriptet åtgärd tooadd ytterligare säker överföring aktiverat storage-konton tooan befintligt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2466d-135">Use script action tooadd additional secure transfer enabled storage accounts tooan existing HDInsight cluster.</span></span>  <span data-ttu-id="2466d-136">Mer information finns i [lägga till ytterligare lagringsutrymme konton tooHDInsight](hdinsight-hadoop-add-storage.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-136">For more information, see [Add additional storage accounts tooHDInsight](hdinsight-hadoop-add-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2466d-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2466d-137">Next steps</span></span>
<span data-ttu-id="2466d-138">I den här kursen har du lärt dig hur toocreate ett HDInsight-kluster och aktivera säker överföring av toohello storage-konton.</span><span class="sxs-lookup"><span data-stu-id="2466d-138">In this tutorial, you have learned how toocreate an HDInsight cluster, and enable secure transfer toohello storage accounts.</span></span>

<span data-ttu-id="2466d-139">toolearn mer information om hur du analyserar data med HDInsight, se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="2466d-139">toolearn more about analyzing data with HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="2466d-140">toolearn mer information om hur du använder Hive med HDInsight, inklusive hur tooperform Hive-frågor från Visual Studio finns [använda Hive med HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="2466d-140">toolearn more about using Hive with HDInsight, including how tooperform Hive queries from Visual Studio, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
* <span data-ttu-id="2466d-141">toolearn om Pig, ett språk används tootransform data, se [använda Pig med HDInsight][hdinsight-use-pig].</span><span class="sxs-lookup"><span data-stu-id="2466d-141">toolearn about Pig, a language used tootransform data, see [Use Pig with HDInsight][hdinsight-use-pig].</span></span>
* <span data-ttu-id="2466d-142">toolearn om MapReduce, ett hur toowrite program som bearbetar data i Hadoop, se [använda MapReduce med HDInsight][hdinsight-use-mapreduce].</span><span class="sxs-lookup"><span data-stu-id="2466d-142">toolearn about MapReduce, a way toowrite programs that process data on Hadoop, see [Use MapReduce with HDInsight][hdinsight-use-mapreduce].</span></span>
* <span data-ttu-id="2466d-143">toolearn om hur du använder hello HDInsight Tools för Visual Studio tooanalyze data i HDInsight, se [komma igång med Visual Studio Hadoop-verktygen för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-143">toolearn about using hello HDInsight Tools for Visual Studio tooanalyze data on HDInsight, see [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

<span data-ttu-id="2466d-144">Mer information om hur HDInsight lagrar data toolearn eller hur tooget data till HDInsight, se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="2466d-144">toolearn more about how HDInsight stores data or how tooget data into HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="2466d-145">Mer information om hur HDInsight använder Azure Storage finns i [Använda Azure Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-145">For information on how HDInsight uses Azure Storage, see [Use Azure Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span>
* <span data-ttu-id="2466d-146">Mer information om hur tooupload data tooHDInsight finns [överför data tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="2466d-146">For information on how tooupload data tooHDInsight, see [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

<span data-ttu-id="2466d-147">toolearn mer om att skapa eller hantera HDInsight-kluster, se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="2466d-147">toolearn more about creating or managing an HDInsight cluster, see hello following articles:</span></span>

* <span data-ttu-id="2466d-148">toolearn om hur du hanterar ditt Linux-baserade HDInsight-kluster finns [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-148">toolearn about managing your Linux-based HDInsight cluster, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
* <span data-ttu-id="2466d-149">toolearn mer om hello-alternativ som du kan välja när du skapar ett HDInsight-kluster finns [skapa HDInsight i Linux med anpassade alternativ](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-149">toolearn more about hello options you can select when creating an HDInsight cluster, see [Creating HDInsight on Linux using custom options](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="2466d-150">Om du är bekant med Linux och Hadoop, men vill tooknow närmare information om Hadoop på hello HDInsight, se [arbeta med HDInsight på Linux](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="2466d-150">If you are familiar with Linux, and Hadoop, but want tooknow specifics about Hadoop on hello HDInsight, see [Working with HDInsight on Linux](hdinsight-hadoop-linux-information.md).</span></span> <span data-ttu-id="2466d-151">Detta ger information som:</span><span class="sxs-lookup"><span data-stu-id="2466d-151">This article provides information such as:</span></span>
  
  * <span data-ttu-id="2466d-152">URL: er för tjänster som ligger på hello klustret, till exempel Ambari och WebHCat</span><span class="sxs-lookup"><span data-stu-id="2466d-152">URLs for services hosted on hello cluster, such as Ambari and WebHCat</span></span>
  * <span data-ttu-id="2466d-153">hello platsen för Hadoop-filer och exempel på hello lokalt filsystem</span><span class="sxs-lookup"><span data-stu-id="2466d-153">hello location of Hadoop files and examples on hello local file system</span></span>
  * <span data-ttu-id="2466d-154">hello användning av Azure Storage (WASB) i stället för HDFS som hello standard datalager</span><span class="sxs-lookup"><span data-stu-id="2466d-154">hello use of Azure Storage (WASB) instead of HDFS as hello default data store</span></span>

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


