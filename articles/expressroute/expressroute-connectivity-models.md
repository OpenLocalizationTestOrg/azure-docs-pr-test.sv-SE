---
title: "Modeller för ExpressRoute-anslutningen: ansluta tooMicrosoft Azure via nätverksleverantörer, utbyte och Ethernet-providers | Microsoft Docs"
description: "Den här artikeln beskriver hello olika lägen för anslutningen mellan hello kundens nätverk och Microsoft Azure och Office 365, Dynamics 365-tjänster. Kunder kan använda MPLS-leverantörer, molnutbyten och Ethernet-leverantörer."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2017
ms.author: cherylmc
ms.openlocfilehash: 2682e6e45b2892869068f132bedb4bb08e3f89a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-connectivity-models"></a>ExpressRoute-anslutningsmodeller
Du kan skapa en anslutning mellan ditt lokala nätverk och hello Microsoft cloud på tre olika sätt, [CloudExchange samplacering](#CloudExchange), [Point-to-point Ethernet-anslutning](#Ethernet), och [Alla-till-alla (IPVPN) anslutning](#IPVPN). Anslutningsleverantörer kan erbjuda en eller flera modeller för anslutningen. Du kan arbeta med din anslutning toopick hello leverantörsmodell som passar dig bäst.
<br><br>

![Modelldiagram för ExpressRoute-anslutning](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <a name="CloudExchange"></a>Samordnad i ett molnutbyte
Om du är samordnad på en plats med en exchange i molnet, kan du sortera virtuella cross-anslutningar toohello Microsoft cloud via hello samplacering providern Ethernet exchange. Samplacering leverantörer kan erbjuda Lager2 cross-anslutningar eller hanterade Layer 3 cross-anslutningar mellan din infrastruktur i hello samplacering anläggning och hello Microsoft cloud.

## <a name="Ethernet"></a>Anslutningar mellan punkter med Ethernet
Du kan ansluta din lokala Datacenter/kontor toohello Microsoft cloud via point-to-point Ethernet-länkar. Point-to-Point Ethernet-leverantörer kan erbjuda nivå 2-anslutningar eller hanterade Layer 3-anslutningar mellan platsen och hello Microsoft cloud.

## <a name="IPVPN"></a>”Any-to-any”-nätverk (IPVPN)
Du kan integrera din WAN med hello Microsoft cloud. IPVPN-leverantörer (vanligtvis MPLS VPN) erbjuder ”any-to-any”-anslutningar mellan dina avdelningskontor och datacentra. hello Microsoft molnet kan vara sammankopplade tooyour WAN toomake den ser ut precis som andra lokalkontoret. WAN-leverantörer erbjuder vanligtvis hanterad Layer 3-anslutning. ExpressRoute-funktioner är alla identiska för alla hello ovan connectivity-modeller. 

## <a name="next-steps"></a>Nästa steg
* Läs mer om ExpressRoute-anslutningar och routningsdomäner. Se [ExpressRoute-anslutningar och routningsdomäner](expressroute-circuit-peerings.md).
* Läs mer om ExpressRoute-funktioner. Se hello [teknisk översikt för ExpressRoute](expressroute-introduction.md)
* Hitta en tjänstleverantör. Se [ExpressRoute-partners och peeringplatser](expressroute-locations.md).
* Kontrollera att alla krav är uppfyllda. Se [ExpressRoute-krav](expressroute-prerequisites.md).
* Se toohello krav för [routning](expressroute-routing.md), [NAT](expressroute-nat.md), och [QoS](expressroute-qos.md).
* Konfigurera ExpressRoute-anslutningen.
  * [Skapa en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md)
  * [Konfigurera routning](expressroute-howto-routing-portal-resource-manager.md)
  * [Länka ett VNet tooan ExpressRoute-krets](expressroute-howto-linkvnet-portal-resource-manager.md)
