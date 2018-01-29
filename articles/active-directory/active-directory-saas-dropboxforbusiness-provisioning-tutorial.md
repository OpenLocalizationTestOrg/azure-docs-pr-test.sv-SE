---
title: "Självstudier: Azure Active Directory-integrering med Dropbox för företag | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Dropbox för företag."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: c41760d60d53dee7be36b2af287cd6755605b708
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Dropbox för företag för automatisk Användaretablering

Syftet med den här kursen är att visa de steg som du behöver göra i Dropbox för företag och Azure AD att automatiskt etablera och avetablera användarkonton från Azure AD till Dropbox för företag.

## <a name="prerequisites"></a>Krav

Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:

*   En Azure Active directory-klient.
*   En Dropbox för Business enkel inloggning för aktiverade prenumerationen.
*   Ett användarkonto i Dropbox för företag med Team administratörsbehörigheter.

## <a name="assigning-users-to-dropbox-for-business"></a>Tilldela användare till Dropbox för företag

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar. I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.

Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till din Dropbox för företag. När bestämt, kan du tilldela dessa användare till din Dropbox för företag genom att följa anvisningarna här:

[Tilldela en användare eller grupp till en enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>Viktiga tips för att tilldela användare till Dropbox för företag

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad till Dropbox för företag att testa allokering konfigurationen. Ytterligare användare och/eller grupper kan tilldelas senare.

*   Du måste välja en giltig användarroll när du tilldelar en användare till Dropbox för företag. Rollen ”standard åtkomst” fungerar inte för att etablera...

## <a name="enable-automated-user-provisioning"></a>Aktivera automatiserad etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD till Dropbox för företagets användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i Dropbox för företag baserat på tilldelning av användare och grupper i Azure AD.

>[!Tip]
>Du kan också välja att aktivera SAML-baserade enkel inloggning för Dropbox för företag, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="to-configure-automatic-user-account-provisioning"></a>Konfigurera automatisk användarens konto-etablering:

1. I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat Dropbox för företag för enkel inloggning, söka efter din instans av Dropbox för företag med hjälp av sökfältet. Annars väljer **Lägg till** och Sök efter **Dropbox för företag** i programgalleriet. Välj Dropbox för företag i sökresultatet och lägga till den i listan med program.

3. Välj din instans av Dropbox för företag och sedan den **etablering** fliken.

4. Ange den **Etableringsläge** till **automatisk**. 

    ![Etablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. Under den **administratörsautentiseringsuppgifter** klickar du på **auktorisera**. En Dropbox för Business inloggningsruta öppnas i ett nytt webbläsarfönster.

6. På den **inloggning till Dropbox att länka med Azure AD** dialogrutan Logga in på din Dropbox för företag-klient.

     ![Användaretablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "användaretablering")

7. Bekräfta att du vill ge Azure Active Directory behörighet att göra ändringar i din Dropbox för företag-klient. Klicka på **Tillåt**.
    
      ![Användaretablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "användaretablering")

8. I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din Dropbox för företag. Om anslutningen misslyckas, kontrollera din Dropbox för företag-konto som har administratörsbehörigheter för teamet och försök den **”Godkänn”** steg igen.

9. Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan.

10. Klicka på **spara.**

11. Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare till Dropbox för företag.**

12. I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till Dropbox för företag. De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i Dropbox för företag för uppdateringsåtgärder. Välj knappen Spara för att genomföra ändringarna.

13. Aktivera Azure AD etableras för Dropbox för företag att ändra den **Status för etablering** till **på** i avsnittet Inställningar

14. Klicka på **spara.**

Startar den första synkroniseringen av användare och/eller grupper som tilldelas till Dropbox för företag i avsnittet användare och grupper. Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs. Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av etablering tjänsten i din Dropbox för företag.

Du kan nu skapa ett testkonto. Vänta i upp till 20 minuter för att verifiera att kontot har synkroniserats till Dropbox för företag.

En användare som har slutförts etablering cykel visas med status relaterade.

![Tilldela användare](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "tilldela användare")


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-dropboxforbusiness-tutorial.md)