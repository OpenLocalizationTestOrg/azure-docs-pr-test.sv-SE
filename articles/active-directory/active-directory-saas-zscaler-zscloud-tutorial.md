---
title: "Självstudier: Azure Active Directory-integrering med Zscaler ZSCloud | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zscaler ZSCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 2b6eb113e5725260bc04f50e9218939bf28b1ff0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a><span data-ttu-id="0468d-103">Självstudier: Azure Active Directory-integrering med Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="0468d-103">Tutorial: Azure Active Directory integration with Zscaler ZSCloud</span></span>

<span data-ttu-id="0468d-104">I kursen får lära du att integrera Zscaler ZSCloud med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0468d-104">In this tutorial, you learn how to integrate Zscaler ZSCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0468d-105">Integrera Zscaler ZSCloud med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0468d-105">Integrating Zscaler ZSCloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0468d-106">Du kan styra i Azure AD som har åtkomst till Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="0468d-106">You can control in Azure AD who has access to Zscaler ZSCloud</span></span>
- <span data-ttu-id="0468d-107">Du kan aktivera användarna att automatiskt hämta loggat in på Zscaler ZSCloud (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0468d-107">You can enable your users to automatically get signed-on to Zscaler ZSCloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0468d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0468d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0468d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0468d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0468d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0468d-110">Prerequisites</span></span>

<span data-ttu-id="0468d-111">För att konfigurera Azure AD-integrering med Zscaler ZSCloud, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0468d-111">To configure Azure AD integration with Zscaler ZSCloud, you need the following items:</span></span>

- <span data-ttu-id="0468d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0468d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0468d-113">En Zscaler ZSCloud enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="0468d-113">A Zscaler ZSCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0468d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0468d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0468d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0468d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0468d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0468d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0468d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0468d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0468d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0468d-118">Scenario description</span></span>
<span data-ttu-id="0468d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0468d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0468d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0468d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0468d-121">Att lägga till Zscaler ZSCloud från galleriet</span><span class="sxs-lookup"><span data-stu-id="0468d-121">Adding Zscaler ZSCloud from the gallery</span></span>
2. <span data-ttu-id="0468d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0468d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-zscloud-from-the-gallery"></a><span data-ttu-id="0468d-123">Att lägga till Zscaler ZSCloud från galleriet</span><span class="sxs-lookup"><span data-stu-id="0468d-123">Adding Zscaler ZSCloud from the gallery</span></span>
<span data-ttu-id="0468d-124">Du måste lägga till Zscaler ZSCloud från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Zscaler ZSCloud i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0468d-124">To configure the integration of Zscaler ZSCloud into Azure AD, you need to add Zscaler ZSCloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0468d-125">**Utför följande steg för att lägga till Zscaler ZSCloud från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="0468d-125">**To add Zscaler ZSCloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0468d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0468d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0468d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0468d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0468d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0468d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0468d-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="0468d-133">I sökrutan skriver **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="0468d-133">In the search box, type **Zscaler ZSCloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. <span data-ttu-id="0468d-135">Välj i resultatpanelen **Zscaler ZSCloud**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="0468d-135">In the results panel, select **Zscaler ZSCloud**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0468d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0468d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0468d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler ZSCloud baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0468d-138">In this section, you configure and test Azure AD single sign-on with Zscaler ZSCloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0468d-139">Azure AD måste du känna till användaren i Zscaler ZSCloud motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="0468d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler ZSCloud is to a user in Azure AD.</span></span> <span data-ttu-id="0468d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Zscaler ZSCloud upprättas.</span><span class="sxs-lookup"><span data-stu-id="0468d-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler ZSCloud needs to be established.</span></span>

<span data-ttu-id="0468d-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="0468d-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zscaler ZSCloud.</span></span>

<span data-ttu-id="0468d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Zscaler ZSCloud, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0468d-142">To configure and test Azure AD single sign-on with Zscaler ZSCloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0468d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0468d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0468d-144">**[Konfigurera proxyinställningar](#configuring-proxy-settings)**  - om du vill konfigurera proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="0468d-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
2. <span data-ttu-id="0468d-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0468d-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)**  - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0468d-146">**[Skapa en testanvändare Zscaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**  – har en motsvarighet för Britta Simon Zscaler ZSCloud som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0468d-146">**[Creating a Zscaler ZSCloud test user](#creating-a-zscaler-zscloud-test-user)** - to have a counterpart of Britta Simon in Zscaler ZSCloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0468d-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0468d-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0468d-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0468d-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0468d-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0468d-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0468d-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="0468d-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="0468d-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Zscaler ZSCloud:**</span><span class="sxs-lookup"><span data-stu-id="0468d-151">**To configure Azure AD single sign-on with Zscaler ZSCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="0468d-152">I Azure-portalen på den **Zscaler ZSCloud** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0468d-152">In the Azure portal, on the **Zscaler ZSCloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0468d-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0468d-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. <span data-ttu-id="0468d-156">På den **Zscaler ZSCloud domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0468d-156">On the **Zscaler ZSCloud Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     <span data-ttu-id="0468d-158">I den **inloggnings-URL** textruta, Skriv URL: en som används av dina användare logga in i tillämpningsprogrammet ZScaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="0468d-158">In the **Sign-on URL** textbox, type the URL used by your users to sign-on to your ZScaler ZSCloud application.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="0468d-159">Du måste uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="0468d-159">You have to update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="0468d-160">Kontakta [Zscaler ZSCloud klienten supportteamet](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="0468d-160">Contact [Zscaler ZSCloud Client support team](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) to get this value.</span></span> 
 
4. <span data-ttu-id="0468d-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="0468d-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. <span data-ttu-id="0468d-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0468d-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0468d-165">På den **Zscaler ZSCloud Configuration** klickar du på **konfigurera Zscaler ZSCloud** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="0468d-165">On the **Zscaler ZSCloud Configuration** section, click **Configure Zscaler ZSCloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0468d-166">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="0468d-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. <span data-ttu-id="0468d-168">I en annan webbläsarfönster loggar du in på webbplatsen ZScaler ZSCloud företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="0468d-168">In a different web browser window, log in to your ZScaler ZSCloud company site as an administrator.</span></span>

8. <span data-ttu-id="0468d-169">Klicka på menyn högst upp **Administration**.</span><span class="sxs-lookup"><span data-stu-id="0468d-169">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="0468d-170">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="0468d-170">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="0468d-171">Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.</span><span class="sxs-lookup"><span data-stu-id="0468d-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="0468d-172">![Hantera användare och autentisering](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "hantera användare och autentisering")</span><span class="sxs-lookup"><span data-stu-id="0468d-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="0468d-173">I den **Välj autentiseringsalternativ för din organisation** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0468d-173">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="0468d-174">![Autentisering](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="0468d-174">![Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="0468d-175">a.</span><span class="sxs-lookup"><span data-stu-id="0468d-175">a.</span></span> <span data-ttu-id="0468d-176">Välj **autentisera med SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0468d-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="0468d-177">b.</span><span class="sxs-lookup"><span data-stu-id="0468d-177">b.</span></span> <span data-ttu-id="0468d-178">Klicka på **konfigureras SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0468d-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="0468d-179">På den **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg och klicka sedan på **klar**</span><span class="sxs-lookup"><span data-stu-id="0468d-179">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="0468d-180">![Enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="0468d-180">![Single Sign-On](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="0468d-181">a.</span><span class="sxs-lookup"><span data-stu-id="0468d-181">a.</span></span> <span data-ttu-id="0468d-182">Klistra in den **SAML inloggning tjänst-URL för enkel** värde i den **URL för SAML-portalen som användare om autentisering skickas** textruta.</span><span class="sxs-lookup"><span data-stu-id="0468d-182">Paste the **SAML Single Sign-On Service URL** value into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="0468d-183">b.</span><span class="sxs-lookup"><span data-stu-id="0468d-183">b.</span></span> <span data-ttu-id="0468d-184">I den **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.</span><span class="sxs-lookup"><span data-stu-id="0468d-184">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="0468d-185">c.</span><span class="sxs-lookup"><span data-stu-id="0468d-185">c.</span></span> <span data-ttu-id="0468d-186">Om du vill överföra din hämtat certifikat klickar du på **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="0468d-186">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="0468d-187">d.</span><span class="sxs-lookup"><span data-stu-id="0468d-187">d.</span></span> <span data-ttu-id="0468d-188">Välj **aktivera etablering av SAML-automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="0468d-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="0468d-189">På den **konfigurera användarautentisering** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0468d-189">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="0468d-190">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="0468d-190">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="0468d-191">a.</span><span class="sxs-lookup"><span data-stu-id="0468d-191">a.</span></span> <span data-ttu-id="0468d-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0468d-192">Click **Save**.</span></span>

    <span data-ttu-id="0468d-193">b.</span><span class="sxs-lookup"><span data-stu-id="0468d-193">b.</span></span> <span data-ttu-id="0468d-194">Klicka på **aktivera nu**.</span><span class="sxs-lookup"><span data-stu-id="0468d-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="0468d-195">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="0468d-195">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="0468d-196">Konfigurera proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="0468d-196">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="0468d-197">Starta **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="0468d-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="0468d-198">Välj **Internetalternativ** från den **verktyg** menyn för att öppna den **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-198">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="0468d-199">![Internet-alternativ](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet-alternativ")</span><span class="sxs-lookup"><span data-stu-id="0468d-199">![Internet Options](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="0468d-200">Klicka på den **anslutningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="0468d-200">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="0468d-201">![Anslutningar](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "anslutningar")</span><span class="sxs-lookup"><span data-stu-id="0468d-201">![Connections](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="0468d-202">Klicka på **LAN-inställningar** att öppna den **LAN-inställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-202">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="0468d-203">Utför följande steg i avsnittet Proxy-server:</span><span class="sxs-lookup"><span data-stu-id="0468d-203">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="0468d-204">![Proxyserver](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "proxyserver")</span><span class="sxs-lookup"><span data-stu-id="0468d-204">![Proxy server](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="0468d-205">a.</span><span class="sxs-lookup"><span data-stu-id="0468d-205">a.</span></span> <span data-ttu-id="0468d-206">Välj **använder en proxyserver för nätverket**.</span><span class="sxs-lookup"><span data-stu-id="0468d-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="0468d-207">b.</span><span class="sxs-lookup"><span data-stu-id="0468d-207">b.</span></span> <span data-ttu-id="0468d-208">Ange i textrutan adress **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="0468d-208">In the Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="0468d-209">c.</span><span class="sxs-lookup"><span data-stu-id="0468d-209">c.</span></span> <span data-ttu-id="0468d-210">Ange i textrutan Port **80**.</span><span class="sxs-lookup"><span data-stu-id="0468d-210">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="0468d-211">d.</span><span class="sxs-lookup"><span data-stu-id="0468d-211">d.</span></span> <span data-ttu-id="0468d-212">Välj **Använd ingen proxyserver för lokala adresser**.</span><span class="sxs-lookup"><span data-stu-id="0468d-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="0468d-213">e.</span><span class="sxs-lookup"><span data-stu-id="0468d-213">e.</span></span> <span data-ttu-id="0468d-214">Klicka på **OK** att stänga den **inställningar för lokalt nätverk (LAN)** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-214">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="0468d-215">Klicka på **OK** att stänga den **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-215">Click **OK** to close the **Internet Options** dialog.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0468d-216">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0468d-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="0468d-217">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0468d-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0468d-219">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="0468d-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0468d-220">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0468d-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0468d-222">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0468d-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0468d-224">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0468d-226">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0468d-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0468d-228">a.</span><span class="sxs-lookup"><span data-stu-id="0468d-228">a.</span></span> <span data-ttu-id="0468d-229">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0468d-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0468d-230">b.</span><span class="sxs-lookup"><span data-stu-id="0468d-230">b.</span></span> <span data-ttu-id="0468d-231">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0468d-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0468d-232">c.</span><span class="sxs-lookup"><span data-stu-id="0468d-232">c.</span></span> <span data-ttu-id="0468d-233">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0468d-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0468d-234">d.</span><span class="sxs-lookup"><span data-stu-id="0468d-234">d.</span></span> <span data-ttu-id="0468d-235">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0468d-235">Click **Create**.</span></span>

### <a name="creating-a-zscaler-zscloud-test-user"></a><span data-ttu-id="0468d-236">Skapa en testanvändare Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="0468d-236">Creating a Zscaler ZSCloud test user</span></span>

<span data-ttu-id="0468d-237">Om du vill aktivera Azure AD-användare kan logga in på ZScaler ZSCloud etableras de till ZScaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="0468d-237">To enable Azure AD users to log in to ZScaler ZSCloud, they must be provisioned to ZScaler ZSCloud.</span></span>  
<span data-ttu-id="0468d-238">När det gäller ZScaler ZSCloud är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0468d-238">In the case of ZScaler ZSCloud, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="0468d-239">Utför följande steg för att konfigurera användaretablering:</span><span class="sxs-lookup"><span data-stu-id="0468d-239">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="0468d-240">Logga in på ditt **Zscaler** klient.</span><span class="sxs-lookup"><span data-stu-id="0468d-240">Log in to your **Zscaler** tenant.</span></span>

2. <span data-ttu-id="0468d-241">Klicka på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="0468d-241">Click **Administration**.</span></span>   
   
    <span data-ttu-id="0468d-242">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="0468d-242">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="0468d-243">Klicka på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="0468d-243">Click **User Management**.</span></span>   
        
     <span data-ttu-id="0468d-244">![Lägg till](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="0468d-244">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

4. <span data-ttu-id="0468d-245">I den **användare** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0468d-245">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="0468d-246">![Lägg till](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="0468d-246">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="0468d-247">Utför följande steg i avsnittet Lägg till användare:</span><span class="sxs-lookup"><span data-stu-id="0468d-247">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="0468d-248">![Lägg till användare](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="0468d-248">![Add User](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="0468d-249">a.</span><span class="sxs-lookup"><span data-stu-id="0468d-249">a.</span></span> <span data-ttu-id="0468d-250">Typ av **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och **avdelning** av en giltig AAD-konto som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="0468d-250">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="0468d-251">b.</span><span class="sxs-lookup"><span data-stu-id="0468d-251">b.</span></span> <span data-ttu-id="0468d-252">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0468d-252">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="0468d-253">Du kan använda något annat ZScaler ZSCloud användarens konto skapas verktyg eller API: er som tillhandahålls av ZScaler ZSCloud att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="0468d-253">You can use any other ZScaler ZSCloud user account creation tools or APIs provided by ZScaler ZSCloud to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0468d-254">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="0468d-254">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0468d-255">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="0468d-255">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler ZSCloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0468d-257">**Om du vill tilldela Zscaler ZSCloud Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0468d-257">**To assign Britta Simon to Zscaler ZSCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="0468d-258">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0468d-258">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0468d-260">Välj i listan med program **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="0468d-260">In the applications list, select **Zscaler ZSCloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. <span data-ttu-id="0468d-262">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0468d-262">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0468d-264">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0468d-264">Click **Add** button.</span></span> <span data-ttu-id="0468d-265">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0468d-267">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="0468d-267">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0468d-268">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0468d-269">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0468d-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0468d-270">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0468d-270">Testing single sign-on</span></span>

<span data-ttu-id="0468d-271">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0468d-271">If you want to test your single sign-on settings, open the Access Panel.</span></span>

<span data-ttu-id="0468d-272">När du klickar på panelen Zscaler ZSCloud på åtkomstpanelen du bör få automatiskt loggat in på ditt Zscaler ZSCloud program.</span><span class="sxs-lookup"><span data-stu-id="0468d-272">When you click the Zscaler ZSCloud tile in the Access Panel, you should get automatically signed-on to your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="0468d-273">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0468d-273">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0468d-274">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0468d-274">Additional resources</span></span>

* [<span data-ttu-id="0468d-275">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0468d-275">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0468d-276">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0468d-276">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png

