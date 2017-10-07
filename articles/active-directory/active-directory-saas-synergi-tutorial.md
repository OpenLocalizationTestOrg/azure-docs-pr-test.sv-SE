---
title: "Självstudier: Azure Active Directory-integrering med Synergi | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Synergi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 73c970e1-f1ba-420b-b225-414fdf93b140
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5d4a9a596db2a60dda5bcac2c86b88b460a2e2cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-synergi"></a>Självstudier: Azure Active Directory-integrering med Synergi

I kursen får du lära dig hur toointegrate Synergi med Azure Active Directory (AD Azure).

Integrera Synergi med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooSynergi.
- Du kan låta dina användare tooautomatically get inloggade tooSynergi (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Synergi måste hello följande objekt:

- En Azure AD-prenumeration
- En Synergi enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Synergi från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-synergi-from-hello-gallery"></a>Att lägga till Synergi från hello-galleriet
tooconfigure hello integrering av Synergi i Azure AD, behöver du tooadd Synergi hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Synergi från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Synergi**väljer **Synergi** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Synergi i hello resultatlistan](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Synergi baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Synergi är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Synergi toobe upprättas.

I Synergi, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Synergi, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Synergi](#create-a-synergi-test-user)**  -toohave en motsvarighet för Britta Simon i Synergi som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Synergi.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Synergi:**

1. I hello Azure-portalen på hello **Synergi** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_samlbase.png)

3. På hello **Synergi domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och synergi domän med enkel inloggning information](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.irmsecurity.com`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.irmsecurity.com/sso/<organization id>`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [Synergi supportteamet](https://www.irmsecurity.com/contact/) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-synergi-tutorial/tutorial_general_400.png)

6. På hello **Synergi Configuration** klickar du på **konfigurera Synergi** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enhets-ID** från hello **Snabbreferens avsnitt.**

    ![Synergi konfiguration](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_configure.png) 

7. tooconfigure enkel inloggning på **Synergi** sida, behöver du toosend hello hämtas **Certificate(Base64) Sign-Out URL och SAML enhets-ID** för[Synergi supportteamet](https://www.irmsecurity.com/contact/).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-synergi-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-synergi-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-synergi-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-synergi-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
  
### <a name="create-a-synergi-test-user"></a>Skapa en testanvändare Synergi

I det här avsnittet skapar du en användare som kallas Britta Simon i Synergi. Arbeta med [Synergi supportteamet](https://www.irmsecurity.com/contact/) att lägga till hello användare i hello Synergi plattform. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSynergi.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooSynergi utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Synergi**.

    ![hello Synergi länken i listan med program hello](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Synergi programmet när du klickar på hello Synergi panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_203.png

