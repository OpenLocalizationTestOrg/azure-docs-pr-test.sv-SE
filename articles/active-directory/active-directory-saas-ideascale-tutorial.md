---
title: "Självstudier: Azure Active Directory-integrering med IdeaScale | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och IdeaScale."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 10722b137e7565ee165e73994fd5a60b994719bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Självstudier: Azure Active Directory-integrering med IdeaScale

I kursen får du lära dig hur toointegrate IdeaScale med Azure Active Directory (AD Azure).

Integrera IdeaScale med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooIdeaScale
- Du kan aktivera din användare tooautomatically get inloggade tooIdeaScale (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med IdeaScale, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En IdeaScale enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till IdeaScale från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-ideascale-from-hello-gallery"></a>Att lägga till IdeaScale från hello-galleriet
tooconfigure hello integrering av IdeaScale i Azure AD, behöver du tooadd IdeaScale hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd IdeaScale från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **IdeaScale**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. Markera hello resultat på panelen **IdeaScale**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med IdeaScale baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i IdeaScale är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i IdeaScale toobe upprättas.

I IdeaScale, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med IdeaScale, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare IdeaScale](#creating-an-ideascale-test-user)**  -toohave en motsvarighet för Britta Simon i IdeaScale som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt IdeaScale program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med IdeaScale:**

1. I hello Azure-portalen på hello **IdeaScale** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. På hello **IdeaScale domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.ideascale.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [IdeaScale klienten supportteamet](http://support.ideascale.com/) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. På hello **IdeaScale Configuration** klickar du på **konfigurera IdeaScale** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enhets-ID** från hello **Snabbreferens avsnittet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. Logga in tooyour IdeaScale företagets webbplats som en administratör i en annan webbläsarfönster.

8. Gå för**Community inställningar**.
   
    ![Community-inställningar](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community-inställningar")

9. Gå för**säkerhet \> inställningar för enkel inloggning**.
   
    ![Enkel inloggning inställningar](./media/active-directory-saas-ideascale-tutorial/ic790848.png "enkel inloggning inställningar")

10. Som **enkel inloggning typen**väljer **SAML 2.0**.
   
    ![Enkel inloggning typen](./media/active-directory-saas-ideascale-tutorial/ic790849.png "enkel inloggning typ")

11. På hello **inställningar för enkel inloggning** dialogrutan utföra hello följande steg:
   
    ![Enkel inloggning inställningar](./media/active-directory-saas-ideascale-tutorial/ic790850.png "enkel inloggning inställningar")
   
    a. I **SAML IdP enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.

    b. Kopiera hello innehållet hämtas metadata från Azure-portalen och klistra in den i hello **SAML IdP Metadata** textruta.

    c. I **logga ut lyckade URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.

    d. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-ideascale-test-user"></a>Skapa en testanvändare IdeaScale

tooenable Azure AD-användare toolog i IdeaScale, måste de vara etablerade i tooIdeaScale. Hello gäller IdeaScale är etablering en manuell aktivitet.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **IdeaScale** företagets webbplats som administratör.

2. Gå för**Community inställningar**.
   
    ![Community-inställningar](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community-inställningar")

3. Gå för**grundläggande inställningar \> medlem Management**.

4. Klicka på **lägga till medlem**.
   
    ![Hantering av medlem](./media/active-directory-saas-ideascale-tutorial/ic790852.png "medlem Management")

5. I hello Lägg till ny medlem avsnitt, utföra hello följande steg:
   
    ![Lägg till ny medlem](./media/active-directory-saas-ideascale-tutorial/ic790853.png "Lägg till ny medlem")
   
    a. I hello **e-postadresser** textruta typen hello e-postadress på en giltig AAD-konto som du vill tooprovision.
   
    b. Klicka på **spara ändringar**. 
   
    >[!NOTE]
    >hello Azure Active Directory användare får ett e-postmeddelande med en länk tooconfirm hello-konto innan den aktiveras.
      
>[!NOTE]
>Du kan använda något annat IdeaScale användarens konto skapas verktyg eller API: er som tillhandahålls av IdeaScale tooprovision AAD-användarkonton.
 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIdeaScale.

![Tilldela användare][200] 

**tooassign Britta Simon tooIdeaScale utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **IdeaScale**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning


hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour IdeaScale programmet när du klickar på hello IdeaScale panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

