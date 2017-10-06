---
title: "Självstudier: Azure Active Directory-integrering med Evernote | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Evernote."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 4d7017e571ed12a0b155aa188c6b0ecb3c9898a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Självstudier: Azure Active Directory-integrering med Evernote

I kursen får du lära dig hur toointegrate Evernote med Azure Active Directory (AD Azure).

Integrera Evernote med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooEvernote.
- Du kan låta dina användare tooautomatically get inloggade tooEvernote (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Evernote, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Evernote enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Evernote från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-evernote-from-hello-gallery"></a>Att lägga till Evernote från hello-galleriet
tooconfigure hello integrering av Evernote i Azure AD, behöver du tooadd Evernote hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Evernote från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Evernote**väljer **Evernote** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Evernote i hello resultatlistan](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Evernote baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Evernote är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Evernote toobe upprättas.

I Evernote, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Evernote, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Evernote](#create-an-evernote-test-user)**  -toohave en motsvarighet för Britta Simon i Evernote som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Evernote program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Evernote:**

1. I hello Azure-portalen på hello **Evernote** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. På hello **Evernote domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet i IDP initierade läge hello:

    ![URL: er och Evernote domän med enkel inloggning information](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    I hello **identifierare** textruta typen hello URL:`https://www.evernote.com/saml2`

4. Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du inte vill tooconfigure hello programmet hello **SP** initierade läge:

    ![URL: er och Evernote domän med enkel inloggning information](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    I hello **inloggning URL** textruta typen hello URL:`https://www.evernote.com/Login.action`   

5. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. På hello **Evernote Configuration** klickar du på **konfigurera Evernote** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Evernote konfiguration](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. Logga in på webbplatsen Evernote företag som en administratör i en annan webbläsarfönster.

9. Gå för**Admin Console**

    ![Administrationskonsolen](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. Från hello **Admin Console**, gå för**”säkerhet”** och välj **' Single Sign-On-**

    ![SSO-inställning](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. Konfigurera hello följande värden:

    ![Certifikat-inställning](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    a.  **Aktivera enkel inloggning:** enkel inloggning är aktiverad som standard (klicka på **inaktivera enkel inloggning** tooremove hello SSO krav)

    b. Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **SAML HTTP-begäran URL** textruta.

    c. Öppna hello hämtat certifikat från Azure AD i en anteckningar och kopiera hello innehåll inklusive ”BEGIN CERTIFICATE” och ”END CERTIFICATE” och klistra in den i hello **X.509-certifikat** textruta. 

    d.Click **spara ändringar**

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-an-evernote-test-user"></a>Skapa en testanvändare Evernote

I ordning tooenable Azure AD-användare toolog i Evernote, måste de etableras i Evernote.  
Hello gäller Evernote är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour Evernote företagets webbplats som administratör.

2. Klicka på hello **Admin Console**.

    ![Administrationskonsolen](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. Från hello **Admin Console**, gå för**'Lägg till användare ”**.

    ![Lägg till testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. **Lägg till teammedlemmar** i hello **e-post** textruta Skriv hello e-postadressen för användarkontot och klicka på **bjuda in.**

    ![Lägg till testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. När inbjudan har skickats får hello Azure Active Directory användare ett e-tooaccept hello inbjudan.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEvernote.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooEvernote utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Evernote**.

    ![Hej Evernote länken i listan med program hello](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få inloggade tooyour Evernote programmet när du klickar på hello Evernote panelen i hello åtkomstpanelen. Du måste logga in som en organisationskonto men måste toolog in med ditt eget konto. 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png

