---
title: "Självstudier: Azure Active Directory-integrering med Salesforce Sandbox | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Salesforce Sandbox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7539f08356568a17ebfcee2764bbbefa129b0553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Självstudier: Azure Active Directory-integrering med Salesforce Sandbox

I kursen får du lära dig hur toointegrate Salesforce Sandbox med Azure Active Directory (AD Azure).

Integrera Salesforce Sandbox med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSalesforce Sandbox
- Du kan aktivera din användare tooautomatically get inloggade tooSalesforce begränsat läge (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Salesforce Sandbox måste hello följande objekt:

- En Azure AD-prenumeration
- En Salesforce Sandbox enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Salesforce Sandbox från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-salesforce-sandbox-from-hello-gallery"></a>Att lägga till Salesforce Sandbox från hello-galleriet
tooconfigure hello integrering av Salesforce Sandbox i Azure AD, behöver du tooadd Salesforce Sandbox hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Salesforce Sandbox från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Salesforce Sandbox**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. Markera hello resultat på panelen **Salesforce Sandbox**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Salesforce Sandbox baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Salesforce Sandbox är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Salesforce Sandbox toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Salesforce Sandbox.

tooconfigure och testa Azure AD enkel inloggning med Salesforce Sandbox, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Salesforce Sandbox](#creating-a-salesforce-sandbox-test-user)**  -toohave en motsvarighet för Britta Simon i Salesforce Sandbox som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Salesforce-Sandbox-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Salesforce Sandbox:**

1. I hello Azure-portalen på hello **Salesforce Sandbox** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. På hello **Salesforce Sandbox domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<subdomain>.my.salesforce.com`

    > [!NOTE] 
    > Det här värdet är inte hello verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [Salesforce Sandbox klienten supportteamet](https://help.salesforce.com/support) tooget det här värdet.


4. Om du redan har konfigurerat enkel inloggning för en annan Salesforce Sandbox-instans i katalogen, så du måste också konfigurera hello **identifierare** toohave hello samma värde som hello **inloggning URL**. 
    
    >[!Note]
    >Hej **identifierare** fältet finns genom att kontrollera hello **visa avancerade inställningar** kryssruta på hello **konfigurera App-URL** sidan för hello dialog 


5. På hello **SAML-signeringscertifikat** klickar du på **certifikat** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. På hello **Salesforce Sandbox Configuration** klickar du på **konfigurera Salesforce Sandbox** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS>
8. Öppna en ny flik i webbläsaren och logga in tooyour Salesforce Sandbox-administratörskonto.

9. Hello-menyn överst hello **installationsprogrammet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. Hello navigeringsfönstret hello vänster, klicka på **säkerhetsåtgärder**, och klicka sedan på **inställningar för enkel inloggning**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. På hello inställningar för enkel inloggning, utför du följande hello: ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)
     
     a.  Välj **SAML aktiverat**. 

     b.  Klicka på **Ny**.

12. Utför följande hello på hello SAML enkel inloggning inställningar:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    a.In hello textrutan, hello-typnamn för hello-konfiguration (t.ex.: *SPSSOWAAD\_Test*). 

    b. Klistra in **SMAL enhets-ID** värdet till hello **utfärdaren** textruta.

    c. I hello **enhets-Id** textruta typen **https://test.salesforce.com** om det är hello första Salesforce Sandbox-instans som du lägger till tooyour directory. Om du redan har lagt till en instans av Salesforce begränsat, och därefter för hello **enhets-ID** typen i hello **logga URL**, som ska vara i formatet:`http://company.my.salesforce.com`  
 
    d. Klicka på **Bläddra** tooupload hello hämtat certifikat.  

    e. Som **SAML identitetstyp**väljer **Assertion innehåller hello Federation ID från hello användarobjektet**.
 
    f. Som **SAML identitet plats**väljer **identitet är i hello NameIdentifier elementet i hello ämne instruktionen**.

    g. Klistra in **inloggning tjänst-URL för enkel** till hello **identitet providern inloggnings-URL** textruta. 

    h. SFDC har inte stöd för SAML logga ut.  Som en tillfällig lösning kan du klistra in 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' till hello **identitet providern logga ut URL** textruta.

    Jag. Som **providern initierade begära Tjänstbindning**väljer **HTTP POST**. 

    j. Klicka på **Spara**.

### <a name="enable-your-domain"></a>Aktivera din domän
Det här avsnittet förutsätter att du redan har skapat en domän.  Mer information finns i [definierar domännamn](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**tooenable din domän, utföra hello följande steg:**

1. Hello vänstra navigeringsfönstret, klicka på **domänhantering**, och klicka sedan på **min domän.**
   
     ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   >Kontrollera att din domän har konfigurerats korrekt. 

2. I hello **sidan inloggningsinställningar** klickar du på **redigera**, sedan som **Autentiseringstjänsten**väljer hello namnet på hello SAML enkel inloggning inställningen från hello tidigare avsnittet och klicka slutligen på **spara**.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

Så snart du har en domän som har konfigurerats, bör användarna använda hello domän URL toologin toohello Salesforce sandbox.  

tooget hello värdet för hello URL på hello SSO profil du har skapat i föregående avsnitt i hello.    

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-salesforce-sandbox-test-user"></a>Skapa en testanvändare Salesforce Sandbox

I det här avsnittet skapas en användare som kallas Britta Simon i Salesforce Sandbox. Salesforce Sandbox stöder just-in-time-allokering som är aktiverad som standard.
Det finns ingen åtgärd objekt i det här avsnittet. Om en användare inte redan finns i Salesforce Sandbox, skapas en ny när du försöker tooaccess Salesforce Sandbox.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSalesforce Sandbox.

![Tilldela användare][200] 

**tooassign Britta Simon tooSalesforce Sandbox, utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Salesforce Sandbox**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

