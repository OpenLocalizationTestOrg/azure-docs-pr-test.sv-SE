---
title: "Länka ett virtuellt nätverk tooan ExpressRoute-krets: PowerShell: Azure | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur toolink virtuella nätverk (Vnet) tooExpressRoute kretsar med hjälp av hello Resource Manager-distributionsmodellen och PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: daacb6e5-705a-456f-9a03-c4fc3f8c1f7e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: ganesr
ms.openlocfilehash: e75a9f6b42fa8e1a579e4f19882ec99b277b545f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="92af4-103">Ansluta ett virtuellt nätverk tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="92af4-103">Connect a virtual network tooan ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="92af4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="92af4-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="92af4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="92af4-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="92af4-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="92af4-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="92af4-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="92af4-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="92af4-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="92af4-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="92af4-109">Den här artikeln hjälper dig att länka virtuella nätverk (Vnet) tooAzure ExpressRoute-kretsar med hello Resource Manager-distributionsmodellen och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92af4-109">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello Resource Manager deployment model and PowerShell.</span></span> <span data-ttu-id="92af4-110">Virtuella nätverk kan antingen vara i hello samma prenumeration eller en del av en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="92af4-110">Virtual networks can either be in hello same subscription or part of another subscription.</span></span> <span data-ttu-id="92af4-111">Den här artikeln visar också hur tooupdate ett virtuellt nätverk länkar.</span><span class="sxs-lookup"><span data-stu-id="92af4-111">This article also shows you how tooupdate a virtual network link.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="92af4-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="92af4-112">Before you begin</span></span>
* <span data-ttu-id="92af4-113">Installera hello senaste versionen av hello Azure PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="92af4-113">Install hello latest version of hello Azure PowerShell modules.</span></span> <span data-ttu-id="92af4-114">Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="92af4-114">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="92af4-115">Granska hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="92af4-115">Review hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="92af4-116">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="92af4-116">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="92af4-117">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha hello krets aktiveras med anslutningsleverantören.</span><span class="sxs-lookup"><span data-stu-id="92af4-117">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="92af4-118">Se till att du har privat Azure-peering konfigurerats för kretsen.</span><span class="sxs-lookup"><span data-stu-id="92af4-118">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="92af4-119">Se hello [konfigurera routning](expressroute-howto-routing-arm.md) artikel routning anvisningar.</span><span class="sxs-lookup"><span data-stu-id="92af4-119">See hello [configure routing](expressroute-howto-routing-arm.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="92af4-120">Kontrollera att privat Azure-peering har konfigurerats och hello BGP-peering mellan ditt nätverk och Microsoft är igång så att du kan aktivera anslutning för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="92af4-120">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="92af4-121">Se till att du har ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="92af4-121">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="92af4-122">Följ instruktionerna för hello för[skapa en virtuell nätverksgateway för ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="92af4-122">Follow hello instructions too[create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="92af4-123">En virtuell nätverksgateway expressroute använder hello GatewayType ExpressRoute, inte VPN.</span><span class="sxs-lookup"><span data-stu-id="92af4-123">A virtual network gateway for ExpressRoute uses hello GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="92af4-124">Du kan länka upp too10-virtuella nätverk tooa standard ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="92af4-124">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="92af4-125">Alla virtuella nätverk måste vara i hello samma geopolitiska region när du använder en standard ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="92af4-125">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="92af4-126">Du kan länka en virtuella nätverk utanför hello geopolitiska regionens hello ExpressRoute-krets eller ansluta ett stort antal virtuella nätverk tooyour ExpressRoute-krets om du har aktiverat hello ExpressRoute premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="92af4-126">You can link a virtual networks outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="92af4-127">Kontrollera hello [vanliga frågor och svar](expressroute-faqs.md) för mer information om hello premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="92af4-127">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>


## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="92af4-128">Ansluta ett virtuellt nätverk i hello samma prenumeration tooa krets</span><span class="sxs-lookup"><span data-stu-id="92af4-128">Connect a virtual network in hello same subscription tooa circuit</span></span>
<span data-ttu-id="92af4-129">Du kan ansluta ett virtuellt nätverk gateway tooan ExpressRoute-kretsen med hjälp av hello följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="92af4-129">You can connect a virtual network gateway tooan ExpressRoute circuit by using hello following cmdlet.</span></span> <span data-ttu-id="92af4-130">Kontrollera att hello virtuella nätverkets gateway har skapats och är redo för att länka innan du kör cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="92af4-130">Make sure that hello virtual network gateway is created and is ready for linking before you run hello cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="92af4-131">Ansluta ett virtuellt nätverk i en annan prenumeration tooa-krets</span><span class="sxs-lookup"><span data-stu-id="92af4-131">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="92af4-132">Du kan dela en ExpressRoute-krets över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="92af4-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="92af4-133">hello följande bild visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="92af4-133">hello following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="92af4-134">Varje hello mindre moln inom hello stora moln är används toorepresent prenumerationer som tillhör toodifferent avdelningar inom en organisation.</span><span class="sxs-lookup"><span data-stu-id="92af4-134">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="92af4-135">Var och en av hello avdelningar inom hello organisation kan använda sina egna prenumeration för att distribuera sina tjänster, men de kan dela ett ExpressRoute-kretsen tooconnect tillbaka tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="92af4-135">Each of hello departments within hello organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="92af4-136">En avdelning (i det här exemplet: IT) kan äga hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="92af4-136">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="92af4-137">Andra prenumerationer i hello organisation kan använda hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="92af4-137">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="92af4-138">Anslutningar och bandbredd avgifter för hello ExpressRoute-krets kommer att tillämpas toohello prenumerationsägaren.</span><span class="sxs-lookup"><span data-stu-id="92af4-138">Connectivity and bandwidth charges for hello ExpressRoute circuit will be applied toohello subscription owner.</span></span> <span data-ttu-id="92af4-139">Alla virtuella nätverk dela hello samma bandbredd.</span><span class="sxs-lookup"><span data-stu-id="92af4-139">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="92af4-141">Administration - krets ägare och krets användare</span><span class="sxs-lookup"><span data-stu-id="92af4-141">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="92af4-142">hello 'kretsägaren' är en behörig Privilegierade användare i hello ExpressRoute-kretsen resurs.</span><span class="sxs-lookup"><span data-stu-id="92af4-142">hello 'circuit owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="92af4-143">Hej kretsägaren kan skapa tillstånd som kan lösas av krets-användare.</span><span class="sxs-lookup"><span data-stu-id="92af4-143">hello circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="92af4-144">Kretsen användare är ägare till virtuella nätverk gateways som inte är inom hello samma prenumeration som hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="92af4-144">Circuit users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="92af4-145">Kretsen användare kan lösa tillstånd (en auktorisering per virtuellt nätverk).</span><span class="sxs-lookup"><span data-stu-id="92af4-145">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="92af4-146">Hej kretsägaren har hello power toomodify samt återkalla tillstånd när som helst.</span><span class="sxs-lookup"><span data-stu-id="92af4-146">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="92af4-147">Återkalla ett tillstånd som resulterar i alla anslutningar tas bort från hello prenumeration vars åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="92af4-147">Revoking an authorization results in all link connections being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="92af4-148">Kretsen ägare åtgärder</span><span class="sxs-lookup"><span data-stu-id="92af4-148">Circuit owner operations</span></span>

<span data-ttu-id="92af4-149">**toocreate tillstånd**</span><span class="sxs-lookup"><span data-stu-id="92af4-149">**toocreate an authorization**</span></span>

<span data-ttu-id="92af4-150">Hej kretsägaren skapar tillstånd.</span><span class="sxs-lookup"><span data-stu-id="92af4-150">hello circuit owner creates an authorization.</span></span> <span data-ttu-id="92af4-151">Detta resulterar i hello skapandet av auktoriseringsnyckel som kan användas av en krets användaren tooconnect sina virtuella nätverk gateways toohello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="92af4-151">This results in hello creation of an authorization key that can be used by a circuit user tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="92af4-152">Ett tillstånd som är giltig för endast en anslutning.</span><span class="sxs-lookup"><span data-stu-id="92af4-152">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="92af4-153">följande cmdlet fragment visas hur hello toocreate tillstånd:</span><span class="sxs-lookup"><span data-stu-id="92af4-153">hello following cmdlet snippet shows how toocreate an authorization:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


<span data-ttu-id="92af4-154">hello svar toothis innehåller hello auktoriseringsnyckeln och status:</span><span class="sxs-lookup"><span data-stu-id="92af4-154">hello response toothis will contain hello authorization key and status:</span></span>

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



<span data-ttu-id="92af4-155">**tooreview tillstånd**</span><span class="sxs-lookup"><span data-stu-id="92af4-155">**tooreview authorizations**</span></span>

<span data-ttu-id="92af4-156">Hej kretsägaren kan granska alla tillstånd som utfärdats för en viss krets genom att köra följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="92af4-156">hello circuit owner can review all authorizations that are issued on a particular circuit by running hello following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="92af4-157">**tooadd tillstånd**</span><span class="sxs-lookup"><span data-stu-id="92af4-157">**tooadd authorizations**</span></span>

<span data-ttu-id="92af4-158">Hej kretsägaren kan lägga till tillstånd med hjälp av hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="92af4-158">hello circuit owner can add authorizations by using hello following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="92af4-159">**toodelete tillstånd**</span><span class="sxs-lookup"><span data-stu-id="92af4-159">**toodelete authorizations**</span></span>

<span data-ttu-id="92af4-160">Hej kretsägaren kan återkalla/ta bort tillstånd toohello användare genom att köra följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="92af4-160">hello circuit owner can revoke/delete authorizations toohello user by running hello following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a><span data-ttu-id="92af4-161">Kretsen användaren åtgärder</span><span class="sxs-lookup"><span data-stu-id="92af4-161">Circuit user operations</span></span>

<span data-ttu-id="92af4-162">Hej kretsanvändare måste hello peer-ID och auktoriseringsnyckel från hello kretsägaren.</span><span class="sxs-lookup"><span data-stu-id="92af4-162">hello circuit user needs hello peer ID and an authorization key from hello circuit owner.</span></span> <span data-ttu-id="92af4-163">Hej auktoriseringsnyckeln är ett GUID.</span><span class="sxs-lookup"><span data-stu-id="92af4-163">hello authorization key is a GUID.</span></span>

<span data-ttu-id="92af4-164">Peer-ID kan kontrolleras från hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="92af4-164">Peer ID can be checked from hello following command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="92af4-165">**tooredeem en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="92af4-165">**tooredeem a connection authorization**</span></span>

<span data-ttu-id="92af4-166">hello kretsanvändare kan köra följande cmdlet tooredeem hello ett länk-tillstånd:</span><span class="sxs-lookup"><span data-stu-id="92af4-166">hello circuit user can run hello following cmdlet tooredeem a link authorization:</span></span>

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="92af4-167">**toorelease en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="92af4-167">**toorelease a connection authorization**</span></span>

<span data-ttu-id="92af4-168">Du kan släppa tillstånd genom att ta bort hello-anslutning som länkar hello ExpressRoute-kretsen toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="92af4-168">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="modify-a-virtual-network-connection"></a><span data-ttu-id="92af4-169">Ändra en virtuell nätverksanslutning</span><span class="sxs-lookup"><span data-stu-id="92af4-169">Modify a virtual network connection</span></span>
<span data-ttu-id="92af4-170">Du kan uppdatera vissa egenskaper för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="92af4-170">You can update certain properties of a virtual network connection.</span></span> 

<span data-ttu-id="92af4-171">**tooupdate hello anslutning vikt**</span><span class="sxs-lookup"><span data-stu-id="92af4-171">**tooupdate hello connection weight**</span></span>

<span data-ttu-id="92af4-172">Det virtuella nätverket kan vara anslutna toomultiple ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="92af4-172">Your virtual network can be connected toomultiple ExpressRoute circuits.</span></span> <span data-ttu-id="92af4-173">Hello samma prefix får du från mer än en ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="92af4-173">You may receive hello same prefix from more than one ExpressRoute circuit.</span></span> <span data-ttu-id="92af4-174">toochoose vilken anslutning toosend trafik avsedda för det här prefixet, du kan ändra *RoutingWeight* för en anslutning.</span><span class="sxs-lookup"><span data-stu-id="92af4-174">toochoose which connection toosend traffic destined for this prefix, you can change *RoutingWeight* of a connection.</span></span> <span data-ttu-id="92af4-175">Trafik skickas på hello anslutning med hello högsta *RoutingWeight*.</span><span class="sxs-lookup"><span data-stu-id="92af4-175">Traffic will be sent on hello connection with hello highest *RoutingWeight*.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

<span data-ttu-id="92af4-176">Hej mängd *RoutingWeight* är 0 too32000.</span><span class="sxs-lookup"><span data-stu-id="92af4-176">hello range of *RoutingWeight* is 0 too32000.</span></span> <span data-ttu-id="92af4-177">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92af4-177">hello default value is 0.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92af4-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92af4-178">Next steps</span></span>
<span data-ttu-id="92af4-179">Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="92af4-179">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
