---
title: "Självstudier: Azure Active Directory-integrering med Autotask arbetsplats | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Autotask arbetsplats."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 7f820f24e8e9493fa2e1c075f2ef61d7eaa84f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Självstudier: Azure Active Directory-integrering med Autotask arbetsplats

I kursen får du lära dig hur toointegrate Autotask arbetsplats med Azure Active Directory (AD Azure).

Integrera Autotask arbetsplats med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooAutotask arbetsplats
- Du kan aktivera din användare tooautomatically get inloggade tooAutotask arbetsplats (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Autotask arbetsplats, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Autotask arbetsplatsen enkel inloggning på aktiverade prenumeration
- Du måste vara administratör eller super administratör i arbetsytan.
- Du måste ha ett administratörskontot i hello Azure AD.
- hello-användare som använder den här funktionen måste ha konton inom arbetsplats och hello Azure AD och deras e-postadresser för både måste matcha.

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Autotask arbetsplats från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-autotask-workplace-from-hello-gallery"></a>Att lägga till Autotask arbetsplats från hello-galleriet
tooconfigure hello integrering av Autotask arbetsplats i Azure AD, behöver du tooadd Autotask arbetsplats hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Autotask arbetsplats från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Autotask arbetsplats**väljer **Autotask arbetsplats** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Autotask arbetsplats i hello resulterar lista](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Autotask arbetsplats baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren Autotask arbetsplatsen är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Autotask arbetsplats toobe upprättas.

Tilldela hello värdet hello Autotask arbetsplatsen **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Autotask arbetsplats, måste toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Autotask arbetsplats](#create-an-autotask-workplace-test-user)**  -toohave en motsvarighet för Britta Simon Autotask arbetsplatsen som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Autotask arbetsplats.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Autotask arbetsplats:**

1. I hello Azure-portalen på hello **Autotask arbetsplats** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. Om du inte vill tooconfigure hello programmet i **IDP** initierade läge, utför följande steg på hello hello **Autotask arbetsplats domän och URL: er** avsnitt:

    ![Autotask arbetsplats domän URL: er och enkel inloggning information för IDP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

4. Om du inte vill tooconfigure hello programmet i **SP** initierade läge, kontrollera **visa avancerade inställningar för URL: en** och utföra hello följande steg:

    ![Autotask arbetsplats domän URL: er och enkel inloggning information för SP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.awp.autotask.net/loginsso`
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Kontakta [Autotask arbetsplats klienten supportteamet](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget dessa värden. 

5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_400.png)

7. I en annan webbläsarfönster hello logga in tooWorkplace Online med administratörsbehörighet.

    >[!Note]
    >När du konfigurerar hello IdP, måste en underdomän toobe anges. tooconfirm hello rätt underdomän, inloggning tooWorkplace Online. När du loggade in gör du Observera toohello underdomän i hello-URL.
    >hello underdomän ingår hello mellan hello ”https://” och ”.awp.autotask.net/” och bör vara oss eu, ca eller au.

8. Gå för**Configuration** > **enkel inloggning** och utföra hello följande steg:

    ![Autotask enkel inloggning konfiguration](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    a. Välj hello **XML-metadatafil** alternativ och sedan ladda upp hello **XML-Metadata för** hämtas från Azure-portalen.

    b. Klicka på **aktivera enkel inloggning**.
    
    ![Godkänn Autotask Single Sign-on konfiguration](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Välj hello **jag bekräfta informationen är korrekt och den här IdP tillförlitliga** kryssrutan.

    d. Klicka på **godkänna**.
     
>[!Note]
>Om du behöver hjälp med att konfigurera Autotask arbetsplats finns [den här sidan](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget hjälp med ditt arbetsplatskonto.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.

### <a name="create-an-autotask-workplace-test-user"></a>Skapa en testanvändare Autotask arbetsplats

I det här avsnittet skapar du en användare som kallas Britta Simon i Autotask. Se tillsammans med [Autotask arbetsplats supportteamet](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello användare i hello Autotask arbetsplats plattform.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAutotask arbetsplats.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooAutotask arbetsplats utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Autotask arbetsplats**.

    ![Hej Autotask arbetsplats länken i listan med program hello](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Autotask arbetsplats programmet när du klickar på hello Autotask arbetsplats panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_203.png

