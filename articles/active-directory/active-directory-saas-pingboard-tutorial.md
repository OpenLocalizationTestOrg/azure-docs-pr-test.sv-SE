---
title: "Självstudier: Azure Active Directory-integrering med Pingboard | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Pingboard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 0a916b1f9ef32d8124aa11284d2115bb4fc0bbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Självstudier: Azure Active Directory-integrering med Pingboard

I kursen får du lära dig hur toointegrate Pingboard med Azure Active Directory (AD Azure).

Integrera Pingboard med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooPingboard
- Du kan aktivera din användare tooautomatically get inloggade tooPingboard (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Pingboard, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Pingboard enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Pingboard från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-pingboard-from-hello-gallery"></a>Att lägga till Pingboard från hello-galleriet
tooconfigure hello integrering av Pingboard i Azure AD, behöver du tooadd Pingboard hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Pingboard från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Pingboard**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. Markera hello resultat på panelen **Pingboard**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pingboard baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Pingboard är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Pingboard toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Pingboard.

tooconfigure och testa Azure AD enkel inloggning med Pingboard, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Pingboard](#creating-a-pingboard-test-user)**  -toohave en motsvarighet för Britta Simon i Pingboard som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Pingboard program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Pingboard:**

1. I hello Azure Management portal på hello **Pingboard** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. På hello **Pingboard domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    a. I hello **identifierare** textruta hello TYPVÄRDE som:`http://<entity-id>.pingboard.com/sp`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<entity-id>.pingboard.com/auth/saml/consume`

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL. Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare. Kontakta [Pingboard klienten supportteamet](https://support.pingboard.com/) tooget dessa värden. 

4. Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    a. I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`http://<sub-domain>.pingboard.com/sign_in`
     
5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. tooconfigure SSO på Pingboard sida, öppna ett nytt fönster i webbläsaren och logga in tooyour Pingboard konto. Du måste vara en Pingboard admin tooset in enkel inloggning på.

8. Välj hello översta menyn **appar > integreringar**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  På hello **integreringar** kan hitta hello **”Azure Active Directory”** panelen och klicka på den.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. I hello spärrad som följer Klicka **”konfigurera”**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. På hello följande sida, ser du att ”Azure SSO-integrering är aktiverad”.. Öppna hello hämtas Metadata XML-fil i en anteckningar och klistra in hello innehåll i **IDP Metadata**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. hello-filen kommer att valideras och om allt stämmer aktiveras nu för enkel inloggning

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-pingboard-test-user"></a>Skapa en testanvändare Pingboard

I ordning tooenable Azure AD-användare toolog i Pingboard, måste de etableras i Pingboard.  
Hello gäller Pingboard är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour Pingboard företagets webbplats som administratör.

2. Klicka på **”Lägg till medarbetare”** knappen på **Directory** sidan.

    ![Lägga till medarbetare](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. På hello **”Lägg till medarbetare”** dialogrutan utför hello följande steg.

    ![Bjud in personer](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    a. I hello **fullständiga namn** textruta hello fullständiga typnamnet för Britta Simon.

    b. I hello **e-post** textruta typen hello e-postadress för Britta Simon konto.

    c. I hello **befattning** textruta typen hello befattningen Britta Simon.

    d. I hello **plats** listrutan, Välj hello platsen för Britta Simon.
    
    e. Klicka på **Lägg till**.   

4. En bekräftelseskärm kommer nu att tooconfirm hello lägga till användaren.
    
    ![Bekräfta](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooPingboard sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooPingboard utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Pingboard**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Pingboard programmet när du klickar på hello Pingboard panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
