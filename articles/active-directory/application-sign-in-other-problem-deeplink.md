---
title: "logga in tooan program med hjälp av en djuplänken aaaProblems | Microsoft Docs"
description: "Hur tootroubleshoot problem med åtkomst till ett program från en Webbadress med hjälp av Azure AD-djuplänken"
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
ms.openlocfilehash: dc82410001ac05895cc0244c3a89ace71bcf01a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-using-a-deeplink"></a>Problem med att logga i tooan program som använder en djuplänken

Hej åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets eller skolkonto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program som hello Azure AD-administratör har ge dem tillgång till. 

Dessa program är konfigurerade för hello användare i hello Azure AD-portalen. hello-programmet måste konfigureras korrekt och tilldelade toohello användare eller en grupp hello användare är medlem i toosee hello programmet hello åtkomstpanelen.

Djuplänkar eller användarbehörighet är URL: er länkar som användarna kan använda tooaccess sina lösenord SSO-program direkt från sina webbläsare URL fält. Genom att gå toothis länken användare att automatiskt inloggad hello programmet utan att först ha toogo toohello åtkomstpanelen. Detta är hello samma länk som användaren använder tooaccess programmen från startprogrammet hello Office 365.

## <a name="general-issues-toocheck-first"></a>Allmänna problem toocheck först

-   Kontrollera att du med hjälp av en **webbläsare** som uppfyller hello minimikraven för hello åtkomstpanelen.

-   Kontrollera att hello användarens webbläsare har lagt till hello URL för hello programmet tooits **tillförlitliga platser**.

-   Se till att toocheck hello programmet **konfigurerats** korrekt.

-   Se till att hello användarkonto **aktiverat** för inloggningar.

-   Kontrollera att hello användarens konto är **inte låst.**

-   Se till att hello användarens **lösenord inte har upphört att gälla eller har glömt.**

-   Kontrollera att **Multifaktorautentisering** inte blockerar åtkomst.

-   Kontrollera att en **principen för villkorlig åtkomst** eller **identitetsskydd** principen inte blockerar åtkomst.

-   Se till att en användares **autentisering kontaktinformation** fungerar toodate tooallow Multifaktorautentisering eller villkorlig åtkomst principer toobe tillämpas.

-   Se till att tooalso försök rensar cookies i webbläsaren och försök toosign i igen.

## <a name="checking-hello-deeplink"></a>Kontrollera hello djuplänken

toocheck om du har rätt hello-djuplänken gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

7.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

8.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

9.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

10. Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

11. Välj hello-program som du vill använda hello Kontrollera hello djuplänken för.

12. Hitta hello etikett **användaren åtkomst-URL**. Du djuplänken ska matcha denna URL.

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

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet

tooconfigure ett program från hello Azure AD-galleriet måste du:

-   [Lägga till ett program från hello Azure AD-galleriet](#add-an-application-from-the-Azure-AD-gallery)

-   [Konfigurera hello program för lösenord för enkel inloggning](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Lägga till ett program från hello Azure AD-galleriet

tooadd ett program från hello Azure AD-galleriet så hello nedan:

1.  Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**.

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.

6.  I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.

7.  Välj hello-program som du vill använda tooconfigure för enkel inloggning.

8.  Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.

9.  Klicka på **Lägg till** knappen tooadd hello program.

Efter en kort period vara kan toosee hello programmets konfiguration bladet.

### <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurera hello program för lösenord för enkel inloggning

tooconfigure enkel inloggning för ett program gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj hello läge **lösenordsbaserade inloggning.**

9.  [Tilldela användare toohello program](#how-to-assign-a-user-to-an-application-directly).

10. Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare. Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Hur tooconfigure lösenord enkel inloggning för ett program för icke-galleriet

tooconfigure ett program från hello Azure AD-galleriet måste du:

-   [Lägga till ett icke-galleriet program](#add-a-non-gallery-application)

-   [Konfigurera hello program för lösenord för enkel inloggning](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Lägga till ett icke-galleriet program

tooadd ett program från hello Azure AD-galleriet så hello nedan:

1.  Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**.

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.

6.  Klicka på **icke-galleriet program.**

7.  Ange hello namnet på ditt program i hello **namn** textruta. Välj **lägga till.**

Efter en kort period vara kan toosee hello programmets konfiguration bladet.

### <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurera hello program för lösenord för enkel inloggning

tooconfigure enkel inloggning för ett program gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

    1.  Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj hello läge **lösenordsbaserade inloggning.**

9.  Ange hello **inloggnings-URL**. Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till. Kontrollera hello inloggning fält är synliga på hello-URL.

10. Tilldela användare toohello program.

11. Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare. Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.

## <a name="how-tooassign-a-user-tooan-application-directly"></a>Hur tooassign en tooan användarprogram direkt

tooassign en eller flera användare tooan programmet direkt, gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooassign en toofrom hello-användarlistan.

7.  När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.

8.  Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.

9.  Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.

10. Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.

11. Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**. Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.

12. **Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.

13. När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.

14. **Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.

15. Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.

Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Om de här åtgärderna inte hello lösa problemet med hello. 

Öppna ett supportärende med hello efter information om tillgängliga:

-   Fel-ID för korrelation

-   UPN (användarens e-postadress)

-   Klient-ID

-   Typ av webbläsare

-   Tidszon och tid/tidsperioden under fel inträffar

-   Fiddler spårningar

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
