---
title: "Självstudier: Azure Active Directory-integrering med DocuSign | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: e4ef40b8f5af20d811d8d806d2bd7e2039c55052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Självstudier: Azure Active Directory-integrering med DocuSign

I kursen får du lära dig hur toointegrate DocuSign med Azure Active Directory (AD Azure).

Integrera DocuSign med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooDocuSign
- Du kan aktivera din användare tooautomatically get inloggade tooDocuSign (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med DocuSign, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En DocuSign enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till DocuSign från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-docusign-from-hello-gallery"></a>Att lägga till DocuSign från hello-galleriet
tooconfigure hello integrering av DocuSign i Azure AD, behöver du tooadd DocuSign hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd DocuSign från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **DocuSign**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. Markera hello resultat på panelen **DocuSign**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med DocuSign baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i DocuSign är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i DocuSign toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i DocuSign.

tooconfigure och testa Azure AD enkel inloggning med DocuSign, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare DocuSign](#creating-a-docusign-test-user)**  -toohave en motsvarighet för Britta Simon i DocuSign som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt DocuSign program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med DocuSign:**

1. I hello Azure-portalen på hello **DocuSign** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base 64)** och spara certifikatet på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. På hello **DocuSign Configuration** på Azure portal, klicka på **konfigurera DocuSign** tooopen inloggning konfigurera fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. I en annan webbläsarfönster inloggningen tooyour **DocuSign administrationsportalen** som administratör.

6. I hello navigeringsmenyn hello vänster klickar du på **domäner**.
   
    ![Konfigurera enkel inloggning][51]

7. Hello högra rutan, klicka på **anspråk domän**.
   
    ![Konfigurera enkel inloggning][52]

8. På hello **anspråk en domän** dialogrutan i hello **domännamn** textruta Skriv företagets domän och klicka sedan på **anspråk**. Se till att du verifiera hello domän och hello status är aktiv.
   
    ![Konfigurera enkel inloggning][53]

9. Klicka på menyn hello vänster **identitetsleverantörer**  
   
    ![Konfigurera enkel inloggning][54]
10. I hello högra rutan, klickar du på **lägga till identitetsleverantör**. 
   
    ![Konfigurera enkel inloggning][55]

11. På hello **identitet providerinställningar** utför hello följande steg:
   
    ![Konfigurera enkel inloggning][56]

    a. I hello **namn** textruta, ange ett unikt namn för din konfiguration. Använd inte blanksteg.

    b. Klistra in **SAML enhets-ID** till hello **identitet providern utfärdaren** textruta.

    c. Klistra in **SAML enkel inloggning Tjänstwebbadress** till hello **identitet providern inloggnings-URL** textruta.

    d. Klistra in **Sign-Out URL** till hello **identitet providern logga ut URL** textruta.

    e. Välj **logga AuthN begäran**.

    f. Som **skicka AuthN-begäran från**väljer **efter**.

    g. Som **skicka logga ut begäran från**väljer **hämta**.

12. I hello **anpassad attributmappning** hello fältet väljer du vill toomap med Azure AD-anspråk. I det här exemplet hello **emailaddress** anspråk mappas med hello värdet **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Det är hello anspråk för standardnamnet från Azure AD för e-anspråk. 
   
    > [!NOTE]
    > Använd hello lämpliga **användar-ID** toomap hello användaren från Azure AD tooDocuSign mappning. Välj hello rätt fält och ange hello lämpligt värde baserat på dina organisationsinställningar.
          
    ![Konfigurera enkel inloggning][57]

13. I hello **providern identitetscertifikat** klickar du på **Lägg till certifikat**, och sedan ladda upp hello-certifikat som du har hämtat från Azure AD-portalen.   
   
    ![Konfigurera enkel inloggning][58]

14. Klicka på **Spara**.

15. I hello **identitetsleverantörer** klickar du på **åtgärder**, och klicka sedan på **slutpunkter**.   
   
    ![Konfigurera enkel inloggning][59]
 
16. I hello **visa SAML 2.0 slutpunkter** avsnittet på **DocuSign administrationsportal**, utföra hello följande steg:
   
    ![Konfigurera enkel inloggning][60]
   
    a. Kopiera hello **Service Provider utfärdar-URL**, och sedan klistra in i hello **identifierare** textruta på **DocuSign domän och URL: er** avsnitt i hello Azure portal följande hello mönster: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.
   
    b. Kopiera hello **Service Provider inloggnings-URL**, och sedan klistra in i hello **logga URL** textruta på **DocuSign domän och URL: er** avsnitt i hello Azure portal följande hello mönster: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    c.  Klicka på **Stäng**
    
17. Klicka på hello Azure-portalen, **spara**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-docusign-test-user"></a>Skapa en testanvändare DocuSign

Programmet stöder **precis i tid användaretablering** och efter autentisering användare skapas i hello program automatiskt.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooDocuSign sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooDocuSign utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **DocuSign**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour DocuSign programmet när du klickar på hello DocuSign panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png

