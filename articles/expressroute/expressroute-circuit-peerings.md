---
title: "aaaAzure ExpressRoute kretsar och routningsdomäner | Microsoft Docs"
description: "Den här sidan innehåller en översikt över ExpressRoute-kretsar och routningsdomäner hello."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6f0c5d8e-cc60-4a04-8641-2c211bda93d9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 1d43cbf668accdd7aa4efb053ea1e9027d10e4a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-circuits-and-routing-domains"></a>ExpressRoute-kretsar och routningsdomäner
 Måste du sortera en *ExpressRoute-krets* tooconnect din lokala infrastruktur tooMicrosoft med en anslutning. hello bilden nedan ger en logisk representation av anslutningen mellan din WAN och Microsoft.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>ExpressRoute-kretsar
En *ExpressRoute-krets* representerar en logisk anslutning mellan din lokala infrastruktur och dina Microsoft-molntjänster med en anslutning. Du kan ordna flera ExpressRoute-kretsar. Varje krets kan vara i hello samma eller olika regioner, och kan vara anslutna tooyour lokala via olika anslutningen leverantörer. 

ExpressRoute-kretsar mappa inte tooany fysiska enheter. En anslutning identifieras unikt med en standard GUID kallas för en nyckel (s-nyckel). hello nyckel är hello enda information som utbyts mellan Microsoft hello anslutningen provider och du. hello s-nyckeln är inte en hemlighet av säkerhetsskäl. Det finns en 1:1-mappning mellan en ExpressRoute-krets och hello s-nyckel.

En ExpressRoute-krets kan ha upp toothree oberoende peerkopplingar: Azure offentlig, Azure privata och Microsoft. Varje peering är två oberoende BGP sessioner av dem redundant konfigureras för hög tillgänglighet. Det finns en 1: n (1 < = N < = 3) mappning mellan en ExpressRoute-krets och routning domäner. En ExpressRoute-krets kan ha en, två eller alla tre peerkopplingar aktiveras per ExpressRoute-kretsen.

Varje kretsen har en fast bandbredd (50 Mbit/s, 100 Mbit/s, 200 Mbit/s, 500 Mbit/s, 1 Gbit/s, 10 Gbit/s) och är mappade tooa anslutning provider och en peering plats. hello bandbredd som du väljer delas mellan alla hello peerkopplingar för hello krets. 

### <a name="quotas-limits-and-limitations"></a>Kvoter, gränser och begränsningar
Standardkvoter och gränser gäller för alla ExpressRoute-kretsen. Se toohello [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md) för aktuell information om kvoter.

## <a name="expressroute-routing-domains"></a>ExpressRoute-routningsdomäner
En ExpressRoute-krets har flera routningsdomäner associerad: Azure offentlig, Azure privata och Microsoft. Var och en av hello routningsdomäner har konfigurerats identiskt på ett par av routrar (aktiv-aktiv eller läsa in delar configuration) för hög tillgänglighet. Azure-tjänster är kategoriserade som *Azure offentliga* och *Azure privata* toorepresent hello IP-adressering scheman.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="private-peering"></a>Privat peering
Azure compute tjänster, nämligen virtuella datorer (IaaS) och molntjänster (PaaS), som har distribuerats i ett virtuellt nätverk kan vara ansluten via hello privat peering domän. hello privat peering domän anses toobe en betrodd förlängning av Kärnnätverket i Microsoft Azure. Du kan ställa in dubbelriktad anslutning mellan ditt core nätverk och virtuella Azure-nätverk (Vnet). Den här peering kan du ansluta toovirtual datorer och molntjänster direkt på sina privata IP-adresser.  

Du kan ansluta flera virtuella nätverk toohello privat peering domäner. Granska hello [vanliga frågor om sidan](expressroute-faqs.md) information om gränser och begränsningar. Du kan besöka hello [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md) för aktuell information om begränsningar.  Se toohello [routning](expressroute-routing.md) för detaljerad information om routningskonfiguration.

### <a name="public-peering"></a>Offentlig peering
Tjänster som Azure Storage, SQL-databaser och webbplatser erbjuds på offentliga IP-adresser. Privat kan du ansluta tooservices som finns på den offentliga IP-adresser, inklusive VIP av dina molntjänster via hello offentlig peering routningsdomän. Du kan ansluta hello offentlig peering domän tooyour DMZ och ansluta tooall Azure-tjänster på sina offentliga IP-adresser från din WAN utan tooconnect via hello internet. 

Anslutningen är alltid initieras från din WAN tooMicrosoft Azure services. Microsoft Azure-tjänster inte kan tooinitiate anslutningar till nätverket via den här routningsdomän. När offentlig peering är aktiverat, kommer du att kunna tooconnect tooall Azure services. Vi tillåter inte att tooselectively Välj tjänster som vi annonserar vägar till. Du kan granska hello listan över prefix som vi annonsera tooyou via den här peering på hello [IP-intervall i Microsoft Azure Datacenter](http://www.microsoft.com/download/details.aspx?id=41653) sidan. hello sidan uppdateras varje vecka.

Du kan definiera filter för anpassad routning inom din tooconsume endast hello nätverksvägar du behöver. Se toohello [routning](expressroute-routing.md) för detaljerad information om routningskonfiguration. 

Se hello [vanliga frågor om sidan](expressroute-faqs.md) mer information om tjänster som stöds via hello offentlig peering routningsdomän. 

### <a name="microsoft-peering"></a>Microsoft-peering
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Anslutningen tooall andra Microsoft online services (till exempel Office 365-tjänster) blir via hello Microsoft-peering. Vi Aktivera dubbelriktad anslutning mellan dina WAN och Microsoft-molntjänster via hello Microsoft peering routningsdomän. Du måste ansluta tooMicrosoft molntjänster endast med offentliga IP-adresser som ägs av dig eller anslutningsleverantören och du måste följa tooall hello definierade regler. Se hello [ExpressRoute krav](expressroute-prerequisites.md) för mer information.

Se hello [vanliga frågor om sidan](expressroute-faqs.md) för mer information om tjänster som stöds, kostnader och konfigurationsinformation. Se hello [ExpressRoute platser](expressroute-locations.md) sidan information om hello lista över anslutningen leverantörer som erbjuder peering Microsoft-supporten.

## <a name="routing-domain-comparison"></a>Routning domän jämförelse
hello tabellen nedan jämförs hello tre routningsdomäner.

|  | **Privat Peering** | **Offentlig Peering** | **Microsoft-Peering** |
| --- | --- | --- | --- |
| **Max. # prefix stöd per peering** |4000 som standard 10 000 med ExpressRoute Premium |200 |200 |
| **IP-adressintervall som stöds** |En giltig IPv4-adress inom din WAN. |Offentliga IPv4-adresser som ägs av dig eller anslutningsleverantören. |Offentliga IPv4-adresser som ägs av dig eller anslutningsleverantören. |
| **SOM antal krav** |Privata och offentliga som tal. Du måste äga hello offentliga som tal om du väljer toouse en. |Privata och offentliga som tal. Du måste dock bevisa ägarskapet för offentliga IP-adresser. |Privata och offentliga som tal. Du måste dock bevisa ägarskapet för offentliga IP-adresser. |
| **Routing gränssnitts-IP-adresser** |RFC1918 och offentliga IP-adresser |Offentliga IP-adresser registrerade tooyou i Routning register. |Offentliga IP-adresser registrerade tooyou i Routning register. |
| **MD5-Hash-support** |Ja |Ja |Ja |

Du kan välja tooenable en eller flera av hello routningsdomäner som en del av ExpressRoute-kretsen. Du kan välja toohave alla hello routningsdomäner som spärra hello samma VPN om du vill toocombine dem i en enda routningsdomän. Du kan också publicera dem på olika routningsdomäner liknande toohello diagram. hello bör konfiguration är att privat peering är ansluten direkt toohello Kärnnätverket och hello offentliga och Microsoft peering länkar är anslutna tooyour DMZ.

Om du väljer toohave alla tre peeringsessioner, måste du ha tre par BGP-sessioner (ett par för varje typ av peering). hello BGP-sessionen par ger en hög tillgänglighet länk. Om du ansluter via layer 2 anslutningen leverantörer ansvarar du för att konfigurera och hantera routning. Du kan lära dig mer genom att granska hello [arbetsflöden](expressroute-workflows.md) för att ställa in ExpressRoute.

## <a name="next-steps"></a>Nästa steg
* Hitta en tjänstleverantör. Se [ExpressRoute service providers och platser](expressroute-locations.md).
* Kontrollera att alla krav är uppfyllda. Se [ExpressRoute-krav](expressroute-prerequisites.md).
* Konfigurera ExpressRoute-anslutningen.
  * [Skapa och hantera ExpressRoute-kretsar](expressroute-howto-circuit-portal-resource-manager.md)
  * [Konfigurera routning (peering) för ExpressRoute-kretsar](expressroute-howto-routing-portal-resource-manager.md)

