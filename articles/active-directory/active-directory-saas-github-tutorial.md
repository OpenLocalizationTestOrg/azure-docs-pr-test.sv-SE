---
title: "Självstudier: Azure Active Directory-integrering med GitHub | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och GitHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 688779de4e6627e49c0e3e8a7576f2f8c7a81ab1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Självstudier: Azure Active Directory-integrering med GitHub

I kursen får du lära dig hur toointegrate GitHub med Azure Active Directory (AD Azure).

Integrera GitHub med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooGitHub
- Du kan aktivera din användare tooautomatically get inloggade tooGitHub (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med GitHub, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En GitHub enkel inloggning på aktiverade prenumeration


> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.


tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till GitHub från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning


## <a name="adding-github-from-hello-gallery"></a>Att lägga till GitHub från hello-galleriet
tooconfigure hello integrering av GitHub i Azure AD, behöver du tooadd GitHub hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd GitHub från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **GitHub.com**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. Markera hello resultat på panelen **GitHub**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med GitHub baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i GitHub är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i GitHub toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i GitHub.

tooconfigure och testa Azure AD enkel inloggning med GitHub, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare i GitHub](#creating-a-GitHub-test-user)**  -toohave en motsvarighet för Britta Simon i GitHub som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt GitHub-program.

**Utför följande hello tooconfigure Azure AD enkel inloggning med GitHub:**

1. I hello Azure Management portal på hello **GitHub** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. På hello **GitHub domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    a. I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`https://github.com/orgs/<entity-id>/sso`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://github.com/orgs/<entity-id>`

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska rör-URL och identifierare. Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare. Gå tooGitHub Admin avsnittet tooretrieve dessa värden. 

4. På hello **användarattribut** väljer **användar-ID** som user.mail.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. På hello **GitHub Configuration** klickar du på **konfigurera GitHub** tooopen **konfigurera inloggning** fönster.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. Logga in på webbplatsen GitHub organisation som en administratör i en annan webbläsarfönster.

12. Navigera för**inställningar** och på **säkerhet**

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. Kontrollera hello **aktivera SAML-autentisering** rutan avslöja hello enkel inloggning konfigurationsfält. Använd sedan hello enkel inloggning URL värdet tooupdate hello enkel inloggnings-URL på Azure AD-konfiguration.

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. Konfigurera hello följande fält:

    a. **Logga in URL: en**: Ange **SAML enkel inloggning Tjänstwebbadress** från hello **konfigurera GitHub** avsnittet om Azure AD

    b. **Utfärdaren**: Ange **SAML enhets-ID** från hello **konfigurera GitHub** avsnittet om Azure AD

    c. **Certifikatets offentliga**: öppna hello hämtat certifikat från Azure AD i en anteckningar och kopiera hello innehåll inklusive ”BEGIN CERTIFICATE” och ”END CERTIFICATE”

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. Klicka på **Test SAML-konfiguration** tooconfirm som inga verifieringsfel eller fel under enkel inloggning.

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. Klicka på **spara**

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**. 


### <a name="creating-a-github-test-user"></a>Skapa en testanvändare i GitHub

I ordning tooenable Azure AD-användare toolog i GitHub, måste de etableras i GitHub.  
Hello gäller GitHub är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour GitHub företagets webbplats som administratör.

2. Klicka på **personer**.

    ![Personer](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "personer")

3. Klicka på **inbjudan medlem**.

    ![Bjuda in användare](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "bjuda in användare")

4. På hello **inbjudan medlem** dialogrutan utför hello följande steg:

    a. I hello **e-post** textruta typen hello e-postadress för Britta Simon konto.

    ![Bjuda in](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "bjuda in")
    
    b. Klicka på **skicka inbjudan**.

    ![Bjuda in](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "bjuda in")

    > [!NOTE]
    > hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooGitHub sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooGitHub utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **GitHub.com**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    


### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få inloggade tooyour GitHub programmet när du klickar på hello GitHub-panelen i hello åtkomstpanelen. Du måste logga in som en organisationskonto men måste toolog in med ditt eget konto.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
