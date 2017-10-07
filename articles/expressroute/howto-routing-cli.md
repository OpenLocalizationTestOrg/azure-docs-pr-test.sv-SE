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
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a>Skapa och ändra routning för en ExpressRoute-krets med hjälp av CLI

Den här artikeln hjälper dig att skapa och hantera routningskonfiguration för en ExpressRoute-krets i hello Resource Manager-distributionsmodellen med hjälp av CLI. Du kan också kontrollera hello status, uppdatera eller ta bort och ta bort etableringen av peerkopplingar för en ExpressRoute-krets. Om du vill toouse en annan metod toowork med kretsen, väljer du en artikel från hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - privat peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - offentlig peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft-peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Förutsättningar för konfiguration

* Innan du börjar installera hello senaste versionen av hello CLI-kommandona (2.0 eller senare). Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli).
* Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöde](expressroute-workflows.md) sidor innan du börjar konfigurera.
* Du måste ha en aktiv ExpressRoute-krets. Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](howto-circuit-cli.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter. Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd du toobe kan toorun hello kommandon i den här artikeln.

Dessa anvisningar gäller endast toocircuits som skapats med leverantörer som erbjuder tjänster nivå 2. Om du använder en tjänstprovider som erbjuder hanterade Layer 3-tjänster (vanligtvis en IPVPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.

Du kan konfigurera en, två eller alla tre peerkopplingar (Azure privat, Azure offentliga och Microsoft) för en ExpressRoute-krets. Du kan konfigurera peerings i valfri ordning. Du måste dock se till att du har slutfört hello konfigurationen för varje peering en i taget.

## <a name="azure-private-peering"></a>Azures privata peering

Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure privat peering-konfiguration för en ExpressRoute-krets.

### <a name="toocreate-azure-private-peering"></a>toocreate privat Azure-peering

1. Installera hello senaste versionen av Azure CLI. Du måste använda hello senaste versionen av hello Azure-kommandoradsgränssnittet (CLI). * Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.

  ```azurecli
  az login
  ```

  Välj hello-prenumeration som du vill toocreate ExpressRoute-krets

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Skapa en ExpressRoute-krets. Följ hello instruktioner toocreate en [ExpressRoute-krets](howto-circuit-cli.md) och har etablerats av hello anslutning providern.

  Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig. Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt. Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.
3. Kontrollera hello ExpressRoute-kretsen toomake är etablerad och aktiverats. Använd följande exempel hello:

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
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Konfigurera privat Azure-peering för hello krets. Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:

  * Ett/30-undernät för hello primära länken. hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.
  * Ett/30-undernät för hello sekundära länken. hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.
  * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
  * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal. Du kan använda ett privat AS-tal för den här peeringen. Se till att du inte använder 65515.
  * **Valfritt -** en MD5-hash om du väljer toouse en.

  Använd följande exempel tooconfigure Azure privat peering för kretsen hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  Om du väljer toouse en MD5-hash, Använd följande exempel hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.
  > 
  > 

### <a name="tooview-azure-private-peering-details"></a>tooview Azure privat peering information

Du kan få konfigurationsinformation genom att använda hello följande exempel:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

hello utdata är liknande toohello följande exempel:

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

### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate Azure privat peering-konfiguration

Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande exempel. I det här exemplet uppdateras hello VLAN-ID för hello krets från 100 too500.

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="toodelete-azure-private-peering"></a>toodelete privat Azure-peering

Du kan ta bort peering konfigurationen genom att köra hello följande exempel:

> [!WARNING]
> Du måste se till att alla virtuella nätverk avlänkas från hello ExpressRoute-krets innan du kör det här exemplet. 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="azure-public-peering"></a>Azures offentliga peering

Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure offentlig peering-konfiguration för en ExpressRoute-krets.

### <a name="toocreate-azure-public-peering"></a>toocreate offentlig Azure-peering

1. Installera hello senaste versionen av Azure CLI. Du måste använda hello senaste versionen av hello Azure-kommandoradsgränssnittet (CLI). * Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.

  ```azurecli
  az login
  ```

  Välj hello prenumeration som du vill toocreate ExpressRoute-kretsen.

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Skapa en ExpressRoute-krets.  Följ hello instruktioner toocreate en [ExpressRoute-krets](howto-circuit-cli.md) och har etablerats av hello anslutning providern.

  Om anslutningsleverantören erbjuder hanteringstjänster Layer 3 kan begära du att anslutningsleverantören aktiverar privat Azure-peering för dig. Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt. Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.
3. Kontrollera hello ExpressRoute-kretsen tooensure etablerats och aktiverats. Använd följande exempel hello:

  ```azurecli
  az network express-route list
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
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Konfigurera offentlig Azure-peering för hello krets. Kontrollera att du har följande information innan du fortsätter ytterligare hello.

  * Ett/30-undernät för hello primära länken. Detta måste vara ett giltigt offentligt IPv4-prefix.
  * Ett/30-undernät för hello sekundära länken. Detta måste vara ett giltigt offentligt IPv4-prefix.
  * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
  * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal.
  * **Valfritt -** en MD5-hash om du väljer toouse en.

  Kör följande exempel tooconfigure Azure offentlig peering för kretsen hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  Om du väljer toouse en MD5-hash, Använd följande exempel hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.

### <a name="tooview-azure-public-peering-details"></a>tooview Azure offentlig peering information

Du kan hämta konfigurationsinformation med hello följande exempel:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

hello utdata är liknande toohello följande exempel:

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

### <a name="tooupdate-azure-public-peering-configuration"></a>tooupdate Azure offentlig peering-konfiguration

Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande exempel. I det här exemplet uppdateras hello VLAN-ID för hello krets från 200 too600.

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="toodelete-azure-public-peering"></a>toodelete offentlig Azure-peering

Du kan ta bort peering konfigurationen genom att köra hello följande exempel:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="microsoft-peering"></a>Microsoft-peering

Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Microsoft peering konfiguration för en ExpressRoute-krets.

> [!IMPORTANT]
> Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft hello-peering, även om filter routning inte har definierats. Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets. Mer information finns i [konfigurera route filter för Microsoft-peering](how-to-routefilter-powershell.md).
> 
> 

### <a name="toocreate-microsoft-peering"></a>toocreate Microsoft-peering

1. Installera hello senaste versionen av Azure CLI. Använd hello senaste versionen av hello Azure-kommandoradsgränssnittet (CLI). * Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.

  ```azurecli
  az login
  ```

  Välj hello prenumeration som du vill toocreate ExpressRoute-kretsen.

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Skapa en ExpressRoute-krets. Följ hello instruktioner toocreate en [ExpressRoute-krets](howto-circuit-cli.md) och har etablerats av hello anslutning providern.

  Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig. Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt. Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.

3. Kontrollera hello ExpressRoute-kretsen toomake är etablerad och aktiverats. Använd följande exempel hello:

  ```azurecli
  az network express-route list
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
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Konfigurera Microsoft-peering för hello krets. Kontrollera att du har hello följande information innan du fortsätter.

  * Ett/30-undernät för hello primära länken. Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.
  * Ett/30-undernät för hello sekundära länken. Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.
  * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
  * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal.
  * Annonserade prefix: du måste ange en lista över alla prefix du planerar tooadvertise över hello BGP-sessionen. Endast offentliga IP-adressprefix accepteras. Om du planerar toosend en uppsättning prefix, kan du skicka en kommaavgränsad lista. Dessa prefix måste vara registrerade tooyou i en RIR / IRR.
  * **Valfritt -** kunden ASN: Om du är reklam prefix som inte är registrerade toohello peering som du kan ange hello som antalet toowhich som de har registrerats.
  * Routning namn i registret: Du kan ange hello RIR / IRR mot vilken hello som antalet och prefix har registrerats.
  * **Valfritt -** en MD5-hash om du väljer toouse en.

   Kör följande exempel tooconfigure Microsoft-peering för kretsen hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="tooget-microsoft-peering-details"></a>tooget Microsoft peering information

Du kan få konfigurationsinformation genom att använda hello följande exempel:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

hello utdata är liknande toohello följande exempel:

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

### <a name="tooupdate-microsoft-peering-configuration"></a>tooupdate Microsoft peering-konfiguration

Du kan uppdatera någon del av hello konfiguration. hello annonserade prefix för hello krets uppdateras från 123.1.0.0/24 too124.1.0.0/24 i följande exempel hello:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="toodelete-microsoft-peering"></a>toodelete Microsoft-peering

Du kan ta bort peering konfigurationen genom att köra hello följande exempel:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="next-steps"></a>Nästa steg

Nästa steg [länka VNet-tooan ExpressRoute-krets](howto-linkvnet-cli.md).

* Mer information om ExpressRoute-arbetsflöden finns i [ExpressRoute-arbetsflöden](expressroute-workflows.md).
* Mer information om krets-peering finns i [ExpressRoute-kretsar och routningsdomäner](expressroute-circuit-peerings.md).
* Mer information om hur du arbetar med virtuella nätverk finns i [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).