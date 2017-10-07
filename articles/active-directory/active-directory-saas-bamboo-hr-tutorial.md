---
title: "Självstudier: Azure Active Directory-integrering med BambooHR | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BambooHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: jeedes
ms.openlocfilehash: f9083f846beb3a4bf4cebbf18b42aba2dfef2472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Självstudier: Azure Active Directory-integrering med BambooHR

I kursen får du lära dig hur toointegrate BambooHR med Azure Active Directory (AD Azure).

Integrera BambooHR med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooBambooHR
- Du kan aktivera din användare tooautomatically get inloggade tooBambooHR (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med BambooHR, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En BambooHR enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till BambooHR från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-bamboohr-from-hello-gallery"></a>Att lägga till BambooHR från hello-galleriet
tooconfigure hello integrering av BambooHR i Azure AD, behöver du tooadd BambooHR hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd BambooHR från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **BambooHR**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_search.png)

5. Markera hello resultat på panelen **BambooHR**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BambooHR baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i BambooHR är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BambooHR toobe upprättas.

I BambooHR, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med BambooHR, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare BambooHR](#creating-a-bamboohr-test-user)**  -toohave en motsvarighet för Britta Simon i BambooHR som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt BambooHR program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med BambooHR:**

1. I hello Azure-portalen på hello **BambooHR** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. På hello **BambooHR domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.bamboohr.com`
    
    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [BambooHR klienten supportteamet](https://www.bamboohr.com/contact.php) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. På hello **BambooHR Configuration** klickar du på **konfigurera BambooHR** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

6. Logga in på webbplatsen BambooHR företag som en administratör i en annan webbläsarfönster.

7. Utför följande hello på hello webbsida:
   
    ![Enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "enkel inloggning")   

    a. Klicka på **appar**.
   
    b. Hello appar menyn hello vänster, klickar du på **enkel inloggning**.
   
    c. Klicka på **SAML enkel inloggning**.

8. I hello **SAML enkel inloggning** avsnittet, utföra hello följande steg:
   
    ![SAML enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML enkel inloggning")
   
    a. Klistra in hello **SAML enkel inloggning Tjänstwebbadress** värdet till hello **inloggnings-Url för SSO** textruta.
      
    b. Öppna Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta
   
    c. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-bamboohr-test-user"></a>Skapa en testanvändare BambooHR

tooenable Azure AD-användare toolog i tooBambooHR, måste de etableras i BambooHR.  

Hello gäller BambooHR är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **BambooHR** plats som administratör.

2. Klicka i hello verktygsfältet hello längst upp **inställningar**.
   
    ![Ange](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "inställning")

3. Klicka på **översikt**.

4. Hello vänstra navigeringsfönstret, gå för**säkerhet \> användare**.

5. Skriv hello användarnamn, lösenord och e-postadressen till en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.

6. Klicka på **Spara**.
        
>[!NOTE]
>Du kan använda något annat BambooHR användarens konto skapas verktyg eller API: er som tillhandahålls av BambooHR tooprovision AAD-användarkonton.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBambooHR.

![Tilldela användare][200] 

**tooassign Britta Simon tooBambooHR utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **BambooHR**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour BambooHR programmet när du klickar på hello BambooHR panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

