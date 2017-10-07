---
title: "Självstudier: Azure Active Directory-integrering med Tableau Server | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Tableau Server."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: feb2087bd6ae6ddcb920901e6719688fc95ae287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Självstudier: Azure Active Directory-integrering med Tableau Server

I kursen får du lära dig hur toointegrate Tableau Server med Azure Active Directory (AD Azure).

Integrera Tableau Server med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooTableau Server
- Du kan aktivera din användare tooautomatically get inloggade tooTableau Server (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Tableau Server behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Tableau Server enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Tableau Server från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-tableau-server-from-hello-gallery"></a>Att lägga till Tableau Server från hello-galleriet
tooconfigure hello integrering av Tableau Server i Azure AD, behöver du tooadd Tableau Server hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Tableau Server från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Tableau Server**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. Markera hello resultat på panelen **Tableau Server**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Tableau Server baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Tableau Server är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Tableau servern toobe upprättas.

Tilldela hello värdet hello i Tableau Server **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Tableau Server behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Tableau Server](#creating-a-tableau-server-test-user)**  -toohave en motsvarighet för Britta Simon i Tableau servern som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i serverprogrammet Tableau.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Tableau Server:**

1. I hello Azure-portalen på hello **Tableau Server** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. På hello **URL: er och Tableau serverdomänen** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://azure.<domain name>.link`
    
    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://azure.<domain name>.link`

    c. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > hello föregående värden är inte verkliga värden. Senare kan uppdatera du hello värden med hello faktiska URL och identifierare från konfigurationssidan för hello Tableau Server. 

4. Tableau serverprogrammet förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello **”användarattribut”** avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel på hello samma.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:
    
    | Attributets namn | Attributvärdet |
    | ---------------| --------------- |    
    | användarnamn | *User.DisplayName* |

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.
    
    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **Ok**


6. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS>
8. tooget SSO konfigurerats för ditt program måste toosign på tooyour Tableau Server-klient som administratör.
   
   a. Klicka på hello i hello Tableau serverkonfiguration **SAML** fliken.
  
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Markera kryssrutan för hello av **Använd SAML för enkel inloggning**.
   
   c. Tableau Server Retur-URL – hello URL som Tableau Server-användare kommer åt, till exempel http://tableau_server. Du bör inte använda http://localhost. Med en URL som avslutande snedstreck (till exempel http://tableau_server/) stöds inte. Kopiera **Tableau Server Retur-URL** och klistra in den tooAzure AD **logga URL** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.
   
   d. SAML enhets-ID – hello enhets-ID som unikt identifierar din Tableau Server installation toohello IdP. Du kan ange Tableau Server URL: en igen här, om du vill, men den har inte toobe Tableau-Serveradress. Kopiera **SAML enhets-ID** och klistra in den tooAzure AD **identifierare** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.
     
   e. Klicka på hello **exportera metadatafil** och öppna den i hello text redigeringsprogram. Leta upp Assertion konsumenten tjänst-URL med Http Post och Index 0 och kopiera hello-URL. Klistra in den tooAzure AD **Reply URL** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.
   
   f. Leta upp din Federationsmetadata-fil som hämtas från Azure-portalen och överföra den i hello **SAML Idp metadatafil**.
   
   g. Klicka på hello **OK** knappen hello Tableau serverkonfiguration på sidan.
   
    >[!NOTE] 
    >Kunden har tooupload alla certifikat i hello Tableau Server SAML SSO-konfigurationen och kommer hämta ignoreras i hello flödet för enkel inloggning.
    >Om du behöver hjälp konfigurerar SAML på Tableau Server sedan se toothis artikel [konfigurera SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-tableau-server-test-user"></a>Skapa en testanvändare Tableau Server

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Tableau Server. Du måste tooprovision alla hello användare i hello Tableau server. 

Att användarnamnet för användaren hello bör matcha hello-värde som du har konfigurerat i hello Azure AD-attributet för **användarnamn**. Med hello rätt mappning hello integrering ska fungera [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Om du behöver toocreate en användare manuellt, måste toocontact hello Tableau Server-administratören i din organisation.
> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTableau Server.

![Tilldela användare][200] 

**tooassign Britta Simon tooTableau Server, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Tableau Server**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på panelen för hello Tableau Server i hello åtkomstpanelen får automatiskt inloggade tooyour Tableau serverprogram.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

