---
title: "Självstudier: Azure Active Directory-integrering med HappyFox | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och HappyFox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 1190652d7d1144c7eddea339cb3f9175912407fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a>Självstudier: Azure Active Directory-integrering med HappyFox

I kursen får du lära dig hur toointegrate HappyFox med Azure Active Directory (AD Azure).

Integrera HappyFox med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooHappyFox
- Du kan aktivera din användare tooautomatically get inloggade tooHappyFox (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med HappyFox, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En HappyFox enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till HappyFox från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-happyfox-from-hello-gallery"></a>Att lägga till HappyFox från hello-galleriet
tooconfigure hello integrering av HappyFox i Azure AD, behöver du tooadd HappyFox hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd HappyFox från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **HappyFox**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_search.png)

5. Markera hello resultat på panelen **HappyFox**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HappyFox baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i HappyFox är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i HappyFox toobe upprättas.

I HappyFox, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med HappyFox, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare HappyFox](#creating-a-happyfox-test-user)**  -toohave en motsvarighet för Britta Simon i HappyFox som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt HappyFox program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med HappyFox:**

1. I hello Azure-portalen på hello **HappyFox** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_samlbase.png)

3. På hello **HappyFox domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.happyfox.com/`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.happyfox.com/saml/metadata/`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [HappyFox klienten supportteamet](https://support.happyfox.com/home) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_general_400.png)

6. På hello **HappyFox Configuration** klickar du på **konfigurera HappyFox** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnittet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_configure.png) 

7. Logga in tooyour HappyFox personal portal och gå för**hantera**, klicka på **integreringar** fliken.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/header.png) 

8. I hello integreringar fliken klickar du på **konfigurera** under **SAML Integration** tooopen hello enkel inloggning på inställningar.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/configure.png) 

9. Klistra in hello i SAML konfigurationsavsnittet **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen i **mål-URL för SSO** textruta.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/targeturl.png)

10. Öppna hello certifikat hämtas från Azure-portalen i anteckningar och klistra in innehållet i **IdP signatur** avsnitt.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/cert.png)

11. Klicka på **Spara inställningar** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/savesettings.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-happyfox-test-user"></a>Skapa en testanvändare HappyFox

Programmet stöder bara i tid användaretablering och authentication-användare skapas i hello program automatiskt.
        
### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHappyFox.

![Tilldela användare][200] 

**tooassign Britta Simon tooHappyFox utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **HappyFox**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

1. När du klickar på hello HappyFox panelen i hello åtkomstpanelen bör du hämta inloggningssidan för HappyFox program. Du bör se hello **'SAML-** knappen hello-inloggningssida.

    ![plugin-programmet](./media/active-directory-saas-happyfox-tutorial/saml.png) 

2. Klicka på hello **'SAML-** knappen toolog i tooHappyFox med hjälp av Azure AD-kontot.

Läs mer om hello åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_203.png

