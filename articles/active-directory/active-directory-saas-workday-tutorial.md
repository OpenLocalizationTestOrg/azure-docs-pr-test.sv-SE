---
title: "Självstudier: Azure Active Directory-integrering med Workday | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Workday."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: aaa41e372e45f464c4540a70fc79c98dbc06d6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Självstudier: Azure Active Directory-integrering med Workday

I kursen får du lära dig hur toointegrate Workday med Azure Active Directory (AD Azure).

Integrera Workday med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooWorkday
- Du kan aktivera din användare tooautomatically get inloggade tooWorkday (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med arbetsdagen, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Workday enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Workday från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-workday-from-hello-gallery"></a>Att lägga till Workday från hello-galleriet
tooconfigure hello integrering av arbetsdagen i Azure AD, behöver du tooadd Workday hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Workday från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Workday**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. Markera hello resultat på panelen **Workday**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workday baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Workday är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Workday toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Workday.

tooconfigure och testa Azure AD enkel inloggning med arbetsdagen, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Workday](#creating-a-workday-test-user)**  -toohave en motsvarighet för Britta Simon i Workday som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Workday-program.

**tooconfigure Azure AD enkel inloggning med arbetsdagen, utför hello följande steg:**

1. I hello Azure-portalen på hello **Workday** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. På hello **Workday domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    a. I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE] 
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och Reply-URL. Svars-URL: en måste ha en underdomän till exempel: www wd2, wd3, wd3 impl, wd5, wd5 impl). Med hjälp av något som liknar ”*http://www.myworkday.com*” fungerar men ”*http://myworkday.com*” finns inte. Kontakta [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) tooget dessa värden. 
 

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. På hello **Workday Configuration** klickar du på **konfigurera Workday** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS>
7. Logga in tooyour Workday företagets webbplats som en administratör i en annan webbläsarfönster.

8. Gå för**menyn \> arbetsstationen**.
   
    ![Arbetsstationen](./media/active-directory-saas-workday-tutorial/IC782923.png "arbetsstationen")

9. Gå för**administrationen**.
   
    ![Kontot Administration](./media/active-directory-saas-workday-tutorial/IC782924.png "konto Administration")

10. Gå för**redigera klient inställningar – säkerhet**.
   
    ![Redigera klient säkerhet](./media/active-directory-saas-workday-tutorial/IC782925.png "redigera klient säkerhet")

11. I hello **omdirigering av URL: er** avsnittet, utföra hello följande steg:
   
    ![Omdirigering av URL: er](./media/active-directory-saas-workday-tutorial/IC7829581.png "omdirigerings-URL: er")
   
    a. Klicka på **lägga till raden**.
   
    b. I hello **inloggningen omdirigerings-URL** textruta och hello **Mobile omdirigerings-URL** textruta typen hello **inloggnings-URL** du har angett på hello **Workday domän och URL: er** avsnitt i hello Azure-portalen.
   
    c. I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **Sign-Out URL**, och klistra in den i hello **logga ut omdirigerings-URL** textruta.
   
    d.  I **miljö** textruta typnamn hello miljö.  

    >[!NOTE]
    > hello värdet för attributet för hello-miljö är knutna toohello värdet för hello klient-URL:  
    >-Om hello domännamnet för hello Workday-klient-URL börjar med impl till exempel: *https://impl.workday.com/\<klient\>/login-saml2.htmld*), hello **miljö** attributet måste anges tooImplementation.  
    >-Om hello domännamn startas med ett annat, måste toocontact [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello matchar **miljö** värde.

12. I hello **SAML installationsprogrammet** avsnittet, utföra hello följande steg:
   
    ![SAML-installationsprogrammet](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-installationen")
   
    a.  Välj **aktivera SAML-autentisering**.
   
    b.  Klicka på **lägga till raden**.

13. I hello identitetsleverantörer för SAML-avsnittet, utföra hello följande steg:
   
    ![SAML identitetsleverantörer](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML identitetsleverantörer")
   
    a. Skriv ett providernamn i hello identitet providernamn textruta (till exempel: *SPInitiatedSSO*).
   
    b. I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **SAML enhets-ID** värdet och klistrar in den i hello **utfärdaren** textruta.
   
    c. Välj **aktivera arbetsdagar initierade logga ut**.
   
    d. I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **Sign-Out URL** värdet och klistrar in den i hello **logga ut begäran URL** textruta.

    e. Klicka på **providern offentliga nyckel identitetscertifikat**, och klicka sedan på **skapa**. 

    ![Skapa](./media/active-directory-saas-workday-tutorial/IC782928.png "skapa")

    f. Klicka på **skapa x509 offentliga nyckel**. 

    ![Skapa](./media/active-directory-saas-workday-tutorial/IC782929.png "skapa")


14. I hello **visa x509 offentliga nyckel** avsnittet, utföra hello följande steg: 
   
    ![Visa x509 offentliga nyckel](./media/active-directory-saas-workday-tutorial/IC782930.png "visa x509 offentliga nyckel") 
   
    a. I hello **namn** textruta, ange ett namn för certifikatet (till exempel: *utrustningen\_SP*).
   
    b. I hello **giltigt från** textruta typen hello giltig från attributvärde för ditt certifikat.
   
    c.  I hello **till** textruta TYPVÄRDE hello giltig tooattribute av certifikatet.
   
    > [!NOTE]
    > Du kan hämta hello giltigt från datum och hello giltig toodate från hello hämtat certifikat genom att dubbelklicka på den.  hello datum visas under hello **information** fliken.
    > 
    >
   
    d.  Öppna din Base64-kodade certifikatet i anteckningar och kopiera hello innehållet i den.
   
    e.  I hello **certifikat** textruta klistra in hello innehållet i Urklipp.
   
    f.  Klicka på **OK**.

15. Utför följande steg hello: 
   
    ![Konfiguration av SSO](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO-konfiguration")
   
    a.  Aktivera hello **x509 privata nyckel**.
   
    b.  I hello **Service Provider-ID** textruta typen **http://www.workday.com**.
   
    c.  Välj **aktivera SP initierade SAML-autentisering**.
   
    d.  I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in den i hello **IdP SSO-tjänstens URL** textruta.
   
    e. Välj **inte Deflate SP-initierad autentiseringsbegäran**.
   
    f. Som **begära signatur autentiseringsmetod**väljer **SHA256**. 
   
    ![Autentiseringsmetod begäran signatur](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "autentiseringsmetod begäran signatur") 
   
    g. Klicka på **OK**. 
   
    ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE>

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-workday-test-user"></a>Skapa en arbetsdag testanvändare

tooget en testanvändare etableras i Workday måste toocontact hello [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html).
hello [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) skapar hello användare åt dig.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkday.

![Tilldela användare][200] 

**tooassign Britta Simon tooWorkday utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Workday**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

