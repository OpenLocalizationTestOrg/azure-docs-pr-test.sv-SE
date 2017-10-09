---
title: "aaaConfigure din fristående Azure Service Fabric-kluster | Microsoft Docs"
description: "Lär dig hur tooconfigure din fristående eller privata Service Fabric-klustret."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dekapur
ms.openlocfilehash: ce2ad387162a05668bbd3a271c754776fe471850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a><span data-ttu-id="35d70-103">Konfigurationsinställningar för fristående Windows-kluster</span><span class="sxs-lookup"><span data-stu-id="35d70-103">Configuration settings for standalone Windows cluster</span></span>
<span data-ttu-id="35d70-104">Den här artikeln beskriver hur ett fristående Service Fabric med tooconfigure hello ***ClusterConfig.JSON*** fil.</span><span class="sxs-lookup"><span data-stu-id="35d70-104">This article describes how tooconfigure a standalone Service Fabric cluster using hello ***ClusterConfig.JSON*** file.</span></span> <span data-ttu-id="35d70-105">Du kan använda den här filen toospecify information, till exempel hello Service Fabric-noder och deras IP-adresser, olika typer av noder på hello kluster, hello säkerhetskonfigurationer samt hello nätverkstopologi vad gäller fel/uppgradera domäner för din fristående kluster.</span><span class="sxs-lookup"><span data-stu-id="35d70-105">You can use this file toospecify information such as hello Service Fabric nodes and their IP addresses, different types of nodes on hello cluster, hello security configurations as well as hello network topology in terms of fault/upgrade domains, for your standalone cluster.</span></span>

<span data-ttu-id="35d70-106">När du [hämta hello fristående Service Fabric-paket](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), några exempel på hello ClusterConfig.JSON filen är hämtade tooyour arbete datorn.</span><span class="sxs-lookup"><span data-stu-id="35d70-106">When you [download hello standalone Service Fabric package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), a few samples of hello ClusterConfig.JSON file are downloaded tooyour work machine.</span></span> <span data-ttu-id="35d70-107">hello prov med *DevCluster* i sina namn kan du skapa ett kluster med alla tre noder på hello detsamma datorn som logiska noder.</span><span class="sxs-lookup"><span data-stu-id="35d70-107">hello samples having *DevCluster* in their names will help you create a cluster with all three nodes on hello same machine, like logical nodes.</span></span> <span data-ttu-id="35d70-108">Minst en nod måste vara markerad som en primär nod utanför dessa.</span><span class="sxs-lookup"><span data-stu-id="35d70-108">Out of these, at least one node must be marked as a primary node.</span></span> <span data-ttu-id="35d70-109">Det här klustret är användbar för en miljö för utveckling eller testning och kan inte användas som ett produktionskluster.</span><span class="sxs-lookup"><span data-stu-id="35d70-109">This cluster is useful for a development or test environment and is not supported as a production cluster.</span></span> <span data-ttu-id="35d70-110">hello prov med *MultiMachine* i sina namn kan du skapa ett produktionskluster kvalitet, med varje nod på en separat dator.</span><span class="sxs-lookup"><span data-stu-id="35d70-110">hello samples having *MultiMachine* in their names, will help you create a production quality cluster, with each node on a separate machine.</span></span>

1. <span data-ttu-id="35d70-111">*ClusterConfig.Unsecure.DevCluster.JSON* och *ClusterConfig.Unsecure.MultiMachine.JSON* visar hur toocreate ett oskyddat test eller produktion kluster respektive.</span><span class="sxs-lookup"><span data-stu-id="35d70-111">*ClusterConfig.Unsecure.DevCluster.JSON* and *ClusterConfig.Unsecure.MultiMachine.JSON* show how toocreate an unsecured test or production cluster respectively.</span></span> 
2. <span data-ttu-id="35d70-112">*ClusterConfig.Windows.DevCluster.JSON* och *ClusterConfig.Windows.MultiMachine.JSON* visa hur toocreate test- eller klustret skyddas med [Windows-säkerhet](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="35d70-112">*ClusterConfig.Windows.DevCluster.JSON* and  *ClusterConfig.Windows.MultiMachine.JSON* show how toocreate test or production cluster, secured using [Windows security](service-fabric-windows-cluster-windows-security.md).</span></span>
3. <span data-ttu-id="35d70-113">*ClusterConfig.X509.DevCluster.JSON* och *ClusterConfig.X509.MultiMachine.JSON* visa hur toocreate test- eller klustret skyddas med [X509 certifikatbaserad säkerhet](service-fabric-windows-cluster-x509-security.md).</span><span class="sxs-lookup"><span data-stu-id="35d70-113">*ClusterConfig.X509.DevCluster.JSON* and *ClusterConfig.X509.MultiMachine.JSON* show how toocreate test or production cluster, secured using [X509 certificate-based security](service-fabric-windows-cluster-x509-security.md).</span></span> 

<span data-ttu-id="35d70-114">Nu kommer vi att undersöka hello olika delar av en ***ClusterConfig.JSON*** filen enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="35d70-114">Now we will examine hello various sections of a ***ClusterConfig.JSON*** file as below.</span></span>

## <a name="general-cluster-configurations"></a><span data-ttu-id="35d70-115">Allmän klusterkonfigurationer</span><span class="sxs-lookup"><span data-stu-id="35d70-115">General cluster configurations</span></span>
<span data-ttu-id="35d70-116">Detta omfattar hello bred klustret specifika konfigurationer som visas i hello JSON kodfragmentet nedan.</span><span class="sxs-lookup"><span data-stu-id="35d70-116">This covers hello broad cluster specific configurations, as shown in hello JSON snippet below.</span></span>

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

<span data-ttu-id="35d70-117">Du kan ge något eget namn tooyour Service Fabric-kluster genom att tilldela den toohello **namn** variabeln.</span><span class="sxs-lookup"><span data-stu-id="35d70-117">You can give any friendly name tooyour Service Fabric cluster by assigning it toohello **name** variable.</span></span> <span data-ttu-id="35d70-118">Hej **clusterConfigurationVersion** är hello versionsnumret för klustret, bör du öka den varje gång du uppgraderar Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="35d70-118">hello **clusterConfigurationVersion** is hello version number of your cluster; you should increase it every time you upgrade your Service Fabric cluster.</span></span> <span data-ttu-id="35d70-119">Dock bör du lämna hello **apiVersion** toohello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="35d70-119">You should however leave hello **apiVersion** toohello default value.</span></span>

<a id="clusternodes"></a>

## <a name="nodes-on-hello-cluster"></a><span data-ttu-id="35d70-120">Noder i klustret hello</span><span class="sxs-lookup"><span data-stu-id="35d70-120">Nodes on hello cluster</span></span>
<span data-ttu-id="35d70-121">Du kan konfigurera hello noder på Service Fabric-kluster med hjälp av hello **noder** avsnitt som hello följande fragment visas.</span><span class="sxs-lookup"><span data-stu-id="35d70-121">You can configure hello nodes on your Service Fabric cluster by using hello **nodes** section, as hello following snippet shows.</span></span>

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

<span data-ttu-id="35d70-122">Ett Service Fabric-kluster måste innehålla minst 3 noder.</span><span class="sxs-lookup"><span data-stu-id="35d70-122">A Service Fabric cluster must contain at least 3 nodes.</span></span> <span data-ttu-id="35d70-123">Du kan lägga till fler noder toothis avsnittet enligt din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="35d70-123">You can add more nodes toothis section as per your setup.</span></span> <span data-ttu-id="35d70-124">hello i den följande tabellen beskrivs hello konfigurationsinställningarna för varje nod.</span><span class="sxs-lookup"><span data-stu-id="35d70-124">hello following table explains hello configuration settings for each node.</span></span>

| <span data-ttu-id="35d70-125">**Nodkonfiguration**</span><span class="sxs-lookup"><span data-stu-id="35d70-125">**Node configuration**</span></span> | <span data-ttu-id="35d70-126">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="35d70-126">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="35d70-127">Nodnamn</span><span class="sxs-lookup"><span data-stu-id="35d70-127">nodeName</span></span> |<span data-ttu-id="35d70-128">Du kan ge någon toohello nod för eget namn.</span><span class="sxs-lookup"><span data-stu-id="35d70-128">You can give any friendly name toohello node.</span></span> |
| <span data-ttu-id="35d70-129">IP-adress</span><span class="sxs-lookup"><span data-stu-id="35d70-129">iPAddress</span></span> |<span data-ttu-id="35d70-130">Ta reda på hello IP-adressen för noden genom att öppna ett kommandofönster och skriva `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="35d70-130">Find out hello IP address of your node by opening a command window and typing `ipconfig`.</span></span> <span data-ttu-id="35d70-131">Anteckna hello IPV4-adress och tilldela den toohello **iPAddress** variabeln.</span><span class="sxs-lookup"><span data-stu-id="35d70-131">Note hello IPV4 address and assign it toohello **iPAddress** variable.</span></span> |
| <span data-ttu-id="35d70-132">nodeTypeRef</span><span class="sxs-lookup"><span data-stu-id="35d70-132">nodeTypeRef</span></span> |<span data-ttu-id="35d70-133">Varje nod kan tilldelas en annan nod-typen.</span><span class="sxs-lookup"><span data-stu-id="35d70-133">Each node can be assigned a different node type.</span></span> <span data-ttu-id="35d70-134">Hej [nodtyper](#nodetypes) definieras i hello nedan.</span><span class="sxs-lookup"><span data-stu-id="35d70-134">hello [node types](#nodetypes) are defined in hello section below.</span></span> |
| <span data-ttu-id="35d70-135">faultDomain</span><span class="sxs-lookup"><span data-stu-id="35d70-135">faultDomain</span></span> |<span data-ttu-id="35d70-136">Fel domäner aktivera administratörer toodefine hello fysiska noder som kan misslyckas på hello samma tid på grund av tooshared fysiska beroenden.</span><span class="sxs-lookup"><span data-stu-id="35d70-136">Fault domains enable cluster administrators toodefine hello physical nodes that might fail at hello same time due tooshared physical dependencies.</span></span> |
| <span data-ttu-id="35d70-137">upgradeDomain</span><span class="sxs-lookup"><span data-stu-id="35d70-137">upgradeDomain</span></span> |<span data-ttu-id="35d70-138">Uppgraderingsdomäner beskriver uppsättningar med noder som är avstängd för Service Fabric uppgraderas på om hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="35d70-138">Upgrade domains describe sets of nodes that are shut down for Service Fabric upgrades at about hello same time.</span></span> <span data-ttu-id="35d70-139">Du kan välja vilka noder tooassign toowhich uppgraderingsdomäner, som inte begränsas av alla fysiska krav.</span><span class="sxs-lookup"><span data-stu-id="35d70-139">You can choose which nodes tooassign toowhich Upgrade domains, as they are not limited by any physical requirements.</span></span> |

## <a name="cluster-properties"></a><span data-ttu-id="35d70-140">Egenskaper för klustret</span><span class="sxs-lookup"><span data-stu-id="35d70-140">Cluster properties</span></span>
<span data-ttu-id="35d70-141">Hej **egenskaper** avsnitt i hello ClusterConfig.JSON är används tooconfigure hello klustret på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="35d70-141">hello **properties** section in hello ClusterConfig.JSON is used tooconfigure hello cluster as follows.</span></span>

<a id="reliability"></a>

### <a name="reliability"></a><span data-ttu-id="35d70-142">Tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="35d70-142">Reliability</span></span>
<span data-ttu-id="35d70-143">hello begreppet **reliabilityLevel** definierar hello antal repliker eller instanser av hello Service Fabric-systemtjänster som kan köras på hello primära klusternoder hello.</span><span class="sxs-lookup"><span data-stu-id="35d70-143">hello concept of **reliabilityLevel** defines hello number of replicas or instances of hello Service Fabric system services that can run on hello primary nodes of hello cluster.</span></span> <span data-ttu-id="35d70-144">Den avgör hello tillförlitlighet för dessa tjänster och därför hello klustret.</span><span class="sxs-lookup"><span data-stu-id="35d70-144">It determines hello reliability of these services and hence hello cluster.</span></span> <span data-ttu-id="35d70-145">hello-värdet är beräknade hello systemet när klustret skapas och uppgradering.</span><span class="sxs-lookup"><span data-stu-id="35d70-145">hello value is calcuated by hello system at cluster creation and upgrade time.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="35d70-146">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="35d70-146">Diagnostics</span></span>
<span data-ttu-id="35d70-147">Hej **diagnosticsStore** avsnittet kan du tooconfigure parametrar tooenable diagnostik och felsökning nod eller fel i kluster, enligt följande kodavsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="35d70-147">hello **diagnosticsStore** section allows you tooconfigure parameters tooenable diagnostics and troubleshooting node or cluster failures, as shown in hello following snippet.</span></span> 

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

<span data-ttu-id="35d70-148">Hej **metadata** är en beskrivning av kluster-diagnostik och kan anges enligt din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="35d70-148">hello **metadata** is a description of your cluster diagnostics and can be set as per your setup.</span></span> <span data-ttu-id="35d70-149">Dessa variabler bidra till att samla in ETW spårningsloggar, krascher Dumpar samt prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="35d70-149">These variables help in collecting ETW trace logs, crash dumps as well as performance counters.</span></span> <span data-ttu-id="35d70-150">Läs [spårloggen finns det](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) och [ETW-spårning](https://msdn.microsoft.com/library/ms751538.aspx) för mer information om ETW spårningsloggar.</span><span class="sxs-lookup"><span data-stu-id="35d70-150">Read [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) and [ETW Tracing](https://msdn.microsoft.com/library/ms751538.aspx) for more information on ETW trace logs.</span></span> <span data-ttu-id="35d70-151">Alla loggar inklusive [krascher Dumpar](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) och [prestandaräknare](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) kan vara riktad toohello **connectionString** mapp på din dator.</span><span class="sxs-lookup"><span data-stu-id="35d70-151">All logs including [Crash dumps](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) and [performance counters](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) can be directed toohello **connectionString** folder on your machine.</span></span> <span data-ttu-id="35d70-152">Du kan också använda *AzureStorage* för att lagra diagnostik.</span><span class="sxs-lookup"><span data-stu-id="35d70-152">You can also use *AzureStorage* for storing diagnostics.</span></span> <span data-ttu-id="35d70-153">Nedan visas ett exempel fragment.</span><span class="sxs-lookup"><span data-stu-id="35d70-153">See below for a sample snippet.</span></span>

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a><span data-ttu-id="35d70-154">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="35d70-154">Security</span></span>
<span data-ttu-id="35d70-155">Hej **säkerhet** avsnittet krävs för en säker fristående Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="35d70-155">hello **security** section is necessary for a secure standalone Service Fabric cluster.</span></span> <span data-ttu-id="35d70-156">hello följande fragment visas en del av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="35d70-156">hello following snippet shows a part of this section.</span></span>

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

<span data-ttu-id="35d70-157">Hej **metadata** är en beskrivning av säker klustret och kan anges enligt din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="35d70-157">hello **metadata** is a description of your secure cluster and can be set as per your setup.</span></span> <span data-ttu-id="35d70-158">Hej **ClusterCredentialType** och **ServerCredentialType** bestämmer hello typ av säkerhet som implementerar hello kluster och hello noder.</span><span class="sxs-lookup"><span data-stu-id="35d70-158">hello **ClusterCredentialType** and **ServerCredentialType** determine hello type of security that hello cluster and hello nodes will implement.</span></span> <span data-ttu-id="35d70-159">Du kan välja tooeither *X509* för en certifikatbaserad säkerhet eller *Windows* för ett Azure Active Directory-baserad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="35d70-159">They can be set tooeither *X509* for a certificate-based security, or *Windows* for an Azure Active Directory-based security.</span></span> <span data-ttu-id="35d70-160">Hej resten av hello **säkerhet** avsnittet baseras på hello typ av hello säkerhet.</span><span class="sxs-lookup"><span data-stu-id="35d70-160">hello rest of hello **security** section will be based on hello type of hello security.</span></span> <span data-ttu-id="35d70-161">Läs [certifikat baserade säkerhet i ett fristående kluster](service-fabric-windows-cluster-x509-security.md) eller [Windows-säkerhet i ett kluster med fristående](service-fabric-windows-cluster-windows-security.md) information om hur toofill ut hello resten av hello **säkerhet**avsnitt.</span><span class="sxs-lookup"><span data-stu-id="35d70-161">Read [Certificates-based security in a standalone cluster](service-fabric-windows-cluster-x509-security.md) or [Windows security in a standalone cluster](service-fabric-windows-cluster-windows-security.md) for information on how toofill out hello rest of hello **security** section.</span></span>

<a id="nodetypes"></a>

### <a name="node-types"></a><span data-ttu-id="35d70-162">Nodtyper</span><span class="sxs-lookup"><span data-stu-id="35d70-162">Node Types</span></span>
<span data-ttu-id="35d70-163">Hej **nodetypes får** beskrivs hello typ av hello-noder som klustret har.</span><span class="sxs-lookup"><span data-stu-id="35d70-163">hello **nodeTypes** section describes hello type of hello nodes that your cluster has.</span></span> <span data-ttu-id="35d70-164">Minst en nodtyp måste anges för ett kluster, enligt hello kodfragmentet nedan.</span><span class="sxs-lookup"><span data-stu-id="35d70-164">At least one node type must be specified for a cluster, as shown in hello snippet below.</span></span> 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

<span data-ttu-id="35d70-165">Hej **namn** är hello eget namn för den här typen av viss nod.</span><span class="sxs-lookup"><span data-stu-id="35d70-165">hello **name** is hello friendly name for this particular node type.</span></span> <span data-ttu-id="35d70-166">toocreate en nod i den här nodtypen tilldela sitt eget namn toohello **nodeTypeRef** variabel för noden, som tidigare nämnts [ovan](#clusternodes).</span><span class="sxs-lookup"><span data-stu-id="35d70-166">toocreate a node of this node type, assign its friendly name toohello **nodeTypeRef** variable for that node, as mentioned [above](#clusternodes).</span></span> <span data-ttu-id="35d70-167">Definiera hello anslutningens slutpunkter som ska användas för varje nodtyp.</span><span class="sxs-lookup"><span data-stu-id="35d70-167">For each node type, define hello connection endpoints that will be used.</span></span> <span data-ttu-id="35d70-168">Du kan välja ett annat portnummer för dessa slutpunkter för anslutningen, så länge som de inte står i konflikt med andra slutpunkter i det här klustret.</span><span class="sxs-lookup"><span data-stu-id="35d70-168">You can choose any port number for these connection endpoints, as long as they do not conflict with any other endpoints in this cluster.</span></span> <span data-ttu-id="35d70-169">I ett kluster med flera noder finns en eller flera primära noder (d.v.s. **isPrimary** ställa in också*SANT*), beroende på hello [ **reliabilityLevel** ](#reliability).</span><span class="sxs-lookup"><span data-stu-id="35d70-169">In a multi-node cluster, there will be one or more primary nodes (i.e. **isPrimary** set too*true*), depending on hello [**reliabilityLevel**](#reliability).</span></span> <span data-ttu-id="35d70-170">Läs [Service Fabric-kluster kapacitetsplaneringsöverväganden](service-fabric-cluster-capacity.md) information om **nodetypes får** och **reliabilityLevel**, och tooknow vad är primär och hello icke-primär nodtyper.</span><span class="sxs-lookup"><span data-stu-id="35d70-170">Read [Service Fabric cluster capacity planning considerations](service-fabric-cluster-capacity.md) for information on **nodeTypes** and **reliabilityLevel**, and tooknow what are primary and hello non-primary node types.</span></span> 

#### <a name="endpoints-used-tooconfigure-hello-node-types"></a><span data-ttu-id="35d70-171">Slutpunkter används tooconfigure hello nodtyper</span><span class="sxs-lookup"><span data-stu-id="35d70-171">Endpoints used tooconfigure hello node types</span></span>
* <span data-ttu-id="35d70-172">*clientConnectionEndpointPort* hello-porten som används av hello klient tooconnect toohello kluster när du använder hello-klientens API: er.</span><span class="sxs-lookup"><span data-stu-id="35d70-172">*clientConnectionEndpointPort* is hello port used by hello client tooconnect toohello cluster, when using hello client APIs.</span></span> 
* <span data-ttu-id="35d70-173">*clusterConnectionEndpointPort* är hello port där hello noder kommunicerar med varandra.</span><span class="sxs-lookup"><span data-stu-id="35d70-173">*clusterConnectionEndpointPort* is hello port at which hello nodes communicate with each other.</span></span>
* <span data-ttu-id="35d70-174">*leaseDriverEndpointPort* hello-porten som används av klustret hello lån drivrutinen toofind ut om hello noder som fortfarande är aktiva.</span><span class="sxs-lookup"><span data-stu-id="35d70-174">*leaseDriverEndpointPort* is hello port used by hello cluster lease driver toofind out if hello nodes are still active.</span></span> 
* <span data-ttu-id="35d70-175">*serviceConnectionEndpointPort* hello-port som används av hello program och tjänster som distribueras på en nod, toocommunicate med hello Service Fabric-klienten på just den noden.</span><span class="sxs-lookup"><span data-stu-id="35d70-175">*serviceConnectionEndpointPort* is hello port used by hello applications and services deployed on a node, toocommunicate with hello Service Fabric client on that particular node.</span></span>
* <span data-ttu-id="35d70-176">*httpGatewayEndpointPort* hello-porten som används av hello Service Fabric Explorer tooconnect toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="35d70-176">*httpGatewayEndpointPort* is hello port used by hello Service Fabric Explorer tooconnect toohello cluster.</span></span>
* <span data-ttu-id="35d70-177">*ephemeralPorts* åsidosätta hello [dynamiska portar som används av hello OS](https://support.microsoft.com/kb/929851).</span><span class="sxs-lookup"><span data-stu-id="35d70-177">*ephemeralPorts* override hello [dynamic ports used by hello OS](https://support.microsoft.com/kb/929851).</span></span> <span data-ttu-id="35d70-178">Service Fabric använder en del av dessa som programmet portar och hello återstående blir tillgänglig för hello OS.</span><span class="sxs-lookup"><span data-stu-id="35d70-178">Service Fabric will use a part of these as application ports and hello remaining will be available for hello OS.</span></span> <span data-ttu-id="35d70-179">Den kommer även mappa det här intervallet toohello befintligt intervall finns i hello OS, så du kan använda hello-intervall som anges i hello exempelfiler JSON för alla ändamål.</span><span class="sxs-lookup"><span data-stu-id="35d70-179">It will also map this range toohello existing range present in hello OS, so for all purposes, you can use hello ranges given in hello sample JSON files.</span></span> <span data-ttu-id="35d70-180">Du måste toomake att hello skillnaden mellan hello start- och hello portar är minst 255.</span><span class="sxs-lookup"><span data-stu-id="35d70-180">You need toomake sure that hello difference between hello start and hello end ports is at least 255.</span></span> <span data-ttu-id="35d70-181">Om denna skillnad är för låg eftersom det här intervallet delas med hello operativsystem kan du stöta på konflikter.</span><span class="sxs-lookup"><span data-stu-id="35d70-181">You may run into conflicts if this difference is too low, since this range is shared with hello operating system.</span></span> <span data-ttu-id="35d70-182">Se hello konfigureras dynamiskt portintervall genom att köra `netsh int ipv4 show dynamicport tcp`.</span><span class="sxs-lookup"><span data-stu-id="35d70-182">See hello configured dynamic port range by running `netsh int ipv4 show dynamicport tcp`.</span></span>
* <span data-ttu-id="35d70-183">*applicationPorts* är hello-portar som används av hello Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="35d70-183">*applicationPorts* are hello ports that will be used by hello Service Fabric applications.</span></span> <span data-ttu-id="35d70-184">portintervall för hello programmet bör vara tillräckligt stor toocover hello endpoint krav för dina program.</span><span class="sxs-lookup"><span data-stu-id="35d70-184">hello application port range should be large enough toocover hello endpoint requirement of your applications.</span></span> <span data-ttu-id="35d70-185">Det här intervallet ska vara exklusiv från hello dynamiskt portintervall på hello datorn, d.v.s. hello *ephemeralPorts* intervall som angetts i hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="35d70-185">This range should be exclusive from hello dynamic port range on hello machine, i.e. hello *ephemeralPorts* range as set in hello configuration.</span></span>  <span data-ttu-id="35d70-186">Service Fabric använda dessa när nya portarna som krävs, samt ta hand om öppna hello-brandväggen för dessa portar.</span><span class="sxs-lookup"><span data-stu-id="35d70-186">Service Fabric will use these whenever new ports are required, as well as take care of opening hello firewall for these ports.</span></span> 
* <span data-ttu-id="35d70-187">*reverseProxyEndpointPort* är valfria omvänd proxy slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="35d70-187">*reverseProxyEndpointPort* is an optional reverse proxy endpoint.</span></span> <span data-ttu-id="35d70-188">Se [Service Fabric omvänd Proxy](service-fabric-reverseproxy.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="35d70-188">See [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) for more details.</span></span> 

### <a name="log-settings"></a><span data-ttu-id="35d70-189">Logginställningar</span><span class="sxs-lookup"><span data-stu-id="35d70-189">Log Settings</span></span>
<span data-ttu-id="35d70-190">Hej **fabricSettings** avsnittet kan du tooset hello rotkataloger för hello Service Fabric-data och loggfiler.</span><span class="sxs-lookup"><span data-stu-id="35d70-190">hello **fabricSettings** section allows you tooset hello root directories for hello Service Fabric data and logs.</span></span> <span data-ttu-id="35d70-191">Du kan anpassa dessa endast när hello inledande klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="35d70-191">You can customize these only during hello initial cluster creation.</span></span> <span data-ttu-id="35d70-192">Nedan visas ett exempel fragment av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="35d70-192">See below for a sample snippet of this section.</span></span>

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

<span data-ttu-id="35d70-193">Vi rekommenderar att du använder en icke-OS-enhet som hello FabricDataRoot och FabricLogRoot eftersom det ger högre tillförlitlighet mot OS kraschar.</span><span class="sxs-lookup"><span data-stu-id="35d70-193">We recommended using a non-OS drive as hello FabricDataRoot and FabricLogRoot as it provides more reliability against OS crashes.</span></span> <span data-ttu-id="35d70-194">Observera att om du har ändrat endast hello dataroten sedan hello loggen rot placeras på en nivå under hello data rot.</span><span class="sxs-lookup"><span data-stu-id="35d70-194">Note that if you customize only hello data root, then hello log root will be placed one level below hello data root.</span></span>

### <a name="stateful-reliable-service-settings"></a><span data-ttu-id="35d70-195">Tillståndskänsliga Reliable tjänstinställningar</span><span class="sxs-lookup"><span data-stu-id="35d70-195">Stateful Reliable Service Settings</span></span>
<span data-ttu-id="35d70-196">Hej **KtlLogger** avsnittet kan du tooset hello konfigurationsinställningar för Reliable Services.</span><span class="sxs-lookup"><span data-stu-id="35d70-196">hello **KtlLogger** section allows you tooset hello global configuration settings for Reliable Services.</span></span> <span data-ttu-id="35d70-197">Mer information om dessa inställningar finns [konfigurera tillståndskänsliga reliable services](service-fabric-reliable-services-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="35d70-197">For more details on these settings read [Configure stateful reliable services](service-fabric-reliable-services-configuration.md).</span></span>
<span data-ttu-id="35d70-198">hello exemplet nedan visar hur skapade toochange hello hello delade transaktionsloggen som hämtar tooback några tillförlitliga samlingar för tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="35d70-198">hello example below shows how toochange hello hello shared transaction log that gets created tooback any reliable collections for stateful services.</span></span>

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a><span data-ttu-id="35d70-199">Tilläggsfunktioner</span><span class="sxs-lookup"><span data-stu-id="35d70-199">Add-on features</span></span>
<span data-ttu-id="35d70-200">tooconfigure tilläggsfunktioner hello apiVersion måste vara konfigurerade som ”04-2017' eller högre och addonFeatures behöver toobe konfigureras:</span><span class="sxs-lookup"><span data-stu-id="35d70-200">tooconfigure add-on features, hello apiVersion should be configured as '04-2017' or higher, and addonFeatures needs toobe configured:</span></span>

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a><span data-ttu-id="35d70-201">Stöd för behållaren</span><span class="sxs-lookup"><span data-stu-id="35d70-201">Container support</span></span>
<span data-ttu-id="35d70-202">tooenable behållaren stöd för både windows server-behållare och hyper-v-behållare för fristående kluster, hello 'DnsService'-tillägg-funktionen måste toobe aktiverat.</span><span class="sxs-lookup"><span data-stu-id="35d70-202">tooenable container support for both windows server container and hyper-v container for standalone clusters, hello 'DnsService' add-on feature needs toobe enabled.</span></span>


## <a name="next-steps"></a><span data-ttu-id="35d70-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35d70-203">Next steps</span></span>
<span data-ttu-id="35d70-204">När du har en hel ClusterConfig.JSON-fil som är konfigurerade enligt din fristående konfiguration kan du distribuera klustret med följande hello artikel [skapa ett fristående Service Fabric-kluster](service-fabric-cluster-creation-for-windows-server.md) och gå sedan vidare för[visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="35d70-204">Once you have a complete ClusterConfig.JSON file configured as per your standalone cluster setup, you can deploy your cluster by following hello article [Create a standalone Service Fabric cluster](service-fabric-cluster-creation-for-windows-server.md) and then proceed too[visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

