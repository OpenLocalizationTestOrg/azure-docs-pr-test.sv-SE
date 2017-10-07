---
title: "Självstudier: Azure Active Directory-integrering med Google Apps i Azure | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2093b5ab605ec0d7bbefe7a78e1eede34d756f53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a>Självstudier: Azure Active Directory-integrering med Google Apps

I kursen får du lära dig hur toointegrate Google Apps med Azure Active Directory (AD Azure).

Integrera Google Apps med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooGoogle appar
- Du kan aktivera din användare tooautomatically get inloggade tooGoogle appar (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Google Apps behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Google Apps enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).

## <a name="video-tutorial"></a>Videosjälvstudie
Hur tooEnable enkel inloggning tooGoogle appar i två minuter:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
1. **F: är Chromebooks och andra enheter som Chrome kompatibla med Azure AD enkel inloggning?**
   
    A: användare kan Ja, kan toosign till sina Chromebook enheter med hjälp av autentiseringsuppgifterna Azure AD. Se den här [artikel stöd för Google Apps](https://support.google.com/chrome/a/answer/6060880) för information om varför användare kan uppmanas att autentiseringsuppgifter två gånger.

2. **F: om jag aktiverar enkel inloggning, kommer användarna att kunna toouse toosign sina Azure AD-autentiseringsuppgifterna i alla Google-produkten, till exempel Google klassrum, GMail, Google Drive, YouTube och så vidare?**
   
    S: Ja, beroende på [vilka Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) du välja tooenable eller inaktivera för din organisation.

3. **F: kan jag aktivera enkel inloggning för en delmängd av användarna Google Apps?**
   
    A: Aktivera enkel inloggning omedelbart kräver inte alla dina Google Apps användare tooauthenticate med sina autentiseringsuppgifter för Azure AD. Eftersom Google Apps inte stöder att ha flera identitetsleverantörer, hello identitetsleverantören för Google Apps-miljö kan antingen vara Azure AD eller Google-- men inte båda på hello samtidigt.

4. **F: om en användare har loggat in via Windows är de automatiskt autentisera tooGoogle appar utan ett lösenord uppmanas?**
   
    S: finns två alternativ för att aktivera det här scenariot. Först användare kan logga in på Windows 10-enheter via [Azure Active Directory Join](active-directory-azureadjoin-overview.md). Du kan också användare kan logga in på Windows-enheter som är ansluten till domänen tooan lokala Active Directory som har aktiverats för enkel inloggning tooAzure AD via en [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) distribution. Båda alternativen kräver tooperform hello stegen i hello följa kursen tooenable enkel inloggning mellan Azure AD och Google Apps.

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Google Apps från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-google-apps-from-hello-gallery"></a>Lägga till Google Apps från hello-galleriet
tooconfigure hello integrering av Google Apps i Azure AD, behöver du tooadd Google Apps hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Google Apps från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Google Apps**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. Markera hello resultat på panelen **Google Apps**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Google Apps baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Google Apps är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Google Apps toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Google Apps.

tooconfigure och testa Azure AD enkel inloggning med Google Apps behöver toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare för Google Apps](#creating-a-google-apps-test-user)**  -toohave en motsvarighet för Britta Simon i Google-appar som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Google Apps-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Google Apps:**

1. I hello Azure-portalen på hello **Google Apps** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. På hello **Google Apps-domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://mail.google.com/a/<yourdomain>`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera hello värde med hello faktiska inloggnings-URL. Kontakta hello [Google supportteamet](https://www.google.com/contact/).
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. På hello **Google Apps-konfiguration** klickar du på **konfigurera Google Apps** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enkel inloggning URL: en och ändra URL: en lösenord** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. Öppna en ny flik i webbläsaren och logga in på hello [Google Apps-administrationskonsolen](http://admin.google.com/) med ditt administratörskonto.

8. Klicka på **säkerhet**. Om du inte ser hello länken, det kan vara dolt under hello **fler kontroller** menyn längst hello hello-skärmen.
   
    ![Klicka på Security.][10]

9. På hello **säkerhet** klickar du på **Konfigurera enkel inloggning (SSO).**
   
    ![Klicka på enkel inloggning.][11]

10. Utför följande konfigurationsändringar hello:
   
    ![Konfigurera enkel inloggning][12]
   
    a. Välj **installationsprogrammet SSO med tredjeparts identitetsprovider**.

    b. I den **inloggning Sidadress** fältet i Google Apps, klistra in hello värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.

    c. I hello **URL för utloggning** fältet i Google Apps, klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen. 

    d. I hello **ändra lösenord URL** fältet i Google Apps, klistra in hello värdet för **ändra lösenord URL**, som du har kopierat från Azure-portalen. 

    e. I Google Apps för hello **verifieringscertifikat**, överför hello certifikat som du har hämtat från Azure-portalen.

    f. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-google-apps-test-user"></a>Skapa en testanvändare för Google Apps

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Google Apps programvara. Google Apps har stöd för automatisk etablering, vilket är aktiverat som standard. Det finns inga åtgärder i det här avsnittet. Om en användare inte redan finns i Google Apps programvara, skapas en ny när du försöker tooaccess Google Apps programvara.

>[!NOTE] 
>Om du behöver toocreate en användare manuellt kan kontakta hello [Google supportteamet](https://www.google.com/contact/).

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooGoogle appar.

![Tilldela användare][200] 

**tooassign Britta Simon tooGoogle appar, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Google Apps**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet tootest enkel inloggning inställningarna, öppna hello åtkomstpanelen på [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md)sedan logga in på hello testkonto och klicka på **Google Apps** panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png