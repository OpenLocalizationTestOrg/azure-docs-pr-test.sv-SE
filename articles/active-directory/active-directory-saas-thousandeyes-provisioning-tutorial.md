---
title: "Självstudier: Konfigurera ThousandEyes för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooThousandEyes."
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
ms.openlocfilehash: f31883ab685d0ffcd9a830aa4a7d43c056f5f4cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-thousandeyes-for-automatic-user-provisioning"></a>Självstudier: Konfigurera ThousandEyes för automatisk Användaretablering


hello syftet med den här kursen är tooshow du hello steg du behöver tooperform i ThousandEyes och Azure AD tooautomatically etablera och avetablera användarkonton från Azure AD tooThousandEyes. 

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient
*   En ThousandEyes klient med hello [standardplan](https://www.thousandeyes.com/pricing) eller bättre aktiverat 
*   Ett användarkonto i ThousandEyes med administratörsbehörigheter 

> [!NOTE]
> hello Azure AD-etablering integration förlitar sig på hello [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), som är tillgängliga tooThousandEyes team på hello standardplan eller bättre.

## <a name="assigning-users-toothousandeyes"></a>Tilldela användare tooThousandEyes

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad. 

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour ThousandEyes app. När du valt, kan du tilldela dessa användare tooyour ThousandEyes app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toothousandeyes"></a>Viktiga tips för att tilldela användare tooThousandEyes

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad tooThousandEyes tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   När du tilldelar en användare tooThousandEyes, måste du välja antingen hello **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan för hello tilldelning. Hej **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.


## <a name="configuring-user-provisioning-toothousandeyes"></a>Konfigurera användaretablering tooThousandEyes 

Det här avsnittet hjälper dig att ansluta din Azure AD-tooThousandEyes användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i ThousandEyes baserat på användare och grupper tilldelning i Azure AD.

> [!TIP]
> Du kan också välja tooenabled SAML-baserade enkel inloggning för ThousandEyes, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.


### <a name="configure-automatic-user-account-provisioning-toothousandeyes-in-azure-ad"></a>Konfigurera automatisk användarkonto etablering tooThousandEyes i Azure AD


1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat ThousandEyes för enkel inloggning, söka efter din instans av ThousandEyes hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **ThousandEyes** i hello programgalleriet. Välj ThousandEyes från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av ThousandEyes och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**.

    ![ThousandEyes etablering](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. Under hello **administratörsautentiseringsuppgifter** avsnitt, inkommande hello **hemlighet Token** genereras av din ThousandEyes konto (du kan hitta hello token under kontot ThousandEyes: **säkerhet & Autentisering**). 

    ![ThousandEyes etablering](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour ThousandEyes app. Om hello anslutningen misslyckas, kontrollera kontots ThousandEyes har administratörsbehörigheter och försök steg 5 igen.

7. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och kontrollera hello kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.

8. Klicka på **Spara**. 

9. Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooThousandEyes**.

10. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooThousandEyes. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i ThousandEyes för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

11. tooenable hello Azure AD-etablering tjänsten för ThousandEyes, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt

12. Klicka på **Spara**. 

Den här åtgärden startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooThousandEyes i hello användare och grupper avsnitt. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras.

Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-enterprise-apps-manage-provisioning.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nästa steg

* [Lär dig hur tooreview loggar och få rapporter om etablering aktivitet](active-directory-saas-provisioning-reporting.md)
