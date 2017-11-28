---
title: "Självstudier: Azure Active Directory-integrering med Aha! | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Aha!."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7723864b2e1ab2d5b69d86f0fa18416b9d3f9aa3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a><span data-ttu-id="5ea05-104">Självstudier: Azure Active Directory-integrering med Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-104">Tutorial: Azure Active Directory integration with Aha!</span></span>

<span data-ttu-id="5ea05-105">Lär dig hur du integrerar Aha i den här självstudiekursen!</span><span class="sxs-lookup"><span data-stu-id="5ea05-105">In this tutorial, you learn how to integrate Aha!</span></span> <span data-ttu-id="5ea05-106">med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5ea05-106">with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5ea05-107">Integrera Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-107">Integrating Aha!</span></span> <span data-ttu-id="5ea05-108">med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5ea05-108">with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5ea05-109">Du kan styra i Azure AD som har åtkomst till Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-109">You can control in Azure AD who has access to Aha!</span></span>
- <span data-ttu-id="5ea05-110">Du kan aktivera användarna att automatiskt hämta loggat in på Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-110">You can enable your users to automatically get signed-on to Aha!</span></span> <span data-ttu-id="5ea05-111">(Enkel inloggning) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5ea05-111">(Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5ea05-112">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5ea05-112">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5ea05-113">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5ea05-113">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ea05-114">Krav</span><span class="sxs-lookup"><span data-stu-id="5ea05-114">Prerequisites</span></span>

<span data-ttu-id="5ea05-115">Konfigurera Azure AD-integrering med Aha!, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5ea05-115">To configure Azure AD integration with Aha!, you need the following items:</span></span>

- <span data-ttu-id="5ea05-116">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5ea05-116">An Azure AD subscription</span></span>
- <span data-ttu-id="5ea05-117">En Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-117">An Aha!</span></span> <span data-ttu-id="5ea05-118">enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5ea05-118">single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5ea05-119">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5ea05-119">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5ea05-120">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5ea05-120">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5ea05-121">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5ea05-121">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5ea05-122">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5ea05-122">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5ea05-123">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5ea05-123">Scenario description</span></span>
<span data-ttu-id="5ea05-124">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5ea05-124">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5ea05-125">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5ea05-125">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5ea05-126">Lägga till Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-126">Adding Aha!</span></span> <span data-ttu-id="5ea05-127">från galleriet</span><span class="sxs-lookup"><span data-stu-id="5ea05-127">from the gallery</span></span>
2. <span data-ttu-id="5ea05-128">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ea05-128">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-aha-from-the-gallery"></a><span data-ttu-id="5ea05-129">Lägga till Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-129">Adding Aha!</span></span> <span data-ttu-id="5ea05-130">från galleriet</span><span class="sxs-lookup"><span data-stu-id="5ea05-130">from the gallery</span></span>
<span data-ttu-id="5ea05-131">Så här konfigurerar du integrering av Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-131">To configure the integration of Aha!</span></span> <span data-ttu-id="5ea05-132">Du måste lägga till Aha till Azure AD!</span><span class="sxs-lookup"><span data-stu-id="5ea05-132">into Azure AD, you need to add Aha!</span></span> <span data-ttu-id="5ea05-133">från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5ea05-133">from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5ea05-134">**Att lägga till Aha! Utför följande steg från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="5ea05-134">**To add Aha! from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5ea05-135">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5ea05-135">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5ea05-137">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-137">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5ea05-138">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-138">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5ea05-140">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ea05-140">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5ea05-142">I sökrutan skriver **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-142">In the search box, type **Aha!**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/tutorial_aha_search.png)

5. <span data-ttu-id="5ea05-144">Välj i resultatpanelen **Aha!**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="5ea05-144">In the results panel, select **Aha!**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5ea05-146">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ea05-146">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5ea05-147">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-147">In this section, you configure and test Azure AD single sign-on with Aha!</span></span> <span data-ttu-id="5ea05-148">baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5ea05-148">based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5ea05-149">För enkel inloggning ska fungera, Azure AD som behöver veta vilka motsvarande användaren i Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-149">For single sign-on to work, Azure AD needs to know what the counterpart user in Aha!</span></span> <span data-ttu-id="5ea05-150">är att en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5ea05-150">is to a user in Azure AD.</span></span> <span data-ttu-id="5ea05-151">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-151">In other words, a link relationship between an Azure AD user and the related user in Aha!</span></span> <span data-ttu-id="5ea05-152">måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="5ea05-152">needs to be established.</span></span>

<span data-ttu-id="5ea05-153">I Aha!, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5ea05-153">In Aha!, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5ea05-154">Konfigurera och testa Azure AD enkel inloggning med Aha!, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5ea05-154">To configure and test Azure AD single sign-on with Aha!, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5ea05-155">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5ea05-155">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5ea05-156">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5ea05-156">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5ea05-157">**[Skapa en Aha! testanvändare](#creating-an-aha-test-user)**  – har en motsvarighet för Britta Simon Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-157">**[Creating an Aha! test user](#creating-an-aha-test-user)** - to have a counterpart of Britta Simon in Aha!</span></span> <span data-ttu-id="5ea05-158">som är länkade till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5ea05-158">that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5ea05-159">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5ea05-159">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5ea05-160">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5ea05-160">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5ea05-161">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ea05-161">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5ea05-162">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-162">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Aha!</span></span> <span data-ttu-id="5ea05-163">Programmet.</span><span class="sxs-lookup"><span data-stu-id="5ea05-163">application.</span></span>

<span data-ttu-id="5ea05-164">**Konfigurera Azure AD enkel inloggning med Aha!, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5ea05-164">**To configure Azure AD single sign-on with Aha!, perform the following steps:**</span></span>

1. <span data-ttu-id="5ea05-165">I Azure-portalen på den **Aha!**</span><span class="sxs-lookup"><span data-stu-id="5ea05-165">In the Azure portal, on the **Aha!**</span></span> <span data-ttu-id="5ea05-166">programmet integrationssidan klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-166">application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5ea05-168">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5ea05-168">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_aha_samlbase.png)

3. <span data-ttu-id="5ea05-170">På den **Aha! Domänen och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ea05-170">On the **Aha! Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_aha_url.png)

    <span data-ttu-id="5ea05-172">a.</span><span class="sxs-lookup"><span data-stu-id="5ea05-172">a.</span></span> <span data-ttu-id="5ea05-173">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.aha.io/session/new`</span><span class="sxs-lookup"><span data-stu-id="5ea05-173">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.aha.io/session/new`</span></span>

    <span data-ttu-id="5ea05-174">b.</span><span class="sxs-lookup"><span data-stu-id="5ea05-174">b.</span></span> <span data-ttu-id="5ea05-175">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.aha.io`</span><span class="sxs-lookup"><span data-stu-id="5ea05-175">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.aha.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5ea05-176">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5ea05-176">These values are not real.</span></span> <span data-ttu-id="5ea05-177">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="5ea05-177">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5ea05-178">Kontakta [Aha! Klienten supportteamet](https://www.aha.io/company/contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5ea05-178">Contact [Aha! Client support team](https://www.aha.io/company/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="5ea05-179">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="5ea05-179">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_aha_certificate.png) 

5. <span data-ttu-id="5ea05-181">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5ea05-181">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5ea05-183">Logga in på ditt Aha i ett annat webbläsarfönster!</span><span class="sxs-lookup"><span data-stu-id="5ea05-183">In a different web browser window, log in to your Aha!</span></span> <span data-ttu-id="5ea05-184">företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="5ea05-184">company site as an administrator.</span></span>

7. <span data-ttu-id="5ea05-185">Klicka på menyn högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-185">In the menu on the top, click **Settings**.</span></span>

    <span data-ttu-id="5ea05-186">![Inställningar för](./media/active-directory-saas-aha-tutorial/IC798950.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="5ea05-186">![Settings](./media/active-directory-saas-aha-tutorial/IC798950.png "Settings")</span></span>

8. <span data-ttu-id="5ea05-187">Klicka på **konto**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-187">Click **Account**.</span></span>
   
    <span data-ttu-id="5ea05-188">![Profilen](./media/active-directory-saas-aha-tutorial/IC798951.png "profil")</span><span class="sxs-lookup"><span data-stu-id="5ea05-188">![Profile](./media/active-directory-saas-aha-tutorial/IC798951.png "Profile")</span></span>

9. <span data-ttu-id="5ea05-189">Klicka på **säkerhet och enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-189">Click **Security and single sign-on**.</span></span>
   
    <span data-ttu-id="5ea05-190">![Säkerhet och enkel inloggning](./media/active-directory-saas-aha-tutorial/IC798952.png "säkerhet och enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="5ea05-190">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798952.png "Security and single sign-on")</span></span>

10. <span data-ttu-id="5ea05-191">I **enkel inloggning** avsnittet som **identitetsleverantör**väljer **SAML2.0**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-191">In **Single Sign-On** section, as **Identity Provider**, select **SAML2.0**.</span></span>
   
    <span data-ttu-id="5ea05-192">![Säkerhet och enkel inloggning](./media/active-directory-saas-aha-tutorial/IC798953.png "säkerhet och enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="5ea05-192">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798953.png "Security and single sign-on")</span></span>

11. <span data-ttu-id="5ea05-193">På den **enkel inloggning** konfigurationen utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ea05-193">On the **Single Sign-On** configuration page, perform the following steps:</span></span>
    
    <span data-ttu-id="5ea05-194">![Enkel inloggning](./media/active-directory-saas-aha-tutorial/IC798954.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="5ea05-194">![Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798954.png "Single Sign-On")</span></span>
    
       <span data-ttu-id="5ea05-195">a.</span><span class="sxs-lookup"><span data-stu-id="5ea05-195">a.</span></span> <span data-ttu-id="5ea05-196">I den **namn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5ea05-196">In the **Name** textbox, type a name for your configuration.</span></span>

       <span data-ttu-id="5ea05-197">b.</span><span class="sxs-lookup"><span data-stu-id="5ea05-197">b.</span></span> <span data-ttu-id="5ea05-198">För **konfigurera med hjälp av**väljer **metadatafil**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-198">For **Configure using**, select **Metadata File**.</span></span>
   
       <span data-ttu-id="5ea05-199">c.</span><span class="sxs-lookup"><span data-stu-id="5ea05-199">c.</span></span> <span data-ttu-id="5ea05-200">Om du vill överföra din hämtade metadatafil klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-200">To upload your downloaded metadata file, click **Browse**.</span></span>
   
       <span data-ttu-id="5ea05-201">d.</span><span class="sxs-lookup"><span data-stu-id="5ea05-201">d.</span></span> <span data-ttu-id="5ea05-202">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-202">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="5ea05-203">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="5ea05-203">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5ea05-204">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="5ea05-204">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5ea05-205">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5ea05-205">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5ea05-206">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5ea05-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="5ea05-207">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5ea05-207">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5ea05-209">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="5ea05-209">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5ea05-210">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5ea05-210">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5ea05-212">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-212">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5ea05-214">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ea05-214">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5ea05-216">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ea05-216">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5ea05-218">a.</span><span class="sxs-lookup"><span data-stu-id="5ea05-218">a.</span></span> <span data-ttu-id="5ea05-219">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-219">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5ea05-220">b.</span><span class="sxs-lookup"><span data-stu-id="5ea05-220">b.</span></span> <span data-ttu-id="5ea05-221">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5ea05-221">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5ea05-222">c.</span><span class="sxs-lookup"><span data-stu-id="5ea05-222">c.</span></span> <span data-ttu-id="5ea05-223">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-223">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5ea05-224">d.</span><span class="sxs-lookup"><span data-stu-id="5ea05-224">d.</span></span> <span data-ttu-id="5ea05-225">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-225">Click **Create**.</span></span>
 
### <a name="creating-an-aha-test-user"></a><span data-ttu-id="5ea05-226">Skapa en Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-226">Creating an Aha!</span></span> <span data-ttu-id="5ea05-227">testanvändare</span><span class="sxs-lookup"><span data-stu-id="5ea05-227">test user</span></span>

<span data-ttu-id="5ea05-228">Aktivera Azure AD-användare kan logga in på Aha!, de måste etableras i Aha!.</span><span class="sxs-lookup"><span data-stu-id="5ea05-228">To enable Azure AD users to log in to Aha!, they must be provisioned into Aha!.</span></span>  

<span data-ttu-id="5ea05-229">När det gäller Aha!, etablering är en automatisk uppgift.</span><span class="sxs-lookup"><span data-stu-id="5ea05-229">In the case of Aha!, provisioning is an automated task.</span></span> <span data-ttu-id="5ea05-230">Det finns ingen åtgärd-objekt.</span><span class="sxs-lookup"><span data-stu-id="5ea05-230">There is no action item for you.</span></span>

<span data-ttu-id="5ea05-231">Användare skapas automatiskt om det behövs under det första enkla inloggning försöket.</span><span class="sxs-lookup"><span data-stu-id="5ea05-231">Users are automatically created if necessary during the first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="5ea05-232">Du kan använda andra Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-232">You can use any other Aha!</span></span> <span data-ttu-id="5ea05-233">Verktyg för att skapa användaren konto eller API: er som tillhandahålls av Aha!</span><span class="sxs-lookup"><span data-stu-id="5ea05-233">user account creation tools or APIs provided by Aha!</span></span> <span data-ttu-id="5ea05-234">att tillhandahålla AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="5ea05-234">to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5ea05-235">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="5ea05-235">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5ea05-236">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Aha!.</span><span class="sxs-lookup"><span data-stu-id="5ea05-236">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Aha!.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5ea05-238">**Att tilldela Aha Britta Simon!, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5ea05-238">**To assign Britta Simon to Aha!, perform the following steps:**</span></span>

1. <span data-ttu-id="5ea05-239">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-239">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5ea05-241">Välj i listan med program **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-241">In the applications list, select **Aha!**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_aha_app.png) 

3. <span data-ttu-id="5ea05-243">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5ea05-243">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5ea05-245">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5ea05-245">Click **Add** button.</span></span> <span data-ttu-id="5ea05-246">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ea05-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5ea05-248">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="5ea05-248">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5ea05-249">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ea05-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5ea05-250">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ea05-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5ea05-251">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ea05-251">Testing single sign-on</span></span>

<span data-ttu-id="5ea05-252">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5ea05-252">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="5ea05-253">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5ea05-253">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ea05-254">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5ea05-254">Additional resources</span></span>

* [<span data-ttu-id="5ea05-255">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5ea05-255">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5ea05-256">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5ea05-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aha-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aha-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aha-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aha-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aha-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aha-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aha-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aha-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aha-tutorial/tutorial_general_203.png

