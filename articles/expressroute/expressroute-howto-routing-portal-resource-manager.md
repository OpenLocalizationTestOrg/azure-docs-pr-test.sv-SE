---
title: "Hur tooconfigure routning (peering) för en ExpressRoute-krets: hanteraren för filserverresurser: Azure | Microsoft Docs"
description: "Den här artikeln vägleder dig genom hello steg för att skapa och etablera hello private, offentlig och Microsoft-peering i ExpressRoute-kretsen. Den här artikeln beskriver också hur toocheck hello status, uppdatera eller ta bort peerkopplingar för kretsen."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: a8ca25f488dde728cb3b06cd2c91873f3069feaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a><span data-ttu-id="7638d-104">Skapa och ändra peering för en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7638d-104">Create and modify peering for an ExpressRoute circuit</span></span>

<span data-ttu-id="7638d-105">Den här artikeln hjälper dig att skapa och hantera routningskonfiguration för en ExpressRoute-krets i hello Resource Manager-distributionsmodellen med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7638d-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using hello Azure portal.</span></span> <span data-ttu-id="7638d-106">Du kan också kontrollera hello status, uppdatera eller ta bort och ta bort etableringen av peerkopplingar för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="7638d-107">Om du vill toouse en annan metod toowork med kretsen, väljer du en artikel från hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="7638d-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>


## <a name="configuration-prerequisites"></a><span data-ttu-id="7638d-108">Förutsättningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="7638d-108">Configuration prerequisites</span></span>

* <span data-ttu-id="7638d-109">Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md) sidan hello [routningskrav](expressroute-routing.md) sida och hello [arbetsflöden](expressroute-workflows.md) sidan innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="7638d-109">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="7638d-110">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-110">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="7638d-111">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="7638d-111">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="7638d-112">Hej ExpressRoute-krets måste vara i ett etablerad och aktiverade tillstånd du toobe kan toorun hello-cmdlets i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7638d-112">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets in hello next sections.</span></span>
* <span data-ttu-id="7638d-113">Om du planerar toouse en delad nyckel/MD5-hash, vara säker på att toouse detta på båda sidor av hello tunnel och gränsen hello antal tecken tooa högst 25.</span><span class="sxs-lookup"><span data-stu-id="7638d-113">If you plan toouse a shared key/MD5 hash, be sure toouse this on both sides of hello tunnel and limit hello number of characters tooa maximum of 25.</span></span>

<span data-ttu-id="7638d-114">Dessa anvisningar gäller endast toocircuits som skapats med leverantörer som erbjuder tjänster nivå 2.</span><span class="sxs-lookup"><span data-stu-id="7638d-114">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="7638d-115">Om du använder en tjänstprovider som erbjuder hanterade Layer 3-tjänster (vanligtvis en IPVPN, t.ex. MPLS), anslutningsleverantören konfigurerar och hanterar routning för dig.</span><span class="sxs-lookup"><span data-stu-id="7638d-115">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="7638d-116">Vi annonseras för närvarande inte peerkopplingar som konfigurerats av leverantörer via hello service management portal.</span><span class="sxs-lookup"><span data-stu-id="7638d-116">We currently do not advertise peerings configured by service providers through hello service management portal.</span></span> <span data-ttu-id="7638d-117">Vi arbetar på att kunna aktivera den här funktionen snart.</span><span class="sxs-lookup"><span data-stu-id="7638d-117">We are working on enabling this capability soon.</span></span> <span data-ttu-id="7638d-118">Kontrollera med leverantören innan du konfigurerar BGP peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="7638d-118">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="7638d-119">Du kan konfigurera en, två eller alla tre peerings (Azure privat, Azure offentlig och Microsoft) för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-119">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="7638d-120">Du kan konfigurera peerings i valfri ordning.</span><span class="sxs-lookup"><span data-stu-id="7638d-120">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="7638d-121">Du måste dock se till att du har slutfört hello konfigurationen för varje peering en i taget.</span><span class="sxs-lookup"><span data-stu-id="7638d-121">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="7638d-122">Azures privata peering</span><span class="sxs-lookup"><span data-stu-id="7638d-122">Azure private peering</span></span>

<span data-ttu-id="7638d-123">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure privat peering-konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-123">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="7638d-124">toocreate privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="7638d-124">toocreate Azure private peering</span></span>

1. <span data-ttu-id="7638d-125">Konfigurera hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="7638d-125">Configure hello ExpressRoute circuit.</span></span> <span data-ttu-id="7638d-126">Se till att hello kretsen är helt etablerad av hello anslutningen providern innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="7638d-126">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing.</span></span>

  ![lista](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="7638d-128">Konfigurera privat Azure-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-128">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="7638d-129">Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:</span><span class="sxs-lookup"><span data-stu-id="7638d-129">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="7638d-130">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="7638d-130">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="7638d-131">hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="7638d-131">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="7638d-132">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="7638d-132">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="7638d-133">hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="7638d-133">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="7638d-134">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="7638d-134">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="7638d-135">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="7638d-135">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="7638d-136">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-136">AS number for peering.</span></span> <span data-ttu-id="7638d-137">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="7638d-137">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="7638d-138">Du kan använda ett privat AS-tal för den här peeringen.</span><span class="sxs-lookup"><span data-stu-id="7638d-138">You can use a private AS number for this peering.</span></span> <span data-ttu-id="7638d-139">Se till att du inte använder 65515.</span><span class="sxs-lookup"><span data-stu-id="7638d-139">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="7638d-140">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="7638d-140">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="7638d-141">Välj hello Azure privat peering-raden som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="7638d-141">Select hello Azure Private peering row, as shown in hello following example:</span></span>

  ![privata](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. <span data-ttu-id="7638d-143">Konfigurera privat peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-143">Configure private peering.</span></span> <span data-ttu-id="7638d-144">hello följande bild visar ett Konfigurationsexempel på:</span><span class="sxs-lookup"><span data-stu-id="7638d-144">hello following image shows a configuration example:</span></span>

  ![Konfigurera privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. <span data-ttu-id="7638d-146">Spara hello konfigurationen när du har angett alla parametrar.</span><span class="sxs-lookup"><span data-stu-id="7638d-146">Save hello configuration once you have specified all parameters.</span></span> <span data-ttu-id="7638d-147">När hello konfiguration har tagits emot, kan du se något liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="7638d-147">After hello configuration has been accepted successfully, you see something similar toohello following example:</span></span>

  ![Spara privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="7638d-149">tooview Azure privat peering information</span><span class="sxs-lookup"><span data-stu-id="7638d-149">tooview Azure private peering details</span></span>

<span data-ttu-id="7638d-150">Du kan visa hello egenskaper för privat Azure-peering genom att välja hello peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-150">You can view hello properties of Azure private peering by selecting hello peering.</span></span>

![Visa privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="7638d-152">tooupdate Azure privat peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="7638d-152">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="7638d-153">Du kan välja hello raden för peering och ändra hello peering egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7638d-153">You can select hello row for peering and modify hello peering properties.</span></span>

![Uppdatera privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="7638d-155">toodelete privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="7638d-155">toodelete Azure private peering</span></span>

<span data-ttu-id="7638d-156">Du kan ta bort peering konfigurationen genom att välja ikonen Ta bort hello enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="7638d-156">You can remove your peering configuration by selecting hello delete icon, as shown in hello following image:</span></span>

![ta bort privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a><span data-ttu-id="7638d-158">Azures offentliga peering</span><span class="sxs-lookup"><span data-stu-id="7638d-158">Azure public peering</span></span>

<span data-ttu-id="7638d-159">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure offentlig peering-konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-159">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="7638d-160">toocreate offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="7638d-160">toocreate Azure public peering</span></span>

1. <span data-ttu-id="7638d-161">Konfigurera ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="7638d-161">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="7638d-162">Se till att hello kretsen är helt etablerad av hello anslutningen providern innan du fortsätter ytterligare.</span><span class="sxs-lookup"><span data-stu-id="7638d-162">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing further.</span></span>

  ![Visa en lista med offentlig peering](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="7638d-164">Konfigurera offentlig Azure-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-164">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="7638d-165">Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:</span><span class="sxs-lookup"><span data-stu-id="7638d-165">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="7638d-166">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="7638d-166">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="7638d-167">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="7638d-167">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="7638d-168">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="7638d-168">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="7638d-169">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="7638d-169">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="7638d-170">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="7638d-170">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="7638d-171">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="7638d-171">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="7638d-172">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-172">AS number for peering.</span></span> <span data-ttu-id="7638d-173">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="7638d-173">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="7638d-174">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="7638d-174">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="7638d-175">Välj hello Azure offentlig peering-raden som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="7638d-175">Select hello Azure public peering row, as shown in hello following image:</span></span>

  ![Välj offentlig peering rad](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. <span data-ttu-id="7638d-177">Konfigurera offentlig peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-177">Configure public peering.</span></span> <span data-ttu-id="7638d-178">hello följande bild visar ett Konfigurationsexempel på:</span><span class="sxs-lookup"><span data-stu-id="7638d-178">hello following image shows a configuration example:</span></span>

  ![Konfigurera offentlig peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. <span data-ttu-id="7638d-180">Spara hello konfigurationen när du har angett alla parametrar.</span><span class="sxs-lookup"><span data-stu-id="7638d-180">Save hello configuration once you have specified all parameters.</span></span> <span data-ttu-id="7638d-181">När hello konfiguration har tagits emot, kan du se något liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="7638d-181">After hello configuration has been accepted successfully, you see something similar toohello following example:</span></span>

  ![Spara offentlig peering konfiguration](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="7638d-183">tooview Azure offentlig peering information</span><span class="sxs-lookup"><span data-stu-id="7638d-183">tooview Azure public peering details</span></span>

<span data-ttu-id="7638d-184">Du kan visa hello egenskaper för offentlig Azure-peering genom att välja hello peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-184">You can view hello properties of Azure public peering by selecting hello peering.</span></span>

![Visa offentlig peering egenskaper](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="7638d-186">tooupdate Azure offentlig peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="7638d-186">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="7638d-187">Du kan välja hello raden för peering och ändra hello peering egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7638d-187">You can select hello row for peering and modify hello peering properties.</span></span>

![Välj offentlig peering rad](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="7638d-189">toodelete offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="7638d-189">toodelete Azure public peering</span></span>

<span data-ttu-id="7638d-190">Du kan ta bort peering konfigurationen genom att välja ikonen Ta bort hello som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="7638d-190">You can remove your peering configuration by selecting hello delete icon, as shown in hello following example:</span></span>

![ta bort offentlig peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a><span data-ttu-id="7638d-192">Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="7638d-192">Microsoft peering</span></span>

<span data-ttu-id="7638d-193">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Microsoft peering konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-193">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7638d-194">Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft hello-peering, även om filter routning inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="7638d-194">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="7638d-195">Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-195">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="7638d-196">Mer information finns i [konfigurera route filter för Microsoft-peering](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7638d-196">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="7638d-197">toocreate Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="7638d-197">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="7638d-198">Konfigurera ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="7638d-198">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="7638d-199">Se till att hello kretsen är helt etablerad av hello anslutningen providern innan du fortsätter ytterligare.</span><span class="sxs-lookup"><span data-stu-id="7638d-199">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing further.</span></span>

  ![Visa en lista över Microsoft-peering](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="7638d-201">Konfigurera Microsoft-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="7638d-201">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="7638d-202">Kontrollera att du har hello följande information innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="7638d-202">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="7638d-203">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="7638d-203">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="7638d-204">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="7638d-204">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="7638d-205">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="7638d-205">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="7638d-206">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="7638d-206">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="7638d-207">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="7638d-207">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="7638d-208">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="7638d-208">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="7638d-209">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-209">AS number for peering.</span></span> <span data-ttu-id="7638d-210">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="7638d-210">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="7638d-211">Annonserade prefix: du måste ange en lista över alla prefix du planerar tooadvertise över hello BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="7638d-211">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="7638d-212">Endast offentliga IP-adressprefix accepteras.</span><span class="sxs-lookup"><span data-stu-id="7638d-212">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="7638d-213">Om du planerar toosend en uppsättning prefix, kan du skicka en kommaavgränsad lista.</span><span class="sxs-lookup"><span data-stu-id="7638d-213">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="7638d-214">Dessa prefix måste vara registrerade tooyou i en RIR / IRR.</span><span class="sxs-lookup"><span data-stu-id="7638d-214">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="7638d-215">**Valfritt -** kunden ASN: Om du är reklam prefix som inte är registrerade toohello peering som du kan ange hello som antalet toowhich som de har registrerats.</span><span class="sxs-lookup"><span data-stu-id="7638d-215">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="7638d-216">Routning namn i registret: Du kan ange hello RIR / IRR mot vilken hello som antalet och prefix har registrerats.</span><span class="sxs-lookup"><span data-stu-id="7638d-216">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="7638d-217">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="7638d-217">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="7638d-218">Du kan välja hello peering gärna tooconfigure, enligt hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="7638d-218">You can select hello peering you wish tooconfigure, as shown in hello following example.</span></span> <span data-ttu-id="7638d-219">Välj hello Microsoft peering rad.</span><span class="sxs-lookup"><span data-stu-id="7638d-219">Select hello Microsoft peering row.</span></span>

  ![Välj Microsoft peering rad](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. <span data-ttu-id="7638d-221">Konfigurera Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-221">Configure Microsoft peering.</span></span> <span data-ttu-id="7638d-222">hello följande bild visar ett Konfigurationsexempel på:</span><span class="sxs-lookup"><span data-stu-id="7638d-222">hello following image shows a configuration example:</span></span>

  ![Konfigurera Microsoft-peering](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. <span data-ttu-id="7638d-224">Spara hello konfigurationen när du har angett alla parametrar.</span><span class="sxs-lookup"><span data-stu-id="7638d-224">Save hello configuration once you have specified all parameters.</span></span>

  <span data-ttu-id="7638d-225">Om kretsen hämtar tooa 'Validation behövs-tillstånd (enligt hello bild), måste du öppna en stöd biljett tooshow bevis av äganderätten till hello prefix tooour supportteamet.</span><span class="sxs-lookup"><span data-stu-id="7638d-225">If your circuit gets tooa 'Validation needed' state (as shown in hello image), you must open a support ticket tooshow proof of ownership of hello prefixes tooour support team.</span></span>

  ![Spara konfigurationen för Microsoft peering](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  <span data-ttu-id="7638d-227">Du kan öppna ett supportärende direkt från hello-portalen, enligt följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="7638d-227">You can open a support ticket directly from hello portal, as shown in hello following example:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. <span data-ttu-id="7638d-228">När hello konfiguration har tagits emot, kan du se något liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="7638d-228">After hello configuration has been accepted successfully, you see something similar toohello following image:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="tooview-microsoft-peering-details"></a><span data-ttu-id="7638d-229">tooview Microsoft peering information</span><span class="sxs-lookup"><span data-stu-id="7638d-229">tooview Microsoft peering details</span></span>

<span data-ttu-id="7638d-230">Du kan visa hello egenskaper för offentlig Azure-peering genom att välja hello peering.</span><span class="sxs-lookup"><span data-stu-id="7638d-230">You can view hello properties of Azure public peering by selecting hello peering.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="7638d-231">tooupdate Microsoft peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="7638d-231">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="7638d-232">Du kan välja hello raden för peering och ändra hello peering egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7638d-232">You can select hello row for peering and modify hello peering properties.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="7638d-233">toodelete Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="7638d-233">toodelete Microsoft peering</span></span>

<span data-ttu-id="7638d-234">Du kan ta bort peering konfigurationen genom att välja ikonen Ta bort hello enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="7638d-234">You can remove your peering configuration by selecting hello delete icon, as shown in hello following image:</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a><span data-ttu-id="7638d-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7638d-235">Next steps</span></span>

<span data-ttu-id="7638d-236">Nästa steg [länka VNet-tooan ExpressRoute-krets](expressroute-howto-linkvnet-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="7638d-236">Next step, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-portal-resource-manager.md)</span></span>
* <span data-ttu-id="7638d-237">Mer information om ExpressRoute-arbetsflöden finns i [ExpressRoute-arbetsflöden](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="7638d-237">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="7638d-238">Mer information om krets-peering finns i [ExpressRoute-kretsar och routningsdomäner](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="7638d-238">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="7638d-239">Mer information om hur du arbetar med virtuella nätverk finns i [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7638d-239">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
