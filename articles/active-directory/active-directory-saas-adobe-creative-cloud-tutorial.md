---
title: "Självstudier: Azure Active Directory-integrering med Adobe kreativa molntjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Adobe kreativa moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5e66255e9785465974a23cd3ef79c24e28c0250f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Självstudier: Azure Active Directory-integrering med Adobe kreativa moln

I kursen får du lära dig hur toointegrate Adobe kreativa moln med Azure Active Directory (AD Azure).

Integrera Adobe kreativa moln med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooAdobe kreativa moln
- Du kan aktivera din användare tooautomatically get inloggade tooAdobe kreativa molnet (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Adobe kreativa molnet, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Adobe kreativa molnet enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Adobe kreativa moln från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-adobe-creative-cloud-from-hello-gallery"></a>Lägga till Adobe kreativa moln från hello-galleriet
tooconfigure hello integrering av Adobe kreativa moln i Azure AD, behöver du tooadd Adobe kreativa molnet hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Adobe kreativa moln från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Adobe kreativa molnet**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. Markera hello resultat på panelen **Adobe kreativa molnet**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Adobe kreativa molnet baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Adobe kreativa molnet är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Adobe kreativa molnet toobe upprätta.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Adobe kreativa molnet.

tooconfigure och testa Azure AD enkel inloggning med Adobe kreativa molnet, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Adobe kreativa molnet](#creating-an-adobe-creative-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i Adobe kreativa moln som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i tillämpningsprogrammet Adobe kreativa molnet.

**tooconfigure Azure AD enkel inloggning med Adobe kreativa moln, utför följande steg hello:**

1. I hello Azure Management portal på hello **Adobe kreativa molnet** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. På hello **Adobe kreativa molnet domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    a. I hello **identifierare** textruta hello TYPVÄRDE som:`https://www.okta.com/saml2/service-provider/<token>`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL. Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare. Om du behöver toocreate en användare manuellt, måste toocontact hello Adobe kreativa molnet supportteamet.

4. På hello **Adobe kreativa molnet domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    a. Klicka på hello **visa avancerade inställningar för URL: en** alternativet

    b. I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`https://adobe.com`

5. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. På hello **Adobe kreativa Molnkonfigurationen** klickar du på **Konfigurera Adobe kreativa molnet** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-Id** och **URL för SAML SSO-Service** från Snabbreferens avsnitt.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. I en annan webbläsarfönstret, inloggning tooyour Adobe kreativa molnet klient som administratör.

8.  Gå för**identitet** hello vänstra navigeringsfönstret och klicka på din domän. Utför följande steg hello **enkel inloggning på konfiguration krävs** avsnitt.

    ![Inställningar för](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "inställningar")

9. Klicka på **Bläddra** tooupload hello hämtat certifikat från Azure AD för**IDP certifikat**.

10. I hello **IDP utfärdaren** textruta placera hello värdet för **SAML enhets-Id** som du kopierade från **konfigurera inloggning** avsnitt i Azure-portalen.

11. I hello **IDP inloggnings-URL** textruta placera hello värdet för **URL för SAML SSO-Service** som du kopierade från **konfigurera inloggning** avsnitt i Azure-portalen.

12. Välj **HTTP - omdirigering** som **IDP bindning**.

13. Välj **e-postadress** som **inloggningen Användarinställning**.
 
14. Klicka på **spara** knappen.

15. hello instrumentpanelen visas nu hello XML **”hämta Metadata”** filen. Den innehåller Adobe EntityDescriptor Webbadressen och AssertionConsumerService. Öppna hello-filen och konfigurera dem i hello Azure AD-program.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Använd hello EntityDescriptor värdet Adobe tillhandahålls för **identifierare** på hello **konfigurera Appinställningar** dialogrutan.

    b. Använd hello AssertionConsumerService värdet Adobe tillhandahålls för **Reply URL** på hello **konfigurera Appinställningar** dialogrutan.
 
### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**. 

### <a name="creating-an-adobe-creative-cloud-test-user"></a>Skapa en testanvändare Adobe kreativa moln

I ordning tooenable Azure AD-användare toolog i Adobe kreativa moln, måste de etableras i Adobe kreativa moln.  
I hello fallet med Adobe kreativa moln är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour Adobe kreativa molnet företagets platsen som en administratör.

2. Klicka på **personer**.

    ![Personer](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "personer")

3. Klicka på **bjuda in användare**.

    ![Bjuda in användare](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "bjuda in användare")

4. På hello **bjuda in personer** dialogrutan utför hello följande steg:

    ![Bjuda in](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "bjuda in")

    a. I hello **e-post** textruta typen hello e-postadress för Britta Simon konto.
    
    b. Klicka på **bjuda in**.

    > [!NOTE]
    > hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att ge sina access tooAdobe kreativa molnet.

![Tilldela användare][200] 

**tooassign Britta Simon tooAdobe kreativa moln, utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Adobe kreativa molnet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Adobe kreativa molnet panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Adobe kreativa molnapp.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png