---
title: "Självstudier: Azure Active Directory-integrering med arbetsytan Lms | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och arbetsytan LMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: 2212b7a81b66d1afd1aa78d1487b07b6d7b84129
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a><span data-ttu-id="292aa-103">Självstudier: Azure Active Directory-integrering med arbetsytan LMS</span><span class="sxs-lookup"><span data-stu-id="292aa-103">Tutorial: Azure Active Directory integration with Canvas LMS</span></span>

<span data-ttu-id="292aa-104">I kursen får lära du att integrera arbetsytan med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="292aa-104">In this tutorial, you learn how to integrate Canvas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="292aa-105">Integrera arbetsytan med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="292aa-105">Integrating Canvas with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="292aa-106">Du kan styra i Azure AD som har åtkomst till arbetsytan</span><span class="sxs-lookup"><span data-stu-id="292aa-106">You can control in Azure AD who has access to Canvas</span></span>
- <span data-ttu-id="292aa-107">Du kan aktivera användarna att automatiskt hämta loggat in på arbetsytan (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="292aa-107">You can enable your users to automatically get signed-on to Canvas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="292aa-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="292aa-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="292aa-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="292aa-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="292aa-110">Krav</span><span class="sxs-lookup"><span data-stu-id="292aa-110">Prerequisites</span></span>

<span data-ttu-id="292aa-111">För att konfigurera Azure AD-integrering med arbetsyta, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="292aa-111">To configure Azure AD integration with Canvas, you need the following items:</span></span>

- <span data-ttu-id="292aa-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="292aa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="292aa-113">En arbetsyta enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="292aa-113">A Canvas single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="292aa-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="292aa-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="292aa-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="292aa-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="292aa-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="292aa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="292aa-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="292aa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="292aa-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="292aa-118">Scenario description</span></span>
<span data-ttu-id="292aa-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="292aa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="292aa-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="292aa-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="292aa-121">Att lägga till arbetsytan från galleriet</span><span class="sxs-lookup"><span data-stu-id="292aa-121">Adding Canvas from the gallery</span></span>
2. <span data-ttu-id="292aa-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="292aa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-canvas-from-the-gallery"></a><span data-ttu-id="292aa-123">Att lägga till arbetsytan från galleriet</span><span class="sxs-lookup"><span data-stu-id="292aa-123">Adding Canvas from the gallery</span></span>
<span data-ttu-id="292aa-124">Du måste lägga till arbetsytan från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av arbetsytan i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="292aa-124">To configure the integration of Canvas into Azure AD, you need to add Canvas from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="292aa-125">**Utför följande steg för att lägga till arbetsytan från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="292aa-125">**To add Canvas from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="292aa-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="292aa-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="292aa-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="292aa-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="292aa-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="292aa-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="292aa-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="292aa-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="292aa-133">I sökrutan skriver **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="292aa-133">In the search box, type **Canvas**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. <span data-ttu-id="292aa-135">Välj i resultatpanelen **arbetsytan**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="292aa-135">In the results panel, select **Canvas**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="292aa-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="292aa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="292aa-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med arbetsytan baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="292aa-138">In this section, you configure and test Azure AD single sign-on with Canvas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="292aa-139">Azure AD måste du känna till motsvarande användaren i arbetsytan till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="292aa-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Canvas is to a user in Azure AD.</span></span> <span data-ttu-id="292aa-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i arbetsytan upprättas.</span><span class="sxs-lookup"><span data-stu-id="292aa-140">In other words, a link relationship between an Azure AD user and the related user in Canvas needs to be established.</span></span>

<span data-ttu-id="292aa-141">I arbetsytan och tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="292aa-141">In Canvas, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="292aa-142">Om du vill konfigurera och testa Azure AD enkel inloggning med arbetsyta, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="292aa-142">To configure and test Azure AD single sign-on with Canvas, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="292aa-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="292aa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="292aa-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="292aa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="292aa-145">**[Skapa en arbetsyta testanvändare](#creating-a-canvas-test-user)**  – du har en motsvarighet för Britta Simon arbetsytan som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="292aa-145">**[Creating a Canvas test user](#creating-a-canvas-test-user)** - to have a counterpart of Britta Simon in Canvas that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="292aa-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="292aa-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="292aa-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="292aa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="292aa-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="292aa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="292aa-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program för arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="292aa-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Canvas application.</span></span>

<span data-ttu-id="292aa-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med arbetsyta:**</span><span class="sxs-lookup"><span data-stu-id="292aa-150">**To configure Azure AD single sign-on with Canvas, perform the following steps:**</span></span>

1. <span data-ttu-id="292aa-151">I Azure-portalen på den **arbetsytan** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="292aa-151">In the Azure portal, on the **Canvas** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="292aa-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="292aa-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. <span data-ttu-id="292aa-155">På den **arbetsytan domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="292aa-155">On the **Canvas Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    <span data-ttu-id="292aa-157">a.</span><span class="sxs-lookup"><span data-stu-id="292aa-157">a.</span></span> <span data-ttu-id="292aa-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant-name>.instructure.com`</span><span class="sxs-lookup"><span data-stu-id="292aa-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.instructure.com`</span></span>

    <span data-ttu-id="292aa-159">b.</span><span class="sxs-lookup"><span data-stu-id="292aa-159">b.</span></span> <span data-ttu-id="292aa-160">I den **identifierare** textruta Skriv det värde som använder följande mönster:`https://<tenant-name>.instructure.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="292aa-160">In the **Identifier** textbox, type the value using the following pattern: `https://<tenant-name>.instructure.com/saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="292aa-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="292aa-161">These values are not real.</span></span> <span data-ttu-id="292aa-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="292aa-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="292aa-163">Kontakta [arbetsytan klienten supportteamet](https://community.canvaslms.com/community/help) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="292aa-163">Contact [Canvas Client support team](https://community.canvaslms.com/community/help) to get these values.</span></span> 
 
4. <span data-ttu-id="292aa-164">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="292aa-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. <span data-ttu-id="292aa-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="292aa-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="292aa-168">På den **arbetsytan Configuration** klickar du på **konfigurera arbetsytan** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="292aa-168">On the **Canvas Configuration** section, click **Configure Canvas** to open **Configure sign-on** window.</span></span> <span data-ttu-id="292aa-169">Kopiera den **ändra lösenord URL, Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="292aa-169">Copy the **Change Password URL, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. <span data-ttu-id="292aa-171">I en annan webbläsarfönster loggar du in på webbplatsen arbetsytan företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="292aa-171">In a different web browser window, log in to your Canvas company site as an administrator.</span></span>

8. <span data-ttu-id="292aa-172">Gå till **kurser \> hanterade konton \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="292aa-172">Go to **Courses \> Managed Accounts \> Microsoft**.</span></span>
   
    <span data-ttu-id="292aa-173">![Arbetsytan](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "arbetsytan")</span><span class="sxs-lookup"><span data-stu-id="292aa-173">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

9. <span data-ttu-id="292aa-174">I navigeringsfönstret till vänster, Välj **autentisering**, och klicka sedan på **Lägg till ny SAML-Config**.</span><span class="sxs-lookup"><span data-stu-id="292aa-174">In the navigation pane on the left, select **Authentication**, and then click **Add New SAML Config**.</span></span>
   
    <span data-ttu-id="292aa-175">![Autentisering](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="292aa-175">![Authentication](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentication")</span></span>

10. <span data-ttu-id="292aa-176">Utför följande steg på sidan aktuella integrering:</span><span class="sxs-lookup"><span data-stu-id="292aa-176">On the Current Integration page, perform the following steps:</span></span>
   
    <span data-ttu-id="292aa-177">![Aktuella Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "aktuella integrering")</span><span class="sxs-lookup"><span data-stu-id="292aa-177">![Current Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Current Integration")</span></span>

    <span data-ttu-id="292aa-178">a.</span><span class="sxs-lookup"><span data-stu-id="292aa-178">a.</span></span> <span data-ttu-id="292aa-179">I **IdP enhets-ID** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="292aa-179">In **IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="292aa-180">b.</span><span class="sxs-lookup"><span data-stu-id="292aa-180">b.</span></span> <span data-ttu-id="292aa-181">I **loggen URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="292aa-181">In **Log On URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal .</span></span>

    <span data-ttu-id="292aa-182">c.</span><span class="sxs-lookup"><span data-stu-id="292aa-182">c.</span></span> <span data-ttu-id="292aa-183">I **logga ut URL** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="292aa-183">In **Log Out URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="292aa-184">d.</span><span class="sxs-lookup"><span data-stu-id="292aa-184">d.</span></span> <span data-ttu-id="292aa-185">I **ändra lösenord länk** textruta klistra in värdet för **ändra lösenord URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="292aa-185">In **Change Password Link** textbox, paste the value of **Change Password URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="292aa-186">e.</span><span class="sxs-lookup"><span data-stu-id="292aa-186">e.</span></span> <span data-ttu-id="292aa-187">I **certifikat fingeravtryck** textruta klistra in den **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="292aa-187">In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>      
        
    <span data-ttu-id="292aa-188">f.</span><span class="sxs-lookup"><span data-stu-id="292aa-188">f.</span></span> <span data-ttu-id="292aa-189">Från den **inloggningen attributet** väljer **NameID**.</span><span class="sxs-lookup"><span data-stu-id="292aa-189">From the **Login Attribute** list, select **NameID**.</span></span>

    <span data-ttu-id="292aa-190">g.</span><span class="sxs-lookup"><span data-stu-id="292aa-190">g.</span></span> <span data-ttu-id="292aa-191">Från den **identifierare Format** väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="292aa-191">From the **Identifier Format** list, select **emailAddress**.</span></span>

    <span data-ttu-id="292aa-192">h.</span><span class="sxs-lookup"><span data-stu-id="292aa-192">h.</span></span> <span data-ttu-id="292aa-193">Klicka på **spara autentiseringsinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="292aa-193">Click **Save Authentication Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="292aa-194">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="292aa-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="292aa-195">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="292aa-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="292aa-196">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="292aa-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="292aa-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="292aa-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="292aa-198">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="292aa-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="292aa-200">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="292aa-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="292aa-201">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="292aa-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="292aa-203">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="292aa-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="292aa-205">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="292aa-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="292aa-207">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="292aa-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="292aa-209">a.</span><span class="sxs-lookup"><span data-stu-id="292aa-209">a.</span></span> <span data-ttu-id="292aa-210">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="292aa-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="292aa-211">b.</span><span class="sxs-lookup"><span data-stu-id="292aa-211">b.</span></span> <span data-ttu-id="292aa-212">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="292aa-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="292aa-213">c.</span><span class="sxs-lookup"><span data-stu-id="292aa-213">c.</span></span> <span data-ttu-id="292aa-214">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="292aa-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="292aa-215">d.</span><span class="sxs-lookup"><span data-stu-id="292aa-215">d.</span></span> <span data-ttu-id="292aa-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="292aa-216">Click **Create**.</span></span>
 
### <a name="creating-a-canvas-test-user"></a><span data-ttu-id="292aa-217">Skapa en arbetsyta testanvändare</span><span class="sxs-lookup"><span data-stu-id="292aa-217">Creating a Canvas test user</span></span>

<span data-ttu-id="292aa-218">Om du vill aktivera Azure AD-användare kan logga in på arbetsytan, måste de etableras i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="292aa-218">To enable Azure AD users to log in to Canvas, they must be provisioned into Canvas.</span></span>

<span data-ttu-id="292aa-219">Om arbetsytan är användaretablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="292aa-219">In case of Canvas, user provisioning is a manual task.</span></span>

<span data-ttu-id="292aa-220">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="292aa-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="292aa-221">Logga in på ditt **arbetsytan** klient.</span><span class="sxs-lookup"><span data-stu-id="292aa-221">Log in to your **Canvas** tenant.</span></span>

2. <span data-ttu-id="292aa-222">Gå till **kurser \> hanterade konton \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="292aa-222">Go to **Courses \> Managed Accounts \> Microsoft**.</span></span>
   
   <span data-ttu-id="292aa-223">![Arbetsytan](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "arbetsytan")</span><span class="sxs-lookup"><span data-stu-id="292aa-223">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

3. <span data-ttu-id="292aa-224">Klicka på **användare**.</span><span class="sxs-lookup"><span data-stu-id="292aa-224">Click **Users**.</span></span>
   
   <span data-ttu-id="292aa-225">![Användare](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "användare")</span><span class="sxs-lookup"><span data-stu-id="292aa-225">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Users")</span></span>

4. <span data-ttu-id="292aa-226">Klicka på **lägga till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="292aa-226">Click **Add New User**.</span></span>
   
   <span data-ttu-id="292aa-227">![Användare](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "användare")</span><span class="sxs-lookup"><span data-stu-id="292aa-227">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Users")</span></span>

5. <span data-ttu-id="292aa-228">Utför följande steg på guiden Lägg till en ny användare dialogrutan sida:</span><span class="sxs-lookup"><span data-stu-id="292aa-228">On the Add a New User dialog page, perform the following steps:</span></span>
   
   <span data-ttu-id="292aa-229">![Lägg till användare](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="292aa-229">![Add User](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Add User")</span></span>
   
   <span data-ttu-id="292aa-230">a.</span><span class="sxs-lookup"><span data-stu-id="292aa-230">a.</span></span> <span data-ttu-id="292aa-231">I den **fullständiga namn** textruta anger du namnet på användaren som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="292aa-231">In the **Full Name** textbox, enter the name of user like **BrittaSimon**.</span></span>

   <span data-ttu-id="292aa-232">b.</span><span class="sxs-lookup"><span data-stu-id="292aa-232">b.</span></span> <span data-ttu-id="292aa-233">I den **e-post** textruta ange e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="292aa-233">In the **Email** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="292aa-234">c.</span><span class="sxs-lookup"><span data-stu-id="292aa-234">c.</span></span> <span data-ttu-id="292aa-235">I den **inloggning** textruta ange användarens Azure AD e-postadress som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="292aa-235">In the **Login** textbox, enter the user’s Azure AD email address like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="292aa-236">d.</span><span class="sxs-lookup"><span data-stu-id="292aa-236">d.</span></span> <span data-ttu-id="292aa-237">Välj **e-användaren om det här kontot skapas**.</span><span class="sxs-lookup"><span data-stu-id="292aa-237">Select **Email the user about this account creation**.</span></span>

   <span data-ttu-id="292aa-238">e.</span><span class="sxs-lookup"><span data-stu-id="292aa-238">e.</span></span> <span data-ttu-id="292aa-239">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="292aa-239">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="292aa-240">Du kan använda något annat arbetsytan användarens konto skapas verktyg eller API: er som tillhandahålls av arbetsytan etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="292aa-240">You can use any other Canvas user account creation tools or APIs provided by Canvas to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="292aa-241">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="292aa-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="292aa-242">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="292aa-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Canvas.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="292aa-244">**Om du vill tilldela arbetsytan Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="292aa-244">**To assign Britta Simon to Canvas, perform the following steps:**</span></span>

1. <span data-ttu-id="292aa-245">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="292aa-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="292aa-247">Välj i listan med program **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="292aa-247">In the applications list, select **Canvas**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. <span data-ttu-id="292aa-249">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="292aa-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="292aa-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="292aa-251">Click **Add** button.</span></span> <span data-ttu-id="292aa-252">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="292aa-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="292aa-254">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="292aa-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="292aa-255">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="292aa-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="292aa-256">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="292aa-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="292aa-257">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="292aa-257">Testing single sign-on</span></span>

<span data-ttu-id="292aa-258">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="292aa-258">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="292aa-259">När du klickar på arbetsytan panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt program i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="292aa-259">When you click the Canvas tile in the Access Panel, you should get automatically signed-on to your Canvas application.</span></span>
<span data-ttu-id="292aa-260">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="292aa-260">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="292aa-261">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="292aa-261">Additional resources</span></span>

* [<span data-ttu-id="292aa-262">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="292aa-262">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="292aa-263">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="292aa-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

