---
title: "aaaAzure ExpressRoute vanliga frågor och svar | Microsoft Docs"
description: "Hej ExpressRoute vanliga frågor och svar innehåller information om stöd för Azure-tjänster, kostnad, Data och anslutningar, SERVICENIVÅAVTAL, leverantörer och platser, bandbredd och ytterligare teknisk information."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 09b17bc4-d0b3-4ab0-8c14-eed730e1446e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: c01e83f1497103e2fa85251dce6fb41844e46e9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-faq"></a>Vanliga frågor och svar för ExpressRoute

## <a name="what-is-expressroute"></a>Vad är ExpressRoute?

ExpressRoute är en Azure-tjänst som låter dig skapa privata anslutningar mellan Microsoft-datacenter och den infrastruktur som är lokalt eller i en samordning anläggning. ExpressRoute-anslutningar överskrider inte hello offentliga Internet och ger högre säkerhet, tillförlitlighet och hastighet med minskar svarstider än vanliga anslutningar över hello Internet.

### <a name="what-are-hello-benefits-of-using-expressroute-and-private-network-connections"></a>Vad är hello fördelarna med att använda ExpressRoute- och privata nätverksanslutningar?

ExpressRoute-anslutningar inte överskrider hello offentliga Internet. De ger högre säkerhet, tillförlitlighet och hastighet med lägre och konsekvent svarstider än vanliga anslutningar över hello Internet. I vissa fall kan använda ExpressRoute anslutningar tootransfer data mellan lokala enheter och Azure kan ge betydande kostnadsfördelar.

### <a name="where-is-hello-service-available"></a>Var finns hello tjänsten?

Se den här sidan för tjänstlokalisering och tillgänglighet: [ExpressRoute partners och platser](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-tooconnect-toomicrosoft-if-i-dont-have-partnerships-with-one-of-hello-expressroute-carrier-partners"></a>Hur kan jag använda ExpressRoute tooconnect tooMicrosoft om jag inte har partnerskap med en partner för hello ExpressRoute-operatör?

Du kan välja ett regionalt operatör och mark Ethernet-anslutningar tooone hello stöds Exchange-providerplatser. Du kan sedan peer med Microsoft hello providern plats. Kontrollera hello sista avsnittet i [ExpressRoute partners och platser](expressroute-locations.md) toosee om tjänstleverantören finns i någon av hello exchange platser. Du kan sedan beställa en ExpressRoute-krets via hello service provider tooconnect tooAzure.

### <a name="how-much-does-expressroute-cost"></a>Hur mycket kostar ExpressRoute?

Kontrollera [prisinformationen](https://azure.microsoft.com/pricing/details/expressroute/) information om priser.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-hello-vpn-connection-i-purchase-from-my-network-service-provider-have-toobe-hello-same-speed"></a>Om jag betala för en ExpressRoute-krets för en viss bandbredd hello jag köpa från leverantören för nätverket för VPN-anslutningen har toobe hello samma hastighet?

Nej. Du kan köpa en VPN-anslutning med en hastighet från leverantören. Din anslutning tooAzure är dock begränsad toohello ExpressRoute-kretsen bandbredd som du köper.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-hello-ability-tooburst-up-toohigher-speeds-if-necessary"></a>Måste jag hello möjlighet tooburst in toohigher hastigheter vid behov om jag betala för en viss bandbredd en ExpressRoute-krets?

Ja. ExpressRoute-kretsar är konfigurerade tooallow tooburst in tootwo gånger hello Bandbreddsgräns som du angav för utan extra kostnad. Kontakta din service provider toosee om de stöder denna funktion.

### <a name="can-i-use-hello-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Kan jag använda hello samma privata nätverksanslutning med virtuella nätverk och andra Azure-tjänster samtidigt?

Ja. En ExpressRoute-krets, anges en gång, kan du tooaccess tjänster inom ett virtuellt nätverk och andra Azure-tjänster samtidigt. Du ansluter toovirtual nätverk över hello privat peering sökväg och tooother tjänster via hello offentlig peering sökväg.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>Erbjuder ExpressRoute en serviceavtalet (SLA)?

Mer information finns i hello [ExpressRoute SLA](https://azure.microsoft.com/support/legal/sla/) sidan.

## <a name="supported-services"></a>Tjänster som stöds

Har stöd för ExpressRoute [tre routningsdomäner](expressroute-circuit-peerings.md) för olika typer av tjänster.

### <a name="private-peering"></a>Privat peering

* Virtuella nätverk, inklusive alla virtuella datorer och molntjänster

### <a name="public-peering"></a>Offentlig peering

* Power BI
* Dynamics 365 för ekonomi och åtgärder (kallades Dynamics AX Online)
* De flesta av hello Azure tjänster med hello följa några undantag:
  * CDN
  * Visual Studio Team Services belastningen testning
  * Multifaktorautentisering
  * Traffic Manager

### <a name="microsoft-peering"></a>Microsoft-peering

* [Office 365](http://aka.ms/ExpressRouteOffice365)
* Dynamics 365 kunden Engagement-program (kallades CRM Online)
  * Dynamics 365 för försäljning
  * Dynamics 365 för kundservice
  * Dynamics 365 för fältet Service
  * Dynamics 365 för Project Service

## <a name="data-and-connections"></a>Data och anslutningar

### <a name="are-there-limits-on-hello-amount-of-data-that-i-can-transfer-using-expressroute"></a>Finns det någon gräns på hello mängden data som jag kan överföra med ExpressRoute?

Vi Ange inte en gräns på hello mängd av dataöverföring. Se för[prisinformationen](https://azure.microsoft.com/pricing/details/expressroute/) information om priser för bandbredd.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Vilka anslutningshastigheter stöds av ExpressRoute?

Bandbredd-erbjudanden som stöds:

50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 2 Gbps, 5 Gbps, 10 Gbps

### <a name="which-service-providers-are-available"></a>Vilka leverantörer som är tillgängliga?

Se [ExpressRoute partners och platser](expressroute-locations.md) hello lista över leverantörer och platser.

## <a name="technical-details"></a>Teknisk information

### <a name="what-are-hello-technical-requirements-for-connecting-my-on-premises-location-tooazure"></a>Vad är hello tekniska krav för att ansluta min lokala plats tooAzure?

Se [ExpressRoute förutsättningssidan](expressroute-prerequisites.md) krav.

### <a name="are-connections-tooexpressroute-redundant"></a>Är anslutningar tooExpressRoute redundant?

Ja. Varje ExpressRoute-kretsen har ett redundant par mellan tooprovide anslutningar som konfigurerats med hög tillgänglighet.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Kommer det att förlora anslutningen om en av länkarna ExpressRoute misslyckas?

Du kommer inte att förlora anslutningen om någon av hello mellan anslutningar misslyckas. En redundant anslutning är tillgänglig toosupport hello belastningen på nätverket. Dessutom kan du skapa flera kretsar i en annan peering plats tooachieve fel återhämtning.

### <a name="onep2plink"></a>Om jag vill inte samordnas på en exchange i molnet och leverantören erbjuder Point-to-point-anslutning, måste tooorder två fysiska anslutningar mellan min lokala nätverk och Microsoft?

Om leverantören kan upprätta två Ethernet virtuella kretsar över hello fysiska anslutningen, behöver du bara en fysisk anslutning. hello fysiska anslutningen (till exempel en optical fiber) avslutas på ett lager 1 (L1)-enhet (se bild hello). hello två Ethernet virtuella kretsar är märkta med olika VLAN-ID, ett för hello primära krets, och en för hello sekundär. Dessa VLAN-ID anges i hello yttre 802.1Q Ethernet-huvudet. hello inre 802.1Q Ethernet-huvudet (visas inte) är mappade tooa specifika [ExpressRoute routningsdomän](expressroute-circuit-peerings.md).

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)

### <a name="can-i-extend-one-of-my-vlans-tooazure-using-expressroute"></a>Kan jag utöka en min VLAN tooAzure med ExpressRoute?

Nej. Vi stöder inte tillägg för layer-2 anslutningen till Azure.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Kan jag har fler än en ExpressRoute-krets i min prenumeration?

Ja. Du kan ha fler än en ExpressRoute-krets i din prenumeration. hello Standardgränsen anges too10. Om det behövs kan du kontakta Microsoft Support tooincrease hello gränsen.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Kan jag ha ExpressRoute-kretsar från olika leverantörer?

Ja. Du kan ha ExpressRoute-kretsar med många leverantörer. Varje ExpressRoute-kretsen är associerad med endast en leverantör. 

### <a name="can-i-have-multiple-expressroute-circuits-in-hello-same-location"></a>Kan jag har flera ExpressRoute-kretsar i hello samma plats?

Ja. Du kan ha flera ExpressRoute-kretsar, hello med samma eller olika leverantörer i hello samma plats. Men du kan inte länka mer än en ExpressRoute-kretsen toohello samma virtuella nätverk från hello samma plats.

### <a name="how-do-i-connect-my-virtual-networks-tooan-expressroute-circuit"></a>Hur ansluter mitt virtuella nätverk tooan ExpressRoute-krets

hello stegen är:

* Upprätta en ExpressRoute-krets och har hello-leverantör aktivera den.
* Du eller hello-providern, måste konfigurera hello BGP peering (s).
* Länka hello virtuellt nätverk toohello ExpressRoute-kretsen.

Mer information finns i [kretsetablering och kretsstatus i ExpressRoute-arbetsflöden](expressroute-workflows.md).

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>Finns det anslutningen gränser för min ExpressRoute-krets?

Ja. Hej [ExpressRoute partners och platser](expressroute-locations.md) artikeln innehåller en översikt över hello anslutningen gränser för en ExpressRoute-krets. Anslutning för en ExpressRoute-krets är begränsad tooa enda geopolitiska region. Anslutningen kan vara expanderade toocross geopolitiska regioner genom att aktivera hello ExpressRoute premium-funktion.

### <a name="can-i-link-toomore-than-one-virtual-network-tooan-expressroute-circuit"></a>Kan jag koppla toomore än ett virtuellt nätverk tooan ExpressRoute-krets?

Ja. Du kan ha webbplatsanslutningar too10 virtuella nätverk på en standard ExpressRoute-krets och uppåt too100 på en [premium ExpressRoute-krets](#expressroute-premium). 

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-tooa-single-expressroute-circuit"></a>Jag har flera Azure-prenumerationer som innehåller virtuella nätverk. Kan jag ansluta virtuella nätverk som finns i separata prenumerationer tooa enda ExpressRoute-krets?

Ja. Du kan tillåta dig too10 andra Azure-prenumerationer toouse en enda ExpressRoute-krets. Den här gränsen kan ökas genom att aktivera hello ExpressRoute premium-funktion.

Mer information finns i [dela en ExpressRoute-krets över flera prenumerationer](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-toohello-same-circuit-isolated-from-each-other"></a>Är virtuella nätverk anslutet toohello samma krets isolerade från varandra?

Nej. Från en routning perspektiv, alla virtuella nätverk länkade toohello samma ExpressRoute-kretsen är en del av samma routningsdomän hello och inte är isolerade från varandra. Om du behöver vidarebefordra isolering måste toocreate en separat ExpressRoute-krets.

### <a name="can-i-have-one-virtual-network-connected-toomore-than-one-expressroute-circuit"></a>Kan jag ha ett virtuellt nätverk anslutet toomore än en ExpressRoute-krets?

Ja. Du kan länka ett enda virtuellt nätverk med in toofour ExpressRoute-kretsar. De måste beställas via fyra olika [ExpressRoute platser](expressroute-locations.md).

### <a name="can-i-access-hello-internet-from-my-virtual-networks-connected-tooexpressroute-circuits"></a>Kan jag använda hello Internet från mitt virtuella nätverk anslutet tooExpressRoute kretsar?

Ja. Om du inte har annonseras standardvägar (0.0.0.0/0) eller Internet väg prefix via hello BGP-sessionen, kan du ansluta toohello Internet från ett virtuellt nätverk länkade tooan ExpressRoute-kretsen.

### <a name="can-i-block-internet-connectivity-toovirtual-networks-connected-tooexpressroute-circuits"></a>Kan jag blockera Internet anslutning toovirtual nätverk anslutet tooExpressRoute kretsar?

Ja. Du kan annonsera standard vägar (0.0.0.0/0) tooblock alla Internet anslutning toovirtual datorer distribueras inom ett virtuellt nätverk och skicka all trafik ut via hello ExpressRoute-kretsen.

Om du annonserar ut standardrutter tvinga vi trafik tooservices erbjuds via offentlig peering (till exempel Azure storage och SQL-databas) tillbaka tooyour lokaler. Du måste tooconfigure dina routrar tooreturn trafik tooAzure via hello offentlig peering sökväg eller via hello Internet.

### <a name="can-virtual-networks-linked-toohello-same-expressroute-circuit-talk-tooeach-other"></a>Kan virtuella nätverk länkade toohello samma ExpressRoute-krets prata tooeach andra?

Ja. Virtuella datorer i virtuella nätverk anslutet toohello samma ExpressRoute-kretsen kan kommunicera med varandra.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>Kan jag använda plats-till-plats-anslutning för virtuella nätverk tillsammans med ExpressRoute?

Ja. ExpressRoute kan samexistera med plats-till-plats-VPN.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-toouse-expressroute"></a>Kan jag flytta ett virtuellt nätverk från plats-till-plats / punkt-till-plats configuration toouse ExpressRoute?

Ja. Du måste toocreate en ExpressRoute-gateway i det virtuella nätverket. Det finns ett litet driftstopp som är kopplad till hello process.

### <a name="why-is-there-a-public-ip-address-associated-with-hello-expressroute-gateway-on-a-virtual-network"></a>Varför är det en offentlig IP-adress som är associerade med hello ExpressRoute-gateway på ett virtuellt nätverk?

hello offentliga IP-adressen används för intern hantering. Den här offentliga IP-adressen är inte synliga toohello Internet och utgör inte en sårbart för det virtuella nätverket.

### <a name="what-do-i-need-tooconnect-tooazure-storage-over-expressroute"></a>Vad gör jag behöver tooconnect tooAzure lagring via ExpressRoute?

Du måste upprätta en ExpressRoute-krets och konfigurera flöden för offentlig peering.

### <a name="are-there-limits-on-hello-number-of-routes-i-can-advertise"></a>Finns det någon gräns för hello antalet vägar som jag kan annonsera?

Ja. Vi emot too4000 väg prefix för privat peering och 200 för offentlig peering och Microsoft-peering. Du kan öka den här too10 000 vägar för privat peering om du aktiverar hello ExpressRoute premium-funktion.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-hello-bgp-session"></a>Finns det några begränsningar för IP-adressintervall som jag kan annonsera över hello BGP-sessionen?

Vi tar inte emot privata prefix (RFC1918) i hello offentliga och Microsoft BGP-peeringsessionen.

### <a name="what-happens-if-i-exceed-hello-bgp-limits"></a>Vad händer om jag överskrida hello BGP?

BGP-sessioner kommer att tas bort. De kommer att återställas när hello prefix antal hamnar under hello gränsen.

### <a name="what-is-hello-expressroute-bgp-hold-time-can-it-be-adjusted"></a>Vad är hello ExpressRoute BGP håller tid? Kan den justeras?

hello driftstid är 180. hello keep-alive-meddelanden skickas var 60: e sekund. Dessa är fasta inställningar på hello Microsoft sida som inte kan ändras. Det är möjligt att du tooconfigure olika timers och hello BGP-sessionen parametrar förhandlas därefter.

### <a name="after-i-advertise-hello-default-route-00000-toomy-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-tooi-fix-this"></a>Jag kan inte aktivera Windows som körs på min Azure Virtual Machines när jag visar hello standard väg (0.0.0.0/0) toomy virtuella nätverk. Hur tooI åtgärda detta?

hello följande hjälpa Azure att identifiera hello aktiveringsbegäran:

1. Upprätta hello offentlig peering för ExpressRoute-kretsen.
2. Utföra en DNS-sökning och hitta hello IP-adressen för **kms.core.windows.net**
3. hello nyckelhanteringstjänsten måste identifiera den hello aktiveringsbegäran kommer från Azure och kontrollera hello begäran. Utför något av följande tre uppgifter hello:

   * I det lokala nätverket hello-vägtrafik är avsedda att hello IP-adress som du hämtade i steg 2 tillbaka tooAzure via hello offentlig peering.
   * Har din anmärkningar providern hår-pin hello trafik tillbaka tooAzure via hello offentlig peering.
   * Skapa en användardefinierad väg att punkter hello IP-adress som har Internetåtkomst som nästa hopp och Använd den toohello adressundernät där de virtuella datorerna är.

### <a name="can-i-change-hello-bandwidth-of-an-expressroute-circuit"></a>Kan jag ändra hello bandbredden för en ExpressRoute-krets?

Ja, kan du försöka tooincrease hello bandbredden för ExpressRoute-krets i hello Azure-portalen eller med hjälp av PowerShell. Om det finns kapacitet på hello fysiska port där kretsen har skapats, lyckas ändringen. 

Om din ändring misslyckas, innebär antingen det finns inte tillräckligt med kapacitet kvar på hello aktuella porten och du behöver toocreate en ny ExpressRoute-krets med hello högre bandbredd eller om det finns inga ytterligare kapacitet på den platsen, i så fall kan du inte tooincrease hello bandbredd. 

Du har också toofollow med din anslutning providern tooensure uppdatering hello begränsas i sina nätverk toosupport hello bandbredd ökning. Du kan emellertid minska hello bandbredden för ExpressRoute-kretsen. Du har toocreate en ny ExpressRoute-krets med låg bandbredd och ta bort gamla hello-kretsen.

### <a name="how-do-i-change-hello-bandwidth-of-an-expressroute-circuit"></a>Hur ändrar jag hello bandbredden för en ExpressRoute-krets?

Du kan uppdatera hello bandbredden för hello ExpressRoute-kretsen med hello REST API eller PowerShell-cmdlet.

## <a name="expressroute-premium"></a>ExpressRoute premium

### <a name="what-is-expressroute-premium"></a>Vad är premium ExpressRoute?

ExpressRoute premium är en samling hello följande funktioner:

* Ökat routning tabell från 4000 vägar too10 000 vägar för privat peering.
* Ökat antal Vnet som kan vara anslutna toohello ExpressRoute-krets (standardvärdet är 10). Mer information finns i hello [ExpressRoute gränser](#limits) tabell.
* Anslutningen tooOffice 365 och Dynamics 365.
* Globala anslutningar över hello Microsoft Kärnnätverket. Du kan nu koppla ett virtuellt nätverk i ett geopolitiska region med en ExpressRoute-krets i en annan region.<br>
    **Exempel:**

    *  Du kan länka ett VNet som skapats i västra Europa tooan ExpressRoute-krets som skapats i Silicon dal. 
    *  På hello offentlig peering annonseras prefix från andra geopolitiska regioner så att du kan ansluta till, exempelvis SQL Azure i västra Europa från en krets i Silicon dal.


### <a name="limits"></a>Hur många Vnet kan jag koppla tooan ExpressRoute-krets om du har aktiverat ExpressRoute premium?

hello följande tabeller visar hello ExpressRoute gränser och hello antal Vnet per ExpressRoute-krets:

[!INCLUDE [ExpressRoute limits](../../includes/expressroute-limits.md)]

### <a name="how-do-i-enable-expressroute-premium"></a>Hur aktiverar ExpressRoute premium?

ExpressRoute premium-funktioner kan aktiveras när hello-funktionen är aktiverad och kan stängas av genom att uppdatera hello krets tillstånd. Du kan aktivera ExpressRoute premium vid krets skapandet eller kan anropa hello REST API / PowerShell-cmdlet.

### <a name="how-do-i-disable-expressroute-premium"></a>Hur inaktiverar ExpressRoute premium?

Du kan inaktivera ExpressRoute premium genom att anropa hello REST API eller PowerShell-cmdlet. Du måste se till att du har skalats din anslutning måste toomeet hello standardgränser innan du inaktiverar ExpressRoute premium. Om din användning skalas utöver hello standardgränser, misslyckas hello begäran toodisable ExpressRoute premium.

### <a name="can-i-pick-and-choose-hello-features-i-want-from-hello-premium-feature-set"></a>Kan jag välja hello-funktioner som jag vill hello premium funktionsuppsättningen?

Nej. Du kan inte välja hello funktioner. Vi aktiverar alla funktioner när du aktiverar ExpressRoute premium.

### <a name="how-much-does-expressroute-premium-cost"></a>Hur mycket kostar ExpressRoute premium?

Se för[prisinformationen](https://azure.microsoft.com/pricing/details/expressroute/) för kostnaden.

### <a name="do-i-pay-for-expressroute-premium-in-addition-toostandard-expressroute-charges"></a>Betalar jag för ExpressRoute premium dessutom toostandard ExpressRoute avgifter?

Ja. ExpressRoute premium avgifter gäller ovanpå ExpressRoute-kretsen avgifter och kostnader som krävs av hello anslutning providern.

## <a name="expressroute-for-office-365-and-dynamics-365"></a>ExpressRoute för Office 365 och Dynamics 365

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-tooconnect-toooffice-365-services-and-dynamics-365"></a>Hur skapar jag en ExpressRoute-kretsen tooconnect tooOffice 365-tjänster och Dynamics 365?

1. Granska hello [ExpressRoute förutsättningssidan](expressroute-prerequisites.md) toomake till hello krav är uppfyllda.
2. tooensure som anslutningen måste är uppfyllda, granska hello lista över leverantörer och platser i hello [ExpressRoute partners och platser](expressroute-locations.md) artikel.
3. Planera dina kapacitetskrav genom att granska [planering och prestandajustering för Office 365](http://aka.ms/tune/).
4. Följ steg hello i hello arbetsflöden tooset upp anslutningar [kretsetablering och kretsstatus i ExpressRoute-arbetsflöden](expressroute-workflows.md).

> [!IMPORTANT]
> Kontrollera att du har aktiverat ExpressRoute premium tillägg när du konfigurerar tooOffice 365 tjänster och Dynamics 365.
> 
> 

### <a name="do-i-need-tooenable-azure-public-peering-tooconnect-toooffice-365-services-and-dynamics-365"></a>Behöver jag tooenable Azure offentlig peering tooconnect tooOffice 365 tjänster och Dynamics 365?

Nej, du behöver bara tooenable Microsoft-Peering. Autentisering trafik tooAzure AD skickas via Microsoft-Peering. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-toooffice-365-services-and-dynamics-365"></a>Stöder Mina befintliga ExpressRoute-kretsar tooOffice 365 tjänster och Dynamics 365?

Ja. Befintlig ExpressRoute-krets kan vara konfigurerade toosupport tooOffice 365 tjänster. Kontrollera att du har tillräcklig kapacitet tooconnect tooOffice 365 tjänster och att du har aktiverat premium-tillägg. [Nätverksplanering och prestandajustering för Office 365](http://aka.ms/tune/) hjälper dig planera anslutningen måste. Se även [skapa och ändra en ExpressRoute-krets](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Vilka Office 365 tjänster kan nås via en ExpressRoute-anslutning?

Se för[Office 365-URL: er och IP-adressintervall](http://aka.ms/o365endpoints) sida för en uppdaterad lista över tjänster som stöds via ExpressRoute.

### <a name="how-much-does-expressroute-for-office-365-services-and-dynamics-365-cost"></a>Hur mycket kostar ExpressRoute för Office 365-tjänster och Dynamics 365 kostnaden?

Office 365-tjänster och Dynamics 365 kräver premium tillägget toobe aktiverat. Se hello [informationssida med priser](https://azure.microsoft.com/pricing/details/expressroute/) för kostnader.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Vilka regioner som stöds ExpressRoute för Office 365 i?

Se [ExpressRoute partners och platser](expressroute-locations.md) information.

### <a name="can-i-access-office-365-over-hello-internet-even-if-expressroute-was-configured-for-my-organization"></a>Kan jag använda Office 365 via hello Internet, även om ExpressRoute har konfigurerats för min organisation?

Ja. Office 365 slutpunkter kan nås via hello Internet, även om ExpressRoute har konfigurerats för ditt nätverk. Om du är på en plats som är konfigurerade tooconnect tooOffice 365 tjänster via ExpressRoute ansluter via ExpressRoute.

### <a name="can-i-access-office-365-us-government-community-gcc-services-over-an-azure-us-government-expressroute-circuit"></a>Kan jag använda Office 365 US Government Community (GCC) tjänster via en Azure US Government ExpressRoute-krets?

Ja. Office 365 GCC slutpunkter kan nås via hello Azure US Government ExpressRoute. Dock hello du första måste tooopen ett supportärende i Azure portal tooprovide hello prefix som du avser tooadvertise tooMicrosoft. Din tooOffice 365 GCC tjänster upprättas när hello supportärende är löst. 

### <a name="can-dynamics-365-for-operations-formerly-known-as-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Dynamics 365 för åtgärder (kallades Dynamics AX Online) tillgänglig via en ExpressRoute-anslutning?

Ja. [Dynamics 365 för åtgärder](https://www.microsoft.com/dynamics365/operations) finns på Azure. Du kan aktivera offentlig Azure-peering på din ExpressRoute-kretsen tooconnect tooit.

## <a name="route-filters-for-microsoft-peering"></a>Filter för routning för Microsoft-peering

### <a name="i-am-turning-on-microsoft-peering-for-hello-first-time-what-routes-will-i-see"></a>Jag aktivera Microsoft-peering för hello första gången, vilka vägar visas?

Alla vägar visas inte. Du har tooattach en väg filter tooyour krets toostart prefix annonser. Instruktioner finns i [konfigurera route filter för Microsoft-peering](how-to-routefilter-powershell.md).

### <a name="i-turned-on-microsoft-peering-and-now-i-am-trying-tooselect-exchange-online-but-it-is-giving-me-an-error-that-i-am-not-authorized-toodo-it"></a>Jag aktiverade på Microsoft-peering och nu jag försöker tooselect Exchange Online, men det ge mig ett fel som inte jag auktoriserad toodo den.

När du använder filter routning, kan en kund aktivera Microsoft-peering. För förbrukning av Office 365-tjänster, behöver du dock fortfarande tooget behörighet av Office 365.

### <a name="do-i-need-tooget-authorization-for-turning-on-dynamics-365-over-microsoft-peering"></a>Behöver jag tooget auktorisering för att aktivera Dynamics 365 över Microsoft-peering?

Nej, du behöver inte tillståndet för Dynamics 365. Du kan skapa en regel och välja Dynamics 365 community utan behörighet.

### <a name="i-already-have-microsoft-peering-how-can-i-take-advantage-of-route-filters"></a>Jag har redan Microsoft-peering, hur kan jag dra nytta av filter Routning?

Du kan skapa en vägfilter, Välj hello tjänster du vill toouse och bifoga hello filter tooyour Microsoft-peering. Instruktioner finns i [konfigurera route filter för Microsoft-peering](how-to-routefilter-powershell.md).

### <a name="i-have-microsoft-peering-at-one-location-now-i-am-trying-tooenable-it-at-another-location-and-i-am-not-seeing-any-prefixes"></a>Jag har Microsoft peering på en plats nu jag försök tooenable den på en annan plats och jag inte ser alla prefix.

* Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft-peering, även om filter routning inte har definierats.

* Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets. Inget prefix visas som standard.
