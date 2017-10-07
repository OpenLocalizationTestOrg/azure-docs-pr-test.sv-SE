---
title: "aaaProblem inloggning toohello åtkomst panelen webbplats | Microsoft Docs"
description: "Vägledning tootroubleshoot problem som kan uppstå när toosign i toouse hello åtkomstpanelen"
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
ms.reviwer: japere
ms.openlocfilehash: 1037f7c5fbaa9425760ad5739b383c716d5fc3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-signing-in-toohello-access-panel-website"></a>Problem med att logga i toohello åtkomst panelen webbplats

Hej åtkomstpanelen är en webbaserad portal som aktiverar en användare som har ett arbets- eller skolkonto konto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program att hello Azure AD-administratör har gett dem åtkomst till. En användare med Azure AD-versioner kan också använda självbetjäning och funktioner för hantering av appen via hello åtkomstpanelen. Hej åtkomstpanelen skiljer sig från hello Azure-portalen och kräver inte användare toohave en Azure-prenumeration.

Användarna kan logga in toohello åtkomstpanelen om de har ett arbets- eller skolkonto konto i Azure AD.

-   Användare kan autentiseras av Azure AD direkt.

-   Användare kan autentiseras med hjälp av Active Directory Federation Services (AD FS).

-   Användare kan autentiseras av Windows Server Active Directory.

Om en användare har en prenumeration på Azure eller Office 365 och har använt hello Azure-portalen eller ett Office 365-program, kommer de att kunna toouse hello åtkomstpanelen sömlöst utan att behöva toosign i igen. Användare som inte autentiseras att ange toosign i med hjälp av hello användarnamn och lösenord för sitt konto i Azure AD. Om hello organisationen har konfigurerat federation, räcker att skriva hello användarnamn.

## <a name="general-issues-toocheck-first"></a>Allmänna problem toocheck först 

-   Kontrollera att hello användaren loggar in toohello **korrigera URL**: <https://myapps.microsoft.com>

-   Kontrollera att hello användarens webbläsare har lagt till hello URL tooits **tillförlitliga platser**

-   Se till att hello användarkonto **aktiverat** för inloggningar.

-   Kontrollera att hello användarens konto är **inte låst.**

-   Se till att hello användarens **lösenord inte har upphört att gälla eller har glömt.**

-   Kontrollera att **Multifaktorautentisering** inte blockerar åtkomst.

-   Kontrollera att en **principen för villkorlig åtkomst** eller **identitetsskydd** principen inte blockerar åtkomst.

-   Se till att en användares **autentisering kontaktinformation** fungerar toodate tooallow Multifaktorautentisering eller villkorlig åtkomst principer toobe tillämpas.

-   Se till att tooalso försök rensar cookies i webbläsaren och försök toosign i igen.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>Möte Webbläsarkrav för hello åtkomstpanelen

Hej åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat CSS. toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare. Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.

För lösenordsbaserad SSO kan hello användarens webbläsare vara:

-   Internet Explorer 8, 9, 10, 11--på Windows 7 eller senare

-   Kanten på Windows 10 årsdagar Edition eller senare 

-   Chrome--På Windows 7 eller senare, och i MacOS X eller senare

-   Firefox 26.0 eller senare--på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare


## <a name="problems-with-hello-users-account"></a>Problem med hello användarkonto

Åtkomst toohello åtkomstpanelen blockeras på grund av tooa problem med hello användarkonto. Här följer några metoder som du kan felsöka och lösa problem med användare och deras kontoinställningar:

-   [Kontrollera om det finns ett konto i Azure Active Directory](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Kontrollera status för en användare](#check-a-users-account-status)

-   [Återställa en användares lösenord](#reset-a-users-password)

-   [Aktivera lösenordsåterställning via självbetjäning](#enable-self-service-password-reset)

-   [Kontrollera status för multifaktorautentisering för en användare](#check-a-users-multi-factor-authentication-status)

-   [Kontrollera en användares kontaktinformation för autentisering](#check-a-users-authentication-contact-info)

-   [Kontrollera en användares gruppmedlemskap](#check-a-users-group-memberships)

-   [Kontrollera en användares tilldelade licenser](#check-a-users-assigned-licenses)

-   [Tilldela en användare en licens](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Kontrollera om det finns ett konto i Azure Active Directory

toocheck om användarkontot finns, så hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Kontrollera hello egenskaper för hello användaren objektet toobe till att de ser ut som förväntat och inga data saknas.

### <a name="check-a-users-account-status"></a>Kontrollera status för en användare

toocheck en användare konto status, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **profil**.

8.  Under **inställningar** se till att **Block logga in** har angetts för**nr**.

### <a name="reset-a-users-password"></a>Återställa en användares lösenord

tooreset en användares lösenord, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på hello **Återställ lösenord** knappen hello överst på bladet för hello användare.

8.  Klicka på hello **Återställ lösenord** hello-knappen **Återställ lösenord** bladet som visas.

9.  Kopiera hello **tillfälligt lösenord** eller **ange ett nytt lösenord** för hello användare.

10. Meddela den nya lösenord toohello användaren, de nödvändiga toochange lösenordet vid nästa inloggning tooAzure Active Directory.

### <a name="enable-self-service-password-reset"></a>Aktivera lösenordsåterställning via självbetjäning

tooenable självbetjäning lösenord återställa, hello distribution av följande sätt:

-   [Aktivera användare tooreset sina Azure Active Directory-lösenord](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [Aktivera användare tooreset eller ändra sina lokala Active Directory-lösenord](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a>Kontrollera status för multifaktorautentisering för en användare

toocheck en användare har Multi-Factor authentication status, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  Klicka på hello **Multifaktorautentisering** knappen hello överst i hello-bladet.

7.  En gång hello **Multi-Factor Authentication-administrationsportalen** belastning, kontrollera att du är på hello **användare** fliken.

8.  Hitta hello användaren i hello lista över användare genom att söka, filtrering och sortering.

9.  Välj hello användaren hello listan över användare och **aktivera**, **inaktivera**, eller **framtvinga** multifaktorautentisering efter behov.

   >[!NOTE]
   >Om en användare finns i en **tvingande** tillstånd, så kan du ange dem för**inaktiverad** tillfälligt toolet dem tillbaka till deras konto. När de är tillbaka i sedan kan du ändra tillståndet för**aktiverad** igen toorequire dem toore-register kontaktuppgifter under nästa inloggning. Du kan också följa hello stegen i hello [Kontrollera en användares autentisering kontaktinformation](#check-a-users-authentication-contact-info) tooverify eller ange informationen för dessa.
   >
   >

### <a name="check-a-users-authentication-contact-info"></a>Kontrollera en användares kontaktinformation för autentisering

toocheck en användarautentisering kontaktuppgifter som används för multifaktorautentisering, villkorlig åtkomst, identitetsskydd och återställning av lösenord gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **profil**.

8.  Rulla nedåt för**autentisering kontaktinformation**.

9.  **Granska** hello data som registrerats för hello användaren och uppdatera efter behov.

### <a name="check-a-users-group-memberships"></a>Kontrollera en användares gruppmedlemskap

toocheck en användare gruppmedlemskap, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **grupper** toosee vilka grupper hello användaren är medlem i.

### <a name="check-a-users-assigned-licenses"></a>Kontrollera en användares tilldelade licenser

toocheck en användare tilldelade licenser, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.

### <a name="assign-a-user-a-license"></a>Tilldela en användare en licens 

tooassign en licens tooa användare gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.

8.  Klicka på hello **tilldela** knappen.

9.  Välj **en eller flera produkter** hello listan över tillgängliga produkter.

10. **Valfria** klickar du på hello **tilldelning alternativ** objektet toogranularly tilldela produkter. Klicka på **Ok** när detta har slutförts.

11. Klicka på hello **tilldela** knappen tooassign dessa licenser toothis användare.

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a>Om felsökningen inte löser problemet hello

Öppna ett supportärende med hello efter information om tillgängliga:

-   Fel-ID för korrelation

-   UPN (användarens e-postadress)

-   Klient-ID:t

-   Typ av webbläsare

-   Tidszon och tid/tidsperioden under fel inträffar

-   Fiddler spårningar

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
