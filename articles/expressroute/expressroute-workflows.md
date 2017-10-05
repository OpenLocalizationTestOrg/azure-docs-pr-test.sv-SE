---
title: "Arbetsflöden för att konfigurera en ExpressRoute-krets | Microsoft Docs"
description: "Den här sidan vägleder dig genom arbetsflödena för att konfigurera ExpressRoute-krets och peerkopplingar"
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 55e0418c-e0bf-44a7-9aa1-720076df9297
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: cba1b2cfee379e7d2b079bcb3089981ef1044d66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a><span data-ttu-id="cbad8-103">Arbetsflöden i ExpressRoute för kretsetablering och kretstillstånd</span><span class="sxs-lookup"><span data-stu-id="cbad8-103">ExpressRoute workflows for circuit provisioning and circuit states</span></span>
<span data-ttu-id="cbad8-104">Den här sidan vägleder dig genom tjänsten etablering och routning configuration arbetsflöden på en hög nivå.</span><span class="sxs-lookup"><span data-stu-id="cbad8-104">This page walks you through the service provisioning and routing configuration workflows at a high level.</span></span>

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

<span data-ttu-id="cbad8-105">Följande bild och motsvarande steg visar de uppgifter som du måste följa för att få en ExpressRoute-krets etablerats slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="cbad8-105">The following figure and corresponding steps show the tasks you must follow in order to have an ExpressRoute circuit provisioned end-to-end.</span></span> 

1. <span data-ttu-id="cbad8-106">Använd PowerShell för att konfigurera en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="cbad8-106">Use PowerShell to configure an ExpressRoute circuit.</span></span> <span data-ttu-id="cbad8-107">Följ instruktionerna i den [skapa ExpressRoute-kretsar](expressroute-howto-circuit-classic.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="cbad8-107">Follow the instructions in the [Create ExpressRoute circuits](expressroute-howto-circuit-classic.md) article for more details.</span></span>
2. <span data-ttu-id="cbad8-108">Ordning anslutningar från Internet-leverantören.</span><span class="sxs-lookup"><span data-stu-id="cbad8-108">Order connectivity from the service provider.</span></span> <span data-ttu-id="cbad8-109">Den här processen varierar.</span><span class="sxs-lookup"><span data-stu-id="cbad8-109">This process varies.</span></span> <span data-ttu-id="cbad8-110">Kontakta anslutningsleverantören för mer information om hur du beställer anslutningen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-110">Contact your connectivity provider for more details about how to order connectivity.</span></span>
3. <span data-ttu-id="cbad8-111">Kontrollera att kretsen har har etablerats genom att verifiera ExpressRoute-kretsen Etableringsstatus via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cbad8-111">Ensure that the circuit has been provisioned successfully by verifying the ExpressRoute circuit provisioning state through PowerShell.</span></span> 
4. <span data-ttu-id="cbad8-112">Konfigurera routning domäner.</span><span class="sxs-lookup"><span data-stu-id="cbad8-112">Configure routing domains.</span></span> <span data-ttu-id="cbad8-113">Om anslutningsleverantören hanterar Layer 3 åt dig, kommer de att konfigurera routning för kretsen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-113">If your connectivity provider manages Layer 3 for you, they will configure routing for your circuit.</span></span> <span data-ttu-id="cbad8-114">Om anslutningsleverantören erbjuder endast nivå 2-tjänster, måste du konfigurera routning per riktlinjer som beskrivs i den [routningskrav](expressroute-routing.md) och [routningskonfiguration](expressroute-howto-routing-classic.md) sidor.</span><span class="sxs-lookup"><span data-stu-id="cbad8-114">If your connectivity provider only offers Layer 2 services, you must configure routing per guidelines described in the [routing requirements](expressroute-routing.md) and [routing configuration](expressroute-howto-routing-classic.md) pages.</span></span>
   
   * <span data-ttu-id="cbad8-115">Aktivera privat Azure-peering - du måste aktivera den här peering för att ansluta till virtuella datorer / molntjänster distribueras inom virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="cbad8-115">Enable Azure private peering - You must enable this peering to connect to VMs / cloud services deployed within virtual networks.</span></span>
   * <span data-ttu-id="cbad8-116">Aktivera Azure offentlig peering - måste du aktivera Azure offentlig peering om du vill ansluta till Azure-tjänster finns på den offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="cbad8-116">Enable Azure public peering - You must enable Azure public peering if you wish to connect to Azure services hosted on public IP addresses.</span></span> <span data-ttu-id="cbad8-117">Detta är ett krav för att komma åt Azure-resurser om du har valt att aktivera standardroutning för privat Azure-peering.</span><span class="sxs-lookup"><span data-stu-id="cbad8-117">This is a requirement to access Azure resources if you have chosen to enable default routing for Azure private peering.</span></span>
   * <span data-ttu-id="cbad8-118">Aktivera Microsoft-peering - måste du aktivera åtkomst till Office 365 och Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="cbad8-118">Enable Microsoft peering - You must enable this to access Office 365 and Dynamics 365.</span></span> 
     
     > [!IMPORTANT]
     > <span data-ttu-id="cbad8-119">Du måste se till att du använder en separat proxyserver / kant för att ansluta till Microsoft än som du använder för Internet.</span><span class="sxs-lookup"><span data-stu-id="cbad8-119">You must ensure that you use a separate proxy / edge to connect to Microsoft than the one you use for the Internet.</span></span> <span data-ttu-id="cbad8-120">Med hjälp av samma kant för ExpressRoute- och Internet orsaka asymmetriska Routning och orsaka anslutningen avbrott för ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="cbad8-120">Using the same edge for both ExpressRoute and the Internet will cause asymmetric routing and cause connectivity outages for your network.</span></span>
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. <span data-ttu-id="cbad8-121">Länka virtuella nätverk för ExpressRoute-kretsar - du kan länka virtuella nätverk till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-121">Linking virtual networks to ExpressRoute circuits - You can link virtual networks to your ExpressRoute circuit.</span></span> <span data-ttu-id="cbad8-122">Följ anvisningarna [att länka Vnet](expressroute-howto-linkvnet-arm.md) till kretsen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-122">Follow instructions [to link VNets](expressroute-howto-linkvnet-arm.md) to your circuit.</span></span> <span data-ttu-id="cbad8-123">Dessa Vnet kan antingen vara i samma Azure-prenumerationen som ExpressRoute-kretsen eller kan vara i en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cbad8-123">These VNets can either be in the same Azure subscription as the ExpressRoute circuit, or can be in a different subscription.</span></span>

## <a name="expressroute-circuit-provisioning-states"></a><span data-ttu-id="cbad8-124">ExpressRoute-krets etablering tillstånd</span><span class="sxs-lookup"><span data-stu-id="cbad8-124">ExpressRoute circuit provisioning states</span></span>
<span data-ttu-id="cbad8-125">Varje ExpressRoute-kretsen har två lägen:</span><span class="sxs-lookup"><span data-stu-id="cbad8-125">Each ExpressRoute circuit has two states:</span></span>

* <span data-ttu-id="cbad8-126">Etableringsstatusen för service provider</span><span class="sxs-lookup"><span data-stu-id="cbad8-126">Service provider provisioning state</span></span>
* <span data-ttu-id="cbad8-127">Status</span><span class="sxs-lookup"><span data-stu-id="cbad8-127">Status</span></span>

<span data-ttu-id="cbad8-128">Status representerar Microsofts etablering.</span><span class="sxs-lookup"><span data-stu-id="cbad8-128">Status represents Microsoft's provisioning state.</span></span> <span data-ttu-id="cbad8-129">Ange den här egenskapen är aktiverad när du skapar en Expressroute-krets</span><span class="sxs-lookup"><span data-stu-id="cbad8-129">This property is set to Enabled when you create an Expressroute circuit</span></span>

<span data-ttu-id="cbad8-130">Anslutningen providern etableringsstatusen representerar tillståndet på anslutningen leverantörens sida.</span><span class="sxs-lookup"><span data-stu-id="cbad8-130">The connectivity provider provisioning state represents the state on the connectivity provider's side.</span></span> <span data-ttu-id="cbad8-131">Det kan antingen vara *NotProvisioned*, *etablering*, eller *etablerad*.</span><span class="sxs-lookup"><span data-stu-id="cbad8-131">It can either be *NotProvisioned*, *Provisioning*, or *Provisioned*.</span></span> <span data-ttu-id="cbad8-132">ExpressRoute-kretsen måste vara i etablerad tillstånd att kunna använda den.</span><span class="sxs-lookup"><span data-stu-id="cbad8-132">The ExpressRoute circuit must be in Provisioned state for you to be able to use it.</span></span>

### <a name="possible-states-of-an-expressroute-circuit"></a><span data-ttu-id="cbad8-133">Möjliga tillstånd för en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="cbad8-133">Possible states of an ExpressRoute circuit</span></span>
<span data-ttu-id="cbad8-134">Det här avsnittet innehåller ut möjliga tillstånd för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="cbad8-134">This section lists out the possible states for an ExpressRoute circuit.</span></span>

<span data-ttu-id="cbad8-135">**Vid skapandet**</span><span class="sxs-lookup"><span data-stu-id="cbad8-135">**At creation time**</span></span>

<span data-ttu-id="cbad8-136">ExpressRoute-kretsen i följande läge visas när du kör PowerShell-cmdlet för att skapa ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-136">You will see the ExpressRoute circuit in the following state as soon as you run the PowerShell cmdlet to create the ExpressRoute circuit.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="cbad8-137">**När anslutningen providern håller på att etablera kretsen**</span><span class="sxs-lookup"><span data-stu-id="cbad8-137">**When connectivity provider is in the process of provisioning the circuit**</span></span>

<span data-ttu-id="cbad8-138">ExpressRoute-kretsen i följande läge visas när du skickar tjänstnyckeln till providern för anslutningen och de har startat etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-138">You will see the ExpressRoute circuit in the following state as soon as you pass the service key to the connectivity provider and they have started the provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


<span data-ttu-id="cbad8-139">**När anslutningen providern har slutfört etableringsprocessen**</span><span class="sxs-lookup"><span data-stu-id="cbad8-139">**When connectivity provider has completed the provisioning process**</span></span>

<span data-ttu-id="cbad8-140">ExpressRoute-kretsen i följande läge visas så snart anslutningen providern har slutfört etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-140">You will see the ExpressRoute circuit in the following state as soon as the connectivity provider has completed the provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

<span data-ttu-id="cbad8-141">Etablerad och aktiverad är bara Tillståndet kretsen kan vara i att kunna använda den.</span><span class="sxs-lookup"><span data-stu-id="cbad8-141">Provisioned and Enabled is the only state the circuit can be in for you to be able to use it.</span></span> <span data-ttu-id="cbad8-142">Om du använder en nivå 2-provider kan du konfigurera routning för kretsen endast när det är i det här tillståndet.</span><span class="sxs-lookup"><span data-stu-id="cbad8-142">If you are using a Layer 2 provider, you can configure routing for your circuit only when it is in this state.</span></span>

<span data-ttu-id="cbad8-143">**När anslutningen provider avetablering kretsen**</span><span class="sxs-lookup"><span data-stu-id="cbad8-143">**When connectivity provider is deprovisioning the circuit**</span></span>

<span data-ttu-id="cbad8-144">Om du har begärt leverantören att ta bort etableringen av ExpressRoute-kretsen visas kretsen angetts till följande tillstånd när tjänstleverantör har slutförts avetableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-144">If you requested the service provider to deprovision the ExpressRoute circuit, you will see the circuit set to the following state after the service provider has completed the deprovisioning process.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="cbad8-145">Du kan välja att aktivera det igen om det behövs eller som kör PowerShell-cmdletar för att ta bort kretsen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-145">You can choose to re-enable it if needed, or run PowerShell cmdlets to delete the circuit.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="cbad8-146">Om du kör PowerShell-cmdlet för att ta bort kretsen vid etablering av ServiceProviderProvisioningState eller etablerad misslyckas åtgärden.</span><span class="sxs-lookup"><span data-stu-id="cbad8-146">If you run the PowerShell cmdlet to delete the circuit when the ServiceProviderProvisioningState is Provisioning or Provisioned the operation will fail.</span></span> <span data-ttu-id="cbad8-147">Se tillsammans med anslutningsleverantören för att ta bort etableringen ExpressRoute-kretsen först och sedan ta bort kretsen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-147">Please work with your connectivity provider to deprovision the ExpressRoute circuit first and then delete the circuit.</span></span> <span data-ttu-id="cbad8-148">Microsoft fortsätter att debiterar kretsen förrän du kör PowerShell-cmdlet för att ta bort kretsen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-148">Microsoft will continue to bill the circuit until you run the PowerShell cmdlet to delete the circuit.</span></span>
> 
> 

## <a name="routing-session-configuration-state"></a><span data-ttu-id="cbad8-149">Routning sessionstillstånd konfiguration</span><span class="sxs-lookup"><span data-stu-id="cbad8-149">Routing session configuration state</span></span>
<span data-ttu-id="cbad8-150">BGP Etableringsstatus får du reda på om BGP-sessionen har aktiverats på Microsoft edge.</span><span class="sxs-lookup"><span data-stu-id="cbad8-150">The BGP provisioning state lets you know if the BGP session has been enabled on the Microsoft edge.</span></span> <span data-ttu-id="cbad8-151">Tillståndet måste aktiveras för att du ska kunna använda peering.</span><span class="sxs-lookup"><span data-stu-id="cbad8-151">The state must be enabled for you to be able to use the peering.</span></span>

<span data-ttu-id="cbad8-152">Det är viktigt att kontrollera tillståndet för BGP-sessionen särskilt för Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="cbad8-152">It is important to check the BGP session state especially for Microsoft peering.</span></span> <span data-ttu-id="cbad8-153">Förutom BGP Etableringsstatus, det finns ett annat tillstånd som kallas *annonserade offentliga prefix tillstånd*.</span><span class="sxs-lookup"><span data-stu-id="cbad8-153">In addition to the BGP provisioning state, there is another state called *advertised public prefixes state*.</span></span> <span data-ttu-id="cbad8-154">Tillståndet annonserade offentliga prefix måste vara i *konfigurerats* tillstånd, både för BGP-sessionen ska dig och dina routning för att fungera slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="cbad8-154">The advertised public prefixes state must be in *configured* state, both for the BGP session to be up and for your routing to work end-to-end.</span></span> 

<span data-ttu-id="cbad8-155">Om tillståndet annonserade offentliga prefix anges till en *validering krävs* tillstånd, BGP-sessionen inte är aktiverad, som annonserade prefix inte matchade antalet AS i någon av Routning register.</span><span class="sxs-lookup"><span data-stu-id="cbad8-155">If the advertised public prefix state is set to a *validation needed* state, the BGP session is not enabled, as the advertised prefixes did not match the AS number in any of the routing registries.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="cbad8-156">Om den annonserade offentliga prefix är i tillståndet *manuell verifiering* tillstånd, måste du öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) och bevisa att du äger annonserade längs med associerade autonomt systemnummer IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="cbad8-156">If the advertised public prefixes state is in *manual validation* state, you must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and provide evidence that you own the IP addresses advertised along with the associated Autonomous System number.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="cbad8-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cbad8-157">Next steps</span></span>
* <span data-ttu-id="cbad8-158">Konfigurera ExpressRoute-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="cbad8-158">Configure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="cbad8-159">Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="cbad8-159">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-arm.md)
  * [<span data-ttu-id="cbad8-160">Konfigurera routning</span><span class="sxs-lookup"><span data-stu-id="cbad8-160">Configure routing</span></span>](expressroute-howto-routing-arm.md)
  * [<span data-ttu-id="cbad8-161">Länka ett VNet till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="cbad8-161">Link a VNet to an ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

