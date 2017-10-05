---
title: "Skapa och ändra en Azure ExpressRoute-krets: CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur du skapar, etablera, verifiera, uppdatera, ta bort och ta bort etableringen av en ExpressRoute-krets med hjälp av CLI."
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
ms.openlocfilehash: 1a1c9a96b772868e2c832e9ff57874038c0db2d4
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a><span data-ttu-id="c0d49-103">Skapa och ändra en ExpressRoute-krets med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="c0d49-103">Create and modify an ExpressRoute circuit using CLI</span></span>


<span data-ttu-id="c0d49-104">Den här artikeln beskriver hur du skapar en Azure ExpressRoute-krets med hjälp av kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="c0d49-104">This article describes how to create an Azure ExpressRoute circuit by using the Command Line Interface (CLI).</span></span> <span data-ttu-id="c0d49-105">Den här artikeln visar även hur att kontrollera status, uppdatera eller ta bort och ta bort etableringen av en krets.</span><span class="sxs-lookup"><span data-stu-id="c0d49-105">This article also shows you how to check the status, update, or delete and deprovision a circuit.</span></span> <span data-ttu-id="c0d49-106">Om du vill använda en annan metod för att arbeta med ExpressRoute-kretsar kan du välja artikeln från följande lista:</span><span class="sxs-lookup"><span data-stu-id="c0d49-106">If you want to use a different method to work with ExpressRoute circuits, you can select the article from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c0d49-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c0d49-107">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="c0d49-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0d49-108">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="c0d49-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c0d49-109">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="c0d49-110">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c0d49-110">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="c0d49-111">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="c0d49-111">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a><span data-ttu-id="c0d49-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c0d49-112">Before you begin</span></span>

* <span data-ttu-id="c0d49-113">Innan du börjar ska du installera den senaste versionen av CLI-kommandona (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="c0d49-113">Before beginning, install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="c0d49-114">Information om att installera CLI-kommandona finns i [Installera Azure CLI 2.0](/cli/azure/install-azure-cli) och [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c0d49-114">For information about installing the CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli) and [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>
* <span data-ttu-id="c0d49-115">Granska de [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="c0d49-115">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="c0d49-116">Skapa och etablera en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-116">Create and provision an ExpressRoute circuit</span></span>

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a><span data-ttu-id="c0d49-117">1. Logga in på ditt Azure-konto och välja din prenumeration</span><span class="sxs-lookup"><span data-stu-id="c0d49-117">1. Sign in to your Azure account and select your subscription</span></span>

<span data-ttu-id="c0d49-118">Påbörja din konfiguration genom att logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c0d49-118">To begin your configuration, sign in to your Azure account.</span></span> <span data-ttu-id="c0d49-119">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="c0d49-119">Use the following examples to help you connect:</span></span>

```azurecli
az login
```

<span data-ttu-id="c0d49-120">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="c0d49-120">Check the subscriptions for the account.</span></span>

```azurecli
az account list
```

<span data-ttu-id="c0d49-121">Välj den prenumeration som du vill skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="c0d49-121">Select the subscription for which you want to create an ExpressRoute circuit.</span></span>

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="c0d49-122">2. Hämta en lista över providers som stöds, platser och bandbredder</span><span class="sxs-lookup"><span data-stu-id="c0d49-122">2. Get the list of supported providers, locations, and bandwidths</span></span>

<span data-ttu-id="c0d49-123">Innan du skapar en ExpressRoute-krets måste listan över providers stöds anslutning, platser och alternativ för bandbredd.</span><span class="sxs-lookup"><span data-stu-id="c0d49-123">Before you create an ExpressRoute circuit, you need the list of supported connectivity providers, locations, and bandwidth options.</span></span> <span data-ttu-id="c0d49-124">De CLI kommandot 'az express route lista--leverantörer' returnerar informationen som du ska använda i senare steg:</span><span class="sxs-lookup"><span data-stu-id="c0d49-124">The CLI command 'az network express-route list-service-providers' returns this information, which you’ll use in later steps:</span></span>

```azurecli
az network express-route list-service-providers
```

<span data-ttu-id="c0d49-125">Svaret liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c0d49-125">The response is similar to the following example:</span></span>

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

<span data-ttu-id="c0d49-126">Kontrollera svaret för att se om anslutningsleverantören anges.</span><span class="sxs-lookup"><span data-stu-id="c0d49-126">Check the response to see if your connectivity provider is listed.</span></span> <span data-ttu-id="c0d49-127">Anteckna följande information som du behöver när du skapar en krets:</span><span class="sxs-lookup"><span data-stu-id="c0d49-127">Make a note of the following information, which you will need when you create a circuit:</span></span>

* <span data-ttu-id="c0d49-128">Namn</span><span class="sxs-lookup"><span data-stu-id="c0d49-128">Name</span></span>
* <span data-ttu-id="c0d49-129">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="c0d49-129">PeeringLocations</span></span>
* <span data-ttu-id="c0d49-130">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="c0d49-130">BandwidthsOffered</span></span>

<span data-ttu-id="c0d49-131">Nu är du redo att skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="c0d49-131">You're now ready to create an ExpressRoute circuit.</span></span>

### <a name="3-create-an-expressroute-circuit"></a><span data-ttu-id="c0d49-132">3. Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-132">3. Create an ExpressRoute circuit</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0d49-133">ExpressRoute-krets faktureras från den tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="c0d49-133">Your ExpressRoute circuit is billed from the moment a service key is issued.</span></span> <span data-ttu-id="c0d49-134">Utför den här åtgärden när anslutningen providern är redo att etablera kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-134">Perform this operation when the connectivity provider is ready to provision the circuit.</span></span>
> 
> 

<span data-ttu-id="c0d49-135">Om du inte redan har en resursgrupp kan skapa du en innan du skapar ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-135">If you don't already have a resource group, you must create one before you create your ExpressRoute circuit.</span></span> <span data-ttu-id="c0d49-136">Du kan skapa en resursgrupp genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0d49-136">You can create a resource group by running the following command:</span></span>

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

<span data-ttu-id="c0d49-137">I följande exempel visas hur du skapar en 200 Mbit/s ExpressRoute-krets via Equinix i Silicon dal.</span><span class="sxs-lookup"><span data-stu-id="c0d49-137">The following example shows how to create a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="c0d49-138">Om du använder en annan provider och andra inställningar, i stället informationen när du gör din begäran.</span><span class="sxs-lookup"><span data-stu-id="c0d49-138">If you're using a different provider and different settings, substitute that information when you make your request.</span></span> 

<span data-ttu-id="c0d49-139">Se till att du anger rätt SKU-nivå och SKU-familjen:</span><span class="sxs-lookup"><span data-stu-id="c0d49-139">Make sure that you specify the correct SKU tier and SKU family:</span></span>

* <span data-ttu-id="c0d49-140">SKU-nivå bestämmer om en ExpressRoute-standard eller premium-tillägg ett ExpressRoute är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="c0d49-140">SKU tier determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="c0d49-141">Du kan ange Standard om du för att få standard SKU eller Premium-för premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="c0d49-141">You can specify 'Standard' to get the standard SKU or 'Premium' for the premium add-on.</span></span>
* <span data-ttu-id="c0d49-142">SKU-familjen avgör vilken faktureringsinformation.</span><span class="sxs-lookup"><span data-stu-id="c0d49-142">SKU family determines the billing type.</span></span> <span data-ttu-id="c0d49-143">Du kan ange 'Metereddata' för en plan för förbrukade data och 'Unlimiteddata' för en obegränsad plan.</span><span class="sxs-lookup"><span data-stu-id="c0d49-143">You can specify 'Metereddata' for a metered data plan and 'Unlimiteddata' for an unlimited data plan.</span></span> <span data-ttu-id="c0d49-144">Du kan ändra fakturering typen från 'Metereddata' till 'Unlimiteddata', men du kan inte ändra typen från 'Unlimiteddata' till 'Metereddata'.</span><span class="sxs-lookup"><span data-stu-id="c0d49-144">You can change the billing type from 'Metereddata' to 'Unlimiteddata', but you can't change the type from 'Unlimiteddata' to 'Metereddata'.</span></span>


<span data-ttu-id="c0d49-145">ExpressRoute-krets faktureras från den tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="c0d49-145">Your ExpressRoute circuit is billed from the moment a service key is issued.</span></span> <span data-ttu-id="c0d49-146">Följande exempel är en begäran om en ny nyckel:</span><span class="sxs-lookup"><span data-stu-id="c0d49-146">The following example is a request for a new service key:</span></span>

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

<span data-ttu-id="c0d49-147">Svaret innehåller nyckeln för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c0d49-147">The response contains the service key.</span></span>

### <a name="4-list-all-expressroute-circuits"></a><span data-ttu-id="c0d49-148">4. Visa en lista med alla ExpressRoute-kretsar</span><span class="sxs-lookup"><span data-stu-id="c0d49-148">4. List all ExpressRoute circuits</span></span>

<span data-ttu-id="c0d49-149">Kör kommandot 'az express route nätverkslistan' om du vill hämta en lista över alla ExpressRoute-kretsar som du skapade.</span><span class="sxs-lookup"><span data-stu-id="c0d49-149">To get a list of all the ExpressRoute circuits that you created, run the 'az network express-route list' command.</span></span> <span data-ttu-id="c0d49-150">Du kan hämta den här informationen när som helst genom att använda det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="c0d49-150">You can retrieve this information at any time by using this command.</span></span> <span data-ttu-id="c0d49-151">Om du vill visa alla kretsar ringa utan parametrar.</span><span class="sxs-lookup"><span data-stu-id="c0d49-151">To list all circuits, make the call with no parameters.</span></span>

```azurecli
az network express-route list
```

<span data-ttu-id="c0d49-152">Nyckeln för tjänsten finns i den *ServiceKey* i svaret.</span><span class="sxs-lookup"><span data-stu-id="c0d49-152">Your service key is listed in the *ServiceKey* field of the response.</span></span>

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

<span data-ttu-id="c0d49-153">Du kan få en detaljerad beskrivning av alla parametrar genom att köra kommandot med de '-h ”parametern.</span><span class="sxs-lookup"><span data-stu-id="c0d49-153">You can get detailed descriptions of all the parameters by running the command using the '-h' parameter.</span></span>

```azurecli
az network express-route list -h
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a><span data-ttu-id="c0d49-154">5. Skicka tjänstnyckeln till anslutningsleverantören för etablering</span><span class="sxs-lookup"><span data-stu-id="c0d49-154">5. Send the service key to your connectivity provider for provisioning</span></span>

<span data-ttu-id="c0d49-155">'ServiceProviderProvisioningState' innehåller information om det aktuella tillståndet för etablering på sida-leverantör.</span><span class="sxs-lookup"><span data-stu-id="c0d49-155">'ServiceProviderProvisioningState' provides information about the current state of provisioning on the service-provider side.</span></span> <span data-ttu-id="c0d49-156">Status innehåller tillståndet för Microsoft-sida.</span><span class="sxs-lookup"><span data-stu-id="c0d49-156">The status provides the state on the Microsoft side.</span></span> <span data-ttu-id="c0d49-157">Mer information finns i [arbetsflöden artikel](expressroute-workflows.md#expressroute-circuit-provisioning-states).</span><span class="sxs-lookup"><span data-stu-id="c0d49-157">For more information, see the [Workflows article](expressroute-workflows.md#expressroute-circuit-provisioning-states).</span></span>

<span data-ttu-id="c0d49-158">När du skapar en ny ExpressRoute-krets är kretsen i följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="c0d49-158">When you create a new ExpressRoute circuit, the circuit is in the following state:</span></span>

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

<span data-ttu-id="c0d49-159">Kretsen ändras till följande tillstånd när anslutningen providern håller på att du:</span><span class="sxs-lookup"><span data-stu-id="c0d49-159">The circuit changes to the following state when the connectivity provider is in the process of enabling it for you:</span></span>

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

<span data-ttu-id="c0d49-160">Du kan använda en ExpressRoute-krets, måste den vara i följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="c0d49-160">For you to be able to use an ExpressRoute circuit, it must be in the following state:</span></span>

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a><span data-ttu-id="c0d49-161">6. Regelbundet kontrollera status och tillståndet för nyckeln krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-161">6. Periodically check the status and the state of the circuit key</span></span>

<span data-ttu-id="c0d49-162">Kontrollera status och tillståndet för krets nyckeln får du reda på när din leverantör har aktiverat kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-162">Checking the status and the state of the circuit key lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="c0d49-163">När kretsen har konfigurerats, visas 'ServiceProviderProvisioningState' som etablerad, som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c0d49-163">After the circuit has been configured, 'ServiceProviderProvisioningState' appears as 'Provisioned', as shown in the following example:</span></span>

```azurecli
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

<span data-ttu-id="c0d49-164">Svaret liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c0d49-164">The response is similar to the following example:</span></span>

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

### <a name="7-create-your-routing-configuration"></a><span data-ttu-id="c0d49-165">7. Skapa din routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="c0d49-165">7. Create your routing configuration</span></span>

<span data-ttu-id="c0d49-166">Stegvisa instruktioner finns i [ExpressRoute-krets routningskonfiguration](howto-routing-cli.md) artikeln om du vill skapa och ändra krets peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="c0d49-166">For step-by-step instructions, see the [ExpressRoute circuit routing configuration](howto-routing-cli.md) article to create and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0d49-167">Dessa anvisningar gäller endast för kretsar som skapats med leverantörer som erbjuder layer 2 tjänster.</span><span class="sxs-lookup"><span data-stu-id="c0d49-167">These instructions only apply to circuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="c0d49-168">Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören konfigurerar och hanterar routning för dig.</span><span class="sxs-lookup"><span data-stu-id="c0d49-168">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span>
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="c0d49-169">8. Länka ett virtuellt nätverk till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-169">8. Link a virtual network to an ExpressRoute circuit</span></span>

<span data-ttu-id="c0d49-170">Därefter länka ett virtuellt nätverk till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-170">Next, link a virtual network to your ExpressRoute circuit.</span></span> <span data-ttu-id="c0d49-171">Använd den [länka virtuella nätverk för ExpressRoute-kretsar](howto-linkvnet-cli.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c0d49-171">Use the [Linking virtual networks to ExpressRoute circuits](howto-linkvnet-cli.md) article.</span></span>

## <span data-ttu-id="c0d49-172"><a name="modify"></a>Ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-172"><a name="modify"></a>Modifying an ExpressRoute circuit</span></span>

<span data-ttu-id="c0d49-173">Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-173">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span> <span data-ttu-id="c0d49-174">Du kan göra följande ändringar utan avbrott:</span><span class="sxs-lookup"><span data-stu-id="c0d49-174">You can make the following changes with no downtime:</span></span>

* <span data-ttu-id="c0d49-175">Du kan aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-175">You can enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="c0d49-176">Du kan öka bandbredden för ExpressRoute-krets, under förutsättning att det finns tillgänglig kapacitet på porten.</span><span class="sxs-lookup"><span data-stu-id="c0d49-176">You can increase the bandwidth of your ExpressRoute circuit provided there is capacity available on the port.</span></span> <span data-ttu-id="c0d49-177">Dock stöds nedgradera bandbredden för en krets inte.</span><span class="sxs-lookup"><span data-stu-id="c0d49-177">However, downgrading the bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="c0d49-178">Du kan ändra avläsning planen från förbrukade Data till obegränsad.</span><span class="sxs-lookup"><span data-stu-id="c0d49-178">You can change the metering plan from Metered Data to Unlimited Data.</span></span> <span data-ttu-id="c0d49-179">Men stöds om du ändrar avläsning planen från obegränsad till förbrukade Data inte.</span><span class="sxs-lookup"><span data-stu-id="c0d49-179">However, changing the metering plan from Unlimited Data to Metered Data is not supported.</span></span>
* <span data-ttu-id="c0d49-180">Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.</span><span class="sxs-lookup"><span data-stu-id="c0d49-180">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="c0d49-181">Mer information om gränser och begränsningar finns i [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="c0d49-181">For more information on limits and limitations, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

### <a name="to-enable-the-expressroute-premium-add-on"></a><span data-ttu-id="c0d49-182">Så här aktiverar du ExpressRoute premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="c0d49-182">To enable the ExpressRoute premium add-on</span></span>

<span data-ttu-id="c0d49-183">Du kan aktivera ExpressRoute premium-tillägg för befintliga kretsen med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0d49-183">You can enable the ExpressRoute premium add-on for your existing circuit by using the following command:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

<span data-ttu-id="c0d49-184">Kretsen har nu tilläggsfunktioner för ExpressRoute premium aktiverat.</span><span class="sxs-lookup"><span data-stu-id="c0d49-184">The circuit now has the ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="c0d49-185">Vi börjar fakturering för premium-tillägg-funktionen när kommandot har körts.</span><span class="sxs-lookup"><span data-stu-id="c0d49-185">We begin billing you for the premium add-on capability as soon as the command has successfully run.</span></span>

### <a name="to-disable-the-expressroute-premium-add-on"></a><span data-ttu-id="c0d49-186">Att inaktivera tillägget ExpressRoute premium</span><span class="sxs-lookup"><span data-stu-id="c0d49-186">To disable the ExpressRoute premium add-on</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0d49-187">Den här åtgärden kan misslyckas om du använder resurser som är större än vad som tillåts för standard-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-187">This operation can fail if you're using resources that are greater than what is permitted for the standard circuit.</span></span>
> 
> 

<span data-ttu-id="c0d49-188">Innan du inaktiverar ExpressRoute premium-tillägg, förstå följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="c0d49-188">Before disabling the ExpressRoute premium add-on, understand the following criteria:</span></span>

* <span data-ttu-id="c0d49-189">Innan du Nedgradera från premium till standard måste du kontrollera att du har färre än 10 virtuella nätverk som är kopplad till kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-189">Before you downgrade from premium to standard, you must make sure that you have fewer than 10 virtual networks linked to the circuit.</span></span> <span data-ttu-id="c0d49-190">Om du har fler än 10 din begäran om uppdatering misslyckas och vi debitera dig till Premier.</span><span class="sxs-lookup"><span data-stu-id="c0d49-190">If you have more than 10, your update request fails, and we bill you at premium rates.</span></span>
* <span data-ttu-id="c0d49-191">Du måste ta bort länken alla virtuella nätverk i andra geopolitiska regioner.</span><span class="sxs-lookup"><span data-stu-id="c0d49-191">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="c0d49-192">Om du inte ta bort länken alla virtuella nätverk, din begäran om uppdatering misslyckas och vi debitera dig till Premier.</span><span class="sxs-lookup"><span data-stu-id="c0d49-192">If you don't unlink all your virtual networks, your update request fails and we bill you at premium rates.</span></span>
* <span data-ttu-id="c0d49-193">Din routningstabellen måste vara mindre än 4 000 vägar för privat peering.</span><span class="sxs-lookup"><span data-stu-id="c0d49-193">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="c0d49-194">Om din väg tabell är större än 4 000 vägar, släpper BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-194">If your route table size is greater than 4,000 routes, the BGP session drops.</span></span> <span data-ttu-id="c0d49-195">Sessionen kommer inte att reenabled tills antalet annonserade prefix understiger 4 000.</span><span class="sxs-lookup"><span data-stu-id="c0d49-195">The session won't be reenabled until the number of advertised prefixes is below 4,000.</span></span>

<span data-ttu-id="c0d49-196">Du kan inaktivera ExpressRoute premium-tillägg för befintlig krets med hjälp av följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c0d49-196">You can disable the ExpressRoute premium add-on for the existing circuit by using the following example:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a><span data-ttu-id="c0d49-197">Uppdatera bandbredd för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-197">To update the ExpressRoute circuit bandwidth</span></span>

<span data-ttu-id="c0d49-198">Alternativen stöds bandbredd för providern Kontrollera den [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="c0d49-198">For the supported bandwidth options for your provider, check the [ExpressRoute FAQ](expressroute-faqs.md).</span></span> <span data-ttu-id="c0d49-199">Du kan välja valfri storlek som är större än storleken på en befintlig krets.</span><span class="sxs-lookup"><span data-stu-id="c0d49-199">You can pick any size greater than the size of your existing circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0d49-200">Om det finns för lite kapacitet på befintliga porten, kan du behöva återskapa ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-200">If there is inadequate capacity on the existing port, you may have to recreate the ExpressRoute circuit.</span></span> <span data-ttu-id="c0d49-201">Du kan inte uppgradera kretsen om det finns inga ytterligare kapacitet på den platsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-201">You cannot upgrade the circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="c0d49-202">Du kan minska bandbredden för en ExpressRoute-krets utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="c0d49-202">You cannot reduce the bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="c0d49-203">Nedgradera bandbredd måste du ta bort etableringen av ExpressRoute-kretsen och etablera en ny ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="c0d49-203">Downgrading bandwidth requires you to deprovision the ExpressRoute circuit, and then reprovision a new ExpressRoute circuit.</span></span>
>

<span data-ttu-id="c0d49-204">När du väljer den storlek som du behöver, använder du följande kommando för att ändra storlek på kretsen:</span><span class="sxs-lookup"><span data-stu-id="c0d49-204">After you decide the size you need, use the following command to resize your circuit:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

<span data-ttu-id="c0d49-205">Kretsen storlek på Microsoft-sida.</span><span class="sxs-lookup"><span data-stu-id="c0d49-205">Your circuit is sized up on the Microsoft side.</span></span> <span data-ttu-id="c0d49-206">Därefter måste du kontakta anslutningsleverantören om du vill uppdatera konfigurationer på sidan så att den matchar den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-206">Next, you must contact your connectivity provider to update configurations on their side to match this change.</span></span> <span data-ttu-id="c0d49-207">När du har gjort det här meddelandet börjar fakturering du för alternativet uppdaterade bandbredd.</span><span class="sxs-lookup"><span data-stu-id="c0d49-207">After you make this notification, we begin billing you for the updated bandwidth option.</span></span>

### <a name="to-move-the-sku-from-metered-to-unlimited"></a><span data-ttu-id="c0d49-208">Om du vill flytta SKU: N från förbrukning till obegränsad</span><span class="sxs-lookup"><span data-stu-id="c0d49-208">To move the SKU from metered to unlimited</span></span>

<span data-ttu-id="c0d49-209">Du kan ändra en ExpressRoute-krets SKU med hjälp av följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c0d49-209">You can change the SKU of an ExpressRoute circuit by using the following example:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a><span data-ttu-id="c0d49-210">Styra åtkomsten till klassiskt och Resource Manager-miljöer</span><span class="sxs-lookup"><span data-stu-id="c0d49-210">To control access to the classic and Resource Manager environments</span></span>

<span data-ttu-id="c0d49-211">Granska instruktionerna i [flytta ExpressRoute-kretsar från klassiskt till Resource Manager-distributionsmodellen](expressroute-howto-move-arm.md).</span><span class="sxs-lookup"><span data-stu-id="c0d49-211">Review the instructions in [Move ExpressRoute circuits from the classic to the Resource Manager deployment model](expressroute-howto-move-arm.md).</span></span>

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="c0d49-212">Borttagning och tar bort en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-212">Deprovisioning and deleting an ExpressRoute circuit</span></span>

<span data-ttu-id="c0d49-213">Om du vill ta bort etableringen och tar bort en ExpressRoute-krets, kontrollera att du är medveten om följande villkor:</span><span class="sxs-lookup"><span data-stu-id="c0d49-213">To deprovision and delete an ExpressRoute circuit, make sure you understand the following criteria:</span></span>

* <span data-ttu-id="c0d49-214">Du måste ta bort länken alla virtuella nätverk från ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-214">You must unlink all virtual networks from the ExpressRoute circuit.</span></span> <span data-ttu-id="c0d49-215">Kontrollera om något virtuellt nätverk är kopplade till kretsen om åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="c0d49-215">If this operation fails, check to see if any virtual networks are linked to the circuit.</span></span>
* <span data-ttu-id="c0d49-216">Om leverantören för ExpressRoute-kretsen Etableringsläge är **etablering** eller **etablerad**, du måste arbeta med tjänstleverantören för att ta bort etableringen krets på sidan.</span><span class="sxs-lookup"><span data-stu-id="c0d49-216">If the ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned**, you must work with your service provider to deprovision the circuit on their side.</span></span> <span data-ttu-id="c0d49-217">Vi kan fortsätta att reservera resurser och debitera dig tills tjänstleverantör har slutförts avetablering kretsen och meddela oss.</span><span class="sxs-lookup"><span data-stu-id="c0d49-217">We continue to reserve resources and bill you until the service provider completes deprovisioning the circuit and notifies us.</span></span>
* <span data-ttu-id="c0d49-218">Du kan ta bort kretsen tjänstleverantör har avetableras kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-218">You can delete the circuit if the service provider has deprovisioned the circuit.</span></span> <span data-ttu-id="c0d49-219">När en anslutning är avetableras tjänstleverantör Etableringsstatus som **inte etablerats**.</span><span class="sxs-lookup"><span data-stu-id="c0d49-219">When a circuit is deprovisioned, the service provider provisioning state is set to **Not provisioned**.</span></span> <span data-ttu-id="c0d49-220">Detta stoppar faktureringen för kretsen.</span><span class="sxs-lookup"><span data-stu-id="c0d49-220">This stops billing for the circuit.</span></span>

<span data-ttu-id="c0d49-221">Du kan ta bort ExpressRoute-krets genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c0d49-221">You can delete your ExpressRoute circuit by running the following command:</span></span>

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="c0d49-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0d49-222">Next steps</span></span>

<span data-ttu-id="c0d49-223">När du har skapat kretsen gör att du följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c0d49-223">After you create your circuit, make sure that you do the following tasks:</span></span>

* [<span data-ttu-id="c0d49-224">Skapa och ändra routning för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-224">Create and modify routing for your ExpressRoute circuit</span></span>](howto-routing-cli.md)
* [<span data-ttu-id="c0d49-225">Länka ditt virtuella nätverk till ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="c0d49-225">Link your virtual network to your ExpressRoute circuit</span></span>](howto-linkvnet-cli.md)