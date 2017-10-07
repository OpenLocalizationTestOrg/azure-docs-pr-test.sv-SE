---
title: "Självstudier: Azure Active Directory-integrering med Cezanne HR programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Cezanne HR-programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 3675acd8871d62c2277def8074f7aa39ac46e2a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a><span data-ttu-id="dc801-103">Självstudier: Integrera Azure Active Directory med Cezanne HR-programvara</span><span class="sxs-lookup"><span data-stu-id="dc801-103">Tutorial: Integrate Azure Active Directory with Cezanne HR software</span></span>

<span data-ttu-id="dc801-104">I kursen får du lära dig hur toointegrate Cezanne HR-program med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="dc801-104">In this tutorial, you learn how toointegrate Cezanne HR software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dc801-105">Integrera Cezanne HR-program med Azure AD ger dig hello följande fördelar.</span><span class="sxs-lookup"><span data-stu-id="dc801-105">Integrating Cezanne HR software with Azure AD provides you with hello following benefits.</span></span> <span data-ttu-id="dc801-106">Du kan:</span><span class="sxs-lookup"><span data-stu-id="dc801-106">You can:</span></span>

- <span data-ttu-id="dc801-107">Kontrollera i Azure AD som har åtkomst tooCezanne HR-programvara.</span><span class="sxs-lookup"><span data-stu-id="dc801-107">Control in Azure AD who has access tooCezanne HR software.</span></span>
- <span data-ttu-id="dc801-108">Aktivera användare tooautomatically-inloggning tooCezanne HR-program med enkel inloggning (SSO) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="dc801-108">Enable your users tooautomatically sign in tooCezanne HR software with single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="dc801-109">Hantera dina konton i en central plats: hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc801-109">Manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="dc801-110">toolearn mer information om programvara som en tjänst (SaaS) app-integrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dc801-110">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and SSO with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc801-111">Krav</span><span class="sxs-lookup"><span data-stu-id="dc801-111">Prerequisites</span></span>

<span data-ttu-id="dc801-112">tooconfigure Azure AD-integrering med Cezanne HR-programvara du behöver hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="dc801-112">tooconfigure Azure AD integration with Cezanne HR software, you need hello following items:</span></span>

- <span data-ttu-id="dc801-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dc801-113">An Azure AD subscription</span></span>
- <span data-ttu-id="dc801-114">Cezanne HR-programvara SSO-aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="dc801-114">A Cezanne HR software SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dc801-115">tootest hello steg i den här självstudiekursen, rekommenderar vi att du inte använder en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="dc801-115">tootest hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="dc801-116">tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="dc801-116">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="dc801-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="dc801-117">Don't use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dc801-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dc801-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dc801-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="dc801-119">Scenario description</span></span>
<span data-ttu-id="dc801-120">I kursen får testa du Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="dc801-120">In this tutorial, you test Azure AD SSO in a test environment.</span></span> 

<span data-ttu-id="dc801-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="dc801-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="dc801-122">Lägga till Cezanne HR programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="dc801-122">Adding Cezanne HR software from hello gallery</span></span>
* <span data-ttu-id="dc801-123">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="dc801-123">Configuring and testing Azure AD SSO</span></span>

## <a name="add-cezanne-hr-software-from-hello-gallery"></a><span data-ttu-id="dc801-124">Lägg till Cezanne HR programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="dc801-124">Add Cezanne HR software from hello gallery</span></span>
<span data-ttu-id="dc801-125">tooconfigure hello integrering av Cezanne HR-program i Azure AD och lägga till Cezanne HR programvara från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="dc801-125">tooconfigure hello integration of Cezanne HR software into Azure AD, add Cezanne HR software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="dc801-126">tooadd Cezanne HR programvara från galleriet hello hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-126">tooadd Cezanne HR software from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="dc801-127">I hello  **[Azure-portalen](https://portal.azure.com)**, i hello vänstra rutan, Välj hello **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc801-127">In hello **[Azure portal](https://portal.azure.com)**, in hello left pane, select hello **Azure Active Directory** button.</span></span> 

    ![Hej ”Azure Active Directory”-knappen][1]

2. <span data-ttu-id="dc801-129">Välj **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dc801-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![Hej ”alla program” länk][2]
    
3. <span data-ttu-id="dc801-131">tooadd ett nytt program hello överst i hello **alla program** dialogrutan **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="dc801-131">tooadd a new application, at hello top of hello **All applications** dialog box, select **New application**.</span></span>

    ![Hej ”nya program” knappen][3]

4. <span data-ttu-id="dc801-133">Skriv i sökrutan hello **Cezanne HR programvara**.</span><span class="sxs-lookup"><span data-stu-id="dc801-133">In hello search box, type **Cezanne HR Software**.</span></span>

    ![hello sökrutan](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. <span data-ttu-id="dc801-135">Markera i hello resultat **Cezanne HR programvara** och välj sedan hello **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="dc801-135">In hello results list, select **Cezanne HR Software** and then select hello **Add** button tooadd hello application.</span></span>

    ![hello resultatlistan](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="dc801-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dc801-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="dc801-138">I det här avsnittet kan du konfigurera och testa Azure AD SSO med Cezanne HR programvara baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="dc801-138">In this section, you configure and test Azure AD SSO with Cezanne HR software based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dc801-139">För enkel inloggning toowork måste Azure AD tooknow hello Cezanne HR programvara motsvarighet toohello Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="dc801-139">For SSO toowork, Azure AD needs tooknow hello Cezanne HR software counterpart toohello Azure AD user.</span></span> <span data-ttu-id="dc801-140">Med andra ord måste du upprätta en länk relationen mellan en Azure AD-användare och hello relaterade användaren i hello Cezanne HR-programvara.</span><span class="sxs-lookup"><span data-stu-id="dc801-140">In other words, you must establish a link relationship between an Azure AD user and hello related user in hello Cezanne HR software.</span></span>

<span data-ttu-id="dc801-141">tooestablish hello länken relationen, tilldela hello Cezanne HR programvara **användarnamn** värde som hello Azure AD **användarnamn** värde.</span><span class="sxs-lookup"><span data-stu-id="dc801-141">tooestablish hello link relationship, assign hello Cezanne HR software **user name** value as hello Azure AD **Username** value.</span></span>

<span data-ttu-id="dc801-142">tooconfigure och testa Azure AD enkel inloggning med hjälp av Cezanne HR-program, fullständig hello följande byggblock.</span><span class="sxs-lookup"><span data-stu-id="dc801-142">tooconfigure and test Azure AD SSO by using Cezanne HR software, complete hello following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="dc801-143">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dc801-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="dc801-144">I det här avsnittet kan du aktivera Azure AD SSO i hello Azure-portalen och konfigurera enkel inloggning i programmet Cezanne HR hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-144">In this section, you can enable Azure AD SSO in hello Azure portal and configure SSO in your Cezanne HR software application by doing hello following:</span></span>

1. <span data-ttu-id="dc801-145">I hello Azure-portalen på hello **Cezanne HR programvara** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dc801-145">In hello Azure portal, on hello **Cezanne HR Software** application integration page, select **Single sign-on**.</span></span>

    ![Hej ”enkel inloggning” kommando][4]

2. <span data-ttu-id="dc801-147">tooenable SSO i hello **enkel inloggning** dialogrutan, Välj hello **läge** som **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dc801-147">tooenable SSO, in hello **Single sign-on** dialog box, select hello **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Hej ”Mode-ruta](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. <span data-ttu-id="dc801-149">Under **Cezanne HR programvara domän och URL: er**, hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-149">Under **Cezanne HR Software Domain and URLs**, do hello following:</span></span>

    ![Hej ”Cezanne HR programvara domän och URL: er” avsnittet](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    <span data-ttu-id="dc801-151">a.</span><span class="sxs-lookup"><span data-stu-id="dc801-151">a.</span></span> <span data-ttu-id="dc801-152">I hello **inloggnings-URL** Skriv en URL som har hello följande syntax:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="dc801-152">In hello **Sign-on URL** box, type a URL that has hello following syntax: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span></span>

    <span data-ttu-id="dc801-153">b.</span><span class="sxs-lookup"><span data-stu-id="dc801-153">b.</span></span> <span data-ttu-id="dc801-154">I hello **Reply URL** Skriv en URL som har hello följande syntax:`https://w3.cezanneondemand.com:443/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="dc801-154">In hello **Reply URL** box, type a URL that has hello following syntax: `https://w3.cezanneondemand.com:443/<tenantid>`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="dc801-155">hello föregående värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="dc801-155">hello preceding values are not real.</span></span> <span data-ttu-id="dc801-156">Uppdatera dem med hello faktiska reply URL och hello inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="dc801-156">Update them with hello actual reply URL and hello sign-on URL.</span></span> <span data-ttu-id="dc801-157">tooobtain hello värden, kontakta hello [Cezanne HR programvara klienten supportteamet](mailto:info@cezannehr.com).</span><span class="sxs-lookup"><span data-stu-id="dc801-157">tooobtain hello values, contact hello [Cezanne HR software client support team](mailto:info@cezannehr.com).</span></span>

4. <span data-ttu-id="dc801-158">Under **SAML-signeringscertifikat**väljer **certifikat (Base64)**, och sedan spara hello certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="dc801-158">Under **SAML Signing Certificate**, select **Certificate (Base64)**, and then save hello certificate file on your computer.</span></span>

    ![Hej ”SAML signeringscertifikat” avsnittet](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. <span data-ttu-id="dc801-160">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dc801-160">Select **Save**.</span></span>

    ![Hej ”spara”.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="dc801-162">Under **Cezanne HR programvarukonfiguration**väljer **konfigurera Cezanne HR programvara** tooopen hello **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="dc801-162">Under **Cezanne HR Software Configuration**, select **Configure Cezanne HR Software** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="dc801-163">Kopiera hello **SAML enhets-ID** och **SAML enkel inloggning** URL: en från hello **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dc801-163">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service** URL from hello **Quick Reference** section.</span></span>

    ![Hej ”Cezanne HR programvara” konfigurationsavsnitt](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. <span data-ttu-id="dc801-165">Logga tooyour Cezanne HR programvara innehavaren som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="dc801-165">In a different web browser window, sign on tooyour Cezanne HR software tenant as an administrator.</span></span>

8. <span data-ttu-id="dc801-166">I hello vänster och välj **systeminställningarna**.</span><span class="sxs-lookup"><span data-stu-id="dc801-166">In hello left pane, select **System Setup**.</span></span> <span data-ttu-id="dc801-167">Välj **säkerhetsinställningar** > **enkel inloggning Configuration**.</span><span class="sxs-lookup"><span data-stu-id="dc801-167">Select **Security Settings** > **Single Sign-On Configuration**.</span></span>

    ![Hej ”enkel inloggning Configuration” länk](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. <span data-ttu-id="dc801-169">I hello **Tillåt användare toolog i med hjälp av följande tjänster för enkel inloggning (SSO) hello** rutan, Välj hello **SAML 2.0** kryssrutan och välj hello **Advanced Configuration** alternativet.</span><span class="sxs-lookup"><span data-stu-id="dc801-169">In hello **Allow users toolog in using hello following Single Sign-On (SSO) services** pane, select hello **SAML 2.0** check box and select hello **Advanced Configuration** option.</span></span>

    ![Enkel inloggning services-alternativ](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. <span data-ttu-id="dc801-171">Välj **lägga till nya**.</span><span class="sxs-lookup"><span data-stu-id="dc801-171">Select **Add New**.</span></span>

    ![Hej ”Lägg till ny” knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. <span data-ttu-id="dc801-173">Under **SAML 2.0 identitetsleverantörer**, hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-173">Under **SAML 2.0 Identity Providers**, do hello following:</span></span>

    ![Hej ”identitetsleverantörer SAML 2.0” avsnittet](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    <span data-ttu-id="dc801-175">a.</span><span class="sxs-lookup"><span data-stu-id="dc801-175">a.</span></span> <span data-ttu-id="dc801-176">I hello **visningsnamn** ange hello namn för din identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="dc801-176">In hello **Display Name** box, enter hello name of your identity provider.</span></span>

    <span data-ttu-id="dc801-177">b.</span><span class="sxs-lookup"><span data-stu-id="dc801-177">b.</span></span> <span data-ttu-id="dc801-178">I hello **identifiering** klistra in hello **SAML enhets-ID** som du kopierade från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc801-178">In hello **Entity Identifier** box, paste hello **SAML Entity ID** that you copied from hello Azure portal.</span></span> 

    <span data-ttu-id="dc801-179">c.</span><span class="sxs-lookup"><span data-stu-id="dc801-179">c.</span></span> <span data-ttu-id="dc801-180">I hello **SAML bindning** väljer **efter**.</span><span class="sxs-lookup"><span data-stu-id="dc801-180">In hello **SAML Binding** list box, select **POST**.</span></span>

    <span data-ttu-id="dc801-181">d.</span><span class="sxs-lookup"><span data-stu-id="dc801-181">d.</span></span> <span data-ttu-id="dc801-182">I hello **säkerhetstoken tjänstslutpunkten** klistra in hello **SAML enkel inloggning** URL som du kopierade från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc801-182">In hello **Security Token Service Endpoint** box, paste hello **SAML Single Sign-On Service** URL that you copied from hello Azure portal.</span></span> 
    
    <span data-ttu-id="dc801-183">e.</span><span class="sxs-lookup"><span data-stu-id="dc801-183">e.</span></span> <span data-ttu-id="dc801-184">I hello **användarnamn ID-attributet** ange `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="dc801-184">In hello **User ID Attribute Name** box, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="dc801-185">f.</span><span class="sxs-lookup"><span data-stu-id="dc801-185">f.</span></span> <span data-ttu-id="dc801-186">tooupload hello hämtat certifikat från Azure AD, Välj hello **överför** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc801-186">tooupload hello downloaded certificate from Azure AD, select hello **Upload** button.</span></span>
    
    <span data-ttu-id="dc801-187">g.</span><span class="sxs-lookup"><span data-stu-id="dc801-187">g.</span></span> <span data-ttu-id="dc801-188">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc801-188">Select **OK**.</span></span> 

12. <span data-ttu-id="dc801-189">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dc801-189">Select **Save**.</span></span>

    ![Hej ”spara”.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> <span data-ttu-id="dc801-191">När du ställer in hello app kan du läsa en kortare version av hello före instruktioner i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dc801-191">As you set up hello app, you can read a concise version of hello preceding instructions in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="dc801-192">När du har lagt till hello appen från hello **Active Directory** > **företagsprogram** avsnitt, Välj hello **enkel inloggning** fliken. Sedan åtkomst hello inbäddade dokumentation från hello **Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dc801-192">After you add hello app from hello **Active Directory** > **Enterprise applications** section, select hello **Single sign-on** tab. Then access hello embedded documentation from hello **Configuration** section.</span></span> 

<span data-ttu-id="dc801-193">toolearn mer om hello inbäddade dokumentationen funktionen finns [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="dc801-193">toolearn more about hello embedded documentation feature, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="dc801-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="dc801-194">Create an Azure AD test user</span></span>
<span data-ttu-id="dc801-195">I det här avsnittet skapar du testanvändare Britta Simon i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc801-195">In this section, you create test user Britta Simon in hello Azure portal.</span></span>

![hello testanvändare Britta Simon][100]

<span data-ttu-id="dc801-197">toocreate en testanvändare i Azure AD hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-197">toocreate a test user in Azure AD, do hello following:</span></span>

1. <span data-ttu-id="dc801-198">I hello **Azure-portalen**, i hello vänstra rutan, Välj hello **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc801-198">In hello **Azure portal**, in hello left pane, select hello **Azure Active Directory** button.</span></span>

    ![Hej ”Azure Active Directory”-knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dc801-200">toodisplay hello lista över användare, Välj **användare och grupper** > **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="dc801-200">toodisplay hello list of users, select **Users and groups** > **All users**.</span></span>
    
    ![Hej ”alla användare” länk](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    <span data-ttu-id="dc801-202">Hej **alla användare** öppnas.</span><span class="sxs-lookup"><span data-stu-id="dc801-202">hello **All users** dialog box opens.</span></span>

3. <span data-ttu-id="dc801-203">tooopen hello **användare** dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="dc801-203">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![Hej ”Lägg till”-knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dc801-205">I hello **användaren** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-205">In hello **User** dialog box, do hello following:</span></span>
 
    ![dialogrutan för hello ”användare”](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dc801-207">a.</span><span class="sxs-lookup"><span data-stu-id="dc801-207">a.</span></span> <span data-ttu-id="dc801-208">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dc801-208">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dc801-209">b.</span><span class="sxs-lookup"><span data-stu-id="dc801-209">b.</span></span> <span data-ttu-id="dc801-210">I hello **användarnamn** Skriv användaren Britta Simon **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="dc801-210">In hello **User name** box, type user Britta Simon's **email address**.</span></span>

    <span data-ttu-id="dc801-211">c.</span><span class="sxs-lookup"><span data-stu-id="dc801-211">c.</span></span> <span data-ttu-id="dc801-212">Välj hello **visa lösenordet** kryssrutan och Observera hello värdet som har genererats i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="dc801-212">Select hello **Show Password** check box, and then note hello value that was generated in hello **Password** box.</span></span>

    <span data-ttu-id="dc801-213">d.</span><span class="sxs-lookup"><span data-stu-id="dc801-213">d.</span></span> <span data-ttu-id="dc801-214">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dc801-214">Select **Create**.</span></span>
 
### <a name="create-a-cezanne-hr-software-test-user"></a><span data-ttu-id="dc801-215">Skapa en testanvändare för Cezanne HR-programvara</span><span class="sxs-lookup"><span data-stu-id="dc801-215">Create a Cezanne HR software test user</span></span>

<span data-ttu-id="dc801-216">tooenable Azure AD-användare toosign i tooCezanne HR-program, måste de etableras i Cezanne HR-programvara.</span><span class="sxs-lookup"><span data-stu-id="dc801-216">tooenable Azure AD users toosign in tooCezanne HR software, they must be provisioned into Cezanne HR software.</span></span> <span data-ttu-id="dc801-217">Hello gäller Cezanne HR programvara är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dc801-217">In hello case of Cezanne HR software, provisioning is a manual task.</span></span>

<span data-ttu-id="dc801-218">Konfigurera ett användarkonto genom att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-218">Provision a user account by doing hello following:</span></span>

1.  <span data-ttu-id="dc801-219">Logga in tooyour Cezanne HR programvara företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="dc801-219">Sign in tooyour Cezanne HR software company site as an administrator.</span></span>

2.  <span data-ttu-id="dc801-220">I hello vänster och välj **systeminställningarna** > **hantera användare** > **Lägg till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="dc801-220">In hello left pane, select **System Setup** > **Manage Users** > **Add New User**.</span></span>

    <span data-ttu-id="dc801-221">![Hej ”Lägg till nya användare” länken](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="dc801-221">![hello "Add New User" link](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "New User")</span></span>

3.  <span data-ttu-id="dc801-222">Under **detaljer**, hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-222">Under **Person Details**, do hello following:</span></span>

    <span data-ttu-id="dc801-223">![Hej ”Person” informationsavsnittet](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="dc801-223">![hello "Person Details" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "New User")</span></span>
    
    <span data-ttu-id="dc801-224">a.</span><span class="sxs-lookup"><span data-stu-id="dc801-224">a.</span></span> <span data-ttu-id="dc801-225">Ange **intern användare** som **OFF**.</span><span class="sxs-lookup"><span data-stu-id="dc801-225">Set **Internal User** as **OFF**.</span></span>
    
    <span data-ttu-id="dc801-226">b.</span><span class="sxs-lookup"><span data-stu-id="dc801-226">b.</span></span> <span data-ttu-id="dc801-227">I hello **Förnamn** rutan, typen hello användarens förnamn, till exempel **Britta**.</span><span class="sxs-lookup"><span data-stu-id="dc801-227">In hello **First Name** box, type hello user's first name, for example, **Britta**.</span></span>  
 
    <span data-ttu-id="dc801-228">c.</span><span class="sxs-lookup"><span data-stu-id="dc801-228">c.</span></span> <span data-ttu-id="dc801-229">I hello **efternamn** rutan, typen hello användarens efternamn, till exempel **Simon**.</span><span class="sxs-lookup"><span data-stu-id="dc801-229">In hello **Last Name** box, type hello user's last name, for example, **Simon**.</span></span>
    
    <span data-ttu-id="dc801-230">d.</span><span class="sxs-lookup"><span data-stu-id="dc801-230">d.</span></span> <span data-ttu-id="dc801-231">I hello **e-post** skriver hello användarens e-postadress, till exempel Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="dc801-231">In hello **E-mail** box, type hello user's email address, for example, Brittasimon@contoso.com.</span></span>

4.  <span data-ttu-id="dc801-232">Under **kontoinformation**, hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc801-232">Under **Account Information**, do hello following:</span></span>

    <span data-ttu-id="dc801-233">![Hej ”information om kontot”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="dc801-233">![hello "Account Information" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "New User")</span></span>
    
    <span data-ttu-id="dc801-234">a.</span><span class="sxs-lookup"><span data-stu-id="dc801-234">a.</span></span> <span data-ttu-id="dc801-235">I hello **användarnamn** skriver hello användarens e-postadress, till exempel Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="dc801-235">In hello **Username** box, type hello user's email address, for example, Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="dc801-236">b.</span><span class="sxs-lookup"><span data-stu-id="dc801-236">b.</span></span> <span data-ttu-id="dc801-237">I hello **lösenord** skriver hello användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="dc801-237">In hello **Password** box, type hello user's password.</span></span>
    
    <span data-ttu-id="dc801-238">c.</span><span class="sxs-lookup"><span data-stu-id="dc801-238">c.</span></span> <span data-ttu-id="dc801-239">I hello **säkerhetsroll** väljer **HR Professional**.</span><span class="sxs-lookup"><span data-stu-id="dc801-239">In hello **Security Role** box, select **HR Professional**.</span></span>
    
    <span data-ttu-id="dc801-240">d.</span><span class="sxs-lookup"><span data-stu-id="dc801-240">d.</span></span> <span data-ttu-id="dc801-241">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc801-241">Select **OK**.</span></span>

5. <span data-ttu-id="dc801-242">På hello **enkel inloggning** på fliken hello **SAML 2.0 identifierare** väljer **Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="dc801-242">On hello **Single sign-on** tab, in hello **SAML 2.0 Identifiers** section, select **Add New**.</span></span>

    <span data-ttu-id="dc801-243">![Hej ”Lägg till ny” knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "användare")</span><span class="sxs-lookup"><span data-stu-id="dc801-243">![hello "Add New" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "User")</span></span>

6. <span data-ttu-id="dc801-244">I hello **identitetsleverantör** Välj identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="dc801-244">In hello **Identity Provider** list box, select your identity provider.</span></span> <span data-ttu-id="dc801-245">I hello **användar-ID** ange hello e-postadress för test användaren Britta Simons konto.</span><span class="sxs-lookup"><span data-stu-id="dc801-245">In hello **User Identifier** box, enter hello email address for test user Britta Simon's account.</span></span>

    <span data-ttu-id="dc801-246">![Hej ”identitetsleverantör” och ”User ID”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "användare")</span><span class="sxs-lookup"><span data-stu-id="dc801-246">![hello "Identity Provider" and "User Identifier" boxes](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "User")</span></span>
    
7. <span data-ttu-id="dc801-247">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dc801-247">Select **Save**.</span></span>

    <span data-ttu-id="dc801-248">![Hej ”spara” knappen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "användare")</span><span class="sxs-lookup"><span data-stu-id="dc801-248">![hello "Save" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "User")</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="dc801-249">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="dc801-249">Assign hello Azure AD test user</span></span>

<span data-ttu-id="dc801-250">I det här avsnittet Aktivera testanvändare Britta Simon toouse Azure SSO genom att bevilja åtkomst tooCezanne HR programvara.</span><span class="sxs-lookup"><span data-stu-id="dc801-250">In this section, you enable test user Britta Simon toouse Azure SSO by granting access tooCezanne HR software.</span></span>

![Testa användaråtkomst][200] 

1. <span data-ttu-id="dc801-252">Öppna hello program och gå sedan toohello directory vyn i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc801-252">In hello Azure portal, open hello applications view and then go toohello directory view.</span></span> <span data-ttu-id="dc801-253">Välj **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dc801-253">Select **Enterprise applications** > **All applications**.</span></span>

    ![Hej ”alla program” länk][201] 

2. <span data-ttu-id="dc801-255">Välj i listan med program hello **Cezanne HR programvara**.</span><span class="sxs-lookup"><span data-stu-id="dc801-255">In hello applications list, select **Cezanne HR Software**.</span></span>

    ![listan med hello ”program”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. <span data-ttu-id="dc801-257">Välj hello menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="dc801-257">In hello menu on hello left, select **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="dc801-259">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="dc801-259">Select **Add**.</span></span> <span data-ttu-id="dc801-260">I hello **Lägg uppdrag** dialogrutan **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="dc801-260">Then in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![”Användare och grupper” länk][203]

5. <span data-ttu-id="dc801-262">I hello **användare och grupper** i dialogrutan hello **användare** väljer **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="dc801-262">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="dc801-263">I hello **användare och grupper** dialogrutan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="dc801-263">In hello **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="dc801-264">I hello **Lägg uppdrag** dialogrutan **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="dc801-264">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-sso"></a><span data-ttu-id="dc801-265">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dc801-265">Test SSO</span></span>

<span data-ttu-id="dc801-266">I det här avsnittet testa du konfigurationen av Azure AD SSO med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="dc801-266">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="dc801-267">När du väljer hello Cezanne HR programvara panelen i hello åtkomstpanelen logga in automatiskt tooyour Cezanne HR-program.</span><span class="sxs-lookup"><span data-stu-id="dc801-267">When you select hello Cezanne HR software tile in hello Access Panel, you sign on automatically tooyour Cezanne HR software application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc801-268">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc801-268">Next steps</span></span>

* [<span data-ttu-id="dc801-269">Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc801-269">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dc801-270">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dc801-270">What is application access and SSO with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

