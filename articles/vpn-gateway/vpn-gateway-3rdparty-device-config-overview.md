---
title: aaaAbout 3 part VPN-enhetens konfiguration tooconnect tooAzure VPN-gatewayer | Microsoft Docs
description: "Den här artikeln innehåller en översikt över 3 part VPN enhetskonfigurationer för att ansluta tooAzure VPN-gatewayer."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 3bb4fc94bc625386c2d0a02e1dcbdeb38ee0665e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-3rd-party-vpn-device-configurations"></a><span data-ttu-id="67773-103">Översikt över 3 part VPN enhetskonfigurationer</span><span class="sxs-lookup"><span data-stu-id="67773-103">Overview of 3rd party VPN device configurations</span></span>
<span data-ttu-id="67773-104">Den här artikeln innehåller en översikt över lokala VPN-enhetskonfigurationer för att ansluta tooAzure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="67773-104">This article provides an overview of on-premises VPN device configurations for connecting tooAzure VPN gateways.</span></span> <span data-ttu-id="67773-105">hello exemplet Azure-nätverk och VPN-gateway installationen kommer att använda tooconnect toodifferent lokala VPN-enheter med hello samma parametrar.</span><span class="sxs-lookup"><span data-stu-id="67773-105">hello sample Azure virtual network and VPN gateway setup will be used tooconnect toodifferent on-premises VPN devices with hello same parameters.</span></span>

## <a name="device-requirements"></a><span data-ttu-id="67773-106">Enhetskrav på</span><span class="sxs-lookup"><span data-stu-id="67773-106">Device requirements</span></span>
<span data-ttu-id="67773-107">Azure VPN-gatewayer Använd standard IPsec/IKE-protokoll-paket för S2S VPN-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="67773-107">Azure VPN gateways use standard IPsec/IKE protocol suites for S2S VPN tunnels.</span></span> <span data-ttu-id="67773-108">Se för[om VPN-enheter](vpn-gateway-about-vpn-devices.md) hello mer IPsec/IKE protokollparametrar och standard kryptografiska algoritmer för Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="67773-108">Refer too[About VPN devices](vpn-gateway-about-vpn-devices.md) for hello detailed IPsec/IKE protocol parameters and default cryptographic algorithms for Azure VPN gateways.</span></span> <span data-ttu-id="67773-109">Du kan också ange hello exakta kombinationen av kryptografiska algoritmer och viktiga fördelar för en viss anslutning enligt beskrivningen i [om krav för kryptografiska](vpn-gateway-about-compliance-crypto.md).</span><span class="sxs-lookup"><span data-stu-id="67773-109">You can optionally specify hello exact combination of cryptographic algorithms and key strengths for a specific connection as described in [About cryptographic requirements](vpn-gateway-about-compliance-crypto.md).</span></span>

## <span data-ttu-id="67773-110"><a name ="singletunnel"></a>En VPN-tunnel</span><span class="sxs-lookup"><span data-stu-id="67773-110"><a name ="singletunnel"></a>Single VPN tunnel</span></span>
<span data-ttu-id="67773-111">hello första topologi består av en S2S VPN-tunnel mellan en Azure VPN-gateway och din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="67773-111">hello first topology consists of a single S2S VPN tunnel between an Azure VPN gateway and your on-premises VPN device.</span></span> <span data-ttu-id="67773-112">Du kan också konfigurera BGP över hello VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="67773-112">You can optionally configure BGP across hello VPN tunnel.</span></span>

![en tunnel](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

<span data-ttu-id="67773-114">Se för[Konfigurera plats-till-plats-anslutning](vpn-gateway-howto-site-to-site-resource-manager-portal.md) detaljerad, stegvisa anvisningar.</span><span class="sxs-lookup"><span data-stu-id="67773-114">Refer too[Configure site-to-site connection](vpn-gateway-howto-site-to-site-resource-manager-portal.md) for detailed, step-by-step guidance.</span></span> <span data-ttu-id="67773-115">hello följande avsnitt listas hello parametrar och ge exempel PowerShell-skriptet toohelp dig att komma igång.</span><span class="sxs-lookup"><span data-stu-id="67773-115">hello following sections list hello parameters and provide a sample PowerShell script toohelp you get started.</span></span>

### <a name="network-and-vpn-gateway-information"></a><span data-ttu-id="67773-116">Information om nätverk och VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="67773-116">Network and VPN gateway information</span></span>
<span data-ttu-id="67773-117">Det här avsnittet listas hello parametrar för hello exemplen ovan.</span><span class="sxs-lookup"><span data-stu-id="67773-117">This section list hello parameters for hello examples above.</span></span>

| <span data-ttu-id="67773-118">**Parametern**</span><span class="sxs-lookup"><span data-stu-id="67773-118">**Parameter**</span></span>                | <span data-ttu-id="67773-119">**Värde**</span><span class="sxs-lookup"><span data-stu-id="67773-119">**Value**</span></span>                    |
| ---                          | ---                          |
| <span data-ttu-id="67773-120">VNet-adressprefixer</span><span class="sxs-lookup"><span data-stu-id="67773-120">VNet address prefixes</span></span>        | <span data-ttu-id="67773-121">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="67773-121">10.11.0.0/16</span></span><br><span data-ttu-id="67773-122">10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="67773-122">10.12.0.0/16</span></span> |
| <span data-ttu-id="67773-123">Azure VPN gateway-IP</span><span class="sxs-lookup"><span data-stu-id="67773-123">Azure VPN gateway IP</span></span>         | <span data-ttu-id="67773-124">Azure VPN-Gateway IP</span><span class="sxs-lookup"><span data-stu-id="67773-124">Azure VPN Gateway IP</span></span>         |
| <span data-ttu-id="67773-125">Lokala adressprefix</span><span class="sxs-lookup"><span data-stu-id="67773-125">On-premises address prefixes</span></span> | <span data-ttu-id="67773-126">10.51.0.0/16</span><span class="sxs-lookup"><span data-stu-id="67773-126">10.51.0.0/16</span></span><br><span data-ttu-id="67773-127">10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="67773-127">10.52.0.0/16</span></span> |
| <span data-ttu-id="67773-128">Lokala VPN-enhetens IP</span><span class="sxs-lookup"><span data-stu-id="67773-128">On-premises VPN device IP</span></span>    | <span data-ttu-id="67773-129">Lokala VPN-enhetens IP</span><span class="sxs-lookup"><span data-stu-id="67773-129">On-premises VPN device IP</span></span>    |
| <span data-ttu-id="67773-130">* VNet BGP ASN</span><span class="sxs-lookup"><span data-stu-id="67773-130">*VNet BGP ASN</span></span>                | <span data-ttu-id="67773-131">65010</span><span class="sxs-lookup"><span data-stu-id="67773-131">65010</span></span>                        |
| <span data-ttu-id="67773-132">* Azure BGP-peer-IP</span><span class="sxs-lookup"><span data-stu-id="67773-132">*Azure BGP peer IP</span></span>           | <span data-ttu-id="67773-133">10.12.255.30</span><span class="sxs-lookup"><span data-stu-id="67773-133">10.12.255.30</span></span>                 |
| <span data-ttu-id="67773-134">* Lokala BGP ASN</span><span class="sxs-lookup"><span data-stu-id="67773-134">*On-premises BGP ASN</span></span>         | <span data-ttu-id="67773-135">65050</span><span class="sxs-lookup"><span data-stu-id="67773-135">65050</span></span>                        |
| <span data-ttu-id="67773-136">* Lokala BGP-peer-IP</span><span class="sxs-lookup"><span data-stu-id="67773-136">*On-premises BGP peer IP</span></span>     | <span data-ttu-id="67773-137">10.52.255.254</span><span class="sxs-lookup"><span data-stu-id="67773-137">10.52.255.254</span></span>                |

* <span data-ttu-id="67773-138">(*) Valfria parametrar för BGP endast</span><span class="sxs-lookup"><span data-stu-id="67773-138">(*) Optional parameters for BGP only</span></span>

### <a name="sample-powershell-script"></a><span data-ttu-id="67773-139">PowerShell-exempelskript</span><span class="sxs-lookup"><span data-stu-id="67773-139">Sample PowerShell script</span></span>
<span data-ttu-id="67773-140">[Skapa en S2S VPN-anslutning med hjälp av PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md) har hello detaljerade instruktioner.</span><span class="sxs-lookup"><span data-stu-id="67773-140">[Create a S2S VPN connection using PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md) has hello detailed instructions.</span></span> <span data-ttu-id="67773-141">Det här avsnittet innehåller ett exempel skriptet tooget som du startade.</span><span class="sxs-lookup"><span data-stu-id="67773-141">This section provides a sample script tooget you started.</span></span>

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subcription_Name"
$RG1           = "TestRG1"
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
$VNet1ASN      = 65010
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GWIPName1     = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect tooyour subscription and create a new resource group

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create hello S2S VPN connection

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <span data-ttu-id="67773-142"><a name ="policybased"></a>[Valfritt] Använda anpassade IPsec/IKE-princip med ”UsePolicyBasedTrafficSelectors”</span><span class="sxs-lookup"><span data-stu-id="67773-142"><a name ="policybased"></a> [Optional] Use custom IPsec/IKE policy with "UsePolicyBasedTrafficSelectors"</span></span>
<span data-ttu-id="67773-143">Om VPN-enheter inte stöder ”alla-till-alla” trafik väljare (route-baserade/VTI-baserad konfiguration), kommer du behöver toocreate en anpassad princip för IPsec/IKE och konfigurera alternativet ”UsePolicyBasedTrafficSelectors”, enligt beskrivningen i [i den här artikeln ](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="67773-143">If your VPN devices do not support "any-to-any" traffic selectors (route-based/VTI-based configuration), you will need toocreate a custom IPsec/IKE policy and configure "UsePolicyBasedTrafficSelectors" option as described in [this article](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67773-144">Du måste toocreate en IPsec/IKE-princip i ordning tooenable ”UsePolicyBasedTrafficSelectors” alternativet på hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="67773-144">You need toocreate an IPsec/IKE policy in order tooenable "UsePolicyBasedTrafficSelectors" option on hello connection.</span></span>

<span data-ttu-id="67773-145">hello exempelskript nedan skapar en IPsec/IKE-princip med hello följande algoritmer och parametrar:</span><span class="sxs-lookup"><span data-stu-id="67773-145">hello sample script below creates an IPsec/IKE policy with hello following algorithms and parameters:</span></span>
* <span data-ttu-id="67773-146">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="67773-146">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="67773-147">IPsec: AES256, SHA1, PFS24 SA livstid 7200 sekunder & 20480000KB (20GB)</span><span class="sxs-lookup"><span data-stu-id="67773-147">IPsec: AES256, SHA1, PFS24, SA Lifetime 7200 seconds & 20480000KB (20GB)</span></span>

<span data-ttu-id="67773-148">Det gäller hello princip och aktiverar ”UesPolicyBasedTrafficSelectors” på hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="67773-148">It then applies hello policy and enables "UesPolicyBasedTrafficSelectors" on hello connection.</span></span>

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <span data-ttu-id="67773-149"><a name ="bgp"></a>[Valfritt] Använda BGP för S2S VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="67773-149"><a name ="bgp"></a>[Optional] Use BGP on S2S VPN connection</span></span>
<span data-ttu-id="67773-150">Du kan eventuellt använda BGP på hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="67773-150">You can optionally use BGP on hello connection.</span></span> <span data-ttu-id="67773-151">Se [BGP för VPN-gateway](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="67773-151">See [BGP for VPN gateway](vpn-gateway-bgp-resource-manager-ps.md).</span></span> <span data-ttu-id="67773-152">Det finns två skillnader:</span><span class="sxs-lookup"><span data-stu-id="67773-152">There are two differences:</span></span>

<span data-ttu-id="67773-153">hello lokala adressprefix kan vara en enskild värdadress, hello lokala BGP peer-IP-adress:</span><span class="sxs-lookup"><span data-stu-id="67773-153">hello on-premises address prefixes can be a single host address, hello on-premises BGP peer IP address:</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

<span data-ttu-id="67773-154">Du måste ange ”-Enablegpg” för$ True när du skapar hello anslutning:</span><span class="sxs-lookup"><span data-stu-id="67773-154">You must set "-EnableBGP" too$True when creating hello connection:</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

## <a name="next-steps"></a><span data-ttu-id="67773-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67773-155">Next steps</span></span>
<span data-ttu-id="67773-156">Se [konfigurera aktiv-aktiv VPN-gatewayer för anslutningar mellan platser och VNet-till-VNet-anslutningar](vpn-gateway-activeactive-rm-powershell.md) åtgärder tooconfigure aktiv-aktiv mellan platser och VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="67773-156">See [Configuring Active-Active VPN Gateways for Cross-Premises and VNet-to-VNet Connections](vpn-gateway-activeactive-rm-powershell.md) for steps tooconfigure active-active cross-premises and VNet-to-VNet connections.</span></span>

