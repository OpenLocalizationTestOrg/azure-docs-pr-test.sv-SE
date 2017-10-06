---
title: "Självstudier: Azure Active Directory-integrering med antal samverkande SAML SSO av Microsoft | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och växer samman SAML SSO av Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ace23800e3908c8125052b4a2edcaae71f19935d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Självstudier: Azure Active Directory-integrering med antal samverkande SAML SSO av Microsoft

I kursen får du lära dig hur toointegrate växer samman SAML SSO av Microsoft med Azure Active Directory (AD Azure).

Integrera växer samman SAML SSO av Microsoft med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooConfluence SAML SSO av Microsoft
- Du kan aktivera din användare tooautomatically get inloggade tooConfluence SAML SSO av Microsoft (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med antal samverkande SAML SSO av Microsoft, behöver du hello följande objekt:

- En Azure AD-prenumeration
- Antal samverkande server installerat program på en Windows 64-bitars server (lokalt eller på hello molnet IaaS-infrastruktur)
- Antal samverkande servern är HTTPS-aktiverade
- Obs hello stöds versioner för antal samverkande plugin-programmet enligt under avsnittet.
- Antal samverkande servern kan nås på internet särskilt tooAzure AD inloggningen sidan för verifiering och ska kunna tooreceive hello-token från Azure AD
- Admin-autentiseringsuppgifter anges i antal samverkande
- WebSudo har inaktiverats i antal samverkande
- Testanvändare som skapats i hello växer samman serverprogram

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö för växer samman. Testa hello integration först i utvecklings- eller mellanlagring av programmet hello-miljön och Använd hello-produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Antal samverkande versioner som stöds 

Från och med nu stöds följande antal samverkande-versioner:

- Antal samverkande: 5.0 too5.10

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till växer samman SAML SSO av Microsoft från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-confluence-saml-sso-by-microsoft-from-hello-gallery"></a>Att lägga till växer samman SAML SSO av Microsoft från hello-galleriet
tooconfigure hello-integrering av antal samverkande SAML SSO av Microsoft Azure AD, behöver du tooadd växer samman SAML SSO av Microsoft hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd växer samman SAML SSO av Microsoft från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **växer samman SAML SSO av Microsoft**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. Markera hello resultat på panelen **växer samman SAML SSO av Microsoft**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren växer samman SAML SSO av Microsoft är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren växer samman SAML SSO av Microsoft toobe upprättas.

Antal samverkande SAML SSO av Microsoft, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa ett antal samverkande SAML SSO av Microsoft testanvändare](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  -toohave en motsvarighet för Britta Simon växer samman SAML SSO från Microsoft som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din växer samman SAML SSO av Microsoft-program.

**tooconfigure Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft, utför följande steg hello:**

1. I hello Azure-portalen på hello **växer samman SAML SSO av Microsoft** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. På hello **växer samman SAML SSO av URL: er och Microsoft Domain** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/plugins/servlet/saml/auth`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/`

    c. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Porten är valfria om det är en namngiven URL. Dessa värden tas emot under hello konfigurationen av antal samverkande plugin som beskrivs senare i hello kursen.
 

4. toogenerate hello **Metadata** url, utföra hello följande steg:

    a. Klicka på **App registreringar**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    b. Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    c. Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    d. Gå nu toohello egenskapssida **växer samman SAML SSO av Microsoft** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    e. Generera hello **URL för tjänstmetadata** med hello följer mönstret: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` och kopiera det här värdet i anteckningar eftersom den används senare för att konfigurera hello hello plugin-programmet.

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. Kontakta [Microsoft](mailto:waadpartners@microsoft.com) med hello följande information för hello växer samman plugin-programmet.
    
    *   Kundnamn:
    *   Namn på primär domän:
    *   Azure AD Premium: Ja/Nej (plugin-programmet blir tillgängliga tooall hello kund lediga, grundläggande och Premium-SKU)
    *   Antalet användare som kommer att använda den här integreringen:
    *   Antal samverkande Version:
    *   Kommentarer:

7. Logga in tooyour växer samman instans som en administratör i en annan webbläsarfönster.

8. Hovra över kugge och klicka på hello **tillägg**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

9. Klicka under tillägg fliken avsnitt **Hantera tillägg**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon72.png)

10. Ladda upp hello plugin-program som tillhandahålls av Microsoft manuellt. När hello plugin-programmet installeras, visas det i **användaren installerat** tillägg avsnitt i **Hantera tillägg** avsnitt.

11. Klicka på **konfigurera** tooconfigure hello nytt plugin-program.

12. Utför följande steg på konfigurationssidan:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon5.png)
 
    a. I **URL för tjänstmetadata** klistra in hello **URL för tjänstmetadata** genereras från Azure AD och klicka på hello **lösa** knappen. Den läser hello IdP metadata-URL och fyller i informationen för alla hello-fält.

    > [!Note]
    > Standardplatsen för SAML användar-ID är namnidentifierare. Du kan ändra det här attributet tooan-alternativet och ange hello lämpliga attributets namn.

    > [!TIP]
    > Kontrollera att det finns bara ett certifikat mappas mot hello app så att det finns inga fel vid matchning av hello metadata. Om det finns flera certifikat på lösa hello metadata får administratör ett fel.
    
    b. Kopiera hello **identifierare, svars-URL: en och URL: en inloggning** värden och klistra in dem i **identifierare, svars-URL: en och URL: en inloggning** textrutor respektive i **växer samman SAML SSO av URL: er och Microsoft Domain**  avsnitt på Azure-portalen.

    c. I **knappen inloggningsnamnet** hello typnamn knappen organisationen vill hello användare toosee på inloggningsskärmen.

    d. I **SAML användar-ID platser** väljer du antingen **användar-ID är i hello NameIdentifier elementet i hello ämne instruktionen** eller **användar-ID är i ett element med attributet**.  Detta ID har toobe hello växer samman användar-id. Om hello användar-id inte matchas sedan kan inte användare toolog i. 
    
    e. Om du väljer **användar-ID är i ett element med attributet** alternativet i **attributnamn** textruta hello-typnamn för hello-attribut som där användar-Id förväntas. 

    f. Om du använder hello federerade domänen (t.ex. AD FS etc.) med Azure AD, klicka på hello **aktivera identifiering av startsfär** och konfigurera hello **domännamn**.
    
    g. I **domännamn** typen hello domännamn här vid hello ADFS-baserade inloggningen.

    h. Kontrollera **aktivera enkel inloggning ut** om du vill toolog ut från Azure AD när en användare loggar från växer samman. 

    Jag. Klicka på **spara** knappen toosave hello inställningar.


> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Skapa ett antal samverkande SAML SSO av Microsoft testanvändare

tooenable Azure AD-användare toolog i tooConfluence på lokal server de måste etableras i antal samverkande SAML SSO av Microsoft. Antal samverkande SAML SSO av Microsoft är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour växer samman på lokal server som administratör.

2. Hovra över kugge och klicka på hello **Användarhantering**.

    ![Lägga till medarbetare](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. Under avsnittet för användare, klickar du på **lägga till användare** fliken. På hello **”Lägg till en användare”** dialogrutan utför hello följande steg:

    ![Lägga till medarbetare](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    a. I hello **användarnamn** textruta hello e-post för användare som Britta Simon.

    b. I hello **fullständiga namn** textruta typen hello användarens fullständiga namn som Britta Simon.

    c. I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.

    d. I hello **lösenord** textruta hello lösenordstyp för Britta Simon.

    e. Klicka på **Bekräfta lösenord** hello lösenord.
    
    f. Klicka på **Lägg till** knappen.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooConfluence SAML SSO av Microsoft.

![Tilldela användare][200] 

**tooassign Britta Simon tooConfluence SAML SSO av Microsoft, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **växer samman SAML SSO av Microsoft**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello växer samman SAML SSO av Microsoft-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour växer samman SAML SSO av Microsoft-program.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

