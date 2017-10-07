---
title: "Konfigurera lösenord enkel inloggning för ett icke-galleriet program aaaProblem | Microsoft Docs"
description: "Förstå hello vanliga problem personer yta när du konfigurerar lösenord enkel inloggning för anpassade program för icke-galleriet som inte listas i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 3aee0a4c525bb3da338da2da0882ec572cf0e5e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Konfigurera lösenord enkel inloggning för ett icke-galleriet program problem

Den här artikeln hjälper dig toounderstand hello vanliga problem personer står inför när du konfigurerar **lösenord enkel inloggning** med ett icke-galleriet program.

## <a name="how-toocapture-sign-in-fields-for-an-application"></a>Hur inloggning toocapture fält för ett program

Logga in fältet stöds bara för HTML-aktiverade inloggningssidor och är **stöds inte för icke-standard inloggningssidor**, som de som använder Flash eller andra icke-HTML-aktiverade tekniker.

Det finns två sätt som du kan avbilda inloggning fält för dina anpassade program:

-   Fältet för automatisk inloggning avbildning

-   Fältet för manuell inloggning avbildning

**Fältet för automatisk inloggning avbilda** fungerar väl med de flesta HTML-aktiverade inloggningssidor, om de använder **välkända DIV-ID: N för hello användarnamn och lösenord Ange** fältet. Hej hur detta fungerar är genom skrapning hello HTML på hello sidan toofind DIV-ID: N som matchar vissa villkor och sedan spara att metadata för det här programmet så att vi kan spela upp lösenord tooit senare.

**Fältet för manuell inloggning avbilda** kan användas i hello ärende hello programmet **leverantör inte etiketten** hello ange fälten som används för inloggning. Fältet för manuell inloggning avbilda kan också användas i hello fallet när hello **leverantör återgivningar flera fält** som inte kan identifieras automatiskt. Azure AD kan lagra data för så många fält som finns på hello inloggningssidan så länge du berätta var dessa fält är i hello-sidan.

I allmänhet **om automatisk inloggning fältet avbilda inte fungerar, föreslår vi alltid försöker hello manuella alternativet.**

### <a name="how-tooautomatically-capture-sign-in-fields-for-an-application"></a>Hur tooautomatically avbilda inloggning fält för ett program

tooconfigure **lösenordsbaserade enkel inloggning** för ett program som använder **fält för automatisk inloggning avbilda**, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj hello läge **lösenordsbaserade inloggning.**

9.  Ange hello **inloggnings-URL**. Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till. **Se till att hello inloggning fält är synliga på hello-URL som du anger**.

10. Klicka på hello **spara** knappen.

11. När du gör det, kommer vi automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och kan du överföra toouse Azure AD toosecurely lösenord toothat program med hjälp av hello åtkomst panelen webbläsartillägg.

## <a name="how-toomanually-capture-sign-in-fields-for-an-application"></a>Hur toomanually avbilda inloggning fält för ett program

toomanually avbilda inloggning fält, måste du först ha hello åtkomst panelen webbläsartillägget installerad och **inte körs i inPrivate incognito eller privat läge.** tooinstall hello webbläsartillägg, följ hello stegen i hello [hur tooinstall hello åtkomst panelen webbläsartillägget](#i-cannot-manually-detect-sign-in-fields-for-my-application) avsnitt.

tooconfigure **lösenordsbaserade enkel inloggning** för ett program som använder **manuell inloggning fältet avbilda**, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj hello läge **lösenordsbaserade inloggning.**

9.  Ange hello **inloggnings-URL**. Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till. **Se till att hello inloggning fält är synliga på hello-URL som du anger**.

10. Klicka på hello **spara** knappen.

11. När du gör det, kommer vi automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och kan du överföra toouse Azure AD toosecurely lösenord toothat program med hjälp av hello åtkomst panelen webbläsartillägg. Hello om detta misslyckas, kan du **ändra hello inloggning läge toouse manuell inloggning fältet avbilda** genom att fortsätta toostep 12.

12. Klicka på **konfigurera &lt;appname&gt; inställningar för lösenord enkel inloggning**.

13. Välj hello **identifieras manuellt inloggning fält** konfigurationsalternativet.

14. Klicka på **OK**.

15. Klicka på **Spara**.

16. Följ hello på skärmen instruktioner toouse hello åtkomstpanelen.

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Felet ”Det gick inte att hitta alla fält på den URL” visas

Det här felet visas när automatisk identifiering av inloggning fält misslyckas. Det här problemet, försök manuell inloggning fältet identifiering av följande hello stegen i hello tooresolve [hur toomanually avbilda inloggning fält för ett program](#how-to-manually-capture-sign-in-fields-for-an-application) avsnitt.

## <a name="i-see-an-unable-toosave-single-sign-on-configuration-error"></a>Visas felmeddelandet ”Det gick inte toosave Single Sign-on-konfiguration”

I vissa sällsynta fall kan uppdatera hello enkel inloggning konfigurationen misslyckas. tooresolve detta försök spara hello enkel inloggning konfigurationen igen.

Om problemet kvarstår toofail konsekvent, öppna ett supportärende och ange hello information som samlats in i hello [hur toosee hello information om en portalmeddelandet](#i-cannot-manually-detect-sign-in-fields-for-my-application) och [hur tooget genom att skicka meddelande information tooa stöd för tekniker](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) avsnitt.

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a>Jag kan inte manuellt identifiera tecken i fälten för Mina program

Hello-beteenden som kan uppstå när Manuell identifiering inte fungerar bland annat:

-   hello manuell bildtagningen fanns toowork, men hello fält avbildas inte korrekt

-   hello höger fält hämta inte markeras när du utför hello avbildningsprocessen

-   hello processen tar mig toohello programmets inloggningssida som förväntat, men ingenting händer

-   Manuell avbilda visas toowork men SSO inträffa inte när användarna navigerar toohello programmet från hello åtkomstpanelen.

Kontrollera hello följande om du stöter på något av följande problem:

-   Kontrollera att du har hello senaste versionen av hello åtkomst panelen webbläsartillägget **installerat** och **aktiverat** genom att följa stegen hello i hello [hur tooinstall hello åtkomst panelen webbläsare tillägget](#how-to-install-the-access-panel-browser-extension) avsnitt.

-   Se till att du inte försöker hello processen när webbläsaren i **incognito InPrivate- eller privat läge**. tillägget för hello åtkomst panelen stöds inte i dessa lägen.

-   Se till att användarna inte försöker toosign i toohello programmet hello åtkomst panelen när i **incognito InPrivate- eller privat läge**. tillägget för hello åtkomst panelen stöds inte i dessa lägen.

-   Försök hello manuella processen igen, säkerställt hello röda markörer är över hello rätt fält.

-   Om hello manuell bildtagningen verkar toohang eller hello inloggningssidan inte göra något (fall 3 ovan), försök hello manuella processen igen. Men nu när du har slutfört hello process, tryck på hello **F12** knappen tooopen webbläsarens developer-konsolen. Öppna en gång, hello **konsolen** och skriv **window.location= ”&lt;ange hello tecken i url som du angav när du konfigurerar hello app&gt;”** och tryck sedan på  **Ange**. Denna kraft en sida omdirigera som avslutas hello processen och lagra hello fält som har spelats in.

Om inget av dessa sätt fungerar för dig kan vi hjälpa. Öppna ett supportärende hello uppgifter om du försökte samt hello information som samlats in i hello [hur toosee hello information om en portalmeddelandet](#i-cannot-manually-detect-sign-in-fields-for-my-application) och [hur tooget genom att skicka meddelandeinformation tooa stöd tekniker](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) avsnitten (om tillämpligt).

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hur tooinstall hello åtkomst panelen webbläsartillägg

tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:

1.  Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.

2.  Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.

3.  I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.

4.  Baserat på din webbläsare vara du riktad toohello länken. **Lägg till** hello tillägget tooyour webbläsare.

5.  Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.

6.  När den har installerats, **starta om** webbläsarsessionen.

7.  Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program.

Du kan också hämta hello-tillägget för Chrome och Firefox från hello Direktlänkar nedan:

-   [Tillägget för Chrome åtkomst panelen](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Tillägget för Firefox åtkomst panelen](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-toosee-hello-details-of-a-portal-notification"></a>Hur toosee hello information om ett meddelande om portal

Du kan se hello information om eventuella portalmeddelandet genom att följa hello stegen nedan:

1.  Klicka på hello **meddelanden** ikon (hello bell) i hello övre högra hörnet av hello Azure-portalen

2.  Markera alla meddelanden i en **fel** tillstånd (de med en röd (!) nästa toothem).

  >! Observera] du kan klicka på meddelanden i en **lyckade** eller **pågår** tillstånd.
  >
  >

3.  Den här öppna hello **detaljer** bladet.

4.  Använd den här informationen själv toounderstand mer information om hello problem.

5.  Om du fortfarande behöver hjälp kan också dela informationen med en stöd tekniker eller hello grupp tooget produkthjälp med ditt problem.

6.  Klicka på hello **kopia** **ikonen** toohello höger i hello **Kopieringsfel** textruta toocopy alla hello meddelande information tooshare med en support eller produkt grupp tekniker.

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a>Hur tooget genom att skicka meddelande information tooa supportteknikern

Det är mycket viktigt att du dela **alla hello information som visas nedan** med en supporttekniker om du behöver hjälp, så att de kan hjälpa dig snabbt. Du kan göra det enkelt av **tar en skärmbild** eller genom att klicka på hello **kopiera felikonen**, hitta toohello höger om hello **Kopieringsfel** textruta.

## <a name="notification-details-explained"></a>Meddelandeinformation förklaras

hello nedan förklaras mer vad varje hello avisering artiklar innebär och ger exempel på var och en av dem.

### <a name="essential-notification-items"></a>Viktigt meddelande objekt

-   **Rubrik** – hello beskrivande rubrik av hello-meddelande

    -   Exempel – **Application proxy-inställningar**

-   **Beskrivning** – hello beskrivning av vad som hänt på grund av hello åtgärden

    -   Exempel – **intern url har angett används redan av ett annat program**

-   **Meddelande-Id** – hello unikt id för hello-meddelande

    -   Exempel – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Id för klientbegäran** – hello specifik begäran-id som din webbläsare

    -   Exempel – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Tid UTC stämpel** – hello tidsstämpeln då uppstod hello-meddelande i UTC

    -   Exempel – **2017-03-23T19:50:43.7583681Z**

-   **Internt transaktions-Id** – hello internt ID vi kan använda toolook hello fel i vårt system

    -   Exempel – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN** – hello-användaren som utförde åtgärden hello

    -   Exempel –**tperkins@f128.info**

-   **Klient-Id** – hello unikt ID för hello-klient som hello användaren som utförde åtgärden hello var medlem av

    -   Exempel – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Användarobjektet Id** – hello unikt ID för hello-användaren som utförde åtgärden hello

    -   Exempel – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Detaljerat meddelande objekt

-   **Visningsnamn** – **(kan vara tom)** en mer detaljerad visningsnamn för hello-fel

    -   Exempel * – **Application proxy-inställningar**

-   **Status för** – hello viss status av hello-meddelande

    -   Exempel * – **misslyckades**

-   **Objekt-Id** – **(kan vara tom)** hello mot vilken hello åtgärden utfördes objekt-ID

    -   Exempel – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Information om** – hello detaljerad beskrivning av vad som hänt på grund av hello åtgärden

    -   Exempel – **intern url 'http://bing.com/' är ogiltig eftersom den redan används**

-   **Kopiera fel** – Klicka på hello **kopiera-ikonen** toohello höger i hello **Kopieringsfel** textruta toocopy alla hello meddelande information tooshare med en support eller produkt grupp tekniker

    -   Exempel –```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)

