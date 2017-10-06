---
title: "Konfigurera lösenord enkel inloggning för ett program för Azure AD-galleriet aaaProblem | Microsoft Docs"
description: "Förstå hello vanliga problem personer yta när du konfigurerar lösenord enkel inloggning för program som redan finns i hello Azure AD Application Gallery"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 78c37c52453c375bf7ccbca6df5c9008be4ce642
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Konfigurera lösenord enkel inloggning för ett program för Azure AD-galleriet problem

Den här artikeln hjälper dig toounderstand hello vanliga problem personer står inför när du konfigurerar **lösenord enkel inloggning** med ett program för Azure AD-galleriet.

## <a name="credentials-are-filled-in-but-hello-extension-does-not-submit-them"></a>Autentiseringsuppgifterna är ifylld, men hello-tillägget Skicka inte dem

Det händer vanligtvis om hello programvaruleverantören har ändrats loggar in sidan nyligen tooadd ett fält, ändra en underliggande identifierare som vi använde toodetect hello fälten användarnamn och lösenord eller ändra hur hello inloggning uppleva fungerar för sina program. Lyckligtvis i många fall kan kan Microsoft arbeta med program leverantörer toorapidly Lös problemen.

När Microsoft har tekniker tooautomatically identifiera när dessa integreringar Bryt, inte men ibland vi kan toofind problemen rätt passivt eller de ta lite tid toofix. Hello om när något av dessa integreringar inte fungerar korrekt, gärna vi om du har öppnat ett supportärende så att vi kan åtgärda det så snabbt som möjligt.

I tillägg toothis **om du inte har kontakt med programvaruleverantören,** **skicka dem till vår sätt** så att vi kan arbeta med dem. toonatively integrera sina program med Azure Active Directory. Du kan skicka hello leverantör toohello [visar en lista över dina program i hello Azure Active Directory-programgalleriet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget dem har startats.

## <a name="credentials-are-filled-in-and-submitted-but-hello-page-indicates-hello-credentials-are-incorrect"></a>Autentiseringsuppgifterna är ifylld och har skickats, men hello-sidan anger hello autentiseringsuppgifterna är felaktiga

tooresolve problemet kan första Kontrollera hello följande:

-   Hello användare först försöka för**inloggning toohello programmet webbplats direkt** med hello sparade autentiseringsuppgifter för dem.

  * Om det fungerar sedan har hello användare klickar du på hello **uppdatera autentiseringsuppgifterna** hello-knappen **programmet panelen** i hello **appar** avsnitt i hello [program Öppna Kontrollpanelen](https://myapps.microsoft.com/) tooupdate dem toohello senaste kända fungerar användarnamn och lösenord.

   * Om du eller en annan administratör tilldelad hello autentiseringsuppgifterna för denna användare att hitta hello användare eller grupp program tilldelningen genom att gå toohello **användare och grupper** fliken av programmet hello, välja hello tilldelning och Klicka på hello **referenser uppdatering** knappen.

-   Om användaren hello tilldelats sina egna autentiseringsuppgifter, har hello användare **Kontrollera toobe till att deras lösenord inte har gått ut i programmet hello** och i så fall **uppdatera sina utgångna lösenord** genom att logga in toohello programmet direkt.

   * När hello lösenord har uppdaterats i hello program kan begära hello användaren tooclick hello **uppdatera autentiseringsuppgifterna** hello-knappen **programmet panelen** i hello **appar** avsnitt i hello [programmet åtkomstpanelen](https://myapps.microsoft.com/) tooupdate dem toohello senaste kända fungerar användarnamn och lösenord.

   * Om du eller en annan administratör tilldelad hello autentiseringsuppgifterna för denna användare att hitta hello användare eller grupp program tilldelningen genom att gå toohello **användare och grupper** fliken av programmet hello, välja hello tilldelning och Klicka på hello **referenser uppdatering** knappen.

-   Har hello användaren update hello åtkomst panelen webbläsartillägget genom att följa hello stegen nedan i hello [hur tooinstall hello åtkomst panelen webbläsartillägget](#how-to-install-the-access-panel-browser-extension) avsnitt.

-   Se till att hello åtkomst panelen webbläsartillägget körs och aktiverat i användarens webbläsare.

-   Se till att användarna inte försöker toosign i toohello programmet hello åtkomst panelen när i **incognito InPrivate- eller privat läge**. tillägget för hello åtkomst panelen stöds inte i dessa lägen.

Om detta inte fungerar kan bero det på hello fall som en ändring inträffat på hello program sida som tillfälligt bruten hello programmet integrering med Azure AD. T.ex, kan detta inträffa när hello programvaruleverantören introducerar ett skript på webbsidan som fungerar annorlunda för manuell eller automatisk indata, som orsakar automated integration som våra egna, toobreak. Lyckligtvis i många fall kan kan Microsoft arbeta med program leverantörer toorapidly Lös problemen.

När Microsoft har tekniker tooautomatically identifiera när dessa integreringar Bryt, inte men ibland vi kan toofind problemen rätt passivt eller de ta lite tid toofix. Hello om när något av dessa integreringar inte fungerar korrekt, gärna vi om du har öppnat ett supportärende så att vi kan åtgärda det så snabbt som möjligt.

I tillägg toothis **om du inte har kontakt med programvaruleverantören,** **skicka dem till vår sätt** så att vi kan arbeta med dem. toonatively integrera sina program med Azure Active Directory. Du kan skicka hello leverantör toohello [visar en lista över dina program i hello Azure Active Directory-programgalleriet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget dem har startats.

## <a name="hello-extension-works-in-chrome-and-firefox-but-not-in-internet-explorer"></a>hello tillägg fungerar i Chrome och Firefox, men inte i Internet Explorer

Det finns två huvudsakliga orsaker toothis problemet:

-   Beroende på hello säkerhetsinställningar som är aktiverad i Internet Explorer om hello webbplatsen inte är en del av en **zonen Betrodda**, ibland våra skript blockeras från att köras för hello program.

  *  tooresolve kan instruera hello användaren för**Lägg till webbplatsen för hello program** toohello **tillförlitliga platser** listan inom sina **säkerhetsinställningar för Internet Explorer**. Du kan skicka dina användare toohello [hur tooadd en plats toomy betrodda platser listan](https://answers.microsoft.com/en-us/ie/forum/ie9-windows_7/how-do-i-add-a-site-to-my-trusted-sites-list/98cc77c8-b364-e011-8dfc-68b599b31bf5) artikel detaljerade anvisningar.

-   I sällsynta fall kan Säkerhetsvalidering för Internet Explorer ibland orsaka hello sidan tooload långsammare än hello våra skript körs.

   * Tyvärr kan kan den här situationen variera beroende på hello webbläsarens version, DATORHASTIGHET eller webbplats som besöks. I det här fallet föreslår vi att du kontaktar supporten kan vi lösa hello-integrering för den här specifika program.

I tillägg toothis **om du inte har kontakt med programvaruleverantören,** **skicka dem till vår sätt** så att vi kan arbeta med dem. toonatively integrera sina program med Azure Active Directory. Du kan skicka hello leverantör toohello [visar en lista över dina program i hello Azure Active Directory-programgalleriet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget dem har startats.

## <a name="check-if-hello-applications-login-page-has-changed-recently-or-requires-an-additional-field"></a>Kontrollera om hello programmets inloggningssida har ändrats nyligen eller kräver ett ytterligare fält

Om hello programmets inloggningssida har ändrats drastiskt leder ibland detta till vår integreringar toobreak. Ett exempel på detta är när en programvaruleverantören läggs tecknet i fältet, en captcha eller multifaktorautentisering tootheir inträffar. Lyckligtvis i många fall kan kan Microsoft arbeta med program leverantörer toorapidly Lös problemen.

När Microsoft har tekniker tooautomatically identifiera när dessa integreringar Bryt, inte men ibland vi kan toofind utfärdar dessa direkt. Annars vidta de vissa tid toofix. När något av dessa integreringar inte fungerar korrekt i hello fall skulle vi uppskattar öppna ett supportärende så att vi kan åtgärda det så snabbt som möjligt.

I tillägg toothis **om du inte har kontakt med programvaruleverantören,** **skicka dem till vår sätt** så att vi kan arbeta med dem. toonatively integrera sina program med Azure Active Directory. Du kan skicka hello leverantör toohello [visar en lista över dina program i hello Azure Active Directory-programgalleriet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget dem har startats.

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hur tooinstall hello åtkomst panelen webbläsartillägg

tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:

1.  Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.

2.  Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.

3.  I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.

4.  Baserat på din webbläsare vara du riktad toohello länken. **Lägg till** hello tillägget tooyour webbläsare.

5.  Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.

6.  När den har installerats, **starta om** webbläsarsessionen.

7.  Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program

Du kan också hämta hello-tillägget för Chrome och Firefox från hello Direktlänkar nedan:

-   [Tillägget för Chrome åtkomst panelen](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Tillägget för Firefox åtkomst panelen](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)

