---
title: "aaaNAT krav för ExpressRoute-kretsar | Microsoft Docs"
description: "Den här sidan innehåller detaljerade krav för att konfigurera och hantera NAT för ExpressRoute-kretsar."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 867bf936-c851-485f-84c8-d8d6e33fee9f
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: 09a0e841235de3f6b85e32172d7f99f20b5baf54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-nat-requirements"></a>ExpressRoutes NAT-krav
tooconnect tooMicrosoft molntjänster med hjälp av ExpressRoute, du behöver tooset in och hantera NAT-enheter. Vissa anslutningsleverantörer erbjuder konfigurering och hantering av NAT som en hanterad tjänst. Kontrollera med din anslutning providern toosee om de erbjuder dessa tjänster. Om inte, måste du följa toohello krav som beskrivs nedan. 

Granska hello [ExpressRoute kretsar och routningsdomäner](expressroute-circuit-peerings.md) sidan tooget en översikt över hello olika routningsdomäner. toomeet hello offentliga IP-adress krav för Azure offentliga och Microsoft-peering, rekommenderar vi att du konfigurerar NAT mellan ditt nätverk och Microsoft. Det här avsnittet innehåller en detaljerad beskrivning av hello NAT infrastruktur du behöver tooset upp.

## <a name="nat-requirements-for-azure-public-peering"></a>NAT-krav för Azures offentliga peering
hello Azure offentlig peering sökvägen kan du tooconnect tooall tjänster i Azure via sina offentliga IP-adresser. Dessa inkluderar tjänsterna i hello [ExpessRoute vanliga frågor och svar](expressroute-faqs.md) och tjänster ISV: er på Microsoft Azure som värd. 

> [!IMPORTANT]
> Anslutningen tooMicrosoft Azure services på offentlig peering alltid initieras från nätverket till hello Microsoft-nätverk. Därför kan sessioner inte initieras från Microsoft Azure-tjänster tooyour nätverk via ExpressRoute. Om du använder, paket som skickats toothese annonserade IP-adresser kommer att använda hello internet i stället för ExpressRoute.
> 

Trafik tooMicrosoft Azure på offentlig peering måste vara SNATed toovalid offentliga IPv4-adresser innan de kan ange hello Microsoft-nätverk. hello bilden nedan ger en övergripande bild av hur hello NAT kan ställas in toomeet hello ovan krav.

![](./media/expressroute-nat/expressroute-nat-azure-public.png) 

### <a name="nat-ip-pool-and-route-advertisements"></a>NAT:s IP-pool och routningsmeddelanden
Du måste se till att trafik kommer in hello Azure offentlig peering sökväg med en giltig offentlig IPv4-adress. Microsoft måste vara kan toovalidate hello ägarskap för hello IPv4-NAT-adresspoolen mot ett nationellt routning Internet-register (RIR) eller en routning Internet-register (IR). En kontroll utförs utifrån hello som tal som är peerkopplat med och hello IP-adresser som används för hello NAT. Se toohello [ExpressRoute routningskrav](expressroute-routing.md) sidan information om routning register.

Det finns inga begränsningar för hello tidslängd hello NAT IP-adressprefixet annonserade via den här peering. Du måste övervaka hello NAT-pool och se till att du inte lite NAT-sessioner.

> [!IMPORTANT]
> hello NAT IP-pool annonserade tooMicrosoft inte får vara annonserade toohello Internet. Detta bryts anslutningen tooother Microsoft-tjänster.
> 
> 

## <a name="nat-requirements-for-microsoft-peering"></a>NAT-krav för Microsoft-peering
hello Microsoft peering sökvägen kan du ansluta tooMicrosoft molntjänster som inte stöds via hello Azure offentlig peering sökväg. hello lista över tjänster inkluderar Office 365-tjänster, till exempel Exchange Online, SharePoint Online, Skype för företag och Dynamics 365. Microsoft förväntar toosupport dubbelriktad anslutning på hello Microsoft-peering. Trafik tooMicrosoft molntjänster måste vara SNATed toovalid offentliga IPv4-adresser innan de kan ange hello Microsoft-nätverk. Trafik tooyour nätverket från Microsoft-molntjänster måste vara SNATed på din Internet-edge tooprevent [asymmetriska routning](expressroute-asymmetric-routing.md). hello bilden nedan ger en övergripande bild av hur hello NAT ska konfigureras för Microsoft-peering.

![](./media/expressroute-nat/expressroute-nat-microsoft.png) 

### <a name="traffic-originating-from-your-network-destined-toomicrosoft"></a>Trafik från ditt nätverk avsedda tooMicrosoft
* Du måste se till att trafiken är att ange hello Microsoft peering sökväg med en giltig offentlig IPv4-adress. Microsoft måste vara kan toovalidate hello ägare hello IPv4-NAT-adresspoolen mot hello nationellt routning internet-register (RIR) eller en routning internet-register (IR). En kontroll utförs utifrån hello som tal som är peerkopplat med och hello IP-adresser som används för hello NAT. Se toohello [ExpressRoute routningskrav](expressroute-routing.md) sidan information om routning register.
* IP-adresser som används för hello Azure offentlig peering installationsprogrammet och andra ExpressRoute-kretsar får inte vara annonserade tooMicrosoft via hello BGP-sessionen. Det finns ingen begränsning för hello tidslängd hello NAT IP-adressprefixet annonserade via den här peering.
  
  > [!IMPORTANT]
  > hello NAT IP-pool annonserade tooMicrosoft inte får vara annonserade toohello Internet. Detta bryts anslutningen tooother Microsoft-tjänster.
  > 
  > 

### <a name="traffic-originating-from-microsoft-destined-tooyour-network"></a>Trafik som skickas från Microsoft avsedda tooyour nätverk
* Vissa scenarier kräver Microsoft tooinitiate anslutning tooservice slutpunkter finns i nätverket. Ett typexempel på hello scenariot skulle vara anslutningen tooADFS servrar i nätverket från Office 365. I sådana fall måste du läcka ut rätt prefix från nätverket till hello Microsoft-peering. 
* Du måste SNAT Microsoft-trafik i hello Internet kant slutpunkter i ditt nätverk tooprevent [asymmetriska routning](expressroute-asymmetric-routing.md). Begäranden **och svar** med en mål-IP-adress som matchar en väg som tagits emot via ExpressRoute skickas alltid till ExpressRoute. Asymmetrisk routning finns om hello begäran tas emot via hello Internet med hello svar skickades via ExpressRoute. SNATing hello inkommande Microsoft-trafik i hello Internet edge tvingar svar trafik tillbaka toohello Internet-gränsen, lösa problem med hello.

![Asymmetrisk routning med ExpressRoute](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="next-steps"></a>Nästa steg
* Se toohello krav för [routning](expressroute-routing.md) och [QoS](expressroute-qos.md).
* Arbetsflödesinformation finns i [ExpressRoute-kretsens etablering av arbetsflöden och kretsstatus](expressroute-workflows.md).
* Konfigurera ExpressRoute-anslutningen.
  
  * [Skapa en ExpressRoute-krets](expressroute-howto-circuit-classic.md)
  * [Konfigurera routning](expressroute-howto-routing-classic.md)
  * [Länka ett VNet tooan ExpressRoute-krets](expressroute-howto-linkvnet-classic.md)

