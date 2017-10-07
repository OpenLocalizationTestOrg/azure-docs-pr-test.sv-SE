---
title: "Självstudier: Azure Active Directory-integrering med Menlo säkerhet | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Menlo säkerhet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 193d12eedf31f4f08e1d141936d6e918c36a2109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a>Självstudier: Azure Active Directory-integrering med Menlo säkerhet

I kursen får du lära dig hur toointegrate Menlo säkerhet med Azure Active Directory (AD Azure).

Integrera Menlo säkerhet med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooMenlo säkerhet
- Du kan aktivera din användare tooautomatically get inloggade tooMenlo säkerhet (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD. [Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Menlo säkerhet, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Menlo säkerhet enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Menlo säkerhet från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-menlo-security-from-hello-gallery"></a>Lägga till Menlo säkerhet från hello-galleriet
tooconfigure hello integrering av Menlo säkerhet i Azure AD, behöver du tooadd Menlo säkerhet hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Menlo säkerhet från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Menlo säkerhet**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_search.png)

5. Markera hello resultat på panelen **Menlo säkerhet**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Menlo säkerhet baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Menlo säkerhet är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Menlo säkerhet toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Menlo säkerhet.

tooconfigure och testa Azure AD enkel inloggning med Menlo säkerhet, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Menlo säkerhet](#creating-a-menlo-security-test-user)**  -toohave en motsvarighet för Britta Simon i Menlo säkerhet som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Menlo säkerhetsprogram.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Menlo säkerhet:**

1. I hello Azure-portalen på hello **Menlo säkerhet** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

3. På hello **Menlo säkerhetsdomän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.menlosecurity.com/account/login`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`

    > [!NOTE] 
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Menlo Säkerhetsklient supportteamet](https://www.menlosecurity.com/menlo-contact) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_400.png)

6. På hello **Menlo säkerhetskonfiguration** klickar du på **konfigurera säkerheten för Menlo** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

7. tooconfigure enkel inloggning på **Menlo säkerhet** sida, inloggning toohello **Menlo säkerhet** webbplats som administratör.

8. Under **inställningar** gå för**autentisering** och utföra följande åtgärder:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/menlo_user_setup.png)

    a. Markera kryssrutan hello **aktivera användarautentisering med SAML**.

    b. Välj **Tillåt extern åtkomst** för**Ja**.

    c. Under **SAML-providern**väljer **Azure Active Directory**.

    d. **SAML 2.0 Endpoint** : klistra in hello **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    e. **Service-Identifier (utfärdaren)** : klistra in hello **SAML enhets-ID** som du har kopierat från Azure-portalen.

    f. **X.509-certifikat** : öppna hello **certifikat (Base64)** hämtas från hello Azure-portalen i anteckningar och klistra in den i den här rutan.

    g. Klicka på **spara** toosave hello inställningar.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-menlo-security-test-user"></a>Skapa en testanvändare Menlo säkerhet
 
I det här avsnittet kan du skapa en användare som kallas Britta Simon i Menlo säkerhet. Arbeta med [Menlo Säkerhetsklient supportteamet](https://www.menlosecurity.com/menlo-contact) tooadd hello användare i hello Menlo säkerhet plattform. Användare måste skapas och aktiveras innan du använder enkel inloggning. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMenlo säkerhet.

![Tilldela användare][200] 

**tooassign Britta Simon tooMenlo säkerhet, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Menlo säkerhet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen.

Öppna ett webbläsarfönster i ett ”InPrivate” eller ”Incognito” läge-tootrigger en ny autentisering.  I Internet Explorer, använder du Ctrl + Skift + P.  I Chrome, använder du Ctrl + Shift + N.  Hello privat webbläsarfönster bläddrar du tooa skyddade resurser och utföra en Azure AD-inloggning.  Du kommer att vidtas toohello begärda webbplatsen i en session isolering efter genomförd inloggning.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_203.png

