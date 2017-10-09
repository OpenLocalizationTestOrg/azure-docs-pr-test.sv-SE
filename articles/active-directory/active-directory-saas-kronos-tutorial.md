---
title: "Självstudier: Azure Active Directory-integrering med Kronos | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Kronos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 16fd5c203162d10b78f51b00d79017adaf8632c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a>Självstudier: Azure Active Directory-integrering med Kronos

I kursen får du lära dig hur toointegrate Kronos med Azure Active Directory (AD Azure).

Integrera Kronos med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooKronos
- Du kan aktivera din användare tooautomatically get inloggade tooKronos (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Kronos, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En **Kronos arbetsstyrka centrala** SSO aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Kronos från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-kronos-from-hello-gallery"></a>Att lägga till Kronos från hello-galleriet
tooconfigure hello integrering av Kronos i Azure AD, behöver du tooadd Kronos hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Kronos från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Kronos**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_search.png)

5. Markera hello resultat på panelen **Kronos**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kronos baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Kronos är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Kronos toobe upprättas.

I Kronos, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Kronos, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Kronos](#creating-a-kronos-test-user)**  -toohave en motsvarighet för Britta Simon i Kronos som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Kronos program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Kronos:**

1. I hello Azure-portalen på hello **Kronos** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_samlbase.png)

3. På hello **Kronos domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.kronos.net/`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.kronos.net/wfc/navigator/logonWithUID`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [Kronos supportteam](https://www.kronos.in/contact/en-in/form) tooget dessa värden.
 
4. Tillämpningsprogrammet Kronos förväntar hello SAML intyg i ett specifikt format. Arbeta med [Kronos supportteam](https://www.kronos.in/contact/en-in/form) första tooidentify hello rätt användar-ID, som är mappad till hello program. Ta dig hello vägledning om hello-attribut som de vill också toouse för att mappningen.
 
     Microsoft rekommenderar att du använder hello **”NameIdentifier”** attribut som användaridentifierare. Du kan hantera hello värden för attributen från hello **”användarattribut”** avsnitt på sidan för integrering av programmet.
     
     hello följande skärmbild visar ett exempel för det här. Vi har här mappat hello **användar-ID (nameid)** med **ExtractMailPrefix()** funktion **user.userprincipalname**. Detta ger hello prefixvärdet av e-post för hello-användare som är hello unikt användar-ID. Toohello Kronos program skickas i varje lyckat svar. 
     
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_attribute.png)

5. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan:

    a. Välj i listrutan för hello användar-ID, **ExtractMailPrefix**.

    b. I hello **e** listrutan, Välj **user.userprincipalname**.

6. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_certificate.png) 

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_general_400.png)

8. tooconfigure enkel inloggning på **Kronos** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Kronos supportteam](https://www.kronos.in/contact/en-in/form). 

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-kronos-test-user"></a>Skapa en testanvändare Kronos

I det här avsnittet skapar du en användare som kallas Britta Simon i Kronos. Kronos programmet måste alla hello användare toobe etableras i hello program innan du utför enkel inloggning. 

Arbeta med hello [Kronos supportteam](https://www.kronos.in/contact/en-in/form) tooprovision dessa användare till hello program. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooKronos.

![Tilldela användare][200] 

**tooassign Britta Simon tooKronos utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Kronos**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Kronos programmet när du klickar på hello Kronos panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png

