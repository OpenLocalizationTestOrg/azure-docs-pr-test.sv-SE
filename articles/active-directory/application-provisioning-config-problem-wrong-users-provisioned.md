---
title: "aaaWrong uppsättning användare som etablerade tooan Azure AD-galleriet program | Microsoft Docs"
description: "Lär dig hur toofind varför en annan uppsättning användare som är etablerad tooan program än de som du förväntade dig"
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
ms.openlocfilehash: adb90b12a53fb3160ce2b73b2559df92b283438e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="wrong-set-of-users-are-being-provisioned-tooan-azure-ad-gallery-application"></a>Fel uppsättning användare som etablerade tooan Azure AD-galleriet program

Vilka användare etablerade toohello app är främst drivs av vilka användare och grupper har **tilldelade** toohello program.

Använd hello resurserna nedan toolearn hur toocheck vilka användare och grupper som har tilldelats tooan program i Azure Active Directory.

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

Om etablering har konfigurerats och körs redan för en app kan nya användare ska vara etablerade tooan programmet i cirka 10 minuter. Kontrollera hello **granskningsloggar** mer information.

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

Om etablering har konfigurerats och körs redan för en app kan nya användare som ingår i hello grupp ska vara etablerade tooan programmet i cirka 10 minuter. Kontrollera hello **granskningsloggar** mer information.

>[!IMPORTANT]
>Etablering av hello gruppnamn och gruppera informationen i tillägget toohello medlemmar om stöd för vissa program. Du kan aktivera eller inaktivera den här funktionen genom att aktivera eller inaktivera hello **mappning** för gruppobjekt visas i hello **etablering** fliken. 
>
>

Om etablering grupper är aktiverat, måste tooreview hello attributet mappningar tooensure ett lämpligt fält som används för hello ”matchande ID”. Detta kan vara hello namn eller e-post alias, som hello grupp och dess medlemmar inte etableras om hello matchning av egenskap är tomt eller inte fyllts i för en grupp i Azure AD.

## <a name="next-steps"></a>Nästa steg
[Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](active-directory-saas-app-provisioning.md)
