---
title: "Självstudier: Azure Active Directory-integrering med ScaleX Enterprise | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ScaleX Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: e398b98d9e0957b5f92c82359651c345d22c3a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Självstudier: Azure Active Directory-integrering med ScaleX Enterprise

I kursen får du lära dig hur toointegrate ScaleX Enterprise med Azure Active Directory (AD Azure).

Integrera ScaleX Enterprise med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooScaleX Enterprise
- Du kan aktivera din användare tooautomatically get inloggade tooScaleX Enterprise (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD. Vad är programåtkomst och enkel inloggning med [Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med ScaleX Enterprise, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En ScaleX Enterprise enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om detta är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till ScaleX Enterprise från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-scalex-enterprise-from-hello-gallery"></a>Att lägga till ScaleX Enterprise från hello-galleriet
tooconfigure hello integrering av ScaleX Enterprise i tooAzure AD, behöver du tooadd ScaleX Enterprise hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd ScaleX Enterprise från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **ScaleX Enterprise**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. Markera hello resultat på panelen **ScaleX Enterprise**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ScaleX Enterprise baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ScaleX företag är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i ScaleX Enterprise toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ScaleX företag.

tooconfigure och testa Azure AD enkel inloggning med ScaleX Enterprise, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  -toohave en motsvarighet för Britta Simon i ScaleX företag som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ScaleX Enterprise-programmet.

**Utför följande hello tooconfigure Azure AD enkel inloggning med ScaleX Enterprise:**

1. I hello Azure-portalen på hello **ScaleX Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. På hello **ScaleX företagsdomänen och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. I hello **identifierare** textruta hello TYPVÄRDE med hello följande mönster:`https://platform.rescale.com/saml2/<company id>/`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://platform.rescale.com/saml2/<company id>/acs/`

4. Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Detta är inte hello verkliga värden. Uppdatera dessa värden med hello faktiska identifierare, Reply URL eller inloggnings-URL. Kontakta [ScaleX Enterprise-klienten supportteamet](http://info.rescale.com/contact_sales) tooget dessa värden. 

5. Tillämpningsprogrammet ScaleX förväntar hello SAML intyg i ett specifikt format, vilket kräver att du toomodify attributet mappningar tooyour SAML-token attribut-konfiguration. Klicka på **visa och redigera andra användarattribut** kryssrutan tooopen hello anpassade attribut inställningar.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Högerklicka på hello attributet **namnet** och klicka på Ta bort.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    b. Klicka på **e-postadress** attributet tooopen hello Redigera attribut fönster. Ändra dess värde från **user.mail** för**user.userprincipalname** och klicka på Ok.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. På hello **ScaleX företagskonfiguration** klickar du på **konfigurera ScaleX Enterprise** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. tooconfigure enkel inloggning på **ScaleX Enterprise** sida, inloggning toohello ScaleX Enterprise företagets webbplats som administratör.

9. Klicka på hello-menyn i hello övre högra och välj **Contoso Administration**.

    > [!NOTE] 
    > Contoso är bara ett exempel. Detta bör vara faktiska företagets namn. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. Välj **integreringar** hello översta menyn och välj **enkel inloggning**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. Fylla hello på följande sätt:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Välj **”skapa alla användare som kan autentisera med enkel inloggning”.**

    b. **Saml-leverantör**: klistra in hello värde ***urn: oasis: namn: tc: SAML:2.0:nameid-format: beständiga***

    c. **Namnet på identitetsleverantör e-fält i ACS-svar**: klistra in hello värde`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Identitet providern EntityDescriptor enhets-ID:** klistra in hello **SAML enhets-ID** värde kopieras från hello Azure-portalen.

    e. **Identitet providern SingleSignOnService URL:** klistra in hello **SAML inloggning tjänst-URL för enkel** från hello Azure-portalen.

    f. **Providern offentliga X509 identitetscertifikat:** öppna hello X509 certifikat hämtas från hello Azure i anteckningar och klistra in hello innehållet i den här rutan. Se till att det finns inga radbrytningar i hello mellersta hello certifikat innehåll.
    
    g. Kontrollera följande kryssrutorna hello: **aktiverad, kryptera NameID och logga AuthnRequests.**

    h. Klicka på **SSO uppdateringsinställningar** toosave hello inställningar.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>Skapa en testanvändare ScaleX Enterprise

tooenable Azure AD-användare toolog i tooScaleX Enterprise, måste de vara etablerade i tooScaleX Enterprise. Etablering är en automatisk uppgift hello gäller ScaleX Enterprise, och inga manuella steg krävs. Alla användare som kan autentisera med autentiseringsuppgifter för enkel inloggning ska etableras automatiskt på hello ScaleX sida.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja användare åtkomst tooScaleX Enterprise.

![Tilldela användare][200] 

**tooassign Britta Simon tooScaleX Enterprise, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **ScaleX Enterprise**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.

### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Klicka på hello ScaleX Enterprise-panelen i hello åtkomstpanelen, får du automatiskt inloggade tooyour ScaleX företagsprogram. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

