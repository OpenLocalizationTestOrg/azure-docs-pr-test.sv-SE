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
ms.openlocfilehash: 964eedc7698e42bf0e082f0105845f2a339daf57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a><span data-ttu-id="875da-103">Konfigurera aktiv-aktiv S2S VPN-anslutningar med Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="875da-103">Configure active-active S2S VPN connections with Azure VPN Gateways</span></span>

<span data-ttu-id="875da-104">Den här artikeln vägleder dig genom hello steg toocreate aktiv-aktiv mellan platser och VNet-till-VNet-anslutningar med hello Resource Manager-distributionsmodellen och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="875da-104">This article walks you through hello steps toocreate active-active cross-premises and VNet-to-VNet connections using hello Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-highly-available-cross-premises-connections"></a><span data-ttu-id="875da-105">Om hög tillgänglighet anslutningar mellan platser</span><span class="sxs-lookup"><span data-stu-id="875da-105">About Highly Available Cross-Premises Connections</span></span>
<span data-ttu-id="875da-106">tooachieve hög tillgänglighet för anslutningar mellan platser och VNet-till-VNet-anslutningen bör du distribuera flera VPN-gatewayer och upprätta flera parallella anslutningar mellan ditt nätverk och Azure.</span><span class="sxs-lookup"><span data-stu-id="875da-106">tooachieve high availability for cross-premises and VNet-to-VNet connectivity, you should deploy multiple VPN gateways and establish multiple parallel connections between your networks and Azure.</span></span> <span data-ttu-id="875da-107">Se [hög tillgänglighet mellan platser och VNet-till-VNet-anslutningar](vpn-gateway-highlyavailable.md) en översikt över alternativ för nätverksanslutning och topologi.</span><span class="sxs-lookup"><span data-stu-id="875da-107">Please see [Highly Available Cross-Premises and VNet-to-VNet Connectivity](vpn-gateway-highlyavailable.md) for an overview of connectivity options and topology.</span></span>

<span data-ttu-id="875da-108">Den här artikeln innehåller instruktioner för hello tooset upp en aktiv-aktiv mellan lokala VPN-anslutning och aktiv-aktiv anslutning mellan två virtuella nätverk:</span><span class="sxs-lookup"><span data-stu-id="875da-108">This article provides hello instructions tooset up an active-active cross-premises VPN connection, and active-active connection between two virtual networks:</span></span>

* [<span data-ttu-id="875da-109">Del 1 - Skapa och konfigurera Azure VPN-gateway i aktivt-aktivt läge</span><span class="sxs-lookup"><span data-stu-id="875da-109">Part 1 - Create and configure your Azure VPN gateway in active-active mode</span></span>](#aagateway)
* [<span data-ttu-id="875da-110">Del 2 – upprätta aktiv-aktiv anslutningar mellan platser</span><span class="sxs-lookup"><span data-stu-id="875da-110">Part 2 - Establish active-active cross-premises connections</span></span>](#aacrossprem)
* [<span data-ttu-id="875da-111">Del 3 – skapa aktiv-aktiv VNet-till-VNet-anslutningar</span><span class="sxs-lookup"><span data-stu-id="875da-111">Part 3 - Establish active-active VNet-to-VNet connections</span></span>](#aav2v)
* [<span data-ttu-id="875da-112">En del 4 – uppdatera en befintlig gateway mellan aktiv-aktiv och aktivt vänteläge</span><span class="sxs-lookup"><span data-stu-id="875da-112">Part 4 - Update existing gateway between active-active and active-standby</span></span>](#aaupdate)

<span data-ttu-id="875da-113">Du kan kombinera dessa tillsammans toobuild en mer komplexa, hög tillgänglighet nätverkstopologi som uppfyller dina behov.</span><span class="sxs-lookup"><span data-stu-id="875da-113">You can combine these together toobuild a more complex, highly available network topology that meets your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="875da-114">Observera att hello aktiv-aktiv läge använder endast hello följande SKU: er:</span><span class="sxs-lookup"><span data-stu-id="875da-114">Please note that hello active-active mode uses only hello following SKUs:</span></span> 
  * <span data-ttu-id="875da-115">VpnGw1 VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="875da-115">VpnGw1, VpnGw2, VpnGw3</span></span>
  * <span data-ttu-id="875da-116">HighPerformance (för gamla äldre SKU: er)</span><span class="sxs-lookup"><span data-stu-id="875da-116">HighPerformance (for old legacy SKUs)</span></span>
> 
> 

## <span data-ttu-id="875da-117"><a name ="aagateway"></a>Del 1 - Skapa och konfigurera VPN-gatewayer för aktiv-aktiv</span><span class="sxs-lookup"><span data-stu-id="875da-117"><a name ="aagateway"></a>Part 1 - Create and configure active-active VPN gateways</span></span>
<span data-ttu-id="875da-118">hello följande konfigurerar Azure VPN-gateway i lägena för aktiv-aktiv.</span><span class="sxs-lookup"><span data-stu-id="875da-118">hello following steps will configure your Azure VPN gateway in active-active modes.</span></span> <span data-ttu-id="875da-119">hello viktiga skillnader mellan hello aktiv-aktiv och aktivt vänteläge gateway:</span><span class="sxs-lookup"><span data-stu-id="875da-119">hello key differences between hello active-active and active-standby gateways:</span></span>

* <span data-ttu-id="875da-120">Du behöver toocreate två Gateway IP-konfigurationer med två offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="875da-120">You need toocreate two Gateway IP configurations with two public IP addresses</span></span>
* <span data-ttu-id="875da-121">Hej EnableActiveActiveFeature flagga måste anges</span><span class="sxs-lookup"><span data-stu-id="875da-121">You need set hello EnableActiveActiveFeature flag</span></span>
* <span data-ttu-id="875da-122">hello gateway SKU: N måste vara VpnGw1 VpnGw2, VpnGw3 eller HighPerformance (äldre SKU).</span><span class="sxs-lookup"><span data-stu-id="875da-122">hello gateway SKU must be VpnGw1, VpnGw2, VpnGw3, or HighPerformance (legacy SKU).</span></span>

<span data-ttu-id="875da-123">hello är andra egenskaper hello samma som hello aktiva aktiva gateways.</span><span class="sxs-lookup"><span data-stu-id="875da-123">hello other properties are hello same as hello non-active-active gateways.</span></span> 

### <a name="before-you-begin"></a><span data-ttu-id="875da-124">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="875da-124">Before you begin</span></span>
* <span data-ttu-id="875da-125">Kontrollera att du har en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="875da-125">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="875da-126">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="875da-126">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="875da-127">Du behöver tooinstall hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="875da-127">You'll need tooinstall hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="875da-128">Se [översikt av Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="875da-128">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="875da-129">Steg 1 – Skapa och konfigurera VNet1</span><span class="sxs-lookup"><span data-stu-id="875da-129">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="875da-130">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="875da-130">1. Declare your variables</span></span>
<span data-ttu-id="875da-131">I den här övningen börjar vi med att deklarera våra variabler.</span><span class="sxs-lookup"><span data-stu-id="875da-131">For this exercise, we'll start by declaring our variables.</span></span> <span data-ttu-id="875da-132">hello exemplet nedan förklarar hello variabler med hello värden för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="875da-132">hello example below declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="875da-133">Vara säker på att tooreplace hello värden med dina egna när du konfigurerar för produktion.</span><span class="sxs-lookup"><span data-stu-id="875da-133">Be sure tooreplace hello values with your own when configuring for production.</span></span> <span data-ttu-id="875da-134">Du kan använda dessa variabler om du kör via hello steg toobecome bekant med den här typen av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="875da-134">You can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="875da-135">Ändra hello variabler, kopiera och klistra in i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="875da-135">Modify hello variables, and then copy and paste into your PowerShell console.</span></span>

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="875da-136">2. Anslut tooyour prenumeration och skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="875da-136">2. Connect tooyour subscription and create a new resource group</span></span>
<span data-ttu-id="875da-137">Kontrollera att du växla tooPowerShell läge toouse hello Resource Manager-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="875da-137">Make sure you switch tooPowerShell mode toouse hello Resource Manager cmdlets.</span></span> <span data-ttu-id="875da-138">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="875da-138">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="875da-139">Öppna PowerShell-konsolen och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="875da-139">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="875da-140">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="875da-140">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="875da-141">3. Skapa TestVNet1</span><span class="sxs-lookup"><span data-stu-id="875da-141">3. Create TestVNet1</span></span>
<span data-ttu-id="875da-142">hello exemplet nedan skapar ett virtuellt nätverk med namnet TestVNet1 och tre undernät, en kallas GatewaySubnet, en klientdel som anropats och ett kallas Backend.</span><span class="sxs-lookup"><span data-stu-id="875da-142">hello sample below creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="875da-143">När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="875da-143">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="875da-144">Om du ger det något annat namn går det inte att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="875da-144">If you name it something else, your gateway creation will fail.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-active-active-mode"></a><span data-ttu-id="875da-145">Steg 2 – Skapa hello VPN-gateway för TestVNet1 med aktiv-aktiv läge</span><span class="sxs-lookup"><span data-stu-id="875da-145">Step 2 - Create hello VPN gateway for TestVNet1 with active-active mode</span></span>
#### <a name="1-create-hello-public-ip-addresses-and-gateway-ip-configurations"></a><span data-ttu-id="875da-146">1. Skapa hello offentliga IP-adresser och IP-konfigurationer för gateway</span><span class="sxs-lookup"><span data-stu-id="875da-146">1. Create hello public IP addresses and gateway IP configurations</span></span>
<span data-ttu-id="875da-147">Begäran två offentliga IP-adresser toobe allokerade toohello gateway skapas för ditt VNet.</span><span class="sxs-lookup"><span data-stu-id="875da-147">Request two public IP addresses toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="875da-148">Du måste också definiera hello undernät och IP-konfigurationer som krävs.</span><span class="sxs-lookup"><span data-stu-id="875da-148">You'll also define hello subnet and IP configurations required.</span></span>

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-hello-vpn-gateway-with-active-active-configuration"></a><span data-ttu-id="875da-149">2. Skapa hello VPN-gateway med aktiv-aktiv konfiguration</span><span class="sxs-lookup"><span data-stu-id="875da-149">2. Create hello VPN gateway with active-active configuration</span></span>
<span data-ttu-id="875da-150">Skapa hello virtuell nätverksgateway för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="875da-150">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="875da-151">Observera att det finns två GatewayIpConfig poster och hello EnableActiveActiveFeature flaggan har angetts.</span><span class="sxs-lookup"><span data-stu-id="875da-151">Note that there are two GatewayIpConfig entries, and hello EnableActiveActiveFeature flag is set.</span></span> <span data-ttu-id="875da-152">Skapa en gateway kan ta ett tag (45 minuter eller mer toocomplete).</span><span class="sxs-lookup"><span data-stu-id="875da-152">Creating a gateway can take a while (45 minutes or more toocomplete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-hello-gateway-public-ip-addresses-and-hello-bgp-peer-ip-address"></a><span data-ttu-id="875da-153">3. Hämta hello gateway offentliga IP-adresser och hello BGP-Peer-IP-adress</span><span class="sxs-lookup"><span data-stu-id="875da-153">3. Obtain hello gateway public IP addresses and hello BGP Peer IP address</span></span>
<span data-ttu-id="875da-154">När du har skapat hello gateway måste tooobtain hello BGP-Peer-IP-adress på hello Azure VPN-Gateway.</span><span class="sxs-lookup"><span data-stu-id="875da-154">Once hello gateway is created, you will need tooobtain hello BGP Peer IP address on hello Azure VPN Gateway.</span></span> <span data-ttu-id="875da-155">Den här adressen är nödvändiga tooconfigure hello Azure VPN-Gateway som en BGP-Peer för dina lokala VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="875da-155">This address is needed tooconfigure hello Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

<span data-ttu-id="875da-156">Använd hello följande cmdlets tooshow hello två offentliga IP-adresser för VPN-gateway och deras motsvarande BGP-Peer-IP-adresser för varje gateway-instans:</span><span class="sxs-lookup"><span data-stu-id="875da-156">Use hello following cmdlets tooshow hello two public IP addresses allocated for your VPN gateway, and their corresponding BGP Peer IP addresses for each gateway instance:</span></span>

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

<span data-ttu-id="875da-157">hello ordning hello offentliga IP-adresser för hello gateway-instanser och hello motsvarande BGP-Peering-adresser är hello samma.</span><span class="sxs-lookup"><span data-stu-id="875da-157">hello order of hello public IP addresses for hello gateway instances and hello corresponding BGP Peering Addresses are hello same.</span></span> <span data-ttu-id="875da-158">I det här exemplet hello gateway VM med en offentlig IP för 40.112.190.5 använder 10.12.255.4 som sin BGP-Peering adress och hello gateway med 138.91.156.129 använder 10.12.255.5.</span><span class="sxs-lookup"><span data-stu-id="875da-158">In this example, hello gateway VM with public IP of 40.112.190.5 will use 10.12.255.4 as its BGP Peering Address, and hello gateway with 138.91.156.129 will use 10.12.255.5.</span></span> <span data-ttu-id="875da-159">Den här informationen behövs när du ställer in dina lokala VPN-enheter som ansluter toohello aktiv-aktiv gateway.</span><span class="sxs-lookup"><span data-stu-id="875da-159">This information is needed when you set up your on premises VPN devices connecting toohello active-active gateway.</span></span> <span data-ttu-id="875da-160">hello gateway framgår hello diagrammet nedan med alla adresser:</span><span class="sxs-lookup"><span data-stu-id="875da-160">hello gateway is shown in hello diagram below with all addresses:</span></span>

![aktiv-aktiv gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

<span data-ttu-id="875da-162">Du kan använda den här gatewayen tooestablish aktiv-aktiv mellan lokala eller VNet-till-VNet-anslutningen när hello gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="875da-162">Once hello gateway is created, you can use this gateway tooestablish active-active cross-premises or VNet-to-VNet connection.</span></span> <span data-ttu-id="875da-163">hello följande avsnitt kommer att gå igenom hello steg toocomplete hello övningen.</span><span class="sxs-lookup"><span data-stu-id="875da-163">hello following sections will walk through hello steps toocomplete hello exercise.</span></span>

## <span data-ttu-id="875da-164"><a name ="aacrossprem"></a>Del 2 – upprätta en anslutning för aktiv-aktiv mellan platser</span><span class="sxs-lookup"><span data-stu-id="875da-164"><a name ="aacrossprem"></a>Part 2 - Establish an active-active cross-premises connection</span></span>
<span data-ttu-id="875da-165">tooestablish en anslutning mellan platser, behöver du toocreate en lokal nätverksgateway toorepresent din lokala VPN-enhet och anslutningen tooconnect hello Azure VPN-gateway med hello lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="875da-165">tooestablish a cross-premises connection, you need toocreate a Local Network Gateway toorepresent your on-premises VPN device, and a Connection tooconnect hello Azure VPN gateway with hello local network gateway.</span></span> <span data-ttu-id="875da-166">I det här exemplet är hello Azure VPN-gateway i aktivt-aktivt läge.</span><span class="sxs-lookup"><span data-stu-id="875da-166">In this example, hello Azure VPN gateway is in active-active mode.</span></span> <span data-ttu-id="875da-167">Även om det finns endast en lokal VPN-enheter (lokal nätverksgateway) och en anslutning resurser, kommer båda Azure VPN gateway-instanser därför upprätta S2S VPN-tunnlar med hello lokala enhet.</span><span class="sxs-lookup"><span data-stu-id="875da-167">As a result, even though there is only one on-premises VPN device (local network gateway) and one connection resource, both Azure VPN gateway instances will establish S2S VPN tunnels with hello on-premises device.</span></span>

<span data-ttu-id="875da-168">Innan du fortsätter, kontrollera att du har slutfört [del 1](#aagateway) för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="875da-168">Before proceeding, please make sure you have completed [Part 1](#aagateway) of this exercise.</span></span>

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a><span data-ttu-id="875da-169">Steg 1 – Skapa och konfigurera hello lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="875da-169">Step 1 - Create and configure hello local network gateway</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="875da-170">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="875da-170">1. Declare your variables</span></span>
<span data-ttu-id="875da-171">Den här övningen fortsätter toobuild hello-konfiguration visas i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="875da-171">This exercise will continue toobuild hello configuration shown in hello diagram.</span></span> <span data-ttu-id="875da-172">Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="875da-172">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

<span data-ttu-id="875da-173">Ett par saker toonote om hello lokala gateway nätverksparametrar:</span><span class="sxs-lookup"><span data-stu-id="875da-173">A couple of things toonote regarding hello local network gateway parameters:</span></span>

* <span data-ttu-id="875da-174">hello lokal nätverksgateway kan vara i hello samma eller annan plats och grupp som hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="875da-174">hello local network gateway can be in hello same or different location and resource group as hello VPN gateway.</span></span> <span data-ttu-id="875da-175">Det här exemplet visar dem i olika resursgrupper men i hello samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="875da-175">This example shows them in different resource groups but in hello same Azure location.</span></span>
* <span data-ttu-id="875da-176">Om det finns bara en lokal VPN-enhet som du ser ovan, kan hello aktiv-aktiv anslutning arbeta med eller utan BGP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="875da-176">If there is only one on-premises VPN device as shown above, hello active-active connection can work with or without BGP protocol.</span></span> <span data-ttu-id="875da-177">Det här exemplet använder BGP för hello mellan lokala anslutningen.</span><span class="sxs-lookup"><span data-stu-id="875da-177">This example uses BGP for hello cross-premises connection.</span></span>
* <span data-ttu-id="875da-178">Om BGP är aktiverat är hello prefix som du behöver toodeclare för hello lokal nätverksgateway hello värden adressen för BGP-Peer-IP-adress på VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="875da-178">If BGP is enabled, hello prefix you need toodeclare for hello local network gateway is hello host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="875da-179">I det här fallet är det en /32 prefixet ”10.52.255.253/32”.</span><span class="sxs-lookup"><span data-stu-id="875da-179">In this case, it's a /32 prefix of "10.52.255.253/32".</span></span>
* <span data-ttu-id="875da-180">Du måste använda olika BGP, ASN: er, mellan ditt lokala nätverk och Azure VNet som en påminnelse.</span><span class="sxs-lookup"><span data-stu-id="875da-180">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="875da-181">Om de är hello samma måste toochange ditt VNet ASN om din lokala VPN-enhet redan använder hello ASN toopeer med andra BGP-grannar.</span><span class="sxs-lookup"><span data-stu-id="875da-181">If they are hello same, you need toochange your VNet ASN if your on-premises VPN device already uses hello ASN toopeer with other BGP neighbors.</span></span>

#### <a name="2-create-hello-local-network-gateway-for-site5"></a><span data-ttu-id="875da-182">2. Skapa hello lokal nätverksgateway för Site5</span><span class="sxs-lookup"><span data-stu-id="875da-182">2. Create hello local network gateway for Site5</span></span>
<span data-ttu-id="875da-183">Innan du fortsätter, kontrollera att du är fortfarande ansluten tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="875da-183">Before you continue, please make sure you are still connected tooSubscription 1.</span></span> <span data-ttu-id="875da-184">Skapa hello resursgrupp om den inte har skapats ännu.</span><span class="sxs-lookup"><span data-stu-id="875da-184">Create hello resource group if it is not yet created.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="875da-185">Steg 2 – ansluta hello VNet gateway och lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="875da-185">Step 2 - Connect hello VNet gateway and local network gateway</span></span>
#### <a name="1-get-hello-two-gateways"></a><span data-ttu-id="875da-186">1. Hämta hello två gateways</span><span class="sxs-lookup"><span data-stu-id="875da-186">1. Get hello two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a><span data-ttu-id="875da-187">2. Skapa hello TestVNet1 tooSite5 anslutning</span><span class="sxs-lookup"><span data-stu-id="875da-187">2. Create hello TestVNet1 tooSite5 connection</span></span>
<span data-ttu-id="875da-188">I det här steget skapar hello anslutning från TestVNet1 tooSite5_1 med ”Enablegpg” ställa in också$ True.</span><span class="sxs-lookup"><span data-stu-id="875da-188">In this step, you will create hello connection from TestVNet1 tooSite5_1 with "EnableBGP" set too$True.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a><span data-ttu-id="875da-189">3. VPN och BGP parametrar för din lokala VPN-enhet</span><span class="sxs-lookup"><span data-stu-id="875da-189">3. VPN and BGP parameters for your on-premises VPN device</span></span>
<span data-ttu-id="875da-190">hello exemplet nedan visar hello-parametrar som du ska ange i hello BGP konfigurationsavsnittet på din lokala VPN-enhet för den här övningen:</span><span class="sxs-lookup"><span data-stu-id="875da-190">hello example below lists hello parameters you will enter into hello BGP configuration section on your on-premises VPN device for this exercise:</span></span>

    - <span data-ttu-id="875da-191">Site5 ASN: 65050</span><span class="sxs-lookup"><span data-stu-id="875da-191">Site5 ASN            : 65050</span></span>
    - <span data-ttu-id="875da-192">Site5 BGP IP: 10.52.255.253</span><span class="sxs-lookup"><span data-stu-id="875da-192">Site5 BGP IP         : 10.52.255.253</span></span>
    - <span data-ttu-id="875da-193">Prefix tooannounce: (till exempel) 10.51.0.0/16 och 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="875da-193">Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16</span></span>
    - <span data-ttu-id="875da-194">Azure VNet ASN: 65010</span><span class="sxs-lookup"><span data-stu-id="875da-194">Azure VNet ASN       : 65010</span></span>
    - <span data-ttu-id="875da-195">Azure VNet BGP IP 1: 10.12.255.4 för tunnel too40.112.190.5</span><span class="sxs-lookup"><span data-stu-id="875da-195">Azure VNet BGP IP 1  : 10.12.255.4 for tunnel too40.112.190.5</span></span>
    - <span data-ttu-id="875da-196">Azure VNet BGP IP 2: 10.12.255.5 för tunnel too138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="875da-196">Azure VNet BGP IP 2  : 10.12.255.5 for tunnel too138.91.156.129</span></span>
    - <span data-ttu-id="875da-197">Statiska vägar: mål 10.12.255.4/32, nexthop hello VPN-tunnel gränssnittet too40.112.190.5 mål 10.12.255.5/32, nexthop hello VPN-tunnel gränssnittet too138.91.156.129</span><span class="sxs-lookup"><span data-stu-id="875da-197">Static routes        : Destination 10.12.255.4/32, nexthop hello VPN tunnel interface too40.112.190.5                        Destination 10.12.255.5/32, nexthop hello VPN tunnel interface too138.91.156.129</span></span>
    - <span data-ttu-id="875da-198">eBGP Multihopp: Kontrollera hello ”Multihopp” alternativet för eBGP är aktiverat på enheten om det behövs</span><span class="sxs-lookup"><span data-stu-id="875da-198">eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed</span></span>

<span data-ttu-id="875da-199">hello-anslutningen ska upprättas efter några minuter och hello BGP-peeringsessionen startar när hello IPsec-anslutning har upprättats.</span><span class="sxs-lookup"><span data-stu-id="875da-199">hello connection should be established after a few minutes, and hello BGP peering session will start once hello IPsec connection is established.</span></span> <span data-ttu-id="875da-200">Hittills har det här exemplet konfigureras endast en lokal VPN-enhet, vilket resulterar i hello diagrammet nedan:</span><span class="sxs-lookup"><span data-stu-id="875da-200">This example so far has configured only one on-premises VPN device, resulting in hello diagram shown below:</span></span>

![aktiv-aktiv-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-toohello-active-active-vpn-gateway"></a><span data-ttu-id="875da-202">Steg 3 – ansluta två lokala VPN-enheter toohello aktiv-aktiv VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="875da-202">Step 3 - Connect two on-premises VPN devices toohello active-active VPN gateway</span></span>
<span data-ttu-id="875da-203">Om du har två VPN-enheter på hello samma lokala nätverk, kan du uppnå dubbla redundans genom anslutande hello Azure VPN gateway toohello andra VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="875da-203">If you have two VPN devices at hello same on-premises network, you can achieve dual redundancy by connecting hello Azure VPN gateway toohello second VPN device.</span></span>

#### <a name="1-create-hello-second-local-network-gateway-for-site5"></a><span data-ttu-id="875da-204">1. Skapa hello andra lokal nätverksgateway för Site5</span><span class="sxs-lookup"><span data-stu-id="875da-204">1. Create hello second local network gateway for Site5</span></span>
<span data-ttu-id="875da-205">Observera att hello IP-adressen för gateway-adressprefix och adress för BGP-peering för hello andra lokal nätverksgateway inte får överlappa med hello tidigare lokal nätverksgateway för hello samma lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="875da-205">Note that hello gateway IP address, address prefix, and BGP peering address for hello second local network gateway must not overlap with hello previous local network gateway for hello same on-premises network.</span></span>

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-hello-vnet-gateway-and-hello-second-local-network-gateway"></a><span data-ttu-id="875da-206">2. Ansluta hello VNet gateway och hello andra lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="875da-206">2. Connect hello VNet gateway and hello second local network gateway</span></span>
<span data-ttu-id="875da-207">Skapa hello anslutning från TestVNet1 tooSite5_2 med ”Enablegpg” ställa in också$ True</span><span class="sxs-lookup"><span data-stu-id="875da-207">Create hello connection from TestVNet1 tooSite5_2 with "EnableBGP" set too$True</span></span>

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a><span data-ttu-id="875da-208">3. Parametrarna för VPN och BGP för andra lokala VPN-enheten</span><span class="sxs-lookup"><span data-stu-id="875da-208">3. VPN and BGP parameters for your second on-premises VPN device</span></span>
<span data-ttu-id="875da-209">På liknande sätt nedan visar hello parametrar kommer in hello andra VPN-enhet:</span><span class="sxs-lookup"><span data-stu-id="875da-209">Similarly, below lists hello parameters you will enter into hello second VPN device:</span></span>

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel too40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel too138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop hello VPN tunnel interface too40.112.190.5
                         Destination 10.12.255.5/32, nexthop hello VPN tunnel interface too138.91.156.129
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="875da-210">När hello-anslutning (tunnlar) upprättas, har du dubbla redundant VPN-enheter och ansluter dina lokala nätverk och Azure-tunnlar:</span><span class="sxs-lookup"><span data-stu-id="875da-210">Once hello connection (tunnels) are established, you will have dual redundant VPN devices and tunnels connecting your on-premises network and Azure:</span></span>

![dubbla-redundans-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <span data-ttu-id="875da-212"><a name ="aav2v"></a>Del 3: upprätta en anslutning för aktiv-aktiv VNet-till-VNet</span><span class="sxs-lookup"><span data-stu-id="875da-212"><a name ="aav2v"></a>Part 3 - Establish an active-active VNet-to-VNet connection</span></span>
<span data-ttu-id="875da-213">Det här avsnittet skapar du en aktiv-aktiv VNet-till-VNet-anslutning med BGP.</span><span class="sxs-lookup"><span data-stu-id="875da-213">This section creates an active-active VNet-to-VNet connection with BGP.</span></span> 

<span data-ttu-id="875da-214">hello anvisningarna nedan fortsätta från hello stegen ovan.</span><span class="sxs-lookup"><span data-stu-id="875da-214">hello instructions below continue from hello previous steps listed above.</span></span> <span data-ttu-id="875da-215">Du måste slutföra [del 1](#aagateway) toocreate TestVNet1 och konfigurera hello VPN-Gateway med BGP.</span><span class="sxs-lookup"><span data-stu-id="875da-215">You must complete [Part 1](#aagateway) toocreate and configure TestVNet1 and hello VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a><span data-ttu-id="875da-216">Steg 1 – Skapa TestVNet2 och hello VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="875da-216">Step 1 - Create TestVNet2 and hello VPN gateway</span></span>
<span data-ttu-id="875da-217">Det är viktigt toomake till att hello IP-adressutrymme hello nytt virtuellt nätverk, TestVNet2, inte överlappar med någon av VNet-intervall.</span><span class="sxs-lookup"><span data-stu-id="875da-217">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="875da-218">I det här exemplet hello virtuella nätverk som tillhör toohello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="875da-218">In this example, hello virtual networks belong toohello same subscription.</span></span> <span data-ttu-id="875da-219">Du kan konfigurera VNet-till-VNet-anslutningar mellan olika prenumerationer; Se för[konfigurera VNet-till-VNet-anslutningen](vpn-gateway-vnet-vnet-rm-ps.md) toolearn mer information.</span><span class="sxs-lookup"><span data-stu-id="875da-219">You can set up VNet-to-VNet connections between different subscriptions; please refer too[Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) toolearn more details.</span></span> <span data-ttu-id="875da-220">Kontrollera att du lägger till hello ”-Enablegpg $True” när skapar hello anslutningar tooenable BGP.</span><span class="sxs-lookup"><span data-stu-id="875da-220">Make sure you add hello "-EnableBgp $True" when creating hello connections tooenable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="875da-221">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="875da-221">1. Declare your variables</span></span>
<span data-ttu-id="875da-222">Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="875da-222">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

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

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a><span data-ttu-id="875da-223">2. Skapa TestVNet2 i hello ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="875da-223">2. Create TestVNet2 in hello new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-active-active-vpn-gateway-for-testvnet2"></a><span data-ttu-id="875da-224">3. Skapa hello aktiv-aktiv VPN-gateway för TestVNet2</span><span class="sxs-lookup"><span data-stu-id="875da-224">3. Create hello active-active VPN gateway for TestVNet2</span></span>
<span data-ttu-id="875da-225">Begäran två offentliga IP-adresser toobe allokerade toohello gateway skapas för ditt VNet.</span><span class="sxs-lookup"><span data-stu-id="875da-225">Request two public IP addresses toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="875da-226">Du måste också definiera hello undernät och IP-konfigurationer som krävs.</span><span class="sxs-lookup"><span data-stu-id="875da-226">You'll also define hello subnet and IP configurations required.</span></span>

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

<span data-ttu-id="875da-227">Skapa hello VPN-gateway med hello som antal och hello ”EnableActiveActiveFeature”-flagga.</span><span class="sxs-lookup"><span data-stu-id="875da-227">Create hello VPN gateway with hello AS number and hello "EnableActiveActiveFeature" flag.</span></span> <span data-ttu-id="875da-228">Observera att du måste åsidosätta hello standard ASN på Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="875da-228">Note that you must override hello default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="875da-229">hello ASN: er för hello anslutna Vnet måste vara olika tooenable BGP och transitroutning.</span><span class="sxs-lookup"><span data-stu-id="875da-229">hello ASNs for hello connected VNets must be different tooenable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="875da-230">Steg 2 – ansluta hello TestVNet1 och TestVNet2 gateways</span><span class="sxs-lookup"><span data-stu-id="875da-230">Step 2 - Connect hello TestVNet1 and TestVNet2 gateways</span></span>
<span data-ttu-id="875da-231">I det här exemplet är både gateways i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="875da-231">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="875da-232">Du kan slutföra det här steget i hello samma PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="875da-232">You can complete this step in hello same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="875da-233">1. Hämta båda gateways</span><span class="sxs-lookup"><span data-stu-id="875da-233">1. Get both gateways</span></span>
<span data-ttu-id="875da-234">Kontrollera att du loggar in och ansluta tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="875da-234">Make sure you log in and connect tooSubscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="875da-235">2. Skapa både anslutningar</span><span class="sxs-lookup"><span data-stu-id="875da-235">2. Create both connections</span></span>
<span data-ttu-id="875da-236">I det här steget ska du skapa hello anslutning från TestVNet1 tooTestVNet2 och hello anslutning från TestVNet2 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="875da-236">In this step, you will create hello connection from TestVNet1 tooTestVNet2, and hello connection from TestVNet2 tooTestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="875da-237">Vara säker på att tooenable BGP för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="875da-237">Be sure tooenable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="875da-238">När du har slutfört de här stegen hello anslutning ska upprätta några minuter och hello BGP-peeringsessionen tas upp när hello VNet-till-VNet-anslutningen har slutförts med dubbla redundans:</span><span class="sxs-lookup"><span data-stu-id="875da-238">After completing these steps, hello connection will be establish in a few minutes, and hello BGP peering session will be up once hello VNet-to-VNet connection is completed with dual redundancy:</span></span>

![aktiv-aktiv-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <span data-ttu-id="875da-240"><a name ="aaupdate"></a>En del 4 – uppdatera en befintlig gateway mellan aktiv-aktiv och aktivt vänteläge</span><span class="sxs-lookup"><span data-stu-id="875da-240"><a name ="aaupdate"></a>Part 4 - Update existing gateway between active-active and active-standby</span></span>
<span data-ttu-id="875da-241">hello sista avsnittet beskriver hur du kan konfigurera en befintlig Azure VPN-gateway i aktivt vänteläge tooactive-aktivt läge eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="875da-241">hello last section will describe how you can configure an existing Azure VPN gateway from active-standby tooactive-active mode, or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="875da-242">Det här avsnittet innehåller hello steg tooresize en äldre SKU (gamla SKU) av en VPN-gateway som redan har skapat från Standard tooHighPerformance.</span><span class="sxs-lookup"><span data-stu-id="875da-242">This section includes hello steps tooresize a legacy SKU (old SKU) of an already created VPN gateway from Standard tooHighPerformance.</span></span> <span data-ttu-id="875da-243">Dessa steg ska du inte uppgradera en gamla äldre SKU tooone av hello nya SKU: er.</span><span class="sxs-lookup"><span data-stu-id="875da-243">These steps do not upgrade an old legacy SKU tooone of hello new SKUs.</span></span>
> 
> 

### <a name="configure-an-active-standby-gateway-tooactive-active-gateway"></a><span data-ttu-id="875da-244">Konfigurera en gateway i aktivt vänteläge tooactive active-gateway</span><span class="sxs-lookup"><span data-stu-id="875da-244">Configure an active-standby gateway tooactive-active gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="875da-245">1. Gateway-parametrar</span><span class="sxs-lookup"><span data-stu-id="875da-245">1. Gateway parameters</span></span>
<span data-ttu-id="875da-246">följande exempel hello konverterar en gateway i aktivt vänteläge till en aktiv-aktiv gateway.</span><span class="sxs-lookup"><span data-stu-id="875da-246">hello following example converts an active-standby gateway into an active-active gateway.</span></span> <span data-ttu-id="875da-247">Du behöver toocreate en annan offentlig IP-adress och sedan lägga till en andra Gateway IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="875da-247">You need toocreate another public IP address, then add a second Gateway IP configuration.</span></span> <span data-ttu-id="875da-248">Nedan visas hello parametrar används:</span><span class="sxs-lookup"><span data-stu-id="875da-248">Below shows hello parameters used:</span></span>

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

#### <a name="2-create-hello-public-ip-address-then-add-hello-second-gateway-ip-configuration"></a><span data-ttu-id="875da-249">2. Skapa hello offentliga IP-adressen och lägger till hello andra gateway IP-konfiguration</span><span class="sxs-lookup"><span data-stu-id="875da-249">2. Create hello public IP address, then add hello second gateway IP configuration</span></span>

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-hello-gateway"></a><span data-ttu-id="875da-250">3. Aktivera aktiv-aktiv läge och uppdatera hello-gateway</span><span class="sxs-lookup"><span data-stu-id="875da-250">3. Enable active-active mode and update hello gateway</span></span>
<span data-ttu-id="875da-251">Du måste ange hello gateway-objekt i PowerShell tootrigger hello faktiska uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="875da-251">You must set hello gateway object in PowerShell tootrigger hello actual update.</span></span> <span data-ttu-id="875da-252">hello SKU hello virtuell nätverksgateway måste också ändras (storleksändrade) tooHighPerformance eftersom den har skapats tidigare som Standard.</span><span class="sxs-lookup"><span data-stu-id="875da-252">hello SKU of hello virtual network gateway must also be changed (resized) tooHighPerformance since it was created previously as Standard.</span></span>

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

<span data-ttu-id="875da-253">Den här uppdateringen kan ta 30 too45 minuter.</span><span class="sxs-lookup"><span data-stu-id="875da-253">This update can take 30 too45 minutes.</span></span>

### <a name="configure-an-active-active-gateway-tooactive-standby-gateway"></a><span data-ttu-id="875da-254">Konfigurera en gateway för aktiv-aktiv tooactive standby-gateway</span><span class="sxs-lookup"><span data-stu-id="875da-254">Configure an active-active gateway tooactive-standby gateway</span></span>
#### <a name="1-gateway-parameters"></a><span data-ttu-id="875da-255">1. Gateway-parametrar</span><span class="sxs-lookup"><span data-stu-id="875da-255">1. Gateway parameters</span></span>
<span data-ttu-id="875da-256">Använd hello samma parametrar som ovan, hämta hello namnet på hello IP-konfigurationen som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="875da-256">Use hello same parameters as above, get hello name of hello IP configuration you want tooremove.</span></span>

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-hello-gateway-ip-configuration-and-disable-hello-active-active-mode"></a><span data-ttu-id="875da-257">2. Ta bort hello gateway IP-konfigurationen och inaktivera hello aktiv-aktiv läge</span><span class="sxs-lookup"><span data-stu-id="875da-257">2. Remove hello gateway IP configuration and disable hello active-active mode</span></span>
<span data-ttu-id="875da-258">Dessutom måste du ange hello gateway-objekt i PowerShell tootrigger hello faktiska uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="875da-258">Similarly, you must set hello gateway object in PowerShell tootrigger hello actual update.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

<span data-ttu-id="875da-259">Den här uppdateringen kan ta upp too30 för 45 minuter.</span><span class="sxs-lookup"><span data-stu-id="875da-259">This update can take up too30 too 45 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="875da-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="875da-260">Next steps</span></span>
<span data-ttu-id="875da-261">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="875da-261">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="875da-262">Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="875da-262">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
