---
title: "Självstudier: Azure Active Directory-integrering med molntjänster Lifesize | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Lifesize moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ae599907e872571b3220de7122006c7db8db4a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Självstudier: Azure Active Directory-integrering med Lifesize moln

I kursen får du lära dig hur toointegrate Lifesize moln med Azure Active Directory (AD Azure).

Integrera Lifesize moln med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooLifesize moln
- Du kan aktivera din användare tooautomatically get inloggade tooLifesize molnet (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Lifesize molnet, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Lifesize enkel inloggning aktiverad molnprenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Lifesize moln från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-lifesize-cloud-from-hello-gallery"></a>Att lägga till Lifesize moln från hello-galleriet
tooconfigure hello integrering av Lifesize moln i Azure AD, behöver du tooadd Lifesize moln hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Lifesize moln från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Lifesize moln**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. Markera hello resultat på panelen **Lifesize moln**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Du konfigurera och testa Azure AD enkel inloggning med Lifesize molnet baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Lifesize molnet är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Lifesize molnet toobe upprätta.

Tilldela hello värdet hello i Lifesize molnet **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Lifesize molnet, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Lifesize moln](#creating-a-lifesize-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i Lifesize moln som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Lifesize moln.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Lifesize molnet:**

1. I hello Azure-portalen på hello **Lifesize moln** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. På hello **Lifesize moln domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.lifesizecloud.com/ls/?acs`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.lifesizecloud.com/<companyname>`

     
4. Kontrollera **visa avancerade inställningar för URL: en**, utföra hello följande steg:  
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    I hello **Relay tillstånd** textruta, ange ett URL-Adressen med hello följer mönstret:`https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >Observera att detta inte är hello verkliga värden. du har tooupdate dessa värden med hello faktiska inloggnings-URL, Relay tillstånd och identifierare. Kontakta [Lifesize Cloud klienten supportteamet](https://www.lifesize.com/support) tooget inloggnings-URL, och identifierar-värden och du kan hämta Relay tillstånd värde från SSO-konfigurationen som beskrivs senare i självstudiekursen hello.

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. På hello **Lifesize Molnkonfigurationen** klickar du på **konfigurera Lifesize moln** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. tooget SSO konfigurerats för ditt program, logga in på hello Lifesize molnapp med administratörsrättigheter.

8. Klicka på ditt namn i hello övre högra hörnet och klicka sedan på hello **avancerade inställningar**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. I avancerade inställningar nu klickar du på hello hello **SSO Configuration** länk. Hello SSO konfigurationssidan för din instans öppnas.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. Konfigurera följande värden i hello SSO Konfigurationsgränssnittet hello.    
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    a. I **identitet providern utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.

    b.  I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    c. Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta.
  
    d. I hello SAML-attributet mappningar för hello förnamn textrutan anger hello-värde som **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    
    e. I hello SAML attributmappning för hello **efternamn** textruta ange hello-värde som **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    
    f. I hello SAML attributmappning för hello **e-post** textruta ange hello-värde som **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

11. toocheck hello konfiguration kan du klicka på hello **Test** knappen.
   
    >[!NOTE]
    >För lyckad testning behöver toocomplete hello konfigurationsguiden i Azure AD och även ange åtkomst toousers eller grupper som kan utföra hello test.

12. Aktivera hello SSO genom att kontrollera på hello **aktivera enkel inloggning** knappen.

13. Klicka på hello **uppdatering** knappen så att alla hello inställningarna sparas. Detta genererar hello RelayState värde. Kopiera hello RelayState-värde som har genererats i textrutan för hello, klistra in den i hello **Relay tillstånd** textruta under **Lifesize moln domän och URL: er** avsnitt. 

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-lifesize-cloud-test-user"></a>Skapa en testanvändare Lifesize moln

I det här avsnittet kan du skapa en användare som kallas Britta Simon i Lifesize molnet. Lifesize molnet stöder automatisk användaretablering. Efter en lyckad autentisering vid Azure AD hello användaren automatiskt att etableras i hello program. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLifesize moln.

![Tilldela användare][200] 

**tooassign Britta Simon tooLifesize moln, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Lifesize moln**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Lifesize moln panelen i hello åtkomstpanelen bör du hämta inloggningssidan för Lifesize molnapp.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

