---
title: "aaaHow tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet | Microsoft Docs"
description: "Hur tooconfigure ett program för säkra lösenordsbaserade enkel inloggning när det finns redan i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 7a93bff119b477d946368686fc2d9006ca2722a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet

När du lägger till ett program från hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), du har hello väljer du hur dina användare toosign i toothat program. Du kan konfigurera detta val när som helst genom att välja hello **enkel inloggning** navigeringsobjektet på ett affärsprogram i hello [Azure Portal](https://portal.azure.com/).

En av hello enkel inloggning metoder tillgängliga tooyou är hello [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) alternativet. Detta är ett bra sätt tooget igång snabbt integrera program i Azure AD, och du kan:

-   Aktivera **enkel inloggning för användarnas** genom att lagra och spela upp användarnamn och lösenord för hello programmet på ett säkert sätt som du har integrerat med Azure AD

-   **Stöd för program som kräver flera inloggning fält** för program som kräver mer än bara användarnamn och lösenord fält toosign i

-   **Anpassa hello etiketter** av hello användarnamn och lösenord indatafält användarna ser på hello [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) när de anger sina autentiseringsuppgifter

-   Tillåt din **användare** tooprovide användarnamn och lösenord för alla program-konton som de skriver manuellt på hello [åtkomstpanelen för programmet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Tillåt en **tillhör hello affärsgrupp** toospecify hello användarnamn och lösenord tilldelad tooa användare med hjälp av hello [Self-Service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funktion

-   Tillåt en **administratör** toospecify hello användarnamn och lösenord som tilldelats tooa användare med hjälp av referenser för uppdatering av hello funktionen när [tilldela en tooan-användarprogram](#assign-a-user-to-an-application-directly)

-   Tillåt en **administratör** toospecify hello delade användarnamnet eller lösenordet som används av en grupp människor med hjälp av autentiseringsuppgifter för uppdatering av hello funktionen när [tilldela en grupp tooan program](#assign-an-application-to-a-group-directly)

Nedan beskrivs hur du kan aktivera [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) tooan program som redan hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

## <a name="overview-of-steps-required"></a>Översikt över steg som krävs
tooconfigure ett program från hello Azure AD-galleriet måste du:

-   [Lägga till ett program från hello Azure AD-galleriet](#add-an-application-from-the-azure-ad-gallery)

-   [Konfigurera hello program för lösenord för enkel inloggning](#configure-the-application-for-password-single-sign-on)

-   [Tilldela hello programmet tooa användare eller grupp](#assign-the-application-to-a-user-or-a-group)

    -   [Tilldela en användare tooan program direkt](#assign-a-user-to-an-application-directly)

    -   [Tilldela en programgrupp tooa direkt](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-hello-azure-ad-gallery"></a>Lägga till ett program från hello Azure AD-galleriet

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

## <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurera hello program för lösenord för enkel inloggning

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

9.  [Tilldela användare toohello program](#assign-a-user-to-an-application-directly).

10. Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare. Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.

## <a name="assign-a-user-tooan-application-directly"></a>Tilldela en användare tooan program direkt

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

## <a name="assign-an-application-tooa-group-directly"></a>Tilldela en programgrupp tooa direkt

tooassign en eller flera grupper tooan programmet direkt, följ hello stegen nedan:

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

10. Typen i hello **fullständig gruppnamn** av hello-grupp som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.

11. Hovra över hello **grupp** i hello listan tooreveal en **kryssrutan**. Klicka på hello kryssrutan nästa toohello gruppens profil foto eller logotypen tooadd användaren-toohello **valda** lista.

12. **Valfritt:** om du vill ha för**lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här gruppen toohello **valda** lista.

13. När du har valt grupper klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.

14. **Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello grupper som du har valt.

15. Klicka på hello **tilldela** knappen tooassign hello programmet toohello valda grupper.

Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
