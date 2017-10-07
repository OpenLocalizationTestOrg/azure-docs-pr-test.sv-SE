---
title: "Länka ett virtuellt nätverk tooan ExpressRoute-krets: CLI: Azure | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur toolink virtuella nätverk (Vnet) tooExpressRoute kretsar med hjälp av hello Resource Manager-modellen och CLI."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlit
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: 1251f016d9b94d3fee81de1df164cb085cbe9d78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-cli"></a><span data-ttu-id="3c678-103">Ansluta ett virtuellt nätverk tooan ExpressRoute-kretsen med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="3c678-103">Connect a virtual network tooan ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="3c678-104">Den här artikeln hjälper dig att länka virtuella nätverk (Vnet) tooAzure ExpressRoute-kretsar med hjälp av CLI.</span><span class="sxs-lookup"><span data-stu-id="3c678-104">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits using CLI.</span></span> <span data-ttu-id="3c678-105">toolink med hjälp av Azure CLI hello virtuella nätverk måste skapas med hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="3c678-105">toolink using Azure CLI, hello virtual networks must be created using hello Resource Manager deployment model.</span></span> <span data-ttu-id="3c678-106">De kan antingen vara i hello samma prenumeration, eller en del av en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3c678-106">They can either be in hello same subscription, or part of another subscription.</span></span> <span data-ttu-id="3c678-107">Om du vill toouse en annan metod tooconnect ditt VNet tooan ExpressRoute-krets, kan du välja en artikel hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="3c678-107">If you want toouse a different method tooconnect your VNet tooan ExpressRoute circuit, you can select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c678-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3c678-108">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="3c678-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c678-109">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="3c678-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3c678-110">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="3c678-111">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3c678-111">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="3c678-112">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="3c678-112">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="3c678-113">Förutsättningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="3c678-113">Configuration prerequisites</span></span>

* <span data-ttu-id="3c678-114">Du måste hello senaste versionen av hello-kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="3c678-114">You need hello latest version of hello command-line interface (CLI).</span></span> <span data-ttu-id="3c678-115">Mer information finns i [installera Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3c678-115">For more information, see [Install Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="3c678-116">Du behöver tooreview hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="3c678-116">You need tooreview hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="3c678-117">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="3c678-117">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="3c678-118">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](howto-circuit-cli.md) och ha hello krets aktiveras med anslutningsleverantören.</span><span class="sxs-lookup"><span data-stu-id="3c678-118">Follow hello instructions too[create an ExpressRoute circuit](howto-circuit-cli.md) and have hello circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="3c678-119">Se till att du har privat Azure-peering konfigurerats för kretsen.</span><span class="sxs-lookup"><span data-stu-id="3c678-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="3c678-120">Se hello [konfigurera routning](howto-routing-cli.md) artikel routning anvisningar.</span><span class="sxs-lookup"><span data-stu-id="3c678-120">See hello [configure routing](howto-routing-cli.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="3c678-121">Kontrollera att Azure privat peering är konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="3c678-121">Ensure that Azure private peering is configured.</span></span> <span data-ttu-id="3c678-122">hello BGP-peering mellan ditt nätverk och Microsoft måste vara in så att du kan aktivera anslutning för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="3c678-122">hello BGP peering between your network and Microsoft must be up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="3c678-123">Se till att du har ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="3c678-123">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="3c678-124">Följ instruktionerna för hello för[konfigurera en virtuell nätverksgateway för ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="3c678-124">Follow hello instructions too[Configure a virtual network gateway for ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span> <span data-ttu-id="3c678-125">Vara säker på att toouse `--gateway-type ExpressRoute`.</span><span class="sxs-lookup"><span data-stu-id="3c678-125">Be sure toouse `--gateway-type ExpressRoute`.</span></span>

* <span data-ttu-id="3c678-126">Du kan länka upp too10-virtuella nätverk tooa standard ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="3c678-126">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="3c678-127">Alla virtuella nätverk måste vara i hello samma geopolitiska region när du använder en standard ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="3c678-127">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="3c678-128">Om du aktiverar hello ExpressRoute premium-tillägg kan du länka ett virtuellt nätverk utanför hello geopolitiska regionens hello ExpressRoute-krets eller ansluta ett stort antal virtuella nätverk tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="3c678-128">If you enable hello ExpressRoute premium add-on, you can link a virtual network outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="3c678-129">Mer information om hello premium-tillägg finns hello [vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="3c678-129">For more information about hello premium add-on, see hello [FAQ](expressroute-faqs.md).</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="3c678-130">Ansluta ett virtuellt nätverk i hello samma prenumeration tooa krets</span><span class="sxs-lookup"><span data-stu-id="3c678-130">Connect a virtual network in hello same subscription tooa circuit</span></span>

<span data-ttu-id="3c678-131">Du kan ansluta ett virtuellt nätverk gateway tooan ExpressRoute-kretsen med hjälp av hello exempel.</span><span class="sxs-lookup"><span data-stu-id="3c678-131">You can connect a virtual network gateway tooan ExpressRoute circuit by using hello example.</span></span> <span data-ttu-id="3c678-132">Kontrollera att den virtuella nätverksgatewayen hello har skapats och är redo för att länka innan du kör kommandot hello.</span><span class="sxs-lookup"><span data-stu-id="3c678-132">Make sure that hello virtual network gateway is created and is ready for linking before you run hello command.</span></span>

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="3c678-133">Ansluta ett virtuellt nätverk i en annan prenumeration tooa-krets</span><span class="sxs-lookup"><span data-stu-id="3c678-133">Connect a virtual network in a different subscription tooa circuit</span></span>

<span data-ttu-id="3c678-134">Du kan dela en ExpressRoute-krets över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="3c678-134">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="3c678-135">hello bilden nedan visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="3c678-135">hello figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="3c678-136">Varje hello mindre moln inom hello stora moln är används toorepresent prenumerationer som tillhör toodifferent avdelningar inom en organisation.</span><span class="sxs-lookup"><span data-stu-id="3c678-136">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="3c678-137">Var och en av hello avdelningar inom hello organisation kan använda sina egna prenumeration för att distribuera sina tjänster, men de kan dela ett ExpressRoute-kretsen tooconnect tillbaka tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="3c678-137">Each of hello departments within hello organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="3c678-138">En avdelning (i det här exemplet: IT) kan äga hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="3c678-138">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="3c678-139">Andra prenumerationer i hello organisation kan använda hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="3c678-139">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="3c678-140">Anslutningar och bandbredd kostnader för dedikerade hello krets kommer att tillämpas toohello ExpressRoute-kretsen ägare.</span><span class="sxs-lookup"><span data-stu-id="3c678-140">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute Circuit Owner.</span></span> <span data-ttu-id="3c678-141">Alla virtuella nätverk dela hello samma bandbredd.</span><span class="sxs-lookup"><span data-stu-id="3c678-141">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="3c678-143">Administration - krets ägare och krets användare</span><span class="sxs-lookup"><span data-stu-id="3c678-143">Administration - Circuit Owners and Circuit Users</span></span>

<span data-ttu-id="3c678-144">hello 'Kretsägaren' är en behörig Privilegierade användare i hello ExpressRoute-kretsen resurs.</span><span class="sxs-lookup"><span data-stu-id="3c678-144">hello 'Circuit Owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="3c678-145">Hej Kretsägaren kan skapa tillstånd som kan lösas av krets-användare.</span><span class="sxs-lookup"><span data-stu-id="3c678-145">hello Circuit Owner can create authorizations that can be redeemed by 'Circuit Users'.</span></span> <span data-ttu-id="3c678-146">Kretsen användare är ägare till virtuella nätverk gateways som inte är inom hello samma prenumeration som hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="3c678-146">Circuit Users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="3c678-147">Kretsen användare kan lösa tillstånd (en auktorisering per virtuellt nätverk).</span><span class="sxs-lookup"><span data-stu-id="3c678-147">Circuit Users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="3c678-148">Hej Kretsägaren har hello power toomodify samt återkalla tillstånd när som helst.</span><span class="sxs-lookup"><span data-stu-id="3c678-148">hello Circuit Owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="3c678-149">När ett tillstånd återkallas tas alla anslutningar bort från hello prenumeration vars åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="3c678-149">When an authorization is revoked, all link connections are deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="3c678-150">Kretsen ägare åtgärder</span><span class="sxs-lookup"><span data-stu-id="3c678-150">Circuit Owner operations</span></span>

<span data-ttu-id="3c678-151">**toocreate tillstånd**</span><span class="sxs-lookup"><span data-stu-id="3c678-151">**toocreate an authorization**</span></span>

<span data-ttu-id="3c678-152">Hej Kretsägaren skapar tillstånd, vilket skapar auktoriseringsnyckel som kan användas av en Kretsanvändare tooconnect sina virtuella nätverk gateways toohello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="3c678-152">hello Circuit Owner creates an authorization, which creates an authorization key that can be used by a Circuit User tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="3c678-153">Ett tillstånd som är giltig för endast en anslutning.</span><span class="sxs-lookup"><span data-stu-id="3c678-153">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="3c678-154">följande exempel visar hur hello toocreate tillstånd:</span><span class="sxs-lookup"><span data-stu-id="3c678-154">hello following example shows how toocreate an authorization:</span></span>

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

<span data-ttu-id="3c678-155">hello svaret innehåller hello auktoriseringsnyckeln och status:</span><span class="sxs-lookup"><span data-stu-id="3c678-155">hello response contains hello authorization key and status:</span></span>

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

<span data-ttu-id="3c678-156">**tooreview tillstånd**</span><span class="sxs-lookup"><span data-stu-id="3c678-156">**tooreview authorizations**</span></span>

<span data-ttu-id="3c678-157">Hej Kretsägaren kan granska alla tillstånd som utfärdats för en viss krets genom att köra hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3c678-157">hello Circuit Owner can review all authorizations that are issued on a particular circuit by running hello following example:</span></span>

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

<span data-ttu-id="3c678-158">**tooadd tillstånd**</span><span class="sxs-lookup"><span data-stu-id="3c678-158">**tooadd authorizations**</span></span>

<span data-ttu-id="3c678-159">Hej Kretsägaren kan lägga till tillstånd med hjälp av hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3c678-159">hello Circuit Owner can add authorizations by using hello following example:</span></span>

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

<span data-ttu-id="3c678-160">**toodelete tillstånd**</span><span class="sxs-lookup"><span data-stu-id="3c678-160">**toodelete authorizations**</span></span>

<span data-ttu-id="3c678-161">Hej Kretsägaren kan återkalla/ta bort tillstånd toohello användare genom att köra hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3c678-161">hello Circuit Owner can revoke/delete authorizations toohello user by running hello following example:</span></span>

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a><span data-ttu-id="3c678-162">Kretsen användare</span><span class="sxs-lookup"><span data-stu-id="3c678-162">Circuit User operations</span></span>

<span data-ttu-id="3c678-163">Hej Kretsanvändare måste hello peer-ID och auktoriseringsnyckel från hello Kretsägaren.</span><span class="sxs-lookup"><span data-stu-id="3c678-163">hello Circuit User needs hello peer ID and an authorization key from hello Circuit Owner.</span></span> <span data-ttu-id="3c678-164">Hej auktoriseringsnyckeln är ett GUID.</span><span class="sxs-lookup"><span data-stu-id="3c678-164">hello authorization key is a GUID.</span></span>

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="3c678-165">**tooredeem en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="3c678-165">**tooredeem a connection authorization**</span></span>

<span data-ttu-id="3c678-166">Hej Kretsanvändare kan köra hello följande exempel tooredeem ett länk-tillstånd:</span><span class="sxs-lookup"><span data-stu-id="3c678-166">hello Circuit User can run hello following example tooredeem a link authorization:</span></span>

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="3c678-167">**toorelease en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="3c678-167">**toorelease a connection authorization**</span></span>

<span data-ttu-id="3c678-168">Du kan släppa tillstånd genom att ta bort hello-anslutning som länkar hello ExpressRoute-kretsen toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="3c678-168">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c678-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c678-169">Next steps</span></span>

<span data-ttu-id="3c678-170">Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="3c678-170">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
