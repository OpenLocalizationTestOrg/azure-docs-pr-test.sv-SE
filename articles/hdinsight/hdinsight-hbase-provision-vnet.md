---
title: "aaaCreate HBase-kluster i ett virtuellt nätverk - Azure | Microsoft Docs"
description: "Komma igång med HBase i Azure HDInsight. Lär dig hur toocreate HDInsight HBase-kluster i Azure Virtual Network."
keywords: 
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 097338a5a650bb607a9f6f9ddb59bb88d098b56f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a><span data-ttu-id="3ccfe-104">Skapa HBase-kluster i HDInsight i Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="3ccfe-104">Create HBase clusters on HDInsight in Azure Virtual Network</span></span>
<span data-ttu-id="3ccfe-105">Lär dig hur toocreate Azure HDInsight HBase-kluster i en [Azure Virtual Network][1].</span><span class="sxs-lookup"><span data-stu-id="3ccfe-105">Learn how toocreate Azure HDInsight HBase clusters in an [Azure Virtual Network][1].</span></span>

<span data-ttu-id="3ccfe-106">Med virtuell nätverksintegration HBase-kluster kan vara distribuerade toohello samma virtuella nätverk som dina program så att program kan kommunicera med HBase direkt.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-106">With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span> <span data-ttu-id="3ccfe-107">hello fördelar:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-107">hello benefits include:</span></span>

* <span data-ttu-id="3ccfe-108">Direktanslutning hello web application toohello noder i hello HBase-kluster, vilket möjliggör kommunikation via HBase Java fjärrproceduranrop anropa API: er (RPC).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-108">Direct connectivity of hello web application toohello nodes of hello HBase cluster, which enables communication via HBase Java remote procedure call (RPC) APIs.</span></span>
* <span data-ttu-id="3ccfe-109">Bättre prestanda genom att inte låta trafiken gå över flera gateways och belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-109">Improved performance by not having your traffic go over multiple gateways and load-balancers.</span></span>
* <span data-ttu-id="3ccfe-110">hello möjlighet tooprocess känslig information på ett säkrare sätt utan att exponera en offentlig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-110">hello ability tooprocess sensitive information in a more secure manner without exposing a public endpoint.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="3ccfe-111">Krav</span><span class="sxs-lookup"><span data-stu-id="3ccfe-111">Prerequisites</span></span>
<span data-ttu-id="3ccfe-112">Innan du påbörjar den här självstudien måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-112">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="3ccfe-113">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-113">**An Azure subscription**.</span></span> <span data-ttu-id="3ccfe-114">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-114">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="3ccfe-115">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-115">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="3ccfe-116">Se [installera och använda Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-116">See [Install and use Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).</span></span>

## <a name="create-hbase-cluster-into-virtual-network"></a><span data-ttu-id="3ccfe-117">Skapa HBase-kluster i virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="3ccfe-117">Create HBase cluster into virtual network</span></span>
<span data-ttu-id="3ccfe-118">I det här avsnittet skapar du en Linux-baserade HBase-kluster med hello beroende Azure Storage-konto i ett virtuellt Azure-nätverk med hjälp av en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-118">In this section, you create a Linux-based HBase cluster with hello dependent Azure Storage account in an Azure virtual network using an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="3ccfe-119">För andra metoder för att skapa kluster och förstå hello inställningar, se [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-119">For other cluster creation methods and understanding hello settings, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="3ccfe-120">För mer information om hur du använder en mall toocreate Hadoop-kluster i HDInsight, se [skapa Hadoop-kluster i HDInsight med hjälp av Azure Resource Manager-mallar](hdinsight-hadoop-create-windows-clusters-arm-templates.md)</span><span class="sxs-lookup"><span data-stu-id="3ccfe-120">For more information about using a template toocreate Hadoop clusters in HDInsight, see [Create Hadoop clusters in HDInsight using Azure Resource Manager templates](hdinsight-hadoop-create-windows-clusters-arm-templates.md)</span></span>

> [!NOTE]
> <span data-ttu-id="3ccfe-121">Vissa egenskaper är hårdkodat till hello mall.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-121">Some properties are hard-coded into hello template.</span></span> <span data-ttu-id="3ccfe-122">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-122">For example:</span></span>
>
> * <span data-ttu-id="3ccfe-123">**Plats**: östra USA 2</span><span class="sxs-lookup"><span data-stu-id="3ccfe-123">**Location**: East US 2</span></span>
> * <span data-ttu-id="3ccfe-124">**Klusterversion**: 3.5</span><span class="sxs-lookup"><span data-stu-id="3ccfe-124">**Cluster version**: 3.5</span></span>
> * <span data-ttu-id="3ccfe-125">**Klustret worker nodsantalet**: 2</span><span class="sxs-lookup"><span data-stu-id="3ccfe-125">**Cluster worker node count**: 2</span></span>
> * <span data-ttu-id="3ccfe-126">**Storage-konto som standard**: en unik sträng</span><span class="sxs-lookup"><span data-stu-id="3ccfe-126">**Default storage account**: a unique string</span></span>
> * <span data-ttu-id="3ccfe-127">**Virtuella nätverksnamnet**: &lt;klusternamn >-vnet</span><span class="sxs-lookup"><span data-stu-id="3ccfe-127">**Virtual network name**: &lt;Cluster Name>-vnet</span></span>
> * <span data-ttu-id="3ccfe-128">**Virtuella nätverkets adressutrymme**: 10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="3ccfe-128">**Virtual network address space**: 10.0.0.0/16</span></span>
> * <span data-ttu-id="3ccfe-129">**Undernätnamnet**: Undernät1</span><span class="sxs-lookup"><span data-stu-id="3ccfe-129">**Subnet name**: subnet1</span></span>
> * <span data-ttu-id="3ccfe-130">**Adressintervall för gatewayundernät**: 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="3ccfe-130">**Subnet address range**: 10.0.0.0/24</span></span>
>
> <span data-ttu-id="3ccfe-131">&lt;Klusternamn > ersätts med hello klusternamnet som du anger när du använder hello mall.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-131">&lt;Cluster Name> is replaced with hello cluster name you provide when using hello template.</span></span>
>
>

1. <span data-ttu-id="3ccfe-132">Klicka på hello följande bild tooopen hello mallen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-132">Click hello following image tooopen hello template in hello Azure portal.</span></span> <span data-ttu-id="3ccfe-133">hello-mallen finns i [Azure QuickStart mallar](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-133">hello template is located in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).</span></span>

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="3ccfe-134">Från hello **anpassad distribution** bladet ange hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-134">From hello **Custom deployment** blade, enter hello following properties:</span></span>

   * <span data-ttu-id="3ccfe-135">**Prenumerationen**: Välj ett Azure-prenumeration används toocreate hello HDInsight-kluster, hello beroende lagringskontot och hello virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-135">**Subscription**: Select an Azure subscription used toocreate hello HDInsight cluster, hello dependent Storage account and hello Azure virtual network.</span></span>
   * <span data-ttu-id="3ccfe-136">**Resursgruppen**: Välj **Skapa nytt**, och ange namn på en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-136">**Resource group**: Select **Create new**, and specify a new resource group name.</span></span>
   * <span data-ttu-id="3ccfe-137">**Plats**: Välj en plats för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-137">**Location**: Select a location for hello resource group.</span></span>
   * <span data-ttu-id="3ccfe-138">**Klusternamn**: Ange ett namn för hello Hadoop-kluster toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-138">**ClusterName**: Enter a name for hello Hadoop cluster toobe created.</span></span>
   * <span data-ttu-id="3ccfe-139">**Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är **admin**.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-139">**Cluster login name and password**: hello default login name is **admin**.</span></span>
   * <span data-ttu-id="3ccfe-140">**SSH-användarnamn och lösenord**: hello Standardanvändarnamnet är **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-140">**SSH username and password**: hello default username is **sshuser**.</span></span>  <span data-ttu-id="3ccfe-141">Du kan byta namn.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-141">You can rename it.</span></span>
   * <span data-ttu-id="3ccfe-142">**Jag accepterar toohello villkor hello ovan**: (Välj)</span><span class="sxs-lookup"><span data-stu-id="3ccfe-142">**I agree toohello terms and hello conditions stated above**: (Select)</span></span>
3. <span data-ttu-id="3ccfe-143">Klicka på **Köp**.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-143">Click **Purchase**.</span></span> <span data-ttu-id="3ccfe-144">Det tar cirka 20 minuter toocreate ett kluster.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-144">It takes about around 20 minutes toocreate a cluster.</span></span> <span data-ttu-id="3ccfe-145">När hello klustret har skapats måste du klicka på hello klusterbladet i hello portal tooopen den.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-145">Once hello cluster is created, you can click hello cluster blade in hello portal tooopen it.</span></span>

<span data-ttu-id="3ccfe-146">När du har slutfört hello kursen kanske du vill toodelete hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-146">After you complete hello tutorial, you might want toodelete hello cluster.</span></span> <span data-ttu-id="3ccfe-147">Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-147">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span> <span data-ttu-id="3ccfe-148">Du debiteras också för ett HDInsight-kluster, även när det inte används.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-148">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="3ccfe-149">Eftersom hello avgifterna för hello klustret är flera gånger större än hello avgifterna för lagring, är det ekonomiskt meningsfullt toodelete kluster när de inte används.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-149">Since hello charges for hello cluster are many times more than hello charges for storage, it makes economic sense toodelete clusters when they are not in use.</span></span> <span data-ttu-id="3ccfe-150">Hello instruktioner för att ta bort ett kluster finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen](hdinsight-administer-use-management-portal.md#delete-clusters).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-150">For hello instructions of deleting a cluster, see [Manage Hadoop clusters in HDInsight by using hello Azure portal](hdinsight-administer-use-management-portal.md#delete-clusters).</span></span>

<span data-ttu-id="3ccfe-151">toobegin arbetar med ditt nya kluster med HBase, du kan använda hello procedurer som påträffas i [komma igång med HBase med Hadoop i HDInsight](hdinsight-hbase-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-151">toobegin working with your new HBase cluster, you can use hello procedures found in [Get started using HBase with Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md).</span></span>

## <a name="connect-toohello-hbase-cluster-using-hbase-java-rpc-apis"></a><span data-ttu-id="3ccfe-152">Ansluta toohello HBase-kluster med HBase Java RPC-API: er</span><span class="sxs-lookup"><span data-stu-id="3ccfe-152">Connect toohello HBase cluster using HBase Java RPC APIs</span></span>
1. <span data-ttu-id="3ccfe-153">Skapa en infrastruktur som en tjänst (IaaS) virtuell dator i hello samma virtuella Azure-nätverket och hello samma undernät.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-153">Create an infrastructure as a service (IaaS) virtual machine into hello same Azure virtual network and hello same subnet.</span></span> <span data-ttu-id="3ccfe-154">Anvisningar om hur du skapar en ny virtuell IaaS-dator finns [skapa en virtuell dator kör Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-154">For instructions on creating a new IaaS virtual machine, see [Create a Virtual Machine Running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md).</span></span> <span data-ttu-id="3ccfe-155">När följande hello i det här dokumentet, måste du använda följande värden för hello nätverkskonfigurationen hello:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-155">When following hello steps in this document, you must use hello following values for hello Network configuration:</span></span>

   * <span data-ttu-id="3ccfe-156">**Virtuellt nätverk**: &lt;klusternamn >-vnet</span><span class="sxs-lookup"><span data-stu-id="3ccfe-156">**Virtual network**: &lt;Cluster name>-vnet</span></span>
   * <span data-ttu-id="3ccfe-157">**Undernät**: Undernät1</span><span class="sxs-lookup"><span data-stu-id="3ccfe-157">**Subnet**: subnet1</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="3ccfe-158">Ersätt &lt;klusternamn > med hello namn du använde när du skapar hello HDInsight-kluster i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-158">Replace &lt;Cluster name> with hello name you used when creating hello HDInsight cluster in previous steps.</span></span>
   >
   >

   <span data-ttu-id="3ccfe-159">Med dessa värden hello virtuella datorn är placerad i hello samma virtuella nätverk och undernät som hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-159">Using these values, hello virtual machine is placed in hello same virtual network and subnet as hello HDInsight cluster.</span></span> <span data-ttu-id="3ccfe-160">Den här konfigurationen låter dem toodirectly kommunicera med varandra.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-160">This configuration allows them toodirectly communicate with each other.</span></span> <span data-ttu-id="3ccfe-161">Det finns ett sätt toocreate ett HDInsight-kluster med en tom kantnod.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-161">There is a way toocreate an HDInsight cluster with an empty edge node.</span></span> <span data-ttu-id="3ccfe-162">hello kantnod kan vara används toomanage hello.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-162">hello edge node can be used toomanage hello cluster.</span></span>  <span data-ttu-id="3ccfe-163">Mer information finns i [använda tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-163">For more information, see [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md).</span></span>

2. <span data-ttu-id="3ccfe-164">När du använder en Java-program tooconnect tooHBase via fjärranslutning, måste du använda hello fullständigt kvalificerade domännamnet (FQDN).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-164">When using a Java application tooconnect tooHBase remotely, you must use hello fully qualified domain name (FQDN).</span></span> <span data-ttu-id="3ccfe-165">toodetermine, måste du hämta hello anslutningsspecifika DNS-suffix hello HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-165">toodetermine this, you must get hello connection-specific DNS suffix of hello HBase cluster.</span></span> <span data-ttu-id="3ccfe-166">toodo att du kan använda någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-166">toodo that, you can use one of hello following methods:</span></span>

   * <span data-ttu-id="3ccfe-167">Använd en Web webbläsare toomake ett Ambari-anrop:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-167">Use a Web browser toomake an Ambari call:</span></span>

     <span data-ttu-id="3ccfe-168">Bläddra toohttps: / /&lt;klusternamn >.azurehdinsight.net/api/v1/clusters/&lt;klusternamn > / värd? minimal_response = true.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-168">Browse toohttps://&lt;ClusterName>.azurehdinsight.net/api/v1/clusters/&lt;ClusterName>/hosts?minimal_response=true.</span></span> <span data-ttu-id="3ccfe-169">Den blir en JSON-fil med hello DNS-suffix.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-169">It turns a JSON file with hello DNS suffixes.</span></span>
   * <span data-ttu-id="3ccfe-170">Använda hello Ambari webbplats:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-170">Use hello Ambari website:</span></span>

     1. <span data-ttu-id="3ccfe-171">Bläddra för https://&lt;klusternamn >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-171">Browse too https://&lt;ClusterName>.azurehdinsight.net.</span></span>
     2. <span data-ttu-id="3ccfe-172">Klicka på **värdar** hello översta menyn.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-172">Click **Hosts** from hello top menu.</span></span>
   * <span data-ttu-id="3ccfe-173">Använd Curl toomake REST-anrop:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-173">Use Curl toomake REST calls:</span></span>

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     <span data-ttu-id="3ccfe-174">Hitta hello ”värddatornamn” post i hello JavaScript Object Notation (JSON) data returnerades.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-174">In hello JavaScript Object Notation (JSON) data returned, find hello "host_name" entry.</span></span> <span data-ttu-id="3ccfe-175">Den innehåller hello FQDN för hello noder i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-175">It contains hello FQDN for hello nodes in hello cluster.</span></span> <span data-ttu-id="3ccfe-176">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-176">For example:</span></span>

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     <span data-ttu-id="3ccfe-177">hello-delen av hello domän namn som börjar med hello klusternamnet är hello DNS-suffix.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-177">hello portion of hello domain name beginning with hello cluster name is hello DNS suffix.</span></span> <span data-ttu-id="3ccfe-178">Till exempel mycluster.b1.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-178">For example, mycluster.b1.cloudapp.net.</span></span>
   * <span data-ttu-id="3ccfe-179">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3ccfe-179">Use Azure PowerShell</span></span>

     <span data-ttu-id="3ccfe-180">Använd hello följande Azure PowerShell-skriptet tooregister hello **Get-ClusterDetail** funktion, vilket kan vara används tooreturn hello DNS-suffix:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-180">Use hello following Azure PowerShell script tooregister hello **Get-ClusterDetail** function, which can be used tooreturn hello DNS suffix:</span></span>

    ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information toofacilitate an HDInsight cluster-to-cluster scenario within hello same virtual network.
            .Description
            This command shows hello following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run hello HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows hello FQDN suffix of hosts in hello cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows hello value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run hello HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows hello FQDN suffix of hosts in hello cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
    ```

     <span data-ttu-id="3ccfe-181">Använd hello följande kommando efter körs hello Azure PowerShell-skriptet tooreturn hello DNS-suffix genom att använda hello **Get-ClusterDetail** funktion.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-181">After running hello Azure PowerShell script, use hello following command tooreturn hello DNS suffix by using hello **Get-ClusterDetail** function.</span></span> <span data-ttu-id="3ccfe-182">Ange klusternamnet i HDInsight HBase, admin namn och lösenord för administratör när du använder det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-182">Specify your HDInsight HBase cluster name, admin name, and admin password when using this command.</span></span>

    ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
    ```

     <span data-ttu-id="3ccfe-183">Det här kommandot returnerar hello DNS-suffix.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-183">This command returns hello DNS suffix.</span></span> <span data-ttu-id="3ccfe-184">Till exempel **yourclustername.b4.internal.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-184">For example, **yourclustername.b4.internal.cloudapp.net**.</span></span>


<!--
3.    Change hello primary DNS suffix configuration of hello virtual machine. This enables hello virtual machine tooautomatically resolve hello host name of hello HBase cluster without explicit specification of hello suffix. For example, hello *workernode0* host name will be correctly resolved tooworkernode0 of hello HBase cluster.

    toomake hello configuration change:

    1. RDP into hello virtual machine.
    2. Open **Local Group Policy Editor**. hello executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** toohello value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot hello virtual machine.
-->

<span data-ttu-id="3ccfe-185">tooverify som hello virtuella datorn kan kommunicera med hello HBase-kluster, kommandot hello `ping headnode0.<dns suffix>` från hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-185">tooverify that hello virtual machine can communicate with hello HBase cluster, use hello command `ping headnode0.<dns suffix>` from hello virtual machine.</span></span> <span data-ttu-id="3ccfe-186">Till exempel ping headnode0.mycluster.b1.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-186">For example, ping headnode0.mycluster.b1.cloudapp.net.</span></span>

<span data-ttu-id="3ccfe-187">toouse informationen i ett Java-program som du kan följa hello stegen i [använder Maven toobuild Java-program som använder HBase med HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) toocreate ett program.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-187">toouse this information in a Java application, you can follow hello steps in [Use Maven toobuild Java applications that use HBase with HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) toocreate an application.</span></span> <span data-ttu-id="3ccfe-188">toohave hello program ansluta tooa HBase-fjärrservern, ändra hello **hbase-site.xml** Zookeeper i filen i det här exemplet toouse hello FQDN.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-188">toohave hello application connect tooa remote HBase server, modify hello **hbase-site.xml** file in this example toouse hello FQDN for Zookeeper.</span></span> <span data-ttu-id="3ccfe-189">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-189">For example:</span></span>

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> <span data-ttu-id="3ccfe-190">Mer information om namnmatchning i virtuella Azure-nätverk, inklusive hur toouse egna DNS-servern finns [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="3ccfe-190">For more information about name resolution in Azure virtual networks, including how toouse your own DNS server, see [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="3ccfe-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ccfe-191">Next steps</span></span>
<span data-ttu-id="3ccfe-192">I kursen får du lära sig hur toocreate ett HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3ccfe-192">In this tutorial, you learned how toocreate an HBase cluster.</span></span> <span data-ttu-id="3ccfe-193">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="3ccfe-193">toolearn more, see:</span></span>

* [<span data-ttu-id="3ccfe-194">Komma igång med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3ccfe-194">Get started with HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="3ccfe-195">Använd tom edge-noder i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3ccfe-195">Use empty edge nodes in HDInsight</span></span>](hdinsight-apps-use-edge-node.md)
* [<span data-ttu-id="3ccfe-196">Konfigurera HBase-replikering i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3ccfe-196">Configure HBase replication in HDInsight</span></span>](hdinsight-hbase-replication.md)
* [<span data-ttu-id="3ccfe-197">Skapa Hadoop-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3ccfe-197">Create Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="3ccfe-198">Komma igång med HBase med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3ccfe-198">Get started using HBase with Hadoop in HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
* [<span data-ttu-id="3ccfe-199">Analysera Twitter-åsikter med HBase i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3ccfe-199">Analyze Twitter sentiment with HBase in HDInsight</span></span>](hdinsight-hbase-analyze-twitter-sentiment.md)
* <span data-ttu-id="3ccfe-200">[Översikt över virtuella nätverk][vnet-overview]</span><span class="sxs-lookup"><span data-stu-id="3ccfe-200">[Virtual Network Overview][vnet-overview]</span></span>

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: /powershell/azureps-cmdlets-docs


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Etablera information för hello ny HBase-kluster"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Använd skriptåtgärder toocustomize ett HBase-kluster"

[azure-preview-portal]: https://portal.azure.com
