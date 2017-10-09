---
title: 'Azure AD Connect: Direkt-autentisering | Microsoft Docs'
description: "Den här artikeln beskriver Azure Active Directory (AD Azure) direkt-autentisering och hur det kan Azure AD inloggningar genom att verifiera användarnas lösenord mot lokala Active Directory."
services: active-directory
keywords: "Vad är Azure AD Connect direkt autentisering, installera Active Directory, komponenter som krävs för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: 56cfb2c4f4afc7d46c926a392ae6ec01e96224d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Användaren logga in med Azure Active Directory direkt-autentisering

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Vad är Azure Active Directory direkt-autentisering?

Azure Active Directory (AD Azure) direkt-autentisering kan dina användare toosign i tooboth lokala och molnbaserade program med hjälp av hello samma lösenord. Den här funktionen ger användarna en bättre upplevelse - en mindre lösenord tooremember och minskar supportkostnaderna IT eftersom användarna är mindre troligt tooforget hur toosign i. När användare loggar in med Azure AD, verifierar den här funktionen användarnas lösenord direkt mot din lokala Active Directory.

Den här funktionen är ett alternativ för[Azure AD-lösenord hash-synkronisering](active-directory-aadconnectsync-implement-password-synchronization.md), vilket ger hello samma fördelen molnet autentisering tooorganizations. Dock tillåter principer för säkerhet och efterlevnad i vissa organisationer inte dessa organisationer toosend användarnas lösenord, även i en hashformaterats formuläret utanför deras interna gränser. Direkt-autentisering är hello rätta lösningen för dessa organisationer.

![Azure AD-direkt-autentisering](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Du kan kombinera direkt-autentisering med hello [sömlös enkel inloggning](active-directory-aadconnect-sso.md) funktion. Det innebär när användarna har åtkomst till program på sina företagets datorer i företagsnätverket, de behöver inte tootype i sina lösenord toosign i.

>[!IMPORTANT]
>Azure AD direkt-autentisering är för närvarande under förhandsgranskning.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Viktiga fördelar med att använda Azure AD direkt-autentisering

- *Bättre användarupplevelse*
  - Användare använder hello samma lösenord toosign i både lokala och molnbaserade program.
  - Användarna snabbare pratar toohello supportavdelning lösa problem med lösenord.
  - Användarna kan utföra [självbetjäning lösenordshantering](../active-directory-passwords-overview.md) uppgifter i hello molnet.
- *Enkelt toodeploy & administrera*
  - Inget behov av komplexa lokala distributioner eller nätverkskonfigurationen.
  - Behöver installeras bara lightweight agent toobe lokalt.
  - Inga hanteringskostnader. hello agenten får automatiskt förbättringar och felkorrigeringar.
- *Skydda*
  - Lokala lösenord lagras aldrig i hello moln i någon form.
  - hello agent blir bara utgående anslutningar från i nätverket. Det finns därför ingen krav tooinstall hello agent i ett perimeternätverk, också kallas en demilitariserad zon.
  - Skyddar dina användarkonton med fungerar sömlöst med [Azure AD villkorliga åtkomstprinciper](../active-directory-conditional-access-azure-portal.md), inklusive Multi-Factor Authentication (MFA) och av [filtrera lösenord nyckelsökningsangrepp](active-directory-aadconnect-pass-through-authentication-smart-lockout.md).
- *Hög tillgänglighet*
  - Ytterligare agenter kan installeras på flera lokala servrar tooprovide hög tillgänglighet för inloggningsförfrågningar.

## <a name="feature-highlights"></a>Funktionen visar

- Har stöd för användarinloggning i alla webbläsarbaserad webbprogram och i Microsoft Office-program som använder [modern autentisering](https://aka.ms/modernauthga).
- Logga in användarnamn kan vara antingen hello lokalt Standardanvändarnamnet (`userPrincipalName`) eller ett annat attribut som konfigurerats i Azure AD Connect (kallas även `Alternate ID`).
- hello-funktionen fungerar sömlöst med [villkorlig åtkomst](../active-directory-conditional-access.md) funktioner, till exempel Multi-Factor Authentication (MFA) toohelp skyddar dina användare.
- Miljöer med flera skogar stöds om det finns skogsförtroenden mellan AD-skogar och om routning är korrekt konfigurerad.
- Det är en kostnadsfri funktion och du inte behöver några betald utgåvor av Azure AD toouse den.
- Den kan aktiveras [Azure AD Connect](active-directory-aadconnect.md).
- Den använder en lightweight lokal agent som lyssnar efter och svarar toopassword validering begäranden.
- Installera flera agenter ger hög tillgänglighet för inloggningsförfrågningar.
- Den [skyddar](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) dina lokala konton mot brute force angrepp av lösenord i hello molnet.

## <a name="next-steps"></a>Nästa steg

- [**Snabbstart** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – komma igång och körs direkt i Azure AD-autentisering.
- [**Aktuella begränsningar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -funktionen är för närvarande under förhandsgranskning. Läs om vilka scenarier som stöds och vilka som inte är.
- [**Tekniska ingående** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -förstå hur funktionen fungerar.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-pass-through-authentication-faq.md) -toofrequently frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**Azure AD sömlös SSO** ](active-directory-aadconnect-sso.md) -mer information om den här funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
