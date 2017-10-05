---
title: "Ansluta ett virtuellt Azure-nätverk till ett annat VNet: Azure CLI | Microsoft Docs"
description: "Den här artikeln visar hur du ansluter virtuella nätverk tillsammans med hjälp av Azure Resource Manager och Azure CLI."
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
ms.openlocfilehash: ae42f661b39e8b6170fd415d758404fb33009ccc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a><span data-ttu-id="d3110-103">Konfigurera en VPN-gatewayanslutning mellan virtuella nätverk med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d3110-103">Configure a VNet-to-VNet VPN gateway connection using Azure CLI</span></span>

<span data-ttu-id="d3110-104">Den här artikeln visar hur du skapar en VPN-gatewayanslutning mellan virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d3110-104">This article shows you how to create a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="d3110-105">De virtuella nätverken kan finnas i samma eller olika regioner och i samma eller olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="d3110-105">The virtual networks can be in the same or different regions, and from the same or different subscriptions.</span></span> <span data-ttu-id="d3110-106">När du ansluter virtuella nätverk från olika prenumerationer, behöver inte prenumerationerna vara associerade med samma Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="d3110-106">When connecting VNets from different subscriptions, the subscriptions do not need to be associated with the same Active Directory tenant.</span></span> 

<span data-ttu-id="d3110-107">Anvisningarna i den här artikeln gäller för Resource Manager-distributionsmodellen och användning av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d3110-107">The steps in this article apply to the Resource Manager deployment model and use Azure CLI.</span></span> <span data-ttu-id="d3110-108">Du kan också skapa den här konfigurationen med ett annat distributionsverktyg eller en annan distributionsmodell genom att välja ett annat alternativ i listan nedan:</span><span class="sxs-lookup"><span data-stu-id="d3110-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d3110-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3110-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="d3110-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d3110-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="d3110-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d3110-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="d3110-112">Azure Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="d3110-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="d3110-113">Ansluta olika distributionsmodeller – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3110-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="d3110-114">Ansluta olika distributionsmodeller – PowerShell</span><span class="sxs-lookup"><span data-stu-id="d3110-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="d3110-115">Du ansluter ett virtuellt nätverk till ett annat virtuellt nätverk (VNet-till-VNet) på nästan samma sätt som du ansluter ett VNet till en lokal plats.</span><span class="sxs-lookup"><span data-stu-id="d3110-115">Connecting a virtual network to another virtual network (VNet-to-VNet) is similar to connecting a VNet to an on-premises site location.</span></span> <span data-ttu-id="d3110-116">Båda typerna av anslutning använder en VPN-gateway för att få en säker tunnel med IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="d3110-116">Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="d3110-117">Om dina VNets finns i samma region kan det vara bättre att ansluta dem med hjälp av VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="d3110-117">If your VNets are in the same region, you may want to consider connecting them using VNet Peering.</span></span> <span data-ttu-id="d3110-118">Ingen VPN-gateway används för VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="d3110-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="d3110-119">Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3110-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="d3110-120">VNet-till-VNet-kommunikation kan kombineras med konfigurationer för flera platser.</span><span class="sxs-lookup"><span data-stu-id="d3110-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="d3110-121">Därmed kan du etablera nätverkstopologier som kombinerar anslutningar mellan olika anläggningar med virtuell nätverksanslutning enligt följande diagram:</span><span class="sxs-lookup"><span data-stu-id="d3110-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in the following diagram:</span></span>

![Om anslutningar](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <span data-ttu-id="d3110-123"><a name="why"></a>Varför ska man ansluta virtuella nätverk?</span><span class="sxs-lookup"><span data-stu-id="d3110-123"><a name="why"></a>Why connect virtual networks?</span></span>

<span data-ttu-id="d3110-124">Du kan vilja ansluta virtuella nätverk av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="d3110-124">You may want to connect virtual networks for the following reasons:</span></span>

* <span data-ttu-id="d3110-125">**Geografisk redundans i flera regioner och geografisk närvaro**</span><span class="sxs-lookup"><span data-stu-id="d3110-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="d3110-126">Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="d3110-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="d3110-127">Med Azure Traffic Manager och Load Balancer kan du konfigurera arbetsbelastning med hög tillgänglighet och geografisk redundans över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="d3110-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="d3110-128">Ett viktigt exempel är att konfigurera att SQL alltid är aktiverat med tillgänglighetsgrupper som är spridda över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="d3110-128">One important example is to set up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="d3110-129">**Regionala flernivåprogram med isolering eller administrativa gränser**</span><span class="sxs-lookup"><span data-stu-id="d3110-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="d3110-130">Inom samma region kan du konfigurera flernivåprogram med flera virtuella nätverk som är anslutna till varandra på grund av isolering eller administrativa krav.</span><span class="sxs-lookup"><span data-stu-id="d3110-130">Within the same region, you can set up multi-tier applications with multiple virtual networks connected together due to isolation or administrative requirements.</span></span>

<span data-ttu-id="d3110-131">Mer information om anslutningar mellan virtuella nätverk finns i [Vanliga frågor om VNet-till-VNet](#faq) i slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d3110-131">For more information about VNet-to-VNet connections, see the [VNet-to-VNet FAQ](#faq) at the end of this article.</span></span>

### <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="d3110-132">Vilka steg ska jag använda?</span><span class="sxs-lookup"><span data-stu-id="d3110-132">Which set of steps should I use?</span></span>

<span data-ttu-id="d3110-133">I den här artikeln beskrivs två uppsättningar med steg.</span><span class="sxs-lookup"><span data-stu-id="d3110-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="d3110-134">En uppsättning steg för [virtuella nätverk som finns i samma prenumeration](#samesub) och en annan för [virtuella nätverk som finns i olika prenumerationer](#difsub).</span><span class="sxs-lookup"><span data-stu-id="d3110-134">One set of steps for [VNets that reside in the same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span>

## <span data-ttu-id="d3110-135"><a name="samesub"></a>Så här ansluter du VNets som finns i samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="d3110-135"><a name="samesub"></a>Connect VNets that are in the same subscription</span></span>

![v2v-diagram](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="d3110-137">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="d3110-137">Before you begin</span></span>

<span data-ttu-id="d3110-138">Innan du börjar ska du installera den senaste versionen av CLI-kommandona (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="d3110-138">Before beginning, install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="d3110-139">Information om att installera CLI-kommandona finns i [Installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d3110-139">For information about installing the CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

### <span data-ttu-id="d3110-140"><a name="Plan"></a>Planera dina IP-adressintervall</span><span class="sxs-lookup"><span data-stu-id="d3110-140"><a name="Plan"></a>Plan your IP address ranges</span></span>

<span data-ttu-id="d3110-141">I följande steg ska vi skapa två virtuella nätverk och deras respektive gateway-undernät och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="d3110-141">In the following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="d3110-142">Sedan ska vi skapa en VPN-anslutning mellan de två virtuella nätverken.</span><span class="sxs-lookup"><span data-stu-id="d3110-142">We then create a VPN connection between the two VNets.</span></span> <span data-ttu-id="d3110-143">Det är viktigt att planera IP-adressintervallen för nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d3110-143">It’s important to plan the IP address ranges for your network configuration.</span></span> <span data-ttu-id="d3110-144">Tänk på att inga av dina VNet-intervall eller lokala nätverksintervall får överlappa varandra på något sätt.</span><span class="sxs-lookup"><span data-stu-id="d3110-144">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="d3110-145">I de här exemplen tar vi inte med någon DNS-server.</span><span class="sxs-lookup"><span data-stu-id="d3110-145">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="d3110-146">Information om hur du använder namnmatchning för dina virtuella nätverk finns i [Namnmatchning](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="d3110-146">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="d3110-147">Vi använder följande värden i exemplen:</span><span class="sxs-lookup"><span data-stu-id="d3110-147">We use the following values in the examples:</span></span>

<span data-ttu-id="d3110-148">**Värden för TestVNet1:**</span><span class="sxs-lookup"><span data-stu-id="d3110-148">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="d3110-149">VNet-namn: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="d3110-149">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="d3110-150">Resursgrupp: TestRG1</span><span class="sxs-lookup"><span data-stu-id="d3110-150">Resource Group: TestRG1</span></span>
* <span data-ttu-id="d3110-151">Plats: Östra USA</span><span class="sxs-lookup"><span data-stu-id="d3110-151">Location: East US</span></span>
* <span data-ttu-id="d3110-152">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="d3110-152">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="d3110-153">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="d3110-153">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="d3110-154">BackEnd: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="d3110-154">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="d3110-155">GatewaySubnet: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="d3110-155">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="d3110-156">GatewayName: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="d3110-156">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="d3110-157">Offentlig IP: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="d3110-157">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="d3110-158">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="d3110-158">VPNType: RouteBased</span></span>
* <span data-ttu-id="d3110-159">Connection(1to4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="d3110-159">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="d3110-160">Connection(1to5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="d3110-160">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="d3110-161">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="d3110-161">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="d3110-162">**Värden för TestVNet4:**</span><span class="sxs-lookup"><span data-stu-id="d3110-162">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="d3110-163">VNet-namn: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="d3110-163">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="d3110-164">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="d3110-164">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="d3110-165">FrontEnd: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="d3110-165">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="d3110-166">BackEnd: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="d3110-166">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="d3110-167">GatewaySubnet: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="d3110-167">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="d3110-168">Resursgrupp: TestRG4</span><span class="sxs-lookup"><span data-stu-id="d3110-168">Resource Group: TestRG4</span></span>
* <span data-ttu-id="d3110-169">Plats: Västra USA</span><span class="sxs-lookup"><span data-stu-id="d3110-169">Location: West US</span></span>
* <span data-ttu-id="d3110-170">GatewayName: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="d3110-170">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="d3110-171">Offentlig IP: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="d3110-171">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="d3110-172">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="d3110-172">VPNType: RouteBased</span></span>
* <span data-ttu-id="d3110-173">Anslutning: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="d3110-173">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="d3110-174">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="d3110-174">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="d3110-175"><a name="Connect"></a>Steg 1 - Ansluta till din prenumeration</span><span class="sxs-lookup"><span data-stu-id="d3110-175"><a name="Connect"></a>Step 1 - Connect to your subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <span data-ttu-id="d3110-176"><a name="TestVNet1"></a>Steg 2 – Skapa och konfigurera TestVNet1</span><span class="sxs-lookup"><span data-stu-id="d3110-176"><a name="TestVNet1"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="d3110-177">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d3110-177">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. <span data-ttu-id="d3110-178">Skapa TestVNet1 och undernäten för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="d3110-178">Create TestVNet1 and the subnets for TestVNet1.</span></span> <span data-ttu-id="d3110-179">I följande exempel skapas ett virtuellt nätverk med namnet ”TestVNet1” och ett undernät, ”FrontEnd”.</span><span class="sxs-lookup"><span data-stu-id="d3110-179">This example creates a virtual network named TestVNet1 and a subnet named FrontEnd.</span></span>

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. <span data-ttu-id="d3110-180">Skapa ytterligare ett adressutrymme för ett backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="d3110-180">Create an additional address space for the backend subnet.</span></span> <span data-ttu-id="d3110-181">Observera att i det här steget ska vi ange såväl adressutrymmet som vi skapade tidigare som ytterligare adressutrymme ett som vi vill lägga till.</span><span class="sxs-lookup"><span data-stu-id="d3110-181">Notice that in this step, we specify both the address space that we created earlier, and the additional address space that we want to add.</span></span> <span data-ttu-id="d3110-182">Detta beror på att kommandot [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) skriver över de tidigare inställningarna.</span><span class="sxs-lookup"><span data-stu-id="d3110-182">This is because the [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) command overwrites the previous settings.</span></span> <span data-ttu-id="d3110-183">Se till att ange alla adressprefixen när du använder det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="d3110-183">Make sure to specify all of the address prefixes when using this command.</span></span>

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. <span data-ttu-id="d3110-184">Skapa backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="d3110-184">Create the backend subnet.</span></span>
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. <span data-ttu-id="d3110-185">Skapa gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="d3110-185">Create the gateway subnet.</span></span> <span data-ttu-id="d3110-186">Observera att gateway-undernätet har namnet 'GatewaySubnet'.</span><span class="sxs-lookup"><span data-stu-id="d3110-186">Notice that the gateway subnet is named 'GatewaySubnet'.</span></span> <span data-ttu-id="d3110-187">Namnet är obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="d3110-187">This name is required.</span></span> <span data-ttu-id="d3110-188">I det här exemplet använder gateway-undernätet en /27.</span><span class="sxs-lookup"><span data-stu-id="d3110-188">In this example, the gateway subnet is using a /27.</span></span> <span data-ttu-id="d3110-189">Även om det är möjligt att skapa ett gateway-subnät som är så litet som /29 så rekommenderar vi att du skapar ett större subnät som inkluderar fler adresser genom att välja minst /28 eller /27.</span><span class="sxs-lookup"><span data-stu-id="d3110-189">While it is possible to create a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="d3110-190">Det tillåter tillräckligt med adresser för att rymma möjliga övriga konfigurationer som du kan behöva i framtiden.</span><span class="sxs-lookup"><span data-stu-id="d3110-190">This will allow for enough addresses to accommodate possible additional configurations that you may want in the future.</span></span>

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. <span data-ttu-id="d3110-191">Begär en offentlig IP-adress som ska allokeras till den gateway som du ska skapa för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="d3110-191">Request a public IP address to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="d3110-192">Observera att AllocationMethod är Dynamic.</span><span class="sxs-lookup"><span data-stu-id="d3110-192">Notice that the AllocationMethod is Dynamic.</span></span> <span data-ttu-id="d3110-193">Du kan inte ange den IP-adress som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="d3110-193">You cannot specify the IP address that you want to use.</span></span> <span data-ttu-id="d3110-194">Den allokeras dynamiskt till gatewayen.</span><span class="sxs-lookup"><span data-stu-id="d3110-194">It's dynamically allocated to your gateway.</span></span>

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. <span data-ttu-id="d3110-195">Skapa den virtuella nätverksgatewayen för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="d3110-195">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="d3110-196">VNet-till-VNet-konfigurationer kräver VpnType RouteBased.</span><span class="sxs-lookup"><span data-stu-id="d3110-196">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="d3110-197">Om du kör det här kommandot med parametern ”--no-wait” ser du ingen feedback eller utdata.</span><span class="sxs-lookup"><span data-stu-id="d3110-197">If you run this command using the '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="d3110-198">Parametern '--no-wait' gör att gatewayen kan skapas i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="d3110-198">The '--no-wait' parameter allows the gateway to create in the background.</span></span> <span data-ttu-id="d3110-199">Det innebär inte att VPN-gateway skapas i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="d3110-199">It does not mean that the VPN gateway finishes creating immediately.</span></span> <span data-ttu-id="d3110-200">Att skapa en gateway kan ofta ta 45 minuter eller mer, beroende på den gateway-SKU som använder.</span><span class="sxs-lookup"><span data-stu-id="d3110-200">Creating a gateway can often take 45 minutes or more, depending on the gateway SKU that you use.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="d3110-201"><a name="TestVNet4"></a>Steg 3 – Skapa och konfigurera TestVNet4</span><span class="sxs-lookup"><span data-stu-id="d3110-201"><a name="TestVNet4"></a>Step 3 - Create and configure TestVNet4</span></span>

1. <span data-ttu-id="d3110-202">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d3110-202">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. <span data-ttu-id="d3110-203">Skapa TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="d3110-203">Create TestVNet4.</span></span>

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. <span data-ttu-id="d3110-204">Skapa extra undernät för TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="d3110-204">Create additional subnets for TestVNet4.</span></span>

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. <span data-ttu-id="d3110-205">Skapa gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="d3110-205">Create the gateway subnet.</span></span>

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. <span data-ttu-id="d3110-206">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d3110-206">Request a Public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. <span data-ttu-id="d3110-207">Skapa den virtuella nätverksgatewayen TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="d3110-207">Create the TestVNet4 virtual network gateway.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="d3110-208"><a name="createconnect"></a>Steg 4 – Skapa anslutningarna</span><span class="sxs-lookup"><span data-stu-id="d3110-208"><a name="createconnect"></a>Step 4 - Create the connections</span></span>

<span data-ttu-id="d3110-209">Nu har du två VNets med VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="d3110-209">You now have two VNets with VPN gateways.</span></span> <span data-ttu-id="d3110-210">Nästa steg är att skapa VPN-gateway-anslutningar mellan de virtuella nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="d3110-210">The next step is to create VPN gateway connections between the virtual network gateways.</span></span> <span data-ttu-id="d3110-211">Om du har använt exemplen ovan är VNet-gatewayerna i olika resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="d3110-211">If you used the examples above, your VNet gateways are in different resource groups.</span></span> <span data-ttu-id="d3110-212">När gatewayer finns i olika resursgrupper, måste du identifiera och ange resurs-ID för varje gateway när en anslutning upprättas.</span><span class="sxs-lookup"><span data-stu-id="d3110-212">When gateways are in different resource groups, you need to identify and specify the resource IDs for each gateway when making a connection.</span></span> <span data-ttu-id="d3110-213">Om dina Vnets är i samma resursgrupp kan du använda den [andra uppsättningen anvisningar](#samerg) eftersom du inte behöver ange resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="d3110-213">If your VNets are in the same resource group, you can use the [second set of instructions](#samerg) because you don't need to specify the resource IDs.</span></span>

### <span data-ttu-id="d3110-214"><a name="diffrg"></a>Ansluta VNets som finns i olika resursgrupper</span><span class="sxs-lookup"><span data-stu-id="d3110-214"><a name="diffrg"></a>To connect VNets that reside in different resource groups</span></span>

1. <span data-ttu-id="d3110-215">Hämta resurs-ID för VNet1GW från utdata från följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d3110-215">Get the Resource ID of VNet1GW from the output of the following command:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="d3110-216">Hitta raden ”id”: i resultatet.</span><span class="sxs-lookup"><span data-stu-id="d3110-216">In the output, find the "id:" line.</span></span> <span data-ttu-id="d3110-217">Värden inom citattecken behövs för att skapa anslutningen i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d3110-217">The values within the quotes are needed to create the connection in the next section.</span></span> <span data-ttu-id="d3110-218">Kopiera värdena till en textredigerare, till exempel Anteckningar, så att du enkelt kan klistra in dem när du skapar din anslutning.</span><span class="sxs-lookup"><span data-stu-id="d3110-218">Copy these values to a text editor, such as Notepad, so that you can easily paste them when creating your connection.</span></span>

  <span data-ttu-id="d3110-219">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="d3110-219">Example output:</span></span>

  ```
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65515, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
   }, 
  "enableBgp": false, 
  "etag": "W/\"ecb42bc5-c176-44e1-802f-b0ce2962ac04\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW", 
  "ipConfigurations":
  ```

  <span data-ttu-id="d3110-220">Kopiera värdena efter **”id”:** inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="d3110-220">Copy the values after **"id":** within the quotes.</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. <span data-ttu-id="d3110-221">Hämta resurs-ID för VNet4GW och kopiera värdena till en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="d3110-221">Get the Resource ID of VNet4GW and copy the values to a text editor.</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. <span data-ttu-id="d3110-222">Skapa TestVNet1-till-TestVNet4-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d3110-222">Create the TestVNet1 to TestVNet4 connection.</span></span> <span data-ttu-id="d3110-223">I det här steget ska du skapa anslutningen från TestVNet1 till TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="d3110-223">In this step, you create the connection from TestVNet1 to TestVNet4.</span></span> <span data-ttu-id="d3110-224">Den finns en delad nyckel som refereras i exemplen.</span><span class="sxs-lookup"><span data-stu-id="d3110-224">There is a shared key referenced in the examples.</span></span> <span data-ttu-id="d3110-225">Du kan använda egna värden för den delade nyckeln.</span><span class="sxs-lookup"><span data-stu-id="d3110-225">You can use your own values for the shared key.</span></span> <span data-ttu-id="d3110-226">Det är viktigt att den delade nyckeln matchar båda anslutningarna.</span><span class="sxs-lookup"><span data-stu-id="d3110-226">The important thing is that the shared key must match for both connections.</span></span> <span data-ttu-id="d3110-227">Att skapa en anslutning kan ta en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="d3110-227">Creating a connection takes a short while to complete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. <span data-ttu-id="d3110-228">Skapa TestVNet4-till-TestVNet1-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d3110-228">Create the TestVNet4 to TestVNet1 connection.</span></span> <span data-ttu-id="d3110-229">Det här steget liknar det ovan, förutom att du skapar anslutningen från TestVNet4 till TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="d3110-229">This step is similar to the one above, except you are creating the connection from TestVNet4 to TestVNet1.</span></span> <span data-ttu-id="d3110-230">Kontrollera att de delade nycklarna matchar.</span><span class="sxs-lookup"><span data-stu-id="d3110-230">Make sure the shared keys match.</span></span> <span data-ttu-id="d3110-231">Det tar några minuter att upprätta anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d3110-231">It takes a few minutes to establish the connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. <span data-ttu-id="d3110-232">Verifiera dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d3110-232">Verify your connections.</span></span> <span data-ttu-id="d3110-233">Se [Verifiera din anslutning](#verify).</span><span class="sxs-lookup"><span data-stu-id="d3110-233">See [Verify your connection](#verify).</span></span>

### <span data-ttu-id="d3110-234"><a name="samerg"></a>För att ansluta Vnets som finns i samma resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d3110-234"><a name="samerg"></a>To connect VNets that reside in the same resource group</span></span>

1. <span data-ttu-id="d3110-235">Skapa TestVNet1-till-TestVNet4-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d3110-235">Create the TestVNet1 to TestVNet4 connection.</span></span> <span data-ttu-id="d3110-236">I det här steget ska du skapa anslutningen från TestVNet1 till TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="d3110-236">In this step, you create the connection from TestVNet1 to TestVNet4.</span></span> <span data-ttu-id="d3110-237">Lägg märke att till resursgrupper är samma i exemplen.</span><span class="sxs-lookup"><span data-stu-id="d3110-237">Notice the resource groups are the same in the examples.</span></span> <span data-ttu-id="d3110-238">Du ser även en delad nyckel som refereras i exemplen.</span><span class="sxs-lookup"><span data-stu-id="d3110-238">You also see a shared key referenced in the examples.</span></span> <span data-ttu-id="d3110-239">Du kan använda egna värden för den delade nyckeln, men den delade nyckeln måste matcha för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d3110-239">You can use your own values for the shared key, however, the shared key must match for both connections.</span></span> <span data-ttu-id="d3110-240">Att skapa en anslutning kan ta en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="d3110-240">Creating a connection takes a short while to complete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. <span data-ttu-id="d3110-241">Skapa TestVNet4-till-TestVNet1-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d3110-241">Create the TestVNet4 to TestVNet1 connection.</span></span> <span data-ttu-id="d3110-242">Det här steget liknar det ovan, förutom att du skapar anslutningen från TestVNet4 till TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="d3110-242">This step is similar to the one above, except you are creating the connection from TestVNet4 to TestVNet1.</span></span> <span data-ttu-id="d3110-243">Kontrollera att de delade nycklarna matchar.</span><span class="sxs-lookup"><span data-stu-id="d3110-243">Make sure the shared keys match.</span></span> <span data-ttu-id="d3110-244">Det tar några minuter att upprätta anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d3110-244">It takes a few minutes to establish the connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. <span data-ttu-id="d3110-245">Verifiera dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d3110-245">Verify your connections.</span></span> <span data-ttu-id="d3110-246">Se [Verifiera din anslutning](#verify).</span><span class="sxs-lookup"><span data-stu-id="d3110-246">See [Verify your connection](#verify).</span></span>

## <span data-ttu-id="d3110-247"><a name="difsub"></a>Ansluta VNets som finns i olika prenumerationer</span><span class="sxs-lookup"><span data-stu-id="d3110-247"><a name="difsub"></a>Connect VNets that are in different subscriptions</span></span>

![v2v-diagram](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

<span data-ttu-id="d3110-249">I det här scenariot ansluter vi TestVNet1 och TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="d3110-249">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="d3110-250">VNets finns i olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="d3110-250">The VNets reside different subscriptions.</span></span> <span data-ttu-id="d3110-251">Prenumerationerna behöver inte vara associerade med samma Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="d3110-251">The subscriptions do not need to be associated with the same Active Directory tenant.</span></span> <span data-ttu-id="d3110-252">I stegen för den här konfigurationen lägger vi till ytterligare en VNet-till-VNet-anslutning för att kunna ansluta TestVNet1 till TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="d3110-252">The steps for this configuration add an additional VNet-to-VNet connection in order to connect TestVNet1 to TestVNet5.</span></span>

### <span data-ttu-id="d3110-253"><a name="TestVNet1diff"></a>Steg 5 – Skapa och konfigurera TestVNet1</span><span class="sxs-lookup"><span data-stu-id="d3110-253"><a name="TestVNet1diff"></a>Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="d3110-254">Dessa instruktioner bygger på stegen i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d3110-254">These instructions continue from the steps in the preceding sections.</span></span> <span data-ttu-id="d3110-255">Du måste slutföra [Steg 1](#Connect) och [Steg 2](#TestVNet1) för att kunna skapa och konfigurera TestVNet1 och VPN Gateway för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="d3110-255">You must complete [Step 1](#Connect) and [Step 2](#TestVNet1) to create and configure TestVNet1 and the VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="d3110-256">I den här konfigurationen krävs det inte att du skapar TestVNet4 i enlighet med föregående avsnitt, men om du gör det går det ändå att använda de här instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="d3110-256">For this configuration, you are not required to create TestVNet4 from the previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="d3110-257">När du har slutfört steg 1 och steg 2 fortsätter du med steg 6 (nedan).</span><span class="sxs-lookup"><span data-stu-id="d3110-257">Once you complete Step 1 and Step 2, continue with Step 6 (below).</span></span>

### <span data-ttu-id="d3110-258"><a name="verifyranges"></a>Steg 6 – Kontrollera IP-adressintervaller</span><span class="sxs-lookup"><span data-stu-id="d3110-258"><a name="verifyranges"></a>Step 6 - Verify the IP address ranges</span></span>

<span data-ttu-id="d3110-259">När du skapar ytterligare anslutningar är det viktigt att se till att IP-adressutrymmet för det nya virtuella nätverket, inte överlappar några av dina VNet-intervall eller lokala intervall för nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="d3110-259">When creating additional connections, it's important to verify that the IP address space of the new virtual network does not overlap with any of your other VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="d3110-260">Använd följande VNet-värden för TestVNet5 i den här övningen:</span><span class="sxs-lookup"><span data-stu-id="d3110-260">For this exercise, you can use the following values for the TestVNet5:</span></span>

<span data-ttu-id="d3110-261">**Värden för TestVNet5:**</span><span class="sxs-lookup"><span data-stu-id="d3110-261">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="d3110-262">VNet-namn: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="d3110-262">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="d3110-263">Resursgrupp: TestRG5</span><span class="sxs-lookup"><span data-stu-id="d3110-263">Resource Group: TestRG5</span></span>
* <span data-ttu-id="d3110-264">Plats: Östra Japan</span><span class="sxs-lookup"><span data-stu-id="d3110-264">Location: Japan East</span></span>
* <span data-ttu-id="d3110-265">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="d3110-265">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="d3110-266">FrontEnd: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="d3110-266">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="d3110-267">BackEnd: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="d3110-267">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="d3110-268">GatewaySubnet: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="d3110-268">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="d3110-269">GatewayName: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="d3110-269">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="d3110-270">Offentlig IP: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="d3110-270">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="d3110-271">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="d3110-271">VPNType: RouteBased</span></span>
* <span data-ttu-id="d3110-272">Anslutning: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="d3110-272">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="d3110-273">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="d3110-273">ConnectionType: VNet2VNet</span></span>

### <span data-ttu-id="d3110-274"><a name="TestVNet5"></a>Steg 7 – Skapa och konfigurera TestVNet5</span><span class="sxs-lookup"><span data-stu-id="d3110-274"><a name="TestVNet5"></a>Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="d3110-275">Det här steget måste utföras i den nya prenumerationen, prenumeration 5.</span><span class="sxs-lookup"><span data-stu-id="d3110-275">This step must be done in the context of the new subscription, Subscription 5.</span></span> <span data-ttu-id="d3110-276">Den här delen kan utföras av administratören i en annan organisation som äger prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="d3110-276">This part may be performed by the administrator in a different organization that owns the subscription.</span></span> <span data-ttu-id="d3110-277">För att växla mellan prenumerationer använder du ' az account list --all' för att visa en lista över prenumerationer som är tillgängliga för ditt konto. Använd sedan ' az account set--subscription <subscriptionID>' för att växla till den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="d3110-277">To switch between subscriptions use 'az account list --all' to list the subscriptions available to your account, then use 'az account set --subscription <subscriptionID>' to switch to the subscription that you want to use.</span></span>

1. <span data-ttu-id="d3110-278">Kontrollera att du är ansluten till prenumeration 5 och sedan skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d3110-278">Make sure you are connected to Subscription 5, then create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. <span data-ttu-id="d3110-279">Skapa TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="d3110-279">Create TestVNet5.</span></span>

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. <span data-ttu-id="d3110-280">Lägg till undernät.</span><span class="sxs-lookup"><span data-stu-id="d3110-280">Add subnets.</span></span>

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. <span data-ttu-id="d3110-281">Lägg till gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="d3110-281">Add the gateway subnet.</span></span>

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. <span data-ttu-id="d3110-282">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d3110-282">Request a public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. <span data-ttu-id="d3110-283">Skapa TestVNet5-gatewayen</span><span class="sxs-lookup"><span data-stu-id="d3110-283">Create the TestVNet5 gateway</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="d3110-284"><a name="connections5"></a>Steg 8 – Skapa anslutningarna</span><span class="sxs-lookup"><span data-stu-id="d3110-284"><a name="connections5"></a>Step 8 - Create the connections</span></span>

<span data-ttu-id="d3110-285">Vi har upp steget i två CLI-sessioner som kallas för **[Prenumeration 1]** och **[Prenumeration 5]** eftersom gatewayerna finns i olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="d3110-285">We split this step into two CLI sessions marked as **[Subscription 1]**, and **[Subscription 5]** because the gateways are in the different subscriptions.</span></span> <span data-ttu-id="d3110-286">För att växla mellan prenumerationer använder du ' az account list --all' för att visa en lista över prenumerationer som är tillgängliga för ditt konto. Använd sedan ' az account set--subscription <subscriptionID>' för att växla till den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="d3110-286">To switch between subscriptions use 'az account list --all' to list the subscriptions available to your account, then use 'az account set --subscription <subscriptionID>' to switch to the subscription that you want to use.</span></span>

1. <span data-ttu-id="d3110-287">**[Prenumeration 1]** Logga in och anslut till Prenumeration 1.</span><span class="sxs-lookup"><span data-stu-id="d3110-287">**[Subscription 1]** Log in and connect to Subscription 1.</span></span> <span data-ttu-id="d3110-288">Kör följande kommando för att hämta namn och ID för gatewayen från utdata:</span><span class="sxs-lookup"><span data-stu-id="d3110-288">Run the following command to get the name and ID of the Gateway from the output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="d3110-289">Kopiera utdata för ”id:”.</span><span class="sxs-lookup"><span data-stu-id="d3110-289">Copy the output for "id:".</span></span> <span data-ttu-id="d3110-290">Skicka ID och namn på den VNet-gatewayen (VNet1GW) till prenumeration 5-administratören via e-post eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="d3110-290">Send the ID and the name of the VNet gateway (VNet1GW) to the administrator of Subscription 5 via email or another method.</span></span>

  <span data-ttu-id="d3110-291">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="d3110-291">Example output:</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. <span data-ttu-id="d3110-292">**[Prenumeration 5]** Logga in och anslut till Prenumeration 5.</span><span class="sxs-lookup"><span data-stu-id="d3110-292">**[Subscription 5]** Log in and connect to Subscription 5.</span></span> <span data-ttu-id="d3110-293">Kör följande kommando för att hämta namn och ID för gatewayen från utdata:</span><span class="sxs-lookup"><span data-stu-id="d3110-293">Run the following command to get the name and ID of the Gateway from the output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  <span data-ttu-id="d3110-294">Kopiera utdata för ”id:”.</span><span class="sxs-lookup"><span data-stu-id="d3110-294">Copy the output for "id:".</span></span> <span data-ttu-id="d3110-295">Skicka ID och namn på den VNet-gatewayen (VNet5GW) till prenumeration 1-administratören via e-post eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="d3110-295">Send the ID and the name of the VNet gateway (VNet5GW) to the administrator of Subscription 1 via email or another method.</span></span>

3. <span data-ttu-id="d3110-296">**[Prenumeration 1]** I det här steget ska du skapa anslutningen från TestVNet1 till TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="d3110-296">**[Subscription 1]** In this step, you create the connection from TestVNet1 to TestVNet5.</span></span> <span data-ttu-id="d3110-297">Du kan använda egna värden för den delade nyckeln, men den delade nyckeln måste matcha för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d3110-297">You can use your own values for the shared key, however, the shared key must match for both connections.</span></span> <span data-ttu-id="d3110-298">Att skapa en anslutning kan ta en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="d3110-298">Creating a connection can take a short while to complete.</span></span> <span data-ttu-id="d3110-299">Kontrollera att du ansluter till Prenumeration 1.</span><span class="sxs-lookup"><span data-stu-id="d3110-299">Make sure you connect to Subscription 1.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. <span data-ttu-id="d3110-300">**[Prenumeration 5]** Det här steget liknar det ovan, förutom att du skapar anslutningen från TestVNet5 till TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="d3110-300">**[Subscription 5]** This step is similar to the one above, except you are creating the connection from TestVNet5 to TestVNet1.</span></span> <span data-ttu-id="d3110-301">Kontrollera att de delade nycklarna matchar och att du ansluter till Prenumeration 5.</span><span class="sxs-lookup"><span data-stu-id="d3110-301">Make sure that the shared keys match and that you connect to Subscription 5.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <span data-ttu-id="d3110-302"><a name="verify"></a>Kontrollera anslutningarna</span><span class="sxs-lookup"><span data-stu-id="d3110-302"><a name="verify"></a>Verify the connections</span></span>
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <span data-ttu-id="d3110-303"><a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet</span><span class="sxs-lookup"><span data-stu-id="d3110-303"><a name="faq"></a>VNet-to-VNet FAQ</span></span>
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="d3110-304">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3110-304">Next steps</span></span>

* <span data-ttu-id="d3110-305">När anslutningen är klar kan du lägga till virtuella datorer till dina virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d3110-305">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="d3110-306">Mer information finns i [Dokumentationen för virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="d3110-306">For more information, see the [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="d3110-307">Information om BGP finns i [BGP-översikt](vpn-gateway-bgp-overview.md) och [Så här konfigurerar du BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d3110-307">For information about BGP, see the [BGP Overview](vpn-gateway-bgp-overview.md) and [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
