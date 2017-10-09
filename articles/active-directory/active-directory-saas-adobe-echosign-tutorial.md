---
title: "Självstudier: Azure Active Directory-integrering med Adobe | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Adobe logga."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b4b07907f30a0890003554a02a76d968400b43ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Självstudier: Azure Active Directory-integrering med Adobe

I kursen får du lära dig hur toointegrate Adobe logga med Azure Active Directory (AD Azure).

Integrera Adobe inloggning med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooAdobe inloggning
- Du kan aktivera din användare tooautomatically get inloggade tooAdobe logga (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Adobe, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Adobe logga enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Adobe logga från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-adobe-sign-from-hello-gallery"></a>Lägga till Adobe logga från hello-galleriet
tooconfigure hello integrering av Adobe inloggning i Azure AD, behöver du tooadd Adobe logga hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Adobe logga från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Adobe logga**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Markera hello resultat på panelen **Adobe logga**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Adobe baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Adobe logga är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Adobe logga toobe upprättas.

I Adobe logga tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Adobe, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Adobe logga](#creating-an-adobe-sign-test-user)**  -toohave en motsvarighet för Britta Simon i Adobe tecken som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Adobe signera programmet.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Adobe tecken:**

1. I hello Azure-portalen på hello **Adobe logga** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. På hello **Adobe logga domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.echosign.com/`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.echosign.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Adobe logga klienten supportteamet](https://helpx.adobe.com/in/contact/support.html) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. På hello **Adobe logga Configuration** klickar du på **Konfigurera Adobe logga** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. Logga in tooyour Adobe logga företagets webbplats som en administratör i en annan webbläsarfönster.

8. Hello-menyn överst hello **konto**, och hello navigeringsfönstret hello vänster, klicka **SAML inställningar** under **kontoinställningar**.
   
   ![Kontot](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "konto")

9. I hello SAML-inställningar, utföra hello följande steg:
   
   ![Inställningar för SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML-inställningar")
   
   a. Som **SAML läge**väljer **SAML obligatoriska**.
   
   b. Välj **Tillåt EchoSign Kontoadministratörer toolog i med hjälp av autentiseringsuppgifterna EchoSign**.
   
   c. Som **Användarskapelse**väljer **Lägg automatiskt till användare som autentiseras via SAML**.

10. Flytta, utför hello följande steg:

       ![Inställningar för SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML-inställningar")

    a. Klistra in **SAML enhets-ID**, som du har kopierat från Azure-portalen i hello **IdP enhets-ID** textruta.
    
    b. Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från Azure-portalen i hello **IdP inloggnings-URL** textruta.
   
    c. Klistra in **Sign-Out URL**, som du har kopierat från Azure-portalen i hello **IdP logga ut URL** textruta.

    d. Öppna din hämtade **Certificate(Base64)** filen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **IdP certifikat** textruta

    e. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-adobe-sign-test-user"></a>Skapa en Adobe logga testanvändare

tooenable Azure AD-användare toolog i tooAdobe logga de att etableras i Adobe logga. I hello fallet Adobe tecken är etablering en manuell aktivitet.

>[!NOTE]
>Du kan använda något annat Adobe logga användarens konto skapas verktyg eller API: er som tillhandahålls av Adobe logga tooprovision AAD användarkonton. 

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **Adobe logga** företagets webbplats som administratör.

2. Hello-menyn överst hello **konto**, och hello navigeringsfönstret hello vänster, klicka **användare och grupper**, och klicka sedan på **skapa en ny användare**.
   
   ![Kontot](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "konto")
   
3. I hello **skapa nya användare** avsnittet, utföra hello följande steg:
   
   ![Skapa användare](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "skapa användare")
   
   a. Typen hello **e-postadress**, **Förnamn**, och **efternamn** av en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.
   
   b. Klicka på **skapa användare**.

>[!NOTE]
>hello Azure Active Directory användare får ett e-postmeddelande som innehåller en länk tooconfirm hello-konto innan den aktiveras. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAdobe logga.

![Tilldela användare][200] 

**tooassign Britta Simon tooAdobe signera, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Adobe logga**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

När du klickar på hello Adobe logga panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Adobe signera programmet.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

