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
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>Skapa och ändra en ExpressRoute-krets med hjälp av CLI


Den här artikeln beskriver hur hello toocreate Azure ExpressRoute-kretsen med hjälp av kommandoradsgränssnittet (CLI). Den här artikeln beskriver också hur toocheck hello status, uppdatera, eller ta bort och ta bort etableringen av en krets. Om du vill toouse en annan metod toowork med ExpressRoute-kretsar, kan du välja hello artikel hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a>Innan du börjar

* Innan du börjar installera hello senaste versionen av hello CLI-kommandona (2.0 eller senare). Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli) och [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).
* Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.

## <a name="create-and-provision-an-expressroute-circuit"></a>Skapa och etablera en ExpressRoute-krets

### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a>1. Logga in tooyour Azure-konto och välja din prenumeration

toobegin konfigurationen kan logga in tooyour Azure-konto. Använd hello följande exempel toohelp du ansluta:

```azurecli
az login
```

Kontrollera hello prenumerationer för hello-kontot.

```azurecli
az account list
```

Välj hello prenumeration som du vill toocreate en ExpressRoute-krets.

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>2. Hämta hello lista över providers som stöds, platser och bandbredder

Innan du skapar en ExpressRoute-krets måste hello lista över providers stöds anslutning, platser och alternativ för bandbredd. hello CLI kommandot 'az express route lista--leverantörer' returnerar informationen som du ska använda i senare steg:

```azurecli
az network express-route list-service-providers
```

hello svaret är liknande toohello följande exempel:

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

Kontrollera hello svar toosee om anslutningsleverantören visas. Anteckna hello följande information som du behöver när du skapar en krets:

* Namn
* PeeringLocations
* BandwidthsOffered

Nu är du redo toocreate en ExpressRoute-krets.

### <a name="3-create-an-expressroute-circuit"></a>3. Skapa en ExpressRoute-krets

> [!IMPORTANT]
> ExpressRoute-krets faktureras från hello tidpunkt då en nyckel har utfärdats. Utföra denna åtgärd när hello anslutningen providern är klar tooprovision hello krets.
> 
> 

Om du inte redan har en resursgrupp kan skapa du en innan du skapar ExpressRoute-kretsen. Du kan skapa en resursgrupp genom att köra följande kommando hello:

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

hello som följande exempel visar hur toocreate en 200 Mbit/s-ExpressRoute-krets via Equinix i Silicon dal. Om du använder en annan provider och andra inställningar, i stället informationen när du gör din begäran. 

Se till att du anger hello rätt SKU-nivå och SKU-familjen:

* SKU-nivå bestämmer om en ExpressRoute-standard eller premium-tillägg ett ExpressRoute är aktiverad. Du kan ange ”Standard” tooget hello SKU standard eller Premium-för hello premium.
* SKU-familjen anger hello fakturering typen. Du kan ange 'Metereddata' för en plan för förbrukade data och 'Unlimiteddata' för en obegränsad plan. Du kan ändra hello fakturering typen från 'Metereddata too'Unlimiteddata, men du kan inte ändra hello från 'Unlimiteddata' too'Metereddata'.


ExpressRoute-krets faktureras från hello tidpunkt då en nyckel har utfärdats. hello följande exempel är en begäran om en ny nyckel:

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

hello svaret innehåller hello nyckel.

### <a name="4-list-all-expressroute-circuits"></a>4. Visa en lista med alla ExpressRoute-kretsar

tooget en lista över alla hello ExpressRoute-kretsar som du har skapat kör hello 'az nätverket express route list'-kommando. Du kan hämta den här informationen när som helst genom att använda det här kommandot. toolist alla grupper, se hello anropa utan parametrar.

```azurecli
az network express-route list
```

Nyckeln för tjänsten finns i hello *ServiceKey* i hello svar.

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

Du kan få en detaljerad beskrivning av alla hello parametrar genom körs hello-kommandot med hello '-h-parametern.

```azurecli
az network express-route list -h
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>5. Skicka hello-nyckel tooyour anslutning leverantör för etablering

'ServiceProviderProvisioningState' innehåller information om hello aktuell status för etablering på hello-leverantör sida. hello status innehåller hello tillstånd om hello Microsoft sida. Mer information finns i hello [arbetsflöden artikel](expressroute-workflows.md#expressroute-circuit-provisioning-states).

När du skapar en ny ExpressRoute-krets har hello kretsen hello följande tillstånd:

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

hello krets ändrar toohello efter tillstånd när hello anslutningen leverantören är i hello innebär att aktivera den åt dig:

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

Om du toobe kan toouse en ExpressRoute-krets, måste den finnas i hello följande tillstånd:

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>6. Kontrollera regelbundet hello status och hello tillståndet för hello krets nyckel

Kontrollera hello status och hello tillståndet för hello krets nyckel kan du veta när leverantören har aktiverat kretsen. När hello kretsen har konfigurerats, visas 'ServiceProviderProvisioningState' som etablerad, som visas i följande exempel hello:

```azurecli
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

hello svaret är liknande toohello följande exempel:

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

### <a name="7-create-your-routing-configuration"></a>7. Skapa din routningskonfiguration

Stegvisa instruktioner finns i hello [ExpressRoute-krets routningskonfiguration](howto-routing-cli.md) artikel toocreate och ändra krets peerkopplingar.

> [!IMPORTANT]
> Dessa anvisningar gäller endast toocircuits som skapas med leverantörer som erbjuder layer 2 tjänster. Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören konfigurerar och hanterar routning för dig.
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a>8. Länka ett virtuellt nätverk tooan ExpressRoute-krets

Därefter länka ett virtuellt nätverk tooyour ExpressRoute-kretsen. Använd hello [länka virtuella nätverk tooExpressRoute kretsar](howto-linkvnet-cli.md) artikel.

## <a name="modify"></a>Ändra en ExpressRoute-krets

Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen. Du kan göra följande ändringar utan avbrott hello:

* Du kan aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.
* Du kan öka hello bandbredden för ExpressRoute-krets under förutsättning att det finns tillgänglig kapacitet på hello port. Dock stöds nedgradera hello bandbredden för en krets inte. 
* Du kan ändra hello avläsning planen från förbrukade Data tooUnlimited Data. Dock ändra hello avläsning plan från obegränsad tooMetered Data inte stöds.
* Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.

Mer information om gränser och begränsningar finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).

### <a name="tooenable-hello-expressroute-premium-add-on"></a>tooenable hello ExpressRoute premium-tillägg

Du kan aktivera hello ExpressRoute premium-tillägg för en befintlig krets med hjälp av hello följande kommando:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

hello kretsen har nu hello ExpressRoute premium tilläggsfunktioner aktiverad. Vi börjar fakturering för hello premium-tillägg-funktionen som hello-kommandot har körts.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>toodisable hello ExpressRoute premium-tillägg

> [!IMPORTANT]
> Den här åtgärden kan misslyckas om du använder resurser som är större än vad som tillåts för standard hello-kretsen.
> 
> 

Innan du inaktiverar hello ExpressRoute premium-tillägg, Förstå hello följande kriterier:

* Innan du Nedgradera från premium toostandard måste du kontrollera att du har färre än 10 virtuella nätverk länkade toohello krets. Om du har fler än 10 din begäran om uppdatering misslyckas och vi debitera dig till Premier.
* Du måste ta bort länken alla virtuella nätverk i andra geopolitiska regioner. Om du inte ta bort länken alla virtuella nätverk, din begäran om uppdatering misslyckas och vi debitera dig till Premier.
* Din routningstabellen måste vara mindre än 4 000 vägar för privat peering. Om din väg tabell är större än 4 000 vägar, släpper hello BGP-sessionen. hello sessionen kommer inte att reenabled tills hello antalet annonserade prefix understiger 4 000.

Du kan inaktivera hello ExpressRoute premium-tillägg för hello befintlig krets med hjälp av hello följande exempel:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>tooupdate hello ExpressRoute-kretsen bandbredd

Kontrollera hello hello stöds bandbredd alternativ för providern [ExpressRoute vanliga frågor och svar](expressroute-faqs.md). Du kan välja valfri storlek som är större än hello storleken på en befintlig krets.

> [!IMPORTANT]
> Om det finns för lite kapacitet på befintlig hello-port, kanske toorecreate hello ExpressRoute-kretsen. Du kan inte uppgradera hello krets om det finns inga ytterligare kapacitet på den platsen.
>
> Du kan minska hello bandbredden för en ExpressRoute-krets utan avbrott. Nedgradera bandbredd måste du toodeprovision hello ExpressRoute-krets och etablera en ny ExpressRoute-krets.
>

När du har bestämt hello storlek måste använda följande kommando tooresize hello kretsen:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

Kretsen storlek på hello Microsoft sida. Därefter måste du kontakta din anslutning providern tooupdate konfigurationer på deras sida toomatch ändringen. Vi börjar fakturering du för hello uppdateras bandbredd alternativet när du har gjort det här meddelandet.

### <a name="toomove-hello-sku-from-metered-toounlimited"></a>toomove hello SKU från förbrukade toounlimited

Du kan ändra hello SKU av en ExpressRoute-krets med hello följande exempel:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a>toocontrol åtkomst toohello klassisk och Resource Manager-miljöer

Hello-instruktioner i [flytta ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen](expressroute-howto-move-arm.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Borttagning och tar bort en ExpressRoute-krets

toodeprovision och ta bort en ExpressRoute-krets viktigt att du förstår hello följande kriterier:

* Du måste ta bort länken alla virtuella nätverk från hello ExpressRoute-kretsen. Om den här åtgärden misslyckas, kan du kontrollera toosee om något virtuellt nätverk är länkat toohello krets.
* Om hello ExpressRoute-kretsen tjänstleverantör Etableringsstatus **etablering** eller **etablerad**, du måste arbeta med service provider toodeprovision hello kretsen på sidan. Vi fortsätta tooreserve resurser och debitera dig tills hello-leverantör har slutförts avställningsskript hello-krets och meddela oss.
* Du kan ta bort hello krets hello-leverantör har avetableras hello krets. När en anslutning är avetableras hello-leverantör Etableringsstatus har angetts för**inte etablerats**. Detta stoppar fakturering för hello krets.

Du kan ta bort ExpressRoute-krets genom att köra följande kommando hello:

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Nästa steg

När du har skapat kretsen, se till att du hello följande uppgifter:

* [Skapa och ändra routning för ExpressRoute-krets](howto-routing-cli.md)
* [Länka ditt virtuella nätverk tooyour ExpressRoute-krets](howto-linkvnet-cli.md)
