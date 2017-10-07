---
title: "Självstudier: Azure Active Directory-integrering med ScreenSteps | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ScreenSteps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: fd041e5fe4552727eeda2dabc1773d8043d410f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Självstudier: Azure Active Directory-integrering med ScreenSteps

I kursen får du lära dig hur toointegrate ScreenSteps med Azure Active Directory (AD Azure).

Integrera ScreenSteps med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooScreenSteps.
- Du kan låta dina användare tooautomatically get inloggade tooScreenSteps (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med ScreenSteps, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En ScreenSteps enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till ScreenSteps från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-screensteps-from-hello-gallery"></a>Att lägga till ScreenSteps från hello-galleriet
tooconfigure hello integrering av ScreenSteps i Azure AD, behöver du tooadd ScreenSteps hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd ScreenSteps från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **ScreenSteps**väljer **ScreenSteps** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![ScreenSteps i hello resultatlistan](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ScreenSteps baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ScreenSteps är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ScreenSteps toobe upprättas.

I ScreenSteps, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med ScreenSteps, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare ScreenSteps](#create-a-screensteps-test-user)**  -toohave en motsvarighet för Britta Simon i ScreenSteps som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt ScreenSteps program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ScreenSteps:**

1. I hello Azure-portalen på hello **ScreenSteps** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. På hello **ScreenSteps domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och ScreenSteps domän med enkel inloggning information](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.ScreenSteps.com`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL, som beskrivs senare i den här kursen. 

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. På hello **ScreenSteps Configuration** klickar du på **konfigurera ScreenSteps** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**

    ![ScreenSteps konfiguration](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. Logga in på webbplatsen ScreenSteps företag som en administratör i en annan webbläsarfönster.

8. Klicka på **kontoinställningar**.

    ![Kontohantering](./media/active-directory-saas-screensteps-tutorial/ic778523.png "kontohantering")

9. Klicka på **enkel inloggning**.

    ![Fjärrautentiseringen](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")

10. Klicka på **skapa slutpunkten för enkel inloggning**.

    ![Fjärrautentiseringen](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")

11. I hello **skapa enkel inloggning Endpoint** avsnittet, utföra hello följande steg:

    ![Skapa en slutpunkt för autentisering](./media/active-directory-saas-screensteps-tutorial/ic778526.png "skapa en slutpunkt för autentisering")
    
    a. I hello **rubrik** textruta skriver du ett namn.
    
    b. Från hello **läge** väljer **SAML**.
    
    c. Klicka på **Skapa**.

12. **Redigera** hello ny slutpunkt.

    ![Redigera endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "redigera slutpunkt")

13. I hello **Redigera enkel inloggning Endpoint** avsnittet, utföra hello följande steg:

    ![Remote autentiseringsslutpunkten](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication slutpunkt")

    a. Klicka på **överför ny SAML certifikatfilen**, och sedan ladda upp hello certifikat som du har hämtat från Azure-portalen.
    
    b. Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **Remote inloggnings-URL** textruta.
    
    c. Klistra in **Sign-Out URL** -värde som du har kopierat från hello Azure-portalen i hello **URL för utloggning** textruta.
    
    d. Välj en **grupp** tooassign användare toowhen de har etablerats.
    
    e. Klicka på **uppdatering**.

    f. Kopiera hello **konsument-URL för SAML** toohello Urklipp och klistra in i toohello **inloggnings-URL** TextBox-kontroll i **ScreenSteps domän och URL: er** avsnitt.
    
    g. Returnera toohello **Redigera enkel inloggning Endpoint**.
    
    h. Klicka på hello **göra standard för kontot** knappen toouse den här slutpunkten för alla användare som loggar in i ScreenSteps. Du kan också klicka på hello **lägga till tooSite** knappen toouse specifika platser i den här slutpunkten **ScreenSteps**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-screensteps-test-user"></a>Skapa en testanvändare ScreenSteps

I det här avsnittet skapar du en användare som kallas Britta Simon i ScreenSteps. Arbeta med [ScreenSteps klienten supportteamet](https://www.screensteps.com/contact) att lägga till hello användare i hello ScreenSteps plattform. Användare måste skapas och aktiveras innan du använder enkel inloggning. 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooScreenSteps.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooScreenSteps utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **ScreenSteps**.

    ![Hej ScreenSteps länken i listan med program hello](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour ScreenSteps programmet när du klickar på hello ScreenSteps panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

