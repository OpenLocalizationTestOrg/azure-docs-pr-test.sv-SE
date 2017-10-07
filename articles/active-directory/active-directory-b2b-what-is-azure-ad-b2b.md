---
title: "aaaWhat är Azure Active Directory B2B-samarbete? | Microsoft Docs"
description: "Företagsomfattande relationer genom att aktivera business partners tooselectively åtkomst till företagets program har stöd för Azure Active Directory B2B-samarbete."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.custom: aaddev
ms.reviewer: sasubram
ms.openlocfilehash: 359989b66f3d012c306e8748a607662fffacb919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Vad är Azure AD B2B-samarbete?

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

Azure AD business-to-business (B2B) funktioner kan en organisation med Azure AD toowork enkelt och säkert användare från någon annan organisation små eller stora. Dessa organisationer kan vara med Azure AD eller utan, eller med en IT-organisation eller utan. 

Organisationer som använder Azure AD kan ge åtkomst toodocuments, resurser och program tootheir partners, samtidigt som fullständig kontroll över företagets data. Utvecklare kan använda hello Azure AD business-to-business-API: er toowrite program som samordnar två organisationer i mer på ett säkert sätt. Det är också praktiskt för slutanvändare toonavigate.

97% av våra kunder bett oss om Azure AD B2B-samarbete är mycket viktigt toothem.

![cirkeldiagram](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

Vi hade cirka 3 miljoner användare använder redan Azure AD B2B-samarbetesfunktioner från och med tidig April 2017. Och mer än 23% av Azure AD-organisationer som har fler än 10 användare drar nytta redan av dessa funktioner.

## <a name="hello-key-benefits-of-azure-ad-b2b-collaboration-tooyour-organization"></a>hello viktiga fördelar med Azure AD B2B-samarbete tooyour organisation

### <a name="work-with-any-user-from-any-partner"></a>Arbeta med alla användare från någon part

* Partners använda sina egna autentiseringsuppgifter

* Inga krav för partners toouse Azure AD

* Inga externa kataloger eller komplexa installation krävs

### <a name="simple-and-secure-collaboration"></a>Enkelt och säkert samarbete

* Ge åtkomst tooany företagets app eller data, samtidigt som du använder avancerade, Azure AD-påslagen auktoriseringsprinciper

* Enkelt för användare

* Säkerhet i företagsklass för appar och data

### <a name="no-management-overhead"></a>Inga hanteringskostnader

* Ingen extern hantering av konto eller lösenord

* Ingen synkronisering eller manuell livscykel kontohantering

* Inga externa administrativa kostnader

## <a name="you-can-easily-add-b2b-collaboration-users-tooyour-organization"></a>Du kan enkelt lägga till B2B-samarbete användare tooyour organisation

Administratörer kan lägga till B2B-samarbete (Gäst) användare i hello [Azure-portalen](https://portal.azure.com).

![Lägg till gästanvändare](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-toobring-their-own-identity"></a>Aktivera din medarbetare toobring sin egen identitet

B2B medarbetare kan logga in med en identitet efter eget val. Om hello användaren har inte ett Microsoft-konto eller ett Azure AD-konto – en skapas för dem sömlöst vid hello för erbjudande åtgärd.

![logga in identitet val](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-tooapplication-and-group-owners"></a>Delegera tooapplication och gruppen ägare 
Program- och gruppen ägare kan lägga till B2B-användare direkt tooany program som intresserar dig, om det är ett Microsoft-program eller inte. Administratörer kan delegera behörighet tooadd B2B användare toonon-administratörer. Icke-administratörer kan använda hello [åtkomstpanelen för Azure AD-programmet](https://myapps.microsoft.com) tooadd B2B collaboration tooapplications för användare eller grupper.

![åtkomstpanelen](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![lägga till medlem](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Auktoriseringsprinciper skyddar företagets innehållet

Principer för villkorlig åtkomst, till exempel multifaktorautentisering, kan tillämpas:
- På nivån för hello-klient
- På programnivå hello
- För specifika användare tooprotect företagets appar och data

![lägga till medlem](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-tooeasily-build-applications-tooonboard"></a>Använd våra API: er och exempel kod tooeasily bygga program tooonboard
Ta din externa partners på tåget i sätt anpassade tooyour organisations behov.

Med hjälp av hello [B2B-samarbetsinbjudan API: er](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), du kan anpassa dina onboarding-upplevelser, inklusive att skapa registrering självbetjäningsportaler. Vi tillhandahåller exempelkod för en självbetjäningsportal [på Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![registreringsportalen](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

Med Azure AD B2B-samarbete du hello alla fördelar med Azure AD tooprotect din partnerrelationer på ett sätt som slutanvändarna hitta enkelt och intuitivt. Därför gå vidare, koppling hello tusentalsavgränsare för organisationer som redan använder Azure AD B2B för sina externt samarbete!

## <a name="next-steps"></a>Nästa steg

* Administratören upplevelser finns i hello [Azure-portalen](https://portal.azure.com).

* Information worker upplevelser är tillgängliga i hello [åtkomstpanelen](https://myapps.microsoft.com).

* Och som alltid ansluta med hello Produktteamet för feedback, diskussioner och förslag till våra [Microsoft teknisk Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).

Läs andra artiklar om Azure AD B2B-samarbete:

* [Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [hello-element i hello e-postinbjudan för B2B-samarbete](active-directory-b2b-invitation-email.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samarbete och licensiering](active-directory-b2b-licensing.md)
* [Felsökning av Azure Active Directory B2B-samarbete](active-directory-b2b-troubleshooting.md)
* [Vanliga och frågor svar om Azure Active Directory B2B-samarbete](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-samarbete API och anpassning](active-directory-b2b-api.md)
* [Multi-Factor Authentication för användare av B2B-samarbete](active-directory-b2b-mfa-instructions.md)
* [Lägg till B2B-samarbete användare utan inbjudan](active-directory-b2b-add-user-without-invite.md)
* [B2B-samarbete användaren granskning och rapportering](active-directory-b2b-auditing-and-reporting.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
