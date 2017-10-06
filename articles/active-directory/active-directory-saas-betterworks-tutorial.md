---
title: "Självstudier: Azure Active Directory-integrering med BetterWorks | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BetterWorks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 9803593124318ea82e5a8888cc5a95b5da84472e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a>Självstudier: Azure Active Directory-integrering med BetterWorks

I kursen får du lära dig hur toointegrate BetterWorks med Azure Active Directory (AD Azure).

Integrera BetterWorks med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooBetterWorks
- Du kan aktivera din användare tooautomatically get inloggade tooBetterWorks (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med BetterWorks, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En BetterWorks enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till BetterWorks från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-betterworks-from-hello-gallery"></a>Att lägga till BetterWorks från hello-galleriet
tooconfigure hello integrering av BetterWorks i Azure AD, behöver du tooadd BetterWorks hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd BetterWorks från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **BetterWorks**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_search.png)

5. Markera hello resultat på panelen **BetterWorks**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BetterWorks baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i BetterWorks är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BetterWorks toobe upprättas.

I BetterWorks, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med BetterWorks, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare BetterWorks](#creating-a-betterworks-test-user)**  -toohave en motsvarighet för Britta Simon i BetterWorks som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt BetterWorks program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med BetterWorks:**

1. I hello Azure-portalen på hello **BetterWorks** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_samlbase.png)

3. På hello **BetterWorks domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.betterworks.com/saml2/metadata/`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.betterworks.com/saml2/acs/`

4. På hello **BetterWorks domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url1.png)

    a. Klicka på hello **visa avancerade inställningar för URL: en**.

    b. I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.betterworks.com`

    > [!NOTE] 
    > Detta är inte verkliga värden. Uppdatera dessa värden med hello Reply URL identifierare och den faktiska logga URL. Kontakta [BetterWorks supportteam](mailto:support@betterworks.com) tooget dessa värden.
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_certificate.png)  

5. BetterWorks program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello ”**attributet**” fliken av programmet hello. hello följande skärmbild visar ett exempel för det här. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_attribute.png)

6. På hello **SAML-token attribut** dialogrutan för varje rad som visas i hello nedan, utföra hello följande steg:
 
   | Attributets namn | Attributvärdet |
   | -------------- |  ------------ |
   | saml_token     | bd189cf6-1701-11e6-8f90-d26992eca2a5 |

   a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_05.png)

   b. I hello **namn** textruta hello attributnamn visas för den raden. 

   c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
   d. Klicka på **OK**.

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_general_400.png)

8. tooconfigure enkel inloggning på **BetterWorks** sida, behöver du toosend hello hämtas **XML-Metadata för** för[BetterWorks supportteam](mailto:support@betterworks.com).


> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-betterworks-test-user"></a>Skapa en testanvändare BetterWorks

I det här avsnittet skapar du en användare som kallas Britta Simon i BetterWorks. Arbeta med [BetterWorks supportteam](mailto:support@betterworks.com) tooadd hello användare i hello BetterWorks plattform.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBetterWorks.

![Tilldela användare][200] 

**tooassign Britta Simon tooBetterWorks utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **BetterWorks**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour BetterWorks programmet när du klickar på hello BetterWorks panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png

