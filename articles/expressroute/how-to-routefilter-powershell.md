---
title: "Konfigurera filter för routning för Azure ExpressRoute Microsoft peering: PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur tooconfigure väg filtrerar för Microsoft-Peering med hjälp av PowerShell"
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
ms.openlocfilehash: 395bcf7d6ad43c643ff041b154f8e4b797083e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="50478-103">Konfigurera routningsfilter för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="50478-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="50478-104">Vägfilter är ett sätt tooconsume en delmängd av tjänster som stöds via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="50478-104">Route filters are a way tooconsume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="50478-105">hello stegen i den här artikeln hjälpen du konfigurera och hantera filter för routning för ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="50478-105">hello steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="50478-106">Dynamics 365-tjänster och Office 365-tjänster som Exchange Online, SharePoint Online och Skype för företag, kan nås via hello Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="50478-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through hello Microsoft peering.</span></span> <span data-ttu-id="50478-107">När Microsoft-peering konfigureras i en ExpressRoute-krets, visas alla prefix relaterade toothese tjänster i hello BGP-sessioner som upprättas.</span><span class="sxs-lookup"><span data-stu-id="50478-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related toothese services are advertised through hello BGP sessions that are established.</span></span> <span data-ttu-id="50478-108">Ett värde för BGP-gemenskapen är bifogade tooevery prefix tooidentify hello tjänst som erbjuds via hello prefix.</span><span class="sxs-lookup"><span data-stu-id="50478-108">A BGP community value is attached tooevery prefix tooidentify hello service that is offered through hello prefix.</span></span> <span data-ttu-id="50478-109">En lista över hello BGP community värden och hello tjänster de mappas till finns [BGP communities](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="50478-109">For a list of hello BGP community values and hello services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="50478-110">Om du behöver tooall tjänster har ett stort antal prefix annonserats via BGP.</span><span class="sxs-lookup"><span data-stu-id="50478-110">If you require connectivity tooall services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="50478-111">Detta ökar avsevärt hello storleken på hello vägtabeller upprätthålls av routrar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="50478-111">This significantly increases hello size of hello route tables maintained by routers within your network.</span></span> <span data-ttu-id="50478-112">Om du planerar tooconsume endast en delmängd tjänster som erbjuds via Microsoft-peering kan du minska hello storleken på din vägtabeller på två sätt.</span><span class="sxs-lookup"><span data-stu-id="50478-112">If you plan tooconsume only a subset of services offered through Microsoft peering, you can reduce hello size of your route tables in two ways.</span></span> <span data-ttu-id="50478-113">Du kan:</span><span class="sxs-lookup"><span data-stu-id="50478-113">You can:</span></span>

- <span data-ttu-id="50478-114">Filtrera bort oönskade prefix genom att använda filter för routning på BGP-communities.</span><span class="sxs-lookup"><span data-stu-id="50478-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="50478-115">Detta är ett vanligt nätverk förfarande och används ofta i flera nätverk.</span><span class="sxs-lookup"><span data-stu-id="50478-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="50478-116">Definiera filter för Routning och Använd dem tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="50478-116">Define route filters and apply them tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="50478-117">Ett filter för vägen är en ny resurs som du kan välja hello lista över tjänster du planera tooconsume via Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="50478-117">A route filter is a new resource that lets you select hello list of services you plan tooconsume through Microsoft peering.</span></span> <span data-ttu-id="50478-118">ExpressRoute-routrar bara skicka hello listan över prefix som tillhör toohello tjänster som identifierats i hello flödet filter.</span><span class="sxs-lookup"><span data-stu-id="50478-118">ExpressRoute routers only send hello list of prefixes that belong toohello services identified in hello route filter.</span></span>

### <span data-ttu-id="50478-119"><a name="about"></a>Om vägen filter</span><span class="sxs-lookup"><span data-stu-id="50478-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="50478-120">När Microsoft-peering har konfigurerats på ExpressRoute-krets, upprättar hello Microsoft edge routrar ett par med BGP-sessioner med hello routrar i utkanten (ditt eller leverantören connectivity).</span><span class="sxs-lookup"><span data-stu-id="50478-120">When Microsoft peering is configured on your ExpressRoute circuit, hello Microsoft edge routers establish a pair of BGP sessions with hello edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="50478-121">Inga vägar är annonserade tooyour nätverk.</span><span class="sxs-lookup"><span data-stu-id="50478-121">No routes are advertised tooyour network.</span></span> <span data-ttu-id="50478-122">tooenable väg annonser tooyour nätverk, måste du associera en vägfilter.</span><span class="sxs-lookup"><span data-stu-id="50478-122">tooenable route advertisements tooyour network, you must associate a route filter.</span></span>

<span data-ttu-id="50478-123">En vägfilter kan du identifiera tjänster du vill använda tooconsume via ExpressRoute-krets Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="50478-123">A route filter lets you identify services you want tooconsume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="50478-124">Det är i princip en lista för tillåten för alla hello BGP community-värden.</span><span class="sxs-lookup"><span data-stu-id="50478-124">It is essentially a white list of all hello BGP community values.</span></span> <span data-ttu-id="50478-125">När en väg filter resurs definieras och anslutna tooan ExpressRoute-krets, är alla prefix som mappar toohello BGP community värden annonserade tooyour nätverk.</span><span class="sxs-lookup"><span data-stu-id="50478-125">Once a route filter resource is defined and attached tooan ExpressRoute circuit, all prefixes that map toohello BGP community values are advertised tooyour network.</span></span>

<span data-ttu-id="50478-126">toobe kan tooattach vägfilter med Office 365-tjänster på dem, måste du ha auktorisering tooconsume Office 365-tjänster via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="50478-126">toobe able tooattach route filters with Office 365 services on them, you must have authorization tooconsume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="50478-127">Om du inte är auktoriserade tooconsume Office 365-tjänster genom ExpressRoute misslyckas hello åtgärden tooattach vägfilter.</span><span class="sxs-lookup"><span data-stu-id="50478-127">If you are not authorized tooconsume Office 365 services through ExpressRoute, hello operation tooattach route filters fails.</span></span> <span data-ttu-id="50478-128">Läs mer om hello auktoriseringen [Azure ExpressRoute för Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="50478-128">For more information about hello authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="50478-129">TooDynamics 365 tjänster kräver inte någon tillstånd.</span><span class="sxs-lookup"><span data-stu-id="50478-129">Connectivity tooDynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50478-130">Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft-peering, även om filter routning inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="50478-130">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="50478-131">Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets.</span><span class="sxs-lookup"><span data-stu-id="50478-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span>
> 
> 

### <span data-ttu-id="50478-132"><a name="workflow"></a>Arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="50478-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="50478-133">toobe kan toosuccessfully ansluta tooservices via Microsoft-peering måste du slutföra hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="50478-133">toobe able toosuccessfully connect tooservices through Microsoft peering, you must complete hello following configuration steps:</span></span>

- <span data-ttu-id="50478-134">Du måste ha en aktiv ExpressRoute-krets som har Microsoft peering etablerade.</span><span class="sxs-lookup"><span data-stu-id="50478-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="50478-135">Du kan använda följande anvisningar tooaccomplish hello dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="50478-135">You can use hello following instructions tooaccomplish these tasks:</span></span>
  - <span data-ttu-id="50478-136">[Skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="50478-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="50478-137">Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd.</span><span class="sxs-lookup"><span data-stu-id="50478-137">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="50478-138">[Skapa Microsoft-peering](expressroute-circuit-peerings.md) om du hanterar direkt hello BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="50478-138">[Create Microsoft peering](expressroute-circuit-peerings.md) if you manage hello BGP session directly.</span></span> <span data-ttu-id="50478-139">Har anslutningsleverantören etablera Microsoft-peering för kretsen.</span><span class="sxs-lookup"><span data-stu-id="50478-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="50478-140">Du måste skapa och konfigurera ett filter för vägen.</span><span class="sxs-lookup"><span data-stu-id="50478-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="50478-141">Identifiera hello tjänster du med tooconsume via Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="50478-141">Identify hello services you with tooconsume through Microsoft peering</span></span>
    - <span data-ttu-id="50478-142">Identifiera hello lista över BGP community-värden som är associerade med hello-tjänster</span><span class="sxs-lookup"><span data-stu-id="50478-142">Identify hello list of BGP community values associated with hello services</span></span>
    - <span data-ttu-id="50478-143">Skapa en regel tooallow hello prefix listan matchande hello BGP community värden</span><span class="sxs-lookup"><span data-stu-id="50478-143">Create a rule tooallow hello prefix list matching hello BGP community values</span></span>

-  <span data-ttu-id="50478-144">Du måste koppla hello flödet filter toohello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="50478-144">You must attach hello route filter toohello ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="50478-145">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="50478-145">Before you begin</span></span>

<span data-ttu-id="50478-146">Innan du börjar konfigurera måste du kontrollera att du uppfyller följande kriterier hello:</span><span class="sxs-lookup"><span data-stu-id="50478-146">Before you begin configuration, make sure you meet hello following criteria:</span></span>

 - <span data-ttu-id="50478-147">Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="50478-147">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="50478-148">Mer information finns i [installera och konfigurera Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="50478-148">For more information, see [Install and configure Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span></span>

  > [!NOTE]
  > <span data-ttu-id="50478-149">Hämta hello senaste versionen från hello PowerShell-galleriet i stället för att använda hello Installer.</span><span class="sxs-lookup"><span data-stu-id="50478-149">Download hello latest version from hello PowerShell Gallery, rather than using hello Installer.</span></span> <span data-ttu-id="50478-150">hello Installer för närvarande stöder inte hello som krävs för cmdlet: ar.</span><span class="sxs-lookup"><span data-stu-id="50478-150">hello Installer currently does not support hello required cmdlets.</span></span>
  > 

 - <span data-ttu-id="50478-151">Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="50478-151">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="50478-152">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="50478-152">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="50478-153">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="50478-153">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="50478-154">Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd.</span><span class="sxs-lookup"><span data-stu-id="50478-154">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="50478-155">Du måste ha ett aktivt Microsoft-peering.</span><span class="sxs-lookup"><span data-stu-id="50478-155">You must have an active Microsoft peering.</span></span> <span data-ttu-id="50478-156">Följ anvisningarna på [skapa och ändra peering konfiguration](expressroute-circuit-peerings.md)</span><span class="sxs-lookup"><span data-stu-id="50478-156">Follow instructions at [Create and modifying peering configuration](expressroute-circuit-peerings.md)</span></span>

### <a name="log-in-tooyour-azure-account"></a><span data-ttu-id="50478-157">Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="50478-157">Log in tooyour Azure account</span></span>

<span data-ttu-id="50478-158">Innan du påbörjar den här konfigurationen måste du logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="50478-158">Before beginning this configuration, you must log in tooyour Azure account.</span></span> <span data-ttu-id="50478-159">hello cmdlet efterfrågar hello inloggningsuppgifterna för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="50478-159">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="50478-160">När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="50478-160">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span>

<span data-ttu-id="50478-161">Öppna PowerShell-konsol med utökade privilegier och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="50478-161">Open your PowerShell console with elevated privileges, and connect tooyour account.</span></span> <span data-ttu-id="50478-162">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="50478-162">Use hello following example toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="50478-163">Om du har flera Azure-prenumerationer, kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="50478-163">If you have multiple Azure subscriptions, check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="50478-164">Ange hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="50478-164">Specify hello subscription that you want toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <span data-ttu-id="50478-165"><a name="prefixes"></a>Steg 1.</span><span class="sxs-lookup"><span data-stu-id="50478-165"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="50478-166">Hämta en lista över prefix och värden för BGP-gemenskapen</span><span class="sxs-lookup"><span data-stu-id="50478-166">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="50478-167">1. Hämta en lista över BGP community värden</span><span class="sxs-lookup"><span data-stu-id="50478-167">1. Get a list of BGP community values</span></span>

<span data-ttu-id="50478-168">Använd hello följande cmdlet tooget hello lista över BGP community-värden som är kopplad till tjänster som är tillgänglig via Microsoft-peering och hello listan över prefix som är kopplade till dem:</span><span class="sxs-lookup"><span data-stu-id="50478-168">Use hello following cmdlet tooget hello list of BGP community values associated with services accessible through Microsoft peering, and hello list of prefixes associated with them:</span></span>

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a><span data-ttu-id="50478-169">2. Skapa en lista över hello värden som du vill toouse</span><span class="sxs-lookup"><span data-stu-id="50478-169">2. Make a list of hello values that you want toouse</span></span>

<span data-ttu-id="50478-170">Se en lista över BGP community-värden som du vill ha toouse i hello flödet filter.</span><span class="sxs-lookup"><span data-stu-id="50478-170">Make a list of BGP community values you want toouse in hello route filter.</span></span> <span data-ttu-id="50478-171">Ett exempel är hello BGP community-värdet för Dynamics 365 tjänsterna 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="50478-171">As an example, hello BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="50478-172"><a name="filter"></a>Steg 2.</span><span class="sxs-lookup"><span data-stu-id="50478-172"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="50478-173">Skapa en vägfilter och en filterregeln</span><span class="sxs-lookup"><span data-stu-id="50478-173">Create a route filter and a filter rule</span></span>

<span data-ttu-id="50478-174">En vägfilter kan ha endast en regel och hello regeln måste vara av typen ”Tillåt'.</span><span class="sxs-lookup"><span data-stu-id="50478-174">A route filter can have only one rule, and hello rule must be of type 'Allow'.</span></span> <span data-ttu-id="50478-175">Den här regeln kan ha en lista över BGP community-värden som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="50478-175">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="50478-176">1. Skapa ett vägfilter</span><span class="sxs-lookup"><span data-stu-id="50478-176">1. Create a route filter</span></span>

<span data-ttu-id="50478-177">Först skapa hello flödet filter.</span><span class="sxs-lookup"><span data-stu-id="50478-177">First, create hello route filter.</span></span> <span data-ttu-id="50478-178">hello-kommando 'Ny AzureRmRouteFilter' skapar bara en väg filter resurs.</span><span class="sxs-lookup"><span data-stu-id="50478-178">hello command 'New-AzureRmRouteFilter' only creates a route filter resource.</span></span> <span data-ttu-id="50478-179">När du har skapat hello resurs måste du skapa en regel och bifoga den toohello vägen filtreringsobjekt.</span><span class="sxs-lookup"><span data-stu-id="50478-179">After you create hello resource, you must then create a rule and attach it toohello route filter object.</span></span> <span data-ttu-id="50478-180">Kör följande kommando toocreate hello en väg filter resurs:</span><span class="sxs-lookup"><span data-stu-id="50478-180">Run hello following command toocreate a route filter resource:</span></span>

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="50478-181">2. Skapa en regel för filter</span><span class="sxs-lookup"><span data-stu-id="50478-181">2. Create a filter rule</span></span>

<span data-ttu-id="50478-182">Du kan ange en uppsättning BGP communities som en kommaavgränsad lista, enligt hello exempel.</span><span class="sxs-lookup"><span data-stu-id="50478-182">You can specify a set of BGP communities as a comma-separated list, as shown in hello example.</span></span> <span data-ttu-id="50478-183">Kör följande kommando toocreate hello en ny regel:</span><span class="sxs-lookup"><span data-stu-id="50478-183">Run hello following command toocreate a new rule:</span></span>
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-hello-rule-toohello-route-filter"></a><span data-ttu-id="50478-184">3. Lägg till hello regeln toohello vägfilter</span><span class="sxs-lookup"><span data-stu-id="50478-184">3. Add hello rule toohello route filter</span></span>

<span data-ttu-id="50478-185">Hello kör följande kommando tooadd hello filter regeln toohello vägfilter:</span><span class="sxs-lookup"><span data-stu-id="50478-185">Run hello following command tooadd hello filter rule toohello route filter:</span></span>
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="50478-186"><a name="attach"></a>Steg 3.</span><span class="sxs-lookup"><span data-stu-id="50478-186"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="50478-187">Koppla hello flödet filter tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="50478-187">Attach hello route filter tooan ExpressRoute circuit</span></span>

<span data-ttu-id="50478-188">Kör följande kommando tooattach hello flödet filter toohello ExpressRoute-krets, förutsatt att du har endast Microsoft-peering hello:</span><span class="sxs-lookup"><span data-stu-id="50478-188">Run hello following command tooattach hello route filter toohello ExpressRoute circuit, assuming you have only Microsoft peering:</span></span>

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="50478-189"><a name="getproperties"></a>tooget hello egenskaperna för en vägfilter</span><span class="sxs-lookup"><span data-stu-id="50478-189"><a name="getproperties"></a>tooget hello properties of a route filter</span></span>

<span data-ttu-id="50478-190">tooget hello egenskaper för ett flöde filter, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="50478-190">tooget hello properties of a route filter, use hello following steps:</span></span>

1. <span data-ttu-id="50478-191">Hello kör följande kommando tooget hello routeresurs filter:</span><span class="sxs-lookup"><span data-stu-id="50478-191">Run hello following command tooget hello route filter resource:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. <span data-ttu-id="50478-192">Hämta hello flödet filterregler för hello route-filter resursen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="50478-192">Get hello route filter rules for hello route-filter resource by running hello following command:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <span data-ttu-id="50478-193"><a name="updateproperties"></a>tooupdate hello egenskaperna för en vägfilter</span><span class="sxs-lookup"><span data-stu-id="50478-193"><a name="updateproperties"></a>tooupdate hello properties of a route filter</span></span>

<span data-ttu-id="50478-194">Om hello flödet filter är redan kopplad tooa krets, uppdateringar toohello BGP community listan automatiskt sprids ändringar av lämplig prefix annons via hello upprätta BGP-sessioner.</span><span class="sxs-lookup"><span data-stu-id="50478-194">If hello route filter is already attached tooa circuit, updates toohello BGP community list automatically propagates appropriate prefix advertisement changes through hello established BGP sessions.</span></span> <span data-ttu-id="50478-195">Du kan uppdatera hello BGP över vägfiltret med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="50478-195">You can update hello BGP community list of your route filter using hello following command:</span></span>

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="50478-196"><a name="detach"></a>toodetach ett flöde filter från en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="50478-196"><a name="detach"></a>toodetach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="50478-197">När en vägfilter är frånkopplat från hello ExpressRoute-krets har inget prefix annonserats via hello BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="50478-197">Once a route filter is detached from hello ExpressRoute circuit, no prefixes are advertised through hello BGP session.</span></span> <span data-ttu-id="50478-198">Du kan koppla från ett flöde filter från en ExpressRoute-krets med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="50478-198">You can detach a route filter from an ExpressRoute circuit using hello following command:</span></span>
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="50478-199"><a name="delete"></a>toodelete vägfilter</span><span class="sxs-lookup"><span data-stu-id="50478-199"><a name="delete"></a>toodelete a route filter</span></span>

<span data-ttu-id="50478-200">Du kan endast ta bort vägen filter om den inte är ansluten tooany krets.</span><span class="sxs-lookup"><span data-stu-id="50478-200">You can only delete a route filter if it is not attached tooany circuit.</span></span> <span data-ttu-id="50478-201">Kontrollera hello flödet filtret inte är ansluten tooany krets innan du försöker toodelete den.</span><span class="sxs-lookup"><span data-stu-id="50478-201">Ensure that hello route filter is not attached tooany circuit before attempting toodelete it.</span></span> <span data-ttu-id="50478-202">Du kan ta bort en vägfilter som använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="50478-202">You can delete a route filter using hello following command:</span></span>

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="50478-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50478-203">Next steps</span></span>

<span data-ttu-id="50478-204">Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="50478-204">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
