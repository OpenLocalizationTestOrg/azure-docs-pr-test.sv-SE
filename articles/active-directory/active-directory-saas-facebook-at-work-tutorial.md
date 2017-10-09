---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: fd19b3f178a2aee7dd2f204d6d3cf6df8fe6b444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="04229-103">Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook</span><span class="sxs-lookup"><span data-stu-id="04229-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="04229-104">I kursen får du lära dig hur toointegrate arbetsplats av Facebook med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="04229-104">In this tutorial, you learn how toointegrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="04229-105">Integrera arbetsplats av Facebook med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="04229-105">Integrating Workplace by Facebook with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="04229-106">Du kan styra i Azure AD som har åtkomst till tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="04229-106">You can control in Azure AD who has access tooWorkplace by Facebook.</span></span>
- <span data-ttu-id="04229-107">Du kan aktivera användarna tooautomatically hämta inloggad tooWorkplace av Facebook (enkel inloggning) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="04229-107">You can enable your users tooautomatically get signed on tooWorkplace by Facebook (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="04229-108">Du kan hantera dina konton i en central plats: hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04229-108">You can manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="04229-109">Mer information om programvara som en tjänst (SaaS) appintegrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="04229-109">For more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04229-110">Krav</span><span class="sxs-lookup"><span data-stu-id="04229-110">Prerequisites</span></span>

<span data-ttu-id="04229-111">tooconfigure Azure AD-integrering med arbetsplats av Facebook, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="04229-111">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="04229-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="04229-112">An Azure AD subscription</span></span>
- <span data-ttu-id="04229-113">En arbetsplats av Facebook enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="04229-113">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="04229-114">tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="04229-114">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="04229-115">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="04229-115">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="04229-116">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="04229-116">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="04229-117">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="04229-117">Scenario description</span></span>
<span data-ttu-id="04229-118">I kursen får testa du Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="04229-118">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="04229-119">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="04229-119">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="04229-120">Lägg till arbetsplatsen av Facebook från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="04229-120">Add Workplace by Facebook from hello gallery.</span></span>
2. <span data-ttu-id="04229-121">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="04229-121">Configure and test Azure AD single sign-on.</span></span>

## <a name="add-workplace-by-facebook-from-hello-gallery"></a><span data-ttu-id="04229-122">Lägg till arbetsplatsen av Facebook från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="04229-122">Add Workplace by Facebook from hello gallery</span></span>
<span data-ttu-id="04229-123">tooconfigure hello integrering av arbetsplatsen av Facebook i Azure AD, lägga till arbetsplatsen av Facebook från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="04229-123">tooconfigure hello integration of Workplace by Facebook into Azure AD, add Workplace by Facebook from hello gallery tooyour list of managed SaaS apps.</span></span>

1. <span data-ttu-id="04229-124">I hello [Azure-portalen](https://portal.azure.com), i hello vänstra rutan, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="04229-124">In hello [Azure portal](https://portal.azure.com), in hello left pane, select **Azure Active Directory**.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="04229-126">Bläddra för**företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="04229-126">Browse too**Enterprise applications** > **All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="04229-128">tooadd hello nya program, Välj **nytt program** överst hello hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="04229-128">tooadd hello new application, select **New application** on hello top of hello dialog box.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="04229-130">Skriv i sökrutan hello **arbetsplats av Facebook**, och välj **arbetsplats av Facebook** resultaten.</span><span class="sxs-lookup"><span data-stu-id="04229-130">In hello search box, type **Workplace by Facebook**, and select **Workplace by Facebook** from results.</span></span> <span data-ttu-id="04229-131">Välj sedan **Lägg till**, tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="04229-131">Then select **Add**, tooadd hello application.</span></span>

    ![Arbetsplats av Facebook i hello resultatlistan](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="04229-133">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="04229-133">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="04229-134">I det här avsnittet kan du konfigurera och testa Azure AD SSO till arbetsplats av Facebook, baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="04229-134">In this section, you configure and test Azure AD SSO with Workplace by Facebook, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="04229-135">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren arbetsplatsen av Facebook är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="04229-135">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Workplace by Facebook is tooa user in Azure AD.</span></span> <span data-ttu-id="04229-136">Med andra ord ska ett länkat förhållande mellan en Azure AD-användare och hello relaterade användare i arbetsplats av Facebook upprättas.</span><span class="sxs-lookup"><span data-stu-id="04229-136">In other words, a linked relationship between an Azure AD user and hello related user in Workplace by Facebook should be established.</span></span>

<span data-ttu-id="04229-137">Etablera den här relationen genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** arbetsplatsen av Facebook.</span><span class="sxs-lookup"><span data-stu-id="04229-137">Establish this relationship by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workplace by Facebook.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="04229-138">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="04229-138">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="04229-139">Du aktiverar Azure AD SSO i hello Azure-portalen i det här avsnittet och du konfigurerar enkel inloggning i din arbetsplats Facebook-programmet.</span><span class="sxs-lookup"><span data-stu-id="04229-139">In this section, you enable Azure AD SSO in hello Azure portal, and you configure SSO in your Workplace by Facebook application.</span></span>

1. <span data-ttu-id="04229-140">I hello Azure-portalen på hello **arbetsplats av Facebook** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="04229-140">In hello Azure portal, on hello **Workplace by Facebook** application integration page, select **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="04229-142">I hello **enkel inloggning** dialogrutan **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="04229-142">In hello **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on** tooenable SSO.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="04229-144">I hello **arbetsplats Facebook-domänen och URL: er** avsnittet, hello följande:</span><span class="sxs-lookup"><span data-stu-id="04229-144">In hello **Workplace by Facebook Domain and URLs** section, do hello following:</span></span>

    <span data-ttu-id="04229-145">a.</span><span class="sxs-lookup"><span data-stu-id="04229-145">a.</span></span> <span data-ttu-id="04229-146">I hello **inloggnings-URL** text skriver en URL som använder hello följande mönster:`https://<company subdomain>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="04229-146">In hello **Sign-on URL** text box, type a URL that uses hello following pattern: `https://<company subdomain>.facebook.com`</span></span>

    <span data-ttu-id="04229-147">b.</span><span class="sxs-lookup"><span data-stu-id="04229-147">b.</span></span> <span data-ttu-id="04229-148">I hello **identifierare** text skriver en URL som använder hello följande mönster:`https://www.facebook.com/company/<scim company id>`</span><span class="sxs-lookup"><span data-stu-id="04229-148">In hello **Identifier** text box, type a URL that uses hello following pattern: `https://www.facebook.com/company/<scim company id>`</span></span>

    > [!NOTE]
    > <span data-ttu-id="04229-149">Dessa värden är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="04229-149">These values are an example only.</span></span> <span data-ttu-id="04229-150">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="04229-150">Update these values with hello actual sign-on URL and identifier.</span></span> <span data-ttu-id="04229-151">Kontakta hello [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="04229-151">Contact hello [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) tooget these values.</span></span> 

4. <span data-ttu-id="04229-152">I hello **SAML-signeringscertifikat** väljer **certifikat (Base64)**, och sedan spara hello certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="04229-152">In hello **SAML Signing Certificate** section, select **Certificate (Base64)**, and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="04229-154">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="04229-154">Select **Save**.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="04229-156">I hello **arbetsplats Facebook konfigurationen** väljer **konfigurera arbetsplats av Facebook** tooopen hello **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="04229-156">In hello **Workplace by Facebook Configuration** section, select **Configure Workplace by Facebook** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="04229-157">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="04229-157">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference** section.</span></span>

    ![Arbetsplats av Facebook-konfiguration](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. <span data-ttu-id="04229-159">Logga in tooyour arbetsplats av Facebook företagets plats som en administratör i ett annat webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="04229-159">In a different web browser window, sign in tooyour Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="04229-160">Som en del av hello SAML autentiseringsprocessen, kan arbetsplatsen använda frågesträngar för in too2.5 kilobyte i storlek i ordning toopass parametrar tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="04229-160">As part of hello SAML authentication process, Workplace can use query strings of up too2.5 kilobytes in size in order toopass parameters tooAzure AD.</span></span>

8. <span data-ttu-id="04229-161">I hello **företagets instrumentpanelen**, gå toohello **autentisering** fliken.</span><span class="sxs-lookup"><span data-stu-id="04229-161">In hello **Company Dashboard**, go toohello **Authentication** tab.</span></span>

9. <span data-ttu-id="04229-162">Under **SAML-autentisering**väljer **SSO endast** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="04229-162">Under **SAML Authentication**, select **SSO Only** from hello drop-down list.</span></span>

10. <span data-ttu-id="04229-163">Ange värden för hello kopieras från hello **arbetsplats Facebook konfigurationen** avsnitt i hello Azure-portalen i hello motsvarande fält:</span><span class="sxs-lookup"><span data-stu-id="04229-163">Enter hello values copied from hello **Workplace by Facebook Configuration** section of hello Azure portal into hello corresponding fields:</span></span>

    *   <span data-ttu-id="04229-164">I den **SAML URL** klistra in hello värdet för textrutan **inloggning tjänst-URL för enkel**, som du har kopierat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04229-164">In the **SAML URL** text box, paste hello value of **Single Sign-On Service URL**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="04229-165">I den **utfärdar-URL för SAML** klistra in hello värdet för textrutan **SAML enhets-ID**, som du har kopierat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04229-165">In the **SAML Issuer URL** text box, paste hello value of **SAML Entity ID**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="04229-166">I **SAML logga ut omdirigera (valfritt)**, klistra in hello värdet för **Sign-Out URL**, som du har kopierat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04229-166">In **SAML Logout Redirect (optional)**, paste hello value of **Sign-Out URL**, which you have copied from hello Azure portal.</span></span>
    *   <span data-ttu-id="04229-167">Öppna din **Base64-kodat certifikat** i anteckningar som hämtats från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04229-167">Open your **base-64 encoded certificate** in Notepad, downloaded from hello Azure portal.</span></span> <span data-ttu-id="04229-168">Kopiera hello innehållet i den till Urklipp och klistra in den toothe **SAML certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="04229-168">Copy hello content of it into your clipboard, and then paste it toothe **SAML Certificate** text box.</span></span>

11. <span data-ttu-id="04229-169">Du kan behöva tooenter hello målgruppen URL mottagande URL och ACS (Assertion konsumenten Service) URL som visas under hello **SAML-konfiguration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="04229-169">You might need tooenter hello audience URL, recipient URL, and ACS (Assertion Consumer Service) URL, listed under hello **SAML Configuration** section.</span></span>

12. <span data-ttu-id="04229-170">Rulla toohello längst ned i avsnittet hello och markera **Test SSO**.</span><span class="sxs-lookup"><span data-stu-id="04229-170">Scroll toohello bottom of hello section, and select **Test SSO**.</span></span> <span data-ttu-id="04229-171">Ett popup-fönster visas med hello Azure AD-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="04229-171">A pop-up window appears, with hello Azure AD sign-in page.</span></span> <span data-ttu-id="04229-172">tooauthenticate, ange dina autentiseringsuppgifter som vanligt.</span><span class="sxs-lookup"><span data-stu-id="04229-172">tooauthenticate, enter your credentials as normal.</span></span> <span data-ttu-id="04229-173">Kontrollera hello e-postadress returnerade tillbaka från Azure AD är hello samma som hello arbetsplatskonto som du är inloggad med.</span><span class="sxs-lookup"><span data-stu-id="04229-173">Ensure hello email address returned back from Azure AD is hello same as hello Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="04229-174">Rulla toohello längst ned på sidan hello och välj om hello test har slutförts, **spara**.</span><span class="sxs-lookup"><span data-stu-id="04229-174">If hello test has finished successfully, scroll toohello bottom of hello page and select **Save**.</span></span>

14. <span data-ttu-id="04229-175">Alla som använder arbetsplats kan nu se med Azure AD-inloggningssida för autentisering.</span><span class="sxs-lookup"><span data-stu-id="04229-175">Anyone using Workplace is now presented with Azure AD sign-in page for authentication.</span></span>

<span data-ttu-id="04229-176">Du kan välja tooconfigure SAML utloggning URL, vilket kan vara används toopoint vid utloggning hello Azure AD-sidan.</span><span class="sxs-lookup"><span data-stu-id="04229-176">You can choose tooconfigure a SAML sign out URL, which can be used toopoint at hello Azure AD sign-out page.</span></span> <span data-ttu-id="04229-177">När den här inställningen har aktiverats och konfigurerats, hello användaren är inte längre utloggning sidan för dirigerad toohello arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="04229-177">When this setting is enabled and configured, hello user is no longer directed toohello Workplace sign-out page.</span></span> <span data-ttu-id="04229-178">I stället är hello användaren omdirigerade toohello URL som har lagts till i hello SAML utloggning omdirigering inställningen.</span><span class="sxs-lookup"><span data-stu-id="04229-178">Instead, hello user is redirected toohello URL that was added in hello SAML sign-out redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="04229-179">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello app.</span><span class="sxs-lookup"><span data-stu-id="04229-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app.</span></span> <span data-ttu-id="04229-180">När du lägger till den här appen från hello **Active Directory** > **företagsprogram** väljer du helt enkelt hello **enkel inloggning** fliken och åtkomst hello inbäddade dokumentationen via hello **Configuration** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="04229-180">After adding this app from hello **Active Directory** > **Enterprise Applications** section, simply select hello **Single Sign-On** tab, and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="04229-181">Du kan läsa mer om hello inbäddade dokumentationen funktionen i hello [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="04229-181">You can read more about hello embedded documentation feature in hello [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="configure-reauthentication-frequency"></a><span data-ttu-id="04229-182">Konfigurera omautentisering frekvens</span><span class="sxs-lookup"><span data-stu-id="04229-182">Configure reauthentication frequency</span></span>

<span data-ttu-id="04229-183">Du kan konfigurera arbetsplats tooprompt för SAML kontroll varje dag, tre dagar, en vecka, två veckor, en månad eller aldrig.</span><span class="sxs-lookup"><span data-stu-id="04229-183">You can configure Workplace tooprompt for a SAML check every day, three days, one week, two weeks, one month, or never.</span></span>

> [!NOTE] 
><span data-ttu-id="04229-184">hello minsta hello SAML-kontroll av mobila program värdet för tooone vecka.</span><span class="sxs-lookup"><span data-stu-id="04229-184">hello minimum value for hello SAML check on mobile applications is set tooone week.</span></span>

<span data-ttu-id="04229-185">Du kan också tvinga en SAML återställa för alla användare.</span><span class="sxs-lookup"><span data-stu-id="04229-185">You can also force a SAML reset for all users.</span></span> <span data-ttu-id="04229-186">toodo detta, Använd hello **kräver SAML-autentisering för alla användare nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="04229-186">toodo this, use hello **Require SAML authentication for all users now** button.</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="04229-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="04229-187">Create an Azure AD test user</span></span>
<span data-ttu-id="04229-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="04229-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

1. <span data-ttu-id="04229-190">I hello **Azure-portalen**, i hello vänstra rutan, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="04229-190">In hello **Azure portal**, in hello left pane, select **Azure Active Directory**.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="04229-192">toodisplay hello lista över användare, gå för**användare och grupper**, och välj **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="04229-192">toodisplay hello list of users, go too**Users and groups**, and select **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="04229-194">tooopen hello **användare** dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="04229-194">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="04229-196">I hello **användaren** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="04229-196">In hello **User** dialog box, do hello following:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="04229-198">a.</span><span class="sxs-lookup"><span data-stu-id="04229-198">a.</span></span> <span data-ttu-id="04229-199">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="04229-199">In hello **Name** text box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="04229-200">b.</span><span class="sxs-lookup"><span data-stu-id="04229-200">b.</span></span> <span data-ttu-id="04229-201">I hello **användarnamn** textruta, typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="04229-201">In hello **User name** text box, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="04229-202">c.</span><span class="sxs-lookup"><span data-stu-id="04229-202">c.</span></span> <span data-ttu-id="04229-203">Välj **visa lösenordet**, och Skriv ned.</span><span class="sxs-lookup"><span data-stu-id="04229-203">Select **Show Password**, and write it down.</span></span>

    <span data-ttu-id="04229-204">d.</span><span class="sxs-lookup"><span data-stu-id="04229-204">d.</span></span> <span data-ttu-id="04229-205">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="04229-205">Select **Create**.</span></span>
 
### <a name="create-a-workplace-by-facebook-test-user"></a><span data-ttu-id="04229-206">Skapa en arbetsplats av Facebook testanvändare</span><span class="sxs-lookup"><span data-stu-id="04229-206">Create a Workplace by Facebook test user</span></span>

<span data-ttu-id="04229-207">I det här avsnittet skapas en användare som kallas Britta Simon i arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="04229-207">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="04229-208">Arbetsplats av Facebook stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="04229-208">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="04229-209">Det finns inga åtgärder i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="04229-209">There is no action for you in this section.</span></span> <span data-ttu-id="04229-210">Om en användare inte finns i arbetsplats av Facebook, skapas en ny när du försöker tooaccess arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="04229-210">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt tooaccess Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="04229-211">Om du behöver toocreate en användare manuellt kan kontakta hello [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/).</span><span class="sxs-lookup"><span data-stu-id="04229-211">If you need toocreate a user manually, contact hello [Workplace by Facebook Client support team](https://workplace.fb.com/faq/).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="04229-212">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="04229-212">Assign hello Azure AD test user</span></span>

<span data-ttu-id="04229-213">I det här avsnittet kan aktivera du Britta Simon toouse Azure SSO genom att bevilja åtkomst tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="04229-213">In this section, you enable Britta Simon toouse Azure SSO by granting access tooWorkplace by Facebook.</span></span>

![Tilldela användare][200] 

1. <span data-ttu-id="04229-215">I hello Azure visas portal, öppna hello program.</span><span class="sxs-lookup"><span data-stu-id="04229-215">In hello Azure portal, open hello applications view.</span></span> <span data-ttu-id="04229-216">Gå toohello directory vyn finns för**företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="04229-216">Go toohello directory view, go too**Enterprise applications**, and then select **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="04229-218">Välj i listan med program hello **arbetsplats av Facebook**.</span><span class="sxs-lookup"><span data-stu-id="04229-218">In hello applications list, select **Workplace by Facebook**.</span></span>

    ![hello arbetsplats av Facebook-länken i listan med program hello](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="04229-220">Välj hello menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="04229-220">In hello menu on hello left, select **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="04229-222">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="04229-222">Select **Add**.</span></span> <span data-ttu-id="04229-223">Sedan hello **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="04229-223">Then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="04229-225">I hello **användare och grupper** dialogrutan **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="04229-225">In hello **Users and groups** dialog box, select **Britta Simon** in hello users list.</span></span>

6. <span data-ttu-id="04229-226">I hello **användare och grupper** dialogrutan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="04229-226">In hello **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="04229-227">I hello **Lägg uppdrag** dialogrutan **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="04229-227">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="04229-228">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="04229-228">Test single sign-on</span></span>

<span data-ttu-id="04229-229">Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="04229-229">If you want tootest your SSO settings, open hello Access Panel.</span></span>
<span data-ttu-id="04229-230">Mer information finns i [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="04229-230">For more information, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="04229-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="04229-231">Next steps</span></span>

* <span data-ttu-id="04229-232">Se hello [lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="04229-232">See hello [list of tutorials on how toointegrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md).</span></span>
* <span data-ttu-id="04229-233">Läs [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="04229-233">Read [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>
* <span data-ttu-id="04229-234">Läs mer om hur för[konfigurera användaretablering](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="04229-234">Learn more about how too[configure user provisioning](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span></span>



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

