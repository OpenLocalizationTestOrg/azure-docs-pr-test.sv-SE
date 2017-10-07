---
title: "aaaPrerequisites för införandet av Azure ExpressRoute | Microsoft Docs"
description: "Den här sidan innehåller en lista över kraven toobe uppfyllda innan du kan ordna Azure ExpressRoute-kretsen."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: f872d25e-acfd-405d-9d1b-dcb9f323a2ff
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/30/2017
ms.author: cherylmc
ms.openlocfilehash: 524c86f6570dc6e6505fe55323b8508e8eeff791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute-krav och checklista
tooconnect tooMicrosoft cloud services med hjälp av ExpressRoute, måste tooverify att följande krav som anges i följande avsnitt hello hello har uppfyllts.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure-konto
* Ett giltigt och aktivt Microsoft Azure-konto. Det här kontot är nödvändig tooset in hello ExpressRoute-kretsen. ExpressRoute-kretsar är resurser i Azure-prenumerationer. En Azure-prenumeration är ett krav, även om anslutningen är begränsad toonon Azure Microsofts molntjänster, till exempel Office 365-tjänster och Dynamics 365.
* En aktiv prenumeration på Office 365 (om du använder Office 365-tjänster). Mer information finns i hello [särskilda krav för Office 365](#office-365-specific-requirements) i den här artikeln.

## <a name="connectivity-provider"></a>Anslutningsleverantör

* Du kan arbeta med en [ExpressRoute anslutning partner](expressroute-locations.md#partners) tooconnect toohello Microsoft cloud. Du kan konfigurera en anslutning mellan ditt lokala nätverk och Microsoft på [tre sätt](expressroute-introduction.md).
* Om leverantören inte är en partner för ExpressRoute-anslutningen kan du fortfarande kan ansluta toohello Microsoft cloud via en [exchange molntjänstleverantör](expressroute-locations.md#connectivity-through-exchange-providers).

## <a name="network-requirements"></a>Nätverkskrav
* **Redundant anslutning**: Det krävs inte någon redundans på den fysiska anslutningen mellan dig och din leverantör. Microsoft kräver redundant BGP-sessioner toobe konfigurera mellan Microsofts routrar och hello peering routrar, även när du har precis [en fysisk anslutning tooa moln exchange](expressroute-faqs.md#onep2plink).
* **Routning**: beroende på hur du ansluter toohello Microsoft Cloud du eller din leverantör måste tooset in och hantera hello BGP-sessioner för [routningsdomänerna](expressroute-circuit-peerings.md). Vissa Ethernet-anslutningsleverantörer eller molnutbytesleverantörer kan erbjuda BGP-hantering som en mervärdestjänst.
* **NAT**: Microsoft godkänner bara offentliga IP-adresser via Microsoft-peering. Om du använder privata IP-adresser i ditt lokala nätverk kan du eller din providern behöver tootranslate hello privata IP-adresser toohello offentliga IP-adresser [med hello NAT](expressroute-nat.md).
* **QoS**: Skype för företag har olika tjänster (till exempel: röst, video, text) som kräver särskild QoS-behandling. Du och din leverantör bör följa hello [QoS-krav](expressroute-qos.md).
* **Nätverkssäkerhet**: Överväg att [nätverkssäkerhet](../best-practices-network-security.md) när du ansluter toohello Microsoft Cloud via ExpressRoute.

## <a name="office-365"></a>Office 365
Om du tänker tooenable Office 365 ExpressRoute, granska hello efter dokument för mer information om krav för Office 365.

* [Översikt över ExpressRoute för Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Routning med ExpressRoute för Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [URL:er och IP-adressintervall för Office 365](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Nätverksplanering och prestandajustering för Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Räknare och verktyg för nätverksbandbredd](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
* [Office 365-integration med lokala miljöer](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [ExpressRoute på Office 365 – avancerade utbildningsvideor](https://channel9.msdn.com/series/aer/)

## <a name="dynamics-365"></a>Dynamics 365
Om du tänker tooenable Dynamics 365 ExpressRoute, granska hello efter dokument för mer information om Dynamics 365

* [Dynamics 365 och ExpressRoute-dokumentation](http://download.microsoft.com/download/B/2/8/B2896B38-9832-417B-9836-9EF240C0A212/Microsoft%20Dynamics%20365%20and%20ExpressRoute.pdf)
* [Dynamics 365-URL:er](https://support.microsoft.com/kb/2655102) och [IP-adressintervall](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Nästa steg
* Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).
* Hitta en ExpressRoute-anslutningsleverantör. Se [ExpressRoute-partners och peeringplatser](expressroute-locations.md).
* Se toorequirements för [routning](expressroute-routing.md), [NAT](expressroute-nat.md), och [QoS](expressroute-qos.md).
* Konfigurera ExpressRoute-anslutningen.
  * [Skapa en ExpressRoute-krets](expressroute-howto-circuit-classic.md)
  * [Konfigurera routning](expressroute-howto-routing-classic.md)
  * [Länka ett VNet tooan ExpressRoute-krets](expressroute-howto-linkvnet-classic.md)
