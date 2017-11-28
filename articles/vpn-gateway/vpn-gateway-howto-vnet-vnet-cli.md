---
title: "Ansluta virtuella nätverk tooanother VNet: Azure CLI | Microsoft Docs"
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
ms.openlocfilehash: 70113914bcae03c80f9ad133ff081d1cf37fc309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a><span data-ttu-id="778a3-103">Konfigurera en VPN-gatewayanslutning mellan virtuella nätverk med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="778a3-103">Configure a VNet-to-VNet VPN gateway connection using Azure CLI</span></span>

<span data-ttu-id="778a3-104">Den här artikeln beskrivs hur du toocreate en VPN-gateway-anslutningen mellan virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="778a3-104">This article shows you how toocreate a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="778a3-105">hello virtuella nätverk kan vara i samma eller olika regioner hello och hello från samma eller olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="778a3-105">hello virtual networks can be in hello same or different regions, and from hello same or different subscriptions.</span></span> <span data-ttu-id="778a3-106">När du ansluter Vnet från olika prenumerationer, hello prenumerationer inte behöver toobe som är associerade med hello samma Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="778a3-106">When connecting VNets from different subscriptions, hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> 

<span data-ttu-id="778a3-107">hello stegen i den här artikeln gäller toohello Resource Manager-modellen och använder Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="778a3-107">hello steps in this article apply toohello Resource Manager deployment model and use Azure CLI.</span></span> <span data-ttu-id="778a3-108">Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="778a3-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="778a3-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="778a3-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="778a3-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="778a3-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="778a3-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="778a3-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="778a3-112">Azure Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="778a3-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="778a3-113">Ansluta olika distributionsmodeller – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="778a3-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="778a3-114">Ansluta olika distributionsmodeller – PowerShell</span><span class="sxs-lookup"><span data-stu-id="778a3-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="778a3-115">Ansluta ett virtuellt nätverk tooanother virtuellt nätverk (VNet-till-VNet) är liknande tooconnecting ett VNet tooan lokal plats.</span><span class="sxs-lookup"><span data-stu-id="778a3-115">Connecting a virtual network tooanother virtual network (VNet-to-VNet) is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="778a3-116">Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="778a3-116">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="778a3-117">Om ditt Vnet i hello samma region som du kanske vill tooconsider ansluter dem med hjälp av VNet-Peering.</span><span class="sxs-lookup"><span data-stu-id="778a3-117">If your VNets are in hello same region, you may want tooconsider connecting them using VNet Peering.</span></span> <span data-ttu-id="778a3-118">Ingen VPN-gateway används för VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="778a3-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="778a3-119">Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="778a3-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="778a3-120">VNet-till-VNet-kommunikation kan kombineras med konfigurationer för flera platser.</span><span class="sxs-lookup"><span data-stu-id="778a3-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="778a3-121">På så sätt kan du etablera nätverkstopologier som kombinerar korsanslutningar med mellan virtuell nätverksanslutning som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="778a3-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in hello following diagram:</span></span>

![Om anslutningar](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <span data-ttu-id="778a3-123"><a name="why"></a>Varför ska man ansluta virtuella nätverk?</span><span class="sxs-lookup"><span data-stu-id="778a3-123"><a name="why"></a>Why connect virtual networks?</span></span>

<span data-ttu-id="778a3-124">Du kanske vill tooconnect virtuella nätverk för hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="778a3-124">You may want tooconnect virtual networks for hello following reasons:</span></span>

* <span data-ttu-id="778a3-125">**Geografisk redundans i flera regioner och geografisk närvaro**</span><span class="sxs-lookup"><span data-stu-id="778a3-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="778a3-126">Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="778a3-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="778a3-127">Med Azure Traffic Manager och Load Balancer kan du konfigurera arbetsbelastning med hög tillgänglighet och geografisk redundans över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="778a3-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="778a3-128">Ett viktigt exempel är tooset in SQL Always On med Tillgänglighetsgrupper sprida över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="778a3-128">One important example is tooset up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="778a3-129">**Regionala flernivåprogram med isolering eller administrativa gränser**</span><span class="sxs-lookup"><span data-stu-id="778a3-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="778a3-130">Hej i samma region, du kan konfigurera flera nivåer program med flera virtuella nätverk som kopplar samman förfallodatum tooisolation eller administrativa krav.</span><span class="sxs-lookup"><span data-stu-id="778a3-130">Within hello same region, you can set up multi-tier applications with multiple virtual networks connected together due tooisolation or administrative requirements.</span></span>

<span data-ttu-id="778a3-131">Mer information om VNet-till-VNet-anslutningar finns hello [VNet-till-VNet vanliga frågor och svar](#faq) hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="778a3-131">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span>

### <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="778a3-132">Vilka steg ska jag använda?</span><span class="sxs-lookup"><span data-stu-id="778a3-132">Which set of steps should I use?</span></span>

<span data-ttu-id="778a3-133">I den här artikeln beskrivs två uppsättningar med steg.</span><span class="sxs-lookup"><span data-stu-id="778a3-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="778a3-134">En uppsättning steg för [Vnet som finns i hello samma prenumeration](#samesub), och en annan för [Vnet som finns i olika prenumerationer](#difsub).</span><span class="sxs-lookup"><span data-stu-id="778a3-134">One set of steps for [VNets that reside in hello same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span>

## <span data-ttu-id="778a3-135"><a name="samesub"></a>Ansluta Vnet som finns i hello samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="778a3-135"><a name="samesub"></a>Connect VNets that are in hello same subscription</span></span>

![v2v-diagram](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="778a3-137">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="778a3-137">Before you begin</span></span>

<span data-ttu-id="778a3-138">Innan du börjar installera hello senaste versionen av hello CLI-kommandona (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="778a3-138">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="778a3-139">Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="778a3-139">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

### <span data-ttu-id="778a3-140"><a name="Plan"></a>Planera dina IP-adressintervall</span><span class="sxs-lookup"><span data-stu-id="778a3-140"><a name="Plan"></a>Plan your IP address ranges</span></span>

<span data-ttu-id="778a3-141">I följande steg hello, skapar vi två virtuella nätverk samt deras respektive gateway-undernät och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="778a3-141">In hello following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="778a3-142">Vi skapar sedan en VPN-anslutning mellan hello två Vnet.</span><span class="sxs-lookup"><span data-stu-id="778a3-142">We then create a VPN connection between hello two VNets.</span></span> <span data-ttu-id="778a3-143">Det är viktigt tooplan hello IP-adressintervall för nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="778a3-143">It’s important tooplan hello IP address ranges for your network configuration.</span></span> <span data-ttu-id="778a3-144">Tänk på att inga av dina VNet-intervall eller lokala nätverksintervall får överlappa varandra på något sätt.</span><span class="sxs-lookup"><span data-stu-id="778a3-144">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="778a3-145">I de här exemplen tar vi inte med någon DNS-server.</span><span class="sxs-lookup"><span data-stu-id="778a3-145">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="778a3-146">Information om hur du använder namnmatchning för dina virtuella nätverk finns i [Namnmatchning](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="778a3-146">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="778a3-147">Vi använder följande värden i hello exempel hello:</span><span class="sxs-lookup"><span data-stu-id="778a3-147">We use hello following values in hello examples:</span></span>

<span data-ttu-id="778a3-148">**Värden för TestVNet1:**</span><span class="sxs-lookup"><span data-stu-id="778a3-148">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="778a3-149">VNet-namn: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="778a3-149">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="778a3-150">Resursgrupp: TestRG1</span><span class="sxs-lookup"><span data-stu-id="778a3-150">Resource Group: TestRG1</span></span>
* <span data-ttu-id="778a3-151">Plats: Östra USA</span><span class="sxs-lookup"><span data-stu-id="778a3-151">Location: East US</span></span>
* <span data-ttu-id="778a3-152">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="778a3-152">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="778a3-153">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="778a3-153">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="778a3-154">BackEnd: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="778a3-154">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="778a3-155">GatewaySubnet: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="778a3-155">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="778a3-156">GatewayName: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="778a3-156">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="778a3-157">Offentlig IP: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="778a3-157">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="778a3-158">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="778a3-158">VPNType: RouteBased</span></span>
* <span data-ttu-id="778a3-159">Connection(1to4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="778a3-159">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="778a3-160">Connection(1to5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="778a3-160">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="778a3-161">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="778a3-161">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="778a3-162">**Värden för TestVNet4:**</span><span class="sxs-lookup"><span data-stu-id="778a3-162">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="778a3-163">VNet-namn: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="778a3-163">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="778a3-164">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="778a3-164">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="778a3-165">FrontEnd: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="778a3-165">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="778a3-166">BackEnd: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="778a3-166">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="778a3-167">GatewaySubnet: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="778a3-167">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="778a3-168">Resursgrupp: TestRG4</span><span class="sxs-lookup"><span data-stu-id="778a3-168">Resource Group: TestRG4</span></span>
* <span data-ttu-id="778a3-169">Plats: Västra USA</span><span class="sxs-lookup"><span data-stu-id="778a3-169">Location: West US</span></span>
* <span data-ttu-id="778a3-170">GatewayName: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="778a3-170">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="778a3-171">Offentlig IP: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="778a3-171">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="778a3-172">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="778a3-172">VPNType: RouteBased</span></span>
* <span data-ttu-id="778a3-173">Anslutning: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="778a3-173">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="778a3-174">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="778a3-174">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="778a3-175"><a name="Connect"></a>Steg 1 – ansluta tooyour prenumeration</span><span class="sxs-lookup"><span data-stu-id="778a3-175"><a name="Connect"></a>Step 1 - Connect tooyour subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <span data-ttu-id="778a3-176"><a name="TestVNet1"></a>Steg 2 – Skapa och konfigurera TestVNet1</span><span class="sxs-lookup"><span data-stu-id="778a3-176"><a name="TestVNet1"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="778a3-177">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="778a3-177">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. <span data-ttu-id="778a3-178">Skapa TestVNet1 och hello undernät för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="778a3-178">Create TestVNet1 and hello subnets for TestVNet1.</span></span> <span data-ttu-id="778a3-179">I följande exempel skapas ett virtuellt nätverk med namnet ”TestVNet1” och ett undernät, ”FrontEnd”.</span><span class="sxs-lookup"><span data-stu-id="778a3-179">This example creates a virtual network named TestVNet1 and a subnet named FrontEnd.</span></span>

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. <span data-ttu-id="778a3-180">Skapa en ytterligare adressutrymme för hello backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="778a3-180">Create an additional address space for hello backend subnet.</span></span> <span data-ttu-id="778a3-181">Observera att i det här steget ska vi anger både hello-adressutrymmet som vi skapade tidigare och hello ytterligare adressutrymme som vi vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="778a3-181">Notice that in this step, we specify both hello address space that we created earlier, and hello additional address space that we want tooadd.</span></span> <span data-ttu-id="778a3-182">Detta beror på att hello [az network vnet uppdatering](https://docs.microsoft.com/cli/azure/network/vnet#update) kommando skriver över tidigare hello-inställningarna.</span><span class="sxs-lookup"><span data-stu-id="778a3-182">This is because hello [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) command overwrites hello previous settings.</span></span> <span data-ttu-id="778a3-183">Se till att toospecify alla hello adressprefix när du använder det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="778a3-183">Make sure toospecify all of hello address prefixes when using this command.</span></span>

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. <span data-ttu-id="778a3-184">Skapa hello backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="778a3-184">Create hello backend subnet.</span></span>
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. <span data-ttu-id="778a3-185">Skapa hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="778a3-185">Create hello gateway subnet.</span></span> <span data-ttu-id="778a3-186">Observera att hello gateway-undernätet har namnet 'GatewaySubnet'.</span><span class="sxs-lookup"><span data-stu-id="778a3-186">Notice that hello gateway subnet is named 'GatewaySubnet'.</span></span> <span data-ttu-id="778a3-187">Namnet är obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="778a3-187">This name is required.</span></span> <span data-ttu-id="778a3-188">I det här exemplet hello gateway-undernätet med hjälp av en minst/27.</span><span class="sxs-lookup"><span data-stu-id="778a3-188">In this example, hello gateway subnet is using a /27.</span></span> <span data-ttu-id="778a3-189">Det är möjligt toocreate ett gatewayundernät så liten som /29, rekommenderar vi att du skapar ett större undernät som innehåller flera adresser genom att välja minst /28 eller minst/27.</span><span class="sxs-lookup"><span data-stu-id="778a3-189">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="778a3-190">Detta gör att tillräckligt många adresser tooaccommodate möjliga ytterligare konfigurationer som du vill i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="778a3-190">This will allow for enough addresses tooaccommodate possible additional configurations that you may want in hello future.</span></span>

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. <span data-ttu-id="778a3-191">Begär en offentlig IP-adress toobe allokerade toohello gateway skapas för ditt VNet.</span><span class="sxs-lookup"><span data-stu-id="778a3-191">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="778a3-192">Observera att hello AllocationMethod är dynamiska.</span><span class="sxs-lookup"><span data-stu-id="778a3-192">Notice that hello AllocationMethod is Dynamic.</span></span> <span data-ttu-id="778a3-193">Du kan inte ange hello IP-adress som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="778a3-193">You cannot specify hello IP address that you want toouse.</span></span> <span data-ttu-id="778a3-194">Det är dynamiskt allokerade tooyour gateway.</span><span class="sxs-lookup"><span data-stu-id="778a3-194">It's dynamically allocated tooyour gateway.</span></span>

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. <span data-ttu-id="778a3-195">Skapa hello virtuell nätverksgateway för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="778a3-195">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="778a3-196">VNet-till-VNet-konfigurationer kräver VpnType RouteBased.</span><span class="sxs-lookup"><span data-stu-id="778a3-196">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="778a3-197">Om du kör det här kommandot med hello – inget - wait-parameter, kan du inte ser några feedback eller utdata.</span><span class="sxs-lookup"><span data-stu-id="778a3-197">If you run this command using hello '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="778a3-198">hello tillåter – inget - wait-parameter som hello gateway toocreate i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="778a3-198">hello '--no-wait' parameter allows hello gateway toocreate in hello background.</span></span> <span data-ttu-id="778a3-199">Det innebär inte att hello VPN-gateway är klar skapar omedelbart.</span><span class="sxs-lookup"><span data-stu-id="778a3-199">It does not mean that hello VPN gateway finishes creating immediately.</span></span> <span data-ttu-id="778a3-200">Skapa en gateway kan ofta ta 45 minuter eller mer beroende på hello gateway SKU som du använder.</span><span class="sxs-lookup"><span data-stu-id="778a3-200">Creating a gateway can often take 45 minutes or more, depending on hello gateway SKU that you use.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="778a3-201"><a name="TestVNet4"></a>Steg 3 – Skapa och konfigurera TestVNet4</span><span class="sxs-lookup"><span data-stu-id="778a3-201"><a name="TestVNet4"></a>Step 3 - Create and configure TestVNet4</span></span>

1. <span data-ttu-id="778a3-202">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="778a3-202">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. <span data-ttu-id="778a3-203">Skapa TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="778a3-203">Create TestVNet4.</span></span>

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. <span data-ttu-id="778a3-204">Skapa extra undernät för TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="778a3-204">Create additional subnets for TestVNet4.</span></span>

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. <span data-ttu-id="778a3-205">Skapa hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="778a3-205">Create hello gateway subnet.</span></span>

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. <span data-ttu-id="778a3-206">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="778a3-206">Request a Public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. <span data-ttu-id="778a3-207">Skapa hello TestVNet4 virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="778a3-207">Create hello TestVNet4 virtual network gateway.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="778a3-208"><a name="createconnect"></a>Steg 4 – skapa hello-anslutningar</span><span class="sxs-lookup"><span data-stu-id="778a3-208"><a name="createconnect"></a>Step 4 - Create hello connections</span></span>

<span data-ttu-id="778a3-209">Nu har du två VNets med VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="778a3-209">You now have two VNets with VPN gateways.</span></span> <span data-ttu-id="778a3-210">hello nästa steg är toocreate VPN gateway-anslutningar mellan hello virtuella nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="778a3-210">hello next step is toocreate VPN gateway connections between hello virtual network gateways.</span></span> <span data-ttu-id="778a3-211">Om du har använt hello exemplen ovan är VNet-gateways i olika resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="778a3-211">If you used hello examples above, your VNet gateways are in different resource groups.</span></span> <span data-ttu-id="778a3-212">När gatewayer har olika resursgrupper kan du behöver tooidentify och ange hello resurs-ID för varje gateway när en anslutning upprättas.</span><span class="sxs-lookup"><span data-stu-id="778a3-212">When gateways are in different resource groups, you need tooidentify and specify hello resource IDs for each gateway when making a connection.</span></span> <span data-ttu-id="778a3-213">Om ditt Vnet i hello samma resursgrupp kan du använda hello [andra uppsättningen anvisningar](#samerg) eftersom du inte behöver toospecify hello resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="778a3-213">If your VNets are in hello same resource group, you can use hello [second set of instructions](#samerg) because you don't need toospecify hello resource IDs.</span></span>

### <span data-ttu-id="778a3-214"><a name="diffrg"></a>tooconnect Vnet som finns i olika resursgrupper</span><span class="sxs-lookup"><span data-stu-id="778a3-214"><a name="diffrg"></a>tooconnect VNets that reside in different resource groups</span></span>

1. <span data-ttu-id="778a3-215">Hämta hello resurs-ID för VNet1GW från hello utdata från hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="778a3-215">Get hello Resource ID of VNet1GW from hello output of hello following command:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="778a3-216">Hello utdata och hitta hello ”id”: rad.</span><span class="sxs-lookup"><span data-stu-id="778a3-216">In hello output, find hello "id:" line.</span></span> <span data-ttu-id="778a3-217">hello värden inom citattecken hello är nödvändiga toocreate hello anslutning i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="778a3-217">hello values within hello quotes are needed toocreate hello connection in hello next section.</span></span> <span data-ttu-id="778a3-218">Kopiera dessa värden tooa textredigerare, till exempel Anteckningar, så att du enkelt kan klistra in dem när du skapar din anslutning.</span><span class="sxs-lookup"><span data-stu-id="778a3-218">Copy these values tooa text editor, such as Notepad, so that you can easily paste them when creating your connection.</span></span>

  <span data-ttu-id="778a3-219">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="778a3-219">Example output:</span></span>

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

  <span data-ttu-id="778a3-220">Kopiera hello värden efter **”id”:** inom hello citattecken.</span><span class="sxs-lookup"><span data-stu-id="778a3-220">Copy hello values after **"id":** within hello quotes.</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. <span data-ttu-id="778a3-221">Hämta hello VNet4GW resurs-ID och kopiera hello värden tooa textredigerare.</span><span class="sxs-lookup"><span data-stu-id="778a3-221">Get hello Resource ID of VNet4GW and copy hello values tooa text editor.</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. <span data-ttu-id="778a3-222">Skapa hello TestVNet1 tooTestVNet4 anslutning.</span><span class="sxs-lookup"><span data-stu-id="778a3-222">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="778a3-223">I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet4.</span><span class="sxs-lookup"><span data-stu-id="778a3-223">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="778a3-224">Det finns en delad nyckel som refereras i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="778a3-224">There is a shared key referenced in hello examples.</span></span> <span data-ttu-id="778a3-225">Du kan använda egna värden för hello delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="778a3-225">You can use your own values for hello shared key.</span></span> <span data-ttu-id="778a3-226">hello viktig sak är att hello delad nyckel måste matcha för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="778a3-226">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="778a3-227">Skapa en anslutning tar en kort när toocomplete.</span><span class="sxs-lookup"><span data-stu-id="778a3-227">Creating a connection takes a short while toocomplete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. <span data-ttu-id="778a3-228">Skapa hello TestVNet4 tooTestVNet1 anslutning.</span><span class="sxs-lookup"><span data-stu-id="778a3-228">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="778a3-229">Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet4 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="778a3-229">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="778a3-230">Se till att matcha hello delade nycklar.</span><span class="sxs-lookup"><span data-stu-id="778a3-230">Make sure hello shared keys match.</span></span> <span data-ttu-id="778a3-231">Det tar några minuter tooestablish hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="778a3-231">It takes a few minutes tooestablish hello connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. <span data-ttu-id="778a3-232">Verifiera dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="778a3-232">Verify your connections.</span></span> <span data-ttu-id="778a3-233">Se [Verifiera din anslutning](#verify).</span><span class="sxs-lookup"><span data-stu-id="778a3-233">See [Verify your connection](#verify).</span></span>

### <span data-ttu-id="778a3-234"><a name="samerg"></a>tooconnect Vnet som finns i hello samma resursgrupp</span><span class="sxs-lookup"><span data-stu-id="778a3-234"><a name="samerg"></a>tooconnect VNets that reside in hello same resource group</span></span>

1. <span data-ttu-id="778a3-235">Skapa hello TestVNet1 tooTestVNet4 anslutning.</span><span class="sxs-lookup"><span data-stu-id="778a3-235">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="778a3-236">I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet4.</span><span class="sxs-lookup"><span data-stu-id="778a3-236">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="778a3-237">Meddelande hello resursgrupper är hello samma i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="778a3-237">Notice hello resource groups are hello same in hello examples.</span></span> <span data-ttu-id="778a3-238">Du kan också se en delad nyckel som refereras i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="778a3-238">You also see a shared key referenced in hello examples.</span></span> <span data-ttu-id="778a3-239">Du kan använda egna värden för hello delad nyckel, men hello delad nyckel måste matcha för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="778a3-239">You can use your own values for hello shared key, however, hello shared key must match for both connections.</span></span> <span data-ttu-id="778a3-240">Skapa en anslutning tar en kort när toocomplete.</span><span class="sxs-lookup"><span data-stu-id="778a3-240">Creating a connection takes a short while toocomplete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. <span data-ttu-id="778a3-241">Skapa hello TestVNet4 tooTestVNet1 anslutning.</span><span class="sxs-lookup"><span data-stu-id="778a3-241">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="778a3-242">Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet4 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="778a3-242">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="778a3-243">Se till att matcha hello delade nycklar.</span><span class="sxs-lookup"><span data-stu-id="778a3-243">Make sure hello shared keys match.</span></span> <span data-ttu-id="778a3-244">Det tar några minuter tooestablish hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="778a3-244">It takes a few minutes tooestablish hello connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. <span data-ttu-id="778a3-245">Verifiera dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="778a3-245">Verify your connections.</span></span> <span data-ttu-id="778a3-246">Se [Verifiera din anslutning](#verify).</span><span class="sxs-lookup"><span data-stu-id="778a3-246">See [Verify your connection](#verify).</span></span>

## <span data-ttu-id="778a3-247"><a name="difsub"></a>Ansluta VNets som finns i olika prenumerationer</span><span class="sxs-lookup"><span data-stu-id="778a3-247"><a name="difsub"></a>Connect VNets that are in different subscriptions</span></span>

![v2v-diagram](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

<span data-ttu-id="778a3-249">I det här scenariot ansluter vi TestVNet1 och TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="778a3-249">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="778a3-250">Hej Vnet finnas olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="778a3-250">hello VNets reside different subscriptions.</span></span> <span data-ttu-id="778a3-251">hello prenumerationer behöver inte toobe som är associerade med hello samma Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="778a3-251">hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> <span data-ttu-id="778a3-252">hello steg för den här konfigurationen kan du lägga till ytterligare VNet-till-VNet en anslutning i ordning tooconnect TestVNet1 tooTestVNet5.</span><span class="sxs-lookup"><span data-stu-id="778a3-252">hello steps for this configuration add an additional VNet-to-VNet connection in order tooconnect TestVNet1 tooTestVNet5.</span></span>

### <span data-ttu-id="778a3-253"><a name="TestVNet1diff"></a>Steg 5 – Skapa och konfigurera TestVNet1</span><span class="sxs-lookup"><span data-stu-id="778a3-253"><a name="TestVNet1diff"></a>Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="778a3-254">Dessa instruktioner fortsätta från hello stegen i föregående avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="778a3-254">These instructions continue from hello steps in hello preceding sections.</span></span> <span data-ttu-id="778a3-255">Du måste slutföra [steg 1](#Connect) och [steg 2](#TestVNet1) toocreate TestVNet1 och konfigurera hello VPN-Gateway för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="778a3-255">You must complete [Step 1](#Connect) and [Step 2](#TestVNet1) toocreate and configure TestVNet1 and hello VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="778a3-256">För den här konfigurationen är du inte nödvändiga toocreate TestVNet4 från hello föregående avsnitt, men om du skapar det, kommer det står i konflikt med de här stegen.</span><span class="sxs-lookup"><span data-stu-id="778a3-256">For this configuration, you are not required toocreate TestVNet4 from hello previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="778a3-257">När du har slutfört steg 1 och steg 2 fortsätter du med steg 6 (nedan).</span><span class="sxs-lookup"><span data-stu-id="778a3-257">Once you complete Step 1 and Step 2, continue with Step 6 (below).</span></span>

### <span data-ttu-id="778a3-258"><a name="verifyranges"></a>Steg 6 – verifiera hello IP-adressintervall</span><span class="sxs-lookup"><span data-stu-id="778a3-258"><a name="verifyranges"></a>Step 6 - Verify hello IP address ranges</span></span>

<span data-ttu-id="778a3-259">När du skapar ytterligare anslutningar är det viktigt tooverify som inte överlappar hello IP-adressutrymme hello nytt virtuellt nätverk med någon av dina VNet intervall eller lokala nätverksintervall för gateway.</span><span class="sxs-lookup"><span data-stu-id="778a3-259">When creating additional connections, it's important tooverify that hello IP address space of hello new virtual network does not overlap with any of your other VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="778a3-260">Du kan använda följande värden för hello TestVNet5 hello i den här övningen:</span><span class="sxs-lookup"><span data-stu-id="778a3-260">For this exercise, you can use hello following values for hello TestVNet5:</span></span>

<span data-ttu-id="778a3-261">**Värden för TestVNet5:**</span><span class="sxs-lookup"><span data-stu-id="778a3-261">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="778a3-262">VNet-namn: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="778a3-262">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="778a3-263">Resursgrupp: TestRG5</span><span class="sxs-lookup"><span data-stu-id="778a3-263">Resource Group: TestRG5</span></span>
* <span data-ttu-id="778a3-264">Plats: Östra Japan</span><span class="sxs-lookup"><span data-stu-id="778a3-264">Location: Japan East</span></span>
* <span data-ttu-id="778a3-265">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="778a3-265">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="778a3-266">FrontEnd: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="778a3-266">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="778a3-267">BackEnd: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="778a3-267">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="778a3-268">GatewaySubnet: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="778a3-268">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="778a3-269">GatewayName: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="778a3-269">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="778a3-270">Offentlig IP: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="778a3-270">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="778a3-271">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="778a3-271">VPNType: RouteBased</span></span>
* <span data-ttu-id="778a3-272">Anslutning: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="778a3-272">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="778a3-273">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="778a3-273">ConnectionType: VNet2VNet</span></span>

### <span data-ttu-id="778a3-274"><a name="TestVNet5"></a>Steg 7 – Skapa och konfigurera TestVNet5</span><span class="sxs-lookup"><span data-stu-id="778a3-274"><a name="TestVNet5"></a>Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="778a3-275">Det här steget måste utföras i hello kontext hello ny prenumeration, prenumeration 5.</span><span class="sxs-lookup"><span data-stu-id="778a3-275">This step must be done in hello context of hello new subscription, Subscription 5.</span></span> <span data-ttu-id="778a3-276">Den här delen kan utföras av Hej administratör i en annan organisation som äger hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="778a3-276">This part may be performed by hello administrator in a different organization that owns hello subscription.</span></span> <span data-ttu-id="778a3-277">tooswitch mellan prenumerationer Använd ' az listan över--alla ' toolist hello prenumerationer tillgängliga tooyour konto och sedan använda ' az inställt--prenumeration <subscriptionID>' tooswitch toohello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="778a3-277">tooswitch between subscriptions use 'az account list --all' toolist hello subscriptions available tooyour account, then use 'az account set --subscription <subscriptionID>' tooswitch toohello subscription that you want toouse.</span></span>

1. <span data-ttu-id="778a3-278">Kontrollera att du är ansluten tooSubscription 5 och sedan skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="778a3-278">Make sure you are connected tooSubscription 5, then create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. <span data-ttu-id="778a3-279">Skapa TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="778a3-279">Create TestVNet5.</span></span>

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. <span data-ttu-id="778a3-280">Lägg till undernät.</span><span class="sxs-lookup"><span data-stu-id="778a3-280">Add subnets.</span></span>

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. <span data-ttu-id="778a3-281">Lägg till hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="778a3-281">Add hello gateway subnet.</span></span>

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. <span data-ttu-id="778a3-282">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="778a3-282">Request a public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. <span data-ttu-id="778a3-283">Skapa hello TestVNet5 gateway</span><span class="sxs-lookup"><span data-stu-id="778a3-283">Create hello TestVNet5 gateway</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="778a3-284"><a name="connections5"></a>Steg 8 – skapa hello-anslutningar</span><span class="sxs-lookup"><span data-stu-id="778a3-284"><a name="connections5"></a>Step 8 - Create hello connections</span></span>

<span data-ttu-id="778a3-285">Vi dela det här steget i två CLI-sessioner som markerats som **[prenumeration 1]**, och **[prenumeration 5]** eftersom hello gateways hello olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="778a3-285">We split this step into two CLI sessions marked as **[Subscription 1]**, and **[Subscription 5]** because hello gateways are in hello different subscriptions.</span></span> <span data-ttu-id="778a3-286">tooswitch mellan prenumerationer Använd ' az listan över--alla ' toolist hello prenumerationer tillgängliga tooyour konto och sedan använda ' az inställt--prenumeration <subscriptionID>' tooswitch toohello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="778a3-286">tooswitch between subscriptions use 'az account list --all' toolist hello subscriptions available tooyour account, then use 'az account set --subscription <subscriptionID>' tooswitch toohello subscription that you want toouse.</span></span>

1. <span data-ttu-id="778a3-287">**[Prenumeration 1]**  Logga in och ansluta tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="778a3-287">**[Subscription 1]** Log in and connect tooSubscription 1.</span></span> <span data-ttu-id="778a3-288">Hello kör följande kommando tooget hello namn och ID för hello Gateway från hello utdata:</span><span class="sxs-lookup"><span data-stu-id="778a3-288">Run hello following command tooget hello name and ID of hello Gateway from hello output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="778a3-289">Kopiera hello utdata för ”id”:.</span><span class="sxs-lookup"><span data-stu-id="778a3-289">Copy hello output for "id:".</span></span> <span data-ttu-id="778a3-290">Skicka hello-ID och hello namnet på hello virtuella nätverk (VNet1GW) toohello gatewayadministratören prenumeration 5 via e-post eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="778a3-290">Send hello ID and hello name of hello VNet gateway (VNet1GW) toohello administrator of Subscription 5 via email or another method.</span></span>

  <span data-ttu-id="778a3-291">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="778a3-291">Example output:</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. <span data-ttu-id="778a3-292">**[Prenumerationen 5]**  Logga in och ansluta tooSubscription 5.</span><span class="sxs-lookup"><span data-stu-id="778a3-292">**[Subscription 5]** Log in and connect tooSubscription 5.</span></span> <span data-ttu-id="778a3-293">Hello kör följande kommando tooget hello namn och ID för hello Gateway från hello utdata:</span><span class="sxs-lookup"><span data-stu-id="778a3-293">Run hello following command tooget hello name and ID of hello Gateway from hello output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  <span data-ttu-id="778a3-294">Kopiera hello utdata för ”id”:.</span><span class="sxs-lookup"><span data-stu-id="778a3-294">Copy hello output for "id:".</span></span> <span data-ttu-id="778a3-295">Skicka hello-ID och hello namnet på hello virtuella nätverk (VNet5GW) toohello gatewayadministratören prenumeration 1 via e-post eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="778a3-295">Send hello ID and hello name of hello VNet gateway (VNet5GW) toohello administrator of Subscription 1 via email or another method.</span></span>

3. <span data-ttu-id="778a3-296">**[Prenumeration 1]**  i det här steget kan du skapa hello anslutning från TestVNet1 tooTestVNet5.</span><span class="sxs-lookup"><span data-stu-id="778a3-296">**[Subscription 1]** In this step, you create hello connection from TestVNet1 tooTestVNet5.</span></span> <span data-ttu-id="778a3-297">Du kan använda egna värden för hello delad nyckel, men hello delad nyckel måste matcha för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="778a3-297">You can use your own values for hello shared key, however, hello shared key must match for both connections.</span></span> <span data-ttu-id="778a3-298">Skapa en anslutning kan ta en kort när toocomplete.</span><span class="sxs-lookup"><span data-stu-id="778a3-298">Creating a connection can take a short while toocomplete.</span></span> <span data-ttu-id="778a3-299">Kontrollera att du ansluter tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="778a3-299">Make sure you connect tooSubscription 1.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. <span data-ttu-id="778a3-300">**[Prenumerationen 5]**  Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet5 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="778a3-300">**[Subscription 5]** This step is similar toohello one above, except you are creating hello connection from TestVNet5 tooTestVNet1.</span></span> <span data-ttu-id="778a3-301">Kontrollera att hello delade nycklar matchar och du ansluter tooSubscription 5.</span><span class="sxs-lookup"><span data-stu-id="778a3-301">Make sure that hello shared keys match and that you connect tooSubscription 5.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <span data-ttu-id="778a3-302"><a name="verify"></a>Kontrollera hello-anslutningar</span><span class="sxs-lookup"><span data-stu-id="778a3-302"><a name="verify"></a>Verify hello connections</span></span>
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <span data-ttu-id="778a3-303"><a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet</span><span class="sxs-lookup"><span data-stu-id="778a3-303"><a name="faq"></a>VNet-to-VNet FAQ</span></span>
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="778a3-304">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="778a3-304">Next steps</span></span>

* <span data-ttu-id="778a3-305">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="778a3-305">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="778a3-306">Mer information finns i hello [virtuella datorer dokumentationen](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="778a3-306">For more information, see hello [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="778a3-307">Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="778a3-307">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
