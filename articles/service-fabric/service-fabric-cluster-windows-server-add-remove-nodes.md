---
title: "aaaAdd eller ta bort noder tooa fristående Service Fabric-kluster | Microsoft Docs"
description: "Lär dig hur tooadd eller ta bort noder tooan Azure Service Fabric-kluster på en fysisk eller virtuell dator som kör Windows Server, som kan vara lokalt eller i något moln."
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
ms.openlocfilehash: 1da908ad9840faa052e0b4021bc2d4ce732b02bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-nodes-tooa-standalone-service-fabric-cluster-running-on-windows-server"></a><span data-ttu-id="30140-103">Lägg till eller ta bort noder tooa fristående Service Fabric-kluster körs på Windows Server</span><span class="sxs-lookup"><span data-stu-id="30140-103">Add or remove nodes tooa standalone Service Fabric cluster running on Windows Server</span></span>
<span data-ttu-id="30140-104">När du har [skapa fristående Service Fabric-klustret på Windows Server-datorer](service-fabric-cluster-creation-for-windows-server.md)behoven för din verksamhet kan ändras och du kanske behöver tooadd eller ta bort noder tooyour klustret.</span><span class="sxs-lookup"><span data-stu-id="30140-104">After you have [created your standalone Service Fabric cluster on Windows Server machines](service-fabric-cluster-creation-for-windows-server.md), your business needs may change and you might need tooadd or remove nodes tooyour cluster.</span></span> <span data-ttu-id="30140-105">Den här artikeln innehåller detaljerade anvisningar tooachieve detta.</span><span class="sxs-lookup"><span data-stu-id="30140-105">This article provides detailed steps tooachieve this.</span></span> <span data-ttu-id="30140-106">Observera att lägga till/ta bort noden funktionen inte stöds i kluster för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="30140-106">Please note that add/remove node functionality is not supported in local development clusters.</span></span>

## <a name="add-nodes-tooyour-cluster"></a><span data-ttu-id="30140-107">Lägga till noder tooyour kluster</span><span class="sxs-lookup"><span data-stu-id="30140-107">Add nodes tooyour cluster</span></span>
1. <span data-ttu-id="30140-108">Förbered hello VM/dator du vill tooadd tooyour klustret genom att följa anvisningarna i hello hello [Förbered hello datorer toomeet hello krav för klusterdistribution](service-fabric-cluster-creation-for-windows-server.md) avsnitt</span><span class="sxs-lookup"><span data-stu-id="30140-108">Prepare hello VM/machine you want tooadd tooyour cluster by following hello steps mentioned in hello [Prepare hello machines toomeet hello prerequisites for cluster deployment](service-fabric-cluster-creation-for-windows-server.md) section</span></span>
2. <span data-ttu-id="30140-109">Identifiera vilka feldomän och uppgraderingsdomänen som du är pågående tooadd den VM/datorn</span><span class="sxs-lookup"><span data-stu-id="30140-109">Identify which fault domain and upgrade domain you are going tooadd this VM/machine to</span></span>
3. <span data-ttu-id="30140-110">Fjärrskrivbord (RDP) till hello VM/dator som du vill tooadd toohello kluster</span><span class="sxs-lookup"><span data-stu-id="30140-110">Remote desktop (RDP) into hello VM/machine that you want tooadd toohello cluster</span></span>
4. <span data-ttu-id="30140-111">Kopiera eller [hämta hello fristående paketet för Service Fabric för Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) toohello VM/datorn och packa hello-paket</span><span class="sxs-lookup"><span data-stu-id="30140-111">Copy or [download hello standalone package for Service Fabric for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) toohello VM/machine and unzip hello package</span></span>
5. <span data-ttu-id="30140-112">Kör Powershell med förhöjd behörighet och gå toohello platsen för hello uppackade</span><span class="sxs-lookup"><span data-stu-id="30140-112">Run Powershell with elevated privileges, and navigate toohello location of hello unzipped package</span></span>
6. <span data-ttu-id="30140-113">Kör hello *AddNode.ps1* skript med hello parametrar som beskriver hello nya nod tooadd.</span><span class="sxs-lookup"><span data-stu-id="30140-113">Run hello *AddNode.ps1* script with hello parameters describing hello new node tooadd.</span></span> <span data-ttu-id="30140-114">hello exemplet nedan lägger till en ny nod med namnet VM5, med typen NodeType0 och IP-adress 182.17.34.52, i UD1 och fd: / dc1/r0.</span><span class="sxs-lookup"><span data-stu-id="30140-114">hello example below adds a new node called VM5, with type NodeType0 and IP address 182.17.34.52, into UD1 and fd:/dc1/r0.</span></span> <span data-ttu-id="30140-115">Hej *ExistingClusterConnectionEndPoint* finns redan en Anslutningens slutpunkt för en nod i hello befintligt kluster, vilket kan vara hello IP-adressen för *alla* nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="30140-115">hello *ExistingClusterConnectionEndPoint* is a connection endpoint for a node already in hello existing cluster, which can be hello IP address of *any* node in hello cluster.</span></span>

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    <span data-ttu-id="30140-116">När hello skriptet är klar kan du kontrollera om hello ny nod har lagts till genom att köra hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="30140-116">Once hello script finishes running, you can check if hello new node has been added by running hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.</span></span>

7. <span data-ttu-id="30140-117">tooensure konsekvent på olika noder i klustret hello, måste du påbörja en uppgradering av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="30140-117">tooensure consistency across different nodes in hello cluster, you must initiate a configuration upgrade.</span></span> <span data-ttu-id="30140-118">Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello senaste konfigurationsfilen och Lägg till hello nyligen tillagda noden för avsnittet ”noder”.</span><span class="sxs-lookup"><span data-stu-id="30140-118">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello latest configuration file and add hello newly added node too"Nodes" section.</span></span> <span data-ttu-id="30140-119">Det rekommenderas också tooalways har hello senaste klusterkonfigurationen tillgängliga hello om du behöver tooredploy ett kluster med hello samma konfiguration.</span><span class="sxs-lookup"><span data-stu-id="30140-119">It is also recommended tooalways have hello latest cluster configuration available in hello case that you need tooredploy a cluster with hello same configuration.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. <span data-ttu-id="30140-120">Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="30140-120">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    <span data-ttu-id="30140-121">Du kan övervaka hello hello uppgraderingen på Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="30140-121">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="30140-122">Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="30140-122">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-nodes-tooclusters-configured-with-windows-security-using-gmsa"></a><span data-ttu-id="30140-123">Lägg till noder tooclusters konfigurerade med Windows-säkerhet som använder gMSA</span><span class="sxs-lookup"><span data-stu-id="30140-123">Add nodes tooclusters configured with Windows Security using gMSA</span></span>
<span data-ttu-id="30140-124">För kluster som har konfigurerats med gruppen hanteras Service Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx), kan en ny nod läggas till med hjälp av en uppgradering av konfiguration:</span><span class="sxs-lookup"><span data-stu-id="30140-124">For clusters configured with Group Managed Service Account(gMSA)(https://technet.microsoft.com/library/hh831782.aspx), a new node can be added using a configuration upgrade:</span></span>
1. <span data-ttu-id="30140-125">Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) på någon av hello befintliga noderna tooget hello senaste konfigurationsfilen och Lägg till information om hello ny nod som du vill att tooadd i-noder ”hello” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="30140-125">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) on any of hello existing nodes tooget hello latest configuration file and add details about hello new node you want tooadd in hello "Nodes" section.</span></span> <span data-ttu-id="30140-126">Kontrollera att nya hello-noden är en del av hello samma grupp hanterat konto.</span><span class="sxs-lookup"><span data-stu-id="30140-126">Make sure hello new node is part of hello same group managed account.</span></span> <span data-ttu-id="30140-127">Det här kontot måste vara administratör på alla datorer.</span><span class="sxs-lookup"><span data-stu-id="30140-127">This account should be an Administrator on all machines.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. <span data-ttu-id="30140-128">Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="30140-128">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>
    ```
    <span data-ttu-id="30140-129">Du kan övervaka hello hello uppgraderingen på Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="30140-129">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="30140-130">Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="30140-130">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-node-types-tooyour-cluster"></a><span data-ttu-id="30140-131">Lägga till noden typer tooyour kluster</span><span class="sxs-lookup"><span data-stu-id="30140-131">Add node types tooyour cluster</span></span>
<span data-ttu-id="30140-132">I ordning tooadd en ny nodtyp, ändra din konfiguration tooinclude hello nya nodtyp i ”nodetypes får” under ”egenskaper” och börjar en konfiguration uppgradera med hjälp av [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="30140-132">In order tooadd a new node type, modify your configuration tooinclude hello new node type in "NodeTypes" section under "Properties" and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span> <span data-ttu-id="30140-133">När hello uppgraderingen är klar kan du lägga till nya noder tooyour klustret med den här nodtypen.</span><span class="sxs-lookup"><span data-stu-id="30140-133">Once hello upgrade completes, you can add new nodes tooyour cluster with this node type.</span></span>

## <a name="remove-nodes-from-your-cluster"></a><span data-ttu-id="30140-134">Ta bort noder från klustret</span><span class="sxs-lookup"><span data-stu-id="30140-134">Remove nodes from your cluster</span></span>
<span data-ttu-id="30140-135">En nod kan tas bort från ett kluster med en konfiguration uppgradering i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="30140-135">A node can be removed from a cluster using a configuration upgrade, in hello following manner:</span></span>

1. <span data-ttu-id="30140-136">Kör [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello senaste konfigurationsfilen och *ta bort* hello nod från avsnittet ”noder”.</span><span class="sxs-lookup"><span data-stu-id="30140-136">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello latest configuration file and *remove* hello node from "Nodes" section.</span></span>
<span data-ttu-id="30140-137">Lägg till hello ”NodesToBeRemoved” parametern för ”inställningar” avsnittet i avsnittet ”FabricSettings”.</span><span class="sxs-lookup"><span data-stu-id="30140-137">Add hello "NodesToBeRemoved" parameter too"Setup" section inside "FabricSettings" section.</span></span> <span data-ttu-id="30140-138">Hej ”värde” ska vara en kommaavgränsad lista över nodnamn noder som behöver toobe tas bort.</span><span class="sxs-lookup"><span data-stu-id="30140-138">hello "value" should be a comma separated list of node names of nodes that need toobe removed.</span></span>

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
2. <span data-ttu-id="30140-139">Kör [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="30140-139">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    <span data-ttu-id="30140-140">Du kan övervaka hello hello uppgraderingen på Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="30140-140">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="30140-141">Du kan också köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="30140-141">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

> [!NOTE]
> <span data-ttu-id="30140-142">Borttagning av noder kan starta flera uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="30140-142">Removal of nodes may initiate multiple upgrades.</span></span> <span data-ttu-id="30140-143">Vissa noder har markerats med `IsSeedNode=”true”` tagga och kan identifieras genom att fråga hello klustret manifest med `Get-ServiceFabricClusterManifest`.</span><span class="sxs-lookup"><span data-stu-id="30140-143">Some nodes are marked with `IsSeedNode=”true”` tag and can be identified by querying hello cluster manifest using `Get-ServiceFabricClusterManifest`.</span></span> <span data-ttu-id="30140-144">Borttagning av dessa noder kan ta längre tid än andra eftersom hello seed noder måste toobe flyttas i dessa scenarier.</span><span class="sxs-lookup"><span data-stu-id="30140-144">Removal of such nodes may take longer than others since hello seed nodes will have toobe moved around in such scenarios.</span></span> <span data-ttu-id="30140-145">hello klustret måste upprätthålla minst 3 primära noden typen noder.</span><span class="sxs-lookup"><span data-stu-id="30140-145">hello cluster must maintain a minimum of 3 primary node type nodes.</span></span>
> 
> 

### <a name="remove-node-types-from-your-cluster"></a><span data-ttu-id="30140-146">Ta bort nod från klustret</span><span class="sxs-lookup"><span data-stu-id="30140-146">Remove node types from your cluster</span></span>
<span data-ttu-id="30140-147">Innan du tar bort en nodtyp, kontrollera om det finns några noder som refererar till hello nodtypen.</span><span class="sxs-lookup"><span data-stu-id="30140-147">Before removing a node type, please double check if there are any nodes referencing hello node type.</span></span> <span data-ttu-id="30140-148">Ta bort dessa noder innan du tar bort hello motsvarande nodtypen.</span><span class="sxs-lookup"><span data-stu-id="30140-148">Remove these nodes before removing hello corresponding node type.</span></span> <span data-ttu-id="30140-149">När alla motsvarande noder har tagits bort kan du ta bort hello NodeType från hello klusterkonfigurationen och börja en konfiguration uppgradera med hjälp av [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="30140-149">Once all corresponding nodes are removed, you can remove hello NodeType from hello cluster configuration and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span>


### <a name="replace-primary-nodes-of-your-cluster"></a><span data-ttu-id="30140-150">Ersätt primära noder i klustret</span><span class="sxs-lookup"><span data-stu-id="30140-150">Replace primary nodes of your cluster</span></span>
<span data-ttu-id="30140-151">hello ersättning av primära noder måste vara utförs en nod efter en annan, i stället för att ta bort och sedan lägga till i batchar.</span><span class="sxs-lookup"><span data-stu-id="30140-151">hello replacement of primary nodes should be performed one node after another, instead of removing and then adding in batches.</span></span>


## <a name="next-steps"></a><span data-ttu-id="30140-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30140-152">Next steps</span></span>
* [<span data-ttu-id="30140-153">Konfigurationsinställningar för fristående Windows-kluster</span><span class="sxs-lookup"><span data-stu-id="30140-153">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="30140-154">Skydda ett fristående kluster på Windows med X509 certifikat</span><span class="sxs-lookup"><span data-stu-id="30140-154">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)
* [<span data-ttu-id="30140-155">Skapa ett fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows</span><span class="sxs-lookup"><span data-stu-id="30140-155">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)

