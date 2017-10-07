---
title: 'Optimera ExpressRoute-routning: Azure | Microsoft Docs'
description: "Den här sidan innehåller information om hur toooptimize routning när du har mer än en ExpressRoute kretsar som anslutning mellan Microsoft och corp-nätverk."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
ms.assetid: fca53249-d9c3-4cff-8916-f8749386a4dd
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/06/2017
ms.author: charwen
ms.openlocfilehash: ebcfb638f67a9ac78c3e476668bfd0bb0ffb9985
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-expressroute-routing"></a>Optimera ExpressRoute-routning
När du har flera ExpressRoute-kretsar har mer än en sökväg tooconnect tooMicrosoft. Därför kan hända att något sämre routning - som är trafiken kan ta en längre sökväg tooreach Microsoft, och Microsoft tooyour nätverk. hello längre hello nätverkssökväg hello högre hello latens. Svarstiden har direkt inverkan på programmens prestanda och användarupplevelse. Den här artikeln kommer illustrerar problemet och förklara hur toooptimize routning med hello standard routning tekniker.

## <a name="suboptimal-routing-from-customer-toomicrosoft"></a>Något sämre routning från kunden tooMicrosoft
Nu ska vi titta Stäng på hello routning problem med ett exempel. Anta att du har två kontor i hello USA, en i Los Angeles och en i New York. Ditt kontor ansluts i ett WAN (Wide Area Network), som kan vara antingen ditt eget stamnät eller leverantörens IP VPN. Du har två ExpressRoute-kretsar, en i oss Väst och en i oss East, som även är anslutna på hello WAN. Naturligtvis, har du två sökvägar tooconnect toohello Microsoft-nätverk. Anta nu att du har Azure-distribution (t.ex. Azure App Service) i både ”USA, västra” och ”USA, östra”. Din avsikt är tooconnect användarna i Los Angeles tooAzure oss Väst och användarna i New York tooAzure oss Öst eftersom tjänstadministratören meddelar att användare i varje office access hello Närliggande Azure-tjänster för optimal upplevelse. Tyvärr fungerar hello plan bra för hello ostkust användare men inte för hello någon annan användare. hello beror hello problemet på hello följande. På varje ExpressRoute-krets annonsera vi tooyou både hello prefix i Azure oss East (23.100.0.0/16) och hello-prefix i Azure oss Väst (13.100.0.0/16). Om du inte vet vilka prefixet är från vilken region kan du inte är kan tootreat den på olika sätt. WAN-nätverk kan tänka både hello prefix är närmare tooUS Öst än oss Väst och därför dirigera både office användare toohello ExpressRoute-krets i oss East. Du har många missnöjda användare i hello Los Angeles office i hello slutet.

![ExpressRoute fall 1 problem - något sämre routning från kunden tooMicrosoft](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### <a name="solution-use-bgp-communities"></a>Lösning: Använd BGP-communities
toooptimize tooknow vilka prefixet är från Azure oss Väst och vilka från Azure oss Öst måste routning för både användare av office. Vi kodar informationen genom att använda [BGP Community-värden](expressroute-routing.md). Vi har tilldelat en unik BGP Community värdet tooeach Azure-region, t.ex. ”12076:51004” för östra USA och ”12076:51006” för västra USA. Nu när du vet vilket prefix är från vilken Azure-region, kan du konfigurera de ExpressRoute-kretsar som ska användas. Eftersom vi använder hello BGP tooexchange routning information kan du använda BGPS lokala inställningar tooinfluence routning. Du kan tilldela en högre lokala inställningar värdet too13.100.0.0/16 i oss Väst än i oss East och på samma sätt kan en högre lokala inställningar värdet too23.100.0.0/16 i oss East än i oss Väst i vårt exempel. Den här konfigurationen säkerställer att, när båda sökvägar tooMicrosoft, användarna i Los Angeles tar hello ExpressRoute-krets i oss West tooconnect tooAzure oss Väst användarna i New York vidta hello ExpressRoute i oss Öst tooAzure oss Öst . Routning är optimerad på båda sidorna. 

![ExpressRoute fall 1 – Lösning: Använd BGP-communities](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

> [!NOTE]
> hello kan samma teknik som använder lokala inställningar vara tillämpade toorouting från kunden tooAzure virtuellt nätverk. Vi tagga inte BGP Community värdet toohello prefix som annonseras från Azure tooyour nätverk. Eftersom du vet vilket av distributionen av virtuella nätverk är nära toowhich av dina office, du kan dock konfigurera dina routrar därefter tooprefer en ExpressRoute-kretsen tooanother.
>
>

## <a name="suboptimal-routing-from-microsoft-toocustomer"></a>Något sämre routning från Microsoft toocustomer
Här är ett annat exempel där anslutningar från Microsoft tar en längre sökväg tooreach nätverket. I detta fall måste du använda lokala Exchange-servrar och Exchange Online i en [hybridmiljö](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx). Kontoret är anslutna tooa WAN. Du annonserar hello prefixen för dina lokala servrar i både din kontor tooMicrosoft via hello två ExpressRoute-kretsar. Exchange Online initierar anslutningar toohello lokala servrar i sådana fall postlåda migrering. Tyvärr är hello anslutning tooyour Los Angeles office routade toohello ExpressRoute-krets i oss East innan passerar hello hela kontinent tillbaka toohello någon annan. hello hello problemet beror på liknande toohello först en. Hello Microsoft network går inte att avgöra vilka kund-prefixet är nära tooUS Öst och vilket som ligger nära tooUS Väst utan några tips. Det händer toopick hello Felaktigt sökvägsformat tooyour office i Los Angeles.

![ExpressRoute-fall 2 - något sämre routning från Microsoft toocustomer](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### <a name="solution-use-as-path-prepending"></a>Lösning: Använd AS PATH
Det finns två lösningar toohello problem. hello först är en att du helt enkelt annonsera lokala-prefix för kontoret Los Angeles 177.2.0.0/31 på hello ExpressRoute-krets i oss Väst och dina lokala prefix för kontoret New York 177.2.0.2/31 på hello ExpressRoute-krets i oss East. Det är därför bara en sökväg för Microsoft tooconnect tooeach på kontoret. Det finns inga tveksamheter och routningen optimeras. Med den här designen måste toothink om din strategi för växling vid fel. Hello händelse som hello sökvägen tooMicrosoft via ExpressRoute är bruten måste du toomake till att Exchange Online kan fortfarande Anslut tooyour lokala servrar. 

andra hello-lösning är att du fortsätter hello prefix tooadvertise både på både ExpressRoute-kretsar och dessutom du ger oss en ledtråd för vilka prefix är nära toowhich något av ditt kontor. Eftersom vi stöder BGP AS sökväg tillagt, kan du konfigurera hello som sökväg för din prefixet tooinfluence routning. I det här exemplet kan du förlänga hello som sökväg för 172.2.0.0/31 i oss East så att vi kommer att föredra hello ExpressRoute-krets i oss Väst för trafik till det här prefixet (som vårt nätverk kommer tror hello sökvägen toothis prefixet är kortare hello west). På liknande sätt kan du förlänga hello som sökväg för 172.2.0.2/31 i oss Väst så att vi ska föredrar hello ExpressRoute-krets i oss East. Routning är optimerat för båda kontoren. Med den här designen kan Exchange Online fortfarande nå dig via en annan ExpressRoute-krets och ditt WAN om en ExpressRoute-krets bryts. 

> [!IMPORTANT]
> Vi ta bort privata talen i hello som sökväg för hello prefix som tas emot på Microsoft-Peering. Du behöver tooappend offentliga som siffror i hello AS SÖKVÄGEN tooinfluence routning för Microsoft-Peering.
> 
> 

![ExpressRoute fall 2 – Lösning: Använd AS PATH-prepending](./media/expressroute-optimize-routing/expressroute-case2-solution.png)

> [!NOTE]
> Medan hello-exempel som anges här är för Microsoft och offentliga peerkopplingar, vi stöder hello samma funktioner för hello privat peering. Hello AS sökväg tillagt fungerar också i en enda ExpressRoute-krets, tooinfluence hello val av hello primära och sekundära sökvägar.
> 
> 

## <a name="suboptimal-routing-between-virtual-networks"></a>Icke-optimal routning mellan virtuella nätverk
Du kan aktivera tooVirtual virtuellt nätverk (som även kallas ”virtuella nätverk”) med ExpressRoute kommunikation genom att koppla tooan ExpressRoute-kretsen. När du länkar dem. toomultiple ExpressRoute-kretsar något sämre routning kan inträffa mellan hello Vnet. Vi tar ett exempel. Anta att du har två ExpressRoute-kretsar, en i ”USA, västra” och en i ”USA, östra”. Du har två virtuella nätverk i varje region. Webbservrar har distribuerats i ett virtuellt nätverk och programservrar i hello andra. För redundans, länkar hello två Vnet i varje region tooboth hello lokala ExpressRoute-krets och hello remote ExpressRoute-kretsen. Som visas nedan, från varje VNet finns två sökvägar toohello andra VNet. Hej Vnet vet inte vilka ExpressRoute-kretsen är lokala och vilket som är fjärransluten. Därför som lika-kostnad-Multi-Path ECMP () routning tooload balans mellan VNet-trafik, vissa trafikflöden tar hello längre sökväg och hämta dirigeras på hello remote ExpressRoute-kretsen.

![ExpressRoute fall 3 – Icke-optimal routning mellan virtuella nätverk](./media/expressroute-optimize-routing/expressroute-case3-problem.png)

### <a name="solution-assign-a-high-weight-toolocal-connection"></a>Lösning: tilldela en hög vikt toolocal anslutning
hello-lösning är enkel. Eftersom du vet var hello Vnet och hello kretsar finns, kan du berätta vilken sökväg du varje VNet ska föredrar. Specifikt för det här exemplet du tilldelar en högre vikt toohello lokal anslutning än toohello fjärranslutning (se hello configuration exempel [här](expressroute-howto-linkvnet-arm.md#modify-a-virtual-network-connection)). När ett VNet tar emot hello prefixet hello andra VNet på flera anslutningar som den kommer att föredra hello anslutning med hello högsta vikt toosend trafik till det prefixet.

![ExpressRoute fall 3 lösning – tilldela hög vikt toolocal anslutning](./media/expressroute-optimize-routing/expressroute-case3-solution.png)

> [!NOTE]
> Du kan även påverka routning från VNet tooyour lokalt nätverk om du har flera ExpressRoute-kretsar genom att konfigurera hello vikten för en anslutning i stället för att tillämpa AS SÖKVÄGEN prepending, en teknik som beskrivs i hello andra scenariot ovan. För varje prefixet alltid beskrivs hello anslutning vikt innan hello AS sökvägslängden när du bestämmer hur toosend trafik.
>
>
