---
title: "Azure AD Connect: Sömlös enkel inloggning | Microsoft Docs"
description: "Det här avsnittet beskriver Azure Active Directory (AD Azure) sömlös enkel inloggning och hur kan du tooprovide SANT enkel inloggning för företagets stationära användare i företagsnätverket."
services: active-directory
keywords: "Vad är Azure AD Connect, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 11c778c37ee39866dcc3a14ab648cfcd8e322e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sömlös enkel inloggning

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Vad är Azure Active Directory sömlös enkel inloggning?

Azure Active Directory sömlös enkel inloggning (Azure AD sömlös SSO) automatiskt loggar användarna när de är på företagets enheter anslutna tooyour företagets nätverk. När aktiverat kan användarna inte behöver tootype i sina lösenord toosign i tooAzure AD och normalt även ange sina användarnamn. Den här funktionen ger dina användare enkel åtkomst tooyour molnbaserade program utan att behöva ytterligare lokala komponenter.

Sömlös SSO kan kombineras med antingen hello [synkronisering av lösenords-hash-](active-directory-aadconnectsync-implement-password-synchronization.md) eller [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md) inloggning metoder.

![Sömlös enkel inloggning](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>Sömlös SSO är för närvarande under förhandsgranskning. Den här funktionen är _inte_ tillämpliga tooActive Directory Federation Services (ADFS).

## <a name="key-benefits"></a>Viktiga fördelar

- *Bättre användarupplevelse*
  - Användarna loggas automatiskt i både lokala och molnbaserade program.
  - Användarna har inte tooenter sina lösenord flera gånger.
- *Enkelt toodeploy & administrera*
  - Inga ytterligare komponenter behövs lokalt toomake detta arbete.
  - Fungerar med valfri metod för autentisering i molnet - [synkronisering av lösenords-hash-](active-directory-aadconnectsync-implement-password-synchronization.md) eller [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md).
  - Kan rullas toosome eller alla användare med hjälp av en Grupprincip.
  - Registrera Windows 10-enheter med Azure AD utan hello krävs för AD FS-infrastruktur. Den här funktionen kräver att du toouse version 2.1 eller senare av hello [till arbetsplatsen klienten](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Funktionen visar

- Logga in användarnamnet kan vara antingen hello lokalt Standardanvändarnamnet (`userPrincipalName`) eller ett annat attribut som konfigurerats i Azure AD Connect (`Alternate ID`). Båda använder fall arbete eftersom sömlös SSO använder hello `securityIdentifier` anspråk i hello Kerberos-biljetten toolook in hello motsvarande användarobjekt i Azure AD.
- Sömlös SSO är en opportunistisk funktion. Om det misslyckas av någon anledning hello inloggning användarupplevelse återgår tooits reguljära beteende - engångsfaktorautentisering, hello användare behöver tooenter sitt lösenord på hello-inloggningssida.
- Om ett program vidarebefordrar en `domain_hint` (OpenID Connect) eller `whr` (SAML) parameter - identifierar din klient eller `login_hint` parameter - identifierar hello användare i Azure AD-inloggning begäran användare loggas automatiskt in utan dem. Ange användarnamn eller lösenord.
- Den kan aktiveras via Azure AD Connect.
- Det är en kostnadsfri funktion och du inte behöver några betald utgåvor av Azure AD toouse den.
- Den stöds på web webbläsarbaserad och Office-klienter som stöder [modern autentisering](https://aka.ms/modernauthga) på plattformar och webbläsare som stöder Kerberos-autentisering:

| OS\Browser |Internet Explorer|Kant|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Ja|Nej|Ja|Ja\*|Saknas
|Windows 8.1|Ja|Saknas|Ja|Ja\*|Saknas
|Windows 8|Ja|Saknas|Ja|Ja\*|Saknas
|Windows 7|Ja|Saknas|Ja|Ja\*|Saknas
|Mac OS X|Saknas|Saknas|Ja\*|Ja\*|Ja\*

\*Kräver [ytterligare konfiguration](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

>[!IMPORTANT]
>Vi nyligen återställde stöd för Edge tooinvestigate kunden rapporterade problem.

>[!NOTE]
>För Windows 10 hello rekommendation är toouse [Azure AD Join](../active-directory-azureadjoin-overview.md) för hello optimala enkel inloggning med Azure AD.

## <a name="next-steps"></a>Nästa steg

- [**Snabbstart** ](active-directory-aadconnect-sso-quick-start.md) – få och kör Azure AD sömlös SSO.
- [**Tekniska ingående** ](active-directory-aadconnect-sso-how-it-works.md) -förstå hur funktionen fungerar.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-sso-faq.md) -toofrequently frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
