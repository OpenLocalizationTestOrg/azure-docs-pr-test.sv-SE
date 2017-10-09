---
title: "aaaOverview för hög tillgänglighet konfigurationer med Azure VPN-gatewayer | Microsoft Docs"
description: "Den här artikeln ger en översikt över konfigurationsalternativen för hög tillgänglighet med Azure VPN-gateways."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2016
ms.author: yushwang
ms.openlocfilehash: 316293b9ac79645bf9bb9e89fbc4aa8f3eacd209
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Anslutning med hög tillgänglighet på flera platser och VNet-till-VNet-anslutning
Den här artikeln ger en översikt över konfigurationsalternativen för anslutning på flera platser och VNet-till-VNet-anslutning med Azure VPN-gateways.

## <a name = "activestandby"></a>Om Azure VPN gateway-redundans
Varje Azure VPN-gateway består av två instanser i en aktiv-standby-konfiguration. För att planerat underhåll och oplanerade avbrott som händer toohello aktiva instansen, skulle hello vänteläge instansen ta över (failover) automatiskt och återuppta hello S2S VPN eller VNet-till-VNet-anslutningar. hello växla kommer att orsaka ett kort avbrott. Hello anslutningen ska återställas inom 10 too15 sekunder för planerat underhåll. Hello anslutning återställning är längre om 1 minut too1 för oplanerade problem och en halv minuter i hello värsta fall. För P2S VPN-klienten anslutningar toohello gateway kommer att kopplas från hello P2S-anslutningar och hello användare behöver tooreconnect från hello-klientdatorer.

![Aktiv-standby](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Anslutning med hög tillgänglighet på flera platser
tooprovide bättre tillgänglighet för din mellan lokala anslutningar, ett par olika alternativ är tillgängliga:

* Flera lokala VPN-enheter
* Aktiv-aktiv Azure VPN-gateway
* En kombination av båda

### <a name = "activeactiveonprem"></a>Flera lokala VPN-enheter
Du kan använda flera VPN-enheter från din lokala nätverk tooconnect tooyour Azure VPN-gateway, som visas i följande diagram hello:

![Flera lokala VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Den här konfigurationen innehåller flera aktiva tunnlar från hello samma Azure VPN gateway tooyour lokala enheter i hello samma plats. Det finns vissa krav och begränsningar:

1. Du måste toocreate flera S2S VPN-anslutningar från tooAzure din VPN-enheter. När du ansluter flera VPN-enheter från hello samma lokala nätverk tooAzure, behöver du toocreate en lokal nätverksgateway för varje VPN-enhet, och en anslutning från din Azure VPN gateway toohello lokala nätverksgateway.
2. hello lokala nätverksgatewayer motsvarande tooyour VPN-enheter måste ha unika offentliga IP-adresser i hello ”GatewayIpAddress”-egenskap.
3. BGP krävs för den här konfigurationen. Varje lokal nätverksgateway som representerar en VPN-enhet måste ha en unik BGP peer-IP-adress anges i hello ”BgpPeerIpAddress”-egenskap.
4. Hej AddressPrefix egenskapsfältet i varje lokal nätverksgateway får inte överlappa. Du måste ange hello ”BgpPeerIpAddress” i /32 CIDR-format i hello AddressPrefix fält, till exempel 10.200.200.254/32.
5. Du bör använda BGP tooadvertise hello samma prefix för hello samma lokala nätverk prefix tooyour Azure VPN-gateway och vidarebefordras hello trafik genom tunnlarna samtidigt.
6. Varje anslutning är räknas av mot hello maximala antalet tunnlar för din Azure VPN-gateway, 10 för Basic och Standard SKU: er och 30 för HighPerformance SKU. 

I den här konfigurationen hello Azure VPN-gateway är fortfarande i aktivt vänteläge läge, så hello samma funktion och korta avbrott sker ändå enligt [ovan](#activestandby). Men konfigurationen skyddar mot fel och avbrott i ditt lokala nätverk och dina VPN-enheter.

### <a name="active-active-azure-vpn-gateway"></a>Aktiv-aktiv Azure VPN-gateway
Du kan nu skapa en Azure VPN-gateway i en aktiv-aktiv-konfiguration där båda instanser av hello gateway VMs upprättar S2S VPN-tunnlar tooyour lokala VPN-enhet, som visas hello följande diagram:

![Aktiv-aktiv](./media/vpn-gateway-highlyavailable/active-active.png)

Varje Azure gateway-instans har en unik offentliga IP-adress i den här konfigurationen, och varje upprättar en IPsec/IKE S2S VPN-tunnel tooyour lokala VPN-enheten som anges i din lokala nätverksgatewayen och anslutningen. Observera att både VPN-tunnlar är en del av hello samma anslutning. Du kommer fortfarande behöver tooconfigure tooaccept din lokala VPN-enhet eller upprätta två S2S VPN-tunnlar toothose två Azure VPN gateway offentliga IP-adresser.

Eftersom hello Azure gateway-instanser i aktiv-aktiv konfiguration, hello trafik från din virtuella Azure-nätverket tooyour lokalt nätverk kommer att dirigeras via båda tunnlar samtidigt, även om din lokala VPN-enhet kan ge företräde åt en tunnel över hello andra. Observera även om hello kommer alltid att passerar samma TCP eller UDP-flöde hello samma tunnel eller sökväg, såvida inte en underhållshändelse sker på en av hello instanser.

När en planerat underhåll eller oplanerad händelse inträffar tooone gateway-instans hello IPsec-tunneln från instans tooyour lokala VPN-enhet kommer att kopplas från. hello motsvarande vägar på VPN-enheter ska tas bort eller återkallas automatiskt så att hello trafik ska växlas över toohello andra aktiva IPsec-tunneln. Hello växla sker automatiskt från hello påverkas instans toohello active instans på hello Azure sida.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Dubbel redundans: aktiv-aktiv VPN-gateways för både Azure och lokala nätverk
hello säkraste alternativet är toocombine hello aktiv-aktiv gateways på ditt nätverk och ett Azure, enligt hello diagrammet nedan.

![Dubbel redundans](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Här du skapar och konfigurera hello Azure VPN-gateway i en aktiv-aktiv konfiguration och skapar två lokala nätverksgatewayer och två anslutningar för din två lokala VPN-enheter som beskrivs ovan. hello resultatet är en helt nätverksanslutningen för 4 IPsec-tunnlar mellan dina virtuella Azure-nätverket och ditt lokala nätverk.

Alla gatewayer och tunnlar aktiva från hello Azure sida, så att trafik hello sprids över alla 4 tunnlar samtidigt, även om varje TCP eller UDP flöda kommer igen Följ hello samma tunnel eller sökvägen från hello Azure sida. Trots att du kan se något bättre genomflöde över hello IPsec-tunnlar genom att sprida hello trafik, är hello syftet med den här konfigurationen för hög tillgänglighet. Och på grund av toohello statistiska uppbyggnad hello sprida, är det svårt tooprovide hello mått på hur olika programtrafik villkor påverkar hello sammanställda genomflöde.

Den här topologin kräver två lokala nätverksgatewayer och två anslutningar toosupport hello par med lokala VPN-enheter och BGP är obligatoriska tooallow hello två anslutningar toohello samma lokala nätverk. Dessa krav är hello samma som hello [ovan](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>VNet-till-VNet-anslutningar med hög tillgänglighet via Azure VPN-gateways
hello kan samma aktiv-aktiv konfiguration också använda tooAzure VNet-till-VNet-anslutningar. Du kan skapa aktiv-aktiv VPN-gatewayer för både virtuella nätverk och Anslut dem tillsammans tooform hello samma fullständig nät anslutningen för 4 tunnlar mellan hello två Vnet, enligt hello diagrammet nedan:

![VNet-till-VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Detta säkerställer att det finns alltid ett par med tunnlar mellan hello två virtuella nätverk för alla händelser som planerat underhåll, tillhandahåller ännu bättre tillgänglighet. Även om hello samma topologi för korsanslutningar kräver två anslutningar, behöver hello VNet-till-VNet-topologi som visas ovan bara en anslutning för varje gateway. Dessutom är BGP valfri såvida transitroutning över hello VNet-till-VNet-anslutning krävs.

## <a name="next-steps"></a>Nästa steg
Se [konfigurera aktiv-aktiv VPN-gatewayer för anslutningar mellan platser och VNet-till-VNet-anslutningar](vpn-gateway-activeactive-rm-powershell.md) åtgärder tooconfigure aktiv-aktiv mellan platser och VNet-till-VNet-anslutningar.

