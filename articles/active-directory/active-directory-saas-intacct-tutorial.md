---
title: "Självstudier: Azure Active Directory-integrering med Intacct | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Intacct."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3500039615166c2f61fb408d85bb82dfaefba134
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Självstudier: Azure Active Directory-integrering med Intacct

I kursen får du lära dig hur toointegrate Intacct med Azure Active Directory (AD Azure).

Integrera Intacct med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooIntacct
- Du kan aktivera din användare tooautomatically get inloggade tooIntacct (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Intacct, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Intacct enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Intacct från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-intacct-from-hello-gallery"></a>Att lägga till Intacct från hello-galleriet
tooconfigure hello integrering av Intacct i Azure AD, behöver du tooadd Intacct hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Intacct från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Intacct**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. Markera hello resultat på panelen **Intacct**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Intacct baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Intacct är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Intacct toobe upprättas.

I Intacct, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Intacct, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Intacct](#creating-an-intacct-test-user)**  -toohave en motsvarighet för Britta Simon i Intacct som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Intacct program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Intacct:**

1. I hello Azure-portalen på hello **Intacct** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. På hello **Intacct domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska Reply-URL. Kontakta [Intacct supportteamet](https://us.intacct.com/support) tooget det här värdet.

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. På hello **Intacct Configuration** klickar du på **konfigurera Intacct** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. Logga in tooyour Intacct företagets webbplats som en administratör i en annan webbläsarfönster.

8. Klicka på hello **företagets** fliken och klicka sedan på **företagets information**.

    ![Företagets](./media/active-directory-saas-intacct-tutorial/ic790037.png "företag")

9. Klicka på hello **säkerhet** fliken och klicka sedan på **redigera**.

    ![Säkerhet](./media/active-directory-saas-intacct-tutorial/ic790038.png "säkerhet")

10. I hello **enkel inloggning (SSO)** avsnittet, utföra hello följande steg:

    ![Enkel inloggning](./media/active-directory-saas-intacct-tutorial/ic790039.png "enkel inloggning")

    a. Välj **aktivera enkel inloggning på**.

    b. Som **identitet providertyp**väljer **SAML 2.0**.

    c. I **utfärdar-URL** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.
   
    d. I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    e. Öppna din **Base64-** kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat** rutan.
   
    f. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-intacct-test-user"></a>Skapa en testanvändare Intacct

tooset konfigurerar Azure AD-användare så att de kan logga in tooIntacct, de måste etableras i Intacct. Intacct är etablering en manuell aktivitet.

**tooprovision användarkonton, utför följande steg hello:**

1. Logga in tooyour **Intacct** klient.

2. Klicka på hello **företagets** fliken och klicka sedan på **användare**.

    ![Användare](./media/active-directory-saas-intacct-tutorial/ic790041.png "användare")
3. Klicka på hello **Lägg till** fliken.

    ![Lägg till](./media/active-directory-saas-intacct-tutorial/ic790042.png "Lägg till")
4. I hello **användarinformation** avsnittet, utföra hello följande steg:

    ![Användarinformation](./media/active-directory-saas-intacct-tutorial/ic790043.png "användarinformation")

    a. Ange hello **användar-ID**, hello **efternamn**, **Förnamn**, hello **e-postadress**, hello **rubrik**, och hello **Phone** för ett Azure AD-konto som du vill tooprovision till hello **användarinformation** avsnitt.

    b. Välj hello **administratörsrättigheter** för ett Azure AD-konto som du vill tooprovision.
   
    c. Klicka på **Spara**. hello Azure AD-kontot innehavaren får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.

>[!NOTE]
>tooprovision användarkonton i Azure AD, som du kan använda andra Intacct användare skapa verktyg och API: er som tillhandahålls av Intacct.
        
### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIntacct.

![Tilldela användare][200] 

**tooassign Britta Simon tooIntacct utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Intacct**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.

När du klickar på hello Intacct panelen i hello åtkomstpanelen bör vara loggas du automatiskt tooyour Intacct program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

