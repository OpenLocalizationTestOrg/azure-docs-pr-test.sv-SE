---
title: "Självstudier: Azure Active Directory-integrering med Alcumus Info Exchange | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Alcumus Info Exchange."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d26034b8-f0d5-4f65-aa56-0fc168ceec8c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 4ef9f4d654b6c451db44f929bdad1016304168b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Självstudier: Azure Active Directory-integrering med Alcumus Info Exchange

I kursen får du lära dig hur toointegrate Alcumus Info Exchange med Azure Active Directory (AD Azure).

Integrera Alcumus Info Exchange med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooAlcumus Info Exchange
- Du kan aktivera din användare tooautomatically get inloggade tooAlcumus Info Exchange (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Alcumus Info Exchange måste hello följande objekt:

- En Azure AD-prenumeration
- En Alcumus Info Exchange enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Alcumus Info Exchange från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-alcumus-info-exchange-from-hello-gallery"></a>Att lägga till Alcumus Info Exchange från hello-galleriet
tooconfigure hello integrering av Alcumus Info Exchange i Azure AD, behöver du tooadd Alcumus Info Exchange hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Alcumus Info Exchange från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Alcumus Info Exchange**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_search.png)

5. Markera hello resultat på panelen **Alcumus Info Exchange**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Alcumus Info Exchange baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Alcumus Info Exchange är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Alcumus Info Exchange toobe upprätta.

Tilldela hello värdet hello i Alcumus Info Exchange **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Alcumus Info Exchange, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Alcumus Info Exchange](#creating-an-alcumus-info-exchange-test-user)**  -toohave en motsvarighet för Britta Simon i Alcumus Info Exchange som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Alcumus Info Exchange-program.

**tooconfigure Azure AD enkel inloggning med Alcumus Info Exchange utför hello följande steg:**

1. I hello Azure-portalen på hello **Alcumus Info Exchange** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_samlbase.png)

3. På hello **Alcumus information Exchange-domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.info-exchange.com`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.info-exchange.com/Auth/`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com) tooget dessa värden.
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_400.png)

6. tooconfigure enkel inloggning på **Alcumus Info Exchange** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-alcumus-info-exchange-test-user"></a>Skapa en testanvändare Alcumus Info Exchange

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Alcumus Info Exchange.

toocreate en användare som kallas Britta Simon i Alcumus Info Exchange, kontakta hello [Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com).

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAlcumus Info Exchange.

![Tilldela användare][200] 

**tooassign Britta Simon tooAlcumus Info Exchange, utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Alcumus Info Exchange**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.  
När du klickar på hello Alcumus Info Exchange-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Alcumus information Exchange-program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png

