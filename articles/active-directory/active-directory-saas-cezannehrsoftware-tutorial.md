---
title: "Självstudier: Azure Active Directory-integrering med Cezanne HR programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Cezanne HR-programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 3675acd8871d62c2277def8074f7aa39ac46e2a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a>Självstudier: Integrera Azure Active Directory med Cezanne HR-programvara

I kursen får du lära dig hur toointegrate Cezanne HR-program med Azure Active Directory (AD Azure).

Integrera Cezanne HR-program med Azure AD ger dig hello följande fördelar. Du kan:

- Kontrollera i Azure AD som har åtkomst tooCezanne HR-programvara.
- Aktivera användare tooautomatically-inloggning tooCezanne HR-program med enkel inloggning (SSO) med sina Azure AD-konton.
- Hantera dina konton i en central plats: hello Azure-portalen.

toolearn mer information om programvara som en tjänst (SaaS) app-integrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Cezanne HR-programvara du behöver hello följande objekt:

- En Azure AD-prenumeration
- Cezanne HR-programvara SSO-aktiverade prenumeration

> [!NOTE]
> tootest hello steg i den här självstudiekursen, rekommenderar vi att du inte använder en produktionsmiljö.

tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD SSO i en testmiljö. 

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

* Lägga till Cezanne HR programvara från hello-galleriet
* Konfigurera och testa Azure AD-SSO

## <a name="add-cezanne-hr-software-from-hello-gallery"></a>Lägg till Cezanne HR programvara från hello-galleriet
tooconfigure hello integrering av Cezanne HR-program i Azure AD och lägga till Cezanne HR programvara från hello galleriet tooyour lista över hanterade SaaS-appar.

tooadd Cezanne HR programvara från galleriet hello hello följande:

1. I hello  **[Azure-portalen](https://portal.azure.com)**, i hello vänstra rutan, Välj hello **Azure Active Directory** knappen. 

    ![Hej ”Azure Active Directory”-knappen][1]

2. Välj **företagsprogram** > **alla program**.

    ![Hej ”alla program” länk][2]
    
3. tooadd ett nytt program hello överst i hello **alla program** dialogrutan **nytt program**.

    ![Hej ”nya program” knappen][3]

4. Skriv i sökrutan hello **Cezanne HR programvara**.

    ![hello sökrutan](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. Markera i hello resultat **Cezanne HR programvara** och välj sedan hello **Lägg till** knappen tooadd hello program.

    ![hello resultatlistan](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD SSO med Cezanne HR programvara baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow hello Cezanne HR programvara motsvarighet toohello Azure AD-användare. Med andra ord måste du upprätta en länk relationen mellan en Azure AD-användare och hello relaterade användaren i hello Cezanne HR-programvara.

tooestablish hello länken relationen, tilldela hello Cezanne HR programvara **användarnamn** värde som hello Azure AD **användarnamn** värde.

tooconfigure och testa Azure AD enkel inloggning med hjälp av Cezanne HR-program, fullständig hello följande byggblock.

### <a name="configure-azure-ad-sso"></a>Konfigurera Azure AD för enkel inloggning

I det här avsnittet kan du aktivera Azure AD SSO i hello Azure-portalen och konfigurera enkel inloggning i programmet Cezanne HR hello följande:

1. I hello Azure-portalen på hello **Cezanne HR programvara** programmet integration anger **enkel inloggning**.

    ![Hej ”enkel inloggning” kommando][4]

2. tooenable SSO i hello **enkel inloggning** dialogrutan, Välj hello **läge** som **SAML-baserade inloggning**.
 
    ![Hej ”Mode-ruta](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. Under **Cezanne HR programvara domän och URL: er**, hello följande:

    ![Hej ”Cezanne HR programvara domän och URL: er” avsnittet](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. I hello **inloggnings-URL** Skriv en URL som har hello följande syntax:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`

    b. I hello **Reply URL** Skriv en URL som har hello följande syntax:`https://w3.cezanneondemand.com:443/<tenantid>`    
     
    > [!NOTE] 
    > hello föregående värden är inte verkliga. Uppdatera dem med hello faktiska reply URL och hello inloggnings-URL. tooobtain hello värden, kontakta hello [Cezanne HR programvara klienten supportteamet](mailto:info@cezannehr.com).

4. Under **SAML-signeringscertifikat**väljer **certifikat (Base64)**, och sedan spara hello certifikatfilen på datorn.

    ![Hej ”SAML signeringscertifikat” avsnittet](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Välj **Spara**.

    ![Hej ”spara”.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. Under **Cezanne HR programvarukonfiguration**väljer **konfigurera Cezanne HR programvara** tooopen hello **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID** och **SAML enkel inloggning** URL: en från hello **Snabbreferens** avsnitt.

    ![Hej ”Cezanne HR programvara” konfigurationsavsnitt](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. Logga tooyour Cezanne HR programvara innehavaren som en administratör i en annan webbläsarfönster.

8. I hello vänster och välj **systeminställningarna**. Välj **säkerhetsinställningar** > **enkel inloggning Configuration**.

    ![Hej ”enkel inloggning Configuration” länk](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. I hello **Tillåt användare toolog i med hjälp av följande tjänster för enkel inloggning (SSO) hello** rutan, Välj hello **SAML 2.0** kryssrutan och välj hello **Advanced Configuration** alternativet.

    ![Enkel inloggning services-alternativ](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. Välj **lägga till nya**.

    ![Hej ”Lägg till ny” knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. Under **SAML 2.0 identitetsleverantörer**, hello följande:

    ![Hej ”identitetsleverantörer SAML 2.0” avsnittet](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. I hello **visningsnamn** ange hello namn för din identitetsprovider.

    b. I hello **identifiering** klistra in hello **SAML enhets-ID** som du kopierade från hello Azure-portalen. 

    c. I hello **SAML bindning** väljer **efter**.

    d. I hello **säkerhetstoken tjänstslutpunkten** klistra in hello **SAML enkel inloggning** URL som du kopierade från hello Azure-portalen. 
    
    e. I hello **användarnamn ID-attributet** ange `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. tooupload hello hämtat certifikat från Azure AD, Välj hello **överför** knappen.
    
    g. Välj **OK**. 

12. Välj **Spara**.

    ![Hej ”spara”.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> När du ställer in hello app kan du läsa en kortare version av hello före instruktioner i hello [Azure-portalen](https://portal.azure.com). När du har lagt till hello appen från hello **Active Directory** > **företagsprogram** avsnitt, Välj hello **enkel inloggning** fliken. Sedan åtkomst hello inbäddade dokumentation från hello **Configuration** avsnitt. 

toolearn mer om hello inbäddade dokumentationen funktionen finns [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
I det här avsnittet skapar du testanvändare Britta Simon i hello Azure-portalen.

![hello testanvändare Britta Simon][100]

toocreate en testanvändare i Azure AD hello följande:

1. I hello **Azure-portalen**, i hello vänstra rutan, Välj hello **Azure Active Directory** knappen.

    ![Hej ”Azure Active Directory”-knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, Välj **användare och grupper** > **alla användare**.
    
    ![Hej ”alla användare” länk](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    Hej **alla användare** öppnas.

3. tooopen hello **användare** dialogrutan **Lägg till**.
 
    ![Hej ”Lägg till”-knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. I hello **användaren** dialogrutan rutan, hello följande:
 
    ![dialogrutan för hello ”användare”](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** Skriv användaren Britta Simon **e-postadress**.

    c. Välj hello **visa lösenordet** kryssrutan och Observera hello värdet som har genererats i hello **lösenord** rutan.

    d. Välj **Skapa**.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Skapa en testanvändare för Cezanne HR-programvara

tooenable Azure AD-användare toosign i tooCezanne HR-program, måste de etableras i Cezanne HR-programvara. Hello gäller Cezanne HR programvara är etablering en manuell aktivitet.

Konfigurera ett användarkonto genom att göra hello följande:

1.  Logga in tooyour Cezanne HR programvara företagets webbplats som administratör.

2.  I hello vänster och välj **systeminställningarna** > **hantera användare** > **Lägg till nya användare**.

    ![Hej ”Lägg till nya användare” länken](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "ny användare")

3.  Under **detaljer**, hello följande:

    ![Hej ”Person” informationsavsnittet](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "ny användare")
    
    a. Ange **intern användare** som **OFF**.
    
    b. I hello **Förnamn** rutan, typen hello användarens förnamn, till exempel **Britta**.  
 
    c. I hello **efternamn** rutan, typen hello användarens efternamn, till exempel **Simon**.
    
    d. I hello **e-post** skriver hello användarens e-postadress, till exempel Brittasimon@contoso.com.

4.  Under **kontoinformation**, hello följande:

    ![Hej ”information om kontot”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "ny användare")
    
    a. I hello **användarnamn** skriver hello användarens e-postadress, till exempel Brittasimon@contoso.com.
    
    b. I hello **lösenord** skriver hello användarens lösenord.
    
    c. I hello **säkerhetsroll** väljer **HR Professional**.
    
    d. Välj **OK**.

5. På hello **enkel inloggning** på fliken hello **SAML 2.0 identifierare** väljer **Lägg till ny**.

    ![Hej ”Lägg till ny” knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "användare")

6. I hello **identitetsleverantör** Välj identitetsprovider. I hello **användar-ID** ange hello e-postadress för test användaren Britta Simons konto.

    ![Hej ”identitetsleverantör” och ”User ID”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "användare")
    
7. Välj **Spara**.

    ![Hej ”spara” knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "användare")

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera testanvändare Britta Simon toouse Azure SSO genom att bevilja åtkomst tooCezanne HR programvara.

![Testa användaråtkomst][200] 

1. Öppna hello program och gå sedan toohello directory vyn i hello Azure-portalen. Välj **företagsprogram** > **alla program**.

    ![Hej ”alla program” länk][201] 

2. Välj i listan med program hello **Cezanne HR programvara**.

    ![listan med hello ”program”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. Välj hello menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Välj **Lägg till**. I hello **Lägg uppdrag** dialogrutan **användare och grupper**.

    ![”Användare och grupper” länk][203]

5. I hello **användare och grupper** i dialogrutan hello **användare** väljer **Britta Simon**.

6. I hello **användare och grupper** dialogrutan **Välj**.

7. I hello **Lägg uppdrag** dialogrutan **tilldela**.
    
### <a name="test-sso"></a>Testa enkel inloggning

I det här avsnittet testa du konfigurationen av Azure AD SSO med hjälp av hello åtkomstpanelen.

När du väljer hello Cezanne HR programvara panelen i hello åtkomstpanelen logga in automatiskt tooyour Cezanne HR-program.

## <a name="next-steps"></a>Nästa steg

* [Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

