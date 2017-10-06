---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa kraven för incident rResponse | Microsoft Docs"
description: "Fastställa funktioner för övervakning och rapportering för hello hybrididentitetslösning som kan utnyttjas av IT tootake åtgärder tooidentify och risken för potentiella hot"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: a3d2a459-599b-4b67-8e51-7369ee25082d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 7084096f318ef461e8331fb6edde1b77d4108466
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Fastställa kraven på incidentsvar för din hybrididentitetslösning
Stora eller medelstora organisationer troligen har en [säkerhet incidenter](https://technet.microsoft.com/library/cc700825.aspx) i plats toohelp IT att vidta åtgärder i enlighet med detta toohello andelen incident. hello identity management-systemet är en viktig komponent i hello incidenter processen eftersom det kan vara används toohelp identifiera vem som utförde en specifik åtgärd mot hello mål. Hej hybrididentitetslösning måste vara kan tooprovide övervakning och rapporteringsfunktioner som kan utnyttjas av IT tootake åtgärder tooidentify och minimera potentiella hot. I en typisk incidentsvarsplanen har du följande faser som en del av planen hello hello:

1. Inledande kontroll.
2. Incident kommunikation.
3. Skada och minska kreditkortsrisker.
4. Identifiering av vad det var kompromettering och allvarlighetsgrad.
5. Bevis konservering.
6. Meddelande tooappropriate parter.
7. Systemåterställning.
8. Dokumentation.
9. Bedömning av skada och kostnader.
10. Processen och planera revision.

Under hello identifiering av IT-avdelningen och olika allvarlighetsgrad fas, blir det nödvändigt tooidentify hello system som har komprometterats filer som har öppnats och fastställa hello känslighet av dessa filer. Hybrid identity systemet ska vara kan toofulfill dessa krav tooassist dig att identifiera hello användare som gjort dessa ändringar. 

## <a name="monitoring-and-reporting"></a>Övervakning och rapportering
Många gånger hello identitetssystem kan också i första kompatibilitetsutvärderingsfasen huvudsakligen om hello system har skapats i gransknings- och rapporteringsfunktioner. IT-administratören måste vara kan tooidentify en misstänkt aktivitet under hello inledande bedömningen eller hello systemet ska vara kan tootrigger den automatiskt baserat på en förkonfigurerad uppgift. Många aktiviteter kan tyda på ett möjligt angrepp, men i andra fall kan ett felaktigt konfigurerat system leda tooa antalet falska positiva identifieringar i ett system för identifiering av adressintrång. 

hello identity management-systemet bör hjälpa IT-administratörer tooidentify och rapporterar de misstänkta aktiviteterna. Vanligtvis kan dessa tekniska krav uppfyllas genom att övervaka alla system och har en rapporteringsfunktion som kan fokusera på potentiella hot. Använd hello frågor under toohelp du utformar din hybrididentitetslösning när med hänsyn till kraven på incidentsvar tänka på:

* Har företaget en incidenter för säkerhet på plats?
  * Om Ja, hello aktuella identity management-systemet används som en del av hello?
* Behöver företaget tooidentify misstänkta inloggningsförsök från användare för olika enheter?
* Behöver företaget toodetect potentiellt komprometterade användarens autentiseringsuppgifter?
* Behöver företaget tooaudit användarens åtkomst och åtgärden?
* Behöver företaget tooknow när en användare återställa sina lösenord?

## <a name="policy-enforcement"></a>Tvingande principer
Vid skada och risker minskning-fas är det viktigt tooquickly minska hello faktiska och potentiella effekterna av ett angrepp. Den åtgärd som ska tas kan nu göra hello skillnaden mellan en mindre och en större. hello exakta svaret beror på din organisation och hello uppbyggnad hello angrepp som kan uppstå. Om hello inledande assessment slutsatsen att ett konto komprometteras, behöver du tooenforce princip tooblock det här kontot. Det är bara ett exempel där hello identity management-systemet kommer utnyttjas. Använd hello frågor under toohelp som du skapar din hybrididentitetslösning med hänsyn till hur principer kommer att tillämpas tooreact tooan pågående incident:

* Har företaget principer plats tooblock användare från åtkomst hello nätverket vid behov?
  * Om Ja, kan hello aktuella lösningen integreras med hello hybrid identity management system att du är pågående tooadopt?
* Behöver företaget tooenforce villkorlig åtkomst för användare som är i karantän? 

> [!NOTE]
> Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret. [Definiera en strategi för data protection](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) kommer att överskrida hello tillgängliga alternativ och fördelar/nackdelar med varje alternativ.  Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.
> 
> 

## <a name="next-steps"></a>Nästa steg
[Definiera en strategi för skydd av data](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

