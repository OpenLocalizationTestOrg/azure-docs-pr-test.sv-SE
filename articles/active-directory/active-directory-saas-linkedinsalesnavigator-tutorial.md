---
title: "Självstudier: Azure Active Directory-integrering med LinkedInSalesNavigator | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LinkedInSalesNavigator."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 443d302d40d7af16aba5114e00963f23ea8d12d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Självstudier: Azure Active Directory-integrering med LinkedIn försäljning Navigator

I kursen får du lära dig hur toointegrate LinkedIn försäljning Navigator med Azure Active Directory (AD Azure).

Integrera LinkedIn försäljning Navigator med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooLinkedIn försäljning Navigator
- Du kan aktivera din användare tooautomatically get inloggade tooLinkedIn försäljning Navigator (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, Bläddra [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med LinkedIn försäljning Navigator måste hello följande objekt:

- En Azure AD-prenumeration
- En LinkedIn försäljning Navigator enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Undvik att använda i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till LinkedIn försäljning Navigator från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-linkedin-sales-navigator-from-hello-gallery"></a>Lägga till LinkedIn försäljning Navigator från hello-galleriet
tooconfigure hello integrering av LinkedIn försäljning Navigator i Azure AD, behöver du tooadd LinkedIn försäljning Navigator hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd LinkedIn försäljning Navigator från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **LinkedIn försäljning Navigator**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. Markera hello resultat på panelen **LinkedIn försäljning Navigator**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LinkedIn försäljning Navigator baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LinkedIn försäljning Navigator är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i LinkedIn försäljning Navigator toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LinkedIn försäljning Navigator.

tooconfigure och testa Azure AD enkel inloggning med LinkedIn försäljning Navigator, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare LinkedIn försäljning Navigator](#creating-a-linkedin-sales-navigator-test-user)**  -toohave en motsvarighet för Britta Simon i LinkedIn försäljning Navigator som är länkade toohello Azure AD-representation av hello användare.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LinkedIn försäljning Navigator.

**tooconfigure Azure AD enkel inloggning med LinkedIn försäljning Navigator utför hello följande steg:**

1. I hello Azure-portalen på hello **LinkedIn försäljning Navigator** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan i **läge** Välj **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. I en annan webbläsarfönster inloggning tooyour **LinkedIn försäljning Navigator** webbplats som administratör.

4. I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**. Markera också **försäljning Navigator** hello listrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. Klicka på **eller klicka här tooload och kopiera enskilda fält från hello form** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. På Azure-portalen under **URL: er och domänen för LinkedIn förs Navigator** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. I hello **identifierare** textruta ange hello **enhets-ID** kopieras från LinkedIn-portalen 

    b. I hello **Reply URL** textruta ange hello **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen

7. Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

8. Din **LinkedIn försäljning Navigator** program förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour SAML-token attribut konfiguration. hello följande skärmbild visar ett exempel. Hej standardvärdet **användar-ID** är **user.userprincipalname** men LinkedIn försäljning Navigator förväntar att det toobe som mappats till hello användarens e-postadress. Du kan använda **user.mail** attribut hello listan eller använda hello rätt attribut-värde baserat på konfigurationen för din organisation. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut. hello användare behöver tooadd fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och hello-värdet är toobe som mappats till **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive

    | Attributets namn | Attributvärdet |
    | --- | --- |    
    | E-post| User.Mail |
    | Avdelning| User.Department |
    | Förnamn| User.givenName |
    | Efternamn| User.surname |
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Klicka på **lägga till attributet** tooopen hello attributet dialogrutan.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. I hello **namn** textruta hello attributnamn visas för den raden.
    
    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **Ok**

10. Utför följande steg på hello hello **namn** attribut -

    a. Klicka på hello attributet tooopen hello **Redigera attribut** fönster.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    b. Ta bort hello URL-värdet från hello **namnområde**.
    
    c. Klicka på **Ok** toosave hello inställningen.

11. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. Gå för**LinkedIn admininställningar** avsnitt. Klicka på **överför XML-filen** tooupload hello Metadata XML-fil som du har hämtat från hello Azure-portalen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. Klicka på **på** tooenable enkel inloggning. SSO status ändras från **inte ansluten** för**ansluten**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Skapa en testanvändare LinkedIn försäljning Navigator

Länkade försäljning Navigator programmet stöder bara i tid JIT-användaretablering och authentication-användare skapas i hello program automatiskt. Aktivera **automatiskt tilldela licenser** tooassign en licens toohello användare.
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLinkedIn försäljning Navigator.

![Tilldela användare][200] 

**tooassign Britta Simon tooLinkedIn försäljning Navigator utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **LinkedIn försäljning Navigator**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello LinkedIn försäljning Navigator panelen i hello åtkomstpanelen ska omdirigerade tooOrganizational sidan där du har tooprovide din personliga LinkedIn-kontoinformation. Den länkar ditt eget konto med ditt företag LinkedIn-konto. Läs mer om hello åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png

