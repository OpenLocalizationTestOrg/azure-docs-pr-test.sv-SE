---
title: "Självstudier: Integrera Azure Active Directory med vxMaintain | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och vxMaintain."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 937ea276d898986fc5a953c96fddabdc8940309f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a><span data-ttu-id="b47a9-103">Självstudier: Integrera Azure Active Directory med vxMaintain</span><span class="sxs-lookup"><span data-stu-id="b47a9-103">Tutorial: Integrate Azure Active Directory with vxMaintain</span></span>

<span data-ttu-id="b47a9-104">I kursen får du lära dig hur toointegrate vxMaintain med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b47a9-104">In this tutorial, you learn how toointegrate vxMaintain with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b47a9-105">Den här integreringen ger flera viktiga fördelar.</span><span class="sxs-lookup"><span data-stu-id="b47a9-105">This integration provides several important benefits.</span></span> <span data-ttu-id="b47a9-106">Du kan:</span><span class="sxs-lookup"><span data-stu-id="b47a9-106">You can:</span></span>

- <span data-ttu-id="b47a9-107">Kontrollen i Azure AD som har åtkomst till toovxMaintain.</span><span class="sxs-lookup"><span data-stu-id="b47a9-107">Control in Azure AD who has access toovxMaintain.</span></span>
- <span data-ttu-id="b47a9-108">Aktivera användare tooautomatically-inloggning toovxMaintain med enkel inloggning (SSO) med hjälp av sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="b47a9-108">Enable your users tooautomatically sign in toovxMaintain with single sign-on (SSO) by using their Azure AD accounts.</span></span>
- <span data-ttu-id="b47a9-109">Hantera dina konton i en central plats: hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b47a9-109">Manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="b47a9-110">toolearn mer om SaaS appintegrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b47a9-110">toolearn more about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b47a9-111">Krav</span><span class="sxs-lookup"><span data-stu-id="b47a9-111">Prerequisites</span></span>

<span data-ttu-id="b47a9-112">tooconfigure Azure AD-integrering med vxMaintain, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b47a9-112">tooconfigure Azure AD integration with vxMaintain, you need hello following items:</span></span>

- <span data-ttu-id="b47a9-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b47a9-113">An Azure AD subscription</span></span>
- <span data-ttu-id="b47a9-114">VxMaintain SSO-aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="b47a9-114">A vxMaintain SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b47a9-115">När du testar hello stegen i den här självstudiekursen, rekommenderar vi att du inte använder en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b47a9-115">When you test hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="b47a9-116">tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b47a9-116">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="b47a9-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b47a9-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b47a9-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b47a9-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b47a9-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b47a9-119">Scenario description</span></span>
<span data-ttu-id="b47a9-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b47a9-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="b47a9-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b47a9-121">hello scenario that this tutorial outlines consists of two main building blocks:</span></span>

* <span data-ttu-id="b47a9-122">Att lägga till vxMaintain från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b47a9-122">Adding vxMaintain from hello gallery</span></span>
* <span data-ttu-id="b47a9-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b47a9-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="add-vxmaintain-from-hello-gallery"></a><span data-ttu-id="b47a9-124">Lägg till vxMaintain från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b47a9-124">Add vxMaintain from hello gallery</span></span>
<span data-ttu-id="b47a9-125">tooconfigure hello integrering av vxMaintain med Azure AD, behöver du tooadd vxMaintain hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b47a9-125">tooconfigure hello integration of vxMaintain with Azure AD, you need tooadd vxMaintain from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b47a9-126">tooadd vxMaintain från galleriet hello hello följande:</span><span class="sxs-lookup"><span data-stu-id="b47a9-126">tooadd vxMaintain from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="b47a9-127">I hello [Azure-portalen](https://portal.azure.com), i hello vänstra rutan, Välj hello **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="b47a9-127">In hello [Azure portal](https://portal.azure.com), in hello left pane, select hello **Azure Active Directory** button.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="b47a9-129">Välj **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![Hej ”företagsprogram” fönstret][2]
    
3. <span data-ttu-id="b47a9-131">tooadd ett program i hello **alla program** dialogrutan **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-131">tooadd an application, in hello **All applications** dialog box, select **New application**.</span></span>

    ![Hej ”nya program” knappen][3]

4. <span data-ttu-id="b47a9-133">Skriv i sökrutan hello **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-133">In hello search box, type **vxMaintain**.</span></span>

    ![Hej ”enkel inloggning läget” listrutan](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. <span data-ttu-id="b47a9-135">Markera i hello resultat **vxMaintain**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-135">In hello results list, select **vxMaintain**, and then select **Add**.</span></span>

    ![Hej vxMaintain länk](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b47a9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b47a9-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="b47a9-138">I det här avsnittet kan du konfigurera och testa Azure AD SSO med hjälp av vxMaintain, baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b47a9-138">In this section, you configure and test Azure AD SSO by using vxMaintain, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b47a9-139">För enkel inloggning toowork måste Azure AD tooknow hello vxMaintain motsvarighet toohello Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="b47a9-139">For SSO toowork, Azure AD needs tooknow hello vxMaintain counterpart toohello Azure AD user.</span></span> <span data-ttu-id="b47a9-140">Det vill säga måste du upprätta en länk relation mellan hello Azure AD-användare och hello motsvarande vxMaintain användare.</span><span class="sxs-lookup"><span data-stu-id="b47a9-140">That is, you must establish a link relationship between hello Azure AD user and hello corresponding vxMaintain user.</span></span>

<span data-ttu-id="b47a9-141">tooestablish hello länken relationen, tilldela hello vxMaintain **användarnamn** värde som hello Azure AD **användarnamn** värde.</span><span class="sxs-lookup"><span data-stu-id="b47a9-141">tooestablish hello link relationship, assign hello vxMaintain **user name** value as hello Azure AD **Username** value.</span></span>

<span data-ttu-id="b47a9-142">tooconfigure och testa Azure AD enkel inloggning med hjälp av vxMaintain fullständig hello följande byggblock.</span><span class="sxs-lookup"><span data-stu-id="b47a9-142">tooconfigure and test Azure AD SSO by using vxMaintain, complete hello following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="b47a9-143">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b47a9-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="b47a9-144">I det här avsnittet kan du både aktivera Azure AD SSO i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet vxMaintain hello följande:</span><span class="sxs-lookup"><span data-stu-id="b47a9-144">In this section, you can both enable Azure AD SSO in hello Azure portal and configure SSO in your vxMaintain application by doing hello following:</span></span>

1. <span data-ttu-id="b47a9-145">I hello Azure-portalen på hello **vxMaintain** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-145">In hello Azure portal, on hello **vxMaintain** application integration page, select **Single sign-on**.</span></span>

    ![Hej ”enkel inloggning” kommando][4]

2. <span data-ttu-id="b47a9-147">tooenable SSO i hello **läget för enkel inloggning** listrutan, Välj **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-147">tooenable SSO, in hello **Single Sign-on Mode** drop-down list, select **SAML-based Sign-on**.</span></span>
 
    ![Hej ”SAML-baserade inloggning” kommando](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. <span data-ttu-id="b47a9-149">Under **vxMaintain domän och URL: er**, hello följande:</span><span class="sxs-lookup"><span data-stu-id="b47a9-149">Under **vxMaintain Domain and URLs**, do hello following:</span></span>

    ![Hej vxMaintain domän och URL: er](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    <span data-ttu-id="b47a9-151">a.</span><span class="sxs-lookup"><span data-stu-id="b47a9-151">a.</span></span> <span data-ttu-id="b47a9-152">I hello **identifierare** Skriv en URL som har hello följande syntax:`https://<company name>.verisae.com`</span><span class="sxs-lookup"><span data-stu-id="b47a9-152">In hello **Identifier** box, type a URL that has hello following syntax: `https://<company name>.verisae.com`</span></span>

    <span data-ttu-id="b47a9-153">b.</span><span class="sxs-lookup"><span data-stu-id="b47a9-153">b.</span></span> <span data-ttu-id="b47a9-154">I hello **Reply URL** Skriv en URL som har hello följande syntax:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span><span class="sxs-lookup"><span data-stu-id="b47a9-154">In hello **Reply URL** box, type a URL that has hello following syntax: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b47a9-155">hello föregående värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b47a9-155">hello preceding values are not real.</span></span> <span data-ttu-id="b47a9-156">Uppdatera dem med hello faktiska identifierare och reply URL.</span><span class="sxs-lookup"><span data-stu-id="b47a9-156">Update them with hello actual identifier and reply URL.</span></span> <span data-ttu-id="b47a9-157">tooobtain hello värden, kontakta hello [vxMaintain supportteamet](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="b47a9-157">tooobtain hello values, contact hello [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>
 
4. <span data-ttu-id="b47a9-158">Under **SAML-signeringscertifikat**väljer **XML-Metadata för**, och sedan spara hello metadata filen tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="b47a9-158">Under **SAML Signing Certificate**, select **Metadata XML**, and then save hello metadata file tooyour computer.</span></span>

    ![Hej ”SAML signeringscertifikat” avsnittet](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. <span data-ttu-id="b47a9-160">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-160">Select **Save**.</span></span>

    ![knappen Spara för hello](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b47a9-162">tooconfigure **vxMaintain** SSO, skicka hello hämtas **XML-Metadata för** filen toohello [vxMaintain supportteamet](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="b47a9-162">tooconfigure **vxMaintain** SSO, send hello downloaded **Metadata XML** file toohello [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="b47a9-163">När du ställer in hello app kan du läsa en kortare version av hello före instruktioner i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b47a9-163">As you set up hello app, you can read a concise version of hello preceding instructions in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b47a9-164">När du har lagt till hello appen från hello **Active Directory** > **företagsprogram** avsnitt, Välj hello **enkel inloggning** fliken och åtkomst hello inbäddade dokumentation från hello **Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b47a9-164">After you add hello app from hello **Active Directory** > **Enterprise Applications** section, select hello **Single Sign-On** tab, and then access hello embedded documentation from hello **Configuration** section.</span></span> 
>
><span data-ttu-id="b47a9-165">toolearn mer om hello inbäddade dokumentationen funktionen finns [hantera enkel inloggning för företagsappar](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="b47a9-165">toolearn more about hello embedded documentation feature, see [Managing single sign-on for enterprise apps](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b47a9-166">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b47a9-166">Create an Azure AD test user</span></span>
<span data-ttu-id="b47a9-167">I det här avsnittet skapar du testanvändare Britta Simon i hello Azure-portalen hello följande:</span><span class="sxs-lookup"><span data-stu-id="b47a9-167">In this section, you create test user Britta Simon in hello Azure portal by doing hello following:</span></span>

![hello Azure AD-testanvändare][100]

1. <span data-ttu-id="b47a9-169">I hello **Azure-portalen**, i hello vänstra rutan, Välj hello **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="b47a9-169">In hello **Azure portal**, in hello left pane, select hello **Azure Active Directory** button.</span></span>

    ![Hej ”Azure Active Directory”-knappen](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b47a9-171">toodisplay en lista över användare, gå för**användare och grupper** > **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-171">toodisplay a list of users, go too**Users and groups** > **All users**.</span></span>
    
    <span data-ttu-id="b47a9-172">![Hej ”alla användare” länk](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span><span class="sxs-lookup"><span data-stu-id="b47a9-172">![hello "All users" link](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span></span>  
    <span data-ttu-id="b47a9-173">Hej **alla användare** öppnas.</span><span class="sxs-lookup"><span data-stu-id="b47a9-173">hello **All users** dialog box opens.</span></span> 

3. <span data-ttu-id="b47a9-174">tooopen hello **användare** dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-174">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b47a9-176">I hello **användaren** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="b47a9-176">In hello **User** dialog box, do hello following:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b47a9-178">a.</span><span class="sxs-lookup"><span data-stu-id="b47a9-178">a.</span></span> <span data-ttu-id="b47a9-179">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-179">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b47a9-180">b.</span><span class="sxs-lookup"><span data-stu-id="b47a9-180">b.</span></span> <span data-ttu-id="b47a9-181">I hello **användarnamn** rutan typen hello användarens e-postadress test Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b47a9-181">In hello **User name** box, type hello email address of test user Britta Simon.</span></span>

    <span data-ttu-id="b47a9-182">c.</span><span class="sxs-lookup"><span data-stu-id="b47a9-182">c.</span></span> <span data-ttu-id="b47a9-183">Välj hello **visa lösenordet** kryssrutan och Observera hello värdet som har genererats i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="b47a9-183">Select hello **Show Password** check box, and then note hello value that was generated in hello **Password** box.</span></span>

    <span data-ttu-id="b47a9-184">d.</span><span class="sxs-lookup"><span data-stu-id="b47a9-184">d.</span></span> <span data-ttu-id="b47a9-185">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-185">Select **Create**.</span></span>
 
### <a name="create-a-vxmaintain-test-user"></a><span data-ttu-id="b47a9-186">Skapa en testanvändare vxMaintain</span><span class="sxs-lookup"><span data-stu-id="b47a9-186">Create a vxMaintain test user</span></span>

<span data-ttu-id="b47a9-187">I det här avsnittet skapar du testanvändare Britta Simon i vxMaintain.</span><span class="sxs-lookup"><span data-stu-id="b47a9-187">In this section, you create test user Britta Simon in vxMaintain.</span></span> <span data-ttu-id="b47a9-188">tooadd i hello vxMaintain platform arbeta med den [vxMaintain supportteamet](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="b47a9-188">tooadd users in hello vxMaintain platform, work with the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span> <span data-ttu-id="b47a9-189">Innan du använder SSO, skapa och aktivera hello användare.</span><span class="sxs-lookup"><span data-stu-id="b47a9-189">Before you use SSO, create and activate hello users.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="b47a9-190">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b47a9-190">Assign hello Azure AD test user</span></span>

<span data-ttu-id="b47a9-191">I det här avsnittet kan aktivera du testanvändare Britta Simon toouse Azure SSO genom att bevilja åtkomst toovxMaintain.</span><span class="sxs-lookup"><span data-stu-id="b47a9-191">In this section, you enable test user Britta Simon toouse Azure SSO by granting access toovxMaintain.</span></span> <span data-ttu-id="b47a9-192">toodo så hello följande:</span><span class="sxs-lookup"><span data-stu-id="b47a9-192">toodo so, do hello following:</span></span>

![Testanvändare i hello visningsnamn lista][200] 

1. <span data-ttu-id="b47a9-194">I hello Azure-portalen **program** visa finns för**Directory** Visa > **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-194">In hello Azure portal **Applications** view, go too**Directory** view > **Enterprise applications** > **All applications**.</span></span>

    ![Hej ”alla program” länk][201] 

2. <span data-ttu-id="b47a9-196">I hello **program** väljer **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-196">In hello **Applications** list, select **vxMaintain**.</span></span>

    ![Hej vxMaintain länk](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. <span data-ttu-id="b47a9-198">I hello vänster och välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-198">In hello left pane, select **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="b47a9-200">Välj **Lägg till** och sedan, i hello **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-200">Select **Add** and then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][203]

5. <span data-ttu-id="b47a9-202">I hello **användare och grupper** i dialogrutan hello **användare** väljer **Britta Simon**, och välj sedan hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="b47a9-202">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**, and then select hello **Select** button.</span></span>

7. <span data-ttu-id="b47a9-203">I hello **Lägg uppdrag** dialogrutan **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="b47a9-203">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-your-azure-ad-single-sign-on"></a><span data-ttu-id="b47a9-204">Testa din Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b47a9-204">Test your Azure AD single sign-on</span></span>

<span data-ttu-id="b47a9-205">I det här avsnittet testa du konfigurationen av Azure AD SSO med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b47a9-205">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="b47a9-206">Du väljer hello **vxMaintain** panelen i hello åtkomstpanelen ska logga in dig på tooyour vxMaintain program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b47a9-206">Selecting hello **vxMaintain** tile in hello Access Panel should sign you in tooyour vxMaintain application automatically.</span></span>

<span data-ttu-id="b47a9-207">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b47a9-207">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b47a9-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b47a9-208">Next steps</span></span>

* [<span data-ttu-id="b47a9-209">Lista över självstudier om att integrera SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b47a9-209">List of tutorials on integrating SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b47a9-210">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b47a9-210">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

