---
title: "Självstudier: Azure Active Directory-integrering med UserEcho | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och UserEcho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: efe4a94ed6e5d22d153565d4782850eac4dff37b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Självstudier: Azure Active Directory-integrering med UserEcho

I kursen får du lära dig hur toointegrate UserEcho med Azure Active Directory (AD Azure).

Integrera UserEcho med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooUserEcho
- Du kan aktivera din användare tooautomatically get inloggade tooUserEcho (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med UserEcho, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En UserEcho enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till UserEcho från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-userecho-from-hello-gallery"></a>Att lägga till UserEcho från hello-galleriet
tooconfigure hello integrering av UserEcho i Azure AD, behöver du tooadd UserEcho hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd UserEcho från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **UserEcho**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_search.png)

5. Markera hello resultat på panelen **UserEcho**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med UserEcho baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i UserEcho är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i UserEcho toobe upprättas.

I UserEcho, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med UserEcho, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare UserEcho](#creating-a-userecho-test-user)**  -toohave en motsvarighet för Britta Simon i UserEcho som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt UserEcho program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med UserEcho:**

1. I hello Azure-portalen på hello **UserEcho** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_samlbase.png)

3. På hello **UserEcho domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.userecho.com/`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [UserEcho klienten supportteamet](https://feedback.userecho.com/) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_general_400.png)

6. På hello **UserEcho Configuration** klickar du på **konfigurera UserEcho** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_configure.png) 

7. Logga in tooyour UserEcho företagets webbplats som en administratör i ett nytt webbläsarfönster.

8. Klicka på din användare namn tooexpand hello-menyn i hello verktygsfältet hello överst och klicka sedan på **installationsprogrammet**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

9. Klicka på **integreringar**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 

10. Klicka på **webbplats**, och klicka sedan på **enkel inloggning (SAML2)**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 

11. På hello **enkel inloggning (SAML)** utför hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)
    
    a. Som **SAML-aktiverade**väljer **Ja**.
    
    b. Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **SAML SSO URL** textruta.
    
    c. Klistra in **Sign-Out URL**, som du har kopierat från hello Azure-portalen i hello **Remote logoout URL** textruta.
    
    d. Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **X.509-certifikat** textruta.
    
    e. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-userecho-test-user"></a>Skapa en testanvändare UserEcho

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i UserEcho.

**toocreate en användare som kallas Britta Simon i UserEcho, utför följande steg hello:**

1. Inloggning tooyour UserEcho företagets webbplats som administratör.

2. Klicka på din användare namn tooexpand hello-menyn i hello verktygsfältet hello överst och klicka sedan på **installationsprogrammet**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

3. Klicka på **användare**, tooexpand hello **användare** avsnitt.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

4. Klicka på **användare**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

5. Klicka på **bjuda in en ny användare**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)

6. På hello **bjuda in en ny användare** dialogrutan utföra hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    a. I hello **namn** textruta, ange namnet på hello användaren som Britta Simon.
    
    b.  I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.
    
    c. Klicka på **bjuda in**.

En inbjudan har skickats tooBritta, vilket gör att hennes toostart med UserEcho. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooUserEcho.

![Tilldela användare][200] 

**tooassign Britta Simon tooUserEcho utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **UserEcho**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.  

Du bör få automatiskt inloggade tooyour UserEcho programmet när du klickar på hello UserEcho panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png

