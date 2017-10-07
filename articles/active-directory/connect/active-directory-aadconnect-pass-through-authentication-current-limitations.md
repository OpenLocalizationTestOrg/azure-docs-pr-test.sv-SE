---
title: "Azure AD Connect: Direkt autentisering - aktuella begränsningar | Microsoft Docs"
description: "Den här artikeln beskriver hello aktuella begränsningar i Azure Active Directory (AD Azure) direkt-autentisering."
services: active-directory
keywords: "Azure AD Connect direkt-autentisering, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: billmath
ms.openlocfilehash: 2933745d071aae205c44659e6ea92697f390effb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory direkt-autentisering: Aktuella begränsningar

>[!IMPORTANT]
>Azure AD direkt-autentisering är för närvarande under förhandsgranskning. Det är en kostnadsfri funktion och du inte behöver några betald utgåvor av Azure AD toouse den. Direkt-autentisering är endast tillgängligt i hello world wide instans av Azure AD och inte på [Microsoft Cloud Tyskland](http://www.microsoft.de/cloud-deutschland) och [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Scenarier som stöds

hello följande scenarier stöds fullt ut under förhandsgranskningen:

- Användarinloggningar i alla webbläsarbaserad webbprogram.
- Användarinloggningar i Office 365-klientprogram som stöder [modern autentisering](https://aka.ms/modernauthga).
- Azure AD-anslutning för Windows 10-enheter.
- Stöd för Exchange ActiveSync.

## <a name="unsupported-scenarios"></a>Scenarier som inte stöds

hello följande scenarier är _inte_ stöds under förhandsgranskningen:

- Användarinloggningar till äldre Office-program (Office 2013 eller tidigare). Organisationer är rekommenderar tooswitch toomodern autentisering, om möjligt. Modern autentisering tillåter stöd för direkt-autentisering, men också kan du skydda din användare användarkonton med [villkorlig åtkomst](../active-directory-conditional-access.md) funktioner, till exempel Multi-Factor Authentication (MFA).
- Användarinloggningar till Skype för företag-klientprogram, inklusive Skype för företag 2016.
- Användarinloggningar i PowerShell v1.0. Det rekommenderas att du använder PowerShell version 2.0 i stället.

>[!IMPORTANT]
>Aktivera synkronisering av lösenords-Hash som en lösning för scenarier som inte stöds på hello [valfria funktioner](active-directory-aadconnect-get-started-custom.md#optional-features) sida i hello Azure AD Connect-guiden. Hash-synkronisering av lösenord fungerar som en reserv för hello föregående scenarier _endast_ (och _inte_ som en allmän tooPass via Reservautentisering). Om du inte behöver dessa scenarier kan du inaktivera synkronisering av lösenords-Hash.

## <a name="next-steps"></a>Nästa steg
- [**Snabbstart** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – komma igång och körs direkt i Azure AD-autentisering.
- [**Tekniska ingående** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -förstå hur funktionen fungerar.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-pass-through-authentication-faq.md) -toofrequently frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**Azure AD sömlös SSO** ](active-directory-aadconnect-sso.md) -mer information om den här funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
