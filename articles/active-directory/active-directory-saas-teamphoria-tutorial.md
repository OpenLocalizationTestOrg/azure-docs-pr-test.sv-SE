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
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a><span data-ttu-id="727db-103">Självstudier: Azure Active Directory-integrering med Teamphoria</span><span class="sxs-lookup"><span data-stu-id="727db-103">Tutorial: Azure Active Directory integration with Teamphoria</span></span>

<span data-ttu-id="727db-104">I kursen får du lära dig hur toointegrate Teamphoria med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="727db-104">In this tutorial, you learn how toointegrate Teamphoria with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="727db-105">Integrera Teamphoria med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="727db-105">Integrating Teamphoria with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="727db-106">Du kan styra i Azure AD som har åtkomst till tooTeamphoria</span><span class="sxs-lookup"><span data-stu-id="727db-106">You can control in Azure AD who has access tooTeamphoria</span></span>
- <span data-ttu-id="727db-107">Du kan aktivera din användare tooautomatically get inloggade tooTeamphoria (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="727db-107">You can enable your users tooautomatically get signed-on tooTeamphoria (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="727db-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="727db-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="727db-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="727db-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Teamphoria, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="727db-110">Krav</span><span class="sxs-lookup"><span data-stu-id="727db-110">Prerequisites</span></span>

<span data-ttu-id="727db-111">tooconfigure Azure AD-integrering med Teamphoria, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="727db-111">tooconfigure Azure AD integration with Teamphoria, you need hello following items:</span></span>

- <span data-ttu-id="727db-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="727db-112">An Azure AD subscription</span></span>
- <span data-ttu-id="727db-113">En Teamphoria enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="727db-113">A Teamphoria single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="727db-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="727db-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="727db-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="727db-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="727db-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="727db-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="727db-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="727db-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="727db-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="727db-118">Scenario description</span></span>
<span data-ttu-id="727db-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="727db-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="727db-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="727db-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="727db-121">Att lägga till Teamphoria från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="727db-121">Adding Teamphoria from hello gallery</span></span>
2. <span data-ttu-id="727db-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="727db-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamphoria-from-hello-gallery"></a><span data-ttu-id="727db-123">Att lägga till Teamphoria från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="727db-123">Adding Teamphoria from hello gallery</span></span>
<span data-ttu-id="727db-124">tooconfigure hello integrering av Teamphoria i Azure AD, behöver du tooadd Teamphoria hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="727db-124">tooconfigure hello integration of Teamphoria into Azure AD, you need tooadd Teamphoria from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="727db-125">**tooadd Teamphoria från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="727db-125">**tooadd Teamphoria from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="727db-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="727db-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="727db-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="727db-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="727db-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="727db-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="727db-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="727db-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="727db-133">Skriv i sökrutan hello **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="727db-133">In hello search box, type **Teamphoria**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. <span data-ttu-id="727db-135">Markera hello resultat på panelen **Teamphoria**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="727db-135">In hello results panel, select **Teamphoria**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="727db-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="727db-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="727db-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Teamphoria baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="727db-138">In this section, you configure and test Azure AD single sign-on with Teamphoria based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="727db-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Teamphoria är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="727db-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Teamphoria is tooa user in Azure AD.</span></span> <span data-ttu-id="727db-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Teamphoria toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="727db-140">In other words, a link relationship between an Azure AD user and hello related user in Teamphoria needs toobe established.</span></span>

<span data-ttu-id="727db-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="727db-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Teamphoria.</span></span>

<span data-ttu-id="727db-142">tooconfigure och testa Azure AD enkel inloggning med Teamphoria, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="727db-142">tooconfigure and test Azure AD single sign-on with Teamphoria, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="727db-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="727db-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="727db-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="727db-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="727db-145">**[Skapa en testanvändare Teamphoria](#creating-a-teamphoria-test-user)**  -toohave en motsvarighet för Britta Simon i Teamphoria som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="727db-145">**[Creating a Teamphoria test user](#creating-a-teamphoria-test-user)** - toohave a counterpart of Britta Simon in Teamphoria that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="727db-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="727db-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="727db-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="727db-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="727db-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="727db-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="727db-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Teamphoria program.</span><span class="sxs-lookup"><span data-stu-id="727db-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Teamphoria application.</span></span>

<span data-ttu-id="727db-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Teamphoria:**</span><span class="sxs-lookup"><span data-stu-id="727db-150">**tooconfigure Azure AD single sign-on with Teamphoria, perform hello following steps:**</span></span>

1. <span data-ttu-id="727db-151">I hello Azure Management portal på hello **Teamphoria** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="727db-151">In hello Azure Management portal, on hello **Teamphoria** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="727db-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="727db-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. <span data-ttu-id="727db-155">På hello **Teamphoria domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="727db-155">On hello **Teamphoria Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    <span data-ttu-id="727db-157">a.</span><span class="sxs-lookup"><span data-stu-id="727db-157">a.</span></span> <span data-ttu-id="727db-158">I hello **inloggnings-URL** textruta typen hello URL med hello följande mönster:`https://<sub-domain>.teamphoria.com/login`</span><span class="sxs-lookup"><span data-stu-id="727db-158">In hello **Sign-on URL** textbox, type hello URL using hello following pattern: `https://<sub-domain>.teamphoria.com/login`</span></span>  

    > [!NOTE] 
    > <span data-ttu-id="727db-159">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="727db-159">Please note that these are not hello real values.</span></span> <span data-ttu-id="727db-160">Du har tooupdate dessa värden med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="727db-160">You have tooupdate these values with hello actual Sign-On URL.</span></span> <span data-ttu-id="727db-161">Kontakta [Teamphoria klienten supportteamet](https://www.teamphoria.com/) tooget hello inloggning URL.</span><span class="sxs-lookup"><span data-stu-id="727db-161">Contact [Teamphoria Client support team](https://www.teamphoria.com/) tooget hello Sign-on URL.</span></span> 

4. <span data-ttu-id="727db-162">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="727db-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. <span data-ttu-id="727db-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="727db-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="727db-166">På hello **Teamphoria Configuration** klickar du på **konfigurera Teamphoria** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="727db-166">On hello **Teamphoria Configuration** section, click **Configure Teamphoria** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="727db-167">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="727db-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. <span data-ttu-id="727db-169">tooconfigure enkel inloggning på **Teamphoria** sida, inloggning tooyour Teamphoria program som administratör.</span><span class="sxs-lookup"><span data-stu-id="727db-169">tooconfigure single sign-on on **Teamphoria** side, Login tooyour Teamphoria application as an administrator.</span></span>

8. <span data-ttu-id="727db-170">Gå för**ADMINISTRATIONSINSTÄLLNINGAR** alternativet i hello verktygsfältet till vänster och under hello hello konfigurera fliken klickar du på **enda SIGN-ON** tooopen hello SSO configuration fönster.</span><span class="sxs-lookup"><span data-stu-id="727db-170">Go too**ADMIN SETTINGS** option in hello left toolbar and under hello hello Configure Tab click on **SINGLE SIGN-ON** tooopen hello SSO configuration window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. <span data-ttu-id="727db-172">Klicka på **lägga till nya IDENTITETSLEVERANTÖR** alternativet i övre högra hörnet hello tooopen hello form för att lägga till hello inställningar för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="727db-172">Click on **ADD NEW IDENTITY PROVIDER** option in hello top right corner tooopen hello form for adding hello settings for SSO.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. <span data-ttu-id="727db-174">Ange hello detaljer i hello fält som beskrivs nedan-</span><span class="sxs-lookup"><span data-stu-id="727db-174">Enter hello details in hello fields as described below-</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    <span data-ttu-id="727db-176">a.</span><span class="sxs-lookup"><span data-stu-id="727db-176">a.</span></span> <span data-ttu-id="727db-177">**VISNINGSNAMN** : Ange hello visningsnamnet för hello plugin-programmet på hello Administrationssida.</span><span class="sxs-lookup"><span data-stu-id="727db-177">**DISPLAY NAME** : Enter hello display name of hello plugin on hello admin page.</span></span>

    <span data-ttu-id="727db-178">b.</span><span class="sxs-lookup"><span data-stu-id="727db-178">b.</span></span> <span data-ttu-id="727db-179">**KNAPPNAMN** : hello namn hello fliken som kommer att visas på inloggningssidan för hello för att logga in via enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="727db-179">**BUTTON NAME** : hello name of hello tab which will display on hello login page for logging in via SSO.</span></span>

    <span data-ttu-id="727db-180">c.</span><span class="sxs-lookup"><span data-stu-id="727db-180">c.</span></span> <span data-ttu-id="727db-181">**CERTIFIKATET** : öppna hello certifikat hämtade tidigare från hello Azure-portalen i anteckningar, kopiera hello innehållet i hello samma och klistra in den här hello i rutan.</span><span class="sxs-lookup"><span data-stu-id="727db-181">**CERTIFICATE** : Open hello Certificate downloaded earlier from hello Azure portal in notepad, copy hello contents of hello same and paste it here in hello box.</span></span>

    <span data-ttu-id="727db-182">d.</span><span class="sxs-lookup"><span data-stu-id="727db-182">d.</span></span> <span data-ttu-id="727db-183">**STARTPUNKTEN** : klistra in hello **SAML inloggning tjänst-URL för enkel** kopieras tidigare formuläret hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="727db-183">**ENTRY POINT** : Paste hello **SAML Single Sign-On Service URL** copied earlier form hello Azure portal.</span></span>

    <span data-ttu-id="727db-184">e.</span><span class="sxs-lookup"><span data-stu-id="727db-184">e.</span></span> <span data-ttu-id="727db-185">Växla hello alternativet för**ON** och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="727db-185">Switch hello option too**ON** and click on **SAVE**.</span></span> 

<!--### Next steps

tooensure users can sign-in tooTeamphoria after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooTeamphoria in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="727db-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="727db-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="727db-187">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="727db-187">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="727db-189">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="727db-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="727db-190">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="727db-190">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="727db-192">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="727db-192">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="727db-194">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="727db-194">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="727db-196">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="727db-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="727db-198">a.</span><span class="sxs-lookup"><span data-stu-id="727db-198">a.</span></span> <span data-ttu-id="727db-199">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="727db-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="727db-200">b.</span><span class="sxs-lookup"><span data-stu-id="727db-200">b.</span></span> <span data-ttu-id="727db-201">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="727db-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="727db-202">c.</span><span class="sxs-lookup"><span data-stu-id="727db-202">c.</span></span> <span data-ttu-id="727db-203">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="727db-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="727db-204">d.</span><span class="sxs-lookup"><span data-stu-id="727db-204">d.</span></span> <span data-ttu-id="727db-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="727db-205">Click **Create**.</span></span>
 
### <a name="creating-a-teamphoria-test-user"></a><span data-ttu-id="727db-206">Skapa en testanvändare Teamphoria</span><span class="sxs-lookup"><span data-stu-id="727db-206">Creating a Teamphoria test user</span></span>

<span data-ttu-id="727db-207">I ordning tooenable Azure AD-användare toolog i Teamphoria, måste de etableras i Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="727db-207">In order tooenable Azure AD users toolog into Teamphoria, they must be provisioned into Teamphoria.</span></span> <span data-ttu-id="727db-208">Hello gäller Teamphoria är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="727db-208">In hello case of Teamphoria, provisioning is a manual task.</span></span>

<span data-ttu-id="727db-209">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="727db-209">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="727db-210">Logga in tooyour Teamphoria företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="727db-210">Log in tooyour Teamphoria company site as an administrator.</span></span>

2. <span data-ttu-id="727db-211">Klicka på **ADMIN** inställningar för hello verktygsfältet till vänster och på hello **hantera** fliken Klicka på **användare** tooopen hello Administrationssida för användare.</span><span class="sxs-lookup"><span data-stu-id="727db-211">Click on **ADMIN** settings on hello left toolbar and under hello **MANAGE** tab Click on **USERS** tooopen hello admin page for users.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. <span data-ttu-id="727db-213">Klicka på hello **manuell bjuda in** alternativet.</span><span class="sxs-lookup"><span data-stu-id="727db-213">Click on hello **MANUAL INVITE** option.</span></span>

    ![Bjud in personer](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. <span data-ttu-id="727db-215">Utför följande åtgärder på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="727db-215">On this page, perform following action.</span></span> 
    
    ![Bjud in personer](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    <span data-ttu-id="727db-217">a.</span><span class="sxs-lookup"><span data-stu-id="727db-217">a.</span></span> <span data-ttu-id="727db-218">I hello **e-postadress** textruta hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="727db-218">In hello **EMAIL ADDRESS** textbox, hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="727db-219">b.</span><span class="sxs-lookup"><span data-stu-id="727db-219">b.</span></span> <span data-ttu-id="727db-220">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="727db-220">In hello **FIRST NAME** textbox, type **Britta**.</span></span>

    <span data-ttu-id="727db-221">c.</span><span class="sxs-lookup"><span data-stu-id="727db-221">c.</span></span> <span data-ttu-id="727db-222">I hello **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="727db-222">In hello **LAST NAME** textbox, type **Simon**.</span></span>

    <span data-ttu-id="727db-223">d.</span><span class="sxs-lookup"><span data-stu-id="727db-223">d.</span></span> <span data-ttu-id="727db-224">Klicka på **inbjudan 1 användaren**.</span><span class="sxs-lookup"><span data-stu-id="727db-224">Click **INVITE 1 USER**.</span></span> <span data-ttu-id="727db-225">Användaren måste tooaccept hello inbjudan tooget skapas i hello system.</span><span class="sxs-lookup"><span data-stu-id="727db-225">User needs tooaccept hello invite tooget created in hello system.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="727db-226">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="727db-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="727db-227">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooTeamphoria sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="727db-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooTeamphoria.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="727db-229">**tooassign Britta Simon tooTeamphoria utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="727db-229">**tooassign Britta Simon tooTeamphoria, perform hello following steps:**</span></span>

1. <span data-ttu-id="727db-230">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="727db-230">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="727db-232">Välj i listan med program hello **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="727db-232">In hello applications list, select **Teamphoria**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. <span data-ttu-id="727db-234">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="727db-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="727db-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="727db-236">Click **Add** button.</span></span> <span data-ttu-id="727db-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="727db-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="727db-239">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="727db-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="727db-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="727db-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="727db-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="727db-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="727db-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="727db-242">Testing single sign-on</span></span>

<span data-ttu-id="727db-243">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="727db-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="727db-244">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="727db-244">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="727db-245">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="727db-245">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="727db-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="727db-246">Additional resources</span></span>

* [<span data-ttu-id="727db-247">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="727db-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="727db-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="727db-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

