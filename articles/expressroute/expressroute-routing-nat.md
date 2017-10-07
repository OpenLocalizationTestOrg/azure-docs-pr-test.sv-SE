---
title: "NAT för Azure ExpressRoute | Microsoft Docs"
description: "Den här sidan innehåller detaljerade krav för att konfigurera och hantera routning för ExpressRoute-kretsar."
documentationcenter: na
services: expressroute
author: osamazia
manager: ganesr
editor: 
ms.assetid: eaaf0393-d384-4496-9a5c-328e94c262a7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: osamam
ms.openlocfilehash: 7a8b760df90b545b5fbde2f614aef62dd3985bb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="nat-for-expressroute"></a>NAT för ExpressRoute

tooconnect tooMicrosoft molntjänster med hjälp av ExpressRoute, du behöver tooset in och hantera routning. Vissa anslutningsleverantörer erbjuder konfigurering och hantering av routning som en hanterad tjänst. Kontrollera med din anslutning providern toosee om de erbjuder den här tjänsten. Om de inte måste du följa toohello följande krav. 

Se toohello [kretsar och routningsdomäner](expressroute-circuit-peerings.md) artikel en beskrivning av hello routning sessioner som behöver toobe som angetts i toofacilitate anslutning.

> [!NOTE]
> Microsoft stöder inte några protokoll för router-redundans (t.ex., HSRP, VRRP) för konfigurationer med hög tillgänglighet. Vi förlitar oss på ett redundant par med BGP-sessioner per peering för hög tillgänglighet.
> 
> 

## <a name="ip-addresses-used-for-peerings"></a>IP-adresser som används för peerings

Du behöver tooreserve några block av IP-adresser tooconfigure routning mellan ditt nätverk och Microsofts Enterprise routrar i utkanten (MSEEs). Det här avsnittet innehåller en lista över kraven och beskriver hello regler för hur IP-adresserna måste förvärvas och används.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP-adresser för Azures privata peering

Du kan använda privata IP-adresser eller offentliga IP-adresser tooconfigure hello peerkopplingar. hello-adressintervall som används för att konfigurera flöden får inte överlappa med adressen adressintervall som används toocreate virtuella nätverk i Azure. 

* Du måste reservera ett /29-undernät eller två /30-undernät för routningsgränssnitten.
* hello-undernät som används för routning kan antingen vara privata IP-adresser eller offentliga IP-adresser.
* hello undernät inte vara i konflikt med hello-intervall som reserverats av hello kunden för användning i hello Microsoft cloud.
* Om ett /29-undernät används delas det upp i två /30-undernät. 
  * Hej först/30-undernät som ska användas för hello primära länken och hello andra /30 undernät kommer att användas för hello sekundära länken.
  * För varje hello /30 undernät, måste du använda hello första IP-adressen för hello /30 undernät på routern. Microsoft använder hello andra IP-adressen för hello /30 undernät tooset in BGP-sessionen.
  * Du måste ställa in både BGP-sessioner för våra [tillgänglighets-SLA](https://azure.microsoft.com/support/legal/sla/) toobe som är giltig.  

#### <a name="example-for-private-peering"></a>Exempel på privat peering

Om du väljer toouse a.b.c.d/29 tooset in hello peering delas den upp i två /30 undernät. Vi ska titta på hur hello a.b.c.d/29 undernät används i hello exemplet nedan. 

a.b.c.d/29 kommer att dela tooa.b.c.d/30 och a.b.c.d+4/30 och gått ned tooMicrosoft via hello etablering API: er. Du använder a.b.c.d+1 som hello VRF IP för hello primära PE och Microsoft kommer att använda a.b.c.d+2 som hello VRF IP för hello primära MSEE. Du använder a.b.c.d+5 som hello VRF IP för hello sekundära PE och Microsoft kommer att använda a.b.c.d+6 som hello VRF IP för hello sekundära MSEE.

Överväg att fall där du väljer 192.168.100.128/29 tooset in privat peering. 192.168.100.128/29 innehåller adresser från 192.168.100.128 too192.168.100.135, däribland:

* 192.168.100.128/30 kommer att tilldelas toolink1, med hjälp av 192.168.100.129 och Microsoft med hjälp av 192.168.100.130-providern.
* 192.168.100.132/30 kommer att tilldelas toolink2, med hjälp av 192.168.100.133 och Microsoft med hjälp av 192.168.100.134-providern.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP-adresser för Azure offentlig och Microsoft peering

Du måste använda offentliga IP-adresser som du äger för att ställa in hello BGP-sessioner. Microsoft måste vara kan tooverify hello ägarskap för hello IP-adresser via Routning Internet-register- och Internet-routning register. 

* Du måste använda ett unikt/29 undernät eller två /30 undernät tooset in hello BGP-peering för varje peering per ExpressRoute-krets (om du har fler än en). 
* Om ett /29-undernät används delas det upp i två /30-undernät. 
  * Hej först/30-undernät som ska användas för hello primära länken och hello andra /30 undernät kommer att användas för hello sekundära länken.
  * För varje hello /30 undernät, måste du använda hello första IP-adressen för hello /30 undernät på routern. Microsoft använder hello andra IP-adressen för hello /30 undernät tooset in BGP-sessionen.
  * Du måste ställa in både BGP-sessioner för våra [tillgänglighets-SLA](https://azure.microsoft.com/support/legal/sla/) toobe som är giltig.

## <a name="public-ip-address-requirement"></a>Krav för offentliga IP-adress

### <a name="private-peering"></a>Privat peering

Du kan välja toouse offentligt eller privat IPv4-adresser för privat peering. Vi erbjuder slutpunkt-till-slutpunkt isolering av din trafik så adressöverlappning med andra kunder är inte möjligt med privat peering. Dessa adresser är inte annonserade tooInternet. 

### <a name="public-peering"></a>Offentlig peering

hello Azure offentlig peering sökvägen kan du tooconnect tooall tjänster i Azure via sina offentliga IP-adresser. Dessa inkluderar tjänsterna i hello [ExpessRoute vanliga frågor och svar](expressroute-faqs.md) och tjänster ISV: er på Microsoft Azure som värd. Anslutningen tooMicrosoft Azure services på offentlig peering alltid initieras från nätverket till hello Microsoft-nätverk. Du måste använda offentliga IP-adresser för hello trafik är avsedda tooMicrosoft nätverket.

### <a name="microsoft-peering"></a>Microsoft-peering

hello Microsoft peering sökvägen kan du ansluta tooMicrosoft molntjänster som inte stöds via hello Azure offentlig peering sökväg. hello lista över tjänster inkluderar Office 365-tjänster, till exempel Exchange Online, SharePoint Online, Skype för företag och Dynamics 365. Microsoft stöder dubbelriktad anslutning på hello Microsoft-peering. Trafik tooMicrosoft molntjänster måste använda giltiga offentliga IPv4-adresser innan de kan ange hello Microsoft-nätverk.

Kontrollera att din IP-adress och som number är registrerade tooyou i något av hello register som anges nedan.

* [ARIN](https://www.arin.net/)
* [APNIC](https://www.apnic.net/)
* [AFRINIC](https://www.afrinic.net/)
* [LACNIC](http://www.lacnic.net/)
* [RIPENCC](https://www.ripe.net/)
* [RADB](http://www.radb.net/)
* [ALTDB](http://altdb.net/)

> [!IMPORTANT]
> Offentlig IP adresser annonserade tooMicrosoft via ExpressRoute inte får vara annonserade toohello Internet. Detta kan bryta anslutningen tooother Microsoft-tjänster. Offentliga IP-adresser som används av servrar i ditt nätverk som kommunicerar med O365 slutpunkter inom Microsoft kan dock annonseras över ExpressRoute. 
> 
> 

## <a name="dynamic-route-exchange"></a>Dynamiskt routningsutbyte

Routningsutbytet kommer att ske via EBGP-protokollet. EBGP sessioner upprättas mellan hello MSEEs och dina routrar. Autentisering av BGP-sessioner är inte något krav. Om det behövs kan en MD5-hash konfigureras. Se hello [konfigurera routning](expressroute-howto-routing-classic.md) och [krets etablering arbetsflöden och krets tillstånd](expressroute-workflows.md) för information om att konfigurera BGP-sessioner.

## <a name="autonomous-system-numbers"></a>Autonoma systemnummer

Microsoft använder AS 12076 för Azures offentliga, Azures privata och Microsofts peering. Vi har reserverats ASN: er från 65515 too65520 för intern användning. Både 16- och 32-bitars AS-nummer stöds.

Det finns inga krav på symmetri vid dataöverföring. hello framåtriktade och returnera sökvägar kan bläddra i olika router-par. Identiska vägar måste annonseras från någon av sidorna över flera kretspar som du äger. Vägens mått är inte obligatoriska toobe som är identiska.

## <a name="route-aggregation-and-prefix-limits"></a>Vägsammanställning och begränsningar för prefix

Vi stöder in too4000 prefix annonserade toous via hello privat Azure-peering. Detta kan du öka upp too10 000 prefix om hello ExpressRoute premium tillägget är aktiverat. Vi emot too200 prefix per BGP-sessionen för offentlig Azure och Microsoft-peering. 

hello BGP-sessionen kommer att tas bort om hello antalet prefix överskrider hello gränsen. Vi ska ta emot standardvägar på hello privat peering länk endast. Providern måste filtrera bort standardvägen, privata IP-adresser (RFC 1918) från hello Azure offentliga och Microsoft peering sökvägar. 

## <a name="transit-routing-and-cross-region-routing"></a>Överföringsroutning och routning mellan regioner

ExpressRoute kan inte konfigureras som överföringsroutrar. Du har toorely på anslutningsleverantören för transit routning tjänster.

## <a name="advertising-default-routes"></a>Annonsering av standardvägar

Standardvägar tillåts bara i Azures privata peeringsessioner. I så fall, kommer vi Vidarebefordra all trafik från hello tillhörande virtuella nätverk tooyour nätverk. Reklam standardvägar i privat peering leder hello internet sökvägen från Azure blockeras. Du måste förlita sig på företagets edge tooroute trafiken från och toohello internet för tjänster som finns i Azure. 

 tooenable anslutning tooother Azure-tjänster och infrastrukturtjänster, måste du se till något av följande objekt hello är på plats:

* Offentlig Azure-peering är aktiverat tooroute trafik toopublic slutpunkter
* Du använder användardefinierade routning tooallow internet-anslutning för varje undernät som kräver Internet-anslutning.

> [!NOTE]
> Annonsering av standardvägar bryter Windows- och andra VM-licensaktiveringar. Följ anvisningarna [här](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) toowork runt problemet.
> 
> 

## <a name="support-for-bgp-communities-preview"></a>Stöd för BGP-communities (förhandsgranskning)

Det här avsnittet innehåller en översikt över hur BGP-communities kommer att användas med ExpressRoute. Microsoft kommer att meddela vägar i hello offentliga och Microsoft peering sökvägar med vägar som är märkta med lämpliga community-värden. Hej motiveringen för att göra det och hello information om community värdena beskrivs nedan. Microsoft, men inte följer någon värden taggade tooroutes annonserade tooMicrosoft.

Om du ansluter tooMicrosoft via ExpressRoute på alla en peering plats i en geopolitiska region, har du åtkomst tooall Microsoft-molntjänster över alla regioner inom hello geopolitiska. 

Om du har anslutit tooMicrosoft i Amsterdam via ExpressRoute kommer du till exempel har åtkomst tooall Microsoft molntjänster i Norra Europa och västra Europa. 

Se toohello [ExpressRoute partners och peering platser](expressroute-locations.md) sida för en detaljerad lista över geopolitiska regioner, tillhörande Azure-regioner och motsvarande ExpressRoute-peering platser.

Du kan köpa mer än en ExpressRoute-krets per geopolitisk region. Med flera anslutningar erbjuder betydande fördelar för hög tillgänglighet på grund av toogeo-redundans. I fall där du har flera ExpressRoute-kretsar får du hello samma uppsättning prefix annonserade från Microsoft på hello offentlig peering och Microsoft peering sökvägar. Det innebär att du har flera sökvägar från ditt nätverk till Microsoft. Detta kan medföra optimalt routning beslut toobe som gjorts i nätverket. Därför kan du få bästa möjliga upplevelse toodifferent tjänster. 

Microsoft kommer att märka prefix har annonserats via offentlig peering och Microsoft peering med lämpliga värden för BGP gemenskapen som visar hello region hello prefix finns i. Du kan utgå ifrån hello community värden toomake lämplig routning beslut toooffer [optimala routning toocustomers](expressroute-optimize-routing.md).

| **Geopolitisk region** | **Microsoft Azure-region** | **BGP-community värde** |
| --- | --- | --- |
| **Nordamerika** | | |
| Östra USA |12076:51004 | |
| Östra USA 2 |12076:51005 | |
| Västra USA |12076:51006 | |
| Västra USA 2 |12076:51026 | |
| Västra centrala USA |12076:51027 | |
| Norra centrala USA |12076:51007 | |
| Södra centrala USA |12076:51008 | |
| Centrala USA |12076:51009 | |
| Centrala Kanada |12076:51020 | |
| Östra Kanada |12076:51021 | |
| **Sydamerika** | | |
| Södra Brasilien |12076:51014 | |
| **Europa** | | |
| Norra Europa |12076:51003 | |
| Västra Europa |12076:51002 | |
| **Asien och stillahavsområdet** | | |
| Östasien |12076:51010 | |
| Sydostasien |12076:51011 | |
| **Japan** | | |
| Östra Japan |12076:51012 | |
| Västra Japan |12076:51013 | |
| **Australien** | | |
| Östra Australien |12076:51015 | |
| Sydöstra Australien |12076:51016 | |
| **Indien** | | |
| Södra Indien |12076:51019 | |
| Västra Indien |12076:51018 | |
| Centrala Indien |12076:51017 | |

Alla vägar som annonserats från Microsoft märks med hello lämpliga community-värde. 

> [!IMPORTANT]
> Globala prefix taggas med ett lämpligt community-värde och annonseras endast när ExpressRoute-premiumtillägget är aktiverat.
> 
> 

Dessutom toohello ovan, Microsoft kommer också tagga prefix baserat på hello-tjänsten som de tillhör. Detta gäller endast toohello Microsoft-peering. hello tabellen nedan ger en mappning av tjänsten tooBGP gemenskapens.

| **Tjänst** | **BGP-community värde** |
| --- | --- |
| **Exchange** |12076:5010 |
| **SharePoint** |12076:5020 |
| **Skype för företag** |12076:5030 |
| **Dynamics 365** |12076:5040 |
| **Andra Office 365-tjänster** |12076:5100 |

> [!NOTE]
> BGP community värden som du angett för hello vägar annonserade tooMicrosoft följdes inte av Microsoft.
> 
> 

## <a name="next-steps"></a>Nästa steg

* Konfigurera ExpressRoute-anslutningen.
  
  * [Skapa en ExpressRoute-krets för hello klassiska distributionsmodellen](expressroute-howto-circuit-classic.md) eller [skapa och ändra en ExpressRoute-krets med Azure Resource Manager](expressroute-howto-circuit-arm.md)
  * [Konfigurera routning för hello klassiska distributionsmodellen](expressroute-howto-routing-classic.md) eller [konfigurera routning för hello Resource Manager-distributionsmodellen](expressroute-howto-routing-arm.md)
  * [Länka en klassiska VNet tooan ExpressRoute-krets](expressroute-howto-linkvnet-classic.md) eller [länka en Resource Manager VNet tooan ExpressRoute-krets](expressroute-howto-linkvnet-arm.md)

