---
title: "Anpassa HDInsight-kluster med skriptåtgärder - Azure | Microsoft Docs"
description: "Lär dig hur du anpassar HDInsight-kluster med skriptåtgärder."
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
ms.openlocfilehash: ec95b6d66c71b4278dd1e16807fcc75f5e8b1c36
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="e5cd7-103">Anpassa Windows-baserade HDInsight-kluster med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="e5cd7-103">Customize Windows-based HDInsight clusters using Script Action</span></span>
<span data-ttu-id="e5cd7-104">**Script åtgärd** kan användas för att anropa [anpassade skript](hdinsight-hadoop-script-actions.md) när klustret skapas för att installera ytterligare programvara på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-104">**Script Action** can be used to invoke [custom scripts](hdinsight-hadoop-script-actions.md) during the cluster creation process for installing additional software on a cluster.</span></span>

<span data-ttu-id="e5cd7-105">Informationen i den här artikeln är specifik för Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-105">The information in this article is specific to Windows-based HDInsight clusters.</span></span> <span data-ttu-id="e5cd7-106">Linux-baserade kluster, se [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-106">For Linux-based clusters, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e5cd7-107">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e5cd7-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="e5cd7-109">HDInsight-kluster kan anpassas i en mängd andra sätt, till exempel inklusive ytterligare Azure Storage-konton, ändringar i Hadoop konfigurationsfiler (core-site.xml, hive-site.xml osv.), eller lägga till delade bibliotek (t.ex. Hive, Oozie) till vanliga platser i klustret.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-109">HDInsight clusters can be customized in a variety of other ways as well, such as including additional Azure Storage accounts, changing the Hadoop configuration files (core-site.xml, hive-site.xml, etc.), or adding shared libraries (e.g., Hive, Oozie) into common locations in the cluster.</span></span> <span data-ttu-id="e5cd7-110">Dessa anpassningar kan göras via Azure PowerShell, Azure HDInsight .NET SDK eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-110">These customizations can be done through Azure PowerShell, the Azure HDInsight .NET SDK, or the Azure portal.</span></span> <span data-ttu-id="e5cd7-111">Mer information finns i [skapa Hadoop-kluster i HDInsight][hdinsight-provision-cluster].</span><span class="sxs-lookup"><span data-stu-id="e5cd7-111">For more information, see [Create Hadoop clusters in HDInsight][hdinsight-provision-cluster].</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a><span data-ttu-id="e5cd7-112">Skriptåtgärder i klusterskapandeprocessen</span><span class="sxs-lookup"><span data-stu-id="e5cd7-112">Script Action in the cluster creation process</span></span>
<span data-ttu-id="e5cd7-113">Skriptåtgärder används endast när ett kluster som håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-113">Script Action is only used while a cluster is in the process of being created.</span></span> <span data-ttu-id="e5cd7-114">Följande diagram illustrerar när skriptet körs under skapandeprocessen:</span><span class="sxs-lookup"><span data-stu-id="e5cd7-114">The following diagram illustrates when Script Action is executed during the creation process:</span></span>

<span data-ttu-id="e5cd7-115">![HDInsight-kluster anpassning och faserna när klustret skapas][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="e5cd7-115">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="e5cd7-116">När skriptet körs klustret försätts den **ClusterCustomization** steget.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-116">When the script is running, the cluster enters the **ClusterCustomization** stage.</span></span> <span data-ttu-id="e5cd7-117">I det här skedet skriptet körs under kontot system admin parallellt på noderna i klustret, och ger fullständig admin-privilegier på noderna.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-117">At this stage, the script is run under the system admin account, in parallel on all the specified nodes in the cluster, and provides full admin privileges on the nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="e5cd7-118">Eftersom du har administratörsrättigheter på klusternoder under den **ClusterCustomization** skede, du kan använda skriptet för att utföra åtgärder som att stoppa och starta tjänster, inklusive Hadoop-relaterade tjänster.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-118">Because you have admin privileges on the cluster nodes during the **ClusterCustomization** stage, you can use the script to perform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="e5cd7-119">Därför som en del av skript, måste du kontrollera att tjänsterna Ambari och andra Hadoop-relaterade tjänster är igång innan körningen för skriptet.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-119">So, as part of the script, you must ensure that the Ambari services and other Hadoop-related services are up and running before the script finishes running.</span></span> <span data-ttu-id="e5cd7-120">De här tjänsterna krävs har kännedom om hälsa och tillstånd för klustret medan det skapas.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-120">These services are required to successfully ascertain the health and state of the cluster while it is being created.</span></span> <span data-ttu-id="e5cd7-121">Om du ändrar någon konfiguration på det kluster som påverkar dessa tjänster måste du använda hjälpfunktioner som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-121">If you change any configuration on the cluster that affects these services, you must use the helper functions that are provided.</span></span> <span data-ttu-id="e5cd7-122">Läs mer om hjälpfunktioner [utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="e5cd7-122">For more information about helper functions, see [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>
>
>

<span data-ttu-id="e5cd7-123">Utdata och i felloggarna för skriptet lagras i standardkontot för lagring som du angav för klustret.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-123">The output and the error logs for the script are stored in the default Storage account you specified for the cluster.</span></span> <span data-ttu-id="e5cd7-124">Loggfilerna lagras i en tabell med namnet **u < \cluster-name-fragment >< \time-stamp > setuplog**.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-124">The logs are stored in a table with the name **u<\cluster-name-fragment><\time-stamp>setuplog**.</span></span> <span data-ttu-id="e5cd7-125">Det här är sammanställda loggar från skriptet köras på alla noder (huvudnod och arbetarnoder) i klustret.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-125">These are aggregate logs from the script run on all the nodes (head node and worker nodes) in the cluster.</span></span>

<span data-ttu-id="e5cd7-126">Varje kluster kan acceptera flera skriptåtgärder som anropas i den ordning som de har angetts.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-126">Each cluster can accept multiple script actions that are invoked in the order in which they are specified.</span></span> <span data-ttu-id="e5cd7-127">Ett skript kan vara kördes på huvudnoden, arbetsnoderna eller båda.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-127">A script can be ran on the head node, the worker nodes, or both.</span></span>

<span data-ttu-id="e5cd7-128">HDInsight tillhandahåller flera skript för att installera följande komponenter i HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="e5cd7-128">HDInsight provides several scripts to install the following components on HDInsight clusters:</span></span>

| <span data-ttu-id="e5cd7-129">Namn</span><span class="sxs-lookup"><span data-stu-id="e5cd7-129">Name</span></span> | <span data-ttu-id="e5cd7-130">Skript</span><span class="sxs-lookup"><span data-stu-id="e5cd7-130">Script</span></span> |
| --- | --- |
| <span data-ttu-id="e5cd7-131">**Installera Spark**</span><span class="sxs-lookup"><span data-stu-id="e5cd7-131">**Install Spark**</span></span> |<span data-ttu-id="e5cd7-132">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-132">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="e5cd7-133">Se [installera och använda Spark i HDInsight-kluster][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="e5cd7-133">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="e5cd7-134">**Installera R**</span><span class="sxs-lookup"><span data-stu-id="e5cd7-134">**Install R**</span></span> |<span data-ttu-id="e5cd7-135">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-135">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="e5cd7-136">Se [installera och använda R i HDInsight-kluster][hdinsight-install-r].</span><span class="sxs-lookup"><span data-stu-id="e5cd7-136">See [Install and use R on HDInsight clusters][hdinsight-install-r].</span></span> |
| <span data-ttu-id="e5cd7-137">**Installera Solr**</span><span class="sxs-lookup"><span data-stu-id="e5cd7-137">**Install Solr**</span></span> |<span data-ttu-id="e5cd7-138">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-138">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="e5cd7-139">Se [installerar och använder Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-139">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="e5cd7-140">- **Installera Giraph**</span><span class="sxs-lookup"><span data-stu-id="e5cd7-140">- **Install Giraph**</span></span> |<span data-ttu-id="e5cd7-141">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-141">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="e5cd7-142">Se [installerar och använder Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-142">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |
| <span data-ttu-id="e5cd7-143">**Läsa in Hive-bibliotek**</span><span class="sxs-lookup"><span data-stu-id="e5cd7-143">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="e5cd7-144">https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-144">https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1.</span></span> <span data-ttu-id="e5cd7-145">Se [lägga till Hive-bibliotek i HDInsight-kluster](hdinsight-hadoop-add-hive-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="e5cd7-145">See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md)</span></span> |

## <a name="call-scripts-using-the-azure-portal"></a><span data-ttu-id="e5cd7-146">Anropa skript med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="e5cd7-146">Call scripts using the Azure portal</span></span>
<span data-ttu-id="e5cd7-147">**Från Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="e5cd7-147">**From the Azure portal**</span></span>

1. <span data-ttu-id="e5cd7-148">Skapa ett kluster, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-148">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
2. <span data-ttu-id="e5cd7-149">Under valfri konfiguration för den **skriptåtgärder** bladet, klickar du på **lägga till skriptåtgärd** att ge information om skriptåtgärd, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="e5cd7-149">Under Optional Configuration, for the **Script Actions** blade, click **add script action** to provide details about the script action, as shown below:</span></span>

    <span data-ttu-id="e5cd7-150">![Använd skriptåtgärder för att anpassa ett kluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Använd skriptåtgärder att anpassa ett kluster")</span><span class="sxs-lookup"><span data-stu-id="e5cd7-150">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="e5cd7-151">Egenskap</span><span class="sxs-lookup"><span data-stu-id="e5cd7-151">Property</span></span></th><th><span data-ttu-id="e5cd7-152">Värde</span><span class="sxs-lookup"><span data-stu-id="e5cd7-152">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="e5cd7-153">Namn</span><span class="sxs-lookup"><span data-stu-id="e5cd7-153">Name</span></span></td>
            <td><span data-ttu-id="e5cd7-154">Ange ett namn för skriptåtgärden.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-154">Specify a name for the script action.</span></span></td></tr>
        <tr><td><span data-ttu-id="e5cd7-155">Skript-URI</span><span class="sxs-lookup"><span data-stu-id="e5cd7-155">Script URI</span></span></td>
            <td><span data-ttu-id="e5cd7-156">Ange URI till det skript som anropas för att anpassa klustret.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-156">Specify the URI to the script that is invoked to customize the cluster.</span></span> <span data-ttu-id="e5cd7-157">S</span><span class="sxs-lookup"><span data-stu-id="e5cd7-157">s</span></span></td></tr>
        <tr><td><span data-ttu-id="e5cd7-158">HEAD/Worker</span><span class="sxs-lookup"><span data-stu-id="e5cd7-158">Head/Worker</span></span></td>
            <td><span data-ttu-id="e5cd7-159">Ange noderna (**Head** eller **Worker**) som anpassning skriptet körs.</b>.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-159">Specify the nodes (**Head** or **Worker**) on which the customization script is run.</b>.</span></span>
        <tr><td><span data-ttu-id="e5cd7-160">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e5cd7-160">Parameters</span></span></td>
            <td><span data-ttu-id="e5cd7-161">Ange parametrar, om det krävs av skriptet.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-161">Specify the parameters, if required by the script.</span></span></td></tr>
    </table>

    <span data-ttu-id="e5cd7-162">Tryck på RETUR för att lägga till fler än en skriptåtgärd för att installera flera komponenter i klustret.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-162">Press ENTER to add more than one script action to install multiple components on the cluster.</span></span>
3. <span data-ttu-id="e5cd7-163">Klicka på **Välj** spara skriptet åtgärd konfigurationen och fortsätta med skapa ett kluster.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-163">Click **Select** to save the script action configuration and continue with cluster creation.</span></span>

## <a name="call-scripts-using-azure-powershell"></a><span data-ttu-id="e5cd7-164">Anropa skript med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e5cd7-164">Call scripts using Azure PowerShell</span></span>
<span data-ttu-id="e5cd7-165">Den här följande PowerShell-skript visar hur du installerar Spark på Windows-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-165">This following PowerShell script demonstrates how to install Spark on Windows based HDInsight cluster.</span></span>  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect to Azure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare the dependent components
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

    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action to the cluster configuration
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


<span data-ttu-id="e5cd7-166">Om du vill installera andra program, måste du ersätta skriptfilen i skriptet:</span><span class="sxs-lookup"><span data-stu-id="e5cd7-166">To install other software, you will need to replace the script file in the script:</span></span>

<span data-ttu-id="e5cd7-167">När du uppmanas, anger du autentiseringsuppgifterna för klustret.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-167">When prompted, enter the credentials for the cluster.</span></span> <span data-ttu-id="e5cd7-168">Det kan ta flera minuter innan klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-168">It can take several minutes before the cluster is created.</span></span>

## <a name="call-scripts-using-net-sdk"></a><span data-ttu-id="e5cd7-169">Anropa skript med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="e5cd7-169">Call scripts using .NET SDK</span></span>
<span data-ttu-id="e5cd7-170">I följande exempel visar hur du installerar Spark på Windows-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-170">The following sample demonstrates how to install Spark on Windows based HDInsight cluster.</span></span> <span data-ttu-id="e5cd7-171">Om du vill installera annan programvara som behöver du ersätta skriptfilen i koden.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-171">To install other software, you will need to replace the script file in the code.</span></span>

<span data-ttu-id="e5cd7-172">**Så här skapar du ett HDInsight-kluster med Spark**</span><span class="sxs-lookup"><span data-stu-id="e5cd7-172">**To create an HDInsight cluster with Spark**</span></span>

1. <span data-ttu-id="e5cd7-173">Skapa ett C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-173">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="e5cd7-174">Kör följande kommando från Nuget Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-174">From the Nuget Package Manager Console, run the following command.</span></span>

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. <span data-ttu-id="e5cd7-175">Använd följande using-instruktioner i filen Program.cs:</span><span class="sxs-lookup"><span data-stu-id="e5cd7-175">Use the following using statements in the Program.cs file:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. <span data-ttu-id="e5cd7-176">Placera koden i klassen med följande:</span><span class="sxs-lookup"><span data-stu-id="e5cd7-176">Place the code in the class with the following:</span></span>

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
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
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
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
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. <span data-ttu-id="e5cd7-177">Tryck på **F5** för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-177">Press **F5** to run the application.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="e5cd7-178">Stöd för öppen källkod programvara som används i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="e5cd7-178">Support for open-source software used on HDInsight clusters</span></span>
<span data-ttu-id="e5cd7-179">Tjänsten Microsoft Azure HDInsight är en flexibel plattform som låter dig skapa stordataprogram i molnet med hjälp av ett ekosystem med öppen källkod tekniker formaterat runt Hadoop.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-179">The Microsoft Azure HDInsight service is a flexible platform that enables you to build big-data applications in the cloud by using an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="e5cd7-180">Microsoft Azure tillhandahåller en allmän supportnivå för öppen källkod tekniker som beskrivs i den **stöd för Scope** avsnitt i den <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ webbplats</a>.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-180">Microsoft Azure provides a general level of support for open-source technologies, as discussed in the **Support Scope** section of the <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ website</a>.</span></span> <span data-ttu-id="e5cd7-181">HDInsight-tjänst ger ytterligare en säkerhetsnivå för stöd för några av komponenterna som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-181">The HDInsight service provides an additional level of support for some of the components, as described below.</span></span>

<span data-ttu-id="e5cd7-182">Det finns två typer av komponenter som öppen källkod som är tillgängliga i HDInsight-tjänst:</span><span class="sxs-lookup"><span data-stu-id="e5cd7-182">There are two types of open-source components that are available in the HDInsight service:</span></span>

* <span data-ttu-id="e5cd7-183">**Inbyggda komponenter** -komponenterna är förinstallerat på HDInsight-kluster och tillhandahåller huvudfunktionerna i klustret.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-183">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of the cluster.</span></span> <span data-ttu-id="e5cd7-184">Till exempel YARN ResourceManager Hive-frågespråket (HiveQL) och Mahout biblioteket som hör till den här kategorin.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-184">For example, YARN ResourceManager, the Hive query language (HiveQL), and the Mahout library belong to this category.</span></span> <span data-ttu-id="e5cd7-185">En fullständig lista över komponenter i serverkluster finns i [vad är nytt i Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md) </a>.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-185">A full list of cluster components is available in [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md)</a>.</span></span>
* <span data-ttu-id="e5cd7-186">**Anpassade komponenter** -kan du som användare i klustret, installera eller använda i din arbetsbelastning någon komponent som är tillgängliga i communityn eller skapades av du.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-186">**Custom components** - You, as a user of the cluster, can install or use in your workload any component available in the community or created by you.</span></span>

<span data-ttu-id="e5cd7-187">Inbyggda komponenter stöds fullt ut och Microsoft-supporten hjälper att isolera och lösa problem relaterade till komponenterna.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-187">Built-in components are fully supported, and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>

> [!WARNING]
> <span data-ttu-id="e5cd7-188">Komponenter som ingår i HDInsight-kluster stöds fullt ut och Microsoft-supporten hjälper att isolera och lösa problem relaterade till komponenterna.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-188">Components provided with the HDInsight cluster are fully supported and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>
>
> <span data-ttu-id="e5cd7-189">Anpassade komponenter få kommersiellt rimliga stöd för att hjälpa dig att felsöka problemet ytterligare.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-189">Custom components receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="e5cd7-190">Detta kan resultera i att lösa problemet eller där du uppmanas att engagera tillgängliga kanaler för öppen källkod där djup expertis för att teknik finns.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-190">This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="e5cd7-191">Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-191">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="e5cd7-192">Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-192">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span></span>
>
>

<span data-ttu-id="e5cd7-193">HDInsight-tjänst finns flera sätt att använda anpassade komponenter.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-193">The HDInsight service provides several ways to use custom components.</span></span> <span data-ttu-id="e5cd7-194">Oavsett hur en komponent används eller installeras i klustret, gäller samma nivå av support.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-194">Regardless of how a component is used or installed on the cluster, the same level of support applies.</span></span> <span data-ttu-id="e5cd7-195">Nedan visas en lista över de vanligaste sätten att anpassade komponenter kan användas på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="e5cd7-195">Below is a list of the most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="e5cd7-196">Jobbet - Hadoop och andra typer av jobb som kör eller använda anpassade komponenter kan skickas till klustret.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-196">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted to the cluster.</span></span>
2. <span data-ttu-id="e5cd7-197">Anpassning av kluster - när klustret skapas som du kan ange ytterligare inställningar och anpassade komponenter som ska installeras på klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-197">Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on the cluster nodes.</span></span>
3. <span data-ttu-id="e5cd7-198">Exempel - för populära anpassade komponenter, Microsoft och andra kan ge exempel på hur du kan använda de här komponenterna i HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-198">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on the HDInsight clusters.</span></span> <span data-ttu-id="e5cd7-199">De här exemplen tillhandahålls utan stöd.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-199">These samples are provided without support.</span></span>

## <a name="develop-script-action-scripts"></a><span data-ttu-id="e5cd7-200">Utveckla skriptåtgärd skript</span><span class="sxs-lookup"><span data-stu-id="e5cd7-200">Develop Script Action scripts</span></span>
<span data-ttu-id="e5cd7-201">Se [utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="e5cd7-201">See [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>

## <a name="see-also"></a><span data-ttu-id="e5cd7-202">Se även</span><span class="sxs-lookup"><span data-stu-id="e5cd7-202">See also</span></span>
* <span data-ttu-id="e5cd7-203">[Skapa Hadoop-kluster i HDInsight] [ hdinsight-provision-cluster] innehåller instruktioner om hur du skapar ett HDInsight-kluster med hjälp av andra anpassade alternativ.</span><span class="sxs-lookup"><span data-stu-id="e5cd7-203">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how to create an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="e5cd7-204">[Utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="e5cd7-204">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="e5cd7-205">[Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="e5cd7-205">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="e5cd7-206">[Installera och använda R i HDInsight-kluster][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="e5cd7-206">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="e5cd7-207">[Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-207">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="e5cd7-208">[Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="e5cd7-208">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


<span data-ttu-id="e5cd7-209">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Steg när klustret skapas"</span><span class="sxs-lookup"><span data-stu-id="e5cd7-209">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Stages during cluster creation"</span></span>
