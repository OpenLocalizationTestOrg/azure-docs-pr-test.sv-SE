---
title: aaaAsymmetric Routning | Microsoft Docs
description: "Den här artikeln vägleder dig genom hello problem en kund kan utsättas med asymmetriska routning i ett nätverk som har flera länkar tooa mål."
documentationcenter: na
services: expressroute
author: osamazia
manager: carmonm
editor: 
ms.assetid: a754bff9-95c9-44b5-9796-377fc21e8322
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: osamam
ms.openlocfilehash: 01a16242437a3674dcfe27b074911a829a6c1abd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="asymmetric-routing-with-multiple-network-paths"></a>Asymmetrisk routning med flera nätverksvägar
Den här artikeln förklarar hur utgående och återvändande nätverkstrafik kan ta olika vägar när det finns flera vägar tillgängliga mellan nätverkskällan och målet.

Det är viktigt toounderstand två begrepp toounderstand asymmetriska routning. En är hello effekten av flera nätverkssökvägar. hello andra är hur enheter som en brandvägg, hålla tillstånd. Dessa typer av enheter kallas tillståndskänsliga enheter. En kombination av dessa två faktorer skapar scenarier som trafik ignoreras av en tillståndskänslig enhet eftersom hello tillståndskänslig enheten inte kunde identifiera att trafiken kommer med själva hello-enheten.

## <a name="multiple-network-paths"></a>Flera nätverksvägar
När ett företagsnätverk har endast en länk toohello Internet via deras Internet-leverantör, alla trafik tooand från hello Internet flyttar hello samma sökväg. Företag köpa ofta flera kretsar som redundanta sökvägar, tooimprove nätverk drifttid. Då kan returnera det är möjligt att trafik som går utanför hello nätverk, toohello Internet genomgår en länk och hello trafik som passerar en annan länk. Detta kallas asymmetrisk routning. Omvänd nätverkstrafik tar asymmetriska routning, en annan sökväg från hello ursprungliga flöde.

![Nätverk med flera sökvägar](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Även om det uppstår i första hand i hello Internet, gäller även asymmetriska routning tooother kombinationer av flera sökvägar. Det gäller, till exempel både tooan Internet sökväg och ett privat sökväg gå toohello samma mål och toomultiple privata sökvägar som går toohello samma mål.

Varje router hello vägen från källan toodestination beräknar hello bästa sökvägen tooreach ett mål. hello baseras routerns bestämning av bästa möjliga sökvägar på två huvudsakliga faktorer:

* Routning mellan externa nätverk baseras på ett routningsprotokoll, Border Gateway Protocol (BGP). BGP tar annonser från grannar och kör dem via en serie steg toodetermine hello bästa sökväg toohello avsedda mål. Hello bästa sökvägen lagras i routningstabellen.
* hello längden på nätmasken som är kopplade till en väg påverkar routning sökvägar. Om en router tar emot flera annonser för hello samma IP-adress men med olika nätmasker hello router föredrar hello annons med nätmasken längre tid eftersom den har anses vara en mer specifik väg.

## <a name="stateful-devices"></a>Tillståndskänsliga enheter
Routrar titta på hello IP-rubriken för ett paket för routning. Vissa enheter leta djupare i hello-paket. Normalt tittar enheterna på Layer4-huvuden (Transmission Control Protocol eller TCP; eller User Datagram Protocol eller UDP), eller Layer7-huvuden (Application Layer). Dessa typer av enheter är antingen säkerhetsenheter eller enheter för bandbreddsoptimering. 

En brandvägg är ett vanligt exempel på en tillståndskänslig enhet. En brandvägg tillåter eller nekar ett paket toopass via dess gränssnitt baserat på olika fält som protokoll, TCP/UDP-port och URL-huvuden. Den här nivån av paketinspektion placerar kraftig belastningen på hello enhet. tooimprove prestanda hello brandväggen undersöker hello första paketet i ett flöde. Om det tillåter hello paket tooproceed behålls hello flödesinformation i tabell tillstånd. Alla efterföljande paket relaterade toothis flöde tillåts baserat på hello bestämning. Ett paket som ingår i en befintlig flöde kan nå hello brandväggen. Om hello brandväggen har ingen tidigare tillståndsinformation om den, släpper hello brandväggen hello-paket.

## <a name="asymmetric-routing-with-expressroute"></a>Asymmetrisk routning med ExpressRoute
När du ansluter tooMicrosoft via Azure ExpressRoute som dina ändringar i nätverket detta:

* Du har flera länkar tooMicrosoft. En länk är din befintliga Internet-anslutning och hello andra är via ExpressRoute. Vissa trafik tooMicrosoft kan gå igenom hello Internet men komma tillbaka via ExpressRoute, och vice versa.
* Du får mer specifika IP-adresser via ExpressRoute. Så för trafik från din tooMicrosoft nätverk för tjänster som erbjuds via ExpressRoute föredrar routrar alltid ExpressRoute.

toounderstand hello effekt dessa två ändringar har på ett nätverk, nu ska vi titta vissa scenarier. Du har bara en krets toohello Internet och du använder alla Microsoft-tjänster via hello Internet exempelvis. hello trafik från ditt nätverk tooMicrosoft och tillbaka går hello samma Internet-länk och passerar genom hello brandväggen. hello brandväggen registrerar hello flödet ser hello första paketet och returnera paket tillåts eftersom hello flödet finns i hello tillstånd tabellen.

![Asymmetrisk routning med ExpressRoute](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)

Sedan aktiverar du ExpressRoute och använder tjänster som erbjuds av Microsoft via ExpressRoute. Alla andra tjänster från Microsoft förbrukas över hello Internet. Du kan distribuera en separat brandvägg på din kant som är anslutna tooExpressRoute. Microsoft meddelar mer specifikt prefixen tooyour nätverk via ExpressRoute för vissa tjänster. Infrastruktur för routning väljer ExpressRoute som hello primära sökvägen för dessa prefix. Om du inte annonserar din offentliga IP-adresser tooMicrosoft via ExpressRoute kommunicerar Microsoft med din offentliga IP-adresser via hello Internet. Vidarebefordra trafik från nätverket-tooMicrosoft använder ExpressRoute och omvänd trafik från Microsoft använder hello Internet. När hello brandväggen hello kant ser ett svarspaket för ett flöde som inte hittas i tabellen för hello tillstånd, släpper hello returnerade trafik.

Om du väljer toouse hello samma network address translation (NAT) poolen för ExpressRoute och hello Internet visas liknande problem med hello klienter i nätverket på privata IP-adresser. Begäranden för tjänster som Windows Update att gå via hello Internet eftersom IP-adresser för de här tjänsterna inte annonseras via ExpressRoute. Dock kommer hello returnerade trafik tillbaka via ExpressRoute. Om Microsoft tar emot en IP-adress med hello samma nätmask från hello Internet och ExpressRoute den föredrar ExpressRoute över hello Internet. Om en brandvägg eller en annan tillståndskänslig enhet som finns på din nätverksgräns och inför ExpressRoute har ingen tidigare information om hello flödet, släpper hälsningspaket som tillhör toothat flöde.

## <a name="asymmetric-routing-solutions"></a>Lösningar för asymmetrisk routning
Du har två huvudsakliga alternativ toosolve hello problemet med asymmetriska routning. En är via Routning och hello andra är med hjälp av käll-baserade NAT (SNAT).

### <a name="routing"></a>Routning
Kontrollera att din offentliga IP-adresser är länkar till annonserade tooappropriate wide area network (WAN). Till exempel om du vill toouse hello Internet för autentiseringstrafik och ExpressRoute för e-post-trafik kan ska du inte annonseras din offentliga IP-adresser för Active Directory Federation Services (AD FS) via ExpressRoute. På samma sätt bör inte tooexpose en lokal AD FS tooIP serveradresser som hello router tar emot via ExpressRoute. Vägar som tas emot via ExpressRoute är mer specifika så att de ExpressRoute hello primära sökvägen för autentisering trafik tooMicrosoft. Detta leder till asymmetrisk routning.

Om du vill toouse ExpressRoute för autentisering, se till att du annonserar AD FS offentliga IP-adresser via ExpressRoute utan NAT. På så sätt kan trafik som kommer från Microsoft och går tooan lokal AD FS-servern går över ExpressRoute. Returnerar trafik från kunden tooMicrosoft använder ExpressRoute eftersom den är hello prioriterade vägen över hello Internet.

### <a name="source-based-nat"></a>Källbaserad NAT
Ett annat sätt att lösa problem med asymmetrisk routning är att använda SNAT. Till exempel har du inte annonseras hello offentliga IP-adressen för en lokal SMTP Simple Mail Transfer Protocol ()-server via ExpressRoute eftersom toouse hello Internet för den här typen av kommunikation. En begäran som har sitt ursprung med Microsoft och sedan öppnar tooyour lokalt SMTP-servern passerar hello Internet. Du SNAT hello inkommande begäran tooan interna IP-adress. Omvänd trafik från hello SMTP-server går toohello gränsbrandvägg (som du använder för NAT) i stället för via ExpressRoute. hello returnerade trafik går tillbaka via hello Internet.

![Nätverkskonfiguration för källbaserad NAT](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="asymmetric-routing-detection"></a>Identifiering av asymmetrisk routning
Traceroute är hello bästa sätt toomake till att nätverkstrafiken passerar genom hello förväntade sökvägen. Om du räknar trafik från din lokala SMTP server tooMicrosoft tootake hello Internet sökväg förväntades hello traceroute är från hello SMTP server tooOffice 365. hello resultatet verifierar att trafiken verkligen lämna ditt nätverk mot hello Internet och inte mot ExpressRoute.

