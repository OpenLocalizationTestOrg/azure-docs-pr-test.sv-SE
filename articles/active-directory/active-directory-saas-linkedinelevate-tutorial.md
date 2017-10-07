---
title: "Självstudier: Azure Active Directory-integrering med LinkedIn höjer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LinkedIn höjer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 189bd72c230be7dc0c0b934f94ea01e84af9ad23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Självstudier: Azure Active Directory-integrering med LinkedIn höjer

I kursen får du lära dig hur toointegrate LinkedIn höjer med Azure Active Directory (AD Azure).

Integrera LinkedIn höjer med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooLinkedIn utöka
- Du kan aktivera din användare tooautomatically get inloggade tooLinkedIn utöka (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med LinkedIn höjer du behöver hello följande objekt:

- En Azure AD-prenumeration
- En LinkedIn höjer enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till LinkedIn höjer från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-linkedin-elevate-from-hello-gallery"></a>Lägga till LinkedIn höjer från hello-galleriet
tooconfigure hello integrering av LinkedIn höjer i Azure AD, behöver du tooadd LinkedIn höjer hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd LinkedIn höjer från galleriet hello utför hello följande steg:**

1. I hello ** [Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **LinkedIn höjer**. I resultatrutan, klickar du på **LinkedIn höjer** tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LinkedIn höjer baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LinkedIn höjer är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i LinkedIn höjer toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LinkedIn höjer.

tooconfigure och testa Azure AD enkel inloggning med LinkedIn höjer du behöver toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare LinkedIn höjer](#creating-a-linkedin-elevate-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt LinkedIn höjer program.

**Utför följande hello tooconfigure Azure AD enkel inloggning med LinkedIn höjer:**

1. I hello Azure Management portal på hello **LinkedIn höjer** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. I en annan webbläsarfönstret, inloggning tooyour LinkedIn höjer klient som administratör.

4. I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**. Markera också **utöka - höjer AAD Test** hello listrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. Klicka på **eller klicka här tooload och kopiera enskilda fält från hello form** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. På Azure Portal under **LinkedIn höja domän och URL: er**, utför följande steg om du vill tooconfigure SSO hello i **IdP initierade** läge

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    a. I hello **identifierare** textruta ange hello **enhets-ID** kopieras från LinkedIn-portalen 

    b. I hello **Reply URL** textruta ange hello **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen

7. Om du vill tooconfigure SSO i **SP-initierad**, klickar du på Visa avancerade URL inställningen alternativ i konfigurationsavsnittet hello och konfigurera hello logga på URL: en med hello följande mönster:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. Tillämpningsprogrammet LinkedIn höjer förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration. hello följande skärmbild visar ett exempel för det här. Hej standardvärdet **användar-ID** är **user.userprincipalname** men LinkedIn höjer förväntar denna toobe som mappats till hello användarens e-postadress. Som du kan använda **user.mail** attribut hello listan eller använda hello rätt attribut-värde baserat på konfigurationen för din organisation. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut. Du behöver tooadd en annan anspråk med namnet **avdelning** och hello-värdet måste mappas för toobe**user.department**.

    | Attributets namn | Attributvärdet |
    | --- | --- |    
    | Avdelning| User.Department |

      ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      a. Klicka på Lägg till attributet tooopen hello attributet informationssidan Lägg till hello avdelning attribut som visas nedan-

      ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      b. Klicka på **Ok** toosave hello attribut.

      c. Ändra hello namn för hello attributet **emailaddress** för**e-post**.


10. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. Klicka på **Spara**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. Gå för**LinkedIn admininställningar** avsnitt. Överför hello XML-fil som du precis har laddat ned från hello Azure-portalen genom att klicka på hello överför XML-filalternativet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. Klicka på **på** tooenable enkel inloggning. SSO status kommer att ändras från **inte ansluten** för**ansluten**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**. 

### <a name="creating-a-linkedin-elevate-test-user"></a>Skapa en LinkedIn höjer testanvändare

Länkade höjer programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt. Hej administratör på sidan Inställningar på hello LinkedIn höjer portal Vänd hello växeln **automatiskt tilldela licenser** tooactive tooenable precis tid etablering och detta kommer också tilldela en licens toohello användare.

   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooLinkedIn utöka.

![Tilldela användare][200] 

**tooassign Britta Simon tooLinkedIn utöka, utför följande steg hello:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **LinkedIn höjer**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello LinkedIn höjer panelen i hello åtkomstpanelen du bör få hello Azure inloggning sidan och på efter lyckad inloggning kan du bör få till LinkedIn höjer programmet.

## <a name="additional-resources"></a>Ytterligare resurser

* [Självstudier: Konfigurera LinkedIn höjer för automatisk användaretablering med Azure Active Directory](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
