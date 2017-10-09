---
title: "aaaProblems inloggning tooan programmet från hello åtkomstpanelen | Microsoft Docs"
description: "Hur hello tootroubleshoot problem med åtkomst till ett program från Microsoft Azure AD-åtkomstpanelen på myapps.microsoft.com"
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
ms.reviewer: japere
ms.openlocfilehash: 346c4da06416bb9b330bdd5b1201253af19ba58b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-from-hello-access-panel"></a>Problem med att logga i tooan program från hello åtkomstpanelen

Hej åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets eller skolkonto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program som hello Azure AD-administratör har ge dem tillgång till. 

Dessa program är konfigurerade för hello användare i hello Azure AD-portalen. hello-programmet måste konfigureras korrekt och tilldelade toohello användare eller en grupp hello användare är medlem i toosee hello programmet hello åtkomstpanelen.

hello typ av appar som visas kanske en användare finns i hello följande kategorier:

-   Office 365-program

-   Microsoft och tredje parts program som har konfigurerats med federationsbaserat enkel inloggning

-   Lösenordsbaserade SSO-program

-   Program med befintliga lösningar för enkel inloggning

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

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>Möte Webbläsarkrav för hello åtkomstpanelen

Hej åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat CSS. toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare. Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.

För lösenordsbaserad SSO kan hello användarens webbläsare vara:

-   Internet Explorer 8, 9, 10, 11--på Windows 7 eller senare

-   Kanten på Windows 10 årsdagar Edition eller senare

-   Chrome--På Windows 7 eller senare, och i MacOS X eller senare

-   Firefox 26.0 eller senare--på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hur tooinstall hello åtkomst panelen webbläsartillägg

tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:

1.  Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.

2.  Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.

3.  I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.

4.  Baserat på din webbläsare vara du riktad toohello länken. **Lägg till** hello tillägget tooyour webbläsare.

5.  Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.

6.  När den har installerats, **starta om** webbläsarsessionen.

7.  Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program

Du kan också hämta hello-tillägget för Chrome och kanten på hello Direktlänkar nedan:

-   [Tillägget för Chrome åtkomst panelen](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Tillägget för Microsoft Edge åtkomst panelen](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Hur tooconfigure federerad enkel inloggning för ett program för Azure AD-galleriet

Alla program i hello Azure AD-galleriet aktiverad med Enterprise Single Sign-On-funktionen har en stegvis självstudiekurs som är tillgängliga. Du kan komma åt hello [lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) en stegvisa instruktioner för detaljer.

tooconfigure ett program från hello Azure AD-galleriet måste du:

-   [Lägga till ett program från hello Azure AD-galleriet](#add-an-application)

-   [Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Hämta Azure AD-metadata och certifikat](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Tilldela användare toohello program](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Lägga till ett program från hello Azure AD-galleriet

tooadd ett program från hello Azure AD-galleriet så hello nedan:

1.  Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet

6.  I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.

7.  Välj hello-program som du vill använda tooconfigure för enkel inloggning.

8.  Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.

9.  Klicka på **Lägg till** knappen tooadd hello program.

Efter en kort period vara kan toosee hello programmets konfiguration bladet.

### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a>Konfigurera enkel inloggning för ett program från hello Azure AD-galleriet

tooconfigure enkel inloggning för ett program gör hello nedan:

1.  <span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj **SAML-baserade inloggning** från hello **läge** listrutan.

9.  Ange hello krävs värden i **domän och URL: er.** Du bör få värdena från hello programvaruleverantören.

   1. tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde. Hello identifierare är också ett obligatoriskt värde för vissa program.

   2. tooconfigure hello program som IdP-initierad SSO hello Reply-URL som det är ett obligatoriskt värde. Hello identifierare är också ett obligatoriskt värde för vissa program.

10. **Valfritt:** klickar du på **visa avancerade inställningar för URL: en** om du vill toosee hello icke obligatoriska värden.

11. I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.

12. **Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.

   tooadd ett attribut:

   1. Klicka på **Lägg till attributet**. Ange hello **namn** och hello väljer hello **värdet** hello listrutan.

   2. Klicka på **spara.** Du kan se hello nytt attribut i hello tabell.

13. Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program. Du har dessutom hello metadata URL: er och certifikat som krävs för toosetup SSO med hello program.

14. Klicka på **spara** toosave hello konfiguration.

15. Tilldela användare toohello program.

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram

tooselect hello användar-ID och lägga till användarattribut, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill tooshow här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du har konfigurerat för enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Under hello **användarattribut** väljer hello Unik identifierare för användarna i hello **användar-ID** listrutan. hello måste markerade alternativet toomatch hello förväntat värde i hello programanvändare tooauthenticate hello.

    >[!NOTE]
    >Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest. Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.
    >
    >

9.  tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.

   tooadd ett attribut:

   1. Klicka på **Lägg till attributet**. Ange hello **namn** och hello väljer hello **värdet** hello listrutan.

   2. Klicka på **spara.** Du kan se hello nytt attribut i hello tabell.

### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Hämta metadata för hello Azure AD eller certifikat

toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du har konfigurerat för enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen. Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.

    Azure AD Ange inte en URL tooget hello metadata. hello metadata kan endast hämtas som en XML-fil.

## <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a>Hur tooconfigure federerad enkel inloggning för ett program för icke-galleriet

tooconfigure ett icke-galleriet program behöver du toohave Azure AD premium och hello programmet stöder SAML 2.0. Mer information om Azure AD-versioner finns [priser för Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)](#configuring-single-sign-on)

-   [Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Hämta Azure AD-metadata och certifikat](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)](#configuring-single-sign-on)

### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)

tooconfigure enkel inloggning för ett program som inte är i hello Azure AD-galleriet gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet

6.  Klicka på **icke-galleriet programmet** i hello **lägga till egna app** avsnitt

7.  Ange hello namnet på programmet hello i hello **namn** textruta.

8.  Klicka på **Lägg till** knappen tooadd hello program.

9.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

10. Välj **SAML-baserade inloggning** i hello **läge** listrutan

11. Ange hello krävs värden i **domän och URL: er.** Du bör få värdena från hello programvaruleverantören.

  1. tooconfigure hello program som IdP-initierad SSO ange hello Reply URL och hello identifierare.

  2. **Valfritt:** tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde.

12. I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.

13. **Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.

   tooadd ett attribut:

   1. Klicka på **Lägg till attributet**. Ange hello **namn** och hello väljer hello **värdet** hello listrutan.

   2. Klicka på **spara.** Du kan se hello nytt attribut i hello tabell.

14. Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program. Du har även Azure AD-URL: er och certifikat som krävs för programmet hello.

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram

tooselect hello användar-ID och lägga till användarattribut, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du har konfigurerat för enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Under hello **användarattribut** väljer hello Unik identifierare för användarna i hello **användar-ID** listrutan. hello måste markerade alternativet toomatch hello förväntat värde i hello programanvändare tooauthenticate hello.

   >[!NOTE]
   >Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest. Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.
   >
   >

9.  tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.

   tooadd ett attribut:

   1. Klicka **Lägg till attributet**. Ange hello **namn** och hello väljer hello **värdet** hello listrutan.

   2 Klicka **spara.** Du kan se hello nytt attribut i hello tabell.

### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Hämta metadata för hello Azure AD eller certifikat

toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du har konfigurerat för enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen. Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.

    Azure AD Ange inte en URL tooget hello metadata. hello metadata kan endast hämtas som en XML-fil.

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet

tooconfigure ett program från hello Azure AD-galleriet måste du:

-   [Lägga till ett program från hello Azure AD-galleriet](#add-an-application)

-   [Konfigurera hello program för lösenord för enkel inloggning](#configure-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Lägga till ett program från hello Azure AD-galleriet

tooadd ett program från hello Azure AD-galleriet så hello nedan:

1.  Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet

6.  I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program

7.  Välj hello-program som du vill använda tooconfigure för enkel inloggning

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

6.  Välj hello-program som du vill tooconfigure enkel inloggning

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj hello läge **lösenordsbaserade inloggning.**

9.  Tilldela användare toohello program.

10. Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare. Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Hur tooconfigure lösenord enkel inloggning för ett program för icke-galleriet

tooconfigure ett program från hello Azure AD-galleriet måste du:

-   [Lägga till ett icke-galleriet program](#add-a-non-gallery-application)

-   [Konfigurera hello program för lösenord för enkel inloggning](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Lägga till ett icke-galleriet program

tooadd ett program från hello Azure AD-galleriet så hello nedan:

1.  Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet

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

 * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

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

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Om de här åtgärderna inte hello problemet hello

Öppna ett supportärende med hello efter information om tillgängliga:

-   Fel-ID för korrelation

-   UPN (användarens e-postadress)

-   Klient-ID

-   Typ av webbläsare

-   Tidszon och tid/tidsperioden under fel inträffar

-   Fiddler spårningar

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)

