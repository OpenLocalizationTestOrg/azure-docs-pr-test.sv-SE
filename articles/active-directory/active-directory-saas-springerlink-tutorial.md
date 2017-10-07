---
title: "Självstudier: Azure Active Directory-integrering med Springer länk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Springer länk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58cdf029-bdc0-43c4-a469-b921c2a669bd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: jeedes
ms.openlocfilehash: dabd2f72b3a195fc359826a4863a197e5019f5c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springer-link"></a>Självstudier: Azure Active Directory-integrering med Springer länk

I kursen får du lära dig hur toointegrate Springer länka med Azure Active Directory (AD Azure).

Integrera Springer länken med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSpringer länk.
- Du kan låta dina användare tooautomatically get inloggade tooSpringer länk (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Springer länk, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En länk Springer enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Ny Springer länk från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-springer-link-from-hello-gallery"></a>Ny Springer länk från hello-galleriet
tooconfigure hello integrering av Springer länken i Azure AD, behöver du tooadd Springer länken hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Springer länk från galleriet hello utför hello följande steg:**

1. I hello ** [Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Springer länken**väljer **Springer länken** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Springer länken i resultatlistan hello](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Springer länk baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Springer länk är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Springer länken toobe upprättas.

Tilldela hello värdet för hello i Springer länken **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Springer länk, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.
4. **[Testa enkel inloggning](#test-single-sign-on) ** -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Springer länk.

**Utför följande hello tooconfigure Azure AD enkel inloggning med Springer länk:**

1. I hello Azure-portalen på hello **Springer länken** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_samlbase.png)

3. På hello **Springer länken domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:

    ![URL: er och springer länken domän med enkel inloggning information](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url1.png)

    a. I hello **identifierare** textruta typen hello URL:`https://fsso.springer.com`

    b. I hello **Reply URL** textruta typen hello URL:`https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`    

4. Kontrollera **visa avancerade inställningar för URL: en**. Om du inte vill tooconfigure hello programmet i **SP** initierade läge:

    ![URL: er och springer länken domän med enkel inloggning information](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url.png)

    I hello **inloggnings-URL** textruta typen hello URL:`https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`    

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-springerlink-tutorial/tutorial_general_400.png)

6. toogenerate hello **Metadata** url, utföra hello följande steg:

    a. Klicka på **App registreringar**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appregistrations.png)
   
    b. Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpointicon.png)

    c. Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpoint.png)
     
    d. Gå nu toohello egenskapssida **Springer länken** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appid.png)

    e. Generera hello **URL för tjänstmetadata** med hello följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

7. tooconfigure enkel inloggning på **Springer länken** sida, behöver du toosend hello genereras **URL för tjänstmetadata** för[Springer länken supportteamet](mailto:identity@springernature.com).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello ** Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-springerlink-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-springerlink-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-springerlink-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-springerlink-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSpringer länk.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooSpringer länken, utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Springer länk**.

    ![hello Springer länken länken i listan med program hello](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Springer länken programmet när du klickar på hello Springer länken panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_203.png

