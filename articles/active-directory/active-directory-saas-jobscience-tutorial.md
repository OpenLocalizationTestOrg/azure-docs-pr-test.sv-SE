---
title: "Självstudier: Azure Active Directory-integrering med Jobscience | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Jobscience."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 4a4c78aad6d324795a15a9569542afc23b4716d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Självstudier: Azure Active Directory-integrering med Jobscience

I kursen får du lära dig hur toointegrate Jobscience med Azure Active Directory (AD Azure).

Integrera Jobscience med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooJobscience
- Du kan aktivera din användare tooautomatically get inloggade tooJobscience (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Jobscience, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Jobscience enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Jobscience från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-jobscience-from-hello-gallery"></a>Att lägga till Jobscience från hello-galleriet
tooconfigure hello integrering av Jobscience i Azure AD, behöver du tooadd Jobscience hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Jobscience från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Jobscience**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_search.png)

5. Markera hello resultat på panelen **Jobscience**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Jobscience baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Jobscience är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Jobscience toobe upprättas.

I Jobscience, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Jobscience, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Jobscience](#creating-a-jobscience-test-user)**  -toohave en motsvarighet för Britta Simon i Jobscience som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Jobscience program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Jobscience:**

1. I hello Azure-portalen på hello **Jobscience** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. På hello **Jobscience domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`http://<company name>.my.salesforce.com`
    
    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Hämta det här värdet genom att [Jobscience klienten supportteamet](https://www.jobscience.com/support) eller från hello SSO profil skapas som beskrivs senare i hello kursen. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_general_400.png)

6. På hello **Jobscience Configuration** klickar du på **konfigurera Jobscience** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_configure.png) 

7. Logga in tooyour Jobscience företagets webbplats som administratör.

8. Gå för**installationsprogrammet**.
   
   ![Installationsprogrammet](./media/active-directory-saas-jobscience-tutorial/IC784358.png "installationen")

9. Hello vänstra navigeringsfönstret i hello **administrera** klickar du på **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän** tooopen hello  **Min domän** sidan. 
   
   ![Min domän](./media/active-directory-saas-jobscience-tutorial/ic767825.png "min domän")

10. tooverify som din domän har angetts korrekt, kontrollera att den är i ”**steg 4 distribueras tooUsers**” och granska din ”**Mina Domäninställningar**”.

    ![Domän distribuerat tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "domän distribueras tooUser")

11. Klicka på på hello Jobscience företagets **säkerhetsåtgärder**, och klicka sedan på **inställningar för enkel inloggning**.
    
    ![Säkerhetsåtgärder](./media/active-directory-saas-jobscience-tutorial/ic784364.png "säkerhetsåtgärder")

12. I hello **inställningar för enkel inloggning** avsnittet, utföra hello följande steg:
    
    ![Enkel inloggning inställningar](./media/active-directory-saas-jobscience-tutorial/ic781026.png "enkel inloggning inställningar")
    
    a. Välj **SAML aktiverat**.

    b. Klicka på **Ny**.

13. På hello **SAML enkel inloggning inställningen redigera** dialogrutan utföra hello följande steg:
    
    ![SAML enkel inloggning inställningen](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML enkel inloggning inställningen")
    
    a. I hello **namn** textruta, ange ett namn för din konfiguration.

    b. I **utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.

    c. I hello **enhets-Id** textruta typ`https://salesforce-jobscience.com`

    d. Klicka på **Bläddra** tooupload Azure AD-certifikat.

    e. Som **SAML identitetstyp**väljer **Assertion innehåller hello Federation ID från hello användarobjektet**.

    f. Som **SAML identitet plats**väljer **identitet är i hello NameIdentfier elementet i hello ämne instruktionen**.

    g. I **identitet providern inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.

    h. I **identitet providern logga ut URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.

    Jag. Klicka på **Spara**.

14. Hello vänstra navigeringsfönstret i hello **administrera** klickar du på **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän** tooopen hello  **Min domän** sidan. 
    
    ![Min domän](./media/active-directory-saas-jobscience-tutorial/ic767825.png "min domän")

15. På hello **min domän** i hello sidan **inloggningen sidan anpassning** klickar du på **redigera**.
    
    ![Inloggningssidan anpassning](./media/active-directory-saas-jobscience-tutorial/ic767826.png "inloggningssidan anpassning")

16. På hello **inloggningen sidan anpassning** i hello sidan **Autentiseringstjänsten** avsnitt, hello namnet på din **SAML SSO inställningar** visas. Markera den och klicka sedan på **spara**.
    
    ![Inloggningssidan anpassning](./media/active-directory-saas-jobscience-tutorial/ic784366.png "inloggningssidan anpassning")

17. tooget hello SP initieras för enkel inloggning på inloggnings-URL-Klicka på hello **inställningar för enkel inloggning** i hello **säkerhetsåtgärder** menyn avsnitt.

    ![Säkerhetsåtgärder](./media/active-directory-saas-jobscience-tutorial/ic784368.png "säkerhetsåtgärder")
    
    Klicka på hello SSO-profil som du har skapat i hello ovanstående steg. Den här sidan visar hello enkel inloggning på URL: en för ditt företag (till exempel [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).    

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-jobscience-test-user"></a>Skapa en testanvändare Jobscience

I ordning tooenable Azure AD-användare toolog i tooJobscience, måste de etableras i Jobscience. Hello gäller Jobscience är etablering en manuell aktivitet.

>[!NOTE]
>Du kan använda något annat Jobscience användarens konto skapas verktyg eller API: er som tillhandahålls av Jobscience tooprovision Azure Active Directory användarkonton.
>  
        
**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **Jobscience** företagets webbplats som administratör.

2. Gå tooSetup.
   
   ![Installationsprogrammet](./media/active-directory-saas-jobscience-tutorial/ic784358.png "installationen")
3. Gå för**hantera användare \> användare**.
   
   ![Användare](./media/active-directory-saas-jobscience-tutorial/ic784369.png "användare")
4. Klicka på **ny användare**.
   
   ![Alla användare](./media/active-directory-saas-jobscience-tutorial/ic784370.png "alla användare")
5. På hello **Redigera användare** dialogrutan utföra hello följande steg:
   
   ![Redigera användare](./media/active-directory-saas-jobscience-tutorial/ic784371.png "Redigera användare")
   
   a. I hello **Förnamn** textruta, ange ett förnamn för hello användare som Britta.
   
   b. I hello **efternamn** textruta anger efternamn för användaren hello som Simon.
   
   c. I hello **Alias** textruta typnamn hello användaren som brittas alias.

   d. I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.

   e. I hello **användarnamn** textruta, Skriv ett användarnamn för användaren som Brittasimon@contoso.com.

   f. I hello **smeknamn** textruta typnamn nick för användaren som Simon.

   g. Klicka på **Spara**.

    
> [!NOTE]
> hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooJobscience.

![Tilldela användare][200] 

**tooassign Britta Simon tooJobscience utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Jobscience**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Jobscience programmet när du klickar på hello Jobscience panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_203.png

