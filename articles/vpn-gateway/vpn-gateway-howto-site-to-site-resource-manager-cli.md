---
title: "Ansluta din lokala nätverk tooan virtuella Azure-nätverket: plats-till-plats-VPN: CLI | Microsoft Docs"
description: "Steg toocreate en IPSec-anslutning från ditt lokala nätverk tooan virtuella Azure-nätverket via hello offentliga Internet. Dessa steg hjälper dig att skapa en plats-till-plats-anslutning med VPN-gateway med hjälp av CLI."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: c652cf2caf3928cdeb19d7dc329f6db101e5ed90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Skapa ett virtuellt nätverk med en VPN-anslutning från plats till plats med CLI

Den här artikeln visar hur hello toouse Azure CLI toocreate plats-till-plats VPN-gateway-anslutningen från ditt lokala nätverk toohello VNet. hello stegen i den här artikeln gäller toohello Resource Manager-modellen. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:<br>

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Diagram över plats-till-plats-anslutning med VPN-gateway](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel. Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit. Mer information om VPN-gatewayer finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Innan du börjar

Kontrollera att du har uppfyllt hello följande villkor innan du påbörjar konfiguration:

* Kontrollera att du har en kompatibel VPN-enhet och någon som kan tooconfigure den. Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md) för mer information om kompatibla VPN-enheter och enhetskonfiguration.
* Kontrollera att du har en extern offentlig IPv4-adress för VPN-enheten. Den här IP-adressen får inte finnas bakom en NAT.
* Om du är bekant med hello IP-adressintervall som finns i ditt lokala nätverk konfiguration måste du toocoordinate med någon som kan ge de detaljer du. När du skapar den här konfigurationen måste du ange hello IP-adressintervall adressprefix Azure dirigerar tooyour lokal plats. Ingen av hello undernät i ditt lokala nätverk kan över lap med hello virtuella undernät som du vill tooconnect till.
* Kontrollera att du har installerat senaste versionen av hello CLI-kommandona (2.0 eller senare). Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli) och [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).

### <a name="example"></a>Exempelvärden

Du kan använda följande värden toocreate en testmiljö hello eller referera toothese värden toobetter förstå hello exemplen i den här artikeln:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.0.0/24 
GatewaySubnet           = 10.11.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Ansluta tooyour prenumeration

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="rg"></a>2. Skapa en resursgrupp

hello skapas följande exempel en resursgrupp med namnet 'TestRG1' hello 'eastus' plats. Om du redan har en resursgrupp i hello region som du vill toocreate ditt VNet, kan du använda den i stället.

```azurecli
az group create --name TestRG1 --location eastus
```

## <a name="VNet"></a>3. Skapa ett virtuellt nätverk

Om du inte redan har ett virtuellt nätverk, skapar du en med hjälp av hello [az network vnet skapa](/cli/azure/network/vnet#create) kommando. När du skapar ett virtuellt nätverk, se till att hello adressutrymmen som du anger inte överlappa hello-adressutrymmen som finns på ditt lokala nätverk.

hello skapas följande exempel ett virtuellt nätverk med namnet 'TestVNet1' och ett undernät, 'Undernät1'.

```azurecli
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## 4. <a name="gwsub"></a>Skapa hello gateway-undernät

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

För den här konfigurationen behöver du också ett gateway-undernät. hello virtuell nätverksgateway använder ett gateway-undernät som innehåller hello IP-adresser som används av hello VPN gateway-tjänster. När du skapar ett gateway-undernät måste det få namnet ”GatewaySubnet”. Om du ger det något annat namn kan du skapa undernätet, men Azure behandlar det inte som ett gateway-undernät.

hello storleken på hello gateway-undernätet som du anger beror på hello VPN gateway-konfigurationen som du vill toocreate. Det är möjligt toocreate ett gatewayundernät så liten som /29, rekommenderar vi att du skapar ett större undernät som innehåller flera adresser genom att välja minst/27 eller /28. Med ett större gateway-undernät tillåter tillräckligt med IP-adresser tooaccommodate framtida konfigurationer.

Använd hello [az undernät för virtuellt nätverk skapa](/cli/azure/network/vnet/subnet#create) kommandot toocreate hello gateway-undernätet.

```azurecli
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Skapa hello lokal nätverksgateway

hello lokal nätverksgateway refererar vanligtvis tooyour lokal plats. Du namnge hello plats som Azure kan se tooit och sedan ange hello IP-adress för hello lokala VPN-enhet toowhich ska du skapa en anslutning. Du kan också ange hello IP-adressprefix som vidarebefordras via hello VPN-gateway toohello VPN-enhet. hello-adressprefix som du anger är hello prefix finns i ditt lokala nätverk. Om ditt lokala nätverk ändras kan uppdatera du enkelt hello prefix.

Använd hello följande värden:

* Hej *--gateway-ip-adress* är hello IP-adressen för din lokala VPN-enhet. VPN-enheten får inte finnas bakom en NAT.
* Hej *--lokala adressprefix* är din lokala adressutrymmen.

Använd hello [az lokala-nätverksgateway skapa](/cli/azure/network/local-gateway#create) kommandot tooadd en lokal nätverksgateway med flera adressprefix:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Begär en offentlig IP-adress

En VPN-gateway måste ha en offentlig IP-adress. Du först begära hello IP-adressresurs och läser sedan jobbhistorikposten tooit när du skapar din virtuella nätverksgateway. hello IP-adressen tilldelas dynamiskt toohello resurs när hello VPN-gateway har skapats. VPN-gateway stöder för närvarande endast *dynamisk* offentlig IP-adressallokering. Du kan inte begära en statisk offentlig IP-adresstilldelning. Detta innebär dock inte att hello IP-adressen ändras när den har tilldelats tooyour VPN-gateway. hello endast tid hello offentliga IP-adressändringarna är när hello gateway bort och återskapas. Den ändras inte vid storleksändring, återställning eller annat internt underhåll/uppgraderingar av din VPN-gateway.

Använd hello [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create) kommandot toorequest en dynamiska offentliga IP-adress.

```azurecli
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. Skapa hello VPN-gateway

Skapa hello VPN-gateway för virtuellt nätverk. Skapa en VPN-gateway kan ta upp too45 minuter eller mer toocomplete.

Använd hello följande värden:

* Hej *--gateway-typen* för plats-till-plats konfigurationen är *Vpn*. hello gateway-typ är alltid specifika toohello konfiguration som du implementerar. Se [Gatewaytyper](vpn-gateway-about-vpn-gateway-settings.md#gwtype) för mer information.
* Hej *--vpn-typ* kan vara *RouteBased* (kallas tooas en dynamisk Gateway i viss dokumentation) eller *PolicyBased* (kallas tooas en statisk Gateway i vissa dokumentationen). hello-inställningen är särskilda toorequirements för hello-enhet som du ansluter till. Mer information om VPN-gatewaytyper finns i [Om konfigurationsinställningar för VPN-gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* Välj hello Gateway-SKU som du vill toouse. Det finns konfigurationsbegränsningar för vissa SKU:er. Se [Gateway SKU:er](vpn-gateway-about-vpn-gateway-settings.md#gwsku) för mer information.

Skapa hello VPN-gateway med hello [az vnet-nätverksgateway skapa](/cli/azure/network/vnet-gateway#create) kommando. Om du kör det här kommandot med hello – inget - wait-parameter, kan du inte ser några feedback eller utdata. Den här parametern kan hello gateway toocreate i hello bakgrund. Det tar cirka 45 minuter toocreate en gateway.

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <a name="VPNDevice"></a>8. Konfigurera din VPN-enhet

Plats-till-platsanslutningar tooan lokalt nätverk kräver en VPN-enhet. I det här steget konfigurerar du VPN-enheten. När du konfigurerar VPN-enhet behöver du hello följande:

- En delad nyckel. Detta är hello samma delade nyckel som du anger när du skapar din plats-till-plats VPN-anslutning. I vårt exempel använder vi en enkel delad nyckel. Vi rekommenderar att du generera en mer komplex viktiga toouse.
- hello offentliga IP-adressen för din virtuella nätverksgateway. Du kan visa hello offentliga IP-adressen med hjälp av hello Azure-portalen, PowerShell eller CLI. toofind hello offentliga IP-adressen för din virtuella nätverksgateway, Använd hello [az offentliga ip-lista över](/cli/azure/network/public-ip#list) kommando. För att enkelt läsa är hello utdata formaterad toodisplay hello lista över offentliga IP-adresser i tabellformat.

  ```azurecli
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="CreateConnection"></a>9. Skapa hello VPN-anslutning

Skapa hello plats-till-plats VPN-anslutning mellan din virtuella nätverksgateway och din lokala VPN-enhet. Betala per närmare toohello delade nyckelvärde, vilket måste matcha hello konfigurerade delade nyckelvärdet för VPN-enheten.

Skapa hello anslutning med hello [az vpn-nätverksanslutning skapa](/cli/azure/network/vpn-connection#create) kommando.

```azurecli
az network vpn-connection create --name VNet1toSite2 -resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Efter en kort när hello upprättas anslutning.

## <a name="toverify"></a>10. Kontrollera hello VPN-anslutning

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

Om du vill toouse en annan metod tooverify din anslutning finns [verifiera en anslutning för VPN-Gateway](vpn-gateway-verify-connection-resource-manager.md).

## <a name="connectVM"></a>tooconnect tooa virtuell dator

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="tasks"></a>Vanliga åtgärder

Det här avsnittet tar upp vanliga kommandon som är användbara när du arbetar med plats-till-plats-konfigurationer. Hello fullständig lista över nätverk CLI-kommandon, se [Azure CLI - nätverk](/cli/azure/network).

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a>Nästa steg

* När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Information om tvingad tunneltrafik finns i [Om forcerade tunnlar](vpn-gateway-forced-tunneling-rm.md).
* Mer information om aktiv-aktiv-anslutningar med hög tillgänglighet finns i [Anslutning med hög tillgänglighet på flera platser och VNet-till-VNet-anslutning](vpn-gateway-highlyavailable.md).
* Om du vill ha en lista över Azure CLI-nätverkskommandon finns det i [Azure CLI](https://docs.microsoft.com/cli/azure/network).