---
title: "Självstudier: Azure Active Directory-integrering med TargetProcess | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TargetProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 05c574e2c18d7f73edc6c094093a6e59d46b8e6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a>Självstudier: Azure Active Directory-integrering med TargetProcess

I kursen får du lära dig hur toointegrate TargetProcess med Azure Active Directory (AD Azure).

Integrera TargetProcess med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooTargetProcess
- Du kan aktivera din användare tooautomatically get inloggade tooTargetProcess (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med TargetProcess, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En TargetProcess enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till TargetProcess från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="add-targetprocess-from-hello-gallery"></a>Lägg till TargetProcess från hello-galleriet
tooconfigure hello integrering av TargetProcess i Azure AD, behöver du tooadd TargetProcess hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd TargetProcess från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **TargetProcess**väljer **TargetProcess** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Lägg till TargetProcess från galleriet](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TargetProcess baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TargetProcess är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TargetProcess toobe upprättas.

I TargetProcess, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med TargetProcess, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare TargetProcess](#create-a-targetprocess-test-user)**  -toohave en motsvarighet för Britta Simon i TargetProcess som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TargetProcess program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TargetProcess:**

1. I hello Azure-portalen på hello **TargetProcess** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![SAML-baserade inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. På hello **TargetProcess domän och URL: er** avsnittet, utföra hello följande steg:

    ![Avsnittet TargetProcess domän och URL: er](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.tpondemand.com/`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.tpondemand.com/`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [TargetProcess klienten supportteamet](mailto:support@targetprocess.com) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. Klicka på **spara** knappen.

    ![Knappen Spara](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. På hello **TargetProcess Configuration** klickar du på **konfigurera TargetProcess** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![TargetProcess konfigurationsavsnitt](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. Inloggning tooyour TargetProcess programmet som administratör.

8. Hello-menyn överst hello **installationsprogrammet**.
   
    ![Konfiguration](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. Klicka på **inställningar**.
   
    ![Inställningar](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. Klicka på **enkel inloggning**.
   
    ![Klicka på enkel inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. På hello enkel inloggning dialogrutan Inställningar, utföra hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    a. Klicka på **aktivera enkel inloggning**.
    
    b. I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    c. Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **certifikat** textruta.
    
    d. Klicka på **aktivera JIT etablering**.

    e. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![toodisplay hello lista över användare](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Knappen Lägg till](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Avsnittet för användare](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-targetprocess-test-user"></a>Skapa en testanvändare TargetProcess

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i TargetProcess.

TargetProcess stöder just-in-time-etablering. Du har redan aktiverats i [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).

Det finns ingen åtgärd objekt i det här avsnittet.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTargetProcess.

![Tilldela användare][200] 

**tooassign Britta Simon tooTargetProcess utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **TargetProcess**.

    ![TargetProcess i listan över appar](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour TargetProcess programmet när du klickar på hello TargetProcess panelen i hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png

