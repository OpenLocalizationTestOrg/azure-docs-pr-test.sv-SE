---
title: "Självstudier: Azure Active Directory-integrering med Salesforce | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: a916be8dbf0b4c6173cda873936a53cd1f3ff12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Salesforce för automatisk Användaretablering

hello syftet med den här kursen är tooshow hello steg krävs tooperform i Salesforce och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooSalesforce.

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient.
*   Du måste ha en giltig klient för Salesforce för arbets- eller Salesforce för utbildning. Du kan använda ett kostnadsfritt utvärderingskonto för antingen service.
*   Ett användarkonto i Salesforce-teamet administratörsbehörigheter.

## <a name="assigning-users-toosalesforce"></a>Tilldela användare tooSalesforce

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Salesforce-app. När du valt, kan du tilldela dessa användare tooyour Salesforce-app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce"></a>Viktiga tips för att tilldela användare tooSalesforce

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad tooSalesforce tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*  Du måste välja en giltig användarroll när du tilldelar en tooSalesforce för användaren. Hej ”standard” rollen fungerar inte för etablering

    > [!NOTE]
    > Den här appen importerar anpassade roller från Salesforce som en del av hello etableringsprocessen, vilken hello-kund vill kanske tooselect när du tilldelar användare

## <a name="enable-automated-user-provisioning"></a>Aktivera automatiserad etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD-tooSalesforce användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Salesforce baserat på tilldelning av användare och grupper i Azure AD .

>[!Tip]
>Du kan också välja tooenabled SAML-baserade enkel inloggning för Salesforce, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatisk användarens konto-etablering:

hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooSalesforce.

1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat Salesforce för enkel inloggning söka efter din instans av Salesforce hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **Salesforce** i hello programgalleriet. Välj Salesforce från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av Salesforce och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**. 
![etablering](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)

5. Under hello **administratörsautentiseringsuppgifter** avsnittet, ange hello följande inställningar:
   
    a. I hello **administratörsanvändarnamnet** textruta typen en Salesforce-kontonamn som har hello **systemadministratören** profil i Salesforce.com som tilldelats.
   
    b. I hello **adminlösenord** textruta typen hello lösenordet för kontot.

6. tooget Salesforce säkerhets-token öppnar en ny flik och logga in på hello samma Salesforce-administratörskonto. Klicka på ditt namn i hello övre högra hörnet av hello-sidan, och klicka sedan på **Mina inställningar**.

     ![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "aktivera automatisk användaretablering")
7. På hello vänstra navigationsfönstret klickar du på **personliga** tooexpand hello Närliggande avsnitt och klickar sedan på **Återställ mina säkerhetstoken**.
  
    ![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "aktivera automatisk användaretablering")
8. På hello **Återställ mina säkerhetstoken** klickar du på **återställa säkerhetstoken** knappen.

    ![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "aktivera automatisk användaretablering")
9. Kontrollera hello inkorg som är associerade med den här administratörskonto. Leta efter ett e-postmeddelande från Salesforce.com som innehåller hello ny säkerhetstoken.
10. Kopiera hello token, gå tooyour Azure AD-fönstret och klistra in den i hello **Socket Token** fältet.

11. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Salesforce-app.

12. I hello **e-postmeddelande** anger hello e-postadressen för en person eller grupp som ska ta emot meddelanden om etablering fel och markera kryssrutan hello nedan.

13. Klicka på **spara.**  
    
14.  Välj under hello mappningar avsnitt, **tooSalesforce synkronisera Azure Active Directory-användare.**

15. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooSalesforce. Observera att hello attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Salesforce för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

16. tooenable hello Azure AD-etablering tjänsten för Salesforce, ändra hello **Status för etablering** för**på** i hello inställningar

17. Klicka på **spara.**

Detta startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooSalesforce i hello användare och grupper avsnitt. Observera att hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras på ditt Salesforce-app.

Du kan nu skapa ett testkonto. Vänta tills in tooverify hello kontot har synkroniserats tooSalesforce too20 i minuter.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-salesforce-tutorial.md)