---
title: "Självstudier: Azure Active Directory-integrering med samarbetsfunktioner Innovation | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och samarbetsfunktioner Innovation."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e85fabfe11a380129f319a101aa7c7a9491260f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a>Självstudier: Azure Active Directory-integrering med samarbetsfunktioner Innovation

I kursen får du lära dig hur toointegrate samarbetsfunktioner Innovation med Azure Active Directory (AD Azure).

Integrera samarbetsfunktioner Innovation med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooCollaborative Innovation
- Du kan aktivera din användare tooautomatically get inloggade tooCollaborative Innovation (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med samarbetsfunktioner Innovation, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En gemensam Innovation enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till samarbetsfunktioner Innovation från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-collaborative-innovation-from-hello-gallery"></a>Att lägga till samarbetsfunktioner Innovation från hello-galleriet
tooconfigure hello integrering av samarbetsfunktioner Innovation i Azure AD, behöver du tooadd samarbetsfunktioner Innovation hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd samarbetsfunktioner Innovation från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **samarbetsfunktioner Innovation**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

5. Markera hello resultat på panelen **samarbetsfunktioner Innovation**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med samarbetsfunktioner Innovation baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren samarbetsfunktioner innovation är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i samarbetsfunktioner Innovation toobe upprättas.

Tilldela hello värdet hello samarbetsfunktioner innovation **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med samarbetsfunktioner Innovation, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare samarbetsfunktioner Innovation](#creating-a-collaborative-innovation-test-user)**  -toohave en motsvarighet för Britta Simon samarbetsfunktioner innovation som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet samarbetsfunktioner Innovation.

**Utför följande hello tooconfigure Azure AD enkel inloggning med samarbetsfunktioner Innovation:**

1. I hello Azure-portalen på hello **samarbetsfunktioner Innovation** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

3. På hello **samarbetsfunktioner Innovation domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.foundry.<companyname>.com/`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.foundry.<companyname>.com`
    
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [samarbetsfunktioner Innovation klienten supportteamet](https://www.unilever.com/contact/) tooget dessa värden.  

4. Samarbetsfunktioner Innovation program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/attribute.png)
    
5. Klicka på **visa och redigera andra användarattribut** kryssrutan i hello **användarattribut** avsnittet tooexpand hello attribut. Utför följande steg på varje hello visas attribut - hello

    | Attributets namn | Attributvärdet |
    | ---------------| --------------- |    
    | givenName | User.givenName |
    | Efternamn | User.surname |
    | e-postadress | User.userPrincipalName |
    | namn | User.userPrincipalName |

    a. Klicka på hello attributet tooopen hello **Redigera attribut** fönster.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/url_update.png)

    b. Ta bort hello URL-värdet från hello **Namespace**.
    
    c. Klicka på **Ok** toosave hello inställningen.

6. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_400.png)

8. tooconfigure enkel inloggning på **samarbetsfunktioner Innovation** sida, behöver du toosend hello hämtas **XML-Metadata för** för[samarbetsfunktioner Innovation supportteamet](https://www.unilever.com/contact/). De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-collaborative-innovation-test-user"></a>Skapa en testanvändare samarbetsfunktioner Innovation

tooenable Azure AD-användare toolog i tooCollaborative Innovation, måste de etableras i samarbetsfunktioner Innovation.  

Vid det här programmet är etablering automatisk eftersom programmet hello stöder bara i tid användaretablering. Så finns det inget behov av tooperform här några steg.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCollaborative Innovation.

![Tilldela användare][200] 

**tooassign Britta Simon tooCollaborative Innovation, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **samarbetsfunktioner Innovation**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få inloggningssidan för samarbetsfunktioner Innovation program när du klickar på hello samarbetsfunktioner Innovation panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_203.png

