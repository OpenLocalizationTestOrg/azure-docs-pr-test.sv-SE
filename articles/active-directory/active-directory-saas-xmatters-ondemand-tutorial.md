---
title: "Självstudier: Azure Active Directory-integrering med xMatters OnDemand | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och xMatters OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 7cc8f9f0d8cefc8a60b9514027437ced50c66242
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Självstudier: Azure Active Directory-integrering med xMatters OnDemand

I kursen får du lära dig hur toointegrate xMatters OnDemand med Azure Active Directory (AD Azure).

Integrera xMatters OnDemand med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooxMatters OnDemand
- Du kan aktivera din användare tooautomatically get inloggade tooxMatters OnDemand (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med xMatters OnDemand, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En xMatters OnDemand enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till xMatters OnDemand från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-xmatters-ondemand-from-hello-gallery"></a>Att lägga till xMatters OnDemand från hello-galleriet
tooconfigure hello integrering av xMatters OnDemand i Azure AD, behöver du tooadd xMatters OnDemand hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd xMatters OnDemand från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **xMatters OnDemand**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. Markera hello resultat på panelen **xMatters OnDemand**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med xMatters OnDemand baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork Azure AD måste tooknow vilka hello motsvarighet användaren i xMatters OnDemand är tooa i Azure AD. Med andra ord en länk mellan en Azure AD-användare och hello relaterade användare i xMatters OnDemand måste toobe upprättas.

Tilldela hello värdet för hello i xMatters OnDemand **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med xMatters OnDemand, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare för xMatters OnDemand](#creating-a-xmatters-ondemand-test-user)**  -toohave en motsvarighet för Britta Simon i xMatters OnDemand som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din xMatters OnDemand-programmet.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med xMatters OnDemand:**

1. I hello Azure-portalen på hello **xMatters OnDemand** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. På hello **xMatters OnDemand domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [xMatters OnDemand supportteam](https://www.xmatters.com/company/contact-us/) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikatfilen lokalt som **c:\\XMatters OnDemand.cer**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > Du behöver tooforward hello certifikat toohello [xMatters OnDemand supportteam](https://www.xmatters.com/company/contact-us/). hello certifikat måste toobe som laddats upp av hello xMatters supportteamet innan du kan slutföra hello enkel inloggning konfigurationen. 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. På hello **xMatters OnDemand Configuration** klickar du på **konfigurera xMatters OnDemand** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. I en annan webbläsarfönster logga in tooyour XMatters OnDemand företagets webbplats som administratör.

8. Klicka i hello verktygsfältet hello längst upp **Admin**, och klicka sedan på **företagsinformation** i hello navigeringsfältet hello vänster sida.
   
    ![Admin](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")

9. På hello **SAML-konfiguration** utför hello följande steg:
   
    ![SAML-konfiguration](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-konfiguration")
   
    a. Välj **aktivera SAML**.
   
    b. Klistra in **SAML enhets-ID**, som du har kopierat från hello Azure-portalen i hello **identitet Provider-ID** textruta.
   
    c. Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **enkel inloggning på URL: en** textruta.
   
    d. Klistra in **Sign-Out URL**, som du har kopierat från hello Azure-portalen i hello **URL för enkel logga ut** textruta.
   
    e. Klicka på hello företagsinformation sidan hello överst **spara ändringar**.
    
    ![Företagets information](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "företagets information")

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-xmatters-ondemand-test-user"></a>Skapa en xMatters OnDemand testanvändare

I ordning tooenable Azure AD-användare toolog i tooXMatters OnDemand, måste de etableras i XMatters OnDemand. Hello gäller XMatters OnDemand är etablering en manuell aktivitet.

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a>tooprovision användarkonton, utföra hello följande steg:
1. Logga in tooyour **XMatters OnDemand** klient.

2.  Klicka på **användare** fliken och klicka sedan på **Lägg till användare**.
  
    ![Användare](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "användare")

3. I hello **lägga till en användare** avsnittet, utföra hello följande steg:
   
    ![Lägga till en användare](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "lägga till en användare")

    a. Välj **Active**.

    b. I hello **användar-ID** textruta typen hello användar-id för användaren som Brittasimon@contoso.com.
   
    c. I hello **Förnamn** textruta Ange först namnet på hello användaren som Britta.

    d. I hello **efternamn** textruta hello användaren som Simon typen efternamn.
    
    e. I hello **plats** textruta RETUR hello giltig plats för ett giltigt Azure AD-kontot som du vill tooprovision.
    
    f. Klicka på **Spara**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooxMatters OnDemand.

![Tilldela användare][200] 

**tooassign Britta Simon tooxMatters OnDemand, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **xMatters OnDemand**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour xMatters OnDemand programmet när du klickar på hello xMatters OnDemand-panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png

