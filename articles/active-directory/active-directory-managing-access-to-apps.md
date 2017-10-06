---
title: "aaaManaging åtkomst tooapps med hjälp av Azure AD | Microsoft Docs"
description: "Beskriver hur Azure Active Directory kan organisationer toospecify hello appar toowhich varje användare har åtkomst till."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: b0829f18-9e57-4107-925d-5f0457d81671
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: b9461b7a1cc8913cd8fb4a4ce0778afe03274935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-access-tooapps"></a>Hantera åtkomst tooapps
Pågående hantering, användning utvärdering och rapportering fortsätta toobe en utmaning när en app har integrerats i din organisation identitetssystem. I många fall kan IT-administratörer eller supportavdelningen har tootake en pågående aktiv roll i att hantera åtkomst tooyour appar. Ibland utförs tilldelning av en allmän eller avdelningar IT-teamet. Hello tilldelning beslut är ofta avsedda toobe delegerad toohello beslutsfattare inom företaget, som kräver godkännande innan IT gör hello tilldelning.  Andra organisationer investera i integrering med ett befintligt automatiserade identitets- och -system, t.ex. rollbaserad åtkomstkontroll (RBAC) eller attributbaserad åtkomstkontroll (ABAC). Både hello-integrering och regeln utveckling tenderar toobe specialiserade och dyrt. Övervakning och rapportering om antingen metoden är en egen separat kostsamma och komplexa investering.

## <a name="how-does-azure-active-directory-help"></a>Hur hjälper Azure Active Directory?
 Azure AD stöder omfattande åtkomsthantering för konfigurerade-program organisationer tooeasily uppnå hello rätt principer, från automatisk, attributbaserad tilldelning (ABAC eller RBAC-scenario) genom delegering och inklusive administratörshantering av. Med Azure AD kan du enkelt åstadkomma komplexa principer kombinera flera management modeller för ett enskilt tillämpningsprogram och kan även återanvända management regler för program med hello samma målgrupper.

* [Lägger till nya eller befintliga program](active-directory-sso-integrate-saas-apps.md)

 Azure AD application tilldelning fokuserar på två primära tilldelning lägen:

* **Tilldelning av enskilda** en IT-administratör med globala administratörsbehörigheter för directory välja enskilda användarkonton och ge dem åtkomst toohello program.
* **Gruppbaserad tilldelning (betald Azure AD)** en IT-administratör med directory globala administratörsbehörigheter kan tilldela en grupp toohello program. Specifika användare åtkomst bestäms av om de är medlemmar i gruppen hello hello försök tooaccess hello program. Med andra ord kan en administratör effektivt skapa en tilldelningsregel anger ”alla aktuella medlemmar i hello som tilldelats gruppen har åtkomst toohello program”. Med det här alternativet om tilldelning kan administratörer kan dra nytta av någon av Azure AD grupphanteringsalternativ, inklusive [attributbaserad dynamiska grupper](active-directory-accessmanagement-manage-groups.md), externt system (exempelvis lokala Active Directory eller Workday), eller hanteras av en administratör eller egen service-hanterade grupper. En grupp kan enkelt tilldelas toomultiple appar säkerställer att program med tilldelning tillhörighet kan dela tilldelningsregler, vilket minskar hello övergripande hanteringen. Observera att kapslad grupp medlemskap inte stöds för gruppbaserad tilldelning tooapplications just nu.

Med dessa tilldelning av två lägen kan kan administratörer uppnå metoden alla önskvärt tilldelning.

Med Azure AD, är användning och tilldelning reporting helt integrerade, aktivera administratörer tooeasily rapporten på tilldelningen av Tilldelningsfel och även användning.

## <a name="complex-application-assignment-with-azure-ad"></a>Programtilldelning av komplexa med Azure AD
Överväg att ett program som Salesforce. I många organisationer används främst Salesforce av hello marknadsförings- och organisationer. Medlemmar i hello Marknadsföringsteamet har ofta mycket Privilegierade åtkomst tooSalesforce medan medlemmarna i säljgruppen hello har begränsad åtkomst. I många fall har en bred uppsättning informationsarbetare begränsad åtkomst toohello program. Undantag toothese regler göra det svårare för frågor. Det är ofta hello förmånsrätt av hello marknadsföring eller försäljning ledande team toogrant en tillgång eller ändra deras roller oberoende av dessa allmänna regler.

Med Azure AD kan program som Salesforce vara förkonfigurerade för enkel inloggning (SSO) och automatisk etablering. När programmet hello har konfigurerats kan en administratör ta hello engångsåtgärd toocreate och tilldela hello relevanta grupper. I det här exemplet kan en administratör kör hello följande tilldelningar:

* [Dynamiska grupper](active-directory-accessmanagement-manage-groups.md) kan vara definierade tooautomatically representera alla medlemmar i hello marknadsförings- och grupper med hjälp av attribut som avdelningen eller rollen:
  
  * Alla medlemmar i marknadsföring grupper tilldelas toohello ”marknadsföring” roll i Salesforce
  * Alla medlemmar i försäljnings team grupper tilldelas toohello ”säljare” roll i Salesforce. En ytterligare förfining kan använda flera grupper som representerar regionala försäljning team som tilldelats toodifferent Salesforce-roller.
* undantagsmekanism för tooenable hello gick att skapa en grupp med självbetjäning för varje roll. Hej ”Salesforce marknadsföring undantag” grupp kan till exempel skapas som en grupp för självbetjäning. hello gruppen kan tilldelas toohello Salesforce marknadsföring roll och hello marknadsföring ledning kan göras ägare. Medlemmar i hello marknadsföring ledning kan lägga till eller ta bort användare, ange en princip för koppling eller även godkänna eller neka begäranden toojoin för enskilda användare. Detta stöds via en information worker lämpliga upplevelse som inte kräver särskilda utbildning för ägare eller medlemmar.

I det här fallet är alla tilldelade användare automatiskt etablerade tooSalesforce eftersom grupper för tillagda toodifferent sina rolltilldelning skulle uppdateras i Salesforce. Användare skulle kunna toodiscover och åtkomst till Salesforce via åtkomstpanelen för hello Microsoft-program, Office webbklienter eller även genom att gå tootheir organisationens Salesforce-inloggningssidan. Administratörer skulle vara kan tooeasily visa användnings- och tilldelning status med hjälp av Azure AD-rapportering.

Administratörer kan använda [villkorlig åtkomst i Azure AD](active-directory-conditional-access.md) tooset åtkomstprinciper för specifika roller. Dessa principer kan innehålla om åtkomst tillåts utanför hello företagsmiljö och även Multifaktorautentisering eller Enhetsåtkomst krav tooachieve i olika fall.

## <a name="how-can-i-get-started"></a>Hur kan jag igång?
Första, om du inte redan använder Azure AD och du är IT-administratör:

* [Prova!](https://azure.microsoft.com/trial/get-started-active-directory/) -Du kan registrera dig för en kostnadsfri 30-dagars utvärderingsversion idag och distribuera din första molnlösning under 5 minuter med hjälp av den här länken

Azure AD-funktioner som aktiverar delning av konto är:

* [Grupptilldelning](active-directory-accessmanagement-self-service-group-management.md)
* Lägga till program tooAzure AD
* Komma igång med tilldelning
* Programmet tilldelning vanliga frågor och svar
* [Appen instrumentpanelen/användningsrapporter](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Var kan jag lära sig mer?
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Skydda appar med villkorlig åtkomst](active-directory-conditional-access.md)
* [Självbetjäning hantering/SSAA](active-directory-accessmanagement-self-service-group-management.md)

