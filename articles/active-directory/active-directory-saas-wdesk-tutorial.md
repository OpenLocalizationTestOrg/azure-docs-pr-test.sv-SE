---
title: "Självstudier: Azure Active Directory-integrering med Wdesk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Wdesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 2c278a5e4f50cc362663efc0f826ff0c0fcf6e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Självstudier: Azure Active Directory-integrering med Wdesk

I kursen får du lära dig hur toointegrate Wdesk med Azure Active Directory (AD Azure).

Integrera Wdesk med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooWdesk
- Du kan aktivera din användare tooautomatically get inloggade tooWdesk (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD. [Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Wdesk, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Wdesk enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Wdesk från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-wdesk-from-hello-gallery"></a>Att lägga till Wdesk från hello-galleriet
tooconfigure hello integrering av Wdesk i Azure AD, behöver du tooadd Wdesk hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Wdesk från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Wdesk**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. Markera hello resultat på panelen **Wdesk**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Wdesk baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Wdesk är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Wdesk toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Wdesk.

tooconfigure och testa Azure AD enkel inloggning med Wdesk, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Wdesk](#creating-a-wdesk-test-user)**  -toohave en motsvarighet för Britta Simon i Wdesk som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Wdesk program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Wdesk:**

1. I hello Azure-portalen på hello **Wdesk** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. På hello **Wdesk domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

4. Kontrollera **visa avancerade inställningar för URL: en**. Om du inte vill tooconfigure hello programmet i **SP** initierade läge, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Du kan få värdena från WDesk portal när du konfigurerar hello enkel inloggning. 
  
5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. I en annan webbläsarfönstret, inloggning tooWdesk som en administratör.

8. I hello längst ned till vänster, klickar du på **Admin** och välj **kontoadministratören**:
 
     ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. Wdesk Admin, navigera för**säkerhet**, sedan **SAML** > **SAML inställningar**:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. Under **allmänna inställningar**, kontrollera hello **aktivera SAML enkel inloggning**:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. Under **information om Service Provider**, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Kopiera hello **Inloggningswebbadressen** och klistra in den i **inloggnings-Url** textruta på Azure-portalen.
   
      b. Kopiera hello **Url för tjänstmetadata** och klistra in den i **identifierare** textruta på Azure-portalen.
       
      c. Kopiera hello **konsumenten url** och klistra in den i **Reply Url** textruta på Azure-portalen.
   
      d. Klicka på **spara** på Azure portal toosave hello ändringar.      

12. Klicka på **konfigurera inställningarna för IdP** tooopen **redigera inställningar för IdP** dialogrutan. Klicka på **Välj fil** toolocate hello **Metadata.xml** fil som du sparade från Azure-portalen sedan ladda upp den.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. Klicka på **spara ändringar**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-wdesk-test-user"></a>Skapa en testanvändare Wdesk

tooenable Azure AD-användare toolog i tooWdesk, måste de etableras i Wdesk. I Wdesk är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooWdesk som en administratör.
2. Navigera för**Admin** > **kontoadministratören**.

     ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. Klicka på **medlemmar** under **personer**.

4. Klicka på **Lägg till medlem** tooopen **Lägg till medlem** dialogrutan. 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. I **användare** text Ange hello användarnamnet för användaren som  **brittasimon@contoso.com**  och på **Fortsätt** knappen.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  Ange hello information som visas nedan:
  
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    a. I **e-post** text Ange hello e-postadressen för användaren som  **brittasimon@contoso.com** .

    b. I **Förnamn** text Ange hello första namn för användaren som **Britta**.

    c. I **efternamn** text Ange hello efternamn för användaren som **Simon**.

7. Klicka på **spara medlemmen** knappen.  

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWdesk.

![Tilldela användare][200] 

**tooassign Britta Simon tooWdesk utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Wdesk**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Wdesk programmet när du klickar på hello Wdesk panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

