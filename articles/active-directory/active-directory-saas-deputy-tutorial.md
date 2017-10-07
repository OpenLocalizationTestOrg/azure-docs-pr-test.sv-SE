---
title: "Självstudier: Azure Active Directory-integrering med vice | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och vice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 42f65b758682ce2513b6bb38ef40a19f955c88c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Självstudier: Azure Active Directory-integrering med vice

I kursen får du lära dig hur toointegrate vice med Azure Active Directory (AD Azure).

Integrera vice med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooDeputy
- Du kan aktivera din användare tooautomatically get inloggade tooDeputy (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med vice måste hello följande objekt:

- En Azure AD-prenumeration
- En vice enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till vice från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-deputy-from-hello-gallery"></a>Att lägga till vice från hello-galleriet
tooconfigure hello integrering av vice i Azure AD, behöver du tooadd vice hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd vice från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **vice**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_search.png)

5. Markera hello resultat på panelen **vice**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med vice baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i vice är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i vice toobe upprättas.

I vice, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med vice, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare vice](#creating-a-deputy-test-user)**  -toohave en motsvarighet för Britta Simon i vice som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet vice.

**tooconfigure Azure AD enkel inloggning med vice, utför hello följande steg:**

1. I hello Azure-portalen på hello **vice** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_samlbase.png)

3. På hello **URL: er och vice domän** om du vill tooconfigure hello programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url1.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

4. Kontrollera **visa avancerade inställningar för URL: en**. Om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url2.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<your-subdomain>.<region>.deputy.com`
    
    >[!NOTE]
    > Vice region suffix är valfritt eller den ska använda något av följande: Australien | na | Europa | som | la | af | en | ent Australien | ent na | ent Europa | ent-som | överordnad la | överordnad af | en överordnad

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Kontakta [vice supportteamet](https://www.deputy.com/call-centers-customer-support-scheduling-software) tooget dessa värden. 

5. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_general_400.png)
    
7. På hello **vice Configuration** klickar du på **konfigurera vice** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_configure.png) 

8. Navigera toohello följande URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config). Gå för**säkerhetsinställningar** och på **redigera**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

9. På den här **säkerhetsinställningar** utför nedanstående steg.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)
    
    a. Aktivera **inloggning via sociala medier**.
   
    b. Öppna din Base64-kodat certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **OpenSSL certifikat** textruta.
   
    c. Skriv i hello SAML SSO URL: en textruta`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. Ersätt i hello SAML SSO URL: en textruta `<your subdomain>` med din underdomänen.
   
    e. Ersätt i hello SAML SSO URL: en textruta `<saml sso url>` med hello **SAML inloggning tjänst-URL för enkel** du har kopierat från hello Azure-portalen.
   
    f. Klicka på **Spara inställningar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-deputy-test-user"></a>Skapa en vice testanvändare

tooenable Azure AD-användare toolog i tooDeputy, måste de etableras i vice. Vid vice är etablering en manuell aktivitet.

#### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision ett användarkonto, utför följande steg hello:
1. Logga in tooyour vice företagets webbplats som administratör.

2. Klicka på hello övre navigeringsfönstret, **personer**.
   
   ![Personer](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "personer")

3. Klicka på hello **lägga till personer** och klicka sedan på **lägga till en enda person**.
   
   ![Lägg till personer](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "lägga till personer")

4. Utför följande steg hello och klickar på **Spara & bjuda in**.
   
   ![Ny användare](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "ny användare")

   a. I hello **namn** textruta, ange namnet på hello användaren som **BrittaSimon**.
   
   b. I hello **e-post** textruta typen hello e-postadressen för en Azure AD-konto som du vill tooprovision.
   
   c. I hello **fungerar på** textruta typnamn hello företag.
   
   d. Klicka på **Spara & bjuda in** knappen.

5. hello AAD användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras. Du kan använda något annat vice användarens konto skapas verktyg eller API: er som tillhandahålls av vice tooprovision AAD användarkonton.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDeputy.

![Tilldela användare][200] 

**tooassign Britta Simon tooDeputy utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **vice**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour vice programmet när du klickar på hello vice panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png

