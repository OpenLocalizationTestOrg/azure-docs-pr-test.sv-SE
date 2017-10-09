---
title: "Självstudier: Azure Active Directory-integrering med Workstars | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Workstars."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 51a4a4e4-ff60-4971-b3f8-a0367b70d220
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 86250d7538f058d2a821ded7dda0b2fc185d80df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workstars"></a>Självstudier: Azure Active Directory-integrering med Workstars

I kursen får du lära dig hur toointegrate Workstars med Azure Active Directory (AD Azure).

Integrera Workstars med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooWorkstars.
- Du kan låta dina användare tooautomatically get inloggade tooWorkstars (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Workstars, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Workstars enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Workstars från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-workstars-from-hello-gallery"></a>Att lägga till Workstars från hello-galleriet
tooconfigure hello integrering av Workstars i Azure AD, behöver du tooadd Workstars hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Workstars från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Workstars**väljer **Workstars** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Workstars i hello resultatlistan](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workstars baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Workstars är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Workstars toobe upprättas.

I Workstars, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Workstars, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Workstars](#create-a-workstars-test-user)**  -toohave en motsvarighet för Britta Simon i Workstars som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Workstars program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Workstars:**

1. I hello Azure-portalen på hello **Workstars** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_samlbase.png)

3. På hello **Workstars domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och Workstars domän med enkel inloggning information](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_url.png)

    a. I hello **identifierare** textruta typen hello URL:`https://workstars.com`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.workstars.com/saml/login_check`

    > [!NOTE] 
    > hello-värdet är inte verkliga. Hello uppdateringsvärde med hello faktiska Reply-URL. Kontakta [Workstars supportteam](https://support.workstars.com) tooget hello värde.
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-workstars-tutorial/tutorial_general_400.png)

6. På hello **Workstars Configuration** klickar du på **konfigurera Workstars** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Workstars konfiguration](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_configure.png) 

7. Logga in tooyour Workstars företagets webbplats som en administratör i ett nytt webbläsarfönster.

8. I hello verktygsfältet klickar du på **inställningar**.

    ![Workstars sett](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_sett.png)

9. Gå för**inloggning** > **inställningar**.

    ![Workstars inloggning](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_signon.png)

    ![Workstars inställningar](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_settings.png)

10. På hello **enkel inloggning på (SAML) - inställningar** utför hello följande steg:
    
    ![Workstars saml](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_saml.png)

    a. I **identitet providernamn** textruta typen **Office 365**.

    b. I hello **identitet providern enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.

    c. Kopiera hello innehållet hello hämtat certifikatfilen i anteckningar och klistra in den i hello **x509 certifikat** textruta. 

    d. I hello **SAML SSO URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.
    
    e. I hello **Remote logga ut URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen. 

    f. Välj **namn-ID** som **e-post (standard)**.

    g. Klicka på **Bekräfta**.
    
> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-workstars-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-workstars-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-workstars-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-workstars-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
  
### <a name="create-a-workstars-test-user"></a>Skapa en testanvändare Workstars

I det här avsnittet skapar du en användare som kallas Britta Simon i Workstars. Arbeta med [Workstars supportteam](https://support.workstars.com) tooadd hello användare i hello Workstars plattform.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkstars.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooWorkstars utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Workstars**.

    ![Hej Workstars länken i listan med program hello](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Workstars programmet när du klickar på hello Workstars panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_203.png

