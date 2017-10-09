---
title: "Självstudier: Azure Active Directory-integrering med hjälp Scout | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och hjälpa Scout."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 58edd140eb1eb5980796ca743b5f7acd891729a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a><span data-ttu-id="0581b-103">Självstudier: Azure Active Directory-integrering med hjälp Scout</span><span class="sxs-lookup"><span data-stu-id="0581b-103">Tutorial: Azure Active Directory integration with Help Scout</span></span>

<span data-ttu-id="0581b-104">I kursen får du lära dig hur toointegrate hjälpa säljare med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0581b-104">In this tutorial, you learn how toointegrate Help Scout with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0581b-105">Du får hello följande fördelar från hjälp Scout integrera med Azure AD:</span><span class="sxs-lookup"><span data-stu-id="0581b-105">You get hello following benefits from integrating Help Scout with Azure AD:</span></span>

- <span data-ttu-id="0581b-106">Du kan styra vem som har åtkomst tooHelp Scout i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0581b-106">In Azure AD, you can control who has access tooHelp Scout.</span></span>
- <span data-ttu-id="0581b-107">Du kan logga in automatiskt dina användare tooHelp Scout med hjälp av enkel inloggning och användarens Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="0581b-107">You can automatically sign in your users tooHelp Scout by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="0581b-108">Du kan hantera dina konton i en enda central plats, hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0581b-108">You can manage your accounts in one, central location, hello Azure portal.</span></span>

<span data-ttu-id="0581b-109">toolearn mer information om programvara som en tjänst (SaaS) app-integrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0581b-109">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0581b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0581b-110">Prerequisites</span></span>

<span data-ttu-id="0581b-111">tooset av Azure AD-integrering med hjälp Scout måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0581b-111">tooset up Azure AD integration with Help Scout, you need hello following items:</span></span>

- <span data-ttu-id="0581b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0581b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0581b-113">En prenumeration som hjälper Scout med enkel inloggning aktiverad</span><span class="sxs-lookup"><span data-stu-id="0581b-113">A Help Scout subscription, with single sign-on turned on</span></span> 

> [!NOTE]
> <span data-ttu-id="0581b-114">Om du testar hello stegen i den här kursen rekommenderar vi att du inte testa dem i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0581b-114">If you test hello steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="0581b-115">Rekommendationer för att testa hello stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="0581b-115">Recommendations for testing hello steps in this tutorial:</span></span>

- <span data-ttu-id="0581b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0581b-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="0581b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [skaffa en kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0581b-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0581b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0581b-118">Scenario description</span></span>
<span data-ttu-id="0581b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0581b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="0581b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0581b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0581b-121">Lägg till hjälp Scout från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="0581b-121">Add Help Scout from hello gallery.</span></span>
2. <span data-ttu-id="0581b-122">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0581b-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-help-scout-from-hello-gallery"></a><span data-ttu-id="0581b-123">Lägg till hjälp Scout från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0581b-123">Add Help Scout from hello gallery</span></span>
<span data-ttu-id="0581b-124">tooset in hello integration av hjälpa Scout med Azure AD hello Gallery och lägga till hjälp Scout tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="0581b-124">tooset up hello integration of Help Scout with Azure AD, in hello gallery, add Help Scout tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0581b-125">tooadd hjälper Scout från galleriet hello:</span><span class="sxs-lookup"><span data-stu-id="0581b-125">tooadd Help Scout from hello gallery:</span></span>

1. <span data-ttu-id="0581b-126">I hello [Azure-portalen](https://portal.azure.com), i hello vänstra menyn, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0581b-126">In hello [Azure portal](https://portal.azure.com), in hello left menu, select **Azure Active Directory**.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="0581b-128">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0581b-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![hello Enterprise program sida][2]
    
3. <span data-ttu-id="0581b-130">tooadd nya program, Välj **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="0581b-130">tooadd a new application, select **New application**.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="0581b-132">Skriv i sökrutan hello **hjälp Scout**.</span><span class="sxs-lookup"><span data-stu-id="0581b-132">In hello search box, enter **Help Scout**.</span></span> <span data-ttu-id="0581b-133">I sökresultaten hello väljer **hjälp Scout**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0581b-133">In hello search results, select **Help Scout**, and then select **Add**.</span></span>

    ![Underlätta Scout hello resultatlistan](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="0581b-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0581b-135">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="0581b-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med hjälp Scout baserat på en användare med namnet *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="0581b-136">In this section, you set up and test Azure AD single sign-on with Help Scout based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="0581b-137">För enkel inloggning toowork måste Azure AD tooknow hello Azure AD motsvarighet användare i Hjälp Scout.</span><span class="sxs-lookup"><span data-stu-id="0581b-137">For single sign-on toowork, Azure AD needs tooknow hello Azure AD counterpart user in Help Scout.</span></span> <span data-ttu-id="0581b-138">En länk relationen mellan en Azure AD-användare och hello relaterade användare i Hjälp Scout måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="0581b-138">A link relationship between an Azure AD user and hello related user in Help Scout must be established.</span></span>

<span data-ttu-id="0581b-139">tooestablish hello länka relation i Hjälp Scout för **användarnamn**, tilldela hello värdet för hello **användarnamn** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0581b-139">tooestablish hello link relationship, in Help Scout, for **Username**, assign hello value of hello **user name** in Azure AD.</span></span>

<span data-ttu-id="0581b-140">tooconfigure och testa Azure AD enkel inloggning med hjälp Scout fullständig hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="0581b-140">tooconfigure and test Azure AD single sign-on with Help Scout, complete hello following tasks:</span></span>

1. <span data-ttu-id="0581b-141">[Konfigurera Azure AD enkel inloggning](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="0581b-141">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="0581b-142">Ställer in en användare toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0581b-142">Sets up a user toouse this feature.</span></span>
2. <span data-ttu-id="0581b-143">[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="0581b-143">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="0581b-144">Testerna Azure AD enkel inloggning med hello användaren Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0581b-144">Tests Azure AD single sign-on with hello user Britta Simon.</span></span>
3. <span data-ttu-id="0581b-145">[Skapa en testanvändare hjälpa Scout](#create-a-help-scout-test-user).</span><span class="sxs-lookup"><span data-stu-id="0581b-145">[Create a Help Scout test user](#create-a-help-scout-test-user).</span></span> <span data-ttu-id="0581b-146">Skapar en motsvarighet för Britta Simon i Hjälp Scout som är länkade toohello Azure AD-representation av hello användare.</span><span class="sxs-lookup"><span data-stu-id="0581b-146">Creates a counterpart of Britta Simon in Help Scout that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="0581b-147">[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="0581b-147">[Assign hello Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="0581b-148">Ställer in Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0581b-148">Sets up Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0581b-149">[Testa enkel inloggning](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="0581b-149">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="0581b-150">Verifierar att hello-konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0581b-150">Verifies that hello configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="0581b-151">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0581b-151">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="0581b-152">I det här avsnittet kan du ställa in Azure AD enkel inloggning i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0581b-152">In this section, you set up Azure AD single sign-on in hello Azure portal.</span></span> <span data-ttu-id="0581b-153">Sedan kan konfigurera du enkel inloggning i tillämpningsprogrammet att Scout.</span><span class="sxs-lookup"><span data-stu-id="0581b-153">Then, you set up single sign-on in your Help Scout application.</span></span>

<span data-ttu-id="0581b-154">tooset in Azure AD enkel inloggning med hjälp Scout:</span><span class="sxs-lookup"><span data-stu-id="0581b-154">tooset up Azure AD single sign-on with Help Scout:</span></span>

1. <span data-ttu-id="0581b-155">I hello Azure-portalen på hello **hjälp Scout** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0581b-155">In hello Azure portal, on hello **Help Scout** application integration page, select **Single sign-on**.</span></span>
 
    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="0581b-157">På hello **enkel inloggning** sidan för **läge**väljer **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0581b-157">On hello **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. <span data-ttu-id="0581b-159">Under **hjälp Scout domän och URL: er**om du vill tooset in hello program i IDP-initierad läge fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0581b-159">Under **Help Scout Domain and URLs**, if you want tooset up hello application in IDP-initiated mode, complete hello following steps:</span></span>

    1. <span data-ttu-id="0581b-160">I hello **identifierare** ange en URL som har hello följande mönster:`urn:auth0:helpscout:<instancename>`</span><span class="sxs-lookup"><span data-stu-id="0581b-160">In hello **Identifier** box, enter a URL that has hello following pattern: `urn:auth0:helpscout:<instancename>`</span></span>

    2. <span data-ttu-id="0581b-161">I hello **Reply URL** ange en URL som har hello följande mönster:`https://helpscout.auth0.com/login/callback?connection=<instancename>`</span><span class="sxs-lookup"><span data-stu-id="0581b-161">In hello **Reply URL** box, enter a URL that has hello following pattern: `https://helpscout.auth0.com/login/callback?connection=<instancename>`</span></span>

    ![Hjälpinformation Scout domän URL: er och enkel inloggning](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. <span data-ttu-id="0581b-163">Om du vill tooset in hello program i SP-initierad läge, väljer hello **visa avancerade inställningar för URL: en** kryssrutan och sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="0581b-163">If you want tooset up hello application in SP-initiated mode, select hello **Show advanced URL settings** check box, and then do hello following:</span></span>

    * <span data-ttu-id="0581b-164">I hello **inloggning URL** ange en URL som har hello följande format:`https://secure.helpscout.net/members/login/`</span><span class="sxs-lookup"><span data-stu-id="0581b-164">In hello **Sign on URL** box, enter a URL that has hello following format: `https://secure.helpscout.net/members/login/`</span></span>

    ![Hjälpinformation Scout domän URL: er och enkel inloggning](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > <span data-ttu-id="0581b-166">hello värden i dessa URL: er är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="0581b-166">hello values in these URLs are for demonstration only.</span></span> <span data-ttu-id="0581b-167">Uppdatera hello värden med hello faktiska identifierare Webbadressen och svar.</span><span class="sxs-lookup"><span data-stu-id="0581b-167">Update hello values with hello actual identifier URL and reply URL.</span></span> <span data-ttu-id="0581b-168">tooget dessa värden, kontakta [hjälp Scout supportteamet](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="0581b-168">tooget these values, contact [Help Scout support team](mailto:help@helpscout.com).</span></span> 

5. <span data-ttu-id="0581b-169">Under **SAML-signeringscertifikat**väljer **XML-Metadata för**, och sedan spara hello metadatafil på datorn.</span><span class="sxs-lookup"><span data-stu-id="0581b-169">Under **SAML Signing Certificate**, select **Metadata XML**, and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. <span data-ttu-id="0581b-171">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0581b-171">Select **Save**.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="0581b-173">tooset in enkel inloggning på hello hjälpa Scout sida, skicka hello hämtas metadata XML-filen toohello [hjälp Scout supportteamet](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="0581b-173">tooset up single sign-on on hello Help Scout side, send hello downloaded metadata XML file toohello [Help Scout support team](mailto:help@helpscout.com).</span></span> <span data-ttu-id="0581b-174">hello hjälpa Scout supportteamet gäller den här inställningen så att hello SAML enkel inloggning-anslutning är korrekt inställda på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="0581b-174">hello Help Scout support team applies this setting so that hello SAML single sign-on connection is set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="0581b-175">Du kan läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="0581b-175">You can read a concise version of these instructions in hello [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="0581b-176">När du har lagt till hello appen genom att välja **Active Directory** > **företagsprogram**väljer hello **enkel inloggning** fliken. Du kan komma åt inbäddade hello-dokumentationen i hello **Configuration** avsnitt på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="0581b-176">After you add hello app by selecting **Active Directory** > **Enterprise Applications**, select hello **Single Sign-On** tab. You can access hello embedded documentation in hello **Configuration** section, at hello bottom of hello page.</span></span> <span data-ttu-id="0581b-177">Mer information finns i [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="0581b-177">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="0581b-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0581b-178">Create an Azure AD test user</span></span>

<span data-ttu-id="0581b-179">I det här avsnittet i hello Azure-portalen skapar du en användare med namnet Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0581b-179">In this section, in hello Azure portal, you create a test user named Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="0581b-181">toocreate en testanvändare i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="0581b-181">toocreate a test user in Azure AD:</span></span>

1. <span data-ttu-id="0581b-182">Välj i hello Azure-portalen hello vänstra menyn **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0581b-182">In hello Azure portal, in hello left menu, select **Azure Active Directory**.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="0581b-184">toodisplay hello lista över användare, Välj **användare och grupper**, och välj sedan **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0581b-184">toodisplay hello list of users, select **Users and groups**, and then select **All users**.</span></span>

    ![Välj användare och grupper och välj sedan alla användare](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="0581b-186">tooopen hello **användare** dialogrutan hello överst i hello **alla användare** väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0581b-186">tooopen hello **User** dialog box, at hello top of hello **All Users** page, select **Add**.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="0581b-188">I hello **användaren** dialogrutan, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0581b-188">In hello **User** dialog box, complete hello following steps:</span></span>

    1. <span data-ttu-id="0581b-189">I hello **namn** ange **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0581b-189">In hello **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="0581b-190">I hello **användarnamn** ange hello användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="0581b-190">In hello **User name** box, enter hello email address of user Britta Simon.</span></span>

    3. <span data-ttu-id="0581b-191">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="0581b-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    4. <span data-ttu-id="0581b-192">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0581b-192">Select **Create**.</span></span>

        ![hello användardialogrutan](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a><span data-ttu-id="0581b-194">Skapa en hjälp Scout testanvändare</span><span class="sxs-lookup"><span data-stu-id="0581b-194">Create a Help Scout test user</span></span>

<span data-ttu-id="0581b-195">hello-objekt för det här avsnittet är toocreate en användare med namnet Britta Simon i Hjälp Scout.</span><span class="sxs-lookup"><span data-stu-id="0581b-195">hello object of this section is toocreate a user named Britta Simon in Help Scout.</span></span> <span data-ttu-id="0581b-196">Hjälp Scout stöder just-in-time (JIT) etablering, som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="0581b-196">Help Scout supports just-in-time (JIT) provisioning, which is turned on by default.</span></span>

<span data-ttu-id="0581b-197">Det finns ingen åtgärd eller uppgift toocomplete i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0581b-197">In this section, there's no action or task toocomplete.</span></span> <span data-ttu-id="0581b-198">Om en användare inte redan finns i Hjälp Scout, skapas en ny när du försöker tooaccess hjälpa Scout.</span><span class="sxs-lookup"><span data-stu-id="0581b-198">If a user doesn't already exist in Help Scout, a new one is created when you attempt tooaccess Help Scout.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="0581b-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="0581b-199">Assign hello Azure AD test user</span></span>

<span data-ttu-id="0581b-200">I det här avsnittet Tillåt hello användaren Britta Simon toouse Azure AD enkel inloggning genom att tilldela hello användarens konto åtkomst tooHelp Scout.</span><span class="sxs-lookup"><span data-stu-id="0581b-200">In this section, you allow hello user Britta Simon toouse Azure AD single sign-on by granting hello user account access tooHelp Scout.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="0581b-202">tooassign Britta Simon tooHelp Scout:</span><span class="sxs-lookup"><span data-stu-id="0581b-202">tooassign Britta Simon tooHelp Scout:</span></span>

1. <span data-ttu-id="0581b-203">Öppna hello program i hello Azure-portalen, och sedan gå toohello directory vyn.</span><span class="sxs-lookup"><span data-stu-id="0581b-203">In hello Azure portal, open hello applications view, and then go toohello directory view.</span></span> <span data-ttu-id="0581b-204">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0581b-204">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0581b-206">Välj i listan med program hello **hjälp Scout**.</span><span class="sxs-lookup"><span data-stu-id="0581b-206">In hello applications list, select **Help Scout**.</span></span>

    ![hello hjälpa Scout länken i listan med program hello](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. <span data-ttu-id="0581b-208">Välj hello vänstra menyn **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0581b-208">In hello left menu, select **Users and groups**.</span></span>

    ![hello användare och grupper länk][202]

4. <span data-ttu-id="0581b-210">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0581b-210">Select **Add**.</span></span> <span data-ttu-id="0581b-211">Klicka sedan på hello **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0581b-211">Then, on hello **Add Assignment** page, select **Users and groups**.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="0581b-213">På hello **användare och grupper** i hello lista över användare, Välj sidan **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0581b-213">On hello **Users and groups** page, in hello list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="0581b-214">På hello **användare och grupper** väljer **Välj**.</span><span class="sxs-lookup"><span data-stu-id="0581b-214">On hello **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="0581b-215">På hello **Lägg uppdrag** väljer **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="0581b-215">On hello **Add Assignment** page, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="0581b-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0581b-216">Test single sign-on</span></span>

<span data-ttu-id="0581b-217">I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0581b-217">In this section, you test your Azure AD single sign-on configuration by using hello access panel.</span></span>

<span data-ttu-id="0581b-218">När du väljer hello hjälpa Scout panelen i hello åtkomstpanelen bör vara loggas du automatiskt tooyour hjälpa Scout program.</span><span class="sxs-lookup"><span data-stu-id="0581b-218">When you select hello Help Scout tile in hello access panel, you should be automatically signed in tooyour Help Scout application.</span></span>

<span data-ttu-id="0581b-219">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0581b-219">For more information about the access panel, see [Introduction toohello access panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0581b-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0581b-220">Additional resources</span></span>

* [<span data-ttu-id="0581b-221">Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0581b-221">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0581b-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0581b-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

