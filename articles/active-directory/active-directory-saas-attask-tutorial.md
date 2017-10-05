---
title: "Självstudier: Azure Active Directory-integrering med @Task| Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: ebb19ca6cbaf04106fbce937d95651e709854cfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a><span data-ttu-id="6289c-103">Självstudier: Azure Active Directory-integrering med@Task</span><span class="sxs-lookup"><span data-stu-id="6289c-103">Tutorial: Azure Active Directory integration with @Task</span></span>
<span data-ttu-id="6289c-104">Syftet med den här kursen är att visa dig hur du integrerar @Task med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6289c-104">The objective of this tutorial is to show you how to integrate @Task with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="6289c-105">Integrera @Task med Azure AD ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6289c-105">Integrating @Task with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="6289c-106">Du kan styra i Azure AD som har åtkomst till@Task</span><span class="sxs-lookup"><span data-stu-id="6289c-106">You can control in Azure AD who has access to @Task</span></span>
* <span data-ttu-id="6289c-107">Du kan ge användarna automatiskt får loggat in på @Task (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6289c-107">You can enable your users to automatically get signed-on to @Task (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="6289c-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6289c-108">You can manage your accounts in one central location - the Azure classic Portal</span></span>

<span data-ttu-id="6289c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6289c-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6289c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6289c-110">Prerequisites</span></span>
<span data-ttu-id="6289c-111">Konfigurera Azure AD-integrering med @Task, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="6289c-111">To configure Azure AD integration with @Task, you need the following items:</span></span>

* <span data-ttu-id="6289c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6289c-112">An Azure AD subscription</span></span>
* <span data-ttu-id="6289c-113">En @Task enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6289c-113">An @Task single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6289c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6289c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="6289c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6289c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="6289c-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6289c-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="6289c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6289c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="6289c-118">Scenario-beskrivning</span><span class="sxs-lookup"><span data-stu-id="6289c-118">Scenario Description</span></span>
<span data-ttu-id="6289c-119">Syftet med den här kursen är att du ska testa Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6289c-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="6289c-120">Det scenario som beskrivs i den här kursen består av tre huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6289c-120">The scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="6289c-121">Lägga till @Task från galleriet</span><span class="sxs-lookup"><span data-stu-id="6289c-121">Adding @Task from the gallery</span></span> 
2. <span data-ttu-id="6289c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6289c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-task-from-the-gallery"></a><span data-ttu-id="6289c-123">Lägga till @Task från galleriet</span><span class="sxs-lookup"><span data-stu-id="6289c-123">Adding @Task from the gallery</span></span>
<span data-ttu-id="6289c-124">Så här konfigurerar du integrering av @Task till Azure AD, måste du lägga till @Task från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6289c-124">To configure the integration of @Task into Azure AD, you need to add @Task from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6289c-125">**Att lägga till @Task från galleriet utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6289c-125">**To add @Task from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6289c-126">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6289c-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1] 
2. <span data-ttu-id="6289c-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="6289c-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="6289c-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="6289c-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Program][2] 
4. <span data-ttu-id="6289c-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="6289c-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Program][3] 
5. <span data-ttu-id="6289c-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="6289c-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Program][4] 
6. <span data-ttu-id="6289c-135">I sökrutan skriver  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="6289c-135">In the search box, type **@Task**.</span></span>
   
    ![Program][5] 
7. <span data-ttu-id="6289c-137">I resultatfönstret, Välj  **@Task** , och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="6289c-137">In the results pane, select **@Task**, and then click **Complete** to add the application.</span></span>
   
    ![Program][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6289c-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6289c-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6289c-140">Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Azure AD enkel inloggning med @Task baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6289c-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with @Task based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6289c-141">För enkel inloggning ska fungera, Azure AD måste veta vilka motsvarande användaren i @Task till en användare i Azure AD är.</span><span class="sxs-lookup"><span data-stu-id="6289c-141">For single sign-on to work, Azure AD needs to know what the counterpart user in @Task to an user in Azure AD is.</span></span> <span data-ttu-id="6289c-142">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i @Task måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="6289c-142">In other words, a link relationship between an Azure AD user and the related user in @Task needs to be established.</span></span>   
<span data-ttu-id="6289c-143">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i @Task.</span><span class="sxs-lookup"><span data-stu-id="6289c-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in @Task.</span></span>

<span data-ttu-id="6289c-144">Konfigurera och testa Azure AD enkel inloggning med @Task, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6289c-144">To configure and test Azure AD single sign-on with @Task, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6289c-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6289c-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6289c-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6289c-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6289c-147">**[Skapa en @Tasktest användare](#creating-a-halogen-software-test-user)**  – du har en motsvarighet för Britta Simon i @Taskthat är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="6289c-147">**[Creating a @Tasktest user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in @Taskthat is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="6289c-148">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6289c-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6289c-149">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6289c-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6289c-150">Konfigurera Azure AD-Single Sign-On</span><span class="sxs-lookup"><span data-stu-id="6289c-150">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="6289c-151">Syftet med det här avsnittet är att aktivera Azure AD enkel inloggning i den klassiska Azure-portalen och konfigurera enkel inloggning i din @Task program.</span><span class="sxs-lookup"><span data-stu-id="6289c-151">The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your @Task application.</span></span>

<span data-ttu-id="6289c-152">**Konfigurera Azure AD enkel inloggning med @Task, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6289c-152">**To configure Azure AD single sign-on with @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="6289c-153">I den klassiska Azure-portalen på den  **@Task**  integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6289c-153">In the Azure classic portal, on the **@Task** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][6] 
2. <span data-ttu-id="6289c-155">På den **hur vill du användarna att logga in på @Task**  väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6289c-155">On the **How would you like users to sign on to @Task** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][7] 
3. <span data-ttu-id="6289c-157">På den **konfigurera Appinställningar** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6289c-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span>
   
    ![Konfigurera Appinställningar för][8] 
   
     <span data-ttu-id="6289c-159">a.</span><span class="sxs-lookup"><span data-stu-id="6289c-159">a.</span></span> <span data-ttu-id="6289c-160">I den **logga URL** textruta, Skriv URL: en som används av dina användare logga in till din @Task program (t.ex.:*https://<Tenant name>.attask ondemand.com*).</span><span class="sxs-lookup"><span data-stu-id="6289c-160">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your @Task application (e.g.:*https://<Tenant name>.attask-ondemand.com*).</span></span>
   
     <span data-ttu-id="6289c-161">b.</span><span class="sxs-lookup"><span data-stu-id="6289c-161">b.</span></span> <span data-ttu-id="6289c-162">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="6289c-162">Click **Next**.</span></span>
4. <span data-ttu-id="6289c-163">På den **Konfigurera enkel inloggning vid @Task**  klickar du på **hämtar metadata**, spara metadatafilen lokalt på datorn och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6289c-163">On the **Configure single sign-on at @Task** page, click **Download metadata**, save the metadata file locally on your computer, and then click **Next**.</span></span>
   
    ![Vad är Azure AD Connect?][9] 
5. <span data-ttu-id="6289c-165">Logga in på ditt @Task företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="6289c-165">Sign-on to your @Task company site as administrator.</span></span>
6. <span data-ttu-id="6289c-166">Gå till **enkel inloggning på konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="6289c-166">Go to **Single Sign On Configuration**.</span></span>
7. <span data-ttu-id="6289c-167">På den **enkel inloggning** dialogrutan, utför följande steg</span><span class="sxs-lookup"><span data-stu-id="6289c-167">On the **Single Sign-On** dialog, perform the following steps</span></span>
   
    ![Konfigurera enkel inloggning][23]
   
    <span data-ttu-id="6289c-169">a.</span><span class="sxs-lookup"><span data-stu-id="6289c-169">a.</span></span> <span data-ttu-id="6289c-170">Som **typen**väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="6289c-170">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="6289c-171">b.</span><span class="sxs-lookup"><span data-stu-id="6289c-171">b.</span></span> <span data-ttu-id="6289c-172">Välj **Service Provider-ID**.</span><span class="sxs-lookup"><span data-stu-id="6289c-172">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="6289c-173">c.</span><span class="sxs-lookup"><span data-stu-id="6289c-173">c.</span></span> <span data-ttu-id="6289c-174">På den klassiska Azure-portalen, kopiera den **Remote inloggnings-URL**, och klistrar in det i den **Portal Inloggningswebbadressen** textruta.</span><span class="sxs-lookup"><span data-stu-id="6289c-174">On the Azure classic portal, copy the **Remote Login URL**, and then paste it into the **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="6289c-175">d.</span><span class="sxs-lookup"><span data-stu-id="6289c-175">d.</span></span> <span data-ttu-id="6289c-176">På den klassiska Azure-portalen, kopiera den **tjänst-URL för enkel Sign-Out**, och klistrar in det i den **Sign-Out URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="6289c-176">On the Azure classic portal, copy the **Single Sign-Out Service URL**, and then paste it into the **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="6289c-177">e.</span><span class="sxs-lookup"><span data-stu-id="6289c-177">e.</span></span> <span data-ttu-id="6289c-178">På den klassiska Azure-portalen, kopiera den **ändra lösenord URL**, och klistrar in det i den **ändra lösenord URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="6289c-178">On the Azure classic portal, copy the **Change Password URL**, and then paste it into the **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="6289c-179">f.</span><span class="sxs-lookup"><span data-stu-id="6289c-179">f.</span></span> <span data-ttu-id="6289c-180">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6289c-180">Click **Save**.</span></span>
8. <span data-ttu-id="6289c-181">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6289c-181">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Vad är Azure AD Connect?][10]
9. <span data-ttu-id="6289c-183">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="6289c-183">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Vad är Azure AD Connect?][11]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6289c-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6289c-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="6289c-186">Syftet med det här avsnittet är att skapa en testanvändare i den klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6289c-186">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>  

![Skapa Azure AD-användare][20]

<span data-ttu-id="6289c-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="6289c-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6289c-189">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6289c-189">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. <span data-ttu-id="6289c-191">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="6289c-191">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="6289c-192">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="6289c-192">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. <span data-ttu-id="6289c-194">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="6289c-194">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. <span data-ttu-id="6289c-196">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6289c-196">On the **Tell us about this user** dialog page, perform the following steps:</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="6289c-198">a.</span><span class="sxs-lookup"><span data-stu-id="6289c-198">a.</span></span> <span data-ttu-id="6289c-199">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="6289c-199">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="6289c-200">b.</span><span class="sxs-lookup"><span data-stu-id="6289c-200">b.</span></span> <span data-ttu-id="6289c-201">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6289c-201">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="6289c-202">c.</span><span class="sxs-lookup"><span data-stu-id="6289c-202">c.</span></span> <span data-ttu-id="6289c-203">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="6289c-203">Click **Next**.</span></span>
6. <span data-ttu-id="6289c-204">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6289c-204">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    <span data-ttu-id="6289c-206">a.</span><span class="sxs-lookup"><span data-stu-id="6289c-206">a.</span></span> <span data-ttu-id="6289c-207">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="6289c-207">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="6289c-208">b.</span><span class="sxs-lookup"><span data-stu-id="6289c-208">b.</span></span> <span data-ttu-id="6289c-209">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="6289c-209">In the **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="6289c-210">c.</span><span class="sxs-lookup"><span data-stu-id="6289c-210">c.</span></span> <span data-ttu-id="6289c-211">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="6289c-211">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="6289c-212">d.</span><span class="sxs-lookup"><span data-stu-id="6289c-212">d.</span></span> <span data-ttu-id="6289c-213">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="6289c-213">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="6289c-214">e.</span><span class="sxs-lookup"><span data-stu-id="6289c-214">e.</span></span> <span data-ttu-id="6289c-215">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="6289c-215">Click **Next**.</span></span>

7. <span data-ttu-id="6289c-216">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6289c-216">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. <span data-ttu-id="6289c-218">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6289c-218">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="6289c-220">a.</span><span class="sxs-lookup"><span data-stu-id="6289c-220">a.</span></span> <span data-ttu-id="6289c-221">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6289c-221">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="6289c-222">b.</span><span class="sxs-lookup"><span data-stu-id="6289c-222">b.</span></span> <span data-ttu-id="6289c-223">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="6289c-223">Click **Complete**.</span></span>   

### <a name="creating-an-task-test-user"></a><span data-ttu-id="6289c-224">Skapa en @Task testanvändare</span><span class="sxs-lookup"><span data-stu-id="6289c-224">Creating an @Task test user</span></span>
<span data-ttu-id="6289c-225">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i @Task.</span><span class="sxs-lookup"><span data-stu-id="6289c-225">The objective of this section is to create a user called Britta Simon in @Task.</span></span>

<span data-ttu-id="6289c-226">**Så här skapar du en användare som kallas Britta Simon i @Task, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6289c-226">**To create a user called Britta Simon in @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="6289c-227">Logga in på ditt @Task företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="6289c-227">Sign on to your @Task company site as administrator.</span></span>
2. <span data-ttu-id="6289c-228">Klicka på menyn högst upp **personer**.</span><span class="sxs-lookup"><span data-stu-id="6289c-228">In the menu on the top, click **People**.</span></span>
3. <span data-ttu-id="6289c-229">Klicka på **ny Person**.</span><span class="sxs-lookup"><span data-stu-id="6289c-229">Click **New Person**.</span></span> 
4. <span data-ttu-id="6289c-230">I dialogrutan Ny Person att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="6289c-230">On the New Person dialog, perform the following steps:</span></span>
   
    ![Skapa en @Task testanvändare][21] 
   
    <span data-ttu-id="6289c-232">a.</span><span class="sxs-lookup"><span data-stu-id="6289c-232">a.</span></span> <span data-ttu-id="6289c-233">I den **Förnamn** textruta skriver ”Britta”.</span><span class="sxs-lookup"><span data-stu-id="6289c-233">In the **First Name** textbox, type "Britta".</span></span>
   
    <span data-ttu-id="6289c-234">b.</span><span class="sxs-lookup"><span data-stu-id="6289c-234">b.</span></span> <span data-ttu-id="6289c-235">I den **efternamn** textruta skriver ”Simon”.</span><span class="sxs-lookup"><span data-stu-id="6289c-235">In the **Last Name** textbox, type "Simon".</span></span>
   
    <span data-ttu-id="6289c-236">c.</span><span class="sxs-lookup"><span data-stu-id="6289c-236">c.</span></span> <span data-ttu-id="6289c-237">I den **e-postadress** textruta Skriv Britta Simon e-postadress i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6289c-237">In the **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="6289c-238">d.</span><span class="sxs-lookup"><span data-stu-id="6289c-238">d.</span></span> <span data-ttu-id="6289c-239">Klicka på **lägga till personen**.</span><span class="sxs-lookup"><span data-stu-id="6289c-239">Click **Add Person**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6289c-240">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="6289c-240">Assigning the Azure AD test user</span></span>
<span data-ttu-id="6289c-241">Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure enkel inloggning med sin åtkomst beviljas till @Task.</span><span class="sxs-lookup"><span data-stu-id="6289c-241">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to @Task.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6289c-243">**Tilldela Britta Simon till @Task, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6289c-243">**To assign Britta Simon to @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="6289c-244">På den klassiska Azure-portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="6289c-244">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Tilldela användare][201] 
2. <span data-ttu-id="6289c-246">Välj i listan med program  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="6289c-246">In the applications list, select **@Task**.</span></span>
   
    ![Tilldela användare][202] 
3. <span data-ttu-id="6289c-248">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="6289c-248">In the menu on the top, click **Users**.</span></span>
   
    ![Tilldela användare][203] 
4. <span data-ttu-id="6289c-250">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="6289c-250">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="6289c-251">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="6289c-251">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="6289c-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6289c-253">Testing Single Sign-On</span></span>
<span data-ttu-id="6289c-254">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6289c-254">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="6289c-255">När du klickar på den @Task panelen på panelen åtkomst som du bör få automatiskt loggat in på ditt @Task program.</span><span class="sxs-lookup"><span data-stu-id="6289c-255">When you click the @Task tile in the Access Panel, you should get automatically signed-on to your @Task application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6289c-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6289c-256">Additional Resources</span></span>
* [<span data-ttu-id="6289c-257">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6289c-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6289c-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6289c-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






