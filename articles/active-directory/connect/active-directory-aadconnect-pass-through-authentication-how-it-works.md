---
title: "Azure AD Connect: Direkt-autentisering - så här fungerar det? | Microsoft Docs"
description: "Den här artikeln beskriver hur Azure Active Directory direkt-autentisering fungerar."
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
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: ffcebee572a9ba2840e81250651dea45599d65d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure Active Directory direkt-autentisering: Tekniska ingående

>[!IMPORTANT]
>Azure AD direkt-autentisering är för närvarande under förhandsgranskning. 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Hur fungerar Azure Active Directory direkt-autentisering?

När en användare försöker toosign i ett program som skyddas av Azure Active Directory (Azure AD) och om direkt-autentisering är aktiverat på hello-klient hello utförs följande steg:

1. hello användare försöker tooaccess ett program (till exempel hello Outlook Web App - https://outlook.office365.com/owa/).
2. Om hello användaren inte redan har loggat in är hello användaren omdirigerade toohello Azure AD-inloggningssida.
3. hello användaren anger sitt användarnamn och lösenord i hello Azure AD-inloggningssida och klickar på hello ”logga in” knappen.
4. Azure AD på hello inloggning begäran tas emot placerar hello användarnamn och lösenord (krypterade med en offentlig nyckel) i en kö.
5. En lokal direkt autentiseringsagent gör en utgående samtal toohello kö och hämtar hello användarnamn och lösenord för krypterade.
6. hello agent dekrypterar hello lösenord med hjälp av den privata nyckeln.
7. hello agent verifierar sedan hello användarnamn och lösenord mot Active Directory använder standard Windows API: er (en liknande mekanism toowhat används av Active Directory Federation Services). hello användarnamnet kan vara antingen hello lokalt Standardanvändarnamnet (vanligtvis `userPrincipalName`) eller ett annat attribut som konfigurerats i Azure AD Connect (kallas även `Alternate ID`).
8. hello lokala Active Directory domänkontrollanten (DC) sedan utvärderar hello begäran och returnerar hello lämplig respons (lyckad, misslyckad, lösenord upphört att gälla eller användaren utelåst) toohello agent.
9. hello agent returnerar det här svaret tillbaka tooAzure AD.
10. Azure AD utvärderar hello svar och svarar toohello användaren efter behov – exempelvis den loggar hello användaren omedelbart eller begär för multi-Factor Authentication (MFA).
11. Om hello användarens inloggning lyckas är hello användaren kan tooaccess hello program.

hello följande diagram illustrerar alla hello komponenter och hello stegen.

![Direktautentisering](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Nästa steg
- [**Aktuella begränsningar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -funktionen är för närvarande under förhandsgranskning. Läs om vilka scenarier som stöds och vilka som inte är.
- [**Snabbstart** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – komma igång och körs direkt i Azure AD-autentisering.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-pass-through-authentication-faq.md) -toofrequently frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**Azure AD sömlös SSO** ](active-directory-aadconnect-sso.md) -mer information om den här funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
