---
title: "Översikt för ExpressRoute: Utöka ditt lokala nätverk tooAzure över en privat anslutning | Microsoft Docs"
description: "Den här tekniska översikten ExpressRoute beskriver hur en ExpressRoute-anslutning fungerar tooextend ditt lokala nätverk tooAzure över en privat anslutning."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: fd95dcd5-df1d-41d6-85dd-e91d0091af05
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 01301e1205c12ecdab34dc9d9b92bc7489e7826c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-overview"></a>Översikt över ExpressRoute
Microsoft Azure ExpressRoute kan du utöka ditt lokala nätverk till hello Microsoft cloud över en privat anslutning underlättas av en provider för anslutningen. Du kan upprätta anslutningar molntjänster tooMicrosoft, till exempel Microsoft Azure och Office 365 Dynamics 365 med ExpressRoute.

Anslutningen kan vara från ett ”any-to-any”-nätverk (IP VPN), ett ”point-to-point”-nätverk med Ethernet eller en virtuell korsanslutning via en anslutningsleverantör på en samlokaliseringsanläggning. ExpressRoute-anslutningar inte överskrider hello offentliga Internet. Detta tillåter ExpressRoute anslutningar toooffer mer tillförlitlighet, högre hastighet, lägre latens och högre säkerhet än vanliga anslutningar via hello Internet. Mer information om hur tooconnect ditt nätverk tooMicrosoft med ExpressRoute finns [ExpressRoute anslutning modeller](expressroute-connectivity-models.md).

![](./media/expressroute-introduction/expressroute-connection-overview.png)

## <a name="key-benefits"></a>Viktiga fördelar

* Layer 3-anslutningen mellan ditt lokala nätverk och hello Microsoft Cloud med en anslutning. Anslutningen kan vara från ett ”any-to-any”-nätverk (IPVPN), en ”point-to-point”-anslutning med Ethernet, eller med en virtuell korsanslutning via ett Ethernet-utbyte.
* Anslutningen tooMicrosoft molntjänster över alla regioner i hello geopolitiska region.
* Globala tooMicrosoft tjänster över alla regioner med ExpressRoute premium-tillägg.
* Dynamisk routning mellan ditt nätverk och Microsoft med branschens standardprotokoll (BGP).
* Inbyggd redundans i varje peeringplats för högre tillförlitlighet.
* Anslutningens drifttids-[SLA](https://azure.microsoft.com/support/legal/sla/).
* QoS-stöd för Skype för företag.

Mer information finns i hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).

## <a name="features"></a>Funktioner

### <a name="layer-3-connectivity"></a>Layer 3-anslutning
Microsoft använder industry standard dynamisk routing protocol (BGP) tooexchange dirigerar mellan ditt lokala nätverk, dina instanser i Azure och Microsoft offentliga adresser.  Vi upprättar flera BGP-sessioner med ditt nätverk för olika trafikprofiler. Mer information finns i hello [ExpressRoute-krets och routningsdomäner](expressroute-circuit-peerings.md) artikel.

### <a name="redundancy"></a>Redundans
Varje ExpressRoute-krets består av två anslutningar tootwo Microsoft Enterprise routrar i utkanten (MSEEs) från hello anslutningen providern / ditt nätverk kant. Microsoft kräver dubbla BGP-anslutning från hello anslutningen providern / din sida – en tooeach MSEE. Du kan välja inte toodeploy redundant enheter / Ethernet kretsar din slutet. Anslutningen providers använder dock redundant enheter tooensure som anslutningarna är levererat tooMicrosoft i ett redundant sätt. En redundant Layer 3 anslutningsmöjligheter konfiguration är ett krav för våra [SLA](https://azure.microsoft.com/support/legal/sla/) toobe som är giltig.

### <a name="connectivity-toomicrosoft-cloud-services"></a>Anslutningen tooMicrosoft molntjänster
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

ExpressRoute anslutningar Aktivera åtkomst toohello följande tjänster:

* Microsoft Azure-tjänster
* Microsoft Office 365-tjänster
* Microsoft Dynamics 365

Du kan besöka hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) sida för en detaljerad lista över tjänster som stöds via ExpressRoute.

### <a name="connectivity-tooall-regions-within-a-geopolitical-region"></a>Anslutningen tooall regioner inom en geopolitiska region
Du kan ansluta tooMicrosoft i ett av våra [peering platser](expressroute-locations.md) och har åtkomst tooall områden inom hello geopolitiska region. 

Om du har anslutit tooMicrosoft i Amsterdam via ExpressRoute har till exempel åtkomst tooall Microsoft-molntjänster finns i Norra Europa och västra Europa. Se hello [ExpressRoute partners och peering platser](expressroute-locations.md) artikel för en översikt över hello geopolitiska regioner, associerade Microsoft molnområdena och motsvarande platser för ExpressRoute-peering.

### <a name="global-connectivity-with-expressroute-premium-add-on"></a>Global anslutning med ExpressRoutes premiumtillägg
Du kan aktivera hello ExpressRoute premium tillägg funktionen tooextend anslutning över geopolitiska gränser. Om du är ansluten tooMicrosoft i Amsterdam via ExpressRoute kan du exempelvis har åtkomst tooall Microsoft molntjänster i alla regioner över hello world (national moln undantas). Du kan komma åt tjänster som distribueras i Sydamerika eller Australien hello samma sätt som du har åtkomst till hello Nord och Västeuropa regioner.

### <a name="rich-connectivity-partner-ecosystem"></a>Utförligt ekosystem med anslutningspartner
ExpressRoute har ett ständigt växande ekosystem med anslutningsleverantörer och SI-partners. Du kan se toohello [ExpressRoute-providers och platser](expressroute-locations.md) artikel hello senaste informationen.

### <a name="connectivity-toonational-clouds"></a>Anslutningen toonational moln
Microsoft använder isolerade molnmiljöer för särskilda geopolitiska regioner och kundsegment. Se toohello [ExpressRoute-providers och platser](expressroute-locations.md) sida för en lista över providers och nationella moln.

### <a name="bandwidth-options"></a>Bandbreddsalternativ
Du kan köpa ExpressRoute-kretsar för en mängd olika bandbredder. Listan med bandbredder som stöds finns nedan. Vara säker på att toocheck med din anslutning toodetermine hello providerlistan stöds bandbredder ger.

* 50 Mbit/s
* 100 Mbit/s
* 200 Mbit/s
* 500 Mbit/s
* 1 Gbit/s
* 2 Gbit/s
* 5 Gbit/s
* 10 Gbit/s

### <a name="dynamic-scaling-of-bandwidth"></a>Dynamisk skalning av bandbredd
Du kan öka hello ExpressRoute-kretsen bandbredd (för bästa prestanda) utan att behöva tootear ned dina anslutningar. 

### <a name="flexible-billing-models"></a>Flexibla faktureringsmodeller
Du kan välja den faktureringsmodell som passar dig bäst. Välj mellan hello fakturering modeller som anges nedan. Mer information finns i hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).

* **Obegränsad data**. Hej ExpressRoute-krets debiteras baserat på en månadsavgift och alla inkommande och utgående dataöverföring är med gratis. 
* **Avgiftsbelagda data**. Hej ExpressRoute-krets debiteras baserat på en månadsavgift. All inkommande dataöverföring är kostnadsfri. Utgående dataöverföring debiteras per GB data som överförs. Dataöverföringskostnader varierar beroende på region.
* **ExpressRoutes premiumtillägg**. Hej ExpressRoute premium är ett tillägg över hello ExpressRoute-kretsen. Hej ExpressRoute premium-tillägg ger hello följande funktioner: 
  * Ökad väg gränser för offentliga och Azure privat Azure-peering från too10 4 000 vägar, 000 vägar.
  * Global anslutning för tjänster. En ExpressRoute-krets som skapats i en region (exklusive nationella moln) har åtkomst tooresources över en annan region i hälsningsmeddelande. Till exempel kan ett virtuellt nätverk som skapats i västra Europa nås via en ExpressRoute-krets som etablerats i Silicon Valley.
  * Ökat antal VNet länkar per ExpressRoute-krets från 10 tooa större gränsen, beroende på hello bandbredden för hello krets.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

Vanliga frågor om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).

## <a name="next-steps"></a>Nästa steg

* Läs mer om [ExpressRoute-anslutningsmodeller](expressroute-connectivity-models.md).
* Läs mer om ExpressRoute-anslutningar och routningsdomäner. Se [ExpressRoute-anslutningar och routningsdomäner](expressroute-circuit-peerings.md).
* Hitta en tjänstleverantör. Se [ExpressRoute-partners och peeringplatser](expressroute-locations.md).
* Kontrollera att alla krav är uppfyllda. Se [ExpressRoute-krav](expressroute-prerequisites.md).
* Se toohello krav för [routning](expressroute-routing.md), [NAT](expressroute-nat.md), och [QoS](expressroute-qos.md).
* Konfigurera ExpressRoute-anslutningen.
  * [Skapa en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md)
  * [Konfigurera peering för en ExpressRoute-krets](expressroute-howto-routing-portal-resource-manager.md)
  * [Ansluta ett virtuellt nätverk tooan ExpressRoute-krets](expressroute-howto-linkvnet-portal-resource-manager.md)
* Lär dig mer om hello andra nyckeln [nätverk](../networking/networking-overview.md) Azure.
