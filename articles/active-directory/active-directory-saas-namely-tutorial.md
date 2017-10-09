---
title: "Självstudier: Azure Active Directory-integrering med nämligen | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och nämligen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: b0477ca6fa52a21abea7de458f8a99a01e8c25c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Självstudier: Azure Active Directory-integrering med nämligen

I kursen får du lära dig hur toointegrate nämligen med Azure Active Directory (AD Azure).

Integrera nämligen med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooNamely
- Du kan aktivera din användare tooautomatically get inloggade tooNamely (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med nämligen du behöver hello följande objekt:

- En Azure AD-prenumeration
- En nämligen enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till nämligen från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-namely-from-hello-gallery"></a>Att lägga till nämligen från hello-galleriet
tooconfigure hello integrering av nämligen i Azure AD, behöver du tooadd nämligen från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd nämligen från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **nämligen**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. Markera hello resultat på panelen **nämligen**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med nämligen baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i nämligen är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i nämligen toobe upprättas.

I det kan tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med nämligen du behöver toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare nämligen](#creating-a-namely-test-user)**  -toohave en motsvarighet för Britta Simon i nämligen som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din nämligen program.

**tooconfigure Azure AD enkel inloggning med nämligen utför hello följande steg:**

1. I hello Azure-portalen på hello **nämligen** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. På hello **nämligen domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.namely.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.namely.com/saml/metadata`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [nämligen klienten supportteamet](https://www.namely.com/contact/) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. På hello **nämligen Configuration** klickar du på **konfigurera nämligen** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. I ett nytt webbläsarfönster inloggning tooyour nämligen företagets webbplats som administratör.

8. Klicka i hello verktygsfältet hello längst upp **företagets**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. Klicka på hello **inställningar** fliken.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. Klicka på **SAML**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. På hello **SAML inställningar** utför hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    a. Klicka på **aktivera SAML**. 

    b. I hello **identitet providern SSO url** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.
    
    c. Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **providern identitetscertifikat** textruta.
     
    d. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-namely-test-user"></a>Skapa en nämligen testanvändare

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i nämligen.

**toocreate en användare som kallas Britta Simon i nämligen utför hello följande steg:**

1. Inloggning tooyour företagets nämligen platsen som en administratör.

2. Klicka i hello verktygsfältet hello längst upp **personer**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. Klicka på hello **Directory** fliken.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. Klicka på **Lägg till ny Person**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. På hello **Lägg till ny Person** dialogrutan utföra hello följande steg:

    a. I hello **Förnamn** textruta typen **Britta**.

    b. I hello **efternamn** textruta typen **Simon**.

    c. I hello **e-post** textruta typen hello **e-postadress** av BrittaSimon.

    d. Klicka på **Spara**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNamely.

![Tilldela användare][200] 

**tooassign Britta Simon tooNamely utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **nämligen**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.

När du klickar på hello nämligen panelen i hello åtkomstpanelen, du får automatiskt inloggade tooyour nämligen program

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

