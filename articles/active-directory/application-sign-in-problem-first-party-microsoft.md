---
title: aaaProblems inloggning tooa Microsoft-program | Microsoft Docs
description: "Felsöka vanliga problem med står inför när du loggar in toofirst parts Microsoft Applications med Azure AD (till exempel Office 365)"
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
ms.openlocfilehash: 35849ca8dbaa909d17b6d0da572f5c11041a8559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="problems-signing-in-tooa-microsoft-application"></a>Problem med att logga i tooa Microsoft-program

Microsoft Applications (till exempel Office 365 Exchange, SharePoint, Yammer, etc.) tilldelas och hanteras annorlunda än 3 part SaaS-program eller andra program som du har integrerat med Azure AD för enkel inloggning på.

Det finns tre sätt som en användare kan få åtkomst tooa Microsoft publicerade program.

-   För program i hello Office 365 eller andra betald paket användare som beviljas åtkomst via **licens tilldelning** antingen direkt tootheir användarkonto, eller via en grupp med vår gruppbaserade licens tilldelning-funktionen.

-   För program som Microsoft eller tredje part publicerar fritt för alla toouse användare beviljas åtkomst via **användargodkännande**. This0 innebär att de loggar in toohello program med Azure AD arbets- eller Skol-konto och Tillåt toohave åtkomst toosome begränsad uppsättning data på sitt konto.

-   För program som Microsoft eller 3 part publicerar fritt för alla toouse användare även beviljas åtkomst via **administratör medgivande**. Det innebär att en administratör har fastställt hello program kan användas av alla i hello organisation, så att de loggar in toohello program med ett globalt administratörskonto och bevilja åtkomst tooeveryone i hello organisation.

tootroubleshoot problemet, börja med hello [allmänna problemområden med programåtkomst tooconsider](#general-problem-areas-with-application-access-to-consider) och läsa hello [genomgång: steg tootroubleshoot Microsoft Application åtkomst](#walkthrough-steps-to-troubleshoot-microsoft-application-access) tooget hello detaljer.

## <a name="general-problem-areas-with-application-access-tooconsider"></a>Allmänna problemområden med programåtkomst tooconsider

Nedan visas en lista över hello allmänna problemområden detaljer om om du har en uppfattning om där toostart, men vi rekommenderar att du läsa hello genomgången tooget igång snabbare: [genomgång: steg tootroubleshoot Microsoft Application åtkomst](#walkthrough-steps-to-troubleshoot-microsoft-application-access).

-   [Problem med hello användarkonto](#problems-with-the-users-account)

-   [Problem med grupper](#problems-with-groups)

-   [Problem med principer för villkorlig åtkomst](#problems-with-conditional-access-policies)

-   [Problem med programmet medgivande](#problems-with-application-consent)

## <a name="steps-tootroubleshoot-microsoft-application-access"></a>Steg tootroubleshoot Microsoft Application åtkomst

Nedan är några vanliga problem avdelningen stöter på när användarna inte logga in tooa Microsoft-program.

-   Allmänna problem toocheck först

  * Kontrollera att hello användaren loggar in toohello **korrigera URL** och inte en lokal programmets URL.

  * Kontrollera att hello användarens konto är **inte låst.**

  * Se till att hello **användarkontot finns** i Azure Active Directory. [Kontrollera om det finns ett konto i Azure Active Directory](#problems-with-the-users-account)

  * Se till att hello användarkonto **aktiverat** för inloggningar. [Kontrollera status för en användare](#problems-with-the-users-account)

  * Se till att hello användarens **lösenord inte har upphört att gälla eller har glömt.** [Återställa en användares lösenord](#reset-a-users-password) eller [aktivera lösenordsåterställning via självbetjäning](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

   * Kontrollera att **Multifaktorautentisering** inte blockerar åtkomst. [Kontrollera status för en användares multifaktorautentisering](#check-a-users-multi-factor-authentication-status) eller [Kontrollera en användares kontaktinformation för autentisering](#check-a-users-authentication-contact-info)

   * Kontrollera att en **principen för villkorlig åtkomst** eller **identitetsskydd** principen inte blockerar åtkomst. [Kontrollera en viss villkorlig åtkomstprincip](#problems-with-conditional-access-policies) eller [Kontrollera principen för villkorlig åtkomst för ett visst program](#check-a-specific-applications-conditional-access-policy) eller [inaktivera en princip för specifik villkorlig åtkomst](#disable-a-specific-conditional-access-policy)

   * Se till att en användares **autentisering kontaktinformation** fungerar toodate tooallow Multifaktorautentisering eller villkorlig åtkomst principer toobe tillämpas. [Kontrollera status för en användares multifaktorautentisering](#check-a-users-multi-factor-authentication-status) eller [Kontrollera en användares kontaktinformation för autentisering](#check-a-users-authentication-contact-info)

-   För **Microsoft** **program som kräver en licens** (till exempel Office 365) sparas följer toocheck vissa specifika problem när du har konstaterat hello allmänna problem ovan:

   * Se till att användaren hello eller har en **-licens.** [Kontrollera en användares tilldelade licenser](#check-a-users-assigned-licenses) eller [Kontrollera tilldelade licenser för en grupp](#check-a-groups-assigned-licenses)

   * Om hello licens **tilldelade tooa** **statisk grupp**, se till att hello **användaren är medlem** i gruppen. [Kontrollera en användares gruppmedlemskap](#check-a-users-group-memberships)

   * Om hello licens **tilldelade tooa** **dynamisk grupp**, se till att hello **dynamisk gruppregeln är korrekt**. [Kontrollera kriterier för medlemskap i en dynamisk grupp](#check-a-dynamic-groups-membership-criteria)

   * Om hello licens **tilldelade tooa** **dynamisk grupp**, se till att den dynamiska gruppen hello har **har behandlats klart** sitt medlemskap och den hello **användaren är en medlemmen** (Detta kan ta lite tid). [Kontrollera en användares gruppmedlemskap](#check-a-users-group-memberships)

   *  När du kontrollera hello tilldelas Kontrollera hello licens **inte gått**.

   *  Kontrollera att hello licens **för programmet hello** de använder.

-   För **Microsoft** **program som inte kräver en licens**, här är några andra saker toocheck:

   * Om programmet hello begär **behörigheter på objektnivå** (till exempel ”komma åt den här användarens postlåda”), kontrollera hello användaren har loggat in toohello program och har utfört en **användarnivå medgivande åtgärden**  toolet hello programmet åtkomst till sina data.

   * Om programmet hello begär **på administratörsnivå** (till exempel ”åt postlådor för alla användare”), se till att en Global administratör har genomfört en **administratörsnivå medgivande åtgärden på räkning av alla användare** i hello organisation.

## <a name="problems-with-hello-users-account"></a>Problem med hello användarkonto

Programåtkomst blockeras på grund av tooa problem med en användare som har tilldelats toohello program. Här följer några metoder som du kan felsöka och lösa problem med användare och deras kontoinställningar:

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

  * **Obs**: om en användare finns i en **tvingande** tillstånd, så kan du ange dem för**inaktiverad** tillfälligt toolet dem tillbaka till deras konto. När de är tillbaka i sedan kan du ändra tillståndet för**aktiverad** igen toorequire dem toore-register kontaktuppgifter under nästa inloggning. Du kan också följa hello stegen i hello [Kontrollera en användares autentisering kontaktinformation](#check-a-users-authentication-contact-info) tooverify eller ange informationen för dessa.

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

## <a name="problems-with-groups"></a>Problem med grupper

Programåtkomst blockeras på grund av tooa problem med en grupp som har tilldelats toohello program. Här följer några metoder som du kan felsöka och lösa problem med grupper och gruppmedlemskap:

-   [Kontrollera medlemskapet för en grupp](#check-a-groups-membership)

-   [Kontrollera kriterier för medlemskap i en dynamisk grupp](#check-a-dynamic-groups-membership-criteria)

-   [Kontrollera tilldelade licenser för en grupp](#check-a-groups-assigned-licenses)

-   [Ombearbeta licenser för en grupp](#reprocess-a-groups-licenses)

-   [Tilldela en licens för en grupp](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Kontrollera medlemskapet för en grupp

toocheck en grupps medlemskap, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla grupper**.

6.  **Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **medlemmar** tooreview hello lista över användare som tilldelats toothis grupp.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Kontrollera kriterier för medlemskap i en dynamisk grupp 

toocheck kriterier för medlemskap av en dynamisk grupp gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla grupper**.

6.  **Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **dynamiskt medlemskapsregler.**

8.  Granska hello **enkel** eller **avancerade** regel definierad för den här gruppen och se till att hello-användare som du vill toobe medlem i den här gruppen uppfyller dessa villkor.

### <a name="check-a-groups-assigned-licenses"></a>Kontrollera tilldelade licenser för en grupp

toocheck en grupp tilldelade licenser, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla grupper**.

6.  **Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **licenser** toosee vilken licenser hello grupp för närvarande har tilldelats.

### <a name="reprocess-a-groups-licenses"></a>Ombearbeta licenser för en grupp

tooreprocess en grupp tilldelade licenser, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla grupper**.

6.  **Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **licenser** toosee vilken licenser hello grupp för närvarande har tilldelats.

8.  Klicka på hello **Ombearbeta** knappen tooensure som hello användarlicenser toothis gruppens medlemmar är uppdaterade. Det kan ta lång tid, beroende på hello storleken och komplexiteten för hello grupp.

   >[!NOTE]
   >toodo detta snabbare, Överväg att tillfälligt tilldela en licens toohello användare direkt. [Tilldela en användare en licens](#problems-with-application-consent).
   >
   >

### <a name="assign-a-group-a-license"></a>Tilldela en licens för en grupp

tooassign tooa licensgrupp, så hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla grupper**.

6.  **Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **licenser** toosee vilken licenser hello grupp för närvarande har tilldelats.

8.  Klicka på hello **tilldela** knappen.

9.  Välj **en eller flera produkter** hello listan över tillgängliga produkter.

10. **Valfria** klickar du på hello **tilldelning alternativ** objektet toogranularly tilldela produkter. Klicka på **Ok** när detta har slutförts.

11. Klicka på hello **tilldela** knappen tooassign dessa licenser toothis grupp. Det kan ta lång tid, beroende på hello storleken och komplexiteten för hello grupp.

   >[!NOTE]
   >toodo detta snabbare, Överväg att tillfälligt tilldela en licens toohello användare direkt. [Tilldela en användare en licens](#problems-with-application-consent).
   > 
   >

## <a name="problems-with-conditional-access-policies"></a>Problem med principer för villkorlig åtkomst

### <a name="check-a-specific-conditional-access-policy"></a>Kontrollera en princip för specifik villkorlig åtkomst

toocheck, eller validera en princip för enskild villkorlig åtkomst:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** i hello navigeringsmenyn.

5.  Klicka på hello **villkorlig åtkomst** navigeringsobjektet.

6.  Klicka på hello princip som du är intresserad undersöks.

7.  Granska att det finns inga särskilda villkor, tilldelningar och andra inställningar som kan blockera åtkomst.

   >[!NOTE]
   >Du tootemporarily inaktivera den här principen tooensure den inte påverkar logga modulerna toodo detta, ange hello **aktiverar principen** växla för**nr** och klicka på hello **spara** knappen .
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Kontrollera principen för villkorlig åtkomst för ett visst program

toocheck eller verifiera principen för ett enda program för tillfället konfigurerade villkorlig åtkomst:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** i hello navigeringsmenyn.

5.  Klicka på **alla program**.

6.  Sök Visa namn eller ett program-id för hello programmet du är intresserad av eller hello användaren försöker toosign i tooby program.

     >[!NOTE]
     >Om hello-program som du letar efter inte visas klickar du på hello **Filter** knappen och expandera hello omfattning hello lista för**alla program**. Om du vill toosee fler kolumner, klickar du på hello **kolumner** knappen tooadd ytterligare information om dina program.
     >
     >

7.  Klicka på hello **villkorlig åtkomst** navigeringsobjektet.

8.  Klicka på hello princip som du är intresserad undersöks.

9.  Granska att det finns inga särskilda villkor, tilldelningar och andra inställningar som kan blockera åtkomst.

     >[!NOTE]
     >Du tootemporarily inaktivera den här principen tooensure den inte påverkar logga modulerna toodo detta, ange hello **aktiverar principen** växla för**nr** och klicka på hello **spara** knappen .
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Inaktivera en princip för specifik villkorlig åtkomst

toocheck, eller validera en princip för enskild villkorlig åtkomst:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** i hello navigeringsmenyn.

5.  Klicka på hello **villkorlig åtkomst** navigeringsobjektet.

6.  Klicka på hello princip som du är intresserad undersöks.

7.  Inaktivera hello princip genom att ange hello **aktiverar principen** växla för**nr** och klicka på hello **spara** knappen.

## <a name="problems-with-application-consent"></a>Problem med programmet medgivande

Programåtkomst blockeras eftersom hello åtkomstbehörighet medgivande åtgärden inte har inträffat. Här följer några metoder som du kan felsöka och lösa problem med programmet medgivande:

-   [Utföra en åtgärd på användarnivå medgivande](#perform-a-user-level-consent-operation)

-   [Utföra administratörsnivå medgivande åtgärden för alla program](#perform-administrator-level-consent-operation-for-any-application)

-   [Utföra administratörsnivå medgivande för en enskild klient-program](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Utföra administratörsnivå medgivande för ett program med flera innehavare](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Utföra en åtgärd på användarnivå medgivande

-   Alla öppna ID Connect-aktiverade program som begär behörighet, utför navigera toohello programmet inloggning skärmen en nivå medgivande toohello användarprogram för hello inloggad användare.

-   Om du inte vill toodo detta programmässigt, se [begär enskilda användargodkännande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>Utföra administratörsnivå medgivande åtgärden för alla program

-   För **bara program som utvecklats med hello V1 programmodell**, du kan framtvinga den här administratören nivån medgivande toooccur genom att lägga till ”**? uppmana = admin\_medgivande**” toohello slutet av en programmets tecken i URL: en.

-   För **alla program som utvecklats med hello V2 programmodell**, kan du använda den här nivån administratör medgivande toooccur genom att följa instruktionerna hello under hello **begär hello behörighet från en katalog Admin** avsnitt i [med hello medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Utföra administratörsnivå medgivande för en enskild klient-program

-   För **program enkel klienter** som begär behörighet (till exempel de som du utvecklar eller äger i din organisation), kan du utföra en **administrativ nivå medgivande** åtgärden för alla användare genom att logga in som Global administratör och klicka på hello **bevilja behörigheter** knappen hello överst i hello **programmet registret -&gt; alla program -&gt; Välj en App - &gt; Nödvändiga behörigheter** bladet.

-   För **alla program som utvecklats med hello V1 eller V2 programmodell**, kan du använda den här nivån administratör medgivande toooccur genom att följa instruktionerna hello under hello **begära hello behörigheter från en katalogadministratör** avsnitt i [med hello medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Utföra administratörsnivå medgivande för ett program med flera innehavare

-   För **program med flera klienter** som begär behörighet (t.ex. ett program en tredje part eller Microsoft, utvecklar), kan du utföra en **administrativ nivå medgivande** igen. Logga in som Global administratör och klicka på hello **bevilja behörigheter** knappen under hello **företagsprogram -&gt; alla program -&gt; Välj en App -&gt; Behörigheter** bladet (tillgänglig snart).

-   Du kan även tillämpa den här nivån administratör medgivande toooccur genom att följa instruktionerna hello under hello **begär hello behörighet från en katalogadministratör** avsnitt i [med hello medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Nästa steg
[Med hjälp av hello admin medgivande-slutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

