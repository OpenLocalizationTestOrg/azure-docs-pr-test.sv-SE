---
title: 'Anslut Azure VPN-gatewayer toomultiple lokalt principbaserad VPN-enheter: Azure Resource Manager: PowerShell | Microsoft Docs'
description: "Den här artikeln beskriver hur du konfigurerar Azure ruttbaserad VPN-gateway toomultiple principbaserad VPN-enheter med Azure Resource Manager och PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/27/2017
ms.author: yushwang
ms.openlocfilehash: 866c78d96305207106a66cc3300c355e4b6bfbb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-azure-vpn-gateways-toomultiple-on-premises-policy-based-vpn-devices-using-powershell"></a>Anslut Azure VPN-gatewayer toomultiple lokala principbaserad VPN-enheter med hjälp av PowerShell

Den här artikeln hjälper dig att konfigurera en Azure ruttbaserad VPN-gateway tooconnect toomultiple lokalt principbaserad VPN-enheter genom att använda anpassade IPsec/IKE-principer för S2S VPN-anslutningar.

## <a name="about-policy-based-and-route-based-vpn-gateways"></a>Om principbaserade och ruttbaserade VPN-gatewayer

Princip - *kontra* ruttbaserad VPN-enheter skiljer sig åt i hur hello IPSec-trafik väljare anges för en anslutning:

* **Principbaserad** VPN-enheter använder hello kombinationer av adressprefix från båda nätverk toodefine hur krypterade/dekrypteras trafiken via IPsec-tunnlar. Den bygger vanligtvis på brandväggsenheter som utför filtrering av nätverkspaket. IPSec-tunneln kryptering och dekryptering läggs toohello paketfilter och bearbetningsmotorn.
* **Ruttbaserad** VPN-enheter med hjälp av alla-till-alla (jokertecken) trafik väljare och låter routning/vidarebefordran tabeller direkt trafik toodifferent IPSec-tunnlar. Den bygger vanligtvis på routern plattformar, där varje IPsec-tunneln modelleras som ett nätverksgränssnitt eller VTI (virtuell tunnelgränssnitt).

hello Markera följande diagram hello två modeller:

### <a name="policy-based-vpn-example"></a>Principbaserad VPN-exempel
![principbaserad](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Ruttbaserad VPN-exempel
![ruttbaserad](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>Azure-stöd för principbaserad VPN
Azure stöder för närvarande, båda lägena för VPN-gatewayer: ruttbaserad VPN-gatewayer och policy-baserad VPN-gatewayer. De bygger på olika interna plattformar, vilket resulterar i olika specifikationer:

|                          | **PolicyBased VPN-Gateway** | **RouteBased VPN-Gateway**               |
| ---                      | ---                         | ---                                      |
| **Azure Gateway-SKU**    | Basic                       | Basic, Standard, HighPerformance         |
| **IKE-version**          | IKEv1                       | IKEv2                                    |
| **Max. S2S-anslutningar** | **1**                       | Basic-standarden: 10<br> HighPerformance: 30 |
|                          |                             |                                          |

Med hello anpassad IPsec/IKE-princip kan du nu konfigurera Azure ruttbaserad VPN-gatewayer toouse prefix-baserad trafik väljare med alternativet ”**PolicyBasedTrafficSelectors**”, tooconnect tooon lokala principbaserad VPN-enheter. Den här funktionen kan du tooconnect från Azure-nätverk och VPN-gateway toomultiple lokala principbaserad VPN-brandväggen enheter, ta bort hello enda anslutningsgränsen från hello aktuella Azure principbaserade VPN-gatewayer.

> [!IMPORTANT]
> 1. tooenable den här anslutningen dina lokala principbaserad VPN-enheter måste ha stöd för **IKEv2** tooconnect toohello Azure ruttbaserad VPN-gatewayer. Kontrollera din VPN-enhetsspecifikationer.
> 2. hello lokala nätverk som ansluter via principbaserad VPN-enheter med den här funktionen kan bara ansluta toohello virtuella Azure-nätverk; **kan inte de transit tooother lokala nätverk eller virtuella nätverk via hello samma Azure VPN-gateway**.
> 3. hello konfigurationsalternativet är en del av hello anpassade IPsec/IKE-princip. Om du aktiverar alternativ för hello principbaserad trafik väljare, måste du ange hello hela policyn (IPsec/IKE algoritmer för kryptering och integritet, viktiga fördelar och säkerhetsassociationer).

hello följande diagram visar varför transitroutning via Azure VPN-gateway fungerar inte med hello principbaserad alternativet:

![principbaserad överföring](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Enligt i hello diagram har hello Azure VPN-gateway trafik väljare från hello virtuellt nätverk tooeach hello-prefix för lokala nätverket, men inte hello anslutningen mellan prefix. Till exempel lokal plats 2, 3, och plats 4 kan varje kommunicera tooVNet1 respektive, men kan inte ansluta via hello Azure VPN gateway tooeach andra. hello diagram visar hello korskoppling trafik väljare som inte är tillgängliga i hello Azure VPN-gatewayen under den här konfigurationen.

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a>Konfigurera principbaserad trafik väljare på en anslutning

hello instruktionerna i den här artikeln Följ hello samma exempel som beskrivs i [konfigurera IPsec/IKE-principen för S2S eller VNet-till-VNet-anslutningar](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish en S2S VPN-anslutning. Detta visas i följande diagram hello:

![s2s-princip](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

Hej arbetsflöde tooenable den här anslutningen:
1. Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway för anslutningen mellan platser
2. Skapa en IPsec/IKE-princip
3. Använd hello principen när du skapar en S2S eller VNet-till-VNet-anslutning och **aktivera hello principbaserad trafik väljare** på hello-anslutning
4. Om hello anslutning redan har skapats, kan du tillämpa eller uppdatera hello princip tooan befintlig anslutning

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a>Aktivera principbaserad trafik väljare på en anslutning

Kontrollera att du har slutfört [del 3 hello konfigurera IPsec/IKE princip artikel](vpn-gateway-ipsecikepolicy-rm-powershell.md) för det här avsnittet. följande exempel använder hello hello samma parametrar och steg:

### <a name="step-1---create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>Steg 1 – Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway

#### <a name="1-declare-your-variables--connect-tooyour-subscription"></a>1. Deklarera variablerna & ansluter tooyour prenumeration
Den här övningen starta genom att deklarera våra variabler. Vara säker på att tooreplace hello värden med dina egna när du konfigurerar för produktion.

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```
toouse hello Resource Manager cmdlets, se till att du växla tooPowerShell läge. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

Öppna PowerShell-konsolen och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>2. Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway
hello följande exempel skapar hello virtuellt nätverk, TestVNet1 med tre undernät och hello VPN-gateway. När du ersätter värdena är det viktigt att du alltid namnger gateway-undernätet specifikt 'GatewaySubnet'. Om du ger det något annat namn går det inte att skapa gatewayen.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a>Steg 2 – Skapa en S2S VPN-anslutning med en IPsec/IKE-princip

#### <a name="1-create-an-ipsecike-policy"></a>1. Skapa en IPsec/IKE-princip

> [!IMPORTANT]
> Du måste toocreate en IPsec/IKE-princip i ordning tooenable ”UsePolicyBasedTrafficSelectors” alternativet på hello-anslutning.

hello skapar följande exempel en IPsec/IKE-princip med dessa algoritmer och parametrar:
* IKEv2: AES256, SHA384 DHGroup24
* IPsec: AES256, SHA256, PFS24 SA livstid 3600 sekunder & 2048KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a>2. Skapa hello S2S VPN-anslutning med principbaserad trafik väljare och IPsec/IKE-princip
Skapa en S2S VPN-anslutning och tillämpa hello IPsec/IKE-princip skapas i hello föregående steg. Tänk på att hello ytterligare parameter ”-UsePolicyBasedTrafficSelectors $True” som gör det möjligt för principbaserad trafik väljare på hello-anslutning.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

När du har slutfört steg hello hello S2S VPN-anslutning använder hello IPsec/IKE princip har definierats, och aktivera principbaserad trafik väljare på hello-anslutning. Du kan upprepa samma steg tooadd fler anslutningar tooadditional lokalt principbaserad VPN-enheter från hello hello samma Azure VPN-gateway.

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a>Uppdatera principbaserad trafik väljare för en anslutning
hello sista avsnittet visar hur tooupdate hello principbaserad trafik väljare alternativ för en befintlig S2S VPN-anslutning.

### <a name="1-get-hello-connection"></a>1. Hämta hello-anslutning
Hämta hello anslutning resurs.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-hello-policy-based-traffic-selectors-option"></a>2. Markera alternativet för hello principbaserad trafik väljare
hello visar följande rad om hello principbaserad trafik väljare som används för hello-anslutningen:

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

Om hello rad returnerar ”**SANT**”, sedan principbaserad trafik väljare konfigureras på hello anslutning; annars returneras ”**FALSKT**”.

### <a name="3-update-hello-policy-based-traffic-selectors-on-a-connection"></a>3. Uppdatera hello principbaserad trafik väljare på en anslutning
När du har fått hello anslutning resurs, kan du aktivera eller inaktivera hello-alternativet.

#### <a name="disable-usepolicybasedtrafficselectors"></a>Inaktivera UsePolicyBasedTrafficSelectors
hello följande exempel inaktiverar hello principbaserad trafik väljare alternativet, men lämnar kvar hello IPsec/princip oförändrade:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a>Aktivera UsePolicyBasedTrafficSelectors
hello följande exempel aktiveras hello principbaserad trafik väljare alternativet, men lämnar kvar hello IPsec/princip oförändrade:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a>Nästa steg
När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.

Granska även [konfigurera IPsec/IKE-principen för S2S VPN- eller VNet-till-VNet-anslutningar](vpn-gateway-ipsecikepolicy-rm-powershell.md) för mer information om anpassade IPsec/IKE-principer.
