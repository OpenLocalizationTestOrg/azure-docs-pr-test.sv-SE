---
title: "Självstudier: Azure Active Directory-integrering med Picturepark | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Picturepark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 3d826d3f73aad2f0d123f8697c6caafad7bc926a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Självstudier: Azure Active Directory-integrering med Picturepark

I kursen får du lära dig hur toointegrate Picturepark med Azure Active Directory (AD Azure).

Integrera Picturepark med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooPicturepark
- Du kan aktivera din användare tooautomatically get inloggade tooPicturepark (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Picturepark, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Picturepark enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Picturepark från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-picturepark-from-hello-gallery"></a>Att lägga till Picturepark från hello-galleriet
tooconfigure hello integrering av Picturepark i Azure AD, behöver du tooadd Picturepark hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Picturepark från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Picturepark**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. Markera hello resultat på panelen **Picturepark**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Picturepark baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Picturepark är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Picturepark toobe upprättas.

I Picturepark, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Picturepark, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Picturepark](#creating-a-picturepark-test-user)**  -toohave en motsvarighet för Britta Simon i Picturepark som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Picturepark program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Picturepark:**

1. I hello Azure-portalen på hello **Picturepark** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. På hello **Picturepark domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.picturepark.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret: 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Picturepark klienten supportteamet](https://picturepark.com/about/contact/) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. På hello **Picturepark Configuration** klickar du på **konfigurera Picturepark** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. Logga in på webbplatsen Picturepark företag som en administratör i en annan webbläsarfönster.

8. Klicka i hello verktygsfältet hello längst upp **Administrationsverktyg**, och klicka sedan på **hanteringskonsolen**.
   
    ![Konsolen för hantering av](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")

9. Klicka på **autentisering**, och klicka sedan på **identitetsleverantörer**.
   
    ![Autentisering](./media/active-directory-saas-picturepark-tutorial/ic795063.png "autentisering")

10. I hello **identitet providerkonfigurationen** avsnittet, utföra hello följande steg:
   
    ![Identitet providerkonfigurationen](./media/active-directory-saas-picturepark-tutorial/ic795064.png "identitet providerkonfigurationen")
   
    a. Klicka på **Lägg till**.
  
    b. Ange ett namn för din konfiguration.
   
    c. Välj **anges som standard**.
   
    d. I **Issuer URI** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
   
    e. I **betrodda utfärdare USB utskrifts** textruta klistra in hello värdet för **tumavtrycket** som du har kopierat från **SAML-signeringscertifikat** avsnitt. 

11. Klicka på **JoinDefaultUsersGroup**.

12. tooset hello **Emailaddress** attribut i hello **anspråk** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` och klicka på **spara**.

      ![Konfigurationen](./media/active-directory-saas-picturepark-tutorial/ic795065.png "konfiguration")

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-picturepark-test-user"></a>Skapa en testanvändare Picturepark

I ordning tooenable Azure AD-användare toolog i Picturepark, måste de etableras i Picturepark. Hello gäller Picturepark är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **Picturepark** klient.

2. Klicka i hello verktygsfältet hello längst upp **Administrationsverktyg**, och klicka sedan på **användare**.
   
    ![Användare](./media/active-directory-saas-picturepark-tutorial/ic795067.png "användare")

3. I hello **översikt över användare** klickar du på **ny**.
   
    ![Användarhantering](./media/active-directory-saas-picturepark-tutorial/ic795068.png "användarhantering")

4. På hello **skapa användare** dialogrutan hello utför följande steg på en giltig Azure Active Directory-användare som du vill tooprovision:
   
    ![Skapa användare](./media/active-directory-saas-picturepark-tutorial/ic795069.png "skapa användare")
   
    a. I hello **e-postadress** textruta typen hello **e-postadress** för hello användare  **BrittaSimon@contoso.com** .  
   
    b. I hello **lösenord** och **Bekräfta lösenord** textrutor, typen hello **lösenord** av BrittaSimon. 
   
    c. I hello **Förnamn** textruta typen hello **Förnamn** för hello användare **Britta**. 
   
    d. I hello **efternamn** textruta typen hello **efternamn** för hello användare **Simon**.
   
    e. I hello **företagets** textruta typen hello **företagsnamn** för hello användare. 
   
    f. I hello **land** textruta väljer hello **land** för hello användare.
  
    g. I hello **ZIP** textruta typen hello **postnummer** på hello ort.
   
    h. I hello **Stad** textruta typen hello **Ortnamn** för hello användare.

    Jag. Välj en **språk**.
   
    j. Klicka på **Skapa**.

>[!NOTE]
>Du kan använda något annat Picturepark användarens konto skapas verktyg eller API: er som tillhandahålls av Picturepark tooprovision användarkonton i Azure AD.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPicturepark.

![Tilldela användare][200] 

**tooassign Britta Simon tooPicturepark utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Picturepark**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Picturepark programmet när du klickar på hello Picturepark panelen i hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png

