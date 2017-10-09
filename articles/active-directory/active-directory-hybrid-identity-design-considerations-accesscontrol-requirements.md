---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa krav på åtkomstkontroll | Microsoft Docs"
description: "Omfattar hello pelare för identitets- och identifiera krav för resurser för användare i en hybridmiljö."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: f0c22629f732a4c13ee7a24456651bec7637c387
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Fastställa krav på åtkomstkontroll för din hybrididentitetslösning
Vid utformning av en organisation sina hybrididentitetslösning som de kan också använda den här möjligheten tooreview åtkomstkrav för hello resurser som de planerar toomake den tillgänglig för användare. hello dataåtkomst mellan alla fyra pelare identitet, som är:

* Administration
* Autentisering
* Auktorisering
* Granskning

hello avsnitten som följer beskriver autentisering och auktorisering mer detaljerat, administration och granskning ingår i hello hybrid identity livscykel. Läs [fastställa hanteringsuppgifter för hybrid identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) mer information om dessa funktioner.

> [!NOTE]
> Läs [hello fyra pelare av Identity - Identity Management i hello ålder av Hybrid-IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) mer information om var och en av dessa pelare.
> 
> 

## <a name="authentication-and-authorization"></a>Autentisering och auktorisering
Det finns olika scenarier för autentisering och auktorisering, dessa scenarier har specifika krav som måste uppfyllas av hello hybrididentitetslösning att hello företag är pågående tooadopt. Scenarier som involverar Business tooBusiness (B2B) kommunikation kan lägga till en extra utmaning för IT-administratörer eftersom de måste tooensure hello autentisering och auktorisering metod som används av hello organisation kan kommunicera med sina affärspartner. Under hello designa process för krav på autentisering och auktorisering, kontrollerar du att hello följande frågor besvaras:

* Kommer din organisation autentisera och auktorisera endast användare som finns i deras identitet hanteringssystemet?
  * Finns det några planer för B2B-scenarier?
  * Om Ja, du redan vet vilka protokoll (SAML OAuth, Kerberos, token eller certifikat) kommer att använda tooconnect både företag?
* Stöder hello hybrididentitetslösning att du ska tooadopt dessa protokoll?

En annan viktig punkt tooconsider är där hello autentisering databasen som ska användas av användare och partners kommer att finnas och hello administrativa modellen toobe används. Överväg följande två alternativ för kärnor hello:

* Centraliserad: i den här modellen hello användarens autentiseringsuppgifter, principer och administration kan vara lokalt centraliserad eller i hello molnet.
* Hybrid: i den här modellen hello användarens autentiseringsuppgifter, principer och administration ska finnas lokalt centraliserad och en replikerad i hello molnet.

Vilken modell som företaget antar varierar bl.a tootheir affärsbehov vill du tooanswer hello följande frågor tooidentify där hello identity management-systemet finns och hello administrativa läge toouse:

* För närvarande har organisationen en Identitetshantering lokalt?
  * Om Ja, de planerar tookeep den?
  * Finns det några förordning eller efterlevnad krav som din organisation måste följa de bestämmer där hello identity management-systemet ska finnas?
* Använder organisationen enkel inloggning för appar som finns lokalt eller i molnet hello?
  * Om Ja, hello införandet av en modell för hybrid identity påverkar processen?

## <a name="access-control"></a>Access Control
Autentisering och auktorisering är core element tooenable åtkomst toocorporate data via användarens verifiering, är det också viktigt toocontrol hello åtkomstnivå som användarna har och hello andelen administratörer har över hello resurser som de hanterar. Din hybrididentitetslösning måste vara kan tooprovide detaljerade åtkomst tooresources delegering och rollbaserad åtkomstkontroll. Se till att följande fråga hello besvaras om åtkomstkontroll:

* Ditt företag har fler än en användare med förhöjda privilegier toomanage systemet identitet?
  * Om Ja, måste varje användare hello samma åtkomstnivå?
* Skulle företagets behov toodelegate åtkomst toousers toomanage specifika resurser?
  * Om Ja, hur ofta detta sker?
* Behöver företaget toointegrate kontroll åtkomstfunktioner mellan lokala och moln resurser?
* Behöver företaget toolimit åtkomst tooresources enligt toosome villkor?
* Företaget skulle ha alla program som behöver en anpassad kontroll åtkomst toosome resurser?
  * Om Ja, var apparna finns (lokalt eller i molnet hello)?
  * Om Ja, där är de mål resurser (lokalt eller i molnet hello)?

> [!NOTE]
> Se till att tootake anteckningar för varje svar och förstå hello anledningen hello svaret. [Definiera en strategi för Data Protection](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) kommer att överskrida hello tillgängliga alternativ och fördelar/nackdelar med varje alternativ.  Genom att besvara frågorna väljer du vilket alternativ som bäst passar dina affärsbehov.
> 
> 

## <a name="next-steps"></a>Nästa steg
[Fastställa kraven på incidentsvar](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

