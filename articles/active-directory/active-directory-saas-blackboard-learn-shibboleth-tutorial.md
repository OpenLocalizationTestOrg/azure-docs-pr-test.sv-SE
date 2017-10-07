---
title: "Självstudier: Azure Active Directory-integrering med svart tavla Läs - Shibboleth | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och svart tavla Läs - Shibboleth."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e435cbb4-c0f0-400e-943c-5c923fa8ddf2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 40aa3ec5f42b93157af3c56daaadfa66203b21d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a>Självstudier: Azure Active Directory-integrering med svart tavla Läs - Shibboleth

I kursen får du lära dig hur toointegrate svart tavla Läs - Shibboleth med Azure Active Directory (AD Azure).

Integrera svart tavla Läs - Shibboleth med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooBlackboard Läs - Shibboleth
- Du kan aktivera din användare tooautomatically get inloggade tooBlackboard Läs - Shibboleth (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med svart tavla Läs - Shibboleth, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En svart tavla Läs - Shibboleth enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägger till svart tavla Läs - Shibboleth från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-blackboard-learn---shibboleth-from-hello-gallery"></a>Lägger till svart tavla Läs - Shibboleth från hello-galleriet
tooconfigure hello integrering av svart tavla Läs - Shibboleth till Azure AD måste tooadd svart tavla Läs - Shibboleth hello galleriet tooyour listan över hanterade SaaS-appar.

**svart tavla Läs - tooadd Shibboleth från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **svart tavla Läs - Shibboleth**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_search.png)

5. Markera hello resultat på panelen **svart tavla Läs - Shibboleth**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med svart tavla Läs - Shibboleth baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste tooknow vilka hello motsvarighet användare i svart tavla Läs för enkel inloggning toowork - Shibboleth är tooa användare i Azure AD. Med andra ord en länk mellan en Azure AD-användare och hello relaterade användare i svart tavla Läs - Shibboleth måste toobe upprättas.

Tilldela hello värdet för hello i svart tavla Läs - Shibboleth, **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med svart tavla Läs - Shibboleth, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en svart tavla Läs - Shibboleth testanvändare](#creating-a-blackboard-learn---shibboleth-test-user)**  - toohave en motsvarighet för Britta Simon i svart tavla Läs - Shibboleth som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din svart tavla Läs - Shibboleth program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med svart tavla Läs - Shibboleth:**

1. I hello Azure-portalen på hello **svart tavla Läs - Shibboleth** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_samlbase.png)

3. På hello **svart tavla Läs - URL: er och Shibboleth domän** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/Login`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourblackoardlearnserver>.blackboardlearn.com/shibboleth-sp`

    c. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/SAML2/POST`
 
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Kontakta [svart tavla Läs - Shibboleth klienten supportteamet](https://www.blackboard.com/forms/contact-us_form.aspx) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_400.png)
    
6. På hello **svart tavla Läs - Shibboleth Configuration** klickar du på **konfigurera svart tavla Läs - Shibboleth** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_configure.png) 

7. tooconfigure enkel inloggning på **svart tavla Läs - Shibboleth** sida, behöver du toosend hello hämtas **XML-Metadata för** och **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning URL: en** för[svart tavla Läs - Shibboleth supportteamet](https://www.blackboard.com/forms/contact-us_form.aspx).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-blackboard-learn---shibboleth-test-user"></a>Skapa en svart tavla Läs - Shibboleth testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i svart tavla Läs - Shibboleth. Arbeta med din [svart tavla Läs - Shibboleth supportteamet](https://www.blackboard.com/forms/contact-us_form.aspx) tooadd hello användare i hello svart tavla Läs - Shibboleth plattform.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBlackboard Läs - Shibboleth.

![Tilldela användare][200] 

**tooassign Britta Simon tooBlackboard Läs - Shibboleth, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **svart tavla Läs - Shibboleth**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello svart tavla Läs - Shibboleth panelen i hello åtkomstpanelen, bör du hämta automatiskt inloggade tooyour svart tavla Läs - Shibboleth program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_203.png

