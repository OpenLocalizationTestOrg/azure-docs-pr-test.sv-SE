---
title: "aaaUser användardefinierade vägar och IP-vidarebefordran i Azure | Microsoft Docs"
description: "Lär dig hur tooconfigure användardefinierade vägar (UDR) och IP-vidarebefordran tooforward trafik toonetwork virtuella installationer i Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: c39076c4-11b7-4b46-a904-817503c4b486
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f1f1d46166d5a7c776f472b7ade1354d943ece10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-defined-routes-and-ip-forwarding"></a>Användardefinierade vägar och IP-vidarebefordran

När du lägger till virtuella datorer (VM) tooa virtuella nätverk (VNet) i Azure, ser du att hello är kan toocommunicate med varandra över hello nätverk automatiskt. Du behöver inte toospecify en gateway, även om hello virtuella datorer i olika undernät. hello samma sak gäller för kommunikation från hello VMs toohello offentliga Internet och även tooyour lokala nätverk när en hybridanslutning från Azure tooyour eget datacenter finns.

Det här flödet av kommunikation är möjligt eftersom Azure använder en uppsättning system vägar toodefine hur IP-trafiken flödar. Systemvägar kontrollerar hello kommunikationsflödet i hello följande scenarier:

* Inifrån hello i samma undernät.
* Från ett undernät tooanother inom ett VNet.
* Från virtuella datorer toohello Internet.
* Från ett VNet tooanother virtuella nätverk via en VPN-gateway.
* Från ett VNet tooanother virtuella nätverk via VNet-Peering (Service länkning).
* Från ett VNet tooyour lokalt nätverk via en VPN-gateway.

hello bilden nedan visar en enkel installation med ett VNet, två undernät och ett fåtal virtuella datorer tillsammans med hello systemvägar som låter IP-trafik tooflow.

![Systemvägar i Azure](./media/virtual-networks-udr-overview/Figure1.png)

Även om hello användningen av systemvägar förenklar trafik automatiskt för din distribution, finns det fall där du vill toocontrol hello routning av paket via en virtuell installation. Du kan därför genom att skapa användardefinierade vägar som anger hello next hop för paket som flödar tooa undernätverk toogo tooyour virtuell installation i stället och aktivera IP-vidarebefordring för hello VM som körs som hello virtuell installation.

hello bilden nedan visar ett exempel på användardefinierade vägar och IP-vidarebefordring tooforce paket skickas tooone undernät från en annan toogo via en virtuell installation på ett tredje undernät.

![Systemvägar i Azure](./media/virtual-networks-udr-overview/Figure2.png)

> [!IMPORTANT]
> Användardefinierade vägar är tillämpade tootraffic lämnar ett undernät från alla resurser (t.ex. nätverksgränssnitt ansluten tooVMs) i hello undernät. Du kan inte skapa vägar toospecify hur trafik försätts i ett undernät från hello Internet, t.ex. hello-enheten som du vidarebefordrar trafik toocannot finnas i hello samma undernät där hello trafiken kommer från. Skapa alltid ett separat undernät för dina installationer. 
> 
> 

## <a name="route-resource"></a>Routeresurs
Paketen dirigeras över ett TCP/IP-nätverk baserat på en routingtabell som definierats vid varje nod i hello fysiska nätverk. En routingtabell är en samling individuella vägare används toodecide där tooforward paket utifrån hello mål IP-adress. En väg består av hello följande:

| Egenskap | Beskrivning | Villkor | Överväganden |
| --- | --- | --- | --- |
| Adressprefix |hello CIDR toowhich hello målvägen gäller till exempel 10.1.0.0/16. |Måste vara ett giltigt CIDR-intervall som representerar adresser på hello offentliga Internet, Azure-nätverk eller lokala datacenter. |Se till att hello **adressprefixet** innehåller inte hello-adress för hello **för nästa hopp**, annars hamnar dina paket i en evig loop från källan hello toohello nexthop utan att någonsin nå hello mål. |
| Nexthop-typ |hello typ av Azure-hop hello paketet ska skickas till. |Måste vara något av följande värden hello: <br/> **Virtuellt nätverk**. Representerar hello lokala virtuella nätverket. Till exempel om du har två undernät, 10.1.0.0/16 och 10.2.0.0/16 i hello samma virtuella nätverk måste hello vägen för varje undernät i vägtabellen hello nexthop-värdet för *virtuellt nätverk*. <br/> **Virtuell nätverksgateway**. Representerar en Azure S2S VPN-gateway. <br/> **Internet**. Representerar hello Internet standardgateway som tillhandahålls av hello Azure-infrastrukturen. <br/> **Virtuell installation**. Representerar en virtuell installation som du har lagt till tooyour virtuella Azure-nätverket. <br/> **Ingen**. Representerar ett svart hål. Paket som vidarebefordras tooa svart hål vidarebefordras inte alls. |Överväg att använda **virtuell installation** toodirect trafik tooa VM eller Azure belastningsutjämnare interna IP-adress.  Den här typen kan hello specificering av en IP-adress som beskrivs nedan. Överväg att använda en **ingen** skriver toostop paket från flödet tooa givet mål. |
| Nexthop-adress |hello nästa hoppadress innehåller hello IP-adressen som paket ska vidarebefordras till. Nexthop-värden tillåts bara i vägar där hello nästa hopptyp är *virtuell installation*. |Måste vara en IP-adress som kan nås i hello virtuellt nätverk där hello användardefinierad väg används, utan att gå via en **virtuell nätverksgateway**. hello IP-adress har toobe på hello samma virtuella nätverk där den används, eller på ett peered virtuellt nätverk. |Om hello IP-adressen representerar en virtuell dator, kontrollera att du aktiverar [IP-vidarebefordring](#IP-forwarding) i Azure för hello VM. Om hello IP-adress representerar hello interna IP-adressen för Azure belastningsutjämnare, kontrollera att du har en matchande regel för varje port för belastningsutjämning vill tooload saldo.|

Vissa hello ”NextHopType” värden har olika namn i Azure PowerShell:

* Virtuellt nätverk är VnetLocal
* Virtuell nätverksgateway är VirtualNetworkGateway
* Virtuell tillämpning är VirtualAppliance
* Internet är Internet
* Ingen är Ingen

### <a name="system-routes"></a>Systemvägar
Varje undernät som skapats i ett virtuellt nätverk ansluts automatiskt till en routingtabell som innehåller hello följande regler för systemvägar:

* **Lokal VNet-regel**: Den här regeln skapas automatiskt för varje undernät i ett virtuellt nätverk. Det anger att det finns en direktlänk mellan hello virtuella datorer i hello VNet och det inte finns något mellanliggande nexthop.
* **Lokal regel**: den här regeln gäller tooall trafik toohello lokala adressintervallet och använder VPN-gateway som hello nästa hopp-mål.
* **Regel för Internet**: den här regeln hanterar all trafik toohello offentliga Internet (adress prefixet 0.0.0.0/0) och använder hello infrastrukturens internet-gateway som hello nästa hopp för all trafik toohello Internet.

### <a name="user-defined-routes"></a>Användardefinierade vägar
I de flesta miljöer behöver du bara hello systemvägarna som definierats av Azure. Du kan dock behöver toocreate en routingtabell och lägga till en eller flera vägar i vissa fall, som:

* Tvingad tunneltrafik toohello Internet via ditt lokala nätverk.
* Använda virtuella installationer i din Azure-miljö.

I ovanstående hello scenarier, du har toocreate en routingtabell och lägga till användardefinierade vägar tooit. Du kan ha flera vägtabeller och hello samma routingtabell kan vara associerade tooone eller flera undernät. Och varje undernät kan bara vara associerad tooa enda vägtabell. Alla virtuella datorer och molntjänster i ett undernät kan du använda hello flödet tabellen associerad toothat undernätet.

Undernät förlitar sig på systemvägar tills en routingtabell är associerade toohello undernät. När det finns en koppling, så sköts routningen baserat på längsta prefix-matchning (LPM) bland användardefinierade vägar och systemvägar. Om det finns baserat fler än en väg med samma LPM matchar sedan väljs en väg hello på dess ursprung i hello följande ordning:

1. Användardefinierad väg
2. BGP-väg (när ExpressRoute används)
3. Systemväg

toolearn hur toocreate användardefinierade vägar finns [hur tooCreate vägar och aktiverar IP-vidarebefordran i Azure](virtual-network-create-udr-arm-template.md).

> [!IMPORTANT]
> Användardefinierade vägar är endast tillämpade tooAzure virtuella datorer och molntjänster. Till exempel om du vill tooadd en virtuell brandväggsinstallation mellan ditt lokala nätverk och Azure, behöver toocreate en användardefinierad väg för dina Azure-routningstabeller som vidarebefordrar all trafik toohello lokal adressutrymme toohello virtuella installation. Du kan också lägga till en användardefinierad väg (UDR) toohello GatewaySubnet tooforward all trafik från lokala tooAzure via hello virtuell installation. Det här är ett nytt tillägg.
> 
> 

### <a name="bgp-routes"></a>BGP-vägar
Om du har en ExpressRoute-anslutning mellan ditt lokala nätverk och Azure, kan du aktivera toopropagate BGP-vägar från ditt lokala nätverk tooAzure. BGP-vägarna används i hello samma sätt som systemvägar och användaren definierats vägar i varje Azure-undernät. Mer information finns i [Introduktion till ExpressRoute](../expressroute/expressroute-introduction.md).

> [!IMPORTANT]
> Du kan konfigurera din Azure-miljön toouse Tvingad tunneltrafik via ditt lokala nätverk genom att skapa en användardefinierad väg för undernätet 0.0.0.0/0 som använder hello VPN-gateway som hello nästa hopp. Det fungerar dock bara om du använder en VPN-gateway, inte ExpressRoute. Tvingad tunneling med ExpressRoute konfigureras via BGP.
> 
> 

## <a name="ip-forwarding"></a>IP-vidarebefordran
Som beskrivs ovan, en hello huvudsakliga skälen toocreate är en användardefinierad väg tooforward trafik tooa virtuell installation. En virtuell installation finns inget mer än en virtuell dator som kör ett program som används för toohandle nätverkstrafik på något sätt, till exempel en brandvägg eller NAT-enhet.

Den här virtuella installations VM måste vara kan tooreceive inkommande trafik som är adresserad inte tooitself. tooallow en tooreceive VM-trafik adresserad tooother mål, måste du aktivera IP-vidarebefordring för hello VM. Det här är en Azure-inställning, inte en inställning i hello gästoperativsystemet.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[skapar vägar i Resource Manager-modellen hello](virtual-network-create-udr-arm-template.md) och kopplar dem till toosubnets. 
* Lär dig hur för[skapar vägar i hello klassiska distributionsmodellen](virtual-network-create-udr-classic-ps.md) och kopplar dem till toosubnets.

