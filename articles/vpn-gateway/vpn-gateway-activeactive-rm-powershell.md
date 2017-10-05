---
title: "Konfigurera aktiv-aktiv S2S VPN-anslutningar för VPN-gatewayer: Azure Resource Manager: PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar aktiv-aktiv anslutningar med Azure VPN-gatewayer med Azure Resource Manager och PowerShell."
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
ms.date: 08/16/2017
ms.author: yushwang
ms.openlocfilehash: a9f71b566ffdb163f95634835f64589a700d712f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a><span data-ttu-id="6c9a4-103">Konfigurera aktiv-aktiv S2S VPN-anslutningar med Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="6c9a4-103">Configure active-active S2S VPN connections with Azure VPN Gateways</span></span>

<span data-ttu-id="6c9a4-104">Den här artikeln vägleder dig genom stegen för att skapa aktiv-aktiv mellan platser och VNet-till-VNet-anslutningar med Resource Manager-distributionsmodellen och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-104">This article walks you through the steps to create active-active cross-premises and VNet-to-VNet connections using the Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-highly-available-cross-premises-connections"></a><span data-ttu-id="6c9a4-105">Om hög tillgänglighet anslutningar mellan platser</span><span class="sxs-lookup"><span data-stu-id="6c9a4-105">About Highly Available Cross-Premises Connections</span></span>
<span data-ttu-id="6c9a4-106">För att uppnå hög tillgänglighet för anslutningar mellan platser och VNet-till-VNet-anslutningar bör du distribuera flera VPN-gatewayer och upprätta flera parallella anslutningar mellan ditt nätverk och Azure.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-106">To achieve high availability for cross-premises and VNet-to-VNet connectivity, you should deploy multiple VPN gateways and establish multiple parallel connections between your networks and Azure.</span></span> <span data-ttu-id="6c9a4-107">Se [hög tillgänglighet mellan platser och VNet-till-VNet-anslutningar](vpn-gateway-highlyavailable.md) en översikt över alternativ för nätverksanslutning och topologi.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-107">Please see [Highly Available Cross-Premises and VNet-to-VNet Connectivity](vpn-gateway-highlyavailable.md) for an overview of connectivity options and topology.</span></span>

<span data-ttu-id="6c9a4-108">Den här artikeln innehåller instruktioner för att ställa in en aktiv-aktiv mellan lokala VPN-anslutning och aktiv-aktiv anslutning mellan två virtuella nätverk:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-108">This article provides the instructions to set up an active-active cross-premises VPN connection, and active-active connection between two virtual networks:</span></span>

* [<span data-ttu-id="6c9a4-109">Del 1 - Skapa och konfigurera Azure VPN-gateway i aktivt-aktivt läge</span><span class="sxs-lookup"><span data-stu-id="6c9a4-109">Part 1 - Create and configure your Azure VPN gateway in active-active mode</span></span>](#aagateway)
* [<span data-ttu-id="6c9a4-110">Del 2 – upprätta aktiv-aktiv anslutningar mellan platser</span><span class="sxs-lookup"><span data-stu-id="6c9a4-110">Part 2 - Establish active-active cross-premises connections</span></span>](#aacrossprem)
* [<span data-ttu-id="6c9a4-111">Del 3 – skapa aktiv-aktiv VNet-till-VNet-anslutningar</span><span class="sxs-lookup"><span data-stu-id="6c9a4-111">Part 3 - Establish active-active VNet-to-VNet connections</span></span>](#aav2v)
* [<span data-ttu-id="6c9a4-112">En del 4 – uppdatera en befintlig gateway mellan aktiv-aktiv och aktivt vänteläge</span><span class="sxs-lookup"><span data-stu-id="6c9a4-112">Part 4 - Update existing gateway between active-active and active-standby</span></span>](#aaupdate)

<span data-ttu-id="6c9a4-113">Du kan kombinera dem tillsammans för att skapa en mer komplexa, hög tillgänglighet nätverkstopologi som uppfyller dina behov.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-113">You can combine these together to build a more complex, highly available network topology that meets your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c9a4-114">Observera att aktivt-aktivt läge använder bara de följande SKU: er:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-114">Please note that the active-active mode uses only the following SKUs:</span></span> 
  * <span data-ttu-id="6c9a4-115">VpnGw1 VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="6c9a4-115">VpnGw1, VpnGw2, VpnGw3</span></span>
  * <span data-ttu-id="6c9a4-116">HighPerformance (för gamla äldre SKU: er)</span><span class="sxs-lookup"><span data-stu-id="6c9a4-116">HighPerformance (for old legacy SKUs)</span></span>
> 
> 

## <span data-ttu-id="6c9a4-117"><a name ="aagateway"></a>Del 1 - Skapa och konfigurera VPN-gatewayer för aktiv-aktiv</span><span class="sxs-lookup"><span data-stu-id="6c9a4-117"><a name ="aagateway"></a>Part 1 - Create and configure active-active VPN gateways</span></span>
<span data-ttu-id="6c9a4-118">Följande steg konfigurerar Azure VPN-gateway i lägena för aktiv-aktiv.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-118">The following steps will configure your Azure VPN gateway in active-active modes.</span></span> <span data-ttu-id="6c9a4-119">De viktigaste skillnaderna mellan aktiv-aktiv och aktivt vänteläge gateway:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-119">The key differences between the active-active and active-standby gateways:</span></span>

* <span data-ttu-id="6c9a4-120">Du måste skapa två Gateway IP-konfigurationer med två offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="6c9a4-120">You need to create two Gateway IP configurations with two public IP addresses</span></span>
* <span data-ttu-id="6c9a4-121">Du måste ange flaggan EnableActiveActiveFeature</span><span class="sxs-lookup"><span data-stu-id="6c9a4-121">You need set the EnableActiveActiveFeature flag</span></span>
* <span data-ttu-id="6c9a4-122">Gateway-SKU: N måste vara VpnGw1 VpnGw2, VpnGw3 eller HighPerformance (äldre SKU).</span><span class="sxs-lookup"><span data-stu-id="6c9a4-122">The gateway SKU must be VpnGw1, VpnGw2, VpnGw3, or HighPerformance (legacy SKU).</span></span>

<span data-ttu-id="6c9a4-123">De andra egenskaperna är samma som aktiv aktiv gateway.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-123">The other properties are the same as the non-active-active gateways.</span></span> 

### <a name="before-you-begin"></a><span data-ttu-id="6c9a4-124">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6c9a4-124">Before you begin</span></span>
* <span data-ttu-id="6c9a4-125">Kontrollera att du har en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-125">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="6c9a4-126">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6c9a4-126">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="6c9a4-127">Du måste installera Azure Resource Managers PowerShell-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-127">You'll need to install the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="6c9a4-128">Se [översikt av Azure PowerShell](/powershell/azure/overview) för mer information om installation av PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-128">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="6c9a4-129">Steg 1 – Skapa och konfigurera VNet1</span><span class="sxs-lookup"><span data-stu-id="6c9a4-129">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="6c9a4-130">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="6c9a4-130">1. Declare your variables</span></span>
<span data-ttu-id="6c9a4-131">I den här övningen börjar vi med att deklarera våra variabler.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-131">For this exercise, we'll start by declaring our variables.</span></span> <span data-ttu-id="6c9a4-132">I exemplet nedan deklarerar vi variablerna med de värden som gäller för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-132">The example below declares the variables using the values for this exercise.</span></span> <span data-ttu-id="6c9a4-133">Se till att ersätta värdena med dina egna när du konfigurerar för produktion.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-133">Be sure to replace the values with your own when configuring for production.</span></span> <span data-ttu-id="6c9a4-134">Du kan använda dessa variabler om du använder anvisningarna för att bekanta dig med den här typen av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-134">You can use these variables if you are running through the steps to become familiar with this type of configuration.</span></span> <span data-ttu-id="6c9a4-135">Ändra variablerna samt kopiera och klistra in dem i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-135">Modify the variables, and then copy and paste into your PowerShell console.</span></span>

```powershell
$Sub1 = "Ross"
$RG1 = "TestAARG1"
$Location1 = "West US"
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
$GW1IPName1 = "VNet1GWIP1"
$GW1IPName2 = "VNet1GWIP2"
$GW1IPconf1 = "gw1ipconf1"
$GW1IPconf2 = "gw1ipconf2"
$Connection12 = "VNet1toVNet2"
$Connection151 = "VNet1toSite5_1"
$Connection152 = "VNet1toSite5_2"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="6c9a4-136">2. Anslut till din prenumeration och skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="6c9a4-136">2. Connect to your subscription and create a new resource group</span></span>
<span data-ttu-id="6c9a4-137">Se till att växla till PowerShell-läget för att kunna använda Resource Manager-cmdletarna.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-137">Make sure you switch to PowerShell mode to use the Resource Manager cmdlets.</span></span> <span data-ttu-id="6c9a4-138">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6c9a4-138">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="6c9a4-139">Öppna PowerShell-konsolen och anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-139">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="6c9a4-140">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-140">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="6c9a4-141">3. Skapa TestVNet1</span><span class="sxs-lookup"><span data-stu-id="6c9a4-141">3. Create TestVNet1</span></span>
<span data-ttu-id="6c9a4-142">Exemplet nedan skapar ett virtuellt nätverk med namnet TestVNet1 och tre undernät, där ett kallas GatewaySubnet, ett kallas FrontEnd och ett kallas BackEnd.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-142">The sample below creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="6c9a4-143">När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-143">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="6c9a4-144">Om du ger det något annat namn går det inte att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-144">If you name it something else, your gateway creation will fail.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a><span data-ttu-id="6c9a4-145">Steg 2 – skapa VPN-gateway för TestVNet1 med aktiv-aktiv läge</span><span class="sxs-lookup"><span data-stu-id="6c9a4-145">Step 2 - Create the VPN gateway for TestVNet1 with active-active mode</span></span>
#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a><span data-ttu-id="6c9a4-146">1. Skapa offentliga IP-adresser och gateway IP-konfigurationer</span><span class="sxs-lookup"><span data-stu-id="6c9a4-146">1. Create the public IP addresses and gateway IP configurations</span></span>
<span data-ttu-id="6c9a4-147">Begäran om två offentliga IP-adresser som ska allokeras till den gateway som du skapar för din VNet.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-147">Request two public IP addresses to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="6c9a4-148">Du måste också definiera undernätet och IP-konfigurationer som krävs.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-148">You'll also define the subnet and IP configurations required.</span></span>

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a><span data-ttu-id="6c9a4-149">2. Skapa VPN-gateway med aktiv-aktiv konfiguration</span><span class="sxs-lookup"><span data-stu-id="6c9a4-149">2. Create the VPN gateway with active-active configuration</span></span>
<span data-ttu-id="6c9a4-150">Skapa den virtuella nätverksgatewayen för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-150">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="6c9a4-151">Observera att det finns två GatewayIpConfig poster och flaggan EnableActiveActiveFeature har angetts.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-151">Note that there are two GatewayIpConfig entries, and the EnableActiveActiveFeature flag is set.</span></span> <span data-ttu-id="6c9a4-152">Det kan ta en stund att skapa en gateway (45 minuter eller mer).</span><span class="sxs-lookup"><span data-stu-id="6c9a4-152">Creating a gateway can take a while (45 minutes or more to complete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a><span data-ttu-id="6c9a4-153">3. Hämta gateway offentliga IP-adresser och BGP-Peer-IP-adress</span><span class="sxs-lookup"><span data-stu-id="6c9a4-153">3. Obtain the gateway public IP addresses and the BGP Peer IP address</span></span>
<span data-ttu-id="6c9a4-154">När du har skapat, behöver du skaffa BGP-Peer-IP-adressen på Azure VPN-Gateway.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-154">Once the gateway is created, you will need to obtain the BGP Peer IP address on the Azure VPN Gateway.</span></span> <span data-ttu-id="6c9a4-155">Den här adressen behövs för att konfigurera Azure VPN-Gateway som en BGP-Peer för dina lokala VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-155">This address is needed to configure the Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

<span data-ttu-id="6c9a4-156">Du kan använda följande cmdletar för att visa de två offentliga IP-adresser som har allokerats för din VPN-gateway och deras motsvarande BGP-Peer-IP-adresser för varje gateway-instans:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-156">Use the following cmdlets to show the two public IP addresses allocated for your VPN gateway, and their corresponding BGP Peer IP addresses for each gateway instance:</span></span>

```powershell

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }
```

<span data-ttu-id="6c9a4-157">Ordningen för den offentliga IP-adresserna för gateway-instanser och motsvarande BGP-Peering adresserna är samma.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-157">The order of the public IP addresses for the gateway instances and the corresponding BGP Peering Addresses are the same.</span></span> <span data-ttu-id="6c9a4-158">I det här exemplet använder gateway VM med en offentlig IP för 40.112.190.5 10.12.255.4 som sin BGP-Peering adress och gateway med 138.91.156.129 använder 10.12.255.5.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-158">In this example, the gateway VM with public IP of 40.112.190.5 will use 10.12.255.4 as its BGP Peering Address, and the gateway with 138.91.156.129 will use 10.12.255.5.</span></span> <span data-ttu-id="6c9a4-159">Den här informationen behövs när du konfigurerar din på lokala VPN-enheter som ansluter till aktiv-aktiv-gateway.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-159">This information is needed when you set up your on premises VPN devices connecting to the active-active gateway.</span></span> <span data-ttu-id="6c9a4-160">Gatewayen visas i diagrammet nedan med alla adresser:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-160">The gateway is shown in the diagram below with all addresses:</span></span>

![aktiv-aktiv gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

<span data-ttu-id="6c9a4-162">När gatewayen är skapad, kan du använda den här gatewayen för att etablera aktiv-aktiv mellan lokala eller VNet-till-VNet-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-162">Once the gateway is created, you can use this gateway to establish active-active cross-premises or VNet-to-VNet connection.</span></span> <span data-ttu-id="6c9a4-163">Följande avsnitt beskriver stegen för att slutföra den här övningen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-163">The following sections will walk through the steps to complete the exercise.</span></span>

## <span data-ttu-id="6c9a4-164"><a name ="aacrossprem"></a>Del 2 – upprätta en anslutning för aktiv-aktiv mellan platser</span><span class="sxs-lookup"><span data-stu-id="6c9a4-164"><a name ="aacrossprem"></a>Part 2 - Establish an active-active cross-premises connection</span></span>
<span data-ttu-id="6c9a4-165">För att upprätta en anslutning mellan platser, måste du skapa en lokal nätverksgateway som representerar din lokala VPN-enhet och en anslutning för att ansluta Azure VPN-gateway med den lokala nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-165">To establish a cross-premises connection, you need to create a Local Network Gateway to represent your on-premises VPN device, and a Connection to connect the Azure VPN gateway with the local network gateway.</span></span> <span data-ttu-id="6c9a4-166">I det här exemplet är Azure VPN-gateway i aktivt-aktivt läge.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-166">In this example, the Azure VPN gateway is in active-active mode.</span></span> <span data-ttu-id="6c9a4-167">Även om det finns endast en lokal VPN-enheter (lokal nätverksgateway) och en anslutning resurser, kommer båda Azure VPN gateway-instanser därför upprätta S2S VPN-tunnlar med den lokala enheten.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-167">As a result, even though there is only one on-premises VPN device (local network gateway) and one connection resource, both Azure VPN gateway instances will establish S2S VPN tunnels with the on-premises device.</span></span>

<span data-ttu-id="6c9a4-168">Innan du fortsätter, kontrollera att du har slutfört [del 1](#aagateway) för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-168">Before proceeding, please make sure you have completed [Part 1](#aagateway) of this exercise.</span></span>

### <a name="step-1---create-and-configure-the-local-network-gateway"></a><span data-ttu-id="6c9a4-169">Steg 1 – Skapa och konfigurera den lokala nätverksgatewayen</span><span class="sxs-lookup"><span data-stu-id="6c9a4-169">Step 1 - Create and configure the local network gateway</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="6c9a4-170">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="6c9a4-170">1. Declare your variables</span></span>
<span data-ttu-id="6c9a4-171">Den här övningen kommer att fortsätta att skapa den konfiguration som visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-171">This exercise will continue to build the configuration shown in the diagram.</span></span> <span data-ttu-id="6c9a4-172">Ersätt värdena med de som du vill använda för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-172">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

<span data-ttu-id="6c9a4-173">Några saker att Observera för de lokala nätverket gateway-parametrarna:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-173">A couple of things to note regarding the local network gateway parameters:</span></span>

* <span data-ttu-id="6c9a4-174">Den lokala nätverksgatewayen kan vara i samma eller en annan plats och resursgruppen som VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-174">The local network gateway can be in the same or different location and resource group as the VPN gateway.</span></span> <span data-ttu-id="6c9a4-175">Det här exemplet visas de i olika resursgrupper men i samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-175">This example shows them in different resource groups but in the same Azure location.</span></span>
* <span data-ttu-id="6c9a4-176">Om det finns bara en lokal VPN-enhet som du ser ovan, kan aktiv-aktiv anslutning arbeta med eller utan BGP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-176">If there is only one on-premises VPN device as shown above, the active-active connection can work with or without BGP protocol.</span></span> <span data-ttu-id="6c9a4-177">Det här exemplet använder BGP för anslutningen mellan platser.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-177">This example uses BGP for the cross-premises connection.</span></span>
* <span data-ttu-id="6c9a4-178">Om BGP är aktiverat, är det prefix som du måste deklarera för den lokala nätverksgatewayen värdadressen av BGP-Peer-IP-adress på VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-178">If BGP is enabled, the prefix you need to declare for the local network gateway is the host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="6c9a4-179">I det här fallet är det en /32 prefixet ”10.52.255.253/32”.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-179">In this case, it's a /32 prefix of "10.52.255.253/32".</span></span>
* <span data-ttu-id="6c9a4-180">Du måste använda olika BGP, ASN: er, mellan ditt lokala nätverk och Azure VNet som en påminnelse.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-180">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="6c9a4-181">Om de är samma som du behöver ändra ditt VNet ASN om din lokala VPN-enhet använder redan ASN till peer-datorn med andra BGP-grannar.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-181">If they are the same, you need to change your VNet ASN if your on-premises VPN device already uses the ASN to peer with other BGP neighbors.</span></span>

#### <a name="2-create-the-local-network-gateway-for-site5"></a><span data-ttu-id="6c9a4-182">2. Skapa lokal nätverksgateway för Site5</span><span class="sxs-lookup"><span data-stu-id="6c9a4-182">2. Create the local network gateway for Site5</span></span>
<span data-ttu-id="6c9a4-183">Innan du fortsätter kontrollerar du att du fortfarande är ansluten till Prenumeration 1.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-183">Before you continue, please make sure you are still connected to Subscription 1.</span></span> <span data-ttu-id="6c9a4-184">Skapa resursgruppen om det inte har skapats ännu.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-184">Create the resource group if it is not yet created.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="6c9a4-185">Steg 2 – ansluta VNet gateway och lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="6c9a4-185">Step 2 - Connect the VNet gateway and local network gateway</span></span>
#### <a name="1-get-the-two-gateways"></a><span data-ttu-id="6c9a4-186">1. Hämta två gateways</span><span class="sxs-lookup"><span data-stu-id="6c9a4-186">1. Get the two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a><span data-ttu-id="6c9a4-187">2. Skapa TestVNet1 till Site5 anslutning</span><span class="sxs-lookup"><span data-stu-id="6c9a4-187">2. Create the TestVNet1 to Site5 connection</span></span>
<span data-ttu-id="6c9a4-188">I det här steget skapar du anslutningen från TestVNet1 till Site5_1 med värdet $True ”Enablegpg”.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-188">In this step, you will create the connection from TestVNet1 to Site5_1 with "EnableBGP" set to $True.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a><span data-ttu-id="6c9a4-189">3. VPN och BGP parametrar för din lokala VPN-enhet</span><span class="sxs-lookup"><span data-stu-id="6c9a4-189">3. VPN and BGP parameters for your on-premises VPN device</span></span>
<span data-ttu-id="6c9a4-190">Exemplet nedan visas de parametrar som du ska ange i konfigurationsavsnittet BGP på din lokala VPN-enhet för den här övningen:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-190">The example below lists the parameters you will enter into the BGP configuration section on your on-premises VPN device for this exercise:</span></span>

    - <span data-ttu-id="6c9a4-191">Site5 ASN: 65050</span><span class="sxs-lookup"><span data-stu-id="6c9a4-191">Site5 ASN            : 65050</span></span>
    - <span data-ttu-id="6c9a4-192">Site5 BGP IP: 10.52.255.253</span><span class="sxs-lookup"><span data-stu-id="6c9a4-192">Site5 BGP IP         : 10.52.255.253</span></span>
    - <span data-ttu-id="6c9a4-193">Prefix för att meddela: (till exempel) 10.51.0.0/16 och 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6c9a4-193">Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16</span></span>
    - <span data-ttu-id="6c9a4-194">Azure VNet ASN: 65010</span><span class="sxs-lookup"><span data-stu-id="6c9a4-194">Azure VNet ASN       : 65010</span></span>
    - <span data-ttu-id="6c9a4-195">Azure VNet BGP IP 1: 10.12.255.4 för tunnel till 40.112.190.5</span><span class="sxs-lookup"><span data-stu-id="6c9a4-195">Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5</span></span>
    - <span data-ttu-id="6c9a4-196">Azure VNet BGP IP 2: 10.12.255.5 för tunnel till 138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="6c9a4-196">Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129</span></span>
    - <span data-ttu-id="6c9a4-197">Statiska vägar: mål 10.12.255.4/32, nexthop VPN-tunnel gränssnitt till 40.112.190.5 mål 10.12.255.5/32, nexthop VPN-tunnel gränssnitt till 138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="6c9a4-197">Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5                        Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129</span></span>
    - <span data-ttu-id="6c9a4-198">eBGP Multihopp: se till att alternativet ”Multihopp” för eBGP är aktiverat på enheten om det behövs</span><span class="sxs-lookup"><span data-stu-id="6c9a4-198">eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed</span></span>

<span data-ttu-id="6c9a4-199">Anslutningen ska upprättas efter några minuter och BGP-peeringsessionen startar när IPsec-anslutning har upprättats.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-199">The connection should be established after a few minutes, and the BGP peering session will start once the IPsec connection is established.</span></span> <span data-ttu-id="6c9a4-200">Hittills har det här exemplet konfigureras bara en lokal VPN-enhet, vilket resulterar i diagrammet nedan:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-200">This example so far has configured only one on-premises VPN device, resulting in the diagram shown below:</span></span>

![aktiv-aktiv-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a><span data-ttu-id="6c9a4-202">Steg 3 – ansluta två lokala VPN-enheter till aktiv-aktiv VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="6c9a4-202">Step 3 - Connect two on-premises VPN devices to the active-active VPN gateway</span></span>
<span data-ttu-id="6c9a4-203">Om du har två VPN-enheter på samma lokala nätverk kan du uppnå dubbla redundans genom att ansluta Azure VPN-gatewayen till andra VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-203">If you have two VPN devices at the same on-premises network, you can achieve dual redundancy by connecting the Azure VPN gateway to the second VPN device.</span></span>

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a><span data-ttu-id="6c9a4-204">1. Skapa den andra lokala nätverksgatewayen för Site5</span><span class="sxs-lookup"><span data-stu-id="6c9a4-204">1. Create the second local network gateway for Site5</span></span>
<span data-ttu-id="6c9a4-205">Observera att gateway IP-adress, adressprefix och adress för BGP-peering för andra lokala nätverksgateway inte får överlappa med föregående lokal nätverksgateway för samma lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-205">Note that the gateway IP address, address prefix, and BGP peering address for the second local network gateway must not overlap with the previous local network gateway for the same on-premises network.</span></span>

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a><span data-ttu-id="6c9a4-206">2. Ansluta en gateway för virtuellt nätverk och andra lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="6c9a4-206">2. Connect the VNet gateway and the second local network gateway</span></span>
<span data-ttu-id="6c9a4-207">Skapa anslutningen från TestVNet1 till Site5_2 med värdet $True ”EnableBGP”</span><span class="sxs-lookup"><span data-stu-id="6c9a4-207">Create the connection from TestVNet1 to Site5_2 with "EnableBGP" set to $True</span></span>

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a><span data-ttu-id="6c9a4-208">3. Parametrarna för VPN och BGP för andra lokala VPN-enheten</span><span class="sxs-lookup"><span data-stu-id="6c9a4-208">3. VPN and BGP parameters for your second on-premises VPN device</span></span>
<span data-ttu-id="6c9a4-209">På liknande sätt nedan visar parametrarna skriver du in i andra VPN-enhet:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-209">Similarly, below lists the parameters you will enter into the second VPN device:</span></span>

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="6c9a4-210">När anslutningen (tunnlar) är klar har du dubbla redundant VPN-enheter och ansluter dina lokala nätverk och Azure-tunnlar:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-210">Once the connection (tunnels) are established, you will have dual redundant VPN devices and tunnels connecting your on-premises network and Azure:</span></span>

![dubbla-redundans-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <span data-ttu-id="6c9a4-212"><a name ="aav2v"></a>Del 3: upprätta en anslutning för aktiv-aktiv VNet-till-VNet</span><span class="sxs-lookup"><span data-stu-id="6c9a4-212"><a name ="aav2v"></a>Part 3 - Establish an active-active VNet-to-VNet connection</span></span>
<span data-ttu-id="6c9a4-213">Det här avsnittet skapar du en aktiv-aktiv VNet-till-VNet-anslutning med BGP.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-213">This section creates an active-active VNet-to-VNet connection with BGP.</span></span> 

<span data-ttu-id="6c9a4-214">Anvisningarna nedan fortsätter från föregående steg ovan.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-214">The instructions below continue from the previous steps listed above.</span></span> <span data-ttu-id="6c9a4-215">Du måste slutföra [del 1](#aagateway) att skapa och konfigurera TestVNet1 och VPN-Gateway med BGP.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-215">You must complete [Part 1](#aagateway) to create and configure TestVNet1 and the VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a><span data-ttu-id="6c9a4-216">Steg 1 – Skapa TestVNet2 och VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="6c9a4-216">Step 1 - Create TestVNet2 and the VPN gateway</span></span>
<span data-ttu-id="6c9a4-217">Det är viktigt att se till att IP-adressutrymmet för det nya virtuella nätverket TestVNet2, inte överlappar med någon av VNet-intervall.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-217">It is important to make sure that the IP address space of the new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="6c9a4-218">I det här exemplet tillhör de virtuella nätverken samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-218">In this example, the virtual networks belong to the same subscription.</span></span> <span data-ttu-id="6c9a4-219">Du kan konfigurera VNet-till-VNet-anslutningar mellan olika prenumerationer; Se [konfigurera VNet-till-VNet-anslutningen](vpn-gateway-vnet-vnet-rm-ps.md) att lära dig mer information.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-219">You can set up VNet-to-VNet connections between different subscriptions; please refer to [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) to learn more details.</span></span> <span data-ttu-id="6c9a4-220">Kontrollera att du lägger till den ”-Enablegpg $True” när du skapar anslutningar för att aktivera BGP.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-220">Make sure you add the "-EnableBgp $True" when creating the connections to enable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="6c9a4-221">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="6c9a4-221">1. Declare your variables</span></span>
<span data-ttu-id="6c9a4-222">Ersätt värdena med de som du vill använda för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-222">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG2 = "TestAARG2"
$Location2 = "East US"
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
$GW2IPName1 = "VNet2GWIP1"
$GW2IPconf1 = "gw2ipconf1"
$GW2IPName2 = "VNet2GWIP2"
$GW2IPconf2 = "gw2ipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a><span data-ttu-id="6c9a4-223">2. Skapa TestVNet2 i den nya resursgruppen</span><span class="sxs-lookup"><span data-stu-id="6c9a4-223">2. Create TestVNet2 in the new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a><span data-ttu-id="6c9a4-224">3. Skapa VPN-gateway för aktiv-aktiv för TestVNet2</span><span class="sxs-lookup"><span data-stu-id="6c9a4-224">3. Create the active-active VPN gateway for TestVNet2</span></span>
<span data-ttu-id="6c9a4-225">Begäran om två offentliga IP-adresser som ska allokeras till den gateway som du skapar för din VNet.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-225">Request two public IP addresses to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="6c9a4-226">Du måste också definiera undernätet och IP-konfigurationer som krävs.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-226">You'll also define the subnet and IP configurations required.</span></span>

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

<span data-ttu-id="6c9a4-227">Skapa VPN-gateway med många AS och flaggan ”EnableActiveActiveFeature”.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-227">Create the VPN gateway with the AS number and the "EnableActiveActiveFeature" flag.</span></span> <span data-ttu-id="6c9a4-228">Observera att du måste åsidosätta standardvärdet ASN på Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-228">Note that you must override the default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="6c9a4-229">ASN: er för det anslutna Vnet måste vara olika för att aktivera BGP och transitroutning.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-229">The ASNs for the connected VNets must be different to enable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="6c9a4-230">Steg 2 – ansluta TestVNet1 och TestVNet2 gateway</span><span class="sxs-lookup"><span data-stu-id="6c9a4-230">Step 2 - Connect the TestVNet1 and TestVNet2 gateways</span></span>
<span data-ttu-id="6c9a4-231">I det här exemplet är både gateways i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-231">In this example, both gateways are in the same subscription.</span></span> <span data-ttu-id="6c9a4-232">Du kan slutföra det här steget i samma PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-232">You can complete this step in the same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="6c9a4-233">1. Hämta båda gateways</span><span class="sxs-lookup"><span data-stu-id="6c9a4-233">1. Get both gateways</span></span>
<span data-ttu-id="6c9a4-234">Kontrollera att du har loggat in och anslutit till Prenumeration 1.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-234">Make sure you log in and connect to Subscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="6c9a4-235">2. Skapa både anslutningar</span><span class="sxs-lookup"><span data-stu-id="6c9a4-235">2. Create both connections</span></span>
<span data-ttu-id="6c9a4-236">I det här steget skapar du anslutningen från TestVNet1 till TestVNet2 och anslutningen från TestVNet2 till TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-236">In this step, you will create the connection from TestVNet1 to TestVNet2, and the connection from TestVNet2 to TestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="6c9a4-237">Glöm inte att aktivera BGP för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-237">Be sure to enable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="6c9a4-238">När du har slutfört de här stegen anslutningen upprättas i några minuter och BGP-är peering-session när VNet-till-VNet-anslutningen har slutförts med dubbla redundans:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-238">After completing these steps, the connection will be establish in a few minutes, and the BGP peering session will be up once the VNet-to-VNet connection is completed with dual redundancy:</span></span>

![aktiv-aktiv-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <span data-ttu-id="6c9a4-240"><a name ="aaupdate"></a>En del 4 – uppdatera en befintlig gateway mellan aktiv-aktiv och aktivt vänteläge</span><span class="sxs-lookup"><span data-stu-id="6c9a4-240"><a name ="aaupdate"></a>Part 4 - Update existing gateway between active-active and active-standby</span></span>
<span data-ttu-id="6c9a4-241">Det sista avsnittet beskriver hur du kan konfigurera en befintlig Azure VPN-gateway från aktivt vänteläge till aktiv-aktiv läge, och vice versa.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-241">The last section will describe how you can configure an existing Azure VPN gateway from active-standby to active-active mode, or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="6c9a4-242">Det här avsnittet innehåller steg för att ändra storlek på en äldre SKU (gamla SKU) för en redan har skapat VPN-gateway från Standard på HighPerformance.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-242">This section includes the steps to resize a legacy SKU (old SKU) of an already created VPN gateway from Standard to HighPerformance.</span></span> <span data-ttu-id="6c9a4-243">De här stegen uppgradera inte en gamla äldre SKU till en ny SKU: er.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-243">These steps do not upgrade an old legacy SKU to one of the new SKUs.</span></span>
> 
> 

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a><span data-ttu-id="6c9a4-244">Konfigurera ett aktivt vänteläge gateway till aktiv-aktiv gateway</span><span class="sxs-lookup"><span data-stu-id="6c9a4-244">Configure an active-standby gateway to active-active gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="6c9a4-245">1. Gateway-parametrar</span><span class="sxs-lookup"><span data-stu-id="6c9a4-245">1. Gateway parameters</span></span>
<span data-ttu-id="6c9a4-246">I följande exempel konverterar ett aktivt vänteläge gateway till en aktiv-aktiv gateway.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-246">The following example converts an active-standby gateway into an active-active gateway.</span></span> <span data-ttu-id="6c9a4-247">Du måste skapa en annan offentlig IP-adress och sedan lägga till en andra Gateway IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-247">You need to create another public IP address, then add a second Gateway IP configuration.</span></span> <span data-ttu-id="6c9a4-248">Nedan visas de parametrar som används:</span><span class="sxs-lookup"><span data-stu-id="6c9a4-248">Below shows the parameters used:</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"

$vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a><span data-ttu-id="6c9a4-249">2. Skapa den offentliga IP-adressen och lägger till andra gateway IP-konfiguration</span><span class="sxs-lookup"><span data-stu-id="6c9a4-249">2. Create the public IP address, then add the second gateway IP configuration</span></span>

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a><span data-ttu-id="6c9a4-250">3. Aktivera läget för aktiv-aktiv och uppdatera gatewayen</span><span class="sxs-lookup"><span data-stu-id="6c9a4-250">3. Enable active-active mode and update the gateway</span></span>
<span data-ttu-id="6c9a4-251">Du måste ange gateway-objektet i PowerShell för att utlösa den faktiska uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-251">You must set the gateway object in PowerShell to trigger the actual update.</span></span> <span data-ttu-id="6c9a4-252">SKU för den virtuella nätverksgatewayen måste också ändras (storleksändrade) till HighPerformance eftersom den har skapats tidigare som Standard.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-252">The SKU of the virtual network gateway must also be changed (resized) to HighPerformance since it was created previously as Standard.</span></span>

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

<span data-ttu-id="6c9a4-253">Den här uppdateringen kan ta 30 till 45 minuter.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-253">This update can take 30 to 45 minutes.</span></span>

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a><span data-ttu-id="6c9a4-254">Konfigurera en aktiv-aktiv gateway till aktivt vänteläge gateway</span><span class="sxs-lookup"><span data-stu-id="6c9a4-254">Configure an active-active gateway to active-standby gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="6c9a4-255">1. Gateway-parametrar</span><span class="sxs-lookup"><span data-stu-id="6c9a4-255">1. Gateway parameters</span></span>
<span data-ttu-id="6c9a4-256">Använd samma parametrar som ovan, hämta namnet på IP-konfiguration som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-256">Use the same parameters as above, get the name of the IP configuration you want to remove.</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a><span data-ttu-id="6c9a4-257">2. Ta bort gateway-IP-konfigurationen och inaktivera aktiv-aktiv-läge</span><span class="sxs-lookup"><span data-stu-id="6c9a4-257">2. Remove the gateway IP configuration and disable the active-active mode</span></span>
<span data-ttu-id="6c9a4-258">Dessutom måste du ange gateway-objektet i PowerShell för att utlösa den faktiska uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-258">Similarly, you must set the gateway object in PowerShell to trigger the actual update.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

<span data-ttu-id="6c9a4-259">Den här uppdateringen kan ta upp till 30 till 45 minuter.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-259">This update can take up to 30 to  45 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c9a4-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c9a4-260">Next steps</span></span>
<span data-ttu-id="6c9a4-261">När anslutningen är klar kan du lägga till virtuella datorer till dina virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-261">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="6c9a4-262">Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="6c9a4-262">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>