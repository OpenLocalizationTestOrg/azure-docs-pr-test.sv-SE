---
title: "Hämtningen av ARP tabeller: klassiska: Azure ExpressRoute felsökning | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för att hämta ARP tabeller för en ExpressRoute-krets."
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: b5856acf-03c2-4933-8111-6ce12998d92a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: fcc847b7e30fd55ca759830e0254ab7542e7663e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-arp-tables-in-the-classic-deployment-model"></a><span data-ttu-id="6623f-103">Hämtningen av ARP tabeller i den klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="6623f-103">Getting ARP tables in the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6623f-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6623f-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="6623f-105">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="6623f-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="6623f-106">Den här artikeln vägleder dig genom stegen för att hämta tabellerna protokollet ARP (Address Resolution) för Azure ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="6623f-106">This article walks you through the steps for getting the Address Resolution Protocol (ARP) tables for your Azure ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6623f-107">Det här dokumentet är avsedda att hjälpa dig att diagnostisera och åtgärda problem med enkla.</span><span class="sxs-lookup"><span data-stu-id="6623f-107">This document is intended to help you diagnose and fix simple issues.</span></span> <span data-ttu-id="6623f-108">Det är inte avsedd att vara en ersättning för Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="6623f-108">It is not intended to be a replacement for Microsoft support.</span></span> <span data-ttu-id="6623f-109">Om du inte kan lösa problemet med hjälp av följande riktlinjer, öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="6623f-109">If you can't solve the problem by using the following guidance, open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="6623f-110">Åtgärda ARP (Resolution Protocol) och ARP-tabeller</span><span class="sxs-lookup"><span data-stu-id="6623f-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="6623f-111">ARP är en nivå 2-protokollet som definieras i [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="6623f-111">ARP is a Layer 2 protocol that's defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="6623f-112">ARP används för att mappa en Ethernet-adress (MAC-adress) till en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="6623f-112">ARP is used to map an Ethernet address (MAC address) to an IP address.</span></span>

<span data-ttu-id="6623f-113">En ARP-tabell ger en mappning av IPv4-adress och MAC-adress för en viss peering.</span><span class="sxs-lookup"><span data-stu-id="6623f-113">An ARP table provides a mapping of the IPv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="6623f-114">ARP-tabell för en ExpressRoute-krets peering innehåller följande information för varje gränssnitt (primär eller sekundär):</span><span class="sxs-lookup"><span data-stu-id="6623f-114">The ARP table for an ExpressRoute circuit peering provides the following information for each interface (primary and secondary):</span></span>

1. <span data-ttu-id="6623f-115">Mappning av en lokal router-gränssnittet IP-adress till en MAC-adress</span><span class="sxs-lookup"><span data-stu-id="6623f-115">Mapping of an on-premises router interface IP address to a MAC address</span></span>
2. <span data-ttu-id="6623f-116">Mappning av en ExpressRoute router-gränssnittet IP-adress till en MAC-adress</span><span class="sxs-lookup"><span data-stu-id="6623f-116">Mapping of an ExpressRoute router interface IP address to a MAC address</span></span>
3. <span data-ttu-id="6623f-117">Mappningen ålder</span><span class="sxs-lookup"><span data-stu-id="6623f-117">The age of the mapping</span></span>

<span data-ttu-id="6623f-118">ARP-tabeller kan hjälpa med verifierar nivå 2-konfiguration och felsökning av grundläggande nivå 2-anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="6623f-118">ARP tables can help with validating Layer 2 configuration and with troubleshooting basic Layer 2 connectivity issues.</span></span>

<span data-ttu-id="6623f-119">Följande är ett exempel på en ARP-tabell:</span><span class="sxs-lookup"><span data-stu-id="6623f-119">Following is an example of an ARP table:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="6623f-120">Följande avsnitt innehåller information om hur du visar ARP-tabeller som ses av routrar i utkanten av ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6623f-120">The following section provides information about how to view the ARP tables that are seen by the ExpressRoute edge routers.</span></span>

## <a name="prerequisites-for-using-arp-tables"></a><span data-ttu-id="6623f-121">Krav för att använda ARP-tabeller</span><span class="sxs-lookup"><span data-stu-id="6623f-121">Prerequisites for using ARP tables</span></span>
<span data-ttu-id="6623f-122">Kontrollera att du har följande innan du fortsätter:</span><span class="sxs-lookup"><span data-stu-id="6623f-122">Ensure that you have the following before you continue:</span></span>

* <span data-ttu-id="6623f-123">En giltig ExpressRoute-krets som är konfigurerad med minst en peering.</span><span class="sxs-lookup"><span data-stu-id="6623f-123">A valid ExpressRoute circuit that's configured with at least one peering.</span></span> <span data-ttu-id="6623f-124">Kretsen måste vara fullständigt konfigurerade av providern för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6623f-124">The circuit must be fully configured by the connectivity provider.</span></span> <span data-ttu-id="6623f-125">Du (eller anslutningsleverantören) måste konfigurera minst en av peerkopplingar (Azure privat, Azure offentliga eller Microsoft) på den här kretsen.</span><span class="sxs-lookup"><span data-stu-id="6623f-125">You (or your connectivity provider) must configure at least one of the peerings (Azure private, Azure public, or Microsoft) on this circuit.</span></span>
* <span data-ttu-id="6623f-126">IP-adressintervall som används för att konfigurera peerkopplingar (Azure privat, Azure offentliga och Microsoft).</span><span class="sxs-lookup"><span data-stu-id="6623f-126">IP address ranges that are used for configuring the peerings (Azure private, Azure public, and Microsoft).</span></span> <span data-ttu-id="6623f-127">Granska IP-adresstilldelning exemplen i den [ExpressRoute routning kravsidan](expressroute-routing.md) att få en förståelse för hur IP-adresser mappas till gränssnitt på din aise och ExpressRoute-sida.</span><span class="sxs-lookup"><span data-stu-id="6623f-127">Review the IP address assignment examples in the [ExpressRoute routing requirements page](expressroute-routing.md) to get an understanding of how IP addresses are mapped to interfaces on your aise and on the ExpressRoute side.</span></span> <span data-ttu-id="6623f-128">Du kan få information om peering konfiguration genom att granska den [konfigurationssidan för ExpressRoute-peering](expressroute-howto-routing-classic.md).</span><span class="sxs-lookup"><span data-stu-id="6623f-128">You can get information about the peering configuration by reviewing the [ExpressRoute peering configuration page](expressroute-howto-routing-classic.md).</span></span>
* <span data-ttu-id="6623f-129">Information från nätverk team eller anslutning leverantören om MAC-adresser för de gränssnitt som används med IP-adresserna.</span><span class="sxs-lookup"><span data-stu-id="6623f-129">Information from your networking team or connectivity provider about the MAC addresses of the interfaces that are used with these IP addresses.</span></span>
* <span data-ttu-id="6623f-130">Senaste Windows PowerShell-modulen för Azure (version 1,50 eller senare).</span><span class="sxs-lookup"><span data-stu-id="6623f-130">The latest Windows PowerShell module for Azure (version 1.50 or later).</span></span>

## <a name="arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="6623f-131">ARP-tabeller för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="6623f-131">ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="6623f-132">Det här avsnittet innehåller instruktioner om hur du visar ARP-tabeller för varje typ av peering med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6623f-132">This section provides instructions about how to view the ARP tables for each type of peering by using PowerShell.</span></span> <span data-ttu-id="6623f-133">Innan du fortsätter måste du eller anslutningsleverantören att konfigurera peering.</span><span class="sxs-lookup"><span data-stu-id="6623f-133">Before you continue, either you or your connectivity provider needs to configure the peering.</span></span> <span data-ttu-id="6623f-134">Varje kretsen har två sökvägar (primär eller sekundär).</span><span class="sxs-lookup"><span data-stu-id="6623f-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="6623f-135">Du kan kontrollera ARP-tabell för varje sökväg oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="6623f-135">You can check the ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="6623f-136">ARP-tabeller för privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="6623f-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="6623f-137">Följande cmdlet ger ARP tabeller för privat Azure-peering:</span><span class="sxs-lookup"><span data-stu-id="6623f-137">The following cmdlet provides the ARP tables for Azure private peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

<span data-ttu-id="6623f-138">Följande är exempel på utdata för en av sökvägarna:</span><span class="sxs-lookup"><span data-stu-id="6623f-138">Following is sample output for one of the paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="6623f-139">ARP-tabeller för offentlig Azure-peering:</span><span class="sxs-lookup"><span data-stu-id="6623f-139">ARP tables for Azure public peering:</span></span>
<span data-ttu-id="6623f-140">Följande cmdlet ger ARP tabeller för offentlig Azure-peering:</span><span class="sxs-lookup"><span data-stu-id="6623f-140">The following cmdlet provides the ARP tables for Azure public peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

<span data-ttu-id="6623f-141">Följande är exempel på utdata för en av sökvägarna:</span><span class="sxs-lookup"><span data-stu-id="6623f-141">Following is sample output for one of the paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="6623f-142">Följande är exempel på utdata för en av sökvägarna:</span><span class="sxs-lookup"><span data-stu-id="6623f-142">Following is sample output for one of the paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="6623f-143">ARP-tabeller för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="6623f-143">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="6623f-144">Följande cmdlet ger ARP tabeller för Microsoft-peering:</span><span class="sxs-lookup"><span data-stu-id="6623f-144">The following cmdlet provides the ARP tables for Microsoft peering:</span></span>

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


<span data-ttu-id="6623f-145">För en av sökvägarna visas exempel på utdata nedan:</span><span class="sxs-lookup"><span data-stu-id="6623f-145">Sample output is shown below for one of the paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a><span data-ttu-id="6623f-146">Hur du använder den här informationen</span><span class="sxs-lookup"><span data-stu-id="6623f-146">How to use this information</span></span>
<span data-ttu-id="6623f-147">ARP-tabell för en peering kan användas för att validera nivå 2-konfiguration och anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6623f-147">The ARP table of a peering can be used to validate Layer 2 configuration and connectivity.</span></span> <span data-ttu-id="6623f-148">Det här avsnittet innehåller en översikt över hur ARP-tabeller ser ut i olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="6623f-148">This section provides an overview of how ARP tables look in different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a><span data-ttu-id="6623f-149">När en anslutning är i tillståndet för operativa (förväntade) ARP-tabell</span><span class="sxs-lookup"><span data-stu-id="6623f-149">ARP table when a circuit is in an operational (expected) state</span></span>
* <span data-ttu-id="6623f-150">ARP-tabellen har en post för den lokala sidan med en giltig IP- och MAC-adress och liknande information för Microsoft-sida.</span><span class="sxs-lookup"><span data-stu-id="6623f-150">The ARP table has an entry for the on-premises side with a valid IP and MAC address, and a similar entry for the Microsoft side.</span></span>
* <span data-ttu-id="6623f-151">Den sista oktetten i den lokala IP-adressen är alltid ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="6623f-151">The last octet of the on-premises IP address is always an odd number.</span></span>
* <span data-ttu-id="6623f-152">Den sista oktetten av Microsoft IP-adressen är alltid ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="6623f-152">The last octet of the Microsoft IP address is always an even number.</span></span>
* <span data-ttu-id="6623f-153">Samma MAC-adress visas på Microsoft-sida för alla tre peerkopplingar (primära och sekundära).</span><span class="sxs-lookup"><span data-stu-id="6623f-153">The same MAC address appears on the Microsoft side for all three peerings (primary/secondary).</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a><span data-ttu-id="6623f-154">När den lokala eller när anslutningen-provider-sida har problem med ARP-tabell</span><span class="sxs-lookup"><span data-stu-id="6623f-154">ARP table when it's on-premises or when the connectivity-provider side has problems</span></span>
 <span data-ttu-id="6623f-155">Endast en post visas i ARP-tabell.</span><span class="sxs-lookup"><span data-stu-id="6623f-155">Only one entry appears in the ARP table.</span></span> <span data-ttu-id="6623f-156">Mappningen mellan MAC-adressen och IP-adressen som används på Microsoft-sida visas.</span><span class="sxs-lookup"><span data-stu-id="6623f-156">It shows the mapping between the MAC address and the IP address that's used on the Microsoft side.</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> <span data-ttu-id="6623f-157">Öppna en supportbegäran med anslutningsleverantören för att lösa det. Om det uppstår ett problem så här.</span><span class="sxs-lookup"><span data-stu-id="6623f-157">If you experience an issue like this, open a support request with your connectivity provider to resolve it.</span></span>
> 
> 

### <a name="arp-table-when-the-microsoft-side-has-problems"></a><span data-ttu-id="6623f-158">ARP-tabellen när Microsoft sida har problem</span><span class="sxs-lookup"><span data-stu-id="6623f-158">ARP table when the Microsoft side has problems</span></span>
* <span data-ttu-id="6623f-159">En ARP-tabell som visas för en peering om det finns problem på Microsoft-sida visas inte.</span><span class="sxs-lookup"><span data-stu-id="6623f-159">You will not see an ARP table shown for a peering if there are issues on the Microsoft side.</span></span>
* <span data-ttu-id="6623f-160">Öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="6623f-160">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="6623f-161">Ange att du har ett problem med nivå 2-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6623f-161">Specify that you have an issue with Layer 2 connectivity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6623f-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6623f-162">Next steps</span></span>
* <span data-ttu-id="6623f-163">Kontrollera Layer 3 konfigurationer för ExpressRoute-krets:</span><span class="sxs-lookup"><span data-stu-id="6623f-163">Validate Layer 3 configurations for your ExpressRoute circuit:</span></span>
  * <span data-ttu-id="6623f-164">Hämta en väg sammanfattning om du vill bestämma tillståndet för BGP-sessioner.</span><span class="sxs-lookup"><span data-stu-id="6623f-164">Get a route summary to determine the state of BGP sessions.</span></span>
  * <span data-ttu-id="6623f-165">Hämta en routningstabell för att avgöra vilka prefix har annonserats över ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6623f-165">Get a route table to determine which prefixes are advertised across ExpressRoute.</span></span>
* <span data-ttu-id="6623f-166">Validera dataöverföring genom att granska byte in och ut.</span><span class="sxs-lookup"><span data-stu-id="6623f-166">Validate data transfer by reviewing bytes in and out.</span></span>
* <span data-ttu-id="6623f-167">Öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om det fortfarande uppstår problem.</span><span class="sxs-lookup"><span data-stu-id="6623f-167">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

