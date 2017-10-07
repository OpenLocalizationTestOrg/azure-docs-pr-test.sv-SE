---
title: "Självstudier: Azure Active Directory-integrering med Bambu av brysselkål sociala | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Bambu av brysselkål sociala."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2b9ddbc-cab7-40d6-aca1-5b171cab4199
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: c9998772c032476f9a18054873022f55b4bb78be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bambu-by-sprout-social"></a>Självstudier: Azure Active Directory-integrering med Bambu av brysselkål sociala

I kursen får du lära dig hur toointegrate Bambu av brysselkål sociala med Azure Active Directory (AD Azure).

Integrera Bambu av brysselkål sociala med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooBambu av brysselkål sociala
- Du kan aktivera din användare tooautomatically get inloggade tooBambu av brysselkål sociala (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Bambu by Sprout Social, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Bambu by Sprout Social.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Bambu av brysselkål sociala måste hello följande objekt:

- En Azure AD-prenumeration
- En Bambu av brysselkål sociala enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Bambu av brysselkål sociala från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-bambu-by-sprout-social-from-hello-gallery"></a>Att lägga till Bambu av brysselkål sociala från hello-galleriet
tooconfigure hello integrering av Bambu av brysselkål sociala i Azure AD, behöver du tooadd Bambu av brysselkål sociala hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Bambu av brysselkål sociala från galleriet hello utför hello följande steg:**

1. I hello  **[Azure Portal](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan tooadd nytt program.

    ![Program][3]

4. Skriv i sökrutan hello **Bambu av brysselkål sociala**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_search.png)

5. Markera hello resultat på panelen **Bambu av brysselkål sociala**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Bambu av brysselkål sociala baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Bambu av brysselkål sociala är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Bambu av brysselkål sociala toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Bambu av brysselkål sociala.

tooconfigure och testa Azure AD enkel inloggning med Bambu av brysselkål sociala, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en Bambu av brysselkål sociala testanvändare](#creating-a-bambu-by-sprout-social-test-user)**  -toohave en motsvarighet för Britta Simon i Bambu av brysselkål sociala som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Bambu av brysselkål sociala program.

**tooconfigure Azure AD enkel inloggning med Bambu av brysselkål sociala utför hello följande steg:**

1. I hello Azure-portalen på hello **Bambu av brysselkål sociala** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_samlbase.png)

3. På hello **Bambu brysselkål sociala domänen och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_url.png)

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_400.png)
    
6. På hello **Bambu brysselkål sociala konfigurationen** klickar du på **konfigurera Bambu av brysselkål sociala** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_configure.png) 

7. tooconfigure enkel inloggning på **Bambu av brysselkål sociala** sida, behöver du toosend hello hämtas **XML-Metadata för** och **SAML inloggning tjänst-URL för enkel** för[Bambu av brysselkål sociala support](mailto:support@getbambu.com). De ska ställa in detta i ordning toohave hello SAML SSO anslutningen korrekt på båda sidor.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka på hello **enkel inloggning** fliken och åtkomst hello inbäddade dokumentationen via hello **Configuration** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

<!--### Next steps

tooensure users can sign-in tooBambu by Sprout Social after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Bambu by Sprout Social prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooBambu by Sprout Social in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Bambu by Sprout Social users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **Britta Simon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-bambu-by-sprout-social-test-user"></a>Skapa en Bambu av brysselkål sociala testanvändare

Programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooBambu av brysselkål sociala.

![Tilldela användare][200] 

**tooassign Britta Simon tooBambu av brysselkål sociala utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Bambu av brysselkål sociala**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Bambu av brysselkål sociala program när du klickar på hello Bambu av brysselkål sociala panelen i hello åtkomstpanelen. Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_203.png

