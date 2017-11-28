---
title: "Självstudier: Azure Active Directory-integrering med Zscaler | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zscaler."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 73c81691b68ee820e1d905a17b4f2ab6b6ceb5fd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler"></a><span data-ttu-id="be278-103">Självstudier: Azure Active Directory-integrering med Zscaler</span><span class="sxs-lookup"><span data-stu-id="be278-103">Tutorial: Azure Active Directory integration with Zscaler</span></span>

<span data-ttu-id="be278-104">I kursen får lära du att integrera Zscaler med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="be278-104">In this tutorial, you learn how to integrate Zscaler with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="be278-105">Integrera Zscaler med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="be278-105">Integrating Zscaler with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="be278-106">Du kan styra i Azure AD som har åtkomst till Zscaler</span><span class="sxs-lookup"><span data-stu-id="be278-106">You can control in Azure AD who has access to Zscaler</span></span>
- <span data-ttu-id="be278-107">Du kan aktivera användarna att automatiskt hämta loggat in på Zscaler (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="be278-107">You can enable your users to automatically get signed-on to Zscaler (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="be278-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="be278-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="be278-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="be278-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be278-110">Krav</span><span class="sxs-lookup"><span data-stu-id="be278-110">Prerequisites</span></span>

<span data-ttu-id="be278-111">För att konfigurera Azure AD-integrering med Zscaler, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="be278-111">To configure Azure AD integration with Zscaler, you need the following items:</span></span>

- <span data-ttu-id="be278-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="be278-112">An Azure AD subscription</span></span>
- <span data-ttu-id="be278-113">En Zscaler enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="be278-113">A Zscaler single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="be278-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="be278-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="be278-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="be278-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="be278-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="be278-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="be278-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="be278-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="be278-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="be278-118">Scenario description</span></span>
<span data-ttu-id="be278-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="be278-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="be278-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="be278-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="be278-121">Att lägga till Zscaler från galleriet</span><span class="sxs-lookup"><span data-stu-id="be278-121">Adding Zscaler from the gallery</span></span>
2. <span data-ttu-id="be278-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be278-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-from-the-gallery"></a><span data-ttu-id="be278-123">Att lägga till Zscaler från galleriet</span><span class="sxs-lookup"><span data-stu-id="be278-123">Adding Zscaler from the gallery</span></span>
<span data-ttu-id="be278-124">Du måste lägga till Zscaler från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Zscaler i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="be278-124">To configure the integration of Zscaler into Azure AD, you need to add Zscaler from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="be278-125">**Utför följande steg för att lägga till Zscaler från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="be278-125">**To add Zscaler from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="be278-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="be278-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="be278-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="be278-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="be278-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="be278-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="be278-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="be278-133">I sökrutan skriver **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="be278-133">In the search box, type **Zscaler**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_search.png)

5. <span data-ttu-id="be278-135">Välj i resultatpanelen **Zscaler**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="be278-135">In the results panel, select **Zscaler**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="be278-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be278-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="be278-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="be278-138">In this section, you configure and test Azure AD single sign-on with Zscaler based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="be278-139">Azure AD måste du känna till användaren i Zscaler motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="be278-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler is to a user in Azure AD.</span></span> <span data-ttu-id="be278-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Zscaler upprättas.</span><span class="sxs-lookup"><span data-stu-id="be278-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler needs to be established.</span></span>

<span data-ttu-id="be278-141">I Zscaler, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="be278-141">In Zscaler, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="be278-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Zscaler, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="be278-142">To configure and test Azure AD single sign-on with Zscaler, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="be278-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="be278-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="be278-144">**[Konfigurera proxyinställningar](#configuring-proxy-settings)**  - om du vill konfigurera proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="be278-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="be278-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be278-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="be278-146">**[Skapa en testanvändare Zscaler](#creating-a-zscaler-test-user)**  – du har en motsvarighet för Britta Simon i Zscaler som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="be278-146">**[Creating a Zscaler test user](#creating-a-zscaler-test-user)** - to have a counterpart of Britta Simon in Zscaler that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="be278-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="be278-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="be278-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="be278-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="be278-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be278-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="be278-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Zscaler program.</span><span class="sxs-lookup"><span data-stu-id="be278-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler application.</span></span>

<span data-ttu-id="be278-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Zscaler:**</span><span class="sxs-lookup"><span data-stu-id="be278-151">**To configure Azure AD single sign-on with Zscaler, perform the following steps:**</span></span>

1. <span data-ttu-id="be278-152">I Azure-portalen på den **Zscaler** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="be278-152">In the Azure portal, on the **Zscaler** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="be278-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="be278-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_samlbase.png)

3. <span data-ttu-id="be278-156">På den **Zscaler domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="be278-156">On the **Zscaler Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_url.png)

    <span data-ttu-id="be278-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.zsclaer.net`</span><span class="sxs-lookup"><span data-stu-id="be278-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.zsclaer.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="be278-159">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="be278-159">This value is not real.</span></span> <span data-ttu-id="be278-160">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="be278-160">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="be278-161">Kontakta [Zscaler klienten supportteamet](https://www.zscaler.com/company/contact) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="be278-161">Contact [Zscaler Client support team](https://www.zscaler.com/company/contact) to get this value.</span></span> 

4. <span data-ttu-id="be278-162">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="be278-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_certificate.png) 

5. <span data-ttu-id="be278-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="be278-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="be278-166">På den **Zscaler Configuration** klickar du på **konfigurera Zscaler** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="be278-166">On the **Zscaler Configuration** section, click **Configure Zscaler** to open **Configure sign-on** window.</span></span> <span data-ttu-id="be278-167">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="be278-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_configure.png) 

7. <span data-ttu-id="be278-169">I en annan webbläsarfönster loggar du in på webbplatsen ZScaler företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="be278-169">In a different web browser window, log in to your ZScaler company site as an administrator.</span></span>

8. <span data-ttu-id="be278-170">Klicka på menyn högst upp **Administration**.</span><span class="sxs-lookup"><span data-stu-id="be278-170">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="be278-171">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="be278-171">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="be278-172">Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.</span><span class="sxs-lookup"><span data-stu-id="be278-172">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="be278-173">![Hantera användare och autentisering](./media/active-directory-saas-zscaler-tutorial/ic800207.png "hantera användare och autentisering")</span><span class="sxs-lookup"><span data-stu-id="be278-173">![Manage Users & Authentication](./media/active-directory-saas-zscaler-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="be278-174">I den **Välj autentiseringsalternativ för din organisation** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="be278-174">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="be278-175">![Autentisering](./media/active-directory-saas-zscaler-tutorial/ic800208.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="be278-175">![Authentication](./media/active-directory-saas-zscaler-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="be278-176">a.</span><span class="sxs-lookup"><span data-stu-id="be278-176">a.</span></span> <span data-ttu-id="be278-177">Välj **autentisera med SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="be278-177">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="be278-178">b.</span><span class="sxs-lookup"><span data-stu-id="be278-178">b.</span></span> <span data-ttu-id="be278-179">Klicka på **konfigureras SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="be278-179">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="be278-180">På den **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg och klicka sedan på **klar**</span><span class="sxs-lookup"><span data-stu-id="be278-180">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="be278-181">![Enkel inloggning](./media/active-directory-saas-zscaler-tutorial/ic800209.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="be278-181">![Single Sign-On](./media/active-directory-saas-zscaler-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="be278-182">a.</span><span class="sxs-lookup"><span data-stu-id="be278-182">a.</span></span> <span data-ttu-id="be278-183">Klistra in den **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat från Azure-portalen i den **URL för SAML-portalen som användare om autentisering skickas** textruta.</span><span class="sxs-lookup"><span data-stu-id="be278-183">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="be278-184">b.</span><span class="sxs-lookup"><span data-stu-id="be278-184">b.</span></span> <span data-ttu-id="be278-185">I den **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.</span><span class="sxs-lookup"><span data-stu-id="be278-185">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="be278-186">c.</span><span class="sxs-lookup"><span data-stu-id="be278-186">c.</span></span> <span data-ttu-id="be278-187">Om du vill överföra din hämtat certifikat klickar du på **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="be278-187">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="be278-188">d.</span><span class="sxs-lookup"><span data-stu-id="be278-188">d.</span></span> <span data-ttu-id="be278-189">Välj **aktivera etablering av SAML-automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="be278-189">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="be278-190">På den **konfigurera användarautentisering** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="be278-190">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="be278-191">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="be278-191">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="be278-192">a.</span><span class="sxs-lookup"><span data-stu-id="be278-192">a.</span></span> <span data-ttu-id="be278-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="be278-193">Click **Save**.</span></span>

    <span data-ttu-id="be278-194">b.</span><span class="sxs-lookup"><span data-stu-id="be278-194">b.</span></span> <span data-ttu-id="be278-195">Klicka på **aktivera nu**.</span><span class="sxs-lookup"><span data-stu-id="be278-195">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="be278-196">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="be278-196">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="be278-197">Konfigurera proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="be278-197">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="be278-198">Starta **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="be278-198">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="be278-199">Välj **Internetalternativ** från den **verktyg** menyn för att öppna den **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-199">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="be278-200">![Internet-alternativ](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet-alternativ")</span><span class="sxs-lookup"><span data-stu-id="be278-200">![Internet Options](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="be278-201">Klicka på den **anslutningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="be278-201">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="be278-202">![Anslutningar](./media/active-directory-saas-zscaler-tutorial/ic769493.png "anslutningar")</span><span class="sxs-lookup"><span data-stu-id="be278-202">![Connections](./media/active-directory-saas-zscaler-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="be278-203">Klicka på **LAN-inställningar** att öppna den **LAN-inställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-203">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="be278-204">Utför följande steg i avsnittet Proxy-server:</span><span class="sxs-lookup"><span data-stu-id="be278-204">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="be278-205">![Proxyserver](./media/active-directory-saas-zscaler-tutorial/ic769494.png "proxyserver")</span><span class="sxs-lookup"><span data-stu-id="be278-205">![Proxy server](./media/active-directory-saas-zscaler-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="be278-206">a.</span><span class="sxs-lookup"><span data-stu-id="be278-206">a.</span></span> <span data-ttu-id="be278-207">Välj **använder en proxyserver för nätverket**.</span><span class="sxs-lookup"><span data-stu-id="be278-207">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="be278-208">b.</span><span class="sxs-lookup"><span data-stu-id="be278-208">b.</span></span> <span data-ttu-id="be278-209">Ange i textrutan adress **gateway.zscaler.net**.</span><span class="sxs-lookup"><span data-stu-id="be278-209">In the Address textbox, type **gateway.zscaler.net**.</span></span>

    <span data-ttu-id="be278-210">c.</span><span class="sxs-lookup"><span data-stu-id="be278-210">c.</span></span> <span data-ttu-id="be278-211">Ange i textrutan Port **80**.</span><span class="sxs-lookup"><span data-stu-id="be278-211">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="be278-212">d.</span><span class="sxs-lookup"><span data-stu-id="be278-212">d.</span></span> <span data-ttu-id="be278-213">Välj **Använd ingen proxyserver för lokala adresser**.</span><span class="sxs-lookup"><span data-stu-id="be278-213">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="be278-214">e.</span><span class="sxs-lookup"><span data-stu-id="be278-214">e.</span></span> <span data-ttu-id="be278-215">Klicka på **OK** att stänga den **inställningar för lokalt nätverk (LAN)** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-215">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="be278-216">Klicka på **OK** att stänga den **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-216">Click **OK** to close the **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="be278-217">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="be278-217">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="be278-218">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="be278-218">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="be278-219">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="be278-219">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="be278-220">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="be278-220">Creating an Azure AD test user</span></span>
<span data-ttu-id="be278-221">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be278-221">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="be278-223">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="be278-223">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="be278-224">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="be278-224">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="be278-226">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="be278-226">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="be278-228">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-228">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="be278-230">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="be278-230">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="be278-232">a.</span><span class="sxs-lookup"><span data-stu-id="be278-232">a.</span></span> <span data-ttu-id="be278-233">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="be278-233">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="be278-234">b.</span><span class="sxs-lookup"><span data-stu-id="be278-234">b.</span></span> <span data-ttu-id="be278-235">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="be278-235">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="be278-236">c.</span><span class="sxs-lookup"><span data-stu-id="be278-236">c.</span></span> <span data-ttu-id="be278-237">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="be278-237">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="be278-238">d.</span><span class="sxs-lookup"><span data-stu-id="be278-238">d.</span></span> <span data-ttu-id="be278-239">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="be278-239">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-test-user"></a><span data-ttu-id="be278-240">Skapa en testanvändare Zscaler</span><span class="sxs-lookup"><span data-stu-id="be278-240">Creating a Zscaler test user</span></span>

<span data-ttu-id="be278-241">Om du vill aktivera Azure AD-användare kan logga in på ZScaler etableras de för ZScaler.</span><span class="sxs-lookup"><span data-stu-id="be278-241">To enable Azure AD users to log in to ZScaler, they must be provisioned to ZScaler.</span></span>  
<span data-ttu-id="be278-242">När det gäller ZScaler är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="be278-242">In the case of ZScaler, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="be278-243">Utför följande steg för att konfigurera användaretablering:</span><span class="sxs-lookup"><span data-stu-id="be278-243">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="be278-244">Logga in på ditt **Zscaler** klient.</span><span class="sxs-lookup"><span data-stu-id="be278-244">Log in to your **Zscaler** tenant.</span></span>

2. <span data-ttu-id="be278-245">Klicka på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="be278-245">Click **Administration**.</span></span>   
   
    <span data-ttu-id="be278-246">![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="be278-246">![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="be278-247">Klicka på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="be278-247">Click **User Management**.</span></span>   
        
     <span data-ttu-id="be278-248">![Lägg till](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="be278-248">![Add](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="be278-249">I den **användare** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="be278-249">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="be278-250">![Lägg till](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="be278-250">![Add](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="be278-251">Utför följande steg i avsnittet Lägg till användare:</span><span class="sxs-lookup"><span data-stu-id="be278-251">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="be278-252">![Lägg till användare](./media/active-directory-saas-zscaler-tutorial/ic781038.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="be278-252">![Add User](./media/active-directory-saas-zscaler-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="be278-253">a.</span><span class="sxs-lookup"><span data-stu-id="be278-253">a.</span></span> <span data-ttu-id="be278-254">Typ av **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och **avdelning** av en giltig AAD-konto som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="be278-254">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="be278-255">b.</span><span class="sxs-lookup"><span data-stu-id="be278-255">b.</span></span> <span data-ttu-id="be278-256">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="be278-256">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="be278-257">Du kan använda något annat ZScaler användarens konto skapas verktyg eller API: er som tillhandahålls av ZScaler att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="be278-257">You can use any other ZScaler user account creation tools or APIs provided by ZScaler to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="be278-258">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="be278-258">Assigning the Azure AD test user</span></span>

<span data-ttu-id="be278-259">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Zscaler.</span><span class="sxs-lookup"><span data-stu-id="be278-259">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="be278-261">**Om du vill tilldela Zscaler Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="be278-261">**To assign Britta Simon to Zscaler, perform the following steps:**</span></span>

1. <span data-ttu-id="be278-262">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="be278-262">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="be278-264">Välj i listan med program **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="be278-264">In the applications list, select **Zscaler**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_app.png) 

3. <span data-ttu-id="be278-266">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="be278-266">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="be278-268">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="be278-268">Click **Add** button.</span></span> <span data-ttu-id="be278-269">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-269">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="be278-271">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="be278-271">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="be278-272">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-272">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="be278-273">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be278-273">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="be278-274">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be278-274">Testing single sign-on</span></span>

<span data-ttu-id="be278-275">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="be278-275">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="be278-276">När du klickar på panelen Zscaler på åtkomstpanelen du bör få automatiskt loggat in på ditt Zscaler program.</span><span class="sxs-lookup"><span data-stu-id="be278-276">When you click the Zscaler tile in the Access Panel, you should get automatically signed-on to your Zscaler application.</span></span>
<span data-ttu-id="be278-277">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="be278-277">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be278-278">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="be278-278">Additional resources</span></span>

* [<span data-ttu-id="be278-279">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="be278-279">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="be278-280">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="be278-280">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_203.png

