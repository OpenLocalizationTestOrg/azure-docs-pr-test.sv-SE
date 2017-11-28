---
title: "Länka ett virtuellt nätverk tooan ExpressRoute-krets: Azure-portalen | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur toolink virtuella nätverk (Vnet) tooExpressRoute kretsar."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8bedcb11df7e30281fd439afdfb76cc67626a8f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="6339f-103">Ansluta ett virtuellt nätverk tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6339f-103">Connect a virtual network tooan ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6339f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6339f-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="6339f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6339f-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="6339f-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6339f-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="6339f-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6339f-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="6339f-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="6339f-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

<span data-ttu-id="6339f-109">Den här artikeln hjälper dig att länka virtuella nätverk (Vnet) tooAzure ExpressRoute-kretsar med hjälp av hello Resource Manager-modellen och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6339f-109">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello Resource Manager deployment model and hello Azure portal.</span></span> <span data-ttu-id="6339f-110">Virtuella nätverk kan antingen vara i hello samma prenumeration och de kan vara en del av en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6339f-110">Virtual networks can either be in hello same subscription, or they can be part of another subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6339f-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6339f-111">Before you begin</span></span>
* <span data-ttu-id="6339f-112">Granska hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="6339f-112">Review hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="6339f-113">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="6339f-113">You must have an active ExpressRoute circuit.</span></span>
  
  * <span data-ttu-id="6339f-114">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och ha hello krets aktiveras med anslutningsleverantören.</span><span class="sxs-lookup"><span data-stu-id="6339f-114">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider.</span></span>
  * <span data-ttu-id="6339f-115">Se till att du har privat Azure-peering konfigurerats för kretsen.</span><span class="sxs-lookup"><span data-stu-id="6339f-115">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="6339f-116">Se hello [konfigurera routning](expressroute-howto-routing-portal-resource-manager.md) artikel routning anvisningar.</span><span class="sxs-lookup"><span data-stu-id="6339f-116">See hello [Configure routing](expressroute-howto-routing-portal-resource-manager.md) article for routing instructions.</span></span>
  * <span data-ttu-id="6339f-117">Kontrollera att privat Azure-peering har konfigurerats och hello BGP-peering mellan ditt nätverk och Microsoft är igång så att du kan aktivera anslutning för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6339f-117">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="6339f-118">Se till att du har ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="6339f-118">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="6339f-119">Följ instruktionerna för hello för[skapa en virtuell nätverksgateway för ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6339f-119">Follow hello instructions too[create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="6339f-120">En virtuell nätverksgateway expressroute använder hello GatewayType ExpressRoute, inte VPN.</span><span class="sxs-lookup"><span data-stu-id="6339f-120">A virtual network gateway for ExpressRoute uses hello GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="6339f-121">Du kan länka upp too10-virtuella nätverk tooa standard ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6339f-121">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="6339f-122">Alla virtuella nätverk måste vara i hello samma geopolitiska region när du använder en standard ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="6339f-122">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 
* <span data-ttu-id="6339f-123">Du kan länka ett virtuellt nätverk utanför hello geopolitiska regionens hello ExpressRoute-krets eller ansluta ett stort antal virtuella nätverk tooyour ExpressRoute-krets om du har aktiverat hello ExpressRoute premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="6339f-123">You can link a virtual network outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="6339f-124">Kontrollera hello [vanliga frågor och svar](expressroute-faqs.md) för mer information om hello premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="6339f-124">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>
* <span data-ttu-id="6339f-125">Du kan [visa en video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) innan början toobetter förstå hello steg.</span><span class="sxs-lookup"><span data-stu-id="6339f-125">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) before beginning toobetter understand hello steps.</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="6339f-126">Ansluta ett virtuellt nätverk i hello samma prenumeration tooa krets</span><span class="sxs-lookup"><span data-stu-id="6339f-126">Connect a virtual network in hello same subscription tooa circuit</span></span>

### <a name="toocreate-a-connection"></a><span data-ttu-id="6339f-127">toocreate en anslutning</span><span class="sxs-lookup"><span data-stu-id="6339f-127">toocreate a connection</span></span>

> [!NOTE]
> <span data-ttu-id="6339f-128">BGP-konfigurationsinformationen kommer inte att visas om hello nivå 3-providern konfigurerad din peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="6339f-128">BGP configuration information will not show up if hello layer 3 provider configured your peerings.</span></span> <span data-ttu-id="6339f-129">Om kretsen är i ett etablerat tillstånd, bör du kunna toocreate anslutningar.</span><span class="sxs-lookup"><span data-stu-id="6339f-129">If your circuit is in a provisioned state, you should be able toocreate connections.</span></span>
>

1. <span data-ttu-id="6339f-130">Se till att dina ExpressRoute-krets och privat Azure-peering har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="6339f-130">Ensure that your ExpressRoute circuit and Azure private peering have been configured successfully.</span></span> <span data-ttu-id="6339f-131">Följ anvisningarna för hello i [skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och [konfigurera routning](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="6339f-131">Follow hello instructions in [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [Configure routing](expressroute-howto-routing-arm.md).</span></span> <span data-ttu-id="6339f-132">ExpressRoute-krets bör se ut som hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="6339f-132">Your ExpressRoute circuit should look like hello following image:</span></span>

    ![Skärmbild för ExpressRoute-krets](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. <span data-ttu-id="6339f-134">Du kan nu börja etablera en anslutning toolink ditt virtuella nätverk på en gateway-tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6339f-134">You can now start provisioning a connection toolink your virtual network gateway tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="6339f-135">Klicka på **anslutning** > **Lägg till** tooopen hello **Lägg till anslutning** bladet och sedan konfigurera hello värden.</span><span class="sxs-lookup"><span data-stu-id="6339f-135">Click **Connection** > **Add** tooopen hello **Add connection** blade, and then configure hello values.</span></span>

    ![Lägg till anslutning skärmbild](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. <span data-ttu-id="6339f-137">När anslutningen har konfigurerats, visas connection-objektet hello information för hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="6339f-137">After your connection has been successfully configured, your connection object will show hello information for hello connection.</span></span>

     ![Skärmbild av anslutningen objekt](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="toodelete-a-connection"></a><span data-ttu-id="6339f-139">toodelete en anslutning</span><span class="sxs-lookup"><span data-stu-id="6339f-139">toodelete a connection</span></span>
<span data-ttu-id="6339f-140">Du kan ta bort en anslutning genom att välja hello **ta bort** ikon på hello bladet för din anslutning.</span><span class="sxs-lookup"><span data-stu-id="6339f-140">You can delete a connection by selecting hello **Delete** icon on hello blade for your connection.</span></span>

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="6339f-141">Ansluta ett virtuellt nätverk i en annan prenumeration tooa-krets</span><span class="sxs-lookup"><span data-stu-id="6339f-141">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="6339f-142">Du kan dela en ExpressRoute-krets över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6339f-142">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="6339f-143">hello bilden nedan visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6339f-143">hello figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- <span data-ttu-id="6339f-145">Varje hello mindre moln inom hello stora moln är används toorepresent prenumerationer som tillhör toodifferent avdelningar inom en organisation.</span><span class="sxs-lookup"><span data-stu-id="6339f-145">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span>
- <span data-ttu-id="6339f-146">Var och en av hello avdelningar inom hello organisation kan använda sina egna prenumeration för att distribuera sina tjänster, men de kan dela ett ExpressRoute-kretsen tooconnect tillbaka tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="6339f-146">Each of hello departments within hello organization can use their own subscription for deploying their services, but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span>
- <span data-ttu-id="6339f-147">En avdelning (i det här exemplet: IT) kan äga hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6339f-147">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="6339f-148">Andra prenumerationer i hello organisation kan använda hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6339f-148">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6339f-149">Anslutningar och bandbredd kostnader för dedikerade hello krets kommer att tillämpas toohello ExpressRoute-kretsen ägare.</span><span class="sxs-lookup"><span data-stu-id="6339f-149">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute circuit owner.</span></span> <span data-ttu-id="6339f-150">Alla virtuella nätverk dela hello samma bandbredd.</span><span class="sxs-lookup"><span data-stu-id="6339f-150">All virtual networks share hello same bandwidth.</span></span>
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="6339f-151">Administration - krets ägare och krets användare</span><span class="sxs-lookup"><span data-stu-id="6339f-151">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="6339f-152">hello 'kretsägaren' är en behörig Privilegierade användare i hello ExpressRoute-kretsen resurs.</span><span class="sxs-lookup"><span data-stu-id="6339f-152">hello 'circuit owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="6339f-153">Hej kretsägaren kan skapa tillstånd som kan lösas av krets-användare.</span><span class="sxs-lookup"><span data-stu-id="6339f-153">hello circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="6339f-154">Kretsen användare är ägare till virtuella nätverk gateways som inte är inom hello samma prenumeration som hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6339f-154">Circuit users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="6339f-155">Kretsen användare kan lösa tillstånd (en auktorisering per virtuellt nätverk).</span><span class="sxs-lookup"><span data-stu-id="6339f-155">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="6339f-156">Hej kretsägaren har hello power toomodify samt återkalla tillstånd när som helst.</span><span class="sxs-lookup"><span data-stu-id="6339f-156">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="6339f-157">Återkalla ett tillstånd som resulterar i alla anslutningar tas bort från hello prenumeration vars åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="6339f-157">Revoking an authorization results in all link connections being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="6339f-158">Kretsen ägare åtgärder</span><span class="sxs-lookup"><span data-stu-id="6339f-158">Circuit owner operations</span></span>

<span data-ttu-id="6339f-159">**toocreate en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="6339f-159">**toocreate a connection authorization**</span></span>

<span data-ttu-id="6339f-160">Hej kretsägaren skapar tillstånd.</span><span class="sxs-lookup"><span data-stu-id="6339f-160">hello circuit owner creates an authorization.</span></span> <span data-ttu-id="6339f-161">Detta resulterar i hello skapandet av auktoriseringsnyckel som kan användas av en krets användaren tooconnect sina virtuella nätverk gateways toohello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6339f-161">This results in hello creation of an authorization key that can be used by a circuit user tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="6339f-162">Ett tillstånd som är giltig för endast en anslutning.</span><span class="sxs-lookup"><span data-stu-id="6339f-162">An authorization is valid for only one connection.</span></span>

1. <span data-ttu-id="6339f-163">Hej ExpressRoute-bladet, klickar du på **tillstånd** och skriv sedan ett **namn** för hello auktorisering och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="6339f-163">In hello ExpressRoute blade, Click **Authorizations** and then type a **name** for hello authorization and click **Save**.</span></span>

    ![Tillstånd](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. <span data-ttu-id="6339f-165">Kopiera hello när hello konfiguration sparas **resurs-ID** och hello **Auktoriseringsnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="6339f-165">Once hello configuration is saved, copy hello **Resource ID** and hello **Authorization Key**.</span></span>

    ![Auktoriseringsnyckeln](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

<span data-ttu-id="6339f-167">**toodelete en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="6339f-167">**toodelete a connection authorization**</span></span>

<span data-ttu-id="6339f-168">Du kan ta bort en anslutning genom att välja hello **ta bort** ikon på hello bladet för din anslutning.</span><span class="sxs-lookup"><span data-stu-id="6339f-168">You can delete a connection by selecting hello **Delete** icon on hello blade for your connection.</span></span>

### <a name="circuit-user-operations"></a><span data-ttu-id="6339f-169">Kretsen användaren åtgärder</span><span class="sxs-lookup"><span data-stu-id="6339f-169">Circuit user operations</span></span>

<span data-ttu-id="6339f-170">Hej kretsanvändare behöver hello resurs-ID och auktoriseringsnyckel från hello kretsägaren.</span><span class="sxs-lookup"><span data-stu-id="6339f-170">hello circuit user needs hello resource ID and an authorization key from hello circuit owner.</span></span> 

<span data-ttu-id="6339f-171">**tooredeem en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="6339f-171">**tooredeem a connection authorization**</span></span>

1.  <span data-ttu-id="6339f-172">Klicka på hello **+ ny** knappen.</span><span class="sxs-lookup"><span data-stu-id="6339f-172">Click hello **+New** button.</span></span>

    ![Klicka på ny](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  <span data-ttu-id="6339f-174">Sök efter **”anslutning”** i hello Marketplace, markerar du den och klickar på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6339f-174">Search for **"Connection"** in hello Marketplace, select it, and click **Create**.</span></span>

    ![Sök efter anslutning](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  <span data-ttu-id="6339f-176">Se till att hello **anslutningstypen** anges för ”ExpressRoute”.</span><span class="sxs-lookup"><span data-stu-id="6339f-176">Make sure hello **Connection type** is set too"ExpressRoute".</span></span>


4.  <span data-ttu-id="6339f-177">Fyll i hello information och klicka sedan på **OK** hello grunderna-bladet.</span><span class="sxs-lookup"><span data-stu-id="6339f-177">Fill in hello details, then click **OK** in hello Basics blade.</span></span>

    ![Bladet Grundläggande inställningar](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  <span data-ttu-id="6339f-179">I hello **inställningar** bladet, Välj hello **virtuell nätverksgateway** och kontrollera hello **lösa auktorisering** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="6339f-179">In hello **Settings** blade, Select hello **Virtual network gateway** and check hello **Redeem authorization** check box.</span></span>

6.  <span data-ttu-id="6339f-180">Ange hello **auktoriseringsnyckeln** och hello **Peer-krets URI** och namnge hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="6339f-180">Enter hello **Authorization key** and hello **Peer circuit URI** and give hello connection a name.</span></span> <span data-ttu-id="6339f-181">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6339f-181">Click **OK**.</span></span>

    ![Bladet Inställningar](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. <span data-ttu-id="6339f-183">Studera hello hello **sammanfattning** bladet och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6339f-183">Review hello information in hello **Summary** blade and click **OK**.</span></span>


<span data-ttu-id="6339f-184">**toorelease en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="6339f-184">**toorelease a connection authorization**</span></span>

<span data-ttu-id="6339f-185">Du kan släppa tillstånd genom att ta bort hello-anslutning som länkar hello ExpressRoute-kretsen toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="6339f-185">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6339f-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6339f-186">Next steps</span></span>
<span data-ttu-id="6339f-187">Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="6339f-187">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
