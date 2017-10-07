---
title: "Självstudier: Azure Active Directory-integrering med Yonyx interaktiva stödlinjer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Yonyx interaktiva guider och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: 24e30d243143651b8d32535c76dc300931ae5746
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Självstudier: Azure Active Directory-integrering med Yonyx interaktiva stödlinjer

I kursen får du lära dig hur toointegrate Yonyx interaktiv hjälper med Azure Active Directory (AD Azure).

Integrera Yonyx interaktiva guider med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooYonyx interaktiva guider
- Du kan aktivera din användare tooautomatically get inloggade tooYonyx interaktiva guider (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Yonyx interaktiva guider måste hello följande objekt:

- En Azure AD-prenumeration
- En Yonyx interaktiva guider enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Yonyx interaktiva guider från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-yonyx-interactive-guides-from-hello-gallery"></a>Att lägga till Yonyx interaktiva guider från hello-galleriet
tooconfigure hello integrering av Yonyx interaktiva guider i Azure AD, behöver du tooadd Yonyx interaktiva guider hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Yonyx interaktiva guider från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Yonyx interaktiva guider**väljer **Yonyx interaktiva guider** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Yonyx interaktiva guider i hello resultatlistan](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Yonyx interaktiva guider baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Yonyx interaktiva guider är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Yonyx interaktiva guider toobe upprättas.

I Yonyx interaktiva distributionsguider, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Yonyx interaktiva stödlinjer, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Yonyx interaktiva guider](#create-a-yonyx-interactive-guides-test-user)**  -toohave en motsvarighet för Britta Simon i Yonyx interaktiva guider som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Yonyx interaktiva guider.

**tooconfigure Azure AD enkel inloggning med Yonyx interaktiva stödlinjer, utför hello följande steg:**

1. I hello Azure-portalen på hello **Yonyx interaktiva guider** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

3. På hello **Yonyx interaktiva guider domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och Yonyx interaktiva guider domän med enkel inloggning information](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.yonyx.com/y/conversation/?id=<guid number>`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.yonyx.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Yonyx interaktiva guider klienten supportteamet](mailto:support@yonyx.com) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-yonyx-tutorial/tutorial_general_400.png)

6. På hello **Yonyx interaktiva guider konfiguration** klickar du på **konfigurera Yonyx interaktiva guider** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Yonyx interaktiva guider konfiguration](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

7. tooconfigure enkel inloggning på **Yonyx interaktiva guider** sida, behöver du toosend hello hämtas **Certificate(Base64)**, **Sign-Out URL**, **SAML Enkel inloggning Tjänstwebbadress** **SAML enhets-ID** för[Yonyx interaktiva guider supportteam](mailto:support@yonyx.com). De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

  ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-yonyx-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![hello webbinställningar](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![hello användardialogrutan](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a>Skapa en testanvändare Yonyx interaktiva guider

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Yonyx interaktiva guider. Yonyx interaktiva guider stöder just-in-time-etablering, vilket är aktiverat som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas under ett försök tooaccess Yonyx interaktiva guider om den inte finns.

>[!NOTE]
>Om du behöver toocreate en användare manuellt, måste toocontact hello Yonyx interaktiva guider supportteam via < mailto:support@yonyx.com >. 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooYonyx interaktiva guider.

![Tilldela hello användarroll][200]

**tooassign Britta Simon tooYonyx interaktiva guider utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Yonyx interaktiva guider**.

    ![Hej Yonyx interaktiva guider länken i listan med program hello](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Yonyx interaktiva guider programmet när du klickar på hello Yonyx interaktiva guider panelen i hello åtkomstpanelen.

Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png

