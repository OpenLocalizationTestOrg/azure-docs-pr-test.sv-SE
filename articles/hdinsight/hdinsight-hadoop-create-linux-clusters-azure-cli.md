---
title: "aaaCreate Hadoop-kluster med hjälp av kommandoradsverktyget - hello Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate HDInsight-kluster med hello plattformsoberoende Azure CLI 1.0."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 5295b01054b8c23df0e3b75a3e0e8c933ac48b3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-using-hello-azure-cli"></a><span data-ttu-id="cea81-103">Skapa HDInsight-kluster med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cea81-103">Create HDInsight clusters using hello Azure CLI</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="cea81-104">hello stegen i den här genomgången för dokument som skapar ett HDInsight 3.5-kluster med hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="cea81-104">hello steps in this document walk-through creating a HDInsight 3.5 cluster using hello Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cea81-105">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="cea81-105">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cea81-106">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="cea81-106">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="cea81-107">Krav</span><span class="sxs-lookup"><span data-stu-id="cea81-107">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="cea81-108">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="cea81-108">**An Azure subscription**.</span></span> <span data-ttu-id="cea81-109">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="cea81-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="cea81-110">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="cea81-110">**Azure CLI**.</span></span> <span data-ttu-id="cea81-111">hello stegen i det här dokumentet testades senast med Azure CLI version 0.10.14.</span><span class="sxs-lookup"><span data-stu-id="cea81-111">hello steps in this document were last tested with Azure CLI version 0.10.14.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="cea81-112">hello stegen i det här dokumentet fungerar inte med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="cea81-112">hello steps in this document do not work with Azure CLI 2.0.</span></span> <span data-ttu-id="cea81-113">Azure CLI 2.0 stöder inte att skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cea81-113">Azure CLI 2.0 does not support creating an HDInsight cluster.</span></span>

## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="cea81-114">Logga in tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cea81-114">Log in tooyour Azure subscription</span></span>

<span data-ttu-id="cea81-115">Följ stegen i hello [ansluta tooan Azure-prenumeration från hello Azure-kommandoradsgränssnittet (Azure CLI)](../xplat-cli-connect.md) och ansluta tooyour prenumeration med hjälp av hello **inloggning** metod.</span><span class="sxs-lookup"><span data-stu-id="cea81-115">Follow hello steps documented in [Connect tooan Azure subscription from hello Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md) and connect tooyour subscription using hello **login** method.</span></span>

## <a name="create-a-cluster"></a><span data-ttu-id="cea81-116">Skapa ett kluster</span><span class="sxs-lookup"><span data-stu-id="cea81-116">Create a cluster</span></span>

<span data-ttu-id="cea81-117">hello följande steg ska utföras från en kommandorad, till exempel PowerShell- eller Bash.</span><span class="sxs-lookup"><span data-stu-id="cea81-117">hello following steps should be performed from a command line, such as PowerShell or Bash.</span></span>

1. <span data-ttu-id="cea81-118">Använd följande kommando tooauthenticate tooyour Azure-prenumeration hello:</span><span class="sxs-lookup"><span data-stu-id="cea81-118">Use hello following command tooauthenticate tooyour Azure subscription:</span></span>

        azure login

    <span data-ttu-id="cea81-119">Du är begärd tooprovide ditt namn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="cea81-119">You are prompted tooprovide your name and password.</span></span> <span data-ttu-id="cea81-120">Om du har flera Azure-prenumerationer, Använd `azure account set <subscriptionname>` tooset hello prenumeration som hello Azure CLI-kommandon används.</span><span class="sxs-lookup"><span data-stu-id="cea81-120">If you have multiple Azure subscriptions, use `azure account set <subscriptionname>` tooset hello subscription that hello Azure CLI commands use.</span></span>

2. <span data-ttu-id="cea81-121">Växla tooAzure Resource Manager-läge med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cea81-121">Switch tooAzure Resource Manager mode using hello following command:</span></span>

        azure config mode arm

3. <span data-ttu-id="cea81-122">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="cea81-122">Create a resource group.</span></span> <span data-ttu-id="cea81-123">Den här resursgruppen innehåller hello HDInsight-kluster och associerad storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cea81-123">This resource group contains hello HDInsight cluster and associated storage account.</span></span>

        azure group create groupname location

    * <span data-ttu-id="cea81-124">Ersätt `groupname` med ett unikt namn för hello grupp.</span><span class="sxs-lookup"><span data-stu-id="cea81-124">Replace `groupname` with a unique name for hello group.</span></span>

    * <span data-ttu-id="cea81-125">Ersätt `location` med hello geografiska region som du vill toocreate hello grupp i.</span><span class="sxs-lookup"><span data-stu-id="cea81-125">Replace `location` with hello geographic region that you want toocreate hello group in.</span></span>

       <span data-ttu-id="cea81-126">En lista över giltiga platser använder hello `azure location list` kommando och Använd sedan någon av hello platser från hello `Name` kolumn.</span><span class="sxs-lookup"><span data-stu-id="cea81-126">For a list of valid locations, use hello `azure location list` command, and then use one of hello locations from hello `Name` column.</span></span>

4. <span data-ttu-id="cea81-127">Skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="cea81-127">Create a storage account.</span></span> <span data-ttu-id="cea81-128">Det här lagringskontot används som hello standardlagring för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cea81-128">This storage account is used as hello default storage for hello HDInsight cluster.</span></span>

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * <span data-ttu-id="cea81-129">Ersätt `groupname` med hello-grupp som skapades i föregående steg i hello hello namn.</span><span class="sxs-lookup"><span data-stu-id="cea81-129">Replace `groupname` with hello name of hello group created in hello previous step.</span></span>

    * <span data-ttu-id="cea81-130">Ersätt `location` med hello samma plats som används i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="cea81-130">Replace `location` with hello same location used in hello previous step.</span></span>

    * <span data-ttu-id="cea81-131">Ersätt `storagename` med ett unikt namn för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cea81-131">Replace `storagename` with a unique name for hello storage account.</span></span>

        > [!NOTE]
        > <span data-ttu-id="cea81-132">Mer information om hello parametrar som används i det här kommandot använder `azure storage account create -h` tooview hjälp för det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="cea81-132">For more information on hello parameters used in this command, use `azure storage account create -h` tooview help for this command.</span></span>

5. <span data-ttu-id="cea81-133">Hämta hello nyckel används tooaccess hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cea81-133">Retrieve hello key used tooaccess hello storage account.</span></span>

        azure storage account keys list -g groupname storagename

    * <span data-ttu-id="cea81-134">Ersätt `groupname` med hello resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="cea81-134">Replace `groupname` with hello resource group name.</span></span>
    * <span data-ttu-id="cea81-135">Ersätt `storagename` med hello namnet hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cea81-135">Replace `storagename` with hello name of hello storage account.</span></span>

     <span data-ttu-id="cea81-136">Hello data som returneras, spara hello `key` värde för `key1`.</span><span class="sxs-lookup"><span data-stu-id="cea81-136">In hello data that is returned, save hello `key` value for `key1`.</span></span>

6. <span data-ttu-id="cea81-137">Skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cea81-137">Create an HDInsight cluster.</span></span>

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * <span data-ttu-id="cea81-138">Ersätt `groupname` med hello resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="cea81-138">Replace `groupname` with hello resource group name.</span></span>

    * <span data-ttu-id="cea81-139">Ersätt `Hadoop` med hello typ av kluster du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="cea81-139">Replace `Hadoop` with hello cluster type that you wish toocreate.</span></span> <span data-ttu-id="cea81-140">Till exempel `Hadoop`, `HBase`, `Kafka`, `Spark`, eller `Storm`.</span><span class="sxs-lookup"><span data-stu-id="cea81-140">For example, `Hadoop`, `HBase`, `Kafka`, `Spark`, or `Storm`.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="cea81-141">HDInsight-kluster som har olika typer som toohello arbetsbelastning eller teknik som hello klustret är inställd för.</span><span class="sxs-lookup"><span data-stu-id="cea81-141">HDInsight clusters come in various types, which correspond toohello workload or technology that hello cluster is tuned for.</span></span> <span data-ttu-id="cea81-142">Det finns ingen metod som stöds toocreate ett kluster som kombinerar flera typer, till exempel Storm och HBase på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="cea81-142">There is no supported method toocreate a cluster that combines multiple types, such as Storm and HBase on one cluster.</span></span>

    * <span data-ttu-id="cea81-143">Ersätt `location` med hello samma plats som används i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="cea81-143">Replace `location` with hello same location used in previous steps.</span></span>

    * <span data-ttu-id="cea81-144">Ersätt `storagename` med hello lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="cea81-144">Replace `storagename` with hello storage account name.</span></span>

    * <span data-ttu-id="cea81-145">Ersätt `storagekey` med hello-nyckeln som hämtades i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="cea81-145">Replace `storagekey` with hello key obtained in hello previous step.</span></span>

    * <span data-ttu-id="cea81-146">För hello `--defaultStorageContainer` parametern, Använd hello samma namn som du använder för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="cea81-146">For hello `--defaultStorageContainer` parameter, use hello same name as you are using for hello cluster.</span></span>

    * <span data-ttu-id="cea81-147">Ersätt `admin` och `httppassword` med hello namn och lösenord du vill toouse när hello klustret via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cea81-147">Replace `admin` and `httppassword` with hello name and password you wish toouse when accessing hello cluster through HTTPS.</span></span>

    * <span data-ttu-id="cea81-148">Ersätt `sshuser` och `sshuserpassword` med hello användarnamn och lösenord du vill toouse vid åtkomst till hello kluster med SSH</span><span class="sxs-lookup"><span data-stu-id="cea81-148">Replace `sshuser` and `sshuserpassword` with hello username and password you wish toouse when accessing hello cluster using SSH</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="cea81-149">Det här exemplet skapar ett kluster med två worker anteckningar.</span><span class="sxs-lookup"><span data-stu-id="cea81-149">This example creates a cluster with two worker notes.</span></span> <span data-ttu-id="cea81-150">Du kan också ändra hello antalet arbetarnoder när klustret har skapats genom att utföra skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cea81-150">You can also change hello number of worker nodes after cluster creation by performing scaling operations.</span></span> <span data-ttu-id="cea81-151">Om du tänker använda mer än 32 arbetarnoder, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="cea81-151">If you plan on using more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14-GB RAM.</span></span> <span data-ttu-id="cea81-152">Du kan ange hello huvudnod storlek med hello `--headNodeSize` parameter när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="cea81-152">You can set hello head node size by using hello `--headNodeSize` parameter during cluster creation.</span></span>
    >
    > <span data-ttu-id="cea81-153">Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="cea81-153">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

    <span data-ttu-id="cea81-154">Det kan ta flera minuter för hello klustret skapas processen toofinish.</span><span class="sxs-lookup"><span data-stu-id="cea81-154">It may take several minutes for hello cluster creation process toofinish.</span></span> <span data-ttu-id="cea81-155">Vanligtvis cirka 15.</span><span class="sxs-lookup"><span data-stu-id="cea81-155">Usually around 15.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="cea81-156">Felsöka</span><span class="sxs-lookup"><span data-stu-id="cea81-156">Troubleshoot</span></span>

<span data-ttu-id="cea81-157">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="cea81-157">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cea81-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cea81-158">Next steps</span></span>

<span data-ttu-id="cea81-159">Nu när du har skapat ett HDInsight-kluster med hjälp av hello Azure CLI, använder du följande toolearn hur hello toowork med ditt kluster:</span><span class="sxs-lookup"><span data-stu-id="cea81-159">Now that you have successfully created an HDInsight cluster using hello Azure CLI, use hello following toolearn how toowork with your cluster:</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="cea81-160">Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="cea81-160">Hadoop clusters</span></span>

* [<span data-ttu-id="cea81-161">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea81-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="cea81-162">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea81-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="cea81-163">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea81-163">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="cea81-164">HBase-kluster</span><span class="sxs-lookup"><span data-stu-id="cea81-164">HBase clusters</span></span>

* [<span data-ttu-id="cea81-165">Kom igång med HBase på HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea81-165">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="cea81-166">Utveckla Java-program för HBase i HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea81-166">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="cea81-167">Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="cea81-167">Storm clusters</span></span>

* [<span data-ttu-id="cea81-168">Utveckla Java-topologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea81-168">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="cea81-169">Använda Python komponenter i Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea81-169">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="cea81-170">Distribuera och övervaka topologier med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="cea81-170">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
