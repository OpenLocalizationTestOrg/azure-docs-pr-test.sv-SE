---
title: "Skapa och ändra en Azure ExpressRoute-krets: CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate, etablera, verifiera, uppdatera, ta bort och ta bort etableringen av en ExpressRoute-krets med hjälp av CLI."
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
ms.date: 07/25/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 396e325658a59afadb209bb525cbb59ac775ae6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a><span data-ttu-id="c66aa-103">Skapa och ändra en ExpressRoute-krets med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="c66aa-103">Create and modify an ExpressRoute circuit using CLI</span></span>


<span data-ttu-id="c66aa-104">Den här artikeln beskriver hur hello toocreate Azure ExpressRoute-kretsen med hjälp av kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="c66aa-104">This article describes how toocreate an Azure ExpressRoute circuit by using hello Command Line Interface (CLI).</span></span> <span data-ttu-id="c66aa-105">Den här artikeln beskriver också hur toocheck hello status, uppdatera, eller ta bort och ta bort etableringen av en krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-105">This article also shows you how toocheck hello status, update, or delete and deprovision a circuit.</span></span> <span data-ttu-id="c66aa-106">Om du vill toouse en annan metod toowork med ExpressRoute-kretsar, kan du välja hello artikel hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="c66aa-106">If you want toouse a different method toowork with ExpressRoute circuits, you can select hello article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c66aa-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c66aa-107">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="c66aa-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c66aa-108">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="c66aa-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c66aa-109">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="c66aa-110">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c66aa-110">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="c66aa-111">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="c66aa-111">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a><span data-ttu-id="c66aa-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c66aa-112">Before you begin</span></span>

* <span data-ttu-id="c66aa-113">Innan du börjar installera hello senaste versionen av hello CLI-kommandona (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="c66aa-113">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="c66aa-114">Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli) och [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c66aa-114">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli) and [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>
* <span data-ttu-id="c66aa-115">Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="c66aa-115">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="c66aa-116">Skapa och etablera en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c66aa-116">Create and provision an ExpressRoute circuit</span></span>

### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a><span data-ttu-id="c66aa-117">1. Logga in tooyour Azure-konto och välja din prenumeration</span><span class="sxs-lookup"><span data-stu-id="c66aa-117">1. Sign in tooyour Azure account and select your subscription</span></span>

<span data-ttu-id="c66aa-118">toobegin konfigurationen kan logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c66aa-118">toobegin your configuration, sign in tooyour Azure account.</span></span> <span data-ttu-id="c66aa-119">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="c66aa-119">Use hello following examples toohelp you connect:</span></span>

```azurecli
az login
```

<span data-ttu-id="c66aa-120">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="c66aa-120">Check hello subscriptions for hello account.</span></span>

```azurecli
az account list
```

<span data-ttu-id="c66aa-121">Välj hello prenumeration som du vill toocreate en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-121">Select hello subscription for which you want toocreate an ExpressRoute circuit.</span></span>

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="c66aa-122">2. Hämta hello lista över providers som stöds, platser och bandbredder</span><span class="sxs-lookup"><span data-stu-id="c66aa-122">2. Get hello list of supported providers, locations, and bandwidths</span></span>

<span data-ttu-id="c66aa-123">Innan du skapar en ExpressRoute-krets måste hello lista över providers stöds anslutning, platser och alternativ för bandbredd.</span><span class="sxs-lookup"><span data-stu-id="c66aa-123">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span> <span data-ttu-id="c66aa-124">hello CLI kommandot 'az express route lista--leverantörer' returnerar informationen som du ska använda i senare steg:</span><span class="sxs-lookup"><span data-stu-id="c66aa-124">hello CLI command 'az network express-route list-service-providers' returns this information, which you’ll use in later steps:</span></span>

```azurecli
az network express-route list-service-providers
```

<span data-ttu-id="c66aa-125">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c66aa-125">hello response is similar toohello following example:</span></span>

```azurecli
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

<span data-ttu-id="c66aa-126">Kontrollera hello svar toosee om anslutningsleverantören visas.</span><span class="sxs-lookup"><span data-stu-id="c66aa-126">Check hello response toosee if your connectivity provider is listed.</span></span> <span data-ttu-id="c66aa-127">Anteckna hello följande information som du behöver när du skapar en krets:</span><span class="sxs-lookup"><span data-stu-id="c66aa-127">Make a note of hello following information, which you will need when you create a circuit:</span></span>

* <span data-ttu-id="c66aa-128">Namn</span><span class="sxs-lookup"><span data-stu-id="c66aa-128">Name</span></span>
* <span data-ttu-id="c66aa-129">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="c66aa-129">PeeringLocations</span></span>
* <span data-ttu-id="c66aa-130">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="c66aa-130">BandwidthsOffered</span></span>

<span data-ttu-id="c66aa-131">Nu är du redo toocreate en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-131">You're now ready toocreate an ExpressRoute circuit.</span></span>

### <a name="3-create-an-expressroute-circuit"></a><span data-ttu-id="c66aa-132">3. Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c66aa-132">3. Create an ExpressRoute circuit</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c66aa-133">ExpressRoute-krets faktureras från hello tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="c66aa-133">Your ExpressRoute circuit is billed from hello moment a service key is issued.</span></span> <span data-ttu-id="c66aa-134">Utföra denna åtgärd när hello anslutningen providern är klar tooprovision hello krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-134">Perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="c66aa-135">Om du inte redan har en resursgrupp kan skapa du en innan du skapar ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-135">If you don't already have a resource group, you must create one before you create your ExpressRoute circuit.</span></span> <span data-ttu-id="c66aa-136">Du kan skapa en resursgrupp genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="c66aa-136">You can create a resource group by running hello following command:</span></span>

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

<span data-ttu-id="c66aa-137">hello som följande exempel visar hur toocreate en 200 Mbit/s-ExpressRoute-krets via Equinix i Silicon dal.</span><span class="sxs-lookup"><span data-stu-id="c66aa-137">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="c66aa-138">Om du använder en annan provider och andra inställningar, i stället informationen när du gör din begäran.</span><span class="sxs-lookup"><span data-stu-id="c66aa-138">If you're using a different provider and different settings, substitute that information when you make your request.</span></span> 

<span data-ttu-id="c66aa-139">Se till att du anger hello rätt SKU-nivå och SKU-familjen:</span><span class="sxs-lookup"><span data-stu-id="c66aa-139">Make sure that you specify hello correct SKU tier and SKU family:</span></span>

* <span data-ttu-id="c66aa-140">SKU-nivå bestämmer om en ExpressRoute-standard eller premium-tillägg ett ExpressRoute är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="c66aa-140">SKU tier determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="c66aa-141">Du kan ange ”Standard” tooget hello SKU standard eller Premium-för hello premium.</span><span class="sxs-lookup"><span data-stu-id="c66aa-141">You can specify 'Standard' tooget hello standard SKU or 'Premium' for hello premium add-on.</span></span>
* <span data-ttu-id="c66aa-142">SKU-familjen anger hello fakturering typen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-142">SKU family determines hello billing type.</span></span> <span data-ttu-id="c66aa-143">Du kan ange 'Metereddata' för en plan för förbrukade data och 'Unlimiteddata' för en obegränsad plan.</span><span class="sxs-lookup"><span data-stu-id="c66aa-143">You can specify 'Metereddata' for a metered data plan and 'Unlimiteddata' for an unlimited data plan.</span></span> <span data-ttu-id="c66aa-144">Du kan ändra hello fakturering typen från 'Metereddata too'Unlimiteddata, men du kan inte ändra hello från 'Unlimiteddata' too'Metereddata'.</span><span class="sxs-lookup"><span data-stu-id="c66aa-144">You can change hello billing type from 'Metereddata' too'Unlimiteddata', but you can't change hello type from 'Unlimiteddata' too'Metereddata'.</span></span>


<span data-ttu-id="c66aa-145">ExpressRoute-krets faktureras från hello tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="c66aa-145">Your ExpressRoute circuit is billed from hello moment a service key is issued.</span></span> <span data-ttu-id="c66aa-146">hello följande exempel är en begäran om en ny nyckel:</span><span class="sxs-lookup"><span data-stu-id="c66aa-146">hello following example is a request for a new service key:</span></span>

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

<span data-ttu-id="c66aa-147">hello svaret innehåller hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="c66aa-147">hello response contains hello service key.</span></span>

### <a name="4-list-all-expressroute-circuits"></a><span data-ttu-id="c66aa-148">4. Visa en lista med alla ExpressRoute-kretsar</span><span class="sxs-lookup"><span data-stu-id="c66aa-148">4. List all ExpressRoute circuits</span></span>

<span data-ttu-id="c66aa-149">tooget en lista över alla hello ExpressRoute-kretsar som du har skapat kör hello 'az nätverket express route list'-kommando.</span><span class="sxs-lookup"><span data-stu-id="c66aa-149">tooget a list of all hello ExpressRoute circuits that you created, run hello 'az network express-route list' command.</span></span> <span data-ttu-id="c66aa-150">Du kan hämta den här informationen när som helst genom att använda det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="c66aa-150">You can retrieve this information at any time by using this command.</span></span> <span data-ttu-id="c66aa-151">toolist alla grupper, se hello anropa utan parametrar.</span><span class="sxs-lookup"><span data-stu-id="c66aa-151">toolist all circuits, make hello call with no parameters.</span></span>

```azurecli
az network express-route list
```

<span data-ttu-id="c66aa-152">Nyckeln för tjänsten finns i hello *ServiceKey* i hello svar.</span><span class="sxs-lookup"><span data-stu-id="c66aa-152">Your service key is listed in hello *ServiceKey* field of hello response.</span></span>

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

<span data-ttu-id="c66aa-153">Du kan få en detaljerad beskrivning av alla hello parametrar genom körs hello-kommandot med hello '-h-parametern.</span><span class="sxs-lookup"><span data-stu-id="c66aa-153">You can get detailed descriptions of all hello parameters by running hello command using hello '-h' parameter.</span></span>

```azurecli
az network express-route list -h
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="c66aa-154">5. Skicka hello-nyckel tooyour anslutning leverantör för etablering</span><span class="sxs-lookup"><span data-stu-id="c66aa-154">5. Send hello service key tooyour connectivity provider for provisioning</span></span>

<span data-ttu-id="c66aa-155">'ServiceProviderProvisioningState' innehåller information om hello aktuell status för etablering på hello-leverantör sida.</span><span class="sxs-lookup"><span data-stu-id="c66aa-155">'ServiceProviderProvisioningState' provides information about hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="c66aa-156">hello status innehåller hello tillstånd om hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="c66aa-156">hello status provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="c66aa-157">Mer information finns i hello [arbetsflöden artikel](expressroute-workflows.md#expressroute-circuit-provisioning-states).</span><span class="sxs-lookup"><span data-stu-id="c66aa-157">For more information, see hello [Workflows article](expressroute-workflows.md#expressroute-circuit-provisioning-states).</span></span>

<span data-ttu-id="c66aa-158">När du skapar en ny ExpressRoute-krets har hello kretsen hello följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="c66aa-158">When you create a new ExpressRoute circuit, hello circuit is in hello following state:</span></span>

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

<span data-ttu-id="c66aa-159">hello krets ändrar toohello efter tillstånd när hello anslutningen leverantören är i hello innebär att aktivera den åt dig:</span><span class="sxs-lookup"><span data-stu-id="c66aa-159">hello circuit changes toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

<span data-ttu-id="c66aa-160">Om du toobe kan toouse en ExpressRoute-krets, måste den finnas i hello följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="c66aa-160">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="c66aa-161">6. Kontrollera regelbundet hello status och hello tillståndet för hello krets nyckel</span><span class="sxs-lookup"><span data-stu-id="c66aa-161">6. Periodically check hello status and hello state of hello circuit key</span></span>

<span data-ttu-id="c66aa-162">Kontrollera hello status och hello tillståndet för hello krets nyckel kan du veta när leverantören har aktiverat kretsen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-162">Checking hello status and hello state of hello circuit key lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="c66aa-163">När hello kretsen har konfigurerats, visas 'ServiceProviderProvisioningState' som etablerad, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c66aa-163">After hello circuit has been configured, 'ServiceProviderProvisioningState' appears as 'Provisioned', as shown in hello following example:</span></span>

```azurecli
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

<span data-ttu-id="c66aa-164">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c66aa-164">hello response is similar toohello following example:</span></span>

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a><span data-ttu-id="c66aa-165">7. Skapa din routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="c66aa-165">7. Create your routing configuration</span></span>

<span data-ttu-id="c66aa-166">Stegvisa instruktioner finns i hello [ExpressRoute-krets routningskonfiguration](howto-routing-cli.md) artikel toocreate och ändra krets peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="c66aa-166">For step-by-step instructions, see hello [ExpressRoute circuit routing configuration](howto-routing-cli.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c66aa-167">Dessa anvisningar gäller endast toocircuits som skapas med leverantörer som erbjuder layer 2 tjänster.</span><span class="sxs-lookup"><span data-stu-id="c66aa-167">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="c66aa-168">Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören konfigurerar och hanterar routning för dig.</span><span class="sxs-lookup"><span data-stu-id="c66aa-168">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span>
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="c66aa-169">8. Länka ett virtuellt nätverk tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c66aa-169">8. Link a virtual network tooan ExpressRoute circuit</span></span>

<span data-ttu-id="c66aa-170">Därefter länka ett virtuellt nätverk tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-170">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="c66aa-171">Använd hello [länka virtuella nätverk tooExpressRoute kretsar](howto-linkvnet-cli.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c66aa-171">Use hello [Linking virtual networks tooExpressRoute circuits](howto-linkvnet-cli.md) article.</span></span>

## <span data-ttu-id="c66aa-172"><a name="modify"></a>Ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c66aa-172"><a name="modify"></a>Modifying an ExpressRoute circuit</span></span>

<span data-ttu-id="c66aa-173">Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-173">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span> <span data-ttu-id="c66aa-174">Du kan göra följande ändringar utan avbrott hello:</span><span class="sxs-lookup"><span data-stu-id="c66aa-174">You can make hello following changes with no downtime:</span></span>

* <span data-ttu-id="c66aa-175">Du kan aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-175">You can enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="c66aa-176">Du kan öka hello bandbredden för ExpressRoute-krets under förutsättning att det finns tillgänglig kapacitet på hello port.</span><span class="sxs-lookup"><span data-stu-id="c66aa-176">You can increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="c66aa-177">Dock stöds nedgradera hello bandbredden för en krets inte.</span><span class="sxs-lookup"><span data-stu-id="c66aa-177">However, downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="c66aa-178">Du kan ändra hello avläsning planen från förbrukade Data tooUnlimited Data.</span><span class="sxs-lookup"><span data-stu-id="c66aa-178">You can change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="c66aa-179">Dock ändra hello avläsning plan från obegränsad tooMetered Data inte stöds.</span><span class="sxs-lookup"><span data-stu-id="c66aa-179">However, changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="c66aa-180">Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.</span><span class="sxs-lookup"><span data-stu-id="c66aa-180">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="c66aa-181">Mer information om gränser och begränsningar finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="c66aa-181">For more information on limits and limitations, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="c66aa-182">tooenable hello ExpressRoute premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="c66aa-182">tooenable hello ExpressRoute premium add-on</span></span>

<span data-ttu-id="c66aa-183">Du kan aktivera hello ExpressRoute premium-tillägg för en befintlig krets med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c66aa-183">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following command:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

<span data-ttu-id="c66aa-184">hello kretsen har nu hello ExpressRoute premium tilläggsfunktioner aktiverad.</span><span class="sxs-lookup"><span data-stu-id="c66aa-184">hello circuit now has hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="c66aa-185">Vi börjar fakturering för hello premium-tillägg-funktionen som hello-kommandot har körts.</span><span class="sxs-lookup"><span data-stu-id="c66aa-185">We begin billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="c66aa-186">toodisable hello ExpressRoute premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="c66aa-186">toodisable hello ExpressRoute premium add-on</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c66aa-187">Den här åtgärden kan misslyckas om du använder resurser som är större än vad som tillåts för standard hello-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-187">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

<span data-ttu-id="c66aa-188">Innan du inaktiverar hello ExpressRoute premium-tillägg, Förstå hello följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="c66aa-188">Before disabling hello ExpressRoute premium add-on, understand hello following criteria:</span></span>

* <span data-ttu-id="c66aa-189">Innan du Nedgradera från premium toostandard måste du kontrollera att du har färre än 10 virtuella nätverk länkade toohello krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-189">Before you downgrade from premium toostandard, you must make sure that you have fewer than 10 virtual networks linked toohello circuit.</span></span> <span data-ttu-id="c66aa-190">Om du har fler än 10 din begäran om uppdatering misslyckas och vi debitera dig till Premier.</span><span class="sxs-lookup"><span data-stu-id="c66aa-190">If you have more than 10, your update request fails, and we bill you at premium rates.</span></span>
* <span data-ttu-id="c66aa-191">Du måste ta bort länken alla virtuella nätverk i andra geopolitiska regioner.</span><span class="sxs-lookup"><span data-stu-id="c66aa-191">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="c66aa-192">Om du inte ta bort länken alla virtuella nätverk, din begäran om uppdatering misslyckas och vi debitera dig till Premier.</span><span class="sxs-lookup"><span data-stu-id="c66aa-192">If you don't unlink all your virtual networks, your update request fails and we bill you at premium rates.</span></span>
* <span data-ttu-id="c66aa-193">Din routningstabellen måste vara mindre än 4 000 vägar för privat peering.</span><span class="sxs-lookup"><span data-stu-id="c66aa-193">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="c66aa-194">Om din väg tabell är större än 4 000 vägar, släpper hello BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-194">If your route table size is greater than 4,000 routes, hello BGP session drops.</span></span> <span data-ttu-id="c66aa-195">hello sessionen kommer inte att reenabled tills hello antalet annonserade prefix understiger 4 000.</span><span class="sxs-lookup"><span data-stu-id="c66aa-195">hello session won't be reenabled until hello number of advertised prefixes is below 4,000.</span></span>

<span data-ttu-id="c66aa-196">Du kan inaktivera hello ExpressRoute premium-tillägg för hello befintlig krets med hjälp av hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c66aa-196">You can disable hello ExpressRoute premium add-on for hello existing circuit by using hello following example:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="c66aa-197">tooupdate hello ExpressRoute-kretsen bandbredd</span><span class="sxs-lookup"><span data-stu-id="c66aa-197">tooupdate hello ExpressRoute circuit bandwidth</span></span>

<span data-ttu-id="c66aa-198">Kontrollera hello hello stöds bandbredd alternativ för providern [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="c66aa-198">For hello supported bandwidth options for your provider, check hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span> <span data-ttu-id="c66aa-199">Du kan välja valfri storlek som är större än hello storleken på en befintlig krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-199">You can pick any size greater than hello size of your existing circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c66aa-200">Om det finns för lite kapacitet på befintlig hello-port, kanske toorecreate hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-200">If there is inadequate capacity on hello existing port, you may have toorecreate hello ExpressRoute circuit.</span></span> <span data-ttu-id="c66aa-201">Du kan inte uppgradera hello krets om det finns inga ytterligare kapacitet på den platsen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-201">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="c66aa-202">Du kan minska hello bandbredden för en ExpressRoute-krets utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="c66aa-202">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="c66aa-203">Nedgradera bandbredd måste du toodeprovision hello ExpressRoute-krets och etablera en ny ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-203">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit, and then reprovision a new ExpressRoute circuit.</span></span>
>

<span data-ttu-id="c66aa-204">När du har bestämt hello storlek måste använda följande kommando tooresize hello kretsen:</span><span class="sxs-lookup"><span data-stu-id="c66aa-204">After you decide hello size you need, use hello following command tooresize your circuit:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

<span data-ttu-id="c66aa-205">Kretsen storlek på hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="c66aa-205">Your circuit is sized up on hello Microsoft side.</span></span> <span data-ttu-id="c66aa-206">Därefter måste du kontakta din anslutning providern tooupdate konfigurationer på deras sida toomatch ändringen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-206">Next, you must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="c66aa-207">Vi börjar fakturering du för hello uppdateras bandbredd alternativet när du har gjort det här meddelandet.</span><span class="sxs-lookup"><span data-stu-id="c66aa-207">After you make this notification, we begin billing you for hello updated bandwidth option.</span></span>

### <a name="toomove-hello-sku-from-metered-toounlimited"></a><span data-ttu-id="c66aa-208">toomove hello SKU från förbrukade toounlimited</span><span class="sxs-lookup"><span data-stu-id="c66aa-208">toomove hello SKU from metered toounlimited</span></span>

<span data-ttu-id="c66aa-209">Du kan ändra hello SKU av en ExpressRoute-krets med hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c66aa-209">You can change hello SKU of an ExpressRoute circuit by using hello following example:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a><span data-ttu-id="c66aa-210">toocontrol åtkomst toohello klassisk och Resource Manager-miljöer</span><span class="sxs-lookup"><span data-stu-id="c66aa-210">toocontrol access toohello classic and Resource Manager environments</span></span>

<span data-ttu-id="c66aa-211">Hello-instruktioner i [flytta ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen](expressroute-howto-move-arm.md).</span><span class="sxs-lookup"><span data-stu-id="c66aa-211">Review hello instructions in [Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model](expressroute-howto-move-arm.md).</span></span>

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="c66aa-212">Borttagning och tar bort en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c66aa-212">Deprovisioning and deleting an ExpressRoute circuit</span></span>

<span data-ttu-id="c66aa-213">toodeprovision och ta bort en ExpressRoute-krets viktigt att du förstår hello följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="c66aa-213">toodeprovision and delete an ExpressRoute circuit, make sure you understand hello following criteria:</span></span>

* <span data-ttu-id="c66aa-214">Du måste ta bort länken alla virtuella nätverk från hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c66aa-214">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="c66aa-215">Om den här åtgärden misslyckas, kan du kontrollera toosee om något virtuellt nätverk är länkat toohello krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-215">If this operation fails, check toosee if any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="c66aa-216">Om hello ExpressRoute-kretsen tjänstleverantör Etableringsstatus **etablering** eller **etablerad**, du måste arbeta med service provider toodeprovision hello kretsen på sidan.</span><span class="sxs-lookup"><span data-stu-id="c66aa-216">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned**, you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="c66aa-217">Vi fortsätta tooreserve resurser och debitera dig tills hello-leverantör har slutförts avställningsskript hello-krets och meddela oss.</span><span class="sxs-lookup"><span data-stu-id="c66aa-217">We continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="c66aa-218">Du kan ta bort hello krets hello-leverantör har avetableras hello krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-218">You can delete hello circuit if hello service provider has deprovisioned hello circuit.</span></span> <span data-ttu-id="c66aa-219">När en anslutning är avetableras hello-leverantör Etableringsstatus har angetts för**inte etablerats**.</span><span class="sxs-lookup"><span data-stu-id="c66aa-219">When a circuit is deprovisioned, hello service provider provisioning state is set too**Not provisioned**.</span></span> <span data-ttu-id="c66aa-220">Detta stoppar fakturering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="c66aa-220">This stops billing for hello circuit.</span></span>

<span data-ttu-id="c66aa-221">Du kan ta bort ExpressRoute-krets genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="c66aa-221">You can delete your ExpressRoute circuit by running hello following command:</span></span>

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="c66aa-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c66aa-222">Next steps</span></span>

<span data-ttu-id="c66aa-223">När du har skapat kretsen, se till att du hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c66aa-223">After you create your circuit, make sure that you do hello following tasks:</span></span>

* [<span data-ttu-id="c66aa-224">Skapa och ändra routning för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c66aa-224">Create and modify routing for your ExpressRoute circuit</span></span>](howto-routing-cli.md)
* [<span data-ttu-id="c66aa-225">Länka ditt virtuella nätverk tooyour ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c66aa-225">Link your virtual network tooyour ExpressRoute circuit</span></span>](howto-linkvnet-cli.md)
