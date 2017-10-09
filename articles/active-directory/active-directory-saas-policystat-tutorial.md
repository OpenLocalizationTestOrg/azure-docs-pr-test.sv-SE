---
title: "Självstudier: Azure Active Directory-integrering med PolicyStat | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och PolicyStat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 868053cd0d37359fd9b4aeb93dba42cbbaa09845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Självstudier: Azure Active Directory-integrering med PolicyStat

I kursen får du lära dig hur toointegrate PolicyStat med Azure Active Directory (AD Azure).

Integrera PolicyStat med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooPolicyStat
- Du kan aktivera din användare tooautomatically get inloggade tooPolicyStat (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med PolicyStat, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En PolicyStat enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till PolicyStat från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-policystat-from-hello-gallery"></a>Att lägga till PolicyStat från hello-galleriet
tooconfigure hello integrering av PolicyStat i Azure AD, behöver du tooadd PolicyStat hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd PolicyStat från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **PolicyStat**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. Markera hello resultat på panelen **PolicyStat**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PolicyStat baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i PolicyStat är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i PolicyStat toobe upprättas.

I PolicyStat, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med PolicyStat, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare PolicyStat](#creating-a-policystat-test-user)**  -toohave en motsvarighet för Britta Simon i PolicyStat som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt PolicyStat program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med PolicyStat:**

1. I hello Azure-portalen på hello **PolicyStat** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. På hello **PolicyStat domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.policystat.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [PolicyStat klienten supportteamet](http://www.policystat.com/support/) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooPolicyStat med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.

    Hej PolicyStat program förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **SAML-Token attribut** konfiguration.  

     hello följande skärmbild visar ett exempel på detta.

     ![Attribut](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "attribut")

6. mappningar av tooadd hello krävs, utför hello följande steg:

    | Attributets namn    |   Attributvärdet |
    |------------------- | -------------------- |
    | UID | ExtractMailPrefix([mail]) |
    
    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. I hello **attributnamn** textruta typen **uid**.

    c. I hello **attributvärdet** textruta väljer **ExtractMailPrefix()**.    
   
    d. Från hello **e** väljer **User.mail**.
    
    e. Klicka på **Ok**

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. Logga in på webbplatsen PolicyStat företag som en administratör i en annan webbläsarfönster.

9. Klicka på hello **Admin** fliken och klicka sedan på **konfiguration för enkel inloggning** i vänstra navigeringsfönstret.
   
    ![Administratörsmenyn](./media/active-directory-saas-policystat-tutorial/ic808633.png "administratör-menyn")

10. I hello **installationsprogrammet** väljer **aktivera enkel inloggning Integration**.
   
    ![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "enkel inloggning konfiguration")

11. Klicka på **Konfigurera attribut**, och sedan, i hello **Konfigurera attribut** avsnittet, utföra hello följande steg:
   
    ![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "enkel inloggning konfiguration")
   
    a. I hello **Username-attributet** textruta typen **uid**.

    b. I hello **förnamn attributet** textruta typen **Förnamn** för användare **Britta**.

    c. I hello **senaste namnattributet** textruta typen **efternamn** för användare **Simon**.

    d. I hello **e-attributet** textruta typen **emailaddress** för användare  **BrittaSimon@contoso.com** .

    e. Klicka på **spara ändringar**.

12. Klicka på **din IDP Metadata**, och sedan, i hello **din IDP Metadata** avsnittet, utför följande steg hello:
   
    ![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "enkel inloggning konfiguration")
   
    a. Öppna din hämtade metadatafil, kopiera hello innehåll och klistra in den i hello **din identitet providern Metadata** textruta.

    b. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-policystat-test-user"></a>Skapa en testanvändare PolicyStat

I ordning tooenable Azure AD-användare toolog i PolicyStat, måste de etableras i PolicyStat.  

PolicyStat stöder bara i tid användaretablering. Det innebär du inte behöver tooadd hello användare manuellt tooPolicyStat. hello användare ska få automatiskt läggas till på deras första inloggning via enkel inloggning.

>[!NOTE]
>Du kan använda något annat PolicyStat användarens konto skapas verktyg eller API: er som tillhandahålls av PolicyStat tooprovision användarkonton i Azure AD.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPolicyStat.

![Tilldela användare][200] 

**tooassign Britta Simon tooPolicyStat utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **PolicyStat**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour PolicyStat programmet när du klickar på hello PolicyStat panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

