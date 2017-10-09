---
title: "Självstudier: Azure Active Directory-integrering med plattare filer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och plattare filer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 73ca2613b7bbaf9992ecf624ff5defabaa44f7a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Självstudier: Azure Active Directory-integrering med plattare filer

I kursen får du lära dig hur toointegrate plattare filer med Azure Active Directory (AD Azure).

Integrera plattare filer med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooFlatter filer
- Du kan aktivera din användare tooautomatically hämta inloggade tooFlatter filer (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med plattare filer, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En plattare filer enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till plattare filer från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-flatter-files-from-hello-gallery"></a>Lägga till plattare filer från hello-galleriet
tooconfigure hello integrering av plattare filer i Azure AD, behöver du tooadd plattare filer från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd plattare filer från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **plattare filer**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_search.png)

5. Markera hello resultat på panelen **plattare filer**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med plattare filer baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i plattare filer är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i plattare filer toobe upprättas.

Tilldela hello värdet hello i plattare filer **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med plattare filer, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare plattare filer](#creating-a-flatter-files-test-user)**  -toohave en motsvarighet för Britta Simon i plattare filer som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet plattare filer.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med plattare filer:**

1. I hello Azure-portalen på hello **plattare filer** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

3. På hello **plattare filer domän och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_general_400.png)

6. På hello **plattare filer Configuration** klickar du på **konfigurera plattare filer** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

7. Inloggning tooyour plattare filer programmet som administratör.

8. Klicka på **INSTRUMENTPANELEN**. 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  

9. Klicka på **inställningar**, och utför sedan följande steg på hello hello **företagets** fliken: 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    a. Välj **använder SAML 2.0 för autentisering**.
    
    b. Klicka på **konfigurerar du SAML**.

8. På hello **SAML-konfiguration** dialogrutan utföra hello följande steg: 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    a. I hello **domän** textruta, ange en registrerad domän.
   
    >[!NOTE]
    >Om du inte har en registrerad domän ännu, kontakta din plattare filer supportteam via [ support@flatterfiles.com ](mailto:support@flatterfiles.com). 
    
    b. I **identitet providern URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat formuläret Azure-portalen.
   
    c.  Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **providern identitetscertifikat** textruta.

    d. Klicka på **uppdatering**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-flatter-files-test-user"></a>Skapa en testanvändare plattare filer

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i plattare filer.

**toocreate en användare som kallas Britta Simon i plattare filer, utför hello följande steg:**

1. Logga in tooyour **plattare filer** företagets webbplats som administratör.

2. Hello navigeringsfönstret hello vänster, klicka på **inställningar**, och klicka sedan på hello **användare** fliken.
   
    ![Skapa en plattare filer användare](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Klicka på **lägga till användare**. 

4. På hello **Lägg till användare** dialogrutan utföra hello följande steg:
   
    ![Skapa en plattare filer användare](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. I hello **Förnamn** textruta typen **Britta**.
   
    b. I hello **efternamn** textruta typen **Simon**. 
   
    c. I hello **e-postadress** textruta Skriv Brittas e-postadress i hello Azure-portalen.
   
    d. Klicka på **skicka**.   


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst till tooFlatter filer.

![Tilldela användare][200] 

**tooassign Britta Simon tooFlatter filer, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **plattare filer**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour plattare filer programmet när du klickar på hello plattare filer panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png

