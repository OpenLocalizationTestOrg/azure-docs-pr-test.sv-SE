---
title: "aaaError på sidan för ett program när du har loggat in | Microsoft Docs"
description: "Hur tooresolve problem med Azure AD logga in när hello programmet genererar ett fel"
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
ms.openlocfilehash: 317b6f8e6417520ead80ae4e26c591ba6b134683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Fel på sidan för ett program när du loggar in

Azure AD har signerat hello användare i i det här scenariot, men programmet hello visas ett fel som inte tillåter hello toosuccessfully Slutför hello användarinloggning i flödet. I det här scenariot accepterar inte hello programmet hello svar problemet av Azure AD.

Det finns några möjliga orsaker till varför programmet hello accepterade hello svaret från Azure AD. Om hello fel i hello program inte är tillräckligt tydligt tooknow är vad saknas hello svar:

-   Om hello programmet hello Azure AD-galleriet, kan du kontrollera att du har följt alla hello stegen i artikeln hello [hur toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Använda ett verktyg som [Fiddler](http://www.telerik.com/fiddler) toocapture SAML begära SAML-svar och SAML-token.

-   Dela hello SAML-svar med hello programmet leverantör tooknow vad som saknas.

## <a name="missing-attributes-in-hello-saml-response"></a>Saknade attribut i hello SAML-svar

tooadd ett attribut i hello Azure AD configuration toobe skickas som svar på hello Azure AD, så hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Klicka på **visa och redigera alla andra användare attribut under** hello **användarattribut** avsnittet tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.

   tooadd ett attribut:

   * Klicka på **Lägg till attributet**. Ange hello **namn** och hello väljer hello **värdet** hello listrutan.

   * Klicka på **spara.** Du kan se hello nytt attribut i hello tabell.

9.  Spara hello-konfigurationen.

Nästa gång hello användaren loggar in toohello programmet Azure AD skicka hello nytt attribut i hello SAML-svar.

## <a name="hello-application-expects-a-different-user-identifier-value-or-format"></a>hello program förväntar sig ett annat användar-ID-värde eller format

hello misslyckas inloggning toohello programmet eftersom hello SAML-svaret saknar attribut, till exempel roller eller eftersom programmet hello förväntar sig ett annat format för hello ID för entiteterna attribut.

## <a name="add-an-attribute-in-hello-azure-ad-application-configuration"></a>Lägg till ett attribut i programkonfigurationen för hello Azure AD:

toochange hello användar-ID-värde, så hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Under hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.

## <a name="change-entityid-user-identifier-format"></a>Ändra ID för entiteterna (användar-ID)-format

Om programmet hello förväntar sig ett annat format för hello ID för entiteterna attribut. Du kan sedan inte kan tooselect hello ID för entiteterna (användar-ID) format att Azure AD skickar toohello programmet hello svar efter autentisering av användare.

Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest. Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.

## <a name="hello-application-expects-a-different-signature-method-for-hello-saml-response"></a>hello program förväntar sig en annan signaturmetod för hello SAML-svar

toochange vilka delar av hello SAML-token har signerats digitalt av Azure Active Directory. Följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Klicka på **visa avancerade inställningar för signering av certifikat** under hello **SAML-signeringscertifikat** avsnitt.

9.  Välj lämplig hello **signering alternativet** förväntades av programmet hello:

  * Signera SAML-svar

  * Signera SAML-svar och assertion

  * Signera SAML-kontroll

Nästa gång hello användaren loggar in toohello programmet hello Azure AD-inloggning en del av hello SAML-svar som valts.

## <a name="hello-application-expects-hello-signing-algorithm-toobe-sha-1"></a>hello program förväntar sig hello signering toobe algoritmen SHA-1

Som standard loggar Azure AD hello SAML-token med hello algoritm för de flesta. Du bör inte ändra hello logga algoritmen tooSHA-1 om det inte krävs av hello program.

toochange Hej Signeringsalgoritm, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Klicka på **visa avancerade inställningar för signering av certifikat** under hello **SAML-signeringscertifikat** avsnitt.

9.  Välj SHA-1 i hello **signering algoritmen**.

Nästa gång Hej användaren loggar in toohello program, Azure AD logga hello SAML-token med hjälp av SHA-1-algoritmen.

## <a name="next-steps"></a>Nästa steg
[Hur toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
