---
title: "Självstudier: Azure Active Directory-integrering med LockPath Keylight | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LockPath Keylight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: e64a966f24411818abc4cc4ab29a428b5577d012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a><span data-ttu-id="1a163-103">Självstudier: Azure Active Directory-integrering med LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="1a163-103">Tutorial: Azure Active Directory integration with LockPath Keylight</span></span>

<span data-ttu-id="1a163-104">I kursen får lära du att integrera LockPath Keylight med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1a163-104">In this tutorial, you learn how to integrate LockPath Keylight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1a163-105">Integrera LockPath Keylight med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1a163-105">Integrating LockPath Keylight with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1a163-106">Du kan styra i Azure AD som har åtkomst till LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="1a163-106">You can control in Azure AD who has access to LockPath Keylight</span></span>
- <span data-ttu-id="1a163-107">Du kan aktivera användarna att automatiskt hämta loggat in på LockPath Keylight (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1a163-107">You can enable your users to automatically get signed-on to LockPath Keylight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1a163-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1a163-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1a163-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1a163-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a163-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1a163-110">Prerequisites</span></span>

<span data-ttu-id="1a163-111">För att konfigurera Azure AD-integrering med LockPath Keylight, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="1a163-111">To configure Azure AD integration with LockPath Keylight, you need the following items:</span></span>

- <span data-ttu-id="1a163-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1a163-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1a163-113">En LockPath Keylight enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="1a163-113">A LockPath Keylight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1a163-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1a163-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1a163-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1a163-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1a163-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1a163-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1a163-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a163-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1a163-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1a163-118">Scenario description</span></span>
<span data-ttu-id="1a163-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1a163-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1a163-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1a163-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1a163-121">Att lägga till LockPath Keylight från galleriet</span><span class="sxs-lookup"><span data-stu-id="1a163-121">Adding LockPath Keylight from the gallery</span></span>
2. <span data-ttu-id="1a163-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1a163-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lockpath-keylight-from-the-gallery"></a><span data-ttu-id="1a163-123">Att lägga till LockPath Keylight från galleriet</span><span class="sxs-lookup"><span data-stu-id="1a163-123">Adding LockPath Keylight from the gallery</span></span>
<span data-ttu-id="1a163-124">Du måste lägga till LockPath Keylight från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LockPath Keylight i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a163-124">To configure the integration of LockPath Keylight into Azure AD, you need to add LockPath Keylight from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1a163-125">**Utför följande steg för att lägga till LockPath Keylight från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="1a163-125">**To add LockPath Keylight from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1a163-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1a163-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1a163-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1a163-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1a163-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1a163-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="1a163-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a163-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="1a163-133">I sökrutan skriver **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="1a163-133">In the search box, type **LockPath Keylight**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. <span data-ttu-id="1a163-135">Välj i resultatpanelen **LockPath Keylight**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="1a163-135">In the results panel, select **LockPath Keylight**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1a163-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1a163-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1a163-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LockPath Keylight baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1a163-138">In this section, you configure and test Azure AD single sign-on with LockPath Keylight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1a163-139">Azure AD måste du känna till användaren i LockPath Keylight motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="1a163-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LockPath Keylight is to a user in Azure AD.</span></span> <span data-ttu-id="1a163-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i LockPath Keylight upprättas.</span><span class="sxs-lookup"><span data-stu-id="1a163-140">In other words, a link relationship between an Azure AD user and the related user in LockPath Keylight needs to be established.</span></span>

<span data-ttu-id="1a163-141">I LockPath Keylight tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1a163-141">In LockPath Keylight, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1a163-142">Om du vill konfigurera och testa Azure AD enkel inloggning med LockPath Keylight, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1a163-142">To configure and test Azure AD single sign-on with LockPath Keylight, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1a163-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1a163-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1a163-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a163-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1a163-145">**[Skapa en testanvändare LockPath Keylight](#creating-a-lockpath-keylight-test-user)**  – har en motsvarighet för Britta Simon LockPath Keylight som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1a163-145">**[Creating a LockPath Keylight test user](#creating-a-lockpath-keylight-test-user)** - to have a counterpart of Britta Simon in LockPath Keylight that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1a163-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1a163-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1a163-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1a163-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1a163-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1a163-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1a163-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="1a163-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LockPath Keylight application.</span></span>

<span data-ttu-id="1a163-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med LockPath Keylight:**</span><span class="sxs-lookup"><span data-stu-id="1a163-150">**To configure Azure AD single sign-on with LockPath Keylight, perform the following steps:**</span></span>

1. <span data-ttu-id="1a163-151">I Azure-portalen på den **LockPath Keylight** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1a163-151">In the Azure portal, on the **LockPath Keylight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="1a163-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1a163-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. <span data-ttu-id="1a163-155">På den **LockPath Keylight domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1a163-155">On the **LockPath Keylight Domain and URLs** section, perform the following steps::</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    <span data-ttu-id="1a163-157">a.</span><span class="sxs-lookup"><span data-stu-id="1a163-157">a.</span></span> <span data-ttu-id="1a163-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.keylightgrc.com/`</span><span class="sxs-lookup"><span data-stu-id="1a163-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com/`</span></span>

    <span data-ttu-id="1a163-159">b.</span><span class="sxs-lookup"><span data-stu-id="1a163-159">b.</span></span> <span data-ttu-id="1a163-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company name>.keylightgrc.com`</span><span class="sxs-lookup"><span data-stu-id="1a163-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com`</span></span>

    <span data-ttu-id="1a163-161">c.</span><span class="sxs-lookup"><span data-stu-id="1a163-161">c.</span></span> <span data-ttu-id="1a163-162">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.keylightgrc.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="1a163-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com/Login.aspx`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="1a163-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="1a163-163">These values are not real.</span></span> <span data-ttu-id="1a163-164">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="1a163-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="1a163-165">Kontakta [LockPath Keylight klienten supportteamet](https://www.lockpath.com/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="1a163-165">Contact [LockPath Keylight Client support team](https://www.lockpath.com/contact/) to get these values.</span></span> 

4. <span data-ttu-id="1a163-166">På den **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="1a163-166">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. <span data-ttu-id="1a163-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1a163-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="1a163-170">På den **LockPath Keylight Configuration** klickar du på **konfigurera LockPath Keylight** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="1a163-170">On the **LockPath Keylight Configuration** section, click **Configure LockPath Keylight** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1a163-171">Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="1a163-171">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. <span data-ttu-id="1a163-173">Om du vill aktivera enkel inloggning i LockPath Keylight, utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="1a163-173">To enable SSO in LockPath Keylight, perform the following steps:</span></span>
   
    <span data-ttu-id="1a163-174">a.</span><span class="sxs-lookup"><span data-stu-id="1a163-174">a.</span></span> <span data-ttu-id="1a163-175">Inloggning till ditt konto LockPath Keylight som administratör.</span><span class="sxs-lookup"><span data-stu-id="1a163-175">Sign-on to your LockPath Keylight account as administrator.</span></span>
    
    <span data-ttu-id="1a163-176">b.</span><span class="sxs-lookup"><span data-stu-id="1a163-176">b.</span></span> <span data-ttu-id="1a163-177">Klicka på menyn högst upp **Person**, och välj **Keylight installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="1a163-177">In the menu on the top, click **Person**, and select **Keylight Setup**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/401.png) 

    <span data-ttu-id="1a163-179">c.</span><span class="sxs-lookup"><span data-stu-id="1a163-179">c.</span></span> <span data-ttu-id="1a163-180">I trädvyn till vänster klickar du på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="1a163-180">In the treeview on the left, click **SAML**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/402.png) 

    <span data-ttu-id="1a163-182">d.</span><span class="sxs-lookup"><span data-stu-id="1a163-182">d.</span></span> <span data-ttu-id="1a163-183">På den **SAML inställningar** dialogrutan klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="1a163-183">On the **SAML Settings** dialog, click **Edit**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/404.png) 

8. <span data-ttu-id="1a163-185">På den **redigera inställningar för SAML** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1a163-185">On the **Edit SAML Settings** dialog page, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    <span data-ttu-id="1a163-187">a.</span><span class="sxs-lookup"><span data-stu-id="1a163-187">a.</span></span> <span data-ttu-id="1a163-188">Ange **SAML-autentisering** till **Active**.</span><span class="sxs-lookup"><span data-stu-id="1a163-188">Set **SAML authentication** to **Active**.</span></span>

    <span data-ttu-id="1a163-189">b.</span><span class="sxs-lookup"><span data-stu-id="1a163-189">b.</span></span> <span data-ttu-id="1a163-190">Klistra in den **SAML enkel inloggning Tjänstwebbadress** värde som du har kopierat från Azure-portalen i den **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="1a163-190">Paste the **SAML Single Sign-On Service URL** value which you have copied from the Azure portal into the **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="1a163-191">c.</span><span class="sxs-lookup"><span data-stu-id="1a163-191">c.</span></span> <span data-ttu-id="1a163-192">Klistra in den **tjänst-URL för enkel Sign-Out** värde som du har kopierat från Azure-portalen i den **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="1a163-192">Paste the **Single Sign-Out Service URL** value which you have copied from the Azure portal into the **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="1a163-193">d.</span><span class="sxs-lookup"><span data-stu-id="1a163-193">d.</span></span> <span data-ttu-id="1a163-194">Klicka på **Välj fil** välja hämtade LockPath Keylight certifikatet och klicka sedan på **öppna** att ladda upp certifikatet.</span><span class="sxs-lookup"><span data-stu-id="1a163-194">Click **Choose File** to select your downloaded LockPath Keylight certificate, and then click **Open** to upload the certificate.</span></span>

    <span data-ttu-id="1a163-195">e.</span><span class="sxs-lookup"><span data-stu-id="1a163-195">e.</span></span> <span data-ttu-id="1a163-196">Ange **användar-Id för SAML plats** till **NameIdentifier element i instruktionen ämne**.</span><span class="sxs-lookup"><span data-stu-id="1a163-196">Set **SAML User Id location** to **NameIdentifier element of the subject statement**.</span></span>
    
    <span data-ttu-id="1a163-197">f.</span><span class="sxs-lookup"><span data-stu-id="1a163-197">f.</span></span> <span data-ttu-id="1a163-198">Ange den **Keylight Service Provider** med hjälp av följande mönster: **https://&lt;CompanyName&gt;. keylightgrc.com**.</span><span class="sxs-lookup"><span data-stu-id="1a163-198">Provide the **Keylight Service Provider** using the following pattern: **https://&lt;CompanyName&gt;.keylightgrc.com**.</span></span>
    
    <span data-ttu-id="1a163-199">g.</span><span class="sxs-lookup"><span data-stu-id="1a163-199">g.</span></span> <span data-ttu-id="1a163-200">Ange **automatiskt etablera användare** till **Active**.</span><span class="sxs-lookup"><span data-stu-id="1a163-200">Set **Auto-provision users** to **Active**.</span></span>

    <span data-ttu-id="1a163-201">h.</span><span class="sxs-lookup"><span data-stu-id="1a163-201">h.</span></span> <span data-ttu-id="1a163-202">Ange **automatiskt etablera kontotyp** till **fullständig användaren**.</span><span class="sxs-lookup"><span data-stu-id="1a163-202">Set **Auto-provision account type** to **Full User**.</span></span>

    <span data-ttu-id="1a163-203">Jag.</span><span class="sxs-lookup"><span data-stu-id="1a163-203">i.</span></span> <span data-ttu-id="1a163-204">Ange **automatiskt etablera säkerhetsrollen**väljer **standardanvändare med SAML**.</span><span class="sxs-lookup"><span data-stu-id="1a163-204">Set **Auto-provision security role**, select **Standard User with SAML**.</span></span>
    
    <span data-ttu-id="1a163-205">j.</span><span class="sxs-lookup"><span data-stu-id="1a163-205">j.</span></span> <span data-ttu-id="1a163-206">Ange **automatiskt etablera säkerhet config**väljer **Standard Användarkonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="1a163-206">Set **Auto-provision security config**, select **Standard User Configuration**.</span></span>
     
    <span data-ttu-id="1a163-207">k.</span><span class="sxs-lookup"><span data-stu-id="1a163-207">k.</span></span> <span data-ttu-id="1a163-208">I den **e-attributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="1a163-208">In the **Email attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="1a163-209">l.</span><span class="sxs-lookup"><span data-stu-id="1a163-209">l.</span></span> <span data-ttu-id="1a163-210">I den **förnamn attributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="1a163-210">In the **First name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="1a163-211">m.</span><span class="sxs-lookup"><span data-stu-id="1a163-211">m.</span></span> <span data-ttu-id="1a163-212">I den **senaste namnattributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="1a163-212">In the **Last name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
    
    <span data-ttu-id="1a163-213">n.</span><span class="sxs-lookup"><span data-stu-id="1a163-213">n.</span></span> <span data-ttu-id="1a163-214">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1a163-214">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="1a163-215">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="1a163-215">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1a163-216">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="1a163-216">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1a163-217">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1a163-217">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1a163-218">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a163-218">Creating an Azure AD test user</span></span>
<span data-ttu-id="1a163-219">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a163-219">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="1a163-221">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="1a163-221">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1a163-222">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1a163-222">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1a163-224">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1a163-224">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1a163-226">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a163-226">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1a163-228">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1a163-228">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1a163-230">a.</span><span class="sxs-lookup"><span data-stu-id="1a163-230">a.</span></span> <span data-ttu-id="1a163-231">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1a163-231">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1a163-232">b.</span><span class="sxs-lookup"><span data-stu-id="1a163-232">b.</span></span> <span data-ttu-id="1a163-233">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1a163-233">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1a163-234">c.</span><span class="sxs-lookup"><span data-stu-id="1a163-234">c.</span></span> <span data-ttu-id="1a163-235">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1a163-235">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1a163-236">d.</span><span class="sxs-lookup"><span data-stu-id="1a163-236">d.</span></span> <span data-ttu-id="1a163-237">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1a163-237">Click **Create**.</span></span>
 
### <a name="creating-a-lockpath-keylight-test-user"></a><span data-ttu-id="1a163-238">Skapa en testanvändare LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="1a163-238">Creating a LockPath Keylight test user</span></span>

<span data-ttu-id="1a163-239">I det här avsnittet skapar du en användare som kallas Britta Simon i LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="1a163-239">In this section, you create a user called Britta Simon in LockPath Keylight.</span></span> <span data-ttu-id="1a163-240">LockPath Keylight stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="1a163-240">LockPath Keylight supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="1a163-241">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1a163-241">There is no action item for you in this section.</span></span> <span data-ttu-id="1a163-242">En ny användare skapas vid åtkomst till LockPath Keylight om användaren inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="1a163-242">A new user is created when accessing LockPath Keylight if the user doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="1a163-243">Om du behöver skapa en användare manuellt, måste du kontakta den [LockPath Keylight klienten supportteamet](https://www.lockpath.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="1a163-243">If you need to create a user manually, you need to contact the [LockPath Keylight Client support team](https://www.lockpath.com/contact/).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1a163-244">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="1a163-244">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1a163-245">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="1a163-245">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LockPath Keylight.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1a163-247">**Om du vill tilldela LockPath Keylight Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1a163-247">**To assign Britta Simon to LockPath Keylight, perform the following steps:**</span></span>

1. <span data-ttu-id="1a163-248">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1a163-248">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1a163-250">Välj i listan med program **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="1a163-250">In the applications list, select **LockPath Keylight**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. <span data-ttu-id="1a163-252">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1a163-252">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="1a163-254">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1a163-254">Click **Add** button.</span></span> <span data-ttu-id="1a163-255">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a163-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="1a163-257">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="1a163-257">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1a163-258">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a163-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1a163-259">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a163-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1a163-260">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1a163-260">Testing single sign-on</span></span>

<span data-ttu-id="1a163-261">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="1a163-261">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1a163-262">När du klickar på panelen LockPath Keylight på åtkomstpanelen du bör få automatiskt loggat in på ditt LockPath Keylight program.</span><span class="sxs-lookup"><span data-stu-id="1a163-262">When you click the LockPath Keylight tile in the Access Panel, you should get automatically signed-on to your LockPath Keylight application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1a163-263">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1a163-263">Additional resources</span></span>

* [<span data-ttu-id="1a163-264">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1a163-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1a163-265">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1a163-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

