---
title: "aaaCustomize HDInsight-kluster med hjälp av skript åtgärder - Azure | Microsoft Docs"
description: "Lär dig hur toocustomize HDInsight-kluster med skriptåtgärder."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3a63e216-4163-40c1-aa04-6b42fd0162ad
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 076fff23e016db47bc7e9963582a545ad638e691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="89f6f-103">Anpassa Windows-baserade HDInsight-kluster med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="89f6f-103">Customize Windows-based HDInsight clusters using Script Action</span></span>
<span data-ttu-id="89f6f-104">**Script åtgärd** kan vara används tooinvoke [anpassade skript](hdinsight-hadoop-script-actions.md) under hello klusterskapandeprocessen för att installera ytterligare programvara på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-104">**Script Action** can be used tooinvoke [custom scripts](hdinsight-hadoop-script-actions.md) during hello cluster creation process for installing additional software on a cluster.</span></span>

<span data-ttu-id="89f6f-105">hello informationen i den här artikeln är särskilda tooWindows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-105">hello information in this article is specific tooWindows-based HDInsight clusters.</span></span> <span data-ttu-id="89f6f-106">Linux-baserade kluster, se [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="89f6f-106">For Linux-based clusters, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="89f6f-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="89f6f-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="89f6f-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="89f6f-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="89f6f-109">HDInsight-kluster kan anpassas i en mängd andra sätt, till exempel inklusive ytterligare Azure Storage-konton, ändra hello Hadoop konfigurationsfiler (core-site.xml, hive-site.xml osv.) eller att lägga till delade bibliotek (t.ex. Hive, Oozie) i vanliga platser i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-109">HDInsight clusters can be customized in a variety of other ways as well, such as including additional Azure Storage accounts, changing hello Hadoop configuration files (core-site.xml, hive-site.xml, etc.), or adding shared libraries (e.g., Hive, Oozie) into common locations in hello cluster.</span></span> <span data-ttu-id="89f6f-110">Dessa anpassningar kan göras via Azure PowerShell, hello Azure HDInsight .NET SDK eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="89f6f-110">These customizations can be done through Azure PowerShell, hello Azure HDInsight .NET SDK, or hello Azure portal.</span></span> <span data-ttu-id="89f6f-111">Mer information finns i [skapa Hadoop-kluster i HDInsight][hdinsight-provision-cluster].</span><span class="sxs-lookup"><span data-stu-id="89f6f-111">For more information, see [Create Hadoop clusters in HDInsight][hdinsight-provision-cluster].</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-hello-cluster-creation-process"></a><span data-ttu-id="89f6f-112">Skriptåtgärder i hello klusterskapandeprocessen</span><span class="sxs-lookup"><span data-stu-id="89f6f-112">Script Action in hello cluster creation process</span></span>
<span data-ttu-id="89f6f-113">Skriptåtgärder används endast när ett kluster finns i hello processen att skapas.</span><span class="sxs-lookup"><span data-stu-id="89f6f-113">Script Action is only used while a cluster is in hello process of being created.</span></span> <span data-ttu-id="89f6f-114">hello följande diagram illustrerar när skriptet körs under skapandeprocessen hello:</span><span class="sxs-lookup"><span data-stu-id="89f6f-114">hello following diagram illustrates when Script Action is executed during hello creation process:</span></span>

<span data-ttu-id="89f6f-115">![HDInsight-kluster anpassning och faserna när klustret skapas][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="89f6f-115">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="89f6f-116">När hello skriptet körs hello klustret registrerar hello **ClusterCustomization** steget.</span><span class="sxs-lookup"><span data-stu-id="89f6f-116">When hello script is running, hello cluster enters hello **ClusterCustomization** stage.</span></span> <span data-ttu-id="89f6f-117">I det här skedet hello skript körs under hello systemkontot admin, parallellt på alla hello angivna noder i klustret hello och ger fullständig admin-privilegier på hello noder.</span><span class="sxs-lookup"><span data-stu-id="89f6f-117">At this stage, hello script is run under hello system admin account, in parallel on all hello specified nodes in hello cluster, and provides full admin privileges on hello nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="89f6f-118">Eftersom du har administratörsrättigheter på hello klusternoder under den **ClusterCustomization** steg, som du kan använda hello skriptet tooperform åtgärder som att stoppa och starta tjänster, inklusive Hadoop-relaterade tjänster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-118">Because you have admin privileges on hello cluster nodes during the **ClusterCustomization** stage, you can use hello script tooperform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="89f6f-119">Så som en del av hello skript, måste du se till att hello Ambari och andra Hadoop-relaterade tjänsterna är igång innan hello skriptet är klar.</span><span class="sxs-lookup"><span data-stu-id="89f6f-119">So, as part of hello script, you must ensure that hello Ambari services and other Hadoop-related services are up and running before hello script finishes running.</span></span> <span data-ttu-id="89f6f-120">De här tjänsterna krävs toosuccessfully fastställa hello hälsa och status för hello klustret medan det skapas.</span><span class="sxs-lookup"><span data-stu-id="89f6f-120">These services are required toosuccessfully ascertain hello health and state of hello cluster while it is being created.</span></span> <span data-ttu-id="89f6f-121">Om du ändrar någon konfiguration på det kluster som påverkar dessa tjänster måste du använda hello hjälpfunktioner som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="89f6f-121">If you change any configuration on the cluster that affects these services, you must use hello helper functions that are provided.</span></span> <span data-ttu-id="89f6f-122">Läs mer om hjälpfunktioner [utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="89f6f-122">For more information about helper functions, see [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>
>
>

<span data-ttu-id="89f6f-123">hello utdata och hello felloggarna hello skriptet lagras i hello standardkontot för lagring du angett för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="89f6f-123">hello output and hello error logs for hello script are stored in hello default Storage account you specified for hello cluster.</span></span> <span data-ttu-id="89f6f-124">hello loggfilerna lagras i en tabell med namnet hello **u < \cluster-name-fragment >< \time-stamp > setuplog**.</span><span class="sxs-lookup"><span data-stu-id="89f6f-124">hello logs are stored in a table with hello name **u<\cluster-name-fragment><\time-stamp>setuplog**.</span></span> <span data-ttu-id="89f6f-125">Det här är sammanställda loggar från hello skript köras på alla hello noder (huvudnod och arbetarnoder) i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-125">These are aggregate logs from hello script run on all hello nodes (head node and worker nodes) in hello cluster.</span></span>

<span data-ttu-id="89f6f-126">Varje kluster kan acceptera flera skriptåtgärder som startas i hello ordning som de har angetts.</span><span class="sxs-lookup"><span data-stu-id="89f6f-126">Each cluster can accept multiple script actions that are invoked in hello order in which they are specified.</span></span> <span data-ttu-id="89f6f-127">Ett skript kan vara kördes på hello huvudnod, hello arbetarnoder eller båda.</span><span class="sxs-lookup"><span data-stu-id="89f6f-127">A script can be ran on hello head node, hello worker nodes, or both.</span></span>

<span data-ttu-id="89f6f-128">HDInsight tillhandahåller flera skript tooinstall hello följande komponenter i HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="89f6f-128">HDInsight provides several scripts tooinstall hello following components on HDInsight clusters:</span></span>

| <span data-ttu-id="89f6f-129">Namn</span><span class="sxs-lookup"><span data-stu-id="89f6f-129">Name</span></span> | <span data-ttu-id="89f6f-130">Skript</span><span class="sxs-lookup"><span data-stu-id="89f6f-130">Script</span></span> |
| --- | --- |
| <span data-ttu-id="89f6f-131">**Installera Spark**</span><span class="sxs-lookup"><span data-stu-id="89f6f-131">**Install Spark**</span></span> |<span data-ttu-id="89f6f-132">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="89f6f-132">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="89f6f-133">Se [installera och använda Spark i HDInsight-kluster][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="89f6f-133">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="89f6f-134">**Installera R**</span><span class="sxs-lookup"><span data-stu-id="89f6f-134">**Install R**</span></span> |<span data-ttu-id="89f6f-135">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="89f6f-135">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="89f6f-136">Se [installera och använda R i HDInsight-kluster][hdinsight-install-r].</span><span class="sxs-lookup"><span data-stu-id="89f6f-136">See [Install and use R on HDInsight clusters][hdinsight-install-r].</span></span> |
| <span data-ttu-id="89f6f-137">**Installera Solr**</span><span class="sxs-lookup"><span data-stu-id="89f6f-137">**Install Solr**</span></span> |<span data-ttu-id="89f6f-138">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="89f6f-138">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="89f6f-139">Se [installerar och använder Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="89f6f-139">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="89f6f-140">- **Installera Giraph**</span><span class="sxs-lookup"><span data-stu-id="89f6f-140">- **Install Giraph**</span></span> |<span data-ttu-id="89f6f-141">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="89f6f-141">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="89f6f-142">Se [installerar och använder Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="89f6f-142">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |
| <span data-ttu-id="89f6f-143">**Läsa in Hive-bibliotek**</span><span class="sxs-lookup"><span data-stu-id="89f6f-143">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="89f6f-144">https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="89f6f-144">https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1.</span></span> <span data-ttu-id="89f6f-145">Se [lägga till Hive-bibliotek i HDInsight-kluster](hdinsight-hadoop-add-hive-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="89f6f-145">See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md)</span></span> |

## <a name="call-scripts-using-hello-azure-portal"></a><span data-ttu-id="89f6f-146">Anropa skript med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="89f6f-146">Call scripts using hello Azure portal</span></span>
<span data-ttu-id="89f6f-147">**Från hello Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="89f6f-147">**From hello Azure portal**</span></span>

1. <span data-ttu-id="89f6f-148">Skapa ett kluster, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="89f6f-148">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
2. <span data-ttu-id="89f6f-149">Under valfri konfiguration för hello **skriptåtgärder** bladet, klickar du på **lägga till skriptåtgärd** tooprovide information om hello skriptåtgärd enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="89f6f-149">Under Optional Configuration, for hello **Script Actions** blade, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="89f6f-150">![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Använd skriptåtgärder toocustomize ett kluster")</span><span class="sxs-lookup"><span data-stu-id="89f6f-150">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="89f6f-151">Egenskap</span><span class="sxs-lookup"><span data-stu-id="89f6f-151">Property</span></span></th><th><span data-ttu-id="89f6f-152">Värde</span><span class="sxs-lookup"><span data-stu-id="89f6f-152">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="89f6f-153">Namn</span><span class="sxs-lookup"><span data-stu-id="89f6f-153">Name</span></span></td>
            <td><span data-ttu-id="89f6f-154">Ange ett namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="89f6f-154">Specify a name for hello script action.</span></span></td></tr>
        <tr><td><span data-ttu-id="89f6f-155">Skript-URI</span><span class="sxs-lookup"><span data-stu-id="89f6f-155">Script URI</span></span></td>
            <td><span data-ttu-id="89f6f-156">Ange hello URI toohello skript som är anropade toocustomize hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-156">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="89f6f-157">S</span><span class="sxs-lookup"><span data-stu-id="89f6f-157">s</span></span></td></tr>
        <tr><td><span data-ttu-id="89f6f-158">HEAD/Worker</span><span class="sxs-lookup"><span data-stu-id="89f6f-158">Head/Worker</span></span></td>
            <td><span data-ttu-id="89f6f-159">Ange hello noder (**Head** eller **Worker**) på vilken hello anpassning skript körs.</b>.</span><span class="sxs-lookup"><span data-stu-id="89f6f-159">Specify hello nodes (**Head** or **Worker**) on which hello customization script is run.</b>.</span></span>
        <tr><td><span data-ttu-id="89f6f-160">Parametrar</span><span class="sxs-lookup"><span data-stu-id="89f6f-160">Parameters</span></span></td>
            <td><span data-ttu-id="89f6f-161">Ange hello parametrar, om det krävs av hello skript.</span><span class="sxs-lookup"><span data-stu-id="89f6f-161">Specify hello parameters, if required by hello script.</span></span></td></tr>
    </table>

    <span data-ttu-id="89f6f-162">Tryck på RETUR tooadd mer än ett skript åtgärd tooinstall flera komponenter på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="89f6f-162">Press ENTER tooadd more than one script action tooinstall multiple components on hello cluster.</span></span>
3. <span data-ttu-id="89f6f-163">Klicka på **Välj** toosave hello skript konfigurationen och fortsätta med skapa ett kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-163">Click **Select** toosave hello script action configuration and continue with cluster creation.</span></span>

## <a name="call-scripts-using-azure-powershell"></a><span data-ttu-id="89f6f-164">Anropa skript med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="89f6f-164">Call scripts using Azure PowerShell</span></span>
<span data-ttu-id="89f6f-165">Den här följande PowerShell-skript visar hur tooinstall Spark på Windows baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-165">This following PowerShell script demonstrates how tooinstall Spark on Windows based HDInsight cluster.</span></span>  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" toolist IDs.

    $nameToken = "<Enter A Name Token>"  # hello token is use toocreate Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect tooAzure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare hello dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify hello configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action toohello cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


<span data-ttu-id="89f6f-166">tooinstall andra program, behöver du tooreplace hello skriptfilen i hello skript:</span><span class="sxs-lookup"><span data-stu-id="89f6f-166">tooinstall other software, you will need tooreplace hello script file in hello script:</span></span>

<span data-ttu-id="89f6f-167">När du uppmanas du ange hello autentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="89f6f-167">When prompted, enter hello credentials for hello cluster.</span></span> <span data-ttu-id="89f6f-168">Det kan ta flera minuter innan hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="89f6f-168">It can take several minutes before hello cluster is created.</span></span>

## <a name="call-scripts-using-net-sdk"></a><span data-ttu-id="89f6f-169">Anropa skript med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="89f6f-169">Call scripts using .NET SDK</span></span>
<span data-ttu-id="89f6f-170">hello visar följande exempel hur tooinstall Spark på Windows baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-170">hello following sample demonstrates how tooinstall Spark on Windows based HDInsight cluster.</span></span> <span data-ttu-id="89f6f-171">tooinstall andra program, behöver du tooreplace hello skriptfilen i hello kod.</span><span class="sxs-lookup"><span data-stu-id="89f6f-171">tooinstall other software, you will need tooreplace hello script file in hello code.</span></span>

<span data-ttu-id="89f6f-172">**toocreate ett HDInsight-kluster med Spark**</span><span class="sxs-lookup"><span data-stu-id="89f6f-172">**toocreate an HDInsight cluster with Spark**</span></span>

1. <span data-ttu-id="89f6f-173">Skapa ett C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89f6f-173">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="89f6f-174">Kör följande kommando hello från hello Nuget Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="89f6f-174">From hello Nuget Package Manager Console, run hello following command.</span></span>

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. <span data-ttu-id="89f6f-175">Använd hello följande using-instruktioner i hello Program.cs-filen:</span><span class="sxs-lookup"><span data-stu-id="89f6f-175">Use hello following using statements in hello Program.cs file:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. <span data-ttu-id="89f6f-176">Placera hello koden i hello klass med hello följande:</span><span class="sxs-lookup"><span data-stu-id="89f6f-176">Place hello code in hello class with hello following:</span></span>

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is hello GUID for hello PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate tooan Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">hello AAD tenant ID</param>
        /// <param name="ClientId">hello AAD client ID</param>
        /// <param name="SubscriptionId">hello Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/",
                ClientId,
                new Uri("urn:ietf:wg:oauth:2.0:oob"),
                PromptBehavior.Always,
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for hello Resource manager and set hello subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register hello HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. <span data-ttu-id="89f6f-177">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="89f6f-177">Press **F5** toorun hello application.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="89f6f-178">Stöd för öppen källkod programvara som används i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="89f6f-178">Support for open-source software used on HDInsight clusters</span></span>
<span data-ttu-id="89f6f-179">hello Microsoft Azure HDInsight-tjänst är en flexibel plattform som du kan använda toobuild-stordataprogram i hello molnet med hjälp av ett ekosystem med öppen källkod tekniker formaterat runt Hadoop.</span><span class="sxs-lookup"><span data-stu-id="89f6f-179">hello Microsoft Azure HDInsight service is a flexible platform that enables you toobuild big-data applications in hello cloud by using an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="89f6f-180">Microsoft Azure tillhandahåller en allmän supportnivå för öppen källkod tekniker som beskrivs i hello **stöd för Scope** avsnitt i hello <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ webbplats</a>.</span><span class="sxs-lookup"><span data-stu-id="89f6f-180">Microsoft Azure provides a general level of support for open-source technologies, as discussed in hello **Support Scope** section of hello <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ website</a>.</span></span> <span data-ttu-id="89f6f-181">Hej HDInsight-tjänst ger ytterligare en säkerhetsnivå för stöd för några av hello komponenter som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="89f6f-181">hello HDInsight service provides an additional level of support for some of hello components, as described below.</span></span>

<span data-ttu-id="89f6f-182">Det finns två typer av komponenter som öppen källkod som är tillgängliga i hello HDInsight-tjänst:</span><span class="sxs-lookup"><span data-stu-id="89f6f-182">There are two types of open-source components that are available in hello HDInsight service:</span></span>

* <span data-ttu-id="89f6f-183">**Inbyggda komponenter** -komponenterna är förinstallerat på HDInsight-kluster och tillhandahåller huvudfunktionerna i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="89f6f-183">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of hello cluster.</span></span> <span data-ttu-id="89f6f-184">Till exempel tillhör YARN resurshanteraren, hello Hive-frågespråket (HiveQL) och hello Mahout biblioteksresurser toothis kategori.</span><span class="sxs-lookup"><span data-stu-id="89f6f-184">For example, YARN ResourceManager, hello Hive query language (HiveQL), and hello Mahout library belong toothis category.</span></span> <span data-ttu-id="89f6f-185">En fullständig lista över komponenter i serverkluster finns i [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md) </a>.</span><span class="sxs-lookup"><span data-stu-id="89f6f-185">A full list of cluster components is available in [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md)</a>.</span></span>
* <span data-ttu-id="89f6f-186">**Anpassade komponenter** -du som användare av hello klustret kan installera eller använda i din arbetsbelastning någon komponent som är tillgängliga i hello community eller skapades av du.</span><span class="sxs-lookup"><span data-stu-id="89f6f-186">**Custom components** - You, as a user of hello cluster, can install or use in your workload any component available in hello community or created by you.</span></span>

<span data-ttu-id="89f6f-187">Inbyggda komponenter stöds fullt ut och Microsoft Support kommer att tooisolate och lösa problem relaterade toothese komponenter.</span><span class="sxs-lookup"><span data-stu-id="89f6f-187">Built-in components are fully supported, and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>

> [!WARNING]
> <span data-ttu-id="89f6f-188">Komponenter som ingår i hello HDInsight-kluster stöds fullt ut och Microsoft Support kommer att tooisolate och lösa problem relaterade toothese komponenter.</span><span class="sxs-lookup"><span data-stu-id="89f6f-188">Components provided with hello HDInsight cluster are fully supported and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="89f6f-189">Anpassade komponenter får stöd för kommersiellt rimliga toohelp du toofurther hello felsökning.</span><span class="sxs-lookup"><span data-stu-id="89f6f-189">Custom components receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="89f6f-190">Detta kan resultera i att lösa problemet hello eller be tooengage tillgängliga kanaler för hello öppnas teknikerna där djup expertis för att teknik finns.</span><span class="sxs-lookup"><span data-stu-id="89f6f-190">This might result in resolving hello issue OR asking you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="89f6f-191">Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="89f6f-191">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span></span>
>
>

<span data-ttu-id="89f6f-192">Hej HDInsight-tjänst finns flera sätt toouse anpassade komponenter.</span><span class="sxs-lookup"><span data-stu-id="89f6f-192">hello HDInsight service provides several ways toouse custom components.</span></span> <span data-ttu-id="89f6f-193">Oavsett hur en komponent används eller installeras på hello-kluster, gäller hello samma supportnivå.</span><span class="sxs-lookup"><span data-stu-id="89f6f-193">Regardless of how a component is used or installed on hello cluster, hello same level of support applies.</span></span> <span data-ttu-id="89f6f-194">Nedan visas en lista över hello vanligaste sätten att anpassade komponenter kan användas på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="89f6f-194">Below is a list of hello most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="89f6f-195">Jobbet - Hadoop och andra typer av jobb som kör eller använda anpassade komponenter kan vara skickade toohello.</span><span class="sxs-lookup"><span data-stu-id="89f6f-195">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted toohello cluster.</span></span>
2. <span data-ttu-id="89f6f-196">Anpassning av kluster - när klustret skapas som du kan ange ytterligare inställningar och anpassade komponenter som ska installeras på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="89f6f-196">Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on hello cluster nodes.</span></span>
3. <span data-ttu-id="89f6f-197">Exempel - för populära anpassade komponenter, Microsoft och andra kan ge exempel på hur du kan använda de här komponenterna på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="89f6f-197">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on hello HDInsight clusters.</span></span> <span data-ttu-id="89f6f-198">De här exemplen tillhandahålls utan stöd.</span><span class="sxs-lookup"><span data-stu-id="89f6f-198">These samples are provided without support.</span></span>

## <a name="develop-script-action-scripts"></a><span data-ttu-id="89f6f-199">Utveckla skriptåtgärd skript</span><span class="sxs-lookup"><span data-stu-id="89f6f-199">Develop Script Action scripts</span></span>
<span data-ttu-id="89f6f-200">Se [utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="89f6f-200">See [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>

## <a name="see-also"></a><span data-ttu-id="89f6f-201">Se även</span><span class="sxs-lookup"><span data-stu-id="89f6f-201">See also</span></span>
* <span data-ttu-id="89f6f-202">[Skapa Hadoop-kluster i HDInsight] [ hdinsight-provision-cluster] innehåller instruktioner om hur toocreate ett HDInsight-kluster med hjälp av andra anpassade alternativ.</span><span class="sxs-lookup"><span data-stu-id="89f6f-202">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how toocreate an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="89f6f-203">[Utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="89f6f-203">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="89f6f-204">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="89f6f-204">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="89f6f-205">[Installera och använda R i HDInsight-kluster][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="89f6f-205">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="89f6f-206">[Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="89f6f-206">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="89f6f-207">[Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="89f6f-207">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Steg när klustret skapas"
