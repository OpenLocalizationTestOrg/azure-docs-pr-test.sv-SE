---
title: "Självstudier: Azure Active Directory-integrering med svart tavla Läs | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och svart tavla lära dig."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b8ca505-61ea-487c-9a3e-fa50c936df0c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jeedes
ms.openlocfilehash: e94cdd6eaf876d4f66bdd783c442dc468f104e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a>Självstudier: Azure Active Directory-integrering med svart tavla Läs

I kursen får du lära dig hur toointegrate svart tavla Läs med Azure Active Directory (AD Azure).

Integrera svart tavla Läs med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooBlackboard Läs
- Du kan aktivera din användare tooautomatically get inloggade tooBlackboard Läs (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med svart tavla Läs måste hello följande objekt:

- En Azure AD-prenumeration
- En svart tavla Läs enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägger till svart tavla Läs från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-blackboard-learn-from-hello-gallery"></a>Lägger till svart tavla Läs från hello-galleriet
tooconfigure hello integrering av svart tavla Lär dig i Azure AD, behöver du tooadd svart tavla Läs hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd svart tavla Läs från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **svart tavla Läs**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_search.png)

5. Markera hello resultat på panelen **svart tavla Läs**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med svart tavla Läs baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i svart tavla Läs är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i svart tavla Läs toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i svart tavla Läs.

tooconfigure och testa Azure AD enkel inloggning med svart tavla Läs, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en svart tavla Läs testanvändare](#creating-a-blackboard-learn-test-user)**  -toohave en motsvarighet för Britta Simon i svart tavla Läs som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt svart tavla Läs program.

**Utför följande hello tooconfigure Azure AD enkel inloggning med svart tavla Läs:**

1. I hello Azure-portalen på hello **svart tavla Läs** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_samlbase.png)

3. På hello **svart tavla Läs domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.blackboard.com/`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`
    
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [svart tavla Läs klienten supportteamet](https://www.blackboard.com/support/index.aspx) tooget dessa värden. 

4. Svart tavla Läs program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet.
 hello följande skärmbild visar ett exempel på hur den.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute.png)

5. I hello **användarattribut** avsnittet på **enkel inloggning** dialogrutan Konfigurera SAML-token attribut enligt hello bild och utföra hello följande steg. Vi har mappats hello Userprincipalname som hello unika användarattribut här men du kan mappa den toohello lämpligt värde, som unikt särskiljer hello användare i hello organisation och som mappar tooBlackboard Läs username-fält.
           
    | Attributets namn | Attributvärdet |   
    | ---------------| ----------------|
    | urn:oid:1.3.6.1.4.1.5923.1.1.1.6 |User.userPrincipalName |

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_05.png)

    b. I hello **namn** textruta hello attributnamn visas för den raden.

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **OK**.

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_certificate.png)

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_400.png)

8. På hello **svart tavla Läs Configuration** klickar du på **konfigurera svart tavla Läs** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_configure.png) 

9. tooconfigure enkel inloggning på **svart tavla Läs** sida, behöver du toosend hello hämtas **XML-Metadata för** och **SAML enhets-ID** för[svart tavla Läs stöd för](https://www.blackboard.com/support/index.aspx).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-blackboard-learn-test-user"></a>Skapa en svart tavla Läs testanvändare
I det här avsnittet kan du skapa en användare som kallas Britta Simon i svart tavla Läs. 

Svart tavla Läs programmet stöder bara i tid användaretablering. Kontrollera att du har konfigurerat hello anspråk enligt beskrivningen i avsnittet hello  **[konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**
### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBlackboard Läs.

![Tilldela användare][200] 

**tooassign Britta Simon tooBlackboard Läs, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **svart tavla Läs**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour svart tavla Läs programmet när du klickar på hello svart tavla Läs panelen i hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png

