---
title: "Självstudier: Azure Active Directory-integrering med Five9 Plus-kort (CTI, kontakta Center-agenter) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Five9 Plus-kort (CTI, kontakta Center-agenter)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 2e3ea8b5f2a6eaa8ad17d39e03fa490038b14561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Självstudier: Azure Active Directory-integrering med Five9 Plus-kort (CTI, kontakta Center-agenter)

I kursen får du lära dig hur toointegrate Five9 Plus nätverkskort (CTI, kontakta Center-agenter) med Azure Active Directory (AD Azure).

Integrera Five9 Plus nätverkskort (CTI, kontakta Center-agenter) med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooFive9 Plus nätverkskort (CTI, kontakta Center-agenter)
- Du kan aktivera dina användare tooautomatically get inloggade tooFive9 Plus nätverkskort (CTI, kontakta Center-agenter) (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Five9 Plus kort (CTI, kontakta Center-agenter), behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Five9 Plus nätverkskort (CTI, kontakta Center-agenter) enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Five9 Plus kort (CTI, kontakta Center-agenter) från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-hello-gallery"></a>Lägga till Five9 Plus kort (CTI, kontakta Center-agenter) från hello-galleriet
tooconfigure hello integrering av Five9 Plus nätverkskort (CTI, kontakta Center-agenter) i Azure AD, behöver du tooadd Five9 Plus nätverkskort (CTI, kontakta Center-agenter) hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Five9 Plus nätverkskort (CTI, kontakta Center-agenter) från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_search.png)

5. Markera hello resultat på panelen **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Five9 Plus-kort (CTI, kontakta Center-agenter) baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Five9 Plus nätverkskort (CTI, kontakta Center-agenter) är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Five9 Plus nätverkskort (CTI, kontakta Center-agenter) toobe upprättas.

Tilldela hello värdet för hello i Five9 Plus-kort (CTI, kontakta Center-agenter) **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Five9 Plus kort (CTI, kontakta Center-agenter), behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Five9 Plus nätverkskort (CTI, kontakta Center-agenter)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  -toohave en motsvarighet för Britta Simon i Five9 Plus kort (CTI, kontakta Center-agenter) som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Five9 Plus nätverkskort (CTI, kontakta Center-agenter).

**tooconfigure Azure AD enkel inloggning med Five9 Plus-kort (CTI, kontakta Center-agenter), utför följande steg hello:**

1. I hello Azure-portalen på hello **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_samlbase.png)

3. På hello **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)-domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_url.png)
    
    a. I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:

    |    Miljö      |       URL: EN      |
    | :-- | :-- |
    | För ”Five9 Plus Adapter för Microsoft Dynamics CRM” | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | För ”Five9 Plus kortet för Zendesk” | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | För ”Five9 Plus kortet för agenten skrivbord Toolkit” | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:

    |      Miljö     |      URL: EN      |
    | :--                  | :--           |
    | För ”Five9 Plus Adapter för Microsoft Dynamics CRM” | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | För ”Five9 Plus kortet för Zendesk” | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | För ”Five9 Plus kortet för agenten skrivbord Toolkit” | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_general_400.png)

6. På hello **Five9 Plus (CTI, kontakta Center-agenter) kortkonfiguration** klickar du på **konfigurera Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** tooopen **konfigurerainloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_configure.png) 

7. tooconfigure enkel inloggning på **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** sida, behöver du toosend hello hämtas **Certificate(Base64), Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[Five9 Plus nätverkskort (CTI, kontakta Center-agenter) supportteamet](https://www.five9.com/about/contact). Även dessutom för att konfigurera enkel inloggning ytterligare Följ hello nedanstående steg enligt toohello nätverkskort:

    a. ”Five9 Plus kortet för agenten skrivbord Toolkit” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. ”Five9 Plus Adapter för Microsoft Dynamics CRM” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. ”Five9 Plus kortet för Zendesk” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Skapa en testanvändare Five9 Plus nätverkskort (CTI, kontakta Center-agenter)

I det här avsnittet skapar du en användare som kallas Britta Simon i Five9 Plus nätverkskort (CTI, kontakta Center-agenter). Arbeta med [Five9 Plus nätverkskort (CTI, kontakta Center-agenter) supportteamet](https://www.five9.com/about/contact) att lägga till hello användare i hello Five9 Plus nätverkskort (CTI, kontakta Center-agenter)-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooFive9 Plus nätverkskort (CTI, kontakta Center-agenter).

![Tilldela användare][200] 

**tooassign Britta Simon tooFive9 Plus nätverkskort (CTI, kontakta Center-agenter), utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Five9 Plus nätverkskort (CTI, kontakta Center-agenter) panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Five9 Plus nätverkskort (CTI, kontakta Center-agenter) program.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-five9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-five9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-five9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-five9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-five9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-five9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-five9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-five9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-five9-tutorial/tutorial_general_203.png

