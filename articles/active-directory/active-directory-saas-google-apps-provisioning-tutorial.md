---
title: "Självstudier: Konfigurera Google Apps för automatisk användaretablering i Azure | Microsoft Docs"
description: "Lär dig hur tooautomatically etablera och avinstallation etablera användarkontona från Azure AD tooGoogle appar."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: d1fa8449bd6013d1627b3552aaa19db1c0f4f46f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Google Apps för automatisk användaretablering

hello syftet med den här kursen är tooshow du hello steg du behöver tooperform i Google Apps och Azure AD tooautomatically etablera och avetablera användarkonton från Azure AD tooGoogle appar.

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient.
*   Du måste ha en giltig klient för Google Apps för arbets- eller Google Apps för utbildning. Du kan använda ett kostnadsfritt utvärderingskonto för antingen service.
*   Ett användarkonto i Google Apps-teamet administratörsbehörigheter.

## <a name="assigning-users-toogoogle-apps"></a>Tilldela användare tooGoogle appar

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour Google Apps app. När du valt, kan du tilldela dessa användare tooyour Google Apps-app genom att följa hello anvisningarna här: [tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

> [!IMPORTANT]
>*   Vi rekommenderar att en enda Azure AD-användare tilldelas tooGoogle appar tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.
>*   När du tilldelar en användare tooGoogle appar kan välja du hello användaren eller rollen ”grupp” i dialogrutan för tilldelning av hello. Hej ”standard” rollen fungerar inte för etablering.

## <a name="enable-automated-user-provisioning"></a>Aktivera automatisk användaretablering

Det här avsnittet hjälper dig att ansluta dina Azure AD tooGoogle appar användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Google Apps baserat på tilldelning av användare och grupper i Azure AD .

>[!Tip]
>Du kan också välja tooenabled SAML-baserade enkel inloggning för Google Apps följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurera automatisk användarens konto-etablering

> [!NOTE]
> En annan genomförbart alternativ för att automatisera användaretablering tooGoogle appar är toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) som etablerar din lokala Active Directory identiteter tooGoogle appar. Däremot etablerar hello lösning i den här självstudiekursen Azure Active Directory (moln) användare och e-postaktiverade grupper tooGoogle appar. 

1. Logga in på hello [Google Apps-administrationskonsolen](http://admin.google.com/) med ditt administratörskonto och på **säkerhet**. Om du inte ser hello länken, det kan vara dolt under hello **fler kontroller** menyn längst hello hello-skärmen.
   
    ![Klicka på Security.][10]

2. På hello **säkerhet** klickar du på **API-referens för**.
   
    ![Klicka på API-referens.][15]

3. Välj **aktivera API-åtkomst**.
   
    ![Klicka på API-referens.][16]

    > [!IMPORTANT]
    > För varje användare som du avser tooprovision tooGoogle appar, sitt lösenord i Azure Active Directory *måste* vara bundet tooa anpassade domäner. Till exempel användarnamn som ser ut som bob@contoso.onmicrosoft.com accepteras inte av Google Apps medan bob@contoso.com accepteras. Du kan ändra en befintlig användares domän genom att redigera deras egenskaper i Azure AD. Instruktioner för hur tooset en anpassad domän för både Azure Active Directory och Google Apps ingår i följande steg.
      
4. Om du inte har lagt till en anpassad domän namn tooyour Azure Active Directory ännu, så du hello följande steg:
  
    a. I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**. Välj din katalog i hello directory lista. 

    b. Klicka på **domäner namnet** på hello vänstra navigeringsfönstret och klicka sedan på **Lägg till**.
     
     ![Domän](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![Lägg till domän](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Skriv ditt domännamn i hello **domännamn** fältet. Det här domännamnet måste vara hello samma domännamn som du avser toouse för Google Apps. När du är klar klickar du på hello **Lägg till domän** knappen.
     
     ![Domännamn](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Klicka på **nästa** toogo toohello bekräftelsesidan. tooverify som du äger den här domänen måste du redigera hello domänens DNS-poster enligt toohello värden på den här sidan. Du kan välja att använda antingen tooverify **MX-poster** eller **TXT-poster**, beroende på vad du väljer för hello **posttyp** alternativet. Mer omfattande instruktioner för hur tooverify domännamn med Azure AD [lägga till egna domänen namnet tooAzure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domän](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Upprepa föregående steg för alla hello-domäner som du avser tooadd tooyour directory hello.

5. Nu när du har verifierat dina domäner med Azure AD, måste du nu kontrollera dem igen med Google Apps. Utför hello följande steg för varje domän som inte redan har registrerats med Google Apps:
   
    a. I hello [Google Apps-administrationskonsolen](http://admin.google.com/), klickar du på **domäner**.
     
     ![Klicka på domäner][20]

    b. Klicka på **lägga till en domän eller en domän alias**.
     
     ![Lägg till en ny domän][21]

    c. Välj **lägga till en annan domän**, och Skriv hello namn i hello-domän som du vill att tooadd.
     
     ![Ange ditt domännamn][22]

    d. Klicka på **Fortsätt och verifiera domänen ägarskap**. Följ sedan hello steg tooverify att du äger hello domännamn. Omfattande anvisningar för hur tooverify domänen med Google Apps finns i. [Verifiera ditt platsägarskap med Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Upprepa föregående steg för eventuella ytterligare domäner som du avser tooadd tooGoogle appar hello.
     
     > [!WARNING]
     > Om du ändrar hello primär domän för Google Apps klienten, och om du redan har konfigurerat enkel inloggning med Azure AD och du har toorepeat steg #3 under [steg två: Aktivera enkel inloggning](#step-two-enable-single-sign-on).
       
6. I hello [Google Apps-administrationskonsolen](http://admin.google.com/), klickar du på **administratörsroller**.
   
     ![Klicka på Google Apps][26]

7. Bestämma vilka administrativa konto som du vill att toouse toomanage användaretablering. För hello **administratörsroll** på det kontot, redigera hello **privilegier** för rollen. Se till att alla hello **API administratörsrättigheter** aktiverade så att det här kontot kan användas för att etablera.
   
     ![Klicka på Google Apps][27]
   
    > [!NOTE]
    > Om du konfigurerar en produktionsmiljö är bra för hello toocreate ett administratörskonto i Google Apps specifikt för det här steget. Dessa konton måste ha en administratörsroll som är associerade med den som har behörighet som krävs hello API.
     
8. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

9. Om du redan har konfigurerat Google Apps för enkel inloggning söka efter din instans av Google-appar som använder hello sökfältet. Annars väljer **Lägg till** och Sök efter **Google Apps** i hello programgalleriet. Välj Google Apps från hello sökresultaten och lägga till den tooyour listan med program.

10. Välj din instans av Google Apps och sedan hello **etablering** fliken.

11. Ange hello **etablering läge** för**automatisk**. 

     ![Etablering](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera**. En dialogruta för auktorisering av Google Apps öppnas i ett nytt webbläsarfönster.

13. Bekräfta att du vill ha toogive Azure Active Directory toomake behörighetsändringar tooyour Google Apps-klient. Klicka på **acceptera**.
    
     ![Kontrollera behörigheter.][28]

14. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Google Apps app. Om hello anslutningen misslyckas, kontrollera ditt Google Apps-konto som har administratörsbehörigheter för Team och försöker hello **”Godkänn”** steg igen.

15. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.

16. Klicka på **spara.**

17. Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooGoogle appar.**

18. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooGoogle appar. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Google Apps för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

19. tooenable hello Azure AD-etablering tjänsten för Google Apps kan ändra hello **Status för etablering** för**på** i hello inställningar

20. Klicka på **spara.**

Den startar hello inledande synkronisering av alla användare och/eller grupper som har tilldelats tooGoogle appar under hello användare och grupper. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras på Google Apps-app.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-auth.png