---
title: "Skapa och ändra en ExpressRoute-krets: Azure-portalen | Microsoft Docs"
description: "Den här artikeln beskriver hur du skapar, etablera, verifiera, uppdatera, ta bort och ta bort etableringen av en ExpressRoute-krets."
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
ms.openlocfilehash: e3721cd3c031622788f553e71a6555b844f3f7dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a><span data-ttu-id="b897a-103">Skapa och ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-103">Create and modify an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b897a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b897a-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="b897a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b897a-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="b897a-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b897a-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="b897a-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b897a-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="b897a-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="b897a-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="b897a-109">Den här artikeln beskriver hur du skapar en Azure ExpressRoute-krets med hjälp av Azure-portalen och Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b897a-109">This article describes how to create an Azure ExpressRoute circuit by using the Azure portal and the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="b897a-110">Följande steg visar även hur du kontrollerar statusen för kretsen, uppdatera eller ta bort och ta bort etableringen av den.</span><span class="sxs-lookup"><span data-stu-id="b897a-110">The following steps also show you how to check the status of the circuit, update it, or delete and deprovision it.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="b897a-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b897a-111">Before you begin</span></span>
* <span data-ttu-id="b897a-112">Granska de [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="b897a-112">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="b897a-113">Kontrollera att du har åtkomst till den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b897a-113">Ensure that you have access to the [Azure portal](https://portal.azure.com).</span></span>
* <span data-ttu-id="b897a-114">Se till att du har behörighet att skapa nya nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="b897a-114">Ensure that you have permissions to create new networking resources.</span></span> <span data-ttu-id="b897a-115">Kontakta din kontoadministratör om du inte har rätt behörigheter.</span><span class="sxs-lookup"><span data-stu-id="b897a-115">Contact your account administrator if you do not have the right permissions.</span></span>
* <span data-ttu-id="b897a-116">Du kan [visa en video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) innan du påbörjar för att bättre förstå steg.</span><span class="sxs-lookup"><span data-stu-id="b897a-116">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) before beginning in order to better understand the steps.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="b897a-117">Skapa och etablera en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-117">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-to-the-azure-portal"></a><span data-ttu-id="b897a-118">1. Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b897a-118">1. Sign in to the Azure portal</span></span>
<span data-ttu-id="b897a-119">Öppna en webbläsare, navigera till [Azure Portal](http://portal.azure.com) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="b897a-119">From a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="2-create-a-new-expressroute-circuit"></a><span data-ttu-id="b897a-120">2. Skapa en ny ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-120">2. Create a new ExpressRoute circuit</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b897a-121">ExpressRoute-krets kommer att debiteras från den tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="b897a-121">Your ExpressRoute circuit will be billed from the moment a service key is issued.</span></span> <span data-ttu-id="b897a-122">Se till att utföra den här åtgärden när anslutningen providern är redo att etablera kretsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-122">Ensure that you perform this operation when the connectivity provider is ready to provision the circuit.</span></span>
> 
> 

1. <span data-ttu-id="b897a-123">Du kan skapa en ExpressRoute-krets genom att välja alternativet för att skapa en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="b897a-123">You can create an ExpressRoute circuit by selecting the option to create a new resource.</span></span> <span data-ttu-id="b897a-124">Klicka på **ny** > **nätverk** > **ExpressRoute**, enligt följande bild:</span><span class="sxs-lookup"><span data-stu-id="b897a-124">Click **New** > **Networking** > **ExpressRoute**, as shown in the following image:</span></span>
   
    ![Skapa en ExpressRoute-krets](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. <span data-ttu-id="b897a-126">När du klickar på **ExpressRoute**, ser du den **skapa ExpressRoute-krets** bladet.</span><span class="sxs-lookup"><span data-stu-id="b897a-126">After you click **ExpressRoute**, you'll see the **Create ExpressRoute circuit** blade.</span></span> <span data-ttu-id="b897a-127">När du fyller i värden i det här bladet kan du kontrollera att du anger rätt SKU-nivå och datamätning.</span><span class="sxs-lookup"><span data-stu-id="b897a-127">When you're filling in the values on this blade, make sure that you specify the correct SKU tier and data metering.</span></span>
   
   * <span data-ttu-id="b897a-128">**Nivån** bestämmer om en ExpressRoute-standard eller premium-tillägg ett ExpressRoute är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="b897a-128">**Tier** determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="b897a-129">Du kan ange **Standard** att hämta standard SKU: N eller **Premium** för premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="b897a-129">You can specify **Standard** to get the standard SKU or **Premium** for the premium add-on.</span></span>
   * <span data-ttu-id="b897a-130">**Datamätning** avgör vilken faktureringsinformation.</span><span class="sxs-lookup"><span data-stu-id="b897a-130">**Data metering** determines the billing type.</span></span> <span data-ttu-id="b897a-131">Du kan ange **Metered** för en plan för förbrukade data och **obegränsad** för en obegränsad plan.</span><span class="sxs-lookup"><span data-stu-id="b897a-131">You can specify **Metered** for a metered data plan and **Unlimited** for an unlimited data plan.</span></span> <span data-ttu-id="b897a-132">Observera att du kan ändra fakturering typen från **Metered** till **obegränsad**, men du kan inte ändra typen från **obegränsad** till **Metered**.</span><span class="sxs-lookup"><span data-stu-id="b897a-132">Note that you can change the billing type from **Metered** to **Unlimited**, but you can't change the type from **Unlimited** to **Metered**.</span></span>
     
     ![Konfigurera SKU-nivå och datamätning](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> <span data-ttu-id="b897a-134">Tänk på att Peering-platsen visar den [fysisk plats](expressroute-locations.md) där du peering med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b897a-134">Please be aware that the Peering Location indicates the [physical location](expressroute-locations.md) where you are peering with Microsoft.</span></span> <span data-ttu-id="b897a-135">Detta är **inte** kopplad till ”plats”-egenskap som refererar till geografi där Azure Nätverksresursprovidern finns.</span><span class="sxs-lookup"><span data-stu-id="b897a-135">This is **not** linked to "Location" property, which refers to the geography where the Azure Network Resource Provider is located.</span></span> <span data-ttu-id="b897a-136">När de inte är relaterade är det en bra idé att välja en Nätverksresursprovidern geografiskt nära Peering-platsen för kretsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-136">While they are not related, it is a good practice to choose a Network Resource Provider geographically close to the Peering Location of the circuit.</span></span> 
> 
> 

### <a name="3-view-the-circuits-and-properties"></a><span data-ttu-id="b897a-137">3. Visa kretsar och egenskaper</span><span class="sxs-lookup"><span data-stu-id="b897a-137">3. View the circuits and properties</span></span>
<span data-ttu-id="b897a-138">**Visa alla kretsar**</span><span class="sxs-lookup"><span data-stu-id="b897a-138">**View all the circuits**</span></span>

<span data-ttu-id="b897a-139">Du kan visa alla kretsar som du skapade genom att välja **alla resurser** på den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="b897a-139">You can view all the circuits that you created by selecting **All resources** on the left-side menu.</span></span>

![Visa kretsar](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

<span data-ttu-id="b897a-141">**Visa egenskaper**</span><span class="sxs-lookup"><span data-stu-id="b897a-141">**View the properties**</span></span>

    You can view the properties of the circuit by selecting it. On this blade, note the service key for the circuit. You must copy the circuit key for your circuit and pass it down to the service provider to complete the provisioning process. The circuit key is specific to your circuit.

![Visa egenskaper](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a><span data-ttu-id="b897a-143">4. Skicka tjänstnyckeln till anslutningsleverantören för etablering</span><span class="sxs-lookup"><span data-stu-id="b897a-143">4. Send the service key to your connectivity provider for provisioning</span></span>
<span data-ttu-id="b897a-144">På det här bladet **Providerstatus** innehåller information om det aktuella tillståndet för etablering på sida-leverantör.</span><span class="sxs-lookup"><span data-stu-id="b897a-144">On this blade, **Provider status** provides information on the current state of provisioning on the service-provider side.</span></span> <span data-ttu-id="b897a-145">**Kretsen status** ger tillståndet på Microsoft-sida.</span><span class="sxs-lookup"><span data-stu-id="b897a-145">**Circuit status** provides the state on the Microsoft side.</span></span> <span data-ttu-id="b897a-146">Mer information om krets etablering tillstånd finns det [arbetsflöden](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.</span><span class="sxs-lookup"><span data-stu-id="b897a-146">For more information about circuit provisioning states, see the [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="b897a-147">När du skapar en ny ExpressRoute-krets blir kretsen i följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="b897a-147">When you create a new ExpressRoute circuit, the circuit will be in the following state:</span></span>

<span data-ttu-id="b897a-148">Providerstatus: inte etablerats</span><span class="sxs-lookup"><span data-stu-id="b897a-148">Provider status: Not provisioned</span></span><BR>
<span data-ttu-id="b897a-149">Kretsen status: aktiverat</span><span class="sxs-lookup"><span data-stu-id="b897a-149">Circuit status: Enabled</span></span>

![Initiera etableringsprocessen](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

<span data-ttu-id="b897a-151">Kretsen ändras till följande tillstånd när anslutningen providern håller på att du:</span><span class="sxs-lookup"><span data-stu-id="b897a-151">The circuit will change to the following state when the connectivity provider is in the process of enabling it for you:</span></span>

<span data-ttu-id="b897a-152">Providerstatus: etablering</span><span class="sxs-lookup"><span data-stu-id="b897a-152">Provider status: Provisioning</span></span><BR>
<span data-ttu-id="b897a-153">Kretsen status: aktiverat</span><span class="sxs-lookup"><span data-stu-id="b897a-153">Circuit status: Enabled</span></span>

<span data-ttu-id="b897a-154">Du kan använda en ExpressRoute-krets, måste den vara i följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="b897a-154">For you to be able to use an ExpressRoute circuit, it must be in the following state:</span></span>

<span data-ttu-id="b897a-155">Providerstatus: etablerats</span><span class="sxs-lookup"><span data-stu-id="b897a-155">Provider status: Provisioned</span></span><BR>
<span data-ttu-id="b897a-156">Kretsen status: aktiverat</span><span class="sxs-lookup"><span data-stu-id="b897a-156">Circuit status: Enabled</span></span>

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a><span data-ttu-id="b897a-157">5. Regelbundet kontrollera status och tillståndet för nyckeln krets</span><span class="sxs-lookup"><span data-stu-id="b897a-157">5. Periodically check the status and the state of the circuit key</span></span>
<span data-ttu-id="b897a-158">Du kan visa egenskaperna för den krets du är intresserad av genom att välja den.</span><span class="sxs-lookup"><span data-stu-id="b897a-158">You can view the properties of the circuit that you're interested in by selecting it.</span></span> <span data-ttu-id="b897a-159">Kontrollera den **Providerstatus** och se till att den har flyttats till **etablerad** innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="b897a-159">Check the **Provider status** and ensure that it has moved to **Provisioned** before you continue.</span></span>

![Kretsen och providern status](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a><span data-ttu-id="b897a-161">6. Skapa din routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="b897a-161">6. Create your routing configuration</span></span>
<span data-ttu-id="b897a-162">Stegvisa anvisningar finns i den [ExpressRoute-krets routningskonfiguration](expressroute-howto-routing-portal-resource-manager.md) artikeln om du vill skapa och ändra krets peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="b897a-162">For step-by-step instructions, refer to the [ExpressRoute circuit routing configuration](expressroute-howto-routing-portal-resource-manager.md) article to create and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b897a-163">Dessa anvisningar gäller endast för kretsar som skapats med leverantörer som erbjuder layer 2 tjänster.</span><span class="sxs-lookup"><span data-stu-id="b897a-163">These instructions only apply to circuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="b897a-164">Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.</span><span class="sxs-lookup"><span data-stu-id="b897a-164">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="b897a-165">7. Länka ett virtuellt nätverk till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-165">7. Link a virtual network to an ExpressRoute circuit</span></span>
<span data-ttu-id="b897a-166">Därefter länka ett virtuellt nätverk till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-166">Next, link a virtual network to your ExpressRoute circuit.</span></span> <span data-ttu-id="b897a-167">Använd den [länka virtuella nätverk för ExpressRoute-kretsar](expressroute-howto-linkvnet-arm.md) artikeln när du arbetar med Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b897a-167">Use the [Linking virtual networks to ExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with the Resource Manager deployment model.</span></span>

## <a name="getting-the-status-of-an-expressroute-circuit"></a><span data-ttu-id="b897a-168">Hämta status för en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-168">Getting the status of an ExpressRoute circuit</span></span>
<span data-ttu-id="b897a-169">Du kan visa statusen för en krets genom att välja den.</span><span class="sxs-lookup"><span data-stu-id="b897a-169">You can view the status of a circuit by selecting it.</span></span> 

![Status för en ExpressRoute-krets](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="b897a-171">Ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-171">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="b897a-172">Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.</span><span class="sxs-lookup"><span data-stu-id="b897a-172">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="b897a-173">Du kan göra följande med utan avbrott:</span><span class="sxs-lookup"><span data-stu-id="b897a-173">You can do the following with no downtime:</span></span>

* <span data-ttu-id="b897a-174">Aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-174">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="b897a-175">Öka bandbredden för ExpressRoute-krets under förutsättning att det finns tillgänglig kapacitet på porten.</span><span class="sxs-lookup"><span data-stu-id="b897a-175">Increase the bandwidth of your ExpressRoute circuit provided there is capacity available on the port.</span></span> <span data-ttu-id="b897a-176">Observera att nedgradera bandbredden för en krets inte stöds.</span><span class="sxs-lookup"><span data-stu-id="b897a-176">Note that downgrading the bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="b897a-177">Ändra avläsning planen från förbrukade Data till obegränsad.</span><span class="sxs-lookup"><span data-stu-id="b897a-177">Change the metering plan from Metered Data to Unlimited Data.</span></span> <span data-ttu-id="b897a-178">Observera att om du ändrar avläsning planen från obegränsad till förbrukade Data inte stöds.</span><span class="sxs-lookup"><span data-stu-id="b897a-178">Note that changing the metering plan from Unlimited Data to Metered Data is not supported.</span></span>
* <span data-ttu-id="b897a-179">Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.</span><span class="sxs-lookup"><span data-stu-id="b897a-179">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="b897a-180">Mer information om gränser och begränsningar finns i den [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="b897a-180">For more information on limits and limitations, refer to the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

<span data-ttu-id="b897a-181">Om du vill ändra en ExpressRoute-krets, klicka på den **Configuration** som visas i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="b897a-181">To modify an ExpressRoute circuit, click on the **Configuration** as shown in the figure below.</span></span>

![Ändra krets](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

<span data-ttu-id="b897a-183">Du kan ändra bandbredd, SKU och fakturering modellen och tillåter klassiska åtgärder i bladet konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b897a-183">You can modify the bandwidth, SKU, billing model and allow classic operations within the configuration blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b897a-184">Du kan behöva återskapa ExpressRoute-kretsen om det finns för lite kapacitet på befintliga porten.</span><span class="sxs-lookup"><span data-stu-id="b897a-184">You may have to recreate the ExpressRoute circuit if there is inadequate capacity on the existing port.</span></span> <span data-ttu-id="b897a-185">Du kan inte uppgradera kretsen om det finns inga ytterligare kapacitet på den platsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-185">You cannot upgrade the circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="b897a-186">Du kan minska bandbredden för en ExpressRoute-krets utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="b897a-186">You cannot reduce the bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="b897a-187">Nedgradera bandbredd måste du ta bort etableringen av ExpressRoute-kretsen och etablera en ny ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="b897a-187">Downgrading bandwidth requires you to deprovision the ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> <span data-ttu-id="b897a-188">Inaktivera premium-tillägg kan misslyckas om du använder resurser som är större än vad som tillåts för standard-kretsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-188">Disable premium add-on operation can fail if you're using resources that are greater than what is permitted for the standard circuit.</span></span>
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="b897a-189">Borttagning och tar bort en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-189">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="b897a-190">Du kan ta bort ExpressRoute-krets genom att välja den **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="b897a-190">You can delete your ExpressRoute circuit by selecting the **delete** icon.</span></span> <span data-ttu-id="b897a-191">Observera följande:</span><span class="sxs-lookup"><span data-stu-id="b897a-191">Note the following:</span></span>

* <span data-ttu-id="b897a-192">Du måste ta bort länken alla virtuella nätverk från ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-192">You must unlink all virtual networks from the ExpressRoute circuit.</span></span> <span data-ttu-id="b897a-193">Om den här åtgärden misslyckas, kan du kontrollera om något virtuellt nätverk är kopplade till kretsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-193">If this operation fails, check whether any virtual networks are linked to the circuit.</span></span>
* <span data-ttu-id="b897a-194">Om leverantören för ExpressRoute-kretsen Etableringsläge är **etablering** eller **etablerad** du måste arbeta med tjänstleverantören för att ta bort etableringen krets på sidan.</span><span class="sxs-lookup"><span data-stu-id="b897a-194">If the ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider to deprovision the circuit on their side.</span></span> <span data-ttu-id="b897a-195">Vi kommer att fortsätta att reservera resurser och debitera dig tills tjänstleverantör har slutförts avetablering kretsen och meddela oss.</span><span class="sxs-lookup"><span data-stu-id="b897a-195">We will continue to reserve resources and bill you until the service provider completes deprovisioning the circuit and notifies us.</span></span>
* <span data-ttu-id="b897a-196">Om tjänstleverantör har avetableras kretsen (Etableringsstatus tjänstleverantör har angetts till **inte etablerats**) du kan sedan ta bort kretsen.</span><span class="sxs-lookup"><span data-stu-id="b897a-196">If the service provider has deprovisioned the circuit (the service provider provisioning state is set to **Not provisioned**) you can then delete the circuit.</span></span> <span data-ttu-id="b897a-197">Detta förhindrar att faktureringen för kretsen</span><span class="sxs-lookup"><span data-stu-id="b897a-197">This will stop billing for the circuit</span></span>

## <a name="next-steps"></a><span data-ttu-id="b897a-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b897a-198">Next steps</span></span>
<span data-ttu-id="b897a-199">När du har skapat kretsen, kontrollera att du gör följande:</span><span class="sxs-lookup"><span data-stu-id="b897a-199">After you create your circuit, make sure that you do the following:</span></span>

* [<span data-ttu-id="b897a-200">Skapa och ändra routning för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-200">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-portal-resource-manager.md)
* [<span data-ttu-id="b897a-201">Länka ditt virtuella nätverk till ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="b897a-201">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

