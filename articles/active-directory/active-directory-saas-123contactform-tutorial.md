---
title: "Självstudier: Azure Active Directory-integrering med 123ContactForm | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och 123ContactForm."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 931255887845edd1aa7f53b9051a82a2f898e055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a>Självstudier: Azure Active Directory-integrering med 123ContactForm

I kursen får du lära dig hur toointegrate 123ContactForm med Azure Active Directory (AD Azure).

Integrera 123ContactForm med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till too123ContactForm
- Du kan aktivera din användare tooautomatically get inloggade too123ContactForm (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med 123ContactForm, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En 123ContactForm enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till 123ContactForm från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-123contactform-from-hello-gallery"></a>Att lägga till 123ContactForm från hello-galleriet
tooconfigure hello integrering av 123ContactForm i Azure AD, behöver du tooadd 123ContactForm hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd 123ContactForm från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **123ContactForm**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. Markera hello resultat på panelen **123ContactForm**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 123ContactForm baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i 123ContactForm är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i 123ContactForm toobe upprättas.

I 123ContactForm, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med 123ContactForm, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare 123ContactForm](#creating-a-123contactform-test-user)**  -toohave en motsvarighet för Britta Simon i 123ContactForm som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt 123ContactForm program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med 123ContactForm:**

1. I hello Azure-portalen på hello **123ContactForm** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. På hello **123ContactForm domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url1.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`

4. Om du inte vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url2.png)

    a. Klicka på hello **visa avancerade inställningar för URL: en** alternativet

    b. I hello **logga URL** textruta Skriv en URL som:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Du behöver tooupdate dessa värde från faktiska URL: er och identifierare som beskrivs senare i hello kursen.
    
5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. tooconfigure enkel inloggning på **123ContactForm** sida, gå för[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) och utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    a. I hello **e-post** textruta hello e-post för hello användaren engångsfaktorautentisering **BrittaSimon@Contoso.com**.

    b. Klicka på **överför** och bläddra hello Metadata XML-fil som du har hämtat från Azure-portalen.

    c. Klicka på **skicka formuläret**.

8. På hello **konfigurera Appinställningar i Microsoft Azure AD enkel inloggning -** utföra hello följande steg:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url3.png)

    a. Om du inte vill tooconfigure hello programmet i **IDP initierade läge**, kopiera hello **IDENTIFIERARE** värde för din instans och klistra in den i **identifierare** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.
    
    b. Om du inte vill tooconfigure hello programmet i **IDP initierade läge**, kopiera hello **REPLY URL** värde för din instans och klistra in den i **Reply URL** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.

    c. Om du inte vill tooconfigure hello programmet i **SP initierade läge**, kopiera hello **SIGN ON URL** värde för din instans och klistra in den i **logga URL** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-123contactform-test-user"></a>Skapa en testanvändare 123ContactForm

Programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst too123ContactForm.

![Tilldela användare][200] 

**tooassign Britta Simon too123ContactForm utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **123ContactForm**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour 123ContactForm programmet när du klickar på hello 123ContactForm panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

