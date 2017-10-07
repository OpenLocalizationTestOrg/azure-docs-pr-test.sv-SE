---
title: "Självstudier: Azure Active Directory-integrering med SAP-molnet för kund | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP moln för kunden."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 0525ea81122458ab3ac24a5bdb0b5f628405dd05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Självstudier: Azure Active Directory-integrering med SAP-molnet för kunden

I kursen får du lära dig hur toointegrate SAP-molnet för kunden med Azure Active Directory (AD Azure).

Integrera SAP-molnet för kunden med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSAP molnet för kunden
- Du kan aktivera din användare tooautomatically get inloggade tooSAP molnet för kund (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SAP-molnet för kunden, behöver du hello följande objekt:

- En Azure AD-prenumeration
- Ett SAP-moln för kunden enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägger till SAP-molnet för kunden från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sap-cloud-for-customer-from-hello-gallery"></a>Lägger till SAP-molnet för kunden från hello-galleriet
tooconfigure hello integrering av SAP-molnet för kund till Azure AD, behöver du tooadd SAP-molnet för kunden hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SAP-molnet för kunden från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **SAP-molnet för kunden**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. Markera hello resultat på panelen **SAP-molnet för kunden**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP-molnet för kund baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SAP-molnet för kunden är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i SAP-molnet för kunden toobe upprätta.

I SAP moln för kunden, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med SAP-molnet för kunden, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa ett SAP-moln för kunden testanvändare](#creating-a-sap-cloud-for-customer-test-user)**  -toohave en motsvarighet för Britta Simon i SAP-molnet för kund som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din SAP-molnet för kund-program.

**tooconfigure Azure AD enkel inloggning med SAP-molnet för kunden, utför följande steg hello:**

1. I hello Azure-portalen på hello **SAP-molnet för kunden** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. På hello **SAP molnet kunden domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server name>.crm.ondemand.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [SAP-molnet för kunden klienten supportteamet](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget dessa värden. 

4. På hello **användarattribut** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. I **användar-ID** listan, Välj hello **ExtractMailPrefix()** funktion.

    b. Från hello **e** listan, Välj hello användarattribut som du vill använda toouse för din implementering.
    Till exempel om du vill toouse hello EmployeeID som unikt identifierare och du har lagrat hello attributvärdet i hello ExtensionAttribute2, väljer du user.extensionattribute2.  

5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. På hello **SAP-molnet för konfiguration av Customer** klickar du på **SAP konfigurera molnet för kunden** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. tooget SSO konfigurerats, utför följande steg hello:
   
    a. Logga in på molnet SAP för kundens portal med administratörsrättigheter.
   
    b. Navigera toohello **program och vanliga användare hanteringsaktivitet** och klicka på hello **identitetsleverantör** fliken.
   
    c. Klicka på **nya identitetsleverantör** och välj hello metadata XML-fil som du har hämtat från hello Azure-portalen. Genom att importera hello metadata filöverföringar hello system automatiskt hello krävs Signaturcertifikat och certifikat för kryptering.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure Active Directory kräver hello element Assertion konsumenten tjänst-URL i hello SAML-begäran, så Välj hello **inkludera Assertion konsumenten tjänsten URL** kryssrutan.
   
    e. Klicka på **aktivera enkel inloggning**.
   
    f. Spara ändringarna.
   
    g. Klicka på hello **Mina System** fliken.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. I **Azure AD URL: en inloggning** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    Jag. Ange om hello medarbetare manuellt kan välja mellan att logga in med användar-ID och lösenord eller enkel inloggning genom att välja hello **manuell identitet providern markeringen**.
   
    j. I hello **SSO URL** avsnittet Ange hello-URL som ska användas av dina anställda toosign på toohello system. 
    I hello **URL skickas tooEmployee** listan som du kan välja mellan följande alternativ för hello:
   
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

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Skapa ett SAP-moln för kunden testanvändare

I det här avsnittet kan du skapa en användare som kallas Britta Simon i SAP-molnet för kunden. Se tillsammans med [SAP-molnet för kundtjänst](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello användare i hello SAP-molnet för kund-plattformen. 

> [!NOTE]
> Kontrollera att NameID värdet ska vara identiskt med hello användarnamn fält i hello SAP-molnet för kund-plattformen.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAP molnet för kunden.

![Tilldela användare][200] 

**tooassign Britta Simon tooSAP molnet för kunden, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SAP-molnet för kunden**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello SAP-molnet för kund-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour SAP-molnet för kund-program.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

