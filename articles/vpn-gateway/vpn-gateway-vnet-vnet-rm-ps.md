---
title: "Ansluta ett virtuellt Azure-nätverk tooanother VNet: PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 2da30c76867cc3f71d040e63e0dd15d153e15c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a><span data-ttu-id="daad5-103">Konfigurera en VPN-gatewayanslutning mellan virtuella nätverk med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="daad5-103">Configure a VNet-to-VNet VPN gateway connection using PowerShell</span></span>

<span data-ttu-id="daad5-104">Den här artikeln beskrivs hur du toocreate en VPN-gateway-anslutningen mellan virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="daad5-104">This article shows you how toocreate a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="daad5-105">hello virtuella nätverk kan vara i samma eller olika regioner hello och hello från samma eller olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="daad5-105">hello virtual networks can be in hello same or different regions, and from hello same or different subscriptions.</span></span> <span data-ttu-id="daad5-106">När du ansluter Vnet från olika prenumerationer, hello prenumerationer inte behöver toobe som är associerade med hello samma Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="daad5-106">When connecting VNets from different subscriptions, hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> 

<span data-ttu-id="daad5-107">hello stegen i den här artikeln gäller toohello Resource Manager-modellen och använda PowerShell.</span><span class="sxs-lookup"><span data-stu-id="daad5-107">hello steps in this article apply toohello Resource Manager deployment model and use PowerShell.</span></span> <span data-ttu-id="daad5-108">Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="daad5-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="daad5-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="daad5-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="daad5-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="daad5-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="daad5-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="daad5-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="daad5-112">Azure Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="daad5-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="daad5-113">Ansluta olika distributionsmodeller – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="daad5-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="daad5-114">Ansluta olika distributionsmodeller – PowerShell</span><span class="sxs-lookup"><span data-stu-id="daad5-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="daad5-115">Ansluta ett virtuellt nätverk tooanother virtuellt nätverk (VNet-till-VNet) är liknande tooconnecting ett VNet tooan lokal plats.</span><span class="sxs-lookup"><span data-stu-id="daad5-115">Connecting a virtual network tooanother virtual network (VNet-to-VNet) is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="daad5-116">Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="daad5-116">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="daad5-117">Om ditt Vnet i hello samma region som du kanske vill tooconsider ansluter dem med hjälp av VNet-Peering.</span><span class="sxs-lookup"><span data-stu-id="daad5-117">If your VNets are in hello same region, you may want tooconsider connecting them using VNet Peering.</span></span> <span data-ttu-id="daad5-118">Ingen VPN-gateway används för VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="daad5-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="daad5-119">Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="daad5-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="daad5-120">VNet-till-VNet-kommunikation kan kombineras med konfigurationer för flera platser.</span><span class="sxs-lookup"><span data-stu-id="daad5-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="daad5-121">På så sätt kan du etablera nätverkstopologier som kombinerar korsanslutningar med mellan virtuell nätverksanslutning som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="daad5-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in hello following diagram:</span></span>

![Om anslutningar](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a><span data-ttu-id="daad5-123">Varför ska man ansluta virtuella nätverk?</span><span class="sxs-lookup"><span data-stu-id="daad5-123">Why connect virtual networks?</span></span>

<span data-ttu-id="daad5-124">Du kanske vill tooconnect virtuella nätverk för hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="daad5-124">You may want tooconnect virtual networks for hello following reasons:</span></span>

* <span data-ttu-id="daad5-125">**Geografisk redundans i flera regioner och geografisk närvaro**</span><span class="sxs-lookup"><span data-stu-id="daad5-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="daad5-126">Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="daad5-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="daad5-127">Med Azure Traffic Manager och Load Balancer kan du konfigurera arbetsbelastning med hög tillgänglighet och geografisk redundans över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="daad5-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="daad5-128">Ett viktigt exempel är tooset in SQL Always On med Tillgänglighetsgrupper sprida över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="daad5-128">One important example is tooset up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="daad5-129">**Regionala flernivåprogram med isolering eller administrativa gränser**</span><span class="sxs-lookup"><span data-stu-id="daad5-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="daad5-130">Hej i samma region, du kan konfigurera flera nivåer program med flera virtuella nätverk som kopplar samman förfallodatum tooisolation eller administrativa krav.</span><span class="sxs-lookup"><span data-stu-id="daad5-130">Within hello same region, you can set up multi-tier applications with multiple virtual networks connected together due tooisolation or administrative requirements.</span></span>

<span data-ttu-id="daad5-131">Mer information om VNet-till-VNet-anslutningar finns hello [VNet-till-VNet vanliga frågor och svar](#faq) hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="daad5-131">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span>

## <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="daad5-132">Vilka steg ska jag använda?</span><span class="sxs-lookup"><span data-stu-id="daad5-132">Which set of steps should I use?</span></span>

<span data-ttu-id="daad5-133">I den här artikeln beskrivs två uppsättningar med steg.</span><span class="sxs-lookup"><span data-stu-id="daad5-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="daad5-134">En uppsättning steg för [Vnet som finns i hello samma prenumeration](#samesub), och en annan för [Vnet som finns i olika prenumerationer](#difsub).</span><span class="sxs-lookup"><span data-stu-id="daad5-134">One set of steps for [VNets that reside in hello same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span> <span data-ttu-id="daad5-135">hello viktigaste skillnaden mellan hello mängder är om du kan skapa och konfigurera alla virtuella nätverk och gateway resurser inom hello samma PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="daad5-135">hello key difference between hello sets is whether you can create and configure all virtual network and gateway resources within hello same PowerShell session.</span></span>

<span data-ttu-id="daad5-136">hello använder stegen i den här artikeln variabler som har deklarerats hello början av varje avsnitt.</span><span class="sxs-lookup"><span data-stu-id="daad5-136">hello steps in this article use variables that are declared at hello beginning of each section.</span></span> <span data-ttu-id="daad5-137">Om du redan arbetar med befintliga Vnet, kan du ändra hello variabler tooreflect hello inställningarna i din egen miljö.</span><span class="sxs-lookup"><span data-stu-id="daad5-137">If you already are working with existing VNets, modify hello variables tooreflect hello settings in your own environment.</span></span> <span data-ttu-id="daad5-138">Information om hur du använder namnmatchning för dina virtuella nätverk finns i [Namnmatchning](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="daad5-138">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

## <span data-ttu-id="daad5-139"><a name="samesub"></a>Hur tooconnect Vnet som används i hello samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="daad5-139"><a name="samesub"></a>How tooconnect VNets that are in hello same subscription</span></span>

![v2v-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="daad5-141">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="daad5-141">Before you begin</span></span>

<span data-ttu-id="daad5-142">Innan du börjar måste du tooinstall hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets, minst 4.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="daad5-142">Before beginning, you need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets, at least 4.0 or later.</span></span> <span data-ttu-id="daad5-143">Mer information om hur du installerar hello PowerShell-cmdlets finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="daad5-143">For more information about installing hello PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <span data-ttu-id="daad5-144"><a name="Step1"></a>Steg 1 – Planera dina IP-adressintervall</span><span class="sxs-lookup"><span data-stu-id="daad5-144"><a name="Step1"></a>Step 1 - Plan your IP address ranges</span></span>

<span data-ttu-id="daad5-145">I följande steg hello, skapar vi två virtuella nätverk samt deras respektive gateway-undernät och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="daad5-145">In hello following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="daad5-146">Vi skapar sedan en VPN-anslutning mellan hello två Vnet.</span><span class="sxs-lookup"><span data-stu-id="daad5-146">We then create a VPN connection between hello two VNets.</span></span> <span data-ttu-id="daad5-147">Det är viktigt tooplan hello IP-adressintervall för nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="daad5-147">It’s important tooplan hello IP address ranges for your network configuration.</span></span> <span data-ttu-id="daad5-148">Tänk på att inga av dina VNet-intervall eller lokala nätverksintervall får överlappa varandra på något sätt.</span><span class="sxs-lookup"><span data-stu-id="daad5-148">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="daad5-149">I de här exemplen tar vi inte med någon DNS-server.</span><span class="sxs-lookup"><span data-stu-id="daad5-149">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="daad5-150">Information om hur du använder namnmatchning för dina virtuella nätverk finns i [Namnmatchning](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="daad5-150">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="daad5-151">Vi använder följande värden i hello exempel hello:</span><span class="sxs-lookup"><span data-stu-id="daad5-151">We use hello following values in hello examples:</span></span>

<span data-ttu-id="daad5-152">**Värden för TestVNet1:**</span><span class="sxs-lookup"><span data-stu-id="daad5-152">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="daad5-153">VNet-namn: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="daad5-153">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="daad5-154">Resursgrupp: TestRG1</span><span class="sxs-lookup"><span data-stu-id="daad5-154">Resource Group: TestRG1</span></span>
* <span data-ttu-id="daad5-155">Plats: Östra USA</span><span class="sxs-lookup"><span data-stu-id="daad5-155">Location: East US</span></span>
* <span data-ttu-id="daad5-156">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="daad5-156">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="daad5-157">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="daad5-157">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="daad5-158">BackEnd: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="daad5-158">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="daad5-159">GatewaySubnet: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="daad5-159">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="daad5-160">GatewayName: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="daad5-160">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="daad5-161">Offentlig IP: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="daad5-161">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="daad5-162">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="daad5-162">VPNType: RouteBased</span></span>
* <span data-ttu-id="daad5-163">Connection(1to4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="daad5-163">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="daad5-164">Connection(1to5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="daad5-164">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="daad5-165">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="daad5-165">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="daad5-166">**Värden för TestVNet4:**</span><span class="sxs-lookup"><span data-stu-id="daad5-166">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="daad5-167">VNet-namn: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="daad5-167">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="daad5-168">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="daad5-168">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="daad5-169">FrontEnd: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="daad5-169">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="daad5-170">BackEnd: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="daad5-170">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="daad5-171">GatewaySubnet: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="daad5-171">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="daad5-172">Resursgrupp: TestRG4</span><span class="sxs-lookup"><span data-stu-id="daad5-172">Resource Group: TestRG4</span></span>
* <span data-ttu-id="daad5-173">Plats: Västra USA</span><span class="sxs-lookup"><span data-stu-id="daad5-173">Location: West US</span></span>
* <span data-ttu-id="daad5-174">GatewayName: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="daad5-174">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="daad5-175">Offentlig IP: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="daad5-175">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="daad5-176">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="daad5-176">VPNType: RouteBased</span></span>
* <span data-ttu-id="daad5-177">Anslutning: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="daad5-177">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="daad5-178">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="daad5-178">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="daad5-179"><a name="Step2"></a>Steg 2 – Skapa och konfigurera TestVNet1</span><span class="sxs-lookup"><span data-stu-id="daad5-179"><a name="Step2"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="daad5-180">Deklarera dina variabler.</span><span class="sxs-lookup"><span data-stu-id="daad5-180">Declare your variables.</span></span> <span data-ttu-id="daad5-181">Det här exemplet deklarerar hello variabler med hello värden för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="daad5-181">This example declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="daad5-182">I de flesta fall bör du ersätta hello värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="daad5-182">In most cases, you should replace hello values with your own.</span></span> <span data-ttu-id="daad5-183">Du kan använda dessa variabler om du kör via hello steg toobecome bekant med den här typen av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="daad5-183">However, you can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="daad5-184">Ändra hello variabler vid behov, och sedan kopiera och klistra in dem i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="daad5-184">Modify hello variables if needed, then copy and paste them into your PowerShell console.</span></span>

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

2. <span data-ttu-id="daad5-185">Ansluta tooyour-konto.</span><span class="sxs-lookup"><span data-stu-id="daad5-185">Connect tooyour account.</span></span> <span data-ttu-id="daad5-186">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="daad5-186">Use hello following example toohelp you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="daad5-187">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="daad5-187">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="daad5-188">Ange hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="daad5-188">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. <span data-ttu-id="daad5-189">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="daad5-189">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. <span data-ttu-id="daad5-190">Skapa hello undernät för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="daad5-190">Create hello subnet configurations for TestVNet1.</span></span> <span data-ttu-id="daad5-191">I det här exemplet skapas ett virtuellt nätverk med namnet TestVNet1 och tre undernät – GatewaySubnet, FrontEnd och BackEnd.</span><span class="sxs-lookup"><span data-stu-id="daad5-191">This example creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="daad5-192">När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="daad5-192">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="daad5-193">Om du ger det något annat namn går det inte att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="daad5-193">If you name it something else, your gateway creation fails.</span></span>

  <span data-ttu-id="daad5-194">hello används följande exempel hello variabler som du angett tidigare.</span><span class="sxs-lookup"><span data-stu-id="daad5-194">hello following example uses hello variables that you set earlier.</span></span> <span data-ttu-id="daad5-195">I det här exemplet hello gateway-undernätet med hjälp av en minst/27.</span><span class="sxs-lookup"><span data-stu-id="daad5-195">In this example, hello gateway subnet is using a /27.</span></span> <span data-ttu-id="daad5-196">Det är möjligt toocreate ett gatewayundernät så liten som /29, rekommenderar vi att du skapar ett större undernät som innehåller flera adresser genom att välja minst /28 eller minst/27.</span><span class="sxs-lookup"><span data-stu-id="daad5-196">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="daad5-197">Detta gör att tillräckligt många adresser tooaccommodate möjliga ytterligare konfigurationer som du vill i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="daad5-197">This will allow for enough addresses tooaccommodate possible additional configurations that you may want in hello future.</span></span>

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. <span data-ttu-id="daad5-198">Skapa TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="daad5-198">Create TestVNet1.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. <span data-ttu-id="daad5-199">Begär en offentlig IP-adress toobe allokerade toohello gateway skapas för ditt VNet.</span><span class="sxs-lookup"><span data-stu-id="daad5-199">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="daad5-200">Observera att hello AllocationMethod är dynamiska.</span><span class="sxs-lookup"><span data-stu-id="daad5-200">Notice that hello AllocationMethod is Dynamic.</span></span> <span data-ttu-id="daad5-201">Du kan inte ange hello IP-adress som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="daad5-201">You cannot specify hello IP address that you want toouse.</span></span> <span data-ttu-id="daad5-202">Det är dynamiskt allokerade tooyour gateway.</span><span class="sxs-lookup"><span data-stu-id="daad5-202">It's dynamically allocated tooyour gateway.</span></span> 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="daad5-203">Skapa hello gateway-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="daad5-203">Create hello gateway configuration.</span></span> <span data-ttu-id="daad5-204">gateway-konfiguration för hello definierar hello undernät och hello offentliga IP-adressen toouse.</span><span class="sxs-lookup"><span data-stu-id="daad5-204">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="daad5-205">Använd hello exempel toocreate gateway-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="daad5-205">Use hello example toocreate your gateway configuration.</span></span>

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. <span data-ttu-id="daad5-206">Skapa hello gateway för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="daad5-206">Create hello gateway for TestVNet1.</span></span> <span data-ttu-id="daad5-207">I det här steget skapar du hello virtuell nätverksgateway för din TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="daad5-207">In this step, you create hello virtual network gateway for your TestVNet1.</span></span> <span data-ttu-id="daad5-208">VNet-till-VNet-konfigurationer kräver VpnType RouteBased.</span><span class="sxs-lookup"><span data-stu-id="daad5-208">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="daad5-209">Skapa en gateway kan ofta ta 45 minuter eller mer beroende på hello markerad gateway SKU.</span><span class="sxs-lookup"><span data-stu-id="daad5-209">Creating a gateway can often take 45 minutes or more, depending on hello selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a><span data-ttu-id="daad5-210">Steg 3 – Skapa och konfigurera TestVNet4</span><span class="sxs-lookup"><span data-stu-id="daad5-210">Step 3 - Create and configure TestVNet4</span></span>

<span data-ttu-id="daad5-211">När du har konfigurerat TestVNet1 skapar du TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="daad5-211">Once you've configured TestVNet1, create TestVNet4.</span></span> <span data-ttu-id="daad5-212">Följ hello steg nedan, ersätter hello värden med dina egna vid behov.</span><span class="sxs-lookup"><span data-stu-id="daad5-212">Follow hello steps below, replacing hello values with your own when needed.</span></span> <span data-ttu-id="daad5-213">Det här steget kan utföras med hello samma PowerShell-session eftersom den är i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="daad5-213">This step can be done within hello same PowerShell session because it is in hello same subscription.</span></span>

1. <span data-ttu-id="daad5-214">Deklarera dina variabler.</span><span class="sxs-lookup"><span data-stu-id="daad5-214">Declare your variables.</span></span> <span data-ttu-id="daad5-215">Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="daad5-215">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

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
2. <span data-ttu-id="daad5-216">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="daad5-216">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. <span data-ttu-id="daad5-217">Skapa hello undernät för TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="daad5-217">Create hello subnet configurations for TestVNet4.</span></span>

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. <span data-ttu-id="daad5-218">Skapa TestVNet4.</span><span class="sxs-lookup"><span data-stu-id="daad5-218">Create TestVNet4.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. <span data-ttu-id="daad5-219">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="daad5-219">Request a public IP address.</span></span>

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. <span data-ttu-id="daad5-220">Skapa hello gateway-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="daad5-220">Create hello gateway configuration.</span></span>

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. <span data-ttu-id="daad5-221">Skapa hello TestVNet4 gateway.</span><span class="sxs-lookup"><span data-stu-id="daad5-221">Create hello TestVNet4 gateway.</span></span> <span data-ttu-id="daad5-222">Skapa en gateway kan ofta ta 45 minuter eller mer beroende på hello markerad gateway SKU.</span><span class="sxs-lookup"><span data-stu-id="daad5-222">Creating a gateway can often take 45 minutes or more, depending on hello selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-hello-connections"></a><span data-ttu-id="daad5-223">Steg 4 – skapa hello-anslutningar</span><span class="sxs-lookup"><span data-stu-id="daad5-223">Step 4 - Create hello connections</span></span>

1. <span data-ttu-id="daad5-224">Hämta båda virtuella nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="daad5-224">Get both virtual network gateways.</span></span> <span data-ttu-id="daad5-225">Om båda hello gatewayer finns i hello samma prenumeration, som i exemplet hello, kan du slutföra det här steget i hello samma PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="daad5-225">If both of hello gateways are in hello same subscription, as they are in hello example, you can complete this step in hello same PowerShell session.</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. <span data-ttu-id="daad5-226">Skapa hello TestVNet1 tooTestVNet4 anslutning.</span><span class="sxs-lookup"><span data-stu-id="daad5-226">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="daad5-227">I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet4.</span><span class="sxs-lookup"><span data-stu-id="daad5-227">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="daad5-228">Du ser en delad nyckel som refereras i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="daad5-228">You'll see a shared key referenced in hello examples.</span></span> <span data-ttu-id="daad5-229">Du kan använda egna värden för hello delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="daad5-229">You can use your own values for hello shared key.</span></span> <span data-ttu-id="daad5-230">hello viktig sak är att hello delad nyckel måste matcha för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="daad5-230">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="daad5-231">Skapa en anslutning kan ta en kort när toocomplete.</span><span class="sxs-lookup"><span data-stu-id="daad5-231">Creating a connection can take a short while toocomplete.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. <span data-ttu-id="daad5-232">Skapa hello TestVNet4 tooTestVNet1 anslutning.</span><span class="sxs-lookup"><span data-stu-id="daad5-232">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="daad5-233">Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet4 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="daad5-233">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="daad5-234">Se till att matcha hello delade nycklar.</span><span class="sxs-lookup"><span data-stu-id="daad5-234">Make sure hello shared keys match.</span></span> <span data-ttu-id="daad5-235">hello-anslutning upprättas efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="daad5-235">hello connection will be established after a few minutes.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="daad5-236">Verifiera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="daad5-236">Verify your connection.</span></span> <span data-ttu-id="daad5-237">Avsnittet hello [hur tooverify anslutningen](#verify).</span><span class="sxs-lookup"><span data-stu-id="daad5-237">See hello section [How tooverify your connection](#verify).</span></span>

## <span data-ttu-id="daad5-238"><a name="difsub"></a>Hur tooconnect Vnet som finns i olika prenumerationer</span><span class="sxs-lookup"><span data-stu-id="daad5-238"><a name="difsub"></a>How tooconnect VNets that are in different subscriptions</span></span>

![v2v-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

<span data-ttu-id="daad5-240">I det här scenariot ansluter vi TestVNet1 och TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="daad5-240">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="daad5-241">TestVNet1 och TestVNet5 finns i olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="daad5-241">TestVNet1 and TestVNet5 reside in a different subscription.</span></span> <span data-ttu-id="daad5-242">hello prenumerationer behöver inte toobe som är associerade med hello samma Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="daad5-242">hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> <span data-ttu-id="daad5-243">hello skillnaden mellan dessa steg och hello tidigare uppsättningen är att vissa av hello konfigurationssteg måste toobe utförs i en separat PowerShell-session i hello kontexten av andra hello-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="daad5-243">hello difference between these steps and hello previous set is that some of hello configuration steps need toobe performed in a separate PowerShell session in hello context of hello second subscription.</span></span> <span data-ttu-id="daad5-244">Särskilt när hello två prenumerationer hör toodifferent organisationer.</span><span class="sxs-lookup"><span data-stu-id="daad5-244">Especially when hello two subscriptions belong toodifferent organizations.</span></span>

### <a name="step-5---create-and-configure-testvnet1"></a><span data-ttu-id="daad5-245">Steg 5 – Skapa och konfigurera TestVNet1</span><span class="sxs-lookup"><span data-stu-id="daad5-245">Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="daad5-246">Du måste slutföra [steg 1](#Step1) och [steg 2](#Step2) från hello föregående avsnittet toocreate och konfigurera TestVNet1 hello VPN-Gateway för TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="daad5-246">You must complete [Step 1](#Step1) and [Step 2](#Step2) from hello previous section toocreate and configure TestVNet1 and hello VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="daad5-247">För den här konfigurationen är du inte nödvändiga toocreate TestVNet4 från hello föregående avsnitt, men om du skapar det, kommer det står i konflikt med de här stegen.</span><span class="sxs-lookup"><span data-stu-id="daad5-247">For this configuration, you are not required toocreate TestVNet4 from hello previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="daad5-248">När du har slutfört steg 1 och 2 fortsätter du med steg 6 toocreate TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="daad5-248">Once you complete Step 1 and Step 2, continue with Step 6 toocreate TestVNet5.</span></span> 

### <a name="step-6---verify-hello-ip-address-ranges"></a><span data-ttu-id="daad5-249">Steg 6 – verifiera hello IP-adressintervall</span><span class="sxs-lookup"><span data-stu-id="daad5-249">Step 6 - Verify hello IP address ranges</span></span>

<span data-ttu-id="daad5-250">Det är viktigt att att hello IP-adressutrymme hello nytt virtuellt nätverk, TestVNet5, inte överlappar med någon av dina VNet-adressintervall eller en lokal gateway nätverksintervall toomake.</span><span class="sxs-lookup"><span data-stu-id="daad5-250">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet5, does not overlap with any of your VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="daad5-251">I det här exemplet kan hello virtuella nätverk höra toodifferent organisationer.</span><span class="sxs-lookup"><span data-stu-id="daad5-251">In this example, hello virtual networks may belong toodifferent organizations.</span></span> <span data-ttu-id="daad5-252">Du kan använda följande värden för hello TestVNet5 hello i den här övningen:</span><span class="sxs-lookup"><span data-stu-id="daad5-252">For this exercise, you can use hello following values for hello TestVNet5:</span></span>

<span data-ttu-id="daad5-253">**Värden för TestVNet5:**</span><span class="sxs-lookup"><span data-stu-id="daad5-253">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="daad5-254">VNet-namn: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="daad5-254">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="daad5-255">Resursgrupp: TestRG5</span><span class="sxs-lookup"><span data-stu-id="daad5-255">Resource Group: TestRG5</span></span>
* <span data-ttu-id="daad5-256">Plats: Östra Japan</span><span class="sxs-lookup"><span data-stu-id="daad5-256">Location: Japan East</span></span>
* <span data-ttu-id="daad5-257">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="daad5-257">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="daad5-258">FrontEnd: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="daad5-258">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="daad5-259">BackEnd: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="daad5-259">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="daad5-260">GatewaySubnet: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="daad5-260">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="daad5-261">GatewayName: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="daad5-261">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="daad5-262">Offentlig IP: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="daad5-262">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="daad5-263">VPNType: RouteBased</span><span class="sxs-lookup"><span data-stu-id="daad5-263">VPNType: RouteBased</span></span>
* <span data-ttu-id="daad5-264">Anslutning: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="daad5-264">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="daad5-265">ConnectionType: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="daad5-265">ConnectionType: VNet2VNet</span></span>

### <a name="step-7---create-and-configure-testvnet5"></a><span data-ttu-id="daad5-266">Steg 7 – Skapa och konfigurera TestVNet5</span><span class="sxs-lookup"><span data-stu-id="daad5-266">Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="daad5-267">Det här steget måste utföras i hello kontext hello ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="daad5-267">This step must be done in hello context of hello new subscription.</span></span> <span data-ttu-id="daad5-268">Den här delen kan utföras av Hej administratör i en annan organisation som äger hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="daad5-268">This part may be performed by hello administrator in a different organization that owns hello subscription.</span></span>

1. <span data-ttu-id="daad5-269">Deklarera dina variabler.</span><span class="sxs-lookup"><span data-stu-id="daad5-269">Declare your variables.</span></span> <span data-ttu-id="daad5-270">Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="daad5-270">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

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
2. <span data-ttu-id="daad5-271">Ansluta toosubscription 5.</span><span class="sxs-lookup"><span data-stu-id="daad5-271">Connect toosubscription 5.</span></span> <span data-ttu-id="daad5-272">Öppna PowerShell-konsolen och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="daad5-272">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="daad5-273">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="daad5-273">Use hello following sample toohelp you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="daad5-274">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="daad5-274">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="daad5-275">Ange hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="daad5-275">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. <span data-ttu-id="daad5-276">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="daad5-276">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. <span data-ttu-id="daad5-277">Skapa hello undernät för TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="daad5-277">Create hello subnet configurations for TestVNet5.</span></span>

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. <span data-ttu-id="daad5-278">Skapa TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="daad5-278">Create TestVNet5.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. <span data-ttu-id="daad5-279">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="daad5-279">Request a public IP address.</span></span>

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="daad5-280">Skapa hello gateway-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="daad5-280">Create hello gateway configuration.</span></span>

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. <span data-ttu-id="daad5-281">Skapa hello TestVNet5 gateway.</span><span class="sxs-lookup"><span data-stu-id="daad5-281">Create hello TestVNet5 gateway.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-hello-connections"></a><span data-ttu-id="daad5-282">Steg 8 – skapa hello-anslutningar</span><span class="sxs-lookup"><span data-stu-id="daad5-282">Step 8 - Create hello connections</span></span>

<span data-ttu-id="daad5-283">I det här exemplet eftersom hello gateways hello olika prenumerationer, har vi dela det här steget i två PowerShell-sessioner som markerats som [prenumeration 1] och [prenumeration 5].</span><span class="sxs-lookup"><span data-stu-id="daad5-283">In this example, because hello gateways are in hello different subscriptions, we've split this step into two PowerShell sessions marked as [Subscription 1] and [Subscription 5].</span></span>

1. <span data-ttu-id="daad5-284">**[Prenumeration 1]**  Get hello virtuell nätverksgateway för prenumerationen 1.</span><span class="sxs-lookup"><span data-stu-id="daad5-284">**[Subscription 1]** Get hello virtual network gateway for Subscription 1.</span></span> <span data-ttu-id="daad5-285">Logga in och ansluta tooSubscription 1 innan du kör hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="daad5-285">Log in and connect tooSubscription 1 before running hello following example:</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  <span data-ttu-id="daad5-286">Kopiera hello utdata från hello följande element och skicka dessa toohello administratör för prenumeration 5 via e-post eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="daad5-286">Copy hello output of hello following elements and send these toohello administrator of Subscription 5 via email or another method.</span></span>

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  <span data-ttu-id="daad5-287">Dessa två element har värden liknande toohello följande exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="daad5-287">These two elements will have values similar toohello following example output:</span></span>

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. <span data-ttu-id="daad5-288">**[Prenumerationen 5]**  Get hello virtuell nätverksgateway för prenumerationen 5.</span><span class="sxs-lookup"><span data-stu-id="daad5-288">**[Subscription 5]** Get hello virtual network gateway for Subscription 5.</span></span> <span data-ttu-id="daad5-289">Logga in och ansluta tooSubscription 5 innan du kör hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="daad5-289">Log in and connect tooSubscription 5 before running hello following example:</span></span>

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  <span data-ttu-id="daad5-290">Kopiera hello utdata från hello följande element och skicka dessa toohello administratör för prenumeration 1 via e-post eller någon annan metod.</span><span class="sxs-lookup"><span data-stu-id="daad5-290">Copy hello output of hello following elements and send these toohello administrator of Subscription 1 via email or another method.</span></span>

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  <span data-ttu-id="daad5-291">Dessa två element har värden liknande toohello följande exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="daad5-291">These two elements will have values similar toohello following example output:</span></span>

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. <span data-ttu-id="daad5-292">**[Prenumeration 1]**  Skapa hello TestVNet1 tooTestVNet5 anslutning.</span><span class="sxs-lookup"><span data-stu-id="daad5-292">**[Subscription 1]** Create hello TestVNet1 tooTestVNet5 connection.</span></span> <span data-ttu-id="daad5-293">I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet5.</span><span class="sxs-lookup"><span data-stu-id="daad5-293">In this step, you create hello connection from TestVNet1 tooTestVNet5.</span></span> <span data-ttu-id="daad5-294">hello skillnaden här är den $vnet5gw inte går att hämta direkt eftersom den tillhör en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="daad5-294">hello difference here is that $vnet5gw cannot be obtained directly because it is in a different subscription.</span></span> <span data-ttu-id="daad5-295">Du behöver toocreate ett nytt PowerShell-objekt med hello värden kommunicerat från prenumerationen 1 i hello stegen ovan.</span><span class="sxs-lookup"><span data-stu-id="daad5-295">You will need toocreate a new PowerShell object with hello values communicated from Subscription 1 in hello steps above.</span></span> <span data-ttu-id="daad5-296">Använd hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="daad5-296">Use hello example below.</span></span> <span data-ttu-id="daad5-297">Ersätt hello namn-Id och delad nyckel med egna värden.</span><span class="sxs-lookup"><span data-stu-id="daad5-297">Replace hello Name, Id, and shared key with your own values.</span></span> <span data-ttu-id="daad5-298">hello viktig sak är att hello delad nyckel måste matcha för både anslutningar.</span><span class="sxs-lookup"><span data-stu-id="daad5-298">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="daad5-299">Skapa en anslutning kan ta en kort när toocomplete.</span><span class="sxs-lookup"><span data-stu-id="daad5-299">Creating a connection can take a short while toocomplete.</span></span>

  <span data-ttu-id="daad5-300">Anslut tooSubscription 1 innan du kör hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="daad5-300">Connect tooSubscription 1 before running hello following example:</span></span>

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="daad5-301">**[Prenumerationen 5]**  Skapa hello TestVNet5 tooTestVNet1 anslutning.</span><span class="sxs-lookup"><span data-stu-id="daad5-301">**[Subscription 5]** Create hello TestVNet5 tooTestVNet1 connection.</span></span> <span data-ttu-id="daad5-302">Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet5 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="daad5-302">This step is similar toohello one above, except you are creating hello connection from TestVNet5 tooTestVNet1.</span></span> <span data-ttu-id="daad5-303">hello samma process för att skapa ett PowerShell-objekt baserat på hello värden som hämtas från prenumerationen 1 gäller också.</span><span class="sxs-lookup"><span data-stu-id="daad5-303">hello same process of creating a PowerShell object based on hello values obtained from Subscription 1 applies here as well.</span></span> <span data-ttu-id="daad5-304">I det här steget Glöm inte att matcha hello delade nycklar.</span><span class="sxs-lookup"><span data-stu-id="daad5-304">In this step, be sure that hello shared keys match.</span></span>

  <span data-ttu-id="daad5-305">Anslut tooSubscription 5 innan du kör hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="daad5-305">Connect tooSubscription 5 before running hello following example:</span></span>

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <span data-ttu-id="daad5-306"><a name="verify"></a>Hur tooverify en anslutning</span><span class="sxs-lookup"><span data-stu-id="daad5-306"><a name="verify"></a>How tooverify a connection</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <span data-ttu-id="daad5-307"><a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet</span><span class="sxs-lookup"><span data-stu-id="daad5-307"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="daad5-308">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="daad5-308">Next steps</span></span>

* <span data-ttu-id="daad5-309">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="daad5-309">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="daad5-310">Se hello [virtuella datorer dokumentationen](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) för mer information.</span><span class="sxs-lookup"><span data-stu-id="daad5-310">See hello [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) for more information.</span></span>
* <span data-ttu-id="daad5-311">Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="daad5-311">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
