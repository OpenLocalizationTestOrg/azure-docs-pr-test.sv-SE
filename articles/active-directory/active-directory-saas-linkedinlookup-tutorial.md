---
title: "Självstudier: Azure Active Directory-integrering med LinkedIn sökning | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LinkedIn-sökning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: d79c34baa676391699e4b49806f16422fcfe73e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a>Självstudier: Azure Active Directory-integrering med LinkedIn-sökning

I kursen får du lära dig hur toointegrate LinkedIn sökning med Azure Active Directory (AD Azure).

Integrera LinkedIn sökning med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooLinkedIn sökning
- Du kan aktivera din användare tooautomatically get inloggade tooLinkedIn sökning (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med LinkedIn-sökning, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En sökning LinkedIn enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till LinkedIn sökning från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-linkedin-lookup-from-hello-gallery"></a>Lägga till LinkedIn sökning från hello-galleriet
tooconfigure hello integrering av LinkedIn sökning i Azure AD, behöver du tooadd LinkedIn sökning hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd LinkedIn sökning från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan tooadd nytt program.

    ![Program][3]

4. Skriv i sökrutan hello **LinkedIn sökning**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. Markera hello resultat på panelen **LinkedIn sökning**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Du konfigurera och testa Azure AD enkel inloggning med LinkedIn sökning baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LinkedIn sökning är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LinkedIn sökning toobe upprätta.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LinkedIn-sökning.

tooconfigure och testa Azure AD enkel inloggning med LinkedIn-sökning, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare LinkedIn sökning](#creating-an-linkedin-lookup-test-user)**  -toohave en motsvarighet för Britta Simon i LinkedIn uppslag som är länkade toohello Azure AD-representation.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LinkedIn-sökning.

**Utför följande hello tooconfigure Azure AD enkel inloggning med LinkedIn sökning:**

1. I hello Azure-portalen på hello **LinkedIn sökning** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan i **läge** Välj **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. I en annan webbläsarfönster inloggning tooyour **LinkedIn sökning** webbplats som administratör.

4. I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**. Markera också **sökning** hello listrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. Klicka på **eller klicka här tooload och kopiera enskilda fält från hello form** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. På Azure-portalen under **LinkedIn sökning domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    a. I hello **identifierare** textruta ange hello **enhets-ID** kopieras från LinkedIn-portalen 

    b. I hello **Reply URL** textruta ange hello **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen

7. Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`
     
    > [!NOTE] 
    > Detta är inte verkliga värdet. hello användaren har tooupdate dessa värden med hello faktiska inloggnings-URL. Kontakta [LinkedIn sökning klienten supportteamet](https://business.LinkedIn.com/lookup) tooget det här värdet.

8. Din **LinkedIn sökning** program förväntar hello SAML intyg i ett specifikt format. hello användare har tooadd attributet mappningar toohello SAML-token attribut konfiguration. hello följande skärmbild visar ett exempel. Hej standardvärdet **användar-ID** är **user.userprincipalname** men LinkedIn sökning förväntar denna toobe som mappats till hello användarens e-postadress. Du kan använda **user.mail** attribut hello listan eller använda hello rätt attribut-värde baserat på konfigurationen för din organisation. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut. hello användare behöver tooadd fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och hello-värdet är toobe som mappats till **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive

    | Attributets namn | Attributvärdet |
    | --- | --- |
    | E-post| User.Mail |    
    | Avdelning| User.Department |
    | Förnamn| User.givenName |
    | Efternamn| User.surname |

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    a. Klicka på **lägga till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    b. I hello **namn** textruta hello attributnamn visas för den raden.
    
    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **Ok**

10. Utför följande steg på hello hello **namn** attribut -

    a. Klicka på hello attributet tooopen hello **Redigera attribut** fönster.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    b. Ta bort hello URL-värdet från hello **namnområde**.
    
    c. Klicka på **Ok** toosave hello inställningen.

10. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. Gå för**LinkedIn admininställningar** avsnitt. Överför hello XML-fil som du hämtade från hello Azure-portalen genom att klicka på hello **överför XML-filen** alternativet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. Klicka på **på** tooenable enkel inloggning. SSO status ändras från **inte ansluten** för**ansluten**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. Klicka på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **Britta Simon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-linkedin-lookup-test-user"></a>Skapa en testanvändare LinkedIn-sökning

Länkade sökning programmet stöder bara i tid JIT-användaretablering och authentication-användare skapas i hello program automatiskt. Aktivera **automatiskt tilldela licenser** tooassign en licens toohello användare.
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLinkedIn sökning.

![Tilldela användare][200] 

**tooassign Britta Simon tooLinkedIn sökning, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **LinkedIn sökning**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.

    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello LinkedIn sökning panelen i hello åtkomstpanelen ska omdirigerade tooOrganizational sidan där du har tooprovide din personliga LinkedIn-kontoinformation. Den länkar ditt eget konto med ditt företag LinkedIn-konto. 

Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png

