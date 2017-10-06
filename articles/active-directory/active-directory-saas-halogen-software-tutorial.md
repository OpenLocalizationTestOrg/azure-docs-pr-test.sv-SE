---
title: "Självstudier: Azure Active Directory-integrering med Halogen programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Halogen programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bdb67de713771d6e306f287c4b13895f6336f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Självstudier: Azure Active Directory-integrering med Halogen programvara

I kursen får du lära dig hur toointegrate Halogen programvara med Azure Active Directory (AD Azure).

Integrera Halogen programvara med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooHalogen programvara
- Du kan aktivera din användare tooautomatically get inloggade tooHalogen programvara (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Halogen programvara, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Halogen programvara enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning

I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Halogen programvara från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-halogen-software-from-hello-gallery"></a>Lägga till Halogen programvara från hello-galleriet

tooconfigure hello integrering av Halogen programvara i Azure AD, behöver du tooadd Halogen programvara från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd Halogen programvara från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Halogen programvara**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_search.png)

5. Markera hello resultat på panelen **Halogen programvara**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Halogen programvara baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Halogen programvaran är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Halogen programvara toobe upprätta.

Tilldela hello värdet hello Halogen programvaran **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Halogen programvara, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Halogen programvara](#creating-a-halogen-software-test-user)**  -toohave en motsvarighet för Britta Simon Halogen programvara som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Halogen programmet.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Halogen programvara:**

1. I hello Azure-portalen på hello **Halogen programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

3. På hello **Halogen programvara domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://global.hgncloud.com/<companyname>`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret: `https://global.halogensoftware.com/<companyname>`,`https://global.hgncloud.com/<companyname>`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Halogen Programvaruklienten supportteamet](https://support.halogensoftware.com/) tooget dessa värden. 
 


4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_general_400.png)

6. I ett annat webbläsarfönster inloggning tooyour **Halogen programvara** program som administratör.

7. Klicka på hello **alternativ** fliken. 
   
    ![Vad är Azure AD Connect?][12]

8. Hello vänstra navigeringsfönstret, klicka på **SAML-konfiguration**. 
   
    ![Vad är Azure AD Connect?][13]

9. På hello **SAML-konfiguration** utför hello följande steg: 

    ![Vad är Azure AD Connect?][14]

     a. Som **Unik identifierare**väljer **NameID**.

     b. Som **unika identifierare Maps till**väljer **användarnamn**.
  
     c. tooupload metadata för hämtade filer, klickar du på **Bläddra** tooselect hello-fil, och sedan **överför filen**.
 
     d. tootest hello konfiguration, klickar du på **kör Test**. 
    
    >[!NOTE]
    >Du behöver toowait för hello-meddelande ”*hello SAML test har slutförts. Stäng det här fönstret*”. Stäng hello öppnade webbläsarfönstret. Hej **aktivera SAML** kryssrutan är endast aktiverad om hello test har slutförts. 
     
     e. Välj **aktivera SAML**.
    
     f. Klicka på **spara ändringar**. 

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typnamn som **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-halogen-software-test-user"></a>Skapa en testanvändare Halogen programvara

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon Halogen programvaran.

**toocreate en användare som kallas Britta Simon i Halogen programvara, utföra hello följande steg:**

1. Logga in tooyour **Halogen programvara** program som administratör.

2. Klicka på hello **användaren Center** fliken och klicka sedan på **skapa användare**.
   
    ![Vad är Azure AD Connect?][300]  

3. På hello **ny användare** dialogrutan utför hello följande steg:
   
    ![Vad är Azure AD Connect?][301]

    a. I hello **Förnamn** textruta Ange först namnet på hello användaren som **Britta**.
    
    b. I hello **efternamn** textruta Skriv Efternamn för hello användare som **Simon**. 

    c. I hello **användarnamn** textruta typen **Britta Simon**, hello användarnamn som hello Azure-portalen.

    d. I hello **lösenord** textruta, ange ett lösenord för Britta.
    
    e. Klicka på **Spara**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHalogen programvara.

![Tilldela användare][200] 

**tooassign Britta Simon tooHalogen programvara, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Halogen programvara**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Halogen program när du klickar på hello Halogen programvara panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png
