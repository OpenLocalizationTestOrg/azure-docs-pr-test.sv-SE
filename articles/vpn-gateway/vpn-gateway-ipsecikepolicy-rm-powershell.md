---
title: "Konfigurera IPsec/IKE-princip för S2S VPN- eller VNet-till-VNet-anslutningar: Azure Resource Manager: PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar IPsec/IKE-principen för S2S eller VNet-till-VNet-anslutningar med Azure VPN-gatewayer med Azure Resource Manager och PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: yushwang
ms.openlocfilehash: f8d2e29276efdec7071f2aa0d463b1abd64a5253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a>Konfigurera IPsec/IKE-princip för S2S VPN- eller VNet-till-VNet-anslutningar

Den här artikeln vägleder dig genom hello steg tooconfigure IPsec/IKE-principen för plats-till-plats-VPN eller VNet-till-VNet-anslutningar med hello Resource Manager-distributionsmodellen och PowerShell.

## <a name="about"></a>Om IPsec och IKE principparametrar för Azure VPN-gatewayer
IPsec och IKE protokollet som standard stöder en mängd olika krypteringsalgoritmer i olika kombinationer. Se för[om kryptografiska krav och Azure VPN-gatewayer](vpn-gateway-about-compliance-crypto.md) toosee hur detta kan hjälpa att säkerställa mellan platser och VNet-till-VNet-anslutningen uppfyller dina krav efterlevnad och säkerhet.

Den här artikeln innehåller anvisningar toocreate och konfigurera en princip för IPsec/IKE och tillämpa tooa ny eller befintlig anslutning:

* [Del 1 - arbetsflöde toocreate och ange IPsec/IKE-princip](#workflow)
* [Del 2 - stöd för kryptografiska algoritmer och viktiga fördelar](#params)
* [Del 3 – skapa en ny S2S VPN-anslutning med IPsec/IKE-princip](#crossprem)
* [Del 4 – skapa en ny VNet-till-VNet-anslutning med IPsec/IKE-princip](#vnet2vnet)
* [En del 5 – hantera (skapa, lägga till, ta bort) IPsec/IKE-princip för en anslutning](#managepolicy)

> [!IMPORTANT]
> 1. Observera att IPsec/princip bara fungerar på hello efter gateway-SKU: er:
>    * ***VpnGw1 VpnGw2, VpnGw3*** (route-baserade)
>    * ***Standard*** och ***HighPerformance*** (route-baserade)
> 2. Du kan bara ange ***en*** principkombination för en viss anslutning.
> 3. Du måste ange alla algoritmer och parametrar för IKE (Main-läge) och IPsec (snabbläge). Partiell principspecifikationen tillåts inte.
> 4. Kontakta din VPN-leverantör enhetsspecifikationerna tooensure hello principen stöds på din lokala VPN-enheter. S2S eller VNet-till-VNet-anslutningar kan inte upprätta om hello principer är inkompatibla.

## <a name ="workflow"></a>Del 1 - arbetsflöde toocreate och ange IPsec/IKE-princip
Det här avsnittet beskrivs hello arbetsflöde toocreate och uppdatera IPsec/IKE-principen på en S2S VPN- eller VNet-till-VNet-anslutning:
1. Skapa ett virtuellt nätverk och en VPN-gateway
2. Skapa en lokal nätverksgateway för mellan lokala anslutningen eller ett annat virtuellt nätverk och gateway för VNet-till-VNet-anslutningen
3. Skapa en IPsec/IKE-princip med valda algoritmer och parametrar
4. Skapa en anslutning (IPSec- eller VNet2VNet) med hello IPsec/IKE-princip
5. Lägg till/Uppdatera/ta bort en IPsec/IKE-princip för en befintlig anslutning

hello anvisningarna i den här artikeln hjälper dig att installera och konfigurera IPsec/IKE-principer som visas i hello diagram:

![IPSec-princip](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <a name ="params"></a>Del 2 - stöd för kryptografiska algoritmer & viktiga fördelar

hello följande tabell visas hello stöds kryptografiska algoritmer och viktiga styrkor konfigureras av hello-kunder:

| **IPsec/IKEv2**  | **Alternativ**    |
| ---  | --- 
| IKEv2-kryptering | AES256, AES192, AES128, DES3, DES  
| IKEv2 Integrity  | SHA384, SHA256, SHA1, MD5  |
| DH-grupp         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, ingen |
| IPsec-kryptering | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None    |
| IPsec Integrity  | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5 |
| PFS-grupp        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None 
| QM SA-livstid   | (**Valfritt**: standardvärdena är används om inget anges)<br>Sekunder (heltal. **min. 300**/standard 27 000 sekunder)<br>Kilobyte (heltal. **min. 1024**/standard 102400000 kilobyte)   |
| Trafikväljare | UsePolicyBasedTrafficSelectors ** ($True/$False; **Valfritt**, $False som standard om inget anges)    |
|  |  |

> [!IMPORTANT]
> 1. **Om GCMAES används för IPSec-krypteringsalgoritmen, måste du välja hello samma GCMAES algoritmen och nyckellängd IPsec integritet; till exempel med hjälp av GCMAES128 för både**
> 2. IKEv2 Main Säkerhetsassociation livslängd vara högst 28 800 sekunder på hello Azure VPN-gatewayer
> 3. Inställningen ”UsePolicyBasedTrafficSelectors” för$ True i en anslutning konfigurerar hello Azure VPN gateway tooconnect toopolicy-baserad VPN-brandväggen lokalt. Om du aktiverar PolicyBasedTrafficSelectors måste tooensure VPN-enheten har hello matchande trafik väljare som definierats med alla kombinationer av lokalt nätverk (lokal nätverksgateway)-prefix till eller från hello virtuella Azure-nätverksprefix i stället för alla-till-alla. Om din prefix för det lokala nätverket är 10.1.0.0/16 och 10.2.0.0/16 och din prefix för det virtuella nätverket är 192.168.0.0/16 och 172.16.0.0/16, måste till exempel toospecify hello följande trafik väljare:
>    * 10.1.0.0/16 <====> 192.168.0.0/16
>    * 10.1.0.0/16 <====> 172.16.0.0/16
>    * 10.2.0.0/16 <====> 192.168.0.0/16
>    * 10.2.0.0/16 <====> 172.16.0.0/16

Mer information om principbaserad trafik väljare finns [ansluta flera lokala principbaserad VPN-enheter](vpn-gateway-connect-multiple-policybased-rm-ps.md).

följande tabell visar hello hello motsvarande Diffie-Hellman-grupp som stöds av hello anpassad princip:

| **Diffie-Hellman-grupp**  | **DHGroup**              | **PFSGroup** | **Nyckellängd** |
| --- | --- | --- | --- |
| 1                         | DHGroup1                 | PFS1         | 768-bitars MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024-bitars MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048-bitars MODP  |
| 19                        | ECP256                   | ECP256       | 256-bitars ECP    |
| 20                        | ECP384                   | ECP284       | 384-bitars ECP    |
| 24                        | DHGroup24                | PFS24        | 2048-bitars MODP  |

Se för[RFC3526](https://tools.ietf.org/html/rfc3526) och [RFC5114](https://tools.ietf.org/html/rfc5114) för mer information.

## <a name ="crossprem"></a>Del 3 – skapa en ny S2S VPN-anslutning med IPsec/IKE-princip

Det här avsnittet vägleder dig genom hello stegen för att skapa en S2S VPN-anslutning med ett IPsec/IKE-principen. hello följande steg skapar hello anslutning som visas i hello diagram:

![s2s-princip](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

Se [skapa en S2S VPN-anslutning](vpn-gateway-create-site-to-site-rm-powershell.md) mer detaljerad stegvisa instruktioner för att skapa en S2S VPN-anslutning.

### <a name="before"></a>Innan du börjar

* Kontrollera att du har en Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
* Installera hello Azure Resource Manager PowerShell-cmdlets. Se [översikt av Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.

### <a name="createvnet1"></a>Steg 1 – Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway

#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Anslut tooyour prenumeration och skapa en ny resursgrupp

Kontrollera att du växla tooPowerShell läge toouse hello Resource Manager-cmdletar. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

Öppna PowerShell-konsolen och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway

hello följande exempel skapar hello virtuellt nätverk, TestVNet1, med tre undernät och hello VPN-gateway. När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet. Om du ger det något annat namn går det inte att skapa gatewayen.

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

### <a name="s2sconnection"></a>Steg 2 – Skapa en S2S VPN-anslutning med en IPsec/IKE-princip

#### <a name="1-create-an-ipsecike-policy"></a>1. Skapa en IPsec/IKE-princip

Följande exempelskript hello skapar en IPsec/IKE-princip med hello följande algoritmer och parametrar:

* IKEv2: AES256, SHA384 DHGroup24
* IPsec: AES256, SHA256, PFS24 SA livstid 7200 sekunder & 2048KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

Om du använder GCMAES för IPsec, måste du använda hello samma GCMAES algoritmen och nyckellängd för både IPSec-kryptering och integritet, till exempel:

* IKEv2: AES256, SHA384 DHGroup24
* IPsec: **GCMAES256, GCMAES256**, PFS24 SA livstid 7200 sekunder & 2048 KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-hello-ipsecike-policy"></a>2. Skapa hello S2S VPN-anslutning med hello IPsec/IKE-princip

Skapa en S2S VPN-anslutning och tillämpa hello IPsec/princip skapade tidigare.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Du kan lägga till ”-UsePolicyBasedTrafficSelectors $True” toohello Skapa anslutning cmdlet tooenable Azure VPN gateway tooconnect toopolicy-baserade VPN-enheter lokalt, enligt beskrivningen ovan.

> [!IMPORTANT]
> När en IPsec/IKE-princip har angetts på en anslutning, skickar eller acceptera hello IPsec/IKE förslag med angivna kryptografiska algoritmer och viktiga fördelar för viss anslutningen endast hello Azure VPN-gateway. Kontrollera att din lokala VPN-enhet för hello anslutningen använder eller accepterar hello exakt princip kombination, annars hello S2S VPN-tunnel inte att upprätta.


## <a name ="vnet2vnet"></a>Del 4 – skapa en ny VNet-till-VNet-anslutning med IPsec/IKE-princip

hello stegen för att skapa en VNet-till-VNet-anslutning med ett IPsec/IKE-principen är liknande toothat av en S2S VPN-anslutning. hello skapa följande exempelskript hello anslutning som visas i hello diagram:

![v2v-princip](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

Se [skapa en VNet-till-VNet-anslutning](vpn-gateway-vnet-vnet-rm-ps.md) mer detaljerade anvisningar för att skapa en VNet-till-VNet-anslutning. Du måste slutföra [del 3](#crossprem) toocreate TestVNet1 och konfigurera hello VPN-Gateway.

### <a name="createvnet2"></a>Steg 1 – Skapa hello andra virtuella nätverk och VPN-gateway

#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler

Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-hello-second-virtual-network-and-vpn-gateway-in-hello-new-resource-group"></a>2. Skapa hello andra virtuella nätverk och VPN-gateway i hello ny resursgrupp

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-hello-ipsecike-policy"></a>Steg 2 – Skapa en VNet toVNet anslutning med hello IPsec/IKE-princip

Liknande toohello S2S VPN-anslutningen, skapa en IPsec/IKE-princip och sedan tillämpa toopolicy toohello ny anslutning.

#### <a name="1-create-an-ipsecike-policy"></a>1. Skapa en IPsec/IKE-princip

Följande exempelskript hello skapar en annan IPsec/IKE-princip med hello följande algoritmer och parametrar:
* IKEv2: AES128, SHA1, DHGroup14
* IPsec: GCMAES128, GCMAES128, PFS14, SA livstid 7200 sekunder och 4096KB

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 4096
```

#### <a name="2-create-vnet-to-vnet-connections-with-hello-ipsecike-policy"></a>2. Skapa VNet-till-VNet-anslutningar med hello IPsec/IKE-princip

Skapa en VNet-till-VNet-anslutning och tillämpa hello IPsec/IKE-principen som du skapade. I det här exemplet är både gateways i hello samma prenumeration. Hej samma IPsec/IKE-princip i hello så att det är möjligt toocreate och konfigurera både anslutningar med samma PowerShell-session.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> När en IPsec/IKE-princip har angetts på en anslutning, skickar eller acceptera hello IPsec/IKE förslag med angivna kryptografiska algoritmer och viktiga fördelar för viss anslutningen endast hello Azure VPN-gateway. Kontrollera att hello IPsec-principer för både anslutningar är hello samma, annars VNet-till-VNet-anslutningen inte upprättas.

Hello-anslutningen upprättas om några minuter när du har slutfört de här stegen, och du måste hello efter nätverkets topologi som visas i början av hello:

![IPSec-princip](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <a name ="managepolicy"></a>En del 5 - uppdatering IPsec/IKE-princip för en anslutning

hello sista avsnittet visas hur toomanage IPsec/IKE-princip för en befintlig S2S eller VNet-till-VNet-anslutning. hello Övning nedan vägleder dig genom följande åtgärder på en anslutning hello:

1. Visa hello IPsec/IKE-princip för en anslutning
2. Lägg till eller uppdatera hello IPsec/IKE tooa anslutning
3. Ta bort hello IPsec/IKE-princip från en anslutning

hello samma steg gäller tooboth S2S och VNet-till-VNet-anslutningar.

> [!IMPORTANT]
> IPsec/IKE-principen stöds på *Standard* och *HighPerformance* ruttbaserade VPN-gatewayer. Det fungerar inte på hello grundläggande gateway SKU eller hello principbaserad VPN-gateway.

#### <a name="1-show-hello-ipsecike-policy-of-a-connection"></a>1. Visa hello IPsec/IKE-princip för en anslutning

hello som följande exempel visar hur tooget hello IPsec/IKE-princip som konfigurerats på en anslutning. hello skript kan också fortsätta från hello övningarna ovan.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

hello senaste kommando visar hello aktuella IPsec/IKE-princip konfigurerats hello anslutning, om sådana finns. hello följande exempel på utdata är för hello-anslutningen:

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

Om det finns ingen IPsec/IKE-princip som har konfigurerats, hello kommando (PS > $connection6.policy) hämtar det returnerade en tom. Det innebär inte IPsec/IKE inte har konfigurerats på hello anslutning, men att det finns inga anpassade IPsec/IKE-princip. hello faktiska anslutningen använder hello standardprincipen förhandlats mellan din lokala VPN-enhet och hello Azure VPN-gateway.

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Lägg till eller uppdatera en IPsec/IKE-princip för en anslutning

hello steg tooadd en ny princip eller uppdatera en befintlig princip på en anslutning är hello samma: skapa en ny princip och sedan använda hello ny princip toohello anslutning.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup None -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

tooenable ”UsePolicyBasedTrafficSelectors” när du ansluter tooan lokalt principbaserad VPN-enhet, lägga till hello ”-UsePolicyBaseTrafficSelectors” parametern toohello cmdlet, eller ändra värdet för$ False toodisable hello alternativet:

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

Du kan hämta hello anslutningen igen toocheck om hello principen uppdateras.

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Du bör se hello utdata från hello sista raden, som visas i följande exempel hello:

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : GCMAES128
IpsecIntegrity      : GCMAES128
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14--
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Ta bort en IPsec/IKE-princip från en anslutning

När du tar bort hello anpassad princip från en anslutning hello Azure VPN-gateway återgår tillbaka toohello [standardlistan med IPsec/IKE förslag](vpn-gateway-about-vpn-devices.md) och gör en omförhandling igen med din lokala VPN-enhet.

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

Du kan använda hello samma skript toocheck om hello princip har tagits bort från hello-anslutning.

## <a name="next-steps"></a>Nästa steg

Se [ansluta flera lokala principbaserad VPN-enheter](vpn-gateway-connect-multiple-policybased-rm-ps.md) för mer information om principbaserad trafik väljare.

När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.
