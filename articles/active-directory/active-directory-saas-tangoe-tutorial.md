---
title: "Självstudier: Azure Active Directory-integrering med Tangoe kommandot Premium Mobile | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Tangoe kommandot Premium Mobile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 7bd1cd6afade58a4a47a173b249f92eb84e42112
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Självstudier: Azure Active Directory-integrering med Tangoe kommandot Premium Mobile

I kursen får du lära dig hur toointegrate Tangoe kommandot Premium mobil med Azure Active Directory (AD Azure).

Integrera Tangoe kommandot Premium Mobile med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooTangoe kommandot Premium Mobile
- Du kan aktivera din användare tooautomatically get inloggade tooTangoe kommandot Premium Mobile (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Tangoe kommandot Premium Mobile måste hello följande objekt:

- En Azure AD-prenumeration
- En Tangoe kommandot Premium Mobile enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till Tangoe kommandot Premium Mobile från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="add-tangoe-command-premium-mobile-from-hello-gallery"></a>Lägg till Tangoe kommandot Premium Mobile från hello-galleriet
tooconfigure hello integrering av Tangoe kommandot Premium Mobile i Azure AD, behöver du tooadd Tangoe kommandot Premium Mobile hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Tangoe kommando Premium Mobile från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Tangoe kommandot Premium Mobile**väljer **Tangoe kommandot Premium Mobile** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Lägg till Tangoe kommandot Premium Mobile från galleriet ](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Tangoe kommandot Premium Mobile baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Tangoe kommandot Premium Mobile är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i Tangoe kommandot Premium Mobile toobe upprättas.

Tilldela hello värdet hello i Tangoe kommandot Premium Mobile **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Tangoe kommandot Premium Mobile, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Tangoe kommandot Premium Mobile](#create-a-tangoe-command-premium-mobile-test-user)**  -toohave en motsvarighet för Britta Simon i Tangoe kommandot Premium Mobile som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Tangoe kommandot Premium mobila program.

**tooconfigure Azure AD enkel inloggning med Tangoe kommandot Premium Mobile utför hello följande steg:**

1. I hello Azure-portalen på hello **Tangoe kommandot Premium Mobile** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![SAML-baserade inloggning](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_samlbase.png)

3. På hello **Tangoe kommandot Premium Mobile domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och Tangoe kommandot Premium mobila domän](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://sso.tangoe.com/sp/ACS.saml2`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska Reply URL och inloggnings-URL. Kontakta [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_certificate.png) 

5. Klicka på **spara** knappen.

    ![Knappen Spara](./media/active-directory-saas-tangoe-tutorial/tutorial_general_400.png)
    
6. På hello **Tangoe Premium Mobile konfiguration** klickar du på **konfigurera Tangoe kommandot Premium Mobile** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurationsavsnittet Tangoe kommandot Premium Mobile](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_configure.png) 

7. tooget SSO konfigurerats för ditt program bör du kontakta din [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) och ange hello följande:

   - hello hämtade metadatafil
   - Hej **SAML enhets-ID**
   - Hej **SAML enkel inloggning tjänst-URL**
   - Hej **Sign-Out URL**

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Användare och grupper -> alla användare](./media/active-directory-saas-tangoe-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Lägga till användare](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Dialogrutan Användarsida](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a>Skapa en Tangoe kommandot Premium mobila användare

I det här avsnittet kan du skapa en användare som kallas Britta Simon i Tangoe kommandot Premium Mobile. 

Tangoe kommandot Premium Mobile-programmet måste alla hello användare toobe etableras i hello program innan du utför enkel inloggning. Så kan du arbeta med hello [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) tooprovision dessa användare till hello program. 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTangoe kommandot Premium Mobile.

![Tilldela användare][200] 

**tooassign Britta Simon tooTangoe kommandot Premium Mobile utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Tangoe kommandot Premium Mobile**.

    ![Tangoe kommandot Premium Mobile i listan över appar](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.

När du klickar på hello Tangoe kommandot Premium Mobile panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Tangoe kommandot Premium Mobile-programmet. Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png

