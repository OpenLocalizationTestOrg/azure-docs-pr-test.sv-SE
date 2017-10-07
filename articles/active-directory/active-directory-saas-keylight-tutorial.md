---
title: "Självstudier: Azure Active Directory-integrering med LockPath Keylight | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LockPath Keylight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 5485aeb068ba6fbdb4ea9bfc89d401e00c5b1d29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Självstudier: Azure Active Directory-integrering med LockPath Keylight

I kursen får du lära dig hur toointegrate LockPath Keylight med Azure Active Directory (AD Azure).

Integrera LockPath Keylight med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooLockPath Keylight
- Du kan aktivera din användare tooautomatically get inloggade tooLockPath Keylight (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med LockPath Keylight, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En LockPath Keylight enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till LockPath Keylight från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-lockpath-keylight-from-hello-gallery"></a>Att lägga till LockPath Keylight från hello-galleriet
tooconfigure hello integrering av LockPath Keylight i Azure AD, behöver du tooadd LockPath Keylight hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd LockPath Keylight från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **LockPath Keylight**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. Markera hello resultat på panelen **LockPath Keylight**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LockPath Keylight baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LockPath Keylight är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LockPath Keylight toobe upprättas.

Tilldela hello värdet för hello i LockPath Keylight **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med LockPath Keylight, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare LockPath Keylight](#creating-a-lockpath-keylight-test-user)**  -toohave en motsvarighet för Britta Simon i LockPath Keylight som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LockPath Keylight.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LockPath Keylight:**

1. I hello Azure-portalen på hello **LockPath Keylight** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. På hello **LockPath Keylight domän och URL: er** avsnittet, utför följande steg hello::

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.keylightgrc.com/`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.keylightgrc.com`

    c. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.keylightgrc.com/Login.aspx`
    
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Kontakta [LockPath Keylight klienten supportteamet](https://www.lockpath.com/contact/) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. På hello **LockPath Keylight Configuration** klickar du på **konfigurera LockPath Keylight** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. tooenable SSO i LockPath Keylight utför hello följande steg:
   
    a. Inloggning tooyour LockPath Keylight kontot som administratör.
    
    b. Hello-menyn överst hello **Person**, och välj **Keylight installationsprogrammet**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. I hello treeview hello vänster, klickar du på **SAML**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. På hello **SAML inställningar** dialogrutan klickar du på **redigera**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/404.png) 

8. På hello **redigera inställningar för SAML** dialogrutan utför hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    a. Ange **SAML-autentisering** för**Active**.

    b. Klistra in hello **SAML enkel inloggning Tjänstwebbadress** värde som du har kopierat från hello Azure-portalen i hello **identitet providern inloggnings-URL** textruta.

    c. Klistra in hello **tjänst-URL för enkel Sign-Out** värde som du har kopierat från hello Azure-portalen i hello **identitet providern logga ut URL** textruta.

    d. Klicka på **Välj fil** tooselect din hämtade LockPath Keylight certifikat, och klicka sedan på **öppna** tooupload hello certifikat.

    e. Ange **användar-Id för SAML plats** för**NameIdentifier element av hello ämne instruktionen**.
    
    f. Ange hello **Keylight Service Provider** med hello följer mönstret: **https://&lt;CompanyName&gt;. keylightgrc.com**.
    
    g. Ange **automatiskt etablera användare** för**Active**.

    h. Ange **automatiskt etablera kontotyp** för**fullständig användaren**.

    Jag. Ange **automatiskt etablera säkerhetsrollen**väljer **standardanvändare med SAML**.
    
    j. Ange **automatiskt etablera säkerhet config**väljer **Standard Användarkonfiguration**.
     
    k. I hello **e-attributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    l. I hello **förnamn attributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    m. I hello **senaste namnattributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
    
    n. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-lockpath-keylight-test-user"></a>Skapa en testanvändare LockPath Keylight

I det här avsnittet skapar du en användare som kallas Britta Simon i LockPath Keylight. LockPath Keylight stöder just-in-time-allokering som är aktiverad som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas vid åtkomst till LockPath Keylight om hello användaren inte finns ännu. 

>[!NOTE]
>Om du behöver toocreate en användare manuellt, måste toocontact hello [LockPath Keylight klienten supportteamet](https://www.lockpath.com/contact/). 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLockPath Keylight.

![Tilldela användare][200] 

**tooassign Britta Simon tooLockPath Keylight, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **LockPath Keylight**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour LockPath Keylight programmet när du klickar på hello LockPath Keylight panelen i hello åtkomstpanelen. 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

