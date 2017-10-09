---
title: "Självstudier: Azure Active Directory-integrering med anpassningsbar Suite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och anpassningsbar Suite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: af309c27ab74098c1e229c80adb11c96dc2774fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Självstudier: Azure Active Directory-integrering med anpassningsbar Suite

I kursen får du lära dig hur toointegrate anpassningsbar Suite med Azure Active Directory (AD Azure).

Integrera anpassningsbar Suite med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooAdaptive Suite
- Du kan aktivera din användare tooautomatically get inloggade tooAdaptive Suite (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med anpassningsbar Suite måste hello följande objekt:

- En Azure AD-prenumeration
- En anpassningsbar Suite enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till anpassad Suite från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-adaptive-suite-from-hello-gallery"></a>Lägga till anpassad Suite från hello-galleriet
tooconfigure hello integrering av anpassningsbar Suite i Azure AD, behöver du tooadd anpassningsbar Suite hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd anpassningsbar Suite från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **anpassningsbar Suite**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. Markera hello resultat på panelen **anpassningsbar Suite**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med anpassningsbar Suite baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i anpassningsbar Suite är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i anpassningsbar Suite toobe upprättas.

Tilldela hello värdet hello i anpassningsbar Suite **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med anpassningsbar Suite, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en anpassningsbar Suite testanvändare](#creating-an-adaptive-suite-test-user)**  -toohave en motsvarighet för Britta Simon i anpassningsbar Suite som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i anpassningsbar Suite-program.

**Utför följande hello tooconfigure Azure AD enkel inloggning med anpassningsbar Suite:**

1. I hello Azure-portalen på hello **anpassningsbar Suite** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. På hello **anpassningsbar Suite domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > Du kan hämta värdet från hello anpassningsbar Suite **SAML SSO inställningar** sidan.
    >  

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. På hello **anpassningsbar Suite Configuration** klickar du på **konfigurera anpassningsbar Suite** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. Logga in tooyour anpassningsbar Suite företagets webbplats som en administratör i en annan webbläsarfönster.

8. Gå för**Admin**.
   
    ![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")

9. I hello **användare och roller** klickar du på **hantera inställningar för SAML SSO**.
   
    ![Hantera inställningar för SAML SSO](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "hantera SAML SSO-inställningar")

10. På hello **SAML SSO inställningar** utför hello följande steg:
   
    ![Inställningar för SAML SSO](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO-inställningar")

    a. I hello **identitet providernamn** textruta, ange ett namn för din konfiguration.
    
    b. Klistra in hello **SAML enhets-ID** kopieras värdet från Azure-portalen till hello **identitetsleverantör enhets-ID** textruta.
  
    c. Klistra in hello **SAML enkel inloggning Tjänstwebbadress** kopieras värdet från Azure-portalen till hello **identitetsleverantör SSO URL** textruta.
  
    d. Klistra in hello **SAML enkel inloggning Tjänstwebbadress** kopieras värdet från Azure-portalen till hello **anpassad logga ut URL** textruta.
  
    e. tooupload hämtade certifikatet, klicka på **Välj fil**.
  
    f. Välj hello följande, för:
    * **Användar-id för SAML**väljer **användarens användarnamn för anpassningsbar insikter**.
    * **Id för SAML Användarplats**väljer **användar-id i NameID ämne**.
    * **Format för SAML-NameID**väljer **e-postadress**.
    * **Aktivera SAML**väljer **Tillåt SAML SSO och anpassningsbar insikter direktinloggning**.
    
    g. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-adaptive-suite-test-user"></a>Skapa en anpassningsbar Suite testanvändare

tooenable Azure AD-användare toolog i tooAdaptive Suite de att etableras i anpassningsbar Suite.  

* I hello fall av anpassningsbar Suite är etablering en manuell aktivitet.

**tooconfigure användaretablering, utför följande steg hello:** 

1. Logga in tooyour **anpassningsbar Suite** företagets webbplats som administratör.
2. Gå för**Admin**.
   
   ![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")
3. I hello **användare och roller** klickar du på **Lägg till användare**.
   
   ![Lägg till användare](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "lägga till användare")
4. I hello **ny användare** avsnittet, utföra hello följande steg:
   
   ![Skicka](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "skicka")   

   a. Typen hello **namn**, **inloggning**, **e-post**, **lösenord** en giltig Azure Active Directory-användare som du vill tooprovision till hello relaterade textrutor.
  
   b. Välj en **rollen**.
  
   c. Klicka på **skicka**.

>[!NOTE]
>Du kan använda något annat anpassningsbar Suite användarens konto skapas verktyg eller API: er som tillhandahålls av anpassningsbar Suite tooprovision AAD användarkonton.
>  

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAdaptive Suite.

![Tilldela användare][200] 

**tooassign Britta Simon tooAdaptive Suite utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **anpassningsbar Suite**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Microsoft Azure AD enkel inloggning konfiguration av hello åtkomstpanelen.

När du klickar på hello anpassningsbar Suite-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour anpassningsbar Suite-program.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

