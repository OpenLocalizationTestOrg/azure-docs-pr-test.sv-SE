---
title: "Självstudier: Azure Active Directory-integrering med Workday | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Workday."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 164d5c644f120fa86e2b690649241892764b64b7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a><span data-ttu-id="c7735-103">Självstudier: Azure Active Directory-integrering med Workday</span><span class="sxs-lookup"><span data-stu-id="c7735-103">Tutorial: Azure Active Directory integration with Workday</span></span>

<span data-ttu-id="c7735-104">I kursen får lära du att integrera Workday med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c7735-104">In this tutorial, you learn how to integrate Workday with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c7735-105">Integrera Workday med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c7735-105">Integrating Workday with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c7735-106">Du kan styra i Azure AD som har åtkomst till Workday</span><span class="sxs-lookup"><span data-stu-id="c7735-106">You can control in Azure AD who has access to Workday</span></span>
- <span data-ttu-id="c7735-107">Du kan aktivera användarna att automatiskt hämta loggat in på Workday (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c7735-107">You can enable your users to automatically get signed-on to Workday (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c7735-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c7735-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c7735-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c7735-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7735-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c7735-110">Prerequisites</span></span>

<span data-ttu-id="c7735-111">För att konfigurera Azure AD-integrering med arbetsdagen, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c7735-111">To configure Azure AD integration with Workday, you need the following items:</span></span>

- <span data-ttu-id="c7735-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c7735-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c7735-113">En Workday enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c7735-113">A Workday single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c7735-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c7735-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c7735-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c7735-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c7735-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c7735-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c7735-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c7735-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c7735-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c7735-118">Scenario description</span></span>
<span data-ttu-id="c7735-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c7735-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c7735-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c7735-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c7735-121">Att lägga till Workday från galleriet</span><span class="sxs-lookup"><span data-stu-id="c7735-121">Adding Workday from the gallery</span></span>
2. <span data-ttu-id="c7735-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c7735-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workday-from-the-gallery"></a><span data-ttu-id="c7735-123">Att lägga till Workday från galleriet</span><span class="sxs-lookup"><span data-stu-id="c7735-123">Adding Workday from the gallery</span></span>
<span data-ttu-id="c7735-124">Du måste lägga till Workday från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av arbetsdagen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c7735-124">To configure the integration of Workday into Azure AD, you need to add Workday from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c7735-125">**Utför följande steg för att lägga till Workday från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c7735-125">**To add Workday from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c7735-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c7735-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c7735-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c7735-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c7735-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c7735-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c7735-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7735-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c7735-133">I sökrutan skriver **Workday**.</span><span class="sxs-lookup"><span data-stu-id="c7735-133">In the search box, type **Workday**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. <span data-ttu-id="c7735-135">Välj i resultatpanelen **Workday**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c7735-135">In the results panel, select **Workday**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c7735-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c7735-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c7735-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workday baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c7735-138">In this section, you configure and test Azure AD single sign-on with Workday based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c7735-139">Azure AD måste du känna till användaren i Workday motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c7735-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workday is to a user in Azure AD.</span></span> <span data-ttu-id="c7735-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Workday upprättas.</span><span class="sxs-lookup"><span data-stu-id="c7735-140">In other words, a link relationship between an Azure AD user and the related user in Workday needs to be established.</span></span>

<span data-ttu-id="c7735-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Workday.</span><span class="sxs-lookup"><span data-stu-id="c7735-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Workday.</span></span>

<span data-ttu-id="c7735-142">Om du vill konfigurera och testa Azure AD enkel inloggning med arbetsdagen, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c7735-142">To configure and test Azure AD single sign-on with Workday, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c7735-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c7735-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c7735-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c7735-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c7735-145">**[Skapa en testanvändare Workday](#creating-a-workday-test-user)**  – du har en motsvarighet för Britta Simon i Workday som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c7735-145">**[Creating a Workday test user](#creating-a-workday-test-user)** - to have a counterpart of Britta Simon in Workday that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c7735-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c7735-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c7735-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c7735-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c7735-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c7735-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c7735-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Workday-program.</span><span class="sxs-lookup"><span data-stu-id="c7735-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workday application.</span></span>

<span data-ttu-id="c7735-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Workday:**</span><span class="sxs-lookup"><span data-stu-id="c7735-150">**To configure Azure AD single sign-on with Workday, perform the following steps:**</span></span>

1. <span data-ttu-id="c7735-151">I Azure-portalen på den **Workday** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c7735-151">In the Azure portal, on the **Workday** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c7735-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c7735-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. <span data-ttu-id="c7735-155">På den **Workday domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c7735-155">On the **Workday Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    <span data-ttu-id="c7735-157">a.</span><span class="sxs-lookup"><span data-stu-id="c7735-157">a.</span></span> <span data-ttu-id="c7735-158">I den **inloggnings-URL** textruta Skriv värdet som:`https://impl.workday.com/<tenant>/login-saml2.htmld`</span><span class="sxs-lookup"><span data-stu-id="c7735-158">In the **Sign-on URL** textbox, type the value as: `https://impl.workday.com/<tenant>/login-saml2.htmld`</span></span>

    <span data-ttu-id="c7735-159">b.</span><span class="sxs-lookup"><span data-stu-id="c7735-159">b.</span></span> <span data-ttu-id="c7735-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://impl.workday.com/<tenant>/login-saml.htmld`</span><span class="sxs-lookup"><span data-stu-id="c7735-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://impl.workday.com/<tenant>/login-saml.htmld`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c7735-161">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="c7735-161">These values are not the real.</span></span> <span data-ttu-id="c7735-162">Uppdatera dessa värden med den faktiska inloggnings-URL och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="c7735-162">Update these values with the actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="c7735-163">Svars-URL: en måste ha en underdomän till exempel: www wd2, wd3, wd3 impl, wd5, wd5 impl).</span><span class="sxs-lookup"><span data-stu-id="c7735-163">Your reply URL must have a subdomain for example: www, wd2, wd3, wd3-impl, wd5, wd5-impl).</span></span> <span data-ttu-id="c7735-164">Med hjälp av något som liknar ”*http://www.myworkday.com*” fungerar men ”*http://myworkday.com*” finns inte.</span><span class="sxs-lookup"><span data-stu-id="c7735-164">Using something like "*http://www.myworkday.com*" works but "*http://myworkday.com*" does not.</span></span> <span data-ttu-id="c7735-165">Kontakta [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c7735-165">Contact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) to get these values.</span></span> 
 

4. <span data-ttu-id="c7735-166">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c7735-166">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. <span data-ttu-id="c7735-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c7735-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c7735-170">På den **Workday Configuration** klickar du på **konfigurera Workday** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c7735-170">On the **Workday Configuration** section, click **Configure Workday** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c7735-171">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c7735-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    <span data-ttu-id="c7735-172">![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="c7735-172">![Configure Single Sign-On](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span></span>
7. <span data-ttu-id="c7735-173">I en annan webbläsarfönster loggar du in på webbplatsen Workday företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="c7735-173">In a different web browser window, log in to your Workday company site as an administrator.</span></span>

8. <span data-ttu-id="c7735-174">Gå till **menyn \> arbetsstationen**.</span><span class="sxs-lookup"><span data-stu-id="c7735-174">Go to **Menu \> Workbench**.</span></span>
   
    <span data-ttu-id="c7735-175">![Arbetsstationen](./media/active-directory-saas-workday-tutorial/IC782923.png "arbetsstationen")</span><span class="sxs-lookup"><span data-stu-id="c7735-175">![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")</span></span>

9. <span data-ttu-id="c7735-176">Gå till **konto Administration**.</span><span class="sxs-lookup"><span data-stu-id="c7735-176">Go to **Account Administration**.</span></span>
   
    <span data-ttu-id="c7735-177">![Kontot Administration](./media/active-directory-saas-workday-tutorial/IC782924.png "konto Administration")</span><span class="sxs-lookup"><span data-stu-id="c7735-177">![Account Administration](./media/active-directory-saas-workday-tutorial/IC782924.png "Account Administration")</span></span>

10. <span data-ttu-id="c7735-178">Gå till **redigera inställningar för klient – säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="c7735-178">Go to **Edit Tenant Setup – Security**.</span></span>
   
    <span data-ttu-id="c7735-179">![Redigera klient säkerhet](./media/active-directory-saas-workday-tutorial/IC782925.png "redigera klient säkerhet")</span><span class="sxs-lookup"><span data-stu-id="c7735-179">![Edit Tenant Security](./media/active-directory-saas-workday-tutorial/IC782925.png "Edit Tenant Security")</span></span>

11. <span data-ttu-id="c7735-180">I den **omdirigering av URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c7735-180">In the **Redirection URLs** section, perform the following steps:</span></span>
   
    <span data-ttu-id="c7735-181">![Omdirigering av URL: er](./media/active-directory-saas-workday-tutorial/IC7829581.png "omdirigerings-URL: er")</span><span class="sxs-lookup"><span data-stu-id="c7735-181">![Redirection URLs](./media/active-directory-saas-workday-tutorial/IC7829581.png "Redirection URLs")</span></span>
   
    <span data-ttu-id="c7735-182">a.</span><span class="sxs-lookup"><span data-stu-id="c7735-182">a.</span></span> <span data-ttu-id="c7735-183">Klicka på **lägga till raden**.</span><span class="sxs-lookup"><span data-stu-id="c7735-183">Click **Add Row**.</span></span>
   
    <span data-ttu-id="c7735-184">b.</span><span class="sxs-lookup"><span data-stu-id="c7735-184">b.</span></span> <span data-ttu-id="c7735-185">I den **inloggningen omdirigerings-URL** textruta och **Mobile omdirigerings-URL** textruta typ av **inloggnings-URL** du har angett på den **Workday domän och URL: er** på Azure portal.</span><span class="sxs-lookup"><span data-stu-id="c7735-185">In the **Login Redirect URL** textbox and the **Mobile Redirect URL** textbox, type the **Sign-on URL** you have entered on the **Workday Domain and URLs** section of the Azure portal.</span></span>
   
    <span data-ttu-id="c7735-186">c.</span><span class="sxs-lookup"><span data-stu-id="c7735-186">c.</span></span> <span data-ttu-id="c7735-187">I Azure-portalen på den **konfigurera inloggning** fönster, kopiera den **Sign-Out URL**, och klistrar in det i den **logga ut omdirigerings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="c7735-187">In the Azure portal, on the **Configure sign-on** window, copy the **Sign-Out URL**, and then paste it into the **Logout Redirect URL** textbox.</span></span>
   
    <span data-ttu-id="c7735-188">d.</span><span class="sxs-lookup"><span data-stu-id="c7735-188">d.</span></span>  <span data-ttu-id="c7735-189">I **miljö** textruta typnamn miljö.</span><span class="sxs-lookup"><span data-stu-id="c7735-189">In **Environment** textbox, type the environment name.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="c7735-190">Värdet för attributet miljön är knutna till värdet för klient-URL:</span><span class="sxs-lookup"><span data-stu-id="c7735-190">The value of the Environment attribute is tied to the value of the tenant URL:</span></span>  
    ><span data-ttu-id="c7735-191">-Om domännamnet för Workday-klient-URL börjar med impl till exempel: *https://impl.workday.com/\<klient\>/login-saml2.htmld*), **miljö** attributet måste anges till implementering.</span><span class="sxs-lookup"><span data-stu-id="c7735-191">-If the domain name of the Workday tenant URL starts with impl for example: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), the **Environment** attribute must be set to Implementation.</span></span>  
    ><span data-ttu-id="c7735-192">-Om domännamnet startas med ett annat, måste du kontakta [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) att hämta det matchande **miljö** värde.</span><span class="sxs-lookup"><span data-stu-id="c7735-192">-If the domain name starts with something else, you need to contact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) to get the matching **Environment** value.</span></span>

12. <span data-ttu-id="c7735-193">I den **SAML installationsprogrammet** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c7735-193">In the **SAML Setup** section, perform the following steps:</span></span>
   
    <span data-ttu-id="c7735-194">![SAML-installationsprogrammet](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-installationen")</span><span class="sxs-lookup"><span data-stu-id="c7735-194">![SAML Setup](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")</span></span>
   
    <span data-ttu-id="c7735-195">a.</span><span class="sxs-lookup"><span data-stu-id="c7735-195">a.</span></span>  <span data-ttu-id="c7735-196">Välj **aktivera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="c7735-196">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="c7735-197">b.</span><span class="sxs-lookup"><span data-stu-id="c7735-197">b.</span></span>  <span data-ttu-id="c7735-198">Klicka på **lägga till raden**.</span><span class="sxs-lookup"><span data-stu-id="c7735-198">Click **Add Row**.</span></span>

13. <span data-ttu-id="c7735-199">I avsnittet SAML identitetsleverantörer utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="c7735-199">In the SAML Identity Providers section, perform the following steps:</span></span>
   
    <span data-ttu-id="c7735-200">![SAML identitetsleverantörer](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML identitetsleverantörer")</span><span class="sxs-lookup"><span data-stu-id="c7735-200">![SAML Identity Providers](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identity Providers")</span></span>
   
    <span data-ttu-id="c7735-201">a.</span><span class="sxs-lookup"><span data-stu-id="c7735-201">a.</span></span> <span data-ttu-id="c7735-202">Ange ett provider-namn i textrutan identitet providernamn (till exempel: *SPInitiatedSSO*).</span><span class="sxs-lookup"><span data-stu-id="c7735-202">In the Identity Provider Name textbox, type a provider name (for example: *SPInitiatedSSO*).</span></span>
   
    <span data-ttu-id="c7735-203">b.</span><span class="sxs-lookup"><span data-stu-id="c7735-203">b.</span></span> <span data-ttu-id="c7735-204">I Azure-portalen på den **konfigurera inloggning** fönster, kopiera den **SAML enhets-ID** värdet och klistrar in det i den **utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="c7735-204">In the Azure portal, on the **Configure sign-on** window, copy the **SAML Entity ID** value, and then paste it into the **Issuer** textbox.</span></span>
   
    <span data-ttu-id="c7735-205">c.</span><span class="sxs-lookup"><span data-stu-id="c7735-205">c.</span></span> <span data-ttu-id="c7735-206">Välj **aktivera arbetsdagar initierade logga ut**.</span><span class="sxs-lookup"><span data-stu-id="c7735-206">Select **Enable Workday Initiated Logout**.</span></span>
   
    <span data-ttu-id="c7735-207">d.</span><span class="sxs-lookup"><span data-stu-id="c7735-207">d.</span></span> <span data-ttu-id="c7735-208">I Azure-portalen på den **konfigurera inloggning** fönster, kopiera den **Sign-Out URL** värdet och klistrar in det i den **logga ut begäran URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="c7735-208">In the Azure portal, on the **Configure sign-on** window, copy the **Sign-Out URL** value, and then paste it into the **Logout Request URL** textbox.</span></span>

    <span data-ttu-id="c7735-209">e.</span><span class="sxs-lookup"><span data-stu-id="c7735-209">e.</span></span> <span data-ttu-id="c7735-210">Klicka på **providern offentliga nyckel identitetscertifikat**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c7735-210">Click **Identity Provider Public Key Certificate**, and then click **Create**.</span></span> 

    <span data-ttu-id="c7735-211">![Skapa](./media/active-directory-saas-workday-tutorial/IC782928.png "skapa")</span><span class="sxs-lookup"><span data-stu-id="c7735-211">![Create](./media/active-directory-saas-workday-tutorial/IC782928.png "Create")</span></span>

    <span data-ttu-id="c7735-212">f.</span><span class="sxs-lookup"><span data-stu-id="c7735-212">f.</span></span> <span data-ttu-id="c7735-213">Klicka på **skapa x509 offentliga nyckel**.</span><span class="sxs-lookup"><span data-stu-id="c7735-213">Click **Create x509 Public Key**.</span></span> 

    <span data-ttu-id="c7735-214">![Skapa](./media/active-directory-saas-workday-tutorial/IC782929.png "skapa")</span><span class="sxs-lookup"><span data-stu-id="c7735-214">![Create](./media/active-directory-saas-workday-tutorial/IC782929.png "Create")</span></span>


14. <span data-ttu-id="c7735-215">I den **visa x509 offentliga nyckel** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c7735-215">In the **View x509 Public Key** section, perform the following steps:</span></span> 
   
    <span data-ttu-id="c7735-216">![Visa x509 offentliga nyckel](./media/active-directory-saas-workday-tutorial/IC782930.png "visa x509 offentliga nyckel")</span><span class="sxs-lookup"><span data-stu-id="c7735-216">![View x509 Public Key](./media/active-directory-saas-workday-tutorial/IC782930.png "View x509 Public Key")</span></span> 
   
    <span data-ttu-id="c7735-217">a.</span><span class="sxs-lookup"><span data-stu-id="c7735-217">a.</span></span> <span data-ttu-id="c7735-218">I den **namn** textruta, ange ett namn för certifikatet (till exempel: *utrustningen\_SP*).</span><span class="sxs-lookup"><span data-stu-id="c7735-218">In the **Name** textbox, type a name for your certificate (for example: *PPE\_SP*).</span></span>
   
    <span data-ttu-id="c7735-219">b.</span><span class="sxs-lookup"><span data-stu-id="c7735-219">b.</span></span> <span data-ttu-id="c7735-220">I den **giltigt från** textruta skriver giltig från attributvärde för ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="c7735-220">In the **Valid From** textbox, type the valid from attribute value of your certificate.</span></span>
   
    <span data-ttu-id="c7735-221">c.</span><span class="sxs-lookup"><span data-stu-id="c7735-221">c.</span></span>  <span data-ttu-id="c7735-222">I den **till** textruta skriver det giltiga attributvärde för ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="c7735-222">In the **Valid To** textbox, type the valid to attribute value of your certificate.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="c7735-223">Du kan hämta det giltiga från datum och giltiga datum från hämtade certifikatet genom att dubbelklicka på den.</span><span class="sxs-lookup"><span data-stu-id="c7735-223">You can get the valid from date and the valid to date from the downloaded certificate by double-clicking it.</span></span>  <span data-ttu-id="c7735-224">Datumen visas under den **information** fliken.</span><span class="sxs-lookup"><span data-stu-id="c7735-224">The dates are listed under the **Details** tab.</span></span>
    > 
    >
   
    <span data-ttu-id="c7735-225">d.</span><span class="sxs-lookup"><span data-stu-id="c7735-225">d.</span></span>  <span data-ttu-id="c7735-226">Öppna din Base64-kodade certifikatet i anteckningar och kopiera innehållet i den.</span><span class="sxs-lookup"><span data-stu-id="c7735-226">Open your base-64 encoded certificate in notepad, and then copy the content of it.</span></span>
   
    <span data-ttu-id="c7735-227">e.</span><span class="sxs-lookup"><span data-stu-id="c7735-227">e.</span></span>  <span data-ttu-id="c7735-228">I den **certifikat** textruta klistra in innehållet i Urklipp.</span><span class="sxs-lookup"><span data-stu-id="c7735-228">In the **Certificate** textbox, paste the content of your clipboard.</span></span>
   
    <span data-ttu-id="c7735-229">f.</span><span class="sxs-lookup"><span data-stu-id="c7735-229">f.</span></span>  <span data-ttu-id="c7735-230">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7735-230">Click **OK**.</span></span>

15. <span data-ttu-id="c7735-231">Utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c7735-231">Perform the following steps:</span></span> 
   
    <span data-ttu-id="c7735-232">![Konfiguration av SSO](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="c7735-232">![SSO configuration](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO configuration")</span></span>
   
    <span data-ttu-id="c7735-233">a.</span><span class="sxs-lookup"><span data-stu-id="c7735-233">a.</span></span>  <span data-ttu-id="c7735-234">Aktivera den **x509 privata nyckel**.</span><span class="sxs-lookup"><span data-stu-id="c7735-234">Enable the **x509 Private Key Pair**.</span></span>
   
    <span data-ttu-id="c7735-235">b.</span><span class="sxs-lookup"><span data-stu-id="c7735-235">b.</span></span>  <span data-ttu-id="c7735-236">I den **Service Provider-ID** textruta typen **http://www.workday.com**.</span><span class="sxs-lookup"><span data-stu-id="c7735-236">In the **Service Provider ID** textbox, type **http://www.workday.com**.</span></span>
   
    <span data-ttu-id="c7735-237">c.</span><span class="sxs-lookup"><span data-stu-id="c7735-237">c.</span></span>  <span data-ttu-id="c7735-238">Välj **aktivera SP initierade SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="c7735-238">Select **Enable SP Initiated SAML Authentication**.</span></span>
   
    <span data-ttu-id="c7735-239">d.</span><span class="sxs-lookup"><span data-stu-id="c7735-239">d.</span></span>  <span data-ttu-id="c7735-240">I Azure-portalen på den **konfigurera inloggning** fönster, kopiera den **SAML inloggning tjänst-URL för enkel** värdet och klistrar in det i den **IdP SSO-tjänstens URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="c7735-240">In the Azure portal, on the **Configure sign-on** window, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **IdP SSO Service URL** textbox.</span></span>
   
    <span data-ttu-id="c7735-241">e.</span><span class="sxs-lookup"><span data-stu-id="c7735-241">e.</span></span> <span data-ttu-id="c7735-242">Välj **inte Deflate SP-initierad autentiseringsbegäran**.</span><span class="sxs-lookup"><span data-stu-id="c7735-242">Select **Do Not Deflate SP-initiated Authentication Request**.</span></span>
   
    <span data-ttu-id="c7735-243">f.</span><span class="sxs-lookup"><span data-stu-id="c7735-243">f.</span></span> <span data-ttu-id="c7735-244">Som **begära signatur autentiseringsmetod**väljer **SHA256**.</span><span class="sxs-lookup"><span data-stu-id="c7735-244">As **Authentication Request Signature Method**, select **SHA256**.</span></span> 
   
    <span data-ttu-id="c7735-245">![Autentiseringsmetod begäran signatur](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "autentiseringsmetod begäran signatur")</span><span class="sxs-lookup"><span data-stu-id="c7735-245">![Authentication Request Signature Method](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "Authentication Request Signature Method")</span></span> 
   
    <span data-ttu-id="c7735-246">g.</span><span class="sxs-lookup"><span data-stu-id="c7735-246">g.</span></span> <span data-ttu-id="c7735-247">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7735-247">Click **OK**.</span></span> 
   
    <span data-ttu-id="c7735-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span><span class="sxs-lookup"><span data-stu-id="c7735-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span></span>

> [!TIP]
> <span data-ttu-id="c7735-249">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c7735-249">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c7735-250">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c7735-250">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c7735-251">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c7735-251">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c7735-252">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c7735-252">Creating an Azure AD test user</span></span>
<span data-ttu-id="c7735-253">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c7735-253">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c7735-255">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c7735-255">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c7735-256">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c7735-256">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c7735-258">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c7735-258">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c7735-260">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7735-260">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c7735-262">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c7735-262">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c7735-264">a.</span><span class="sxs-lookup"><span data-stu-id="c7735-264">a.</span></span> <span data-ttu-id="c7735-265">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c7735-265">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c7735-266">b.</span><span class="sxs-lookup"><span data-stu-id="c7735-266">b.</span></span> <span data-ttu-id="c7735-267">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c7735-267">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c7735-268">c.</span><span class="sxs-lookup"><span data-stu-id="c7735-268">c.</span></span> <span data-ttu-id="c7735-269">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c7735-269">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c7735-270">d.</span><span class="sxs-lookup"><span data-stu-id="c7735-270">d.</span></span> <span data-ttu-id="c7735-271">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c7735-271">Click **Create**.</span></span>
 
### <a name="creating-a-workday-test-user"></a><span data-ttu-id="c7735-272">Skapa en arbetsdag testanvändare</span><span class="sxs-lookup"><span data-stu-id="c7735-272">Creating a Workday test user</span></span>

<span data-ttu-id="c7735-273">För att få en testanvändare etableras i Workday, måste du kontakta den [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html).</span><span class="sxs-lookup"><span data-stu-id="c7735-273">To get a test user provisioned into Workday, you need to contact the [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html).</span></span>
<span data-ttu-id="c7735-274">Den [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) skapar användaren åt dig.</span><span class="sxs-lookup"><span data-stu-id="c7735-274">The [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) creates the user for you.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c7735-275">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c7735-275">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c7735-276">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Workday.</span><span class="sxs-lookup"><span data-stu-id="c7735-276">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workday.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c7735-278">**Om du vill tilldela Workday Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c7735-278">**To assign Britta Simon to Workday, perform the following steps:**</span></span>

1. <span data-ttu-id="c7735-279">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c7735-279">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c7735-281">Välj i listan med program **Workday**.</span><span class="sxs-lookup"><span data-stu-id="c7735-281">In the applications list, select **Workday**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. <span data-ttu-id="c7735-283">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c7735-283">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c7735-285">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c7735-285">Click **Add** button.</span></span> <span data-ttu-id="c7735-286">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7735-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c7735-288">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c7735-288">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c7735-289">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7735-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c7735-290">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7735-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c7735-291">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c7735-291">Testing single sign-on</span></span>

<span data-ttu-id="c7735-292">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c7735-292">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="c7735-293">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c7735-293">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7735-294">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c7735-294">Additional resources</span></span>

* [<span data-ttu-id="c7735-295">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c7735-295">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c7735-296">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c7735-296">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="c7735-297">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="c7735-297">Configure User Provisioning</span></span>](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

