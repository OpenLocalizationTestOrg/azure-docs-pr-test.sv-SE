---
title: "Självstudier: Azure Active Directory-integrering med LiquidFiles | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LiquidFiles."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 67eb888090f81e0ceb791ed45d564b98fe1eb6d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a>Självstudier: Azure Active Directory-integrering med LiquidFiles

I kursen får du lära dig hur toointegrate LiquidFiles med Azure Active Directory (AD Azure).

Integrera LiquidFiles med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooLiquidFiles
- Du kan aktivera din användare tooautomatically get inloggade tooLiquidFiles (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med LiquidFiles, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En LiquidFiles enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till LiquidFiles från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-liquidfiles-from-hello-gallery"></a>Att lägga till LiquidFiles från hello-galleriet
tooconfigure hello integrering av LiquidFiles i Azure AD, behöver du tooadd LiquidFiles hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd LiquidFiles från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **LiquidFiles**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_search.png)

5. Markera hello resultat på panelen **LiquidFiles**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LiquidFiles baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LiquidFiles är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LiquidFiles toobe upprättas.

I LiquidFiles, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med LiquidFiles, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare LiquidFiles](#creating-a-liquidfiles-test-user)**  -toohave en motsvarighet för Britta Simon i LiquidFiles som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt LiquidFiles program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LiquidFiles:**

1. I hello Azure-portalen på hello **LiquidFiles** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_samlbase.png)

3. På hello **LiquidFiles domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<YOUR_SERVER_URL>/saml/init`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<YOUR_SERVER_URL>`

    c. b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<YOUR_SERVER_URL>/saml/consume`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL, identifierare och Reply-URL. Kontakta [LiquidFiles klienten supportteamet](https://www.liquidfiles.com/support.html) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_400.png)

6. På hello **LiquidFiles Configuration** klickar du på **konfigurera LiquidFiles** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_configure.png)
 
7. Inloggning tooyour LiquidFiles företagets webbplats som administratör.

8. Klicka på **enkel inloggning** i hello **Admin > Configuration** hello-menyn.

9. På hello **konfiguration för enkel inloggning** utför följande steg hello

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_single_01.png)

    a. Som **enkel inloggning på metoden**väljer **SAML 2**.

    b. I hello **IDP inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.

    c. I hello **IDP logga ut URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.

    d. I hello **IDP Cert fingeravtryck** textruta klistra in hello **TUMAVTRYCKET** värde som du har kopierat från Azure-portalen...

    e. Ange hello värde i hello identifierare namnformat textruta **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.

    f. Ange hello värde i hello Authn kontexten textruta **urn: oasis: namn: tc: SAML:2.0:ac:classes:PasswordProtectedTransport**.

    g. Klicka på **Spara**.  

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-liquidfiles-test-user"></a>Skapa en testanvändare LiquidFiles

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i LiquidFiles. Arbeta med din LiquidFiles server-administratören tooget själv läggas till som en användare innan du loggar in tooyour LiquidFiles program.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLiquidFiles.

![Tilldela användare][200] 

**tooassign Britta Simon tooLiquidFiles utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **LiquidFiles**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour LiquidFiles programmet när du klickar på hello LiquidFiles panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_203.png

