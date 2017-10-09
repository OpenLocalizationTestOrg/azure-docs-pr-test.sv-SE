---
title: "Självstudier: Azure Active Directory-integrering med Work.com | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Work.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: dcdc51c884abd78c945b649de99f942d32373cf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Självstudier: Azure Active Directory-integrering med Work.com

I kursen får du lära dig hur toointegrate Work.com med Azure Active Directory (AD Azure).

Integrera Work.com med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooWork.com
- Du kan aktivera din användare tooautomatically get inloggade tooWork.com (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Work.com, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Work.com enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till Work.com från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="add-workcom-from-hello-gallery"></a>Lägg till Work.com från hello-galleriet
tooconfigure hello integrering av Work.com i Azure AD, behöver du tooadd Work.com hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Work.com från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Work.com**väljer **Work.com** från resultatrutan Klicka **Lägg till** knappen tooadd hello program.

    ![Lägg till från galleriet](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Work.com baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Work.com är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Work.com toobe upprättas.

I Work.com, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Work.com, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Work.com](#create-a-workcom-test-user)**  -toohave en motsvarighet för Britta Simon i Work.com som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Work.com program.

>[!NOTE]
>tooconfigure enkel inloggning, måste toosetup ett anpassat domännamn Work.com ännu. Du behöver toodefine minst en domän namn, testa ditt domännamn och distribuera den tooyour hela organisationen.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Work.com:**

1. I hello Azure-portalen på hello **Work.com** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![SAML-baserade inloggning](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. På hello **Work.com domän och URL: er** avsnittet, utföra hello följande:

    ![Avsnittet Work.com domän och URL: er](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`http://<companyname>.my.salesforce.com`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [Work.com klienten supportteamet](https://help.salesforce.com/articleView?id=000159855&type=3) tooget det här värdet. 

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. Klicka på **spara** knappen.

    ![Knappen Spara](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. På hello **Work.com Configuration** klickar du på **konfigurera Work.com** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Work.com konfigurationsavsnitt](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. Logga in tooyour Work.com klient som administratör.

8. Gå för**installationsprogrammet**.
   
    ![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/ic794108.png "installationen")

9. Hello vänstra navigeringsfönstret i hello **administrera** klickar du på **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän** tooopen hello  **Min domän** sidan. 
   
    ![Min domän](./media/active-directory-saas-work-com-tutorial/ic767825.png "min domän")

10. tooverify som din domän har angetts korrekt, kontrollera att den är i ”**steg 4 distribueras tooUsers**” och granska din ”**Mina Domäninställningar**”.
   
    ![Domän distribuerat tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "domän distribueras tooUser")

11. Logga in tooyour Work.com klient.

12. Gå för**installationsprogrammet**.
    
    ![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/ic794108.png "installationen")

13. Expandera hello **säkerhetsåtgärder** -menyn och klicka sedan på **inställningar för enkel inloggning**.
    
    ![Enkel inloggning inställningar](./media/active-directory-saas-work-com-tutorial/ic794113.png "enkel inloggning inställningar")

14. På hello **inställningar för enkel inloggning** dialogrutan utför hello följande steg:
    
    ![SAML aktiverat](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML aktiverad")
    
    a. Välj **SAML aktiverat**.
    
    b. Klicka på **Ny**.

15. I hello **SAML enkel inloggning inställningar** avsnittet, utföra hello följande steg:
    
    ![SAML enkel inloggning inställningen](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML enkel inloggning inställningen")
    
    a. I hello **namn** textruta, ange ett namn för din konfiguration.  
       
    > [!NOTE]
    > Att tillhandahålla ett värde för **namn** automatiskt fylla hello **API-namnet** textruta.
    
    b. I **utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.
    
    c. tooupload hello hämtat certifikat från Azure-portalen klickar du på **Bläddra**.
    
    d. I hello **enhets-Id** textruta typen `https://salesforce-work.com`.
    
    e. Som **SAML identitetstyp**väljer **Assertion innehåller hello Federation ID från hello användarobjektet**.
    
    f. Som **SAML identitet plats**väljer **identitet är i hello NameIdentfier elementet i hello ämne instruktionen**.
    
    g. I **identitet providern inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    h. I **identitet providern logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.
    
    Jag. Som **providern initierade begära Tjänstbindning**väljer **HTTP Post**.
    
    j. Klicka på **Spara**.

16. I din Work.com klassiska portalen på hello vänstra navigationsfönstret klickar du på **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän** tooopen hello **min domän**sidan. 
    
    ![Min domän](./media/active-directory-saas-work-com-tutorial/ic794115.png "min domän")

17. På hello **min domän** i hello sidan **inloggningen sidan anpassning** klickar du på **redigera**.
    
    ![Inloggningssidan anpassning](./media/active-directory-saas-work-com-tutorial/ic767826.png "inloggningssidan anpassning")

14. På hello **inloggningen sidan anpassning** i hello sidan **Autentiseringstjänsten** avsnitt, hello namnet på din **SAML SSO inställningar** visas. Markera den och klicka sedan på **spara**.
    
    ![Inloggningssidan anpassning](./media/active-directory-saas-work-com-tutorial/ic784366.png "inloggningssidan anpassning")

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Användare och grupper -> alla användare](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Lägg till](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Dialogrutan Användarsida](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-workcom-test-user"></a>Skapa en testanvändare Work.com
För Azure Active Directory användare toobe kan toosign i, måste de vara etablerade tooWork.com. Hello gäller Work.com är etablering en manuell aktivitet.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure användaretablering, utför följande steg hello:
1. Inloggning tooyour Work.com företagets webbplats som administratör.

2. Gå för**installationsprogrammet**.
   
    ![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/IC794108.png "installationen")
3. Gå för**hantera användare \> användare**.
   
    ![Hantera användare](./media/active-directory-saas-work-com-tutorial/IC784369.png "hantera användare")

4. Klicka på **ny användare**.
   
    ![Alla användare](./media/active-directory-saas-work-com-tutorial/IC794117.png "alla användare")

5. Hello användare redigera avsnittet, utföra hello följa stegen i attribut för ett giltigt Azure AD-kontot som du vill använda tooprovision hello relaterade textrutor:
   
    ![Redigera användare](./media/active-directory-saas-work-com-tutorial/ic794118.png "Redigera användare")
   
    a. I hello **Förnamn** textruta typen hello **Förnamn** för hello användare **Britta**.
    
    b. I hello **efternamn** textruta typen hello **efternamn** för hello användare **Simon**.
    
    c. I hello **Alias** textruta typen hello **namn** för hello användare **BrittaS**.
    
    d. I hello **e-post** textruta typen hello **e-postadress** för användare  **Brittasimon@contoso.com** .
    
    e. I hello **användarnamn** textruta, Skriv ett användarnamn för användaren som  **Brittasimon@contoso.com** .
    
    f. I hello **smeknamn** textruta typ a **smeknamn** för användare **Simon**.
    
    g. Välj **rollen**, **användarlicens**, och **profil**.
    
    h. Klicka på **Spara**.  
      
    > [!NOTE]
    > hello Azure AD användare får ett e-postmeddelande inklusive en länk tooconfirm hello innan den aktiveras.
    > 
    > 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWork.com.

![Tilldela användare][200] 

**tooassign Britta Simon tooWork.com utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Work.com**.

    ![Work.com i appens lista](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Work.com programmet när du klickar på hello Work.com panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

