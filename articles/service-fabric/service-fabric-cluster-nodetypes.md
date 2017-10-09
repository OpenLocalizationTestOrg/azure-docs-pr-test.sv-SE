---
title: "aaaService Fabric nodtyper och Skalningsuppsättningar | Microsoft Docs"
description: "Beskriver hur Service Fabric-nodtyper relaterade tooVM Skaluppsättningar och hur tooremote ansluter tooa VM Scale Set-instans eller en nod i klustret."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 830ea2816f5864de146a77483c85de26f91c2425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a><span data-ttu-id="ffab5-103">hello förhållandet mellan Service Fabric-nodtyper och Skalningsuppsättningar i virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ffab5-103">hello relationship between Service Fabric node types and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="ffab5-104">Skaluppsättningar för den virtuella datorn är en Azure Compute resurs kan du använda toodeploy och hantera en samling med virtuella datorer som en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="ffab5-104">Virtual Machine Scale Sets are an Azure Compute resource you can use toodeploy and manage a collection of virtual machines as a set.</span></span> <span data-ttu-id="ffab5-105">Varje nodtyp som definieras i Service Fabric-klustret har konfigurerats som en separat Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ffab5-105">Every node type that is defined in a Service Fabric cluster is set up as a separate VM Scale Set.</span></span> <span data-ttu-id="ffab5-106">Varje nodtyp kan sedan skalas upp eller ned separat, har olika uppsättningar av öppna portar och kan ha olika kapacitetsdata.</span><span class="sxs-lookup"><span data-stu-id="ffab5-106">Each node type can then be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>

<span data-ttu-id="ffab5-107">hello följande skärmbild visar ett kluster med två nodtyper: FrontEnd och BackEnd.</span><span class="sxs-lookup"><span data-stu-id="ffab5-107">hello following screen shot shows a cluster that has two node types: FrontEnd and BackEnd.</span></span>  <span data-ttu-id="ffab5-108">Varje nodtyp har fem noder.</span><span class="sxs-lookup"><span data-stu-id="ffab5-108">Each node type has five nodes each.</span></span>

![Skärmbild av ett kluster med två nodtyper][NodeTypes]

## <a name="mapping-vm-scale-set-instances-toonodes"></a><span data-ttu-id="ffab5-110">Mappning av Skaluppsättning instanser toonodes</span><span class="sxs-lookup"><span data-stu-id="ffab5-110">Mapping VM Scale Set instances toonodes</span></span>
<span data-ttu-id="ffab5-111">Som du ser ovan hello VM Scale Set instanser börja från instans 0 och går sedan upp.</span><span class="sxs-lookup"><span data-stu-id="ffab5-111">As you can see above, hello VM Scale Set instances start from instance 0 and then goes up.</span></span> <span data-ttu-id="ffab5-112">hello numrering återspeglas i hello namn.</span><span class="sxs-lookup"><span data-stu-id="ffab5-112">hello numbering is reflected in hello names.</span></span> <span data-ttu-id="ffab5-113">BackEnd_0 är till exempel hello serverdelens VM Scale Set-instans 0.</span><span class="sxs-lookup"><span data-stu-id="ffab5-113">For example, BackEnd_0 is instance 0 of hello BackEnd VM Scale Set.</span></span> <span data-ttu-id="ffab5-114">Den här viss VM Scale Set har fem instanser, med namnet BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 och BackEnd_4.</span><span class="sxs-lookup"><span data-stu-id="ffab5-114">This particular VM Scale Set has five instances, named BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 and BackEnd_4.</span></span>

<span data-ttu-id="ffab5-115">När du skalar upp en VM Scale Set skapas en ny instans.</span><span class="sxs-lookup"><span data-stu-id="ffab5-115">When you scale up a VM Scale Set a new instance is created.</span></span> <span data-ttu-id="ffab5-116">hello nya VM Scale Set instansnamn kommer normalt att hello VM Scale Set namn + hello nästa instansnummer.</span><span class="sxs-lookup"><span data-stu-id="ffab5-116">hello new VM Scale Set instance name will typically be hello VM Scale Set name + hello next instance number.</span></span> <span data-ttu-id="ffab5-117">I vårt exempel är BackEnd_5.</span><span class="sxs-lookup"><span data-stu-id="ffab5-117">In our example, it is BackEnd_5.</span></span>

## <a name="mapping-vm-scale-set-load-balancers-tooeach-node-typevm-scale-set"></a><span data-ttu-id="ffab5-118">Mappning av VM skaluppsättning för att läsa in belastningsutjämning typ och VM-nod tooeach Skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="ffab5-118">Mapping VM scale set load balancers tooeach node type/VM Scale Set</span></span>
<span data-ttu-id="ffab5-119">Om du har distribuerat ditt kluster från hello-portalen eller har hello exempel Resource Manager-mall som vi angav, sedan när du får en lista över alla resurser i en resursgrupp ser du hello belastningsutjämning för varje typ av VM Scale Set eller nod.</span><span class="sxs-lookup"><span data-stu-id="ffab5-119">If you have deployed your cluster from hello portal or have used hello sample Resource Manager template that we provided, then when you get a list of all resources under a Resource Group then you will see hello load balancers for each VM Scale Set or node type.</span></span>

<span data-ttu-id="ffab5-120">hello namn skulle något som liknar: **LB -&lt;NodeType namnet&gt;**.</span><span class="sxs-lookup"><span data-stu-id="ffab5-120">hello name would something like: **LB-&lt;NodeType name&gt;**.</span></span> <span data-ttu-id="ffab5-121">Till exempel LB-sfcluster4doc-0, som visas i den här skärmbilden:</span><span class="sxs-lookup"><span data-stu-id="ffab5-121">For example, LB-sfcluster4doc-0, as shown in this screenshot:</span></span>

![Resurser][Resources]

## <a name="remote-connect-tooa-vm-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="ffab5-123">Fjärranslutning tooa VM Scale Set-instans eller en klusternod</span><span class="sxs-lookup"><span data-stu-id="ffab5-123">Remote connect tooa VM Scale Set instance or a cluster node</span></span>
<span data-ttu-id="ffab5-124">Varje nodtyp som definieras i ett kluster har konfigurerats som en separat Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ffab5-124">Every Node type that is defined in a cluster is set up as a separate VM Scale Set.</span></span>  <span data-ttu-id="ffab5-125">Att innebär hello nodtyper kan skalas upp eller ned separat och kan göras av olika VM SKU: er.</span><span class="sxs-lookup"><span data-stu-id="ffab5-125">That means hello node types can be scaled up or down independently and can be made of different VM SKUs.</span></span> <span data-ttu-id="ffab5-126">Till skillnad från en enda instans VMs får hello VM Scale Set instanser inte en virtuell IP-adress för sina egna.</span><span class="sxs-lookup"><span data-stu-id="ffab5-126">Unlike single instance VMs, hello VM Scale Set instances do not get a virtual IP address of their own.</span></span> <span data-ttu-id="ffab5-127">Så kan det ta en stund ansluta utmaning när du söker efter en IP-adress och port som du kan använda tooremote tooa specifika instansen.</span><span class="sxs-lookup"><span data-stu-id="ffab5-127">So it can be a bit challenging when you are looking for an IP address and port that you can use tooremote connect tooa specific instance.</span></span>

<span data-ttu-id="ffab5-128">Här är hello-åtgärder som du kan följa toodiscover dem.</span><span class="sxs-lookup"><span data-stu-id="ffab5-128">Here are hello steps you can follow toodiscover them.</span></span>

### <a name="step-1-find-out-hello-virtual-ip-address-for-hello-node-type-and-then-inbound-nat-rules-for-rdp"></a><span data-ttu-id="ffab5-129">Steg 1: Ta reda på hello virtuella IP-adressen för hello nodtypen och inkommande NAT-regler för RDP</span><span class="sxs-lookup"><span data-stu-id="ffab5-129">Step 1: Find out hello virtual IP address for hello node type and then Inbound NAT rules for RDP</span></span>
<span data-ttu-id="ffab5-130">I ordning tooget att du behöver tooget hello inkommande NAT-regler värden som definierats som en del av hello resursdefinitionen för **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="ffab5-130">In order tooget that, you need tooget hello inbound NAT rules values that were defined as a part of hello resource definition for **Microsoft.Network/loadBalancers**.</span></span>

<span data-ttu-id="ffab5-131">Navigera i hello portal toohello Load balancer bladet och sedan **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="ffab5-131">In hello portal, navigate toohello Load balancer blade and then **Settings**.</span></span>

![LBBlade][LBBlade]

<span data-ttu-id="ffab5-133">I **inställningar**, klicka på **inkommande NAT-regler**.</span><span class="sxs-lookup"><span data-stu-id="ffab5-133">In **Settings**, click on **Inbound NAT rules**.</span></span> <span data-ttu-id="ffab5-134">Det här nu ger du hello IP-adress och port som du kan använda tooremote ansluta toohello första VM Scale Set-instansen.</span><span class="sxs-lookup"><span data-stu-id="ffab5-134">This now gives you hello IP address and port that you can use tooremote connect toohello first VM Scale Set instance.</span></span> <span data-ttu-id="ffab5-135">I hello skärmbilden nedan, är det **104.42.106.156** och **3389**</span><span class="sxs-lookup"><span data-stu-id="ffab5-135">In hello screenshot below, it is **104.42.106.156** and **3389**</span></span>

![NATRules][NATRules]

### <a name="step-2-find-out-hello-port-that-you-can-use-tooremote-connect-toohello-specific-vm-scale-set-instancenode"></a><span data-ttu-id="ffab5-137">Steg 2: Ta reda på hello-port som du kan använda tooremote Anslut toohello specifik VM Scale Set instans/nod</span><span class="sxs-lookup"><span data-stu-id="ffab5-137">Step 2: Find out hello port that you can use tooremote connect toohello specific VM Scale Set instance/node</span></span>
<span data-ttu-id="ffab5-138">Tidigare i det här dokumentet I talade vi om hur hello VM Scale Set instanser mappa toohello noder.</span><span class="sxs-lookup"><span data-stu-id="ffab5-138">Earlier in this document, I talked about how hello VM Scale Set instances map toohello nodes.</span></span> <span data-ttu-id="ffab5-139">Vi använder den toofigure ut hello exakt porten.</span><span class="sxs-lookup"><span data-stu-id="ffab5-139">We will use that toofigure out hello exact port.</span></span>

<span data-ttu-id="ffab5-140">hello portar tilldelas i stigande ordning hello VM Scale Set-instans.</span><span class="sxs-lookup"><span data-stu-id="ffab5-140">hello ports are allocated in ascending order of hello VM Scale Set instance.</span></span> <span data-ttu-id="ffab5-141">i vårt exempel för hello klientdel nodtyp finns så hello portar för varje hello fem instanser hello följande.</span><span class="sxs-lookup"><span data-stu-id="ffab5-141">so in my example for hello FrontEnd node type, hello ports for each of hello five instances are hello following.</span></span> <span data-ttu-id="ffab5-142">du nu måste toodo hello samma mappning för din VM Scale Set-instans.</span><span class="sxs-lookup"><span data-stu-id="ffab5-142">you now need toodo hello same mapping for your VM Scale Set instance.</span></span>

| <span data-ttu-id="ffab5-143">**VM Scale Set-instans**</span><span class="sxs-lookup"><span data-stu-id="ffab5-143">**VM Scale Set Instance**</span></span> | <span data-ttu-id="ffab5-144">**Port**</span><span class="sxs-lookup"><span data-stu-id="ffab5-144">**Port**</span></span> |
| --- | --- |
| <span data-ttu-id="ffab5-145">FrontEnd_0</span><span class="sxs-lookup"><span data-stu-id="ffab5-145">FrontEnd_0</span></span> |<span data-ttu-id="ffab5-146">3389</span><span class="sxs-lookup"><span data-stu-id="ffab5-146">3389</span></span> |
| <span data-ttu-id="ffab5-147">FrontEnd_1</span><span class="sxs-lookup"><span data-stu-id="ffab5-147">FrontEnd_1</span></span> |<span data-ttu-id="ffab5-148">3390</span><span class="sxs-lookup"><span data-stu-id="ffab5-148">3390</span></span> |
| <span data-ttu-id="ffab5-149">FrontEnd_2</span><span class="sxs-lookup"><span data-stu-id="ffab5-149">FrontEnd_2</span></span> |<span data-ttu-id="ffab5-150">3391</span><span class="sxs-lookup"><span data-stu-id="ffab5-150">3391</span></span> |
| <span data-ttu-id="ffab5-151">FrontEnd_3</span><span class="sxs-lookup"><span data-stu-id="ffab5-151">FrontEnd_3</span></span> |<span data-ttu-id="ffab5-152">3392</span><span class="sxs-lookup"><span data-stu-id="ffab5-152">3392</span></span> |
| <span data-ttu-id="ffab5-153">FrontEnd_4</span><span class="sxs-lookup"><span data-stu-id="ffab5-153">FrontEnd_4</span></span> |<span data-ttu-id="ffab5-154">3393</span><span class="sxs-lookup"><span data-stu-id="ffab5-154">3393</span></span> |
| <span data-ttu-id="ffab5-155">FrontEnd_5</span><span class="sxs-lookup"><span data-stu-id="ffab5-155">FrontEnd_5</span></span> |<span data-ttu-id="ffab5-156">3394</span><span class="sxs-lookup"><span data-stu-id="ffab5-156">3394</span></span> |

### <a name="step-3-remote-connect-toohello-specific-vm-scale-set-instance"></a><span data-ttu-id="ffab5-157">Steg 3: Fjärranslutning toohello specifika VM Scale Set-instans</span><span class="sxs-lookup"><span data-stu-id="ffab5-157">Step 3: Remote connect toohello specific VM Scale Set instance</span></span>
<span data-ttu-id="ffab5-158">Jag använder anslutning till fjärrskrivbord tooconnect toohello FrontEnd_1 i hello skärmbilden nedan:</span><span class="sxs-lookup"><span data-stu-id="ffab5-158">In hello screenshot below I use Remote Desktop Connection tooconnect toohello FrontEnd_1:</span></span>

![RDP][RDP]

## <a name="how-toochange-hello-rdp-port-range-values"></a><span data-ttu-id="ffab5-160">Hur toochange hello RDP-porten vara värden</span><span class="sxs-lookup"><span data-stu-id="ffab5-160">How toochange hello RDP port range values</span></span>
### <a name="before-cluster-deployment"></a><span data-ttu-id="ffab5-161">Innan du Klusterdistribution</span><span class="sxs-lookup"><span data-stu-id="ffab5-161">Before cluster deployment</span></span>
<span data-ttu-id="ffab5-162">När du ställer in hello-kluster med hjälp av en Resource Manager-mall kan du ange hello intervallet i hello **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="ffab5-162">When you are setting up hello cluster using an Resource Manager template, you can specify hello range in hello **inboundNatPools**.</span></span>

<span data-ttu-id="ffab5-163">Gå toohello resursdefinitionen **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="ffab5-163">Go toohello resource definition for **Microsoft.Network/loadBalancers**.</span></span> <span data-ttu-id="ffab5-164">Under som du hittar hello beskrivning för **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="ffab5-164">Under that you find hello description for **inboundNatPools**.</span></span>  <span data-ttu-id="ffab5-165">Ersätt hello *frontendPortRangeStart* och *frontendPortRangeEnd* värden.</span><span class="sxs-lookup"><span data-stu-id="ffab5-165">Replace hello *frontendPortRangeStart* and *frontendPortRangeEnd* values.</span></span>

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a><span data-ttu-id="ffab5-167">Efter distribution</span><span class="sxs-lookup"><span data-stu-id="ffab5-167">After cluster deployment</span></span>
<span data-ttu-id="ffab5-168">Det här är lite mer invecklad och kan resultera i hello VMs återanvänds.</span><span class="sxs-lookup"><span data-stu-id="ffab5-168">This is a bit more involved and may result in hello VMs getting recycled.</span></span> <span data-ttu-id="ffab5-169">Du har nu tooset nya värden med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ffab5-169">You will now have tooset new values using Azure PowerShell.</span></span> <span data-ttu-id="ffab5-170">Kontrollera att Azure PowerShell 1.0 eller senare är installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="ffab5-170">Make sure that Azure PowerShell 1.0 or later is installed on your machine.</span></span> <span data-ttu-id="ffab5-171">Om du inte har gjort det innan jag starkt föreslår att du följer stegen i hello [hur tooinstall och konfigurera Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="ffab5-171">If you have not done this before, I strongly suggest that you follow hello steps outlined in [How tooinstall and configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="ffab5-172">Logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ffab5-172">Sign in tooyour Azure account.</span></span> <span data-ttu-id="ffab5-173">Om det här PowerShell-kommandot misslyckas av någon anledning, bör du kontrollera om du har Azure PowerShell har installerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="ffab5-173">If this PowerShell command fails for some reason, you should check whether you have Azure PowerShell installed correctly.</span></span>

```
Login-AzureRmAccount
```

<span data-ttu-id="ffab5-174">Kör hello följande tooget information på din belastningsutjämnare och du ser hello värden för hello beskrivning för **inboundNatPools**:</span><span class="sxs-lookup"><span data-stu-id="ffab5-174">Run hello following tooget details on your load balancer and you see hello values for hello description for **inboundNatPools**:</span></span>

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

<span data-ttu-id="ffab5-175">Ange *frontendPortRangeEnd* och *frontendPortRangeStart* toohello värden som du vill.</span><span class="sxs-lookup"><span data-stu-id="ffab5-175">Now set *frontendPortRangeEnd* and *frontendPortRangeStart* toohello values you want.</span></span>

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use hello API version that get returned> -Force
```


## <a name="next-steps"></a><span data-ttu-id="ffab5-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ffab5-176">Next steps</span></span>
* [<span data-ttu-id="ffab5-177">Översikt över hello ”var som helst distribuera” funktionen och en jämförelse med Azure-hanterade kluster</span><span class="sxs-lookup"><span data-stu-id="ffab5-177">Overview of hello "Deploy anywhere" feature and a comparison with Azure-managed clusters</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="ffab5-178">Klustersäkerhet</span><span class="sxs-lookup"><span data-stu-id="ffab5-178">Cluster security</span></span>](service-fabric-cluster-security.md)
* [<span data-ttu-id="ffab5-179">Fabric-SDK och komma igång</span><span class="sxs-lookup"><span data-stu-id="ffab5-179"> Service Fabric SDK and getting started</span></span>](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
