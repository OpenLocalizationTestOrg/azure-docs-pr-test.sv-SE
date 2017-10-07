---
title: "Självstudier: Azure Active Directory-integrering med Teamphoria | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Teamphoria."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: f32be9742b76f7fe464036dadc108c62e4a787a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Självstudier: Azure Active Directory-integrering med Teamphoria

I kursen får du lära dig hur toointegrate Teamphoria med Azure Active Directory (AD Azure).

Integrera Teamphoria med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooTeamphoria
- Du kan aktivera din användare tooautomatically get inloggade tooTeamphoria (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure Management portal

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Teamphoria, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Teamphoria, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Teamphoria enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Teamphoria från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-teamphoria-from-hello-gallery"></a>Att lägga till Teamphoria från hello-galleriet
tooconfigure hello integrering av Teamphoria i Azure AD, behöver du tooadd Teamphoria hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Teamphoria från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Teamphoria**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. Markera hello resultat på panelen **Teamphoria**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Teamphoria baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Teamphoria är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Teamphoria toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Teamphoria.

tooconfigure och testa Azure AD enkel inloggning med Teamphoria, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Teamphoria](#creating-a-teamphoria-test-user)**  -toohave en motsvarighet för Britta Simon i Teamphoria som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Teamphoria program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Teamphoria:**

1. I hello Azure Management portal på hello **Teamphoria** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. På hello **Teamphoria domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    a. I hello **inloggnings-URL** textruta typen hello URL med hello följande mönster:`https://<sub-domain>.teamphoria.com/login`  

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska inloggnings-URL. Kontakta [Teamphoria klienten supportteamet](https://www.teamphoria.com/) tooget hello inloggning URL. 

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. På hello **Teamphoria Configuration** klickar du på **konfigurera Teamphoria** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. tooconfigure enkel inloggning på **Teamphoria** sida, inloggning tooyour Teamphoria program som administratör.

8. Gå för**ADMINISTRATIONSINSTÄLLNINGAR** alternativet i hello verktygsfältet till vänster och under hello hello konfigurera fliken klickar du på **enda SIGN-ON** tooopen hello SSO configuration fönster.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. Klicka på **lägga till nya IDENTITETSLEVERANTÖR** alternativet i övre högra hörnet hello tooopen hello form för att lägga till hello inställningar för enkel inloggning.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. Ange hello detaljer i hello fält som beskrivs nedan-

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **VISNINGSNAMN** : Ange hello visningsnamnet för hello plugin-programmet på hello Administrationssida.

    b. **KNAPPNAMN** : hello namn hello fliken som kommer att visas på inloggningssidan för hello för att logga in via enkel inloggning.

    c. **CERTIFIKATET** : öppna hello certifikat hämtade tidigare från hello Azure-portalen i anteckningar, kopiera hello innehållet i hello samma och klistra in den här hello i rutan.

    d. **STARTPUNKTEN** : klistra in hello **SAML inloggning tjänst-URL för enkel** kopieras tidigare formuläret hello Azure-portalen.

    e. Växla hello alternativet för**ON** och klicka på **spara**. 

<!--### Next steps

tooensure users can sign-in tooTeamphoria after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooTeamphoria in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-teamphoria-test-user"></a>Skapa en testanvändare Teamphoria

I ordning tooenable Azure AD-användare toolog i Teamphoria, måste de etableras i Teamphoria. Hello gäller Teamphoria är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour Teamphoria företagets webbplats som administratör.

2. Klicka på **ADMIN** inställningar för hello verktygsfältet till vänster och på hello **hantera** fliken Klicka på **användare** tooopen hello Administrationssida för användare.

    ![Lägga till medarbetare](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. Klicka på hello **manuell bjuda in** alternativet.

    ![Bjud in personer](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. Utför följande åtgärder på den här sidan. 
    
    ![Bjud in personer](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    a. I hello **e-postadress** textruta hello **e-postadress** av BrittaSimon.

    b. I hello **Förnamn** textruta typen **Britta**.

    c. I hello **efternamn** textruta typen **Simon**.

    d. Klicka på **inbjudan 1 användaren**. Användaren måste tooaccept hello inbjudan tooget skapas i hello system.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooTeamphoria sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooTeamphoria utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Teamphoria**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen. Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

