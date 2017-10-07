---
title: "Självstudier: Azure Active Directory-integrering med Clarizen | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Clarizen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: f24ccda3b90e5df9a203a444dfda905043b30276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Självstudier: Azure Active Directory-integrering med Clarizen

I kursen får du lära dig hur toointegrate Azure Active Directory (AD Azure) med Clarizen. Den här integreringen ger du hello följande fördelar:

- Du kan styra, i Azure AD, som har åtkomst till tooClarizen.
- Du kan låta dina användare toobe loggas automatiskt in tooClarizen (enkel inloggning) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats, hello Azure-portalen.

hello-scenario i den här kursen består av två huvudsakliga aktiviteter:

1. Lägg till Clarizen från hello-galleriet.
2. Konfigurera och testa Azure AD enkel inloggning.

Om du vill ha mer information om programvara som en tjänst (SaaS) appintegrering med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
tooconfigure Azure AD-integrering med Clarizen, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Clarizen-prenumeration som är aktiverad för enkel inloggning

tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:

- Testa Azure AD enkel inloggning i en testmiljö. Använd inte i produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en Azure AD-testmiljö, kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-hello-gallery"></a>Lägg till Clarizen från hello-galleriet
tooconfigure hello integrering av Clarizen i Azure AD och lägga till Clarizen från hello galleriet tooyour lista över hanterade SaaS-appar.

1. I hello [Azure-portalen](https://portal.azure.com), i hello vänster, klicka på hello **Azure Active Directory** ikon.

    ![Azure Active Directory-ikonen][1]

2. Klicka på **företagsprogram**. Klicka på **alla program**.

    ![Klicka på ”företagsprogram” och ”alla program”][2]

3. Klicka på hello **Lägg till** knappen hello överst i dialogrutan för hello.

    ![Hej ”Lägg till”-knappen][3]

4. Skriv i sökrutan hello **Clarizen**.

    ![Att skriva ”Clarizen” i sökrutan för hello](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. I resultatfönstret hello väljer **Clarizen**, och klicka sedan på **Lägg till** tooadd hello program.

    ![Att välja Clarizen i resultatfönstret hello](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I följande avsnitt hello, konfigurera och testa Azure AD enkel inloggning med Clarizen baserat på hello testanvändare Britta Simon.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Clarizen är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Clarizen toobe upprättas. Du etablerar den här länken relationen genom att tilldela värdet hello **användarnamn** i Azure AD som hello värde för **användarnamn** i Clarizen.

tooconfigure och testa Azure AD enkel inloggning med Clarizen fullständig hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Clarizen](#create-a-clarizen-test-user)**  toohave en motsvarighet för Britta Simon i Clarizen som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning
Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Clarizen program.

1. I hello Azure-portalen på hello **Clarizen** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Klicka på ”enkel inloggning”][4]

2. I hello **enkel inloggning** i dialogrutan för **läge**väljer **SAML-baserade inloggning** tooenable enkel inloggning.

    ![Att välja ”SAML-baserade inloggning”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. I hello **Clarizen domän och URL: er** avsnittet, utföra hello följande steg:

    ![Kryssrutor för identifierare och svars-URL](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    a. I hello **identifierare** rutan hello TYPVÄRDE som: **Clarizen**

    b. I hello **Reply URL** skriver en URL med hjälp av hello följer mönstret: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Detta är inte hello verkliga värden. Du har toouse hello faktiska identifierare och reply URL. Här föreslår vi att du använder hello unikt värde i en sträng som hello identifierare. tooget hello faktiska värden, kontakta hello [Clarizen supportteamet](https://success.clarizen.com/hc/en-us/requests/new).

4. På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.

    ![Klicka på ”Skapa nytt certifikat”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. I hello **skapa nya certifikat** dialogrutan rutan, klicka hello kalendern och välj ett förfallodatum. Klicka sedan på **Spara**.

    ![Att välja och spara upphör att gälla](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. I hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet**, och klicka sedan på **spara**.

    ![Om du markerar kryssrutan för hello för att göra hello nytt certifikat active](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. I hello **förnyelsecertifikat** dialogrutan klickar du på **OK**.

    ![Klicka på ”OK” tooconfirm som du vill toomake hello certifikat active](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. I hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Klicka på ”(Base64)” toostart hello hämtning](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. I hello **Clarizen Configuration** klickar du på **konfigurera Clarizen** tooopen hello **konfigurera inloggning** fönster.

    ![Klicka på ”Konfigurera Clarizen”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    ![”Konfigurera inloggning” fönster, inklusive filer och URL: er](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. Logga in tooyour Clarizen företagets webbplats som en administratör i en annan webbläsarfönster.

11. Klicka på ditt användarnamn och klicka sedan på **inställningar**.

    ![Klicka på ”inställningar” under ditt användarnamn](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "inställningar")

12. Klicka på hello **globala inställningar** fliken. Sedan Nästa för**federerad autentisering**, klickar du på **redigera**.

    ![”Globala inställningar”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globala inställningar")

13. I hello **federerad autentisering** dialogrutan utför hello följande steg:

    ![”Extern autentisering” dialogrutan](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "federerad autentisering")

    a. Välj **aktivera federerad autentisering**.

    b. Klicka på **överför** tooupload hämtade certifikatet.

    c. I hello **inloggning URL** ange hello värdet för **SAML inloggning tjänst-URL för enkel** från hello Azure AD-konfigurationsfönstret.

    d. I hello **Sign-out URL** ange hello värdet för **Sign-Out URL** från hello Azure AD-konfigurationsfönstret.

    e. Välj **använder POST**.

    f. Klicka på **Spara**.

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Skapa en testanvändare kallas Britta Simon i hello Azure-portalen.

![Namn och e-postadress för användare hello Azure AD][100]

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** ikon.

    ![Azure Active Directory-ikonen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. Klicka på **användare och grupper**, och klicka sedan på **alla användare** toodisplay hello lista över användare.

    ![Klicka på ”användare och grupper” och ”alla användare”](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.

    ![Hej ”Lägg till”-knappen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![Dialogrutan ”användare” med namn, e-postadress och lösenord som har fyllt i](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello e-postadress hello Britta Simon konto.

    c. Välj **visa lösenordet** och anteckna värdet hello **lösenord**.

    d. Klicka på **Skapa**.

### <a name="create-a-clarizen-test-user"></a>Skapa en testanvändare Clarizen
tooenable Azure AD-användare toosign i tooClarizen, måste du etablera användarkonton. Hello gäller Clarizen är etablering en manuell aktivitet.

1. Logga in tooyour Clarizen företagets webbplats som administratör.

2. Klicka på **personer**.

    ![Klicka på ”personer”](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "personer")

3. Klicka på **bjuda in användare**.

    ![”Bjuda in användare” knappen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "bjuda in användare")

4. I hello **bjuda in personer** dialogrutan utför hello följande steg:

    ![”” Dialogrutan](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "bjuda in användare")

    a. I hello **e-post** rutan typen hello e-postadress hello Britta Simon konto.

    b. Klicka på **bjuda in**.

    > [!NOTE]
    > hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD
Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooClarizen sin åtkomst.

![Tilldelad användare][200]

1. I hello Azure-portalen, öppna hello program, toohello directory bläddringsläge, klicka på **företagsprogram**, och klicka sedan på **alla program**.

    ![Klicka på ”företagsprogram” och ”alla program”][201]

2. Välj i listan med program hello **Clarizen**.

    ![Välj Clarizen hello listan](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. Hello vänster klickar du på **användare och grupper**.

    ![Klicka på ”användare och grupper”][202]

4. Klicka på hello **Lägg till** knappen. Sedan hello **Lägg uppdrag** dialogrutan **användare och grupper**.

    ![Hej ”Lägg till”-knappen och dialogrutan för hello ”Lägg till tilldelning”][203]

5. I hello **användare och grupper** dialogrutan **Britta Simon** i hello lista över användare.

6. I hello **användare och grupper** dialogrutan klickar du på hello **Välj** knappen.

7. I hello **Lägg uppdrag** dialogrutan klickar du på hello **tilldela** knappen.

### <a name="test-single-sign-on"></a>Testa enkel inloggning
Testa Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.

När du klickar på hello Clarizen panelen i hello åtkomstpanelen bör vara loggas du automatiskt tooyour Clarizen program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
