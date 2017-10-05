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
ms.openlocfilehash: b00a3fe7ba4b12c2e9c486188c292cd6fafb60a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a><span data-ttu-id="43317-103">Hur du konfigurerar BGP på Azure VPN-gatewayer med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="43317-103">How to configure BGP on Azure VPN Gateways using PowerShell</span></span>
<span data-ttu-id="43317-104">Den här artikeln vägleder dig genom stegen för att aktivera BGP på en mellan lokala plats-till-plats (S2S) VPN-anslutning och en VNet-till-VNet-anslutning med Resource Manager-distributionsmodellen och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="43317-104">This article walks you through the steps to enable BGP on a cross-premises Site-to-Site (S2S) VPN connection and a VNet-to-VNet connection using the Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-bgp"></a><span data-ttu-id="43317-105">Om BGP</span><span class="sxs-lookup"><span data-stu-id="43317-105">About BGP</span></span>
<span data-ttu-id="43317-106">BGP är ett standardroutningsprotokoll som vanligen används på Internet för att utbyta information om routning och åtkomst mellan två eller flera nätverk.</span><span class="sxs-lookup"><span data-stu-id="43317-106">BGP is the standard routing protocol commonly used in the Internet to exchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="43317-107">BGP kan Azure VPN-gatewayer och din lokala VPN-enheter som kallas BGP-peers eller grannar, för att utbyta ”dirigerar” som ger information om båda gateways på tillgängligheten och tillgängligheten för de prefix som ska gå igenom gateways eller routrar som ingår.</span><span class="sxs-lookup"><span data-stu-id="43317-107">BGP enables the Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, to exchange "routes" that will inform both gateways on the availability and reachability for those prefixes to go through the gateways or routers involved.</span></span> <span data-ttu-id="43317-108">BGP kan också möjliggöra överföringsroutning mellan flera nätverk genom att sprida vägar som BGP-gatewayen får information om från en BGP-peer till alla andra BGP-peers.</span><span class="sxs-lookup"><span data-stu-id="43317-108">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer to all other BGP peers.</span></span>

<span data-ttu-id="43317-109">Se [översikt av BGP med Azure VPN-gatewayer](vpn-gateway-bgp-overview.md) för flera diskussion om fördelarna med BGP och att förstå de tekniska krav och överväganden för att använda BGP.</span><span class="sxs-lookup"><span data-stu-id="43317-109">See [Overview of BGP with Azure VPN Gateways](vpn-gateway-bgp-overview.md) for more discussion on benefits of BGP and to understand the technical requirements and considerations of using BGP.</span></span>

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a><span data-ttu-id="43317-110">Komma igång med BGP på Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="43317-110">Getting started with BGP on Azure VPN gateways</span></span>

<span data-ttu-id="43317-111">Den här artikeln vägleder dig genom stegen för att göra följande:</span><span class="sxs-lookup"><span data-stu-id="43317-111">This article walks you through the steps to do the following tasks:</span></span>

* [<span data-ttu-id="43317-112">Del 1 - Aktivera BGP på din Azure VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="43317-112">Part 1 - Enable BGP on your Azure VPN gateway</span></span>](#enablebgp)
* [<span data-ttu-id="43317-113">Del 2 – upprätta en anslutning mellan lokala BGP</span><span class="sxs-lookup"><span data-stu-id="43317-113">Part 2 - Establish a cross-premises connection with BGP</span></span>](#crossprembgp)
* [<span data-ttu-id="43317-114">Del 3: upprätta en anslutning för VNet-till-VNet med BGP</span><span class="sxs-lookup"><span data-stu-id="43317-114">Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>](#v2vbgp)

<span data-ttu-id="43317-115">Varje del av instruktionerna utgör ett grundläggande byggblock för att aktivera BGP i nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="43317-115">Each part of the instructions forms a basic building block for enabling BGP in your network connectivity.</span></span> <span data-ttu-id="43317-116">Om du har slutfört alla tre delar kan skapa topologin som visas i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="43317-116">If you complete all three parts, you build the topology as shown in the following diagram:</span></span>

![BGP-topologi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

<span data-ttu-id="43317-118">Du kan kombinera delar tillsammans för att skapa en mer komplexa, flera hopp övergångsnätverket som uppfyller dina behov.</span><span class="sxs-lookup"><span data-stu-id="43317-118">You can combine parts together to build a more complex, multi-hop, transit network that meets your needs.</span></span>

## <span data-ttu-id="43317-119"><a name ="enablebgp"></a>Del 1 – konfigurera BGP på Azure VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="43317-119"><a name ="enablebgp"></a>Part 1 - Configure BGP on the Azure VPN Gateway</span></span>
<span data-ttu-id="43317-120">Konfigurationsstegen ställa in Azure VPN-gateway BGP-parametrar som visas i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="43317-120">The configuration steps set up the BGP parameters of the Azure VPN gateway as shown in the following diagram:</span></span>

![BGP-Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a><span data-ttu-id="43317-122">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="43317-122">Before you begin</span></span>
* <span data-ttu-id="43317-123">Kontrollera att du har en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="43317-123">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="43317-124">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43317-124">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="43317-125">Installera Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="43317-125">Install the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="43317-126">Mer information om hur du installerar PowerShell-cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="43317-126">For more information about installing the PowerShell cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="43317-127">Steg 1 – Skapa och konfigurera VNet1</span><span class="sxs-lookup"><span data-stu-id="43317-127">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="43317-128">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="43317-128">1. Declare your variables</span></span>
<span data-ttu-id="43317-129">Den här övningen starta genom att deklarera våra variabler.</span><span class="sxs-lookup"><span data-stu-id="43317-129">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="43317-130">I följande exempel deklarerar variabler med hjälp av värdena för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="43317-130">The following example declares the variables using the values for this exercise.</span></span> <span data-ttu-id="43317-131">Se till att ersätta värdena med dina egna när du konfigurerar för produktion.</span><span class="sxs-lookup"><span data-stu-id="43317-131">Be sure to replace the values with your own when configuring for production.</span></span> <span data-ttu-id="43317-132">Du kan använda dessa variabler om du använder anvisningarna för att bekanta dig med den här typen av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="43317-132">You can use these variables if you are running through the steps to become familiar with this type of configuration.</span></span> <span data-ttu-id="43317-133">Ändra variablerna samt kopiera och klistra in dem i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="43317-133">Modify the variables, and then copy and paste into your PowerShell console.</span></span>

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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="43317-134">2. Anslut till din prenumeration och skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="43317-134">2. Connect to your subscription and create a new resource group</span></span>
<span data-ttu-id="43317-135">Kontrollera att du växlar till PowerShell-läge för att använda Resource Manager-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="43317-135">To use the Resource Manager cmdlets, Make sure you switch to PowerShell mode.</span></span> <span data-ttu-id="43317-136">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="43317-136">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="43317-137">Öppna PowerShell-konsolen och anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="43317-137">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="43317-138">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="43317-138">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="43317-139">3. Skapa TestVNet1</span><span class="sxs-lookup"><span data-stu-id="43317-139">3. Create TestVNet1</span></span>
<span data-ttu-id="43317-140">I följande exempel skapar ett virtuellt nätverk med namnet TestVNet1 och tre undernät, en kallas GatewaySubnet, en klientdel som anropats och ett kallas Backend.</span><span class="sxs-lookup"><span data-stu-id="43317-140">The following sample creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="43317-141">När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="43317-141">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="43317-142">Om du ger det något annat namn går det inte att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="43317-142">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a><span data-ttu-id="43317-143">Steg 2 – skapa VPN-Gateway för TestVNet1 med BGP-parametrar</span><span class="sxs-lookup"><span data-stu-id="43317-143">Step 2 - Create the VPN Gateway for TestVNet1 with BGP parameters</span></span>
#### <a name="1-create-the-ip-and-subnet-configurations"></a><span data-ttu-id="43317-144">1. Skapa IP-adress och nätmask konfigurationer</span><span class="sxs-lookup"><span data-stu-id="43317-144">1. Create the IP and subnet configurations</span></span>
<span data-ttu-id="43317-145">Begär en offentlig IP-adress som ska allokeras till den gateway som du ska skapa för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="43317-145">Request a public IP address to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="43317-146">Du måste också definiera krävs undernätet och IP-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="43317-146">You'll also define the required subnet and IP configurations.</span></span>

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a><span data-ttu-id="43317-147">2. Skapa VPN-gateway med AS-nummer</span><span class="sxs-lookup"><span data-stu-id="43317-147">2. Create the VPN gateway with the AS number</span></span>
<span data-ttu-id="43317-148">Skapa den virtuella nätverksgatewayen för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="43317-148">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="43317-149">BGP kräver en Ruttbaserad VPN-gateway och parametern tillägg, Asn - ange ASN (AS Number) för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="43317-149">BGP requires a Route-Based VPN gateway, and also the addition parameter, -Asn, to set the ASN (AS Number) for TestVNet1.</span></span> <span data-ttu-id="43317-150">Om du inte anger parametern ASN tilldelas ASN 65515.</span><span class="sxs-lookup"><span data-stu-id="43317-150">If you do not set the ASN parameter, ASN 65515 is assigned.</span></span> <span data-ttu-id="43317-151">Att skapa en gateway kan ta ett tag (30 minuter eller mer att slutföra).</span><span class="sxs-lookup"><span data-stu-id="43317-151">Creating a gateway can take a while (30 minutes or more to complete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a><span data-ttu-id="43317-152">3. Hämta Azure BGP-Peer-IP-adress</span><span class="sxs-lookup"><span data-stu-id="43317-152">3. Obtain the Azure BGP Peer IP address</span></span>
<span data-ttu-id="43317-153">När gatewayen är skapad, måste du skaffa BGP-Peer-IP-adressen på Azure VPN-Gateway.</span><span class="sxs-lookup"><span data-stu-id="43317-153">Once the gateway is created, you need to obtain the BGP Peer IP address on the Azure VPN Gateway.</span></span> <span data-ttu-id="43317-154">Den här adressen behövs för att konfigurera Azure VPN-Gateway som en BGP-Peer för dina lokala VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="43317-154">This address is needed to configure the Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

<span data-ttu-id="43317-155">Det sista kommandot visas motsvarande BGP konfigurationer på Azure VPN-Gateway. Exempel:</span><span class="sxs-lookup"><span data-stu-id="43317-155">The last command shows the corresponding BGP configurations on the Azure VPN Gateway; for example:</span></span>

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

<span data-ttu-id="43317-156">Du kan använda denna gateway för att upprätta anslutningar mellan lokala eller VNet-till-VNet-anslutning med BGP när gatewayen har skapats.</span><span class="sxs-lookup"><span data-stu-id="43317-156">Once the gateway is created, you can use this gateway to establish cross-premises connection or VNet-to-VNet connection with BGP.</span></span> <span data-ttu-id="43317-157">I följande avsnitt gå igenom stegen för att slutföra den här övningen.</span><span class="sxs-lookup"><span data-stu-id="43317-157">The following sections walk through the steps to complete the exercise.</span></span>

## <span data-ttu-id="43317-158"><a name ="crossprembbgp"></a>Del 2 – upprätta en anslutning mellan lokala BGP</span><span class="sxs-lookup"><span data-stu-id="43317-158"><a name ="crossprembbgp"></a>Part 2 - Establish a cross-premises connection with BGP</span></span>

<span data-ttu-id="43317-159">För att upprätta en anslutning mellan platser, måste du skapa en lokal nätverksgateway som representerar din lokala VPN-enhet och en anslutning till ansluta VPN-gateway med den lokala nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="43317-159">To establish a cross-premises connection, you need to create a Local Network Gateway to represent your on-premises VPN device, and a Connection to connect the VPN gateway with the local network gateway.</span></span> <span data-ttu-id="43317-160">Det finns artiklar som vägleder dig genom stegen, innehåller den här artikeln ytterligare egenskaper som krävs för att ange parametrar för BGP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="43317-160">While there are articles that walk you through these steps, this article contains the additional properties required to specify the BGP configuration parameters.</span></span>

![BGP för mellan platser](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

<span data-ttu-id="43317-162">Innan du fortsätter bör du kontrollera att du har slutfört [del 1](#enablebgp) för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="43317-162">Before proceeding, make sure you have completed [Part 1](#enablebgp) of this exercise.</span></span>

### <a name="step-1---create-and-configure-the-local-network-gateway"></a><span data-ttu-id="43317-163">Steg 1 – Skapa och konfigurera den lokala nätverksgatewayen</span><span class="sxs-lookup"><span data-stu-id="43317-163">Step 1 - Create and configure the local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="43317-164">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="43317-164">1. Declare your variables</span></span>

<span data-ttu-id="43317-165">Den här övningen fortsätter att skapa den konfiguration som visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="43317-165">This exercise continues to build the configuration shown in the diagram.</span></span> <span data-ttu-id="43317-166">Ersätt värdena med de som du vill använda för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="43317-166">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

<span data-ttu-id="43317-167">Några saker att Observera för de lokala nätverket gateway-parametrarna:</span><span class="sxs-lookup"><span data-stu-id="43317-167">A couple of things to note regarding the local network gateway parameters:</span></span>

* <span data-ttu-id="43317-168">Den lokala nätverksgatewayen kan vara i samma eller en annan plats och resursgruppen som VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="43317-168">The local network gateway can be in the same or different location and resource group as the VPN gateway.</span></span> <span data-ttu-id="43317-169">Det här exemplet visar dem i olika resursgrupper på olika platser.</span><span class="sxs-lookup"><span data-stu-id="43317-169">This example shows them in different resource groups in different locations.</span></span>
* <span data-ttu-id="43317-170">Minsta prefixet måste du deklarera för den lokala nätverksgatewayen är värdadressen för BGP-Peer-IP-adress på VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="43317-170">The minimum prefix you need to declare for the local network gateway is the host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="43317-171">I det här fallet är det en /32 prefixet ”10.52.255.254/32”.</span><span class="sxs-lookup"><span data-stu-id="43317-171">In this case, it's a /32 prefix of "10.52.255.254/32".</span></span>
* <span data-ttu-id="43317-172">Du måste använda olika BGP, ASN: er, mellan ditt lokala nätverk och Azure VNet som en påminnelse.</span><span class="sxs-lookup"><span data-stu-id="43317-172">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="43317-173">Om de är samma som du behöver ändra ditt VNet ASN om din lokala VPN-enhet använder redan ASN till peer-datorn med andra BGP-grannar.</span><span class="sxs-lookup"><span data-stu-id="43317-173">If they are the same, you need to change your VNet ASN if your on-premises VPN device already uses the ASN to peer with other BGP neighbors.</span></span>

<span data-ttu-id="43317-174">Innan du fortsätter kontrollerar du att du fortfarande är ansluten till Prenumeration 1.</span><span class="sxs-lookup"><span data-stu-id="43317-174">Before you continue, make sure you are still connected to Subscription 1.</span></span>

#### <a name="2-create-the-local-network-gateway-for-site5"></a><span data-ttu-id="43317-175">2. Skapa lokal nätverksgateway för Site5</span><span class="sxs-lookup"><span data-stu-id="43317-175">2. Create the local network gateway for Site5</span></span>

<span data-ttu-id="43317-176">Se till att skapa resursgruppen om det inte har skapats innan du skapar den lokala nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="43317-176">Be sure to create the resource group if it is not created, before you create the local network gateway.</span></span> <span data-ttu-id="43317-177">Lägg märke till de två andra parametrarna för den lokala nätverksgatewayen: Asn och BgpPeerAddress.</span><span class="sxs-lookup"><span data-stu-id="43317-177">Notice the two additional parameters for the local network gateway: Asn and BgpPeerAddress.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="43317-178">Steg 2 – ansluta VNet gateway och lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="43317-178">Step 2 - Connect the VNet gateway and local network gateway</span></span>

#### <a name="1-get-the-two-gateways"></a><span data-ttu-id="43317-179">1. Hämta två gateways</span><span class="sxs-lookup"><span data-stu-id="43317-179">1. Get the two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a><span data-ttu-id="43317-180">2. Skapa TestVNet1 till Site5 anslutning</span><span class="sxs-lookup"><span data-stu-id="43317-180">2. Create the TestVNet1 to Site5 connection</span></span>

<span data-ttu-id="43317-181">I det här steget skapar du anslutningen från TestVNet1 till Site5.</span><span class="sxs-lookup"><span data-stu-id="43317-181">In this step, you create the connection from TestVNet1 to Site5.</span></span> <span data-ttu-id="43317-182">Du måste ange ”-Enablegpg $True” att aktivera BGP för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="43317-182">You must specify "-EnableBGP $True" to enable BGP for this connection.</span></span> <span data-ttu-id="43317-183">Som tidigare diskuterats, är det möjligt att ha både BGP och icke-BGP-anslutningar för samma Azure VPN-Gateway.</span><span class="sxs-lookup"><span data-stu-id="43317-183">As discussed earlier, it is possible to have both BGP and non-BGP connections for the same Azure VPN Gateway.</span></span> <span data-ttu-id="43317-184">Om BGP är aktiverat i Anslutningsegenskapen ska Azure inte aktivera BGP för den här anslutningen även om BGP parametrar har redan konfigurerats på båda gateways.</span><span class="sxs-lookup"><span data-stu-id="43317-184">Unless BGP is enabled in the connection property, Azure will not enable BGP for this connection even though BGP parameters are already configured on both gateways.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

<span data-ttu-id="43317-185">I följande exempel visas de parametrar som du anger i konfigurationsavsnittet BGP på din lokala VPN-enhet för den här övningen:</span><span class="sxs-lookup"><span data-stu-id="43317-185">The following example lists the parameters you enter into the BGP configuration section on your on-premises VPN device for this exercise:</span></span>

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="43317-186">BGP-peeringsessionen startas en gång IPsec anslutningen upprättas anslutningen upprättas efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="43317-186">The connection is established after a few minutes, and the BGP peering session starts once the IPsec connection is established.</span></span>

## <span data-ttu-id="43317-187"><a name ="v2vbgp"></a>Del 3: upprätta en anslutning för VNet-till-VNet med BGP</span><span class="sxs-lookup"><span data-stu-id="43317-187"><a name ="v2vbgp"></a>Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>

<span data-ttu-id="43317-188">Det här avsnittet lägger till en VNet-till-VNet-anslutning med BGP, som visas i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="43317-188">This section adds a VNet-to-VNet connection with BGP, as shown in the following diagram:</span></span>

![BGP för VNet-till-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

<span data-ttu-id="43317-190">Följande instruktioner fortsätta från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="43317-190">The following instructions continue from the previous steps.</span></span> <span data-ttu-id="43317-191">Du måste slutföra [del I](#enablebgp) att skapa och konfigurera TestVNet1 och VPN-Gateway med BGP.</span><span class="sxs-lookup"><span data-stu-id="43317-191">You must complete [Part I](#enablebgp) to create and configure TestVNet1 and the VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a><span data-ttu-id="43317-192">Steg 1 – Skapa TestVNet2 och VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="43317-192">Step 1 - Create TestVNet2 and the VPN gateway</span></span>

<span data-ttu-id="43317-193">Det är viktigt att se till att IP-adressutrymmet för det nya virtuella nätverket TestVNet2, inte överlappar med någon av VNet-intervall.</span><span class="sxs-lookup"><span data-stu-id="43317-193">It is important to make sure that the IP address space of the new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="43317-194">I det här exemplet tillhör de virtuella nätverken samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="43317-194">In this example, the virtual networks belong to the same subscription.</span></span> <span data-ttu-id="43317-195">Du kan konfigurera VNet-till-VNet-anslutningar mellan olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="43317-195">You can set up VNet-to-VNet connections between different subscriptions.</span></span> <span data-ttu-id="43317-196">Mer information finns i [konfigurera VNet-till-VNet-anslutningen](vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="43317-196">For more information, see [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md).</span></span> <span data-ttu-id="43317-197">Kontrollera att du lägger till den ”-Enablegpg $True” när du skapar anslutningar för att aktivera BGP.</span><span class="sxs-lookup"><span data-stu-id="43317-197">Make sure you add the "-EnableBgp $True" when creating the connections to enable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="43317-198">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="43317-198">1. Declare your variables</span></span>

<span data-ttu-id="43317-199">Ersätt värdena med de som du vill använda för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="43317-199">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

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

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a><span data-ttu-id="43317-200">2. Skapa TestVNet2 i den nya resursgruppen</span><span class="sxs-lookup"><span data-stu-id="43317-200">2. Create TestVNet2 in the new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a><span data-ttu-id="43317-201">3. Skapa VPN-gateway för TestVNet2 med BGP-parametrar</span><span class="sxs-lookup"><span data-stu-id="43317-201">3. Create the VPN gateway for TestVNet2 with BGP parameters</span></span>

<span data-ttu-id="43317-202">Begär offentlig IP-adress som ska allokeras till den gateway som du skapar för din VNet och definiera krävs undernätet och IP-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="43317-202">Request a public IP address to be allocated to the gateway you will create for your VNet and define the required subnet and IP configurations.</span></span>

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

<span data-ttu-id="43317-203">Skapa VPN-gateway med AS-nummer.</span><span class="sxs-lookup"><span data-stu-id="43317-203">Create the VPN gateway with the AS number.</span></span> <span data-ttu-id="43317-204">Du måste åsidosätta standardvärdet ASN på Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="43317-204">You must override the default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="43317-205">ASN: er för det anslutna Vnet måste vara olika för att aktivera BGP och transitroutning.</span><span class="sxs-lookup"><span data-stu-id="43317-205">The ASNs for the connected VNets must be different to enable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="43317-206">Steg 2 – ansluta TestVNet1 och TestVNet2 gateway</span><span class="sxs-lookup"><span data-stu-id="43317-206">Step 2 - Connect the TestVNet1 and TestVNet2 gateways</span></span>

<span data-ttu-id="43317-207">I det här exemplet är både gateways i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="43317-207">In this example, both gateways are in the same subscription.</span></span> <span data-ttu-id="43317-208">Du kan slutföra det här steget i samma PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="43317-208">You can complete this step in the same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="43317-209">1. Hämta båda gateways</span><span class="sxs-lookup"><span data-stu-id="43317-209">1. Get both gateways</span></span>

<span data-ttu-id="43317-210">Kontrollera att du har loggat in och anslutit till Prenumeration 1.</span><span class="sxs-lookup"><span data-stu-id="43317-210">Make sure you log in and connect to Subscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="43317-211">2. Skapa både anslutningar</span><span class="sxs-lookup"><span data-stu-id="43317-211">2. Create both connections</span></span>

<span data-ttu-id="43317-212">I det här steget skapar du anslutningen från TestVNet1 till TestVNet2 och anslutningen från TestVNet2 till TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="43317-212">In this step, you create the connection from TestVNet1 to TestVNet2, and the connection from TestVNet2 to TestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="43317-213">Glöm inte att aktivera BGP för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="43317-213">Be sure to enable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="43317-214">När du har slutfört de här stegen, upprättas anslutningen efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="43317-214">After completing these steps, the connection is established after a few minutes.</span></span> <span data-ttu-id="43317-215">BGP-peeringsessionen är upp när VNet-till-VNet-anslutningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="43317-215">The BGP peering session is up once the VNet-to-VNet connection is completed.</span></span>

<span data-ttu-id="43317-216">Om du har slutfört alla tre delar av den här övningen har du skapat följande nätverkets topologi:</span><span class="sxs-lookup"><span data-stu-id="43317-216">If you completed all three parts of this exercise, you have established the following network topology:</span></span>

![BGP för VNet-till-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a><span data-ttu-id="43317-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43317-218">Next steps</span></span>

<span data-ttu-id="43317-219">När anslutningen är klar kan du lägga till virtuella datorer till dina virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="43317-219">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="43317-220">Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="43317-220">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>