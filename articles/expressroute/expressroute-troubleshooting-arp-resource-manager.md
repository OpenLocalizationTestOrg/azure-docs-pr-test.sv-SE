---
title: "Hämtningen av ARP tabeller: hanteraren för filserverresurser: Azure ExpressRoute felsökning | Microsoft Docs"
description: "Den här sidan innehåller instruktioner om hur du hämtar ARP tabeller för en ExpressRoute-krets"
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
ms.openlocfilehash: a65b1ba2998eae33b3e73bd2492fbbf025eb5946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a><span data-ttu-id="dce20-103">Hämtningen av ARP tabeller i Resource Manager-distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="dce20-103">Getting ARP tables in the Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dce20-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="dce20-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="dce20-105">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="dce20-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="dce20-106">Den här artikeln vägleder dig genom stegen för att lära dig ARP-tabeller för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="dce20-106">This article walks you through the steps to learn the ARP tables for your ExpressRoute circuit.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="dce20-107">Det här dokumentet är avsedda att hjälpa dig att diagnostisera och åtgärda problem med enkla.</span><span class="sxs-lookup"><span data-stu-id="dce20-107">This document is intended to help you diagnose and fix simple issues.</span></span> <span data-ttu-id="dce20-108">Det är inte avsedd att vara en ersättning för Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="dce20-108">It is not intended to be a replacement for Microsoft support.</span></span> <span data-ttu-id="dce20-109">Du måste öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om det inte går att lösa problemet med hjälp av vägledningen som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="dce20-109">You must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are unable to solve the problem using the guidance described below.</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="dce20-110">Åtgärda ARP (Resolution Protocol) och ARP-tabeller</span><span class="sxs-lookup"><span data-stu-id="dce20-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="dce20-111">Protokollet ARP (Address Resolution) är en nivå 2-protokollet som definieras i [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="dce20-111">Address Resolution Protocol (ARP) is a layer 2 protocol defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="dce20-112">ARP används för att mappa en Ethernet-adress (MAC-adress) med en ip-adress.</span><span class="sxs-lookup"><span data-stu-id="dce20-112">ARP is used to map the Ethernet address (MAC address) with an ip address.</span></span>

<span data-ttu-id="dce20-113">ARP-tabell innehåller en mappning av ipv4-adress och MAC-adress för en viss peering.</span><span class="sxs-lookup"><span data-stu-id="dce20-113">The ARP table provides a mapping of the ipv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="dce20-114">ARP-tabell för en ExpressRoute-krets peering innehåller följande information för varje gränssnitt (primär eller sekundär)</span><span class="sxs-lookup"><span data-stu-id="dce20-114">The ARP table for an ExpressRoute circuit peering provides the following information for each interface (primary and secondary)</span></span>

1. <span data-ttu-id="dce20-115">Mappning av lokala router-gränssnittet IP-adress till MAC-adress</span><span class="sxs-lookup"><span data-stu-id="dce20-115">Mapping of on-premises router interface ip address to the MAC address</span></span>
2. <span data-ttu-id="dce20-116">Mappning av ExpressRoute-router-gränssnittet IP-adress till MAC-adress</span><span class="sxs-lookup"><span data-stu-id="dce20-116">Mapping of ExpressRoute router interface ip address to the MAC address</span></span>
3. <span data-ttu-id="dce20-117">Ålder mappningen</span><span class="sxs-lookup"><span data-stu-id="dce20-117">Age of the mapping</span></span>

<span data-ttu-id="dce20-118">ARP-tabeller kan hjälpa dig att verifiera nivå 2-konfiguration och felsökning av grundläggande nivå 2 anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="dce20-118">ARP tables can help validate layer 2 configuration and troubleshooting basic layer 2 connectivity issues.</span></span> 

<span data-ttu-id="dce20-119">Exempel ARP-tabell:</span><span class="sxs-lookup"><span data-stu-id="dce20-119">Example ARP table:</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


<span data-ttu-id="dce20-120">Följande avsnitt innehåller information om hur du kan visa ARP-tabeller som setts av routrar i utkanten av ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="dce20-120">The following section provides information on how you can view the ARP tables seen by the ExpressRoute edge routers.</span></span> 

## <a name="prerequisites-for-learning-arp-tables"></a><span data-ttu-id="dce20-121">Krav för ARP-tabeller</span><span class="sxs-lookup"><span data-stu-id="dce20-121">Prerequisites for learning ARP tables</span></span>
<span data-ttu-id="dce20-122">Kontrollera att du har följande innan du fortskrider ytterligare</span><span class="sxs-lookup"><span data-stu-id="dce20-122">Ensure that you have the following before you progress further</span></span>

* <span data-ttu-id="dce20-123">En giltig ExpressRoute-kretsen har konfigurerats med minst en peering.</span><span class="sxs-lookup"><span data-stu-id="dce20-123">A Valid ExpressRoute circuit configured with at least one peering.</span></span> <span data-ttu-id="dce20-124">Kretsen måste vara fullständigt konfigurerade av providern för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="dce20-124">The circuit must be fully configured by the connectivity provider.</span></span> <span data-ttu-id="dce20-125">Du (eller anslutningsleverantören) måste ha konfigurerat minst en av peerkopplingar (Azure privat, Azure offentliga och Microsoft) på den här kretsen.</span><span class="sxs-lookup"><span data-stu-id="dce20-125">You (or your connectivity provider) must have configured at least one of the peerings (Azure private, Azure public and Microsoft) on this circuit.</span></span>
* <span data-ttu-id="dce20-126">IP-adressintervall som används för att konfigurera peerkopplingar (Azure privat, Azure offentliga och Microsoft).</span><span class="sxs-lookup"><span data-stu-id="dce20-126">IP address ranges used for configuring the peerings (Azure private, Azure public and Microsoft).</span></span> <span data-ttu-id="dce20-127">Granska IP-adresstilldelning exemplen i den [ExpressRoute routning kravsidan](expressroute-routing.md) att få en förståelse för hur ip-adresser mappas till gränssnitt på din sida och på ExpressRoute-sida.</span><span class="sxs-lookup"><span data-stu-id="dce20-127">Review the ip address assignment examples in the [ExpressRoute routing requirements page](expressroute-routing.md) to get an understanding of how ip addresses are mapped to interfaces on your side and on the ExpressRoute side.</span></span> <span data-ttu-id="dce20-128">Du kan få information om peering konfigurationen genom att granska den [konfigurationssidan för ExpressRoute-peering](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="dce20-128">You can get information on the peering configuration by reviewing the [ExpressRoute peering configuration page](expressroute-howto-routing-arm.md).</span></span>
* <span data-ttu-id="dce20-129">Information från ditt nätverk team / anslutningen providern på MAC-adresserna för gränssnitt som används med IP-adresserna.</span><span class="sxs-lookup"><span data-stu-id="dce20-129">Information from your networking team / connectivity provider on the MAC addresses of interfaces used with these IP addresses.</span></span>
* <span data-ttu-id="dce20-130">Du måste ha den senaste PowerShell-modulen för Azure (version 1,50 eller senare).</span><span class="sxs-lookup"><span data-stu-id="dce20-130">You must have the latest PowerShell module for Azure (version 1.50 or newer).</span></span>

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="dce20-131">Hämta ARP tabeller för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dce20-131">Getting the ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="dce20-132">Det här avsnittet innehåller instruktioner om hur du kan visa ARP-tabeller per peering med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dce20-132">This section provides instructions on how you can view the ARP tables per peering using PowerShell.</span></span> <span data-ttu-id="dce20-133">Du eller anslutningsleverantören måste ha konfigurerat peering innan du kommer ytterligare.</span><span class="sxs-lookup"><span data-stu-id="dce20-133">You or your connectivity provider must have configured the peering before progressing further.</span></span> <span data-ttu-id="dce20-134">Varje kretsen har två sökvägar (primär eller sekundär).</span><span class="sxs-lookup"><span data-stu-id="dce20-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="dce20-135">Du kan kontrollera ARP-tabell för varje sökväg oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="dce20-135">You can check the ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="dce20-136">ARP-tabeller för privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="dce20-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="dce20-137">Följande cmdlet ger ARP tabeller för privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="dce20-137">The following cmdlet provides the ARP tables for Azure private peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

<span data-ttu-id="dce20-138">Exempel på utdata nedan för en av sökvägarna</span><span class="sxs-lookup"><span data-stu-id="dce20-138">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="dce20-139">ARP-tabeller för offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="dce20-139">ARP tables for Azure public peering</span></span>
<span data-ttu-id="dce20-140">Följande cmdlet ger ARP tabeller för offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="dce20-140">The following cmdlet provides the ARP tables for Azure public peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


<span data-ttu-id="dce20-141">Exempel på utdata nedan för en av sökvägarna</span><span class="sxs-lookup"><span data-stu-id="dce20-141">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="dce20-142">ARP-tabeller för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="dce20-142">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="dce20-143">Följande cmdlet ger ARP tabeller för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="dce20-143">The following cmdlet provides the ARP tables for Microsoft peering</span></span>

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


<span data-ttu-id="dce20-144">Exempel på utdata nedan för en av sökvägarna</span><span class="sxs-lookup"><span data-stu-id="dce20-144">Sample output is shown below for one of the paths</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a><span data-ttu-id="dce20-145">Hur du använder den här informationen</span><span class="sxs-lookup"><span data-stu-id="dce20-145">How to use this information</span></span>
<span data-ttu-id="dce20-146">ARP-tabell för en peering kan användas för att fastställa Validera nivå 2-konfiguration och anslutningen.</span><span class="sxs-lookup"><span data-stu-id="dce20-146">The ARP table of a peering can be used to determine validate layer 2 configuration and connectivity.</span></span> <span data-ttu-id="dce20-147">Det här avsnittet innehåller en översikt över hur ARP-tabeller ser ut under olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="dce20-147">This section provides an overview of how ARP tables will look under different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a><span data-ttu-id="dce20-148">När en anslutning har drifttillstånd (förväntade tillståndet) ARP-tabell</span><span class="sxs-lookup"><span data-stu-id="dce20-148">ARP table when a circuit is in operational state (expected state)</span></span>
* <span data-ttu-id="dce20-149">ARP-tabellen har en post för den lokala sidan med en giltig IP-adress och MAC-adress och liknande information för Microsoft-sida.</span><span class="sxs-lookup"><span data-stu-id="dce20-149">The ARP table will have an entry for the on-premises side with a valid IP address and MAC address and a similar entry for the Microsoft side.</span></span> 
* <span data-ttu-id="dce20-150">Den sista oktetten i den lokala ip-adressen kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="dce20-150">The last octet of the on-premises ip address will always be an odd number.</span></span>
* <span data-ttu-id="dce20-151">Den sista oktetten av Microsoft ip-adressen kommer alltid att ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="dce20-151">The last octet of the Microsoft ip address will always be an even number.</span></span>
* <span data-ttu-id="dce20-152">Samma MAC-adress visas på sidan för Microsoft för alla 3 peerkopplingar (primära / sekundära).</span><span class="sxs-lookup"><span data-stu-id="dce20-152">The same MAC address will appear on the Microsoft side for all 3 peerings (primary / secondary).</span></span> 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a><span data-ttu-id="dce20-153">ARP-tabellen när lokalt / anslutningen providern sida har problem</span><span class="sxs-lookup"><span data-stu-id="dce20-153">ARP table when on-premises / connectivity provider side has problems</span></span>
<span data-ttu-id="dce20-154">Om det är problem med lokalt eller anslutningen providern kan du se att antingen en enda post visas i ARP-tabell eller lokal MAC-adress visas ofullständiga.</span><span class="sxs-lookup"><span data-stu-id="dce20-154">If there are issues with the on-premises or connectivity provider you may see that either only one entry will appear in the ARP table or the on-prem MAC address will show incomplete.</span></span> <span data-ttu-id="dce20-155">Det här alternativet visas mappningen mellan MAC-adress och IP-adress som används i Microsoft-sida.</span><span class="sxs-lookup"><span data-stu-id="dce20-155">This will show the mapping between the MAC address and IP address used in the Microsoft side.</span></span> 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

<span data-ttu-id="dce20-156">eller</span><span class="sxs-lookup"><span data-stu-id="dce20-156">or</span></span>
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> <span data-ttu-id="dce20-157">Öppna en supportbegäran med anslutningsleverantören för att felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="dce20-157">Open a support request with your connectivity provider to debug such issues.</span></span> <span data-ttu-id="dce20-158">Granska följande information om ARP-tabell saknar IP-adresserna för de gränssnitt som mappats till MAC-adresser:</span><span class="sxs-lookup"><span data-stu-id="dce20-158">If the ARP table does not have IP addresses of the interfaces mapped to MAC addresses, review the following information:</span></span>
> 
> 1. <span data-ttu-id="dce20-159">Om den första IP-adressen för /30 undernät som har tilldelats för länken mellan MSEE PR och MSEE används för gränssnittet på MSEE PR.</span><span class="sxs-lookup"><span data-stu-id="dce20-159">If the first IP address of the /30 subnet assigned for the link between the MSEE-PR and MSEE is used on the interface of MSEE-PR.</span></span> <span data-ttu-id="dce20-160">Azure använder alltid den andra IP-adressen för MSEEs.</span><span class="sxs-lookup"><span data-stu-id="dce20-160">Azure always uses the second IP address for MSEEs.</span></span>
> 2. <span data-ttu-id="dce20-161">Kontrollera om kunden (C-tagg) och service (S-tagg) VLAN-taggarna matchar både på MSEE PR och MSEE.</span><span class="sxs-lookup"><span data-stu-id="dce20-161">Verify if the customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a><span data-ttu-id="dce20-162">ARP-tabellen när Microsoft sida har problem</span><span class="sxs-lookup"><span data-stu-id="dce20-162">ARP table when Microsoft side has problems</span></span>
* <span data-ttu-id="dce20-163">En ARP-tabell som visas för en peering om det finns problem på Microsoft-sida visas inte.</span><span class="sxs-lookup"><span data-stu-id="dce20-163">You will not see an ARP table shown for a peering if there are issues on the Microsoft side.</span></span> 
* <span data-ttu-id="dce20-164">Öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="dce20-164">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="dce20-165">Ange att du har ett problem med nivå 2-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="dce20-165">Specify that you have an issue with layer 2 connectivity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="dce20-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dce20-166">Next Steps</span></span>
* <span data-ttu-id="dce20-167">Kontrollera Layer 3 konfigurationer för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dce20-167">Validate Layer 3 configurations for your ExpressRoute circuit</span></span>
  * <span data-ttu-id="dce20-168">Hämta väg sammanfattning om du vill bestämma tillståndet för BGP-sessioner</span><span class="sxs-lookup"><span data-stu-id="dce20-168">Get route summary to determine the state of BGP sessions</span></span> 
  * <span data-ttu-id="dce20-169">Hämta routningstabellen för att avgöra vilka prefix har annonserats över ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="dce20-169">Get route table to determine which prefixes are advertised across ExpressRoute</span></span>
* <span data-ttu-id="dce20-170">Validera dataöverföring genom att granska byte in / ut</span><span class="sxs-lookup"><span data-stu-id="dce20-170">Validate data transfer by reviewing bytes in / out</span></span>
* <span data-ttu-id="dce20-171">Öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om det fortfarande uppstår problem.</span><span class="sxs-lookup"><span data-stu-id="dce20-171">Open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

