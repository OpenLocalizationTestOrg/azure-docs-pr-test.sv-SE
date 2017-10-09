---
title: "aaaVPN gateway inställningar för Azure-anslutningar mellan lokala | Microsoft Docs"
description: "Lär dig mer om inställningar för VPN-Gateway för Azure virtuella nätverksgatewayer."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: cherylmc
ms.openlocfilehash: 01229d99fa37e30e00aa00f939f488d631b5593c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a>Om konfigurationsinställningar för VPN-Gateway

En VPN-gateway är en typ av virtuell nätverksgateway som skickar krypterad trafik mellan det virtuella nätverket och den lokala platsen i en offentlig anslutning. Du kan också använda en VPN-gateway toosend trafik mellan virtuella nätverk över hello Azure stamnät.

En VPN-gateway-anslutningen förlitar sig på hello konfiguration av flera resurser som innehåller konfigurerbara inställningar. hello-avsnitt i den här artikeln beskrivs hello resurser och inställningar som gäller tooa VPN-gateway för ett virtuellt nätverk som skapats i Resource Manager-distributionsmodellen. Du kan hitta beskrivningar och Topologidiagram för varje anslutning lösning i hello [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel.

## <a name="gwtype"></a>Gateway-typer

Varje virtuellt nätverk kan bara ha en virtuell nätverksgateway för varje typ. När du skapar en virtuell nätverksgateway, måste du se till att hello gateway-typen är korrekta för din konfiguration.

hello tillgängliga värden för - GatewayType är:

* Vpn
* ExpressRoute

En VPN-gateway kräver hello `-GatewayType` *Vpn*.

Exempel:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>Gateway-SKU:er

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-hello-gateway-sku"></a>Konfigurera hello gateway SKU

#### <a name="azure-portal"></a>Azure Portal

Om du använder hello Azure portal toocreate en virtuell nätverksgateway för hanteraren för filserverresurser, kan du välja hello gateway SKU med hjälp av hello listrutan. hello-alternativ som visas är motsvara toohello Gateway-typ och VPN-typ som du väljer.

#### <a name="powershell"></a>PowerShell

hello följande PowerShell-exempel anger hello `-GatewaySku` som VpnGw1.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="resize"></a>Ändra (storleksändringar) för en gateway-SKU

Om du vill tooupgrade tooa din gateway-SKU kraftigare SKU som du kan använda hello `Resize-AzureRmVirtualNetworkGateway` PowerShell-cmdlet. Du kan också nedgradera hello gateway SKU-storleken som använder denna cmdlet.

hello följande PowerShell-exempel visar en gateway-SKU som storleksändras tooVpnGw2.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <a name="connectiontype"></a>Anslutningstyper

I hello Resource Manager-distributionsmodellen kräver varje konfiguration av en specifik virtuell gatewaytyp av nätverksanslutning. hello tillgängliga Resource Manager PowerShell värden för `-ConnectionType` är:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

I följande exempel PowerShell hello, skapar vi en S2S-anslutning som kräver hello anslutningstypen *IPsec*.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>VPN-typer

Du måste ange en VPN-typ när du skapar hello virtuell nätverksgateway för en konfiguration för VPN-gateway. hello VPN-typ som du väljer beror på topologin för hello-anslutning som du vill toocreate. Till exempel kräver en P2S-anslutning en RouteBased VPN-typ. En VPN-typ kan också bero på hello maskinvara som du använder. S2S konfigurationer kräver en VPN-enhet. Vissa VPN-enheter stöder bara en viss typ av VPN.

hello VPN-typ du väljer måste uppfylla alla hello anslutning för hello lösning du vill toocreate. Till exempel om du vill toocreate en S2S VPN-anslutning för gateway och en P2S VPN gateway-anslutning för hello samma virtuella nätverk, kan du använda VPN-typ *RouteBased* eftersom P2S kräver en RouteBased VPN-typ. Du måste också tooverify att VPN-enheten stöds en RouteBased VPN-anslutning. 

När en virtuell nätverksgateway har skapats kan ändra du inte hello VPN-typ. Du har toodelete hello virtuell nätverksgateway och skapa en ny. Det finns två typer av VPN:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

hello följande PowerShell-exempel anger hello `-VpnType` som *RouteBased*. När du skapar en gateway, måste du se till att hello - VpnType stämmer för din konfiguration.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Krav för gateway

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Gateway-undernät

Innan du skapar en VPN-gateway, måste du skapa en gateway-undernätet. hello gateway-undernätet innehåller hello IP-adresser som hello virtuella nätverkets gateway virtuella datorer och tjänster används. När du skapar din virtuella nätverksgateway gateway VMs är distribuerade toohello gateway-undernät och konfigurerad med hello krävs VPN gateway-inställningar. Aldrig måste du distribuera stor (till exempel ytterligare virtuella datorer) toohello gateway-undernätet. hello gateway-undernätet måste ha namnet ”GatewaySubnet' toowork korrekt. Namnge hello gatewayundernät 'GatewaySubnet' kan Azure vet att det är hello undernät toodeploy hello virtuell nätverksgateway virtuella datorer och tjänster.

När du skapar hello gateway-undernät kan ange du hello antalet IP-adresser som hello undernät innehåller. hello IP-adresser i hello gateway-undernät är allokerade toohello gateway VMs och gateway-tjänster. Vissa konfigurationer kräver flera IP-adresser än andra. Titta på hello instruktioner för konfiguration av hello som du vill använda toocreate och kontrollera att hello gateway-undernätet som du vill toocreate uppfyller dessa krav. Dessutom kanske du vill att din gateway-undernätet innehåller tillräckligt med IP-adresser tooaccommodate möjliga framtida ytterligare konfigurationer toomake. Du kan skapa en gateway-undernätet så liten som /29, rekommenderar vi att du skapar ett gateway-undernät för /28 eller större (/ 28 minst/27, /26 osv.). På så sätt kan om du lägger till funktioner i hello framtida du inte har tootear din gateway och sedan ta bort och återskapa hello gateway-undernätet tooallow för flera IP-adresser.

hello visas följande Resource Manager PowerShell-exempel ett gateway-undernät med namnet GatewaySubnet. Du kan se hello CIDR-notering anger en minst/27, vilket tillåter tillräckligt med IP-adresser för de flesta konfigurationer som finns för närvarande.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Lokala nätverksgatewayer

När du skapar en VPN-gateway-konfiguration, representerar hello lokal nätverksgateway ofta den lokala platsen. Hello klassiska distributionsmodellen var hello lokal nätverksgateway som anges tooas en lokal plats. 

Namnge hello lokal nätverksgateway, hello offentliga IP-adressen för hello lokala VPN-enhet och ange hello adressprefix som finns på hello lokal plats. Azure tittar på hello mål-adressprefix för nätverkstrafik, tittar hello-konfiguration som du har angett för din lokala nätverksgateway och därefter dirigerar paket. Du kan också ange lokala nätverksgatewayer för VNet-till-VNet-konfigurationer som använder en VPN-anslutning för gateway.

hello följande PowerShell-exempel skapar en ny lokal nätverksgateway:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Ibland behöver toomodify hello lokala gateway nätverksinställningar. Till exempel när du lägger till eller ändra hello adressintervall, eller om hello IP-adress för hello VPN-enhet ändras. Du kan ändra de här inställningarna i hello klassiska portalen på sidan för hello lokala nätverk för ett klassiska VNet. För hanteraren för filserverresurser, se [ändra lokala gateway nätverksinställningar med hjälp av PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API: er och PowerShell-cmdlets

Ytterligare tekniska resurser och särskild syntax krav för användning av REST API: er, PowerShell-cmdlets eller Azure CLI för VPN-Gateway-konfigurationer finns hello följande sidor:

| **Klassisk** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/azure#networking) |[PowerShell](/powershell/module/azurerm.network#vpn) |
| [REST-API](https://msdn.microsoft.com/library/jj154113) |[REST-API](/rest/api/network/virtualnetworkgateways) |
| Stöds inte | [Azure CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Nästa steg

Mer information om tillgänglig anslutning konfigurationer finns [om VPN-Gateway](vpn-gateway-about-vpngateways.md).