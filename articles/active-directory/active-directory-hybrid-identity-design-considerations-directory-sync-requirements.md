---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa kraven för directory-synkronisering | Microsoft Docs"
description: "Identifiera vilka krav som behövs för att synkronisera alla hello användare mellan on = lokala och molnbaserade för hello enterprise."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6646e3792c65f37c3d62eecdb6c6f3bd257f04f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-directory-synchronization-requirements"></a>Ange krav för directory-synkronisering
Synkronisering är att tillhandahålla användare en identitet i hello molnet baserat på deras lokala identitet. Oavsett om de ska använda synkroniserade kontot för autentisering eller federerad autentisering, måste hello användarna toohave en identitet i hello molnet.  Den här identiteten måste toobe underhålls och uppdateras regelbundet.  hello uppdateringar kan ta många formulär från titel ändringar toopassword ändras.  

Starta genom att utvärdera hello organisationer krav på lokal identitet lösningen och användare. Denna utvärdering är viktiga toodefine hello tekniska krav för hur användaridentiteter skapas och underhålls i molnet hello.  Active Directory är lokala för en majoriteten av organisationer och hello på lokal katalog som användare kommer att synkroniseras från, men i vissa fall detta kommer inte att hello fallet.  

Se till att tooanswer hello följande frågor:

* Har du en AD-skog, flera eller ingen?
  
  * Hur många Azure AD-kataloger kommer du att synkroniseras till?
    
    1. Använder du filtrering?
    2. Har du flera Azure AD Connect-servrar som är planerat?
* Har du en synkronisering verktyget lokalt?
  
  * Om Ja, har du dina användare om användarna har en virtuell katalog/integrering av identiteter?
* Har du alla andra directory lokalt som du vill toosynchronize (t.ex. LDAP-katalogen, HR-databasen, osv)?
  * Ska du göra några GALSync toobe?
  * Vad är hello aktuell status för UPN-namn i din organisation? 
  * Har du en annan katalog som användare autentiseras mot?
  * Använder företaget Microsoft Exchange?
    * Kommer de att en exchange-hybridinstallation?

Nu när du har en uppfattning om synkroniseringskrav för måste toodetermine vilket verktyg som hello rätt en toomeet dessa krav.  Microsoft erbjuder flera verktyg tooaccomplish directory-integrering och synkronisering.  Se hello [Hybrididentitet directory integration verktyg jämförelsetabell](active-directory-hybrid-identity-design-considerations-tools-comparison.md) för mer information. 

Nu när du har synkroniseringskrav och hello-verktyg som gör det för ditt företag måste tooevaluate hello program som använder dessa katalogtjänster. Denna utvärdering är viktigt toodefine hello tekniska krav toointegrate dessa program toohello moln. Se till att tooanswer hello följande frågor:

* Dessa program kommer att flyttas toohello molnet och använda hello-katalog?
* Finns det särskilda attribut som måste synkroniseras toobe toohello molnet så att programmen kan använda dem har?
* Behöver programmen toobe igen skrivs tootake utnyttja molnet auth?
* Dessa program fortsätter toolive lokalt medan användarna komma åt dem med hjälp av molnidentitet hello?

Du måste också toodetermine hello säkerhet kraven och begränsningarna katalogsynkronisering. Denna utvärdering är viktiga tooget en lista över hello kraven som krävs i ordning toocreate och underhålla användaridentiteter i hello molnet. Se till att tooanswer hello följande frågor:

* Där hello synkroniseringsserver placeras?
* Kommer det att vara ansluten till en domän?
* Hello-servern finns på ett begränsat nätverk bakom en brandvägg, till exempel en DMZ?
  * Kommer du att kunna tooopen hello krävs brandväggen portar toosupport synkronisering?
* Har du en återställningsplan för hello synkroniseringsserver?
* Har du ett konto med hello rätt behörigheter för alla skogar som du vill toosynch med?
  * Om företaget inte vet hello svar för den här frågan, granska hello ”behörigheter för Lösenordssynkronisering” i avsnittet hello artikel [installera hello Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) och ta reda på om du redan har en konto med dessa behörigheter eller om du behöver toocreate en.
* Om du har inte mutli-skog sync hello synkronisering kan tooget tooeach-serverskogen?

> [!NOTE]
> Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret. [Fastställa kraven på incidentsvar](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) kommer att överskrida hello-alternativ som är tillgängliga. Har besvarat frågorna väljer du vilket alternativ som bäst passar dina behöver.
> 
> 

## <a name="next-steps"></a>Nästa steg
[Ange krav för multifaktorautentisering](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

