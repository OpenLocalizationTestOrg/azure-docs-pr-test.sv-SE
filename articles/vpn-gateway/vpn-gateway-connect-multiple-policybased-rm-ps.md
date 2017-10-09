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
# <a name="connect-azure-vpn-gateways-toomultiple-on-premises-policy-based-vpn-devices-using-powershell"></a><span data-ttu-id="d75cd-103">Anslut Azure VPN-gatewayer toomultiple lokala principbaserad VPN-enheter med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d75cd-103">Connect Azure VPN gateways toomultiple on-premises policy-based VPN devices using PowerShell</span></span>

<span data-ttu-id="d75cd-104">Den här artikeln hjälper dig att konfigurera en Azure ruttbaserad VPN-gateway tooconnect toomultiple lokalt principbaserad VPN-enheter genom att använda anpassade IPsec/IKE-principer för S2S VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d75cd-104">This article helps you configure an Azure route-based VPN gateway tooconnect toomultiple on-premises policy-based VPN devices leveraging custom IPsec/IKE policies on S2S VPN connections.</span></span>

## <a name="about-policy-based-and-route-based-vpn-gateways"></a><span data-ttu-id="d75cd-105">Om principbaserade och ruttbaserade VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="d75cd-105">About policy-based and route-based VPN gateways</span></span>

<span data-ttu-id="d75cd-106">Princip - *kontra* ruttbaserad VPN-enheter skiljer sig åt i hur hello IPSec-trafik väljare anges för en anslutning:</span><span class="sxs-lookup"><span data-stu-id="d75cd-106">Policy- *vs.* route-based VPN devices differ in how hello IPsec traffic selectors are set on a connection:</span></span>

* <span data-ttu-id="d75cd-107">**Principbaserad** VPN-enheter använder hello kombinationer av adressprefix från båda nätverk toodefine hur krypterade/dekrypteras trafiken via IPsec-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="d75cd-107">**Policy-based** VPN devices use hello combinations of prefixes from both networks toodefine how traffic is encrypted/decrypted through IPsec tunnels.</span></span> <span data-ttu-id="d75cd-108">Den bygger vanligtvis på brandväggsenheter som utför filtrering av nätverkspaket.</span><span class="sxs-lookup"><span data-stu-id="d75cd-108">It is typically built on firewall devices that perform packet filtering.</span></span> <span data-ttu-id="d75cd-109">IPSec-tunneln kryptering och dekryptering läggs toohello paketfilter och bearbetningsmotorn.</span><span class="sxs-lookup"><span data-stu-id="d75cd-109">IPsec tunnel encryption and decryption are added toohello packet filtering and processing engine.</span></span>
* <span data-ttu-id="d75cd-110">**Ruttbaserad** VPN-enheter med hjälp av alla-till-alla (jokertecken) trafik väljare och låter routning/vidarebefordran tabeller direkt trafik toodifferent IPSec-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="d75cd-110">**Route-based** VPN devices use any-to-any (wildcard) traffic selectors, and let routing/forwarding tables direct traffic toodifferent IPsec tunnels.</span></span> <span data-ttu-id="d75cd-111">Den bygger vanligtvis på routern plattformar, där varje IPsec-tunneln modelleras som ett nätverksgränssnitt eller VTI (virtuell tunnelgränssnitt).</span><span class="sxs-lookup"><span data-stu-id="d75cd-111">It is typically built on router platforms where each IPsec tunnel is modeled as a network interface or VTI (virtual tunnel interface).</span></span>

<span data-ttu-id="d75cd-112">hello Markera följande diagram hello två modeller:</span><span class="sxs-lookup"><span data-stu-id="d75cd-112">hello following diagrams highlight hello two models:</span></span>

### <a name="policy-based-vpn-example"></a><span data-ttu-id="d75cd-113">Principbaserad VPN-exempel</span><span class="sxs-lookup"><span data-stu-id="d75cd-113">Policy-based VPN example</span></span>
![principbaserad](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a><span data-ttu-id="d75cd-115">Ruttbaserad VPN-exempel</span><span class="sxs-lookup"><span data-stu-id="d75cd-115">Route-based VPN example</span></span>
![ruttbaserad](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a><span data-ttu-id="d75cd-117">Azure-stöd för principbaserad VPN</span><span class="sxs-lookup"><span data-stu-id="d75cd-117">Azure support for policy-based VPN</span></span>
<span data-ttu-id="d75cd-118">Azure stöder för närvarande, båda lägena för VPN-gatewayer: ruttbaserad VPN-gatewayer och policy-baserad VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="d75cd-118">Currently, Azure supports both modes of VPN gateways: route-based VPN gateways and policy-based VPN gateways.</span></span> <span data-ttu-id="d75cd-119">De bygger på olika interna plattformar, vilket resulterar i olika specifikationer:</span><span class="sxs-lookup"><span data-stu-id="d75cd-119">They are built on different internal platforms, which result in different specifications:</span></span>

|                          | <span data-ttu-id="d75cd-120">**PolicyBased VPN-Gateway**</span><span class="sxs-lookup"><span data-stu-id="d75cd-120">**PolicyBased VPN Gateway**</span></span> | <span data-ttu-id="d75cd-121">**RouteBased VPN-Gateway**</span><span class="sxs-lookup"><span data-stu-id="d75cd-121">**RouteBased VPN Gateway**</span></span>               |
| ---                      | ---                         | ---                                      |
| <span data-ttu-id="d75cd-122">**Azure Gateway-SKU**</span><span class="sxs-lookup"><span data-stu-id="d75cd-122">**Azure Gateway SKU**</span></span>    | <span data-ttu-id="d75cd-123">Basic</span><span class="sxs-lookup"><span data-stu-id="d75cd-123">Basic</span></span>                       | <span data-ttu-id="d75cd-124">Basic, Standard, HighPerformance</span><span class="sxs-lookup"><span data-stu-id="d75cd-124">Basic, Standard, HighPerformance</span></span>         |
| <span data-ttu-id="d75cd-125">**IKE-version**</span><span class="sxs-lookup"><span data-stu-id="d75cd-125">**IKE version**</span></span>          | <span data-ttu-id="d75cd-126">IKEv1</span><span class="sxs-lookup"><span data-stu-id="d75cd-126">IKEv1</span></span>                       | <span data-ttu-id="d75cd-127">IKEv2</span><span class="sxs-lookup"><span data-stu-id="d75cd-127">IKEv2</span></span>                                    |
| <span data-ttu-id="d75cd-128">**Max. S2S-anslutningar**</span><span class="sxs-lookup"><span data-stu-id="d75cd-128">**Max. S2S connections**</span></span> | <span data-ttu-id="d75cd-129">**1**</span><span class="sxs-lookup"><span data-stu-id="d75cd-129">**1**</span></span>                       | <span data-ttu-id="d75cd-130">Basic-standarden: 10</span><span class="sxs-lookup"><span data-stu-id="d75cd-130">Basic/Standard: 10</span></span><br> <span data-ttu-id="d75cd-131">HighPerformance: 30</span><span class="sxs-lookup"><span data-stu-id="d75cd-131">HighPerformance: 30</span></span> |
|                          |                             |                                          |

<span data-ttu-id="d75cd-132">Med hello anpassad IPsec/IKE-princip kan du nu konfigurera Azure ruttbaserad VPN-gatewayer toouse prefix-baserad trafik väljare med alternativet ”**PolicyBasedTrafficSelectors**”, tooconnect tooon lokala principbaserad VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="d75cd-132">With hello custom IPsec/IKE policy, you can now configure Azure route-based VPN gateways toouse prefix-based traffic selectors with option "**PolicyBasedTrafficSelectors**", tooconnect tooon-premises policy-based VPN devices.</span></span> <span data-ttu-id="d75cd-133">Den här funktionen kan du tooconnect från Azure-nätverk och VPN-gateway toomultiple lokala principbaserad VPN-brandväggen enheter, ta bort hello enda anslutningsgränsen från hello aktuella Azure principbaserade VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="d75cd-133">This capability allows you tooconnect from an Azure virtual network and VPN gateway toomultiple on-premises policy-based VPN/firewall devices, removing hello single connection limit from hello current Azure policy-based VPN gateways.</span></span>

> [!IMPORTANT]
> 1. <span data-ttu-id="d75cd-134">tooenable den här anslutningen dina lokala principbaserad VPN-enheter måste ha stöd för **IKEv2** tooconnect toohello Azure ruttbaserad VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="d75cd-134">tooenable this connectivity, your on-premises policy-based VPN devices must support **IKEv2** tooconnect toohello Azure route-based VPN gateways.</span></span> <span data-ttu-id="d75cd-135">Kontrollera din VPN-enhetsspecifikationer.</span><span class="sxs-lookup"><span data-stu-id="d75cd-135">Check your VPN device specifications.</span></span>
> 2. <span data-ttu-id="d75cd-136">hello lokala nätverk som ansluter via principbaserad VPN-enheter med den här funktionen kan bara ansluta toohello virtuella Azure-nätverk; **kan inte de transit tooother lokala nätverk eller virtuella nätverk via hello samma Azure VPN-gateway**.</span><span class="sxs-lookup"><span data-stu-id="d75cd-136">hello on-premises networks connecting through policy-based VPN devices with this mechanism can only connect toohello Azure virtual network; **they cannot transit tooother on-premises networks or virtual networks via hello same Azure VPN gateway**.</span></span>
> 3. <span data-ttu-id="d75cd-137">hello konfigurationsalternativet är en del av hello anpassade IPsec/IKE-princip.</span><span class="sxs-lookup"><span data-stu-id="d75cd-137">hello configuration option is part of hello custom IPsec/IKE connection policy.</span></span> <span data-ttu-id="d75cd-138">Om du aktiverar alternativ för hello principbaserad trafik väljare, måste du ange hello hela policyn (IPsec/IKE algoritmer för kryptering och integritet, viktiga fördelar och säkerhetsassociationer).</span><span class="sxs-lookup"><span data-stu-id="d75cd-138">If you enable hello policy-based traffic selector option, you must specify hello complete policy (IPsec/IKE encryption and integrity algorithms, key strengths, and SA lifetimes).</span></span>

<span data-ttu-id="d75cd-139">hello följande diagram visar varför transitroutning via Azure VPN-gateway fungerar inte med hello principbaserad alternativet:</span><span class="sxs-lookup"><span data-stu-id="d75cd-139">hello following diagram shows why transit routing via Azure VPN gateway doesn't work with hello policy-based option:</span></span>

![principbaserad överföring](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

<span data-ttu-id="d75cd-141">Enligt i hello diagram har hello Azure VPN-gateway trafik väljare från hello virtuellt nätverk tooeach hello-prefix för lokala nätverket, men inte hello anslutningen mellan prefix.</span><span class="sxs-lookup"><span data-stu-id="d75cd-141">As shown in hello diagram, hello Azure VPN gateway has traffic selectors from hello virtual network tooeach of hello on-premises network prefixes, but not hello cross-connection prefixes.</span></span> <span data-ttu-id="d75cd-142">Till exempel lokal plats 2, 3, och plats 4 kan varje kommunicera tooVNet1 respektive, men kan inte ansluta via hello Azure VPN gateway tooeach andra.</span><span class="sxs-lookup"><span data-stu-id="d75cd-142">For example, on-premises site 2, site 3, and site 4 can each communicate tooVNet1 respectively, but cannot connect via hello Azure VPN gateway tooeach other.</span></span> <span data-ttu-id="d75cd-143">hello diagram visar hello korskoppling trafik väljare som inte är tillgängliga i hello Azure VPN-gatewayen under den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d75cd-143">hello diagram shows hello cross-connect traffic selectors that are not available in hello Azure VPN gateway under this configuration.</span></span>

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="d75cd-144">Konfigurera principbaserad trafik väljare på en anslutning</span><span class="sxs-lookup"><span data-stu-id="d75cd-144">Configure policy-based traffic selectors on a connection</span></span>

<span data-ttu-id="d75cd-145">hello instruktionerna i den här artikeln Följ hello samma exempel som beskrivs i [konfigurera IPsec/IKE-principen för S2S eller VNet-till-VNet-anslutningar](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish en S2S VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d75cd-145">hello instructions in this article follow hello same example as described in [Configure IPsec/IKE policy for S2S or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish a S2S VPN connection.</span></span> <span data-ttu-id="d75cd-146">Detta visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="d75cd-146">This is shown in hello following diagram:</span></span>

![s2s-princip](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

<span data-ttu-id="d75cd-148">Hej arbetsflöde tooenable den här anslutningen:</span><span class="sxs-lookup"><span data-stu-id="d75cd-148">hello workflow tooenable this connectivity:</span></span>
1. <span data-ttu-id="d75cd-149">Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway för anslutningen mellan platser</span><span class="sxs-lookup"><span data-stu-id="d75cd-149">Create hello virtual network, VPN gateway, and local network gateway for your cross-premises connection</span></span>
2. <span data-ttu-id="d75cd-150">Skapa en IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="d75cd-150">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="d75cd-151">Använd hello principen när du skapar en S2S eller VNet-till-VNet-anslutning och **aktivera hello principbaserad trafik väljare** på hello-anslutning</span><span class="sxs-lookup"><span data-stu-id="d75cd-151">Apply hello policy when you create a S2S or VNet-to-VNet connection, and **enable hello policy-based traffic selectors** on hello connection</span></span>
4. <span data-ttu-id="d75cd-152">Om hello anslutning redan har skapats, kan du tillämpa eller uppdatera hello princip tooan befintlig anslutning</span><span class="sxs-lookup"><span data-stu-id="d75cd-152">If hello connection is already created, you can apply or update hello policy tooan existing connection</span></span>

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="d75cd-153">Aktivera principbaserad trafik väljare på en anslutning</span><span class="sxs-lookup"><span data-stu-id="d75cd-153">Enable policy-based traffic selectors on a connection</span></span>

<span data-ttu-id="d75cd-154">Kontrollera att du har slutfört [del 3 hello konfigurera IPsec/IKE princip artikel](vpn-gateway-ipsecikepolicy-rm-powershell.md) för det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d75cd-154">Make sure you have completed [Part 3 of hello Configure IPsec/IKE policy article](vpn-gateway-ipsecikepolicy-rm-powershell.md) for this section.</span></span> <span data-ttu-id="d75cd-155">följande exempel använder hello hello samma parametrar och steg:</span><span class="sxs-lookup"><span data-stu-id="d75cd-155">hello following example uses hello same parameters and steps:</span></span>

### <a name="step-1---create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="d75cd-156">Steg 1 – Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="d75cd-156">Step 1 - Create hello virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables--connect-tooyour-subscription"></a><span data-ttu-id="d75cd-157">1. Deklarera variablerna & ansluter tooyour prenumeration</span><span class="sxs-lookup"><span data-stu-id="d75cd-157">1. Declare your variables & connect tooyour subscription</span></span>
<span data-ttu-id="d75cd-158">Den här övningen starta genom att deklarera våra variabler.</span><span class="sxs-lookup"><span data-stu-id="d75cd-158">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="d75cd-159">Vara säker på att tooreplace hello värden med dina egna när du konfigurerar för produktion.</span><span class="sxs-lookup"><span data-stu-id="d75cd-159">Be sure tooreplace hello values with your own when configuring for production.</span></span>

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
<span data-ttu-id="d75cd-160">toouse hello Resource Manager cmdlets, se till att du växla tooPowerShell läge.</span><span class="sxs-lookup"><span data-stu-id="d75cd-160">toouse hello Resource Manager cmdlets, make sure you switch tooPowerShell mode.</span></span> <span data-ttu-id="d75cd-161">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="d75cd-161">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="d75cd-162">Öppna PowerShell-konsolen och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="d75cd-162">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="d75cd-163">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="d75cd-163">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="d75cd-164">2. Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="d75cd-164">2. Create hello virtual network, VPN gateway, and local network gateway</span></span>
<span data-ttu-id="d75cd-165">hello följande exempel skapar hello virtuellt nätverk, TestVNet1 med tre undernät och hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="d75cd-165">hello following example creates hello virtual network, TestVNet1 with three subnets, and hello VPN gateway.</span></span> <span data-ttu-id="d75cd-166">När du ersätter värdena är det viktigt att du alltid namnger gateway-undernätet specifikt 'GatewaySubnet'.</span><span class="sxs-lookup"><span data-stu-id="d75cd-166">When substituting values, it's important that you always name your gateway subnet specifically 'GatewaySubnet'.</span></span> <span data-ttu-id="d75cd-167">Om du ger det något annat namn går det inte att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="d75cd-167">If you name it something else, your gateway creation fails.</span></span>

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

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a><span data-ttu-id="d75cd-168">Steg 2 – Skapa en S2S VPN-anslutning med en IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="d75cd-168">Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="d75cd-169">1. Skapa en IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="d75cd-169">1. Create an IPsec/IKE policy</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d75cd-170">Du måste toocreate en IPsec/IKE-princip i ordning tooenable ”UsePolicyBasedTrafficSelectors” alternativet på hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d75cd-170">You need toocreate an IPsec/IKE policy in order tooenable "UsePolicyBasedTrafficSelectors" option on hello connection.</span></span>

<span data-ttu-id="d75cd-171">hello skapar följande exempel en IPsec/IKE-princip med dessa algoritmer och parametrar:</span><span class="sxs-lookup"><span data-stu-id="d75cd-171">hello following example creates an IPsec/IKE policy with these algorithms and parameters:</span></span>
* <span data-ttu-id="d75cd-172">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="d75cd-172">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="d75cd-173">IPsec: AES256, SHA256, PFS24 SA livstid 3600 sekunder & 2048KB</span><span class="sxs-lookup"><span data-stu-id="d75cd-173">IPsec: AES256, SHA256, PFS24, SA Lifetime 3600 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a><span data-ttu-id="d75cd-174">2. Skapa hello S2S VPN-anslutning med principbaserad trafik väljare och IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="d75cd-174">2. Create hello S2S VPN connection with policy-based traffic selectors and IPsec/IKE policy</span></span>
<span data-ttu-id="d75cd-175">Skapa en S2S VPN-anslutning och tillämpa hello IPsec/IKE-princip skapas i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="d75cd-175">Create an S2S VPN connection and apply hello IPsec/IKE policy created in hello previous step.</span></span> <span data-ttu-id="d75cd-176">Tänk på att hello ytterligare parameter ”-UsePolicyBasedTrafficSelectors $True” som gör det möjligt för principbaserad trafik väljare på hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d75cd-176">Be aware of hello additional parameter "-UsePolicyBasedTrafficSelectors $True"  which enables policy-based traffic selectors on hello connection.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="d75cd-177">När du har slutfört steg hello hello S2S VPN-anslutning använder hello IPsec/IKE princip har definierats, och aktivera principbaserad trafik väljare på hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d75cd-177">After completing hello steps, hello S2S VPN connection will use hello IPsec/IKE policy defined, and enable policy-based traffic selectors on hello connection.</span></span> <span data-ttu-id="d75cd-178">Du kan upprepa samma steg tooadd fler anslutningar tooadditional lokalt principbaserad VPN-enheter från hello hello samma Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="d75cd-178">You can repeat hello same steps tooadd more connections tooadditional on-premises policy-based VPN devices from hello same Azure VPN gateway.</span></span>

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a><span data-ttu-id="d75cd-179">Uppdatera principbaserad trafik väljare för en anslutning</span><span class="sxs-lookup"><span data-stu-id="d75cd-179">Update policy-based traffic selectors for a connection</span></span>
<span data-ttu-id="d75cd-180">hello sista avsnittet visar hur tooupdate hello principbaserad trafik väljare alternativ för en befintlig S2S VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d75cd-180">hello last section shows you how tooupdate hello policy-based traffic selectors option for an existing  S2S VPN connection.</span></span>

### <a name="1-get-hello-connection"></a><span data-ttu-id="d75cd-181">1. Hämta hello-anslutning</span><span class="sxs-lookup"><span data-stu-id="d75cd-181">1. Get hello connection</span></span>
<span data-ttu-id="d75cd-182">Hämta hello anslutning resurs.</span><span class="sxs-lookup"><span data-stu-id="d75cd-182">Get hello connection resource.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-hello-policy-based-traffic-selectors-option"></a><span data-ttu-id="d75cd-183">2. Markera alternativet för hello principbaserad trafik väljare</span><span class="sxs-lookup"><span data-stu-id="d75cd-183">2. Check hello policy-based traffic selectors option</span></span>
<span data-ttu-id="d75cd-184">hello visar följande rad om hello principbaserad trafik väljare som används för hello-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="d75cd-184">hello following line shows whether hello policy-based traffic selectors are used for hello connection:</span></span>

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

<span data-ttu-id="d75cd-185">Om hello rad returnerar ”**SANT**”, sedan principbaserad trafik väljare konfigureras på hello anslutning; annars returneras ”**FALSKT**”.</span><span class="sxs-lookup"><span data-stu-id="d75cd-185">If hello line returns "**True**", then policy-based traffic selectors are configured on hello connection; otherwise it returns "**False**."</span></span>

### <a name="3-update-hello-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="d75cd-186">3. Uppdatera hello principbaserad trafik väljare på en anslutning</span><span class="sxs-lookup"><span data-stu-id="d75cd-186">3. Update hello policy-based traffic selectors on a connection</span></span>
<span data-ttu-id="d75cd-187">När du har fått hello anslutning resurs, kan du aktivera eller inaktivera hello-alternativet.</span><span class="sxs-lookup"><span data-stu-id="d75cd-187">Once you obtain hello connection resource, you can enable or disable hello option.</span></span>

#### <a name="disable-usepolicybasedtrafficselectors"></a><span data-ttu-id="d75cd-188">Inaktivera UsePolicyBasedTrafficSelectors</span><span class="sxs-lookup"><span data-stu-id="d75cd-188">Disable UsePolicyBasedTrafficSelectors</span></span>
<span data-ttu-id="d75cd-189">hello följande exempel inaktiverar hello principbaserad trafik väljare alternativet, men lämnar kvar hello IPsec/princip oförändrade:</span><span class="sxs-lookup"><span data-stu-id="d75cd-189">hello following example disables hello policy-based traffic selectors option, but leaves hello IPsec/IKE policy unchanged:</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a><span data-ttu-id="d75cd-190">Aktivera UsePolicyBasedTrafficSelectors</span><span class="sxs-lookup"><span data-stu-id="d75cd-190">Enable UsePolicyBasedTrafficSelectors</span></span>
<span data-ttu-id="d75cd-191">hello följande exempel aktiveras hello principbaserad trafik väljare alternativet, men lämnar kvar hello IPsec/princip oförändrade:</span><span class="sxs-lookup"><span data-stu-id="d75cd-191">hello following example enables hello policy-based traffic selectors option, but leaves hello IPsec/IKE policy unchanged:</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a><span data-ttu-id="d75cd-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d75cd-192">Next steps</span></span>
<span data-ttu-id="d75cd-193">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d75cd-193">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="d75cd-194">Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="d75cd-194">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>

<span data-ttu-id="d75cd-195">Granska även [konfigurera IPsec/IKE-principen för S2S VPN- eller VNet-till-VNet-anslutningar](vpn-gateway-ipsecikepolicy-rm-powershell.md) för mer information om anpassade IPsec/IKE-principer.</span><span class="sxs-lookup"><span data-stu-id="d75cd-195">Also review [Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) for more details on custom IPsec/IKE policies.</span></span>
