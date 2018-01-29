---
title: "VPN gateway-inställningar för Azure-anslutningar mellan lokala | Microsoft Docs"
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
ms.date: 10/12/2017
ms.author: cherylmc
ms.openlocfilehash: d265c72cfc02710afb630f3b8258602c936d1ebc
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/13/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a>Om konfigurationsinställningar för VPN-Gateway

En VPN-gateway är en typ av virtuell nätverksgateway som skickar krypterad trafik mellan det virtuella nätverket och den lokala platsen i en offentlig anslutning. Du kan också använda en VPN-gateway för att skicka trafik mellan virtuella nätverk i Azure basen.

En VPN-gateway-anslutningen förlitar sig på konfigurationen av flera resurser som innehåller konfigurerbara inställningar. Avsnitten i den här artikeln beskriver de resurser och inställningar som är relaterade till en VPN-gateway för ett virtuellt nätverk som skapats i Resource Manager-modellen. Du kan hitta beskrivningar och Topologidiagram för varje anslutning lösning i den [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel.

## <a name="gwtype"></a>Gateway-typer

Varje virtuellt nätverk kan bara ha en virtuell nätverksgateway för varje typ. När du skapar en virtuell nätverksgateway, måste du se till att gateway-typen är korrekta för din konfiguration.

Tillgängliga värden för - GatewayType är:

* Vpn
* ExpressRoute

Kräver en VPN-gateway den `-GatewayType` *Vpn*.

Exempel:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>Gateway-SKU:er

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-the-gateway-sku"></a>Konfigurera gateway-SKU

#### <a name="azure-portal"></a>Azure Portal

Om du använder Azure-portalen för att skapa en virtuell nätverksgateway Resource Manager kan välja du gateway-SKU: N med hjälp av listmenyn. Alternativen visas motsvarar Gateway-typ och VPN-typ som du väljer.

#### <a name="powershell"></a>PowerShell

Följande PowerShell-exempel anger den `-GatewaySku` som VpnGw1.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="resize"></a>Ändra (storleksändringar) för en gateway-SKU

Om du vill uppgradera din gateway-SKU till en kraftigare SKU kan du använda den `Resize-AzureRmVirtualNetworkGateway` PowerShell-cmdlet. Du kan också nedgradera gateway SKU-storleken som använder denna cmdlet.

Följande PowerShell-exempel visas en gateway-SKU storleksändras till VpnGw2.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <a name="connectiontype"></a>Anslutningstyper

I Resource Manager-distributionsmodellen kräver varje konfiguration av en specifik virtuell gatewaytyp av nätverksanslutning. Tillgängliga PowerShell-värden i Resource Manager för `-ConnectionType` är:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

I följande PowerShell-exempel skapar vi en S2S-anslutning som kräver anslutningstypen *IPsec*.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>VPN-typer

Du måste ange en VPN-typ när du skapar den virtuella nätverksgatewayen för en konfiguration för VPN-gateway. VPN-typ som du väljer beror på topologin anslutning som du vill skapa. Till exempel kräver en P2S-anslutning en RouteBased VPN-typ. En VPN-typ kan också vara beroende av den maskinvara som du använder. S2S konfigurationer kräver en VPN-enhet. Vissa VPN-enheter stöder bara en viss typ av VPN.

VPN-typ du väljer måste uppfylla alla anslutning krav för lösningen du vill skapa. Om du vill skapa en S2S VPN-anslutning för gateway och en P2S VPN gateway-anslutning för samma virtuella nätverk kan du exempelvis använder VPN-typ *RouteBased* eftersom P2S kräver en RouteBased VPN-typ. Du kan också behöva kontrollera att din VPN-enhet stöds en RouteBased VPN-anslutning. 

När en virtuell nätverksgateway har skapats kan ändra du inte typen av VPN. Du måste ta bort den virtuella nätverksgatewayen och skapa en ny. Det finns två typer av VPN:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Följande PowerShell-exempel anger den `-VpnType` som *RouteBased*. När du skapar en gateway måste du kontrollera att -VpnType är rätt för din konfiguration.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Krav för gateway

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Gateway-undernät

Innan du skapar en VPN-gateway, måste du skapa en gateway-undernätet. Gateway-undernätet innehåller IP-adresser som virtuell nätverksgateway virtuella datorer och tjänster. När du skapar din virtuella nätverksgateway kan distribueras till gateway-undernätet gateway VMs och konfigurerad med de nödvändiga inställningarna för VPN-gateway. Aldrig måste du distribuera något annat (till exempel ytterligare virtuella datorer) till gateway-undernätet. Gateway-undernätet måste ha namnet ”GatewaySubnet' ska fungera korrekt. Namngivning av gateway-undernätet 'GatewaySubnet' kan Azure vet att detta är att distribuera virtuell nätverksgateway virtuella datorer och tjänster till undernätet.

När du skapar gatewayundernätet anger du det antal IP-adresser som undernätet innehåller. IP-adresser i gatewayundernätet allokeras till gateway VMs och gateway-tjänster. Vissa konfigurationer kräver flera IP-adresser än andra. Titta på anvisningarna för den konfiguration som du vill skapa och verifiera att gateway-undernätet som du vill skapa uppfyller dessa krav. Dessutom kanske du vill se till gateway-undernätet innehåller IP-adresser räcker för att anpassa framtida ytterligare konfigurationer. Du kan skapa en gateway-undernätet så liten som /29, rekommenderar vi att du skapar ett gateway-undernät för /28 eller större (/ 28 minst/27, /26 osv.). På så sätt kan om du lägger till funktioner i framtiden kommer du inte behöver går sönder din gateway och sedan ta bort och återskapa gateway-undernät så att flera IP-adresser.

I följande exempel för Resource Manager PowerShell visas ett gateway-undernät med namnet GatewaySubnet. Du kan se CIDR-notering anger en minst/27, vilket tillåter tillräckligt med IP-adresser för de flesta konfigurationer som finns för närvarande.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Lokala nätverksgatewayer

När du skapar en VPN-gateway-konfiguration, representerar den lokala nätverksgatewayen ofta den lokala platsen. I den klassiska distributionsmodellen kallades den lokala nätverksgatewayen för Lokal plats. 

Du namnge den lokala nätverksgatewayen, den offentliga IP-adressen för VPN-enhet lokalt och ange adressprefix som finns på den lokala platsen. Azure tittar på mål-adressprefix för nätverkstrafik, tittar du den konfiguration som du har angett för din lokala nätverksgateway och därefter dirigerar paket. Du kan också ange lokala nätverksgatewayer för VNet-till-VNet-konfigurationer som använder en VPN-anslutning för gateway.

Följande PowerShell-exempel skapas en ny lokal nätverksgateway:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Ibland måste du ändra inställningarna för lokala gateway. Till exempel när du lägger till eller ändra adressintervallet, eller om IP-adressen för VPN-enhet ändras. Se [ändra lokala gateway nätverksinställningar med hjälp av PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API: er och PowerShell-cmdlets

Ytterligare tekniska resurser och särskild syntax krav för användning av REST API: er, PowerShell-cmdlets eller Azure CLI för VPN-Gateway-konfigurationer finns på följande sidor:

| **Klassisk** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/azure#networking) |[PowerShell](/powershell/module/azurerm.network#vpn) |
| [REST-API](https://msdn.microsoft.com/library/jj154113) |[REST-API](/rest/api/network/virtualnetworkgateways) |
| Stöds inte | [Azure CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Nästa steg

Mer information om tillgänglig anslutning konfigurationer finns [om VPN-Gateway](vpn-gateway-about-vpngateways.md).
