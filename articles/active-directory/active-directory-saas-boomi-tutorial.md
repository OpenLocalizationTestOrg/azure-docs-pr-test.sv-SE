---
title: "Självstudier: Azure Active Directory-integrering med Boomi | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Boomi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ce64a4561697d311a8c7b1b244315bb552c5cfb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Självstudier: Azure Active Directory-integrering med Boomi

I kursen får du lära dig hur toointegrate Boomi med Azure Active Directory (AD Azure).

Integrera Boomi med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooBoomi
- Du kan aktivera din användare tooautomatically get inloggade tooBoomi (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Boomi, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Boomi enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Boomi från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-boomi-from-hello-gallery"></a>Att lägga till Boomi från hello-galleriet
tooconfigure hello integrering av Boomi i Azure AD, behöver du tooadd Boomi hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Boomi från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Boomi**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. Markera hello resultat på panelen **Boomi**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Boomi baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Boomi är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Boomi toobe upprättas.

I Boomi, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Boomi, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Boomi](#creating-a-boomi-test-user)**  -toohave en motsvarighet för Britta Simon i Boomi som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Boomi program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Boomi:**

1. I hello Azure-portalen på hello **Boomi** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. På hello **Boomi domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://platform.boomi.com/sso/<accountname>/saml`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://platform.boomi.com/sso/<accountname>/saml`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [Boomi supportteamet](https://boomi.com/company/contact/) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. Boomi program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan för varje rad som visas i hello nedan, utföra hello följande steg:

    | Attributets namn | Attributvärdet |
    | -------------- | --------------- |
    | FEDERATION_ID | User.Mail |
    
    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.
    
    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **OK**.

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. På hello **Boomi Configuration** klickar du på **konfigurera Boomi** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. Logga in på webbplatsen Boomi företag som en administratör i en annan webbläsarfönster. 

9. Navigera för**företagsnamn** och gå för**konfigurera**.

10. Klicka på hello **SSO-alternativ** fliken och utföra nedanstående steg.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    a. Kontrollera **aktivera SAML enkel inloggning** kryssrutan.

    b. Klicka på **importera** tooupload hello hämtat certifikat från Azure AD för**providern identitetscertifikat**.
    
    c. I hello **identitet providern inloggnings-URL** textruta placera hello värdet för **SAML inloggning tjänst-URL för enkel** från Azure AD-konfigurationsfönstret.

    d. Som **Federation Id plats**väljer **Federation-Id är i FEDERATION_ID attributelementet** knappen. 

    e. Klicka på **spara** knappen.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-boomi-test-user"></a>Skapa en testanvändare Boomi

I ordning tooenable Azure AD-användare toolog i tooBoomi, måste de etableras i Boomi. Hello gäller Boomi är etablering en manuell aktivitet.

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision ett användarkonto, utför följande steg hello:

1. Logga in tooyour Boomi företagets webbplats som administratör.

2. När du loggar in, navigera för**Användarhantering** och gå för**användare**.

    ![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "användare")

3. Klicka på  **+**  ikon och hello **Lägg till/Underhåll användarroller** öppnas.

    ![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "användare")

    ![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "användare")

    a. I hello **användarens e-postadress** textruta hello e-post för användare som BrittaSimon@contoso.com.
    
    b. I hello **Förnamn** textruta hello första-typnamn för användaren som Britta.

    c. I hello **efternamn** textruta Skriv hello efternamn för användaren som Simon.
    
    d. Ange hello användare **Federation ID**. Varje användare måste ha ett ID för Federation som unikt identifierar hello användaren i hello-konto.
    
    e. Tilldela hello **standardanvändare** rollen toohello användare. Tilldela inte hello administratörsroll eftersom som skulle ge honom normal luften åtkomst samt åtkomst för enkel inloggning.
    
    f. Klicka på **OK**.
    
    > [!NOTE]
    > hello användaren får inte en Välkommen e-postmeddelandet som innehåller ett lösenord som kan vara används toolog i toohello AtomSphere konto eftersom sitt lösenord hanteras via hello identitetsleverantör. Du kan använda andra Boomi användarens konto skapas verktyg eller API: er som tillhandahålls av Boomi tooprovision AAD-användarkonton. 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBoomi.

![Tilldela användare][200] 

**tooassign Britta Simon tooBoomi utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Boomi**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Boomi programmet när du klickar på hello Boomi panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

