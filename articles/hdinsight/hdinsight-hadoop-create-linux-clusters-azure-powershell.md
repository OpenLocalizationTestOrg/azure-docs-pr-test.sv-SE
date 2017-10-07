---
title: "Hadoop-aaaCreate kluster med hjälp av PowerShell - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate Hadoop, HBase, Storm eller Spark kluster på Linux för HDInsight med hjälp av Azure PowerShell."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 53afe4702f6b61a0720ceda48a4a34d7fa8797d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a><span data-ttu-id="d7aca-103">Skapa Linux-baserade kluster i HDInsight med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d7aca-103">Create Linux-based clusters in HDInsight using Azure PowerShell</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="d7aca-104">Azure PowerShell är en kraftfull skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av dina arbetsbelastningar i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d7aca-104">Azure PowerShell is a powerful scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Microsoft Azure.</span></span> <span data-ttu-id="d7aca-105">Det här dokumentet innehåller information om hur toocreate en Linux-baserat HDInsight-kluster med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d7aca-105">This document provides information about how toocreate a Linux-based HDInsight cluster by using Azure PowerShell.</span></span> <span data-ttu-id="d7aca-106">Den innehåller också ett exempelskript.</span><span class="sxs-lookup"><span data-stu-id="d7aca-106">It also includes an example script.</span></span>

> [!NOTE]
> <span data-ttu-id="d7aca-107">Azure PowerShell är bara tillgänglig på Windows-klienter.</span><span class="sxs-lookup"><span data-stu-id="d7aca-107">Azure PowerShell is only available on Windows clients.</span></span> <span data-ttu-id="d7aca-108">Om du använder en Linux-, Unix- eller Mac OS X-klient, se [skapa ett Linux-baserat HDInsight-kluster med Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) information om hur du använder hello Azure CLI toocreate ett kluster.</span><span class="sxs-lookup"><span data-stu-id="d7aca-108">If you are using a Linux, Unix, or Mac OS X client, see [Create a Linux-based HDInsight cluster using Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) for information about using hello Azure CLI toocreate a cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7aca-109">Krav</span><span class="sxs-lookup"><span data-stu-id="d7aca-109">Prerequisites</span></span>
<span data-ttu-id="d7aca-110">Du måste ha hello följande innan du påbörjar den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="d7aca-110">You must have hello following before starting this procedure:</span></span>

* <span data-ttu-id="d7aca-111">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d7aca-111">An Azure subscription.</span></span> <span data-ttu-id="d7aca-112">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="d7aca-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* [<span data-ttu-id="d7aca-113">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d7aca-113">Azure PowerShell</span></span>](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > <span data-ttu-id="d7aca-114">Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017.</span><span class="sxs-lookup"><span data-stu-id="d7aca-114">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="d7aca-115">hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d7aca-115">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="d7aca-116">Följ stegen hello i [installera Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d7aca-116">Please follow hello steps in [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="d7aca-117">Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d7aca-117">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

## <a name="create-cluster"></a><span data-ttu-id="d7aca-118">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="d7aca-118">Create cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="d7aca-119">toocreate ett HDInsight-kluster med hjälp av Azure PowerShell, måste du utföra följande procedurer hello:</span><span class="sxs-lookup"><span data-stu-id="d7aca-119">toocreate an HDInsight cluster by using Azure PowerShell, you must complete hello following procedures:</span></span>

* <span data-ttu-id="d7aca-120">Skapa en Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d7aca-120">Create an Azure resource group</span></span>
* <span data-ttu-id="d7aca-121">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="d7aca-121">Create an Azure Storage account</span></span>
* <span data-ttu-id="d7aca-122">Skapa en Azure Blob-behållare</span><span class="sxs-lookup"><span data-stu-id="d7aca-122">Create an Azure Blob container</span></span>
* <span data-ttu-id="d7aca-123">Skapa ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="d7aca-123">Create an HDInsight cluster</span></span>

<span data-ttu-id="d7aca-124">hello följande skript visar hur toocreate ett nytt kluster:</span><span class="sxs-lookup"><span data-stu-id="d7aca-124">hello following script demonstrates how toocreate a new cluster:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

<span data-ttu-id="d7aca-125">hello värdena du anger för hello klusterinloggning används toocreate hello Hadoop användarkontot för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="d7aca-125">hello values you specify for hello cluster login are used toocreate hello Hadoop user account for hello cluster.</span></span> <span data-ttu-id="d7aca-126">Använd det här kontot tooconnect tooservices hello kluster, till exempel web användargränssnitt eller REST API: er som värd.</span><span class="sxs-lookup"><span data-stu-id="d7aca-126">Use this account tooconnect tooservices hosted on hello cluster such as web UIs or REST APIs.</span></span>

<span data-ttu-id="d7aca-127">hello värden du anger för hello SSH-användare är används toocreate hello SSH-användare för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7aca-127">hello values you specify for hello SSH user are used toocreate hello SSH user for hello cluster.</span></span> <span data-ttu-id="d7aca-128">Använd det här kontot toostart en fjärransluten SSH-session på hello klustret och köra jobb.</span><span class="sxs-lookup"><span data-stu-id="d7aca-128">Use this account toostart a remote SSH session on hello cluster and run jobs.</span></span> <span data-ttu-id="d7aca-129">Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="d7aca-129">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d7aca-130">Om du planerar toouse mer än 32 arbetarnoder (när klustret skapas eller genom att skala hello klustret när den har skapats), måste du också ange en storlek för huvudnod med minst 8 kärnor och 14 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="d7aca-130">If you plan toouse more than 32 worker nodes (either at cluster creation or by scaling hello cluster after creation), you must also specify a head node size with at least 8 cores and 14 GB of RAM.</span></span>
>
> <span data-ttu-id="d7aca-131">Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="d7aca-131">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="d7aca-132">Det kan ta upp too20 minuter toocreate ett kluster.</span><span class="sxs-lookup"><span data-stu-id="d7aca-132">It can take up too20 minutes toocreate a cluster.</span></span>

## <a name="create-cluster-configuration-object"></a><span data-ttu-id="d7aca-133">Skapa kluster: konfigurationsobjekt</span><span class="sxs-lookup"><span data-stu-id="d7aca-133">Create cluster: Configuration object</span></span>

<span data-ttu-id="d7aca-134">Du kan också skapa ett HDInsight configuration objekt med hjälp av `New-AzureRmHDInsightClusterConfig` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d7aca-134">You can also create an HDInsight configuration object using `New-AzureRmHDInsightClusterConfig` cmdlet.</span></span> <span data-ttu-id="d7aca-135">Du kan sedan ändra den här konfigurationen objektet tooenable ytterligare konfigurationsalternativ för klustret.</span><span class="sxs-lookup"><span data-stu-id="d7aca-135">You can then modify this configuration object tooenable additional configuration options for your cluster.</span></span> <span data-ttu-id="d7aca-136">Använd slutligen hello `-Config` parametern för hello `New-AzureRmHDInsightCluster` cmdlet toouse hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d7aca-136">Finally, use hello `-Config` parameter of hello `New-AzureRmHDInsightCluster` cmdlet toouse hello configuration.</span></span>

<span data-ttu-id="d7aca-137">hello skapar följande skript ett configuration-objekt tooconfigure R-Server på typ av HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7aca-137">hello following script creates a configuration object tooconfigure an R Server on HDInsight cluster type.</span></span> <span data-ttu-id="d7aca-138">hello konfiguration kan en kantnod och RStudio ett ytterligare storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d7aca-138">hello configuration enables an edge node, RStudio, and an additional storage account.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> <span data-ttu-id="d7aca-139">Med hjälp av ett lagringskonto i en annan plats än hello HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="d7aca-139">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span> <span data-ttu-id="d7aca-140">När du använder det här exemplet kan du skapa hello ytterligare lagringskonto i hello samma plats som hello-server.</span><span class="sxs-lookup"><span data-stu-id="d7aca-140">When using this example, create hello additional storage account in hello same location as hello server.</span></span>

## <a name="customize-clusters"></a><span data-ttu-id="d7aca-141">Anpassa kluster</span><span class="sxs-lookup"><span data-stu-id="d7aca-141">Customize clusters</span></span>

* <span data-ttu-id="d7aca-142">Se [anpassa HDInsight-kluster med Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="d7aca-142">See [Customize HDInsight clusters using Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).</span></span>
* <span data-ttu-id="d7aca-143">Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d7aca-143">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="d7aca-144">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="d7aca-144">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="d7aca-145">Felsöka</span><span class="sxs-lookup"><span data-stu-id="d7aca-145">Troubleshoot</span></span>

<span data-ttu-id="d7aca-146">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="d7aca-146">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7aca-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7aca-147">Next steps</span></span>

<span data-ttu-id="d7aca-148">Nu när du har skapat ett HDInsight-kluster, Använd följande resurser toolearn hur hello toowork med ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="d7aca-148">Now that you have successfully created an HDInsight cluster, use hello following resources toolearn how toowork with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="d7aca-149">Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="d7aca-149">Hadoop clusters</span></span>

* [<span data-ttu-id="d7aca-150">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="d7aca-150">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="d7aca-151">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="d7aca-151">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="d7aca-152">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="d7aca-152">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="d7aca-153">HBase-kluster</span><span class="sxs-lookup"><span data-stu-id="d7aca-153">HBase clusters</span></span>

* [<span data-ttu-id="d7aca-154">Kom igång med HBase på HDInsight</span><span class="sxs-lookup"><span data-stu-id="d7aca-154">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="d7aca-155">Utveckla Java-program för HBase i HDInsight</span><span class="sxs-lookup"><span data-stu-id="d7aca-155">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="d7aca-156">Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="d7aca-156">Storm clusters</span></span>

* [<span data-ttu-id="d7aca-157">Utveckla Java-topologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="d7aca-157">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="d7aca-158">Använda Python komponenter i Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="d7aca-158">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="d7aca-159">Distribuera och övervaka topologier med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="d7aca-159">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a><span data-ttu-id="d7aca-160">Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="d7aca-160">Spark clusters</span></span>

* [<span data-ttu-id="d7aca-161">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="d7aca-161">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="d7aca-162">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="d7aca-162">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)
* [<span data-ttu-id="d7aca-163">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="d7aca-163">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="d7aca-164">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="d7aca-164">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="d7aca-165">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="d7aca-165">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

