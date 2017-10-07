---
title: "aaaTroubleshoot vägar - PowerShell | Microsoft Docs"
description: "Lär dig hur tootroubleshoot vägar i hello Azure Resource Manager-distributionsmodellen med hjälp av Azure PowerShell."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 7a07806df5c1d0caee921187e6ad29f6755ab535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-routes-using-azure-powershell"></a><span data-ttu-id="fba25-103">Felsöka flöden med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fba25-103">Troubleshoot routes using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fba25-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fba25-104">Azure Portal</span></span>](virtual-network-routes-troubleshoot-portal.md)
> * [<span data-ttu-id="fba25-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fba25-105">PowerShell</span></span>](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="fba25-106">Om det uppstår problem tooor för network connectivity från din Azure virtuell dator (VM) kan vägar påverka din VM trafikflöden.</span><span class="sxs-lookup"><span data-stu-id="fba25-106">If you are experiencing network connectivity issues tooor from your Azure Virtual Machine (VM), routes may be impacting your VM traffic flows.</span></span> <span data-ttu-id="fba25-107">Den här artikeln innehåller en översikt över diagnostikfunktionerna för vägar toohelp fortsätta felsökningen.</span><span class="sxs-lookup"><span data-stu-id="fba25-107">This article provides an overview of diagnostics capabilities for routes toohelp troubleshoot further.</span></span>

<span data-ttu-id="fba25-108">Vägtabeller är kopplad till undernät och börjar gälla på alla nätverksgränssnitt (NIC) i det undernätet.</span><span class="sxs-lookup"><span data-stu-id="fba25-108">Route tables are associated with subnets and are effective on all network interfaces (NIC) in that subnet.</span></span> <span data-ttu-id="fba25-109">hello följande typer av routrar kan vara tillämpade tooeach nätverksgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="fba25-109">hello following types of routes can be applied tooeach network interface:</span></span>

* <span data-ttu-id="fba25-110">**Systemvägar:** varje undernät som skapats i ett Azure Virtual Network (VNet) har som standard system-routningstabeller som Tillåt lokal VNet-trafik, lokal trafik via VPN-gatewayer och Internet-trafik.</span><span class="sxs-lookup"><span data-stu-id="fba25-110">**System routes:** By default, every subnet created in an Azure Virtual Network (VNet) has system route tables that allow local VNet traffic, on-premises traffic via VPN gateways, and Internet traffic.</span></span> <span data-ttu-id="fba25-111">Det finns också systemvägar för peerkoppla Vnet.</span><span class="sxs-lookup"><span data-stu-id="fba25-111">System routes also exist for peered VNets.</span></span>
* <span data-ttu-id="fba25-112">**BGP-vägar:** sprids toonetwork gränssnitt via ExpressRoute eller plats-till-plats VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="fba25-112">**BGP routes:** Propagated toonetwork interfaces through ExpressRoute or site-to-site VPN connections.</span></span> <span data-ttu-id="fba25-113">Mer information om BGP-routning genom att läsa hello [BGP med VPN-gatewayer](../vpn-gateway/vpn-gateway-bgp-overview.md) och [ExpressRoute översikt](../expressroute/expressroute-introduction.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="fba25-113">Learn more about BGP routing by reading hello [BGP with VPN gateways](../vpn-gateway/vpn-gateway-bgp-overview.md) and [ExpressRoute overview](../expressroute/expressroute-introduction.md) articles.</span></span>
* <span data-ttu-id="fba25-114">**Användardefinierade vägar (UDR):** om du använder virtuella installationer i nätverket eller är Tvingad tunneltrafik trafik tooan lokalt nätverk via ett plats-till-plats-VPN, kan du ha användardefinierade vägar (udr: er) som är associerade med din routningstabellen för undernätet.</span><span class="sxs-lookup"><span data-stu-id="fba25-114">**User-defined routes (UDR):** If you are using network virtual appliances or are forced-tunneling traffic tooan on-premises network via a site-to-site VPN, you may have user-defined routes (UDRs) associated with your subnet route table.</span></span> <span data-ttu-id="fba25-115">Om du inte är bekant med udr: er kan läsa hello [användardefinierade vägar](virtual-networks-udr-overview.md#user-defined-routes) artikel.</span><span class="sxs-lookup"><span data-stu-id="fba25-115">If you're not familiar with UDRs, read hello [user-defined routes](virtual-networks-udr-overview.md#user-defined-routes) article.</span></span>

<span data-ttu-id="fba25-116">Med hello olika vägar som kan vara tillämpade tooa nätverksgränssnitt, det kan vara svårt toodetermine vilka sammanställd vägar är effektiva.</span><span class="sxs-lookup"><span data-stu-id="fba25-116">With hello various routes that can be applied tooa network interface, it can be difficult toodetermine which aggregate routes are effective.</span></span> <span data-ttu-id="fba25-117">toohelp felsöka anslutningar för VM-nätverk kan du visa alla hello effektiva vägar för ett nätverksgränssnitt i hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="fba25-117">toohelp troubleshoot VM network connectivity, you can view all hello effective routes for a network interface in hello Azure Resource Manager deployment model.</span></span>

## <a name="using-effective-routes-tootroubleshoot-vm-traffic-flow"></a><span data-ttu-id="fba25-118">Med hjälp av effektiva vägar tootroubleshoot VM trafikflöde</span><span class="sxs-lookup"><span data-stu-id="fba25-118">Using Effective Routes tootroubleshoot VM traffic flow</span></span>
<span data-ttu-id="fba25-119">Den här artikeln använder hello följande scenario som ett exempel tooillustrate hur tootroubleshoot hello gällande vägar för ett nätverksgränssnitt:</span><span class="sxs-lookup"><span data-stu-id="fba25-119">This article uses hello following scenario as an example tooillustrate how tootroubleshoot hello effective routes for a network interface:</span></span>

<span data-ttu-id="fba25-120">En virtuell dator (*VM1*) ansluten toohello VNet (*VNet1*, prefix: 10.9.0.0/16) misslyckas tooconnect tooa VM(VM3) i ett nyligen peered VNet (*VNet3*, prefixet 10.10.0.0/16).</span><span class="sxs-lookup"><span data-stu-id="fba25-120">A VM (*VM1*) connected toohello VNet (*VNet1*, prefix: 10.9.0.0/16) fails tooconnect tooa VM(VM3) in a newly peered VNet (*VNet3*, prefix 10.10.0.0/16).</span></span> <span data-ttu-id="fba25-121">Det finns inga udr: er eller BGP-vägar tillämpade tooVM1 NIC1 nätverk gränssnitt som är anslutet toohello VM, endast systemvägar tillämpas.</span><span class="sxs-lookup"><span data-stu-id="fba25-121">There are no UDRs or BGP routes applied tooVM1-NIC1 network interface connected toohello VM, only system routes are applied.</span></span>

<span data-ttu-id="fba25-122">Den här artikeln förklarar hur toodetermine hello orsaken till hello anslutningsfel med effektiva vägar funktionen i Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="fba25-122">This article explains how toodetermine hello cause of hello connection failure, using effective routes capability in Azure Resource Management deployment model.</span></span>
<span data-ttu-id="fba25-123">Medan hello exemplet används endast systemvägar, hello samma steg kan vara används toodetermine inkommande och utgående anslutningsfel via väg typer.</span><span class="sxs-lookup"><span data-stu-id="fba25-123">While hello example uses only system routes, hello same steps can be used toodetermine inbound and outbound connection failures over any route type.</span></span>

> [!NOTE]
> <span data-ttu-id="fba25-124">Om den virtuella datorn har mer än ett nätverkskort anslutet, kontrollerar du effektivt vägar för varje hello nätverkskort toodiagnose network connectivity problem tooand från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fba25-124">If your VM has more than one NIC attached, check effective routes for each of hello NICs toodiagnose network connectivity issues tooand from a VM.</span></span>
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a><span data-ttu-id="fba25-125">Visa effektiva flöden för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="fba25-125">View effective routes for a virtual machine</span></span>
<span data-ttu-id="fba25-126">toosee hello sammanställd vägar som är kopplade tooa VM, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fba25-126">toosee hello aggregate routes that are applied tooa VM, complete hello following steps:</span></span>

### <a name="view-effective-routes-for-a-network-interface"></a><span data-ttu-id="fba25-127">Visa effektiva vägar för ett nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="fba25-127">View effective routes for a network interface</span></span>
<span data-ttu-id="fba25-128">toosee hello sammanställd vägar som är tillämpade tooa nätverksgränssnitt, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fba25-128">toosee hello aggregate routes that are applied tooa network interface, complete hello following steps:</span></span>

1. <span data-ttu-id="fba25-129">Starta en Azure PowerShell-sessionen och logga in tooAzure.</span><span class="sxs-lookup"><span data-stu-id="fba25-129">Start an Azure PowerShell session and login tooAzure.</span></span> <span data-ttu-id="fba25-130">Om du inte är bekant med Azure PowerShell kan du läsa hello [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="fba25-130">If you’re not familiar with Azure PowerShell, read hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="fba25-131">hello följande kommando returnerar alla vägar tillämpas tooa gränssnitt med namnet *VM1 NIC1* i hello resursgruppen *RG1*.</span><span class="sxs-lookup"><span data-stu-id="fba25-131">hello following command returns all routes applied tooa network interface named *VM1-NIC1* in hello resource group *RG1*.</span></span>
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="fba25-132">Om du inte vet hello namnet på ett nätverksgränssnitt, Skriv följande kommando tooretrieve hello namnen på alla nätverksgränssnitt i en resurs group.* hello</span><span class="sxs-lookup"><span data-stu-id="fba25-132">If you don’t know hello name of a network interface, type hello following command tooretrieve hello names of all network interfaces in a resource group.*</span></span>
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   <span data-ttu-id="fba25-133">hello följande utdata ser ut ungefär toohello utdata för varje flöde tillämpas toohello undernät hello nätverkskortet är anslutet till:</span><span class="sxs-lookup"><span data-stu-id="fba25-133">hello following output looks similar toohello output for each route applied toohello subnet hello NIC is connected to:</span></span>
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   <span data-ttu-id="fba25-134">Observera följande hello i hello utdata:</span><span class="sxs-lookup"><span data-stu-id="fba25-134">Notice hello following in hello output:</span></span>
   
   * <span data-ttu-id="fba25-135">**Namnet**: hello effektiva vägen får vara tomt, om inte uttryckligen anges för användardefinierade vägar.</span><span class="sxs-lookup"><span data-stu-id="fba25-135">**Name**: Name of hello effective route may be empty, unless explicitly specified, for user-defined routes.</span></span> 
   * <span data-ttu-id="fba25-136">**Tillstånd**: Visar hello effektiva vägen.</span><span class="sxs-lookup"><span data-stu-id="fba25-136">**State**: Indicates state of hello effective route.</span></span> <span data-ttu-id="fba25-137">Möjliga värden är ”aktiv” eller ”ogiltig”</span><span class="sxs-lookup"><span data-stu-id="fba25-137">Possible values are "Active" or "Invalid"</span></span>
   * <span data-ttu-id="fba25-138">**Matrisen AddressPrefixes**: Anger hello-adressprefix för hello effektiva vägen i CIDR-notering.</span><span class="sxs-lookup"><span data-stu-id="fba25-138">**AddressPrefixes**: Specifies hello address prefix of hello effective route in CIDR notation.</span></span> 
   * <span data-ttu-id="fba25-139">**nextHopType**: Anger hello nästa hopp för hello angivna vägen.</span><span class="sxs-lookup"><span data-stu-id="fba25-139">**nextHopType**: Indicates hello next hop for hello given route.</span></span> <span data-ttu-id="fba25-140">Möjliga värden är *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, eller *Null*.</span><span class="sxs-lookup"><span data-stu-id="fba25-140">Possible values are *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, or *Null*.</span></span> <span data-ttu-id="fba25-141">Ett värde av *Null* för **nextHopType** i en UDR kan tyda på en ogiltig väg.</span><span class="sxs-lookup"><span data-stu-id="fba25-141">A value of *Null* for **nextHopType** in a UDR may indicate an invalid route.</span></span> <span data-ttu-id="fba25-142">Till exempel om **nextHopType** är *VirtualAppliance* och hello virtuell nätverksenhet virtuella datorn inte är i tillståndet etablerats/körs.</span><span class="sxs-lookup"><span data-stu-id="fba25-142">For example, if **nextHopType** is *VirtualAppliance* and hello network virtual appliance VM is not in a provisioned/running state.</span></span> <span data-ttu-id="fba25-143">Om **nextHopType** är *VPNGateway* och det finns ingen gateway etablerats/körs i hello anges VNet, hello flödet kan bli ogiltig.</span><span class="sxs-lookup"><span data-stu-id="fba25-143">If **nextHopType** is *VPNGateway* and there is no gateway provisioned/running in hello given VNet, hello route may become invalid.</span></span>
   * <span data-ttu-id="fba25-144">**NextHopIpAddress**: Anger hello IP-adress hello nästa hopp för hello effektiva vägen.</span><span class="sxs-lookup"><span data-stu-id="fba25-144">**NextHopIpAddress**: Specifies hello IP address of hello next hop of hello effective route.</span></span>
   
   <span data-ttu-id="fba25-145">hello returnerar följande kommando hello vägar i en enklare tooview tabell:</span><span class="sxs-lookup"><span data-stu-id="fba25-145">hello following command returns hello routes in an easier tooview table:</span></span>
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   <span data-ttu-id="fba25-146">hello är följande utdata några av hello utdata togs emot för hello-scenario som beskrivs ovan:</span><span class="sxs-lookup"><span data-stu-id="fba25-146">hello following output is some of hello output received for hello scenario described previously:</span></span>
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. <span data-ttu-id="fba25-147">Det finns ingen väg visas toohello *WestUS VNet3* VNet (prefixet 10.10.0.0/16)** från *WestUS VNet1* (Prefix 10.9.0.0/16) i hello utdata från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="fba25-147">There is no route listed toohello *WestUS-VNet3* VNet (Prefix 10.10.0.0/16)** from *WestUS-VNet1* (Prefix 10.9.0.0/16) in hello output from hello previous step.</span></span> <span data-ttu-id="fba25-148">I följande bild hello visas hello VNet-peering länken med hello *WestUS VNet3* VNet är i hello *frånkopplad* tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fba25-148">As shown in hello following picture, hello VNet peering link with hello *WestUS-VNet3* VNet is in hello *Disconnected* state.</span></span>
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    <span data-ttu-id="fba25-149">hello dubbelriktad länk för hello peering har brutits, som förklarar varför VM1 gick inte att ansluta tooVM3 i hello *WestUS VNet3* VNet.</span><span class="sxs-lookup"><span data-stu-id="fba25-149">hello bi-directional link for hello peering is broken, which explains why VM1 could not connect tooVM3 in hello *WestUS-VNet3* VNet.</span></span> <span data-ttu-id="fba25-150">Konfigurera en dubbelriktad VNet peering länk igen för *WestUS VNet1* och *WestUS VNet3* Vnet.</span><span class="sxs-lookup"><span data-stu-id="fba25-150">Setup a bi-directional VNet peering link again for *WestUS-VNet1* and *WestUS-VNet3* VNets.</span></span> <span data-ttu-id="fba25-151">hello utdatan när hello VNet-peering länken upprättas korrekt visas nedan:</span><span class="sxs-lookup"><span data-stu-id="fba25-151">hello output returned after hello VNet peering link is correctly established follows:</span></span>
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    <span data-ttu-id="fba25-152">När du bestämmer hello problemet kan du lägga till, ta bort, eller ändra vägar och routningstabeller.</span><span class="sxs-lookup"><span data-stu-id="fba25-152">Once you determine hello issue, you can add, remove, or change routes and route tables.</span></span> <span data-ttu-id="fba25-153">Skriv följande kommando toosee en lista över hello kommandon som används för toodo så hello:</span><span class="sxs-lookup"><span data-stu-id="fba25-153">Type hello following command toosee a list of hello commands used toodo so:</span></span>
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a><span data-ttu-id="fba25-154">Överväganden</span><span class="sxs-lookup"><span data-stu-id="fba25-154">Considerations</span></span>
<span data-ttu-id="fba25-155">Några saker tookeep i åtanke när du granskar hello lista över vägar som returnerades:</span><span class="sxs-lookup"><span data-stu-id="fba25-155">A few things tookeep in mind when reviewing hello list of routes returned:</span></span>

* <span data-ttu-id="fba25-156">Routning baserat på längsta Prefix-matchning (LPM) bland udr: er, systemet och BGP-vägar.</span><span class="sxs-lookup"><span data-stu-id="fba25-156">Routing is based on Longest Prefix Match (LPM) among UDRs, BGP and system routes.</span></span> <span data-ttu-id="fba25-157">Om det finns fler än en väg med hello samma LPM matchar, och sedan väljs en väg baserat på dess ursprung i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="fba25-157">If there is more than one route with hello same LPM match, then a route is selected based on its origin in hello following order:</span></span>
  
  * <span data-ttu-id="fba25-158">Användardefinierad väg</span><span class="sxs-lookup"><span data-stu-id="fba25-158">User-defined route</span></span>
  * <span data-ttu-id="fba25-159">BGP-väg</span><span class="sxs-lookup"><span data-stu-id="fba25-159">BGP route</span></span>
  * <span data-ttu-id="fba25-160">Systemväg (standard)</span><span class="sxs-lookup"><span data-stu-id="fba25-160">System (Default) route</span></span>
    
    <span data-ttu-id="fba25-161">Du kan bara se effektiva vägar som är baserat på alla hello tillgängliga vägar LPM-matchning med effektiva vägar.</span><span class="sxs-lookup"><span data-stu-id="fba25-161">With effective routes, you can only see effective routes that are LPM match based on all hello availble routes.</span></span> <span data-ttu-id="fba25-162">Genom att visa hur hello vägar faktiskt utvärderas till ett visst nätverkskort, blir det mycket enklare tootroubleshoot vägar driftstörningar anslutning till eller från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fba25-162">By showing how hello routes are actually evaluated for a given NIC, this makes it a lot easier tootroubleshoot specific routes that may be impacting connectivity to/from your VM.</span></span>
* <span data-ttu-id="fba25-163">Om du har udr: er och skickar trafik tooa virtuell nätverksenhet (NVA) med *VirtualAppliance* som **nextHopType**, kontrollera att IP-vidarebefordring är aktiverat på hello NVA mottagande hello trafik eller paket tappas.</span><span class="sxs-lookup"><span data-stu-id="fba25-163">If you have UDRs and are sending traffic tooa network virtual appliance (NVA), with *VirtualAppliance* as **nextHopType**, ensure that IP forwarding is enabled on hello NVA receiving hello traffic or packets are dropped.</span></span> 
* <span data-ttu-id="fba25-164">Om framtvingad tunneltrafik aktiveras, blir utgående Internet-trafik dirigeras tooon lokala.</span><span class="sxs-lookup"><span data-stu-id="fba25-164">If Forced tunneling is enabled, all outbound Internet traffic will be routed tooon-premises.</span></span> <span data-ttu-id="fba25-165">RDP/SSH från Internet tooyour VM inte fungerar med den här inställningen, beroende på hur hello lokalt hanterar den här trafiken.</span><span class="sxs-lookup"><span data-stu-id="fba25-165">RDP/SSH from Internet tooyour VM may not work with this setting, depending on how hello on-premises handles this traffic.</span></span> 
  <span data-ttu-id="fba25-166">Tvingad tunneltrafik kan aktiveras:</span><span class="sxs-lookup"><span data-stu-id="fba25-166">Forced-tunneling can be enabled:</span></span>
  * <span data-ttu-id="fba25-167">Om du använder plats-till-plats-VPN genom att ange en användardefinierad väg (UDR) med nextHopType som VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="fba25-167">If using site-to-site VPN, by setting a user-defined route (UDR) with nextHopType as VPN Gateway</span></span>
  * <span data-ttu-id="fba25-168">Om en standardväg annonseras över BGP</span><span class="sxs-lookup"><span data-stu-id="fba25-168">If a default route is advertised over BGP</span></span>
* <span data-ttu-id="fba25-169">För VNet-peering trafik toowork korrekt en systemväg med **nextHopType** *VNetPeering* måste finnas för hello peerkoppla virtuella-prefix för intervallet.</span><span class="sxs-lookup"><span data-stu-id="fba25-169">For VNet peering traffic toowork correctly, a system route with **nextHopType** *VNetPeering* must exist for hello peered VNet’s prefix range.</span></span> <span data-ttu-id="fba25-170">Om en sådan väg finns inte och hello VNet-peering ut länken OK:</span><span class="sxs-lookup"><span data-stu-id="fba25-170">If such a route doesn’t exist and hello VNet peering link looks OK:</span></span>
  * <span data-ttu-id="fba25-171">Vänta några sekunder och försök igen om det är en nyetablerade peering länk.</span><span class="sxs-lookup"><span data-stu-id="fba25-171">Wait a few seconds and retry if it's a newly established peering link.</span></span> <span data-ttu-id="fba25-172">Ibland tar det längre toopropagate vägar tooall hello-nätverksgränssnitt i ett undernät.</span><span class="sxs-lookup"><span data-stu-id="fba25-172">It occasionally takes longer toopropagate routes tooall hello network interfaces in a subnet.</span></span>
  * <span data-ttu-id="fba25-173">Nätverkssäkerhetsgrupp (NSG) regler kan påverka hello trafikflöden.</span><span class="sxs-lookup"><span data-stu-id="fba25-173">Network Security Group (NSG) rules may be impacting hello traffic flows.</span></span> <span data-ttu-id="fba25-174">Mer information finns i hello [felsöka Nätverkssäkerhetsgrupper](virtual-network-nsg-troubleshoot-powershell.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="fba25-174">For more information, see hello [Troubleshoot Network Security Groups](virtual-network-nsg-troubleshoot-powershell.md) article.</span></span>

