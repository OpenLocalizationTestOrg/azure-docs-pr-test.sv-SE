---
title: "Länka ett virtuellt nätverk till en ExpressRoute-krets: PowerShell: Azure | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur du länkar virtuella nätverk (Vnet) för ExpressRoute-kretsar med Resource Manager-distributionsmodellen och PowerShell."
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
ms.openlocfilehash: 8c2f3036f754a98090ab860f95900416690ebf83
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="f7cc5-103">Ansluta ett virtuellt nätverk till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="f7cc5-103">Connect a virtual network to an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7cc5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f7cc5-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="f7cc5-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7cc5-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="f7cc5-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f7cc5-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="f7cc5-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f7cc5-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="f7cc5-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="f7cc5-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="f7cc5-109">Den här artikeln hjälper dig att länka virtuella nätverk (Vnet) Azure ExpressRoute-kretsar med hjälp av Resource Manager-distributionsmodellen och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-109">This article helps you link virtual networks (VNets) to Azure ExpressRoute circuits by using the Resource Manager deployment model and PowerShell.</span></span> <span data-ttu-id="f7cc5-110">Virtuella nätverk kan antingen vara i samma prenumeration eller en del av en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-110">Virtual networks can either be in the same subscription or part of another subscription.</span></span> <span data-ttu-id="f7cc5-111">Den här artikeln beskriver också hur du uppdaterar en länk för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-111">This article also shows you how to update a virtual network link.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="f7cc5-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="f7cc5-112">Before you begin</span></span>
* <span data-ttu-id="f7cc5-113">Installera den senaste versionen av Azure PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-113">Install the latest version of the Azure PowerShell modules.</span></span> <span data-ttu-id="f7cc5-114">Mer information finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f7cc5-114">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="f7cc5-115">Granska de [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-115">Review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="f7cc5-116">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-116">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="f7cc5-117">Följ instruktionerna för att [skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha kretsen aktiveras med anslutningsleverantören.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-117">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="f7cc5-118">Se till att du har privat Azure-peering konfigurerats för kretsen.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-118">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="f7cc5-119">Finns det [konfigurera routning](expressroute-howto-routing-arm.md) artikel routning anvisningar.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-119">See the [configure routing](expressroute-howto-routing-arm.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="f7cc5-120">Kontrollera att privat Azure-peering har konfigurerats och BGP-peering mellan ditt nätverk och Microsoft är igång så att du kan aktivera anslutning för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-120">Ensure that Azure private peering is configured and the BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="f7cc5-121">Se till att du har ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-121">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="f7cc5-122">Följ instruktionerna för att [skapa en virtuell nätverksgateway för ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="f7cc5-122">Follow the instructions to [create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="f7cc5-123">En virtuell nätverksgateway expressroute använder GatewayType ExpressRoute, inte VPN.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-123">A virtual network gateway for ExpressRoute uses the GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="f7cc5-124">Du kan länka upp till 10 virtuella nätverk till en standard ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-124">You can link up to 10 virtual networks to a standard ExpressRoute circuit.</span></span> <span data-ttu-id="f7cc5-125">Alla virtuella nätverk måste vara i samma region geopolitiska när du använder en standard ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-125">All virtual networks must be in the same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="f7cc5-126">Du kan länka en virtuella nätverk utanför geopolitiska region för ExpressRoute-kretsen eller ansluta ett stort antal virtuella nätverk till ExpressRoute-krets om du har aktiverat ExpressRoute premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-126">You can link a virtual networks outside of the geopolitical region of the ExpressRoute circuit, or connect a larger number of virtual networks to your ExpressRoute circuit if you enabled the ExpressRoute premium add-on.</span></span> <span data-ttu-id="f7cc5-127">Kontrollera den [vanliga frågor och svar](expressroute-faqs.md) för mer information om premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-127">Check the [FAQ](expressroute-faqs.md) for more details on the premium add-on.</span></span>


## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="f7cc5-128">Ansluta ett virtuellt nätverk med samma prenumeration till en krets</span><span class="sxs-lookup"><span data-stu-id="f7cc5-128">Connect a virtual network in the same subscription to a circuit</span></span>
<span data-ttu-id="f7cc5-129">Du kan ansluta en virtuell nätverksgateway till en ExpressRoute-krets med hjälp av följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-129">You can connect a virtual network gateway to an ExpressRoute circuit by using the following cmdlet.</span></span> <span data-ttu-id="f7cc5-130">Kontrollera att den virtuella nätverksgatewayen har skapats och är redo för att länka innan du kör cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f7cc5-130">Make sure that the virtual network gateway is created and is ready for linking before you run the cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="f7cc5-131">Anslut ett virtuellt nätverk i en annan prenumeration till en krets</span><span class="sxs-lookup"><span data-stu-id="f7cc5-131">Connect a virtual network in a different subscription to a circuit</span></span>
<span data-ttu-id="f7cc5-132">Du kan dela en ExpressRoute-krets över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="f7cc5-133">Följande bild visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-133">The following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="f7cc5-134">Var och en av mindre molnen i stora molnet används för att representera prenumerationer som hör till olika avdelningar inom en organisation.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-134">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span> <span data-ttu-id="f7cc5-135">Varje avdelning inom organisationen kan använda sina egna prenumeration för att distribuera sina tjänster, men de kan dela en enda ExpressRoute-krets att ansluta till ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-135">Each of the departments within the organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span> <span data-ttu-id="f7cc5-136">En avdelning (i det här exemplet: IT) kan äga ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-136">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="f7cc5-137">Andra prenumerationer inom organisationen kan använda ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-137">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="f7cc5-138">Prenumerationsägaren kommer att gälla anslutningar och bandbredd avgifter för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-138">Connectivity and bandwidth charges for the ExpressRoute circuit will be applied to the subscription owner.</span></span> <span data-ttu-id="f7cc5-139">Alla virtuella nätverk dela på samma bandbredd.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-139">All virtual networks share the same bandwidth.</span></span>
> 
> 

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="f7cc5-141">Administration - krets ägare och krets användare</span><span class="sxs-lookup"><span data-stu-id="f7cc5-141">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="f7cc5-142">Kretsägaren är en auktoriserad Power användare av resursen ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-142">The 'circuit owner' is an authorized Power User of the ExpressRoute circuit resource.</span></span> <span data-ttu-id="f7cc5-143">Kretsägaren kan skapa tillstånd som kan lösas av krets-användare.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-143">The circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="f7cc5-144">Kretsen användare är ägare till virtuella nätverks-gateway som inte är inom samma prenumeration som ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-144">Circuit users are owners of virtual network gateways that are not within the same subscription as the ExpressRoute circuit.</span></span> <span data-ttu-id="f7cc5-145">Kretsen användare kan lösa tillstånd (en auktorisering per virtuellt nätverk).</span><span class="sxs-lookup"><span data-stu-id="f7cc5-145">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="f7cc5-146">Kretsägaren har behörighet att ändra och återkalla tillstånd när som helst.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-146">The circuit owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="f7cc5-147">Återkalla ett tillstånd som resulterar i alla anslutningar tas bort från prenumerationen vars åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-147">Revoking an authorization results in all link connections being deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="f7cc5-148">Kretsen ägare åtgärder</span><span class="sxs-lookup"><span data-stu-id="f7cc5-148">Circuit owner operations</span></span>

<span data-ttu-id="f7cc5-149">**Så här skapar du ett tillstånd**</span><span class="sxs-lookup"><span data-stu-id="f7cc5-149">**To create an authorization**</span></span>

<span data-ttu-id="f7cc5-150">Kretsägaren skapar tillstånd.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-150">The circuit owner creates an authorization.</span></span> <span data-ttu-id="f7cc5-151">Detta resulterar i att skapa auktoriseringsnyckel som kan användas av en kretsanvändare för att ansluta sina virtuella nätverksgatewayerna till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-151">This results in the creation of an authorization key that can be used by a circuit user to connect their virtual network gateways to the ExpressRoute circuit.</span></span> <span data-ttu-id="f7cc5-152">Ett tillstånd som är giltig för endast en anslutning.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-152">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="f7cc5-153">Följande cmdlet utdrag visar hur du skapar ett tillstånd:</span><span class="sxs-lookup"><span data-stu-id="f7cc5-153">The following cmdlet snippet shows how to create an authorization:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


<span data-ttu-id="f7cc5-154">Auktoriseringsnyckeln och status innehåller svar på detta:</span><span class="sxs-lookup"><span data-stu-id="f7cc5-154">The response to this will contain the authorization key and status:</span></span>

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



<span data-ttu-id="f7cc5-155">**Granska tillstånd**</span><span class="sxs-lookup"><span data-stu-id="f7cc5-155">**To review authorizations**</span></span>

<span data-ttu-id="f7cc5-156">Kretsägaren kan granska alla tillstånd som utfärdats för en viss krets genom att köra följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f7cc5-156">The circuit owner can review all authorizations that are issued on a particular circuit by running the following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="f7cc5-157">**Att lägga till tillstånd**</span><span class="sxs-lookup"><span data-stu-id="f7cc5-157">**To add authorizations**</span></span>

<span data-ttu-id="f7cc5-158">Kretsägaren kan lägga till tillstånd genom att använda följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f7cc5-158">The circuit owner can add authorizations by using the following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="f7cc5-159">**Ta bort tillstånd**</span><span class="sxs-lookup"><span data-stu-id="f7cc5-159">**To delete authorizations**</span></span>

<span data-ttu-id="f7cc5-160">Kretsägaren kan återkalla/ta bort tillstånd till användaren genom att köra följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f7cc5-160">The circuit owner can revoke/delete authorizations to the user by running the following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a><span data-ttu-id="f7cc5-161">Kretsen användaren åtgärder</span><span class="sxs-lookup"><span data-stu-id="f7cc5-161">Circuit user operations</span></span>

<span data-ttu-id="f7cc5-162">Kretsen användaren måste peer-ID och auktoriseringsnyckel från kretsägaren.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-162">The circuit user needs the peer ID and an authorization key from the circuit owner.</span></span> <span data-ttu-id="f7cc5-163">Auktoriseringsnyckeln är ett GUID.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-163">The authorization key is a GUID.</span></span>

<span data-ttu-id="f7cc5-164">Peer-ID kan kontrolleras från följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f7cc5-164">Peer ID can be checked from the following command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="f7cc5-165">**Lösa in en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="f7cc5-165">**To redeem a connection authorization**</span></span>

<span data-ttu-id="f7cc5-166">Kretsanvändare kan köra följande cmdlet för att lösa in en länk-tillstånd:</span><span class="sxs-lookup"><span data-stu-id="f7cc5-166">The circuit user can run the following cmdlet to redeem a link authorization:</span></span>

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="f7cc5-167">**Att släppa en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="f7cc5-167">**To release a connection authorization**</span></span>

<span data-ttu-id="f7cc5-168">Du kan släppa tillstånd genom att ta bort anslutningen som länkar ExpressRoute-kretsen till det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-168">You can release an authorization by deleting the connection that links the ExpressRoute circuit to the virtual network.</span></span>

## <a name="modify-a-virtual-network-connection"></a><span data-ttu-id="f7cc5-169">Ändra en virtuell nätverksanslutning</span><span class="sxs-lookup"><span data-stu-id="f7cc5-169">Modify a virtual network connection</span></span>
<span data-ttu-id="f7cc5-170">Du kan uppdatera vissa egenskaper för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-170">You can update certain properties of a virtual network connection.</span></span> 

<span data-ttu-id="f7cc5-171">**Uppdatera anslutning vikt**</span><span class="sxs-lookup"><span data-stu-id="f7cc5-171">**To update the connection weight**</span></span>

<span data-ttu-id="f7cc5-172">Det virtuella nätverket kan vara ansluten till flera ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-172">Your virtual network can be connected to multiple ExpressRoute circuits.</span></span> <span data-ttu-id="f7cc5-173">Du kan få samma prefix från mer än en ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-173">You may receive the same prefix from more than one ExpressRoute circuit.</span></span> <span data-ttu-id="f7cc5-174">Du kan ändra för att välja vilken anslutning du vill skicka trafik till det här prefixet *RoutingWeight* för en anslutning.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-174">To choose which connection to send traffic destined for this prefix, you can change *RoutingWeight* of a connection.</span></span> <span data-ttu-id="f7cc5-175">Trafik skickas på anslutning med den högsta *RoutingWeight*.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-175">Traffic will be sent on the connection with the highest *RoutingWeight*.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

<span data-ttu-id="f7cc5-176">Intervallet för *RoutingWeight* är 0 till 32000.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-176">The range of *RoutingWeight* is 0 to 32000.</span></span> <span data-ttu-id="f7cc5-177">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="f7cc5-177">The default value is 0.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7cc5-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f7cc5-178">Next steps</span></span>
<span data-ttu-id="f7cc5-179">Mer information om ExpressRoute finns i [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="f7cc5-179">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
