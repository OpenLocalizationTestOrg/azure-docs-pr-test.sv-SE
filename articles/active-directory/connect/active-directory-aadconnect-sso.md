---
title: "Azure AD Connect: Sömlös enkel inloggning | Microsoft Docs"
description: "Det här avsnittet beskriver Azure Active Directory (AD Azure) sömlös enkel inloggning och hur du kan ange SANT enkel inloggning för fjärrskrivbord företagsanvändare i företagsnätverket."
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
ms.openlocfilehash: 5a390208f4b7c22e96d7888bcbbd14d8b27667eb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sömlös enkel inloggning

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Vad är Azure Active Directory sömlös enkel inloggning?

Azure Active Directory sömlös enkel inloggning (Azure AD sömlös SSO) automatiskt loggar användarna när de är på sina enheter anslutna till företagsnätverket. När aktiverad behöver användare inte ange sina lösenord för att logga in till Azure AD och normalt även ange sina användarnamn. Den här funktionen ger dina användare enkel åtkomst till dina molnbaserade program utan att behöva ytterligare lokala komponenter.

Sömlös SSO kan kombineras med antingen den [synkronisering av lösenords-hash-](active-directory-aadconnectsync-implement-password-synchronization.md) eller [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md) inloggning metoder.

![Sömlös enkel inloggning](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>Sömlös SSO är för närvarande under förhandsgranskning. Den här funktionen är _inte_ tillämpliga till Active Directory Federation Services (ADFS).

## <a name="key-benefits"></a>Viktiga fördelar

- *Bättre användarupplevelse*
  - Användarna loggas automatiskt i både lokala och molnbaserade program.
  - Användarna behöver inte ange sina lösenord flera gånger.
- *Enkelt att distribuera och administrera*
  - Inga ytterligare komponenter behövs lokalt till att det ska fungera.
  - Fungerar med valfri metod för autentisering i molnet - [synkronisering av lösenords-hash-](active-directory-aadconnectsync-implement-password-synchronization.md) eller [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md).
  - Kan rullas till vissa eller alla användare med hjälp av en Grupprincip.
  - Registrera Windows 10-enheter med Azure AD utan behov av AD FS-infrastruktur. Den här funktionen måste du använda version 2.1 eller senare av den [till arbetsplatsen klienten](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Funktionen visar

- Logga in användarnamnet kan vara antingen lokalt Standardanvändarnamnet (`userPrincipalName`) eller ett annat attribut som konfigurerats i Azure AD Connect (`Alternate ID`). Båda använder fall arbete eftersom sömlös SSO använder den `securityIdentifier` anspråk i Kerberos-biljetten att leta upp motsvarande användarobjekt i Azure AD.
- Sömlös SSO är en opportunistisk funktion. Om det misslyckas av någon anledning går inloggning för användaren tillbaka till sitt vanliga beteende - dvs, användaren måste ange sina lösenord på sidan logga in.
- Om ett program vidarebefordrar en `domain_hint` (OpenID Connect) eller `whr` (SAML) parameter - identifierar din klient eller `login_hint` parameter - identifierar användaren i Azure AD-inloggning begäran användare loggas automatiskt in utan dem anger användarnamn eller lösenord.
- Den kan aktiveras via Azure AD Connect.
- Det är en kostnadsfri funktion och du behöver inte några betald utgåvor av Azure AD för att använda den.
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
>Vi nyligen återställde stöd för kant för att undersöka problem som rapporteras av kunden.

>[!NOTE]
>För Windows 10, rekommenderar vi att du använder [Azure AD Join](../active-directory-azureadjoin-overview.md) för den optimala enkel inloggning med Azure AD.

## <a name="next-steps"></a>Nästa steg

- [**Snabbstart** ](active-directory-aadconnect-sso-quick-start.md) – få och kör Azure AD sömlös SSO.
- [**Tekniska ingående** ](active-directory-aadconnect-sso-how-it-works.md) -förstå hur funktionen fungerar.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-sso-faq.md) -svar på vanliga frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig att lösa vanliga problem med funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
