---
title: "Självstudier: Azure Active Directory-integrering med Zoho | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zoho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 5d1c196d3b97babab196f509ea68e7d61523a7e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a>Självstudier: Azure Active Directory-integrering med Zoho

I kursen får du lära dig hur toointegrate Zoho med Azure Active Directory (AD Azure).

Integrera Zoho med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooZoho.
- Du kan låta dina användare tooautomatically get inloggade tooZoho (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Zoho, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Zoho enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Zoho från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-zoho-from-hello-gallery"></a>Att lägga till Zoho från hello-galleriet
tooconfigure hello integrering av Zoho i Azure AD, behöver du tooadd Zoho hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Zoho från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Zoho**väljer **Zoho** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Zoho i hello resultatlistan](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zoho baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zoho är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Zoho toobe upprättas.

I Zoho, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Zoho, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Zoho](#create-a-zoho-test-user)**  -toohave en motsvarighet för Britta Simon i Zoho som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Zoho program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zoho:**

1. I hello Azure-portalen på hello **Zoho** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. På hello **Zoho domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och Zoho domän med enkel inloggning information](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.zohomail.com`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [Zoho klienten supportteamet](https://www.zoho.com/mail/contact.html) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. På hello **Zoho Configuration** klickar du på **konfigurera Zoho** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, ändra lösenord URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Zoho konfiguration](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. Logga in på webbplatsen för företagets Zoho e-post som en administratör i en annan webbläsarfönster.

8. Gå toohello **Kontrollpanelen**.
   
    ![Kontrollpanelen](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Kontrollpanelen")

9. Klicka på hello **SAML-autentisering** fliken.
   
    ![SAML-autentisering](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML-autentisering")

10. I hello **information om autentisering av SAML** avsnittet, utföra hello följande steg:
   
    ![Information om autentisering av SAML](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML-autentisering-information")
   
    a. I hello **Inloggningswebbadressen** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
   
    b. I hello **logga ut URL** textruta klistra in **Sign-Out URL** som du har kopierat från Azure-portalen.
   
    c. I hello **ändra lösenord URL** textruta klistra in **ändra lösenord URL** som du har kopierat från Azure-portalen.
       
    d. Öppna din Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **PublicKey** textruta.
   
    e. Som **algoritmen**väljer **RSA**.
   
    f. Klicka på **OK**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-zoho-test-user"></a>Skapa en testanvändare Zoho

I ordning tooenable Azure AD-användare toolog till Zoho e-post, måste de etableras i Zoho e-post. Etablering är en manuell aktivitet i hello fallet Zoho e-post.

> [!NOTE]
> Du kan använda något annat Zoho e användarens konto skapas verktyg eller API: er som tillhandahålls av Zoho e tooprovision AAD användarkonton.

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision ett användarkonto, utför följande steg hello:

1. Logga in tooyour **Zoho e** företagets webbplats som administratör.

2. Gå för**Kontrollpanelen \> e-post och dokument**.

3. Gå för**användarinformation \> Lägg till användare**.
   
    ![Lägg till användare](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "lägga till användare")

4. På hello **lägga till användare** dialogrutan utföra hello följande steg:
   
    ![Lägg till användare](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "lägga till användare")
   
    a. I hello **Förnamn** textruta hello första-typnamn för användaren som **Britta**.

    b. I hello **efternamn** textruta Skriv hello efternamn för användaren som **Simon**.

    c. I hello **e-post-ID** textruta hello e-post-id för användaren som  **brittasimon@contoso.com** .

    d. I hello **lösenord** textruta ange lösenord för användaren.
   
    e. Klicka på **OK**.  
      
    > [!NOTE]
    > hello Azure Active Directory användare får ett e-postmeddelande med en länk tooconfirm hello-konto innan den aktiveras.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZoho.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooZoho utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Zoho**.

    ![Hej Zoho länken i listan med program hello](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Zoho programmet när du klickar på hello Zoho panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

