---
title: "Självstudier: Azure Active Directory-integrering med Splunk Enterprise och Splunk molntjänster | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Splunk Enterprise och Splunk moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: b78e9b7161207a74880e912241d5e965b353d1c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a><span data-ttu-id="e0516-103">Självstudier: Azure Active Directory-integrering med Splunk Enterprise och Splunk moln</span><span class="sxs-lookup"><span data-stu-id="e0516-103">Tutorial: Azure Active Directory integration with Splunk Enterprise and Splunk Cloud</span></span>

<span data-ttu-id="e0516-104">I kursen får lära du att integrera Splunk Enterprise och Splunk moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e0516-104">In this tutorial, you learn how to integrate Splunk Enterprise and Splunk Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e0516-105">Integrera Splunk Enterprise och Splunk moln med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e0516-105">Integrating Splunk Enterprise and Splunk Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e0516-106">Du kan styra i Azure AD som har åtkomst till Splunk Enterprise och Splunk moln</span><span class="sxs-lookup"><span data-stu-id="e0516-106">You can control in Azure AD who has access to Splunk Enterprise and Splunk Cloud</span></span>
- <span data-ttu-id="e0516-107">Du kan aktivera användarna att automatiskt hämta loggat in på Splunk Enterprise och Splunk moln enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e0516-107">You can enable your users to automatically get signed-on to Splunk Enterprise and Splunk Cloud single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="e0516-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e0516-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="e0516-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e0516-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0516-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e0516-110">Prerequisites</span></span>

<span data-ttu-id="e0516-111">För att konfigurera Azure AD-integrering med Splunk Enterprise och Splunk molnet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e0516-111">To configure Azure AD integration with Splunk Enterprise and Splunk Cloud, you need the following items:</span></span>

- <span data-ttu-id="e0516-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e0516-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e0516-113">En Splunk Enterprise eller Splunk moln SSO aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e0516-113">A Splunk Enterprise or Splunk Cloud SSO enabled subscription</span></span>


>[!NOTE]
><span data-ttu-id="e0516-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e0516-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="e0516-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e0516-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e0516-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e0516-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="e0516-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e0516-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="e0516-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e0516-118">Scenario description</span></span>
<span data-ttu-id="e0516-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e0516-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="e0516-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e0516-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e0516-121">Att lägga till Splunk Enterprise och Splunk moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="e0516-121">Adding Splunk Enterprise and Splunk Cloud from the gallery</span></span>
2. <span data-ttu-id="e0516-122">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="e0516-122">Configuring and testing Azure AD SSO</span></span>


## <a name="add-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a><span data-ttu-id="e0516-123">Lägg till Splunk Enterprise och Splunk moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="e0516-123">Add Splunk Enterprise and Splunk Cloud from the gallery</span></span>
<span data-ttu-id="e0516-124">Du måste lägga till Splunk Enterprise och Splunk moln från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Splunk Enterprise och Splunk moln i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0516-124">To configure the integration of Splunk Enterprise and Splunk Cloud into Azure AD, you need to add Splunk Enterprise and Splunk Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e0516-125">**Utför följande steg för att lägga till Splunk Enterprise och Splunk moln från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e0516-125">**To add Splunk Enterprise and Splunk Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e0516-126">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e0516-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]

2. <span data-ttu-id="e0516-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="e0516-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="e0516-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="e0516-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Program][2]

4. <span data-ttu-id="e0516-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="e0516-131">Click **Add** at the bottom of the page.</span></span>

    ![Program][3]

5. <span data-ttu-id="e0516-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="e0516-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>

    ![Program][4]

6. <span data-ttu-id="e0516-135">I sökrutan skriver **Splunk Enterprise eller Splunk moln**.</span><span class="sxs-lookup"><span data-stu-id="e0516-135">In the search box, type **Splunk Enterprise or Splunk Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. <span data-ttu-id="e0516-137">I resultatfönstret, Välj **Splunk Enterprise och Splunk moln**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e0516-137">In the results pane, select **Splunk Enterprise and Splunk Cloud**, and then click **Complete** to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e0516-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0516-139">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="e0516-140">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Splunk Enterprise och Splunk molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e0516-140">In this section, you configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e0516-141">Azure AD måste du känna till motsvarande användaren i Splunk Enterprise och Splunk molnet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e0516-141">For single sign-on to work, Azure AD needs to know what the counterpart user in Splunk Enterprise and Splunk Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="e0516-142">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Splunk Enterprise och Splunk molnet upprättas.</span><span class="sxs-lookup"><span data-stu-id="e0516-142">In other words, a link relationship between an Azure AD user and the related user in Splunk Enterprise and Splunk Cloud needs to be established.</span></span>

<span data-ttu-id="e0516-143">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Splunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="e0516-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Splunk Enterprise and Splunk Cloud.</span></span>

<span data-ttu-id="e0516-144">Om du vill konfigurera och testa Azure AD enkel inloggning med Splunk Enterprise och Splunk moln, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e0516-144">To configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e0516-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e0516-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e0516-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0516-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e0516-147">**[Skapa en testanvändare Splunk Enterprise och Splunk moln](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  – du har en motsvarighet för Britta Simon i Splunk företag och Splunk moln som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="e0516-147">**[Creating a Splunk Enterprise and Splunk Cloud test user](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)** - to have a counterpart of Britta Simon in Splunk Enterprise and Splunk Cloud that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="e0516-148">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e0516-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e0516-149">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e0516-149">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e0516-150">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0516-150">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e0516-151">I det här avsnittet Aktivera Azure AD enkel inloggning i den klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Splunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="e0516-151">In this section, you enable Azure AD SSO in the classic portal and configure SSO in your Splunk Enterprise and Splunk Cloud application.</span></span>


<span data-ttu-id="e0516-152">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Splunk Enterprise och Splunk molnet:**</span><span class="sxs-lookup"><span data-stu-id="e0516-152">**To configure Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="e0516-153">I den klassiska portalen på den **Splunk Enterprise och Splunk moln** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0516-153">In the classic portal, on the **Splunk Enterprise and Splunk Cloud** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
     
    ![Konfigurera enkel inloggning][6] 

2. <span data-ttu-id="e0516-155">På den **hur vill du att användarna kan logga in på Splunk Enterprise och Splunk moln** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e0516-155">On the **How would you like users to sign on to Splunk Enterprise and Splunk Cloud** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. <span data-ttu-id="e0516-157">På den **konfigurera Appinställningar** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0516-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. <span data-ttu-id="e0516-159">I den **logga URL** textruta, Skriv URL: en som används av dina användare logga in i tillämpningsprogrammet Splunk Enterprise och Splunk moln med hjälp av följande mönster:`https://<splunkserverUrl>/en-US/app/launcher/home`</span><span class="sxs-lookup"><span data-stu-id="e0516-159">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Splunk Enterprise and Splunk Cloud application using the following pattern: `https://<splunkserverUrl>/en-US/app/launcher/home`</span></span>
  2. <span data-ttu-id="e0516-160">I den **identifierare** textruta ange Webbadressen till Splunk-Server.</span><span class="sxs-lookup"><span data-stu-id="e0516-160">In the **Identifier** textbox, type the URL of your Splunk Server.</span></span>
  3. <span data-ttu-id="e0516-161">I den **Reply URL** textruta anger du URL med följande mönster:`https://<splunkserver>/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="e0516-161">In the **Reply URL** textbox, type the URL with the following pattern: `https://<splunkserver>/saml/acs`</span></span>
  4. <span data-ttu-id="e0516-162">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e0516-162">Click **Next**.</span></span>
 
4. <span data-ttu-id="e0516-163">På den **Konfigurera enkel inloggning på Splunk Enterprise och Splunk moln** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0516-163">On the **Configure single sign-on at Splunk Enterprise and Splunk Cloud** page, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. <span data-ttu-id="e0516-165">Klicka på **hämtar metadata**, och sedan spara filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e0516-165">Click **Download metadata**, and then save the file on your computer.</span></span>
  2. <span data-ttu-id="e0516-166">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e0516-166">Click **Next**.</span></span>

5. <span data-ttu-id="e0516-167">Kontakta Splunk Enterprise och Splunk moln supportteamet för att få SSO konfigurerats för ditt program, och ge dem med följande:</span><span class="sxs-lookup"><span data-stu-id="e0516-167">To get SSO configured for your application, contact Splunk Enterprise and Splunk Cloud support team and provide them with the following:</span></span>

    * <span data-ttu-id="e0516-168">Den hämtade **federaton metadata**</span><span class="sxs-lookup"><span data-stu-id="e0516-168">The downloaded **federaton metadata**</span></span>
6. <span data-ttu-id="e0516-169">I den klassiska portalen markerar du konfigurationen för enkel inloggning bekräftelsen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e0516-169">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Azure AD-Single Sign-On][10]

7. <span data-ttu-id="e0516-171">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="e0516-171">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e0516-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0516-173">Create an Azure AD test user</span></span>
<span data-ttu-id="e0516-174">I det här avsnittet kan du skapa en testanvändare i den klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0516-174">In this section, you create a test user in the classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="e0516-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e0516-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e0516-177">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e0516-177">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="e0516-179">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="e0516-179">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="e0516-180">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="e0516-180">To display the list of users, in the menu on the top, click **Users**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e0516-182">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="e0516-182">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="e0516-184">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0516-184">On the **Tell us about this user** dialog page, perform the following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="e0516-186">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="e0516-186">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="e0516-187">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e0516-187">In the User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="e0516-188">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e0516-188">Click **Next**.</span></span>

6.  <span data-ttu-id="e0516-189">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0516-189">On the **User Profile** dialog page, perform the following steps:</span></span>
  
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. <span data-ttu-id="e0516-191">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="e0516-191">In the **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="e0516-192">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="e0516-192">In the **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="e0516-193">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e0516-193">In the **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="e0516-194">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="e0516-194">In the **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="e0516-195">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e0516-195">Click **Next**.</span></span>

7. <span data-ttu-id="e0516-196">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e0516-196">On the **Get temporary password** dialog page, click **create**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="e0516-198">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0516-198">On the **Get temporary password** dialog page, perform the following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. <span data-ttu-id="e0516-200">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e0516-200">Write down the value of the **New Password**.</span></span>
  2. <span data-ttu-id="e0516-201">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="e0516-201">Click **Complete**.</span></span>   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a><span data-ttu-id="e0516-202">Skapa en Splunk Enterprise och Splunk moln testanvändare</span><span class="sxs-lookup"><span data-stu-id="e0516-202">Create a Splunk Enterprise and Splunk Cloud test user</span></span>

<span data-ttu-id="e0516-203">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Splunk företag och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="e0516-203">In this section, you create a user called Britta Simon in Splunk Enterprise and Splunk Cloud.</span></span> <span data-ttu-id="e0516-204">Se tillsammans med Splunk Enterprise och Splunk moln supportteamet för att lägga till användare i Splunk Enterprise och Splunk Cloud platform.</span><span class="sxs-lookup"><span data-stu-id="e0516-204">Please work with Splunk Enterprise and Splunk Cloud support team to add the users in the Splunk Enterprise and Splunk Cloud platform.</span></span>


### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="e0516-205">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e0516-205">Assign the Azure AD test user</span></span>

<span data-ttu-id="e0516-206">I det här avsnittet kan du aktivera Britta Simon att använda Azure SSOy bevilja sin åtkomst till Splunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="e0516-206">In this section, you enable Britta Simon to use Azure SSOy granting her access to Splunk Enterprise and Splunk Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e0516-208">**Utför följande steg om du vill tilldela Britta Simon Splunk företag och Splunk molnet:**</span><span class="sxs-lookup"><span data-stu-id="e0516-208">**To assign Britta Simon to Splunk Enterprise and Splunk Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="e0516-209">På den klassiska portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="e0516-209">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e0516-211">Välj i listan med program **Splunk Enterprise och Splunk moln**.</span><span class="sxs-lookup"><span data-stu-id="e0516-211">In the applications list, select **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. <span data-ttu-id="e0516-213">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="e0516-213">In the menu on the top, click **Users**.</span></span>

    ![Tilldela användare][203]

4. <span data-ttu-id="e0516-215">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e0516-215">In the Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="e0516-216">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="e0516-216">In the toolbar on the bottom, click **Assign**.</span></span>

    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="e0516-218">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0516-218">Test single sign-on</span></span>

<span data-ttu-id="e0516-219">I det här avsnittet kan du testa din Azure AD-SSOonfiguration med åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e0516-219">In this section, you test your Azure AD SSOonfiguration using the Access Panel.</span></span>

<span data-ttu-id="e0516-220">När du klickar på företaget Splunk Splunk moln panelen i åtkomstpanelen, du ska hämta automatiskt loggat in i tillämpningsprogrammet Splunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="e0516-220">When you click the Splunk Enterprise and Splunk Cloud tile in the Access Panel, you should get automatically signed-on to your Splunk Enterprise and Splunk Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="e0516-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e0516-221">Additional resources</span></span>

* [<span data-ttu-id="e0516-222">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e0516-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e0516-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e0516-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
