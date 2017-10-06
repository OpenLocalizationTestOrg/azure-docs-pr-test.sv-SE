---
title: "Självstudier: Azure Active Directory-integrering med Procore SSO | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Procore enkel inloggning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bb918617f18ba3f44ddde469e6fc411977752f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Självstudier: Azure Active Directory-integrering med Procore enkel inloggning

I kursen får du lära dig hur toointegrate Procore enkel inloggning med Azure Active Directory (AD Azure).

Integrera Procore enkel inloggning med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooProcore enkel inloggning
- Du kan aktivera din användare tooautomatically get inloggade tooProcore SSO (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Procore SSO, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Procore enkel inloggning måste hello följande objekt:

- En Azure AD-prenumeration
- En Procore SSO enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Procore SSO från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-procore-sso-from-hello-gallery"></a>Att lägga till Procore SSO från hello-galleriet
tooconfigure hello integrering av Procore SSO i Azure AD, behöver du tooadd Procore SSO hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Procore SSO från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Procore SSO**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. Markera hello resultat på panelen **Procore SSO**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Procore SSO baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren Procore SSO är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren Procore SSO toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** Procore sso.

tooconfigure och testa Azure AD enkel inloggning med Procore enkel inloggning, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Procore SSO](#creating-a-procore-sso-test-user)**  -toohave en motsvarighet för Britta Simon Procore SSO som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Procore SSO-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Procore SSO:**

1. I hello Azure Management portal på hello **Procore SSO** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. På hello **Procore domän för SSO och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. På hello **Procore SSO Configuration** klickar du på **Konfigurera enkel inloggning för Procore** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. tooconfigure enkel inloggning på **Procore SSO** sida, inloggning tooyour procore företagets webbplats som administratör.

8. Hello verktygslådan listmenyn nedåt, klickar du på **Admin** tooopen hello SSO inställningssidan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. Klistra in hello värden i hello rutor som beskrivs nedan-

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    a. I hello **enkel inloggning på utfärdar-URL** klistra in hello SAML enhets-ID som kopieras från hello Azure-portalen.

    b. I hello **SAML-inloggning på mål-URL** klistra in hello SAML inloggning tjänst-URL för enkel kopieras från hello Azure-portalen.

    c. Öppna hello **XML-Metadata för** hämtade ovan från hello Azure-portalen och kopiera hello certifkatets i hello-tagg med namnet **X509Certificate**. Klistra in hello kopieras värdet till hello **enkel inloggning x509 certifikat** rutan.

10. Klicka på **spara ändringar**.

11. När dessa inställningar måste du toosend hello **domännamn** (t.ex **contoso.com**) via som du loggar in Procore toohello [Procore supportteamet](https://support.procore.com/) och de kommer Aktivera federerad enkel inloggning för domänen.

<!--### Next steps

tooensure users can sign-in tooProcore SSO after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooProcore SSO in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-procore-sso-test-user"></a>Skapa en testanvändare Procore SSO

Följ hello under steg toocreate en Procore testanvändare på sidan.

1. Inloggningen tooyour procore företagets webbplats som administratör.  

2. Hello verktygslådan listmenyn nedåt, klickar du på **Directory** tooopen hello företagets katalogsidan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. Klicka på **lägga till en Person** tooopen hello formuläret och ange utföra följande alternativ -

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    a. I hello **Förnamn** textruta typen användarens förnamn som **Britta**.

    b. I hello **efternamn** textruta typen användarens efternamn som **Simon**.

    c. I hello **e-postadress** textruta typen användarens e-postadress som  **BrittaSimon@contoso.com** .

    d. Välj **behörighet mallen** som **tillämpa behörighet mall senare**.

    e. Klicka på **Skapa**.

4. Kontrollera och uppdatera hello information för hello nyligen tillagda kontakta.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. Klicka på **spara och skicka Invitiation** (om det krävs en inbjudan via e-post) eller **spara** (spara direkt) toocomplete hello användarregistrering.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att ge sina access tooProcore enkel inloggning.

![Tilldela användare][200] 

**tooassign Britta Simon tooProcore SSO utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Procore SSO**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen. Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586). När du klickar på hello Procore SSO-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Procore SSO-program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

