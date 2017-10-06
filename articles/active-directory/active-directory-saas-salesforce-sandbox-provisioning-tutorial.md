---
title: "Självstudier: Azure Active Directory-integrering med Salesforce Sandbox | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Salesforce Sandbox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 06ff50050845383a602b0edd6fca953ddd37cebd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-sandbox-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Salesforce Sandbox för automatisk Användaretablering

hello syftet med den här kursen är tooshow du hello stegen tooperform i Salesforce Sandbox och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooSalesforce Sandbox.

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient.
*   Du måste ha en giltig klient för Salesforce Sandbox för arbete eller Salesforce Sandbox för utbildning. Du kan använda ett kostnadsfritt utvärderingskonto för antingen service.
*   Ett användarkonto i Salesforce Sandbox Team administratörsbehörigheter.

## <a name="assigning-users-toosalesforce-sandbox"></a>Tilldela användare tooSalesforce Sandbox

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserade.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour Salesforce Sandbox app. När du valt, kan du tilldela dessa användare tooyour Salesforce Sandbox appen genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce-sandbox"></a>Viktiga tips för att tilldela användare tooSalesforce Sandbox

* Vi rekommenderar att en enda Azure AD-användare är tilldelad tooSalesforce Sandbox tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

* Du måste välja en giltig användarroll när du tilldelar en användare tooSalesforce Sandbox. Hej ”standard” rollen fungerar inte för etablering.

> [!NOTE]
> Den här appen importerar anpassade roller från Salesforce Sandbox som en del av hello etableringsprocessen, vilken hello-kund vill kanske tooselect när du tilldelar användare.

## <a name="enable-automated-user-provisioning"></a>Aktivera automatiserad etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD tooSalesforce Sandbox användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelad användare konton i Salesforce Sandbox baserat på användare och grupper tilldelning i Azure AD.

>[!Tip]
>Du kan också välja tooenabled SAML-baserade enkel inloggning för Salesforce begränsat läge, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatisk användarens konto-etablering:

hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooSalesforce Sandbox.

1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat Salesforce Sandbox för enkel inloggning, söka efter din instans av Salesforce Sandbox hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **Salesforce Sandbox** i hello programgalleriet. Välj Salesforce Sandbox från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av Salesforce Sandbox och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**. 
    ![etablering](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)

5. Under hello **administratörsautentiseringsuppgifter** avsnittet, ange hello följande inställningar:
   
    a. I hello **administratörsanvändarnamnet** textruta typen en Salesforce Sandbox kontonamn som har hello **systemadministratören** profil i Salesforce.com som tilldelats.
   
    b. I hello **adminlösenord** textruta typen hello lösenordet för kontot.

6. tooget Salesforce Sandbox säkerhets-token öppnar en ny flik och logga in på hello samma Salesforce Sandbox-administratörskonto. Klicka på ditt namn i hello övre högra hörnet av hello-sidan, och klicka sedan på **Mina inställningar**.

     ![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "aktivera automatisk användaretablering")
7. På hello vänstra navigationsfönstret klickar du på **personliga** tooexpand hello Närliggande avsnitt och klickar sedan på **Återställ mina säkerhetstoken**.
  
    ![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "aktivera automatisk användaretablering")
8. På hello **Återställ mina säkerhetstoken** klickar du på hello **återställa säkerhetstoken** knappen.

    ![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "aktivera automatisk användaretablering")
9. Kontrollera hello inkorg som är associerade med den här administratörskonto. Leta efter ett e-postmeddelande från Salesforce-Sandbox.com som innehåller hello ny säkerhetstoken.
10. Kopiera hello token, gå tooyour Azure AD-fönstret och klistra in den i hello **Socket Token** fältet.

11. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Salesforce Sandbox app.

12. I hello **e-postmeddelande** anger hello e-postadressen för en person eller grupp som ska ta emot meddelanden om etablering fel och hello kryssrutan.

13. Klicka på **spara.**  
    
14.  Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooSalesforce Sandbox.**

15. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooSalesforce Sandbox. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Salesforce Sandbox för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

16. tooenable hello Azure AD-etablering tjänsten för Salesforce begränsat läge, ändra hello **Status för etablering** för**på** i hello inställningar

17. Klicka på **spara.**


Den startar hello inledande synkronisering av alla användare och/eller grupper som har tilldelats tooSalesforce Sandbox under hello användare och grupper. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello-tjänsten på Salesforce Sandbox app-etablering.

Du kan nu skapa ett testkonto. Vänta tills in tooverify hello kontot har synkroniserats toosalesforce too20 i minuter.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-salesforcesandbox-tutorial.md)