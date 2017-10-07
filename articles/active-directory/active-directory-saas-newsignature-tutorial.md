---
title: "Självstudier: Azure Active Directory-integrering med molnet för Microsoft Azure-hanteringsportalen | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och molnet hanteringsportal för Microsoft Azure."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9596826e3dc1289b95009bf01ec8b86f823ef345
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Självstudier: Azure Active Directory-integrering med molntjänster Management Portal för Microsoft Azure

I kursen får du lära dig hur toointegrate moln hanteringsportal för Microsoft Azure med Azure Active Directory (AD Azure).

Integrera molnet hanteringsportal för Microsoft Azure med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooCloud hanteringsportal för Microsoft Azure
- Du kan aktivera din användare tooautomatically get inloggade tooCloud hanteringsportal för Microsoft Azure (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med molntjänster hanteringsportal för Microsoft Azure, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En molnet Management Portal för Microsoft Azure enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till molnet hanteringsportal för Microsoft Azure från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-hello-gallery"></a>Att lägga till molnet hanteringsportal för Microsoft Azure från hello-galleriet
tooconfigure hello integrering av molnet hanteringsportal för Microsoft Azure i Azure AD, behöver du tooadd moln Management Portal för Microsoft Azure från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd moln hanteringsportal för Microsoft Azure från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **molnet för Microsoft Azure-hanteringsportalen**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_search.png)

5. Markera hello resultat på panelen **molnet för Microsoft Azure-hanteringsportalen**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med molnet hanteringsportal för Microsoft Azure baserad på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i molnet Management Portal för Microsoft Azure är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i molnet för Microsoft Azure-hanteringsportalen toobe upprättas.

I molnet Management Portal för Microsoft Azure, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med molnet Management Portal för Microsoft Azure, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en moln Management Portal för Microsoft Azure testanvändare](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  -toohave en motsvarighet för Britta Simon i molnet Management Portal för Microsoft Azure som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i molnet Management-portalen för Microsoft Azure-program.

**tooconfigure Azure AD enkel inloggning med molnet Management Portal för Microsoft Azure utför hello följande steg:**

1. I hello Azure-portalen på hello **molnet för Microsoft Azure-hanteringsportalen** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_samlbase.png)

3. På hello **moln hanteringsportal för URL: er och Microsoft Azure Domain** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_url.png)

    a. I hello **inloggnings-URL** textruta, ange en Webbadress med hello följande mönster: 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    b. I hello **identifierare** textruta, ange en Webbadress med hello följande mönster: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    c. I hello **Reply URL** textruta, ange en Webbadress med hello följande mönster: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL, identifierare och svars-URL. Kontakta [moln Management Portal för Microsoft Azure-klient supportteamet](mailto:jczernuszka@newsignature.com) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_general_400.png)

6. På hello **moln hanteringsportal för Microsoft Azure Configuration** klickar du på **konfigurera Portal för molnet för Microsoft Azure** tooopen **konfigurera inloggning**fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_configure.png) 

7. tooconfigure enkel inloggning på **molnet för Microsoft Azure-hanteringsportalen** sida, behöver du toosend hello hämtas **certifikat**, **Sign-Out URL**, **SAML enkel inloggning Tjänstwebbadress** och **SAML enhets-ID** för[moln hanteringsportal för Microsoft Azure-teamet](mailto:jczernuszka@newsignature.com). De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Skapa en moln Management Portal för Microsoft Azure testanvändare

hello syftet med det här avsnittet är toocreate en användare som heter Britta Simon i molnet Management-portalen för Microsoft Azure. Se tillsammans med [moln hanteringsportal för Microsoft Azure-teamet](mailto:jczernuszka@newsignature.com) tooadd hello användare i hello molnet Management Portal för Microsoft Azure-konto.


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCloud för Microsoft Azure-hanteringsportalen.

![Tilldela användare][200] 

**tooassign Britta Simon tooCloud hanteringsportal för Microsoft Azure utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **molnet för Microsoft Azure-hanteringsportalen**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.
När du klickar på hello molnet Management Portal för Microsoft Azure-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour moln Management Portal för Microsoft Azure-program.

Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png

