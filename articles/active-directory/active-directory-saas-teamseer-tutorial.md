---
title: "Självstudier: Azure Active Directory-integrering med TeamSeer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TeamSeer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 876d13e446115acd50b01c7f44db99357045e429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Självstudier: Azure Active Directory-integrering med TeamSeer

I kursen får du lära dig hur toointegrate TeamSeer med Azure Active Directory (AD Azure).

Integrera TeamSeer med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooTeamSeer
- Du kan aktivera din användare tooautomatically get inloggade tooTeamSeer (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med TeamSeer, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En TeamSeer enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till TeamSeer från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-teamseer-from-hello-gallery"></a>Att lägga till TeamSeer från hello-galleriet
tooconfigure hello integrering av TeamSeer i tooAzure AD, behöver du tooadd TeamSeer hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd TeamSeer från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **TeamSeer**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. Markera hello resultat på panelen **TeamSeer**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TeamSeer baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TeamSeer är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TeamSeer toobe upprättas.

I TeamSeer, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med TeamSeer, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare TeamSeer](#creating-a-teamseer-test-user)**  -toohave en motsvarighet för Britta Simon i TeamSeer som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TeamSeer program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TeamSeer:**

1. I hello Azure-portalen på hello **TeamSeer** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. På hello **TeamSeer domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > hello-värdet är inte verkliga. Hello uppdateringsvärde med hello faktiska inloggnings-URL. Kontakta [TeamSeer klienten supportteamet](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello värde. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. På hello **TeamSeer Configuration** klickar du på **konfigurera TeamSeer** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. Logga in tooyour TeamSeer företagets webbplats som en administratör i en annan webbläsarfönster.

8. Gå för**HR Admin**.
   
    ![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")

9. Klicka på **installationsprogrammet**.
   
    ![Installationsprogrammet](./media/active-directory-saas-teamseer-tutorial/ic789635.png "installationen")

10. Klicka på **ställa in information om SAML provider**.
   
    ![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML-inställningar")

11. I hello informationsavsnittet för SAML-providern, utföra hello följande steg:
   
    ![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML-inställningar")   

    a. Klistra in hello **inloggning tjänst-URL för enkel** värdet i toohello **URL** textruta.
          
    b. Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i tooyour Urklipp och klistra in den toohello **IdP offentliga certifikat** textruta.

12. toocomplete hello SAML providerkonfigurationen utför hello följande steg:
    
    ![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML-inställningar") 

    a. I hello **testa e-postadresser**, Skriv hello test användarens e-postadress. 
  
    b. I hello **utfärdaren** textruta typen hello utfärdar-URL för hello-leverantör. 
  
    c. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-teamseer-test-user"></a>Skapa en testanvändare TeamSeer

tooenable Azure AD-användare toolog i tooTeamSeer, måste de vara etablerade i tooShiftPlanning. Hello gäller TeamSeer är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **TeamSeer** företagets webbplats som administratör.

2. Utför följande steg hello:
   
    ![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")  
 
    a. Gå för**HR Admin \> användare**.
  
    b. Klicka på **kör guiden nya användare och hello**.

3. I hello **användarinformation** avsnittet, utföra hello följande steg:
   
    ![Användarinformation](./media/active-directory-saas-teamseer-tutorial/ic789641.png "användarinformation")

    a. Typen hello **Förnamn**, **efternamn**, **användarnamn (e-postadress)** av en giltig AAD-konto som du vill tooprovision i toohello relaterade textrutor.
  
    b. Klicka på **Nästa**.

4. Följ hello på skärmen för att lägga till en ny användare och klicka på **Slutför**.

>[!NOTE]
>Du kan använda något annat TeamSeer användarens konto skapas verktyg eller API: er som tillhandahålls av TeamSeer tooprovision användarkonton i Azure AD. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTeamSeer.

![Tilldela användare][200] 

**tooassign Britta Simon tooTeamSeer utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **TeamSeer**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

