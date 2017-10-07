---
title: "Ansluta din lokala nätverk tooan virtuella Azure-nätverket: plats-till-plats-VPN: PowerShell | Microsoft Docs"
description: "Steg toocreate en IPSec-anslutning från ditt lokala nätverk tooan virtuella Azure-nätverket via hello offentliga Internet. Dessa steg hjälper dig att skapa en plats-till-plats-anslutning med VPN-gateway med hjälp av PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: cb8db1dab3a5488816a7f7e8e63908a4c02f55db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>Skapa ett VNet med en VPN-anslutning för plats-till-plats med hjälp av PowerShell

Den här artikeln visar hur toouse PowerShell toocreate plats-till-plats VPN-gateway-anslutningen från ditt lokala nätverk toohello VNet. hello stegen i den här artikeln gäller toohello Resource Manager-modellen. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassisk portal (klassisk)](vpn-gateway-site-to-site-create.md)
> 
>


En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel. Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit. Mer information om VPN-gatewayer finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).

![Diagram över plats-till-plats-anslutning med VPN-gateway](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-diagram.png)

## <a name="before"></a>Innan du börjar

Kontrollera att du har uppfyllt hello följande villkor innan du börjar din konfiguration:

* Kontrollera att du har en kompatibel VPN-enhet och någon som kan tooconfigure den. Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md) för mer information om kompatibla VPN-enheter och enhetskonfiguration.
* Kontrollera att du har en extern offentlig IPv4-adress för VPN-enheten. Den här IP-adressen får inte finnas bakom en NAT.
* Om du är bekant med hello IP-adressintervall som finns i ditt lokala nätverk konfiguration måste du toocoordinate med någon som kan ge de detaljer du. När du skapar den här konfigurationen måste du ange hello IP-adressintervall adressprefix Azure dirigerar tooyour lokal plats. Ingen av hello undernät i ditt lokala nätverk kan över lap med hello virtuella undernät som du vill tooconnect till.
* Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets. PowerShell-cmdlets uppdateras ofta och du behöver normalt tooupdate din PowerShell-cmdlets tooget hello senaste funktionen. Om du inte uppdaterar din PowerShell-cmdlet misslyckas hello värden som har angetts. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hämtning och installation av PowerShell-cmdlets.

### <a name="example"></a>Exempelvärden

hello exemplen i den här artikeln används hello följande värden. Du kan använda dessa värden toocreate en testmiljö eller referera toothem toobetter förstå hello exemplen i den här artikeln.

```
#Example values

VnetName                = TestVNet1
ResourceGroup           = TestRG1
Location                = East US 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.1.0/28 
GatewaySubnet           = 10.11.0.0/27
LocalNetworkGatewayName = Site2
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24, 20.0.0.0/24
Gateway Name            = VNet1GW
PublicIP                = VNet1GWIP
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2

```


## <a name="Login"></a>1. Ansluta tooyour prenumeration

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="VNet"></a>2. Skapa ett virtuellt nätverk och ett gateway-undernät.

Om du inte redan har ett virtuellt nätverk, skapa ett. När du skapar ett virtuellt nätverk, se till att hello adressutrymmen som du anger inte överlappa hello-adressutrymmen som finns på ditt lokala nätverk.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [No NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="vnet"></a>toocreate ett virtuellt nätverk och ett gateway-undernät

I det här exemplet skapas ett virtuellt nätverk och ett gateway-undernät. Om du redan har ett virtuellt nätverk som du behöver tooadd ett gateway-undernät för att se [tooadd ett gateway-undernätet tooa virtuellt nätverk du redan har skapat](#gatewaysubnet).

Skapa en resursgrupp:

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location 'East US'
```

Skapa ditt virtuella nätverk.

1. Ange hello variabler.

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix 10.11.1.0/28
  ```
2. Skapa hello virtuella nätverk.

  ```powershell
  New-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1 `
  -Location 'East US' -AddressPrefix 10.11.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <a name="gatewaysubnet"></a>tooadd ett virtuellt nätverk för gateway-undernätet tooa du redan har skapat

1. Ange hello variabler.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name TestVet1
  ```
2. Skapa hello gateway-undernätet.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27 -VirtualNetwork $vnet
  ```
3. Ange hello konfiguration.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## 3. <a name="localnet"></a>Skapa hello lokal nätverksgateway

hello lokal nätverksgateway refererar vanligtvis tooyour lokal plats. Du namnge hello plats som Azure kan se tooit och sedan ange hello IP-adress för hello lokala VPN-enhet toowhich ska du skapa en anslutning. Du kan också ange hello IP-adressprefix som vidarebefordras via hello VPN-gateway toohello VPN-enhet. hello-adressprefix som du anger är hello prefix finns i ditt lokala nätverk. Om ditt lokala nätverk ändras kan uppdatera du enkelt hello prefix.

Använd hello följande värden:

* Hej *GatewayIPAddress* är hello IP-adressen för din lokala VPN-enhet. VPN-enheten får inte finnas bakom en NAT.
* Hej *AddressPrefix* är din lokala adressutrymme.

tooadd en lokal nätverksgateway med ett enda adressprefix:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.0.0.0/24'
  ```

tooadd en lokal nätverksgateway med flera adressprefix:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

toomodify IP-adressprefix för din lokala nätverksgateway:<br>
Ibland ändras prefixen för din lokala nätverksgateway. hello steg du vidta toomodify din IP-adress som prefix beror på om du har skapat en VPN-anslutning för gateway. Se hello [ändra IP-adressprefix för en lokal nätverksgateway](#modify) i den här artikeln.

## <a name="PublicIP"></a>4. Begär en offentlig IP-adress

En VPN-gateway måste ha en offentlig IP-adress. Du först begära hello IP-adressresurs och läser sedan jobbhistorikposten tooit när du skapar din virtuella nätverksgateway. hello IP-adressen tilldelas dynamiskt toohello resurs när hello VPN-gateway har skapats. VPN-gateway stöder för närvarande endast *dynamisk* offentlig IP-adressallokering. Du kan inte begära en statisk offentlig IP-adresstilldelning. Detta innebär dock inte att hello IP-adressen ändras när den har tilldelats tooyour VPN-gateway. hello endast tid hello offentliga IP-adressändringarna är när hello gateway bort och återskapas. Den ändras inte vid storleksändring, återställning eller annat internt underhåll/uppgraderingar av din VPN-gateway.

Begära en offentlig IP-adress som ska tilldelas tooyour virtuellt nätverk VPN-gateway.

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>5. Skapa hello gateway IP-adressering konfiguration

gateway-konfiguration för hello definierar hello undernät och hello offentliga IP-adressen toouse. Använd följande exempel toocreate hello gateway-konfiguration:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>6. Skapa hello VPN-gateway

Skapa hello VPN-gateway för virtuellt nätverk. Skapa en VPN-gateway kan ta upp too45 minuter eller mer toocomplete.

Använd hello följande värden:

* Hej *- GatewayType* för plats-till-plats konfigurationen är *Vpn*. hello gateway-typ är alltid specifika toohello konfiguration som du implementerar. Andra gateway-konfigurationer kan kräva -GatewayType ExpressRoute.
* Hej *- VpnType* kan vara *RouteBased* (kallas tooas en dynamisk Gateway i viss dokumentation) eller *PolicyBased* (kallas tooas en statisk Gateway i viss dokumentation ). Mer information om VPN-gatewaytyper finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).
* Välj hello Gateway-SKU som du vill toouse. Det finns konfigurationsbegränsningar för vissa SKU:er. Se [Gateway SKU:er](vpn-gateway-about-vpn-gateway-settings.md#gwsku) för mer information. Om du får ett felmeddelande när du skapar hello VPN-gateway om hello kontrollerar - GatewaySku, du att du har installerat hello senaste versionen av hello PowerShell-cmdlets.

```powershell
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <a name="ConfigureVPNDevice"></a>7. Konfigurera din VPN-enhet

Plats-till-platsanslutningar tooan lokalt nätverk kräver en VPN-enhet. I det här steget konfigurerar du VPN-enheten. När du konfigurerar VPN-enhet behöver du hello följande:

- En delad nyckel. Detta är hello samma delade nyckel som du anger när du skapar din plats-till-plats VPN-anslutning. I vårt exempel använder vi en enkel delad nyckel. Vi rekommenderar att du generera en mer komplex viktiga toouse.
- hello offentliga IP-adressen för din virtuella nätverksgateway. Du kan visa hello offentliga IP-adressen med hjälp av hello Azure-portalen, PowerShell eller CLI. toofind hello offentliga IP-adressen för din virtuella nätverksgateway med hjälp av PowerShell, Använd hello följande exempel:

  ```powershell
  Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG1
  ```

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="CreateConnection"></a>8. Skapa hello VPN-anslutning

Skapa sedan hello plats-till-plats VPN-anslutning mellan din virtuella nätverksgateway och VPN-enheten. Vara säker på att tooreplace hello värden med dina egna. hello delad nyckel måste matcha hello-värde som du använde för din konfiguration för VPN-enhet. Observera att hello '-ConnectionType' för plats-till-plats är *IPsec*.

1. Ange hello variabler.
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
  $local = Get-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1
  ```

2. Skapa hello-anslutning.
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite2 -ResourceGroupName TestRG1 `
  -Location 'East US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

Efter en kort när hello upprättas anslutning.

## <a name="toverify"></a>9. Kontrollera hello VPN-anslutning

Det finns några olika sätt tooverify VPN-anslutning.

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="connectVM"></a>tooconnect tooa virtuell dator

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <a name="modify"></a>Ändra IP-adressprefixen för en lokal nätverksgateway

Ändrar hello IP-adressprefix som du vill att dirigeras tooyour lokal plats, kan du ändra hello lokal nätverksgateway. Det finns två uppsättningar med instruktioner. hello-instruktioner som du väljer beror på om du redan har skapat din gateway-anslutningen.

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Ändra hello gateway IP-adressen för en lokal nätverksgateway

[!INCLUDE [Modify gateway IP address](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Nästa steg

*  När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
