---
title: "Konfigurera BGP på Azure VPN-gatewayer: hanteraren för filserverresurser: PowerShell | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att konfigurera BGP med Azure VPN-gatewayer med Azure Resource Manager och PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 905b11a7-1333-482c-820b-0fd0f44238e5
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: a9d13ae6b319e2efa8965dc2955c9b89ac3fd12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-bgp-on-azure-vpn-gateways-using-powershell"></a><span data-ttu-id="abcd7-103">Hur tooconfigure BGP på Azure VPN-gatewayer med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="abcd7-103">How tooconfigure BGP on Azure VPN Gateways using PowerShell</span></span>
<span data-ttu-id="abcd7-104">Den här artikeln vägleder dig genom hello steg tooenable BGP på en mellan lokala plats-till-plats (S2S) VPN-anslutning och en VNet-till-VNet-anslutning med hello Resource Manager-distributionsmodellen och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="abcd7-104">This article walks you through hello steps tooenable BGP on a cross-premises Site-to-Site (S2S) VPN connection and a VNet-to-VNet connection using hello Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-bgp"></a><span data-ttu-id="abcd7-105">Om BGP</span><span class="sxs-lookup"><span data-stu-id="abcd7-105">About BGP</span></span>
<span data-ttu-id="abcd7-106">BGP är hello standard routingprotokoll ofta används i hello Internet tooexchange Routning och reachability information mellan två eller flera nätverk.</span><span class="sxs-lookup"><span data-stu-id="abcd7-106">BGP is hello standard routing protocol commonly used in hello Internet tooexchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="abcd7-107">BGP möjliggör hello Azure VPN-gatewayer och dina lokala VPN-enheter som kallas BGP-peers eller grannar, tooexchange ”dirigerar” som informerar båda gateways på hello tillgänglighet och tillgängligheten för de prefix toogo via hello gateways eller routrar som ingår.</span><span class="sxs-lookup"><span data-stu-id="abcd7-107">BGP enables hello Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, tooexchange "routes" that will inform both gateways on hello availability and reachability for those prefixes toogo through hello gateways or routers involved.</span></span> <span data-ttu-id="abcd7-108">BGP kan också aktivera transitroutning mellan flera nätverk av sprider vägar som en gateway med BGP lär sig från en BGP peer tooall andra BGP-peers.</span><span class="sxs-lookup"><span data-stu-id="abcd7-108">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer tooall other BGP peers.</span></span>

<span data-ttu-id="abcd7-109">Se [översikt av BGP med Azure VPN-gatewayer](vpn-gateway-bgp-overview.md) mer beskrivning på fördelarna BGP och toounderstand hello tekniska krav och överväganden för att använda BGP.</span><span class="sxs-lookup"><span data-stu-id="abcd7-109">See [Overview of BGP with Azure VPN Gateways](vpn-gateway-bgp-overview.md) for more discussion on benefits of BGP and toounderstand hello technical requirements and considerations of using BGP.</span></span>

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a><span data-ttu-id="abcd7-110">Komma igång med BGP på Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="abcd7-110">Getting started with BGP on Azure VPN gateways</span></span>

<span data-ttu-id="abcd7-111">Den här artikeln vägleder dig genom hello steg toodo hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="abcd7-111">This article walks you through hello steps toodo hello following tasks:</span></span>

* [<span data-ttu-id="abcd7-112">Del 1 - Aktivera BGP på din Azure VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="abcd7-112">Part 1 - Enable BGP on your Azure VPN gateway</span></span>](#enablebgp)
* [<span data-ttu-id="abcd7-113">Del 2 – upprätta en anslutning mellan lokala BGP</span><span class="sxs-lookup"><span data-stu-id="abcd7-113">Part 2 - Establish a cross-premises connection with BGP</span></span>](#crossprembgp)
* [<span data-ttu-id="abcd7-114">Del 3: upprätta en anslutning för VNet-till-VNet med BGP</span><span class="sxs-lookup"><span data-stu-id="abcd7-114">Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>](#v2vbgp)

<span data-ttu-id="abcd7-115">Varje del av hello instruktioner utgör ett grundläggande byggblock för att aktivera BGP i nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="abcd7-115">Each part of hello instructions forms a basic building block for enabling BGP in your network connectivity.</span></span> <span data-ttu-id="abcd7-116">Om du har slutfört alla tre delar kan skapa hello topologi som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="abcd7-116">If you complete all three parts, you build hello topology as shown in hello following diagram:</span></span>

![BGP-topologi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

<span data-ttu-id="abcd7-118">Du kan kombinera delar tillsammans toobuild en mer komplexa, flera hopp övergångsnätverket som uppfyller dina behov.</span><span class="sxs-lookup"><span data-stu-id="abcd7-118">You can combine parts together toobuild a more complex, multi-hop, transit network that meets your needs.</span></span>

## <span data-ttu-id="abcd7-119"><a name ="enablebgp"></a>Del 1 – konfigurera BGP på hello Azure VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="abcd7-119"><a name ="enablebgp"></a>Part 1 - Configure BGP on hello Azure VPN Gateway</span></span>
<span data-ttu-id="abcd7-120">hello konfigurationssteg ställa in hello BGP-parametrarna för hello Azure VPN-gateway som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="abcd7-120">hello configuration steps set up hello BGP parameters of hello Azure VPN gateway as shown in hello following diagram:</span></span>

![BGP-Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a><span data-ttu-id="abcd7-122">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="abcd7-122">Before you begin</span></span>
* <span data-ttu-id="abcd7-123">Kontrollera att du har en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="abcd7-123">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="abcd7-124">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abcd7-124">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="abcd7-125">Installera hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="abcd7-125">Install hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="abcd7-126">Mer information om hur du installerar hello PowerShell-cmdlets finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="abcd7-126">For more information about installing hello PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="abcd7-127">Steg 1 – Skapa och konfigurera VNet1</span><span class="sxs-lookup"><span data-stu-id="abcd7-127">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="abcd7-128">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="abcd7-128">1. Declare your variables</span></span>
<span data-ttu-id="abcd7-129">Den här övningen starta genom att deklarera våra variabler.</span><span class="sxs-lookup"><span data-stu-id="abcd7-129">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="abcd7-130">hello förklarar följande exempel hello variabler med hello värden för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="abcd7-130">hello following example declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="abcd7-131">Vara säker på att tooreplace hello värden med dina egna när du konfigurerar för produktion.</span><span class="sxs-lookup"><span data-stu-id="abcd7-131">Be sure tooreplace hello values with your own when configuring for production.</span></span> <span data-ttu-id="abcd7-132">Du kan använda dessa variabler om du kör via hello steg toobecome bekant med den här typen av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="abcd7-132">You can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="abcd7-133">Ändra hello variabler, kopiera och klistra in i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="abcd7-133">Modify hello variables, and then copy and paste into your PowerShell console.</span></span>

```powershell
$Sub1 = "Replace_With_Your_Subcription_Name"
$RG1 = "TestBGPRG1"
$Location1 = "East US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GWIPName1 = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection12 = "VNet1toVNet2"
$Connection15 = "VNet1toSite5"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="abcd7-134">2. Anslut tooyour prenumeration och skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="abcd7-134">2. Connect tooyour subscription and create a new resource group</span></span>
<span data-ttu-id="abcd7-135">toouse hello Resource Manager cmdlets, se till att du växla tooPowerShell läge.</span><span class="sxs-lookup"><span data-stu-id="abcd7-135">toouse hello Resource Manager cmdlets, Make sure you switch tooPowerShell mode.</span></span> <span data-ttu-id="abcd7-136">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="abcd7-136">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="abcd7-137">Öppna PowerShell-konsolen och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="abcd7-137">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="abcd7-138">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="abcd7-138">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="abcd7-139">3. Skapa TestVNet1</span><span class="sxs-lookup"><span data-stu-id="abcd7-139">3. Create TestVNet1</span></span>
<span data-ttu-id="abcd7-140">hello skapar följande exempel ett virtuellt nätverk med namnet TestVNet1 och tre undernät, en kallas GatewaySubnet, en klientdel som anropats och ett kallas Backend.</span><span class="sxs-lookup"><span data-stu-id="abcd7-140">hello following sample creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="abcd7-141">När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="abcd7-141">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="abcd7-142">Om du ger det något annat namn går det inte att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="abcd7-142">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-bgp-parameters"></a><span data-ttu-id="abcd7-143">Steg 2 – Skapa hello VPN-Gateway för TestVNet1 med BGP-parametrar</span><span class="sxs-lookup"><span data-stu-id="abcd7-143">Step 2 - Create hello VPN Gateway for TestVNet1 with BGP parameters</span></span>
#### <a name="1-create-hello-ip-and-subnet-configurations"></a><span data-ttu-id="abcd7-144">1. Skapa hello IP-adress och nätmask</span><span class="sxs-lookup"><span data-stu-id="abcd7-144">1. Create hello IP and subnet configurations</span></span>
<span data-ttu-id="abcd7-145">Begär en offentlig IP-adress toobe allokerade toohello gateway skapas för ditt VNet.</span><span class="sxs-lookup"><span data-stu-id="abcd7-145">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="abcd7-146">Du måste också definiera hello krävs undernät och IP-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="abcd7-146">You'll also define hello required subnet and IP configurations.</span></span>

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-hello-vpn-gateway-with-hello-as-number"></a><span data-ttu-id="abcd7-147">2. Skapa hello VPN-gateway med hello som tal</span><span class="sxs-lookup"><span data-stu-id="abcd7-147">2. Create hello VPN gateway with hello AS number</span></span>
<span data-ttu-id="abcd7-148">Skapa hello virtuell nätverksgateway för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="abcd7-148">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="abcd7-149">BGP kräver en Ruttbaserad VPN-gateway och hello tillägg parametern, - Asn tooset hello ASN (AS Number) för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="abcd7-149">BGP requires a Route-Based VPN gateway, and also hello addition parameter, -Asn, tooset hello ASN (AS Number) for TestVNet1.</span></span> <span data-ttu-id="abcd7-150">Om du inte anger hello ASN parametern tilldelas ASN 65515.</span><span class="sxs-lookup"><span data-stu-id="abcd7-150">If you do not set hello ASN parameter, ASN 65515 is assigned.</span></span> <span data-ttu-id="abcd7-151">Skapa en gateway kan ta ett tag (30 minuter eller mer toocomplete).</span><span class="sxs-lookup"><span data-stu-id="abcd7-151">Creating a gateway can take a while (30 minutes or more toocomplete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-hello-azure-bgp-peer-ip-address"></a><span data-ttu-id="abcd7-152">3. Hämta hello Azure BGP peer-IP-adress</span><span class="sxs-lookup"><span data-stu-id="abcd7-152">3. Obtain hello Azure BGP Peer IP address</span></span>
<span data-ttu-id="abcd7-153">När hello gateway har skapats måste tooobtain hello BGP-Peer-IP-adress på hello Azure VPN-Gateway.</span><span class="sxs-lookup"><span data-stu-id="abcd7-153">Once hello gateway is created, you need tooobtain hello BGP Peer IP address on hello Azure VPN Gateway.</span></span> <span data-ttu-id="abcd7-154">Den här adressen är nödvändiga tooconfigure hello Azure VPN-Gateway som en BGP-Peer för dina lokala VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="abcd7-154">This address is needed tooconfigure hello Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

<span data-ttu-id="abcd7-155">hello sista kommandot visar hello motsvarande BGP konfigurationer på hello Azure VPN-Gateway. Exempel:</span><span class="sxs-lookup"><span data-stu-id="abcd7-155">hello last command shows hello corresponding BGP configurations on hello Azure VPN Gateway; for example:</span></span>

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

<span data-ttu-id="abcd7-156">När hello gateway har skapats kan använda du denna gateway tooestablish mellan platser eller VNet-till-VNet-anslutning med BGP.</span><span class="sxs-lookup"><span data-stu-id="abcd7-156">Once hello gateway is created, you can use this gateway tooestablish cross-premises connection or VNet-to-VNet connection with BGP.</span></span> <span data-ttu-id="abcd7-157">hello igenom avsnitten hello steg toocomplete hello övningen.</span><span class="sxs-lookup"><span data-stu-id="abcd7-157">hello following sections walk through hello steps toocomplete hello exercise.</span></span>

## <span data-ttu-id="abcd7-158"><a name ="crossprembbgp"></a>Del 2 – upprätta en anslutning mellan lokala BGP</span><span class="sxs-lookup"><span data-stu-id="abcd7-158"><a name ="crossprembbgp"></a>Part 2 - Establish a cross-premises connection with BGP</span></span>

<span data-ttu-id="abcd7-159">tooestablish en anslutning mellan platser, behöver du toocreate en lokal nätverksgateway toorepresent din lokala VPN-enhet och en anslutning tooconnect hello VPN-gateway med hello lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="abcd7-159">tooestablish a cross-premises connection, you need toocreate a Local Network Gateway toorepresent your on-premises VPN device, and a Connection tooconnect hello VPN gateway with hello local network gateway.</span></span> <span data-ttu-id="abcd7-160">Det finns artiklar som vägleder dig genom stegen, innehåller den här artikeln hello ytterligare egenskaper krävs toospecify hello BGP konfigurationsparametrar.</span><span class="sxs-lookup"><span data-stu-id="abcd7-160">While there are articles that walk you through these steps, this article contains hello additional properties required toospecify hello BGP configuration parameters.</span></span>

![BGP för mellan platser](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

<span data-ttu-id="abcd7-162">Innan du fortsätter bör du kontrollera att du har slutfört [del 1](#enablebgp) för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="abcd7-162">Before proceeding, make sure you have completed [Part 1](#enablebgp) of this exercise.</span></span>

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a><span data-ttu-id="abcd7-163">Steg 1 – Skapa och konfigurera hello lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="abcd7-163">Step 1 - Create and configure hello local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="abcd7-164">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="abcd7-164">1. Declare your variables</span></span>

<span data-ttu-id="abcd7-165">Den här övningen fortsätter toobuild hello-konfiguration visas i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="abcd7-165">This exercise continues toobuild hello configuration shown in hello diagram.</span></span> <span data-ttu-id="abcd7-166">Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="abcd7-166">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

<span data-ttu-id="abcd7-167">Ett par saker toonote om hello lokala gateway nätverksparametrar:</span><span class="sxs-lookup"><span data-stu-id="abcd7-167">A couple of things toonote regarding hello local network gateway parameters:</span></span>

* <span data-ttu-id="abcd7-168">hello lokal nätverksgateway kan vara i hello samma eller annan plats och grupp som hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="abcd7-168">hello local network gateway can be in hello same or different location and resource group as hello VPN gateway.</span></span> <span data-ttu-id="abcd7-169">Det här exemplet visar dem i olika resursgrupper på olika platser.</span><span class="sxs-lookup"><span data-stu-id="abcd7-169">This example shows them in different resource groups in different locations.</span></span>
* <span data-ttu-id="abcd7-170">hello minsta prefix som du behöver toodeclare för hello lokal nätverksgateway är hello värden adressen för BGP-Peer-IP-adress på VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="abcd7-170">hello minimum prefix you need toodeclare for hello local network gateway is hello host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="abcd7-171">I det här fallet är det en /32 prefixet ”10.52.255.254/32”.</span><span class="sxs-lookup"><span data-stu-id="abcd7-171">In this case, it's a /32 prefix of "10.52.255.254/32".</span></span>
* <span data-ttu-id="abcd7-172">Du måste använda olika BGP, ASN: er, mellan ditt lokala nätverk och Azure VNet som en påminnelse.</span><span class="sxs-lookup"><span data-stu-id="abcd7-172">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="abcd7-173">Om de är hello samma måste toochange ditt VNet ASN om din lokala VPN-enhet redan använder hello ASN toopeer med andra BGP-grannar.</span><span class="sxs-lookup"><span data-stu-id="abcd7-173">If they are hello same, you need toochange your VNet ASN if your on-premises VPN device already uses hello ASN toopeer with other BGP neighbors.</span></span>

<span data-ttu-id="abcd7-174">Innan du fortsätter kan du kontrollera att du är fortfarande ansluten tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="abcd7-174">Before you continue, make sure you are still connected tooSubscription 1.</span></span>

#### <a name="2-create-hello-local-network-gateway-for-site5"></a><span data-ttu-id="abcd7-175">2. Skapa hello lokal nätverksgateway för Site5</span><span class="sxs-lookup"><span data-stu-id="abcd7-175">2. Create hello local network gateway for Site5</span></span>

<span data-ttu-id="abcd7-176">Vara att toocreate hello resursgrupp om den inte har skapats innan du skapar hello lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="abcd7-176">Be sure toocreate hello resource group if it is not created, before you create hello local network gateway.</span></span> <span data-ttu-id="abcd7-177">Lägg märke till hello två ytterligare parametrar för hello lokal nätverksgateway: Asn och BgpPeerAddress.</span><span class="sxs-lookup"><span data-stu-id="abcd7-177">Notice hello two additional parameters for hello local network gateway: Asn and BgpPeerAddress.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="abcd7-178">Steg 2 – ansluta hello VNet gateway och lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="abcd7-178">Step 2 - Connect hello VNet gateway and local network gateway</span></span>

#### <a name="1-get-hello-two-gateways"></a><span data-ttu-id="abcd7-179">1. Hämta hello två gateways</span><span class="sxs-lookup"><span data-stu-id="abcd7-179">1. Get hello two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a><span data-ttu-id="abcd7-180">2. Skapa hello TestVNet1 tooSite5 anslutning</span><span class="sxs-lookup"><span data-stu-id="abcd7-180">2. Create hello TestVNet1 tooSite5 connection</span></span>

<span data-ttu-id="abcd7-181">I det här steget skapar du hello anslutning från TestVNet1 tooSite5.</span><span class="sxs-lookup"><span data-stu-id="abcd7-181">In this step, you create hello connection from TestVNet1 tooSite5.</span></span> <span data-ttu-id="abcd7-182">Du måste ange ”-Enablegpg $True” tooenable BGP för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="abcd7-182">You must specify "-EnableBGP $True" tooenable BGP for this connection.</span></span> <span data-ttu-id="abcd7-183">Som tidigare diskuterats, det är möjligt toohave både BGP och icke-BGP-anslutningarna för hello samma Azure VPN-Gateway.</span><span class="sxs-lookup"><span data-stu-id="abcd7-183">As discussed earlier, it is possible toohave both BGP and non-BGP connections for hello same Azure VPN Gateway.</span></span> <span data-ttu-id="abcd7-184">Om BGP är aktiverat i Anslutningsegenskapen hello kommer Azure inte aktivera BGP för den här anslutningen även om BGP parametrar har redan konfigurerats på båda gateways.</span><span class="sxs-lookup"><span data-stu-id="abcd7-184">Unless BGP is enabled in hello connection property, Azure will not enable BGP for this connection even though BGP parameters are already configured on both gateways.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

<span data-ttu-id="abcd7-185">hello visas följande exempel hello-parametrar som du anger i hello BGP konfigurationsavsnittet på din lokala VPN-enhet för den här övningen:</span><span class="sxs-lookup"><span data-stu-id="abcd7-185">hello following example lists hello parameters you enter into hello BGP configuration section on your on-premises VPN device for this exercise:</span></span>

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being hello VPN tunnel interface on your device
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="abcd7-186">hello-anslutningen har upprättats efter några minuter och hello BGP peering-sessionen startar när hello IPsec-anslutning har upprättats.</span><span class="sxs-lookup"><span data-stu-id="abcd7-186">hello connection is established after a few minutes, and hello BGP peering session starts once hello IPsec connection is established.</span></span>

## <span data-ttu-id="abcd7-187"><a name ="v2vbgp"></a>Del 3: upprätta en anslutning för VNet-till-VNet med BGP</span><span class="sxs-lookup"><span data-stu-id="abcd7-187"><a name ="v2vbgp"></a>Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>

<span data-ttu-id="abcd7-188">Det här avsnittet lägger till en VNet-till-VNet-anslutning med BGP, som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="abcd7-188">This section adds a VNet-to-VNet connection with BGP, as shown in hello following diagram:</span></span>

![BGP för VNet-till-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

<span data-ttu-id="abcd7-190">hello följa anvisningar fortsätta från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="abcd7-190">hello following instructions continue from hello previous steps.</span></span> <span data-ttu-id="abcd7-191">Du måste slutföra [del I](#enablebgp) toocreate TestVNet1 och konfigurera hello VPN-Gateway med BGP.</span><span class="sxs-lookup"><span data-stu-id="abcd7-191">You must complete [Part I](#enablebgp) toocreate and configure TestVNet1 and hello VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a><span data-ttu-id="abcd7-192">Steg 1 – Skapa TestVNet2 och hello VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="abcd7-192">Step 1 - Create TestVNet2 and hello VPN gateway</span></span>

<span data-ttu-id="abcd7-193">Det är viktigt toomake till att hello IP-adressutrymme hello nytt virtuellt nätverk, TestVNet2, inte överlappar med någon av VNet-intervall.</span><span class="sxs-lookup"><span data-stu-id="abcd7-193">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="abcd7-194">I det här exemplet hello virtuella nätverk som tillhör toohello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="abcd7-194">In this example, hello virtual networks belong toohello same subscription.</span></span> <span data-ttu-id="abcd7-195">Du kan konfigurera VNet-till-VNet-anslutningar mellan olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="abcd7-195">You can set up VNet-to-VNet connections between different subscriptions.</span></span> <span data-ttu-id="abcd7-196">Mer information finns i [konfigurera VNet-till-VNet-anslutningen](vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="abcd7-196">For more information, see [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md).</span></span> <span data-ttu-id="abcd7-197">Kontrollera att du lägger till hello ”-Enablegpg $True” när skapar hello anslutningar tooenable BGP.</span><span class="sxs-lookup"><span data-stu-id="abcd7-197">Make sure you add hello "-EnableBgp $True" when creating hello connections tooenable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="abcd7-198">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="abcd7-198">1. Declare your variables</span></span>

<span data-ttu-id="abcd7-199">Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="abcd7-199">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG2 = "TestBGPRG2"
$Location2 = "West US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GWIPName2 = "VNet2GWIP"
$GWIPconfName2 = "gwipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a><span data-ttu-id="abcd7-200">2. Skapa TestVNet2 i hello ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="abcd7-200">2. Create TestVNet2 in hello new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-vpn-gateway-for-testvnet2-with-bgp-parameters"></a><span data-ttu-id="abcd7-201">3. Skapa hello VPN-gateway för TestVNet2 med BGP-parametrar</span><span class="sxs-lookup"><span data-stu-id="abcd7-201">3. Create hello VPN gateway for TestVNet2 with BGP parameters</span></span>

<span data-ttu-id="abcd7-202">Begär en offentlig IP-adress toobe allokerade toohello gateway du skapar för din VNet och definiera hello krävs undernät och IP-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="abcd7-202">Request a public IP address toobe allocated toohello gateway you will create for your VNet and define hello required subnet and IP configurations.</span></span>

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

<span data-ttu-id="abcd7-203">Skapa hello VPN-gateway med hello som tal.</span><span class="sxs-lookup"><span data-stu-id="abcd7-203">Create hello VPN gateway with hello AS number.</span></span> <span data-ttu-id="abcd7-204">Du måste åsidosätta hello standard ASN på Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="abcd7-204">You must override hello default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="abcd7-205">hello ASN: er för hello anslutna Vnet måste vara olika tooenable BGP och transitroutning.</span><span class="sxs-lookup"><span data-stu-id="abcd7-205">hello ASNs for hello connected VNets must be different tooenable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="abcd7-206">Steg 2 – ansluta hello TestVNet1 och TestVNet2 gateways</span><span class="sxs-lookup"><span data-stu-id="abcd7-206">Step 2 - Connect hello TestVNet1 and TestVNet2 gateways</span></span>

<span data-ttu-id="abcd7-207">I det här exemplet är både gateways i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="abcd7-207">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="abcd7-208">Du kan slutföra det här steget i hello samma PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="abcd7-208">You can complete this step in hello same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="abcd7-209">1. Hämta båda gateways</span><span class="sxs-lookup"><span data-stu-id="abcd7-209">1. Get both gateways</span></span>

<span data-ttu-id="abcd7-210">Kontrollera att du loggar in och ansluta tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="abcd7-210">Make sure you log in and connect tooSubscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="abcd7-211">2. Skapa både anslutningar</span><span class="sxs-lookup"><span data-stu-id="abcd7-211">2. Create both connections</span></span>

<span data-ttu-id="abcd7-212">I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet2 och hello anslutning från TestVNet2 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="abcd7-212">In this step, you create hello connection from TestVNet1 tooTestVNet2, and hello connection from TestVNet2 tooTestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="abcd7-213">Vara säker på att tooenable BGP för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="abcd7-213">Be sure tooenable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="abcd7-214">När du har slutfört de här stegen ansluta hello efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="abcd7-214">After completing these steps, hello connection is established after a few minutes.</span></span> <span data-ttu-id="abcd7-215">hello BGP-peeringsessionen är upp när hello VNet-till-VNet-anslutningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="abcd7-215">hello BGP peering session is up once hello VNet-to-VNet connection is completed.</span></span>

<span data-ttu-id="abcd7-216">Om du har slutfört alla tre delar av den här övningen har du etablerat hello efter nätverkets topologi:</span><span class="sxs-lookup"><span data-stu-id="abcd7-216">If you completed all three parts of this exercise, you have established hello following network topology:</span></span>

![BGP för VNet-till-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a><span data-ttu-id="abcd7-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="abcd7-218">Next steps</span></span>

<span data-ttu-id="abcd7-219">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="abcd7-219">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="abcd7-220">Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="abcd7-220">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
