---
title: "Självstudier: Azure Active Directory-integrering med överväxel | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och överväxel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e68cede7-1130-4813-bd55-22a9a6fc6bf5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: b0aafacdedee25587132fc88ff93829f4367b0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-overdrive"></a>Självstudier: Azure Active Directory-integrering med överväxel 

I kursen får du lära dig hur toointegrate överväxel med Azure Active Directory (AD Azure).

Integrera överväxel med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooOverdrive 
- Du kan aktivera din användare tooautomatically get inloggade tooOverdrive (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med överväxel måste hello följande objekt:

- En Azure AD-prenumeration
- En överväxel enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till överväxel från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-overdrive-from-hello-gallery"></a>Att lägga till överväxel från hello-galleriet
tooconfigure hello integrering av överväxel till Azure AD, behöver du tooadd överväxel hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd överväxel från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **överväxel**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_search.png)

5. Markera hello resultat på panelen **överväxel**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med överväxel baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i överväxel är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i överväxel toobe upprättas.

Tilldela hello värdet för hello i överväxel **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med överväxel, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare överväxel](#creating-an-overdrive-test-user)**  -toohave en motsvarighet för Britta Simon i överväxel som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet överväxel.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med överväxel:**

1. I hello Azure-portalen på hello **överväxel** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_samlbase.png)

3. På hello **överväxel domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`http://<subdomain>.libraryreserve.com`

    > [!NOTE] 
    > hello-värdet är inte verkliga. Hello uppdateringsvärde med hello faktiska inloggnings-URL. Kontakta [överväxel klienten supportteamet](https://help.overdrive.com/) tooget hello värde. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_400.png)

6. tooconfigure enkel inloggning på **överväxel** sida, behöver du toosend hello hämtas **XML-Metadata för** för[överväxel supportteamet](https://help.overdrive.com/). De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-overdrive-test-user"></a>Skapa en överväxel testanvändare

Det finns inga uppgiften för du tooconfigure användaretablering tooOverDrive.  

När en tilldelad användare försöker toolog i tooOverDrive skapas automatiskt ett överväxel konto om det behövs.

>[!NOTE]
>Du kan använda något annat överväxel användarens konto skapas verktyg eller API: er som tillhandahålls av överväxel tooprovision AAD användarkonton.
>

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooOverdrive.

![Tilldela användare][200] 

**tooassign Britta Simon tooOverdrive utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **överväxel**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour överväxel programmet när du klickar på hello överväxel panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_203.png

