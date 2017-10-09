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
# <a name="getting-arp-tables-in-hello-resource-manager-deployment-model"></a>Hämtningen av ARP tabeller i hello Resource Manager-distributionsmodellen
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell – Klassisk](expressroute-troubleshooting-arp-classic.md)
> 
> 

Den här artikeln vägleder dig genom hello steg toolearn hello ARP tabeller för ExpressRoute-kretsen. 

> [!IMPORTANT]
> Det här dokumentet är avsett toohelp du diagnostisera och åtgärda problem med enkla. Det är inte avsedda toobe en ersättning för Microsoft-supporten. Du måste öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om du toosolve hello problemet med hjälp av hello riktlinjer som beskrivs nedan.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Åtgärda ARP (Resolution Protocol) och ARP-tabeller
Protokollet ARP (Address Resolution) är en nivå 2-protokollet som definieras i [RFC 826](https://tools.ietf.org/html/rfc826). ARP kan använda toomap hello Ethernet-adress (MAC-adress) med en ip-adress.

hello ARP-tabell ger en mappning av hello ipv4-adress och MAC-adress för en viss peering. hello ARP-tabell för en ExpressRoute-krets peering ger hello följande information för varje gränssnitt (primär eller sekundär)

1. Mappning av lokala router-gränssnittet IP-adress toohello MAC-adress
2. Mappning av ExpressRoute-router-gränssnittet IP-adress toohello MAC-adress
3. Ålder hello-mappning

ARP-tabeller kan hjälpa dig att verifiera nivå 2-konfiguration och felsökning av grundläggande nivå 2 anslutningsproblem. 

Exempel ARP-tabell: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


hello följande avsnitt innehåller information om hur du visar hello ARP-tabeller som setts av hello ExpressRoute kant routrar. 

## <a name="prerequisites-for-learning-arp-tables"></a>Krav för ARP-tabeller
Kontrollera att du har hello följande innan du fortskrider ytterligare

* En giltig ExpressRoute-kretsen har konfigurerats med minst en peering. hello krets måste konfigureras fullständigt av hello anslutning providern. Du (eller anslutningsleverantören) måste ha konfigurerat minst en av hello peerkopplingar (Azure privat, Azure offentliga och Microsoft) på den här kretsen.
* IP-adressintervall som används för att konfigurera hello peerkopplingar (Azure privat, Azure offentliga och Microsoft). Granska hello IP-adresstilldelning exemplen i hello [ExpressRoute routning kravsidan](expressroute-routing.md) tooget en förståelse av hur ip-adresser är mappad toointerfaces på din sida och hello ExpressRoute sida. Du kan få information om hello peering konfiguration genom att granska hello [konfigurationssidan för ExpressRoute-peering](expressroute-howto-routing-arm.md).
* Information från ditt nätverk team / anslutningen providern på hello MAC-adresser för gränssnitt som används med IP-adresserna.
* Du måste ha hello senaste PowerShell-modulen för Azure (version 1,50 eller senare).

## <a name="getting-hello-arp-tables-for-your-expressroute-circuit"></a>Hämta hello ARP tabeller för ExpressRoute-krets
Det här avsnittet innehåller anvisningar om hur du visar hello ARP-tabeller per peering med hjälp av PowerShell. Du eller anslutningsleverantören måste ha konfigurerat hello peering innan du kommer ytterligare. Varje kretsen har två sökvägar (primär eller sekundär). Du kan kontrollera hello ARP-tabell för varje sökväg oberoende av varandra.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-tabeller för privat Azure-peering
hello följande cmdlet ger hello ARP tabeller för privat Azure-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Exempel på utdata nedan för en hello sökvägar

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-tabeller för offentlig Azure-peering
hello följande cmdlet ger hello ARP tabeller för offentlig Azure-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Exempel på utdata nedan för en hello sökvägar

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-tabeller för Microsoft-peering
hello följande cmdlet ger hello ARP tabeller för Microsoft-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Exempel på utdata nedan för en hello sökvägar

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a>Hur toouse informationen
hello ARP-tabell för en peering kan användas för toodetermine Validera nivå 2-konfiguration och anslutningen. Det här avsnittet innehåller en översikt över hur ARP-tabeller ser ut under olika scenarier.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>När en anslutning har drifttillstånd (förväntade tillståndet) ARP-tabell
* hello ARP-tabell har en post för hello lokalt sida med en giltig IP-adress och MAC-adress och en liknande post för hello Microsoft sida. 
* hello sista oktetten hello lokala ip-adress kommer alltid att ett udda tal.
* hello sista oktetten av hello Microsoft IP-adress ska alltid vara ett jämnt tal.
* hello samma MAC-adress visas på hello Microsoft sida för alla 3 peerkopplingar (primära / sekundära). 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP-tabellen när lokalt / anslutningen providern sida har problem
Om det finns problem med hello lokala eller anslutningen providern kan du se att antingen en enda post visas i hello ARP tabell eller hello lokal MAC-adress visas ofullständiga. Detta visar hello mappning mellan hello MAC-adress och IP-adress som används i hello Microsoft sida. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

eller
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Öppna en supportbegäran med din anslutning providern toodebug sådana problem. Om hello ARP-tabell saknar mappas IP-adresser för hello gränssnitt tooMAC adresser, granska hello följande information:
> 
> 1. Om hello första IP-adressen för hello /30 undernät tilldelas för hello länken mellan hello MSEE PR och MSEE används hello-gränssnittet på MSEE PR. Azure använder alltid hello andra IP-adressen för MSEEs.
> 2. Kontrollera om hello kund (C-tagg) och service (S-tagg) VLAN-taggarna matchar både på MSEE PR och MSEE.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-tabellen när Microsoft sida har problem
* En ARP-tabell som visas för en peering om det finns problem på hello Microsoft sida visas inte. 
* Öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Ange att du har ett problem med nivå 2-anslutningen. 

## <a name="next-steps"></a>Nästa steg
* Kontrollera Layer 3 konfigurationer för ExpressRoute-krets
  * Hämta väg sammanfattning toodetermine hello tillståndet för BGP-sessioner 
  * Hämta väg tabell toodetermine vilka prefix har annonserats över ExpressRoute
* Validera dataöverföring genom att granska byte in / ut
* Öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om det fortfarande uppstår problem.

