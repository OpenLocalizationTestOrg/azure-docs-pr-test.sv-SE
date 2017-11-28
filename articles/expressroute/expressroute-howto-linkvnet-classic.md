---
title: "Länka ett virtuellt nätverk tooan ExpressRoute-krets: PowerShell: klassiska: Azure | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur toolink virtuella nätverk (Vnet) tooExpressRoute kretsar med hjälp av hello klassiska distributionsmodellen och PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: ganesr
ms.openlocfilehash: 6b8a01dcd4bbb9618ec3dd438cf0107538fb2a7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="ab7d3-103">Ansluta ett virtuellt nätverk tooan ExpressRoute-kretsen med hjälp av PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="ab7d3-103">Connect a virtual network tooan ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ab7d3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ab7d3-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="ab7d3-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ab7d3-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="ab7d3-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ab7d3-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="ab7d3-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ab7d3-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="ab7d3-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="ab7d3-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="ab7d3-109">Den här artikeln beskriver hur du länka virtuella nätverk (Vnet) tooAzure ExpressRoute-kretsar med hello klassiska distributionsmodellen och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-109">This article will help you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello classic deployment model and PowerShell.</span></span> <span data-ttu-id="ab7d3-110">Virtuella nätverk kan antingen vara i samma prenumeration hello eller kan vara en del av en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-110">Virtual networks can either be in hello same subscription or can be part of another subscription.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="ab7d3-111">**Om distributionsmodeller för Azure**</span><span class="sxs-lookup"><span data-stu-id="ab7d3-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a><span data-ttu-id="ab7d3-112">Förutsättningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="ab7d3-112">Configuration prerequisites</span></span>
1. <span data-ttu-id="ab7d3-113">Du måste hello senaste versionen av hello Azure PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-113">You need hello latest version of hello Azure PowerShell modules.</span></span> <span data-ttu-id="ab7d3-114">Du kan hämta hello senaste PowerShell-moduler från hello PowerShell avsnitt i hello [Azure hämtar sidan](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ab7d3-114">You can download hello latest PowerShell modules from hello PowerShell section of hello [Azure Downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="ab7d3-115">Följ anvisningarna för hello i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) stegvisa instruktioner om hur tooconfigure din dator toouse hello Azure PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-115">Follow hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for step-by-step guidance on how tooconfigure your computer toouse hello Azure PowerShell modules.</span></span>
2. <span data-ttu-id="ab7d3-116">Du behöver tooreview hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-116">You need tooreview hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
3. <span data-ttu-id="ab7d3-117">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-117">You must have an active ExpressRoute circuit.</span></span>
   * <span data-ttu-id="ab7d3-118">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-classic.md) och har anslutningsleverantören aktivera hello krets.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-118">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have your connectivity provider enable hello circuit.</span></span>
   * <span data-ttu-id="ab7d3-119">Se till att du har privat Azure-peering konfigurerats för kretsen.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="ab7d3-120">Se hello [konfigurera routning](expressroute-howto-routing-classic.md) artikel routning anvisningar.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-120">See hello [Configure routing](expressroute-howto-routing-classic.md) article for routing instructions.</span></span>
   * <span data-ttu-id="ab7d3-121">Kontrollera att privat Azure-peering har konfigurerats och hello BGP-peering mellan ditt nätverk och Microsoft är igång så att du kan aktivera anslutning för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-121">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
   * <span data-ttu-id="ab7d3-122">Du måste ha ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-122">You must have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="ab7d3-123">Följ instruktionerna för hello för[konfigurera ett virtuellt nätverk för ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="ab7d3-123">Follow hello instructions too[configure a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

<span data-ttu-id="ab7d3-124">Du kan länka upp too10 virtuella nätverk tooan ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-124">You can link up too10 virtual networks tooan ExpressRoute circuit.</span></span> <span data-ttu-id="ab7d3-125">Alla virtuella nätverk måste vara i hello samma geopolitiska region.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-125">All virtual networks must be in hello same geopolitical region.</span></span> <span data-ttu-id="ab7d3-126">Du kan länka ett stort antal virtuella nätverk tooyour ExpressRoute-krets eller länka virtuella nätverk som ingår i andra geopolitiska regioner om du har aktiverat hello ExpressRoute premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-126">You can link a larger number of virtual networks tooyour ExpressRoute circuit, or link virtual networks that are in other geopolitical regions if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="ab7d3-127">Kontrollera hello [vanliga frågor och svar](expressroute-faqs.md) för mer information om hello premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-127">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="ab7d3-128">Ansluta ett virtuellt nätverk i hello samma prenumeration tooa krets</span><span class="sxs-lookup"><span data-stu-id="ab7d3-128">Connect a virtual network in hello same subscription tooa circuit</span></span>
<span data-ttu-id="ab7d3-129">Du kan länka ett virtuellt nätverk tooan ExpressRoute-kretsen med hjälp av hello följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-129">You can link a virtual network tooan ExpressRoute circuit by using hello following cmdlet.</span></span> <span data-ttu-id="ab7d3-130">Kontrollera att den virtuella nätverksgatewayen hello har skapats och är redo för att länka innan du kör cmdlet hello.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-130">Make sure that hello virtual network gateway is created and is ready for linking before you run hello cmdlet.</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="ab7d3-131">Ansluta ett virtuellt nätverk i en annan prenumeration tooa-krets</span><span class="sxs-lookup"><span data-stu-id="ab7d3-131">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="ab7d3-132">Du kan dela en ExpressRoute-krets över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="ab7d3-133">hello följande bild visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-133">hello following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="ab7d3-134">Varje hello mindre moln inom hello stora moln är används toorepresent prenumerationer som tillhör toodifferent avdelningar inom en organisation.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-134">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="ab7d3-135">Var och en av hello avdelningar inom hello organisation kan använda sina egna prenumeration för att distribuera sina tjänster, men hello avdelningar kan dela ett ExpressRoute-kretsen tooconnect tillbaka tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-135">Each of hello departments within hello organization can use their own subscription for deploying their services--but hello departments can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="ab7d3-136">En avdelning (i det här exemplet: IT) kan äga hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-136">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="ab7d3-137">Andra prenumerationer i hello organisation kan använda hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-137">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="ab7d3-138">Anslutningar och bandbredd kostnader för dedikerade hello krets kommer att tillämpas toohello ExpressRoute-kretsen ägare.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-138">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute circuit owner.</span></span> <span data-ttu-id="ab7d3-139">Alla virtuella nätverk dela hello samma bandbredd.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-139">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a><span data-ttu-id="ab7d3-141">Administration</span><span class="sxs-lookup"><span data-stu-id="ab7d3-141">Administration</span></span>
<span data-ttu-id="ab7d3-142">Hej *kretsägaren* är Hej administratör/coadministrator hello prenumeration i vilka hello ExpressRoute-kretsen har skapats.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-142">hello *circuit owner* is hello administrator/coadministrator of hello subscription in which hello ExpressRoute circuit is created.</span></span> <span data-ttu-id="ab7d3-143">Hej kretsägaren kan tillåta administratörer/coadministrators av andra prenumerationer, enligt tooas *krets användare*, toouse hello dedikerad anslutning som de äger.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-143">hello circuit owner can authorize administrators/coadministrators of other subscriptions, referred tooas *circuit users*, toouse hello dedicated circuit that they own.</span></span> <span data-ttu-id="ab7d3-144">Kretsen användare som är auktoriserade toouse hello organisation ExpressRoute-kretsen kan länka hello virtuellt nätverk i deras prenumeration toohello ExpressRoute-krets när de har behörighet.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-144">Circuit users who are authorized toouse hello organization's ExpressRoute circuit can link hello virtual network in their subscription toohello ExpressRoute circuit after they are authorized.</span></span>

<span data-ttu-id="ab7d3-145">Hej kretsägaren har hello power toomodify samt återkalla tillstånd när som helst.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-145">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="ab7d3-146">Återkalla ett tillstånd som leder till att alla länkar tas bort från hello prenumeration vars åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-146">Revoking an authorization will result in all links being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="ab7d3-147">Kretsen ägare åtgärder</span><span class="sxs-lookup"><span data-stu-id="ab7d3-147">Circuit owner operations</span></span>

<span data-ttu-id="ab7d3-148">**Skapa ett tillstånd**</span><span class="sxs-lookup"><span data-stu-id="ab7d3-148">**Creating an authorization**</span></span>

<span data-ttu-id="ab7d3-149">Hej kretsägaren auktoriserar hello administratörer av andra prenumerationer toouse hello angivna kretsen.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-149">hello circuit owner authorizes hello administrators of other subscriptions toouse hello specified circuit.</span></span> <span data-ttu-id="ab7d3-150">I följande exempel hello, aktiverar Hej administratör av hello krets (Contoso IT) Hej administratör av en annan prenumeration (Dev-Test) toolink in tootwo virtuella nätverk toohello krets.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-150">In hello following example, hello administrator of hello circuit (Contoso IT) enables hello administrator of another subscription (Dev-Test) toolink up tootwo virtual networks toohello circuit.</span></span> <span data-ttu-id="ab7d3-151">hello Contoso IT-administratören gör detta genom att ange hello Dev-Test Microsoft-ID.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-151">hello Contoso IT administrator enables this by specifying hello Dev-Test Microsoft ID.</span></span> <span data-ttu-id="ab7d3-152">hello cmdlet Skicka inte e-toohello angivna Microsoft-ID.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-152">hello cmdlet doesn't send email toohello specified Microsoft ID.</span></span> <span data-ttu-id="ab7d3-153">Hej kretsägaren måste tooexplicitly meddela hello andra prenumerationsägaren som hello auktorisering är klar.</span><span class="sxs-lookup"><span data-stu-id="ab7d3-153">hello circuit owner needs tooexplicitly notify hello other subscription owner that hello authorization is complete.</span></span>

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

<span data-ttu-id="ab7d3-154">**Granska tillstånd**</span><span class="sxs-lookup"><span data-stu-id="ab7d3-154">**Reviewing authorizations**</span></span>

<span data-ttu-id="ab7d3-155">Hej kretsägaren kan granska alla tillstånd som utfärdats för en viss krets genom att köra följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="ab7d3-155">hello circuit owner can review all authorizations that are issued on a particular circuit by running hello following cmdlet:</span></span>

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


<span data-ttu-id="ab7d3-156">**Uppdaterar tillstånd**</span><span class="sxs-lookup"><span data-stu-id="ab7d3-156">**Updating authorizations**</span></span>

<span data-ttu-id="ab7d3-157">Hej kretsägaren kan ändra tillstånd med hjälp av hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="ab7d3-157">hello circuit owner can modify authorizations by using hello following cmdlet:</span></span>

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


<span data-ttu-id="ab7d3-158">**Ta bort tillstånd**</span><span class="sxs-lookup"><span data-stu-id="ab7d3-158">**Deleting authorizations**</span></span>

<span data-ttu-id="ab7d3-159">Hej kretsägaren kan återkalla/ta bort tillstånd toohello användare genom att köra följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="ab7d3-159">hello circuit owner can revoke/delete authorizations toohello user by running hello following cmdlet:</span></span>

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a><span data-ttu-id="ab7d3-160">Kretsen användaren åtgärder</span><span class="sxs-lookup"><span data-stu-id="ab7d3-160">Circuit user operations</span></span>

<span data-ttu-id="ab7d3-161">**Granska tillstånd**</span><span class="sxs-lookup"><span data-stu-id="ab7d3-161">**Reviewing authorizations**</span></span>

<span data-ttu-id="ab7d3-162">Hej kretsanvändare kan granska tillstånd med hjälp av hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="ab7d3-162">hello circuit user can review authorizations by using hello following cmdlet:</span></span>

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

<span data-ttu-id="ab7d3-163">**Löser länken tillstånd**</span><span class="sxs-lookup"><span data-stu-id="ab7d3-163">**Redeeming link authorizations**</span></span>

<span data-ttu-id="ab7d3-164">hello kretsanvändare kan köra följande cmdlet tooredeem hello ett länk-tillstånd:</span><span class="sxs-lookup"><span data-stu-id="ab7d3-164">hello circuit user can run hello following cmdlet tooredeem a link authorization:</span></span>

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

<span data-ttu-id="ab7d3-165">Kör kommandot i hello nyligen länkade prenumerationen för hello virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="ab7d3-165">Run this command in hello newly linked subscription for hello virtual network:</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a><span data-ttu-id="ab7d3-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab7d3-166">Next steps</span></span>
<span data-ttu-id="ab7d3-167">Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="ab7d3-167">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

