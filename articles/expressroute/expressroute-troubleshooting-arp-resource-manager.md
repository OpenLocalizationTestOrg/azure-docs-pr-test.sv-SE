---
title: "Hämtningen av ARP tabeller: hanteraren för filserverresurser: Azure ExpressRoute felsökning | Microsoft Docs"
description: "Den här sidan innehåller instruktioner om hämtning hello ARP tabeller för en ExpressRoute-krets"
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: 0a6bf1d5-6baf-44dd-87d3-1ebd2fd08bdc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: c386b031814d40ef6ea3ce5e0eaaab9634470e8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-arp-tables-in-hello-resource-manager-deployment-model"></a><span data-ttu-id="9e696-103">Hämtningen av ARP tabeller i hello Resource Manager-distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="9e696-103">Getting ARP tables in hello Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9e696-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9e696-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="9e696-105">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="9e696-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="9e696-106">Den här artikeln vägleder dig genom hello steg toolearn hello ARP tabeller för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="9e696-106">This article walks you through hello steps toolearn hello ARP tables for your ExpressRoute circuit.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="9e696-107">Det här dokumentet är avsett toohelp du diagnostisera och åtgärda problem med enkla.</span><span class="sxs-lookup"><span data-stu-id="9e696-107">This document is intended toohelp you diagnose and fix simple issues.</span></span> <span data-ttu-id="9e696-108">Det är inte avsedda toobe en ersättning för Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="9e696-108">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="9e696-109">Du måste öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om du toosolve hello problemet med hjälp av hello riktlinjer som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="9e696-109">You must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are unable toosolve hello problem using hello guidance described below.</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="9e696-110">Åtgärda ARP (Resolution Protocol) och ARP-tabeller</span><span class="sxs-lookup"><span data-stu-id="9e696-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="9e696-111">Protokollet ARP (Address Resolution) är en nivå 2-protokollet som definieras i [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="9e696-111">Address Resolution Protocol (ARP) is a layer 2 protocol defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="9e696-112">ARP kan använda toomap hello Ethernet-adress (MAC-adress) med en ip-adress.</span><span class="sxs-lookup"><span data-stu-id="9e696-112">ARP is used toomap hello Ethernet address (MAC address) with an ip address.</span></span>

<span data-ttu-id="9e696-113">hello ARP-tabell ger en mappning av hello ipv4-adress och MAC-adress för en viss peering.</span><span class="sxs-lookup"><span data-stu-id="9e696-113">hello ARP table provides a mapping of hello ipv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="9e696-114">hello ARP-tabell för en ExpressRoute-krets peering ger hello följande information för varje gränssnitt (primär eller sekundär)</span><span class="sxs-lookup"><span data-stu-id="9e696-114">hello ARP table for an ExpressRoute circuit peering provides hello following information for each interface (primary and secondary)</span></span>

1. <span data-ttu-id="9e696-115">Mappning av lokala router-gränssnittet IP-adress toohello MAC-adress</span><span class="sxs-lookup"><span data-stu-id="9e696-115">Mapping of on-premises router interface ip address toohello MAC address</span></span>
2. <span data-ttu-id="9e696-116">Mappning av ExpressRoute-router-gränssnittet IP-adress toohello MAC-adress</span><span class="sxs-lookup"><span data-stu-id="9e696-116">Mapping of ExpressRoute router interface ip address toohello MAC address</span></span>
3. <span data-ttu-id="9e696-117">Ålder hello-mappning</span><span class="sxs-lookup"><span data-stu-id="9e696-117">Age of hello mapping</span></span>

<span data-ttu-id="9e696-118">ARP-tabeller kan hjälpa dig att verifiera nivå 2-konfiguration och felsökning av grundläggande nivå 2 anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="9e696-118">ARP tables can help validate layer 2 configuration and troubleshooting basic layer 2 connectivity issues.</span></span> 

<span data-ttu-id="9e696-119">Exempel ARP-tabell:</span><span class="sxs-lookup"><span data-stu-id="9e696-119">Example ARP table:</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


<span data-ttu-id="9e696-120">hello följande avsnitt innehåller information om hur du visar hello ARP-tabeller som setts av hello ExpressRoute kant routrar.</span><span class="sxs-lookup"><span data-stu-id="9e696-120">hello following section provides information on how you can view hello ARP tables seen by hello ExpressRoute edge routers.</span></span> 

## <a name="prerequisites-for-learning-arp-tables"></a><span data-ttu-id="9e696-121">Krav för ARP-tabeller</span><span class="sxs-lookup"><span data-stu-id="9e696-121">Prerequisites for learning ARP tables</span></span>
<span data-ttu-id="9e696-122">Kontrollera att du har hello följande innan du fortskrider ytterligare</span><span class="sxs-lookup"><span data-stu-id="9e696-122">Ensure that you have hello following before you progress further</span></span>

* <span data-ttu-id="9e696-123">En giltig ExpressRoute-kretsen har konfigurerats med minst en peering.</span><span class="sxs-lookup"><span data-stu-id="9e696-123">A Valid ExpressRoute circuit configured with at least one peering.</span></span> <span data-ttu-id="9e696-124">hello krets måste konfigureras fullständigt av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="9e696-124">hello circuit must be fully configured by hello connectivity provider.</span></span> <span data-ttu-id="9e696-125">Du (eller anslutningsleverantören) måste ha konfigurerat minst en av hello peerkopplingar (Azure privat, Azure offentliga och Microsoft) på den här kretsen.</span><span class="sxs-lookup"><span data-stu-id="9e696-125">You (or your connectivity provider) must have configured at least one of hello peerings (Azure private, Azure public and Microsoft) on this circuit.</span></span>
* <span data-ttu-id="9e696-126">IP-adressintervall som används för att konfigurera hello peerkopplingar (Azure privat, Azure offentliga och Microsoft).</span><span class="sxs-lookup"><span data-stu-id="9e696-126">IP address ranges used for configuring hello peerings (Azure private, Azure public and Microsoft).</span></span> <span data-ttu-id="9e696-127">Granska hello IP-adresstilldelning exemplen i hello [ExpressRoute routning kravsidan](expressroute-routing.md) tooget en förståelse av hur ip-adresser är mappad toointerfaces på din sida och hello ExpressRoute sida.</span><span class="sxs-lookup"><span data-stu-id="9e696-127">Review hello ip address assignment examples in hello [ExpressRoute routing requirements page](expressroute-routing.md) tooget an understanding of how ip addresses are mapped toointerfaces on your side and on hello ExpressRoute side.</span></span> <span data-ttu-id="9e696-128">Du kan få information om hello peering konfiguration genom att granska hello [konfigurationssidan för ExpressRoute-peering](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="9e696-128">You can get information on hello peering configuration by reviewing hello [ExpressRoute peering configuration page](expressroute-howto-routing-arm.md).</span></span>
* <span data-ttu-id="9e696-129">Information från ditt nätverk team / anslutningen providern på hello MAC-adresser för gränssnitt som används med IP-adresserna.</span><span class="sxs-lookup"><span data-stu-id="9e696-129">Information from your networking team / connectivity provider on hello MAC addresses of interfaces used with these IP addresses.</span></span>
* <span data-ttu-id="9e696-130">Du måste ha hello senaste PowerShell-modulen för Azure (version 1,50 eller senare).</span><span class="sxs-lookup"><span data-stu-id="9e696-130">You must have hello latest PowerShell module for Azure (version 1.50 or newer).</span></span>

## <a name="getting-hello-arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="9e696-131">Hämta hello ARP tabeller för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="9e696-131">Getting hello ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="9e696-132">Det här avsnittet innehåller anvisningar om hur du visar hello ARP-tabeller per peering med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e696-132">This section provides instructions on how you can view hello ARP tables per peering using PowerShell.</span></span> <span data-ttu-id="9e696-133">Du eller anslutningsleverantören måste ha konfigurerat hello peering innan du kommer ytterligare.</span><span class="sxs-lookup"><span data-stu-id="9e696-133">You or your connectivity provider must have configured hello peering before progressing further.</span></span> <span data-ttu-id="9e696-134">Varje kretsen har två sökvägar (primär eller sekundär).</span><span class="sxs-lookup"><span data-stu-id="9e696-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="9e696-135">Du kan kontrollera hello ARP-tabell för varje sökväg oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="9e696-135">You can check hello ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="9e696-136">ARP-tabeller för privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="9e696-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="9e696-137">hello följande cmdlet ger hello ARP tabeller för privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="9e696-137">hello following cmdlet provides hello ARP tables for Azure private peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

<span data-ttu-id="9e696-138">Exempel på utdata nedan för en hello sökvägar</span><span class="sxs-lookup"><span data-stu-id="9e696-138">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="9e696-139">ARP-tabeller för offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="9e696-139">ARP tables for Azure public peering</span></span>
<span data-ttu-id="9e696-140">hello följande cmdlet ger hello ARP tabeller för offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="9e696-140">hello following cmdlet provides hello ARP tables for Azure public peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


<span data-ttu-id="9e696-141">Exempel på utdata nedan för en hello sökvägar</span><span class="sxs-lookup"><span data-stu-id="9e696-141">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="9e696-142">ARP-tabeller för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="9e696-142">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="9e696-143">hello följande cmdlet ger hello ARP tabeller för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="9e696-143">hello following cmdlet provides hello ARP tables for Microsoft peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


<span data-ttu-id="9e696-144">Exempel på utdata nedan för en hello sökvägar</span><span class="sxs-lookup"><span data-stu-id="9e696-144">Sample output is shown below for one of hello paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a><span data-ttu-id="9e696-145">Hur toouse informationen</span><span class="sxs-lookup"><span data-stu-id="9e696-145">How toouse this information</span></span>
<span data-ttu-id="9e696-146">hello ARP-tabell för en peering kan användas för toodetermine Validera nivå 2-konfiguration och anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9e696-146">hello ARP table of a peering can be used toodetermine validate layer 2 configuration and connectivity.</span></span> <span data-ttu-id="9e696-147">Det här avsnittet innehåller en översikt över hur ARP-tabeller ser ut under olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="9e696-147">This section provides an overview of how ARP tables will look under different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a><span data-ttu-id="9e696-148">När en anslutning har drifttillstånd (förväntade tillståndet) ARP-tabell</span><span class="sxs-lookup"><span data-stu-id="9e696-148">ARP table when a circuit is in operational state (expected state)</span></span>
* <span data-ttu-id="9e696-149">hello ARP-tabell har en post för hello lokalt sida med en giltig IP-adress och MAC-adress och en liknande post för hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="9e696-149">hello ARP table will have an entry for hello on-premises side with a valid IP address and MAC address and a similar entry for hello Microsoft side.</span></span> 
* <span data-ttu-id="9e696-150">hello sista oktetten hello lokala ip-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="9e696-150">hello last octet of hello on-premises ip address will always be an odd number.</span></span>
* <span data-ttu-id="9e696-151">hello sista oktetten av hello Microsoft IP-adress ska alltid vara ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="9e696-151">hello last octet of hello Microsoft ip address will always be an even number.</span></span>
* <span data-ttu-id="9e696-152">hello samma MAC-adress visas på hello Microsoft sida för alla 3 peerkopplingar (primära / sekundära).</span><span class="sxs-lookup"><span data-stu-id="9e696-152">hello same MAC address will appear on hello Microsoft side for all 3 peerings (primary / secondary).</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a><span data-ttu-id="9e696-153">ARP-tabellen när lokalt / anslutningen providern sida har problem</span><span class="sxs-lookup"><span data-stu-id="9e696-153">ARP table when on-premises / connectivity provider side has problems</span></span>
<span data-ttu-id="9e696-154">Om det finns problem med hello lokala eller anslutningen providern kan du se att antingen en enda post visas i hello ARP tabell eller hello lokal MAC-adress visas ofullständiga.</span><span class="sxs-lookup"><span data-stu-id="9e696-154">If there are issues with hello on-premises or connectivity provider you may see that either only one entry will appear in hello ARP table or hello on-prem MAC address will show incomplete.</span></span> <span data-ttu-id="9e696-155">Detta visar hello mappning mellan hello MAC-adress och IP-adress som används i hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="9e696-155">This will show hello mapping between hello MAC address and IP address used in hello Microsoft side.</span></span> 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

<span data-ttu-id="9e696-156">eller</span><span class="sxs-lookup"><span data-stu-id="9e696-156">or</span></span>
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> <span data-ttu-id="9e696-157">Öppna en supportbegäran med din anslutning providern toodebug sådana problem.</span><span class="sxs-lookup"><span data-stu-id="9e696-157">Open a support request with your connectivity provider toodebug such issues.</span></span> <span data-ttu-id="9e696-158">Om hello ARP-tabell saknar mappas IP-adresser för hello gränssnitt tooMAC adresser, granska hello följande information:</span><span class="sxs-lookup"><span data-stu-id="9e696-158">If hello ARP table does not have IP addresses of hello interfaces mapped tooMAC addresses, review hello following information:</span></span>
> 
> 1. <span data-ttu-id="9e696-159">Om hello första IP-adressen för hello /30 undernät tilldelas för hello länken mellan hello MSEE PR och MSEE används hello-gränssnittet på MSEE PR.</span><span class="sxs-lookup"><span data-stu-id="9e696-159">If hello first IP address of hello /30 subnet assigned for hello link between hello MSEE-PR and MSEE is used on hello interface of MSEE-PR.</span></span> <span data-ttu-id="9e696-160">Azure använder alltid hello andra IP-adressen för MSEEs.</span><span class="sxs-lookup"><span data-stu-id="9e696-160">Azure always uses hello second IP address for MSEEs.</span></span>
> 2. <span data-ttu-id="9e696-161">Kontrollera om hello kund (C-tagg) och service (S-tagg) VLAN-taggarna matchar både på MSEE PR och MSEE.</span><span class="sxs-lookup"><span data-stu-id="9e696-161">Verify if hello customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a><span data-ttu-id="9e696-162">ARP-tabellen när Microsoft sida har problem</span><span class="sxs-lookup"><span data-stu-id="9e696-162">ARP table when Microsoft side has problems</span></span>
* <span data-ttu-id="9e696-163">En ARP-tabell som visas för en peering om det finns problem på hello Microsoft sida visas inte.</span><span class="sxs-lookup"><span data-stu-id="9e696-163">You will not see an ARP table shown for a peering if there are issues on hello Microsoft side.</span></span> 
* <span data-ttu-id="9e696-164">Öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="9e696-164">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="9e696-165">Ange att du har ett problem med nivå 2-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9e696-165">Specify that you have an issue with layer 2 connectivity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9e696-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e696-166">Next Steps</span></span>
* <span data-ttu-id="9e696-167">Kontrollera Layer 3 konfigurationer för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="9e696-167">Validate Layer 3 configurations for your ExpressRoute circuit</span></span>
  * <span data-ttu-id="9e696-168">Hämta väg sammanfattning toodetermine hello tillståndet för BGP-sessioner</span><span class="sxs-lookup"><span data-stu-id="9e696-168">Get route summary toodetermine hello state of BGP sessions</span></span> 
  * <span data-ttu-id="9e696-169">Hämta väg tabell toodetermine vilka prefix har annonserats över ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="9e696-169">Get route table toodetermine which prefixes are advertised across ExpressRoute</span></span>
* <span data-ttu-id="9e696-170">Validera dataöverföring genom att granska byte in / ut</span><span class="sxs-lookup"><span data-stu-id="9e696-170">Validate data transfer by reviewing bytes in / out</span></span>
* <span data-ttu-id="9e696-171">Öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om det fortfarande uppstår problem.</span><span class="sxs-lookup"><span data-stu-id="9e696-171">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

