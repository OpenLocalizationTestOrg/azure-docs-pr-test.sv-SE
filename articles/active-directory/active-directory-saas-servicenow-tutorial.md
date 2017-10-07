---
title: "Självstudier: Azure Active Directory-integrering med ServiceNow | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ServiceNow och ServiceNow Express."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: df6a07dd1aa437198fbdb9d0a04ea14f3a320249
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Självstudier: Azure Active Directory-integrering med ServiceNow
I kursen får du lära dig hur toointegrate ServiceNow och ServiceNow Express med Azure Active Directory (AD Azure).

Integrera ServiceNow och ServiceNow Express med Azure AD ger dig hello följande fördelar:

* Du kan styra i Azure AD som har åtkomst till tooServiceNow och ServiceNow Express
* Du kan aktivera din användare tooautomatically get inloggade tooServiceNow och ServiceNow Express (Single Sign-On) med sina Azure AD-konton
* Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
tooconfigure Azure AD-integrering med ServiceNow och ServiceNow Express, behöver du hello följande objekt:

* En Azure AD-prenumeration
* För ServiceNow, instans eller innehavare för ServiceNow Calgary version eller högre
* För ServiceNow Express, en instans av ServiceNow Express, Helsingfors version eller högre
* Hej ServiceNow-klient måste ha hello [flera enkel inloggning på Plugin-providern](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) aktiverat. Detta kan göras med [att skicka en serviceförfrågan](https://hi.service-now.com). 

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.
> 
> 

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till ServiceNow från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning för ServiceNow eller ServiceNow Express

## <a name="adding-servicenow-from-hello-gallery"></a>Att lägga till ServiceNow från hello-galleriet
tooconfigure hello integrering av ServiceNow eller ServiceNow Express i Azure AD, behöver du tooadd ServiceNow hello galleriet tooyour listan över hanterade SaaS-appar. 

**tooadd ServiceNow från galleriet hello utför hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**. 
   
    ![Active Directory][1]
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program][2]
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Program][3]
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Program][4]
6. Skriv i sökrutan hello **ServiceNow**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. I resultatfönstret hello väljer **ServiceNow**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Du konfigurera och testa Azure AD enkel inloggning med ServiceNow eller ServiceNow Express baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ServiceNow är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ServiceNow toobe upprättas.
Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ServiceNow. tooconfigure och testa Azure AD enkel inloggning med ServiceNow, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning för ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  -tooenable användare-toouse den här funktionen.
2. **[Konfigurera Azure AD enkel inloggning för ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  -tooenable användare-toouse den här funktionen.
3. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
4. **[Skapa en testanvändare ServiceNow](#creating-a-servicenow-test-user)**  -toohave en motsvarighet för Britta Simon i ServiceNow som är länkade toohello Azure AD-representation av henne.
5. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
6. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

> [!NOTE]
> Om du vill utelämna tooconfigure ServiceNow steg 2. Likaså om du vill tooconfigure ServiceNow Express utelämna steg 1.
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfigurera Azure AD-Single Sign-On för ServiceNow
1. I klassiska hello Azure AD-portalen, på hello **ServiceNow** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan .
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigurera enkel inloggning")

2. På hello **hur skulle du som användare toosign på tooServiceNow** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigurera enkel inloggning")

3. På hello **konfigurera Appinställningar** utför hello följande steg:
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurera app-URL")
   
    a. i hello **ServiceNow logga URL** textruta, Skriv URL: en används av användare toosign på tooyour ServiceNow programmet enligt hello: `https://<instance-name>.service-now.com`.
   
    b. I hello **identifierare** textruta, Skriv URL: en används av användare toosign på tooyour ServiceNow programmet enligt hello: `https://<instance-name>.service-now.com`.
   
    c. Klicka på **Nästa**

4. toohave Azure AD automatiskt konfigurerar ServiceNow för SAML-baserad autentisering, ange ditt ServiceNow-instansnamn administratörsanvändarnamnet och administratörslösenord i hello **automatiskt konfigurera enkel inloggning** formuläret och klicka på  *Konfigurera*. Observera att hello administratörsanvändarnamnet tillhandahålls måste ha hello **security_admin** roll i ServiceNow för den här toowork. Annars toomanually konfigurera ServiceNow toouse Azure AD som en SAML-identitetsprovider klickar du på **manuellt konfigurera hello program för enkel inloggning**, klicka på **nästa** och fullständig hello följande steg.
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurera app-URL")

5. På hello **Konfigurera enkel inloggning på ServiceNow** klickar du på **hämta certifikat**, spara hello certifikatfilen lokalt på datorn.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigurera enkel inloggning")

6. Inloggning tooyour ServiceNow-programmet som administratör.

7. Aktivera hello *integrering – flera enkel inloggning installationsprogrammet för providern* plugin-programmet genom att följa hello nästa steg:
   
    a. I navigeringsfönstret hello vänster hello gå för**System Definition** avsnittet och klicka sedan på **plugin-program**.
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "aktivera plugin-program")
   
    b. Sök efter *integrering – flera enkel inloggning installationsprogrammet för providern*.
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "aktivera plugin-program")
   
    c. Välj hello plugin-programmet. Rigth Klicka och välj **aktivera eller uppgradering**.
   
    d. Klicka på hello **aktivera** knappen.

8. Hello navigeringsfönstret hello vänster, klicka på **egenskaper**.  
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "konfigurera app-URL")

9. På hello **flera egenskaper för SSO** dialogrutan utföra hello följande steg:
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "konfigurera app-URL")
   
    a. Som **aktivera flera providern SSO**väljer **Ja**.
   
    b. Som **Aktivera felsökningsloggning fick hello flera providern SSO integration**väljer **Ja**.
   
    c. I **hello fält hello användaren tabell som...**  textruta typen **user_name**.
   
    d. Klicka på **Spara**.

10. Hello navigeringsfönstret hello vänster, klicka på **x509 certifikat**.
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Konfigurera enkel inloggning")

11. På hello **X.509-certifikat** dialogrutan klickar du på **ny**.
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Konfigurera enkel inloggning")

12. På hello **X.509-certifikat** dialogrutan utföra hello följande steg:
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigurera enkel inloggning")
    
     a. Klicka på **Ny**.
    
     b. I hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: **TestSAML2.0**).
    
     c. Välj **Active**.
    
     d. Som **Format**väljer **PEM**.
    
     e. Som **typen**väljer **förtroende Store Cert**.
    
     f. Öppna Base64-kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **PEM certifikat** textruta.
    
     g. Klicka på **uppdatering**.

13. Hello navigeringsfönstret hello vänster, klicka på **identitetsleverantörer**.
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Konfigurera enkel inloggning")

14. På hello **identitetsleverantörer** dialogrutan klickar du på **ny**:
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Konfigurera enkel inloggning")

15. På hello **identitetsleverantörer** dialogrutan klickar du på **SAML2 Update1?**:
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Konfigurera enkel inloggning")

16. Utför följande steg hello på hello SAML2 Update1 dialogrutan:
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Konfigurera enkel inloggning")

    a. i hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: **SAML 2.0**).

    b. I hello **Info** textruta typen **e-post** eller **user_name**, beroende på vilket fält används toouniquely identifiera användare i din ServiceNow-distribution. 

    > [!NOTE] 
    > Du kan configue Azure AD tooemit antingen hello Azure AD användar-ID (användarens huvudnamn) eller hello e-postadress som hello Unik identifierare i hello SAML-token genom att gå toohello **ServiceNow > attribut > enkel inloggning** avsnitt i Hej klassiska Azure-portalen och mappningen hello önskad fältet toohello **nameidentifier** attribut. hello-värde som lagras för markerade hello-attributet i Azure AD (t.ex. användarens huvudnamn) måste matcha hello-värde som lagras i ServiceNow för hello angivna fält (t.ex. användarnamn)

    c. I klassiska hello Azure AD-portalen kopiera hello **identitet Provider-ID** värdet och klistrar in den i hello **identitet providern URL** textruta.

    d. I klassiska hello Azure AD-portalen kopiera hello **autentiserings-URL för begäran** värdet och klistrar in den i hello **identitetsleverantörens AuthnRequest** textruta.

    e. I klassiska hello Azure AD-portalen kopiera hello **tjänst-URL för enkel Sign-Out** värdet och klistrar in den i hello **identitetsleverantörens SingleLogoutRequest** textruta.

    f. I hello **ServiceNow Homepage** textruta typen hello Webbadressen till startsidan för ServiceNow-instans.

    > [!NOTE] 
    > Hej ServiceNow instans webbsida är en sammansättning av din **ServieNow klient URL** och **/navpage.do** (t.ex.:`https://fabrikam.service-now.com/navpage.do`).

    g. I hello **enhets-ID / utfärdaren** textruta typen hello URL för din ServiceNow-klient.

    h. I hello **målgruppen URL** textruta typen hello URL för din ServiceNow-klient. 

    Jag. I hello **protokollet bindningen för hello IDP'S SingleLogoutRequest** textruta typen **urn: oasis: namn: tc: SAML:2.0:bindings:HTTP-omdirigera**.

    j. Skriv i hello NameID princip textruta **urn: oasis: namn: tc: SAML:1.1:nameid-format: Okänt**.

    k. Avmarkera **skapa en AuthnContextClass**.

    l. I hello **AuthnContextClassRef metoden**, typen `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Det här krävs bara om du endast organisation i molnet. Om du använder en lokal AD FS eller MFA för autentisering bör du inte konfigurera det här värdet. 

    m. I **klockan skeva** textruta typen **60**.

    n. Som **enkel inloggning på skriptet**väljer **MultiSSO_SAML2_Update1**.

    o. Som **x509 certifikat**väljer hello-certifikat som du har skapat i föregående steg i hello.

    p. Klicka på **skicka**. 

1. Välj hello konfiguration för enkel inloggning bekräftelse på klassiska hello Azure AD-portalen och klicka sedan på **nästa**. 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigurera enkel inloggning")

2. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigurera enkel inloggning")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfigurera Azure AD-Single Sign-On för ServiceNow Express
1. I klassiska hello Azure AD-portalen, på hello **ServiceNow** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan .
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigurera enkel inloggning")

2. På hello **hur skulle du som användare toosign på tooServiceNow** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigurera enkel inloggning")

3. På hello **konfigurera Appinställningar** utför hello följande steg:
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurera app-URL")
   
    a. i hello **ServiceNow logga URL** textruta, Skriv URL: en används av användare toosign på tooyour ServiceNow programmet enligt hello: `https://<instance-name>.service-now.com`.
   
    b. I hello **utfärdar-URL** textruta, Skriv URL: en används av användare toosign på tooyour ServiceNow programmet enligt hello `https://<instance-name>.service-now.com`.
   
    c. Klicka på **Nästa**

4. Klicka på **manuellt konfigurera hello program för enkel inloggning**, klicka på **nästa** och fullständig hello följande steg.
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurera app-URL")

5. På hello **Konfigurera enkel inloggning på ServiceNow** klickar du på **hämta certifikat**, spara hello certifikatfilen lokalt på datorn och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigurera enkel inloggning")

6. Inloggning tooyour ServiceNow Express-program som administratör.

7. Hello navigeringsfönstret hello vänster, klicka på **enkel inloggning**.  
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "konfigurera app-URL")

8. På hello **enkel inloggning** dialogrutan klickar du på hello konfigurationsikon på hello övre högra och ange hello följande egenskaper:
   
    ![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "konfigurera app-URL")
   
    a. Växla **aktivera flera providern SSO** toohello höger.
   
    b. Växla **Aktivera felsökningsloggning för hello flera providern SSO integration** toohello höger.
   
    c. I **hello fält hello användaren tabell som...**  textruta typen **user_name**.
9. På hello **enkel inloggning** dialogrutan klickar du på **Lägg till nytt certifikat**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Konfigurera enkel inloggning")
10. På hello **X.509-certifikat** dialogrutan utföra hello följande steg:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigurera enkel inloggning")
    
    a. I hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: **TestSAML2.0**).
    
    b. Välj **Active**.
    
    c. Som **Format**väljer **PEM**.
    
    d. Som **typen**väljer **förtroende Store Cert**.
    
    e. Skapa en Base64-kodad fil från din hämtat certifikat.
    
    > [!NOTE]
    > Mer information finns i [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o).
    > 
    > 
    
    f. Öppna Base64-kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **PEM certifikat** textruta.
    
    g. Klicka på **uppdatering**.
11. På hello **enkel inloggning** dialogrutan klickar du på **lägga till nya IdP**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Konfigurera enkel inloggning")
12. På hello **lägga till nya identitetsleverantör** dialogrutan under **konfigurera identitetsleverantören**, utföra hello följande steg:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Konfigurera enkel inloggning")

    a. i hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: **SAML 2.0**).

    b. I klassiska hello Azure AD-portalen kopiera hello **identitet Provider-ID** värdet och klistrar in den i hello **identitet providern URL** textruta.

    c. I klassiska hello Azure AD-portalen kopiera hello **autentiserings-URL för begäran** värdet och klistrar in den i hello **identitetsleverantörens AuthnRequest** textruta.

    d. I klassiska hello Azure AD-portalen kopiera hello **tjänst-URL för enkel Sign-Out** värdet och klistrar in den i hello **identitetsleverantörens SingleLogoutRequest** textruta.

    e. Som **providern identitetscertifikat**väljer hello-certifikat som du har skapat i föregående steg i hello.


1. Klicka på **avancerade inställningar**, och under **ytterligare identitetsegenskaper providern**, utföra hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Konfigurera enkel inloggning")
   
    a. I hello **protokollet bindningen för hello IDP'S SingleLogoutRequest** textruta typen **urn: oasis: namn: tc: SAML:2.0:bindings:HTTP-omdirigera**.
   
    b. I hello **NameID princip** textruta typen **urn: oasis: namn: tc: SAML:1.1:nameid-format: Okänt**.    
   
    c. I hello **AuthnContextClassRef metoden**, typen **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
   
    d. Avmarkera **skapa en AuthnContextClass**.

2. Under **ytterligare egenskaper**, utföra hello följande steg:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Konfigurera enkel inloggning")
   
    a. I hello **ServiceNow Homepage** textruta typen hello Webbadressen till startsidan för ServiceNow-instans.
   
    > [!NOTE]
    > Hej ServiceNow instans webbsida är en sammansättning av din **ServieNow klient URL** och **/navpage.do** (t.ex.: `https://fabrikam.service-now.com/navpage.do`).
    > 
    > 
   
    b. I hello **enhets-ID / utfärdaren** textruta typen hello URL för din ServiceNow-klient.
   
    c. I hello **Målgrupps-URI** textruta typen hello URL för din ServiceNow-klient. 
   
    d. I **klockan skeva** textruta typen **60**.
   
    e. I hello **Info** textruta typen **e-post** eller **user_name**, beroende på vilket fält används toouniquely identifiera användare i din ServiceNow-distribution.
   
    > [!NOTE]
    > Du kan configue Azure AD tooemit antingen hello Azure AD användar-ID (användarens huvudnamn) eller hello e-postadress som hello Unik identifierare i hello SAML-token genom att gå toohello **ServiceNow > attribut > enkel inloggning** avsnitt i Hej klassiska Azure-portalen och mappningen hello önskad fältet toohello **nameidentifier** attribut. hello-värde som lagras för markerade hello-attributet i Azure AD (t.ex. användarens huvudnamn) måste matcha hello-värde som lagras i ServiceNow för hello angivna fält (t.ex. användarnamn)
    > 
    > 
   
    f. Klicka på **Spara**. 

3. Välj hello konfiguration för enkel inloggning bekräftelse på klassiska hello Azure AD-portalen och klicka sedan på **nästa**. 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigurera enkel inloggning")

4. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigurera enkel inloggning")

## <a name="configuring-user-provisioning"></a>Konfigurera användaretablering
hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooServiceNow.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure användaretablering, utför följande steg hello:
1. I hello Azure Management klassiska portalen på hello **ServiceNow** integreringssidan för programmet, klickar du på **konfigurera användaretablering**. 
   
    ![Användaretablering](./media/active-directory-saas-servicenow-tutorial/IC769498.png "användaretablering")

2. På hello **ange ditt ServiceNow autentiseringsuppgifter tooenable automatisk användaretablering** anger hello följande inställningar:
   
     a. I hello **ServiceNow instansnamn** textruta typen hello ServiceNow-instansnamnet.
   
     b. I hello **ServiceNow-administratörsanvändarnamnet** textruta hello-typnamn för hello ServiceNow-administratörskonto.
   
     c. I hello **ServiceNow adminlösenord** textruta typen hello lösenordet för kontot.
   
     d. Klicka på **Validera** tooverify din konfiguration.
   
     e. Klicka på hello **nästa** knappen tooopen hello **nästa steg** sidan.
   
     f. Om du vill tooprovision alla användare toothis program, väljer du ”**automatiskt etablera alla användarkonton i hello directory toothis program**”. 
   
    ![Nästa steg](./media/active-directory-saas-servicenow-tutorial/IC698804.png "nästa steg")
   
     g. På hello **nästa steg** klickar du på **Slutför** toosave din konfiguration.

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
I det här avsnittet skapar du en testanvändare i hello klassiska portalen kallas Britta Simon.

![Skapa Azure AD-användare][20]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    a. Välj ny användare i din organisation som typ av användare.
   
    b. I hello användarnamn **textruta**, typen **BrittaSimon**.
   
    c. Klicka på **Nästa**.

6. På hello **användarprofil** dialogrutan utför hello följande steg:
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   a. I hello **Förnamn** textruta typen **Britta**.  
   
   b. I hello **efternamn** textruta typ, **Simon**.
   
   c. I hello **visningsnamn** textruta typen **Britta Simon**.
   
   d. I hello **rollen** väljer **användaren**.
   
   e. Klicka på **Nästa**.

7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    a. Skriv ned hello värdet för hello **nytt lösenord**.
   
    b. Klicka på **Complete** (Slutför).   

### <a name="creating-a-servicenow-test-user"></a>Skapa en testanvändare ServiceNow
I det här avsnittet skapar du en användare som kallas Britta Simon i ServiceNow. I det här avsnittet skapar du en användare som kallas Britta Simon i ServiceNow. Om du inte vet hur tooadd i ditt ServiceNow eller ServiceNow Express användarkonton, Kontakta supportteamet för ServiceNow.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD
I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooServiceNow sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooServiceNow utför hello följande steg:**

1. På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Tilldela användare][201] 

2. Välj i listan med program hello **ServiceNow**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. Hello-menyn överst hello **användare**.
   
    ![Tilldela användare][203] 

4. Välj i listan för alla användare av hello **Britta Simon**.

5. Klicka i hello verktygsfältet hello längst ned **tilldela**.
   
    ![Tilldela användare][205]

### <a name="testing-single-sign-on"></a>Testa enkel inloggning
hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour ServiceNow programmet när du klickar på hello ServiceNow-panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
