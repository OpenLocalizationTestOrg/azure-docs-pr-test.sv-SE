---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa kraven på dataskydd | Microsoft Docs"
description: "När du planerar din hybrididentitetslösning identifiera uppfyller hello kraven på dataskydd för ditt företag och vilka alternativ som är tillgängliga toobest dessa krav."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 189abf9affbc2894c322f362d84222d4e33d472e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Planera för att förbättra datasäkerhet via starkt identitetslösning
hello första steg tooprotect hello data är identifiera vem som kan komma åt dessa data och som en del av den här processen måste toohave en identitetslösning som kan integreras med systemets tooprovide autentisering och auktorisering kapacitet. Autentisering och auktorisering ihop ofta med varandra och deras roller tror många. I verkligheten är de helt annorlunda, som visas i hello bilden nedan:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)

**Livscykelstadier för hantering av mobila enheter**

När du planerar din hybrididentitetslösning måste du förstå kraven på dataskydd hello för ditt företag och vilka alternativ som är tillgängliga toobest uppfyller dessa krav.

> [!NOTE]
> När du är klar med att planera för datasäkerhet läsa [ange krav för multifaktorautentisering](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) tooensure att dina val av multifaktorautentisering kraven inte har påverkats av hello beslut du har gjort i det här avsnittet.
> 
> 

## <a name="determine-data-protection-requirements"></a>Fastställa kraven på dataskydd
I hello ålder mobility, de flesta företag har ett gemensamt mål: aktivera sina användare toobe produktiva på sina mobila enheter lokalt eller fjärranslutet från var som helst i ordning tooincrease produktivitet. Detta kan bero på ett gemensamt mål, men företag som har sådana krav blir också problem angående hello mängden hot som måste uppvägas i ordning tookeep företagsdata säker och skyddar användarens sekretess. Alla företag kan ha olika krav i detta avseende; olika kompatibilitetsregler som varierar bl.a toowhich branschen hello företagets fungerar kommer leda toodifferent designbeslut. 

Det finns vissa säkerhetsaspekter som ska vara utforskade och verifierats, oavsett hello branschen, som beskrivs i nästa avsnitt om hello.

## <a name="data-protection-paths"></a>Sökvägar för data protection
![](./media/hybrid-id-design-considerations/data-protection-paths.png)

**Sökvägar för data protection**

I hello ovan diagram hello identitet komponenten kommer att vara hello först en toobe verifieras innan data används. Dessa data kan vara i olika tillstånd under hello gången den användes. Varje tal i det här diagrammet representerar en sökväg där data kan finnas på någon punkt i tiden. Dessa värden beskrivs nedan:

1. Hello enhetsnivå dataskydd.
2. Dataskydd under överföring.
3. Dataskydd när resten lokala.
4. Dataskydd när resten i hello molnet.

Även om hello tekniska kontroller som gör att IT tooprotect hello själva informationen på var och en av dessa faser inte erbjuds direkt av hello hybrididentitetslösning, är det nödvändigt att hello hybrididentitetslösning kan utnyttja både lokalt och molnet identity management resurser tooidentify hello användare innan du kan bevilja åtkomst till toohello data. När du planerar din hybrididentitetslösning se till att hello följande besvaras frågor enligt tooyour organisations behov:

## <a name="data-protection-at-rest"></a>Skydd av data i vila
Oavsett var finns hello data i vila (enhet, molnet eller lokalt), är det viktigt tooperform organisationen assessment toounderstand hello måste i detta sammanhang. Se till att följande frågor hello uppmanas för det här området:

* Behöver företaget tooprotect data i vila?
  * Om Ja, är hello hybrid identity-lösning kan toointegrate med den befintliga lokala infrastrukturen?
  * Om Ja, är hello hybrid identity-lösning kan toointegrate med dina arbetsbelastningar som finns i hello moln?
* Är hello molnet identity management kan tooprotect hello användarens autentiseringsuppgifter och andra data som lagras i hello molnet?

## <a name="data-protection-in-transit"></a>Dataskydd under överföring
Data under överföring mellan hello enheten och hello datacenter eller hello enheten och hello molnet måste skyddas. Dock innebär som transit inte nödvändigtvis en process för kommunikation med en komponent utanför Molntjänsten; flyttas internt, även, till exempel mellan två virtuella nätverk. Se till att följande frågor hello uppmanas för det här området:

* Behöver företaget tooprotect data under överföring?
  * Om Ja, är hello hybrid identity-lösning kan toointegrate med säkra kontroller, till exempel SSL/TLS?
* Hello molnet Identitetshantering hållas hello trafik tooand hello katalogdatabas (inom och mellan datacenter) signerade?

## <a name="compliance"></a>Efterlevnad
Förordningar, lagar och regelefterlevnad krav varierar bl.a toohello bransch som tillhör företaget. Företag i hög reglerade branscher måste hantera Identitetshantering frågor relaterade toocompliance. Förordningar som Sarbanes-Oxley (SOX), hello Health Insurance Portability och Accountability Act (HIPAA), hello Gramm Leach Bliley Act (GLBA) och Payment Card Industry Data Security Standard (PCI DSS) hello är mycket strikt om identitets- och. Hej hybrididentitetslösning som företaget antar måste ha hello grundfunktionerna som uppfyller hello kraven i en eller flera av dessa regler. Se till att följande frågor hello uppmanas för det här området:

* Är hello hybrididentitetslösning kompatibel med hello regelkrav för ditt företag?
* Innehåller hello hybrididentitetslösning har inbyggda funktioner som gör att ditt företag toobe kompatibla regelkrav? 

> [!NOTE]
> Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret. [Definiera en strategi för Data Protection](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) kommer att överskrida hello tillgängliga alternativ och fördelar/nackdelar med varje alternativ.  Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.
> 
> 

## <a name="next-steps"></a>Nästa steg
 [Ange krav för innehållshantering](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

