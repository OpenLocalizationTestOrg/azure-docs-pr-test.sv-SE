---
title: "Självstudier: Azure Active Directory-integrering med TalentLMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TalentLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 25538086602e58fbaab0fbf223f5b03908a74922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Självstudier: Azure Active Directory-integrering med TalentLMS

I kursen får du lära dig hur toointegrate TalentLMS med Azure Active Directory (AD Azure).

Integrera TalentLMS med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooTalentLMS
- Du kan aktivera din användare tooautomatically get inloggade tooTalentLMS (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med TalentLMS, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En TalentLMS enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till TalentLMS från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-talentlms-from-hello-gallery"></a>Att lägga till TalentLMS från hello-galleriet
tooconfigure hello integrering av TalentLMS i Azure AD, behöver du tooadd TalentLMS hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd TalentLMS från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **TalentLMS**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_search.png)

5. Markera hello resultat på panelen **TalentLMS**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TalentLMS baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TalentLMS är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TalentLMS toobe upprättas.

I TalentLMS, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med TalentLMS, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare TalentLMS](#creating-a-talentlms-test-user)**  -toohave en motsvarighet för Britta Simon i TalentLMS som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TalentLMS program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TalentLMS:**

1. I hello Azure-portalen på hello **TalentLMS** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_samlbase.png)

3. På hello **TalentLMS domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.TalentLMSapp.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`http://<tenant-name>.talentlms.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [TalentLMS klienten supportteamet](https://www.talentlms.com/contact) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet från hello-certifikatet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_general_400.png)

6. På hello **TalentLMS Configuration** klickar du på **konfigurera TalentLMS** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_configure.png)  

7. Logga in tooyour TalentLMS företagets webbplats som en administratör i en annan webbläsarfönster.

8. I hello **konto & inställningar** klickar du på hello **användare** fliken.
   
    ![Konto & inställningar](./media/active-directory-saas-talentlms-tutorial/IC777296.png "konto & inställningar")

9. Klicka på **enkel inloggning (SSO)**,

10. I hello Single Sign-On-avsnittet, utföra hello följande steg:
   
    ![Enkel inloggning](./media/active-directory-saas-talentlms-tutorial/IC777297.png "enkel inloggning")   

    a. Från hello **SSO integration typen** väljer **SAML 2.0**.

    b. I hello **identitetsprovider (IDP)** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.
 
    c. Klistra in hello **tumavtrycket** värdet från Azure-portalen i hello **certifikat fingeravtryck** textruta.    

    d.  I hello **Remote inloggning URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.
 
    e. I hello **extern URL för utloggning** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.

    f. Fyll i hello följande: 

    * I hello **TargetedID** textruta typ`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
     
    * I hello **Förnamn** textruta typ`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`
    
    * I hello **efternamn** textruta typ`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`
    
    * I hello **e-post** textruta typ`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`
    
11. Klicka på **Spara**.
 
> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-talentlms-test-user"></a>Skapa en testanvändare TalentLMS

tooenable Azure AD-användare toolog i tooTalentLMS, måste de etableras i TalentLMS. Hello gäller TalentLMS är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **TalentLMS** klient.

2. Klicka på **användare**, och klicka sedan på **Lägg till användare**.

3. På hello **Lägg till användare** dialogrutan utför hello följande steg:
   
    ![Lägg till användare](./media/active-directory-saas-talentlms-tutorial/IC777299.png "lägga till användare")  

    a. I hello **Förnamn** textruta anger hello först namnet på användaren som **Britta**.

    b. I hello **efternamn** textruta ange hello efternamn för användaren som **Simon**.
 
    c. I hello **e-postadress** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .

    d. Klicka på **lägga till användare**.

>[!NOTE]
>Du kan använda något annat TalentLMS användarens konto skapas verktyg eller API: er som tillhandahålls av TalentLMS tooprovision AAD-användarkonton.
 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTalentLMS.

![Tilldela användare][200] 

**tooassign Britta Simon tooTalentLMS utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **TalentLMS**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

När du klickar på hello TalentLMS panelen i hello åtkomstpanelen bör du få automatiskt inloggade tooyour TalentLMS program

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_203.png

