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
# <a name="getting-arp-tables-in-hello-classic-deployment-model"></a>Hämtningen av ARP tabeller i hello klassiska distributionsmodellen
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell – Klassisk](expressroute-troubleshooting-arp-classic.md)
> 
> 

Den här artikeln vägleder dig genom hello stegen för att hämta hello protokollet ARP (Address Resolution) tabeller för Azure ExpressRoute-krets.

> [!IMPORTANT]
> Det här dokumentet är avsett toohelp du diagnostisera och åtgärda problem med enkla. Det är inte avsedda toobe en ersättning för Microsoft-supporten. Om du inte kan lösa hello problemet med hjälp av följande riktlinjer hello, öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Åtgärda ARP (Resolution Protocol) och ARP-tabeller
ARP är en nivå 2-protokollet som definieras i [RFC 826](https://tools.ietf.org/html/rfc826). ARP kan använda toomap en Ethernet-adress (MAC-adress) tooan IP-adress.

En ARP-tabell ger en mappning av hello IPv4-adress och MAC-adress för en viss peering. hello ARP-tabell för en ExpressRoute-krets peering innehåller hello följande information för varje gränssnitt (primär eller sekundär):

1. Mappning av en lokal router-gränssnittet IP-adress tooa MAC-adress
2. Mappning av en ExpressRoute router-gränssnittet IP-adress tooa MAC-adress
3. hello ålder hello-mappning

ARP-tabeller kan hjälpa med verifierar nivå 2-konfiguration och felsökning av grundläggande nivå 2-anslutningsproblem.

Följande är ett exempel på en ARP-tabell:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


hello följande avsnitt innehåller information om hur tooview hello ARP-tabeller som ses av hello ExpressRoute kant routrar.

## <a name="prerequisites-for-using-arp-tables"></a>Krav för att använda ARP-tabeller
Kontrollera att du har hello följande innan du fortsätter:

* En giltig ExpressRoute-krets som är konfigurerad med minst en peering. hello krets måste konfigureras fullständigt av hello anslutning providern. Du (eller anslutningsleverantören) måste konfigurera minst en av hello peerkopplingar (Azure privat, Azure offentliga eller Microsoft) på den här kretsen.
* IP-adressintervall som används för att konfigurera hello peerkopplingar (Azure privat, Azure offentliga och Microsoft). Granska hello IP-adresstilldelning exemplen i hello [ExpressRoute routning kravsidan](expressroute-routing.md) tooget en förståelse av hur IP-adresser är mappad toointerfaces på din aise och hello ExpressRoute sida. Du kan få information om hello peering konfiguration genom att granska hello [konfigurationssidan för ExpressRoute-peering](expressroute-howto-routing-classic.md).
* Information från nätverk team eller anslutning leverantören om hello MAC-adresser hello-gränssnitt som används med IP-adresserna.
* hello senaste Windows PowerShell-modulen för Azure (version 1,50 eller senare).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP-tabeller för ExpressRoute-krets
Det här avsnittet innehåller instruktioner om hur tooview hello ARP tabeller för varje typ av peering med hjälp av PowerShell. Innan du fortsätter måste du eller anslutningsleverantören tooconfigure hello peering. Varje kretsen har två sökvägar (primär eller sekundär). Du kan kontrollera hello ARP-tabell för varje sökväg oberoende av varandra.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-tabeller för privat Azure-peering
följande cmdlet hello innehåller hello ARP tabeller för privat Azure-peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Följande är exempel på utdata för en hello sökvägar:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-tabeller för offentlig Azure-peering:
följande cmdlet hello innehåller hello ARP tabeller för offentlig Azure-peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Följande är exempel på utdata för en hello sökvägar:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Följande är exempel på utdata för en hello sökvägar:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-tabeller för Microsoft-peering
hello följande cmdlet ger hello ARP tabeller för Microsoft-peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Exempel på utdata nedan för en hello sökvägar:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a>Hur toouse informationen
hello ARP-tabell för en peering kan vara används toovalidate nivå 2-konfiguration och anslutningen. Det här avsnittet innehåller en översikt över hur ARP-tabeller ser ut i olika scenarier.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>När en anslutning är i tillståndet för operativa (förväntade) ARP-tabell
* hello ARP-tabell har en post för hello lokalt sida med en giltig IP- och MAC-adress och en liknande post för hello Microsoft sida.
* hello sista oktetten hello lokala IP-adress är alltid ett udda tal.
* hello sista oktetten av hello Microsoft IP-adressen är alltid ett jämnt tal.
* hello visas samma MAC-adress i hello Microsoft sida för alla tre peerkopplingar (primära och sekundära).

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-hello-connectivity-provider-side-has-problems"></a>När den lokala eller när hello anslutningen-provider på klientsidan har problem med ARP-tabell
 Endast en post visas i hello ARP-tabell. Den visar hello mappning mellan hello MAC och hello IP-adress som används på hello Microsoft sida.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Om det uppstår ett problem så här öppnar du en supportbegäran med din anslutning providern tooresolve begära den.
> 
> 

### <a name="arp-table-when-hello-microsoft-side-has-problems"></a>ARP-tabell när hello Microsoft sida har problem
* En ARP-tabell som visas för en peering om det finns problem på hello Microsoft sida visas inte.
* Öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Ange att du har ett problem med nivå 2-anslutningen.

## <a name="next-steps"></a>Nästa steg
* Kontrollera Layer 3 konfigurationer för ExpressRoute-krets:
  * Hämta en väg sammanfattning toodetermine hello tillståndet för BGP-sessioner.
  * Hämta en väg tabell toodetermine vilka prefix har annonserats över ExpressRoute.
* Validera dataöverföring genom att granska byte in och ut.
* Öppna en supportbegäran med [Microsoft Azure hjälp + support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om det fortfarande uppstår problem.

