---
title: "Självstudier: Konfigurera Slack för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooSlack."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: d0a565bbe0bd7e229b9dd99b1ebbaf67d93a2206
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Slack för automatisk Användaretablering


hello syftet med den här kursen är tooshow du hello stegen tooperform i Slack och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooSlack. 

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active Active directory-klient
*   En Slack-klienten med hello [Plus plan](https://aadsyncfabric.slack.com/pricing) eller bättre aktiverat 
*   Ett användarkonto i Slack med administratörsbehörigheter för Team 

Obs: hello Azure AD-etablering integration förlitar sig på hello [Slack SCIM API](https://api.slack.com/scim) som är tillgängliga tooSlack team på hello Plus plan eller bättre.

## <a name="assigning-users-tooslack"></a>Tilldela användare tooSlack

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är kommer bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD att synkroniseras. 

Innan du konfigurerar och aktiverar hello etableras, behöver du toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Slack app. När du valt, kan du tilldela dessa användare tooyour Slack-app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-tooslack"></a>Viktiga tips för att tilldela användare tooSlack

*   Vi rekommenderar att en enda Azure AD-användare tilldelas tooSlack tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   När du tilldelar en användare tooSlack, måste du välja hello **användaren** eller ”grupp” roll i hello tilldelning dialogrutan. Hej ”standard” rollen fungerar inte för etablering.


## <a name="configuring-user-provisioning-tooslack"></a>Konfigurera användaretablering tooSlack 

Det här avsnittet hjälper dig att ansluta din Azure AD-tooSlack användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Slack baserat på tilldelning av användare och grupper i Azure AD.

**Tips:** du kan också välja tooenabled SAML-baserade enkel inloggning för Slack hello instruktionerna som anges i (Azure portal) [https://portal.azure.com]. Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.


### <a name="tooconfigure-automatic-user-account-provisioning-tooslack-in-azure-ad"></a>tooconfigure automatisk användarkonto etablering tooSlack i Azure AD:


1)  I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2) Om du redan har konfigurerat Slack för enkel inloggning, söka efter din instans av Slack hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **Slack** i hello programgalleriet. Välj Slack från hello sökresultaten och lägga till den tooyour listan med program.

3)  Välj din instans av Slack och sedan hello **etablering** fliken.

4)  Ange hello **etablering läge** för**automatisk**.

![Slack-etablering](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera**. En Slack auktorisering dialogruta öppnas i ett nytt webbläsarfönster. 

6) Logga in till Slack med ditt Team administratörskonto i hello nytt fönster. hello resulterande auktorisering dialogrutan Välj hello Slack team som du vill tooenable etablering för och väljer sedan **auktorisera**. När du är klar, returnerar toohello Azure portal toocomplete hello etablering konfiguration.

![Dialogrutan för auktorisering](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Slack app. Om hello anslutningen misslyckas, kontrollera kontots Slack har administratörsbehörigheter för Team och försök hello ”Godkänn” steg igen.

8) Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fält och markera kryssrutan hello nedan.

9) Klicka på **Spara**. 

10) Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooSlack**.

11) I hello **attributmappning** avsnittet kan du granska hello användarattribut som ska synkroniseras från Azure AD tooSlack. Observera att hello attribut som valts som **matchande** egenskaper kommer att använda toomatch hello användarkonton i Slack för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

12) tooenable hello Azure AD-etablering tjänsten för Slack, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt

13) Klicka på **Spara**. 

Detta startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooSlack i hello användare och grupper avsnitt. Observera att hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tionde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello-tjänsten på appen Slack-etablering.

## <a name="optional-configuring-group-object-provisioning-tooslack"></a>[Valfritt] Konfigurera gruppobjekt etablering tooSlack 

Du kan också, aktivera hello etablering av gruppobjekt från Azure AD tooSlack. Detta skiljer sig från ”tilldela grupper av användare”, i det faktiska gruppobjektet hello dessutom tooits medlemmar kommer att replikeras från Azure AD tooSlack. Om du har en grupp med namnet ”min grupp” i Azure AD, till exempel skapas en identitical grupp med namnet ”min grupp” inuti Slack.

### <a name="tooenable-provisioning-of-group-objects"></a>tooenable etablering av gruppobjekt:

1) Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-grupper tooSlack**.

2) Ange aktiverad tooYes hello attributmappning-bladet.

3) I hello **attributmappning** avsnittet kan du granska hello Gruppattribut som synkroniseras från Azure AD tooSlack. Observera att hello attribut som valts som **matchande** egenskaper kommer att använda toomatch hello grupper i Slack för uppdateringsåtgärder. 

4) Klicka på **Spara**.

Det här resultatet i en grupp objekt som tilldelats tooSlack i hello **användare och grupper** avsnittet fullständigt som synkroniseras från Azure AD tooSlack. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello-tjänsten på appen Slack-etablering.


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-enterprise-apps-manage-provisioning.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
