---
title: "Självstudier: Azure Active Directory-integrering med MOVEit överföring - integrering med Azure AD | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och MOVEit överföringen - integrering med Azure AD."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 5bbe4f2d952bd45c4d58d55ffc3467b4eb871fd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Självstudier: Azure Active Directory-integrering med MOVEit överföring - Azure AD-integrering

I kursen får du lära dig hur toointegrate MOVEit överföring - Azure AD-integrering med Azure Active Directory (AD Azure).

Integrera MOVEit överföring - Azure AD-integrering med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooMOVEit överföring - integrering med Azure AD.
- Du kan låta dina användare tooautomatically get inloggade tooMOVEit överföring - Azure AD-integrering (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med MOVEit överföring - Azure AD-integrering måste hello följande objekt:

- En Azure AD-prenumeration
- En MOVEit överföring - Azure AD-integrering enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till MOVEit överföring - integrering med Azure AD från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-moveit-transfer---azure-ad-integration-from-hello-gallery"></a>Lägga till MOVEit överföring - integrering med Azure AD från hello-galleriet
tooconfigure hello integrering av MOVEit överföring - Azure AD-integrering i Azure AD, behöver du tooadd MOVEit överföring - Azure AD-integrering hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd MOVEit överföring - integrering med Azure AD från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **MOVEit överföring - integrering med Azure AD**väljer **MOVEit överföring - integrering med Azure AD** resultatet-panelen klickar **Lägg till** knappen tooadd hello programmet.

    ![MOVEit överföring - Azure AD-integrering i hello resultatlistan](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med MOVEit överföring - integrering med Azure AD utifrån en testanvändare som kallas ”Britta Simon”.

Azure AD måste tooknow vilken hello motsvarighet användare i MOVEit överföring för enkel inloggning toowork - Azure AD-integrering är tooa användare i Azure AD. Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i MOVEit överföring - Azure AD-integrering måste toobe upprättas.

Tilldela hello värdet för hello i MOVEit överföring - Azure AD-integrering **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med MOVEit överföring - Azure AD-integrering måste toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en MOVEit överföring - Azure AD-integrering testanvändare](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - toohave en motsvarighet för Britta Simon i MOVEit överföring - Azure AD-integrering som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din MOVEit överföring - program för Azure AD-integrering.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med MOVEit överföring - Azure AD-integrering:**

1. I hello Azure-portalen på hello **MOVEit överföring - integrering med Azure AD** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. På hello **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://contoso.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://contoso.com/<tenatid>`

    c. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL. Du kan se dessa värden senare i **URL för tjänstmetadata providern** avsnitt eller kontakta [MOVEit överföring - supportteamet för Azure AD-integrering klienten](https://community.ipswitch.com/s/support) tooget dessa värden.

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. Logga in tooyour MOVEit överföring klient som administratör.

7. Klicka på hello vänstra navigeringsfönstret **inställningar**.

    ![Inställningar för avsnittet på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. Klicka på **enkel inloggning** länk som är under **principer -> användaren Auth**.

    ![Säkerhet principer på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. Klicka på hello URL för tjänstmetadata länken toodownload hello metadata dokument.

    ![URL för tjänstmetadata Provider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Kontrollera **ID för entiteterna** matchar **identifierare** i hello **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnitt.
    * Kontrollera **AssertionConsumerService** -URL: en matchar **REPLY URL** i hello **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnitt.
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. Klicka på **lägga till identitetsleverantör** knappen tooadd en ny Provider för federerad identitet.

    ![Lägg till identitetsleverantören.](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. Klicka på **Bläddra...**  tooselect hello metadatafil som du laddade ned från Azure-portalen och klicka sedan på **lägga till identitetsleverantör** tooupload hello ned filen.

    ![SAML-identitetsprovider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. Välj ”**Ja**” som **aktiverad** i hello **redigera federerad identitet providerinställningar...**  och klickar på **spara**.

    ![Federerade identiteter providerinställningar](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. I hello **redigera federerad identitet providern användarinställningar** utför hello följande åtgärder:
    
    ![Redigera inställningar för federerad identitet-Provider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Välj **SAML NameID** som **inloggningsnamnet**.
    
    b. Välj **andra** som **fullständiga namn** och i hello **attributnamn** textruta placera hello värde: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Välj **andra** som **e-post** och i hello **attributnamn** textruta placera hello värde: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Välj **Ja** som **skapa automatiskt konto på inloggning**.
    
    e. Klicka på **spara** knappen.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>Skapa en MOVEit överföring - testanvändare i Azure AD-integrering

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i MOVEit överföring - integrering med Azure AD. MOVEit överföring - integrering med Azure AD stöder just-in-time-allokering som du har aktiverat. Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas under ett försök tooaccess MOVEit överföring - Azure AD-integrering om den inte finns.

>[!NOTE]
>Om du behöver toocreate en användare manuellt, måste toocontact hello [MOVEit överföring - supportteamet för Azure AD-integrering klienten](https://community.ipswitch.com/s/support).

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMOVEit överföring - integrering med Azure AD.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooMOVEit överföring - Azure AD-integrering utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **MOVEit överföring - integrering med Azure AD**.

    ![Hej MOVEit överföring - Azure AD-integrering länken i listan med program hello](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.

När du klickar på hello MOVEit överföring - panelen i Azure AD-integrering hello åtkomstpanelen får automatiskt inloggade tooyour MOVEit överföring - program för Azure AD-integrering. 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

