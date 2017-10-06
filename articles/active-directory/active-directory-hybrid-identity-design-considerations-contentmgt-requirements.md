---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa kraven för innehållshantering | Microsoft Docs"
description: "Ger inblick i hur toodetermine hello innehållshantering kraven i företaget. Vanligtvis kanske när en användare har sin egen enhet han också flera autentiseringsuppgifter som ska alternerande bl.a toohello program som han använder. Det är viktigt toodifferentiate vilka innehållet har skapats med hjälp av personliga autentiseringsuppgifter jämfört med hello som skapas med företagets autentiseringsuppgifter. Din identitetslösning ska vara kan toointeract med cloud services tooprovide en smidigt toohello slutanvändare vid säkerställa hans sekretess och öka hello skydd mot dataläckage."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 607d366633c37b65ec5cf8ae5c64d73ca1cc96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Ange krav för innehållshantering för din hybrididentitetslösning
Så här fungerar hello innehållshantering kraven påverka för din verksamhet kan direkt ditt beslut på vilka hybrid identity lösning toouse. Med hello spridning av flera enheter och hello möjligheterna för användare toobring sina egna enheter ([BYOD](http://aka.ms/byodcg)), hello företag måste skydda egna data men den måste behålla användarens integritet intakta. Vanligtvis kanske när en användare har sin egen enhet han också flera autentiseringsuppgifter som ska alternerande bl.a toohello program som han använder. Det är viktigt toodifferentiate vilka innehållet har skapats med hjälp av personliga autentiseringsuppgifter jämfört med hello som skapas med företagets autentiseringsuppgifter. Din identitetslösning ska vara kan toointeract med cloud services tooprovide en smidigt toohello slutanvändare vid säkerställa hans sekretess och öka hello skydd mot dataläckage. 

Identity-lösningen kommer utnyttjas av olika tekniska kontroller i ordning tooprovide innehållshantering enligt hello bilden nedan:

![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Säkerhetsåtgärder som kommer kunna utnyttja systemet identity management**

I allmänhet använder innehållshantering krav din identitet hanteringssystemet hello följande områden:

* Sekretess: identifiera hello-användaren som äger en resurs och tillämpa hello lämpliga kontroller toomaintain integritet.
* Dataklassificering: identifiera hello användaren eller gruppen och andelen tooan objektåtkomst enligt tooits klassificering. 
* Läckage av dataskydd: säkerhetsåtgärder som är ansvarig för att skydda tooavoid dataläckage behöver toointeract med hello identitet system toovalidate hello användarens identitet. Detta är även viktigt för spår revision.

> [!NOTE]
> Läs [dataklassificering moln beredskap för](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) mer information om metodtips och riktlinjer för dataklassificering.
> 
> 

När du planerar din hybrididentitetslösning se till att hello följande besvaras frågor enligt tooyour organisations behov:

* Har företaget säkerhetsåtgärder i plats tooenforce datasekretess?
  * Om Ja, kommer dessa säkerhetsåtgärder att kan toointegrate med hello hybrididentitetslösning att du är pågående tooadopt?
* Använder företaget dataklassificering?
  * Om Ja, är hello aktuella lösningen kan toointegrate med hello hybrididentitetslösning att du är pågående tooadopt?
* Har ditt företag för närvarande någon lösning för dataläckage? 
  * Om Ja, är hello aktuella lösningen kan toointegrate med hello hybrididentitetslösning att du är pågående tooadopt?
* Behöver företaget tooaudit åtkomst tooresources?
  * Om Ja, vilken typ av resurser?
  * Om Ja, hur mycket information som krävs?
  * Om Ja, där hello granskningsloggen måste finnas? Lokalt eller i molnet hello?
* Behöver företaget tooencrypt alla e-postmeddelanden som innehåller känsliga data (personnummer, kreditkortsnummer o.s.v.)?
* Behöver företaget tooencrypt alla dokument/innehållet delas med affärspartners?
* Behöver företaget tooenforce företagsprinciper på vissa typer av e-postmeddelanden (gör något alla svar, vidarebefordra inte)?

> [!NOTE]
> Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret. [Definiera en strategi för Data Protection](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) kommer att överskrida hello tillgängliga alternativ och fördelar/nackdelar med varje alternativ.  Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.
> 
> 

## <a name="next-steps"></a>Nästa steg
[Fastställa krav på åtkomstkontroll](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

