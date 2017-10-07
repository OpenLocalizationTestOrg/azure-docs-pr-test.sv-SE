---
title: "Självstudier: Azure Active Directory-integrering med Lucidchart | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Lucidchart."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 774e5f423097650a3cae8e8ca13b2c65b8470736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Självstudier: Azure Active Directory-integrering med Lucidchart

I kursen får du lära dig hur toointegrate Lucidchart med Azure Active Directory (AD Azure).

Integrera Lucidchart med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooLucidchart
- Du kan aktivera din användare tooautomatically get inloggade tooLucidchart (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Lucidchart, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Lucidchart enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Lucidchart från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-lucidchart-from-hello-gallery"></a>Att lägga till Lucidchart från hello-galleriet
tooconfigure hello integrering av Lucidchart i Azure AD, behöver du tooadd Lucidchart hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Lucidchart från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Lucidchart**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_search.png)

5. Markera hello resultat på panelen **Lucidchart**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Lucidchart baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Lucidchart är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Lucidchart toobe upprättas.

I Lucidchart, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Lucidchart, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Lucidchart](#creating-a-lucidchart-test-user)**  -toohave en motsvarighet för Britta Simon i Lucidchart som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Lucidchart program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Lucidchart:**

1. I hello Azure-portalen på hello **Lucidchart** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

3. På hello **Lucidchart domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_url.png)

    I hello **inloggnings-URL** textruta Skriv en URL som:`https://chart2.office.lucidchart.com/saml/sso/azure`

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_general_400.png)

6. Logga in på webbplatsen Lucidchart företag som en administratör i en annan webbläsarfönster.

7. Hello-menyn överst hello **Team**.
   
    ![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")

8. Klicka på **program \> hantera SAML**.
   
    ![Hantera SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "hantera SAML")

9. På hello **SAML autentiseringsinställningar** dialogrutan utför hello följande steg:
   
    a. Välj **aktivera SAML-autentisering**, och klicka sedan på **valfritt**.

    ![Inställningar för SAML-autentisering](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "inställningar för SAML-autentisering")
 
    b. I hello **domän** textruta Skriv din domän och klicka sedan på **ändra certifikatet**.

    ![Ändra certifikat](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "ändra certifikat")
 
    c. Öppna din hämtade metadatafil, kopiera hello innehåll och klistra in den i hello **överföra Metadata** textruta.

    ![Överföra Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "överföra Metadata")
 
    d. Välj **automatiskt lägga till nya användare toohello team**, och klicka sedan på **spara ändringar**.

    ![Spara ändringarna](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "spara ändringar")

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-lucidchart-test-user"></a>Skapa en testanvändare Lucidchart

Det finns inga uppgiften för du tooconfigure användaretablering tooLucidchart.  När en tilldelad användare försöker toolog till Lucidchart med hello åtkomstpanelen, kontrollerar Lucidchart om hello användaren finns.  

Om det finns inget användarkonto ännu, skapas den automatiskt av Lucidchart.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLucidchart.

![Tilldela användare][200] 

**tooassign Britta Simon tooLucidchart utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Lucidchart**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Lucidchart programmet när du klickar på hello Lucidchart panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_203.png

