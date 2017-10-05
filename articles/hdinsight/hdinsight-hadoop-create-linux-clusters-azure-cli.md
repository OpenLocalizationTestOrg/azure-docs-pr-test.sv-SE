---
title: "Skapa Hadoop-kluster med hjälp av kommandoraden-Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du skapar HDInsight-kluster med flera plattformar Azure CLI 1.0."
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
ms.openlocfilehash: 8f2fcb46789d000cd66164508f1159338dcae5f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a><span data-ttu-id="6460f-103">Skapa HDInsight-kluster med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6460f-103">Create HDInsight clusters using the Azure CLI</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="6460f-104">Stegen i den här genomgången för dokument som skapar ett HDInsight 3.5-kluster med Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="6460f-104">The steps in this document walk-through creating a HDInsight 3.5 cluster using the Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6460f-105">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="6460f-105">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="6460f-106">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="6460f-106">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="6460f-107">Krav</span><span class="sxs-lookup"><span data-stu-id="6460f-107">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="6460f-108">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="6460f-108">**An Azure subscription**.</span></span> <span data-ttu-id="6460f-109">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="6460f-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="6460f-110">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="6460f-110">**Azure CLI**.</span></span> <span data-ttu-id="6460f-111">Stegen i det här dokumentet testades senast med Azure CLI version 0.10.14.</span><span class="sxs-lookup"><span data-stu-id="6460f-111">The steps in this document were last tested with Azure CLI version 0.10.14.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6460f-112">Stegen i det här dokumentet fungerar inte med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="6460f-112">The steps in this document do not work with Azure CLI 2.0.</span></span> <span data-ttu-id="6460f-113">Azure CLI 2.0 stöder inte att skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="6460f-113">Azure CLI 2.0 does not support creating an HDInsight cluster.</span></span>

## <a name="log-in-to-your-azure-subscription"></a><span data-ttu-id="6460f-114">Logga in till din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6460f-114">Log in to your Azure subscription</span></span>

<span data-ttu-id="6460f-115">Följ stegen i [Anslut till en Azure-prenumeration från Azure-kommandoradsgränssnittet (Azure CLI)](../xplat-cli-connect.md) och anslut till din prenumeration med hjälp av metoden **inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6460f-115">Follow the steps documented in [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md) and connect to your subscription using the **login** method.</span></span>

## <a name="create-a-cluster"></a><span data-ttu-id="6460f-116">Skapa ett kluster</span><span class="sxs-lookup"><span data-stu-id="6460f-116">Create a cluster</span></span>

<span data-ttu-id="6460f-117">Följande steg ska utföras från en kommandorad, till exempel PowerShell- eller Bash.</span><span class="sxs-lookup"><span data-stu-id="6460f-117">The following steps should be performed from a command line, such as PowerShell or Bash.</span></span>

1. <span data-ttu-id="6460f-118">Använd följande kommando för att autentisera till din Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="6460f-118">Use the following command to authenticate to your Azure subscription:</span></span>

        azure login

    <span data-ttu-id="6460f-119">Du uppmanas att ange ditt namn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="6460f-119">You are prompted to provide your name and password.</span></span> <span data-ttu-id="6460f-120">Om du har flera Azure-prenumerationer, Använd `azure account set <subscriptionname>` att ange den prenumeration som använder Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="6460f-120">If you have multiple Azure subscriptions, use `azure account set <subscriptionname>` to set the subscription that the Azure CLI commands use.</span></span>

2. <span data-ttu-id="6460f-121">Växla till läget Azure Resource Manager, med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6460f-121">Switch to Azure Resource Manager mode using the following command:</span></span>

        azure config mode arm

3. <span data-ttu-id="6460f-122">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6460f-122">Create a resource group.</span></span> <span data-ttu-id="6460f-123">Den här resursgruppen innehåller HDInsight-klustret och associerad storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6460f-123">This resource group contains the HDInsight cluster and associated storage account.</span></span>

        azure group create groupname location

    * <span data-ttu-id="6460f-124">Ersätt `groupname` med ett unikt namn för gruppen.</span><span class="sxs-lookup"><span data-stu-id="6460f-124">Replace `groupname` with a unique name for the group.</span></span>

    * <span data-ttu-id="6460f-125">Ersätt `location` med den geografiska region som du vill skapa gruppen i.</span><span class="sxs-lookup"><span data-stu-id="6460f-125">Replace `location` with the geographic region that you want to create the group in.</span></span>

       <span data-ttu-id="6460f-126">En lista över giltiga platser, använder den `azure location list` kommando och Använd sedan någon av platserna från den `Name` kolumn.</span><span class="sxs-lookup"><span data-stu-id="6460f-126">For a list of valid locations, use the `azure location list` command, and then use one of the locations from the `Name` column.</span></span>

4. <span data-ttu-id="6460f-127">Skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6460f-127">Create a storage account.</span></span> <span data-ttu-id="6460f-128">Det här lagringskontot används som standardlagring för HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="6460f-128">This storage account is used as the default storage for the HDInsight cluster.</span></span>

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * <span data-ttu-id="6460f-129">Ersätt `groupname` med namnet på gruppen som skapades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="6460f-129">Replace `groupname` with the name of the group created in the previous step.</span></span>

    * <span data-ttu-id="6460f-130">Ersätt `location` med samma plats som används i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="6460f-130">Replace `location` with the same location used in the previous step.</span></span>

    * <span data-ttu-id="6460f-131">Ersätt `storagename` med ett unikt namn för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6460f-131">Replace `storagename` with a unique name for the storage account.</span></span>

        > [!NOTE]
        > <span data-ttu-id="6460f-132">Mer information om de parametrar som används i det här kommandot använder `azure storage account create -h` vill visa hjälp för det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="6460f-132">For more information on the parameters used in this command, use `azure storage account create -h` to view help for this command.</span></span>

5. <span data-ttu-id="6460f-133">Hämta den nyckel som används för att komma åt lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6460f-133">Retrieve the key used to access the storage account.</span></span>

        azure storage account keys list -g groupname storagename

    * <span data-ttu-id="6460f-134">Ersätt `groupname` med resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="6460f-134">Replace `groupname` with the resource group name.</span></span>
    * <span data-ttu-id="6460f-135">Ersätt `storagename` med namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6460f-135">Replace `storagename` with the name of the storage account.</span></span>

     <span data-ttu-id="6460f-136">Spara i de data som returneras av `key` värde för `key1`.</span><span class="sxs-lookup"><span data-stu-id="6460f-136">In the data that is returned, save the `key` value for `key1`.</span></span>

6. <span data-ttu-id="6460f-137">Skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="6460f-137">Create an HDInsight cluster.</span></span>

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * <span data-ttu-id="6460f-138">Ersätt `groupname` med resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="6460f-138">Replace `groupname` with the resource group name.</span></span>

    * <span data-ttu-id="6460f-139">Ersätt `Hadoop` med den typ av kluster som du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="6460f-139">Replace `Hadoop` with the cluster type that you wish to create.</span></span> <span data-ttu-id="6460f-140">Till exempel `Hadoop`, `HBase`, `Kafka`, `Spark`, eller `Storm`.</span><span class="sxs-lookup"><span data-stu-id="6460f-140">For example, `Hadoop`, `HBase`, `Kafka`, `Spark`, or `Storm`.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="6460f-141">HDInsight-kluster som har olika typer som motsvarar arbetsbelastning eller teknik som klustret är inställd för.</span><span class="sxs-lookup"><span data-stu-id="6460f-141">HDInsight clusters come in various types, which correspond to the workload or technology that the cluster is tuned for.</span></span> <span data-ttu-id="6460f-142">Det finns ingen stöds metod för att skapa ett kluster som kombinerar flera typer, till exempel Storm och HBase på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="6460f-142">There is no supported method to create a cluster that combines multiple types, such as Storm and HBase on one cluster.</span></span>

    * <span data-ttu-id="6460f-143">Ersätt `location` med samma plats som används i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="6460f-143">Replace `location` with the same location used in previous steps.</span></span>

    * <span data-ttu-id="6460f-144">Ersätt `storagename` med namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6460f-144">Replace `storagename` with the storage account name.</span></span>

    * <span data-ttu-id="6460f-145">Ersätt `storagekey` med den nyckel som hämtades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="6460f-145">Replace `storagekey` with the key obtained in the previous step.</span></span>

    * <span data-ttu-id="6460f-146">För den `--defaultStorageContainer` parametern, använder samma namn som du använder för klustret.</span><span class="sxs-lookup"><span data-stu-id="6460f-146">For the `--defaultStorageContainer` parameter, use the same name as you are using for the cluster.</span></span>

    * <span data-ttu-id="6460f-147">Ersätt `admin` och `httppassword` med namnet och lösenordet som du vill använda när klustret via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6460f-147">Replace `admin` and `httppassword` with the name and password you wish to use when accessing the cluster through HTTPS.</span></span>

    * <span data-ttu-id="6460f-148">Ersätt `sshuser` och `sshuserpassword` med användarnamn och lösenord som du vill använda när du ansluter till klustret via SSH</span><span class="sxs-lookup"><span data-stu-id="6460f-148">Replace `sshuser` and `sshuserpassword` with the username and password you wish to use when accessing the cluster using SSH</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6460f-149">Det här exemplet skapar ett kluster med två worker anteckningar.</span><span class="sxs-lookup"><span data-stu-id="6460f-149">This example creates a cluster with two worker notes.</span></span> <span data-ttu-id="6460f-150">Du kan också ändra antalet arbetarnoder när klustret har skapats genom att utföra skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6460f-150">You can also change the number of worker nodes after cluster creation by performing scaling operations.</span></span> <span data-ttu-id="6460f-151">Om du tänker använda mer än 32 arbetarnoder, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="6460f-151">If you plan on using more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14-GB RAM.</span></span> <span data-ttu-id="6460f-152">Du kan ange huvudnod storlek med den `--headNodeSize` parameter när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="6460f-152">You can set the head node size by using the `--headNodeSize` parameter during cluster creation.</span></span>
    >
    > <span data-ttu-id="6460f-153">Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="6460f-153">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

    <span data-ttu-id="6460f-154">Det kan ta flera minuter för klusterskapandeprocessen ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="6460f-154">It may take several minutes for the cluster creation process to finish.</span></span> <span data-ttu-id="6460f-155">Vanligtvis cirka 15.</span><span class="sxs-lookup"><span data-stu-id="6460f-155">Usually around 15.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="6460f-156">Felsöka</span><span class="sxs-lookup"><span data-stu-id="6460f-156">Troubleshoot</span></span>

<span data-ttu-id="6460f-157">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="6460f-157">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6460f-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6460f-158">Next steps</span></span>

<span data-ttu-id="6460f-159">Nu när du har skapat ett HDInsight-kluster med hjälp av Azure CLI, använder du följande information om hur du arbetar med ditt kluster:</span><span class="sxs-lookup"><span data-stu-id="6460f-159">Now that you have successfully created an HDInsight cluster using the Azure CLI, use the following to learn how to work with your cluster:</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="6460f-160">Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="6460f-160">Hadoop clusters</span></span>

* [<span data-ttu-id="6460f-161">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="6460f-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="6460f-162">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="6460f-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="6460f-163">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="6460f-163">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="6460f-164">HBase-kluster</span><span class="sxs-lookup"><span data-stu-id="6460f-164">HBase clusters</span></span>

* [<span data-ttu-id="6460f-165">Kom igång med HBase på HDInsight</span><span class="sxs-lookup"><span data-stu-id="6460f-165">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="6460f-166">Utveckla Java-program för HBase i HDInsight</span><span class="sxs-lookup"><span data-stu-id="6460f-166">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="6460f-167">Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="6460f-167">Storm clusters</span></span>

* [<span data-ttu-id="6460f-168">Utveckla Java-topologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="6460f-168">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="6460f-169">Använda Python komponenter i Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="6460f-169">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="6460f-170">Distribuera och övervaka topologier med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="6460f-170">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
