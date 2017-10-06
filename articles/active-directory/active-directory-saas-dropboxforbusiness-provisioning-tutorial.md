---
title: "Självstudier: Azure Active Directory-integrering med Dropbox för företag | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Dropbox för företag."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 0fb01eab4f7c6c4516eac64a4343e46ea221f98d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Dropbox för företag för automatisk Användaretablering

hello syftet med den här kursen är tooshow du hello stegen tooperform i Dropbox för företag och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooDropbox för företag.

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient.
*   En Dropbox för Business enkel inloggning för aktiverade prenumerationen.
*   Ett användarkonto i Dropbox för företag med Team administratörsbehörigheter.

## <a name="assigning-users-toodropbox-for-business"></a>Tilldela användare tooDropbox för företag

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour Dropbox för företag. När du valt, kan du tilldela dessa användare tooyour Dropbox för företag genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodropbox-for-business"></a>Viktiga tips för att tilldela användare tooDropbox för företag

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad tooDropbox för Business tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   Du måste välja en giltig användarroll när du tilldelar en användare tooDropbox för företag. Hej ”standard” rollen fungerar inte för att etablera...

## <a name="enable-automated-user-provisioning"></a>Aktivera automatiserad etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD-tooDropbox för företagets användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Dropbox för företag baserat på användare och grupper tilldelning i Azure AD.

>[!Tip]
>Du kan också välja tooenabled SAML-baserade enkel inloggning för Dropbox för företag, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatisk användarens konto-etablering:

1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat Dropbox för företag för enkel inloggning söka efter din instans av Dropbox för företag med hello sökfältet. Annars väljer **Lägg till** och Sök efter **Dropbox för företag** i hello programgalleriet. Välj Dropbox för företag från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av Dropbox för företag och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**. 

    ![Etablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera**. En Dropbox för Business inloggningsruta öppnas i ett nytt webbläsarfönster.

6. På hello **inloggning tooDropbox toolink med Azure AD** dialogrutan Logga in tooyour Dropbox för företag-klient.

     ![Användaretablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "användaretablering")

7. Bekräfta att du vill att toogive Azure Active Directory behörighet toomake ändringar tooyour Dropbox för företag-klient. Klicka på **Tillåt**.
    
      ![Användaretablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "användaretablering")

8. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Dropbox för företag. Om hello anslutningen misslyckas, kontrollera din Dropbox för företag-konto som har administratörsbehörigheter för teamet och försök hello **”Godkänn”** steg igen.

9. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.

10. Klicka på **spara.**

11. Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooDropbox för företag.**

12. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooDropbox för företag. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Dropbox för företag för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

13. tooenable hello Azure AD-etablering tjänsten för Dropbox för företag, ändra hello **Status för etablering** för**på** i hello inställningar

14. Klicka på **spara.**

Den startar hello den första synkroniseringen av användare och/eller grupper som tilldelas tooDropbox för företag i hello användare och grupper avsnitt. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello för företag-tjänsten på din Dropbox-etablering.

Du kan nu skapa ett testkonto. Vänta tills in too20 minuter tooverify hello kontot har synkroniserats tooDropbox för företag.

En användare som har slutförts etablering cykel visas med status relaterade.

![Tilldela användare](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "tilldela användare")


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-dropboxforbusiness-tutorial.md)