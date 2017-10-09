---
title: "aaaAzure Active Directory-hybrididentitet utformning - Bestäm hanteringsuppgifter för hybrid identity | Microsoft Docs"
description: "Med villkorlig åtkomstkontroll kontrollerar hello särskilda villkor som du väljer när du autentiserar användaren hello och innan åtkomst toohello program i Azure Active Directory. När dessa villkor är uppfyllda, hello användare autentiseras och tillåtet åtkomst toohello program."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: d3c0e9b23f43127b3d8e0b3a4e8f03d4bc148c27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Planera för Hybrid Identity livscykel
Identitet är en hello grunderna för enterprise mobility och programmet åtkomststrategi. Om du registrerar på mobila enheter för tooyour eller SaaS-app, är din identitet hello viktiga toogaining åtkomst tooeverything. På den högsta nivån omfattar en lösning för Identitetshantering enhetlig och synkroniseringen mellan din identitet databaser som innehåller automatisering och centralisera hello processen för etablering av resurser. hello identitetslösning ska vara en centraliserad identitet i molnet och lokalt och också använda någon form av federation toomaintain centraliserad identitetsverifiering och på ett säkert sätt delar och samarbetar med externa användare och företag. Resurser mellan operativsystem och program toopeople i eller kopplad till en organisation. Organisationsstruktur kan vara ändrade tooaccommodate hello provisioning principer och procedurer.

Det är också viktigt toohave en identitetslösning är tooempower dina användare genom att ge dem med självbetjäning inträffar tookeep dem produktiva. Identitetslösning är mer robusta om det möjliggör enkel inloggning för användare över alla hello-resurser som de behöver åtkomst till administratörer på alla nivåer anvisningarna standardiserade för att hantera autentiseringsuppgifter. Vissa nivåer för administration kan minskas eller elimineras beroende på hello breda hello etablering hanteringslösning. Dessutom kan du på ett säkert sätt distribuera administrationsfunktioner, manuellt eller automatiskt, mellan olika organisationer. Till exempel kan en domänadministratör fungera endast hello personer och resurser i domänen. Den här användaren kan utföra uppgifter för administration och etablering, men är inte auktoriserad toodo konfigurationsåtgärder, till exempel skapa arbetsflöden.

## <a name="determine-hybrid-identity-management-tasks"></a>Fastställa hanteringsuppgifter för Hybrid Identity
Distribuera administrativa uppgifter i din organisation förbättrar hello Precision och effektivitet för administration och förbättrar hello balans mellan hello arbetsbelastningen för en organisation. Följande är hello vrider som definierar en robust identity management-systemet.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)

Du måste förstå vissa grundläggande egenskaper för hello organisation som kommer att anta hybrididentitet toodefine hybrid identity administrativa uppgifter. Det är viktigt toounderstand hello aktuella databaser som används för identitet källor. Genom att känna till de viktiga element, du har hello grundläggande krav och baserat på att du måste tooask mer detaljerade frågor som leder dig tooa bättre beslut för din identitetslösning.  

När du definierar dessa krav, se till att som hello minst följande frågor besvaras

* Alternativ för etablering: 
  
  * Stöder hello hybrididentitetslösning en robust åtkomst kontohantering och provisioning system?
  * Hur är användare, grupper och lösenord som ska hanteras toobe?
  * Svarar Livscykelhantering för hello identitet? 
    * Hur lång tid tar lösenord uppdateringar konto upphävande?
* Licenshantering: 
  
  * Hello lösning hanterar licens hybrididentitetshantering?
    * Om Ja, vilka funktioner är tillgängliga?
* Hello lösning referensen gruppbaserade licenshantering? 
  
      - Om Ja, är det möjligt tooassign en säkerhet grupp tooit? 
       - Om Ja, hello molnkatalog tilldelar automatiskt licenser tooall hello medlemmar i hello gruppen? 
        - Vad händer om en användare tas bort från gruppen hello eller senare läggs till kommer en licens automatiskt tilldelas eller tas bort efter behov? 
* Integrering med andra identitetsleverantörer för från tredje part:
* Den här kombinerade lösningen integreras med tredje parts identitet providers tooimplement enkel inloggning?
* Är det möjligt toounify alla hello olika identitetsleverantörer till ett identitetssystem med enhetlig?
* Om Ja, hur och som är de och vilka funktioner som är tillgängliga?

## <a name="synchronization-management"></a>Hantering av datasynkronisering
Ett av hello målen för en identitetshanterare för, toobe kan toobring alla hello identitetsleverantörer och hålla dem synkroniserade. Du kan synkronisera hello data baserat på en auktoritativ master identitetsleverantör. I hybrididentitetsscenario med en modell med synkroniserade management, Hantera identiteter för alla användare och enhet i en lokal server och synkronisera hello konton och eventuellt lösenord toohello moln. hello användaren anger hello samma lösenord lokalt som han eller hon i hello molnet och vid inloggning, hello lösenord har verifierats av hello identitetslösning. Den här modellen används en verktyget katalogsynkronisering.

![](./media/hybrid-id-design-considerations/Directory_synchronization.png)tooproper design hello synkronisering av din hybrididentitetslösning se till att hello följande frågor besvaras: • Vad är tillgängliga för hello hybrididentitetslösning hello sync lösningar?
• Vad är hello enkel inloggning som finns tillgängliga?
• Hello alternativ för identitetsfederation mellan B2B och B2C?

## <a name="next-steps"></a>Nästa steg
[Fastställa införandestrategin hybrid identity](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

