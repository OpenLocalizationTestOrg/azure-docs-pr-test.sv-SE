---
title: "Självstudier: Konfigurera LucidChart för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooLucidChart."
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
ms.openlocfilehash: d3af45141731215f2edc8942ad21b016468c1e38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a>Självstudier: Konfigurera LucidChart för automatisk Användaretablering


hello syftet med den här kursen är tooshow du hello stegen tooperform i LucidChart och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooLucidChart. 

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient
*   En LucidChart klient med hello [företagsplan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) eller bättre aktiverat 
*   Ett användarkonto i LucidChart med administratörsbehörigheter 

## <a name="assigning-users-toolucidchart"></a>Tilldela användare tooLucidChart

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad. 

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour LucidChart app. När du valt, kan du tilldela dessa användare tooyour LucidChart app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolucidchart"></a>Viktiga tips för att tilldela användare tooLucidChart

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad tooLucidChart tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   När du tilldelar en användare tooLucidChart, måste du välja antingen hello **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan för hello tilldelning. Hej **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.


## <a name="configuring-user-provisioning-toolucidchart"></a>Konfigurera användaretablering tooLucidChart 

Det här avsnittet hjälper dig att ansluta din Azure AD-tooLucidChart användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i LucidChart baserat på tilldelning av användare och grupper i Azure AD .

> [!TIP]
> Du kan också välja tooenabled SAML-baserade enkel inloggning för LucidChart, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.


### <a name="configure-automatic-user-account-provisioning-toolucidchart-in-azure-ad"></a>Konfigurera automatisk användarkonto etablering tooLucidChart i Azure AD


1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat LucidChart för enkel inloggning, söka efter din instans av LucidChart hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **LucidChart** i hello programgalleriet. Välj LucidChart från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av LucidChart och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**.

    ![LucidChart etablering](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. Under hello **administratörsautentiseringsuppgifter** avsnitt, inkommande hello **hemlighet Token** genereras av din LucidChart konto (du kan hitta hello token under kontot: **Team**  >  **Appintegrering** > **SCIM**). 

    ![LucidChart etablering](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour LucidChart app. Om hello anslutningen misslyckas, kontrollera kontots LucidChart har administratörsbehörigheter och försök steg 5 igen.

7. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och kontrollera hello kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.

8. Klicka på **Spara**. 

9. Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooLucidChart**.

10. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooLucidChart. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i LucidChart för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

11. tooenable hello Azure AD-etablering tjänsten för LucidChart, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt

12. Klicka på **Spara**. 

Den här åtgärden startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooLucidChart i hello användare och grupper avsnitt. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras.

Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-enterprise-apps-manage-provisioning.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nästa steg

* [Lär dig hur tooreview loggar och få rapporter om etablering aktivitet](active-directory-saas-provisioning-reporting.md)
