---
title: "aaaHow tooassign användare och grupper tooan program | Microsoft Docs"
description: "Tilldela användare toohello toogrant programåtkomst"
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
ms.openlocfilehash: e039a26e4b8f88ad747354859f1071b8f74b6789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-and-groups-tooan-application"></a>Hur tooassign användare och grupper tooan program

Innan användarna kan göra något av hello nedan för ett visst program, behöver du toofirst **tilldela dem toohello program** toogrant dem åtkomst till:

-   Åtkomst till ett program genom **navigera toohello programmets URL direkt** (även kallat SP-initierad inloggning).

-   Åtkomst till ett program med hjälp av hello **användaren åtkomst-URL** på ett program **egenskaper** sida (även kallat IDP-initierad inloggning).

-   Se ett program som visas på sina [programmet åtkomstpanelen](https://myapps.microsoft.com/) eller mobila program.

-   Se ett program som visas på sina [startprogrammet för Office 365](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).

## <a name="methods-tooassign-applications-with-azure-active-directory"></a>Metoder tooassign program med Azure Active Directory 

Det finns 3 sätt som du kan tilldela program med Azure Active Directory:

-   [Tilldela en användare direkt tooan programmet som administratör](#assign-a-user-directly-as-an-administrator)

-   [Tilldela en grupp direkt tooan programmet som administratör](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [Aktivera självbetjäning programmet åtkomst tooallow användare toofind sina egna program](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a>Tilldela en användare direkt som en administratör

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

## <a name="assign-a-group-directly-tooan-application-as-an-administrator"></a>Tilldela en grupp direkt tooan programmet som administratör

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

Efter en kort tidsperiod att hello användare inom hello grupper som du har valt kan toolaunch dessa program med hjälp av hello metoder som beskrivs under hello lösning beskrivning. Om dessa är dynamiska grupper, kan det finnas vissa ytterligare bearbetning fördröjning i dessa tilldelningar visas för användare i de tilldelade grupper.

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a>Aktivera självbetjäning programmet åtkomst tooallow användare toofind sina egna program

Självbetjäning programåtkomst är ett bra sätt tooallow användare tooself-identifiera program, eventuellt tillåter hello business tooapprove toothose program. Du kan tillåta hello business grupp toomanage hello autentiseringsuppgifter tilldelade toothose användare för lösenord enkel inloggning på program direkt från deras åtkomst paneler.

tooenable självbetjäning åtkomst tooan program, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooenable självbetjäning toofrom hello åtkomstlista.

7.  När programmet hello läses in klickar du på **självbetjäning** från hello programmet vänstra navigeringsmenyn.

8.  tooenable självbetjäning programåtkomst för det här programmet stänga hello **toorequest åtkomst toothis program för användarna?** växla för**Ja.**

9.  Klicka sedan tooselect hello grupp toowhich användare som begär åtkomst toothis program bör läggas på hello selector nästa toohello etikett **toowhich grupp ska tilldelas användare läggas?** och välja en grupp.

10. **Valfritt:** om du vill toorequire ett företag godkännande innan användarna får åtkomst måste du ange hello **kräver godkännande innan du beviljar åtkomst toothis program?** växla för**Ja**.

11. **Valfritt: för program som använder enkel inloggning för lösenord på endast** om du vill tooallow dessa företag godkännare toospecify hello lösenord som skickas toothis program för godkända användare måste ange hello **Tillåt godkännare tooset användarens lösenord för det här programmet?**  växla för**Ja**.

12. **Valfritt:** toospecify hello business godkännare som tooapprove åtkomst toothis program tillåts på hello selector nästa toohello etikett **vem som får tooapprove åtkomst toothis program?** tooselect in too10 enskilda företag godkännare.

  >[!NOTE]
  >Grupper stöds inte.
  >
  >

13. **Valfritt:** **för program som exponera roller**, om du inte vill tooassign godkända Självbetjäningsanvändare tooa roll, klicka på nästa hello selector-toohello **toowhich roll ska tilldelas användare i den här programmet?**  tooselect hello rollen toowhich dessa användare ska tilldelas.

14. Klicka på hello **spara** knappen hello överst i hello bladet toofinish.

När du har slutfört självbetjäning programkonfigurationen användare kan navigera tootheir [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på hello **+ Lägg till** knappen toofind hello appar toowhich du har aktiverat Självbetjäning åtkomst. Företag godkännare också se ett meddelande i sina [programmet åtkomstpanelen](https://myapps.microsoft.com/). Du kan aktivera ett e-postmeddelande till dem när en användare har begärt åtkomst tooan program som kräver godkännande. 

Dessa godkännanden stöder godkännandearbetsflöden, vilket innebär att om du anger flera godkännare alla godkännare kan godkännare åtkomst toohello program.

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
