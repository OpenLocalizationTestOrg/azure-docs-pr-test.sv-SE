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
# <a name="troubleshoot-routes-using-azure-powershell"></a>Felsöka flöden med hjälp av Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

Om det uppstår problem tooor för network connectivity från din Azure virtuell dator (VM) kan vägar påverka din VM trafikflöden. Den här artikeln innehåller en översikt över diagnostikfunktionerna för vägar toohelp fortsätta felsökningen.

Vägtabeller är kopplad till undernät och börjar gälla på alla nätverksgränssnitt (NIC) i det undernätet. hello följande typer av routrar kan vara tillämpade tooeach nätverksgränssnittet:

* **Systemvägar:** varje undernät som skapats i ett Azure Virtual Network (VNet) har som standard system-routningstabeller som Tillåt lokal VNet-trafik, lokal trafik via VPN-gatewayer och Internet-trafik. Det finns också systemvägar för peerkoppla Vnet.
* **BGP-vägar:** sprids toonetwork gränssnitt via ExpressRoute eller plats-till-plats VPN-anslutningar. Mer information om BGP-routning genom att läsa hello [BGP med VPN-gatewayer](../vpn-gateway/vpn-gateway-bgp-overview.md) och [ExpressRoute översikt](../expressroute/expressroute-introduction.md) artiklar.
* **Användardefinierade vägar (UDR):** om du använder virtuella installationer i nätverket eller är Tvingad tunneltrafik trafik tooan lokalt nätverk via ett plats-till-plats-VPN, kan du ha användardefinierade vägar (udr: er) som är associerade med din routningstabellen för undernätet. Om du inte är bekant med udr: er kan läsa hello [användardefinierade vägar](virtual-networks-udr-overview.md#user-defined-routes) artikel.

Med hello olika vägar som kan vara tillämpade tooa nätverksgränssnitt, det kan vara svårt toodetermine vilka sammanställd vägar är effektiva. toohelp felsöka anslutningar för VM-nätverk kan du visa alla hello effektiva vägar för ett nätverksgränssnitt i hello Azure Resource Manager-distributionsmodellen.

## <a name="using-effective-routes-tootroubleshoot-vm-traffic-flow"></a>Med hjälp av effektiva vägar tootroubleshoot VM trafikflöde
Den här artikeln använder hello följande scenario som ett exempel tooillustrate hur tootroubleshoot hello gällande vägar för ett nätverksgränssnitt:

En virtuell dator (*VM1*) ansluten toohello VNet (*VNet1*, prefix: 10.9.0.0/16) misslyckas tooconnect tooa VM(VM3) i ett nyligen peered VNet (*VNet3*, prefixet 10.10.0.0/16). Det finns inga udr: er eller BGP-vägar tillämpade tooVM1 NIC1 nätverk gränssnitt som är anslutet toohello VM, endast systemvägar tillämpas.

Den här artikeln förklarar hur toodetermine hello orsaken till hello anslutningsfel med effektiva vägar funktionen i Azure Resource Manager-distributionsmodellen.
Medan hello exemplet används endast systemvägar, hello samma steg kan vara används toodetermine inkommande och utgående anslutningsfel via väg typer.

> [!NOTE]
> Om den virtuella datorn har mer än ett nätverkskort anslutet, kontrollerar du effektivt vägar för varje hello nätverkskort toodiagnose network connectivity problem tooand från en virtuell dator.
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a>Visa effektiva flöden för en virtuell dator
toosee hello sammanställd vägar som är kopplade tooa VM, fullständig hello följande steg:

### <a name="view-effective-routes-for-a-network-interface"></a>Visa effektiva vägar för ett nätverksgränssnitt
toosee hello sammanställd vägar som är tillämpade tooa nätverksgränssnitt, fullständig hello följande steg:

1. Starta en Azure PowerShell-sessionen och logga in tooAzure. Om du inte är bekant med Azure PowerShell kan du läsa hello [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.
2. hello följande kommando returnerar alla vägar tillämpas tooa gränssnitt med namnet *VM1 NIC1* i hello resursgruppen *RG1*.
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Om du inte vet hello namnet på ett nätverksgränssnitt, Skriv följande kommando tooretrieve hello namnen på alla nätverksgränssnitt i en resurs group.* hello
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   hello följande utdata ser ut ungefär toohello utdata för varje flöde tillämpas toohello undernät hello nätverkskortet är anslutet till:
   
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
   
   Observera följande hello i hello utdata:
   
   * **Namnet**: hello effektiva vägen får vara tomt, om inte uttryckligen anges för användardefinierade vägar. 
   * **Tillstånd**: Visar hello effektiva vägen. Möjliga värden är ”aktiv” eller ”ogiltig”
   * **Matrisen AddressPrefixes**: Anger hello-adressprefix för hello effektiva vägen i CIDR-notering. 
   * **nextHopType**: Anger hello nästa hopp för hello angivna vägen. Möjliga värden är *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, eller *Null*. Ett värde av *Null* för **nextHopType** i en UDR kan tyda på en ogiltig väg. Till exempel om **nextHopType** är *VirtualAppliance* och hello virtuell nätverksenhet virtuella datorn inte är i tillståndet etablerats/körs. Om **nextHopType** är *VPNGateway* och det finns ingen gateway etablerats/körs i hello anges VNet, hello flödet kan bli ogiltig.
   * **NextHopIpAddress**: Anger hello IP-adress hello nästa hopp för hello effektiva vägen.
   
   hello returnerar följande kommando hello vägar i en enklare tooview tabell:
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   hello är följande utdata några av hello utdata togs emot för hello-scenario som beskrivs ovan:
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. Det finns ingen väg visas toohello *WestUS VNet3* VNet (prefixet 10.10.0.0/16)** från *WestUS VNet1* (Prefix 10.9.0.0/16) i hello utdata från hello föregående steg. I följande bild hello visas hello VNet-peering länken med hello *WestUS VNet3* VNet är i hello *frånkopplad* tillstånd.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    hello dubbelriktad länk för hello peering har brutits, som förklarar varför VM1 gick inte att ansluta tooVM3 i hello *WestUS VNet3* VNet. Konfigurera en dubbelriktad VNet peering länk igen för *WestUS VNet1* och *WestUS VNet3* Vnet. hello utdatan när hello VNet-peering länken upprättas korrekt visas nedan:
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    När du bestämmer hello problemet kan du lägga till, ta bort, eller ändra vägar och routningstabeller. Skriv följande kommando toosee en lista över hello kommandon som används för toodo så hello:
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Överväganden
Några saker tookeep i åtanke när du granskar hello lista över vägar som returnerades:

* Routning baserat på längsta Prefix-matchning (LPM) bland udr: er, systemet och BGP-vägar. Om det finns fler än en väg med hello samma LPM matchar, och sedan väljs en väg baserat på dess ursprung i hello följande ordning:
  
  * Användardefinierad väg
  * BGP-väg
  * Systemväg (standard)
    
    Du kan bara se effektiva vägar som är baserat på alla hello tillgängliga vägar LPM-matchning med effektiva vägar. Genom att visa hur hello vägar faktiskt utvärderas till ett visst nätverkskort, blir det mycket enklare tootroubleshoot vägar driftstörningar anslutning till eller från den virtuella datorn.
* Om du har udr: er och skickar trafik tooa virtuell nätverksenhet (NVA) med *VirtualAppliance* som **nextHopType**, kontrollera att IP-vidarebefordring är aktiverat på hello NVA mottagande hello trafik eller paket tappas. 
* Om framtvingad tunneltrafik aktiveras, blir utgående Internet-trafik dirigeras tooon lokala. RDP/SSH från Internet tooyour VM inte fungerar med den här inställningen, beroende på hur hello lokalt hanterar den här trafiken. 
  Tvingad tunneltrafik kan aktiveras:
  * Om du använder plats-till-plats-VPN genom att ange en användardefinierad väg (UDR) med nextHopType som VPN-Gateway
  * Om en standardväg annonseras över BGP
* För VNet-peering trafik toowork korrekt en systemväg med **nextHopType** *VNetPeering* måste finnas för hello peerkoppla virtuella-prefix för intervallet. Om en sådan väg finns inte och hello VNet-peering ut länken OK:
  * Vänta några sekunder och försök igen om det är en nyetablerade peering länk. Ibland tar det längre toopropagate vägar tooall hello-nätverksgränssnitt i ett undernät.
  * Nätverkssäkerhetsgrupp (NSG) regler kan påverka hello trafikflöden. Mer information finns i hello [felsöka Nätverkssäkerhetsgrupper](virtual-network-nsg-troubleshoot-powershell.md) artikel.

