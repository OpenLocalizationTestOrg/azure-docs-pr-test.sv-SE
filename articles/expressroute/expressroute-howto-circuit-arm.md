---
title: "Skapa och ändra en ExpressRoute-krets: PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate, etablera, verifiera, uppdatera, ta bort och ta bort etableringen av en ExpressRoute-krets."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8d76c577a9cffdd393abac1b76cccc27d92e9e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="91d79-103">Skapa och ändra en ExpressRoute-krets med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="91d79-103">Create and modify an ExpressRoute circuit using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="91d79-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="91d79-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="91d79-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="91d79-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="91d79-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="91d79-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="91d79-107">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="91d79-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="91d79-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="91d79-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="91d79-109">Den här artikeln beskriver hur toocreate ett Azure ExpressRoute-kretsen med hjälp av PowerShell-cmdlets och hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="91d79-109">This article describes how toocreate an Azure ExpressRoute circuit by using PowerShell cmdlets and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="91d79-110">Den här artikeln beskriver också hur toocheck hello kretsstatusen hello, uppdatera eller ta bort och ta bort etableringen av den.</span><span class="sxs-lookup"><span data-stu-id="91d79-110">This article also shows you how toocheck hello status of hello circuit, update it, or delete and deprovision it.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="91d79-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="91d79-111">Before you begin</span></span>
* <span data-ttu-id="91d79-112">Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="91d79-112">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="91d79-113">Mer information finns i [översikt av Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="91d79-113">For more information, see [Overview of Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="91d79-114">Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="91d79-114">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>


## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="91d79-115">Skapa och etablera en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-115">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a><span data-ttu-id="91d79-116">1. Logga in tooyour Azure-konto och välja din prenumeration</span><span class="sxs-lookup"><span data-stu-id="91d79-116">1. Sign in tooyour Azure account and select your subscription</span></span>
<span data-ttu-id="91d79-117">toobegin konfigurationen kan logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="91d79-117">toobegin your configuration, sign in tooyour Azure account.</span></span> <span data-ttu-id="91d79-118">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="91d79-118">Use hello following examples toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="91d79-119">Kontrollera hello prenumerationer för hello konto:</span><span class="sxs-lookup"><span data-stu-id="91d79-119">Check hello subscriptions for hello account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="91d79-120">Välj hello-prenumeration som du vill toocreate en ExpressRoute-krets för:</span><span class="sxs-lookup"><span data-stu-id="91d79-120">Select hello subscription that you want toocreate an ExpressRoute circuit for:</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="91d79-121">2. Hämta hello lista över providers som stöds, platser och bandbredder</span><span class="sxs-lookup"><span data-stu-id="91d79-121">2. Get hello list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="91d79-122">Innan du skapar en ExpressRoute-krets måste hello lista över providers stöds anslutning, platser och alternativ för bandbredd.</span><span class="sxs-lookup"><span data-stu-id="91d79-122">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="91d79-123">Hej PowerShell-cmdleten **Get-AzureRmExpressRouteServiceProvider** returnerar informationen som du ska använda i senare steg:</span><span class="sxs-lookup"><span data-stu-id="91d79-123">hello PowerShell cmdlet **Get-AzureRmExpressRouteServiceProvider** returns this information, which you’ll use in later steps:</span></span>

```powershell
Get-AzureRmExpressRouteServiceProvider
```

<span data-ttu-id="91d79-124">Kontrollera toosee om anslutningsleverantören finns med i listan.</span><span class="sxs-lookup"><span data-stu-id="91d79-124">Check toosee if your connectivity provider is listed there.</span></span> <span data-ttu-id="91d79-125">Anteckna hello följande information.</span><span class="sxs-lookup"><span data-stu-id="91d79-125">Make a note of hello following information.</span></span> <span data-ttu-id="91d79-126">Du behöver det senare när du skapar en krets.</span><span class="sxs-lookup"><span data-stu-id="91d79-126">You'll need it later when you create a circuit.</span></span>

* <span data-ttu-id="91d79-127">Namn</span><span class="sxs-lookup"><span data-stu-id="91d79-127">Name</span></span>
* <span data-ttu-id="91d79-128">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="91d79-128">PeeringLocations</span></span>
* <span data-ttu-id="91d79-129">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="91d79-129">BandwidthsOffered</span></span>

<span data-ttu-id="91d79-130">Nu är du redo toocreate en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="91d79-130">You're now ready toocreate an ExpressRoute circuit.</span></span>   

### <a name="3-create-an-expressroute-circuit"></a><span data-ttu-id="91d79-131">3. Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-131">3. Create an ExpressRoute circuit</span></span>
<span data-ttu-id="91d79-132">Om du inte redan har en resursgrupp kan skapa du en innan du skapar ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="91d79-132">If you don't already have a resource group, you must create one before you create your ExpressRoute circuit.</span></span> <span data-ttu-id="91d79-133">Du kan göra det genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-133">You can do so by running hello following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


<span data-ttu-id="91d79-134">hello som följande exempel visar hur toocreate en 200 Mbit/s-ExpressRoute-krets via Equinix i Silicon dal.</span><span class="sxs-lookup"><span data-stu-id="91d79-134">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="91d79-135">Om du använder en annan provider och andra inställningar, i stället informationen när du gör din begäran.</span><span class="sxs-lookup"><span data-stu-id="91d79-135">If you're using a different provider and different settings, substitute that information when you make your request.</span></span> <span data-ttu-id="91d79-136">hello följande är ett exempelbegäran om en ny nyckel:</span><span class="sxs-lookup"><span data-stu-id="91d79-136">hello following is an example request for a new service key:</span></span>

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

<span data-ttu-id="91d79-137">Se till att du anger hello rätt SKU-nivå och SKU-familjen:</span><span class="sxs-lookup"><span data-stu-id="91d79-137">Make sure that you specify hello correct SKU tier and SKU family:</span></span>

* <span data-ttu-id="91d79-138">SKU-nivå bestämmer om en ExpressRoute-standard eller premium-tillägg ett ExpressRoute är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="91d79-138">SKU tier determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="91d79-139">Du kan ange *Standard* tooget hello standard SKU eller *Premium* för hello premium.</span><span class="sxs-lookup"><span data-stu-id="91d79-139">You can specify *Standard* tooget hello standard SKU or *Premium* for hello premium add-on.</span></span>
* <span data-ttu-id="91d79-140">SKU-familjen anger hello fakturering typen.</span><span class="sxs-lookup"><span data-stu-id="91d79-140">SKU family determines hello billing type.</span></span> <span data-ttu-id="91d79-141">Du kan ange *Metereddata* för en plan för förbrukade data och *Unlimiteddata* för en obegränsad plan.</span><span class="sxs-lookup"><span data-stu-id="91d79-141">You can specify *Metereddata* for a metered data plan and *Unlimiteddata* for an unlimited data plan.</span></span> <span data-ttu-id="91d79-142">Du kan ändra hello fakturering typen från *Metereddata* för*Unlimiteddata*, men du kan inte ändra hello typen från *Unlimiteddata* för*Metereddata* .</span><span class="sxs-lookup"><span data-stu-id="91d79-142">You can change hello billing type from *Metereddata* too*Unlimiteddata*, but you can't change hello type from *Unlimiteddata* too*Metereddata*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91d79-143">ExpressRoute-krets kommer att debiteras från hello tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="91d79-143">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="91d79-144">Se till att utföra den här åtgärden när hello anslutningen providern är klar tooprovision hello krets.</span><span class="sxs-lookup"><span data-stu-id="91d79-144">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="91d79-145">hello svaret innehåller hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="91d79-145">hello response contains hello service key.</span></span> <span data-ttu-id="91d79-146">Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-146">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a><span data-ttu-id="91d79-147">4. Visa en lista med alla ExpressRoute-kretsar</span><span class="sxs-lookup"><span data-stu-id="91d79-147">4. List all ExpressRoute circuits</span></span>
<span data-ttu-id="91d79-148">en lista över alla hello ExpressRoute-kretsar som du skapade tooget kör hello **Get-AzureRmExpressRouteCircuit** kommando:</span><span class="sxs-lookup"><span data-stu-id="91d79-148">tooget a list of all hello ExpressRoute circuits that you created, run hello **Get-AzureRmExpressRouteCircuit** command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

<span data-ttu-id="91d79-149">hello svar ser liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91d79-149">hello response will look similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

<span data-ttu-id="91d79-150">Du kan hämta den här informationen när som helst genom att använda hello `Get-AzureRmExpressRouteCircuit` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="91d79-150">You can retrieve this information at any time by using hello `Get-AzureRmExpressRouteCircuit` cmdlet.</span></span> <span data-ttu-id="91d79-151">Gör hello anropa utan parametrar visar alla hello kretsar.</span><span class="sxs-lookup"><span data-stu-id="91d79-151">Making hello call with no parameters lists all hello circuits.</span></span> <span data-ttu-id="91d79-152">Nyckeln för tjänsten visas i hello *ServiceKey* fält:</span><span class="sxs-lookup"><span data-stu-id="91d79-152">Your service key will be listed in hello *ServiceKey* field:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit
```


<span data-ttu-id="91d79-153">hello svar ser liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91d79-153">hello response will look similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


<span data-ttu-id="91d79-154">Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-154">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="91d79-155">5. Skicka hello-nyckel tooyour anslutning leverantör för etablering</span><span class="sxs-lookup"><span data-stu-id="91d79-155">5. Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="91d79-156">*ServiceProviderProvisioningState* innehåller information om hello aktuell status för etablering på hello-leverantör sida.</span><span class="sxs-lookup"><span data-stu-id="91d79-156">*ServiceProviderProvisioningState* provides information about hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="91d79-157">Status innehåller hello tillstånd om hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="91d79-157">Status provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="91d79-158">Mer information om krets etablering tillstånd finns hello [arbetsflöden](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.</span><span class="sxs-lookup"><span data-stu-id="91d79-158">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="91d79-159">När du skapar en ny ExpressRoute-krets blir hello krets i hello följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="91d79-159">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



<span data-ttu-id="91d79-160">hello krets ändras toohello efter tillstånd när hello anslutningen leverantören är i hello innebär att aktivera den åt dig:</span><span class="sxs-lookup"><span data-stu-id="91d79-160">hello circuit will change toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="91d79-161">Om du toobe kan toouse en ExpressRoute-krets, måste den finnas i hello följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="91d79-161">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="91d79-162">6. Kontrollera regelbundet hello status och hello tillståndet för hello krets nyckel</span><span class="sxs-lookup"><span data-stu-id="91d79-162">6. Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="91d79-163">Kontrollera hello status och hello tillståndet för hello krets nyckel kan du veta när leverantören har aktiverat kretsen.</span><span class="sxs-lookup"><span data-stu-id="91d79-163">Checking hello status and hello state of hello circuit key lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="91d79-164">När hello kretsen har konfigurerats, *ServiceProviderProvisioningState* visas som *etablerad*som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-164">After hello circuit has been configured, *ServiceProviderProvisioningState* appears as *Provisioned*, as shown in hello following example:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


<span data-ttu-id="91d79-165">hello svar ser liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91d79-165">hello response will look similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a><span data-ttu-id="91d79-166">7. Skapa din routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="91d79-166">7. Create your routing configuration</span></span>
<span data-ttu-id="91d79-167">Stegvisa instruktioner finns i hello [ExpressRoute-krets routningskonfiguration](expressroute-howto-routing-arm.md) artikel toocreate och ändra krets peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="91d79-167">For step-by-step instructions, see hello [ExpressRoute circuit routing configuration](expressroute-howto-routing-arm.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91d79-168">Dessa anvisningar gäller endast toocircuits som skapas med leverantörer som erbjuder layer 2 tjänster.</span><span class="sxs-lookup"><span data-stu-id="91d79-168">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="91d79-169">Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.</span><span class="sxs-lookup"><span data-stu-id="91d79-169">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="91d79-170">8. Länka ett virtuellt nätverk tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-170">8. Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="91d79-171">Därefter länka ett virtuellt nätverk tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="91d79-171">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="91d79-172">Använd hello [länka virtuella nätverk tooExpressRoute kretsar](expressroute-howto-linkvnet-arm.md) artikeln när du arbetar med hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="91d79-172">Use hello [Linking virtual networks tooExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with hello Resource Manager deployment model.</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="91d79-173">Hämta hello status för en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-173">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="91d79-174">Du kan hämta den här informationen när som helst genom att använda hello **Get-AzureRmExpressRouteCircuit** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="91d79-174">You can retrieve this information at any time by using hello **Get-AzureRmExpressRouteCircuit** cmdlet.</span></span> <span data-ttu-id="91d79-175">Gör hello anropa utan parametrar visar alla hello kretsar.</span><span class="sxs-lookup"><span data-stu-id="91d79-175">Making hello call with no parameters lists all hello circuits.</span></span>

```powershell
Get-AzureRmExpressRouteCircuit
```


<span data-ttu-id="91d79-176">hello svaret blir liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91d79-176">hello response will be similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


<span data-ttu-id="91d79-177">Du kan få information om en specifik ExpressRoute-krets genom att skicka hello resursgruppens namn och krets namn som ett anrop för parametern toohello:</span><span class="sxs-lookup"><span data-stu-id="91d79-177">You can get information on a specific ExpressRoute circuit by passing hello resource group name and circuit name as a parameter toohello call:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


<span data-ttu-id="91d79-178">hello svar ser liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91d79-178">hello response will look similar toohello following example:</span></span>

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


<span data-ttu-id="91d79-179">Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-179">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <span data-ttu-id="91d79-180"><a name="modify"></a>Ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-180"><a name="modify"></a>Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="91d79-181">Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.</span><span class="sxs-lookup"><span data-stu-id="91d79-181">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="91d79-182">Du kan göra hello följande utan avbrott:</span><span class="sxs-lookup"><span data-stu-id="91d79-182">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="91d79-183">Aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="91d79-183">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="91d79-184">Öka hello bandbredden för ExpressRoute-krets såvida det inte finns tillgänglig kapacitet på hello port.</span><span class="sxs-lookup"><span data-stu-id="91d79-184">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="91d79-185">Nedgradera hello bandbredden för en krets stöds inte.</span><span class="sxs-lookup"><span data-stu-id="91d79-185">Downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="91d79-186">Ändra hello avläsning plan från förbrukade Data tooUnlimited Data.</span><span class="sxs-lookup"><span data-stu-id="91d79-186">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="91d79-187">Ändra hello avläsning plan från obegränsad tooMetered Data inte stöds.</span><span class="sxs-lookup"><span data-stu-id="91d79-187">Changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="91d79-188">Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.</span><span class="sxs-lookup"><span data-stu-id="91d79-188">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="91d79-189">Mer information om gränser och begränsningar finns i toohello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="91d79-189">For more information on limits and limitations, refer toohello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="91d79-190">tooenable hello ExpressRoute premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="91d79-190">tooenable hello ExpressRoute premium add-on</span></span>
<span data-ttu-id="91d79-191">Du kan aktivera hello ExpressRoute premium-tillägg för en befintlig krets med hjälp av följande PowerShell-fragment hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-191">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell snippet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

<span data-ttu-id="91d79-192">hello kretsen har nu hello ExpressRoute premium tilläggsfunktioner aktiverad.</span><span class="sxs-lookup"><span data-stu-id="91d79-192">hello circuit will now have hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="91d79-193">Vi börjar fakturering för hello premium-tillägg-funktionen som hello-kommandot har körts.</span><span class="sxs-lookup"><span data-stu-id="91d79-193">We will begin billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="91d79-194">toodisable hello ExpressRoute premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="91d79-194">toodisable hello ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="91d79-195">Den här åtgärden kan misslyckas om du använder resurser som är större än vad som tillåts för standard hello-kretsen.</span><span class="sxs-lookup"><span data-stu-id="91d79-195">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

<span data-ttu-id="91d79-196">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-196">Note hello following:</span></span>

* <span data-ttu-id="91d79-197">Innan du Nedgradera från premium toostandard måste du se till att antalet virtuella nätverk som är länkade hello toohello kretsen är mindre än 10.</span><span class="sxs-lookup"><span data-stu-id="91d79-197">Before you downgrade from premium toostandard, you must ensure that hello number of virtual networks that are linked toohello circuit is less than 10.</span></span> <span data-ttu-id="91d79-198">Om du inte gör det, uppdateringsbegäran misslyckas och vi debiterar du till Premier.</span><span class="sxs-lookup"><span data-stu-id="91d79-198">If you don't do this, your update request fails, and we will bill you at premium rates.</span></span>
* <span data-ttu-id="91d79-199">Du måste ta bort länken alla virtuella nätverk i andra geopolitiska regioner.</span><span class="sxs-lookup"><span data-stu-id="91d79-199">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="91d79-200">Om du inte gör det, uppdateringsbegäran misslyckas och vi debiterar du till Premier.</span><span class="sxs-lookup"><span data-stu-id="91d79-200">If you don't do this, your update request will fail, and we will bill you at premium rates.</span></span>
* <span data-ttu-id="91d79-201">Din routningstabellen måste vara mindre än 4 000 vägar för privat peering.</span><span class="sxs-lookup"><span data-stu-id="91d79-201">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="91d79-202">Om din väg tabell är större än 4 000 vägar, släpper hello BGP-sessionen och reenabled inte tills hello antalet annonserade prefix hamnar under 4 000.</span><span class="sxs-lookup"><span data-stu-id="91d79-202">If your route table size is greater than 4,000 routes, hello BGP session drops and won't be reenabled until hello number of advertised prefixes goes below 4,000.</span></span>

<span data-ttu-id="91d79-203">Du kan inaktivera hello ExpressRoute premium-tillägg för hello befintlig krets med hjälp av hello följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="91d79-203">You can disable hello ExpressRoute premium add-on for hello existing circuit by using hello following PowerShell cmdlet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="91d79-204">tooupdate hello ExpressRoute-kretsen bandbredd</span><span class="sxs-lookup"><span data-stu-id="91d79-204">tooupdate hello ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="91d79-205">Kontrollera hello stöds bandbredd alternativ för providern [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="91d79-205">For supported bandwidth options for your provider, check hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span> <span data-ttu-id="91d79-206">Du kan välja valfri storlek som är större än hello storleken på en befintlig krets.</span><span class="sxs-lookup"><span data-stu-id="91d79-206">You can pick any size greater than hello size of your existing circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91d79-207">Du kan ha toorecreate hello ExpressRoute-krets om det finns för lite kapacitet på befintlig hello-port.</span><span class="sxs-lookup"><span data-stu-id="91d79-207">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="91d79-208">Du kan inte uppgradera hello krets om det finns inga ytterligare kapacitet på den platsen.</span><span class="sxs-lookup"><span data-stu-id="91d79-208">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="91d79-209">Du kan minska hello bandbredden för en ExpressRoute-krets utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="91d79-209">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="91d79-210">Nedgradera bandbredd kräver toodeprovision hello ExpressRoute-krets och etablera en ny ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="91d79-210">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 

<span data-ttu-id="91d79-211">När du har bestämt vilken storlek du måste använda följande kommando tooresize hello kretsen:</span><span class="sxs-lookup"><span data-stu-id="91d79-211">After you decide what size you need, use hello following command tooresize your circuit:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


<span data-ttu-id="91d79-212">Kretsen ska storleksändras på hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="91d79-212">Your circuit will be sized up on hello Microsoft side.</span></span> <span data-ttu-id="91d79-213">Sedan måste du kontakta din anslutning providern tooupdate konfigurationer på deras sida toomatch ändringen.</span><span class="sxs-lookup"><span data-stu-id="91d79-213">Then you must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="91d79-214">När du har gjort det här meddelandet börjar vi fakturering du för hello uppdateras bandbredd alternativet.</span><span class="sxs-lookup"><span data-stu-id="91d79-214">After you make this notification, we will begin billing you for hello updated bandwidth option.</span></span>

### <a name="toomove-hello-sku-from-metered-toounlimited"></a><span data-ttu-id="91d79-215">toomove hello SKU från förbrukade toounlimited</span><span class="sxs-lookup"><span data-stu-id="91d79-215">toomove hello SKU from metered toounlimited</span></span>
<span data-ttu-id="91d79-216">Du kan ändra hello SKU av en ExpressRoute-krets med hjälp av följande PowerShell-fragment hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-216">You can change hello SKU of an ExpressRoute circuit by using hello following PowerShell snippet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a><span data-ttu-id="91d79-217">toocontrol åtkomst toohello klassisk och Resource Manager-miljöer</span><span class="sxs-lookup"><span data-stu-id="91d79-217">toocontrol access toohello classic and Resource Manager environments</span></span>
<span data-ttu-id="91d79-218">Hello-instruktioner i [flytta ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen](expressroute-howto-move-arm.md).</span><span class="sxs-lookup"><span data-stu-id="91d79-218">Review hello instructions in [Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model](expressroute-howto-move-arm.md).</span></span>  

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="91d79-219">Borttagning och tar bort en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-219">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="91d79-220">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-220">Note hello following:</span></span>

* <span data-ttu-id="91d79-221">Du måste ta bort länken alla virtuella nätverk från hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="91d79-221">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="91d79-222">Om den här åtgärden misslyckas, kan du kontrollera toosee om något virtuellt nätverk är länkat toohello krets.</span><span class="sxs-lookup"><span data-stu-id="91d79-222">If this operation fails, check toosee if any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="91d79-223">Om hello ExpressRoute-kretsen tjänstleverantör Etableringsstatus **etablering** eller **etablerad** du måste arbeta med service provider toodeprovision hello kretsen på sidan.</span><span class="sxs-lookup"><span data-stu-id="91d79-223">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="91d79-224">Kommer att fortsätta tooreserve resurser och debitera dig tills hello-leverantör har slutförts avställningsskript hello-krets och meddela oss.</span><span class="sxs-lookup"><span data-stu-id="91d79-224">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="91d79-225">Om hello-leverantör har avetableras hello krets (hello-leverantör Etableringsstatus har angetts för**inte etablerats**) du kan sedan ta bort hello krets.</span><span class="sxs-lookup"><span data-stu-id="91d79-225">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="91d79-226">Detta förhindrar att faktureringen för hello-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-226">This will stop billing for hello circuit</span></span>

<span data-ttu-id="91d79-227">Du kan ta bort ExpressRoute-krets genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="91d79-227">You can delete your ExpressRoute circuit by running hello following command:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a><span data-ttu-id="91d79-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91d79-228">Next steps</span></span>

<span data-ttu-id="91d79-229">Kontrollera att du hello följande när du har skapat kretsen:</span><span class="sxs-lookup"><span data-stu-id="91d79-229">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="91d79-230">Skapa och ändra routning för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-230">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="91d79-231">Länka ditt virtuella nätverk tooyour ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="91d79-231">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)
