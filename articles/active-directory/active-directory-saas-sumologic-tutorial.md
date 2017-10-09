---
title: "Självstudier: Azure Active Directory-integrering med SumoLogic | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SumoLogic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 2ef1bd329f5db8899f0b57744e4c0f6eed1c532f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Självstudier: Azure Active Directory-integrering med SumoLogic

I kursen får du lära dig hur toointegrate SumoLogic med Azure Active Directory (AD Azure).

Integrera SumoLogic med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooSumoLogic
- Du kan aktivera din användare tooautomatically get inloggade tooSumoLogic (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SumoLogic, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En SumoLogic enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SumoLogic från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sumologic-from-hello-gallery"></a>Att lägga till SumoLogic från hello-galleriet
tooconfigure hello integrering av SumoLogic i Azure AD, behöver du tooadd SumoLogic hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SumoLogic från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **SumoLogic**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. Markera hello resultat på panelen **SumoLogic**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SumoLogic baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SumoLogic är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SumoLogic toobe upprättas.

I SumoLogic, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med SumoLogic, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare SumoLogic](#creating-a-sumologic-test-user)**  -toohave en motsvarighet för Britta Simon i SumoLogic som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt SumoLogic program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SumoLogic:**

1. I hello Azure-portalen på hello **SumoLogic** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. På hello **SumoLogic domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.SumoLogic.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [SumoLogic klienten supportteamet](https://www.sumologic.com/contact-us/) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. På hello **SumoLogic Configuration** klickar du på **konfigurera SumoLogic** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. Logga in tooyour SumoLogic företagets webbplats som en administratör i en annan webbläsarfönster.

8. Gå för**hantera \> säkerhet**.
   
    ![Hantera](./media/active-directory-saas-sumologic-tutorial/ic778556.png "hantera")

9. Klicka på **SAML**.
   
    ![Globala säkerhetsinställningar](./media/active-directory-saas-sumologic-tutorial/ic778557.png "globala säkerhetsinställningar")

10. Från hello **väljer en konfiguration eller skapa en ny** väljer **Azure AD**, och klicka sedan på **konfigurera**.
   
    ![Konfigurera SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "konfigurera SAML 2.0")

11. På hello **konfigurera SAML 2.0** dialogrutan utföra hello följande steg:
   
    ![Konfigurera SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "konfigurera SAML 2.0")
   
    a. I hello **Konfigurationsnamnet** textruta typen **Azure AD**. 

    b. Välj **felsökningsläge**.

    c. I hello **utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen. 

    d. I hello **Authn begära URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.

    e. Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in hello hela certifikatet till **X.509-certifikat** textruta.

    f. Som **e-attributet**väljer **Använd SAML ämne**.  

    g. Välj **SP initierade inloggningen Configuration**.

    h. I hello **inloggningen sökvägen** textruta typen **Azure** och på **spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-sumologic-test-user"></a>Skapa en testanvändare SumoLogic

I ordning tooenable Azure AD-användare toolog i tooSumoLogic, måste de vara etablerade tooSumoLogic.  

* Hello gäller SumoLogic är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **SumoLogic** klient.

2. Gå för**hantera \> användare**.
   
    ![Användare](./media/active-directory-saas-sumologic-tutorial/ic778561.png "användare")

3. Klicka på **Lägg till**.
   
    ![Användare](./media/active-directory-saas-sumologic-tutorial/ic778562.png "användare")

4. På hello **ny användare** dialogrutan utföra hello följande steg:
   
    ![Ny användare](./media/active-directory-saas-sumologic-tutorial/ic778563.png "ny användare") 
 
    a. Typen hello relaterad information på hello Azure AD-konto som du vill tooprovision till hello **Förnamn**, **efternamn**, och **e-post** textrutor.
  
    b. Välj en roll.
  
    c. Som **Status**väljer **Active**.
  
    d. Klicka på **Spara**.

>[!NOTE]
>Du kan använda något annat SumoLogic användarens konto skapas verktyg eller API: er som tillhandahålls av SumoLogic tooprovision AAD-användarkonton. 
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSumoLogic.

![Tilldela användare][200] 

**tooassign Britta Simon tooSumoLogic utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SumoLogic**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour SumoLogic programmet när du klickar på hello SumoLogic panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

