---
title: "Ansluta ett virtuellt Azure-nätverk till ett annat VNet: PowerShell | Microsoft Docs"
description: "Den här artikeln visar hur du ansluter virtuella nätverk tillsammans med hjälp av Azure Resource Manager och PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 8c42c0046ccaa98c572134042fbbb7e883ef93c3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a><span data-ttu-id="2e82a-103">Konfigurera en VPN-gatewayanslutning mellan virtuella nätverk med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e82a-103">Configure a VNet-to-VNet VPN gateway connection using PowerShell</span></span>

<span data-ttu-id="2e82a-104">Den här artikeln visar hur du skapar en VPN-gatewayanslutning mellan virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2e82a-104">This article shows you how to create a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="2e82a-105">De virtuella nätverken kan finnas i samma eller olika regioner och i samma eller olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2e82a-105">The virtual networks can be in the same or different regions, and from the same or different subscriptions.</span></span> <span data-ttu-id="2e82a-106">När du ansluter virtuella nätverk från olika prenumerationer, behöver inte prenumerationerna vara associerade med samma Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="2e82a-106">When connecting VNets from different subscriptions, the subscriptions do not need to be associated with the same Active Directory tenant.</span></span> 

<span data-ttu-id="2e82a-107">Anvisningarna i den här artikeln gäller för Resource Manager-distributionsmodellen och användning av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2e82a-107">The steps in this article apply to the Resource Manager deployment model and use PowerShell.</span></span> <span data-ttu-id="2e82a-108">Du kan också skapa den här konfigurationen med ett annat distributionsverktyg eller en annan distributionsmodell genom att välja ett annat alternativ i listan nedan:</span><span class="sxs-lookup"><span data-stu-id="2e82a-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2e82a-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2e82a-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="2e82a-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e82a-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="2e82a-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2e82a-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="2e82a-112">Azure Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="2e82a-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="2e82a-113">Ansluta olika distributionsmodeller – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2e82a-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="2e82a-114">Ansluta olika distributionsmodeller – PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e82a-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="2e82a-115">Du ansluter ett virtuellt nätverk till ett annat virtuellt nätverk (VNet-till-VNet) på nästan samma sätt som du ansluter ett VNet till en lokal plats.</span><span class="sxs-lookup"><span data-stu-id="2e82a-115">Connecting a virtual network to another virtual network (VNet-to-VNet) is similar to connecting a VNet to an on-premises site location.</span></span> <span data-ttu-id="2e82a-116">Båda typerna av anslutning använder en VPN-gateway för att få en säker tunnel med IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="2e82a-116">Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="2e82a-117">Om dina VNets finns i samma region kan det vara bättre att ansluta dem med hjälp av VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="2e82a-117">If your VNets are in the same region, you may want to consider connecting them using VNet Peering.</span></span> <span data-ttu-id="2e82a-118">Ingen VPN-gateway används för VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="2e82a-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="2e82a-119">Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2e82a-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="2e82a-120">VNet-till-VNet-kommunikation kan kombineras med konfigurationer för flera platser.</span><span class="sxs-lookup"><span data-stu-id="2e82a-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="2e82a-121">Därmed kan du etablera nätverkstopologier som kombinerar anslutningar mellan olika anläggningar med virtuell nätverksanslutning enligt följande diagram:</span><span class="sxs-lookup"><span data-stu-id="2e82a-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in the following diagram:</span></span>

![Om anslutningar](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a><span data-ttu-id="2e82a-123">Varför ska man ansluta virtuella nätverk?</span><span class="sxs-lookup"><span data-stu-id="2e82a-123">Why connect virtual networks?</span></span>

<span data-ttu-id="2e82a-124">Du kan vilja ansluta virtuella nätverk av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="2e82a-124">You may want to connect virtual networks for the following reasons:</span></span>

* <span data-ttu-id="2e82a-125">**Geografisk redundans i flera regioner och geografisk närvaro**</span><span class="sxs-lookup"><span data-stu-id="2e82a-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="2e82a-126">Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="2e82a-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="2e82a-127">Med Azure Traffic Manager och Load Balancer kan du konfigurera arbetsbelastning med hög tillgänglighet och geografisk redundans över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="2e82a-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="2e82a-128">Ett viktigt exempel är att konfigurera att SQL alltid är aktiverat med tillgänglighetsgrupper som är spridda över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="2e82a-128">One important example is to set up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="2e82a-129">**Regionala flernivåprogram med isolering eller administrativa gränser**</span><span class="sxs-lookup"><span data-stu-id="2e82a-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="2e82a-130">Inom samma region kan du konfigurera flernivåprogram med flera virtuella nätverk som är anslutna till varandra på grund av isolering eller administrativa krav.</span><span class="sxs-lookup"><span data-stu-id="2e82a-130">Within the same region, you can set up multi-tier applications with multiple virtual networks connected together due to isolation or administrative requirements.</span></span>

<span data-ttu-id="2e82a-131">Mer information om anslutningar mellan virtuella nätverk finns i [Vanliga frågor om VNet-till-VNet](#faq) i slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2e82a-131">For more information about VNet-to-VNet connections, see the [VNet-to-VNet FAQ](#faq) at the end of this article.</span></span>

## <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="2e82a-132">Vilka steg ska jag använda?</span><span class="sxs-lookup"><span data-stu-id="2e82a-132">Which set of steps should I use?</span></span>

<span data-ttu-id="2e82a-133">I den här artikeln beskrivs två uppsättningar med steg.</span><span class="sxs-lookup"><span data-stu-id="2e82a-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="2e82a-134">En uppsättning steg för [virtuella nätverk som finns i samma prenumeration](#samesub) och en annan för [virtuella nätverk som finns i olika prenumerationer](#difsub).</span><span class="sxs-lookup"><span data-stu-id="2e82a-134">One set of steps for [VNets that reside in the same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span> <span data-ttu-id="2e82a-135">Den viktigaste skillnaden mellan uppsättningarna är huruvida du kan skapa och konfigurera alla virtuella nätverk och gateway-resurser i samma PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="2e82a-135">The key difference between the sets is whether you can create and configure all virtual network and gateway resources within the same PowerShell session.</span></span>

<span data-ttu-id="2e82a-136">I stegen i den här artikeln används variabler som deklareras i början av varje avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2e82a-136">The steps in this article use variables that are declared at the beginning of each section.</span></span> <span data-ttu-id="2e82a-137">Om du redan arbetar med befintliga virtuella nätverk kan du ändra variablerna så att de avspeglar din miljö.</span><span class="sxs-lookup"><span data-stu-id="2e82a-137">If you already are working with existing VNets, modify the variables to reflect the settings in your own environment.</span></span> <span data-ttu-id="2e82a-138">Information om hur du använder namnmatchning för dina virtuella nätverk finns i [Namnmatchning](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="2e82a-138">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

## <span data-ttu-id="2e82a-139"><a name="samesub"></a>Så här ansluter du VNets som finns i samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="2e82a-139"><a name="samesub"></a>How to connect VNets that are in the same subscription</span></span>

![v2v-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="2e82a-141">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2e82a-141">Before you begin</span></span>

<span data-ttu-id="2e82a-142">Du måste installera den senaste versionen av Azure Resource Managers PowerShell-cmdletar, version 4.0 eller senare, innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="2e82a-142">Before beginning, you need to install the latest version of the Azure Resource Manager PowerShell cmdlets, at least 4.0 or later.</span></span> <span data-ttu-id="2e82a-143">Mer information om hur du installerar PowerShell-cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2e82a-143">For more information about installing the PowerShell cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <span data-ttu-id="2e82a-144"><a name="Step1"></a>Steg 1 – Planera dina IP-adressintervall</span><span class="sxs-lookup"><span data-stu-id="2e82a-144"><a name="Step1"></a>Step 1 - Plan your IP address ranges</span></span>

<span data-ttu-id="2e82a-145">I följande steg ska vi skapa två virtuella nätverk och deras respektive gateway-undernät och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="2e82a-145">In the following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="2e82a-146">Sedan ska vi skapa en VPN-anslutning mellan de två virtuella nätverken.</span><span class="sxs-lookup"><span data-stu-id="2e82a-146">We then create a VPN connection between the two VNets.</span></span> <span data-ttu-id="2e82a-147">Det är viktigt att planera IP-adressintervallen för nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-147">It’s important to plan the IP address ranges for your network configuration.</span></span> <span data-ttu-id="2e82a-148">Tänk på att inga av dina VNet-intervall eller lokala nätverksintervall får överlappa varandra på något sätt.</span><span class="sxs-lookup"><span data-stu-id="2e82a-148">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="2e82a-149">I de här exemplen tar vi inte med någon DNS-server.</span><span class="sxs-lookup"><span data-stu-id="2e82a-149">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="2e82a-150">Information om hur du använder namnmatchning för dina virtuella nätverk finns i [Namnmatchning](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="2e82a-150">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="2e82a-151">Vi använder följande värden i exemplen:</span><span class="sxs-lookup"><span data-stu-id="2e82a-151">We use the following values in the examples:</span></span>

<span data-ttu-id="2e82a-152">**Värden för TestVNet1:**</span><span class="sxs-lookup"><span data-stu-id="2e82a-152">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="2e82a-153">VNet-namn: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="2e82a-153">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="2e82a-154">Resursgrupp: TestRG1</span><span class="sxs-lookup"><span data-stu-id="2e82a-154">Resource Group: TestRG1</span></span>
* <span data-ttu-id="2e82a-155">Plats: Östra USA</span><span class="sxs-lookup"><span data-stu-id="2e82a-155">Location: East US</span></span>
* <span data-ttu-id="2e82a-156">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="2e82a-156">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="2e82a-157">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2e82a-157">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="2e82a-158">BackEnd: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2e82a-158">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="2e82a-159">GatewaySubnet: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="2e82a-159">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="2e82a-160">GatewayName: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="2e82a-160">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="2e82a-161">Offentlig IP: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="2e82a-161">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="2e82a-162">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="2e82a-162">VPNType: RouteBased</span></span>
* <span data-ttu-id="2e82a-163">Connection(1to4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="2e82a-163">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="2e82a-164">Connection(1to5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="2e82a-164">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="2e82a-165">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="2e82a-165">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="2e82a-166">**Värden för TestVNet4:**</span><span class="sxs-lookup"><span data-stu-id="2e82a-166">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="2e82a-167">VNet-namn: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="2e82a-167">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="2e82a-168">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="2e82a-168">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="2e82a-169">FrontEnd: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2e82a-169">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="2e82a-170">BackEnd: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2e82a-170">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="2e82a-171">GatewaySubnet: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="2e82a-171">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="2e82a-172">Resursgrupp: TestRG4</span><span class="sxs-lookup"><span data-stu-id="2e82a-172">Resource Group: TestRG4</span></span>
* <span data-ttu-id="2e82a-173">Plats: Västra USA</span><span class="sxs-lookup"><span data-stu-id="2e82a-173">Location: West US</span></span>
* <span data-ttu-id="2e82a-174">GatewayName: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="2e82a-174">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="2e82a-175">Offentlig IP: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="2e82a-175">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="2e82a-176">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="2e82a-176">VPNType: RouteBased</span></span>
* <span data-ttu-id="2e82a-177">Anslutning: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="2e82a-177">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="2e82a-178">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="2e82a-178">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="2e82a-179"><a name="Step2"></a>Steg 2 – Skapa och konfigurera TestVNet1</span><span class="sxs-lookup"><span data-stu-id="2e82a-179"><a name="Step2"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="2e82a-180">Deklarera dina variabler.</span><span class="sxs-lookup"><span data-stu-id="2e82a-180">Declare your variables.</span></span> <span data-ttu-id="2e82a-181">I det här exemplet deklarerar vi variablerna med värdena för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-181">This example declares the variables using the values for this exercise.</span></span> <span data-ttu-id="2e82a-182">I de flesta fall bör du ersätta värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="2e82a-182">In most cases, you should replace the values with your own.</span></span> <span data-ttu-id="2e82a-183">Du kan dock använda dessa variabler om du bara vill följa anvisningarna för att bekanta dig med den här typen av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2e82a-183">However, you can use these variables if you are running through the steps to become familiar with this type of configuration.</span></span> <span data-ttu-id="2e82a-184">Ändra variablerna om det behövs och kopiera och klistra in dem i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-184">Modify the variables if needed, then copy and paste them into your PowerShell console.</span></span>

  ```powershell
  $Sub1 = "Replace_With_Your_Subcription_Name"
  $RG1 = "TestRG1"
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
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconf1"
  $Connection14 = "VNet1toVNet4"
  $Connection15 = "VNet1toVNet5"
  ```

2. <span data-ttu-id="2e82a-185">Anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2e82a-185">Connect to your account.</span></span> <span data-ttu-id="2e82a-186">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="2e82a-186">Use the following example to help you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="2e82a-187">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="2e82a-187">Check the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="2e82a-188">Ange den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="2e82a-188">Specify the subscription that you want to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. <span data-ttu-id="2e82a-189">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="2e82a-189">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. <span data-ttu-id="2e82a-190">Skapa undernätskonfigurationerna för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="2e82a-190">Create the subnet configurations for TestVNet1.</span></span> <span data-ttu-id="2e82a-191">I det här exemplet skapas ett virtuellt nätverk med namnet TestVNet1 och tre undernät – GatewaySubnet, FrontEnd och BackEnd.</span><span class="sxs-lookup"><span data-stu-id="2e82a-191">This example creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="2e82a-192">När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="2e82a-192">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="2e82a-193">Om du ger det något annat namn går det inte att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-193">If you name it something else, your gateway creation fails.</span></span>

  <span data-ttu-id="2e82a-194">I följande exempel används variablerna som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="2e82a-194">The following example uses the variables that you set earlier.</span></span> <span data-ttu-id="2e82a-195">I det här exemplet använder gateway-undernätet en /27.</span><span class="sxs-lookup"><span data-stu-id="2e82a-195">In this example, the gateway subnet is using a /27.</span></span> <span data-ttu-id="2e82a-196">Även om det är möjligt att skapa ett gateway-subnät som är så litet som /29 så rekommenderar vi att du skapar ett större subnät som inkluderar fler adresser genom att välja minst /28 eller /27.</span><span class="sxs-lookup"><span data-stu-id="2e82a-196">While it is possible to create a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="2e82a-197">Det tillåter tillräckligt med adresser för att rymma möjliga övriga konfigurationer som du kan behöva i framtiden.</span><span class="sxs-lookup"><span data-stu-id="2e82a-197">This will allow for enough addresses to accommodate possible additional configurations that you may want in the future.</span></span>

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. <span data-ttu-id="2e82a-198">Skapa TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="2e82a-198">Create TestVNet1.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. <span data-ttu-id="2e82a-199">Begär en offentlig IP-adress som ska allokeras till den gateway som du ska skapa för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="2e82a-199">Request a public IP address to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="2e82a-200">Observera att AllocationMethod är Dynamic.</span><span class="sxs-lookup"><span data-stu-id="2e82a-200">Notice that the AllocationMethod is Dynamic.</span></span> <span data-ttu-id="2e82a-201">Du kan inte ange den IP-adress som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="2e82a-201">You cannot specify the IP address that you want to use.</span></span> <span data-ttu-id="2e82a-202">Den allokeras dynamiskt till gatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-202">It's dynamically allocated to your gateway.</span></span> 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="2e82a-203">Skapa gateway-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-203">Create the gateway configuration.</span></span> <span data-ttu-id="2e82a-204">Gateway-konfigurationen definierar undernätet och den offentliga IP-adress som ska användas.</span><span class="sxs-lookup"><span data-stu-id="2e82a-204">The gateway configuration defines the subnet and the public IP address to use.</span></span> <span data-ttu-id="2e82a-205">Använd exemplet för att skapa gateway-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-205">Use the example to create your gateway configuration.</span></span>

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. <span data-ttu-id="2e82a-206">Skapa gatewayen för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="2e82a-206">Create the gateway for TestVNet1.</span></span> <span data-ttu-id="2e82a-207">I det här steget ska du skapa VNet-gatewayen för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="2e82a-207">In this step, you create the virtual network gateway for your TestVNet1.</span></span> <span data-ttu-id="2e82a-208">VNet-till-VNet-konfigurationer kräver VpnType RouteBased.</span><span class="sxs-lookup"><span data-stu-id="2e82a-208">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="2e82a-209">Att skapa en gateway kan ofta ta 45 minuter eller mer, beroende på vald gateway-SKU.</span><span class="sxs-lookup"><span data-stu-id="2e82a-209">Creating a gateway can often take 45 minutes or more, depending on the selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a><span data-ttu-id="2e82a-210">Steg 3 – Skapa och konfigurera TestVNet4</span><span class="sxs-lookup"><span data-stu-id="2e82a-210">Step 3 - Create and configure TestVNet4</span></span>

<span data-ttu-id="2e82a-211">När du har konfigurerat TestVNet1 skapar du TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="2e82a-211">Once you've configured TestVNet1, create TestVNet4.</span></span> <span data-ttu-id="2e82a-212">Följ stegen nedan och ersätt värdena med dina egna vid behov.</span><span class="sxs-lookup"><span data-stu-id="2e82a-212">Follow the steps below, replacing the values with your own when needed.</span></span> <span data-ttu-id="2e82a-213">Det här steget kan göras inom samma PowerShell-session, eftersom den är i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2e82a-213">This step can be done within the same PowerShell session because it is in the same subscription.</span></span>

1. <span data-ttu-id="2e82a-214">Deklarera dina variabler.</span><span class="sxs-lookup"><span data-stu-id="2e82a-214">Declare your variables.</span></span> <span data-ttu-id="2e82a-215">Ersätt värdena med de som du vill använda för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2e82a-215">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

  ```powershell
  $RG4 = "TestRG4"
  $Location4 = "West US"
  $VnetName4 = "TestVNet4"
  $FESubName4 = "FrontEnd"
  $BESubName4 = "Backend"
  $GWSubName4 = "GatewaySubnet"
  $VnetPrefix41 = "10.41.0.0/16"
  $VnetPrefix42 = "10.42.0.0/16"
  $FESubPrefix4 = "10.41.0.0/24"
  $BESubPrefix4 = "10.42.0.0/24"
  $GWSubPrefix4 = "10.42.255.0/27"
  $GWName4 = "VNet4GW"
  $GWIPName4 = "VNet4GWIP"
  $GWIPconfName4 = "gwipconf4"
  $Connection41 = "VNet4toVNet1"
  ```
2. <span data-ttu-id="2e82a-216">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="2e82a-216">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. <span data-ttu-id="2e82a-217">Skapa undernätskonfigurationerna för TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="2e82a-217">Create the subnet configurations for TestVNet4.</span></span>

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. <span data-ttu-id="2e82a-218">Skapa TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="2e82a-218">Create TestVNet4.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. <span data-ttu-id="2e82a-219">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2e82a-219">Request a public IP address.</span></span>

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. <span data-ttu-id="2e82a-220">Skapa gateway-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-220">Create the gateway configuration.</span></span>

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. <span data-ttu-id="2e82a-221">Skapa TestVNet4-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-221">Create the TestVNet4 gateway.</span></span> <span data-ttu-id="2e82a-222">Att skapa en gateway kan ofta ta 45 minuter eller mer, beroende på vald gateway-SKU.</span><span class="sxs-lookup"><span data-stu-id="2e82a-222">Creating a gateway can often take 45 minutes or more, depending on the selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-the-connections"></a><span data-ttu-id="2e82a-223">Steg 4 – Skapa anslutningarna</span><span class="sxs-lookup"><span data-stu-id="2e82a-223">Step 4 - Create the connections</span></span>

1. <span data-ttu-id="2e82a-224">Hämta båda virtuella nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="2e82a-224">Get both virtual network gateways.</span></span> <span data-ttu-id="2e82a-225">Om båda gatewayerna finns i samma prenumeration, som i exemplet, kan du slutföra det här steget i samma PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="2e82a-225">If both of the gateways are in the same subscription, as they are in the example, you can complete this step in the same PowerShell session.</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. <span data-ttu-id="2e82a-226">Skapa TestVNet1-till-TestVNet4-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-226">Create the TestVNet1 to TestVNet4 connection.</span></span> <span data-ttu-id="2e82a-227">I det här steget ska du skapa anslutningen från TestVNet1 till TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="2e82a-227">In this step, you create the connection from TestVNet1 to TestVNet4.</span></span> <span data-ttu-id="2e82a-228">Du ser en delad nyckel som refereras i exemplen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-228">You'll see a shared key referenced in the examples.</span></span> <span data-ttu-id="2e82a-229">Du kan använda egna värden för den delade nyckeln.</span><span class="sxs-lookup"><span data-stu-id="2e82a-229">You can use your own values for the shared key.</span></span> <span data-ttu-id="2e82a-230">Det är viktigt att den delade nyckeln matchar båda anslutningarna.</span><span class="sxs-lookup"><span data-stu-id="2e82a-230">The important thing is that the shared key must match for both connections.</span></span> <span data-ttu-id="2e82a-231">Att skapa en anslutning kan ta en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="2e82a-231">Creating a connection can take a short while to complete.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. <span data-ttu-id="2e82a-232">Skapa TestVNet4-till-TestVNet1-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-232">Create the TestVNet4 to TestVNet1 connection.</span></span> <span data-ttu-id="2e82a-233">Det här steget liknar det ovan, förutom att du skapar anslutningen från TestVNet4 till TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="2e82a-233">This step is similar to the one above, except you are creating the connection from TestVNet4 to TestVNet1.</span></span> <span data-ttu-id="2e82a-234">Kontrollera att de delade nycklarna matchar.</span><span class="sxs-lookup"><span data-stu-id="2e82a-234">Make sure the shared keys match.</span></span> <span data-ttu-id="2e82a-235">Anslutningen upprättas efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="2e82a-235">The connection will be established after a few minutes.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="2e82a-236">Verifiera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-236">Verify your connection.</span></span> <span data-ttu-id="2e82a-237">Mer information finns i avsnittet [Verifiera anslutningen](#verify).</span><span class="sxs-lookup"><span data-stu-id="2e82a-237">See the section [How to verify your connection](#verify).</span></span>

## <span data-ttu-id="2e82a-238"><a name="difsub"></a>Så här ansluter du VNets som finns i olika prenumerationer</span><span class="sxs-lookup"><span data-stu-id="2e82a-238"><a name="difsub"></a>How to connect VNets that are in different subscriptions</span></span>

![v2v-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

<span data-ttu-id="2e82a-240">I det här scenariot ansluter vi TestVNet1 och TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="2e82a-240">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="2e82a-241">TestVNet1 och TestVNet5 finns i olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2e82a-241">TestVNet1 and TestVNet5 reside in a different subscription.</span></span> <span data-ttu-id="2e82a-242">Prenumerationerna behöver inte vara associerade med samma Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="2e82a-242">The subscriptions do not need to be associated with the same Active Directory tenant.</span></span> <span data-ttu-id="2e82a-243">Skillnaden mellan de här stegen och den tidigare uppsättningen är att några av konfigurationsstegen måste utföras i en separat PowerShell-session för den andra prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-243">The difference between these steps and the previous set is that some of the configuration steps need to be performed in a separate PowerShell session in the context of the second subscription.</span></span> <span data-ttu-id="2e82a-244">Detta gäller särskilt om två prenumerationer tillhör olika organisationer.</span><span class="sxs-lookup"><span data-stu-id="2e82a-244">Especially when the two subscriptions belong to different organizations.</span></span>

### <a name="step-5---create-and-configure-testvnet1"></a><span data-ttu-id="2e82a-245">Steg 5 – Skapa och konfigurera TestVNet1</span><span class="sxs-lookup"><span data-stu-id="2e82a-245">Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="2e82a-246">Du måste slutföra [Steg 1](#Step1) och [Steg 2](#Step2) från det tidigare avsnittet för att kunna skapa och konfigurera TestVNet1 och VPN-gatewayen för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="2e82a-246">You must complete [Step 1](#Step1) and [Step 2](#Step2) from the previous section to create and configure TestVNet1 and the VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="2e82a-247">I den här konfigurationen krävs det inte att du skapar TestVNet4 i enlighet med föregående avsnitt, men om du gör det går det ändå att använda de här instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="2e82a-247">For this configuration, you are not required to create TestVNet4 from the previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="2e82a-248">När du har slutfört steg 1 och steg 2 fortsätter du med steg 6 för att skapa TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="2e82a-248">Once you complete Step 1 and Step 2, continue with Step 6 to create TestVNet5.</span></span> 

### <a name="step-6---verify-the-ip-address-ranges"></a><span data-ttu-id="2e82a-249">Steg 6 – Kontrollera IP-adressintervaller</span><span class="sxs-lookup"><span data-stu-id="2e82a-249">Step 6 - Verify the IP address ranges</span></span>

<span data-ttu-id="2e82a-250">Det är viktigt att se till att IP-adressutrymmet för det nya virtuella nätverket, TestVNet5, inte överlappar några av dina VNet-intervall eller lokala intervall för nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-250">It is important to make sure that the IP address space of the new virtual network, TestVNet5, does not overlap with any of your VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="2e82a-251">I det här exemplet kan de virtuella nätverken tillhöra olika organisationer.</span><span class="sxs-lookup"><span data-stu-id="2e82a-251">In this example, the virtual networks may belong to different organizations.</span></span> <span data-ttu-id="2e82a-252">Använd följande VNet-värden för TestVNet5 i den här övningen:</span><span class="sxs-lookup"><span data-stu-id="2e82a-252">For this exercise, you can use the following values for the TestVNet5:</span></span>

<span data-ttu-id="2e82a-253">**Värden för TestVNet5:**</span><span class="sxs-lookup"><span data-stu-id="2e82a-253">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="2e82a-254">VNet-namn: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="2e82a-254">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="2e82a-255">Resursgrupp: TestRG5</span><span class="sxs-lookup"><span data-stu-id="2e82a-255">Resource Group: TestRG5</span></span>
* <span data-ttu-id="2e82a-256">Plats: Östra Japan</span><span class="sxs-lookup"><span data-stu-id="2e82a-256">Location: Japan East</span></span>
* <span data-ttu-id="2e82a-257">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="2e82a-257">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="2e82a-258">FrontEnd: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2e82a-258">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="2e82a-259">BackEnd: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2e82a-259">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="2e82a-260">GatewaySubnet: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="2e82a-260">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="2e82a-261">GatewayName: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="2e82a-261">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="2e82a-262">Offentlig IP: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="2e82a-262">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="2e82a-263">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="2e82a-263">VPNType: RouteBased</span></span>
* <span data-ttu-id="2e82a-264">Anslutning: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="2e82a-264">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="2e82a-265">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="2e82a-265">ConnectionType: VNet2VNet</span></span>

### <a name="step-7---create-and-configure-testvnet5"></a><span data-ttu-id="2e82a-266">Steg 7 – Skapa och konfigurera TestVNet5</span><span class="sxs-lookup"><span data-stu-id="2e82a-266">Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="2e82a-267">Det här steget måste utföras i den nya prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-267">This step must be done in the context of the new subscription.</span></span> <span data-ttu-id="2e82a-268">Den här delen kan utföras av administratören i en annan organisation som äger prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-268">This part may be performed by the administrator in a different organization that owns the subscription.</span></span>

1. <span data-ttu-id="2e82a-269">Deklarera dina variabler.</span><span class="sxs-lookup"><span data-stu-id="2e82a-269">Declare your variables.</span></span> <span data-ttu-id="2e82a-270">Ersätt värdena med de som du vill använda för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2e82a-270">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

  ```powershell
  $Sub5 = "Replace_With_the_New_Subcription_Name"
  $RG5 = "TestRG5"
  $Location5 = "Japan East"
  $VnetName5 = "TestVNet5"
  $FESubName5 = "FrontEnd"
  $BESubName5 = "Backend"
  $GWSubName5 = "GatewaySubnet"
  $VnetPrefix51 = "10.51.0.0/16"
  $VnetPrefix52 = "10.52.0.0/16"
  $FESubPrefix5 = "10.51.0.0/24"
  $BESubPrefix5 = "10.52.0.0/24"
  $GWSubPrefix5 = "10.52.255.0/27"
  $GWName5 = "VNet5GW"
  $GWIPName5 = "VNet5GWIP"
  $GWIPconfName5 = "gwipconf5"
  $Connection51 = "VNet5toVNet1"
  ```
2. <span data-ttu-id="2e82a-271">Anslut till Prenumeration 5.</span><span class="sxs-lookup"><span data-stu-id="2e82a-271">Connect to subscription 5.</span></span> <span data-ttu-id="2e82a-272">Öppna PowerShell-konsolen och anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2e82a-272">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="2e82a-273">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="2e82a-273">Use the following sample to help you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="2e82a-274">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="2e82a-274">Check the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="2e82a-275">Ange den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="2e82a-275">Specify the subscription that you want to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. <span data-ttu-id="2e82a-276">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="2e82a-276">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. <span data-ttu-id="2e82a-277">Skapa undernätskonfigurationerna för TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="2e82a-277">Create the subnet configurations for TestVNet5.</span></span>

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. <span data-ttu-id="2e82a-278">Skapa TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="2e82a-278">Create TestVNet5.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. <span data-ttu-id="2e82a-279">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2e82a-279">Request a public IP address.</span></span>

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="2e82a-280">Skapa gateway-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-280">Create the gateway configuration.</span></span>

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. <span data-ttu-id="2e82a-281">Skapa TestVNet5-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-281">Create the TestVNet5 gateway.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-the-connections"></a><span data-ttu-id="2e82a-282">Steg 8 – Skapa anslutningarna</span><span class="sxs-lookup"><span data-stu-id="2e82a-282">Step 8 - Create the connections</span></span>

<span data-ttu-id="2e82a-283">I det här exemplet där gatewayerna finns i olika prenumerationer, har vi delat upp steget i två PowerShell-sessioner som kallas för [Prenumeration 1] och [Prenumeration 5].</span><span class="sxs-lookup"><span data-stu-id="2e82a-283">In this example, because the gateways are in the different subscriptions, we've split this step into two PowerShell sessions marked as [Subscription 1] and [Subscription 5].</span></span>

1. <span data-ttu-id="2e82a-284">**[Prenumeration 1]** Hämta den virtuella nätverksgatewayen för Prenumeration 1.</span><span class="sxs-lookup"><span data-stu-id="2e82a-284">**[Subscription 1]** Get the virtual network gateway for Subscription 1.</span></span> <span data-ttu-id="2e82a-285">Logga in och anslut till Prenumeration 1 innan du kör följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2e82a-285">Log in and connect to Subscription 1 before running the following example:</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  <span data-ttu-id="2e82a-286">Kopiera utdatan från följande element och skicka dem till administratören för Prenumeration 5 via e-post eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="2e82a-286">Copy the output of the following elements and send these to the administrator of Subscription 5 via email or another method.</span></span>

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  <span data-ttu-id="2e82a-287">Dessa två element har värden som liknar följande exempelutdata:</span><span class="sxs-lookup"><span data-stu-id="2e82a-287">These two elements will have values similar to the following example output:</span></span>

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. <span data-ttu-id="2e82a-288">**[Prenumeration 5]** Hämta den virtuella nätverksgatewayen för Prenumeration 5.</span><span class="sxs-lookup"><span data-stu-id="2e82a-288">**[Subscription 5]** Get the virtual network gateway for Subscription 5.</span></span> <span data-ttu-id="2e82a-289">Logga in och anslut till Prenumeration 5 innan du kör följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2e82a-289">Log in and connect to Subscription 5 before running the following example:</span></span>

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  <span data-ttu-id="2e82a-290">Kopiera utdatan från följande element och skicka dem till administratören för Prenumeration 1 via e-post eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="2e82a-290">Copy the output of the following elements and send these to the administrator of Subscription 1 via email or another method.</span></span>

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  <span data-ttu-id="2e82a-291">Dessa två element har värden som liknar följande exempelutdata:</span><span class="sxs-lookup"><span data-stu-id="2e82a-291">These two elements will have values similar to the following example output:</span></span>

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. <span data-ttu-id="2e82a-292">**[Prenumeration 1]** Skapa TestVNet1-till-TestVNet5-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-292">**[Subscription 1]** Create the TestVNet1 to TestVNet5 connection.</span></span> <span data-ttu-id="2e82a-293">I det här steget ska du skapa anslutningen från TestVNet1 till TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="2e82a-293">In this step, you create the connection from TestVNet1 to TestVNet5.</span></span> <span data-ttu-id="2e82a-294">Skillnaden här är att $vnet5gw inte kan hämtas direkt, eftersom den finns i en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2e82a-294">The difference here is that $vnet5gw cannot be obtained directly because it is in a different subscription.</span></span> <span data-ttu-id="2e82a-295">Du måste skapa ett nytt PowerShell-objekt med värdena från Prenumeration 1 i stegen ovan.</span><span class="sxs-lookup"><span data-stu-id="2e82a-295">You will need to create a new PowerShell object with the values communicated from Subscription 1 in the steps above.</span></span> <span data-ttu-id="2e82a-296">Använd exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="2e82a-296">Use the example below.</span></span> <span data-ttu-id="2e82a-297">Ersätt namnet, ID:t och den delade nyckeln med dina värden.</span><span class="sxs-lookup"><span data-stu-id="2e82a-297">Replace the Name, Id, and shared key with your own values.</span></span> <span data-ttu-id="2e82a-298">Det är viktigt att den delade nyckeln matchar båda anslutningarna.</span><span class="sxs-lookup"><span data-stu-id="2e82a-298">The important thing is that the shared key must match for both connections.</span></span> <span data-ttu-id="2e82a-299">Att skapa en anslutning kan ta en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="2e82a-299">Creating a connection can take a short while to complete.</span></span>

  <span data-ttu-id="2e82a-300">Anslut till Prenumeration 1 innan du kör följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2e82a-300">Connect to Subscription 1 before running the following example:</span></span>

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="2e82a-301">**[Prenumeration 5]** Skapa TestVNet5-till-TestVNet1-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2e82a-301">**[Subscription 5]** Create the TestVNet5 to TestVNet1 connection.</span></span> <span data-ttu-id="2e82a-302">Det här steget liknar det ovan, förutom att du skapar anslutningen från TestVNet5 till TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="2e82a-302">This step is similar to the one above, except you are creating the connection from TestVNet5 to TestVNet1.</span></span> <span data-ttu-id="2e82a-303">Samma process att skapa ett PowerShell-objekt som baseras på de värden som erhållits från Prenumeration 1 gäller även här.</span><span class="sxs-lookup"><span data-stu-id="2e82a-303">The same process of creating a PowerShell object based on the values obtained from Subscription 1 applies here as well.</span></span> <span data-ttu-id="2e82a-304">Var noga med att de delade nycklarna matchar i det här steget.</span><span class="sxs-lookup"><span data-stu-id="2e82a-304">In this step, be sure that the shared keys match.</span></span>

  <span data-ttu-id="2e82a-305">Anslut till Prenumeration 5 innan du kör följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2e82a-305">Connect to Subscription 5 before running the following example:</span></span>

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <span data-ttu-id="2e82a-306"><a name="verify"></a>Så här verifierar du en anslutning</span><span class="sxs-lookup"><span data-stu-id="2e82a-306"><a name="verify"></a>How to verify a connection</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <span data-ttu-id="2e82a-307"><a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet</span><span class="sxs-lookup"><span data-stu-id="2e82a-307"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="2e82a-308">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e82a-308">Next steps</span></span>

* <span data-ttu-id="2e82a-309">När anslutningen är klar kan du lägga till virtuella datorer till dina virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2e82a-309">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="2e82a-310">Mer information finns i [dokumentationen för Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="2e82a-310">See the [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) for more information.</span></span>
* <span data-ttu-id="2e82a-311">Information om BGP finns i [BGP-översikt](vpn-gateway-bgp-overview.md) och [Så här konfigurerar du BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2e82a-311">For information about BGP, see the [BGP Overview](vpn-gateway-bgp-overview.md) and [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>