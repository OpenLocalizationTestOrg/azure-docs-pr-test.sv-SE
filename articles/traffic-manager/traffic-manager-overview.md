---
title: "aaaWhat är Traffic Manager | Microsoft Docs"
description: "Den här artikeln hjälper dig att förstå vad Traffic Manager är och oavsett om det är hello rätt trafik routning val för ditt program"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.openlocfilehash: 8e63ed11cdcdc03ae9cd28f88f0d1f9dc2cd44ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-traffic-manager"></a>Översikt över Traffic Manager

Microsoft Azure Traffic Manager kan du toocontrol hello distributionen av användartrafik för slutpunkter i olika datacenter. Service-slutpunkter som stöds av Traffic Manager omfattar virtuella Azure-datorer Web Apps och molntjänster. Du kan även använda Traffic Manager med externa slutpunkter som inte tillhör Azure.

Traffic Manager använder hello Domain Name System (DNS) toodirect klient begäranden toohello lämpligaste slutpunkt baserat på en metod som routning av nätverkstrafik och hello hälsotillstånd hello slutpunkter. Traffic Manager erbjuder en uppsättning [routning av nätverkstrafik metoder](traffic-manager-routing-methods.md) och [endpoint övervakningsalternativ](traffic-manager-monitoring.md) toosuit olika program behov och automatisk redundans modeller. Traffic Manager är flexibel toofailure, inklusive hello fel på en hel Azure-region.

## <a name="traffic-manager-benefits"></a>Fördelar med Traffic Manager

Traffic Manager kan hjälpa dig:

* **Förbättra tillgänglighet för viktiga program**

    Traffic Manager ger hög tillgänglighet för dina program genom att övervaka dina slutpunkter och ger automatisk redundans när en slutpunkt som kraschar.

* **Förbättra svarstiden för program med höga prestanda**

    Azure kan du toorun molntjänster eller webbplatser i runt hello world-datacenter. Traffic Manager förbättrar programtillgängligheten genom att dirigera trafik toohello slutpunkt med hello lägsta Nätverksfördröjningen för hello-klienten.

* **Utföra underhåll utan avbrott**

    Du kan utföra åtgärder för planerat underhåll på dina program utan driftavbrott. Traffic Manager dirigerar trafik tooalternative slutpunkter medan hello underhåll pågår.

* **Kombinera lokala och molnbaserade program**

    Traffic Manager stöder extern, Azure-slutpunkter som möjliggör toobe används med hybrid molnet och lokala distributioner, däribland hello ”burst-to-cloud”, ”migrera till moln”, och ”redundans till moln” scenarier.

* **Distribuera trafiken för stora, komplexa distributioner**

    Med hjälp av [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md)routning av nätverkstrafik metoder kan vara kombinerade toocreate avancerade och flexibla regler toosupport hello behov av större och mer komplexa distributioner.

## <a name="how-traffic-manager-works"></a>Så här fungerar Traffic Manager

Azure Traffic Manager kan du toocontrol hello fördelning av trafik över dina slutpunkter för programmet. En slutpunkt är alla mot Internet-tjänst som finns i eller utanför Azure.

Traffic Manager har två viktiga fördelar:

1. Fördelning av trafik enligt tooone av flera [routning av nätverkstrafik metoder](traffic-manager-routing-methods.md)
2. [Kontinuerlig övervakning av hälsotillstånd för slutpunkten](traffic-manager-monitoring.md) och automatisk redundans när slutpunkter misslyckas

När en klient försöker tooconnect tooa service, måste det först lösa hello DNS-namnet på hello-tooan IP-adress. hello klienten ansluter sedan toothat IP-adress tooaccess hello-tjänsten.

**hello viktigaste punkt toounderstand är att Traffic Manager fungerar på hello DNS-nivå.**  Traffic Manager använder DNS-toodirect klienter toospecific tjänstens slutpunkter baserat på hello regler för routning av nätverkstrafik hello-metoden. Klienterna ansluter toohello valt endpoint **direkt**. Traffic Manager är inte en proxy eller en gateway. Traffic Manager kan inte se hello trafik som passerar mellan hello klient- och hello.

### <a name="traffic-manager-example"></a>Traffic Manager-exempel

Contoso Corp har utvecklat en ny partner-portalen. hello-URL för den här portalen är https://partners.contoso.com/login.aspx. hello program finns i tre områden i Azure. tooimprove tillgänglighet och maximera globala prestanda använder de Traffic Manager toodistribute klient trafik toohello närmaste tillgängliga slutpunkt.

tooachieve den här konfigurationen kan de utföra hello följande steg:

1. Distribuera tre instanser av tjänsten. hello DNS-namnen på dessa distributioner är ”contoso us.cloudapp .net', 'contoso eu.cloudapp .net' och 'contoso asia.cloudapp .net'.
2. Skapa en Traffic Manager-profil med namnet contoso.trafficmanager.net, och konfigurera den toouse hello 'prestanda-trafik inkommande metod för hello tre slutpunkterna.
* Konfigurera sina alternativa domännamn, 'partners.contoso.com', toopoint too'contoso.trafficmanager.net ”, med hjälp av en DNS CNAME-post.

![Traffic Manager DNS-konfiguration][1]

> [!NOTE]
> När du använder en alternativa domän med Azure Traffic Manager, måste du använda en CNAME-toopoint alternativa namn tooyour Traffic Manager-domän domännamnet. DNS-standarden tillåter inte toocreate en CNAME-post på hello 'apex' (eller rot) i en domän. Därmed kan du skapa en CNAME-post för ”contoso.com” (kallas ibland för en 'asbestgaller' domän). Du kan bara skapa en CNAME-post för en domän under ”contoso.com”, till exempel ”www.contoso.com”. toowork runt denna begränsning, bör du använda en enkel HTTP-omdirigering toodirect begäranden för ”contoso.com” tooan alternativa namn, till exempel ”www.contoso.com”.

### <a name="how-clients-connect-using-traffic-manager"></a>Hur klienter ska anslutas med Traffic Manager

Fortsätter från hello föregående exempel, när en klient begär hello sidan https://partners.contoso.com/login.aspx hello klienten utför hello följande steg tooresolve hello DNS-namn och upprätta en anslutning:

![Anslutningsupprättande med Traffic Manager][2]

1. hello klienten skickar en DNS-fråga tooits konfigurerats rekursiv DNS-tjänsten tooresolve hello name 'partners.contoso.com'. En rekursiv DNS-tjänsten som ibland kallas en ”lokala DNS-tjänsten inte är värd för DNS-domäner direkt. I stället hello klienten off-loads hello arbete för att kontakta hello betjänar olika auktoritära DNS i hello Internet behövs tooresolve ett DNS-namn.
2. tooresolve hello DNS-namn, hello rekursiv DNS-tjänsten hittar hello namnservrar för hello ”contoso.com” domän. Den kontaktar sedan dessa servrar toorequest hello 'partners.contoso.com' DNS-namnpost. hello contoso.com DNS-servrarna returnerar hello CNAME-post som pekar toocontoso.trafficmanager.net.
3. Därefter hittar hello rekursiv DNS-tjänsten hello namnservrar för hello 'trafficmanager.net' domän, som tillhandahålls av hello Azure Traffic Manager-tjänsten. Sedan skickas en begäran om hello 'contoso.trafficmanager.net' DNS-poster toothose DNS-servrar.
4. hello Traffic Manager-namnservrar får hello-begäran. De välja en slutpunkt som baseras på:

    - hello konfigurerats tillståndet för varje slutpunkt (inaktiverad slutpunkter returneras inte)
    - hello aktuellt hälsotillstånd för varje slutpunkt som bestäms av hälsotillståndet för hello Traffic Manager kontrollerar. Mer information finns i [Traffic Manager-slutpunkten övervakning](traffic-manager-monitoring.md).
    - hello valt routning av nätverkstrafik metod. Mer information finns i [Traffic Manager routning metoder](traffic-manager-routing-methods.md).

5. hello valt endpoint returneras som en annan DNS-CNAME-post. I det här fallet Låt oss anta att contoso-us.cloudapp.net returneras.
6. Därefter hittar hello rekursiv DNS-tjänsten hello namnservrar för hello 'cloudapp.net-domän. Den kontaktar dessa namn servrar toorequest hello ”contoso-us.cloudapp .net-DNS-post. En DNS-”A”-post som innehåller hello IP-adressen för hello USA-baserade tjänstslutpunkten returneras.
7. hello rekursiv DNS-tjänsten konsoliderar hello resultaten och returnerar en enda DNS-svar toohello klient.
8. hello klienten tar emot hello DNS-resultat och ansluter toohello viss IP-adress. hello ansluter klienten toohello programmet tjänstslutpunkten direkt, inte via Trafikhanterarprofilen. Eftersom det är en HTTPS-slutpunkt hello klienten utför hello nödvändiga SSL/TLS-handskakning och gör en HTTP GET-begäran för hello ' / login.aspx-sidan.

hello rekursiv DNS-tjänsten cachelagrar hello DNS-svar tas emot. hello DNS-matchning på hello klientenhet cachelagrar också hello resultat. Cachelagring kan efterföljande DNS-frågor toobe besvaras snabbare genom att använda data från hello cache i stället för att fråga andra namnservrar. hello varaktighet hello cache bestäms av hello-time-to-live ”(TTL)-egenskapen för varje DNS-post. Kortare värden ger snabbare cache upphör att gälla och därmed mer turer toohello Traffic Manager namnservrar. Längre värden innebär att det kan ta längre toodirect trafik från en misslyckad slutpunkt. Traffic Manager kan du tooconfigure hello TTL-värde används i Traffic Manager DNS-svar toobe så lågt som 0 sekunder och lika hög som 2 147 483 647 sekunder (hello största intervall som är kompatibla med [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt)), vilket gör att du toochoose hello värdet som balanserar bästa hello behoven för ditt program.

## <a name="pricing"></a>Prissättning

Information om priser finns [Traffic Manager priser](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

Vanliga frågor om Traffic Manager finns [Traffic Manager vanliga frågor och svar](traffic-manager-FAQs.md)

## <a name="next-steps"></a>Nästa steg

Lär dig mer om Traffic Manager [endpoint övervaknings- och automatisk redundans](traffic-manager-monitoring.md).

Lär dig mer om Traffic Manager [trafikroutningsmetoder](traffic-manager-routing-methods.md).

Lär dig mer om hello andra nyckeln [nätverk](../networking/networking-overview.md) Azure.

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

