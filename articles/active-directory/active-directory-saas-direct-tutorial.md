---
title: "Självstudier: Azure Active Directory-integrering med Direct | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och direkt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c2cd1f0-d14c-42f0-94a8-9b800008b285
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ac663070b39e55eade2c43814b63a9d0374c7316
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-direct"></a>Självstudier: Azure Active Directory-integrering med Direct

I kursen får du lära dig hur toointegrate direkt med Azure Active Directory (AD Azure).

Integrera direkt med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooDirect
- Du kan aktivera din användare tooautomatically get inloggade tooDirect (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med direkta, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En direkt enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägger till direkt från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-direct-from-hello-gallery"></a>Lägger till direkt från hello-galleriet
tooconfigure hello integrering av direkt i Azure AD, behöver du tooadd direkt från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd direkt från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **direkt**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_search.png)

5. Markera hello resultat på panelen **direkt**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Direct baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Direct är tooa i Azure AD. Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i direkta behov toobe upprättas.

I direkt tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med direkta, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en direkt testanvändare](#creating-a-direct-test-user)**  -toohave en motsvarighet för Britta Simon i Direct som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet direkt.

**tooconfigure Azure AD enkel inloggning med direkta, utför följande steg hello:**

1. I hello Azure-portalen på hello **direkt** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_samlbase.png)

3. På hello **direkt domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_url.png)

    I hello **identifierare** textruta typen hello URL:`https://direct4b.com/`

4. Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_url1.png)

     I hello **inloggnings-URL** textruta typen hello URL:`https://direct4b.com/sso` 
    
5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_general_400.png)

7. tooconfigure enkel inloggning på **direkt** sida, behöver du toosend hello hämtas **XML-Metadata för** för[direkt supportteamet](https://direct4b.com/ja/support.html#inquiry). 

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-direct-test-user"></a>Skapa en direkt testanvändare

I det här avsnittet kan du skapa en användare som kallas Britta Simon i direkt. Arbeta med [direkt supportteamet](https://direct4b.com/ja/support.html#inquiry) att lägga till hello användare i hello direkt plattform. Användare måste skapas och aktiveras innan du använder enkel inloggning. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDirect.

![Tilldela användare][200] 

**tooassign Britta Simon tooDirect utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **direkt**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

1. Om du inte vill tootest i **IDP initierade läge**:

    När du klickar på hello **direkt** panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour **direkt** program.

2. Om du inte vill tootest i **SP-initierad läge**:
    
    a. Klicka på hello **direkt** panelen i hello åtkomstpanelen och du kommer att omdirigerade toohello inloggning sidan program.

    b. Ange din `subdomain` i hello textrutan som visas och tryck på '次へ (nästa), och du bör få automatiskt inloggade tooyour **direkt** program.
    
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-direct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-direct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-direct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-direct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-direct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-direct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-direct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-direct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-direct-tutorial/tutorial_general_203.png

