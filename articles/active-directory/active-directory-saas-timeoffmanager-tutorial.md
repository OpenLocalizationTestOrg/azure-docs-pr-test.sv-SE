---
title: "Självstudier: Azure Active Directory-integrering med TimeOffManager | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TimeOffManager."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: c871257bfb49883e31b1c4860a9d7faa70e9ab48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Självstudier: Azure Active Directory-integrering med TimeOffManager

I kursen får du lära dig hur toointegrate TimeOffManager med Azure Active Directory (AD Azure).

Integrera TimeOffManager med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooTimeOffManager
- Du kan aktivera din användare tooautomatically get inloggade tooTimeOffManager (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med TimeOffManager, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En TimeOffManager enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till TimeOffManager från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="add-timeoffmanager-from-hello-gallery"></a>Lägg till TimeOffManager från hello-galleriet
tooconfigure hello integrering av TimeOffManager i Azure AD, behöver du tooadd TimeOffManager hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd TimeOffManager från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **TimeOffManager**väljer **TimeOffManager** från resultatet Kontrollpanelen och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Lägg till från galleriet](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TimeOffManager baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TimeOffManager är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TimeOffManager toobe upprättas.

I TimeOffManager, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med TimeOffManager, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare TimeOffManager](#create-a-timeoffmanager-test-user)**  -toohave en motsvarighet för Britta Simon i TimeOffManager som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TimeOffManager program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TimeOffManager:**

1. I hello Azure-portalen på hello **TimeOffManager** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![SAML-baserade inloggning](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. På hello **TimeOffManager domän och URL: er** avsnittet, utföra hello följande:

     ![Avsnittet TimeOffManager domän och URL: er](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska Reply-URL. Du kan hämta värdet från **inställningssidan för enkelinloggning** som beskrivs senare i självstudiekursen hello eller kontakta [TimeOffManager supportteamet](http://www.timeoffmanager.com/contact-us.aspx).
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooTimeOffManger med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.
    
    Tillämpningsprogrammet TimeOffManger förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration. hello följande skärmbild visar ett exempel för det här.

    ![SAML-token attribut](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "attribut för saml-token")
    
    | Attributets namn | Attributvärdet |
    | --- | --- |
    | Förnamn |User.givenName |
    | Efternamn |User.surname |
    | E-post |User.Mail |
    
    a.  För varje datarad i hello tabellen ovan klickar du på **lägga till användarattribut**.
    
    ![SAML-token attribut](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "attribut för saml-token")
    
    ![SAML-token attribut](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "attribut för saml-token")
    
    b.  I hello **attributnamn** textruta hello attributnamn visas för den raden.
    
    c.  I hello **attributvärdet** textruta väljer hello-attributvärde som visas för den raden.
    
    d.  Klicka på **OK**.
    
6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. På hello **TimeOffManager Configuration** klickar du på **konfigurera TimeOffManager** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![TimeOffManager konfigurationsavsnitt](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. Logga in på webbplatsen TimeOffManager företag som en administratör i en annan webbläsarfönster.

9. Gå för**konto \> kontoalternativ \> inställningar för enkel inloggning**.
   
   ![Enkel inloggning inställningar](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "enkel inloggning inställningar")
7. I hello **inställningar för enkel inloggning** avsnittet, utföra hello följande steg:
   
   ![Enkel inloggning inställningar](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "enkel inloggning inställningar")
   
   a. Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in hello hela certifikatet till **X.509-certifikat** textruta.
   
   b. I **Idp utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.
   
   c. I **IdP slutpunkts-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
   
   d. Som **genomdriva SAML**väljer **nr**.
   
   e. Som **skapa automatiskt användare**väljer **Ja**.
   
   f. I **logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.
   
   g. Klicka på **spara ändringar**.

11. I **inställningar för enkelinloggning** sidan Kopiera hello värdet för **Assertion konsument-tjänstens URL** och klistra in den i hello **Reply URL** textrutan under **TimeOffManager Domänen och URL: er** avsnitt i Azure-portalen. 

      ![Enkel inloggning inställningar](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "enkel inloggning inställningar")

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Användare och grupper--> alla användare](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Knappen Lägg till](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Dialogrutan Användarsida](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-timeoffmanager-test-user"></a>Skapa en testanvändare TimeOffManager

I ordning tooenable Azure AD-användare toolog i TimeOffManager, måste de vara etablerade tooTimeOffManager.  

TimeOffManager stöder bara i tid användaretablering. Det finns ingen åtgärd-objekt.  

hello användare läggs till automatiskt under hello första inloggningen med enkel inloggning på.

>[!NOTE]
>Du kan använda något annat TimeOffManager användarens konto skapas verktyg eller API: er som tillhandahålls av TimeOffManager tooprovision användarkonton i Azure AD.
> 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTimeOffManager.

![Tilldela användare][200] 

**tooassign Britta Simon tooTimeOffManager utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **TimeOffManager**.

    ![TimeOffManager i listan över appar](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour TimeOffManager programmet när du klickar på hello TimeOffManager panelen i hello åtkomstpanelen. Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

