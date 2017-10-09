---
title: "Självstudier: Konfigurera GitHub för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooGitHub."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: c1f0f7a42e4f8a94db3f409cd463e13bb1bc13bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a>Självstudier: Konfigurera GitHub för automatisk Användaretablering


hello syftet med den här kursen är tooshow du hello stegen tooperform i GitHub och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooGitHub. 

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient
*   En Github-klient med hello [företagsplan](https://help.github.com/articles/organization-billing-plans/#business-plan) eller bättre aktiverat 
*   Ett användarkonto i GitHub med administratörsbehörigheter 

> [!NOTE]
> hello Azure AD-etablering integration förlitar sig på hello [GitHub SCIM API](https://developer.github.com/v3/scim/), som är tillgängliga tooGithub team på hello företagsplan eller bättre.

## <a name="assigning-users-toogithub"></a>Tilldela användare tooGitHub

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad. 

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour GitHub app. När du valt, kan du tilldela dessa användare tooyour GitHub-app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toogithub"></a>Viktiga tips för att tilldela användare tooGitHub

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad tooGitHub tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   När du tilldelar en användare tooGitHub, måste du välja antingen hello **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan för hello tilldelning. Hej **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.


## <a name="configuring-user-provisioning-toogithub"></a>Konfigurera användaretablering tooGitHub 

Det här avsnittet hjälper dig att ansluta din Azure AD-tooGitHub användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i GitHub baserat på tilldelning av användare och grupper i Azure AD.

> [!TIP]
> Du kan också välja tooenabled SAML-baserade enkel inloggning för GitHub, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.


### <a name="configure-automatic-user-account-provisioning-toogithub-in-azure-ad"></a>Konfigurera automatisk användarkonto etablering tooGitHub i Azure AD


1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat GitHub för enkel inloggning söka efter din instans av GitHub hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **GitHub** i hello programgalleriet. Välj GitHub från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av GitHub och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**.

    ![GitHub-etablering](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera**. Den här åtgärden öppnar en dialogruta för GitHub-auktorisering i ett nytt webbläsarfönster. 

6. Logga in på GitHub med ditt administratörskonto i hello nytt fönster. I hello resulterande auktorisering dialogrutan väljer hello GitHub team som du vill tooenable etablering för och väljer sedan **auktorisera**. När du är klar, returnerar toohello Azure portal toocomplete hello etablering konfiguration.

    ![Dialogrutan för auktorisering](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. I hello Azure-portalen, ange **klient URL** och klicka på **Testanslutningen** tooensure Azure AD kan ansluta tooyour GitHub app. Om hello anslutningen misslyckas, kan du kontrollera att ditt GitHub-konto som har administratörsbehörigheter och **klient URl** inputted är korrekt och försök hello ”verifiera” steg igen (du kan utgöra **klient URL** av regel: ”https : //api.github.com/scim/v2/organizations/ + < Organizations_name > ”, du kan hitta din organisationer under GitHub-konto: **inställningar** > **organisationer**).

    ![Dialogrutan för auktorisering](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och kontrollera hello kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.

9. Klicka på **Spara**. 

10. Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooGitHub**.

11. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooGitHub. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i GitHub för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

12. tooenable hello Azure AD-etablering tjänsten för GitHub, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt

13. Klicka på **Spara**. 

Den här åtgärden startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooGitHub i hello användare och grupper avsnitt. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras.

Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-enterprise-apps-manage-provisioning.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nästa steg

* [Lär dig hur tooreview loggar och få rapporter om etablering aktivitet](active-directory-saas-provisioning-reporting.md)
