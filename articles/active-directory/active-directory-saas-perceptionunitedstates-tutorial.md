---
title: "Självstudier: Azure Active Directory-integrering med uppfattning USA (icke-UltiPro) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och uppfattning USA (icke-UltiPro)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 874b5da277b9c68504c4af2ac87ed90d2bbd93b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Självstudier: Azure Active Directory-integrering med uppfattning USA (icke-UltiPro)

I kursen får du lära dig hur toointegrate uppfattning USA (icke-UltiPro) med Azure Active Directory (AD Azure).

Integrera uppfattning USA (icke-UltiPro) med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooPerception USA (icke-UltiPro).
- Du kan låta dina användare tooautomatically get inloggade tooPerception USA (icke-UltiPro) (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med uppfattning USA (icke-UltiPro), behöver du hello följande objekt:

- En Azure AD-prenumeration
- En uppfattning USA (icke-UltiPro) enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till uppfattning USA (icke-UltiPro) från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-perception-united-states-non-ultipro-from-hello-gallery"></a>Att lägga till uppfattning USA (icke-UltiPro) från hello-galleriet
tooconfigure hello integrering av uppfattning USA (icke-UltiPro) i Azure AD, behöver du tooadd uppfattning USA (icke-UltiPro) från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd uppfattning USA (icke-UltiPro) från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **uppfattning USA (icke-UltiPro)**väljer **uppfattning USA (icke-UltiPro)** resultatet-panelen klickar **Lägg till** knappen tooadd hello-program.

    ![Uppfattning USA (icke-UltiPro) i hello resultatlistan](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med uppfattning USA (icke-UltiPro) baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i uppfattning USA (icke-UltiPro) är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i uppfattning USA (icke-UltiPro) toobe upprättas.

I uppfattning USA (icke-UltiPro), tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med uppfattning USA (icke-UltiPro), behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare uppfattning USA (icke-UltiPro)](#create-a-perception-united-states-non-ultipro-test-user)**  -toohave en motsvarighet för Britta Simon i uppfattning USA (icke-UltiPro) som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet uppfattning USA (icke-UltiPro).

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med uppfattning USA (icke-UltiPro):**

1. I hello Azure-portalen på hello **uppfattning USA (icke-UltiPro)** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. På hello **domänen uppfattning USA (icke-UltiPro) och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och uppfattning USA (icke-UltiPro)-domän med enkel inloggning information](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    a. I hello **identifierare** textruta typen hello URL:`https://perception.kanjoya.com/sp`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://perception.kanjoya.com/sso?idp=<entity_id>`

    > [!NOTE] 
    > hello-värdet är inte verkliga. Hello värdet uppdateras med hello faktiska Reply URL, vilket beskrivs senare i hello kursen.
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. På hello **uppfattning USA (icke-UltiPro) konfiguration** klickar du på **konfigurera uppfattning USA (icke-UltiPro)** tooopen **konfigurera inloggning** fönstret. Kopiera hello **SAML enhets-ID** från hello **Snabbreferens avsnitt.**

    a. Hej **uppfattning USA (icke-UltiPro)** programmet kräver hello **SAML enhets-ID** -värde som du har kopierat toobe uri-kodad. tooget hello uri-kodad värde, Använd hello följande länk:**http://www.url-encode-decode.com/**.

    b. När du har skaffat hello uri kombinera det procentkodade värdet med hello **Reply URL** som anges nedan -

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    c. Klistra in hello större än värdet i hello **Reply URL** TextBox-kontroll i **domänen uppfattning USA (icke-UltiPro) och URL: er** avsnitt.

    ![Uppfattning USA (icke-UltiPro) konfiguration](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. Logga in tooyour uppfattning USA (icke-UltiPro) företagets webbplats som en administratör i ett nytt webbläsarfönster.

8. I hello verktygsfältet klickar du på **kontoinställningar**.

    ![Uppfattning USA (icke-UltiPro) användare](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. På hello **kontoinställningar** utför hello följande steg:

    ![Uppfattning USA (icke-UltiPro) användare](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. I hello **företagsnamn** textruta hello-typnamn för hello **företagets**.
    
    b. I hello **kontonamn** textruta hello-typnamn för hello **konto**.

    c. I **standard Reply-tooEmail** textruta, typen hello giltig **e-post**.

    d. Välj **SSO identitetsleverantör** som **SAML 2.0**.

10. På hello **SSO Configuration** utför hello följande steg:

    ![Uppfattning USA (icke-UltiPro) SSOConfig](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Välj **SAML NameID typen** som **e-post**.

    b. I hello **SSO Konfigurationsnamnet** textruta hello-typnamn för din **Configuration**.
    
    c. I **identitet providernamn** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen. 

    d. I **SAML domän textruta**, ange hello-domän som  **@contoso.com** .

    e. Klicka på **överför igen** tooupload hello **XML-Metadata för** fil.

    f. Klicka på **uppdatering**.


> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a>Skapa en testanvändare uppfattning USA (icke-UltiPro)

I det här avsnittet skapar du en användare som kallas Britta Simon uppfattning United States (icke-UltiPro). Arbeta med [uppfattning USA (icke-UltiPro) supportteamet](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello användare i hello uppfattning USA (icke-UltiPro)-plattformen.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPerception USA (icke-UltiPro).

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooPerception USA (icke-UltiPro), utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **uppfattning USA (icke-UltiPro)**.

    ![hello uppfattning USA (icke-UltiPro) länken i listan med program hello](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello uppfattning USA (icke-UltiPro)-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour uppfattning USA (icke-UltiPro) program.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

