---
title: "Självstudier: Azure Active Directory-integrering med CloudPassage | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och CloudPassage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 32fb007b90f071626c9b40fb5afc341dd3c1ae99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a>Självstudier: Azure Active Directory-integrering med CloudPassage

I kursen får du lära dig hur toointegrate CloudPassage med Azure Active Directory (AD Azure).

Integrera CloudPassage med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooCloudPassage
- Du kan aktivera din användare tooautomatically get inloggade tooCloudPassage (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med CloudPassage, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En CloudPassage enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till CloudPassage från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-cloudpassage-from-hello-gallery"></a>Att lägga till CloudPassage från hello-galleriet
tooconfigure hello integrering av CloudPassage i Azure AD, behöver du tooadd CloudPassage hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd CloudPassage från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **CloudPassage**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_search.png)

5. Markera hello resultat på panelen **CloudPassage**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med CloudPassage baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i CloudPassage är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i CloudPassage toobe upprättas.

I CloudPassage, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med CloudPassage, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare CloudPassage](#creating-a-cloudpassage-test-user)**  -toohave en motsvarighet för Britta Simon i CloudPassage som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt CloudPassage program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med CloudPassage:**

1. I hello Azure-portalen på hello **CloudPassage** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

3. På hello **CloudPassage domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://portal.cloudpassage.com/saml/init/accountid`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret: `https://portal.cloudpassage.com/saml/consume/accountid`. Du kan hämta värdet för det här attributet genom att klicka på **SSO installationsprogrammet dokumentationen** i hello **inställningar för enkel inloggning** på CloudPassage-portal.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska Reply URL och inloggnings-URL. Kontakta [CloudPassage klienten supportteamet](https://www.cloudpassage.com/company/contact/) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

5. Tillämpningsprogrammet CloudPassage förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.   
hello följande skärmbild visar ett exempel för det här.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

6. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:

    | Attributets namn | Attributvärdet |
    | --- | --- |
    | Förnamn |User.givenName |
    | Efternamn |User.surname |
    | E-post |User.Mail |
    
    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **OK**.

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_400.png)
    
8. På hello **CloudPassage Configuration** klickar du på **konfigurera CloudPassage** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

9. I ett annat fönster i webbläsaren, inloggning tooyour CloudPassage företagets webbplats som administratör.

10. Hello-menyn överst hello **inställningar**, och klicka sedan på **Platsadministration**. 
   
    ![Konfigurera enkel inloggning][12]

11. Klicka på hello **autentiseringsinställningar** fliken. 
   
    ![Konfigurera enkel inloggning][13]

12. I hello **inställningar för enkel inloggning** avsnittet, utföra hello följande steg: 
   
    ![Konfigurera enkel inloggning][14]

    a. Välj **aktivera enkel sign-on(SSO) (SSO installationsprogrammet dokumentation)** kryssrutan.
    
    b. Klistra in **SAML enhets-ID** till hello **utfärdar-URL för SAML** textruta.
  
    c. Klistra in **SAML enkel inloggning Tjänstwebbadress** till hello **slutpunkts-URL för SAML** textruta.
  
    d. Klistra in **Sign-Out URL** till hello **logga ut landningssida** textruta.
  
    e. Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll för hämtat certifikat till Urklipp, och klistra in den i hello **x 509-certifikat** textruta.
  
    f. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-cloudpassage-test-user"></a>Skapa en testanvändare CloudPassage

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i CloudPassage.

**toocreate en användare som kallas Britta Simon i CloudPassage, utför följande steg hello:**

1. Inloggning tooyour **CloudPassage** företagets webbplats som administratör. 

2. Klicka i hello verktygsfältet hello längst upp **inställningar**, och klicka sedan på **Platsadministration**. 
   
   ![Skapa en testanvändare CloudPassage][22] 

3. Klicka på hello **användare** fliken och klicka sedan på **Lägg till nya användare**. 
   
   ![Skapa en testanvändare CloudPassage][23]

4. I hello **Lägg till nya användare** avsnittet, utföra hello följande steg: 
   
   ![Skapa en testanvändare CloudPassage][24]
    
    a. I hello **Förnamn** textruta skriver Britta. 
  
    b. I hello **efternamn** textruta skriver Simon.
  
    c. I hello **användarnamn** textruta hello **e-post** textruta och hello **Skriv e-** textruta anger Brittas användarnamn i Azure AD.
  
    d. Som **behörighet**väljer **aktivera Halo Portal åtkomst**.
  
    e. Klicka på **Lägg till**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCloudPassage.

![Tilldela användare][200] 

**tooassign Britta Simon tooCloudPassage utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **CloudPassage**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour CloudPassage programmet när du klickar på hello CloudPassage panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_203.png

