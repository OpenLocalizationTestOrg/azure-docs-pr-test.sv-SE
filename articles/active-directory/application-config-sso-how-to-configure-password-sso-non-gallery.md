---
title: "aaaHow tooconfigure lösenord enkel inloggning för en icke-galleriet applicationn | Microsoft Docs"
description: "Hur tooconfigure ett anpassat program icke-galleriet för säkra lösenordsbaserade enkel inloggning när det inte finns med i hello Azure AD Application Gallery"
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
ms.openlocfilehash: e3d0e658f792a0a63110a198811edc66acd6ccf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Hur tooconfigure lösenord enkel inloggning för ett program för icke-galleriet

Dessutom toohello alternativ hittades inom hello Azure AD Application Gallery hello alternativet tooadd har också en **icke-galleriet programmet** när hello-program som du vill använda inte finns. Med den här funktionen kan du kan lägga till alla program som redan finns i din organisation eller något tredje parts-program som du kan använda från en leverantör som inte är en del av hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

När du lägger till ett icke-galleriet program kan du konfigurera hello enkel inloggning programmet använder genom att välja hello **enkel inloggning** navigeringsobjektet på ett affärsprogram i hello [Azure-portalen ](https://portal.azure.com/).

En av hello enkel inloggning metoder tillgängliga tooyou är hello [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) alternativet. Med hello **lägga till ett icke-galleriet program** upplevelse, kan du integrera alla program som visas med ett HTML-baserad användarnamn och lösenord för ingående fält, även om den inte är i vår uppsättning redan integrerade program.

Hej hur detta fungerar är av en sida skrapning teknik som är en del av hello åtkomstpanelen tillägg som gör att vi tooauto-identifiera inkommande fälten användarnamn och lösenord, lagra dem på ett säkert sätt för dina specifika programinstansen. På ett säkert sätt replay användarnamn och lösenord toothose fält när en användare navigerar toothat programmet på åtkomstpanelen för hello program.

Detta är ett bra sätt tooget igång snabbt integrera alla slags program i Azure AD, och du kan:

-   Integrera **alla program i hello world** med Azure AD-klienten, så länge den återger en HTML-användarnamn och lösenord inmatningsfält

-   Aktivera **enkel inloggning för användarnas** genom att lagra och spela upp användarnamn och lösenord för hello programmet på ett säkert sätt som du har integrerat med Azure AD

-   **Automatisk identifiering av indata** fält för alla program och att du toomanually identifiera dessa fält med hello webbläsartillägget för åtkomst till Kontrollpanelen, om automatisk identifiering inte hittar dem.

-   **Stöd för program som kräver flera inloggning fält** för program som kräver mer än bara användarnamn och lösenord fält toosign i

-   **Anpassa hello etiketter** av hello användarnamn och lösenord indatafält användarna ser på hello [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) när de anger sina autentiseringsuppgifter

-   Tillåt din **användare** tooprovide användarnamn och lösenord för alla program-konton som de skriver manuellt på hello [åtkomstpanelen för programmet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Tillåt en **tillhör hello affärsgrupp** toospecify hello användarnamn och lösenord tilldelad tooa användare med hjälp av hello [Self-Service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funktion

-   Tillåt en **administratör** toospecify hello användarnamn och lösenord som tilldelats tooa användare med hjälp av referenser för uppdatering av hello funktionen när [tilldela en tooan-användarprogram](#_How_to_configure_1)

-   Tillåt en **administratör** toospecify hello delade användarnamnet eller lösenordet som används av en grupp människor med hjälp av autentiseringsuppgifter för uppdatering av hello funktionen när [tilldela en grupp tooan program](#assign-an-application-to-a-group-directly)

Nedan beskrivs hur du kan aktivera [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) tooany program som du lägger till med hjälp av hello **lägga till ett icke-galleriet program** upplevelse.

## <a name="overview-of-steps-required"></a>Översikt över steg som krävs

tooconfigure ett program från hello Azure AD-galleriet måste du:

-   [Lägga till ett icke-galleriet program](#add-a-non-gallery-application)

-   [Konfigurera hello program för lösenord för enkel inloggning](#configure-the-application-for-password-single-sign-on)

-   [Tilldela hello programmet tooa användare eller grupp](#assign-the-application-to-a-user-or-a-group)

    -   [Tilldela en användare tooan program direkt](#assign-a-user-to-an-application-directly)

    -   [Tilldela en programgrupp tooa direkt](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a>Lägga till ett icke-galleriet program

tooadd ett program från hello Azure AD-galleriet så hello nedan:

1.  Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet

6.  Klicka på **icke-galleriet program.**

7.  Ange hello namnet på ditt program i hello **namn** textruta. Välj **lägga till.**

Efter en kort period vara kan toosee hello programmets konfiguration bladet.

## <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurera hello program för lösenord för enkel inloggning

tooconfigure enkel inloggning för ett program gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för** Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj hello läge **lösenordsbaserade inloggning.**

9.  Ange hello **inloggnings-URL**. Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till. Kontrollera hello inloggning fält är synliga på hello-URL.

10. Tilldela användare toohello program.

11. Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare. Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.

## <a name="assign-a-user-tooan-application-directly"></a>Tilldela en användare tooan program direkt

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

## <a name="assign-an-application-tooa-group-directly"></a>Tilldela en programgrupp tooa direkt

tooassign en eller flera grupper tooan programmet direkt, följ hello stegen nedan:

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

10. Typen i hello **fullständig gruppnamn** av hello-grupp som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.

11. Hovra över hello **grupp** i hello listan tooreveal en **kryssrutan**. Klicka på hello kryssrutan nästa toohello gruppens profil foto eller logotypen tooadd användaren-toohello **valda** lista.

12. **Valfritt:** om du vill ha för**lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här gruppen toohello **valda** lista.

13. När du har valt grupper klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.

14. **Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello grupper som du har valt.

15. Klicka på hello **tilldela** knappen tooassign hello programmet toohello valda grupper.

Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
