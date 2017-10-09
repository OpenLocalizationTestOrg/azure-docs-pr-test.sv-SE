---
title: "aaaWorkflows för att konfigurera en ExpressRoute-krets | Microsoft Docs"
description: "Den här sidan vägleder dig genom hello arbetsflöden för att konfigurera ExpressRoute-krets och peerkopplingar"
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
ms.openlocfilehash: 8e1dfc137401e0d6d53608ae6c8de0085e182eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a><span data-ttu-id="2ecdd-103">Arbetsflöden i ExpressRoute för kretsetablering och kretstillstånd</span><span class="sxs-lookup"><span data-stu-id="2ecdd-103">ExpressRoute workflows for circuit provisioning and circuit states</span></span>
<span data-ttu-id="2ecdd-104">Den här sidan vägleder dig genom hello service etablering och routning configuration arbetsflöden på en hög nivå.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-104">This page walks you through hello service provisioning and routing configuration workflows at a high level.</span></span>

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

<span data-ttu-id="2ecdd-105">hello följande bild och motsvarande steg visar hello uppgifter i ordning toohave måste du följa en ExpressRoute-kretsen etablerade slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-105">hello following figure and corresponding steps show hello tasks you must follow in order toohave an ExpressRoute circuit provisioned end-to-end.</span></span> 

1. <span data-ttu-id="2ecdd-106">Använd PowerShell tooconfigure en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-106">Use PowerShell tooconfigure an ExpressRoute circuit.</span></span> <span data-ttu-id="2ecdd-107">Följ anvisningarna för hello i hello [skapa ExpressRoute-kretsar](expressroute-howto-circuit-classic.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-107">Follow hello instructions in hello [Create ExpressRoute circuits](expressroute-howto-circuit-classic.md) article for more details.</span></span>
2. <span data-ttu-id="2ecdd-108">Ordna anslutningen från hello-leverantören.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-108">Order connectivity from hello service provider.</span></span> <span data-ttu-id="2ecdd-109">Den här processen varierar.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-109">This process varies.</span></span> <span data-ttu-id="2ecdd-110">Kontakta anslutningsleverantören för mer information om hur tooorder anslutning.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-110">Contact your connectivity provider for more details about how tooorder connectivity.</span></span>
3. <span data-ttu-id="2ecdd-111">Se till att hello kretsen har har etablerats genom att verifiera hello ExpressRoute-krets Etableringsstatus via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-111">Ensure that hello circuit has been provisioned successfully by verifying hello ExpressRoute circuit provisioning state through PowerShell.</span></span> 
4. <span data-ttu-id="2ecdd-112">Konfigurera routning domäner.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-112">Configure routing domains.</span></span> <span data-ttu-id="2ecdd-113">Om anslutningsleverantören hanterar Layer 3 åt dig, kommer de att konfigurera routning för kretsen.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-113">If your connectivity provider manages Layer 3 for you, they will configure routing for your circuit.</span></span> <span data-ttu-id="2ecdd-114">Om anslutningsleverantören erbjuder endast nivå 2-tjänster, måste du konfigurera routning per riktlinjer som beskrivs i hello [routningskrav](expressroute-routing.md) och [routningskonfiguration](expressroute-howto-routing-classic.md) sidor.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-114">If your connectivity provider only offers Layer 2 services, you must configure routing per guidelines described in hello [routing requirements](expressroute-routing.md) and [routing configuration](expressroute-howto-routing-classic.md) pages.</span></span>
   
   * <span data-ttu-id="2ecdd-115">Aktivera privat Azure-peering - du måste aktivera den här peering tooconnect tooVMs / molntjänster distribueras inom virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-115">Enable Azure private peering - You must enable this peering tooconnect tooVMs / cloud services deployed within virtual networks.</span></span>
   * <span data-ttu-id="2ecdd-116">Aktivera Azure offentlig peering - du måste aktivera offentlig Azure-peering om du inte vill tooconnect tooAzure services på den offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-116">Enable Azure public peering - You must enable Azure public peering if you wish tooconnect tooAzure services hosted on public IP addresses.</span></span> <span data-ttu-id="2ecdd-117">Detta är ett krav tooaccess Azure-resurser om du har valt tooenable standardroutning för privat Azure-peering.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-117">This is a requirement tooaccess Azure resources if you have chosen tooenable default routing for Azure private peering.</span></span>
   * <span data-ttu-id="2ecdd-118">Aktivera Microsoft-peering - du måste aktivera den här tooaccess Office 365 och Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-118">Enable Microsoft peering - You must enable this tooaccess Office 365 and Dynamics 365.</span></span> 
     
     > [!IMPORTANT]
     > <span data-ttu-id="2ecdd-119">Du måste se till att du använder en separat proxyserver / edge tooconnect tooMicrosoft än hello som du använder för hello Internet.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-119">You must ensure that you use a separate proxy / edge tooconnect tooMicrosoft than hello one you use for hello Internet.</span></span> <span data-ttu-id="2ecdd-120">Med hjälp av hello samma kant för ExpressRoute- och hello Internet innebär orsaka asymmetriska Routning och anslutningen avbrott för ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-120">Using hello same edge for both ExpressRoute and hello Internet will cause asymmetric routing and cause connectivity outages for your network.</span></span>
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. <span data-ttu-id="2ecdd-121">Länka virtuella nätverk tooExpressRoute kretsar – du kan länka virtuella nätverk tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-121">Linking virtual networks tooExpressRoute circuits - You can link virtual networks tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="2ecdd-122">Följ anvisningarna [toolink Vnet](expressroute-howto-linkvnet-arm.md) tooyour krets.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-122">Follow instructions [toolink VNets](expressroute-howto-linkvnet-arm.md) tooyour circuit.</span></span> <span data-ttu-id="2ecdd-123">Dessa Vnet kan antingen vara i samma Azure-prenumeration som hello ExpressRoute-krets hello eller kan vara i en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-123">These VNets can either be in hello same Azure subscription as hello ExpressRoute circuit, or can be in a different subscription.</span></span>

## <a name="expressroute-circuit-provisioning-states"></a><span data-ttu-id="2ecdd-124">ExpressRoute-krets etablering tillstånd</span><span class="sxs-lookup"><span data-stu-id="2ecdd-124">ExpressRoute circuit provisioning states</span></span>
<span data-ttu-id="2ecdd-125">Varje ExpressRoute-kretsen har två lägen:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-125">Each ExpressRoute circuit has two states:</span></span>

* <span data-ttu-id="2ecdd-126">Etableringsstatusen för service provider</span><span class="sxs-lookup"><span data-stu-id="2ecdd-126">Service provider provisioning state</span></span>
* <span data-ttu-id="2ecdd-127">Status</span><span class="sxs-lookup"><span data-stu-id="2ecdd-127">Status</span></span>

<span data-ttu-id="2ecdd-128">Status representerar Microsofts etablering.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-128">Status represents Microsoft's provisioning state.</span></span> <span data-ttu-id="2ecdd-129">Den här egenskapen anges tooEnabled när du skapar en Expressroute-krets</span><span class="sxs-lookup"><span data-stu-id="2ecdd-129">This property is set tooEnabled when you create an Expressroute circuit</span></span>

<span data-ttu-id="2ecdd-130">anslutningen hello providern etableringsstatusen representerar hello på hello anslutningen providern sida.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-130">hello connectivity provider provisioning state represents hello state on hello connectivity provider's side.</span></span> <span data-ttu-id="2ecdd-131">Det kan antingen vara *NotProvisioned*, *etablering*, eller *etablerad*.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-131">It can either be *NotProvisioned*, *Provisioning*, or *Provisioned*.</span></span> <span data-ttu-id="2ecdd-132">Hej ExpressRoute-krets måste vara i etablerad tillstånd för du toobe kan toouse den.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-132">hello ExpressRoute circuit must be in Provisioned state for you toobe able toouse it.</span></span>

### <a name="possible-states-of-an-expressroute-circuit"></a><span data-ttu-id="2ecdd-133">Möjliga tillstånd för en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="2ecdd-133">Possible states of an ExpressRoute circuit</span></span>
<span data-ttu-id="2ecdd-134">Det här avsnittet innehåller ut hello möjliga tillstånd för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-134">This section lists out hello possible states for an ExpressRoute circuit.</span></span>

<span data-ttu-id="2ecdd-135">**Vid skapandet**</span><span class="sxs-lookup"><span data-stu-id="2ecdd-135">**At creation time**</span></span>

<span data-ttu-id="2ecdd-136">Hej ExpressRoute-krets i hello följande tillstånd så snart som du kör hello PowerShell cmdlet toocreate hello ExpressRoute-krets visas.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-136">You will see hello ExpressRoute circuit in hello following state as soon as you run hello PowerShell cmdlet toocreate hello ExpressRoute circuit.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="2ecdd-137">**När anslutningen leverantören är i hello processen för etablering hello-krets**</span><span class="sxs-lookup"><span data-stu-id="2ecdd-137">**When connectivity provider is in hello process of provisioning hello circuit**</span></span>

<span data-ttu-id="2ecdd-138">Du ser hello ExpressRoute-krets i hello följande tillstånd så snart som du skickar hello tjänstleverantör viktiga toohello anslutning och de har startat hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-138">You will see hello ExpressRoute circuit in hello following state as soon as you pass hello service key toohello connectivity provider and they have started hello provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


<span data-ttu-id="2ecdd-139">**När anslutningen providern har slutförts hello etableringsprocessen**</span><span class="sxs-lookup"><span data-stu-id="2ecdd-139">**When connectivity provider has completed hello provisioning process**</span></span>

<span data-ttu-id="2ecdd-140">Du ser hello ExpressRoute-krets i hello följande tillstånd när hello anslutningen providern har slutförts hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-140">You will see hello ExpressRoute circuit in hello following state as soon as hello connectivity provider has completed hello provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

<span data-ttu-id="2ecdd-141">Etablerad och aktiverat är hello tillstånd hello kretsen kan vara i av du toobe kan toouse den.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-141">Provisioned and Enabled is hello only state hello circuit can be in for you toobe able toouse it.</span></span> <span data-ttu-id="2ecdd-142">Om du använder en nivå 2-provider kan du konfigurera routning för kretsen endast när det är i det här tillståndet.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-142">If you are using a Layer 2 provider, you can configure routing for your circuit only when it is in this state.</span></span>

<span data-ttu-id="2ecdd-143">**När anslutningen provider avetablering hello-krets**</span><span class="sxs-lookup"><span data-stu-id="2ecdd-143">**When connectivity provider is deprovisioning hello circuit**</span></span>

<span data-ttu-id="2ecdd-144">Om du har begärt hello service provider toodeprovision hello ExpressRoute-krets visas hello krets ange toohello följande tillstånd när hello-leverantör har slutförts hello avetablering process.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-144">If you requested hello service provider toodeprovision hello ExpressRoute circuit, you will see hello circuit set toohello following state after hello service provider has completed hello deprovisioning process.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="2ecdd-145">Du kan välja toore – aktivera det om behövs eller som kör PowerShell-cmdlets toodelete hello krets.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-145">You can choose toore-enable it if needed, or run PowerShell cmdlets toodelete hello circuit.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="2ecdd-146">Om du kör misslyckas hello PowerShell cmdlet toodelete hello krets när hello ServiceProviderProvisioningState är etablering eller etablerad hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-146">If you run hello PowerShell cmdlet toodelete hello circuit when hello ServiceProviderProvisioningState is Provisioning or Provisioned hello operation will fail.</span></span> <span data-ttu-id="2ecdd-147">Se tillsammans med din anslutning providern toodeprovision hello ExpressRoute-krets först och ta sedan bort hello kretsen.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-147">Please work with your connectivity provider toodeprovision hello ExpressRoute circuit first and then delete hello circuit.</span></span> <span data-ttu-id="2ecdd-148">Microsoft kommer att fortsätta toobill hello krets förrän du kör hello PowerShell cmdlet toodelete hello krets.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-148">Microsoft will continue toobill hello circuit until you run hello PowerShell cmdlet toodelete hello circuit.</span></span>
> 
> 

## <a name="routing-session-configuration-state"></a><span data-ttu-id="2ecdd-149">Routning sessionstillstånd konfiguration</span><span class="sxs-lookup"><span data-stu-id="2ecdd-149">Routing session configuration state</span></span>
<span data-ttu-id="2ecdd-150">hello BGP etableringsstatusen får du reda på om hello BGP-sessionen har aktiverats på hello Microsoft edge.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-150">hello BGP provisioning state lets you know if hello BGP session has been enabled on hello Microsoft edge.</span></span> <span data-ttu-id="2ecdd-151">hello tillstånd måste vara aktiverat för du toobe kan toouse hello peering.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-151">hello state must be enabled for you toobe able toouse hello peering.</span></span>

<span data-ttu-id="2ecdd-152">Det är viktigt toocheck hello BGP sessionstillstånd särskilt för Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-152">It is important toocheck hello BGP session state especially for Microsoft peering.</span></span> <span data-ttu-id="2ecdd-153">Tillägg toohello BGP etableringsstatusen, det finns ett annat tillstånd som kallas *annonserade offentliga prefix tillstånd*.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-153">In addition toohello BGP provisioning state, there is another state called *advertised public prefixes state*.</span></span> <span data-ttu-id="2ecdd-154">hello annonserade offentliga prefix tillstånd måste vara i *konfigurerats* tillstånd, både för hello BGP-sessionen toobe dig och dina routning toowork slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-154">hello advertised public prefixes state must be in *configured* state, both for hello BGP session toobe up and for your routing toowork end-to-end.</span></span> 

<span data-ttu-id="2ecdd-155">Om hello annonserade offentliga prefix status är inställd tooa *validering krävs* tillstånd, hello BGP-sessionen inte är aktiverad, som hello annonserade prefix inte matchade hello som i någon av hello routning register.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-155">If hello advertised public prefix state is set tooa *validation needed* state, hello BGP session is not enabled, as hello advertised prefixes did not match hello AS number in any of hello routing registries.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="2ecdd-156">Om hello annonserade offentliga prefix tillstånd *manuell verifiering* tillstånd, måste du öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) och bevisa att du äger hello IP-adresser annonserade längs hello associeras autonomt systemnummer.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-156">If hello advertised public prefixes state is in *manual validation* state, you must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and provide evidence that you own hello IP addresses advertised along with hello associated Autonomous System number.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="2ecdd-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ecdd-157">Next steps</span></span>
* <span data-ttu-id="2ecdd-158">Konfigurera ExpressRoute-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-158">Configure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="2ecdd-159">Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="2ecdd-159">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-arm.md)
  * [<span data-ttu-id="2ecdd-160">Konfigurera routning</span><span class="sxs-lookup"><span data-stu-id="2ecdd-160">Configure routing</span></span>](expressroute-howto-routing-arm.md)
  * [<span data-ttu-id="2ecdd-161">Länka ett VNet tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="2ecdd-161">Link a VNet tooan ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

