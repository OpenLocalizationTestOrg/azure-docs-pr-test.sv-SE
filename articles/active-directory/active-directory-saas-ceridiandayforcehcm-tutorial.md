---
title: "Självstudier: Azure Active Directory-integrering med Ceridian Dayforce HCM | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Ceridian Dayforce HCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 4d72f29b4e5e30ef8881806d789f6676fc541e2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Självstudier: Azure Active Directory-integrering med Ceridian Dayforce HCM

I kursen får du lära dig hur toointegrate Ceridian Dayforce HCM med Azure Active Directory (AD Azure).

Integrera Ceridian Dayforce HCM med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooCeridian Dayforce HCM.
- Du kan låta dina användare tooautomatically get inloggade tooCeridian Dayforce HCM (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Ceridian Dayforce HCM måste hello följande objekt:

- En Azure AD-prenumeration
- En Ceridian Dayforce HCM enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Ceridian Dayforce HCM från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-ceridian-dayforce-hcm-from-hello-gallery"></a>Att lägga till Ceridian Dayforce HCM från hello-galleriet
tooconfigure hello integrering av Ceridian Dayforce HCM i Azure AD, behöver du tooadd Ceridian Dayforce HCM hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Ceridian Dayforce HCM från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Ceridian Dayforce HCM**väljer **Ceridian Dayforce HCM** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Ceridian Dayforce HCM i hello resultatlistan](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Ceridian Dayforce HCM baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Ceridian Dayforce HCM är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Ceridian Dayforce HCM toobe upprättas.

Tilldela hello värdet för hello i Ceridian Dayforce HCM **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Ceridian Dayforce HCM, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Ceridian Dayforce HCM](#create-a-ceridian-dayforce-hcm-test-user)**  -toohave en motsvarighet för Britta Simon i Ceridian Dayforce HCM som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Ceridian Dayforce HCM-program.

**tooconfigure Azure AD enkel inloggning med Ceridian Dayforce HCM utför hello följande steg:**

1. I hello Azure-portalen på hello **Ceridian Dayforce HCM** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. På hello **Ceridian Dayforce HCM domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    a. I hello **logga URL** textruta typen hello URL som används av dina användare toosign på tooyour Ceridian Dayforce HCM-programmet.
    
    | Miljö | URL: EN |
    | :-- | :-- |
    | För produktion | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | För testet | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:
    
    | Miljö | URL: EN |
    | :-- | :-- |
    | För produktion | `https://ncpingfederate.dayforcehcm.com/sp` |
    | För testet | `https://fs-test.dayforcehcm.com/sp` |
    
    c. I hello **Reply URL** textruta typen hello URL som används av Azure AD toopost hello svar.
    
    | Miljö | URL: EN |
    | :-- | :-- |
    | För produktion | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | För testet | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Kontakta [Ceridian Dayforce HCM klienten supportteamet](https://www.ceridian.com/contact-us/index.html) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. Tillämpningsprogrammet Ceridian Dayforce HCM förväntar hello SAML intyg i ett specifikt format. Arbeta med [Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html) första tooidentify hello rätt användar-ID. Microsoft rekommenderar att du använder hello **”name”** attribut som användaridentifierare. Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.  

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:
    
    | Attributets namn  | Attributvärdet |
    | --------------- | -------------------- |    
    | namn  | User.extensionattribute2 |    

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. I hello **värdet** listan, Välj hello användarattribut som du vill använda toouse för din implementering.
    Till exempel om du vill toouse hello EmployeeID som unikt identifierare och du har lagrat hello attributvärdet i hello ExtensionAttribute2, väljer du **user.extensionattribute2**.
    
    d. Klicka på **OK**.

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. På hello **Ceridian Dayforce HCM Configuration** klickar du på **konfigurera Ceridian Dayforce HCM** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Ceridian Dayforce HCM-konfiguration](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. tooconfigure enkel inloggning på **Ceridian Dayforce HCM** sida, behöver du toosend hello hämtas **XML-Metadata för** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a>Skapa en Ceridian Dayforce HCM testanvändare

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Ceridian Dayforce HCM. Arbeta med hello [Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html) tooget användare som lagts till i hello Ceridian Dayforce HCM-programmet. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCeridian Dayforce HCM.

![Tilldela användare][200] 

**tooassign Britta Simon tooCeridian Dayforce HCM utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Ceridian Dayforce HCM**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCeridian Dayforce HCM.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooCeridian Dayforce HCM utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Ceridian Dayforce HCM**.

    ![Hej Ceridian Dayforce HCM länken i listan med program hello](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.  
Du bör få automatiskt inloggade tooyour Ceridian Dayforce HCM programmet när du klickar på hello Ceridian Dayforce HCM-panelen i hello åtkomstpanelen. 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png

