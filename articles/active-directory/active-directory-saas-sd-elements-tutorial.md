---
title: "Självstudier: Azure Active Directory-integrering med SD element | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SD-element."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 77949e41beb541c9fe8147b1eb2e7995e05bd753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Självstudier: Azure Active Directory-integrering med SD-element

I kursen får du lära dig hur toointegrate SD-element med Azure Active Directory (AD Azure).

Integrera SD-element med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSD element
- Du kan aktivera din användare tooautomatically get inloggade tooSD element (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SD-element måste hello följande objekt:

- En Azure AD-prenumeration
- Ett SD-element enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SD-element från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sd-elements-from-hello-gallery"></a>Att lägga till SD-element från hello-galleriet
tooconfigure hello integrering av SD-element i Azure AD, behöver du tooadd SD-element från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd SD-element från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **SD element**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. Markera hello resultat på panelen **SD element**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med SD-element som baseras på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SD-element är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i SD-element toobe upprättas.

I SD-element, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med SD-element måste toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare SD element](#creating-a-sd-elements-test-user)**  -toohave en motsvarighet för Britta Simon i SD-element som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för SD-element.

**Utför följande hello tooconfigure Azure AD enkel inloggning med SD-element:**

1. I hello Azure-portalen på hello **SD element** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. På hello **SD element domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [SD element supportteam](mailto:support@sdelements.com) tooget dessa värden.

4. Programmet SD-element förväntas hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello **”användarattribut”** fliken av programmet hello. hello följande skärmbild visar ett exempel för det här.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg: 

    | Attributets namn | Attributvärdet |
    | --- | --- |
    | E-post |User.Mail |
    | Förnamn |User.givenName |
    | Efternamn |User.surname |

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.

    d. Klicka på **OK**.
 
6. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. På hello **SD element Configuration** klickar du på **konfigurera SD-element** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. tooget enkel inloggning aktiverad, kontakta din [SD element supportteam](mailto:support@sdelements.com) och ge dem hello hämtade certifikatfil. 

10. I ett annat fönster i webbläsaren, inloggning tooyour SD element klient som administratör.

11. Hello-menyn överst hello **System**, och sedan **enkel inloggning**. 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. På hello **inställningar för enkel inloggning** dialogrutan utföra hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Som **SSO typen**väljer **SAML**.
   
    b. I hello **identitet providern enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen. 
   
    c. I hello **identitet providern enkel inloggning** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen. 
   
    d. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-sd-elements-test-user"></a>Skapa en testanvändare SD-element

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i SD-element. Skapa element SD-användare är en manuell aktivitet i hello fallet SD-element.

**toocreate Britta Simon i SD-element, utför följande steg hello:**

1. I ett webbläsarfönster, inloggning tooyour SD element företagets webbplats som administratör.

2. Hello-menyn överst hello **Användarhantering**, och sedan **användare**.
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. Klicka på **lägga till nya användare**.
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. På hello **Lägg till nya användare** dialogrutan utföra hello följande steg:
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. I hello **e-post** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .
   
    b. I hello **Förnamn** textruta anger hello först namnet på användaren som **Britta**.
   
    c. I hello **efternamn** textruta ange hello efternamn för användaren som **Simon**.
   
    d. Som **rollen**väljer **användaren**. 
   
    e. Klicka på **skapa användare**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSD element.

![Tilldela användare][200] 

**tooassign Britta Simon tooSD element, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SD element**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.
  
Du bör få automatiskt inloggade tooyour SD element programmet när du klickar på hello SD element panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

