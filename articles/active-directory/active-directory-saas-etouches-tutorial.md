---
title: "Självstudier: Azure Active Directory-integrering med etouches | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och etouches."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 5f3ff7550e660b0fc52612140ca55061504b5edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Självstudier: Azure Active Directory-integrering med etouches

I kursen får du lära dig hur toointegrate etouches med Azure Active Directory (AD Azure).

Integrera etouches med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooetouches
- Du kan aktivera din användare tooautomatically get inloggade tooetouches (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med etouches, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En etouches enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till etouches från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-etouches-from-hello-gallery"></a>Att lägga till etouches från hello-galleriet
tooconfigure hello integrering av etouches i Azure AD, behöver du tooadd etouches hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd etouches från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **etouches**väljer **etouches** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![etouches i hello resultatlistan](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med etouches baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i etouches är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i etouches toobe upprättas.

I etouches, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med etouches, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare etouches](#create-an-etouches-test-user)**  -toohave en motsvarighet för Britta Simon i etouches som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt etouches program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med etouches:**

1. I hello Azure-portalen på hello **etouches** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_samlbase.png)

3. På hello **etouches domän och URL: er** avsnittet, utföra hello följande steg:

    ![etouches domän och URL: er med enkel inloggning information](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.eiseverywhere.com/<instance name>`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Du uppdaterar hello värdet med hello faktiska inloggning URL och identifierare som beskrivs senare i hello kursen.
    > 

4. etouches program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello **användarattribut** av programmet hello. hello följande skärmbild visar ett exempel för det här. 

    ![Användarattribut](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_attribute.png) 

5. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:
    
    | Attributets namn | Attributvärdet |
    | ------------------- | -------------------- |
    | E-post | User.Mail |    
    
    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Lägg till attribut](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_04.png)

    ![Attributet dialogrutan Lägg till](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_05.png)

    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **OK**. 

6. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_certificate.png) 

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-etouches-tutorial/tutorial_general_400.png)

8. tooget SSO konfigurerats för ditt program, utför följande steg i hello etouches programmet hello: 

    ![etouches konfiguration](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png) 

    a. Inloggning för**etouches** program med hjälp av hello administratörsrättigheter.
   
    b. Gå toohello **SAML** konfiguration.

    c. I hello **allmänna inställningar** avsnittet, öppna hämtade certifikatet från Azure-portalen i anteckningar, kopiera hello innehåll, och klistra in den i textrutan för hello IDP-metadata. 

    d. Klicka på hello **Spara & förblir** knappen.
  
    e. Klicka på hello **uppdateringsmetadata** knapp i hello SAML Metadata-avsnitt. 

    f. Detta öppnar hello sidan och utföra en enkel inloggning. Hello SSO fungerar en gång och sedan kan du ställa in hello användarnamn.

    g. Välj hello hello användarnamnfältet **e-postadress** enligt hello bilden nedan. 

    h. Kopiera hello **SP enhets-ID** värdet och klistrar in den i hello **identifierare** textruta som finns i **etouches domän och URL: er** avsnitt på Azure-portalen.

    Jag. Kopiera hello **SSO URL / ACS** värdet och klistrar in den i hello **inloggning URL** textruta som finns i **etouches domän och URL: er** avsnitt på Azure-portalen.
   
> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-etouches-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-etouches-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![hello webbinställningar](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![hello användardialogrutan](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-an-etouches-test-user"></a>Skapa en testanvändare etouches

I det här avsnittet skapar du en användare som kallas Britta Simon i etouches. Arbeta med [etouches klienten supportteam](https://www.etouches.com/event-software/support/customer-support/) tooadd hello användare i hello etouches plattform.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooetouches.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooetouches utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **etouches**.

    ![Hej etouches länken i listan med program hello](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning


hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour etouches programmet när du klickar på hello etouches panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png

