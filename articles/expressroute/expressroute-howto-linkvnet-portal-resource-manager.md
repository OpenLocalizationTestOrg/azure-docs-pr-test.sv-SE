---
title: "Länka ett virtuellt nätverk till en ExpressRoute-krets: Azure-portalen | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur du länkar virtuella nätverk (Vnet) för ExpressRoute-kretsar."
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
ms.openlocfilehash: 595c30ab5d9adc6061ad753d952adf894ba80b2f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="a6086-103">Ansluta ett virtuellt nätverk till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="a6086-103">Connect a virtual network to an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a6086-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a6086-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="a6086-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a6086-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="a6086-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a6086-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="a6086-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a6086-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="a6086-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="a6086-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

<span data-ttu-id="a6086-109">Den här artikeln hjälper dig att länka virtuella nätverk (Vnet) Azure ExpressRoute-kretsar med hjälp av Resource Manager-distributionsmodellen och Azure portal.</span><span class="sxs-lookup"><span data-stu-id="a6086-109">This article helps you link virtual networks (VNets) to Azure ExpressRoute circuits by using the Resource Manager deployment model and the Azure portal.</span></span> <span data-ttu-id="a6086-110">Virtuella nätverk kan antingen vara i samma prenumeration och de kan vara en del av en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a6086-110">Virtual networks can either be in the same subscription, or they can be part of another subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a6086-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a6086-111">Before you begin</span></span>
* <span data-ttu-id="a6086-112">Granska de [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="a6086-112">Review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="a6086-113">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a6086-113">You must have an active ExpressRoute circuit.</span></span>
  
  * <span data-ttu-id="a6086-114">Följ instruktionerna för att [skapar du en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och ha kretsen aktiveras med anslutningsleverantören.</span><span class="sxs-lookup"><span data-stu-id="a6086-114">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider.</span></span>
  * <span data-ttu-id="a6086-115">Se till att du har privat Azure-peering konfigurerats för kretsen.</span><span class="sxs-lookup"><span data-stu-id="a6086-115">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="a6086-116">Finns det [konfigurera routning](expressroute-howto-routing-portal-resource-manager.md) artikel routning anvisningar.</span><span class="sxs-lookup"><span data-stu-id="a6086-116">See the [Configure routing](expressroute-howto-routing-portal-resource-manager.md) article for routing instructions.</span></span>
  * <span data-ttu-id="a6086-117">Kontrollera att privat Azure-peering har konfigurerats och BGP-peering mellan ditt nätverk och Microsoft är igång så att du kan aktivera anslutning för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a6086-117">Ensure that Azure private peering is configured and the BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="a6086-118">Se till att du har ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="a6086-118">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="a6086-119">Följ instruktionerna för att [skapa en virtuell nätverksgateway för ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a6086-119">Follow the instructions to [create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="a6086-120">En virtuell nätverksgateway expressroute använder GatewayType ExpressRoute, inte VPN.</span><span class="sxs-lookup"><span data-stu-id="a6086-120">A virtual network gateway for ExpressRoute uses the GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="a6086-121">Du kan länka upp till 10 virtuella nätverk till en standard ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a6086-121">You can link up to 10 virtual networks to a standard ExpressRoute circuit.</span></span> <span data-ttu-id="a6086-122">Alla virtuella nätverk måste vara i samma region geopolitiska när du använder en standard ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a6086-122">All virtual networks must be in the same geopolitical region when using a standard ExpressRoute circuit.</span></span> 
* <span data-ttu-id="a6086-123">Du kan länka ett virtuellt nätverk utanför ExpressRoute-kretsen geopolitiska regionen eller ansluta ett stort antal virtuella nätverk till ExpressRoute-krets om du har aktiverat ExpressRoute premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="a6086-123">You can link a virtual network outside of the geopolitical region of the ExpressRoute circuit, or connect a larger number of virtual networks to your ExpressRoute circuit if you enabled the ExpressRoute premium add-on.</span></span> <span data-ttu-id="a6086-124">Kontrollera den [vanliga frågor och svar](expressroute-faqs.md) för mer information om premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="a6086-124">Check the [FAQ](expressroute-faqs.md) for more details on the premium add-on.</span></span>
* <span data-ttu-id="a6086-125">Du kan [visa en video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) innan du börjar att bättre förstå stegen.</span><span class="sxs-lookup"><span data-stu-id="a6086-125">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) before beginning to better understand the steps.</span></span>

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="a6086-126">Ansluta ett virtuellt nätverk med samma prenumeration till en krets</span><span class="sxs-lookup"><span data-stu-id="a6086-126">Connect a virtual network in the same subscription to a circuit</span></span>

### <a name="to-create-a-connection"></a><span data-ttu-id="a6086-127">Att skapa en anslutning</span><span class="sxs-lookup"><span data-stu-id="a6086-127">To create a connection</span></span>

> [!NOTE]
> <span data-ttu-id="a6086-128">BGP-konfigurationsinformationen kommer inte att visas om nivå 3-providern konfigurerat din peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="a6086-128">BGP configuration information will not show up if the layer 3 provider configured your peerings.</span></span> <span data-ttu-id="a6086-129">Om kretsen är i ett etablerat tillstånd, bör du kunna skapa anslutningar.</span><span class="sxs-lookup"><span data-stu-id="a6086-129">If your circuit is in a provisioned state, you should be able to create connections.</span></span>
>

1. <span data-ttu-id="a6086-130">Se till att dina ExpressRoute-krets och privat Azure-peering har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="a6086-130">Ensure that your ExpressRoute circuit and Azure private peering have been configured successfully.</span></span> <span data-ttu-id="a6086-131">Följ instruktionerna i [skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och [konfigurera routning](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="a6086-131">Follow the instructions in [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [Configure routing](expressroute-howto-routing-arm.md).</span></span> <span data-ttu-id="a6086-132">ExpressRoute-krets bör se ut som följande bild:</span><span class="sxs-lookup"><span data-stu-id="a6086-132">Your ExpressRoute circuit should look like the following image:</span></span>

    ![Skärmbild för ExpressRoute-krets](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. <span data-ttu-id="a6086-134">Du kan nu börja etablera en anslutning om du vill länka din virtuella nätverksgateway till ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a6086-134">You can now start provisioning a connection to link your virtual network gateway to your ExpressRoute circuit.</span></span> <span data-ttu-id="a6086-135">Klicka på **anslutning** > **Lägg till** att öppna den **Lägg till anslutning** bladet och sedan konfigurera värden.</span><span class="sxs-lookup"><span data-stu-id="a6086-135">Click **Connection** > **Add** to open the **Add connection** blade, and then configure the values.</span></span>

    ![Lägg till anslutning skärmbild](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. <span data-ttu-id="a6086-137">När anslutningen har konfigurerats, visas connection-objektet information för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a6086-137">After your connection has been successfully configured, your connection object will show the information for the connection.</span></span>

     ![Skärmbild av anslutningen objekt](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="to-delete-a-connection"></a><span data-ttu-id="a6086-139">Ta bort en anslutning</span><span class="sxs-lookup"><span data-stu-id="a6086-139">To delete a connection</span></span>
<span data-ttu-id="a6086-140">Du kan ta bort en anslutning genom att välja den **ta bort** ikon på bladet för din anslutning.</span><span class="sxs-lookup"><span data-stu-id="a6086-140">You can delete a connection by selecting the **Delete** icon on the blade for your connection.</span></span>

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="a6086-141">Anslut ett virtuellt nätverk i en annan prenumeration till en krets</span><span class="sxs-lookup"><span data-stu-id="a6086-141">Connect a virtual network in a different subscription to a circuit</span></span>
<span data-ttu-id="a6086-142">Du kan dela en ExpressRoute-krets över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="a6086-142">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="a6086-143">Bilden nedan visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="a6086-143">The figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- <span data-ttu-id="a6086-145">Var och en av mindre molnen i stora molnet används för att representera prenumerationer som hör till olika avdelningar inom en organisation.</span><span class="sxs-lookup"><span data-stu-id="a6086-145">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span>
- <span data-ttu-id="a6086-146">Varje avdelning inom organisationen kan använda sina egna prenumeration för att distribuera sina tjänster, men de kan dela en enda ExpressRoute-krets att ansluta till ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="a6086-146">Each of the departments within the organization can use their own subscription for deploying their services, but they can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span>
- <span data-ttu-id="a6086-147">En avdelning (i det här exemplet: IT) kan äga ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a6086-147">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="a6086-148">Andra prenumerationer inom organisationen kan använda ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a6086-148">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a6086-149">Anslutning och bandbredd kostnader för dedikerade kretsen tillämpas på ExpressRoute-kretsen ägare.</span><span class="sxs-lookup"><span data-stu-id="a6086-149">Connectivity and bandwidth charges for the dedicated circuit will be applied to the ExpressRoute circuit owner.</span></span> <span data-ttu-id="a6086-150">Alla virtuella nätverk dela på samma bandbredd.</span><span class="sxs-lookup"><span data-stu-id="a6086-150">All virtual networks share the same bandwidth.</span></span>
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="a6086-151">Administration - krets ägare och krets användare</span><span class="sxs-lookup"><span data-stu-id="a6086-151">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="a6086-152">Kretsägaren är en auktoriserad Power användare av resursen ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a6086-152">The 'circuit owner' is an authorized Power User of the ExpressRoute circuit resource.</span></span> <span data-ttu-id="a6086-153">Kretsägaren kan skapa tillstånd som kan lösas av krets-användare.</span><span class="sxs-lookup"><span data-stu-id="a6086-153">The circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="a6086-154">Kretsen användare är ägare till virtuella nätverks-gateway som inte är inom samma prenumeration som ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a6086-154">Circuit users are owners of virtual network gateways that are not within the same subscription as the ExpressRoute circuit.</span></span> <span data-ttu-id="a6086-155">Kretsen användare kan lösa tillstånd (en auktorisering per virtuellt nätverk).</span><span class="sxs-lookup"><span data-stu-id="a6086-155">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="a6086-156">Kretsägaren har behörighet att ändra och återkalla tillstånd när som helst.</span><span class="sxs-lookup"><span data-stu-id="a6086-156">The circuit owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="a6086-157">Återkalla ett tillstånd som resulterar i alla anslutningar tas bort från prenumerationen vars åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="a6086-157">Revoking an authorization results in all link connections being deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="a6086-158">Kretsen ägare åtgärder</span><span class="sxs-lookup"><span data-stu-id="a6086-158">Circuit owner operations</span></span>

<span data-ttu-id="a6086-159">**Så här skapar du en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="a6086-159">**To create a connection authorization**</span></span>

<span data-ttu-id="a6086-160">Kretsägaren skapar tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a6086-160">The circuit owner creates an authorization.</span></span> <span data-ttu-id="a6086-161">Detta resulterar i att skapa auktoriseringsnyckel som kan användas av en kretsanvändare för att ansluta sina virtuella nätverksgatewayerna till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a6086-161">This results in the creation of an authorization key that can be used by a circuit user to connect their virtual network gateways to the ExpressRoute circuit.</span></span> <span data-ttu-id="a6086-162">Ett tillstånd som är giltig för endast en anslutning.</span><span class="sxs-lookup"><span data-stu-id="a6086-162">An authorization is valid for only one connection.</span></span>

1. <span data-ttu-id="a6086-163">I ExpressRoute-bladet klickar du på **tillstånd** och skriv sedan ett **namn** för auktorisering och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="a6086-163">In the ExpressRoute blade, Click **Authorizations** and then type a **name** for the authorization and click **Save**.</span></span>

    ![Tillstånd](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. <span data-ttu-id="a6086-165">När konfigurationen har sparats kan du kopiera den **resurs-ID** och **Auktoriseringsnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="a6086-165">Once the configuration is saved, copy the **Resource ID** and the **Authorization Key**.</span></span>

    ![Auktoriseringsnyckeln](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

<span data-ttu-id="a6086-167">**Ta bort en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="a6086-167">**To delete a connection authorization**</span></span>

<span data-ttu-id="a6086-168">Du kan ta bort en anslutning genom att välja den **ta bort** ikon på bladet för din anslutning.</span><span class="sxs-lookup"><span data-stu-id="a6086-168">You can delete a connection by selecting the **Delete** icon on the blade for your connection.</span></span>

### <a name="circuit-user-operations"></a><span data-ttu-id="a6086-169">Kretsen användaren åtgärder</span><span class="sxs-lookup"><span data-stu-id="a6086-169">Circuit user operations</span></span>

<span data-ttu-id="a6086-170">Kretsen användaren måste resurs-ID och auktoriseringsnyckel från kretsägaren.</span><span class="sxs-lookup"><span data-stu-id="a6086-170">The circuit user needs the resource ID and an authorization key from the circuit owner.</span></span> 

<span data-ttu-id="a6086-171">**Lösa in en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="a6086-171">**To redeem a connection authorization**</span></span>

1.  <span data-ttu-id="a6086-172">Klicka på den **+ ny** knappen.</span><span class="sxs-lookup"><span data-stu-id="a6086-172">Click the **+New** button.</span></span>

    ![Klicka på ny](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  <span data-ttu-id="a6086-174">Sök efter **”anslutning”** i Marketplace, markerar du den och klickar på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a6086-174">Search for **"Connection"** in the Marketplace, select it, and click **Create**.</span></span>

    ![Sök efter anslutning](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  <span data-ttu-id="a6086-176">Kontrollera att den **anslutningstypen** är inställd på ”ExpressRoute”.</span><span class="sxs-lookup"><span data-stu-id="a6086-176">Make sure the **Connection type** is set to "ExpressRoute".</span></span>


4.  <span data-ttu-id="a6086-177">Fyll i informationen och klicka sedan på **OK** i bladet grundläggande.</span><span class="sxs-lookup"><span data-stu-id="a6086-177">Fill in the details, then click **OK** in the Basics blade.</span></span>

    ![Bladet Grundläggande inställningar](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  <span data-ttu-id="a6086-179">I den **inställningar** bladet Välj den **virtuell nätverksgateway** och kontrollera den **lösa auktorisering** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="a6086-179">In the **Settings** blade, Select the **Virtual network gateway** and check the **Redeem authorization** check box.</span></span>

6.  <span data-ttu-id="a6086-180">Ange den **auktoriseringsnyckeln** och **Peer-krets URI** och namnge anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a6086-180">Enter the **Authorization key** and the **Peer circuit URI** and give the connection a name.</span></span> <span data-ttu-id="a6086-181">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6086-181">Click **OK**.</span></span>

    ![Bladet Inställningar](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. <span data-ttu-id="a6086-183">Granska informationen i den **sammanfattning** bladet och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6086-183">Review the information in the **Summary** blade and click **OK**.</span></span>


<span data-ttu-id="a6086-184">**Att släppa en anslutningsverifiering**</span><span class="sxs-lookup"><span data-stu-id="a6086-184">**To release a connection authorization**</span></span>

<span data-ttu-id="a6086-185">Du kan släppa tillstånd genom att ta bort anslutningen som länkar ExpressRoute-kretsen till det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="a6086-185">You can release an authorization by deleting the connection that links the ExpressRoute circuit to the virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6086-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6086-186">Next steps</span></span>
<span data-ttu-id="a6086-187">Mer information om ExpressRoute finns i [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="a6086-187">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
