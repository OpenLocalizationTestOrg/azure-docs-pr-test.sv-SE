---
title: "Självstudier: Azure Active Directory-integrering med Wingspan eTMF | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Wingspan eTMF."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ace320d3-521c-449c-992f-feabe7538de7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: ed5fda5318a0d3841af8b2db4fb74db550befaea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wingspan-etmf"></a>Självstudier: Azure Active Directory-integrering med Wingspan eTMF

I kursen får du lära dig hur toointegrate Wingspan eTMF med Azure Active Directory (AD Azure).

Integrera Wingspan eTMF med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooWingspan eTMF
- Du kan aktivera din användare tooautomatically get inloggade tooWingspan eTMF (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Wingspan eTMF, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Wingspan eTMF enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Wingspan eTMF från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-wingspan-etmf-from-hello-gallery"></a>Att lägga till Wingspan eTMF från hello-galleriet
tooconfigure hello integrering av Wingspan eTMF i Azure AD, behöver du tooadd Wingspan eTMF hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Wingspan eTMF från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Wingspan eTMF**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_search.png)

5. Markera hello resultat på panelen **Wingspan eTMF**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Wingspan eTMF baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Wingspan eTMF är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Wingspan eTMF toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Wingspan eTMF.

tooconfigure och testa Azure AD enkel inloggning med Wingspan eTMF, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare för Wingspan eTMF](#creating-a-wingspan-etmf-test-user)**  -toohave en motsvarighet för Britta Simon i Wingspan eTMF som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Wingspan eTMF program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Wingspan eTMF:**

1. I hello Azure-portalen på hello **Wingspan eTMF** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_samlbase.png)

3. På hello **Wingspan eTMF domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_url11.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<customer name>.<instance name>.mywingspan.com/saml`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`http://saml.<instance name>.wingspan.com/shibboleth`

    c. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<customer name>.<instance name>.mywingspan.com/`
     
    > [!NOTE] 
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL, identifierare och Reply URL inklusive hello faktiska kund och instansnamn. Kontakta [Wingspan eTMF klienten supportteamet](http://www.wingspan.com/contact-us/) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_400.png)

6. tooconfigure enkel inloggning på **Wingspan eTMF** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Wingspan eTMF support](http://www.wingspan.com/contact-us/). De har konfigurerat detta toohave hello SAML SSO anslutningen korrekt på båda sidor.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-wingspan-etmf-test-user"></a>Skapa en Wingspan eTMF testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i Wingspan eTMF. Arbeta med [Wingspan eTMF stöd](http://www.wingspan.com/contact-us/) tooadd hello användare i hello Wingspan eTMF program. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWingspan eTMF.

![Tilldela användare][200] 

**tooassign Britta Simon tooWingspan eTMF utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Wingspan eTMF**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen. 

Klicka på hello Wingspan eTMF panelen i hello åtkomstpanelen, kommer du att omdirigeras tooOrganization logga in på sidan. Du kommer att inloggade tooyour Wingspan eTMF program efter genomförd inloggning. Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_203.png

