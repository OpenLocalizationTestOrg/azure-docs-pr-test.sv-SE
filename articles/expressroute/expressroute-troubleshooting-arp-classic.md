---
title: "Hämtningen av ARP tabeller: klassiska: Azure ExpressRoute felsökning | Microsoft Docs"
description: "Den här sidan innehåller anvisningar för hämtning hello ARP tabeller för en ExpressRoute-krets."
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
ms.openlocfilehash: 2b01304a38fa0e0def27dbd7c391d7ad8bbdabff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-arp-tables-in-hello-classic-deployment-model"></a><span data-ttu-id="9cde6-103">Hämtningen av ARP tabeller i hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="9cde6-103">Getting ARP tables in hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9cde6-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9cde6-104">PowerShell - Resource Manager</span></span>](expressroute-troubleshooting-arp-resource-manager.md)
> * [<span data-ttu-id="9cde6-105">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="9cde6-105">PowerShell - Classic</span></span>](expressroute-troubleshooting-arp-classic.md)
> 
> 

<span data-ttu-id="9cde6-106">Den här artikeln vägleder dig genom hello stegen för att hämta hello protokollet ARP (Address Resolution) tabeller för Azure ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="9cde6-106">This article walks you through hello steps for getting hello Address Resolution Protocol (ARP) tables for your Azure ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9cde6-107">Det här dokumentet är avsett toohelp du diagnostisera och åtgärda problem med enkla.</span><span class="sxs-lookup"><span data-stu-id="9cde6-107">This document is intended toohelp you diagnose and fix simple issues.</span></span> <span data-ttu-id="9cde6-108">Det är inte avsedda toobe en ersättning för Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="9cde6-108">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="9cde6-109">Om du inte kan lösa hello problemet med hjälp av följande riktlinjer hello, öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="9cde6-109">If you can't solve hello problem by using hello following guidance, open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a><span data-ttu-id="9cde6-110">Åtgärda ARP (Resolution Protocol) och ARP-tabeller</span><span class="sxs-lookup"><span data-stu-id="9cde6-110">Address Resolution Protocol (ARP) and ARP tables</span></span>
<span data-ttu-id="9cde6-111">ARP är en nivå 2-protokollet som definieras i [RFC 826](https://tools.ietf.org/html/rfc826).</span><span class="sxs-lookup"><span data-stu-id="9cde6-111">ARP is a Layer 2 protocol that's defined in [RFC 826](https://tools.ietf.org/html/rfc826).</span></span> <span data-ttu-id="9cde6-112">ARP kan använda toomap en Ethernet-adress (MAC-adress) tooan IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9cde6-112">ARP is used toomap an Ethernet address (MAC address) tooan IP address.</span></span>

<span data-ttu-id="9cde6-113">En ARP-tabell ger en mappning av hello IPv4-adress och MAC-adress för en viss peering.</span><span class="sxs-lookup"><span data-stu-id="9cde6-113">An ARP table provides a mapping of hello IPv4 address and MAC address for a particular peering.</span></span> <span data-ttu-id="9cde6-114">hello ARP-tabell för en ExpressRoute-krets peering innehåller hello följande information för varje gränssnitt (primär eller sekundär):</span><span class="sxs-lookup"><span data-stu-id="9cde6-114">hello ARP table for an ExpressRoute circuit peering provides hello following information for each interface (primary and secondary):</span></span>

1. <span data-ttu-id="9cde6-115">Mappning av en lokal router-gränssnittet IP-adress tooa MAC-adress</span><span class="sxs-lookup"><span data-stu-id="9cde6-115">Mapping of an on-premises router interface IP address tooa MAC address</span></span>
2. <span data-ttu-id="9cde6-116">Mappning av en ExpressRoute router-gränssnittet IP-adress tooa MAC-adress</span><span class="sxs-lookup"><span data-stu-id="9cde6-116">Mapping of an ExpressRoute router interface IP address tooa MAC address</span></span>
3. <span data-ttu-id="9cde6-117">hello ålder hello-mappning</span><span class="sxs-lookup"><span data-stu-id="9cde6-117">hello age of hello mapping</span></span>

<span data-ttu-id="9cde6-118">ARP-tabeller kan hjälpa med verifierar nivå 2-konfiguration och felsökning av grundläggande nivå 2-anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="9cde6-118">ARP tables can help with validating Layer 2 configuration and with troubleshooting basic Layer 2 connectivity issues.</span></span>

<span data-ttu-id="9cde6-119">Följande är ett exempel på en ARP-tabell:</span><span class="sxs-lookup"><span data-stu-id="9cde6-119">Following is an example of an ARP table:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="9cde6-120">hello följande avsnitt innehåller information om hur tooview hello ARP-tabeller som ses av hello ExpressRoute kant routrar.</span><span class="sxs-lookup"><span data-stu-id="9cde6-120">hello following section provides information about how tooview hello ARP tables that are seen by hello ExpressRoute edge routers.</span></span>

## <a name="prerequisites-for-using-arp-tables"></a><span data-ttu-id="9cde6-121">Krav för att använda ARP-tabeller</span><span class="sxs-lookup"><span data-stu-id="9cde6-121">Prerequisites for using ARP tables</span></span>
<span data-ttu-id="9cde6-122">Kontrollera att du har hello följande innan du fortsätter:</span><span class="sxs-lookup"><span data-stu-id="9cde6-122">Ensure that you have hello following before you continue:</span></span>

* <span data-ttu-id="9cde6-123">En giltig ExpressRoute-krets som är konfigurerad med minst en peering.</span><span class="sxs-lookup"><span data-stu-id="9cde6-123">A valid ExpressRoute circuit that's configured with at least one peering.</span></span> <span data-ttu-id="9cde6-124">hello krets måste konfigureras fullständigt av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="9cde6-124">hello circuit must be fully configured by hello connectivity provider.</span></span> <span data-ttu-id="9cde6-125">Du (eller anslutningsleverantören) måste konfigurera minst en av hello peerkopplingar (Azure privat, Azure offentliga eller Microsoft) på den här kretsen.</span><span class="sxs-lookup"><span data-stu-id="9cde6-125">You (or your connectivity provider) must configure at least one of hello peerings (Azure private, Azure public, or Microsoft) on this circuit.</span></span>
* <span data-ttu-id="9cde6-126">IP-adressintervall som används för att konfigurera hello peerkopplingar (Azure privat, Azure offentliga och Microsoft).</span><span class="sxs-lookup"><span data-stu-id="9cde6-126">IP address ranges that are used for configuring hello peerings (Azure private, Azure public, and Microsoft).</span></span> <span data-ttu-id="9cde6-127">Granska hello IP-adresstilldelning exemplen i hello [ExpressRoute routning kravsidan](expressroute-routing.md) tooget en förståelse av hur IP-adresser är mappad toointerfaces på din aise och hello ExpressRoute sida.</span><span class="sxs-lookup"><span data-stu-id="9cde6-127">Review hello IP address assignment examples in hello [ExpressRoute routing requirements page](expressroute-routing.md) tooget an understanding of how IP addresses are mapped toointerfaces on your aise and on hello ExpressRoute side.</span></span> <span data-ttu-id="9cde6-128">Du kan få information om hello peering konfiguration genom att granska hello [konfigurationssidan för ExpressRoute-peering](expressroute-howto-routing-classic.md).</span><span class="sxs-lookup"><span data-stu-id="9cde6-128">You can get information about hello peering configuration by reviewing hello [ExpressRoute peering configuration page](expressroute-howto-routing-classic.md).</span></span>
* <span data-ttu-id="9cde6-129">Information från nätverk team eller anslutning leverantören om hello MAC-adresser hello-gränssnitt som används med IP-adresserna.</span><span class="sxs-lookup"><span data-stu-id="9cde6-129">Information from your networking team or connectivity provider about hello MAC addresses of hello interfaces that are used with these IP addresses.</span></span>
* <span data-ttu-id="9cde6-130">hello senaste Windows PowerShell-modulen för Azure (version 1,50 eller senare).</span><span class="sxs-lookup"><span data-stu-id="9cde6-130">hello latest Windows PowerShell module for Azure (version 1.50 or later).</span></span>

## <a name="arp-tables-for-your-expressroute-circuit"></a><span data-ttu-id="9cde6-131">ARP-tabeller för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="9cde6-131">ARP tables for your ExpressRoute circuit</span></span>
<span data-ttu-id="9cde6-132">Det här avsnittet innehåller instruktioner om hur tooview hello ARP tabeller för varje typ av peering med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9cde6-132">This section provides instructions about how tooview hello ARP tables for each type of peering by using PowerShell.</span></span> <span data-ttu-id="9cde6-133">Innan du fortsätter måste du eller anslutningsleverantören tooconfigure hello peering.</span><span class="sxs-lookup"><span data-stu-id="9cde6-133">Before you continue, either you or your connectivity provider needs tooconfigure hello peering.</span></span> <span data-ttu-id="9cde6-134">Varje kretsen har två sökvägar (primär eller sekundär).</span><span class="sxs-lookup"><span data-stu-id="9cde6-134">Each circuit has two paths (primary and secondary).</span></span> <span data-ttu-id="9cde6-135">Du kan kontrollera hello ARP-tabell för varje sökväg oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="9cde6-135">You can check hello ARP table for each path independently.</span></span>

### <a name="arp-tables-for-azure-private-peering"></a><span data-ttu-id="9cde6-136">ARP-tabeller för privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="9cde6-136">ARP tables for Azure private peering</span></span>
<span data-ttu-id="9cde6-137">följande cmdlet hello innehåller hello ARP tabeller för privat Azure-peering:</span><span class="sxs-lookup"><span data-stu-id="9cde6-137">hello following cmdlet provides hello ARP tables for Azure private peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

<span data-ttu-id="9cde6-138">Följande är exempel på utdata för en hello sökvägar:</span><span class="sxs-lookup"><span data-stu-id="9cde6-138">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a><span data-ttu-id="9cde6-139">ARP-tabeller för offentlig Azure-peering:</span><span class="sxs-lookup"><span data-stu-id="9cde6-139">ARP tables for Azure public peering:</span></span>
<span data-ttu-id="9cde6-140">följande cmdlet hello innehåller hello ARP tabeller för offentlig Azure-peering:</span><span class="sxs-lookup"><span data-stu-id="9cde6-140">hello following cmdlet provides hello ARP tables for Azure public peering:</span></span>

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

<span data-ttu-id="9cde6-141">Följande är exempel på utdata för en hello sökvägar:</span><span class="sxs-lookup"><span data-stu-id="9cde6-141">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


<span data-ttu-id="9cde6-142">Följande är exempel på utdata för en hello sökvägar:</span><span class="sxs-lookup"><span data-stu-id="9cde6-142">Following is sample output for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a><span data-ttu-id="9cde6-143">ARP-tabeller för Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="9cde6-143">ARP tables for Microsoft peering</span></span>
<span data-ttu-id="9cde6-144">hello följande cmdlet ger hello ARP tabeller för Microsoft-peering:</span><span class="sxs-lookup"><span data-stu-id="9cde6-144">hello following cmdlet provides hello ARP tables for Microsoft peering:</span></span>

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


<span data-ttu-id="9cde6-145">Exempel på utdata nedan för en hello sökvägar:</span><span class="sxs-lookup"><span data-stu-id="9cde6-145">Sample output is shown below for one of hello paths:</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a><span data-ttu-id="9cde6-146">Hur toouse informationen</span><span class="sxs-lookup"><span data-stu-id="9cde6-146">How toouse this information</span></span>
<span data-ttu-id="9cde6-147">hello ARP-tabell för en peering kan vara används toovalidate nivå 2-konfiguration och anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9cde6-147">hello ARP table of a peering can be used toovalidate Layer 2 configuration and connectivity.</span></span> <span data-ttu-id="9cde6-148">Det här avsnittet innehåller en översikt över hur ARP-tabeller ser ut i olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="9cde6-148">This section provides an overview of how ARP tables look in different scenarios.</span></span>

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a><span data-ttu-id="9cde6-149">När en anslutning är i tillståndet för operativa (förväntade) ARP-tabell</span><span class="sxs-lookup"><span data-stu-id="9cde6-149">ARP table when a circuit is in an operational (expected) state</span></span>
* <span data-ttu-id="9cde6-150">hello ARP-tabell har en post för hello lokalt sida med en giltig IP- och MAC-adress och en liknande post för hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="9cde6-150">hello ARP table has an entry for hello on-premises side with a valid IP and MAC address, and a similar entry for hello Microsoft side.</span></span>
* <span data-ttu-id="9cde6-151">hello sista oktetten hello lokala IP-adress är alltid ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="9cde6-151">hello last octet of hello on-premises IP address is always an odd number.</span></span>
* <span data-ttu-id="9cde6-152">hello sista oktetten av hello Microsoft IP-adressen är alltid ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="9cde6-152">hello last octet of hello Microsoft IP address is always an even number.</span></span>
* <span data-ttu-id="9cde6-153">hello visas samma MAC-adress i hello Microsoft sida för alla tre peerkopplingar (primära och sekundära).</span><span class="sxs-lookup"><span data-stu-id="9cde6-153">hello same MAC address appears on hello Microsoft side for all three peerings (primary/secondary).</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-hello-connectivity-provider-side-has-problems"></a><span data-ttu-id="9cde6-154">När den lokala eller när hello anslutningen-provider på klientsidan har problem med ARP-tabell</span><span class="sxs-lookup"><span data-stu-id="9cde6-154">ARP table when it's on-premises or when hello connectivity-provider side has problems</span></span>
 <span data-ttu-id="9cde6-155">Endast en post visas i hello ARP-tabell.</span><span class="sxs-lookup"><span data-stu-id="9cde6-155">Only one entry appears in hello ARP table.</span></span> <span data-ttu-id="9cde6-156">Den visar hello mappning mellan hello MAC och hello IP-adress som används på hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="9cde6-156">It shows hello mapping between hello MAC address and hello IP address that's used on hello Microsoft side.</span></span>

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> <span data-ttu-id="9cde6-157">Om det uppstår ett problem så här öppnar du en supportbegäran med din anslutning providern tooresolve begära den.</span><span class="sxs-lookup"><span data-stu-id="9cde6-157">If you experience an issue like this, open a support request with your connectivity provider tooresolve it.</span></span>
> 
> 

### <a name="arp-table-when-hello-microsoft-side-has-problems"></a><span data-ttu-id="9cde6-158">ARP-tabell när hello Microsoft sida har problem</span><span class="sxs-lookup"><span data-stu-id="9cde6-158">ARP table when hello Microsoft side has problems</span></span>
* <span data-ttu-id="9cde6-159">En ARP-tabell som visas för en peering om det finns problem på hello Microsoft sida visas inte.</span><span class="sxs-lookup"><span data-stu-id="9cde6-159">You will not see an ARP table shown for a peering if there are issues on hello Microsoft side.</span></span>
* <span data-ttu-id="9cde6-160">Öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="9cde6-160">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="9cde6-161">Ange att du har ett problem med nivå 2-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9cde6-161">Specify that you have an issue with Layer 2 connectivity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9cde6-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9cde6-162">Next steps</span></span>
* <span data-ttu-id="9cde6-163">Kontrollera Layer 3 konfigurationer för ExpressRoute-krets:</span><span class="sxs-lookup"><span data-stu-id="9cde6-163">Validate Layer 3 configurations for your ExpressRoute circuit:</span></span>
  * <span data-ttu-id="9cde6-164">Hämta en väg sammanfattning toodetermine hello tillståndet för BGP-sessioner.</span><span class="sxs-lookup"><span data-stu-id="9cde6-164">Get a route summary toodetermine hello state of BGP sessions.</span></span>
  * <span data-ttu-id="9cde6-165">Hämta en väg tabell toodetermine vilka prefix har annonserats över ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9cde6-165">Get a route table toodetermine which prefixes are advertised across ExpressRoute.</span></span>
* <span data-ttu-id="9cde6-166">Validera dataöverföring genom att granska byte in och ut.</span><span class="sxs-lookup"><span data-stu-id="9cde6-166">Validate data transfer by reviewing bytes in and out.</span></span>
* <span data-ttu-id="9cde6-167">Öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om det fortfarande uppstår problem.</span><span class="sxs-lookup"><span data-stu-id="9cde6-167">Open a support request with [Microsoft Azure Help+support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) if you are still experiencing issues.</span></span>

