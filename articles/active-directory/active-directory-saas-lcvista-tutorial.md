---
title: "Självstudier: Azure Active Directory-integrering med LCVista | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LCVista."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8db80d6e-3275-419f-aa39-6115a7bc9800
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: 5a4c7eb7d54b8b68c8241a97b9e516a3f6e55c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lcvista"></a>Självstudier: Azure Active Directory-integrering med LCVista

I kursen får du lära dig hur toointegrate LCVista med Azure Active Directory (AD Azure).

Integrera LCVista med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooLCVista
- Du kan aktivera din användare tooautomatically get inloggade tooLCVista (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med LCVista, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En LCVista enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till LCVista från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-lcvista-from-hello-gallery"></a>Att lägga till LCVista från hello-galleriet
tooconfigure hello integrering av LCVista i Azure AD, behöver du tooadd LCVista hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd LCVista från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **LCVista**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_search.png)

5. Markera hello resultat på panelen **LCVista**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LCVista baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LCVista är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LCVista toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LCVista.

tooconfigure och testa Azure AD enkel inloggning med LCVista, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare LCVista](#creating-a-lcvista-test-user)**  -toohave en motsvarighet för Britta Simon i LCVista som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt LCVista program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LCVista:**

1. I hello Azure-portalen på hello **LCVista** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_samlbase.png)

3. På hello **LCVista domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.lcvista.com/rainier/login`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.lcvista.com` 
     
    > [!NOTE] 
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska identifierare och inloggnings-URL. Kontakta [LCVista klienten supportteamet](https://lcvista.com/contact) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_general_400.png)
    
6. På hello **LCVista Configuration** klickar du på **konfigurera LCVista** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_configure.png) 

7.  Logga in tooyour LCVista programmet som administratör.

8. I hello **SAML-Config** avsnittet markerar hello **aktivera SAML inloggningen** och ange hello information som anges i under bilden. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_config.png)

    a. Klistra in hello **utfärdar-URL** som du har kopierat från Azure AD i hello **enhets-ID** avsnitt. 

    b. Klistra in hello **inloggning tjänst-URL för enkel** som du har kopierat från Azure AD i hello **URL** avsnitt.

    c. Från Metadata (XML) som du har hämtat från Azure-portalen, kopiera hello värde **X509Certificate** och klistra in den i hello **x509 certifikat** avsnitt.

    d. I hello **förnamn attributet** textruta klistra in hello värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    e. I hello **senaste namnattributet** textruta klistra in hello värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    f. I hello **e-attributet** textruta klistra in hello värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    g. I hello **Username-attributet** textruta klistra in hello värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. Klicka på **spara** toosave hello inställningar.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-lcvista-test-user"></a>Skapa en testanvändare LCVista

I det här avsnittet skapar du en användare som kallas Britta Simon i LCVista. Du behöver toocontact [LCVista klienten supportteamet](https://lcvista.com/contact) tooadd hello användare i hello LCVista program. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLCVista.

![Tilldela användare][200] 

**tooassign Britta Simon tooLCVista utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **LCVista**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen. Klicka på hello LCVista panelen i hello åtkomstpanelen, kommer du att omdirigeras tooOrganization logga in på sidan. Du kommer att inloggade tooyour LCVista program efter genomförd inloggning. Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_203.png

