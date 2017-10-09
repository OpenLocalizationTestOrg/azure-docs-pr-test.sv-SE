---
title: "Självstudier: Azure Active Directory-integrering med AirWatch | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och AirWatch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e5230d5a36824778a4d9804dadf9f13a0d11a68d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Självstudier: Azure Active Directory-integrering med AirWatch

I kursen får du lära dig hur toointegrate AirWatch med Azure Active Directory (AD Azure).

Integrera AirWatch med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooAirWatch
- Du kan aktivera din användare tooautomatically get inloggade tooAirWatch (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med AirWatch, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En AirWatch enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till AirWatch från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-airwatch-from-hello-gallery"></a>Att lägga till AirWatch från hello-galleriet
tooconfigure hello integrering av AirWatch i Azure AD, behöver du tooadd AirWatch hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd AirWatch från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **AirWatch**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. Markera hello resultat på panelen **AirWatch**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AirWatch baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i AirWatch är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i AirWatch toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i AirWatch.

tooconfigure och testa Azure AD enkel inloggning med AirWatch, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare AirWatch](#creating-a-airwatch-test-user)**  -toohave en motsvarighet för Britta Simon i AirWatch som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt AirWatch program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med AirWatch:**

1. I hello Azure-portalen på hello **AirWatch** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. På hello **AirWatch domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. I hello **identifierare** textruta hello TYPVÄRDE som`AirWatch`

    > [!NOTE] 
    > Det här värdet är inte hello verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [AirWatch klienten supportteamet](http://www.air-watch.com/company/contact-us/) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. På hello **AirWatch Configuration** klickar du på **konfigurera AirWatch** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS>
7. Logga in tooyour AirWatch företagets webbplats som en administratör i en annan webbläsarfönster.

8. Hello vänstra navigeringsfönstret, klicka på **konton**, och klicka sedan på **administratörer**.
   
   ![Administratörer](./media/active-directory-saas-airwatch-tutorial/ic791920.png "administratörer")

9. Expandera hello **inställningar** -menyn och klicka sedan på **Directory Services**.
   
   ![Inställningar för](./media/active-directory-saas-airwatch-tutorial/ic791921.png "inställningar")

10. Klicka på hello **användare** på fliken hello **Bas-DN** textruta Skriv ditt domännamn och klicka sedan på **spara**.
   
   ![Användaren](./media/active-directory-saas-airwatch-tutorial/ic791922.png "användare")

11. Klicka på hello **Server** fliken.
   
   ![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")

12. Utför följande steg hello:
    
    ![Överför](./media/active-directory-saas-airwatch-tutorial/ic791924.png "överför")   
    
    a. Som **katalogtyp**väljer **ingen**.

    b. Välj **använder SAML för autentisering**.

    c. tooupload hello hämtat certifikat klickar du på **överför**.

13. I hello **begära** avsnittet, utföra hello följande steg:
    
    ![Begära](./media/active-directory-saas-airwatch-tutorial/ic791925.png "begäran")  

    a. Som **begära bindning av typen**väljer **efter**.

    b. I hello Azure-portalen på hello **Konfigurera enkel inloggning på Airwatch** dialogrutan sida, kopiera hello **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in den i hello **identitetsleverantören. Enkel inloggning på URL: en** textruta.

    c. Som **NameID Format**väljer **e-postadress**.

    d. Klicka på **Spara**.

14. Klicka på hello **användaren** fliken igen.
    
    ![Användaren](./media/active-directory-saas-airwatch-tutorial/ic791926.png "användare")

15. I hello **attributet** avsnittet, utföra hello följande steg:
    
    ![Attributet](./media/active-directory-saas-airwatch-tutorial/ic791927.png "attribut")

    a. I hello **objektidentifierare** textruta typen **http://schemas.microsoft.com/identity/claims/objectidentifier**.

    b. I hello **användarnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. I hello **visningsnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. I hello **Förnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. I hello **efternamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. I hello **e-post** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. Klicka på **Spara**.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-airwatch-test-user"></a>Skapa en testanvändare AirWatch

tooenable Azure AD-användare toolog i tooAirWatch, måste de vara etablerade i tooAirWatch.

* När AirWatch, etablering är en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **AirWatch** företagets webbplats som administratör.
2. Hello navigeringsfönstret hello vänster, klicka på **konton**, och klicka sedan på **användare**.
   
   ![Användare](./media/active-directory-saas-airwatch-tutorial/ic791929.png "användare")
3. I hello **användare** -menyn klickar du på **listvyn**, och klicka sedan på **Lägg till \> Lägg till användare**.
   
   ![Lägg till användare](./media/active-directory-saas-airwatch-tutorial/ic791930.png "lägga till användare")
4. På hello **Lägg till / redigera användare** dialogrutan utföra hello följande steg:

   ![Lägg till användare](./media/active-directory-saas-airwatch-tutorial/ic791931.png "lägga till användare")   
   1. Typen hello **användarnamn**, **lösenord**, **Bekräfta lösenord**, **Förnamn**, **efternamn**,  **E-postadress** av en giltig Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor.
   2. Klicka på **Spara**.

>[!NOTE]
>Du kan använda något annat AirWatch användarens konto skapas verktyg eller API: er som tillhandahålls av AirWatch tooprovision AAD-användarkonton.
>  

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAirWatch.

![Tilldela användare][200] 

**tooassign Britta Simon tooAirWatch utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **AirWatch**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

