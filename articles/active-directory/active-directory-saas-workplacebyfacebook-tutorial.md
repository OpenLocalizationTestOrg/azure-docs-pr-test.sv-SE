---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1590a66f215f0c093d24ff602c0ad951ba1e1eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="8fa0d-103">Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook</span><span class="sxs-lookup"><span data-stu-id="8fa0d-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="8fa0d-104">I kursen får lära du att integrera arbetsplats av Facebook med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8fa0d-104">In this tutorial, you learn how to integrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8fa0d-105">Integrera arbetsplats av Facebook med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8fa0d-105">Integrating Workplace by Facebook with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8fa0d-106">Du kan styra i Azure AD som har åtkomst till arbetsplats med Facebook</span><span class="sxs-lookup"><span data-stu-id="8fa0d-106">You can control in Azure AD who has access to Workplace by Facebook</span></span>
- <span data-ttu-id="8fa0d-107">Du kan aktivera användarna att automatiskt hämta loggat in på arbetsplatsen av Facebook (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8fa0d-107">You can enable your users to automatically get signed-on to Workplace by Facebook (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8fa0d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8fa0d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8fa0d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8fa0d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8fa0d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8fa0d-110">Prerequisites</span></span>

<span data-ttu-id="8fa0d-111">För att konfigurera Azure AD-integrering med arbetsplats av Facebook, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8fa0d-111">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="8fa0d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8fa0d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8fa0d-113">En arbetsplats av Facebook enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8fa0d-113">A Workplace by Facebook single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8fa0d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8fa0d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8fa0d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8fa0d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8fa0d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8fa0d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8fa0d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8fa0d-118">Scenario description</span></span>
<span data-ttu-id="8fa0d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8fa0d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8fa0d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8fa0d-121">Att lägga till arbetsplatsen av Facebook från galleriet</span><span class="sxs-lookup"><span data-stu-id="8fa0d-121">Adding Workplace by Facebook from the gallery</span></span>
2. <span data-ttu-id="8fa0d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8fa0d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workplace-by-facebook-from-the-gallery"></a><span data-ttu-id="8fa0d-123">Att lägga till arbetsplatsen av Facebook från galleriet</span><span class="sxs-lookup"><span data-stu-id="8fa0d-123">Adding Workplace by Facebook from the gallery</span></span>
<span data-ttu-id="8fa0d-124">Du måste lägga till arbetsplatsen av Facebook från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av arbetsplatsen av Facebook i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-124">To configure the integration of Workplace by Facebook into Azure AD, you need to add Workplace by Facebook from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8fa0d-125">**Utför följande steg för att lägga till arbetsplatsen av Facebook från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8fa0d-125">**To add Workplace by Facebook from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8fa0d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8fa0d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8fa0d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8fa0d-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8fa0d-133">I sökrutan skriver **arbetsplats av Facebook**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-133">In the search box, type **Workplace by Facebook**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. <span data-ttu-id="8fa0d-135">Välj i resultatpanelen **arbetsplats av Facebook**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-135">In the results panel, select **Workplace by Facebook**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8fa0d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8fa0d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8fa0d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning till arbetsplats av Facebook baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-138">In this section, you configure and test Azure AD single sign-on with Workplace by Facebook based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8fa0d-139">Azure AD måste du känna till motsvarande användaren i arbetsplats av Facebook till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workplace by Facebook is to a user in Azure AD.</span></span> <span data-ttu-id="8fa0d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i arbetsplats av Facebook upprättas.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-140">In other words, a link relationship between an Azure AD user and the related user in Workplace by Facebook needs to be established.</span></span>

<span data-ttu-id="8fa0d-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** arbetsplatsen av Facebook.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Workplace by Facebook.</span></span>

<span data-ttu-id="8fa0d-142">Om du vill konfigurera och testa Azure AD enkel inloggning till arbetsplats av Facebook, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8fa0d-142">To configure and test Azure AD single sign-on with Workplace by Facebook, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8fa0d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8fa0d-144">**[Konfigurera omautentisering frekvens](#configuring-reauthentication-frequency)**  - om du vill konfigurera arbetsplats för att fråga efter en SAML-kontroll.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-144">**[Configuring Reauthentication Frequency](#configuring-reauthentication-frequency)** - to configure Workplace to prompt for a SAML check.</span></span>
3. <span data-ttu-id="8fa0d-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="8fa0d-146">**[Skapa en arbetsplats av Facebook testanvändare](#creating-a-workplace-by-facebook-test-user)**  – du har en motsvarighet för Britta Simon arbetsplatsen av Facebook som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-146">**[Creating a Workplace by Facebook test user](#creating-a-workplace-by-facebook-test-user)** - to have a counterpart of Britta Simon in Workplace by Facebook that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="8fa0d-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="8fa0d-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8fa0d-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8fa0d-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8fa0d-150">I det här avsnittet kan du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning på din arbetsplats genom Facebook-programmet.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workplace by Facebook application.</span></span>

<span data-ttu-id="8fa0d-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning till arbetsplats av Facebook:**</span><span class="sxs-lookup"><span data-stu-id="8fa0d-151">**To configure Azure AD single sign-on with Workplace by Facebook, perform the following steps:**</span></span>

1. <span data-ttu-id="8fa0d-152">I Azure-portalen på den **arbetsplats av Facebook** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-152">In the Azure portal, on the **Workplace by Facebook** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8fa0d-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="8fa0d-156">På den **arbetsplats Facebook-domänen och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8fa0d-156">On the **Workplace by Facebook Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    <span data-ttu-id="8fa0d-158">a.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-158">a.</span></span> <span data-ttu-id="8fa0d-159">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<instancename>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="8fa0d-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.facebook.com`</span></span>

    <span data-ttu-id="8fa0d-160">b.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-160">b.</span></span> <span data-ttu-id="8fa0d-161">I den **identifierare** textruta Skriv en URL med följande mönster:`https://www.facebook.com/company/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="8fa0d-161">In the **Identifier** textbox, type a URL using the following pattern: `https://www.facebook.com/company/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8fa0d-162">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-162">These values are not the real.</span></span> <span data-ttu-id="8fa0d-163">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8fa0d-164">Kontakta [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-164">Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) to get these values.</span></span> 

4. <span data-ttu-id="8fa0d-165">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="8fa0d-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8fa0d-169">På den **arbetsplats Facebook konfigurationen** klickar du på **konfigurera arbetsplats av Facebook** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-169">On the **Workplace by Facebook Configuration** section, click **Configure Workplace by Facebook** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8fa0d-170">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="8fa0d-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. <span data-ttu-id="8fa0d-172">I en annan webbläsarfönstret, inloggning till arbetsplatsen av Facebook företagets plats som administratör.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-172">In a different web browser window, login to your Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="8fa0d-173">Som en del av processen för SAML-autentisering, kan arbetsplatsen använda frågesträngar upp till 2,5 kilobyte i storlek för att kunna skicka parametrar till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-173">As part of the SAML authentication process, Workplace may utilize query strings of up to 2.5 kilobytes in size in order to pass parameters to Azure AD.</span></span>

8. <span data-ttu-id="8fa0d-174">I den **företagets instrumentpanelen**, gå till den **autentisering** fliken.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-174">In the **Company Dashboard**, go to the **Authentication** tab.</span></span>

9. <span data-ttu-id="8fa0d-175">Under **SAML-autentisering**väljer **SSO endast** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-175">Under **SAML Authentication**, select **SSO Only** from the drop-down list.</span></span>

10. <span data-ttu-id="8fa0d-176">Ange värden som kopieras från **arbetsplats Facebook konfigurationen** på Azure portal till motsvarande fält:</span><span class="sxs-lookup"><span data-stu-id="8fa0d-176">Input the values copied from **Workplace by Facebook Configuration** section of the Azure portal into the corresponding fields:</span></span>

    *   <span data-ttu-id="8fa0d-177">I **SAML URL** textruta klistra in värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-177">In **SAML URL** textbox, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="8fa0d-178">I **utfärdar-URL för SAML-textrutan**, klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-178">In **SAML Issuer URL textbox**, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="8fa0d-179">I **SAML logga ut omdirigera** (valfritt), klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-179">In **SAML Logout Redirect** (Optional), paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="8fa0d-180">Öppna din **Base64-kodat certifikat** i anteckningar som hämtas från Azure-portalen, kopiera innehållet i den till Urklipp och klistra in den till den **SAML certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-180">Open your **base-64 encoded certificate** in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **SAML Certificate** textbox.</span></span>

11. <span data-ttu-id="8fa0d-181">Du kan behöva målgruppen webbadressen till mottagaren URL och ACS (Assertion konsumenten Service)-URL som visas under den **SAML-konfiguration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-181">You may need to enter the Audience URL, Recipient URL, and ACS (Assertion Consumer Service) URL listed under the **SAML Configuration** section.</span></span>

12. <span data-ttu-id="8fa0d-182">Bläddra längst ned i avsnittet och klicka på den **Test SSO** knappen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-182">Scroll to the bottom of the section and click the **Test SSO** button.</span></span> <span data-ttu-id="8fa0d-183">Detta resulterar i ett popup-fönster som visas med Azure AD-inloggningssidan visas.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-183">This results in a popup window appearing with Azure AD login page presented.</span></span> <span data-ttu-id="8fa0d-184">Ange dina autentiseringsuppgifter i som vanligt för att autentisera.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-184">Enter your credentials in as normal to authenticate.</span></span> 

    <span data-ttu-id="8fa0d-185">**Felsöka:** kontrollera e-postadressen som returneras tillbaka från Azure AD är samma som arbetsplatskontot som du är inloggad med.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-185">**Troubleshooting:** Ensure the email address being returned back from Azure AD is the same as the Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="8fa0d-186">När testet har slutförts, bläddra till längst ned på sidan och klicka på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-186">Once the test has been completed successfully, scroll to the bottom of the page and click the **Save** button.</span></span>

14. <span data-ttu-id="8fa0d-187">Alla användare som använder arbetsplats visas nu med Azure AD-inloggningssidan för autentisering.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-187">All users using Workplace will now be presented with Azure AD login page for authentication.</span></span>

15. <span data-ttu-id="8fa0d-188">**SAML logga ut omdirigera (valfritt)** -</span><span class="sxs-lookup"><span data-stu-id="8fa0d-188">**SAML Logout Redirect (optional)** -</span></span> 

    <span data-ttu-id="8fa0d-189">Kan du också konfigurera en SAML logga ut Url som kan användas för att peka på sidan för Azure AD logga ut.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-189">You can choose to optionally configure a SAML Logout Url, which can be used to point at Azure AD's logout page.</span></span> <span data-ttu-id="8fa0d-190">När den här inställningen är aktiverad och konfigurerad, kommer användaren inte längre dirigeras till sidan Arbetsyta logga ut.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-190">When this setting is enabled and configured, the user will no longer be directed to the Workplace logout page.</span></span> <span data-ttu-id="8fa0d-191">I stället omdirigeras användaren till den url som har lagts till i inställningen för att dirigera om SAML logga ut.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-191">Instead, the user will be redirected to the url that was added in the SAML Logout Redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="8fa0d-192">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="8fa0d-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8fa0d-193">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8fa0d-194">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8fa0d-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="configuring-reauthentication-frequency"></a><span data-ttu-id="8fa0d-195">Konfigurera omautentisering frekvens</span><span class="sxs-lookup"><span data-stu-id="8fa0d-195">Configuring Reauthentication Frequency</span></span>

<span data-ttu-id="8fa0d-196">Du kan konfigurera arbetsplats för att fråga efter en SAML-kontroll varje dag, tre dagar, vecka, vecka, månad eller aldrig.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-196">You can configure Workplace to prompt for a SAML check every day, three days, week, two weeks, month or never.</span></span>

> [!NOTE] 
><span data-ttu-id="8fa0d-197">Minsta värde för SAML-kontroll av mobila program anges till en vecka.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-197">The minimum value for the SAML check on mobile applications is set to one week.</span></span>

<span data-ttu-id="8fa0d-198">Du kan också tvinga en återställning för alla användare med hjälp av knappen SAML: kräver SAML-autentisering för alla användare nu.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-198">You can also force a SAML reset for all users using the button: Require SAML authentication for all users now.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8fa0d-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8fa0d-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="8fa0d-200">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8fa0d-202">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8fa0d-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8fa0d-203">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8fa0d-205">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8fa0d-207">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8fa0d-209">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8fa0d-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8fa0d-211">a.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-211">a.</span></span> <span data-ttu-id="8fa0d-212">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8fa0d-213">b.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-213">b.</span></span> <span data-ttu-id="8fa0d-214">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8fa0d-215">c.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-215">c.</span></span> <span data-ttu-id="8fa0d-216">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8fa0d-217">d.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-217">d.</span></span> <span data-ttu-id="8fa0d-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-218">Click **Create**.</span></span>
 
### <a name="creating-a-workplace-by-facebook-test-user"></a><span data-ttu-id="8fa0d-219">Skapa en arbetsplats av Facebook testanvändare</span><span class="sxs-lookup"><span data-stu-id="8fa0d-219">Creating a Workplace by Facebook test user</span></span>

<span data-ttu-id="8fa0d-220">I det här avsnittet skapas en användare som kallas Britta Simon i arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-220">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="8fa0d-221">Arbetsplats av Facebook stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-221">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="8fa0d-222">Det finns inga åtgärder i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-222">There is no action for you in this section.</span></span> <span data-ttu-id="8fa0d-223">Om en användare inte finns i arbetsplats av Facebook, skapas en ny när du försöker komma åt arbetsplatsen av Facebook.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-223">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt to access Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="8fa0d-224">Om du behöver skapa en användare manuellt, kontakta [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/)</span><span class="sxs-lookup"><span data-stu-id="8fa0d-224">If you need to create a user manually, Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/)</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8fa0d-225">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8fa0d-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8fa0d-226">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workplace by Facebook.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8fa0d-228">**Om du vill tilldela arbetsplats av Facebook Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8fa0d-228">**To assign Britta Simon to Workplace by Facebook, perform the following steps:**</span></span>

1. <span data-ttu-id="8fa0d-229">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8fa0d-231">Välj i listan med program **arbetsplats av Facebook**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-231">In the applications list, select **Workplace by Facebook**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="8fa0d-233">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8fa0d-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-235">Click **Add** button.</span></span> <span data-ttu-id="8fa0d-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8fa0d-238">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8fa0d-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8fa0d-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8fa0d-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8fa0d-241">Testing single sign-on</span></span>

<span data-ttu-id="8fa0d-242">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8fa0d-242">If you want to test your single sign-on settings, open the Access Panel.</span></span>
<span data-ttu-id="8fa0d-243">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8fa0d-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="8fa0d-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8fa0d-244">Additional resources</span></span>

* [<span data-ttu-id="8fa0d-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8fa0d-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8fa0d-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8fa0d-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="8fa0d-247">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="8fa0d-247">Configure User Provisioning</span></span>](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

