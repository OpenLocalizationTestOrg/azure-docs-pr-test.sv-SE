---
title: "Självstudier: Azure Active Directory-integrering med SAP Business ByDesign | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP Business ByDesign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: c14714fd27f8d7fc555f25c7be83fad2b0d7f333
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Självstudier: Azure Active Directory-integration med SAP Business ByDesign

I kursen får du lära dig hur toointegrate SAP Business ByDesign med Azure Active Directory (AD Azure).

Integrera SAP Business ByDesign med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSAP Business ByDesign.
- Du kan låta dina användare tooautomatically get inloggade tooSAP Business ByDesign (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SAP Business ByDesign måste hello följande objekt:

- En Azure AD-prenumeration
- En SAP Business ByDesign enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SAP Business ByDesign från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sap-business-bydesign-from-hello-gallery"></a>Att lägga till SAP Business ByDesign från hello-galleriet
tooconfigure hello integrering av SAP Business ByDesign i Azure AD, behöver du tooadd SAP Business ByDesign hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SAP Business ByDesign från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **SAP Business ByDesign**väljer **SAP Business ByDesign** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![SAP Business ByDesign i hello resultatlistan](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP Business ByDesign baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SAP Business ByDesign är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SAP Business ByDesign toobe upprättas.

Tilldela hello värdet för hello i SAP Business ByDesign **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med SAP Business ByDesign, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en SAP Business ByDesign testanvändare](#create-an-sap-business-bydesign-test-user)**  -toohave en motsvarighet för Britta Simon i SAP Business ByDesign som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för SAP Business ByDesign.

**tooconfigure Azure AD enkel inloggning med SAP Business ByDesign utför hello följande steg:**

1. I hello Azure-portalen på hello **SAP Business ByDesign** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. På hello **SAP Business ByDesign domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och SAP Business ByDesign domän med enkel inloggning information](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<servername>.sapbydesign.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<servername>.sapbydesign.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [SAP Business ByDesign Client supportteamet](https://www.sap.com/products/cloud-analytics.support.html) tooget dessa värden.

4. På hello **användarattribut** avsnittet, utföra hello följande steg:

    ![SAP Business ByDesign attributet avsnitt](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. I **användar-ID** listan, Välj hello **ExtractMailPrefix()** funktion.
    
    b. Från hello **e** listan, Välj hello användarattribut som du vill använda toouse för din implementering. Till exempel om du vill toouse hello EmployeeID som unikt identifierare och du har lagrat hello attributvärdet i hello ExtensionAttribute2, väljer du user.extensionattribute2.   

5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. På hello **SAP Business ByDesign Configuration** klickar du på **konfigurera SAP Business ByDesign** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![SAP Business ByDesign konfiguration](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. tooget SSO konfigurerats för ditt program utför hello följande steg:
   
    a. Inloggning tooyour SAP Business ByDesign portal med administratörsrättigheter.
   
    b. Navigera för**program och vanliga användare hanteringsaktivitet** och klicka på hello **identitetsleverantör** fliken.
   
    c. Klicka på **nya identitetsleverantör** och välj hello metadata XML-fil som du har hämtat från hello Azure-portalen. Genom att importera hello metadata filöverföringar hello system automatiskt hello krävs Signaturcertifikat och certifikat för kryptering.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. tooinclude hello **Assertion konsument-tjänstens URL** till hello SAML-begäran, Välj **inkludera Assertion konsumenten tjänsten URL**.
   
    e. Klicka på **aktivera enkel inloggning**.
   
    f. Spara ändringarna.
   
    g. Klicka på hello **Mina System** fliken.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen den i hello **Azure AD URL: en inloggning** textruta.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    Jag. Ange om hello medarbetare manuellt kan välja mellan att logga in med användar-ID och lösenord eller enkel inloggning genom att välja **manuell identitet providern markeringen**.
   
    j. I hello **SSO URL** avsnittet Ange hello-URL som ska användas av hello medarbetare toologon toohello system. 
    Du kan välja mellan följande alternativ för hello i hello URL skickas tooEmployee listrutan:
   
    **URL: en icke-SSO**
   
    hello skickas endast hello normala URL toohello medarbetare. hello medarbetare kan inte logga in med enkel inloggning, och måste använda lösenordet eller certifikatet i stället.
   
    **URL FÖR ENKEL INLOGGNING** 
   
    hello skickas endast hello SSO URL toohello medarbetare. hello medarbetare kan logga in med enkel inloggning. Autentiseringsbegäran dirigeras via hello IdP.
   
    **Automatiskt val**
   
    Om enkel inloggning inte är aktiv, skickar hello system hello normala URL toohello medarbetare. Om enkel inloggning är aktiv kontrolleras Hej om hello medarbetare har ett lösenord. Om ett lösenord är tillgängligt skickas både SSO Webbadressen och icke-SSO toohello medarbetare. Men om hello anställda har inget lösenord, skickas endast hello SSO URL toohello medarbetare.
   
    k. Spara ändringarna.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>Skapa en SAP Business ByDesign testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i SAP Business ByDesign. Se tillsammans med [SAP Business ByDesign Client supportteamet](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello användare i hello SAP Business ByDesign plattform. 

> [!NOTE]
> Kontrollera att NameID värdet ska vara identiskt med hello användarnamn fält i hello SAP Business ByDesign plattform.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAP Business ByDesign.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooSAP Business ByDesign utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SAP Business ByDesign**.

    ![hello SAP Business ByDesign länken i listan med program hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour SAP Business ByDesign programmet när du klickar på hello SAP Business ByDesign panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

