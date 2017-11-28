---
title: "Skapa och ändra en ExpressRoute-krets: Azure-portalen | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate, etablera, verifiera, uppdatera, ta bort och ta bort etableringen av en ExpressRoute-krets."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: 200418aed6bdebace43a2f57cf532d1c8d13cb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a><span data-ttu-id="6ac25-103">Skapa och ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-103">Create and modify an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6ac25-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6ac25-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="6ac25-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6ac25-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="6ac25-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6ac25-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="6ac25-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6ac25-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="6ac25-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="6ac25-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="6ac25-109">Den här artikeln beskriver hur toocreate Azure ExpressRoute-kretsen med hjälp av hello Azure-portalen och hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="6ac25-109">This article describes how toocreate an Azure ExpressRoute circuit by using hello Azure portal and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="6ac25-110">hello också följande steg visar hur toocheck hello kretsstatusen hello, uppdatera eller ta bort och ta bort etableringen av den.</span><span class="sxs-lookup"><span data-stu-id="6ac25-110">hello following steps also show you how toocheck hello status of hello circuit, update it, or delete and deprovision it.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="6ac25-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6ac25-111">Before you begin</span></span>
* <span data-ttu-id="6ac25-112">Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="6ac25-112">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="6ac25-113">Kontrollera att du har åtkomst toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6ac25-113">Ensure that you have access toohello [Azure portal](https://portal.azure.com).</span></span>
* <span data-ttu-id="6ac25-114">Se till att du har behörigheter toocreate nya nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="6ac25-114">Ensure that you have permissions toocreate new networking resources.</span></span> <span data-ttu-id="6ac25-115">Kontakta din kontoadministratör om du inte har rätt behörigheter för hello.</span><span class="sxs-lookup"><span data-stu-id="6ac25-115">Contact your account administrator if you do not have hello right permissions.</span></span>
* <span data-ttu-id="6ac25-116">Du kan [visa en video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) innan du börjar i ordning toobetter förstå hello steg.</span><span class="sxs-lookup"><span data-stu-id="6ac25-116">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) before beginning in order toobetter understand hello steps.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="6ac25-117">Skapa och etablera en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-117">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-toohello-azure-portal"></a><span data-ttu-id="6ac25-118">1. Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6ac25-118">1. Sign in toohello Azure portal</span></span>
<span data-ttu-id="6ac25-119">Från en webbläsare, navigerar du toohello [Azure-portalen](http://portal.azure.com) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6ac25-119">From a browser, navigate toohello [Azure portal](http://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="2-create-a-new-expressroute-circuit"></a><span data-ttu-id="6ac25-120">2. Skapa en ny ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-120">2. Create a new ExpressRoute circuit</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6ac25-121">ExpressRoute-krets kommer att debiteras från hello tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="6ac25-121">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="6ac25-122">Se till att utföra den här åtgärden när hello anslutningen providern är klar tooprovision hello krets.</span><span class="sxs-lookup"><span data-stu-id="6ac25-122">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

1. <span data-ttu-id="6ac25-123">Du kan skapa en ExpressRoute-krets genom att välja hello alternativet toocreate en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="6ac25-123">You can create an ExpressRoute circuit by selecting hello option toocreate a new resource.</span></span> <span data-ttu-id="6ac25-124">Klicka på **ny** > **nätverk** > **ExpressRoute**, enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="6ac25-124">Click **New** > **Networking** > **ExpressRoute**, as shown in hello following image:</span></span>
   
    ![Skapa en ExpressRoute-krets](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. <span data-ttu-id="6ac25-126">När du klickar på **ExpressRoute**, ser du hello **skapa ExpressRoute-krets** bladet.</span><span class="sxs-lookup"><span data-stu-id="6ac25-126">After you click **ExpressRoute**, you'll see hello **Create ExpressRoute circuit** blade.</span></span> <span data-ttu-id="6ac25-127">När du fyller i hello värden på det här bladet kan du se till att du anger hello rätt SKU-nivå och datamätning.</span><span class="sxs-lookup"><span data-stu-id="6ac25-127">When you're filling in hello values on this blade, make sure that you specify hello correct SKU tier and data metering.</span></span>
   
   * <span data-ttu-id="6ac25-128">**Nivån** bestämmer om en ExpressRoute-standard eller premium-tillägg ett ExpressRoute är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="6ac25-128">**Tier** determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="6ac25-129">Du kan ange **Standard** tooget hello standard SKU eller **Premium** för hello premium.</span><span class="sxs-lookup"><span data-stu-id="6ac25-129">You can specify **Standard** tooget hello standard SKU or **Premium** for hello premium add-on.</span></span>
   * <span data-ttu-id="6ac25-130">**Datamätning** anger hello fakturering typ.</span><span class="sxs-lookup"><span data-stu-id="6ac25-130">**Data metering** determines hello billing type.</span></span> <span data-ttu-id="6ac25-131">Du kan ange **Metered** för en plan för förbrukade data och **obegränsad** för en obegränsad plan.</span><span class="sxs-lookup"><span data-stu-id="6ac25-131">You can specify **Metered** for a metered data plan and **Unlimited** for an unlimited data plan.</span></span> <span data-ttu-id="6ac25-132">Observera att du kan ändra hello fakturering typen från **Metered** för**obegränsad**, men du kan inte ändra hello typen från **obegränsad** för**Metered**.</span><span class="sxs-lookup"><span data-stu-id="6ac25-132">Note that you can change hello billing type from **Metered** too**Unlimited**, but you can't change hello type from **Unlimited** too**Metered**.</span></span>
     
     ![Konfigurera hello SKU-nivå och datamätning](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> <span data-ttu-id="6ac25-134">Tänk på att hello Peering plats anger hello [fysisk plats](expressroute-locations.md) där du peering med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6ac25-134">Please be aware that hello Peering Location indicates hello [physical location](expressroute-locations.md) where you are peering with Microsoft.</span></span> <span data-ttu-id="6ac25-135">Detta är **inte** länkade för ”plats”-egenskap som refererar toohello geografi där hello Azure Nätverksresursprovidern finns.</span><span class="sxs-lookup"><span data-stu-id="6ac25-135">This is **not** linked too"Location" property, which refers toohello geography where hello Azure Network Resource Provider is located.</span></span> <span data-ttu-id="6ac25-136">När de inte är relaterade är det en bra idé toochoose en Nätverksresursprovidern geografiskt nära toohello Peering-platsen för hello krets.</span><span class="sxs-lookup"><span data-stu-id="6ac25-136">While they are not related, it is a good practice toochoose a Network Resource Provider geographically close toohello Peering Location of hello circuit.</span></span> 
> 
> 

### <a name="3-view-hello-circuits-and-properties"></a><span data-ttu-id="6ac25-137">3. Visa hello kretsar och egenskaper</span><span class="sxs-lookup"><span data-stu-id="6ac25-137">3. View hello circuits and properties</span></span>
<span data-ttu-id="6ac25-138">**Visa alla hello kretsar**</span><span class="sxs-lookup"><span data-stu-id="6ac25-138">**View all hello circuits**</span></span>

<span data-ttu-id="6ac25-139">Du kan visa alla hello kretsar som du skapade genom att välja **alla resurser** på hello vänster-menyn.</span><span class="sxs-lookup"><span data-stu-id="6ac25-139">You can view all hello circuits that you created by selecting **All resources** on hello left-side menu.</span></span>

![Visa kretsar](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

<span data-ttu-id="6ac25-141">**Visa hello egenskaper**</span><span class="sxs-lookup"><span data-stu-id="6ac25-141">**View hello properties**</span></span>

    You can view hello properties of hello circuit by selecting it. On this blade, note hello service key for hello circuit. You must copy hello circuit key for your circuit and pass it down toohello service provider toocomplete hello provisioning process. hello circuit key is specific tooyour circuit.

![Visa egenskaper](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="6ac25-143">4. Skicka hello-nyckel tooyour anslutning leverantör för etablering</span><span class="sxs-lookup"><span data-stu-id="6ac25-143">4. Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="6ac25-144">På det här bladet **Providerstatus** innehåller information om hello aktuell status för etablering på hello-leverantör sida.</span><span class="sxs-lookup"><span data-stu-id="6ac25-144">On this blade, **Provider status** provides information on hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="6ac25-145">**Kretsen status** innehåller hello tillstånd om hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="6ac25-145">**Circuit status** provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="6ac25-146">Mer information om krets etablering tillstånd finns hello [arbetsflöden](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.</span><span class="sxs-lookup"><span data-stu-id="6ac25-146">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="6ac25-147">När du skapar en ny ExpressRoute-krets blir hello krets i hello följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="6ac25-147">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

<span data-ttu-id="6ac25-148">Providerstatus: inte etablerats</span><span class="sxs-lookup"><span data-stu-id="6ac25-148">Provider status: Not provisioned</span></span><BR>
<span data-ttu-id="6ac25-149">Kretsen status: aktiverat</span><span class="sxs-lookup"><span data-stu-id="6ac25-149">Circuit status: Enabled</span></span>

![Initiera etableringsprocessen](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

<span data-ttu-id="6ac25-151">hello krets ändras toohello efter tillstånd när hello anslutningen leverantören är i hello innebär att aktivera den åt dig:</span><span class="sxs-lookup"><span data-stu-id="6ac25-151">hello circuit will change toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

<span data-ttu-id="6ac25-152">Providerstatus: etablering</span><span class="sxs-lookup"><span data-stu-id="6ac25-152">Provider status: Provisioning</span></span><BR>
<span data-ttu-id="6ac25-153">Kretsen status: aktiverat</span><span class="sxs-lookup"><span data-stu-id="6ac25-153">Circuit status: Enabled</span></span>

<span data-ttu-id="6ac25-154">Om du toobe kan toouse en ExpressRoute-krets, måste den finnas i hello följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="6ac25-154">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

<span data-ttu-id="6ac25-155">Providerstatus: etablerats</span><span class="sxs-lookup"><span data-stu-id="6ac25-155">Provider status: Provisioned</span></span><BR>
<span data-ttu-id="6ac25-156">Kretsen status: aktiverat</span><span class="sxs-lookup"><span data-stu-id="6ac25-156">Circuit status: Enabled</span></span>

### <a name="5-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="6ac25-157">5. Kontrollera regelbundet hello status och hello tillståndet för hello krets nyckel</span><span class="sxs-lookup"><span data-stu-id="6ac25-157">5. Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="6ac25-158">Du kan visa hello egenskaper för hello kretsen som du är intresserad av genom att välja den.</span><span class="sxs-lookup"><span data-stu-id="6ac25-158">You can view hello properties of hello circuit that you're interested in by selecting it.</span></span> <span data-ttu-id="6ac25-159">Kontrollera hello **Providerstatus** och se till att den har flyttats för**etablerad** innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="6ac25-159">Check hello **Provider status** and ensure that it has moved too**Provisioned** before you continue.</span></span>

![Kretsen och providern status](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a><span data-ttu-id="6ac25-161">6. Skapa din routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="6ac25-161">6. Create your routing configuration</span></span>
<span data-ttu-id="6ac25-162">Stegvisa instruktioner finns i toohello [ExpressRoute-krets routningskonfiguration](expressroute-howto-routing-portal-resource-manager.md) artikel toocreate och ändra krets peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="6ac25-162">For step-by-step instructions, refer toohello [ExpressRoute circuit routing configuration](expressroute-howto-routing-portal-resource-manager.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ac25-163">Dessa anvisningar gäller endast toocircuits som skapas med leverantörer som erbjuder layer 2 tjänster.</span><span class="sxs-lookup"><span data-stu-id="6ac25-163">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="6ac25-164">Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.</span><span class="sxs-lookup"><span data-stu-id="6ac25-164">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="7-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="6ac25-165">7. Länka ett virtuellt nätverk tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-165">7. Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="6ac25-166">Därefter länka ett virtuellt nätverk tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6ac25-166">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="6ac25-167">Använd hello [länka virtuella nätverk tooExpressRoute kretsar](expressroute-howto-linkvnet-arm.md) artikeln när du arbetar med hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="6ac25-167">Use hello [Linking virtual networks tooExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with hello Resource Manager deployment model.</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="6ac25-168">Hämta hello status för en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-168">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="6ac25-169">Du kan visa hello status för en krets genom att välja den.</span><span class="sxs-lookup"><span data-stu-id="6ac25-169">You can view hello status of a circuit by selecting it.</span></span> 

![Status för en ExpressRoute-krets](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="6ac25-171">Ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-171">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="6ac25-172">Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6ac25-172">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="6ac25-173">Du kan göra hello följande utan avbrott:</span><span class="sxs-lookup"><span data-stu-id="6ac25-173">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="6ac25-174">Aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6ac25-174">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="6ac25-175">Öka hello bandbredden för ExpressRoute-krets såvida det inte finns tillgänglig kapacitet på hello port.</span><span class="sxs-lookup"><span data-stu-id="6ac25-175">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="6ac25-176">Observera att nedgradera hello bandbredden för en krets inte stöds.</span><span class="sxs-lookup"><span data-stu-id="6ac25-176">Note that downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="6ac25-177">Ändra hello avläsning plan från förbrukade Data tooUnlimited Data.</span><span class="sxs-lookup"><span data-stu-id="6ac25-177">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="6ac25-178">Observera att ändra hello avläsning plan från obegränsad tooMetered Data inte stöds.</span><span class="sxs-lookup"><span data-stu-id="6ac25-178">Note that changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="6ac25-179">Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.</span><span class="sxs-lookup"><span data-stu-id="6ac25-179">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="6ac25-180">Mer information om gränser och begränsningar finns i toohello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="6ac25-180">For more information on limits and limitations, refer toohello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

<span data-ttu-id="6ac25-181">toomodify en ExpressRoute-krets, klicka på hello **Configuration** enligt hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="6ac25-181">toomodify an ExpressRoute circuit, click on hello **Configuration** as shown in hello figure below.</span></span>

![Ändra krets](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

<span data-ttu-id="6ac25-183">Du kan ändra hello bandbredd, SKU och fakturering modellen och tillåter klassiska åtgärder i hello configuration bladet.</span><span class="sxs-lookup"><span data-stu-id="6ac25-183">You can modify hello bandwidth, SKU, billing model and allow classic operations within hello configuration blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ac25-184">Du kan ha toorecreate hello ExpressRoute-krets om det finns för lite kapacitet på befintlig hello-port.</span><span class="sxs-lookup"><span data-stu-id="6ac25-184">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="6ac25-185">Du kan inte uppgradera hello krets om det finns inga ytterligare kapacitet på den platsen.</span><span class="sxs-lookup"><span data-stu-id="6ac25-185">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="6ac25-186">Du kan minska hello bandbredden för en ExpressRoute-krets utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="6ac25-186">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="6ac25-187">Nedgradera bandbredd kräver toodeprovision hello ExpressRoute-krets och etablera en ny ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="6ac25-187">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> <span data-ttu-id="6ac25-188">Inaktivera premium-tillägg kan misslyckas om du använder resurser som är större än vad som tillåts för standard hello-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6ac25-188">Disable premium add-on operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="6ac25-189">Borttagning och tar bort en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-189">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="6ac25-190">Du kan ta bort ExpressRoute-krets genom att välja hello **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="6ac25-190">You can delete your ExpressRoute circuit by selecting hello **delete** icon.</span></span> <span data-ttu-id="6ac25-191">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="6ac25-191">Note hello following:</span></span>

* <span data-ttu-id="6ac25-192">Du måste ta bort länken alla virtuella nätverk från hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6ac25-192">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="6ac25-193">Om den här åtgärden misslyckas, kontrollera om något virtuellt nätverk är kopplade toohello krets.</span><span class="sxs-lookup"><span data-stu-id="6ac25-193">If this operation fails, check whether any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="6ac25-194">Om hello ExpressRoute-kretsen tjänstleverantör Etableringsstatus **etablering** eller **etablerad** du måste arbeta med service provider toodeprovision hello kretsen på sidan.</span><span class="sxs-lookup"><span data-stu-id="6ac25-194">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="6ac25-195">Kommer att fortsätta tooreserve resurser och debitera dig tills hello-leverantör har slutförts avställningsskript hello-krets och meddela oss.</span><span class="sxs-lookup"><span data-stu-id="6ac25-195">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="6ac25-196">Om hello-leverantör har avetableras hello krets (hello-leverantör Etableringsstatus har angetts för**inte etablerats**) du kan sedan ta bort hello krets.</span><span class="sxs-lookup"><span data-stu-id="6ac25-196">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="6ac25-197">Detta förhindrar att faktureringen för hello-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-197">This will stop billing for hello circuit</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ac25-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ac25-198">Next steps</span></span>
<span data-ttu-id="6ac25-199">Kontrollera att du hello följande när du har skapat kretsen:</span><span class="sxs-lookup"><span data-stu-id="6ac25-199">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="6ac25-200">Skapa och ändra routning för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-200">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-portal-resource-manager.md)
* [<span data-ttu-id="6ac25-201">Länka ditt virtuella nätverk tooyour ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6ac25-201">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

