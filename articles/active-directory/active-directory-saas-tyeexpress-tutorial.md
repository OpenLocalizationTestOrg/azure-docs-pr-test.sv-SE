---
title: "Självstudier: Azure Active Directory-integrering med T & E Express | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och T & E Express."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: 9a568ace8dbc75fadbf37554996b1b597a813d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>Självstudier: Azure Active Directory-integrering med T & E Express

I kursen får du lära dig hur toointegrate T & E Express med Azure Active Directory (AD Azure).

Integrera T & E Express med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Tool & E Express
- Du kan aktivera din användare tooautomatically get inloggade Tool & E Express (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med T & E Express, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En T & E Express enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till d & E Express från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-te-express-from-hello-gallery"></a>Att lägga till d & E Express från hello-galleriet
tooconfigure hello integrering av d & E Express i Azure AD, behöver du tooadd T & E Express hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd T & E Express från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **T & E Express**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_search.png)

5. Markera hello resultat på panelen **T & E Express**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med T & E Express baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i T & E Express är tooa i Azure AD. Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i T & E Express behov toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i T & E Express.

tooconfigure och testa Azure AD enkel inloggning med T & E Express, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare T & E Express](#creating-a-te-express-test-user)**  -toohave en motsvarighet för Britta Simon i T & E Express som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i T & E Express-program.

**tooconfigure Azure AD enkel inloggning med T & E Express, utför följande steg hello:**

1. I hello Azure Management portal på hello **T & E Express** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

3. På hello **& E Express domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    a. I hello **identifierare** textruta hello TYPVÄRDE som:`https://<domain>.tyeexpress.com`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL. Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare. Kontakta [T & E Express supportteam](http://www.tyeexpress.com/contacto.aspx) tooget dessa värden.

5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_400.png)

8. tooconfigure enkel inloggning på **T & E snabb** sida, inloggning toohello T & E express program utan SAML enkel inloggning med administratörsautentiseringsuppgifter.

9. Under hello **Admin** klickar du på **SAML domän** tooOpen hello SAML inställningssidan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tye-SAML.png)

10. Välj hello **Activar(Activate)** alternativet från **nr** för**SI(Yes)**. I hello **identitet providern Metadata** textruta klistra in hello metadata XML som du har donwloaded från Azure-portalen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tyeAdmin.png)

11. Klicka på hello **Guardar(Save)** knappen toosave hello inställningar. 


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-te-express-test-user"></a>Skapa en testanvändare T & E Express

I ordning tooenable Azure AD-användare toolog i T & E Express, måste de etableras i T & E Express.  
Vid T & E Express är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour T & E Express företagets plats som en administratör.

2. Klicka på huvudsidan för användare tooopen hello användare under Admin-taggen.

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-adminusers.png)

3. På startsidan för hello, klickar du på  **+**  tooadd hello användare.

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-usershome.png)

4. Ange alla obligatoriska hello-information som frågan i hello form och klicka på hello spara knappen toosave hello information.

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-usersadd.png)

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access Tool & E Express.

![Tilldela användare][200] 

**tooassign Britta Simon Tool & E Express, utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **T & E Express**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour T & E Express programmet när du klickar på hello T & E Express panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_203.png

