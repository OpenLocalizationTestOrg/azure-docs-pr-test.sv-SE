---
title: "Självstudier: Azure Active Directory-integrering med NetDocuments | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och NetDocuments."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ee9887553595a2492642aed4cb4abcd11d9cf599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Självstudier: Azure Active Directory-integrering med NetDocuments

I kursen får du lära dig hur toointegrate NetDocuments med Azure Active Directory (AD Azure).

Integrera NetDocuments med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooNetDocuments
- Du kan aktivera din användare tooautomatically get inloggade tooNetDocuments (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med NetDocuments, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En NetDocuments enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till NetDocuments från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-netdocuments-from-hello-gallery"></a>Att lägga till NetDocuments från hello-galleriet
tooconfigure hello integrering av NetDocuments i Azure AD, behöver du tooadd NetDocuments hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd NetDocuments från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **NetDocuments**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_search.png)

5. Markera hello resultat på panelen **NetDocuments**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med NetDocuments baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i NetDocuments är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i NetDocuments toobe upprättas.

I NetDocuments, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med NetDocuments, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare NetDocuments](#creating-a-netdocuments-test-user)**  -toohave en motsvarighet för Britta Simon i NetDocuments som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt NetDocuments program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med NetDocuments:**

1. I hello Azure-portalen på hello **NetDocuments** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

3. På hello **NetDocuments domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och Reply-URL. Kontakta [NetDocuments supportteam](https://support.netdocuments.com/hc/) tooget dessa värden.
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_general_400.png)

6. Logga in på webbplatsen NetDocuments företag som en administratör i en annan webbläsarfönster.

7. Gå för**Admin**.

8. Klicka på **Lägg till och ta bort användare och grupper**.
   
    ![Databasen](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "databasen")

9. Klicka på **konfigurera avancerade autentiseringsalternativ**.
    
    ![Konfigurera avancerade autentiseringsalternativ](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "konfigurera avancerade autentiseringsalternativ")

10. På hello **federerad identitet** dialogrutan utföra hello följande steg:
   
    ![Federerad Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "federerad Identitty")
   
    a. Som **federerad identitet servertyp**väljer **Active Directory Federation Services**.
   
    b. Klicka på **Välj fil**, tooupload hello hämtas metadatafil som du har hämtat från Azure-portalen.
   
    c. Klicka på **OK**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-netdocuments-test-user"></a>Skapa en testanvändare NetDocuments

tooenable Azure AD-användare toolog i tooNetDocuments, måste de etableras i NetDocuments.  
Hello gäller NetDocuments är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Att använda på tooyour **NetDocuments** företagets webbplats som administratör.

2. Hello-menyn överst hello **Admin**.
   
    ![Admin](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "Admin")

3. Klicka på **Lägg till och ta bort användare och grupper**.
   
    ![Databasen](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "databasen")

4. I hello **e-postadress** textruta typen hello e-postadress för ett giltigt Azure Active Directory-konto du vill tooprovision och klicka sedan på **Lägg till användare**.
   
    ![E-postadress](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "e-postadress")
   
   >[!NOTE]
   >hello Azure Active Directory användare får ett e-postmeddelande som innehåller en länk tooconfirm hello-konto innan den aktiveras. Du kan använda något annat NetDocuments användarens konto skapas verktyg eller API: er som tillhandahålls av NetDocuments tooprovision Azure Active Directory användarkonton.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNetDocuments.

![Tilldela användare][200] 

**tooassign Britta Simon tooNetDocuments utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **NetDocuments**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour NetDocuments programmet när du klickar på hello NetDocuments panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_203.png

