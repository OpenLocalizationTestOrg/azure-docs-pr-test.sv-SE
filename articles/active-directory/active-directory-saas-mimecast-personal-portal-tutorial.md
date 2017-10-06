---
title: "Självstudier: Azure Active Directory-integrering med Mimecast personliga Portal | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Mimecast personliga Portal."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ee2a8edcab36f295732ac1ebe641ed7fcfc1f2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Självstudier: Azure Active Directory-integrering med Mimecast personliga Portal

I kursen får du lära dig hur toointegrate Mimecast personliga Portal med Azure Active Directory (AD Azure).

Integrera Mimecast personliga Portal med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooMimecast personliga Portal
- Du kan aktivera din användare tooautomatically get inloggade tooMimecast personliga Portal (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Mimecast personliga Portal, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Mimecast personliga Portal enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Mimecast personliga portalen från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-mimecast-personal-portal-from-hello-gallery"></a>Att lägga till Mimecast personliga portalen från hello-galleriet
tooconfigure hello integrering av Mimecast personliga portalen i Azure AD, behöver du tooadd Mimecast personliga Portal hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Mimecast personliga portalen från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Mimecast personliga Portal**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_search.png)

5. Markera hello resultat på panelen **Mimecast personliga Portal**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Mimecast personliga Portal baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Mimecast personliga Portal är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Mimecast personliga Portal toobe upprättas.

I Mimecast personliga Portal, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Mimecast personliga Portal, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Mimecast personliga Portal](#creating-a-mimecast-personal-portal-test-user)**  -toohave en motsvarighet för Britta Simon i Mimecast personliga Portal som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Mimecast personliga Portal.

**tooconfigure Azure AD enkel inloggning med Mimecast personliga Portal, utför hello följande steg:**

1. I hello Azure-portalen på hello **Mimecast personliga Portal** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. På hello **Mimecast personliga Portal domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret: 
    | |     
    | ----------------------------------------|
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|
    | |
   
    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:

    | |     
    | --- |
    | `https://webmail-us.mimecast.com/sso/<companyname>`|
    | `https://webmail-uk.mimecast.com/sso/<companyname>`|    
    | `https://webmail-za.mimecast.com/sso/<companyname>`|
    | `https://webmail.mimecast-offshore.com/sso/<companyname>`|
    ||                                                 
    
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Mimecast personliga Portal klienten supportteamet](https://www.mimecast.com/customer-success/technical-support/) tooget dessa värden. 
 


4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. På hello **Mimecast personliga Portal Configuration** klickar du på **konfigurera Mimecast personliga Portal** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. Logga in på ditt personliga Mimecast Portal som administratör i ett annat webbläsarfönster.

8. Gå för**Services \> program**.
   
    ![Program](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "program")

9. Klicka på **autentisering profiler**.
   
    ![Profiler för autentisering](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "profiler för autentisering")

10. Klicka på **nya autentiseringsprofil**.
   
    ![Nya autentiseringsprofil](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "ny autentiseringsprofil")

11. I hello **autentiseringsprofil** avsnittet, utföra hello följande steg:
   
    ![Autentiseringsprofil](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "autentiseringsprofil")
   
    a. I hello **beskrivning** textruta, ange ett namn för din konfiguration.
   
    b. Välj **genomdriva SAML-autentisering för Mimecast Personal Portal**.
   
    c. Som **Provider**väljer **Azure Active Directory**.
   
    d. I **utfärdar-URL** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.
   
    e. I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
   
    f. I **logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.

    g. Öppna din **Base64-** kodat certifikat i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **identitetscertifikat Provider (Metadata)** textruta.

    h. Välj **tillåta enkel inloggning på**.
   
    Jag. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-mimecast-personal-portal-test-user"></a>Skapa en testanvändare Mimecast personliga Portal

I ordning tooenable Azure AD-användare toolog till Mimecast personliga Portal, måste de etableras i Mimecast personliga Portal. Hello gäller Mimecast personliga Portal är etablering en manuell aktivitet.

Du måste tooregister en domän innan du kan skapa användare.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **Mimecast personliga Portal** som administratör.

2. Gå för**kataloger \> internt**.
   
    ![Kataloger](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "kataloger")

3. Klicka på **registrera nya domän**.
   
    ![Registrera en ny domän](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "registrera ny domän")

4. När den nya domänen har skapats, klickar du på **ny adress**.
   
    ![Ny adress](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "ny adress")

5. Hello nya adress dialog, utföra hello följa stegen i ett giltigt Azure AD-kontot som du vill tooprovision:
   
    ![Spara](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "spara")
   
    a. I hello **e-postadress** textruta typen **e-postadress** för hello användare som  **BrittaSimon@contoso.com** .
    
    b. I hello **globalt namn** textruta typen hello **användarnamn** som **BrittaSimon**.

    c. I hello **lösenord**, och **Bekräfta lösenord** textrutor, typen hello **lösenord** för hello användare.
   
    b. Klicka på **Spara**.

>[!NOTE]
>Du kan använda andra Mimecast personliga Portal användare skapa verktyg eller API: er som tillhandahålls av Mimecast personliga Portal tooprovision Azure AD-användarkonton. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMimecast personliga Portal.

![Tilldela användare][200] 

**tooassign Britta Simon tooMimecast personliga Portal utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Mimecast personliga Portal**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning
I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Mimecast personliga portalprogram när du klickar på hello Mimecast personliga Portal-panelen i hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

