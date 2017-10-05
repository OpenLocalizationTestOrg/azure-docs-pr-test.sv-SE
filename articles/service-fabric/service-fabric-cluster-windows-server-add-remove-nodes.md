---
title: "Lägg till eller ta bort noder i ett fristående Service Fabric-kluster | Microsoft Docs"
description: "Lär dig mer om att lägga till eller ta bort noder till ett Azure Service Fabric-kluster på en fysisk eller virtuell dator som kör Windows Server, som kan vara lokalt eller i något moln."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/02/2017
ms.author: dekapur
ms.openlocfilehash: 9c6035e97de38ff63ef074109afd9f3c7484f828
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a><span data-ttu-id="f61d4-103">Lägg till eller ta bort noder i ett fristående Service Fabric-kluster som körs på Windows Server</span><span class="sxs-lookup"><span data-stu-id="f61d4-103">Add or remove nodes to a standalone Service Fabric cluster running on Windows Server</span></span>
<span data-ttu-id="f61d4-104">När du har [skapa fristående Service Fabric-klustret på Windows Server-datorer](service-fabric-cluster-creation-for-windows-server.md)behoven för din verksamhet kan ändras och du kan behöva lägga till eller ta bort noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="f61d4-104">After you have [created your standalone Service Fabric cluster on Windows Server machines](service-fabric-cluster-creation-for-windows-server.md), your business needs may change and you might need to add or remove nodes to your cluster.</span></span> <span data-ttu-id="f61d4-105">Den här artikeln innehåller detaljerade anvisningar för att åstadkomma detta.</span><span class="sxs-lookup"><span data-stu-id="f61d4-105">This article provides detailed steps to achieve this.</span></span> <span data-ttu-id="f61d4-106">Observera att lägga till/ta bort noden funktionen inte stöds i kluster för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="f61d4-106">Please note that add/remove node functionality is not supported in local development clusters.</span></span>

## <a name="add-nodes-to-your-cluster"></a><span data-ttu-id="f61d4-107">Lägg till noder i klustret</span><span class="sxs-lookup"><span data-stu-id="f61d4-107">Add nodes to your cluster</span></span>
1. <span data-ttu-id="f61d4-108">Förbered den VM/datorn som du vill lägga till i klustret genom att följa anvisningarna i den [förbereda datorer uppfyller kraven för klusterdistribution](service-fabric-cluster-creation-for-windows-server.md) avsnitt</span><span class="sxs-lookup"><span data-stu-id="f61d4-108">Prepare the VM/machine you want to add to your cluster by following the steps mentioned in the [Prepare the machines to meet the prerequisites for cluster deployment](service-fabric-cluster-creation-for-windows-server.md) section</span></span>
2. <span data-ttu-id="f61d4-109">Identifiera vilka feldomän och du ska lägga till den Virtuella/datorn till domänen</span><span class="sxs-lookup"><span data-stu-id="f61d4-109">Identify which fault domain and upgrade domain you are going to add this VM/machine to</span></span>
3. <span data-ttu-id="f61d4-110">Fjärrskrivbord (RDP) till den Virtuella/datorn som du vill lägga till i klustret</span><span class="sxs-lookup"><span data-stu-id="f61d4-110">Remote desktop (RDP) into the VM/machine that you want to add to the cluster</span></span>
4. <span data-ttu-id="f61d4-111">Kopiera eller [hämta fristående paketet för Service Fabric för Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) till VM/datorn och packa upp paketet</span><span class="sxs-lookup"><span data-stu-id="f61d4-111">Copy or [download the standalone package for Service Fabric for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) to the VM/machine and unzip the package</span></span>
5. <span data-ttu-id="f61d4-112">Kör Powershell med förhöjd behörighet och gå till platsen för paketets uppackade</span><span class="sxs-lookup"><span data-stu-id="f61d4-112">Run Powershell with elevated privileges, and navigate to the location of the unzipped package</span></span>
6. <span data-ttu-id="f61d4-113">Kör den *AddNode.ps1* skript med de parametrar som beskriver den nya noden för att lägga till.</span><span class="sxs-lookup"><span data-stu-id="f61d4-113">Run the *AddNode.ps1* script with the parameters describing the new node to add.</span></span> <span data-ttu-id="f61d4-114">Exemplet nedan lägger till en ny nod med namnet VM5, med typen NodeType0 och IP-adress 182.17.34.52, i UD1 och fd: / dc1/r0.</span><span class="sxs-lookup"><span data-stu-id="f61d4-114">The example below adds a new node called VM5, with type NodeType0 and IP address 182.17.34.52, into UD1 and fd:/dc1/r0.</span></span> <span data-ttu-id="f61d4-115">Den *ExistingClusterConnectionEndPoint* är en Anslutningens slutpunkt för en nod som redan finns i det befintliga klustret som kan vara IP-adressen för *alla* nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="f61d4-115">The *ExistingClusterConnectionEndPoint* is a connection endpoint for a node already in the existing cluster, which can be the IP address of *any* node in the cluster.</span></span>

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    <span data-ttu-id="f61d4-116">När skriptet är klar kan du kontrollera om den nya noden har lagts till genom att köra den [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f61d4-116">Once the script finishes running, you can check if the new node has been added by running the [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.</span></span>

7. <span data-ttu-id="f61d4-117">För att säkerställa konsekvens mellan olika noder i klustret måste du påbörja en uppgradering av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f61d4-117">To ensure consistency across different nodes in the cluster, you must initiate a configuration upgrade.</span></span> <span data-ttu-id="f61d4-118">Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) Hämta senaste konfigurationsfilen och lägga till den nya noden i avsnittet ”noder”.</span><span class="sxs-lookup"><span data-stu-id="f61d4-118">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) to get the latest configuration file and add the newly added node to "Nodes" section.</span></span> <span data-ttu-id="f61d4-119">Det rekommenderas också att alltid har den senaste klusterkonfigurationen i det fallet behöver du redploy ett kluster med samma konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f61d4-119">It is also recommended to always have the latest cluster configuration available in the case that you need to redploy a cluster with the same configuration.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. <span data-ttu-id="f61d4-120">Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) att påbörja uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-120">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    <span data-ttu-id="f61d4-121">Du kan övervaka förloppet för uppgraderingen på Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="f61d4-121">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="f61d4-122">Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="f61d4-122">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a><span data-ttu-id="f61d4-123">Lägg till noder i kluster som har konfigurerats med Windows-säkerhet som använder gMSA</span><span class="sxs-lookup"><span data-stu-id="f61d4-123">Add nodes to clusters configured with Windows Security using gMSA</span></span>
<span data-ttu-id="f61d4-124">För kluster som har konfigurerats med gruppen hanteras Service Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx), kan en ny nod läggas till med hjälp av en uppgradering av konfiguration:</span><span class="sxs-lookup"><span data-stu-id="f61d4-124">For clusters configured with Group Managed Service Account(gMSA)(https://technet.microsoft.com/library/hh831782.aspx), a new node can be added using a configuration upgrade:</span></span>
1. <span data-ttu-id="f61d4-125">Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) på någon av de befintliga noderna få den senaste konfigurationsfilen och lägga till information om den nya noden som du vill lägga till i avsnittet ”noder”.</span><span class="sxs-lookup"><span data-stu-id="f61d4-125">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) on any of the existing nodes to get the latest configuration file and add details about the new node you want to add in the "Nodes" section.</span></span> <span data-ttu-id="f61d4-126">Kontrollera att den nya noden är en del av samma hanteras gruppkontot.</span><span class="sxs-lookup"><span data-stu-id="f61d4-126">Make sure the new node is part of the same group managed account.</span></span> <span data-ttu-id="f61d4-127">Det här kontot måste vara administratör på alla datorer.</span><span class="sxs-lookup"><span data-stu-id="f61d4-127">This account should be an Administrator on all machines.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. <span data-ttu-id="f61d4-128">Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) att påbörja uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-128">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    <span data-ttu-id="f61d4-129">Du kan övervaka förloppet för uppgraderingen på Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="f61d4-129">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="f61d4-130">Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="f61d4-130">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-node-types-to-your-cluster"></a><span data-ttu-id="f61d4-131">Lägg till nodtyper i klustret</span><span class="sxs-lookup"><span data-stu-id="f61d4-131">Add node types to your cluster</span></span>
<span data-ttu-id="f61d4-132">Ändra konfigurationen för att inkludera den nya nodtypen i ”nodetypes får” avsnittet under ”egenskaper” och börja en konfiguration för att lägga till en ny nod, uppgradera med hjälp av [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="f61d4-132">In order to add a new node type, modify your configuration to include the new node type in "NodeTypes" section under "Properties" and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span> <span data-ttu-id="f61d4-133">När uppgraderingen är klar kan du lägga till nya noder i klustret med den här nodtypen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-133">Once the upgrade completes, you can add new nodes to your cluster with this node type.</span></span>

## <a name="remove-nodes-from-your-cluster"></a><span data-ttu-id="f61d4-134">Ta bort noder från klustret</span><span class="sxs-lookup"><span data-stu-id="f61d4-134">Remove nodes from your cluster</span></span>
<span data-ttu-id="f61d4-135">En nod kan tas bort från ett kluster med en konfiguration uppgradering på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f61d4-135">A node can be removed from a cluster using a configuration upgrade, in the following manner:</span></span>

1. <span data-ttu-id="f61d4-136">Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) att hämta den senaste konfigurationsfilen och *ta bort* noden från ”noder” i avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f61d4-136">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) to get the latest configuration file and *remove* the node from "Nodes" section.</span></span>
<span data-ttu-id="f61d4-137">Lägga till parametern ”NodesToBeRemoved” till ”inställningar” avsnitt i avsnittet ”FabricSettings”.</span><span class="sxs-lookup"><span data-stu-id="f61d4-137">Add the "NodesToBeRemoved" parameter to "Setup" section inside "FabricSettings" section.</span></span> <span data-ttu-id="f61d4-138">”Värde” ska vara en kommaavgränsad lista över nodnamn med noder som måste tas bort.</span><span class="sxs-lookup"><span data-stu-id="f61d4-138">The "value" should be a comma separated list of node names of nodes that need to be removed.</span></span>

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. <span data-ttu-id="f61d4-139">Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) att påbörja uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-139">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    <span data-ttu-id="f61d4-140">Du kan övervaka förloppet för uppgraderingen på Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="f61d4-140">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="f61d4-141">Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="f61d4-141">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

> [!NOTE]
> <span data-ttu-id="f61d4-142">Borttagning av noder kan starta flera uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="f61d4-142">Removal of nodes may initiate multiple upgrades.</span></span> <span data-ttu-id="f61d4-143">Vissa noder har markerats med `IsSeedNode=”true”` tagga och kan identifieras genom att fråga klustret manifest med `Get-ServiceFabricClusterManifest`.</span><span class="sxs-lookup"><span data-stu-id="f61d4-143">Some nodes are marked with `IsSeedNode=”true”` tag and can be identified by querying the cluster manifest using `Get-ServiceFabricClusterManifest`.</span></span> <span data-ttu-id="f61d4-144">Borttagning av dessa noder kan ta längre tid än andra eftersom seed-noder måste flyttas i dessa scenarier.</span><span class="sxs-lookup"><span data-stu-id="f61d4-144">Removal of such nodes may take longer than others since the seed nodes will have to be moved around in such scenarios.</span></span> <span data-ttu-id="f61d4-145">Klustret måste upprätthålla minst 3 primära noden typen noder.</span><span class="sxs-lookup"><span data-stu-id="f61d4-145">The cluster must maintain a minimum of 3 primary node type nodes.</span></span>
> 
> 

### <a name="remove-node-types-from-your-cluster"></a><span data-ttu-id="f61d4-146">Ta bort nod från klustret</span><span class="sxs-lookup"><span data-stu-id="f61d4-146">Remove node types from your cluster</span></span>
<span data-ttu-id="f61d4-147">Innan du tar bort en nodtyp, kontrollera om det finns några noder som refererar till nodtypen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-147">Before removing a node type, please double check if there are any nodes referencing the node type.</span></span> <span data-ttu-id="f61d4-148">Ta bort dessa noder innan du tar bort motsvarande nodtypen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-148">Remove these nodes before removing the corresponding node type.</span></span> <span data-ttu-id="f61d4-149">När alla motsvarande noder har tagits bort kan du ta bort NodeType från klusterkonfigurationen och börja en konfiguration uppgradera med hjälp av [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="f61d4-149">Once all corresponding nodes are removed, you can remove the NodeType from the cluster configuration and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span>


### <a name="replace-primary-nodes-of-your-cluster"></a><span data-ttu-id="f61d4-150">Ersätt primära noder i klustret</span><span class="sxs-lookup"><span data-stu-id="f61d4-150">Replace primary nodes of your cluster</span></span>
<span data-ttu-id="f61d4-151">Ersättning av primära noder måste vara utförs en nod efter en annan, i stället för att ta bort och sedan lägga till i batchar.</span><span class="sxs-lookup"><span data-stu-id="f61d4-151">The replacement of primary nodes should be performed one node after another, instead of removing and then adding in batches.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f61d4-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f61d4-152">Next steps</span></span>
* [<span data-ttu-id="f61d4-153">Konfigurationsinställningar för fristående Windows-kluster</span><span class="sxs-lookup"><span data-stu-id="f61d4-153">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="f61d4-154">Skydda ett fristående kluster på Windows med X509 certifikat</span><span class="sxs-lookup"><span data-stu-id="f61d4-154">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)
* [<span data-ttu-id="f61d4-155">Skapa ett fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows</span><span class="sxs-lookup"><span data-stu-id="f61d4-155">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)

