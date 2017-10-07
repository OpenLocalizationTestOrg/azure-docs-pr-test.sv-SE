---
title: "Självstudier: Azure Active Directory-integrering med Proofpoint på begäran | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Proofpoint på begäran."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 0f9472ddc01f2c18ffc9e8d2b59a17b3b595515e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>Självstudier: Azure Active Directory-integrering med Proofpoint på begäran

I kursen får du lära dig hur toointegrate Proofpoint på begäran med Azure Active Directory (AD Azure).

Integrera Proofpoint på begäran med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooProofpoint på begäran
- Du kan aktivera din användare tooautomatically get inloggade tooProofpoint på begäran (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Proofpoint på begäran måste hello följande objekt:

- En Azure AD-prenumeration
- En Proofpoint på begäran enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Proofpoint på begäran från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-proofpoint-on-demand-from-hello-gallery"></a>Att lägga till Proofpoint på begäran från hello-galleriet
tooconfigure hello integrering av Proofpoint på begäran i Azure AD, behöver du tooadd Proofpoint på begäran från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd Proofpoint på begäran från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Proofpoint på begäran**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_search.png)

5. Markera hello resultat på panelen **Proofpoint på begäran**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Proofpoint på begäran baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Proofpoint på begäran är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Proofpoint på begäran toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Proofpoint på begäran.

tooconfigure och testa Azure AD enkel inloggning med Proofpoint på begäran måste toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en Proofpoint på begäran testanvändare](#creating-a-proofpoint-on-demand-test-user)**  -toohave en motsvarighet för Britta Simon i Proofpoint på begäran som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Proofpoint på begäran-programmet.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Proofpoint på begäran:**

1. I hello Azure-portalen på hello **Proofpoint på begäran** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
  
    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. På hello **Proofpoint på begäran domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    a.In hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<hostname>.pphosted.com/ppssamlsp_hostname`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<hostname>.pphosted.com/ppssamlsp`

    c.  I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`
     
    > [!NOTE] 
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Kontakta [Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
6. På hello **Proofpoint på begäran-konfiguration** klickar du på **konfigurera Proofpoint på begäran** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

7. tooconfigure enkel inloggning på **Proofpoint på begäran** sida, behöver du toosend hello hämtas **Certificate(Base64)**,**SAML enhets-ID**, och **SAML Enkel inloggning Tjänstwebbadress** för[Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_01.png) 

2. Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **Britta Simon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-proofpoint-on-demand-test-user"></a>Skapa en Proofpoint på begäran testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i Proofpoint på begäran. Arbeta med [Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services) tooadd användare i hello Proofpoint på begäran-plattformen.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooProofpoint på begäran.

![Tilldela användare][200] 

**tooassign Britta Simon tooProofpoint på begäran, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Proofpoint på begäran**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello **Proofpoint på begäran** panelen på hello åtkomstpanelen, bör vara loggas du automatiskt på tooyour Proofpoint på begäran-programmet.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).  

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png

