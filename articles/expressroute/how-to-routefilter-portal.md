---
title: "Konfigurera filter för routning för Azure ExpressRoute Microsoft peering: Portal | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar filter för routning för Microsoft-Peering i Azure Portal"
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
ms.openlocfilehash: f17bf3e475a33cfc617e8a026e9606b3792101f3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="5e213-103">Konfigurera routningsfilter för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="5e213-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="5e213-104">Vägfilter är ett sätt att använda en delmängd av tjänster som stöds via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="5e213-104">Route filters are a way to consume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="5e213-105">Stegen i den här artikeln hjälpa dig att konfigurera och hantera filter för routning för ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="5e213-105">The steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="5e213-106">Dynamics 365-tjänster och Office 365-tjänster som Exchange Online, SharePoint Online och Skype för företag, är tillgängliga via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="5e213-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through the Microsoft peering.</span></span> <span data-ttu-id="5e213-107">När Microsoft-peering konfigureras i en ExpressRoute-krets, visas alla prefix som är relaterade till tjänsterna i BGP-sessioner som upprättas.</span><span class="sxs-lookup"><span data-stu-id="5e213-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related to these services are advertised through the BGP sessions that are established.</span></span> <span data-ttu-id="5e213-108">Ett värde för BGP-gemenskapen är kopplad till varje prefix för att identifiera den tjänst som erbjuds via prefixet.</span><span class="sxs-lookup"><span data-stu-id="5e213-108">A BGP community value is attached to every prefix to identify the service that is offered through the prefix.</span></span> <span data-ttu-id="5e213-109">En lista över BGP community-värden och de tjänster som de mappas till finns [BGP communities](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="5e213-109">For a list of the BGP community values and the services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="5e213-110">Om du behöver ansluta till alla tjänster har ett stort antal prefix annonserats via BGP.</span><span class="sxs-lookup"><span data-stu-id="5e213-110">If you require connectivity to all services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="5e213-111">Detta ökar avsevärt storleken på vägtabeller upprätthålls av routrar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="5e213-111">This significantly increases the size of the route tables maintained by routers within your network.</span></span> <span data-ttu-id="5e213-112">Om du planerar att använda endast en delmängd av tjänster som erbjuds via Microsoft peering kan du minska storleken på din vägtabeller på två sätt.</span><span class="sxs-lookup"><span data-stu-id="5e213-112">If you plan to consume only a subset of services offered through Microsoft peering, you can reduce the size of your route tables in two ways.</span></span> <span data-ttu-id="5e213-113">Du kan:</span><span class="sxs-lookup"><span data-stu-id="5e213-113">You can:</span></span>

- <span data-ttu-id="5e213-114">Filtrera bort oönskade prefix genom att använda filter för routning på BGP-communities.</span><span class="sxs-lookup"><span data-stu-id="5e213-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="5e213-115">Detta är ett vanligt nätverk förfarande och används ofta i flera nätverk.</span><span class="sxs-lookup"><span data-stu-id="5e213-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="5e213-116">Definiera filter för Routning och koppla dem till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="5e213-116">Define route filters and apply them to your ExpressRoute circuit.</span></span> <span data-ttu-id="5e213-117">Ett filter för vägen är en ny resurs där du kan välja i listan över tjänster som du planerar att använda via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="5e213-117">A route filter is a new resource that lets you select the list of services you plan to consume through Microsoft peering.</span></span> <span data-ttu-id="5e213-118">ExpressRoute-routrar bara skicka listan över prefix som hör till de tjänster som identifierats i filtret vägen.</span><span class="sxs-lookup"><span data-stu-id="5e213-118">ExpressRoute routers only send the list of prefixes that belong to the services identified in the route filter.</span></span>

### <span data-ttu-id="5e213-119"><a name="about"></a>Om vägen filter</span><span class="sxs-lookup"><span data-stu-id="5e213-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="5e213-120">När Microsoft-peering har konfigurerats på ExpressRoute-krets, upprättar Microsoft edge routrar ett par med BGP-sessioner med kant-routrar (ditt eller leverantören connectivity).</span><span class="sxs-lookup"><span data-stu-id="5e213-120">When Microsoft peering is configured on your ExpressRoute circuit, the Microsoft edge routers establish a pair of BGP sessions with the edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="5e213-121">Inga vägar annonseras till nätverket.</span><span class="sxs-lookup"><span data-stu-id="5e213-121">No routes are advertised to your network.</span></span> <span data-ttu-id="5e213-122">Om du vill aktivera vägannonser i nätverket, måste du associera en vägfilter.</span><span class="sxs-lookup"><span data-stu-id="5e213-122">To enable route advertisements to your network, you must associate a route filter.</span></span>

<span data-ttu-id="5e213-123">En vägfilter kan du identifiera tjänster du vill använda via ExpressRoute-krets Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="5e213-123">A route filter lets you identify services you want to consume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="5e213-124">Det är i princip en lista för tillåten för alla värden för BGP-communityn.</span><span class="sxs-lookup"><span data-stu-id="5e213-124">It is essentially a white list of all the BGP community values.</span></span> <span data-ttu-id="5e213-125">När en resurs för vägen filter är definierad och ansluten till en ExpressRoute-krets, annonseras alla prefix som mappas till värdena för BGP-gemenskapen till nätverket.</span><span class="sxs-lookup"><span data-stu-id="5e213-125">Once a route filter resource is defined and attached to an ExpressRoute circuit, all prefixes that map to the BGP community values are advertised to your network.</span></span>

<span data-ttu-id="5e213-126">Om du vill kunna koppla vägfilter med Office 365-tjänster på dem. måste du ha tillstånd att använda Office 365-tjänster via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5e213-126">To be able to attach route filters with Office 365 services on them, you must have authorization to consume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="5e213-127">Om du inte har behörighet att använda Office 365-tjänster via ExpressRoute, misslyckas åtgärden att koppla filter för routning.</span><span class="sxs-lookup"><span data-stu-id="5e213-127">If you are not authorized to consume Office 365 services through ExpressRoute, the operation to attach route filters fails.</span></span> <span data-ttu-id="5e213-128">Läs mer om auktoriseringen [Azure ExpressRoute för Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="5e213-128">For more information about the authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="5e213-129">Anslutningen till Dynamics 365-tjänster kräver inte någon tillstånd.</span><span class="sxs-lookup"><span data-stu-id="5e213-129">Connectivity to Dynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e213-130">Microsoft-peering i ExpressRoute-kretsar som konfigurerades före den 1 augusti 2017 kommer att ha alla service prefix annonserade via Microsoft peering, även om filter routning inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="5e213-130">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="5e213-131">Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonseras förrän ett filter för flödet är kopplat till kretsen.</span><span class="sxs-lookup"><span data-stu-id="5e213-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span>
> 
> 

### <span data-ttu-id="5e213-132"><a name="workflow"></a>Arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="5e213-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="5e213-133">För att kunna ansluta till tjänster med hjälp av Microsoft peering, måste du utföra följande konfigurationssteg:</span><span class="sxs-lookup"><span data-stu-id="5e213-133">To be able to successfully connect to services through Microsoft peering, you must complete the following configuration steps:</span></span>

- <span data-ttu-id="5e213-134">Du måste ha en aktiv ExpressRoute-krets som har Microsoft peering etablerade.</span><span class="sxs-lookup"><span data-stu-id="5e213-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="5e213-135">Du kan använda följande instruktioner för att utföra dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="5e213-135">You can use the following instructions to accomplish these tasks:</span></span>
  - <span data-ttu-id="5e213-136">[Skapa en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och ha kretsen aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="5e213-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="5e213-137">ExpressRoute-kretsen måste vara i ett etablerat och aktiverade tillstånd.</span><span class="sxs-lookup"><span data-stu-id="5e213-137">The ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="5e213-138">[Skapa Microsoft-peering](expressroute-howto-routing-portal-resource-manager.md) om du hanterar BGP-sessionen direkt.</span><span class="sxs-lookup"><span data-stu-id="5e213-138">[Create Microsoft peering](expressroute-howto-routing-portal-resource-manager.md) if you manage the BGP session directly.</span></span> <span data-ttu-id="5e213-139">Har anslutningsleverantören etablera Microsoft-peering för kretsen.</span><span class="sxs-lookup"><span data-stu-id="5e213-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="5e213-140">Du måste skapa och konfigurera ett filter för vägen.</span><span class="sxs-lookup"><span data-stu-id="5e213-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="5e213-141">Identifiera tjänsterna du med att använda via Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="5e213-141">Identify the services you with to consume through Microsoft peering</span></span>
    - <span data-ttu-id="5e213-142">Identifiera listan över BGP community-värden som är kopplade till tjänsterna</span><span class="sxs-lookup"><span data-stu-id="5e213-142">Identify the list of BGP community values associated with the services</span></span>
    - <span data-ttu-id="5e213-143">Skapa en regel för att tillåta listan över prefix matchande BGP community-värden</span><span class="sxs-lookup"><span data-stu-id="5e213-143">Create a rule to allow the prefix list matching the BGP community values</span></span>

-  <span data-ttu-id="5e213-144">Du måste koppla filtret vägen till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="5e213-144">You must attach the route filter to the ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5e213-145">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="5e213-145">Before you begin</span></span>

<span data-ttu-id="5e213-146">Innan du börjar konfigurera måste du kontrollera att du uppfyller följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="5e213-146">Before you begin configuration, make sure you meet the following criteria:</span></span>

 - <span data-ttu-id="5e213-147">Granska de [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="5e213-147">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="5e213-148">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="5e213-148">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="5e213-149">Följ anvisningarna för att [Skapa en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och aktivera kretsen av anslutningsprovidern innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="5e213-149">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="5e213-150">ExpressRoute-kretsen måste vara i ett etablerat och aktiverade tillstånd.</span><span class="sxs-lookup"><span data-stu-id="5e213-150">The ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="5e213-151">Du måste ha ett aktivt Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="5e213-151">You must have an active Microsoft peering.</span></span> <span data-ttu-id="5e213-152">Följ anvisningarna på [skapa och ändra peering konfiguration](expressroute-howto-routing-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="5e213-152">Follow instructions at [Create and modifying peering configuration](expressroute-howto-routing-portal-resource-manager.md)</span></span>


## <span data-ttu-id="5e213-153"><a name="prefixes"></a>Steg 1.</span><span class="sxs-lookup"><span data-stu-id="5e213-153"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="5e213-154">Hämta en lista över prefix och värden för BGP-gemenskapen</span><span class="sxs-lookup"><span data-stu-id="5e213-154">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="5e213-155">1. Hämta en lista över BGP community värden</span><span class="sxs-lookup"><span data-stu-id="5e213-155">1. Get a list of BGP community values</span></span>

<span data-ttu-id="5e213-156">BGP community-värden som är kopplad till tjänster som är tillgänglig via Microsoft-peering är tillgängliga i den [ExpressRoute routningskrav](expressroute-routing.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="5e213-156">BGP community values associated with services accessible through Microsoft peering is available in the [ExpressRoute routing requirements](expressroute-routing.md) page.</span></span>

### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a><span data-ttu-id="5e213-157">2. Skapa en lista över de värden som du vill använda</span><span class="sxs-lookup"><span data-stu-id="5e213-157">2. Make a list of the values that you want to use</span></span>

<span data-ttu-id="5e213-158">Se en lista över BGP community-värden som du vill använda i filtret vägen.</span><span class="sxs-lookup"><span data-stu-id="5e213-158">Make a list of BGP community values you want to use in the route filter.</span></span> <span data-ttu-id="5e213-159">Exempelvis är värdet för BGP-community för Dynamics 365-tjänster 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="5e213-159">As an example, the BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="5e213-160"><a name="filter"></a>Steg 2.</span><span class="sxs-lookup"><span data-stu-id="5e213-160"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="5e213-161">Skapa en vägfilter och en filterregeln</span><span class="sxs-lookup"><span data-stu-id="5e213-161">Create a route filter and a filter rule</span></span>

<span data-ttu-id="5e213-162">En vägfilter kan ha endast en regel och regeln måste vara av typen ”Tillåt'.</span><span class="sxs-lookup"><span data-stu-id="5e213-162">A route filter can have only one rule, and the rule must be of type 'Allow'.</span></span> <span data-ttu-id="5e213-163">Den här regeln kan ha en lista över BGP community-värden som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="5e213-163">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="5e213-164">1. Skapa ett vägfilter</span><span class="sxs-lookup"><span data-stu-id="5e213-164">1. Create a route filter</span></span>
<span data-ttu-id="5e213-165">Du kan skapa en vägfilter genom att välja alternativet för att skapa en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="5e213-165">You can create a route filter by selecting the option to create a new resource.</span></span> <span data-ttu-id="5e213-166">Klicka på **ny** > **nätverk** > **RouteFilter**, enligt följande bild:</span><span class="sxs-lookup"><span data-stu-id="5e213-166">Click **New** > **Networking** > **RouteFilter**, as shown in the following image:</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

<span data-ttu-id="5e213-168">Du måste placera route-filtret i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5e213-168">You must place the route filter in a resource group.</span></span> 

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="5e213-170">2. Skapa en regel för filter</span><span class="sxs-lookup"><span data-stu-id="5e213-170">2. Create a filter rule</span></span>

<span data-ttu-id="5e213-171">Du kan lägga till och uppdatera regler genom att välja fliken hantera regeln för vägfilter.</span><span class="sxs-lookup"><span data-stu-id="5e213-171">You can add and update rules by selecting the manage rule tab for your route filter.</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


<span data-ttu-id="5e213-173">Du kan välja vilka tjänster som du vill ansluta till från nedrullningsbara listan och spara regeln när du är klar.</span><span class="sxs-lookup"><span data-stu-id="5e213-173">You can select the services you want to connect to from the drop down list and save the rule when done.</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <span data-ttu-id="5e213-175"><a name="attach"></a>Steg 3.</span><span class="sxs-lookup"><span data-stu-id="5e213-175"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="5e213-176">Koppla filtret vägen till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="5e213-176">Attach the route filter to an ExpressRoute circuit</span></span>

<span data-ttu-id="5e213-177">Du kan koppla filtret vägen till en krets genom att välja knappen ”Lägg till kretsen” och välja ExpressRoute-kretsen från nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="5e213-177">You can attach the route filter to a circuit by selecting the "add Circuit" button and selecting the ExpressRoute circuit from the drop down list.</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

## <span data-ttu-id="5e213-179"><a name="getproperties"></a>Att hämta egenskaperna för en vägfilter</span><span class="sxs-lookup"><span data-stu-id="5e213-179"><a name="getproperties"></a>To get the properties of a route filter</span></span>

<span data-ttu-id="5e213-180">Du kan visa egenskaperna för en vägfilter när du öppnar resursen i portalen.</span><span class="sxs-lookup"><span data-stu-id="5e213-180">You can view properties of a route filter when you open the resource in the portal.</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


## <span data-ttu-id="5e213-182"><a name="updateproperties"></a>Uppdatera egenskaperna för en vägfilter</span><span class="sxs-lookup"><span data-stu-id="5e213-182"><a name="updateproperties"></a>To update the properties of a route filter</span></span>

<span data-ttu-id="5e213-183">Du kan uppdatera listan över BGP community värden är ansluten till en krets genom att välja knappen ”Hantera regeln”.</span><span class="sxs-lookup"><span data-stu-id="5e213-183">You can update the list of BGP community values attached to a circuit by selecting the "Manage rule" button.</span></span>


![Skapa ett vägfilter](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


## <span data-ttu-id="5e213-186"><a name="detach"></a>Att ta bort ett filter för vägen från en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="5e213-186"><a name="detach"></a>To detach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="5e213-187">Om du vill koppla en krets från vägen filtret, högerklickar du på kretsen och klicka på ”avassociera”.</span><span class="sxs-lookup"><span data-stu-id="5e213-187">To detach a circuit from the route filter, right click on the circuit and click on "disassociate".</span></span>

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


## <span data-ttu-id="5e213-189"><a name="delete"></a>Ta bort ett vägfilter</span><span class="sxs-lookup"><span data-stu-id="5e213-189"><a name="delete"></a>To delete a route filter</span></span>

<span data-ttu-id="5e213-190">Du kan ta bort ett filter för vägen genom att välja knappen Ta bort.</span><span class="sxs-lookup"><span data-stu-id="5e213-190">You can delete a route filter by selecting the delete button.</span></span> 

![Skapa ett vägfilter](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a><span data-ttu-id="5e213-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e213-192">Next steps</span></span>

<span data-ttu-id="5e213-193">Mer information om ExpressRoute finns i [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="5e213-193">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>