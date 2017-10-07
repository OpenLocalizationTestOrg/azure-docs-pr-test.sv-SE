---
title: "Självstudier: Azure Active Directory-integrering med Qlik mening Enterprise | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Qlik mening Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 153edb3f8cd41790d4e9a74f15cfb806e5e9e3b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Självstudier: Azure Active Directory-integrering med Qlik mening Enterprise

I kursen får du lära dig hur toointegrate Qlik mening Enterprise med Azure Active Directory (AD Azure).

Integrera Qlik mening Enterprise med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooQlik mening Enterprise.
- Du kan låta dina användare tooautomatically get inloggade tooQlik mening Enterprise (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Qlik mening Enterprise, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Qlik mening Enterprise enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Qlik mening Enterprise från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-qlik-sense-enterprise-from-hello-gallery"></a>Att lägga till Qlik mening Enterprise från hello-galleriet
tooconfigure hello integrering av Qlik mening Enterprise i Azure AD, behöver du tooadd Qlik mening Enterprise hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Qlik mening Enterprise från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Qlik mening Enterprise**väljer **Qlik mening Enterprise** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Qlik mening i resultatlistan hello företag](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Qlik mening Enterprise baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Qlik mening företag är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Qlik mening Enterprise toobe upprättas.

Tilldela hello värdet för hello i Qlik mening Enterprise **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Qlik mening Enterprise, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Qlik mening Enterprise](#create-a-qlik-sense-enterprise-test-user)**  -toohave en motsvarighet för Britta Simon i Qlik mening företag som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Qlik mening Enterprise-programmet.

**tooconfigure Azure AD enkel inloggning med Qlik mening Enterprise, utför hello följande steg:**

1. I hello Azure-portalen på hello **Qlik mening Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. På hello **Qlik mening företagsdomänen och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och Qlik mening företagsdomänen enkel inloggning information](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`
    
    > [!NOTE]
    > Observera hello avslutande snedstreck hello slutet av den här URI. Det krävs.
    
    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare, som beskrivs senare i den här kursen eller kontakta [Qlik mening Enterprise-klienten supportteamet](https://www.qlik.com/us/services/support) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. Förbereda hello Federation Metadata XML-filen så att du kan ladda upp tooQlik mening servern.
   
    > [!NOTE]
    > Innan du laddar upp hello IdP metadata toohello Qlik mening server hello-filen måste redigeras toobe tooremove information tooensure ska fungera korrekt mellan Azure AD och Qlik mening server.
    
    ![QlikSense][qs24]
   
    a. Öppna hello FederationMetaData.xml fil som du har hämtat från Azure-portalen i en textredigerare.
   
    b. Söka efter hello värde **RoleDescriptor**.  Det finns fyra poster (två par inledande och avslutande elementtaggar).
   
    c. Ta bort hello RoleDescriptor taggar och all information mellan från hello-fil.
   
    d. Spara hello-filen och spara den i närheten för användning senare i det här dokumentet.

7. Navigera toohello Qlik meningsfullt Qlik Management Console (QMC) som en användare som kan skapa virtuella proxykonfigurationer.

8. I hello QMC, klickar du på hello **virtuella proxyservrar** menyalternativet.
   
    ![QlikSense][qs6] 

9. Hello ned hello-skärmen, klickar du på hello **Skapa nytt** knappen.
   
    ![QlikSense][qs7]

10. hello visas virtuella proxy redigera.  På hello är höger sida av hello-skärmen en meny för att göra konfigurationsalternativ visas.
   
    ![QlikSense][qs9]

11. Ange hello identifieringsinformation för hello Azure virtuella proxykonfigurationen med hello identifiering kontrolleras kan.
    
    ![QlikSense][qs8]  
    
    a. Hej **beskrivning** fältet är ett eget namn för hello virtuella proxykonfiguration.  Ange ett värde för en beskrivning.
    
    b. Hej **prefixet** identifierar fältet hello virtuella proxy slutpunkt för att ansluta tooQlik mening med Azure AD enkel inloggning.  Ange ett unikt prefix-namn för den här virtuella proxy.
    
    c. **Tidsgräns för inaktivitet (minuter)** är hello tidsgränsen för anslutningar via den här virtuella proxy.
    
    d. Hej **sessionsnamnet cookie-huvud** är hello cookie namn lagra hello sessions-ID för hello Qlik mening sessionen för en användare tar emot efter en lyckad autentisering.  Det här namnet måste vara unikt.

12. Klicka på hello autentisering menyn alternativet toomake den synlig.  hello visas autentisering.
    
    ![QlikSense][qs10]
    
    a. Hej **anonym åtkomstläge** nedrullningsbara avgör om anonyma användare kan komma åt Qlik mening via hello virtuella proxy.  hello standardalternativet är ingen anonym användare.
    
    b. Hej **autentiseringsmetod** nedrullningsbara styr hello autentisering schemat hello virtuella proxy används.  Välj SAML hello nedrullningsbara listan.  Fler alternativ visas som ett resultat.
    
    c. I hello **SAML URI värdfältet**, inkommande hello hostname användare ange tooaccess Qlik mening via den här virtuella SAML-proxyn.  hello värdnamn är hello uri för hello Qlik mening server.
    
    d. I hello **SAML enhets-ID**, ange hello samma värde som angetts för hello SAML värdfältet URI.
    
    e. Hej **SAML IdP metadata** är hello fil redigeras tidigare i hello **redigera Federation Metadata från Azure AD-konfiguration** avsnitt.  **Innan du laddar upp hello IdP metadata hello-filen måste redigeras toobe** tooremove information tooensure ska fungera korrekt mellan Azure AD och Qlik mening server.  **Se toohello anvisningarna ovan om hello-filen har ännu toobe redigeras.**  Om hello-filen har redigerats klickar du på hello Bläddra och välj hello redigerade metadata filen tooupload den toohello virtuella proxykonfiguration.
    
    f. Ange hello namn eller schemat referens för hello SAML-attributet som representerar hello **UserID** Azure AD skickar toohello Qlik mening server.  Referensinformation för schemat finns i hello Azure-app skärmar efter konfiguration.  namnattributet för toouse hello ange `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    g. Ange hello värde för hello **användarkatalog** som ska vara anslutna toousers när de autentiserar tooQlik mening servern via Azure AD.  Hårdkodad värden måste omges av **hakparentes []**.  toouse ett attribut som skickas i hello Azure AD SAML assertion ange hello hello attributets namn i den här textrutan **utan** hakparenteser.
    
    h. Hej **SAML Signeringsalgoritm** anger hello service provider (i det här fallet Qlik mening server) om certifikatsignering för hello virtuella proxykonfiguration.  Om Qlik mening servern använder ett betrott certifikat som genereras med hjälp av Microsoft Enhanced RSA och AES Cryptographic Provider, ändra hello SAML Signeringsalgoritm för**SHA-256**.
    
    Jag. hello SAML attributet mappning avsnittet tillåter ytterligare attribut som grupper toobe skickas tooQlik mening för användning i säkerhetsregler.

13. Klicka på hello **BELASTNINGSUTJÄMNING** menyn alternativet toomake den synlig.  hello visas belastningsutjämning.
    
    ![QlikSense][qs11]

14. Klicka på hello **Lägg till ny servernoden** knappen Välj motorn nod eller noder Qlik mening ska skicka sessioner toofor belastningsutjämning ändamål och på hello **Lägg till** knappen.
    
    ![QlikSense][qs12]

15. Klicka på hello avancerade menyn alternativet toomake den synlig. Avancerade hello-skärmen visas.
    
    ![QlikSense][qs13]
    
    hello värden vitlista identifierar värdnamn som accepteras när du ansluter toohello Qlik mening server.  **Ange hello värdnamn användarna anger när du ansluter tooQlik mening server.** hello-värdnamnet är hello samma värde som hello uri för SAML-värden utan hello https://.

16. Klicka på hello **tillämpa** knappen.
    
    ![QlikSense][qs14]

17. Klicka på OK tooaccept hello varning om proxyservrar länkade toohello virtuella proxy kommer att startas om.
    
    ![QlikSense][qs15]

18. Hello höger på hello-skärmen visas hello associerade objekt.  Klicka på hello **proxyservrar** menyalternativet.
    
    ![QlikSense][qs16]

19. hello visas proxy.  Klicka på hello **länk** längst hello nedre toolink proxy toohello virtuella proxy.
    
    ![QlikSense][qs17]

20. Välj hello proxy-nod som stöd för den här virtuella proxyanslutningen och på hello **länk** knappen.  När du länkar visas hello proxy under associerade proxyservrar.
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. Efter fem tooten sekunder hello uppdatera QMC meddelandet visas.  Klicka på hello **uppdatera QMC** knappen.
    
    ![QlikSense][qs20]

22. När hello QMC uppdaterar, klicka på hello **virtuella proxyservrar** menyalternativet. hello ny SAML virtuella proxy post anges i hello tabell på hello-skärmen.  Klicka på hello virtuella proxy post.
    
    ![QlikSense][qs51]

23. Längst ned hello hello-skärmen aktiveras hello hämta SP metadata knappen.  Klicka på hello **hämta SP metadata** tooa för knappen toosave hello metadatafil.
    
    ![QlikSense][qs52]

24. Öppna hello sp-metadatafil.  Se hello **ID för entiteterna** transaktionen och hello **AssertionConsumerService** transaktionen.  Dessa värden är likvärdig toohello **identifierare** och hello **inloggning URL** i programkonfigurationen hello Azure AD. Klistra in dessa värden i hello **Qlik mening företagsdomänen och URL: er** avsnitt i hello Azure AD-programkonfigurationen om de matchar inte varandra, och sedan bör du ersätta dem i konfigurationsguiden för hello Azure AD-App.
    
    ![QlikSense][qs53]

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

   ![hello Azure Active Directory-knappen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

   ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

   ![hello webbinställningar](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

   ![hello användardialogrutan](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. I hello **namn** skriver **BrittaSimon**.

   b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

   c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

   d. Klicka på **Skapa**.
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a>Skapa en testanvändare Qlik mening Enterprise

I det här avsnittet kan du skapa en användare som kallas Britta Simon i Qlik mening Enterprise. Arbeta med [Qlik mening Enterprise-klienten supportteamet](https://www.qlik.com/us/services/support) att lägga till hello användare i hello Qlik mening Enterprise-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning. 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooQlik mening Enterprise.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooQlik mening Enterprise, utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Qlik mening Enterprise**.

    ![Hej Qlik mening Enterprise länken i listan med program hello](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Qlik mening Enterprise-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Qlik mening företagsprogram. 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

