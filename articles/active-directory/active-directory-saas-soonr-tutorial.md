---
title: "Självstudier: Azure Active Directory-integrering med Soonr arbetsplats | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Soonr arbetsplats."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 76946e4af624d70f2202601ee935523ca3db4314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a><span data-ttu-id="ccd23-103">Självstudier: Azure Active Directory-integrering med Soonr arbetsplats</span><span class="sxs-lookup"><span data-stu-id="ccd23-103">Tutorial: Azure Active Directory integration with Soonr Workplace</span></span>

<span data-ttu-id="ccd23-104">Syftet med den här kursen är att visa dig hur du integrerar Soonr arbetsplats med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ccd23-104">The objective of this tutorial is to show you how to integrate Soonr Workplace with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="ccd23-105">Integrera Soonr arbetsplats med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ccd23-105">Integrating Soonr Workplace with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ccd23-106">Du kan styra i Azure AD som har åtkomst till arbetsplats Soonr</span><span class="sxs-lookup"><span data-stu-id="ccd23-106">You can control in Azure AD who has access to Soonr Workplace</span></span>
- <span data-ttu-id="ccd23-107">Du kan aktivera användarna att automatiskt hämta inloggade Soonr arbetsyta (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ccd23-107">You can enable your users to automatically get signed-on to Soonr Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ccd23-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ccd23-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="ccd23-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ccd23-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccd23-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ccd23-110">Prerequisites</span></span>

<span data-ttu-id="ccd23-111">För att konfigurera Azure AD-integrering med Soonr arbetsplats, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ccd23-111">To configure Azure AD integration with Soonr Workplace, you need the following items:</span></span>

- <span data-ttu-id="ccd23-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ccd23-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ccd23-113">En Soonr arbetsplatsen enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ccd23-113">A Soonr Workplace single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="ccd23-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ccd23-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="ccd23-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ccd23-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ccd23-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ccd23-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="ccd23-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ccd23-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="ccd23-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ccd23-118">Scenario description</span></span>
<span data-ttu-id="ccd23-119">Syftet med den här kursen är att du ska testa Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ccd23-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="ccd23-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ccd23-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ccd23-121">Att lägga till Soonr arbetsplats från galleriet</span><span class="sxs-lookup"><span data-stu-id="ccd23-121">Adding Soonr Workplace from the gallery</span></span>
2. <span data-ttu-id="ccd23-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ccd23-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-soonr-workplace-from-the-gallery"></a><span data-ttu-id="ccd23-123">Att lägga till Soonr arbetsplats från galleriet</span><span class="sxs-lookup"><span data-stu-id="ccd23-123">Adding Soonr Workplace from the gallery</span></span>
<span data-ttu-id="ccd23-124">Du måste lägga till Soonr arbetsplats från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Soonr arbetsplats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ccd23-124">To configure the integration of Soonr Workplace into Azure AD, you need to add Soonr Workplace from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ccd23-125">**Utför följande steg för att lägga till Soonr arbetsplats från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ccd23-125">**To add Soonr Workplace from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ccd23-126">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ccd23-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="ccd23-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="ccd23-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="ccd23-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Program][2]

4. <span data-ttu-id="ccd23-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="ccd23-131">Click **Add** at the bottom of the page.</span></span>

    ![Program][3]

5. <span data-ttu-id="ccd23-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
 
    ![Program][4]

6. <span data-ttu-id="ccd23-135">I sökrutan skriver **Soonr arbetsplats**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-135">In the search box, type **Soonr Workplace**.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. <span data-ttu-id="ccd23-137">I resultatfönstret, Välj **Soonr arbetsplats**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ccd23-137">In the results pane, select **Soonr Workplace**, and then click **Complete** to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ccd23-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ccd23-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ccd23-140">Syftet med det här avsnittet är och hur du kan konfigurera och testa Azure AD enkel inloggning med Soonr arbetsplats utifrån en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ccd23-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with Soonr Workplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ccd23-141">För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i Soonr arbetsplats till en användare i Azure AD är okänt.</span><span class="sxs-lookup"><span data-stu-id="ccd23-141">For single sign-on to work, Azure AD needs to know what the counterpart user in Soonr Workplace to an user in Azure AD is.</span></span> <span data-ttu-id="ccd23-142">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Soonr arbetsplatsen upprättas.</span><span class="sxs-lookup"><span data-stu-id="ccd23-142">In other words, a link relationship between an Azure AD user and the related user in Soonr Workplace needs to be established.</span></span>  

<span data-ttu-id="ccd23-143">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** Soonr arbetsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ccd23-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Soonr Workplace.</span></span>

<span data-ttu-id="ccd23-144">Om du vill konfigurera och testa Azure AD enkel inloggning med Soonr arbetsplats, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ccd23-144">To configure and test Azure AD single sign-on with Soonr Workplace, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ccd23-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ccd23-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ccd23-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ccd23-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ccd23-147">**[Skapa en testanvändare Soonr arbetsplats](#creating-a-soonr-workplace-test-user)**  – du har en motsvarighet för Britta Simon Soonr arbetsplatsen som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="ccd23-147">**[Creating a Soonr Workplace test user](#creating-a-soonr-workplace-test-user)** - to have a counterpart of Britta Simon in Soonr Workplace that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="ccd23-148">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ccd23-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ccd23-149">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ccd23-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ccd23-150">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ccd23-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ccd23-151">I det här avsnittet Aktivera Azure AD enkel inloggning i den klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Soonr arbetsplats.</span><span class="sxs-lookup"><span data-stu-id="ccd23-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Soonr Workplace application.</span></span>


<span data-ttu-id="ccd23-152">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Soonr arbetsplats:**</span><span class="sxs-lookup"><span data-stu-id="ccd23-152">**To configure Azure AD single sign-on with Soonr Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="ccd23-153">I den klassiska Azure-portalen på den **Soonr arbetsplats** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ccd23-153">In the Azure classic portal, on the **Soonr Workplace** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>

    ![Konfigurera enkel inloggning][6] 

2. <span data-ttu-id="ccd23-155">På den **hur vill du att användarna kan logga in på Soonr arbetsplats** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-155">On the **How would you like users to sign on to Soonr Workplace** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. <span data-ttu-id="ccd23-157">På den **konfigurera Appinställningar** dialogrutan utför följande steg:.</span><span class="sxs-lookup"><span data-stu-id="ccd23-157">On the **Configure App Settings** dialog page, perform the following steps:.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    <span data-ttu-id="ccd23-159">a.</span><span class="sxs-lookup"><span data-stu-id="ccd23-159">a.</span></span> <span data-ttu-id="ccd23-160">I den **logga URL** textruta Skriv en URL med följande mönster: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span><span class="sxs-lookup"><span data-stu-id="ccd23-160">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span></span>

    <span data-ttu-id="ccd23-161">b.</span><span class="sxs-lookup"><span data-stu-id="ccd23-161">b.</span></span> <span data-ttu-id="ccd23-162">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-162">Click **Next**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ccd23-163">Observera att detta inte är det verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="ccd23-163">Please note that this is not the real value.</span></span> <span data-ttu-id="ccd23-164">Du måste uppdatera det här värdet med faktiska logga på URL: en.</span><span class="sxs-lookup"><span data-stu-id="ccd23-164">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="ccd23-165">Kontakta supportteamet för Soonr arbetsplats för att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="ccd23-165">Contact Soonr Workplace support team to get this value.</span></span>

4. <span data-ttu-id="ccd23-166">På den **Konfigurera enkel inloggning på Soonr arbetsplats** klickar du på **hämtar metadata** och spara filen på datorn:</span><span class="sxs-lookup"><span data-stu-id="ccd23-166">On the **Configure single sign-on at Soonr Workplace** page, click **Download metadata** and then save the file on your computer:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. <span data-ttu-id="ccd23-168">För att få SSO konfigurerats för ditt program, Kontakta supportteamet Soonr arbetsplats och ange dem med följande:</span><span class="sxs-lookup"><span data-stu-id="ccd23-168">To get SSO configured for your application, contact your Soonr Workplace support team and provide them with the following:</span></span> 

    <span data-ttu-id="ccd23-169">• Den hämtade **Metadata** fil</span><span class="sxs-lookup"><span data-stu-id="ccd23-169">•  The downloaded **Metadata** file</span></span>

    <span data-ttu-id="ccd23-170">• Den **utfärdar-URL**</span><span class="sxs-lookup"><span data-stu-id="ccd23-170">•  The **Issuer URL**</span></span>

    <span data-ttu-id="ccd23-171">• Den **SAML SSO-URL**</span><span class="sxs-lookup"><span data-stu-id="ccd23-171">•  The **SAML SSO URL**</span></span>

    <span data-ttu-id="ccd23-172">• Den **enkel URL för utloggning**</span><span class="sxs-lookup"><span data-stu-id="ccd23-172">•  The **Single Sign-Out Service URL**</span></span>

    >[!NOTE]
    ><span data-ttu-id="ccd23-173">Det här programmet har ersatts av <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask arbetsplats</a> och du kan se <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">detta</a> självstudiekursen för att konfigurera programmet med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ccd23-173">This application is superseded by <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask Workplace</a> and you can refer <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">this</a> tutorial for configuring the application with Azure AD.</span></span>
   
6. <span data-ttu-id="ccd23-174">Välj konfiguration för enkel inloggning bekräftelse i den klassiska Azure-portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-174">In the Azure classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>

    ![Azure AD-Single Sign-On][10]

7. <span data-ttu-id="ccd23-176">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-176">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
  
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ccd23-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ccd23-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="ccd23-179">Syftet med det här avsnittet är att skapa en testanvändare i den klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ccd23-179">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>  

![Skapa Azure AD-användare][20]

<span data-ttu-id="ccd23-181">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ccd23-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ccd23-182">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-182">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="ccd23-184">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="ccd23-184">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="ccd23-185">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-185">To display the list of users, in the menu on the top, click **Users**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ccd23-187">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-187">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="ccd23-189">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ccd23-189">On the **Tell us about this user** dialog page, perform the following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    <span data-ttu-id="ccd23-191">a.</span><span class="sxs-lookup"><span data-stu-id="ccd23-191">a.</span></span> <span data-ttu-id="ccd23-192">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="ccd23-192">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="ccd23-193">b.</span><span class="sxs-lookup"><span data-stu-id="ccd23-193">b.</span></span> <span data-ttu-id="ccd23-194">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-194">In the User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ccd23-195">c.</span><span class="sxs-lookup"><span data-stu-id="ccd23-195">c.</span></span> <span data-ttu-id="ccd23-196">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-196">Click **Next**.</span></span>

6.  <span data-ttu-id="ccd23-197">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ccd23-197">On the **User Profile** dialog page, perform the following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    <span data-ttu-id="ccd23-199">a.</span><span class="sxs-lookup"><span data-stu-id="ccd23-199">a.</span></span> <span data-ttu-id="ccd23-200">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-200">In the **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="ccd23-201">b.</span><span class="sxs-lookup"><span data-stu-id="ccd23-201">b.</span></span> <span data-ttu-id="ccd23-202">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-202">In the **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="ccd23-203">c.</span><span class="sxs-lookup"><span data-stu-id="ccd23-203">c.</span></span> <span data-ttu-id="ccd23-204">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-204">In the **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="ccd23-205">d.</span><span class="sxs-lookup"><span data-stu-id="ccd23-205">d.</span></span> <span data-ttu-id="ccd23-206">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-206">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="ccd23-207">e.</span><span class="sxs-lookup"><span data-stu-id="ccd23-207">e.</span></span> <span data-ttu-id="ccd23-208">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-208">Click **Next**.</span></span>

7. <span data-ttu-id="ccd23-209">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-209">On the **Get temporary password** dialog page, click **create**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="ccd23-211">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ccd23-211">On the **Get temporary password** dialog page, perform the following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="ccd23-213">a.</span><span class="sxs-lookup"><span data-stu-id="ccd23-213">a.</span></span> <span data-ttu-id="ccd23-214">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-214">Write down the value of the **New Password**.</span></span>

    <span data-ttu-id="ccd23-215">b.</span><span class="sxs-lookup"><span data-stu-id="ccd23-215">b.</span></span> <span data-ttu-id="ccd23-216">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="ccd23-216">Click **Complete**.</span></span>   



### <a name="creating-a-soonr-workplace-test-user"></a><span data-ttu-id="ccd23-217">Skapa en testanvändare Soonr arbetsplats</span><span class="sxs-lookup"><span data-stu-id="ccd23-217">Creating a Soonr Workplace test user</span></span>

<span data-ttu-id="ccd23-218">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon Soonr arbetsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ccd23-218">The objective of this section is to create a user called Britta Simon in Soonr Workplace.</span></span> <span data-ttu-id="ccd23-219">Se tillsammans med Soonr arbetsplats supportteamet för att skapa en användare i plattformen.</span><span class="sxs-lookup"><span data-stu-id="ccd23-219">Please work with Soonr Workplace support team to create a user in the platform.</span></span> <span data-ttu-id="ccd23-220">Du kan höja supportärende med Soonr från <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">här</a>.</span><span class="sxs-lookup"><span data-stu-id="ccd23-220">You can raise the support ticket with Soonr from <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">here</a>.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ccd23-221">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ccd23-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ccd23-222">Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till arbetsplats Soonr.</span><span class="sxs-lookup"><span data-stu-id="ccd23-222">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to Soonr Workplace.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ccd23-224">**Om du vill tilldela Soonr arbetsplats Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ccd23-224">**To assign Britta Simon to Soonr Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="ccd23-225">På den klassiska Azure-portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="ccd23-225">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ccd23-227">Välj i listan med program **Soonr arbetsplats**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-227">In the applications list, select **Soonr Workplace**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. <span data-ttu-id="ccd23-229">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-229">In the menu on the top, click **Users**.</span></span>

    ![Tilldela användare][203] 

1. <span data-ttu-id="ccd23-231">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-231">In the Users list, select **Britta Simon**.</span></span>

2. <span data-ttu-id="ccd23-232">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="ccd23-232">In the toolbar on the bottom, click **Assign**.</span></span>

    ![Tilldela användare][205]



### <a name="testing-single-sign-on"></a><span data-ttu-id="ccd23-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ccd23-234">Testing single sign-on</span></span>

<span data-ttu-id="ccd23-235">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ccd23-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="ccd23-236">När du klickar på panelen Soonr arbetsplats på åtkomstpanelen du ska hämta automatiskt loggat in på ditt Soonr arbetsplats program.</span><span class="sxs-lookup"><span data-stu-id="ccd23-236">When you click the Soonr Workplace tile in the Access Panel, you should get automatically signed-on to your Soonr Workplace application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="ccd23-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ccd23-237">Additional resources</span></span>

* [<span data-ttu-id="ccd23-238">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ccd23-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ccd23-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ccd23-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
