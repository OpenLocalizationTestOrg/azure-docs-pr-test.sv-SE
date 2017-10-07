---
title: "Självstudier: Azure Active Directory-integrering med zoomning | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och zoomning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 623a1f428ad1f0aa2c8205b79d61720cad5fc6a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Självstudier: Azure Active Directory-integrering med zoomning

I kursen får du lära dig hur toointegrate Zooma med Azure Active Directory (AD Azure).

Integrera zoomning med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooZoom.
- Du kan låta dina användare tooautomatically get inloggade tooZoom (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med zoomning, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En zoomning enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till zoomning från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-zoom-from-hello-gallery"></a>Att lägga till zoomning från hello-galleriet
tooconfigure hello-integration för zoomning till Azure AD, behöver du tooadd zoomning hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd zoomning från galleriet hello utför hello följande steg:**

1. I hello ** [Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Zooma**väljer **Zooma** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Zooma in hello resultatlistan](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med zoomning baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i zoomning är tooa i Azure AD. Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i zoomning behov toobe upprättas.

Tilldela hello värdet för hello i zoomningsnivå **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med zoomning, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare zoomning](#create-a-zoom-test-user) ** -toohave en motsvarighet för Britta Simon i zoomning som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on) ** -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för zoomning.

**tooconfigure Azure AD enkel inloggning med zoomning, utför följande steg hello:**

1. I hello Azure-portalen på hello **Zooma** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_samlbase.png)

3. På hello **URL: er och zooma domän** avsnittet, utföra hello följande steg:

    ![URL: er och zoomning domän med enkel inloggning information](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.zoom.us`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.zoom.us`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Zooma klienten supportteamet](https://support.zoom.us/hc) tooget dessa värden. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-zoom-tutorial/tutorial_general_400.png)

6. På hello **Zooma Configuration** klickar du på **konfigurera Zooma** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Zooma konfiguration](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_configure.png) 

7. Logga in tooyour zoomning företagets webbplats som en administratör i en annan webbläsarfönster.

8. Klicka på hello **enkel inloggning** fliken.
   
    ![Enkel inloggning fliken](./media/active-directory-saas-zoom-tutorial/IC784700.png "enkel inloggning")

9. Klicka på hello **säkerhetskontroll** fliken och gå sedan toohello **enkel inloggning** inställningar.

10. I hello Single Sign-On-avsnittet, utföra hello följande steg:
   
    ![Enkel inloggning avsnittet](./media/active-directory-saas-zoom-tutorial/IC784701.png "enkel inloggning")
   
    a. I hello **inloggning Sidadress** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.
   
    b. I hello **URL för utloggning** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.
     
    c. Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **providern identitetscertifikat** textruta.

    d. I hello **utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen. 

    e. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello ** Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-zoom-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-zoom-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-zoom-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-zoom-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-zoom-test-user"></a>Skapa en testanvändare zoomning

I ordning tooenable Azure AD-användare toolog i tooZoom, måste de etableras i zoomning. Hello gäller zoomning är etablering en manuell aktivitet.

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision ett användarkonto, utför följande steg hello:

1. Logga in tooyour **Zooma** företagets webbplats som administratör.
 
2. Klicka på hello **kontohantering** fliken och klicka sedan på **Användarhantering**.

3. I hello Användarhantering avsnittet, klickar du på **lägga till användare**.
   
    ![Användarhantering](./media/active-directory-saas-zoom-tutorial/IC784703.png "användarhantering")

4. På hello **lägga till användare** utför hello följande steg:
   
    ![Lägg till användare](./media/active-directory-saas-zoom-tutorial/IC784704.png "lägga till användare")
   
    a. Som **användartyp**väljer **grundläggande**.

    b. I hello **e-postmeddelanden** textruta typen hello e-postadressen till ett giltigt Azure AD-kontot som du vill tooprovision.

    c. Klicka på **Lägg till**.

> [!NOTE]
> Du kan använda något annat zoomning användarens konto skapas verktyg eller API: er som tillhandahålls av zoomning tooprovision Azure Active Directory användarkonton.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZoom.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooZoom utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Zooma**.

    ![hello zoomning länken i listan med program hello](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour zoomning programmet när du klickar på hello Zoom-panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_203.png

