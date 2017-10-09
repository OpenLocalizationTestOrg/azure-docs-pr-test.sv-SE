---
title: "aaaHow tooconfigure federerad enkel inloggning för ett icke-galleriet program | Microsoft Docs"
description: "Hur tooconfigure federerad enkel inloggning för ett anpassat program för icke-galleriet som du vill toointegrate med Azure AD"
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
ms.openlocfilehash: f4a37cb500c075d0ce917ad8f13411f2ce208c56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a>Hur tooconfigure federerad enkel inloggning för ett program för icke-galleriet

tooconfigure ett icke-galleriet program behöver du toohave Azure AD premium och hello programmet stöder SAML 2.0. Mer information om Azure AD-versioner finns [priser för Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="overview-of-steps-required"></a>Översikt över steg som krävs
Nedan finns en hög nivå översikt över hello steg krävs tooconfigure federerad enkel inloggning för ett galleri (t.ex. anpassade) program.

-   [Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)](#_Configuring_single_sign-on)

-   [Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Hämta Azure AD-metadata och certifikat](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)](#_Configuring_single_sign-on)

-   [Tilldela användare toohello program](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-toonon-gallery-applications"></a>Konfigurera enkel inloggning toonon-galleriet program

tooconfigure enkel inloggning för ett program som inte är i hello Azure AD-galleriet gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.

6.  Klicka på **icke-galleriet programmet** i hello **lägga till egna app** avsnitt

7.  Ange hello namnet på programmet hello i hello **namn** textruta.

8.  Klicka på **Lägg till** knappen tooadd hello program.

9.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

10. Välj **SAML-baserade inloggning** i hello **läge** listrutan.

11. Ange hello krävs värden i **domän och URL: er.** Du bör få värdena från hello programvaruleverantören.

   1. tooconfigure hello program som IdP-initierad SSO ange hello Reply URL och hello identifierare.

   2. **Valfritt:** tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde.

12. I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.

13. **Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.

   tooadd ett attribut:

   1. Klicka på **Lägg till attributet**. Ange hello **namn** och hello väljer hello **värdet** hello listrutan.

   2. Klicka på **spara.** Du kan se hello nytt attribut i hello tabell.

14. Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program. Du har även Azure AD-URL: er och certifikat som krävs för programmet hello.

15. [Tilldela användare toohello program.](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram

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

 >[! Observera} Azure AD Välj hello format för attributet NameID hello (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest. Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.
 >
 >

9.  tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.

   tooadd ett attribut:

   1. Klicka på **Lägg till attributet**. Ange hello **namn** och hello väljer hello **värdet** hello listrutan.

   2. Klicka på **spara.** Du kan se hello nytt attribut i hello tabell.

## <a name="download-hello-azure-ad-metadata-or-certificate"></a>Hämta metadata för hello Azure AD eller certifikat

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

## <a name="assign-users-toohello-application"></a>Tilldela användare toohello program

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

Efter en kort tidsperiod att hello användare som du har valt kan toolaunch dessa program med hjälp av hello metoder som beskrivs under hello lösning beskrivning.

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a>Anpassa hello SAML anspråk skickas tooan program

toolearn hur toocustomize hello SAML attributet anspråk skickas tooyour program finns i [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
