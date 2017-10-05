---
title: "Självstudier: Konfigurera LinkedIn höjer för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du konfigurerar Azure Active Directory för att etablera och avetablera användarkonton till LinkedIn höjer automatiskt."
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
ms.openlocfilehash: 526666301aad1e5284c621024649d9cd52c92d18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-linkedin-elevate-for-automatic-user-provisioning"></a>Självstudier: Konfigurera LinkedIn höja för automatisk Användaretablering


Syftet med den här kursen är att visa de steg som du behöver göra i LinkedIn höjer och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD att upphöja LinkedIn. 

## <a name="prerequisites"></a>Krav

Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:

*   En Azure Active Directory-klient
*   En LinkedIn höjer klient 
*   Ett administratörskontot i LinkedIn höjer med åtkomst till LinkedIn Account Center

> [!NOTE]
> Azure Active Directory kan integreras med LinkedIn höjer med hjälp av den [SCIM](http://www.simplecloud.info/) protokoll.

## <a name="assigning-users-to-linkedin-elevate"></a>Tilldela användare till LinkedIn höjer

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar. I samband med automatisk konto användaretablering, kommer endast användare och grupper som har ”tilldelats” till ett program i Azure AD att synkroniseras. 

Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till LinkedIn höjer. När bestämt, kan du tilldela dessa användare till LinkedIn höjer genom att följa anvisningarna här:

[Tilldela en användare eller grupp till en enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-elevate"></a>Viktiga tips för att tilldela användare till LinkedIn höjer

*   Vi rekommenderar att en enda Azure AD-användare tilldelas LinkedIn höjer att testa allokering konfigurationen. Ytterligare användare och/eller grupper kan tilldelas senare.

*   När du tilldelar en användare LinkedIn höjer, måste du välja den **användaren** roll i dialogrutan tilldelning. Rollen ”standard åtkomst” fungerar inte för etablering.


## <a name="configuring-user-provisioning-to-linkedin-elevate"></a>Konfigurering av användarförsörjning att upphöja LinkedIn

Det här avsnittet hjälper dig att ansluta din Azure AD till LinkedIn höjer SCIM användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i LinkedIn höjer baserat på användare och grupptilldelning i Azure AD.

**Tips:** du kan också välja att aktiveras SAML-baserade enkel inloggning för LinkedIn höjer följa instruktionerna i [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-elevate-in-azure-ad"></a>Konfigurera automatisk konto användaretablering att upphöja LinkedIn i Azure AD:


Det första steget är att hämta LinkedIn-åtkomsttoken. Om du är företagsadministratör kan etablera du själv en åtkomst-token. Gå till i din kontocenter **inställningar &gt; globala inställningar** och öppna den **SCIM installationsprogrammet** panelen.

> [!NOTE]
> Om du ansluter till mitt konto direkt i stället för via en länk, kan du nå den med hjälp av följande steg.

1)  Logga in på kontot Center.

2)  Välj **Admin &gt; administrationsinställningar** .

3)  Klicka på **avancerade integreringar** på vänster sidopanelen. Du dirigeras till mitt konto.

4)  Klicka på **+ Lägg till ny SCIM konfiguration** och följer du proceduren genom att fylla i varje fält.

> När autoassign licenser inte är aktiverad, innebär det att endast användardata är synkroniserad.

![LinkedIn höjer etablering](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate1.PNG)

> När autolicense tilldelning har aktiverats måste du Observera programinstansen och licenstypen. Licenser är kopplade till en första komma, först hantera bas tills alla licenser tas.

![LinkedIn höjer etablering](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate2.PNG)

5)  Klicka på **skapa token**. Du bör se din token visas under den **åtkomsttoken** fältet.

6)  Spara åtkomst-token till Urklipp eller en dator innan du lämnar sidan.

7) Logga sedan in till den [Azure-portalen](https://portal.azure.com), och bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.

8) Om du redan har konfigurerat LinkedIn höjer för enkel inloggning, söka efter din instans av LinkedIn höjer med hjälp av sökfältet. Annars väljer **Lägg till** och Sök efter **LinkedIn höjer** i programgalleriet. Välj LinkedIn höjer i sökresultatet och lägga till den i listan med program.

9)  Välj din instans av LinkedIn höjer och sedan den **etablering** fliken.

10) Ange den **Etableringsläge** till **automatisk**.

![LinkedIn höjer etablering](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate3.PNG)

11)  Fyll i följande fält under **administratörsautentiseringsuppgifter** :

* I den **klient URL** anger https://api.linkedin.com.

* I den **hemlighet Token** skriver den åtkomst-token som du genererade i steg 1 och klicka på **Testanslutningen** .

* Du bör se ett meddelande om lyckade på upperright sida av portalen.

12) Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan nedan.

13) Klicka på **Spara**. 

14) I den **attributmappning** avsnittet kan du granska attributen användare och grupper som ska synkroniseras från Azure AD att upphöja LinkedIn. Observera att attribut som är markerade som **matchande** egenskaper som används för att matcha användarkonton och grupper i LinkedIn höjer för uppdateringsåtgärder. Välj knappen Spara för att genomföra ändringarna.

![LinkedIn höjer etablering](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate4.PNG)

15) Om du vill aktivera Azure AD etableras för LinkedIn höjer ändra den **Status för etablering** till **på** i den **inställningar** avsnitt

16) Klicka på **Spara**. 

Detta startar den första synkroniseringen av användare och/eller grupper som tilldelas till LinkedIn höjer i avsnittet användare och grupper. Observera att den första synkroniseringen ta längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs. Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering i appen LinkedIn höjer.


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-enterprise-apps-manage-provisioning.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
