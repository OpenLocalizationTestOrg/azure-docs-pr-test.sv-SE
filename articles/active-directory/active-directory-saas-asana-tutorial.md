---
title: "Självstudier: Azure Active Directory-integrering med Asana | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Asana."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: 9ac35dedc809b2b3dfb461d92a5afb066ccdedde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Självstudier: Azure Active Directory-integrering med Asana

I kursen får du lära dig hur toointegrate Asana med Azure Active Directory (AD Azure).

Integrera Asana med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooAsana
- Du kan aktivera din användare tooautomatically get inloggade tooAsana (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Asana, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Asana enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Asana från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-asana-from-hello-gallery"></a>Att lägga till Asana från hello-galleriet
tooconfigure hello integrering av Asana i Azure AD, behöver du tooadd Asana hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Asana från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Asana**väljer **Asana** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Asana baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Asana är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Asana toobe upprättas.

I Asana, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Asana, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Asana](#create-an-asana-test-user)**  -toohave en motsvarighet för Britta Simon i Asana som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Asana program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Asana:**

1. I hello Azure-portalen på hello **Asana** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. På hello **Asana domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och Asana domän med enkel inloggning information](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    a. I hello **inloggnings-URL** textruta typen URL:`https://app.asana.com/`

    b. I hello **identifierare** textruta TYPVÄRDE:`https://app.asana.com/`
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. På hello **Asana Configuration** klickar du på **konfigurera Asana** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Asana konfiguration](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. I ett annat fönster i webbläsaren, inloggning tooyour Asana program. tooconfigure SSO i Asana hello arbetsytan inställningar genom att klicka på hello arbetsytans namn på hello övre högra hörnet av hello-skärmen. Klicka på  **\<arbetsytans namn\> inställningar**. 
   
    ![Asana sso-inställningar](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. På hello **organisationsinställningar** -fönstret klickar du på **Administration**. Klicka på **medlemmar måste logga in via SAML** tooenable hello SSO konfiguration. hello utför hello följande steg:
   
    ![Konfigurera inställningar för enkel inloggning organisation](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     a. I hello **inloggning Sidadress** textruta klistra in hello **SAML inloggning tjänst-URL för enkel**.

     b. Högerklicka på hello certifikat hämtas från Azure-portalen och öppna hello certifikatfil med anteckningar eller din önskade textredigerare. Kopiera hello innehåll mellan hello börjar och hello slutet certifikat och klistra in den i hello **X.509-certifikat** textruta.

9. Klicka på **Spara**. Gå för[Asana guide för att konfigurera enkel inloggning](https://asana.com/guide/help/premium/authentication#gl-saml) om du behöver ytterligare hjälp.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![hello webbinställningar](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-an-asana-test-user"></a>Skapa en testanvändare Asana

I det här avsnittet skapar du en användare som kallas Britta Simon i Asana.

1. På **Asana**, gå toohello **team** avsnitt på hello vänstra panelen. Klicka på hello plus logga knappen. 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Skriv e-post hello britta.simon@contoso.com i hello textruta och välj sedan **bjuda in**.

3. Klicka på **skicka inbjudan**. hello nya användaren får ett e-postmeddelande till sin e-postkonto. Hon behöver toocreate och validera hello-kontot.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAsana.

![Tilldela hello användarroll][200]

**tooassign Britta Simon tooAsana utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Asana**.

    ![Hej Asana länken i listan med program hello](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD enkel inloggning.

Gå tooAsana inloggningssidan. Infoga hello e-postadress i hello e-postadress textruta britta.simon@contoso.com. Lämna tomt hello lösenordsrutan och klicka sedan på **loggar In**. Du kommer att omdirigerade tooAzure AD-inloggningssidan. Slutför Azure AD-autentiseringsuppgifterna. Nu kan är du inloggad Asana.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
