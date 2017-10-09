---
title: "Självstudier: Azure Active Directory-integrering med Salesforce | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1d848518ee30910e051cdc4746c599219f3b5a3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Självstudier: Azure Active Directory-integrering med Salesforce

I kursen får du lära dig hur toointegrate Salesforce med Azure Active Directory (AD Azure).

Integrera Salesforce med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooSalesforce
- Du kan aktivera din användare tooautomatically get inloggade tooSalesforce (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Salesforce måste hello följande objekt:

- En Azure AD-prenumeration
- En Salesforce enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Salesforce från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-salesforce-from-hello-gallery"></a>Att lägga till Salesforce från hello-galleriet
tooconfigure hello integrering av Salesforce i Azure AD, behöver du tooadd Salesforce hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Salesforce från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Salesforce**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. Markera hello resultat på panelen **Salesforce**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Salesforce baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Salesforce är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Salesforce toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Salesforce.

tooconfigure och testa Azure AD enkel inloggning med Salesforce, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Salesforce](#creating-a-salesforce-test-user)**  -toohave en motsvarighet för Britta Simon i Salesforce som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Salesforce-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Salesforce:**

1. I hello Azure-portalen på hello **Salesforce** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. På hello **Salesforce-domänen och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster: 
   * Enterprise-konto:`https://<subdomain>.my.salesforce.com`
   * Utvecklarkonto för:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE] 
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL. Kontakta [Salesforce klienten supportteamet](https://help.salesforce.com/support) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. På hello **Salesforce Configuration** klickar du på **konfigurera Salesforce** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.** 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS>
7.  Öppna en ny flik i webbläsaren och logga in tooyour Salesforce-administratörskonto.

8.  Under hello **administratör** navigeringsfönstret klickar du på **säkerhetsåtgärder** tooexpand hello relaterade avsnitt. Klicka på **inställningar för enkel inloggning**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  På hello **inställningar för enkel inloggning** klickar du på hello **redigera** knappen.
    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)

      > [!NOTE]
      > Om du är tooenable enkel inloggning inställningarna för ditt Salesforce-konto kan du behöva toocontact [Salesforce klienten supportteamet](https://help.salesforce.com/support). 

10. Välj **SAML aktiverat**, och klicka sedan på **spara**.

      ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. tooconfigure din SAML enkel inloggning klickar du på **ny**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. På hello **SAML enkel inloggning inställningen redigera** kontrollerar hello följande konfigurationer:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. För hello **namnet** skriver du ett eget namn för den här konfigurationen. Att tillhandahålla ett värde för **namn** automatiskt fylla i hello **API-namnet** textruta.

    b. Klistra in **SMAL enhets-ID** värdet till hello **utfärdaren** i Salesforce.

    c. I hello **enhets-Id textruta**, ange ditt Salesforce-domännamn med hello följer mönstret:
      
      * Enterprise-konto:`https://<subdomain>.my.salesforce.com`
      * Utvecklarkonto för:`https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Klicka på **Bläddra** eller **Välj fil** tooopen hello **Välj fil tooUpload** dialogrutan Välj ditt Salesforce-certifikat och klicka sedan på **öppna**tooupload hello certifikat.

    e. För **SAML identitetstyp**väljer **Assertion innehåller användarens salesforce.com användarnamn**.

    f. För **SAML identitet plats**väljer **identitet är i hello NameIdentifier elementet i hello ämne instruktionen**

    g. Klistra in **inloggning tjänst-URL för enkel** till hello **identitet providern inloggnings-URL** i Salesforce.
    
    h. För **providern initierade begära Tjänstbindning**väljer **HTTP-omdirigering**.
    
    Jag. Klicka slutligen på **spara** tooapply SAML enkel inloggning inställningarna.

13. Klicka på hello vänstra navigeringsfönstret i Salesforce **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. Bläddra nedåt toohello **Autentiseringskonfiguration** avsnittet och klicka på hello **redigera** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. I hello **Autentiseringstjänsten** avsnittet Välj hello eget namn för konfigurationen av SAML SSO och klicka sedan på **spara**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Om mer än en autentiseringstjänst väljs användare kan ange tooselect vilka Autentiseringstjänsten de som toosign med vid initiering av enkel inloggning tooyour Salesforce-miljö. Om du inte vill toohappen, så du bör **lämnar du alla andra autentiseringstjänster avmarkerat**.
<CE>    
> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klickar du på **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello vänstra navigeringsfönstret i hello **Azure-portalen**, klickar du på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-salesforce-test-user"></a>Skapa en testanvändare Salesforce

I det här avsnittet skapas en användare som kallas Britta Simon i Salesforce. Salesforce stöder just-in-time-allokering som är aktiverad som standard.
Det finns ingen åtgärd objekt i det här avsnittet. Om en användare inte redan finns i Salesforce, skapas en ny när du försöker tooaccess Salesforce.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSalesforce.

![Tilldela användare][200] 

**tooassign Britta Simon tooSalesforce utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Salesforce**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

tootest enkel inloggning inställningarna, öppna hello åtkomstpanelen på [https://myapps.microsoft.com](https://myapps.microsoft.com/)sedan logga in på hello testkonto och klicka på **Salesforce**.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

