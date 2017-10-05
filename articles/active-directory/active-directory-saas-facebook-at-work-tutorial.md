---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
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
ms.openlocfilehash: 27e62a00832484667117d8718db9f2fc05e2f4e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="841d5-103">Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook</span><span class="sxs-lookup"><span data-stu-id="841d5-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="841d5-104">I kursen får lära du att integrera arbetsplats av Facebook med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="841d5-104">In this tutorial, you learn how to integrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="841d5-105">Integrera arbetsplats av Facebook med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="841d5-105">Integrating Workplace by Facebook with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="841d5-106">Du kan styra i Azure AD som har åtkomst till arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="841d5-106">You can control in Azure AD who has access to Workplace by Facebook.</span></span>
- <span data-ttu-id="841d5-107">Du kan aktivera användarna att automatiskt hämta loggat in på arbetsplatsen av Facebook (enkel inloggning) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="841d5-107">You can enable your users to automatically get signed on to Workplace by Facebook (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="841d5-108">Du kan hantera dina konton i en central plats: Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="841d5-108">You can manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="841d5-109">Mer information om programvara som en tjänst (SaaS) appintegrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="841d5-109">For more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="841d5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="841d5-110">Prerequisites</span></span>

<span data-ttu-id="841d5-111">För att konfigurera Azure AD-integrering med arbetsplats av Facebook, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="841d5-111">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="841d5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="841d5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="841d5-113">En arbetsplats av Facebook enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="841d5-113">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="841d5-114">Följ dessa rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="841d5-114">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="841d5-115">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="841d5-115">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="841d5-116">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="841d5-116">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="841d5-117">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="841d5-117">Scenario description</span></span>
<span data-ttu-id="841d5-118">I kursen får testa du Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="841d5-118">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="841d5-119">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="841d5-119">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="841d5-120">Lägg till arbetsplatsen av Facebook från galleriet.</span><span class="sxs-lookup"><span data-stu-id="841d5-120">Add Workplace by Facebook from the gallery.</span></span>
2. <span data-ttu-id="841d5-121">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="841d5-121">Configure and test Azure AD single sign-on.</span></span>

## <a name="add-workplace-by-facebook-from-the-gallery"></a><span data-ttu-id="841d5-122">Lägg till arbetsplatsen av Facebook från galleriet</span><span class="sxs-lookup"><span data-stu-id="841d5-122">Add Workplace by Facebook from the gallery</span></span>
<span data-ttu-id="841d5-123">För att konfigurera integrering av arbetsplatsen av Facebook i Azure AD, lägga till arbetsplatsen av Facebook från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="841d5-123">To configure the integration of Workplace by Facebook into Azure AD, add Workplace by Facebook from the gallery to your list of managed SaaS apps.</span></span>

1. <span data-ttu-id="841d5-124">I den [Azure-portalen](https://portal.azure.com), i den vänstra rutan, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="841d5-124">In the [Azure portal](https://portal.azure.com), in the left pane, select **Azure Active Directory**.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="841d5-126">Bläddra till **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="841d5-126">Browse to **Enterprise applications** > **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="841d5-128">Om du vill lägga till det nya programmet, Välj **nytt program** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="841d5-128">To add the new application, select **New application** on the top of the dialog box.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="841d5-130">I sökrutan skriver **arbetsplats av Facebook**, och välj **arbetsplats av Facebook** resultaten.</span><span class="sxs-lookup"><span data-stu-id="841d5-130">In the search box, type **Workplace by Facebook**, and select **Workplace by Facebook** from results.</span></span> <span data-ttu-id="841d5-131">Välj sedan **Lägg till**, för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="841d5-131">Then select **Add**, to add the application.</span></span>

    ![Arbetsplats av Facebook i resultatlistan](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="841d5-133">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="841d5-133">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="841d5-134">I det här avsnittet kan du konfigurera och testa Azure AD SSO till arbetsplats av Facebook, baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="841d5-134">In this section, you configure and test Azure AD SSO with Workplace by Facebook, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="841d5-135">För SSO ska fungera måste Azure AD du känna till motsvarande användaren i arbetsplats av Facebook till en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="841d5-135">For SSO to work, Azure AD needs to know what the counterpart user in Workplace by Facebook is to a user in Azure AD.</span></span> <span data-ttu-id="841d5-136">Med andra ord ska ett länkat förhållande mellan en Azure AD-användare och relaterade användaren arbetsplatsen av Facebook upprättas.</span><span class="sxs-lookup"><span data-stu-id="841d5-136">In other words, a linked relationship between an Azure AD user and the related user in Workplace by Facebook should be established.</span></span>

<span data-ttu-id="841d5-137">Etablera den här relationen genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** arbetsplatsen av Facebook.</span><span class="sxs-lookup"><span data-stu-id="841d5-137">Establish this relationship by assigning the value of the **user name** in Azure AD as the value of the **Username** in Workplace by Facebook.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="841d5-138">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="841d5-138">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="841d5-139">Du aktiverar Azure AD SSO i Azure portal och du konfigurerar enkel inloggning i din arbetsplats Facebook-programmet i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="841d5-139">In this section, you enable Azure AD SSO in the Azure portal, and you configure SSO in your Workplace by Facebook application.</span></span>

1. <span data-ttu-id="841d5-140">I Azure-portalen på den **arbetsplats av Facebook** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="841d5-140">In the Azure portal, on the **Workplace by Facebook** application integration page, select **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="841d5-142">I den **enkel inloggning** dialogrutan **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="841d5-142">In the **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on** to enable SSO.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="841d5-144">I den **arbetsplats Facebook-domänen och URL: er** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="841d5-144">In the **Workplace by Facebook Domain and URLs** section, do the following:</span></span>

    <span data-ttu-id="841d5-145">a.</span><span class="sxs-lookup"><span data-stu-id="841d5-145">a.</span></span> <span data-ttu-id="841d5-146">I den **inloggnings-URL** text skriver en URL som använder följande mönster:`https://<company subdomain>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="841d5-146">In the **Sign-on URL** text box, type a URL that uses the following pattern: `https://<company subdomain>.facebook.com`</span></span>

    <span data-ttu-id="841d5-147">b.</span><span class="sxs-lookup"><span data-stu-id="841d5-147">b.</span></span> <span data-ttu-id="841d5-148">I den **identifierare** text skriver en URL som använder följande mönster:`https://www.facebook.com/company/<scim company id>`</span><span class="sxs-lookup"><span data-stu-id="841d5-148">In the **Identifier** text box, type a URL that uses the following pattern: `https://www.facebook.com/company/<scim company id>`</span></span>

    > [!NOTE]
    > <span data-ttu-id="841d5-149">Dessa värden är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="841d5-149">These values are an example only.</span></span> <span data-ttu-id="841d5-150">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="841d5-150">Update these values with the actual sign-on URL and identifier.</span></span> <span data-ttu-id="841d5-151">Kontakta den [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="841d5-151">Contact the [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) to get these values.</span></span> 

4. <span data-ttu-id="841d5-152">I den **SAML-signeringscertifikat** väljer **certifikat (Base64)**, och sedan spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="841d5-152">In the **SAML Signing Certificate** section, select **Certificate (Base64)**, and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="841d5-154">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="841d5-154">Select **Save**.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="841d5-156">I den **arbetsplats Facebook konfigurationen** väljer **konfigurera arbetsplats av Facebook** att öppna den **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="841d5-156">In the **Workplace by Facebook Configuration** section, select **Configure Workplace by Facebook** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="841d5-157">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="841d5-157">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference** section.</span></span>

    ![Arbetsplats av Facebook-konfiguration](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. <span data-ttu-id="841d5-159">I en annan webbläsarfönster inloggningen till din arbetsplats av Facebook företagets plats som administratör.</span><span class="sxs-lookup"><span data-stu-id="841d5-159">In a different web browser window, sign in to your Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="841d5-160">Som en del av processen för SAML-autentisering, kan arbetsplats använda frågesträngar upp till 2,5 kilobyte i storlek för att kunna skicka parametrar till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="841d5-160">As part of the SAML authentication process, Workplace can use query strings of up to 2.5 kilobytes in size in order to pass parameters to Azure AD.</span></span>

8. <span data-ttu-id="841d5-161">I den **företagets instrumentpanelen**, gå till den **autentisering** fliken.</span><span class="sxs-lookup"><span data-stu-id="841d5-161">In the **Company Dashboard**, go to the **Authentication** tab.</span></span>

9. <span data-ttu-id="841d5-162">Under **SAML-autentisering**väljer **SSO endast** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="841d5-162">Under **SAML Authentication**, select **SSO Only** from the drop-down list.</span></span>

10. <span data-ttu-id="841d5-163">Ange de värden som kopieras från den **arbetsplats Facebook konfigurationen** på Azure portal till motsvarande fält:</span><span class="sxs-lookup"><span data-stu-id="841d5-163">Enter the values copied from the **Workplace by Facebook Configuration** section of the Azure portal into the corresponding fields:</span></span>

    *   <span data-ttu-id="841d5-164">I den **SAML URL** text klistra in värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="841d5-164">In the **SAML URL** text box, paste the value of **Single Sign-On Service URL**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="841d5-165">I den **utfärdar-URL för SAML** text klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="841d5-165">In the **SAML Issuer URL** text box, paste the value of **SAML Entity ID**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="841d5-166">I **SAML logga ut omdirigera (valfritt)**, klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="841d5-166">In **SAML Logout Redirect (optional)**, paste the value of **Sign-Out URL**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="841d5-167">Öppna din **Base64-kodat certifikat** i anteckningar, som hämtas från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="841d5-167">Open your **base-64 encoded certificate** in Notepad, downloaded from the Azure portal.</span></span> <span data-ttu-id="841d5-168">Kopiera innehållet i den till Urklipp och klistra in den till den **SAML certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="841d5-168">Copy the content of it into your clipboard, and then paste it to the **SAML Certificate** text box.</span></span>

11. <span data-ttu-id="841d5-169">Du kan behöva ange Webbadressen till målgruppen mottagaren URL: en och URL för ACS (Assertion konsumenten Service), som visas under den **SAML-konfiguration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="841d5-169">You might need to enter the audience URL, recipient URL, and ACS (Assertion Consumer Service) URL, listed under the **SAML Configuration** section.</span></span>

12. <span data-ttu-id="841d5-170">Bläddra längst ned i avsnittet och välj **Test SSO**.</span><span class="sxs-lookup"><span data-stu-id="841d5-170">Scroll to the bottom of the section, and select **Test SSO**.</span></span> <span data-ttu-id="841d5-171">Ett popup-fönster visas med Azure AD-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="841d5-171">A pop-up window appears, with the Azure AD sign-in page.</span></span> <span data-ttu-id="841d5-172">Ange dina autentiseringsuppgifter för att autentisera, som vanligt.</span><span class="sxs-lookup"><span data-stu-id="841d5-172">To authenticate, enter your credentials as normal.</span></span> <span data-ttu-id="841d5-173">Kontrollera den e-postadress som returnerades från Azure AD är samma som arbetsplatskontot som du är inloggad med.</span><span class="sxs-lookup"><span data-stu-id="841d5-173">Ensure the email address returned back from Azure AD is the same as the Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="841d5-174">Om testet har slutförts, bläddra till längst ned på sidan och välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="841d5-174">If the test has finished successfully, scroll to the bottom of the page and select **Save**.</span></span>

14. <span data-ttu-id="841d5-175">Alla som använder arbetsplats kan nu se med Azure AD-inloggningssida för autentisering.</span><span class="sxs-lookup"><span data-stu-id="841d5-175">Anyone using Workplace is now presented with Azure AD sign-in page for authentication.</span></span>

<span data-ttu-id="841d5-176">Du kan välja att konfigurera en SAML-logga ut URL, vilket kan användas för att peka på sidan Azure AD utloggning.</span><span class="sxs-lookup"><span data-stu-id="841d5-176">You can choose to configure a SAML sign out URL, which can be used to point at the Azure AD sign-out page.</span></span> <span data-ttu-id="841d5-177">När den här inställningen har aktiverats och konfigurerats, omdirigeras inte längre användaren till sidan Arbetsyta utloggning.</span><span class="sxs-lookup"><span data-stu-id="841d5-177">When this setting is enabled and configured, the user is no longer directed to the Workplace sign-out page.</span></span> <span data-ttu-id="841d5-178">I stället omdirigeras användaren till den URL som har lagts till i inställningen SAML utloggning omdirigera.</span><span class="sxs-lookup"><span data-stu-id="841d5-178">Instead, the user is redirected to the URL that was added in the SAML sign-out redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="841d5-179">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen.</span><span class="sxs-lookup"><span data-stu-id="841d5-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app.</span></span> <span data-ttu-id="841d5-180">När du lägger till den här appen från den **Active Directory** > **företagsprogram** bara väljer den **enkel inloggning** fliken och åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="841d5-180">After adding this app from the **Active Directory** > **Enterprise Applications** section, simply select the **Single Sign-On** tab, and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="841d5-181">Du kan läsa mer om funktionen inbäddade dokumentationen i den [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="841d5-181">You can read more about the embedded documentation feature in the [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="configure-reauthentication-frequency"></a><span data-ttu-id="841d5-182">Konfigurera omautentisering frekvens</span><span class="sxs-lookup"><span data-stu-id="841d5-182">Configure reauthentication frequency</span></span>

<span data-ttu-id="841d5-183">Du kan konfigurera arbetsplats för att fråga efter en SAML kontroll varje dag, tre dagar, en vecka, två veckor, en månad eller aldrig.</span><span class="sxs-lookup"><span data-stu-id="841d5-183">You can configure Workplace to prompt for a SAML check every day, three days, one week, two weeks, one month, or never.</span></span>

> [!NOTE] 
><span data-ttu-id="841d5-184">Minsta värde för SAML-kontroll av mobila program anges till en vecka.</span><span class="sxs-lookup"><span data-stu-id="841d5-184">The minimum value for the SAML check on mobile applications is set to one week.</span></span>

<span data-ttu-id="841d5-185">Du kan också tvinga en SAML återställa för alla användare.</span><span class="sxs-lookup"><span data-stu-id="841d5-185">You can also force a SAML reset for all users.</span></span> <span data-ttu-id="841d5-186">Det gör du genom att använda den **kräver SAML-autentisering för alla användare nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="841d5-186">To do this, use the **Require SAML authentication for all users now** button.</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="841d5-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="841d5-187">Create an Azure AD test user</span></span>
<span data-ttu-id="841d5-188">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="841d5-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

1. <span data-ttu-id="841d5-190">I den **Azure-portalen**, i den vänstra rutan, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="841d5-190">In the **Azure portal**, in the left pane, select **Azure Active Directory**.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="841d5-192">Om du vill visa en lista över användare, gå till **användare och grupper**, och välj **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="841d5-192">To display the list of users, go to **Users and groups**, and select **All users**.</span></span>
    
    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="841d5-194">Öppna den **användare** dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="841d5-194">To open the **User** dialog box, select **Add**.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="841d5-196">I den **användaren** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="841d5-196">In the **User** dialog box, do the following:</span></span>
 
    ![Dialogrutan användare](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="841d5-198">a.</span><span class="sxs-lookup"><span data-stu-id="841d5-198">a.</span></span> <span data-ttu-id="841d5-199">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="841d5-199">In the **Name** text box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="841d5-200">b.</span><span class="sxs-lookup"><span data-stu-id="841d5-200">b.</span></span> <span data-ttu-id="841d5-201">I den **användarnamn** skriver den **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="841d5-201">In the **User name** text box, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="841d5-202">c.</span><span class="sxs-lookup"><span data-stu-id="841d5-202">c.</span></span> <span data-ttu-id="841d5-203">Välj **visa lösenordet**, och Skriv ned.</span><span class="sxs-lookup"><span data-stu-id="841d5-203">Select **Show Password**, and write it down.</span></span>

    <span data-ttu-id="841d5-204">d.</span><span class="sxs-lookup"><span data-stu-id="841d5-204">d.</span></span> <span data-ttu-id="841d5-205">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="841d5-205">Select **Create**.</span></span>
 
### <a name="create-a-workplace-by-facebook-test-user"></a><span data-ttu-id="841d5-206">Skapa en arbetsplats av Facebook testanvändare</span><span class="sxs-lookup"><span data-stu-id="841d5-206">Create a Workplace by Facebook test user</span></span>

<span data-ttu-id="841d5-207">I det här avsnittet skapas en användare som kallas Britta Simon i arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="841d5-207">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="841d5-208">Arbetsplats av Facebook stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="841d5-208">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="841d5-209">Det finns inga åtgärder i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="841d5-209">There is no action for you in this section.</span></span> <span data-ttu-id="841d5-210">Om en användare inte finns i arbetsplats av Facebook, skapas en ny när du försöker komma åt arbetsplatsen av Facebook.</span><span class="sxs-lookup"><span data-stu-id="841d5-210">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt to access Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="841d5-211">Om du behöver skapa en användare manuellt kan kontakta den [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/).</span><span class="sxs-lookup"><span data-stu-id="841d5-211">If you need to create a user manually, contact the [Workplace by Facebook Client support team](https://workplace.fb.com/faq/).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="841d5-212">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="841d5-212">Assign the Azure AD test user</span></span>

<span data-ttu-id="841d5-213">I det här avsnittet kan du aktivera Britta Simon att använda Azure SSO genom att bevilja åtkomst till arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="841d5-213">In this section, you enable Britta Simon to use Azure SSO by granting access to Workplace by Facebook.</span></span>

![Tilldela användare][200] 

1. <span data-ttu-id="841d5-215">Öppna program i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="841d5-215">In the Azure portal, open the applications view.</span></span> <span data-ttu-id="841d5-216">Gå till vyn directory genom att gå till **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="841d5-216">Go to the directory view, go to **Enterprise applications**, and then select **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="841d5-218">Välj i listan med program **arbetsplats av Facebook**.</span><span class="sxs-lookup"><span data-stu-id="841d5-218">In the applications list, select **Workplace by Facebook**.</span></span>

    ![Arbetsplatsen av Facebook-länken i listan med program](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="841d5-220">Välj på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="841d5-220">In the menu on the left, select **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202] 

4. <span data-ttu-id="841d5-222">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="841d5-222">Select **Add**.</span></span> <span data-ttu-id="841d5-223">I den **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="841d5-223">Then, in the **Add Assignment** pane, select **Users and groups**.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="841d5-225">I den **användare och grupper** dialogrutan **Britta Simon** på användarlistan.</span><span class="sxs-lookup"><span data-stu-id="841d5-225">In the **Users and groups** dialog box, select **Britta Simon** in the users list.</span></span>

6. <span data-ttu-id="841d5-226">I den **användare och grupper** dialogrutan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="841d5-226">In the **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="841d5-227">I den **Lägg uppdrag** dialogrutan **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="841d5-227">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="841d5-228">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="841d5-228">Test single sign-on</span></span>

<span data-ttu-id="841d5-229">Om du vill testa inställningarna för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="841d5-229">If you want to test your SSO settings, open the Access Panel.</span></span>
<span data-ttu-id="841d5-230">Mer information finns i [Introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="841d5-230">For more information, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="841d5-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="841d5-231">Next steps</span></span>

* <span data-ttu-id="841d5-232">Finns det [lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="841d5-232">See the [list of tutorials on how to integrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md).</span></span>
* <span data-ttu-id="841d5-233">Läs [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="841d5-233">Read [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>
* <span data-ttu-id="841d5-234">Mer information om hur du [konfigurera användaretablering](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="841d5-234">Learn more about how to [configure user provisioning](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span></span>



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

