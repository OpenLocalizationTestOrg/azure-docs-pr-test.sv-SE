---
title: "Hur tooconfigure routning för en Azure ExpressRoute-krets: CLI | Microsoft Docs"
description: "Den här artikeln hjälper dig att skapa och etablera hello private, offentlig och Microsoft-peering i ExpressRoute-kretsen. Den här artikeln beskriver också hur toocheck hello status, uppdatera eller ta bort peerkopplingar för kretsen."
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
ms.date: 07/31/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: 33130af050045527cdb316e77821c6d101b6a82a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a><span data-ttu-id="e6818-104">Skapa och ändra routning för en ExpressRoute-krets med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="e6818-104">Create and modify routing for an ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="e6818-105">Den här artikeln hjälper dig att skapa och hantera routningskonfiguration för en ExpressRoute-krets i hello Resource Manager-distributionsmodellen med hjälp av CLI.</span><span class="sxs-lookup"><span data-stu-id="e6818-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using CLI.</span></span> <span data-ttu-id="e6818-106">Du kan också kontrollera hello status, uppdatera eller ta bort och ta bort etableringen av peerkopplingar för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="e6818-107">Om du vill toouse en annan metod toowork med kretsen, väljer du en artikel från hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="e6818-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e6818-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e6818-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="e6818-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6818-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="e6818-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e6818-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="e6818-111">Video - privat peering</span><span class="sxs-lookup"><span data-stu-id="e6818-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="e6818-112">Video - offentlig peering</span><span class="sxs-lookup"><span data-stu-id="e6818-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="e6818-113">Video - Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="e6818-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="e6818-114">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="e6818-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="e6818-115">Förutsättningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="e6818-115">Configuration prerequisites</span></span>

* <span data-ttu-id="e6818-116">Innan du börjar installera hello senaste versionen av hello CLI-kommandona (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="e6818-116">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="e6818-117">Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e6818-117">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="e6818-118">Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöde](expressroute-workflows.md) sidor innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="e6818-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflow](expressroute-workflows.md) pages before you begin configuration.</span></span>
* <span data-ttu-id="e6818-119">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="e6818-120">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](howto-circuit-cli.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="e6818-120">Follow hello instructions too[Create an ExpressRoute circuit](howto-circuit-cli.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="e6818-121">Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd du toobe kan toorun hello kommandon i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e6818-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello commands in this article.</span></span>

<span data-ttu-id="e6818-122">Dessa anvisningar gäller endast toocircuits som skapats med leverantörer som erbjuder tjänster nivå 2.</span><span class="sxs-lookup"><span data-stu-id="e6818-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="e6818-123">Om du använder en tjänstprovider som erbjuder hanterade Layer 3-tjänster (vanligtvis en IPVPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.</span><span class="sxs-lookup"><span data-stu-id="e6818-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

<span data-ttu-id="e6818-124">Du kan konfigurera en, två eller alla tre peerkopplingar (Azure privat, Azure offentliga och Microsoft) för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-124">You can configure one, two, or all three peerings (Azure private, Azure public, and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="e6818-125">Du kan konfigurera peerings i valfri ordning.</span><span class="sxs-lookup"><span data-stu-id="e6818-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="e6818-126">Du måste dock se till att du har slutfört hello konfigurationen för varje peering en i taget.</span><span class="sxs-lookup"><span data-stu-id="e6818-126">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="e6818-127">Azures privata peering</span><span class="sxs-lookup"><span data-stu-id="e6818-127">Azure private peering</span></span>

<span data-ttu-id="e6818-128">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure privat peering-konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-128">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="e6818-129">toocreate privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="e6818-129">toocreate Azure private peering</span></span>

1. <span data-ttu-id="e6818-130">Installera hello senaste versionen av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e6818-130">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="e6818-131">Du måste använda hello senaste versionen av hello Azure-kommandoradsgränssnittet (CLI). * Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="e6818-131">You must use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="e6818-132">Välj hello-prenumeration som du vill toocreate ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="e6818-132">Select hello subscription you want toocreate ExpressRoute circuit</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="e6818-133">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-133">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="e6818-134">Följ hello instruktioner toocreate en [ExpressRoute-krets](howto-circuit-cli.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="e6818-134">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="e6818-135">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="e6818-135">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="e6818-136">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e6818-136">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="e6818-137">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e6818-137">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="e6818-138">Kontrollera hello ExpressRoute-kretsen toomake är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="e6818-138">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="e6818-139">Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-139">Use hello following example:</span></span>

  ```azurecli
  az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
  ```

  <span data-ttu-id="e6818-140">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-140">hello response is similar toohello following example:</span></span>

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
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. <span data-ttu-id="e6818-141">Konfigurera privat Azure-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-141">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="e6818-142">Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-142">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="e6818-143">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="e6818-143">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="e6818-144">hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e6818-144">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="e6818-145">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="e6818-145">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="e6818-146">hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e6818-146">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="e6818-147">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="e6818-147">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="e6818-148">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="e6818-148">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="e6818-149">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="e6818-149">AS number for peering.</span></span> <span data-ttu-id="e6818-150">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="e6818-150">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="e6818-151">Du kan använda ett privat AS-tal för den här peeringen.</span><span class="sxs-lookup"><span data-stu-id="e6818-151">You can use a private AS number for this peering.</span></span> <span data-ttu-id="e6818-152">Se till att du inte använder 65515.</span><span class="sxs-lookup"><span data-stu-id="e6818-152">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="e6818-153">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="e6818-153">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="e6818-154">Använd följande exempel tooconfigure Azure privat peering för kretsen hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-154">Use hello following example tooconfigure Azure private peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  <span data-ttu-id="e6818-155">Om du väljer toouse en MD5-hash, Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-155">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="e6818-156">Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.</span><span class="sxs-lookup"><span data-stu-id="e6818-156">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  > 

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="e6818-157">tooview Azure privat peering information</span><span class="sxs-lookup"><span data-stu-id="e6818-157">tooview Azure private peering details</span></span>

<span data-ttu-id="e6818-158">Du kan få konfigurationsinformation genom att använda hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-158">You can get configuration details by using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

<span data-ttu-id="e6818-159">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-159">hello output is similar toohello following example:</span></span>

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePrivatePeering",
  "ipv6PeeringConfig": null,
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePrivatePeering",
  "peerAsn": 7671,
  "peeringType": "AzurePrivatePeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="e6818-160">tooupdate Azure privat peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="e6818-160">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="e6818-161">Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="e6818-161">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="e6818-162">I det här exemplet uppdateras hello VLAN-ID för hello krets från 100 too500.</span><span class="sxs-lookup"><span data-stu-id="e6818-162">In this example, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="e6818-163">toodelete privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="e6818-163">toodelete Azure private peering</span></span>

<span data-ttu-id="e6818-164">Du kan ta bort peering konfigurationen genom att köra hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-164">You can remove your peering configuration by running hello following example:</span></span>

> [!WARNING]
> <span data-ttu-id="e6818-165">Du måste se till att alla virtuella nätverk avlänkas från hello ExpressRoute-krets innan du kör det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="e6818-165">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this example.</span></span> 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="azure-public-peering"></a><span data-ttu-id="e6818-166">Azures offentliga peering</span><span class="sxs-lookup"><span data-stu-id="e6818-166">Azure public peering</span></span>

<span data-ttu-id="e6818-167">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure offentlig peering-konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-167">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="e6818-168">toocreate offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="e6818-168">toocreate Azure public peering</span></span>

1. <span data-ttu-id="e6818-169">Installera hello senaste versionen av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e6818-169">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="e6818-170">Du måste använda hello senaste versionen av hello Azure-kommandoradsgränssnittet (CLI). * Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="e6818-170">You must use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="e6818-171">Välj hello prenumeration som du vill toocreate ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="e6818-171">Select hello subscription for which you want toocreate ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="e6818-172">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-172">Create an ExpressRoute circuit.</span></span>  <span data-ttu-id="e6818-173">Följ hello instruktioner toocreate en [ExpressRoute-krets](howto-circuit-cli.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="e6818-173">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="e6818-174">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3 kan begära du att anslutningsleverantören aktiverar privat Azure-peering för dig.</span><span class="sxs-lookup"><span data-stu-id="e6818-174">If your connectivity provider offers managed Layer 3 services, you can request that your connectivity provider enables Azure private peering for you.</span></span> <span data-ttu-id="e6818-175">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e6818-175">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="e6818-176">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e6818-176">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="e6818-177">Kontrollera hello ExpressRoute-kretsen tooensure etablerats och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="e6818-177">Check hello ExpressRoute circuit tooensure it is provisioned and also enabled.</span></span> <span data-ttu-id="e6818-178">Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-178">Use hello following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="e6818-179">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-179">hello response is similar toohello following example:</span></span>

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
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. <span data-ttu-id="e6818-180">Konfigurera offentlig Azure-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-180">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="e6818-181">Kontrollera att du har följande information innan du fortsätter ytterligare hello.</span><span class="sxs-lookup"><span data-stu-id="e6818-181">Make sure that you have hello following information before you proceed further.</span></span>

  * <span data-ttu-id="e6818-182">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="e6818-182">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="e6818-183">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="e6818-183">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="e6818-184">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="e6818-184">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="e6818-185">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="e6818-185">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="e6818-186">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="e6818-186">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="e6818-187">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="e6818-187">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="e6818-188">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="e6818-188">AS number for peering.</span></span> <span data-ttu-id="e6818-189">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="e6818-189">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="e6818-190">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="e6818-190">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="e6818-191">Kör följande exempel tooconfigure Azure offentlig peering för kretsen hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-191">Run hello following example tooconfigure Azure public peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  <span data-ttu-id="e6818-192">Om du väljer toouse en MD5-hash, Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-192">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="e6818-193">Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.</span><span class="sxs-lookup"><span data-stu-id="e6818-193">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="e6818-194">tooview Azure offentlig peering information</span><span class="sxs-lookup"><span data-stu-id="e6818-194">tooview Azure public peering details</span></span>

<span data-ttu-id="e6818-195">Du kan hämta konfigurationsinformation med hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-195">You can get configuration details using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

<span data-ttu-id="e6818-196">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-196">hello output is similar toohello following example:</span></span>

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePublicPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePublicPeering",
  "peerAsn": 7671,
  "peeringType": "AzurePublicPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="e6818-197">tooupdate Azure offentlig peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="e6818-197">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="e6818-198">Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="e6818-198">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="e6818-199">I det här exemplet uppdateras hello VLAN-ID för hello krets från 200 too600.</span><span class="sxs-lookup"><span data-stu-id="e6818-199">In this example, hello VLAN ID of hello circuit is being updated from 200 too600.</span></span>

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="e6818-200">toodelete offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="e6818-200">toodelete Azure public peering</span></span>

<span data-ttu-id="e6818-201">Du kan ta bort peering konfigurationen genom att köra hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-201">You can remove your peering configuration by running hello following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="microsoft-peering"></a><span data-ttu-id="e6818-202">Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="e6818-202">Microsoft peering</span></span>

<span data-ttu-id="e6818-203">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Microsoft peering konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-203">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e6818-204">Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft hello-peering, även om filter routning inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="e6818-204">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="e6818-205">Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-205">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="e6818-206">Mer information finns i [konfigurera route filter för Microsoft-peering](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e6818-206">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="e6818-207">toocreate Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="e6818-207">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="e6818-208">Installera hello senaste versionen av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e6818-208">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="e6818-209">Använd hello senaste versionen av hello Azure-kommandoradsgränssnittet (CLI). * Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="e6818-209">Use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="e6818-210">Välj hello prenumeration som du vill toocreate ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="e6818-210">Select hello subscription for which you want toocreate ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="e6818-211">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-211">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="e6818-212">Följ hello instruktioner toocreate en [ExpressRoute-krets](howto-circuit-cli.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="e6818-212">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="e6818-213">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="e6818-213">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="e6818-214">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e6818-214">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="e6818-215">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e6818-215">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>

3. <span data-ttu-id="e6818-216">Kontrollera hello ExpressRoute-kretsen toomake är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="e6818-216">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="e6818-217">Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-217">Use hello following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="e6818-218">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-218">hello response is similar toohello following example:</span></span>

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
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. <span data-ttu-id="e6818-219">Konfigurera Microsoft-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="e6818-219">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="e6818-220">Kontrollera att du har hello följande information innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="e6818-220">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="e6818-221">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="e6818-221">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="e6818-222">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="e6818-222">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="e6818-223">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="e6818-223">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="e6818-224">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="e6818-224">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="e6818-225">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="e6818-225">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="e6818-226">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="e6818-226">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="e6818-227">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="e6818-227">AS number for peering.</span></span> <span data-ttu-id="e6818-228">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="e6818-228">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="e6818-229">Annonserade prefix: du måste ange en lista över alla prefix du planerar tooadvertise över hello BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="e6818-229">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="e6818-230">Endast offentliga IP-adressprefix accepteras.</span><span class="sxs-lookup"><span data-stu-id="e6818-230">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="e6818-231">Om du planerar toosend en uppsättning prefix, kan du skicka en kommaavgränsad lista.</span><span class="sxs-lookup"><span data-stu-id="e6818-231">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="e6818-232">Dessa prefix måste vara registrerade tooyou i en RIR / IRR.</span><span class="sxs-lookup"><span data-stu-id="e6818-232">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="e6818-233">**Valfritt -** kunden ASN: Om du är reklam prefix som inte är registrerade toohello peering som du kan ange hello som antalet toowhich som de har registrerats.</span><span class="sxs-lookup"><span data-stu-id="e6818-233">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="e6818-234">Routning namn i registret: Du kan ange hello RIR / IRR mot vilken hello som antalet och prefix har registrerats.</span><span class="sxs-lookup"><span data-stu-id="e6818-234">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="e6818-235">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="e6818-235">**Optional -** An MD5 hash if you choose toouse one.</span></span>

   <span data-ttu-id="e6818-236">Kör följande exempel tooconfigure Microsoft-peering för kretsen hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-236">Run hello following example tooconfigure Microsoft peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="tooget-microsoft-peering-details"></a><span data-ttu-id="e6818-237">tooget Microsoft peering information</span><span class="sxs-lookup"><span data-stu-id="e6818-237">tooget Microsoft peering details</span></span>

<span data-ttu-id="e6818-238">Du kan få konfigurationsinformation genom att använda hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-238">You can get configuration details by using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

<span data-ttu-id="e6818-239">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-239">hello output is similar toohello following example:</span></span>

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzureMicrosoftPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": {
    "advertisedPublicPrefixes": [
        ""
      ],
     "advertisedPublicPrefixesState": "",
     "customerASN": ,
     "routingRegistryName": ""
  }
  "name": "AzureMicrosoftPeering",
  "peerAsn": ,
  "peeringType": "AzureMicrosoftPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="e6818-240">tooupdate Microsoft peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="e6818-240">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="e6818-241">Du kan uppdatera någon del av hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e6818-241">You can update any part of hello configuration.</span></span> <span data-ttu-id="e6818-242">hello annonserade prefix för hello krets uppdateras från 123.1.0.0/24 too124.1.0.0/24 i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e6818-242">hello advertised prefixes of hello circuit are being updated from 123.1.0.0/24 too124.1.0.0/24 in hello following example:</span></span>

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="e6818-243">toodelete Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="e6818-243">toodelete Microsoft peering</span></span>

<span data-ttu-id="e6818-244">Du kan ta bort peering konfigurationen genom att köra hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e6818-244">You can remove your peering configuration by running hello following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="next-steps"></a><span data-ttu-id="e6818-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6818-245">Next steps</span></span>

<span data-ttu-id="e6818-246">Nästa steg [länka VNet-tooan ExpressRoute-krets](howto-linkvnet-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e6818-246">Next step, [Link a VNet tooan ExpressRoute circuit](howto-linkvnet-cli.md).</span></span>

* <span data-ttu-id="e6818-247">Mer information om ExpressRoute-arbetsflöden finns i [ExpressRoute-arbetsflöden](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="e6818-247">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="e6818-248">Mer information om krets-peering finns i [ExpressRoute-kretsar och routningsdomäner](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="e6818-248">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="e6818-249">Mer information om hur du arbetar med virtuella nätverk finns i [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6818-249">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>