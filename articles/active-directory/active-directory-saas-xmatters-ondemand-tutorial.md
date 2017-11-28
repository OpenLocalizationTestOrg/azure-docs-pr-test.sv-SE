---
title: "Självstudier: Azure Active Directory-integrering med xMatters OnDemand | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och xMatters OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 9bfcb44ed19f167872b3cd9119e2dbdd35c82604
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a><span data-ttu-id="95333-103">Självstudier: Azure Active Directory-integrering med xMatters OnDemand</span><span class="sxs-lookup"><span data-stu-id="95333-103">Tutorial: Azure Active Directory integration with xMatters OnDemand</span></span>

<span data-ttu-id="95333-104">I kursen får lära du att integrera xMatters OnDemand med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="95333-104">In this tutorial, you learn how to integrate xMatters OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="95333-105">Integrera xMatters OnDemand med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="95333-105">Integrating xMatters OnDemand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="95333-106">Du kan styra i Azure AD som har åtkomst till xMatters OnDemand</span><span class="sxs-lookup"><span data-stu-id="95333-106">You can control in Azure AD who has access to xMatters OnDemand</span></span>
- <span data-ttu-id="95333-107">Du kan aktivera användarna att automatiskt hämta loggat in på xMatters OnDemand (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="95333-107">You can enable your users to automatically get signed-on to xMatters OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="95333-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="95333-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="95333-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="95333-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95333-110">Krav</span><span class="sxs-lookup"><span data-stu-id="95333-110">Prerequisites</span></span>

<span data-ttu-id="95333-111">För att konfigurera Azure AD-integrering med xMatters OnDemand, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="95333-111">To configure Azure AD integration with xMatters OnDemand, you need the following items:</span></span>

- <span data-ttu-id="95333-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="95333-112">An Azure AD subscription</span></span>
- <span data-ttu-id="95333-113">En xMatters OnDemand enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="95333-113">A xMatters OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="95333-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="95333-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="95333-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="95333-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="95333-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="95333-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="95333-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="95333-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="95333-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="95333-118">Scenario description</span></span>
<span data-ttu-id="95333-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="95333-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="95333-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="95333-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="95333-121">Att lägga till xMatters OnDemand från galleriet</span><span class="sxs-lookup"><span data-stu-id="95333-121">Adding xMatters OnDemand from the gallery</span></span>
2. <span data-ttu-id="95333-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95333-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-xmatters-ondemand-from-the-gallery"></a><span data-ttu-id="95333-123">Att lägga till xMatters OnDemand från galleriet</span><span class="sxs-lookup"><span data-stu-id="95333-123">Adding xMatters OnDemand from the gallery</span></span>
<span data-ttu-id="95333-124">Du måste lägga till xMatters OnDemand från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av xMatters OnDemand i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95333-124">To configure the integration of xMatters OnDemand into Azure AD, you need to add xMatters OnDemand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="95333-125">**Utför följande steg för att lägga till xMatters OnDemand från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="95333-125">**To add xMatters OnDemand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="95333-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="95333-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="95333-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="95333-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="95333-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="95333-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="95333-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95333-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="95333-133">I sökrutan skriver **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="95333-133">In the search box, type **xMatters OnDemand**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. <span data-ttu-id="95333-135">Välj i resultatpanelen **xMatters OnDemand**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="95333-135">In the results panel, select **xMatters OnDemand**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="95333-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95333-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="95333-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med xMatters OnDemand baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="95333-138">In this section, you configure and test Azure AD single sign-on with xMatters OnDemand based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="95333-139">För enkel inloggning ska fungera, Azure AD som behöver veta vilka motsvarande användaren i xMatters OnDemand är att en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95333-139">For single sign-on to work, Azure AD needs to know what the counterpart user in xMatters OnDemand is to a user in Azure AD.</span></span> <span data-ttu-id="95333-140">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i xMatters OnDemand måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="95333-140">In other words, a link relationship between an Azure AD user and the related user in xMatters OnDemand needs to be established.</span></span>

<span data-ttu-id="95333-141">I xMatters OnDemand, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="95333-141">In xMatters OnDemand, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="95333-142">Om du vill konfigurera och testa Azure AD enkel inloggning med xMatters OnDemand, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="95333-142">To configure and test Azure AD single sign-on with xMatters OnDemand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="95333-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="95333-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="95333-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95333-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="95333-145">**[Skapa en testanvändare för xMatters OnDemand](#creating-a-xmatters-ondemand-test-user)**  – du har en motsvarighet för Britta Simon i xMatters OnDemand som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="95333-145">**[Creating a xMatters OnDemand test user](#creating-a-xmatters-ondemand-test-user)** - to have a counterpart of Britta Simon in xMatters OnDemand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="95333-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="95333-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="95333-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="95333-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="95333-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95333-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="95333-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din xMatters OnDemand-programmet.</span><span class="sxs-lookup"><span data-stu-id="95333-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your xMatters OnDemand application.</span></span>

<span data-ttu-id="95333-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med xMatters OnDemand:**</span><span class="sxs-lookup"><span data-stu-id="95333-150">**To configure Azure AD single sign-on with xMatters OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="95333-151">I Azure-portalen på den **xMatters OnDemand** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="95333-151">In the Azure portal, on the **xMatters OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="95333-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="95333-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. <span data-ttu-id="95333-155">På den **xMatters OnDemand domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="95333-155">On the **xMatters OnDemand Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    <span data-ttu-id="95333-157">a.</span><span class="sxs-lookup"><span data-stu-id="95333-157">a.</span></span> <span data-ttu-id="95333-158">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="95333-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    <span data-ttu-id="95333-159">b.</span><span class="sxs-lookup"><span data-stu-id="95333-159">b.</span></span> <span data-ttu-id="95333-160">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="95333-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > <span data-ttu-id="95333-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="95333-161">These values are not real.</span></span> <span data-ttu-id="95333-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="95333-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="95333-163">Kontakta [xMatters OnDemand supportteam](https://www.xmatters.com/company/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="95333-163">Contact [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/) to get these values.</span></span>

4. <span data-ttu-id="95333-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen lokalt som **c:\\XMatters OnDemand.cer**.</span><span class="sxs-lookup"><span data-stu-id="95333-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file locally as **c:\\XMatters OnDemand.cer**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="95333-166">Du behöver att vidarebefordra certifikatet som den [xMatters OnDemand supportteam](https://www.xmatters.com/company/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="95333-166">You need to forward the certificate to the [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/).</span></span> <span data-ttu-id="95333-167">Certifikatet måste överföras av supportteamet xMatters innan du kan slutföra konfigurationen för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="95333-167">The certificate needs to be uploaded by the xMatters support team before you can finalize the single sign-on configuration.</span></span> 

5. <span data-ttu-id="95333-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="95333-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="95333-170">På den **xMatters OnDemand Configuration** klickar du på **konfigurera xMatters OnDemand** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="95333-170">On the **xMatters OnDemand Configuration** section, click **Configure xMatters OnDemand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="95333-171">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="95333-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. <span data-ttu-id="95333-173">I en annan webbläsarfönster loggar du in på webbplatsen XMatters OnDemand företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="95333-173">In a different web browser window, log in to your XMatters OnDemand company site as an administrator.</span></span>

8. <span data-ttu-id="95333-174">Klicka på i verktygsfältet högst upp **Admin**, och klicka sedan på **företagsinformation** i navigeringsfältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="95333-174">In the toolbar on the top, click **Admin**, and then click **Company Details** in the navigation bar on the left side.</span></span>
   
    <span data-ttu-id="95333-175">![Admin](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="95333-175">![Admin](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")</span></span>

9. <span data-ttu-id="95333-176">På den **SAML-konfiguration** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="95333-176">On the **SAML Configuration** page, perform the following steps:</span></span>
   
    <span data-ttu-id="95333-177">![SAML-konfiguration](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="95333-177">![SAML configuration](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")</span></span>
   
    <span data-ttu-id="95333-178">a.</span><span class="sxs-lookup"><span data-stu-id="95333-178">a.</span></span> <span data-ttu-id="95333-179">Välj **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="95333-179">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="95333-180">b.</span><span class="sxs-lookup"><span data-stu-id="95333-180">b.</span></span> <span data-ttu-id="95333-181">Klistra in **SAML enhets-ID**, som du har kopierat från Azure-portalen i den **identitet Provider-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="95333-181">Paste **SAML Entity ID**, which you have copied from the Azure portal into the **Identity Provider ID** textbox.</span></span>
   
    <span data-ttu-id="95333-182">c.</span><span class="sxs-lookup"><span data-stu-id="95333-182">c.</span></span> <span data-ttu-id="95333-183">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från Azure-portalen i den **enkel inloggning på URL: en** textruta.</span><span class="sxs-lookup"><span data-stu-id="95333-183">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **Single Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="95333-184">d.</span><span class="sxs-lookup"><span data-stu-id="95333-184">d.</span></span> <span data-ttu-id="95333-185">Klistra in **Sign-Out URL**, som du har kopierat från Azure-portalen i den **URL för enkel logga ut** textruta.</span><span class="sxs-lookup"><span data-stu-id="95333-185">Paste **Sign-Out URL**, which you have copied from the Azure portal into the **Single Logout URL** textbox.</span></span>
   
    <span data-ttu-id="95333-186">e.</span><span class="sxs-lookup"><span data-stu-id="95333-186">e.</span></span> <span data-ttu-id="95333-187">På sidan företagsinformation överst **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="95333-187">On the Company Details page, at the top, click **Save Changes**.</span></span>
    
    <span data-ttu-id="95333-188">![Företagets information](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "företagets information")</span><span class="sxs-lookup"><span data-stu-id="95333-188">![Company details](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")</span></span>

> [!TIP]
> <span data-ttu-id="95333-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="95333-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="95333-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="95333-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="95333-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="95333-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="95333-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="95333-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="95333-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95333-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="95333-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="95333-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="95333-196">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="95333-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="95333-198">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="95333-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="95333-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95333-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="95333-202">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="95333-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="95333-204">a.</span><span class="sxs-lookup"><span data-stu-id="95333-204">a.</span></span> <span data-ttu-id="95333-205">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="95333-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="95333-206">b.</span><span class="sxs-lookup"><span data-stu-id="95333-206">b.</span></span> <span data-ttu-id="95333-207">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="95333-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="95333-208">c.</span><span class="sxs-lookup"><span data-stu-id="95333-208">c.</span></span> <span data-ttu-id="95333-209">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="95333-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="95333-210">d.</span><span class="sxs-lookup"><span data-stu-id="95333-210">d.</span></span> <span data-ttu-id="95333-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="95333-211">Click **Create**.</span></span>
 
### <a name="creating-a-xmatters-ondemand-test-user"></a><span data-ttu-id="95333-212">Skapa en xMatters OnDemand testanvändare</span><span class="sxs-lookup"><span data-stu-id="95333-212">Creating a xMatters OnDemand test user</span></span>

<span data-ttu-id="95333-213">För att aktivera Azure AD-användare kan logga in på XMatters OnDemand etableras de i XMatters OnDemand.</span><span class="sxs-lookup"><span data-stu-id="95333-213">In order to enable Azure AD users to log in to XMatters OnDemand, they must be provisioned into XMatters OnDemand.</span></span> <span data-ttu-id="95333-214">När det gäller XMatters OnDemand är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="95333-214">In the case of XMatters OnDemand, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="95333-215">Utför följande steg för att etablera en användarkonton:</span><span class="sxs-lookup"><span data-stu-id="95333-215">To provision a user accounts, perform the following steps:</span></span>
1. <span data-ttu-id="95333-216">Logga in på ditt **XMatters OnDemand** klient.</span><span class="sxs-lookup"><span data-stu-id="95333-216">Log in to your **XMatters OnDemand** tenant.</span></span>

2.  <span data-ttu-id="95333-217">Klicka på **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="95333-217">Click **Users** tab.</span></span> <span data-ttu-id="95333-218">och klicka sedan på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="95333-218">and then click **Add User**.</span></span>
  
    <span data-ttu-id="95333-219">![Användare](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "användare")</span><span class="sxs-lookup"><span data-stu-id="95333-219">![Users](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")</span></span>

3. <span data-ttu-id="95333-220">I den **lägga till en användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="95333-220">In the **Add a User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="95333-221">![Lägga till en användare](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "lägga till en användare")</span><span class="sxs-lookup"><span data-stu-id="95333-221">![Add a User](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")</span></span>

    <span data-ttu-id="95333-222">a.</span><span class="sxs-lookup"><span data-stu-id="95333-222">a.</span></span> <span data-ttu-id="95333-223">Välj **Active**.</span><span class="sxs-lookup"><span data-stu-id="95333-223">Select **Active**.</span></span>

    <span data-ttu-id="95333-224">b.</span><span class="sxs-lookup"><span data-stu-id="95333-224">b.</span></span> <span data-ttu-id="95333-225">I den **användar-ID** textruta ange användar-id för användare som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="95333-225">In the **User ID** textbox, type the user id of user like Brittasimon@contoso.com.</span></span>
   
    <span data-ttu-id="95333-226">c.</span><span class="sxs-lookup"><span data-stu-id="95333-226">c.</span></span> <span data-ttu-id="95333-227">I den **Förnamn** textruta första typnamnet för användaren som Britta.</span><span class="sxs-lookup"><span data-stu-id="95333-227">In the **First Name** textbox, type first name of the user like Britta.</span></span>

    <span data-ttu-id="95333-228">d.</span><span class="sxs-lookup"><span data-stu-id="95333-228">d.</span></span> <span data-ttu-id="95333-229">I den **efternamn** textruta Skriv Efternamn för användaren som Simon.</span><span class="sxs-lookup"><span data-stu-id="95333-229">In the **Last Name** textbox, type last name of the user like Simon.</span></span>
    
    <span data-ttu-id="95333-230">e.</span><span class="sxs-lookup"><span data-stu-id="95333-230">e.</span></span> <span data-ttu-id="95333-231">I den **plats** textruta RETUR giltig plats för ett giltigt Azure AD-kontot som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="95333-231">In the **Site** textbox, Enter the valid site of a valid Azure AD account you want to provision.</span></span>
    
    <span data-ttu-id="95333-232">f.</span><span class="sxs-lookup"><span data-stu-id="95333-232">f.</span></span> <span data-ttu-id="95333-233">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="95333-233">Click **Save**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="95333-234">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="95333-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="95333-235">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till xMatters OnDemand.</span><span class="sxs-lookup"><span data-stu-id="95333-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to xMatters OnDemand.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="95333-237">**Om du vill tilldela xMatters OnDemand Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="95333-237">**To assign Britta Simon to xMatters OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="95333-238">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="95333-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="95333-240">Välj i listan med program **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="95333-240">In the applications list, select **xMatters OnDemand**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. <span data-ttu-id="95333-242">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="95333-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="95333-244">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="95333-244">Click **Add** button.</span></span> <span data-ttu-id="95333-245">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95333-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="95333-247">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="95333-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="95333-248">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95333-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="95333-249">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95333-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="95333-250">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95333-250">Testing single sign-on</span></span>

<span data-ttu-id="95333-251">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="95333-251">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="95333-252">När du klickar på xMatters OnDemand-panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt xMatters OnDemand-programmet.</span><span class="sxs-lookup"><span data-stu-id="95333-252">When you click the xMatters OnDemand tile in the Access Panel, you should get automatically signed-on to your xMatters OnDemand application.</span></span>
<span data-ttu-id="95333-253">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="95333-253">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="95333-254">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="95333-254">Additional resources</span></span>

* [<span data-ttu-id="95333-255">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95333-255">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="95333-256">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="95333-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png

