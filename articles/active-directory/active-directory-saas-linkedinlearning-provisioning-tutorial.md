---
title: "Självstudier: Konfigurera LinkedIn Learning för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooLinkedIn Learning."
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
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: e0503e09ab384723ffb73d6ef1df8be6abfc9294
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-linkedin-learning-for-automatic-user-provisioning"></a>Självstudier: Konfigurera LinkedIn utbildning för automatisk Användaretablering


hello syftet med den här kursen är tooshow du hello stegen tooperform i LinkedIn Learning och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooLinkedIn Learning. 

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active Directory-klient
*   En LinkedIn Learning-klient 
*   Ett administratörskontot i LinkedIn Learning med åtkomst toohello LinkedIn Account Center

> [!NOTE]
> Azure Active Directory kan integreras med LinkedIn Learning med hello [SCIM](http://www.simplecloud.info/) protokoll.

## <a name="assigning-users-toolinkedin-learning"></a>Tilldela användare tooLinkedIn utbildning

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är kommer bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD att synkroniseras. 

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vad användare och/eller grupper i Azure AD representerar hello-användare som behöver åtkomst till tooLinkedIn Learning. När du valt, kan du tilldela dessa användare tooLinkedIn Learning genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolinkedin-learning"></a>Viktiga tips för att tilldela användare tooLinkedIn utbildning

*   Vi rekommenderar att en enda Azure AD-användare tilldelas tooLinkedIn Learning tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   När du tilldelar en användare tooLinkedIn Learning, måste du välja hello **användaren** roll i dialogrutan för hello tilldelning. Hej ”standard” rollen fungerar inte för etablering.


## <a name="configuring-user-provisioning-toolinkedin-learning"></a>Konfigurera användaretablering tooLinkedIn utbildning

Det här avsnittet hjälper dig att ansluta din Azure AD tooLinkedIn utbildning SCIM användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i LinkedIn Learning baserat på användare och grupper tilldelning i Azure AD.

> [!TIP]
> Du kan också välja tooenabled SAML-baserade enkel inloggning för LinkedIn, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.


### <a name="tooconfigure-automatic-user-account-provisioning-toolinkedin-learning-in-azure-ad"></a>automatisk tooconfigure användarkontot som etablerar tooLinkedIn Learning i Azure AD:


hello första steget är tooretrieve LinkedIn-åtkomsttoken. Om du är företagsadministratör kan etablera du själv en åtkomst-token. Gå för i din kontocenter**inställningar &gt; globala inställningar** och öppna hello **SCIM installationsprogrammet** panelen.

> [!NOTE]
> Om du öppnar hello kontocenter direkt i stället för via en länk, kan du nå den med hjälp av hello följande steg.

1)  Logga in tooAccount Center.

2)  Välj **Admin &gt; administrationsinställningar** .

3)  Klicka på **avancerade integreringar** på hello vänstra sidopanelen. Du är riktat toohello kontocenter.

4)  Klicka på **+ Lägg till ny SCIM konfiguration** och följa hello genom att fylla i varje fält.

> När autoassign licenser inte är aktiverad, innebär det att endast användardata är synkroniserad.

![LinkedIn Learning etablering](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_1.PNG)

> När autolicense tilldelning har aktiverats måste toonote programinstansen och licenstyp. Licenser är kopplade till en första komma, först hantera bas tills alla hello licenser tas.

![LinkedIn Learning etablering](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_2.PNG)

5)  Klicka på **skapa token**. Du bör se bildskärmen åtkomst-token under hello **åtkomsttoken** fältet.

6)  Spara din dator eller en åtkomst-token tooyour Urklipp innan de lämnar hello-sidan.

7) Logga sedan in toohello [Azure-portalen](https://portal.azure.com), och bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

8) Om du redan har konfigurerat LinkedIn Learning för enkel inloggning, söka efter din instans av LinkedIn Learning hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **LinkedIn Learning** i hello programgalleriet. Välj LinkedIn Learning från hello sökresultaten och lägga till den tooyour listan med program.

9)  Välj din instans av LinkedIn Learning och sedan hello **etablering** fliken.

10) Ange hello **etablering läge** för**automatisk**.

![LinkedIn Learning etablering](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_3.PNG)

11)  Fyll i följande fält under hello **administratörsautentiseringsuppgifter** :

* I hello **klient URL** anger https://api.linkedin.com.

* I hello **hemlighet Token** skriver hello åtkomst-token som du genererade i steg 1 och klicka på **Testanslutningen** .

* Du bör se ett meddelande om lyckade på hello upperright sida av portalen.

12) Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fält och markera kryssrutan hello nedan.

13) Klicka på **Spara**. 

14) I hello **attributmappning** avsnittet kan du granska hello användar- och Gruppattribut som ska synkroniseras från Azure AD tooLinkedIn Learning. Observera att hello attribut som valts som **matchande** egenskaper kommer att använda toomatch hello användarkonton och grupper i LinkedIn utbildning för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

![LinkedIn Learning etablering](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_4.PNG)

15) tooenable hello Azure AD-etablering tjänsten för LinkedIn, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt

16) Klicka på **Spara**. 

Detta startar hello inledande synkronisering av alla användare och/eller grupper som har tilldelats tooLinkedIn Learning under hello användare och grupper. Observera att hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras LinkedIn Learning appen.


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-enterprise-apps-manage-provisioning.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
