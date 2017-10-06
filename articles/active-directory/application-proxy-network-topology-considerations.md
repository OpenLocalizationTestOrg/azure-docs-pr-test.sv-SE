---
title: "aaaNetwork topologiöverväganden när du använder Azure Active Directory Application Proxy | Microsoft Docs"
description: "Beskriver topologiöverväganden för nätverk när du använder Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 9b8cdd2196efeb92a74e44dde6511f7d3091a968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Topologiöverväganden nätverk när du använder Azure Active Directory Application Proxy

Den här artikeln förklarar topologiöverväganden för nätverk när du använder Azure Active Directory (Azure AD) Application Proxy för att publicera och åtkomst till dina program via fjärranslutning.

## <a name="traffic-flow"></a>Trafikflöde

När ett program har publicerats till Azure AD Application Proxy flödar trafik från hello användare toohello program via tre anslutningar:

1. hello användaren ansluter toohello Azure AD Application Proxy service offentlig slutpunkt på Azure
2. hello Application Proxy-tjänsten ansluter toohello Application Proxy connector
3. hello Application Proxy connector ansluter toohello målprogram

![Diagram över trafikflödet från tootarget-användarprogram](./media/application-proxy-network-topologies/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Klient-plats och tjänsten för Webbprogramproxy

När du registrerar dig för en Azure AD-klient bestäms hello region för din klient av hello land som du anger. När du aktiverar Application Proxy hello Application Proxy-tjänstinstanser för din klient är valt eller skapat i hello samma region som din Azure AD-klient eller hello närmaste region tooit.

Till exempel om regionen för din Azure AD-klient är hello Europeiska unionen (EU), använda alla Application Proxy-kopplingar tjänstinstanser i Azure-datacenter i hello Europa. När dina användare få tillgång till publicerade program, sin trafik som passerar hello Application Proxy-tjänstinstanser på den här platsen.

## <a name="considerations-for-reducing-latency"></a>Överväganden för att minska svarstiden

Alla proxylösningar för introducera fördröjning i anslutningen. Oavsett vilken proxy- eller VPN-lösning som du väljer som din lösning för fjärråtkomst, innehåller den alltid en uppsättning servrar som aktiverar hello anslutning tooinside ditt företagsnätverk.

Organisationer inkluderar vanligtvis server-slutpunkter i sina perimeternätverk. Med Azure AD Application Proxy, men flödar trafiken via hello proxy-tjänst i molnet hello medan hello kopplingar finns i företagsnätverket. Det krävs inga perimeternätverk.

hello nästa avsnitt innehåller ytterligare förslag toohelp du minska svarstiden ytterligare. 

### <a name="connector-placement"></a>Placering av koppling

Programproxy väljer hello placeringen av instanser för dig, baserat på klient-plats. Du får toodecide där tooinstall hello koppling, vilket ger dig hello power toodefine hello svarstid för egenskaper av nätverkstrafiken.

När du ställer in hello Application Proxy-tjänsten, be hello följande frågor:

* Var finns hello app?
* Var finns de flesta användare komma åt hello appen finns?
* Var finns hello Application Proxy instans?
* Har du redan ett dedikerat nätverk anslutning tooAzure Datacenter konfigurera Azure ExpressRoute eller en liknande VPN?

hello-koppling har toocommunicate med både Azure och dina program (steg 2 och 3 i hello trafikflödesdiagram), så hello placeringen av hello connector påverkar hello svarstiden för dessa två anslutningar. När du utvärderar hello placering av hello connector, Kom ihåg hello följande punkter:

* Om du vill toouse Kerberos-begränsad delegering (KCD) för enkel inloggning, måste en fri tooa datacenter med hello-anslutningen. Hello connector-servern måste dessutom toobe domänanslutna.  
* Installera hello närmare toohello anslutningsprogram i osäker.

### <a name="general-approach-toominimize-latency"></a>Allmän metod toominimize svarstid

Du kan minimera hello svarstiden för hello slutpunkt till slutpunkt-trafik genom att optimera varje nätverksanslutning. Varje anslutning kan optimeras genom att:

* Minska hello avståndet mellan hello två parterna i hello hopp.
* Om du väljer hello rätt nätverk tootraverse. Till exempel kan gå igenom ett privat nätverk i stället för hello offentliga Internet gå snabbare, på grund av toodedicated länkar.

Om du har en fast VPN eller ExpressRoute-länk mellan Azure och företagets nätverk, kanske du vill toouse som.

## <a name="focus-your-optimization-strategy"></a>Fokusera din strategi för optimering

Det finns lite som du kan göra toocontrol hello anslutningen mellan användare och hello Application Proxy-tjänsten. Användare kan komma åt dina appar från ett hemnätverk, ett kafé eller ett annat land. I stället kan du optimera hello anslutningar från hello Application Proxy toohello Application Proxy kopplingar toohello-appar. Överväg att använda hello efter mönster i din miljö.

### <a name="pattern-1-put-hello-connector-close-toohello-application"></a>Mönster 1: Stäng toohello Put hello anslutningsprogram

Plats hello Stäng toohello mål anslutningsprogram i hello kundens nätverk. Den här konfigurationen minimerar steg 3 i hello topografi diagram eftersom hello koppling och program är nära. 

Om din anslutningstjänst måste en fri toohello domänkontrollant, är det fördelaktigt med det här mönstret. De flesta av våra kunder att använda det här mönstret eftersom det fungerar bra för de flesta scenarier. Det här mönstret kan även kombineras med mönstret 2 toooptimize trafik mellan hello-tjänsten och hello-anslutningen.

### <a name="pattern-2-take-advantage-of-expressroute-with-public-peering"></a>Mönstret 2: Dra nytta av ExpressRoute med offentlig peering

Om du har konfigurerats med offentlig peering ExpressRoute, kan du använda hello snabbare ExpressRoute-anslutning för trafik mellan Application Proxy och hello-anslutningen. hello connector är fortfarande i nätverket, Stäng toohello app.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>Mönster 3: Dra nytta av ExpressRoute med privat peering

Om du har en dedikerad VPN eller ExpressRoute som konfigurerats med privat peering mellan Azure och företagets nätverk, har du ett annat alternativ. I den här konfigurationen anses vanligtvis hello virtuellt nätverk i Azure som ett tillägg för hello företagsnätverket. Du kan så installera hello connector i hello Azure-datacenter och fortfarande uppfyller kraven för hello låg latens hello connector-app-anslutning.

Latens manipuleras inte eftersom trafiken flödar via en dedikerad anslutning. Du kan också få bättre svarstid för Application Proxy-tjänstanslutning eftersom hello connector installeras i en Azure-datacenter Stäng tooyour plats för Azure AD-klient.

![Diagram över anslutning är installerad i ett Azure-datacenter](./media/application-proxy-network-topologies/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Andra metoder

Hello den här artikeln fokuserar på placering av koppling, men du kan också ändra hello placeringen av hello tooget bättre svarstid programegenskaper.

Mer och mer flyttar organisationer sina nätverk i värdbaserade miljöer. Detta gör tooplace sina appar i en miljö för värdar som ingår också i företagsnätverket och fortfarande att inom hello domän. I det här fallet kan hello mönster som beskrivs i föregående avsnitt hello vara tillämpade toohello nya program platsen. Om du funderar på det här alternativet, se [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md).

Dessutom kan du överväga att ordna dina kopplingar med [connector grupper](active-directory-application-proxy-connectors.md) tootarget appar som finns i olika platser och nätverk. 

## <a name="common-use-cases"></a>Vanliga användarsituationer

I det här avsnittet går vi igenom några vanliga scenarier. Anta att hello Azure AD-klient (och därför proxy tjänsten endpoint) finns i hello USA (US). hello överväganden som beskrivs i den här användningsfall gäller även tooother regioner runt hello världen.

För dessa scenarier kan vi anropa ett ”hopp” för varje anslutning och antal dem lättare beskrivning

- **Hopp 1**: användaren toohello Application Proxy-tjänsten
- **Hopp 2**: Application Proxy tjänstanslutning toohello Application Proxy
- **Hopp 3**: Application Proxy connector toohello målprogram 

### <a name="use-case-1"></a>Användningsfall 1

**Scenario:** hello appar är i en organisations nätverk i hello US, med användare i hello samma region. Inga ExpressRoute eller VPN finns mellan hello Azure-datacenter och hello företagsnätverket.

**Rekommendation:** Följ mönstret 1, som beskrivs i föregående avsnitt i hello. Överväg att använda ExpressRoute för bättre svarstid, om det behövs.

Det här är ett enkelt mönster. Du kan optimera hopp 3 genom att placera hello connector nära hello app. Detta är också en naturlig valet eftersom hello connector installeras normalt med fri toohello appen och toohello datacenter tooperform KCD-åtgärder.

![Diagram som visar att användare, proxy, koppling och app finns i hello oss](./media/application-proxy-network-topologies/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Användningsfall 2

**Scenario:** hello appar är i en organisations nätverk i hello US, med användare som spridda globalt. Inga ExpressRoute eller VPN finns mellan hello Azure-datacenter och hello företagsnätverket.

**Rekommendation:** Följ mönstret 1, som beskrivs i föregående avsnitt i hello. 

Hello vanliga mönstret är igen, toooptimize hopp 3, placerar du hello connector nära hello app. Hopp 3 inte är vanligtvis dyrbara, om det är hello allt i samma region. Hopp 1 kan dock vara dyrare beroende på var användaren hello är eftersom användarna över hello world måste komma åt hello Application Proxy-instans i hello USA. Det är värt att nämna att en proxy-lösning har liknande egenskaper för användare att sprida ut globalt.

![Diagram som visar att användare sprids globalt, men hello proxy-, anslutnings- och app finns i hello oss](./media/application-proxy-network-topologies/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Användningsfall 3

**Scenario:** hello appar är i en organisations nätverk i hello oss. ExpressRoute med offentlig peering finns mellan Azure och hello företagsnätverket.

**Rekommendation:** följer mönster 1 och 2, förklaras i hello föregående avsnitt.

Placera först hello connector så nära som möjligt toohello app. Hello system använder sedan automatiskt ExpressRoute för hopp 2. 

Om hello ExpressRoute länk använder offentlig peering, överlappar hello trafik mellan hello proxy och hello connector länken. Hopp 2 har optimerats svarstid.

![Diagram över ExpressRoute mellan hello proxy och connector](./media/application-proxy-network-topologies/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Användningsfall 4

**Scenario:** hello appar är i en organisations nätverk i hello oss. ExpressRoute med privat peering finns mellan Azure och hello företagsnätverket.

**Rekommendation:** Följ mönstret 3, som beskrivs i föregående avsnitt i hello.

Placera hello connector i hello Azure-datacenter som är anslutna toohello företagsnätverket via privata ExpressRoute-peering. 

hello-anslutningen kan placeras i hello Azure-datacenter. Eftersom hello connector har ändå en fri toohello program och hello datacenter genom hello privat nätverk, förblir hopp 3 optimerad. Dessutom kan optimeras hopp 2 ytterligare.

![Diagram som visar hello connector i ett Azure-datacenter och ExpressRoute mellan hello koppling och appen](./media/application-proxy-network-topologies/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Användningsfall 5

**Scenario:** hello appar är i en organisations nätverk i hello EU, med hello Application Proxy-instans och de flesta användare i hello oss.

**Rekommendation:** plats hello connector nära hello app. Eftersom USA användare har åtkomst till en Application Proxy-instans som händer toobe i hello samma region hopp 1 är inte för dyrt. Hopp 3 är optimerad. Överväg att använda ExpressRoute toooptimize hopp 2. 

![Diagram över användare och proxy i hello USA med hello koppling och appen i hello Europa](./media/application-proxy-network-topologies/application-proxy-pattern5b.png)

Du kan också använda en variant i den här situationen. Om de flesta användare i hello organisation har hello oss, har troligen för att nätverket utökar toohello oss också. Placera hello connector i hello USA och använda hello dedikerad interna företagsnätverket rad toohello programmet i hello Europa. Det här sättet hopp 2 och 3 är optimerad.

![Diagram över användare och proxy connector i hello USA app i hello Europa](./media/application-proxy-network-topologies/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Nästa steg

- [Aktivera Application Proxy](active-directory-application-proxy-enable.md)
- [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md)
- [Aktivera villkorlig åtkomst](active-directory-application-proxy-conditional-access.md)
- [Felsökning av problem med Application Proxy](active-directory-application-proxy-troubleshoot.md)
