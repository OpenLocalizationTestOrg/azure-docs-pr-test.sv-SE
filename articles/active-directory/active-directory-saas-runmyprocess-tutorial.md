---
title: "Självstudier: Azure Active Directory-integrering med RunMyProcess | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och RunMyProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f02acda015aeb8d131d8e3ef88bf50c4e8e94750
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Självstudier: Azure Active Directory-integrering med RunMyProcess

I kursen får du lära dig hur toointegrate RunMyProcess med Azure Active Directory (AD Azure).

Integrera RunMyProcess med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooRunMyProcess
- Du kan aktivera din användare tooautomatically get inloggade tooRunMyProcess (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med RunMyProcess, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En RunMyProcess enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här:[utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till RunMyProcess från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-runmyprocess-from-hello-gallery"></a>Att lägga till RunMyProcess från hello-galleriet
tooconfigure hello integrering av RunMyProcess i Azure AD, behöver du tooadd RunMyProcess hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd RunMyProcess från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **RunMyProcess**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_search.png)

5. Markera hello resultat på panelen **RunMyProcess**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RunMyProcess baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i RunMyProcess är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i RunMyProcess toobe upprättas.

I RunMyProcess, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med RunMyProcess, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare RunMyProcess](#creating-a-runmyprocess-test-user)**  -toohave en motsvarighet för Britta Simon i RunMyProcess som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt RunMyProcess program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med RunMyProcess:**

1. I hello Azure-portalen på hello **RunMyProcess** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

3. På hello **RunMyProcess domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://live.runmyprocess.com/live/<tenant id>`

    > [!NOTE] 
    > hello-värdet är inte verkliga. Hello uppdateringsvärde med hello faktiska inloggnings-URL. Kontakta [RunMyProcess klienten supportteamet](mailto:support@runmyprocess.com) tooget hello värde. 

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_400.png)

6. På hello **RunMyProcess Configuration** klickar du på **konfigurera RunMyProcess** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

7. I en annan webbläsarfönstret, inloggning tooyour RunMyProcess klient som administratör.

8. I det vänstra navigeringsfönstret klickar du på **konto** och välj **Configuration**.
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

9. Gå för**autentiseringsmetod** avsnittet och utföra nedanstående steg:
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    a. Som **metoden**väljer **SSO med Samlv2**. 

    b. I hello **SSO omdirigering** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.

    c. I hello **logga ut omdirigering** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.

    d. I hello **Format för namn-Id** textruta hello TYPVÄRDE av **identifierare namnformat** som **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.

    e. Kopiera hello innehållet hello hämtat certifikatfilen och klistra in den i hello **certifikat** textruta. 
 
    f. Klicka på **spara** ikon.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-runmyprocess-test-user"></a>Skapa en testanvändare RunMyProcess

I ordning tooenable Azure AD-användare toolog i tooRunMyProcess, måste de etableras i RunMyProcess. Hello gäller RunMyProcess är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour RunMyProcess företagets webbplats som administratör.

2. Klicka på **konto** och välj **användare** i vänstra navigeringsfönstret och klicka på **ny användare**.
   
    ![Ny användare](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "ny användare")

3. I hello **användarinställningar** avsnittet, utföra hello följande steg:
   
    ![Profilen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "profil") 
  
    a. Typen hello **namn** och **e-post** av ett giltigt Azure AD-kontot som du vill använda tooprovision hello relaterade textrutor. 

    b. Välj en **IDE språk**, **språk**, och **profil**. 

    c. Välj **skicka konto skapa e-toome**. 

    d. Klicka på **Spara**.
   
    >[!NOTE]
    >Du kan använda något annat RunMyProcess användarens konto skapas verktyg eller API: er som tillhandahålls av RunMyProcess tooprovision Azure Active Directory användarkonton. 
    > 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRunMyProcess.

![Tilldela användare][200] 

**tooassign Britta Simon tooRunMyProcess utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **RunMyProcess**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour RunMyProcess programmet när du klickar på hello RunMyProcess panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png

