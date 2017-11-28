---
title: "Konfigurera filter för routning för Azure ExpressRoute Microsoft peering: Portal | Microsoft Docs"
description: "Den här artikeln beskriver hur tooconfigure vägfilter för att använda Microsoft Peering hello Azure-portalen"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 2a47d465ec5f175d9510cef94606f70f036f0862
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="4af43-103">Konfigurera routningsfilter för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="4af43-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="4af43-104">Vägfilter är ett sätt tooconsume en delmängd av tjänster som stöds via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="4af43-104">Route filters are a way tooconsume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="4af43-105">hello stegen i den här artikeln hjälpen du konfigurera och hantera filter för routning för ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="4af43-105">hello steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="4af43-106">Dynamics 365-tjänster och Office 365-tjänster som Exchange Online, SharePoint Online och Skype för företag, kan nås via hello Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="4af43-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through hello Microsoft peering.</span></span> <span data-ttu-id="4af43-107">När Microsoft-peering konfigureras i en ExpressRoute-krets, visas alla prefix relaterade toothese tjänster i hello BGP-sessioner som upprättas.</span><span class="sxs-lookup"><span data-stu-id="4af43-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related toothese services are advertised through hello BGP sessions that are established.</span></span> <span data-ttu-id="4af43-108">Ett värde för BGP-gemenskapen är bifogade tooevery prefix tooidentify hello tjänst som erbjuds via hello prefix.</span><span class="sxs-lookup"><span data-stu-id="4af43-108">A BGP community value is attached tooevery prefix tooidentify hello service that is offered through hello prefix.</span></span> <span data-ttu-id="4af43-109">En lista över hello BGP community värden och hello tjänster de mappas till finns [BGP communities](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="4af43-109">For a list of hello BGP community values and hello services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="4af43-110">Om du behöver tooall tjänster har ett stort antal prefix annonserats via BGP.</span><span class="sxs-lookup"><span data-stu-id="4af43-110">If you require connectivity tooall services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="4af43-111">Detta ökar avsevärt hello storleken på hello vägtabeller upprätthålls av routrar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="4af43-111">This significantly increases hello size of hello route tables maintained by routers within your network.</span></span> <span data-ttu-id="4af43-112">Om du planerar tooconsume endast en delmängd tjänster som erbjuds via Microsoft-peering kan du minska hello storleken på din vägtabeller på två sätt.</span><span class="sxs-lookup"><span data-stu-id="4af43-112">If you plan tooconsume only a subset of services offered through Microsoft peering, you can reduce hello size of your route tables in two ways.</span></span> <span data-ttu-id="4af43-113">Du kan:</span><span class="sxs-lookup"><span data-stu-id="4af43-113">You can:</span></span>

- <span data-ttu-id="4af43-114">Filtrera bort oönskade prefix genom att använda filter för routning på BGP-communities.</span><span class="sxs-lookup"><span data-stu-id="4af43-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="4af43-115">Detta är ett vanligt nätverk förfarande och används ofta i flera nätverk.</span><span class="sxs-lookup"><span data-stu-id="4af43-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="4af43-116">Definiera filter för Routning och Använd dem tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="4af43-116">Define route filters and apply them tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="4af43-117">Ett filter för vägen är en ny resurs som du kan välja hello lista över tjänster du planera tooconsume via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="4af43-117">A route filter is a new resource that lets you select hello list of services you plan tooconsume through Microsoft peering.</span></span> <span data-ttu-id="4af43-118">ExpressRoute-routrar bara skicka hello listan över prefix som tillhör toohello tjänster som identifierats i hello flödet filter.</span><span class="sxs-lookup"><span data-stu-id="4af43-118">ExpressRoute routers only send hello list of prefixes that belong toohello services identified in hello route filter.</span></span>

### <span data-ttu-id="4af43-119"><a name="about"></a>Om vägen filter</span><span class="sxs-lookup"><span data-stu-id="4af43-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="4af43-120">När Microsoft-peering har konfigurerats på ExpressRoute-krets, upprättar hello Microsoft edge routrar ett par med BGP-sessioner med hello routrar i utkanten (ditt eller leverantören connectivity).</span><span class="sxs-lookup"><span data-stu-id="4af43-120">When Microsoft peering is configured on your ExpressRoute circuit, hello Microsoft edge routers establish a pair of BGP sessions with hello edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="4af43-121">Inga vägar är annonserade tooyour nätverk.</span><span class="sxs-lookup"><span data-stu-id="4af43-121">No routes are advertised tooyour network.</span></span> <span data-ttu-id="4af43-122">tooenable väg annonser tooyour nätverk, måste du associera en vägfilter.</span><span class="sxs-lookup"><span data-stu-id="4af43-122">tooenable route advertisements tooyour network, you must associate a route filter.</span></span>

<span data-ttu-id="4af43-123">En vägfilter kan du identifiera tjänster du vill använda tooconsume via ExpressRoute-krets Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="4af43-123">A route filter lets you identify services you want tooconsume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="4af43-124">Det är i princip en lista för tillåten för alla hello BGP community-värden.</span><span class="sxs-lookup"><span data-stu-id="4af43-124">It is essentially a white list of all hello BGP community values.</span></span> <span data-ttu-id="4af43-125">När en väg filter resurs definieras och anslutna tooan ExpressRoute-krets, är alla prefix som mappar toohello BGP community värden annonserade tooyour nätverk.</span><span class="sxs-lookup"><span data-stu-id="4af43-125">Once a route filter resource is defined and attached tooan ExpressRoute circuit, all prefixes that map toohello BGP community values are advertised tooyour network.</span></span>

<span data-ttu-id="4af43-126">toobe kan tooattach vägfilter med Office 365-tjänster på dem, måste du ha auktorisering tooconsume Office 365-tjänster via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4af43-126">toobe able tooattach route filters with Office 365 services on them, you must have authorization tooconsume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="4af43-127">Om du inte är auktoriserade tooconsume Office 365-tjänster genom ExpressRoute misslyckas hello åtgärden tooattach vägfilter.</span><span class="sxs-lookup"><span data-stu-id="4af43-127">If you are not authorized tooconsume Office 365 services through ExpressRoute, hello operation tooattach route filters fails.</span></span> <span data-ttu-id="4af43-128">Läs mer om hello auktoriseringen [Azure ExpressRoute för Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="4af43-128">For more information about hello authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="4af43-129">TooDynamics 365 tjänster kräver inte någon tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4af43-129">Connectivity tooDynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4af43-130">Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft-peering, även om filter routning inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="4af43-130">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="4af43-131">Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets.</span><span class="sxs-lookup"><span data-stu-id="4af43-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span>
> 
> 

### <span data-ttu-id="4af43-132"><a name="workflow"></a>Arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="4af43-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="4af43-133">toobe kan toosuccessfully ansluta tooservices via Microsoft-peering måste du slutföra hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="4af43-133">toobe able toosuccessfully connect tooservices through Microsoft peering, you must complete hello following configuration steps:</span></span>

- <span data-ttu-id="4af43-134">Du måste ha en aktiv ExpressRoute-krets som har Microsoft peering etablerade.</span><span class="sxs-lookup"><span data-stu-id="4af43-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="4af43-135">Du kan använda följande anvisningar tooaccomplish hello dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="4af43-135">You can use hello following instructions tooaccomplish these tasks:</span></span>
  - <span data-ttu-id="4af43-136">[Skapa en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="4af43-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="4af43-137">Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4af43-137">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="4af43-138">[Skapa Microsoft-peering](expressroute-howto-routing-portal-resource-manager.md) om du hanterar direkt hello BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="4af43-138">[Create Microsoft peering](expressroute-howto-routing-portal-resource-manager.md) if you manage hello BGP session directly.</span></span> <span data-ttu-id="4af43-139">Har anslutningsleverantören etablera Microsoft-peering för kretsen.</span><span class="sxs-lookup"><span data-stu-id="4af43-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="4af43-140">Du måste skapa och konfigurera ett filter för vägen.</span><span class="sxs-lookup"><span data-stu-id="4af43-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="4af43-141">Identifiera hello tjänster du med tooconsume via Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="4af43-141">Identify hello services you with tooconsume through Microsoft peering</span></span>
    - <span data-ttu-id="4af43-142">Identifiera hello lista över BGP community-värden som är associerade med hello-tjänster</span><span class="sxs-lookup"><span data-stu-id="4af43-142">Identify hello list of BGP community values associated with hello services</span></span>
    - <span data-ttu-id="4af43-143">Skapa en regel tooallow hello prefix listan matchande hello BGP community värden</span><span class="sxs-lookup"><span data-stu-id="4af43-143">Create a rule tooallow hello prefix list matching hello BGP community values</span></span>

-  <span data-ttu-id="4af43-144">Du måste koppla hello flödet filter toohello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="4af43-144">You must attach hello route filter toohello ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4af43-145">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="4af43-145">Before you begin</span></span>

<span data-ttu-id="4af43-146">Innan du börjar konfigurera måste du kontrollera att du uppfyller följande kriterier hello:</span><span class="sxs-lookup"><span data-stu-id="4af43-146">Before you begin configuration, make sure you meet hello following criteria:</span></span>

 - <span data-ttu-id="4af43-147">Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="4af43-147">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="4af43-148">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="4af43-148">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="4af43-149">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="4af43-149">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="4af43-150">Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4af43-150">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="4af43-151">Du måste ha ett aktivt Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="4af43-151">You must have an active Microsoft peering.</span></span> <span data-ttu-id="4af43-152">Följ anvisningarna på [skapa och ändra peering konfiguration](expressroute-howto-routing-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="4af43-152">Follow instructions at [Create and modifying peering configuration](expressroute-howto-routing-portal-resource-manager.md)</span></span>


## <span data-ttu-id="4af43-153"><a name="prefixes"></a>Steg 1.</span><span class="sxs-lookup"><span data-stu-id="4af43-153"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="4af43-154">Hämta en lista över prefix och värden för BGP-gemenskapen</span><span class="sxs-lookup"><span data-stu-id="4af43-154">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="4af43-155">1. Hämta en lista över BGP community värden</span><span class="sxs-lookup"><span data-stu-id="4af43-155">1. Get a list of BGP community values</span></span>

<span data-ttu-id="4af43-156">BGP community-värden som är kopplad till tjänster som är tillgänglig via Microsoft-peering finns i hello [ExpressRoute routningskrav](expressroute-routing.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="4af43-156">BGP community values associated with services accessible through Microsoft peering is available in hello [ExpressRoute routing requirements](expressroute-routing.md) page.</span></span>

### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a><span data-ttu-id="4af43-157">2. Skapa en lista över hello värden som du vill toouse</span><span class="sxs-lookup"><span data-stu-id="4af43-157">2. Make a list of hello values that you want toouse</span></span>

<span data-ttu-id="4af43-158">Se en lista över BGP community-värden som du vill ha toouse i hello flödet filter.</span><span class="sxs-lookup"><span data-stu-id="4af43-158">Make a list of BGP community values you want toouse in hello route filter.</span></span> <span data-ttu-id="4af43-159">Ett exempel är hello BGP community-värdet för Dynamics 365 tjänsterna 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="4af43-159">As an example, hello BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="4af43-160"><a name="filter"></a>Steg 2.</span><span class="sxs-lookup"><span data-stu-id="4af43-160"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="4af43-161">Skapa en vägfilter och en filterregeln</span><span class="sxs-lookup"><span data-stu-id="4af43-161">Create a route filter and a filter rule</span></span>

<span data-ttu-id="4af43-162">En vägfilter kan ha endast en regel och hello regeln måste vara av typen ”Tillåt'.</span><span class="sxs-lookup"><span data-stu-id="4af43-162">A route filter can have only one rule, and hello rule must be of type 'Allow'.</span></span> <span data-ttu-id="4af43-163">Den här regeln kan ha en lista över BGP community-värden som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="4af43-163">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="4af43-164">1. Skapa ett vägfilter</span><span class="sxs-lookup"><span data-stu-id="4af43-164">1. Create a route filter</span></span>
<span data-ttu-id="4af43-165">Du kan skapa en vägfilter genom att välja hello alternativet toocreate en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="4af43-165">You can create a route filter by selecting hello option toocreate a new resource.</span></span> <span data-ttu-id="4af43-166">Klicka på **ny** > **nätverk** > **RouteFilter**som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="4af43-166">Click **New** > **Networking** > **RouteFilter**, as shown in hello following image:</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

<span data-ttu-id="4af43-168">Du måste placera hello flödet filter i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4af43-168">You must place hello route filter in a resource group.</span></span> 

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="4af43-170">2. Skapa en regel för filter</span><span class="sxs-lookup"><span data-stu-id="4af43-170">2. Create a filter rule</span></span>

<span data-ttu-id="4af43-171">Du kan lägga till och uppdateringsregler genom att välja hello hantera regel-fliken för vägen filtret.</span><span class="sxs-lookup"><span data-stu-id="4af43-171">You can add and update rules by selecting hello manage rule tab for your route filter.</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


<span data-ttu-id="4af43-173">Du kan välja hello tjänster som du vill tooconnect toofrom hello listrutan och spara hello regel när du är klar.</span><span class="sxs-lookup"><span data-stu-id="4af43-173">You can select hello services you want tooconnect toofrom hello drop down list and save hello rule when done.</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <span data-ttu-id="4af43-175"><a name="attach"></a>Steg 3.</span><span class="sxs-lookup"><span data-stu-id="4af43-175"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="4af43-176">Koppla hello flödet filter tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="4af43-176">Attach hello route filter tooan ExpressRoute circuit</span></span>

<span data-ttu-id="4af43-177">Du kan bifoga hello flödet filter tooa krets genom att välja ”Lägg till kretsen” hello-knappen och välja hello ExpressRoute-krets hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="4af43-177">You can attach hello route filter tooa circuit by selecting hello "add Circuit" button and selecting hello ExpressRoute circuit from hello drop down list.</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

## <span data-ttu-id="4af43-179"><a name="getproperties"></a>tooget hello egenskaperna för en vägfilter</span><span class="sxs-lookup"><span data-stu-id="4af43-179"><a name="getproperties"></a>tooget hello properties of a route filter</span></span>

<span data-ttu-id="4af43-180">Du kan visa egenskaperna för en vägfilter när du öppnar hello resurs i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4af43-180">You can view properties of a route filter when you open hello resource in hello portal.</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


## <span data-ttu-id="4af43-182"><a name="updateproperties"></a>tooupdate hello egenskaperna för en vägfilter</span><span class="sxs-lookup"><span data-stu-id="4af43-182"><a name="updateproperties"></a>tooupdate hello properties of a route filter</span></span>

<span data-ttu-id="4af43-183">Du kan uppdatera hello lista över BGP community värden bifogade tooa krets genom att välja knappen hello ”hantera regel”.</span><span class="sxs-lookup"><span data-stu-id="4af43-183">You can update hello list of BGP community values attached tooa circuit by selecting hello "Manage rule" button.</span></span>


![Skapa ett vägfilter](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


## <span data-ttu-id="4af43-186"><a name="detach"></a>toodetach ett flöde filter från en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="4af43-186"><a name="detach"></a>toodetach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="4af43-187">toodetach en krets från hello flödet filter högerklickar du på hello krets och klicka på ”avassociera”.</span><span class="sxs-lookup"><span data-stu-id="4af43-187">toodetach a circuit from hello route filter, right click on hello circuit and click on "disassociate".</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


## <span data-ttu-id="4af43-189"><a name="delete"></a>toodelete vägfilter</span><span class="sxs-lookup"><span data-stu-id="4af43-189"><a name="delete"></a>toodelete a route filter</span></span>

<span data-ttu-id="4af43-190">Du kan ta bort ett filter för vägen genom att välja hello ta bort.</span><span class="sxs-lookup"><span data-stu-id="4af43-190">You can delete a route filter by selecting hello delete button.</span></span> 

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a><span data-ttu-id="4af43-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4af43-192">Next steps</span></span>

<span data-ttu-id="4af43-193">Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="4af43-193">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
