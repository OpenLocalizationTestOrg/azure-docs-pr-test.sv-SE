---
title: "Självstudier: Azure Active Directory-integrering med TigerText Secure Messenger | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TigerText Secure Messenger."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: a3d7bb9598658c75c567c15751740d885fe4fc27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Självstudier: Azure Active Directory-integrering med TigerText Secure Messenger

I kursen får du lära dig hur toointegrate TigerText Secure Messenger med Azure Active Directory (AD Azure).

Integrera TigerText Secure Messenger med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooTigerText säker Messenger
- Du kan aktivera din användare tooautomatically get inloggade tooTigerText Secure Messenger (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med TigerText Secure Messenger, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En TigerText Secure Messenger enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till TigerText Secure Messenger från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="add-tigertext-secure-messenger-from-hello-gallery"></a>Lägg till TigerText Secure Messenger från hello-galleriet
tooconfigure hello integrering av TigerText säkra Messenger i Azure AD, behöver du tooadd TigerText Secure Messenger hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd TigerText Secure Messenger från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **TigerText Secure Messenger**väljer **TigerText Secure Messenger** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Lägg till från galleriet](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med TigerText Secure Messenger baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TigerText Secure Messenger är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i TigerText Secure Messenger toobe upprättas.

Tilldela hello värdet hello i TigerText Secure Messenger **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med TigerText Secure Messenger, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare TigerText Secure Messenger](#create-a-tigertext-secure-messenger-test-user)**  -toohave en motsvarighet för Britta Simon i TigerText Secure Messenger som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i TigerText Secure Messenger-programmet.

**tooconfigure Azure AD enkel inloggning med TigerText Secure Messenger utför hello följande steg:**

1. I hello Azure-portalen på hello **TigerText Secure Messenger** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![SAML-baserade inloggning](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. På hello **TigerText Secure Messenger domän och URL: er** avsnittet, utföra hello följande steg:

    ![Avsnittet TigerText Secure Messenger domän och URL: er](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    a. I hello **inloggnings-URL** textruta typen URL: en som:`https://home.tigertext.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska identifierare. Kontakta [TigerText Secure Messenger-klienten supportteamet](mailTo:prosupport@tigertext.com) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. Klicka på **spara** knappen.

    ![Knappen Spara](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. tooget enkel inloggning konfigurerats för ditt program, kontakta [TigerText Secure Messenger supportteamet](mailTo:prosupport@tigertext.com) och ge dem hello **hämtade metadata**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Användare och grupper -> alla användare](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Knappen Lägg till](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Användardialogrutan](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a>Skapa en testanvändare TigerText Secure Messenger

I det här avsnittet skapar du en användare som kallas Britta Simon i TigerText. Kontakta för[TigerText Secure Messenger-klienten supportteamet](mailTo:prosupport@tigertext.com) tooadd hello användare i hello TigerText plattform.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTigerText Secure Messenger.

![Tilldela användare][200] 

**tooassign Britta Simon tooTigerText Secure Messenger, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **TigerText Secure Messenger**.

    ![TigerText Secure Messenger i listan över appar](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour TigerText programmet när du klickar på hello TigerText panelen i hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

