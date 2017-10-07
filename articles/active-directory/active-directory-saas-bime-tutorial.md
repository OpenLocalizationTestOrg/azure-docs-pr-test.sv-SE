---
title: "Självstudier: Azure Active Directory-integrering med Bime | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Bime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 1213725028dd8ce90f22fa6e9c50ffabebc8f3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a>Självstudier: Azure Active Directory-integrering med Bime

I kursen får du lära dig hur toointegrate Bime med Azure Active Directory (AD Azure).

Integrera Bime med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooBime
- Du kan aktivera din användare tooautomatically get inloggade tooBime (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Bime, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Bime enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Bime från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-bime-from-hello-gallery"></a>Att lägga till Bime från hello-galleriet
tooconfigure hello integrering av Bime i Azure AD, behöver du tooadd Bime hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Bime från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Bime**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/tutorial_bime_search.png)

5. Markera hello resultat på panelen **Bime**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/tutorial_bime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Bime baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Bime är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Bime toobe upprättas.

I Bime, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Bime, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Bime](#creating-a-bime-test-user)**  -toohave en motsvarighet för Britta Simon i Bime som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Bime program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Bime:**

1. I hello Azure-portalen på hello **Bime** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_samlbase.png)

3. På hello **Bime domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.Bimeapp.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.Bimeapp.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Bime klienten supportteamet](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet från hello-certifikatet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_general_400.png)

6. På hello **Bime Configuration** klickar du på **konfigurera Bime** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_configure.png) 

7. Logga in på webbplatsen Bime företag som en administratör i en annan webbläsarfönster.

8. Klicka i verktygsfältet hello **Admin**, och sedan **konto**.
   
    ![Admin](./media/active-directory-saas-bime-tutorial/ic775558.png "Admin")

9. På konfigurationssidan för hello konto utför hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/ic775559.png "Konfigurera enkel inloggning")
   
    a. Välj **aktivera SAML-autentisering**.

    b. I hello **Remote inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.

    c.  Klistra in hello **tumavtrycket** värdet från Azure-portalen i hello **certifikat fingeravtryck** textruta.       
   
    d. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-bime-test-user"></a>Skapa en testanvändare Bime

I ordning tooenable Azure AD-användare toolog i tooBime, måste de etableras i Bime. Hello gäller Bime är etablering en manuell aktivitet.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **Bime** klient.

2. Klicka i verktygsfältet hello **Admin**, och sedan **användare**.
   
    ![Admin](./media/active-directory-saas-bime-tutorial/ic775561.png "Admin")

3. I hello **användarlistan**, klickar du på **Lägg till nya användare** (”+”).
   
    ![Användare](./media/active-directory-saas-bime-tutorial/ic775562.png "användare")

4. På hello **användarinformation** dialogrutan utför hello följande steg:
   
    ![Användarinformation](./media/active-directory-saas-bime-tutorial/ic775563.png "användarinformation")
   
    a. I hello **Förnamn** textruta anger hello först namnet på användaren som **Britta**.

    b. I hello **efternamn** textruta ange hello efternamn för användaren som **Simon**.
 
    c. I hello **e-post** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .

    d. Klicka på **Spara**.

>[!NOTE]
>Du kan använda något annat Bime användarens konto skapas verktyg eller API: er som tillhandahålls av Bime tooprovision AAD-användarkonton.
>  

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBime.

![Tilldela användare][200] 

**tooassign Britta Simon tooBime utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Bime**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Bime programmet när du klickar på hello Bime panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bime-tutorial/tutorial_general_203.png

