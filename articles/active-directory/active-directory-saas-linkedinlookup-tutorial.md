---
title: "Självstudier: Azure Active Directory-integrering med LinkedIn sökning | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LinkedIn-sökning."
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
ms.openlocfilehash: e296431866a8611b30e72f286884890adf0f7e50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a>Självstudier: Azure Active Directory-integrering med LinkedIn-sökning

I kursen får lära du att integrera LinkedIn sökning med Azure Active Directory (AD Azure).

Integrera LinkedIn sökning med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till LinkedIn-sökning
- Du kan aktivera användarna att automatiskt hämta loggat in på LinkedIn-sökning (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med LinkedIn-sökning, behöver du följande:

- En Azure AD-prenumeration
- En sökning LinkedIn enkel inloggning på aktiverade prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till LinkedIn sökning från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-linkedin-lookup-from-the-gallery"></a>Lägga till LinkedIn sökning från galleriet
Du måste lägga till LinkedIn sökning från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LinkedIn sökning i Azure AD.

**Utför följande steg för att lägga till LinkedIn sökning från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** knappen överst för att lägga till nya program.

    ![Program][3]

4. I sökrutan skriver **LinkedIn sökning**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. Välj i resultatpanelen **LinkedIn sökning**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Du konfigurera och testa Azure AD enkel inloggning med LinkedIn sökning baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

Azure AD måste du känna till motsvarande användaren i LinkedIn sökning till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i LinkedIn sökning upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i LinkedIn-sökning.

Om du vill konfigurera och testa Azure AD enkel inloggning med LinkedIn-sökning, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare LinkedIn sökning](#creating-an-linkedin-lookup-test-user)**  – har en motsvarighet för Britta Simon LinkedIn-sökning som är kopplad till Azure AD-representation.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LinkedIn-sökning.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med LinkedIn sökning:**

1. I Azure-portalen på den **LinkedIn sökning** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan i **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. I en annan webbläsarfönster inloggning till din **LinkedIn sökning** webbplats som administratör.

4. I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**. Markera också **sökning** från den nedrullningsbara listan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. Klicka på **eller klicka här om du vill läsa in och kopiera enskilda fält i formuläret** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. På Azure-portalen under **LinkedIn sökning domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    a. I den **identifierare** textruta, ange den **enhets-ID** kopieras från LinkedIn-portalen 

    b. I den **Reply URL** textruta ange den **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen

7. Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`
     
    > [!NOTE] 
    > Detta är inte verkliga värdet. Användaren måste uppdatera dessa värden med den faktiska inloggnings-URL. Kontakta [LinkedIn sökning klienten supportteamet](https://business.LinkedIn.com/lookup) att hämta det här värdet.

8. Din **LinkedIn sökning** program förväntar SAML-intyg i ett specifikt format. Användaren har att lägga till anpassade attributmappning i konfigurationen för SAML-token attribut. Följande skärmbild visar ett exempel. Standardvärdet för **användar-ID** är **user.userprincipalname** men LinkedIn sökning förväntar detta mappas med användarens e-postadress. Du kan använda **user.mail** attribut i listan eller använda rätt attribut-värde baserat på konfigurationen för din organisation. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange attribut. Användaren behöver lägga till fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och värdet är för att mappa med **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive

    | Attributets namn | Attributvärdet |
    | --- | --- |
    | E-post| User.Mail |    
    | Avdelning| User.Department |
    | Förnamn| User.givenName |
    | Efternamn| User.surname |

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    a. Klicka på **lägga till attributet** att öppna den **lägga till attributet** dialogrutan.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    b. I den **namn** textruta ange attributets namn visas för den raden.
    
    c. Från den **värdet** listan, ange det attributvärde som visas för den raden.
    
    d. Klicka på **Ok**

10. Utför följande steg på den **namn** attribut -

    a. Klicka på attributet för att öppna den **Redigera attribut** fönster.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    b. Ta bort URL-värdet från den **namnområde**.
    
    c. Klicka på **Ok** spara inställningen.

10. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. Gå till **LinkedIn admininställningar** avsnitt. Överför den XML-fil som du hämtade från Azure portal genom att klicka på den **överför XML-filen** alternativet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. Klicka på **på** att aktivera enkel inloggning. SSO status ändras från **inte ansluten** till **ansluten**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned. Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. Gå till **användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. Klicka på **Lägg till** att öppna den **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **Britta Simon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-linkedin-lookup-test-user"></a>Skapa en testanvändare LinkedIn-sökning

Länkade sökning programmet stöder bara i tid JIT-användaretablering och authentication-användare skapas automatiskt i programmet. Aktivera **automatiskt tilldela licenser** tilldela en licens till användaren.
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LinkedIn-sökning.

![Tilldela användare][200] 

**Om du vill tilldela LinkedIn sökning Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **LinkedIn sökning**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.

    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen LinkedIn-sökning på åtkomstpanelen bör du omdirigeras till organisationens sida där du måste ange ditt personliga LinkedIn-kontoinformation. Den länkar ditt eget konto med ditt företag LinkedIn-konto. 

Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
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

