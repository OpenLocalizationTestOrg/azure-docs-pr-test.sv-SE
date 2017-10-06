---
title: "Självstudier: Azure Active Directory-integrering med Litmos | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Litmos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 026fd10058760f2d63d185ef4aa9d7de3b82525e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Självstudier: Azure Active Directory-integrering med Litmos

I kursen får du lära dig hur toointegrate Litmos med Azure Active Directory (AD Azure).

Integrera Litmos med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooLitmos.
- Du kan låta dina användare tooautomatically get inloggade tooLitmos (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Litmos, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Litmos enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Litmos från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-litmos-from-hello-gallery"></a>Att lägga till Litmos från hello-galleriet
tooconfigure hello integrering av Litmos i Azure AD, behöver du tooadd Litmos hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Litmos från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Litmos**väljer **Litmos** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Litmos i hello resultatlistan](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Litmos baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Litmos är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Litmos toobe upprättas.

I Litmos, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Litmos, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Litmos](#create-a-litmos-test-user)**  -toohave en motsvarighet för Britta Simon i Litmos som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Litmos program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Litmos:**

1. I hello Azure-portalen på hello **Litmos** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_samlbase.png)

3. På hello **Litmos domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och Litmos domän med enkel inloggning information](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.litmos.com/account/Login`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.litmos.com/integration/samllogin`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och Reply URL, som beskrivs senare i självstudiekursen eller kontakta [Litmos supportteam](https://www.litmos.com/contact-us/) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_certificate.png)

5. Som en del av hello konfigurationen, måste toocustomize hello **SAML-Token attribut** för tillämpningsprogrammet Litmos.

    ![Attributet avsnitt](./media/active-directory-saas-litmos-tutorial/tutorial_attribute.png)
           
    | Attributets namn   | Attributvärdet |   
    | ---------------  | ----------------|
    | Förnamn |User.givenName |
    | Efternamn  |User.surname |
    | E-post |User.Mail |

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Lägg till attribut](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_04.png)

    ![Lägg till attribut Dailog](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_05.png)

    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **OK**.     

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-litmos-tutorial/tutorial_general_400.png)

7. I ett annat fönster i webbläsaren, inloggning tooyour Litmos företagets webbplats som administratör.

8. I navigeringsfältet hello vänster hello klickar du på **konton**.
   
    ![Kontoavsnittet på App-sida][22] 

9. Klicka på hello **integreringar** fliken.
   
    ![Fliken Integration][23] 

10. På hello **integreringar** , bläddrar du ned för**3 part integreringar**, och klicka sedan på **SAML 2.0** fliken.
   
    ![SAML 2.0 avsnitt][24] 

11. Kopiera hello värde under **hello SAML-slutpunkt för litmos är:** och klistra in den i hello **Reply URL** TextBox-kontroll i hello **Litmos domän och URL: er** avsnitt i Azure-portalen. 
   
    ![SAML-slutpunkt][26] 

12. I din **Litmos** program, utföra hello följande steg:
    
     ![Litmos program][25] 
     
     a. Klicka på **aktivera SAML**.
    
     b. Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **SAML X.509-certifikat** textruta.
     
     c. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-litmos-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-litmos-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
  
### <a name="create-a-litmos-test-user"></a>Skapa en testanvändare Litmos

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Litmos.  
Hej Litmos programmet stöder Just-in-Time-etablering. Det innebär ett konto skapas automatiskt om det behövs under ett försök tooaccess hello-program med hello åtkomstpanelen.

**toocreate en användare som kallas Britta Simon i Litmos, utför följande steg hello:**

1. I ett annat fönster i webbläsaren, inloggning tooyour Litmos företagets webbplats som administratör.

2. I navigeringsfältet hello vänster hello klickar du på **konton**.
   
    ![Kontoavsnittet på App-sida][22] 

3. Klicka på hello **integreringar** fliken.
   
    ![Fliken integreringar][23] 

4. På hello **integreringar** , bläddrar du ned för**3 part integreringar**, och klicka sedan på **SAML 2.0** fliken.
   
    ![SAML 2.0][24] 
    
5. Välj **Autogenerate användare**
   
    ![Automatiskt genererat användare][27] 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLitmos.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooLitmos utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Litmos**.

    ![Hej Litmos länken i listan med program hello](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.  

Du bör få automatiskt inloggade tooyour Litmos programmet när du klickar på hello Litmos panelen i hello åtkomstpanelen. 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png

