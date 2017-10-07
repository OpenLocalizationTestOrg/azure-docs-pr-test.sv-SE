---
title: "Självstudier: Azure Active Directory-integrering med Hightail | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Hightail."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2b36fcf8d5773255fdf89de2dccdceb95c032bd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Självstudier: Azure Active Directory-integrering med Hightail

I kursen får du lära dig hur toointegrate Hightail med Azure Active Directory (AD Azure).

Integrera Hightail med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooHightail
- Du kan aktivera din användare tooautomatically get inloggade tooHightail (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Hightail, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Hightail enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Hightail från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-hightail-from-hello-gallery"></a>Att lägga till Hightail från hello-galleriet
tooconfigure hello integrering av Hightail i Azure AD, behöver du tooadd Hightail hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Hightail från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Hightail**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. Markera hello resultat på panelen **Hightail**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Hightail baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Hightail är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Hightail toobe upprättas.

I Hightail, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Hightail, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Hightail](#creating-a-hightail-test-user)**  -toohave en motsvarighet för Britta Simon i Hightail som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Hightail program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Hightail:**

1. I hello Azure-portalen på hello **Hightail** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. På hello **Hightail domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     I hello **Reply URL** textruta typen hello URL som:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`

    > [!NOTE] 
    > hello är föregående värde inte verkliga värde. Hello värdet uppdateras med hello faktiska Reply URL, vilket beskrivs senare i hello kursen.
 
4. På hello **Hightail domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    a. Klicka på hello **visa avancerade inställningar för URL: en**.

    b. I hello **logga URL** textruta typen hello URL som:`https://www.hightail.com/loginSSO`

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. Hightail program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello **”Atrribute”** fliken av programmet hello. hello följande skärmbild visar ett exempel för det här. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:
    
    | Attributets namn | Attributvärdet |
    | ------------------- | -------------------- |
    | Förnamn | User.givenName |
    | Efternamn | User.surname |
    | E-post | User.Mail |    
    | Användaridentiteten | User.Mail |
    
    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.

    d. Lämna hello **Namespace** tomt.
    
    e. Klicka på **OK**.

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. På hello **Hightail Configuration** klickar du på **konfigurera Hightail** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    >Innan du konfigurerar hello enkel inloggning på Hightail app kan kan du lista för tillåten din e-postdomän med Hightail team så att alla hello användare som använder den här domänen använda enkel inloggning funktionen.


9. tooget SSO konfigurerats för ditt program måste toosign på tooyour Hightail klient som administratör.
   
    a. Hello hello överst klickar du på menyn hello **konto** fliken och markera **konfigurera SAML**.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    b. Markera kryssrutan för hello av **aktivera SAML-autentisering**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c. Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **SAML Tokensigneringscertifikatet** textruta.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    d. I hello **SAML Authority (identitetsleverantör)** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** kopieras från Azure-portalen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Om du inte vill tooconfigure hello programmet i **IDP initierade läge** Välj **”identitetsprovider (IdP) initierade logga in”**. Om **SP initierade läge** Välj **”ServiceProvider (SP) initierade logga in”**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. Kopiera hello SAML konsument-URL för din instans och klistra in den i **Reply URL** TextBox-kontroll i **Hightail domän och URL: er** avsnitt på Azure-portalen.
    
    g. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-hightail-test-user"></a>Skapa en testanvändare Hightail

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Hightail. 

Det finns ingen åtgärd objekt i det här avsnittet. Hightail stöder just-in-time-användaretablering baserat på hello anpassade anspråk. Om du har konfigurerat hello anpassade anspråk som visas i avsnittet hello  **[konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  ovan, skapas automatiskt en användare i hello-program som det inte finns. 

>[!NOTE]
>Om du behöver toocreate en användare manuellt, måste toocontact hello [Hightail supportteamet](mailto:support@hightail.com). 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHightail.

![Tilldela användare][200] 

**tooassign Britta Simon tooHightail utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Hightail**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Hightail programmet när du klickar på hello Hightail panelen i hello åtkomstpanelen.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

