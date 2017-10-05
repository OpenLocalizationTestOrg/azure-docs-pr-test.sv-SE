---
title: "Självstudier: Azure Active Directory-integrering med Adobe kreativa molntjänster | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Adobe kreativa moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 3d13608612c77236346b0e98551d7fc427d602e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Självstudier: Azure Active Directory-integrering med Adobe kreativa moln

I kursen får lära du att integrera Adobe kreativa moln med Azure Active Directory (AD Azure).

Integrera Adobe kreativa moln med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Adobe kreativa moln
- Du kan aktivera användarna att automatiskt hämta loggat in på Adobe kreativa molnet (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Adobe kreativa molnet, behöver du följande:

- En Azure AD-prenumeration
- En Adobe kreativa molnet enkel inloggning på aktiverade prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Adobe kreativa moln från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Att lägga till Adobe kreativa moln från galleriet
Du måste lägga till Adobe kreativa moln från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Adobe kreativa moln i Azure AD.

**Utför följande steg för att lägga till Adobe kreativa moln från galleriet:**

1. I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **Adobe kreativa molnet**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. Välj i resultatpanelen **Adobe kreativa molnet**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Adobe kreativa molnet baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till motsvarande användaren i Adobe kreativa molnet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Adobe kreativa molnet upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Adobe kreativa molnet.

Om du vill konfigurera och testa Azure AD enkel inloggning med Adobe kreativa molnet, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Adobe kreativa molnet](#creating-an-adobe-creative-cloud-test-user)**  – du har en motsvarighet för Britta Simon i Adobe kreativa moln som är kopplad till Azure AD-representation av henne.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i tillämpningsprogrammet Adobe kreativa molnet.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Adobe kreativa molnet:**

1. I Azure-hanteringsportalen på den **Adobe kreativa molnet** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. På den **Adobe kreativa molnet domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    a. I den **identifierare** textruta Skriv värdet som:`https://www.okta.com/saml2/service-provider/<token>`

    b. I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Observera att detta inte är verkliga värden. Du måste uppdatera dessa värden med de faktiska identifierare och Reply-URL. Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren. Om du behöver skapa en användare manuellt måste du kontakta supportteamet Adobe kreativa molnet.

4. På den **Adobe kreativa molnet domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    a. Klicka på den **visa avancerade inställningar för URL: en** alternativet

    b. I den **inloggnings-URL** textruta Skriv värdet som:`https://adobe.com`

5. På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. På den **Adobe kreativa Molnkonfigurationen** klickar du på **Konfigurera Adobe kreativa molnet** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML enhets-Id** och **URL för SAML SSO-Service** från Snabbreferens avsnitt.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. I en annan webbläsarfönster inloggning till Adobe kreativa moln-klient som administratör.

8.  Gå till **identitet** i det vänstra navigeringsfönstret och klicka på din domän. Utför följande steg på **enkel inloggning på konfiguration krävs** avsnitt.

    ![Inställningar för](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "inställningar")

9. Klicka på **Bläddra** hämtade certifikatet från Azure AD för att överföra **IDP certifikat**.

10. I den **IDP utfärdaren** textruta, ange värdet för **SAML enhets-Id** som du kopierade från **konfigurera inloggning** avsnitt i Azure-portalen.

11. I den **IDP inloggnings-URL** textruta, ange värdet för **URL för SAML SSO-Service** som du kopierade från **konfigurera inloggning** avsnitt i Azure-portalen.

12. Välj **HTTP - omdirigering** som **IDP bindning**.

13. Välj **e-postadress** som **inloggningen Användarinställning**.
 
14. Klicka på **spara** knappen.

15. Instrumentpanelen visas nu XML **”hämta Metadata”** filen. Den innehåller Adobe EntityDescriptor Webbadressen och AssertionConsumerService. Öppna filen och konfigurera dem i Azure AD-program.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Använd EntityDescriptor värdet Adobe för **identifierare** på den **konfigurera Appinställningar** dialogrutan.

    b. Använd AssertionConsumerService värdet Adobe för **Reply URL** på den **konfigurera Appinställningar** dialogrutan.
 
### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **BrittaSimon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**. 

### <a name="creating-an-adobe-creative-cloud-test-user"></a>Skapa en testanvändare Adobe kreativa moln

För att aktivera Azure AD-användare att logga in på Adobe kreativa molnet, måste de etableras i Adobe kreativa moln.  
Adobe kreativa moln är etablering en manuell aktivitet.

**Utför följande steg för att etablera en användarkonton:**

1. Logga in på webbplatsen Adobe kreativa molnet företag som administratör.

2. Klicka på **personer**.

    ![Personer](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "personer")

3. Klicka på **bjuda in användare**.

    ![Bjuda in användare](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "bjuda in användare")

4. På den **bjuda in personer** dialogrutan utför följande steg:

    ![Bjuda in](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "bjuda in")

    a. I den **e-post** textruta skriver Britta Simon konto e-postadress.
    
    b. Klicka på **bjuda in**.

    > [!NOTE]
    > Innehavaren för Azure Active Directory-konto ett e-postmeddelande och länken för att bekräfta sina konton innan den aktiveras.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning med sin åtkomst beviljas till Adobe kreativa moln.

![Tilldela användare][200] 

**Om du vill tilldela Adobe kreativa molnet Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **Adobe kreativa molnet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen Adobe kreativa molnet på åtkomstpanelen du bör få automatiskt inloggade i tillämpningsprogrammet Adobe kreativa molnet.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png