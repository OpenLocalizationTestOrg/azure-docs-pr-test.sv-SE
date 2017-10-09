---
title: "Självstudier: Azure Active Directory-integrering med identifiera | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och identifiera."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: f33fc3959f72f875b8c5c4f0abd4e9b6737ca615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Självstudier: Azure Active Directory-integrering med identifiera

I kursen får du lära dig hur toointegrate känner igen med Azure Active Directory (AD Azure).

Identifiera integrera med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooRecognize
- Du kan aktivera din användare tooautomatically get inloggade tooRecognize (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med identifiera, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En identifiera enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till identifiera från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-recognize-from-hello-gallery"></a>Att lägga till identifiera från hello-galleriet
tooconfigure hello integrering av identifiera i Azure AD, behöver du tooadd identifiera hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd identifiera från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **identifiera**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. Markera hello resultat på panelen **identifiera**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med identifiera baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i identifiera är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i identifiera toobe upprättas.

I identifiera, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med identifiera, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare identifiera](#creating-a-recognize-test-user)**  -toohave en motsvarighet för Britta Simon i identifiera som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i identifiera programmet.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med identifiera:**

1. I hello Azure-portalen på hello **identifiera** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. På hello **URL: er och identifiera domänen** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://recognizeapp.com/<your-domain>/saml/sso`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://recognizeapp.com/<your-domain>`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [identifiera klienten supportteamet](mailto:support@recognizeapp.com) få inloggnings-URL och du kan hämta ID-värde genom att öppna hello URL för tjänstmetadata providern från hello SSO inställningar som beskrivs senare i självstudiekursen hello. . 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. På hello **identifiera Configuration** klickar du på **konfigurera identifiera** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. I en annan webbläsarfönstret, inloggning tooyour identifiera klient som administratör.

8. Klicka på hello övre högra hörnet **menyn**. Gå för**företagsadministratör**.
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. Klicka på hello vänstra navigeringsfönstret **inställningar**.
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. Utför följande steg hello **SSO-inställningarna** avsnitt.
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    a. Som **aktivera enkel inloggning**väljer **på**.

    b. I hello **IDP enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.
    
    c. I hello **mål-url för Sso** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
    
    d. I hello **servicenivåmål mål-url** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen. 
    
    e. Öppna din hämtade **certifikat (Base64)** filen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat** textruta.
    
    f. Klicka på hello **Spara inställningar** knappen. 

11. Bredvid hello **SSO inställningar** avsnittet, kopiera hello URL under **url för tjänstmetadata providern**.
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. Öppna hello **Metadata URL-länk** under en tom webbläsare toodownload hello metadata dokument. Kopiera hello EntityDescriptor value(entityID) från hello-filen och klistra in den i **identifierare** TextBox-kontroll i **URL: er och identifiera domänen avsnittet** på Azure-portalen.
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-recognize-test-user"></a>Skapa en testanvändare identifiera

I ordning tooenable Azure AD-användare toolog till identifiera, måste de etableras i identifiera. Identifiera hello gäller är etablering en manuell aktivitet.

Den här appen stöder inte SCIM etablering men har en annan användare sync som etablerar användare. 

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in på webbplatsen identifiera företag som administratör.

2. Klicka på hello övre högra hörnet **menyn**. Gå för**företagsadministratör**.

3. Klicka på hello vänstra navigeringsfönstret **inställningar**.

4. Utför följande steg hello **användaren Sync** avsnitt.
   
   ![Ny användare](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "ny användare")
   
   a. Som **synkronisering aktiverat**väljer **på**.
   
   b. Som **Välj sync providern**väljer **Microsoft / Office 365**.
   
   c. Klicka på **köra användaren synkronisering**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRecognize.

![Tilldela användare][200] 

**tooassign Britta Simon tooRecognize utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **identifiera**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour identifiera programmet när du klickar på hello identifiera panelen i hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png

