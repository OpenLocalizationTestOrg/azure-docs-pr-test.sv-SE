---
title: "Självstudier: Azure Active Directory-integrering med O.C. Turesson - AppreciateHub | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och O.C. Turesson - AppreciateHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 45052cf56e35746d7df5910162e40e3bbcad1aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Självstudier: Azure Active Directory-integrering med O.C. Turesson - AppreciateHub

I kursen får du lära dig hur toointegrate O.C. Turesson - AppreciateHub med Azure Active Directory (AD Azure).

Integrera O.C. Turesson - AppreciateHub med Azure AD innehåller hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooO.C. Turesson - AppreciateHub
- Du kan låta dina användare tooautomatically get inloggade tooO.C. Turesson - AppreciateHub (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med O.C. Turesson - AppreciateHub, behöver du hello följande objekt:

- En Azure AD-prenumeration
- EN O.C. Turesson - AppreciateHub enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till O.C. Turesson - AppreciateHub från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-oc-tanner---appreciatehub-from-hello-gallery"></a>Lägga till O.C. Turesson - AppreciateHub från hello-galleriet
tooconfigure hello integrering av O.C. Turesson - AppreciateHub till Azure AD, behöver du tooadd O.C. Turesson - AppreciateHub hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd O.C. Turesson - AppreciateHub från galleriet hello utföra hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **O.C. Turesson - AppreciateHub**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

5. Markera hello resultat på panelen **O.C. Turesson - AppreciateHub**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med O.C. Turesson - AppreciateHub baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användare i O.C. Turesson - AppreciateHub är tooa användare i Azure AD. Med andra ord relaterade en länk relationen mellan en Azure AD-användare och hello användare i O.C. Turesson - AppreciateHub måste toobe upprättas.

I O.C. Turesson - AppreciateHub, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med O.C. Turesson - AppreciateHub, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en O.C. Turesson - AppreciateHub testanvändare](#creating-a-oc-tanner---appreciatehub-test-user)**  -toohave en motsvarighet för Britta Simon i O.C. Turesson - AppreciateHub som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet kan du aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din O.C. Turesson - AppreciateHub program.

**tooconfigure Azure AD enkel inloggning med O.C. Turesson - AppreciateHub, utföra hello följande steg:**

1. I hello Azure-portalen på hello **O.C. Turesson - AppreciateHub** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

3. På hello **O.C. Turesson - URL: er och AppreciateHub domän** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    a. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska Reply-URL. Kontakta [O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com) tooget det här värdet.

    b. Öppna hello metadatafil med hello följande länk: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).
   
    c. Leta upp hello **md:AssertionConsumerService** nod. 
   
    d. Kopiera hello värdet för hello **plats** attribut. 
   
    ![Konfigurera Appinställningar för][12]
   
    e. I hello **logga URL** textruta tidigare hello-värde som du har fått i hello föregående steg.

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_400.png)

6. tooconfigure enkel inloggning på **O.C. Turesson - AppreciateHub** sida, behöver du toosend hello hämtas **XML-Metadata för** för[O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Skapa en O.C. Turesson - AppreciateHub testanvändare

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i O.C. Turesson - AppreciateHub.

**toocreate en användare kallas Britta Simon i O.C. Turesson - AppreciateHub, utföra hello följande steg:**

Be din [O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com) toocreate en användare som har som nameID attributet hello samma värde som hello användarnamn för Britta Simon i Azure AD.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooO.C. Turesson - AppreciateHub.

![Tilldela användare][200] 

**tooassign Britta Simon tooO.C. Turesson - AppreciateHub, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **O.C. Turesson - AppreciateHub**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.  
När du klickar på hello O.C. Turesson - AppreciateHub panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour O.C. Turesson - AppreciateHub program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png

