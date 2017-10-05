---
title: "Konfigurera filter för routning för Azure ExpressRoute Microsoft peering: PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar filter för routning för Microsoft-Peering med hjälp av PowerShell"
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
ms.date: 08/16/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: de3550c20439fa809869d98b8a57ea3be9c03e7c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="a5ad3-103">Konfigurera routningsfilter för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="a5ad3-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="a5ad3-104">Vägfilter är ett sätt att använda en delmängd av tjänster som stöds via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-104">Route filters are a way to consume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="a5ad3-105">Stegen i den här artikeln hjälpa dig att konfigurera och hantera filter för routning för ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-105">The steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="a5ad3-106">Dynamics 365-tjänster och Office 365-tjänster som Exchange Online, SharePoint Online och Skype för företag, är tillgängliga via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through the Microsoft peering.</span></span> <span data-ttu-id="a5ad3-107">När Microsoft-peering konfigureras i en ExpressRoute-krets, visas alla prefix som är relaterade till tjänsterna i BGP-sessioner som upprättas.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related to these services are advertised through the BGP sessions that are established.</span></span> <span data-ttu-id="a5ad3-108">Ett värde för BGP-gemenskapen är kopplad till varje prefix för att identifiera den tjänst som erbjuds via prefixet.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-108">A BGP community value is attached to every prefix to identify the service that is offered through the prefix.</span></span> <span data-ttu-id="a5ad3-109">En lista över BGP community-värden och de tjänster som de mappas till finns [BGP communities](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="a5ad3-109">For a list of the BGP community values and the services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="a5ad3-110">Om du behöver ansluta till alla tjänster har ett stort antal prefix annonserats via BGP.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-110">If you require connectivity to all services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="a5ad3-111">Detta ökar avsevärt storleken på vägtabeller upprätthålls av routrar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-111">This significantly increases the size of the route tables maintained by routers within your network.</span></span> <span data-ttu-id="a5ad3-112">Om du planerar att använda endast en delmängd av tjänster som erbjuds via Microsoft peering kan du minska storleken på din vägtabeller på två sätt.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-112">If you plan to consume only a subset of services offered through Microsoft peering, you can reduce the size of your route tables in two ways.</span></span> <span data-ttu-id="a5ad3-113">Du kan:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-113">You can:</span></span>

- <span data-ttu-id="a5ad3-114">Filtrera bort oönskade prefix genom att använda filter för routning på BGP-communities.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="a5ad3-115">Detta är ett vanligt nätverk förfarande och används ofta i flera nätverk.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="a5ad3-116">Definiera filter för Routning och koppla dem till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-116">Define route filters and apply them to your ExpressRoute circuit.</span></span> <span data-ttu-id="a5ad3-117">Ett filter för vägen är en ny resurs där du kan välja i listan över tjänster som du planerar att använda via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-117">A route filter is a new resource that lets you select the list of services you plan to consume through Microsoft peering.</span></span> <span data-ttu-id="a5ad3-118">ExpressRoute-routrar bara skicka listan över prefix som hör till de tjänster som identifierats i filtret vägen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-118">ExpressRoute routers only send the list of prefixes that belong to the services identified in the route filter.</span></span>

### <span data-ttu-id="a5ad3-119"><a name="about"></a>Om vägen filter</span><span class="sxs-lookup"><span data-stu-id="a5ad3-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="a5ad3-120">När Microsoft-peering har konfigurerats på ExpressRoute-krets, upprättar Microsoft edge routrar ett par med BGP-sessioner med kant-routrar (ditt eller leverantören connectivity).</span><span class="sxs-lookup"><span data-stu-id="a5ad3-120">When Microsoft peering is configured on your ExpressRoute circuit, the Microsoft edge routers establish a pair of BGP sessions with the edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="a5ad3-121">Inga vägar annonseras till nätverket.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-121">No routes are advertised to your network.</span></span> <span data-ttu-id="a5ad3-122">Om du vill aktivera vägannonser i nätverket, måste du associera en vägfilter.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-122">To enable route advertisements to your network, you must associate a route filter.</span></span>

<span data-ttu-id="a5ad3-123">En vägfilter kan du identifiera tjänster du vill använda via ExpressRoute-krets Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-123">A route filter lets you identify services you want to consume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="a5ad3-124">Det är i princip en lista för tillåten för alla värden för BGP-communityn.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-124">It is essentially a white list of all the BGP community values.</span></span> <span data-ttu-id="a5ad3-125">När en resurs för vägen filter är definierad och ansluten till en ExpressRoute-krets, annonseras alla prefix som mappas till värdena för BGP-gemenskapen till nätverket.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-125">Once a route filter resource is defined and attached to an ExpressRoute circuit, all prefixes that map to the BGP community values are advertised to your network.</span></span>

<span data-ttu-id="a5ad3-126">Om du vill kunna koppla vägfilter med Office 365-tjänster på dem. måste du ha tillstånd att använda Office 365-tjänster via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-126">To be able to attach route filters with Office 365 services on them, you must have authorization to consume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="a5ad3-127">Om du inte har behörighet att använda Office 365-tjänster via ExpressRoute, misslyckas åtgärden att koppla filter för routning.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-127">If you are not authorized to consume Office 365 services through ExpressRoute, the operation to attach route filters fails.</span></span> <span data-ttu-id="a5ad3-128">Läs mer om auktoriseringen [Azure ExpressRoute för Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="a5ad3-128">For more information about the authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="a5ad3-129">Anslutningen till Dynamics 365-tjänster kräver inte någon tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-129">Connectivity to Dynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a5ad3-130">Microsoft-peering i ExpressRoute-kretsar som konfigurerades före den 1 augusti 2017 kommer att ha alla service prefix annonserade via Microsoft peering, även om filter routning inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-130">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="a5ad3-131">Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonseras förrän ett filter för flödet är kopplat till kretsen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span>
> 
> 

### <span data-ttu-id="a5ad3-132"><a name="workflow"></a>Arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="a5ad3-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="a5ad3-133">För att kunna ansluta till tjänster med hjälp av Microsoft peering, måste du utföra följande konfigurationssteg:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-133">To be able to successfully connect to services through Microsoft peering, you must complete the following configuration steps:</span></span>

- <span data-ttu-id="a5ad3-134">Du måste ha en aktiv ExpressRoute-krets som har Microsoft peering etablerade.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="a5ad3-135">Du kan använda följande instruktioner för att utföra dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-135">You can use the following instructions to accomplish these tasks:</span></span>
  - <span data-ttu-id="a5ad3-136">[Skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha kretsen aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="a5ad3-137">ExpressRoute-kretsen måste vara i ett etablerat och aktiverade tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-137">The ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="a5ad3-138">[Skapa Microsoft-peering](expressroute-circuit-peerings.md) om du hanterar BGP-sessionen direkt.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-138">[Create Microsoft peering](expressroute-circuit-peerings.md) if you manage the BGP session directly.</span></span> <span data-ttu-id="a5ad3-139">Har anslutningsleverantören etablera Microsoft-peering för kretsen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="a5ad3-140">Du måste skapa och konfigurera ett filter för vägen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="a5ad3-141">Identifiera tjänsterna du med att använda via Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="a5ad3-141">Identify the services you with to consume through Microsoft peering</span></span>
    - <span data-ttu-id="a5ad3-142">Identifiera listan över BGP community-värden som är kopplade till tjänsterna</span><span class="sxs-lookup"><span data-stu-id="a5ad3-142">Identify the list of BGP community values associated with the services</span></span>
    - <span data-ttu-id="a5ad3-143">Skapa en regel för att tillåta listan över prefix matchande BGP community-värden</span><span class="sxs-lookup"><span data-stu-id="a5ad3-143">Create a rule to allow the prefix list matching the BGP community values</span></span>

-  <span data-ttu-id="a5ad3-144">Du måste koppla filtret vägen till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-144">You must attach the route filter to the ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a5ad3-145">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a5ad3-145">Before you begin</span></span>

<span data-ttu-id="a5ad3-146">Innan du börjar konfigurera måste du kontrollera att du uppfyller följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-146">Before you begin configuration, make sure you meet the following criteria:</span></span>

 - <span data-ttu-id="a5ad3-147">Installera den senaste versionen av Azure Resource Managers PowerShell-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-147">Install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="a5ad3-148">Mer information finns i [installera och konfigurera Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="a5ad3-148">For more information, see [Install and configure Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span></span>

  > [!NOTE]
  > <span data-ttu-id="a5ad3-149">Hämta den senaste versionen från PowerShell-galleriet i stället för med hjälp av installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-149">Download the latest version from the PowerShell Gallery, rather than using the Installer.</span></span> <span data-ttu-id="a5ad3-150">Installationsprogrammet stöder för närvarande inte cmdletarna som krävs.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-150">The Installer currently does not support the required cmdlets.</span></span>
  > 

 - <span data-ttu-id="a5ad3-151">Granska de [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-151">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="a5ad3-152">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-152">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="a5ad3-153">Följ anvisningarna för att [Skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och aktivera kretsen av anslutningsprovidern innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-153">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="a5ad3-154">ExpressRoute-kretsen måste vara i ett etablerat och aktiverade tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-154">The ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="a5ad3-155">Du måste ha ett aktivt Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-155">You must have an active Microsoft peering.</span></span> <span data-ttu-id="a5ad3-156">Följ anvisningarna på [skapa och ändra peering konfiguration](expressroute-circuit-peerings.md)</span><span class="sxs-lookup"><span data-stu-id="a5ad3-156">Follow instructions at [Create and modifying peering configuration](expressroute-circuit-peerings.md)</span></span>

### <a name="log-in-to-your-azure-account"></a><span data-ttu-id="a5ad3-157">Logga in på ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="a5ad3-157">Log in to your Azure account</span></span>

<span data-ttu-id="a5ad3-158">Innan du påbörjar konfigurationen måste du logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-158">Before beginning this configuration, you must log in to your Azure account.</span></span> <span data-ttu-id="a5ad3-159">Den här cmdleten uppmanar dig att ange inloggningsuppgifterna för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-159">The cmdlet prompts you for the login credentials for your Azure account.</span></span> <span data-ttu-id="a5ad3-160">När du har loggat in hämtas dina kontoinställningar så att de blir tillgängliga för Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-160">After logging in, it downloads your account settings so they are available to Azure PowerShell.</span></span>

<span data-ttu-id="a5ad3-161">Öppna PowerShell-konsolen med utökad behörighet och anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-161">Open your PowerShell console with elevated privileges, and connect to your account.</span></span> <span data-ttu-id="a5ad3-162">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-162">Use the following example to help you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a5ad3-163">Om du har flera Azure-prenumerationer anger du prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-163">If you have multiple Azure subscriptions, check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="a5ad3-164">Ange den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-164">Specify the subscription that you want to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <span data-ttu-id="a5ad3-165"><a name="prefixes"></a>Steg 1.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-165"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="a5ad3-166">Hämta en lista över prefix och värden för BGP-gemenskapen</span><span class="sxs-lookup"><span data-stu-id="a5ad3-166">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="a5ad3-167">1. Hämta en lista över BGP community värden</span><span class="sxs-lookup"><span data-stu-id="a5ad3-167">1. Get a list of BGP community values</span></span>

<span data-ttu-id="a5ad3-168">Använd följande cmdlet för att hämta listan över BGP community-värden som är kopplad till tjänster som är tillgänglig via Microsoft-peering och i listan över prefix som är kopplade till dem:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-168">Use the following cmdlet to get the list of BGP community values associated with services accessible through Microsoft peering, and the list of prefixes associated with them:</span></span>

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a><span data-ttu-id="a5ad3-169">2. Skapa en lista över de värden som du vill använda</span><span class="sxs-lookup"><span data-stu-id="a5ad3-169">2. Make a list of the values that you want to use</span></span>

<span data-ttu-id="a5ad3-170">Se en lista över BGP community-värden som du vill använda i filtret vägen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-170">Make a list of BGP community values you want to use in the route filter.</span></span> <span data-ttu-id="a5ad3-171">Exempelvis är värdet för BGP-community för Dynamics 365-tjänster 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-171">As an example, the BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="a5ad3-172"><a name="filter"></a>Steg 2.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-172"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="a5ad3-173">Skapa en vägfilter och en filterregeln</span><span class="sxs-lookup"><span data-stu-id="a5ad3-173">Create a route filter and a filter rule</span></span>

<span data-ttu-id="a5ad3-174">En vägfilter kan ha endast en regel och regeln måste vara av typen ”Tillåt'.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-174">A route filter can have only one rule, and the rule must be of type 'Allow'.</span></span> <span data-ttu-id="a5ad3-175">Den här regeln kan ha en lista över BGP community-värden som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-175">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="a5ad3-176">1. Skapa ett vägfilter</span><span class="sxs-lookup"><span data-stu-id="a5ad3-176">1. Create a route filter</span></span>

<span data-ttu-id="a5ad3-177">Först skapa vägen filtret.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-177">First, create the route filter.</span></span> <span data-ttu-id="a5ad3-178">Kommandot 'Ny AzureRmRouteFilter' skapar bara en väg filter resurs.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-178">The command 'New-AzureRmRouteFilter' only creates a route filter resource.</span></span> <span data-ttu-id="a5ad3-179">När du skapar resursen, måste du sedan skapa en regel och koppla den till filtreringsobjekt vägen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-179">After you create the resource, you must then create a rule and attach it to the route filter object.</span></span> <span data-ttu-id="a5ad3-180">Kör följande kommando för att skapa en väg filter resurs:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-180">Run the following command to create a route filter resource:</span></span>

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="a5ad3-181">2. Skapa en regel för filter</span><span class="sxs-lookup"><span data-stu-id="a5ad3-181">2. Create a filter rule</span></span>

<span data-ttu-id="a5ad3-182">Du kan ange en uppsättning BGP communities som en kommaavgränsad lista som visas i exemplet.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-182">You can specify a set of BGP communities as a comma-separated list, as shown in the example.</span></span> <span data-ttu-id="a5ad3-183">Kör följande kommando för att skapa en ny regel:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-183">Run the following command to create a new rule:</span></span>
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-the-rule-to-the-route-filter"></a><span data-ttu-id="a5ad3-184">3. Lägga till regeln route-filtret</span><span class="sxs-lookup"><span data-stu-id="a5ad3-184">3. Add the rule to the route filter</span></span>

<span data-ttu-id="a5ad3-185">Kör följande kommando för att lägga till filtret regeln vägfiltret:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-185">Run the following command to add the filter rule to the route filter:</span></span>
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="a5ad3-186"><a name="attach"></a>Steg 3.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-186"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="a5ad3-187">Koppla filtret vägen till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="a5ad3-187">Attach the route filter to an ExpressRoute circuit</span></span>

<span data-ttu-id="a5ad3-188">Kör följande kommando för att koppla filtret vägen till ExpressRoute-krets, förutsatt att du har endast Microsoft-peering:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-188">Run the following command to attach the route filter to the ExpressRoute circuit, assuming you have only Microsoft peering:</span></span>

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="a5ad3-189"><a name="getproperties"></a>Att hämta egenskaperna för en vägfilter</span><span class="sxs-lookup"><span data-stu-id="a5ad3-189"><a name="getproperties"></a>To get the properties of a route filter</span></span>

<span data-ttu-id="a5ad3-190">Använd följande steg för att hämta egenskaperna för en vägfilter:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-190">To get the properties of a route filter, use the following steps:</span></span>

1. <span data-ttu-id="a5ad3-191">Kör följande kommando för att hämta filter routeresurs:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-191">Run the following command to get the route filter resource:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. <span data-ttu-id="a5ad3-192">Hämta vägen filterregler för resursen route-filter genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-192">Get the route filter rules for the route-filter resource by running the following command:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <span data-ttu-id="a5ad3-193"><a name="updateproperties"></a>Uppdatera egenskaperna för en vägfilter</span><span class="sxs-lookup"><span data-stu-id="a5ad3-193"><a name="updateproperties"></a>To update the properties of a route filter</span></span>

<span data-ttu-id="a5ad3-194">Om vägfiltret redan är kopplad till en krets uppdateringar i listan BGP community automatiskt rätt prefix annons att ändringar sprids via befintliga BGP-sessioner.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-194">If the route filter is already attached to a circuit, updates to the BGP community list automatically propagates appropriate prefix advertisement changes through the established BGP sessions.</span></span> <span data-ttu-id="a5ad3-195">Du kan uppdatera BGP community listan över vägfiltret med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-195">You can update the BGP community list of your route filter using the following command:</span></span>

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="a5ad3-196"><a name="detach"></a>Att ta bort ett filter för vägen från en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="a5ad3-196"><a name="detach"></a>To detach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="a5ad3-197">När en vägfilter är frånkopplat från ExpressRoute-kretsen har inget prefix annonserats via BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-197">Once a route filter is detached from the ExpressRoute circuit, no prefixes are advertised through the BGP session.</span></span> <span data-ttu-id="a5ad3-198">Du kan koppla från ett flöde filter från en ExpressRoute-krets med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-198">You can detach a route filter from an ExpressRoute circuit using the following command:</span></span>
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="a5ad3-199"><a name="delete"></a>Ta bort ett vägfilter</span><span class="sxs-lookup"><span data-stu-id="a5ad3-199"><a name="delete"></a>To delete a route filter</span></span>

<span data-ttu-id="a5ad3-200">Du kan bara ta bort ett filter för vägen om det inte är kopplat till någon krets.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-200">You can only delete a route filter if it is not attached to any circuit.</span></span> <span data-ttu-id="a5ad3-201">Se till att filtret vägen inte är ansluten till alla krets innan du försöker ta bort den.</span><span class="sxs-lookup"><span data-stu-id="a5ad3-201">Ensure that the route filter is not attached to any circuit before attempting to delete it.</span></span> <span data-ttu-id="a5ad3-202">Du kan ta bort ett filter för vägen med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a5ad3-202">You can delete a route filter using the following command:</span></span>

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="a5ad3-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5ad3-203">Next steps</span></span>

<span data-ttu-id="a5ad3-204">Mer information om ExpressRoute finns i [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="a5ad3-204">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>