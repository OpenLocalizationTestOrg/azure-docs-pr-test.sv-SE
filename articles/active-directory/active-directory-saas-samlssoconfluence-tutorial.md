---
title: "Självstudier: Azure Active Directory-integrering med SAML SSO för antal samverkande resolution GmbH | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAML SSO för antal samverkande resolution GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: fe50636709857ec49023e24bdc8c6cd8c58e3c7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Självstudier: Azure Active Directory-integrering med SAML SSO för antal samverkande resolution GmbH

I kursen får du lära dig hur toointegrate SAML SSO för antal samverkande resolution GmbH med Azure Active Directory (AD Azure).

Integrera SAML SSO för antal samverkande resolution GmbH med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSAML SSO för antal samverkande resolution GmbH
- Du kan aktivera din användare tooautomatically get inloggade tooSAML SSO för antal samverkande resolution GmbH (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SAML SSO för antal samverkande resolution GmbH, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En SAML SSO för växer samman av upplösning GmbH enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SAML SSO för antal samverkande resolution GmbH från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-hello-gallery"></a>Att lägga till SAML SSO för antal samverkande resolution GmbH från hello-galleriet

tooconfigure hello integrering av SAML SSO för antal samverkande resolution GmbH i Azure AD, behöver du tooadd SAML SSO för antal samverkande resolution GmbH hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SAML SSO för antal samverkande resolution GmbH från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **SAML SSO för antal samverkande resolution GmbH**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. Markera hello resultat på panelen **SAML SSO för antal samverkande resolution GmbH**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAML SSO för antal samverkande resolution GmbH baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow användare vilka hello motsvarighet i SAML SSO för antal samverkande resolution GmbH är tooa användare i Azure AD. Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i SAML SSO för antal samverkande resolution GmbH måste toobe upprättas.

SAML SSO för antal samverkande resolution GmbH, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med SAML SSO för antal samverkande resolution GmbH, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en SAML SSO för växer samman av upplösning GmbH testanvändare](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  -toohave en motsvarighet för Britta Simon SAML SSO för antal samverkande resolution GmbH som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din SAML SSO för antal samverkande resolution GmbH program.

**tooconfigure Azure AD enkel inloggning med SAML SSO för antal samverkande resolution GmbH, utför följande steg hello:**

1. I hello Azure-portalen på hello **SAML SSO för antal samverkande resolution GmbH** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. På hello **SAML SSO växer samman resolution GmbH domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`

4. Kontrollera **visa avancerade inställningar för URL: en**. Om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Kontakta [SAML SSO för antal samverkande resolution GmbH klienten supportteam](https://www.resolution.de/go/support) tooget dessa värden. 

5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_400.png)  
    
7. Logga in tooyour i ett annat webbläsarfönster **SAML SSO för antal samverkande upplösning GmbH admin Portal** som administratör.

8. Hovra över kugge och klicka på hello **tillägg**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon1.png)

9. Du kan omdirigerade tooAdministrator sidan. Ange hello lösenord och klicka **Bekräfta** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon2.png)

10. Under **ATLASSIAN MARKETPLACE** klickar du på **söka efter nya tillägg**. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon.png)

11. Sök **SAML enkel inloggning (SSO) för antal samverkande** och på **installera** knappen tooinstall hello ny SAML-plugin-programmet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon7.png)

12. hello plugin installationen startar. Klicka på **Stäng**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon8.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon9.png)

13. Klicka på **Hantera**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon10.png)
    
14. Klicka på **konfigurera** tooconfigure hello nytt plugin-program.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon11.png)

15. Den här nya plugin-program även finns under **användare och säkerhet** fliken.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon3.png)
    
16. På **SAML SingleSignOn Plugin-konfiguration** klickar du på **lägga till ytterligare identitetsleverantör** knappen tooconfigure hello inställningar av identitetsleverantören.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon4.png)

17. Utför följande steg på den här sidan:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon5.png)
 
    a. Lägg till **namn** av hello identitetsprovider (t.ex Azure AD).
    
    b. Lägg till **beskrivning** av hello identitetsprovider (t.ex Azure AD).

    c. Klicka på **XML** och välj hello **Metadata** -fil som du har hämtat från Azure-portalen.

    d. Klicka på **belastningen** knappen.

    e. Den läser hello IdP metadata och fyller hello fält som är markerade i hello skärmbild.   
18. Klicka på **Spara inställningar** knappen toosave hello inställningar.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon6.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>Skapa en SAML SSO för växer samman av upplösning GmbH testanvändare

tooenable Azure AD-användare toolog i tooSAML enkel inloggning för antal samverkande resolution GmbH de måste etableras i SAML SSO för antal samverkande resolution GmbH.  
SAML SSO för antal samverkande resolution GmbH är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour SAML SSO för växer samman av upplösning GmbH företagets plats som administratör.

2. Hovra över kugge och klicka på hello **Användarhantering**.

    ![Lägga till medarbetare](./media/active-directory-saas-samlssoconfluence-tutorial/user1.png) 

3. Under avsnittet för användare, klickar du på **lägga till användare** fliken. På hello **”Lägg till en användare”** dialogrutan utför hello följande steg:

    ![Lägga till medarbetare](./media/active-directory-saas-samlssoconfluence-tutorial/user2.png) 

    a. I hello **användarnamn** textruta hello e-post för användare som Britta Simon.

    b. I hello **fullständiga namn** textruta typen hello användarens fullständiga namn som Britta Simon.

    c. I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.

    d. I hello **lösenord** textruta hello lösenordstyp för Britta Simon.

    e. Klicka på **Bekräfta lösenord** hello lösenord.
    
    f. Klicka på **Lägg till** knappen.    

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAML SSO för antal samverkande resolution GmbH.

![Tilldela användare][200] 

**tooassign Britta Simon tooSAML SSO för antal samverkande resolution GmbH, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SAML SSO för antal samverkande resolution GmbH**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello SAML SSO för växer samman av upplösning GmbH panelen i hello åtkomstpanelen bör du hämta automatiskt inloggade tooyour SAML SSO för antal samverkande resolution GmbH program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_203.png

