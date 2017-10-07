---
title: "Självstudier: Azure Active Directory-integrering med UserVoice | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och UserVoice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 9eade8435ae6c6a3821bbbec9ab7c27ed7ad91ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Självstudier: Azure Active Directory-integrering med UserVoice

I kursen får du lära dig hur toointegrate UserVoice med Azure Active Directory (AD Azure).

Integrera UserVoice med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooUserVoice.
- Du kan låta dina användare tooautomatically get inloggade tooUserVoice (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med UserVoice, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En UserVoice enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till UserVoice från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-uservoice-from-hello-gallery"></a>Att lägga till UserVoice från hello-galleriet
tooconfigure hello integrering av UserVoice i Azure AD, behöver du tooadd UserVoice hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd UserVoice från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **UserVoice**väljer **UserVoice** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![UserVoice i hello resultatlistan](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med UserVoice baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i UserVoice är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i UserVoice toobe upprättas.

I UserVoice, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med UserVoice, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare UserVoice](#create-a-uservoice-test-user)**  -toohave en motsvarighet för Britta Simon i UserVoice som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt UserVoice-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med UserVoice:**

1. I hello Azure-portalen på hello **UserVoice** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. På hello **UserVoice domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och UserVoice domän med enkel inloggning information](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.UserVoice.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [UserVoice klienten supportteamet](https://www.uservoice.com/) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. På hello **UserVoice Configuration** klickar du på **konfigurera UserVoice** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![UserVoice-konfiguration](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. Logga in tooyour UserVoice-webbplatsen för företag som en administratör i en annan webbläsarfönster.

8. Klicka i hello verktygsfältet hello längst upp **inställningar**, och välj sedan **webbportalen** hello-menyn.
   
    ![Inställningar på sidan App](./media/active-directory-saas-uservoice-tutorial/ic777519.png "inställningar")

9. På hello **webbportalen** på fliken hello **användarautentisering** klickar du på **redigera** tooopen hello **redigera användarautentisering** dialogrutan sidan.
   
    ![Webbportalen fliken](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web-portalen")

10. På hello **redigera användarautentisering** dialogrutan utför hello följande steg:
   
    ![Redigera användarautentisering](./media/active-directory-saas-uservoice-tutorial/ic777521.png "redigera användarautentisering")
   
    a. Klicka på **enkel inloggning (SSO)**.
 
    b. Klistra in hello **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **SSO Remote inloggning** textruta.

    c. Klistra in hello **Sign-Out URL** -värde som du har kopierat från hello Azure-portalen i hello **SSO Remote Sign-Out textruta**.
 
    d. Klistra in hello **tumavtrycket** -värde som du har kopierat från Azure-portalen i den **aktuella certifikat SHA1 fingeravtryck** textruta.
    
    e. Klicka på **spara autentiseringsinställningarna**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-uservoice-test-user"></a>Skapa en testanvändare UserVoice

tooenable Azure AD-användare toolog i tooUserVoice, måste de etableras till UserVoice. Hello gäller UserVoice är etablering en manuell aktivitet.

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision ett användarkonto, utför följande steg hello:
1. Logga in tooyour **UserVoice** klient.

2. Gå för**inställningar**.
   
    ![Inställningar för](./media/active-directory-saas-uservoice-tutorial/ic777811.png "inställningar")

3. Klicka på **allmänna**.

4. Klicka på **agenter och behörigheter**.
   
    ![Agenter och behörigheter](./media/active-directory-saas-uservoice-tutorial/ic777812.png "agenter och behörigheter")

5. Klicka på **lägger till administratörer**.
   
    ![Lägg till administratörer](./media/active-directory-saas-uservoice-tutorial/ic777813.png "lägger till administratörer")

6. På hello **bjuda in administratörer** dialogrutan utföra hello följande steg:
   
    ![Bjud in administratörer](./media/active-directory-saas-uservoice-tutorial/ic777814.png "bjuda in administratörer")
   
    a. Ange hello hello konto du vill tooprovision och klicka sedan på e-postadress i hello e-postmeddelanden textruta **Lägg till**.
   
    b. Klicka på **bjuda in**.

> [!NOTE]
> Du kan använda något annat UserVoice användarens konto skapas verktyg eller API: er som tillhandahålls av UserVoice tooprovision AAD-användarkonton.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooUserVoice.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooUserVoice utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **UserVoice**.

    ![Hej UserVoice länken i listan med program hello](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour UserVoice programmet när du klickar på hello UserVoice-panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

