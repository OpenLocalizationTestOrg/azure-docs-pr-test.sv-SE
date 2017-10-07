---
title: "Azure AD Connect: Sömlös Single Sign-On - hur det fungerar | Microsoft Docs"
description: "Den här artikeln beskriver hur hello Azure Active Directory sömlös enkel inloggning funktionen fungerar."
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
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: 17ce35b32832d241068ab878cf7aac42deab74ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory sömlös enkel inloggning: tekniska ingående

Den här artikeln får du teknisk information till hur hello Azure Active Directory sömlös enkel inloggning (SSO sömlös) fungerar.

>[!IMPORTANT]
>hello sömlös SSO-funktionen är för närvarande under förhandsgranskning.

## <a name="how-does-seamless-sso-work"></a>Hur fungerar sömlös SSO?

Det här avsnittet har två delar tooit:
1. hello inställning av hello sömlös SSO-funktionen.
2. Hur fungerar en enskild användare logga in transaktion med sömlös SSO.

### <a name="how-does-set-up-work"></a>Hur ställa in arbete?

Sömlös SSO aktiveras med Azure AD Connect enligt [här](active-directory-aadconnect-sso-quick-start.md). Att aktivera funktionen hello inträffa hello följande:
- Ett konto med namnet `AZUREADSSOACCT` (som motsvarar Azure AD) skapas i din lokala Active Directory (AD).
- hello datorkontos Kerberos dekrypteringsnyckeln delas på ett säkert sätt med Azure AD.
- Dessutom skapas två Kerberos tjänstens huvudnamn (SPN) toorepresent två URL: er som används under Azure AD-inloggning.

>[!NOTE]
> hello datorkonto och Kerberos SPN du hello skapas i varje AD-skog du synkronisera tooAzure AD (med Azure AD Connect) och vars användare du vill ha sömlös SSO. Flytta hello `AZUREADSSOACCT` datorn konto tooan organisationsenhet (OU) där andra datorkonton är lagrade tooensure som hanteras i hello samma sätt och tas inte bort.

>[!IMPORTANT]
>Hög rekommenderar vi att du [rulla över hello Kerberos dekrypteringsnyckeln](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) av hello `AZUREADSSOACCT` datorkonto minst var 30 dagar.

### <a name="how-does-sign-in-with-seamless-sso-work"></a>Hur logga in med sömlös SSO arbete?

När hello installation är klar, fungerar sömlös SSO hello samma sätt som andra inloggning som använder integrerad Windows Windowsautentisering (IWA). hello flödet är följande:

1. hello användare försöker tooaccess ett program (till exempel hello Outlook Web App - https://outlook.office365.com/owa/) från en domänansluten företagets enheter i företagsnätverket.
2. Om hello användaren inte redan har loggat in är hello användaren omdirigerade toohello Azure AD-inloggningssida.

  >[!NOTE]
  >Om hello Azure AD-inloggning begäran innehåller en `domain_hint` (identifiera klient - till exempel, contoso.onmicrosoft.com) eller `login_hint` (identifiera hello användare – till exempel user@contoso.onmicrosoft.com eller user@contoso.com) parametern sedan steg 2 hoppas över.

3. hello användaren skriver sitt användarnamn i hello Azure AD-inloggningssida.
4. Azure AD anropar med hjälp av JavaScript i bakgrunden hello hello webbläsare via ett 401 obehörig svar, tooprovide en Kerberos-biljett.
5. hello webbläsare i sin tur begär en tjänstbiljett från Active Directory för hello `AZUREADSSOACCT` datorkontot (som representerar Azure AD).
6. Active Directory hittar hello datorkontot och returnerar en Kerberos-biljett toohello webbläsare krypteras med hello datorkontos hemlighet.
7. hello webbläsare vidarebefordrar hello Kerberos-biljetten som hämtades från Active Directory tooAzure AD (på en av hello [Azure AD-URL: er som tidigare lagts till toohello webbläsarens intranät Zoninställningar](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).
8. Azure AD dekrypterar hello Kerberos-biljett, som innehåller hello identitet hello användaren loggat in hello företagets enheter med hjälp av hello tidigare delad nyckel.
9. Efter utvärderingen, Azure AD returnerar ett token tillbaka toohello program eller begär hello användaren tooperform ytterligare bevis, till exempel Multi-Factor Authentication.
10. Om hello användarens inloggning lyckas är hello användaren kan tooaccess hello program.

hello följande diagram illustrerar alla hello komponenter och hello stegen.

![Sömlös för enkel inloggning](./media/active-directory-aadconnect-sso/sso2.png)

Sömlös SSO är opportunistisk, vilket innebär att om det misslyckas hello inloggningen faller tillbaka tooits reguljära beteende - engångsfaktorautentisering, hello användare behöver tooenter sina lösenord toosign i.

## <a name="next-steps"></a>Nästa steg

- [**Snabbstart** ](active-directory-aadconnect-sso-quick-start.md) – få och kör Azure AD sömlös SSO.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-sso-faq.md) -toofrequently frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
