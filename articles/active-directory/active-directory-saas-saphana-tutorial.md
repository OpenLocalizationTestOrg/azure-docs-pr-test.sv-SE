---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP HANA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 5ff6bfde0b1d7ab14025a4bc45199f9f826affd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Självstudier: Azure Active Directory-integrering med SAP HANA

I kursen får du lära dig hur toointegrate SAP HANA med Azure Active Directory (AD Azure).

Integrera SAP HANA med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSAP HANA
- Du kan aktivera din användare tooautomatically get inloggade tooSAP HANA (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SAP HANA måste hello följande objekt:

- En Azure AD-prenumeration
- En SAP HANA enkel inloggning aktiverad prenumeration
- Körs HANA instansen antingen på alla offentliga IaaS lokalt, virtuella Azure-datorer eller stora SAP-instanser i Azure
- Hej XSA Administration webbgränssnitt samt HANA Studio installerat på hello HANA instans.

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö för SAP HANA. Testa hello integration först i utvecklings- eller mellanlagring av programmet hello-miljön och Använd hello-produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SAP HANA från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sap-hana-from-hello-gallery"></a>Att lägga till SAP HANA från hello-galleriet
tooconfigure hello integrering av SAP HANA i Azure AD, behöver du tooadd SAP HANA hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SAP HANA från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **SAP HANA**väljer **SAP HANA** resultatet-panelen klickar **Lägg till** knappen tooadd hello program. 

    ![hello nytt program](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP HANA baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SAP HANA är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SAP HANA toobe upprättas.

Tilldela hello värdet för hello i SAP HANA **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med SAP HANA, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare för SAP HANA](#creating-a-sap-hana-test-user)**  -toohave en motsvarighet för Britta Simon i SAP HANA som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för SAP HANA.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SAP HANA:**

1. I hello Azure-portalen på hello **SAP HANA** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. På hello **SAP HANA domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och domän med enkel inloggning information](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. I hello **identifierare** textruta typ som:`HA100` 

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [SAP HANA klienten supportteamet](https://cloudplatform.sap.com/contact.html) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Om certifikatet inte är aktiv sedan aktivera den genom att klicka på hello ”kontrollera nya certifikat active” kryssrutan i hello Azure AD. 

5. SAP HANA program förväntar hello SAML intyg i ett specifikt format. hello följande skärmbild visar ett exempel för det här. Vi har här mappat hello **användar-ID** med **ExtractMailPrefix()** funktion **user.mail**. Detta ger hello prefixvärdet av e-post för hello-användare som är hello unikt användar-ID. Toohello SAP HANA-programmet skickas i varje lyckat svar.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan:

    a. I hello **användar-ID** listrutan, Välj **ExtractMailPrefix**.
    
    b. I hello **e** listrutan, Välj **user.mail**.

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. tooconfigure enkel inloggning på **SAP HANA** sida, inloggning tooyour **HANA XSA webbkonsolen** genom att bläddra toohello respektive HTTPS-slutpunkt.

    > [!Note]
    > I standardkonfigurationen hello omdirigerar hello URL hello begäran tooa inloggningsskärm, vilket kräver hello autentiseringsuppgifterna för ett autentiserat SAP HANA-databas toocomplete hello inloggningsprocessen. hello-användare som loggar in måste ha hello privilegier krävs tooperform SAML administrationsuppgifter.

9. Hello XSA Web Interface, navigera för**SAML-identitetsprovider** och därifrån klickar du på hello **”+”** -knappen på hello längst ned på hello skärmen toodisplay hello lägga till information om identitet rutan och utföra Hej följande steg:

    ![Lägg till identitetsleverantören.](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. I hello **lägga till information om identitet** rutan klistra in hello innehållet i hello XML-Metadata, som du har hämtat från Azure-portalen i hello **Metadata** textruta.

    ![Lägg till inställningar för identitetsleverantören.](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Om hello innehållet i XML-dokumentet för hello är giltiga hello vid parsning av process extraherar hello information som krävs för tooinsert till hello **ämne, enhets-ID och utfärdaren** fälten i hello allmänna Data Skrivbordsstorlek och hello URL fält i hello Skärmen målområdet, till exempel  **Base Webbadressen och SingleSignOn (*)**.

    ![Lägg till inställningar för identitetsleverantören.](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. Hello namnrutan för hello allmänna Data Skrivbordsstorlek, ange ett namn för hello ny SAML SSO identitetsleverantör.

    > [!Note]
    > hello SAML IDP hello namn är obligatoriskt och måste vara unikt. den visas i hello listan med tillgängliga SAML-IDPs som visas, om du väljer SAML som hello autentiseringsmetod för SAP HANA XS program toouse, till exempel i hello Skrivbordsstorlek för autentisering av hello XS artefakt administrationsverktyget.

10. Spara hello information om hello ny SAML-identitetsprovider. Välj **spara** toosave hello information om hello SAML-identitetsprovider och Lägg till hello nya SAML IDP toohello listan över kända SAML IDPs.

    ![Knappen Spara](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. I HANA Studio i hello Systemegenskaper hello **Configuration** fliken, bara filtrera inställningar av **saml** och justera hello **assertion_timeout** från **10 sek** för**120 sek**.

    ![assertion_timeout inställning](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![hello webbinställningar](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![hello användardialogrutan](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-sap-hana-test-user"></a>Skapa en SAP HANA testanvändare

tooenable Azure AD-användare toolog i tooSAP HANA, måste de etableras till SAP HANA.
SAP HANA stöder just-in-time-etablering, vilket är aktiverat som standard.

Om du behöver toocreate en användare manuellt utför hello följande steg:

>[!Note]
>Du kan ändra hello extern autentisering som används av hello användare.
Externa användare autentiseras med hjälp av ett externt system, till exempel Kerberos-system. Detaljerad information om externa identiteter, kontakta din [domänadministratören](https://cloudplatform.sap.com/contact.html).

1. Öppna hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) som administratör och aktivera hello DB-användare för SAML SSO.

    ![Skapa användare](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Skalstreck hello osynliga kryssrutan toohello till vänster i **SAML** och hello konfigurera länken.

3. Klicka på **Lägg till** tooadd hello SAML IDP och klicka på **OK** välja hello lämpliga SAML IDP.

4. Lägg till hello **externa identitet** (t.ex. Här BrittaSimon) eller välj **”alla”** och på **OK**.

    >[!Note]
    >Om ”alla” kryssrutan inte är markerad och sedan hello användarnamnet i HANA måste tooexactly matchar hello namnet på hello användare i hello UPN innan hello domänsuffix (d.v.s. BrittaSimon@contoso.com skulle bli BrittaSimon i HANA).

5. I testsyfte kan du tilldela alla **”XS”** roller toohello användare.

    ![tilldela roller](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > Du bör ge de behörigheterna som är lämpliga för din användningsfall endast.

6. Spara hello användare.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAP HANA.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooSAP HANA, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SAP HANA**.

    ![Tilldela användare](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello SAP HANA-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour SAP HANA-programmet.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

