---
title: "Självstudier: Azure Active Directory-integrering med TOPdesk - offentliga | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TOPdesk - allmänheten."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ef0dd06157ecc3b33814590039f5cbae64e8c916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Självstudier: Azure Active Directory-integrering med TOPdesk - offentliga

I kursen får du lära dig hur toointegrate TOPdesk - offentliga med Azure Active Directory (AD Azure).

Integrera TOPdesk - offentliga med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooTOPdesk - allmänheten.
- Du kan låta dina användare tooautomatically get inloggade tooTOPdesk - offentlig (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med TOPdesk - offentliga måste hello följande objekt:

- En Azure AD-prenumeration
- En TOPdesk - offentliga enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägger till TOPdesk - offentliga från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-topdesk---public-from-hello-gallery"></a>Lägger till TOPdesk - offentliga från hello-galleriet
tooconfigure hello integrering av TOPdesk - offentliga till Azure AD måste tooadd TOPdesk - offentliga hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd TOPdesk - offentliga från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **TOPdesk - offentliga**väljer **TOPdesk - offentliga** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![TOPdesk - allmänheten i hello resultatlistan](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TOPdesk - offentliga baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste tooknow vilka hello motsvarighet användare i TOPdesk för enkel inloggning toowork - offentliga är tooa användare i Azure AD. Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i TOPdesk - offentliga måste toobe upprättas.

I TOPdesk - offentliga, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med TOPdesk - offentlig, måste du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en TOPdesk - offentliga testanvändare](#create-a-topdesk---public-test-user)**  - toohave en motsvarighet för Britta Simon i TOPdesk - offentliga som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din TOPdesk - offentliga program.

**tooconfigure Azure AD enkel inloggning med TOPdesk - offentliga utföra hello följande steg:**

1. I hello Azure-portalen på hello **TOPdesk - offentliga** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. På hello **TOPdesk - URL: er och den offentliga domänen** avsnittet, utföra hello följande steg:

    ![TOPdesk - URL: er och den offentliga domänen enkel inloggning information](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.topdesk.net`
    
    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.topdesk.net/tas/public/login/verify`

    c. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Svars-URL är explaned senare i självstudiekursen. Kontakta [TOPdesk - offentliga klienten supportteamet](https://help.topdesk.com/saas/enterprise/user/) tooget dessa värden.  

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. På hello **TOPdesk - offentliga konfiguration** klickar du på **konfigurera TOPdesk - offentliga** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![TOPdesk - offentliga konfiguration](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. Logga in tooyour **TOPdesk - offentliga** företagets webbplats som administratör.

8. I hello **TOPdesk** -menyn klickar du på **inställningar**.
   
    ![Inställningar för](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "inställningar")

9. Klicka på **inloggningsinställningar**.
   
    ![Inloggningsinställningar](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "inloggningsinställningar")

10. Expandera hello **inloggningsinställningar** -menyn och klicka sedan på **allmänna**.
   
    ![Allmän](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "Allmänt")

11. I hello **offentliga** avsnitt i hello **SAML inloggningen** configuration avsnittet, utföra hello följande steg:
   
    ![Tekniska inställningar](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "tekniska inställningar")
   
    a. Klicka på **hämta** toodownload hello offentliga metadatafil och sedan spara det lokalt på datorn.
   
    b. Öppna hello hämtas metadatafilen och leta upp hello **AssertionConsumerService** nod.

    ![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopiera hello **AssertionConsumerService** värde, klistra in det här värdet i hello **Reply URL** TextBox-kontroll i **TOPdesk - URL: er och den offentliga domänen** avsnitt.      
   
12. toocreate en certifikatfil utför hello följande steg:
    
    ![Certifikatet](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "certifikat")
    
    a. Öppna hello ned metadatafil från Azure-portalen.
    
    b. Expandera hello **RoleDescriptor** nod som har en **xsi: type** av **aggregeringsdesignprocessen: ApplicationServiceType**.
    
    c. Kopiera hello värdet för hello **X509Certificate** nod.
    
    d. Spara hello kopieras **X509Certificate** värdet lokalt på din dator i en fil.

13. I hello **offentliga** klickar du på **Lägg till**.
    
    ![SAML-inloggningen](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML-inloggning")

14. På hello **SAML configuration assistenten** dialogrutan utför hello följande steg:
    
    ![SAML Configuration assistenten](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "assistenten för SAML-konfiguration")
    
    a. tooupload hämtade metadata filen från Azure-portalen under **Federationsmetadata**, klickar du på **Bläddra**.

    b. ditt certifikat filen under tooupload **certifikat (RSA)**, klickar du på **Bläddra**.

    c. tooupload hello logotypfilen som du har fått från supportteamet för hello TOPdesk under **logotypen ikonen**, klickar du på **Bläddra**.

    d. I hello **användarnamn** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. I hello **visningsnamn** textruta, ange ett namn för din konfiguration.

    f. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-topdesk---public-test-user"></a>Skapa en TOPdesk - offentliga testanvändare

I ordning tooenable Azure AD-användare toolog i TOPdesk - offentliga de måste etableras i TOPdesk - allmänheten.  
Hello gäller TOPdesk - offentliga etablering är en manuell aktivitet.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure användaretablering, utför följande steg hello:
1. Logga in tooyour **TOPdesk - offentliga** företagets webbplats som administratör.

2. Hello-menyn överst hello **TOPdesk \> ny \> stödfiler \> Person**.
   
    ![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")

3. Utför följande hello på hello ny Person dialogrutan:
   
    ![Ny Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "ny Person")
   
    a. Klicka på hello på fliken Allmänt.

    b. I hello **efternamn** textruta Ange efternamn hello användaren som Simon
 
    c. Välj en **plats** för hello-kontot.
 
    d. Klicka på **Spara**.

> [!NOTE]
> Du kan använda andra TOPdesk - offentliga användare skapa verktyg eller API: er som tillhandahålls av TOPdesk - offentliga tooprovision Azure AD-användarkonton.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTOPdesk - offentliga.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooTOPdesk - offentliga utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **TOPdesk - offentliga**.

    ![Hej TOPdesk - offentliga länken i listan med program hello](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello TOPdesk - ett offentligt panelen i hello åtkomstpanelen får du automatiskt inloggade tooyour TOPdesk - offentliga program.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

