---
title: "aaaProblems inloggning tooa galleriet program konfigurerade för federerad enkel inloggning | Microsoft Docs"
description: "Riktlinjer för hello specifika fel när du loggar in på ett program som du har konfigurerat för SAML-baserade federerad enkel inloggning med Azure AD"
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
ms.openlocfilehash: ba20a4904860cf063967029cad33fb80f16e4956
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooa-gallery-application-configured-for-federated-single-sign-on"></a>Problem med att logga i tooa galleriet program konfigurerade för federerad enkel inloggning

tootroubleshoot ditt problem, behöver du tooverify hello programkonfigurationen i Azure AD som följer:

-   Du har följt alla hello konfigurationssteg för hello Azure AD-galleriet-program.

-   hello identifierare och Reply-URL som konfigurerats i AAD matchar de förväntade värdena i hello program

-   Du har tilldelat användare toohello program

## <a name="application-not-found-in-directory"></a>Programmet hittades inte i katalog

*Fel AADSTS70001: Programmet med ID 'https://contoso.com' hittades inte i hello directory*.

**Möjlig orsak**

hello utfärdaren attribut skickar från hello programmet tooAzure AD i hello SAML-begäran inte matchar hello identifierare värdet som konfigurerats i programmet hello Azure AD.

**Lösning**

Se till att attributet hello utfärdaren i hello SAML-begäran matchar hello ID-värdet som konfigurerats i Azure AD:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för** Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Gå för**domän och URL: er** avsnitt. Kontrollera att hello-värdet i hello identifierare textruta matchar hello värdet för hello identifierarvärde visas i hello-fel.

När du har uppdaterat hello identifierarvärde i Azure AD och dess matchande hello värdet skickar av programmet hello i hello SAML-begäran, bör du kunna toosign i toohello program.

## <a name="hello-reply-address-does-not-match-hello-reply-addresses-configured-for-hello-application"></a>hello svarsadressen matchar inte hello svar adresser som har konfigurerats för hello program.

*Fel AADSTS50011: hello svarsadressen 'https://contoso.com' matchar inte hello svar adresser som har konfigurerats för hello program*

**Möjlig orsak**

Hej AssertionConsumerServiceURL värdet i hello SAML-begäran matchar inte hello Reply URL värde eller mönster som konfigurerats i Azure AD. Hej AssertionConsumerServiceURL värdet i hello SAML-begäran är hello URL som visas i hello-fel.

**Lösning**

Se till att hello AssertionConsumerServiceURL värde i dess matchande hello Reply URL-värdet som konfigurerats i Azure AD hello SAML-begäran.

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för** Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Gå för**domän och URL: er** avsnitt. Kontrollera eller uppdatera hello värdet i hello Reply URL textruta toomatch hello AssertionConsumerServiceURL värdet i hello SAML-begäran.  
    * Om du inte ser hello Reply URL-textrutan, Välj hello **visa avancerade inställningar för URL: en** kryssrutan.

När du har uppdaterat hello Reply URL-värdet i Azure AD och den har matchar hello värde skickar av hello program i hello SAML-begäran, bör du kunna toosign i toohello program.

## <a name="user-not-assigned-a-role"></a>Användare som inte har tilldelats en roll

*Fel AADSTS50105: hello inloggad användare ”brian@contoso.com' är inte tilldelad tooa roll för programmet hello*.

**Möjlig orsak**

hello användaren har inte beviljats åtkomst toohello program i Azure AD.

**Lösning**

tooassign en eller flera användare tooan programmet direkt, gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för** Alla program.**

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

## <a name="not-a-valid-saml-request"></a>Inte en giltig SAML-begäran

*Fel AADSTS75005: hello-begäran är inte ett giltigt Saml2-protokollmeddelande.*

**Möjlig orsak**

Azure AD stöder inte hello SAML-begäran skickas av programmet hello för enkel inloggning. Några vanliga problem är:

-   Obligatoriska fält i hello SAML-begäran som saknas

-   SAML-kodade metoden

**Lösning**

1.  Avbilda SAML-begäran. Följ hello kursen [hur toodebug SAML-baserade enkel inloggning tooapplications i Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn hur toocapture hello SAML begäran.

2.  Kontakta programvaruleverantören för hello och resursen:

   -   SAML-begäran

   -   [Krav för Azure AD enkel inloggning SAML-protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

De bör verifiera de stöder hello Azure AD SAML-implementering för enkel inloggning.

## <a name="no-resource-in-requiredresourceaccess-list"></a>Ingen resurs i requiredResourceAccess lista

*Fel AADSTS65005:hello klientprogrammet har begärt åtkomst tooresource ' 00000002-0000-0000-c000-000000000000'. Begäran misslyckades på grund av hello-klienten inte har angett den här resursen i listan över requiredResourceAccess*.

**Möjlig orsak**

hello programobjektet är skadad.

**Lösning: alternativ 1**

toosolve hello problem, Lägg till hello unika identifierarvärde i hello Azure AD-konfigurationen. tooadd en identifierarvärde gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för** Alla program.**

6.  Välj hello-program som du har konfigurerat för enkel inloggning.

7.  När hello-programmet läses in, klicka på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn

8.  Under hello **domän och URL: en** avsnittet, kontrollera hello **visa avancerade inställningar för URL: en**.

9.  i hello **identifierare** textruta ange en unik identifierare för programmet hello.

10. **Spara** hello konfiguration.


**Alternativ 2**

Om alternativet 1 ovan inte fungerar för dig, tar du bort hello programmet från hello directory. Sedan kan lägga till och konfigurera om programmet hello, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för** Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning

7.  Klicka på **ta bort** på hello övre vänstra av programmet hello **översikt** bladet.

8.  Uppdatera Azure AD och lägga till hello program från hello Azure AD-galleriet. Konfigurera sedan hello program

<span id="_Hlk477190176" class="anchor"></span>När du konfigurerar om programmet hello ska kunna toosign i toohello program.

## <a name="certificate-or-key-not-configured"></a>Certifikatet eller nyckeln som inte har konfigurerats

*Fel AADSTS50003: Inga signeringsnyckeln konfigurerats.*

**Möjlig orsak**

hello programobjektet är skadad och kan identifiera inte hello-certifikatet som konfigurerats för programmet hello Azure AD.

**Lösning**

toodelete och skapa ett nytt certifikat, hello följande sätt:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

 * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för** Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Klicka på **Skapa nytt certifikat** under hello **SAML signeringscertifikat** avsnitt.

9.  Välj upphör att gälla. Klicka på **spara.**

10. Kontrollera **aktivera nya certifikatet** toooverride hello aktiva certifikatet. Klicka på **spara** hello överst på bladet hello och acceptera tooactivate hello förnyelsecertifikat.

11. Under hello **SAML-signeringscertifikat** klickar du på **ta bort** tooremove hello **används inte** certifikat.

## <a name="problem-when-customizing-hello-saml-claims-sent-tooan-application"></a>Problem när du anpassar hello SAML anspråk skickas tooan program

toolearn hur toocustomize hello SAML attributet anspråk skickas tooyour program finns i [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.

## <a name="next-steps"></a>Nästa steg
[Hur toodebug SAML-baserade enkel inloggning tooapplications i Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)
