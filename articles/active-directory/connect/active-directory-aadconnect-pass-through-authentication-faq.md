---
title: "Azure AD Connect: Direkt autentisering – vanliga frågor och svar | Microsoft Docs"
description: "Svaren toofrequently frågor och svar om Azure Active Directory direkt-autentisering."
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
ms.openlocfilehash: 550e2599177682f8ea971a05485dd5ac12b9b072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory direkt-autentisering: Vanliga frågor och svar

I den här artikeln adressera vi vanliga frågor och svar om Azure Active Directory (AD Azure) direkt-autentisering. Kontrollera för nytt innehåll.

>[!IMPORTANT]
>hello är direkt-autentisering för närvarande i förhandsversion.

## <a name="which-of-hello-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose"></a>Vilket av hello Azure AD-inloggning metoder - direkt-autentisering, lösenord hash-synkronisering och Active Directory Federation Services (AD FS) - ska jag välja?

Det beror på din lokala miljö och organisatoriska krav. Granska den här artikeln för en [jämförelse av hello olika Azure AD-inloggning metoder](active-directory-aadconnect-user-signin.md).

## <a name="is-pass-through-authentication-a-free-feature"></a>Är direkt autentisering en kostnadsfri funktion?

Direkt-autentisering är en kostnadsfri funktion och du inte behöver några betald utgåvor av Azure AD toouse den. Den återstående ledigt när hello funktionen når allmän tillgänglighet.

## <a name="is-pass-through-authentication-available-in-microsoft-cloud-germanyhttpwwwmicrosoftdecloud-deutschland-and-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Är direkt-autentisering finns i [Microsoft Cloud Tyskland](http://www.microsoft.de/cloud-deutschland) och [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/)?

Nej, direkt autentisering är endast tillgängligt i hello world wide instans av Azure AD.

## <a name="does-conditional-accessactive-directory-conditional-accessmd-work-with-pass-through-authentication"></a>Har [villkorlig åtkomst](../active-directory-conditional-access.md) fungerar med direkt autentisering?

Ja, alla funktioner för villkorlig åtkomst, inklusive Azure Multi-Factor Authentication fungerar med direkt-autentisering.

## <a name="does-pass-through-authentication-support-alternate-id-as-hello-username-instead-of-userprincipalname"></a>Stöder direkt autentisering ”Alternativt ID” som hello användarnamn i stället för ”userPrincipalName”?

Ja. Direkt-autentiseringen stöder `Alternate ID` som hello användarnamn konfigurerades i Azure AD Connect enligt [här](active-directory-aadconnect-get-started-custom.md). Inte alla Office 365-program stöder `Alternate ID`. Referera toohello specifika program dokumentationen för hello support-instruktionen.

## <a name="does-password-hash-synchronization-act-as-a-fallback-toopass-through-authentication"></a>Synkronisering av lösenords-Hash fungera som tooPass via Reservautentisering?

Nej, hash-synkronisering av lösenord är inte en generisk tooPass via Reservautentisering. Det fungerar endast som en reserv för [scenarier som inte stöder direkt autentiseringen idag](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). tooavoid användare logga in fel, bör du konfigurera direkt autentisering för [hög tillgänglighet](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-hello-same-server-as-a-pass-through-authentication-agent"></a>Kan jag installera en [Azure AD Application Proxy](../active-directory-application-proxy-get-started.md) connector på hello samma server som en direkt autentiseringsagent?

Ja, den här konfigurationen stöds med hello byta namn på versioner av hello direkt autentiseringsagent (versioner 1.5.193.0 eller senare).

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Vilka versioner av Azure AD Connect och direkt autentiseringsagent behöver du?

Du behöver version 1.1.486.0 eller senare för Azure AD Connect och 1.5.58.0 eller senare för hello direkt autentiseringsagent för den här funktionen toowork. All programvara ska installeras på servrar med Windows Server 2012 R2 eller senare.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-toosign-in-using-pass-through-authentication"></a>Vad händer om min användarens lösenord har upphört att gälla och de försök toosign i med hjälp av direkt autentisering?

Om du har konfigurerat [tillbakaskrivning av lösenord](../active-directory-passwords-update-your-own-password.md) för en viss användare, och om hello användaren loggar in med hjälp av direkt-autentisering, de kan ändra eller återställa sina lösenord. hello lösenord skrivs tillbaka tooon lokala Active Directory som förväntat.

Men om tillbakaskrivning av lösenord inte är konfigurerad för en viss användare eller om hello användaren har inte en giltig Azure AD-licens, hello användaren går inte att uppdatera sitt lösenord i hello molnet. De kan inte uppdatera sina lösenord, även om deras lösenord har upphört att gälla. hello användaren i stället ser meddelandet ”: din organisation tillåter inte att du tooupdate lösenord på den här platsen. Uppdatera den enligt toohello metod som din organisation, eller be administratören om du behöver hjälp ”. hello användare eller Hej administratör har du tooreset sina lösenord i din lokala Active Directory.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Hur direkt autentisering skyddar du mot brute force-attacker lösenord?

Läs [i den här artikeln](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) för mer information.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Vad direkt autentisering agenter kommunicerar via portarna 80 och 443?

- hello autentisering agenter begär HTTPS via port 443 för alla åtgärder för funktionen.
- hello autentisering agenter gör HTTP-förfrågningar via port 80 toodownload SSL listor över återkallade certifikat.

     >[!NOTE]
     >I de senaste uppdateringarna minskas vi hello antalet portar som krävs av hello-funktionen. Om du har äldre versioner av Azure AD Connect eller hello autentiseringsagent att dessa portar hålls också öppna: 5671, 8080, 9090, 9091, 9350, 9352 och 10100 10120.

## <a name="can-hello-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Hello direkt autentisering agenter kan kommunicera via en utgående webbproxyserver?

Ja. Om WPAD (Web Proxy Auto-Discovery) är aktiverad i din lokala miljö, autentisering agenter automatiskt toolocate och använder en webbproxyserver hello nätverket.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-hello-same-server"></a>Kan jag installera två eller fler direkt autentisering agenter på hello samma server?

Nej, kan du endast installera en Agent för direkt-autentisering på en enskild server. Om du vill tooconfigure direkt autentisering för hög tillgänglighet, följer du anvisningarna för hello i detta [artikel](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) i stället.

## <a name="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-toopass-through-authentication"></a>Jag har redan använder Active Directory Federation Services (AD FS) för Azure AD-inloggning. Hur jag växlar den tooPass via autentisering?

Om du konfigurerat AD FS som din inloggningsmetod som använder hello Azure AD Connect-guiden, ändra hello användaren inloggningsmetod tooPass genom autentisering. Den här ändringen kan direkt autentisering hello innehavaren och konverterar _alla_ externa domäner i hanterade domäner. Alla efterföljande inloggning förfrågningar i din klient hanteras av direkt-autentisering. För närvarande, går det inte stöds i Azure AD Connect toouse en kombination av AD FS och direkt-autentisering mellan domäner.

Om AD FS har konfigurerats som hello inloggningsmetod _utanför_ av hello Azure AD Connect-guiden Ändra hello användaren inloggningsmetod tooPass via serverautentisering (alternativ för hello ”konfigurerar inte”). Den här ändringen gör direkt autentisering hello innehavaren. Men alla externa domäner fortsätta toouse AD FS för inloggning. Använd PowerShell toomanually konvertera vissa eller alla dessa externa domäner tooManaged domäner. Efter det att alla inloggning-förfrågningar på hanterade domäner (_endast_) som hanteras av direkt-autentisering.

>[!IMPORTANT]
>Direkt-autentisering kan inte hantera inloggning för endast molnbaserad Azure AD-användare.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment"></a>Kan jag använda direkt autentisering i en miljö med flera skogar AD?

Ja. Miljöer med flera skogar stöds om det finns skogsförtroenden mellan AD-skogar och om routning är korrekt konfigurerad.

## <a name="do-pass-through-authentication-agents-provide-load-balancing-capability"></a>Tillhandahåller direkt autentisering agenter belastningsutjämning kapaciteten?

Nej, installera flera direkt autentisering agenter garanterar [hög tillgänglighet](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability), men inte belastningsutjämning. En eller två hello autentisering agenter kan det sluta hantera hello bulk hello inloggning begäranden.

Lösenord validering begäranden som hello autentisering agenter måste toohandle är förenklad. Därför hanteras belastning och genomsnittlig belastning för de flesta kunder enkelt kan av två eller tre autentisering agenter totalt.

Vi rekommenderar att du installerar agenter autentisering Stäng tooyour domänkontrollanter tooimprove inloggning svarstid.

## <a name="can-i-install-hello-first-pass-through-authentication-agent-on-a-server-other-than-hello-one-that-runs-azure-ad-connect"></a>Kan jag installera hello första direkt autentiseringsagent på en annan server än hello en som kör Azure AD Connect?

Nej, det här scenariot är _inte_ stöds.

## <a name="how-many-pass-through-authentication-agents-should-i-install"></a>Hur många agenter för direkt-autentisering ska installera?

Vi rekommenderar att:

- Du kan installera två eller tre autentisering agenter totalt. Den här konfigurationen är tillräcklig för de flesta kunder.
- Du installerar agenter för autentisering på dina domänkontrollanter (eller Stäng toothem som möjligt) tooimprove inloggning svarstid.
- Du se till att servrar (där autentisering agenter är installerade) läggs toohello samma AD-skog som hello användare vars lösenord måste toobe verifieras.

>[!NOTE]
>Det finns en systembegränsning 12 autentisering agenter per klient.

## <a name="how-can-i-disable-pass-through-authentication"></a>Hur kan jag inaktivera direkt autentisering?

Kör hello Azure AD Connect-guiden och ändra hello användaren inloggningsmetod från direkt tooanother autentiseringsmetod. Den här ändringen inaktiverar direkt autentiseringen hello innehavaren och avinstallerar hello autentiseringsagent från hello-server. Du har toomanually avinstallera hello autentisering agenter från andra servrar.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Vad händer när jag för att avinstallera en Agent för direkt-autentisering?

Avinstallera en Agent för autentisering av direkt från en server gör det toostop accepterar inloggningsförfrågningar. Se till att du har en annan autentisering-agenten som körs innan du utför den här åtgärden tooavoid bryter användare logga in på din klient.

## <a name="next-steps"></a>Nästa steg
- [**Aktuella begränsningar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -funktionen är för närvarande under förhandsgranskning. Läs om vilka scenarier som stöds och vilka som inte är.
- [**Snabbstart** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – komma igång och körs direkt i Azure AD-autentisering.
- [**Tekniska ingående** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -förstå hur funktionen fungerar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**Azure AD sömlös SSO** ](active-directory-aadconnect-sso.md) -mer information om den här funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
