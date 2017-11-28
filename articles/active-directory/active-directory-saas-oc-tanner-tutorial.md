---
title: "Självstudier: Azure Active Directory-integrering med O.C. Turesson - AppreciateHub | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och O.C. Turesson - AppreciateHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 9af12372b30d9ee1575e46be3b4144fc3b73ec69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a><span data-ttu-id="d8bea-105">Självstudier: Azure Active Directory-integrering med O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-105">Tutorial: Azure Active Directory integration with O.C.</span></span> <span data-ttu-id="d8bea-106">Turesson - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="d8bea-106">Tanner - AppreciateHub</span></span>

<span data-ttu-id="d8bea-107">I kursen får du lära dig hur du integrerar O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-107">In this tutorial, you learn how to integrate O.C.</span></span> <span data-ttu-id="d8bea-108">Turesson - AppreciateHub med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d8bea-108">Tanner - AppreciateHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d8bea-109">Integrera O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-109">Integrating O.C.</span></span> <span data-ttu-id="d8bea-110">Turesson - AppreciateHub med Azure AD ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d8bea-110">Tanner - AppreciateHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d8bea-111">Du kan styra i Azure AD som har åtkomst till O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-111">You can control in Azure AD who has access to O.C.</span></span> <span data-ttu-id="d8bea-112">Turesson - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="d8bea-112">Tanner - AppreciateHub</span></span>
- <span data-ttu-id="d8bea-113">Du kan aktivera användarna att automatiskt hämta loggat in på O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-113">You can enable your users to automatically get signed-on to O.C.</span></span> <span data-ttu-id="d8bea-114">Turesson - AppreciateHub (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d8bea-114">Tanner - AppreciateHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d8bea-115">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d8bea-115">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d8bea-116">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d8bea-116">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8bea-117">Krav</span><span class="sxs-lookup"><span data-stu-id="d8bea-117">Prerequisites</span></span>

<span data-ttu-id="d8bea-118">Konfigurera Azure AD-integrering med O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-118">To configure Azure AD integration with O.C.</span></span> <span data-ttu-id="d8bea-119">Turesson - AppreciateHub, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d8bea-119">Tanner - AppreciateHub, you need the following items:</span></span>

- <span data-ttu-id="d8bea-120">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8bea-120">An Azure AD subscription</span></span>
- <span data-ttu-id="d8bea-121">EN O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-121">A O.C.</span></span> <span data-ttu-id="d8bea-122">Turesson - AppreciateHub enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8bea-122">Tanner - AppreciateHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d8bea-123">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8bea-123">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d8bea-124">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d8bea-124">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d8bea-125">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d8bea-125">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d8bea-126">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8bea-126">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d8bea-127">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d8bea-127">Scenario description</span></span>
<span data-ttu-id="d8bea-128">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8bea-128">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d8bea-129">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d8bea-129">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d8bea-130">Lägga till O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-130">Adding O.C.</span></span> <span data-ttu-id="d8bea-131">Turesson - AppreciateHub från galleriet</span><span class="sxs-lookup"><span data-stu-id="d8bea-131">Tanner - AppreciateHub from the gallery</span></span>
2. <span data-ttu-id="d8bea-132">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8bea-132">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a><span data-ttu-id="d8bea-133">Lägga till O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-133">Adding O.C.</span></span> <span data-ttu-id="d8bea-134">Turesson - AppreciateHub från galleriet</span><span class="sxs-lookup"><span data-stu-id="d8bea-134">Tanner - AppreciateHub from the gallery</span></span>
<span data-ttu-id="d8bea-135">Så här konfigurerar du integrering av O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-135">To configure the integration of O.C.</span></span> <span data-ttu-id="d8bea-136">Turesson - AppreciateHub till Azure AD som du behöver lägga till O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-136">Tanner - AppreciateHub into Azure AD, you need to add O.C.</span></span> <span data-ttu-id="d8bea-137">Turesson - AppreciateHub från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d8bea-137">Tanner - AppreciateHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d8bea-138">**Att lägga till O.C. Turesson - AppreciateHub från galleriet, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8bea-138">**To add O.C. Tanner - AppreciateHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d8bea-139">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d8bea-139">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d8bea-141">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-141">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d8bea-142">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-142">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d8bea-144">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8bea-144">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d8bea-146">I sökrutan skriver **O.C. Turesson - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-146">In the search box, type **O.C. Tanner - AppreciateHub**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

5. <span data-ttu-id="d8bea-148">Välj i resultatpanelen **O.C. Turesson - AppreciateHub**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d8bea-148">In the results panel, select **O.C. Tanner - AppreciateHub**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d8bea-150">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8bea-150">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d8bea-151">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-151">In this section, you configure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="d8bea-152">Turesson - AppreciateHub baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d8bea-152">Tanner - AppreciateHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d8bea-153">För enkel inloggning ska fungera, måste Azure AD veta vilka motsvarande användaren i O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-153">For single sign-on to work, Azure AD needs to know what the counterpart user in O.C.</span></span> <span data-ttu-id="d8bea-154">Turesson - AppreciateHub är att en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8bea-154">Tanner - AppreciateHub is to a user in Azure AD.</span></span> <span data-ttu-id="d8bea-155">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-155">In other words, a link relationship between an Azure AD user and the related user in O.C.</span></span> <span data-ttu-id="d8bea-156">Turesson - AppreciateHub måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="d8bea-156">Tanner - AppreciateHub needs to be established.</span></span>

<span data-ttu-id="d8bea-157">I O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-157">In O.C.</span></span> <span data-ttu-id="d8bea-158">Turesson - AppreciateHub, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d8bea-158">Tanner - AppreciateHub, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d8bea-159">Konfigurera och testa Azure AD enkel inloggning med O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-159">To configure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="d8bea-160">Turesson - AppreciateHub, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d8bea-160">Tanner - AppreciateHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d8bea-161">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d8bea-161">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d8bea-162">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8bea-162">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d8bea-163">**[Skapa en O.C. Turesson - AppreciateHub testanvändare](#creating-a-oc-tanner---appreciatehub-test-user)**  – har en motsvarighet för Britta Simon O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-163">**[Creating a O.C. Tanner - AppreciateHub test user](#creating-a-oc-tanner---appreciatehub-test-user)** - to have a counterpart of Britta Simon in O.C.</span></span> <span data-ttu-id="d8bea-164">Turesson - AppreciateHub som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d8bea-164">Tanner - AppreciateHub that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d8bea-165">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d8bea-165">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d8bea-166">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d8bea-166">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d8bea-167">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8bea-167">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d8bea-168">I det här avsnittet kan du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-168">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your O.C.</span></span> <span data-ttu-id="d8bea-169">Turesson - AppreciateHub program.</span><span class="sxs-lookup"><span data-stu-id="d8bea-169">Tanner - AppreciateHub application.</span></span>

<span data-ttu-id="d8bea-170">**Konfigurera Azure AD enkel inloggning med O.C. Turesson - AppreciateHub, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8bea-170">**To configure Azure AD single sign-on with O.C. Tanner - AppreciateHub, perform the following steps:**</span></span>

1. <span data-ttu-id="d8bea-171">I Azure-portalen på den **O.C. Turesson - AppreciateHub** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-171">In the Azure portal, on the **O.C. Tanner - AppreciateHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d8bea-173">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d8bea-173">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

3. <span data-ttu-id="d8bea-175">På den **O.C. Turesson - URL: er och AppreciateHub domän** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8bea-175">On the **O.C. Tanner - AppreciateHub Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    <span data-ttu-id="d8bea-177">a.</span><span class="sxs-lookup"><span data-stu-id="d8bea-177">a.</span></span> <span data-ttu-id="d8bea-178">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span><span class="sxs-lookup"><span data-stu-id="d8bea-178">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d8bea-179">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d8bea-179">This value is not real.</span></span> <span data-ttu-id="d8bea-180">Uppdatera det här värdet med det faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d8bea-180">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="d8bea-181">Kontakta [O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="d8bea-181">Contact [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) to get this value.</span></span>

    <span data-ttu-id="d8bea-182">b.</span><span class="sxs-lookup"><span data-stu-id="d8bea-182">b.</span></span> <span data-ttu-id="d8bea-183">Öppna metadatafilen med hjälp av följande länk: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span><span class="sxs-lookup"><span data-stu-id="d8bea-183">Open the metadata file using the following link: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span></span>
   
    <span data-ttu-id="d8bea-184">c.</span><span class="sxs-lookup"><span data-stu-id="d8bea-184">c.</span></span> <span data-ttu-id="d8bea-185">Leta upp den **md:AssertionConsumerService** nod.</span><span class="sxs-lookup"><span data-stu-id="d8bea-185">Locate the **md:AssertionConsumerService** node.</span></span> 
   
    <span data-ttu-id="d8bea-186">d.</span><span class="sxs-lookup"><span data-stu-id="d8bea-186">d.</span></span> <span data-ttu-id="d8bea-187">Kopiera värdet för den **plats** attribut.</span><span class="sxs-lookup"><span data-stu-id="d8bea-187">Copy the value of the **Location** attribute.</span></span> 
   
    ![Konfigurera Appinställningar för][12]
   
    <span data-ttu-id="d8bea-189">e.</span><span class="sxs-lookup"><span data-stu-id="d8bea-189">e.</span></span> <span data-ttu-id="d8bea-190">I den **logga URL** textruta efter värdet som du har fått i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="d8bea-190">In the **Sign On URL** textbox, past the value you have obtained in the previous step.</span></span>

4. <span data-ttu-id="d8bea-191">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d8bea-191">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

5. <span data-ttu-id="d8bea-193">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d8bea-193">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d8bea-195">Konfigurera enkel inloggning på **O.C. Turesson - AppreciateHub** sida, måste du skicka den hämtade **XML-Metadata för** till [O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com).</span><span class="sxs-lookup"><span data-stu-id="d8bea-195">To configure single sign-on on **O.C. Tanner - AppreciateHub** side, you need to send the downloaded **Metadata XML** to [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com).</span></span>

> [!TIP]
> <span data-ttu-id="d8bea-196">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d8bea-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d8bea-197">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d8bea-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d8bea-198">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d8bea-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d8bea-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8bea-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="d8bea-200">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8bea-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d8bea-202">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d8bea-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d8bea-203">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d8bea-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d8bea-205">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d8bea-207">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8bea-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d8bea-209">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8bea-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d8bea-211">a.</span><span class="sxs-lookup"><span data-stu-id="d8bea-211">a.</span></span> <span data-ttu-id="d8bea-212">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d8bea-213">b.</span><span class="sxs-lookup"><span data-stu-id="d8bea-213">b.</span></span> <span data-ttu-id="d8bea-214">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d8bea-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d8bea-215">c.</span><span class="sxs-lookup"><span data-stu-id="d8bea-215">c.</span></span> <span data-ttu-id="d8bea-216">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d8bea-217">d.</span><span class="sxs-lookup"><span data-stu-id="d8bea-217">d.</span></span> <span data-ttu-id="d8bea-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-218">Click **Create**.</span></span>
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a><span data-ttu-id="d8bea-219">Skapa en O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-219">Creating a O.C.</span></span> <span data-ttu-id="d8bea-220">Turesson - AppreciateHub testanvändare</span><span class="sxs-lookup"><span data-stu-id="d8bea-220">Tanner - AppreciateHub test user</span></span>

<span data-ttu-id="d8bea-221">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-221">The objective of this section is to create a user called Britta Simon in O.C.</span></span> <span data-ttu-id="d8bea-222">Turesson - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="d8bea-222">Tanner - AppreciateHub.</span></span>

<span data-ttu-id="d8bea-223">**Om du vill skapa en användare med namnet Britta Simon i O.C. Turesson - AppreciateHub, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8bea-223">**To create a user called Britta Simon in O.C. Tanner - AppreciateHub, perform the following steps:**</span></span>

<span data-ttu-id="d8bea-224">Be din [O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com) att skapa en användare som som nameID attribut har samma värde som användarnamn för Britta Simon i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8bea-224">Ask your [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) to create a user that has as nameID attribute the same value as the user name of Britta Simon in Azure AD.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d8bea-225">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d8bea-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d8bea-226">I det här avsnittet kan aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to O.C.</span></span> <span data-ttu-id="d8bea-227">Turesson - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="d8bea-227">Tanner - AppreciateHub.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d8bea-229">**Att tilldela O.C. Britta Simon Turesson - AppreciateHub, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8bea-229">**To assign Britta Simon to O.C. Tanner - AppreciateHub, perform the following steps:**</span></span>

1. <span data-ttu-id="d8bea-230">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d8bea-232">Välj i listan med program **O.C. Turesson - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-232">In the applications list, select **O.C. Tanner - AppreciateHub**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

3. <span data-ttu-id="d8bea-234">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d8bea-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d8bea-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d8bea-236">Click **Add** button.</span></span> <span data-ttu-id="d8bea-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8bea-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d8bea-239">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d8bea-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d8bea-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8bea-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d8bea-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8bea-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d8bea-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8bea-242">Testing single sign-on</span></span>

<span data-ttu-id="d8bea-243">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d8bea-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="d8bea-244">När du klickar på O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-244">When you click the O.C.</span></span> <span data-ttu-id="d8bea-245">Turesson - AppreciateHub panelen på panelen åtkomst du ska hämta automatiskt loggat in på ditt O.C.</span><span class="sxs-lookup"><span data-stu-id="d8bea-245">Tanner - AppreciateHub tile in the Access Panel, you should get automatically signed-on to your O.C.</span></span> <span data-ttu-id="d8bea-246">Turesson - AppreciateHub program.</span><span class="sxs-lookup"><span data-stu-id="d8bea-246">Tanner - AppreciateHub application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d8bea-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d8bea-247">Additional resources</span></span>

* [<span data-ttu-id="d8bea-248">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d8bea-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d8bea-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d8bea-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png

