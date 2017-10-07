---
title: "Självstudier: Azure Active Directory-integrering med Egnyte | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Egnyte."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: d86fa28a1e7b23474cb76c08aef8539acadaf24d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Självstudier: Azure Active Directory-integrering med Egnyte

I kursen får du lära dig hur toointegrate Egnyte med Azure Active Directory (AD Azure).

Integrera Egnyte med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooEgnyte
- Du kan aktivera din användare tooautomatically get inloggade tooEgnyte (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Egnyte, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Egnyte enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Egnyte från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-egnyte-from-hello-gallery"></a>Att lägga till Egnyte från hello-galleriet
tooconfigure hello integrering av Egnyte i Azure AD, behöver du tooadd Egnyte hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Egnyte från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Egnyte**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. Markera hello resultat på panelen **Egnyte**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Egnyte baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Egnyte är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Egnyte toobe upprättas.

I Egnyte, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Egnyte, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Egnyte](#creating-an-egnyte-test-user)**  -toohave en motsvarighet för Britta Simon i Egnyte som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Egnyte program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Egnyte:**

1. I hello Azure-portalen på hello **Egnyte** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. På hello **Egnyte domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [Egnyte klienten supportteamet](https://www.egnyte.com/corp/contact_egnyte.html) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. På hello **Egnyte Configuration** klickar du på **konfigurera Egnyte** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. Logga in tooyour Egnyte företagets webbplats som en administratör i en annan webbläsarfönster.

8. Klicka på **inställningar**.
   
   ![Inställningar för](./media/active-directory-saas-egnyte-tutorial/ic787819.png "inställningar")

9. Klicka på menyn hello **inställningar**.

   ![Inställningar för](./media/active-directory-saas-egnyte-tutorial/ic787820.png "inställningar")

10. Klicka på hello **Configuration** fliken och klicka sedan på **säkerhet**.

    ![Säkerhet](./media/active-directory-saas-egnyte-tutorial/ic787821.png "säkerhet")

11. I hello **autentisering för enkel inloggning** avsnittet, utföra hello följande steg:

    ![Enkel inloggning för autentisering](./media/active-directory-saas-egnyte-tutorial/ic787822.png "enkel inloggning för autentisering")   
    
    a. Som **autentisering för enkel inloggning**väljer **SAML 2.0**.
   
    b. Som **identitetsleverantör**väljer **AzureAD**.
   
    c. Klistra in **SAML enkel inloggning Tjänstwebbadress** kopieras från Azure-portalen till hello **identitet providern inloggnings-URL** textruta.
   
    d. Klistra in **SAML enhets-ID** som du har kopierat från Azure-portalen i hello **identitet providern enhets-ID** textruta.
      
    e. Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **providern identitetscertifikat** textruta.
   
    f. Som **standard mappning**väljer **e-postadress**.
   
    g. Som **använder domänspecifika utfärdaren värdet**väljer **inaktiveras**.
   
    h. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-egnyte-test-user"></a>Skapa en testanvändare Egnyte

tooenable Azure AD-användare toolog i tooEgnyte, måste de etableras i Egnyte. Hello gäller Egnyte är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour **Egnyte** företagets webbplats som administratör.

2. Gå för**inställningar \> användare och grupper**.

3. Klicka på **Lägg till nya användare**, och välj sedan hello typ för användare som du vill tooadd.
   
   ![Användare](./media/active-directory-saas-egnyte-tutorial/ic787824.png "användare")

4. I hello **nya standardanvändare** avsnittet, utföra hello följande steg:
   
   ![Nya standardanvändare](./media/active-directory-saas-egnyte-tutorial/ic787825.png "ny vanlig användare")   

   a. Typen hello **e-post**, **användarnamn**, och annan information om ett giltigt Azure Active Directory-konto som du vill tooprovision.
   
   b. Klicka på **Spara**.
    
    >[!NOTE]
    >hello Azure Active Directory användare får ett e-postmeddelande.
    >

>[!NOTE]
>Du kan använda något annat Egnyte användarens konto skapas verktyg eller API: er som tillhandahålls av Egnyte tooprovision AAD-användarkonton.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEgnyte.

![Tilldela användare][200] 

**tooassign Britta Simon tooEgnyte utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Egnyte**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Egnyte programmet när du klickar på hello Egnyte panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

