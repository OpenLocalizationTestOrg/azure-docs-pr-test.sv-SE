---
title: "Självstudier: Azure Active Directory-integrering med Halosys | Microsoft Docs"
description: "Lär dig hur du använder Halosys med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18c5cd8eb4ca211f8ae2b8dd994c0e8c48625a2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a><span data-ttu-id="3bccc-103">Självstudier: Azure Active Directory-integrering med Halosys</span><span class="sxs-lookup"><span data-stu-id="3bccc-103">Tutorial: Azure Active Directory integration with Halosys</span></span>

<span data-ttu-id="3bccc-104">I kursen får lära du att integrera Halosys med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3bccc-104">In this tutorial, you learn how to integrate Halosys with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3bccc-105">Integrera Halosys med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3bccc-105">Integrating Halosys with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3bccc-106">Du kan styra i Azure AD som har åtkomst till Halosys</span><span class="sxs-lookup"><span data-stu-id="3bccc-106">You can control in Azure AD who has access to Halosys</span></span>
- <span data-ttu-id="3bccc-107">Du kan aktivera användarna att automatiskt hämta loggat in på Halosys (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3bccc-107">You can enable your users to automatically get signed-on to Halosys (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3bccc-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3bccc-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="3bccc-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3bccc-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3bccc-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3bccc-110">Prerequisites</span></span>

<span data-ttu-id="3bccc-111">För att konfigurera Azure AD-integrering med Halosys, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3bccc-111">To configure Azure AD integration with Halosys, you need the following items:</span></span>

- <span data-ttu-id="3bccc-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3bccc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3bccc-113">En Halosys enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="3bccc-113">A Halosys single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="3bccc-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3bccc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="3bccc-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3bccc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3bccc-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3bccc-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="3bccc-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3bccc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="3bccc-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3bccc-118">Scenario description</span></span>
<span data-ttu-id="3bccc-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3bccc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="3bccc-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3bccc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3bccc-121">Att lägga till Halosys från galleriet</span><span class="sxs-lookup"><span data-stu-id="3bccc-121">Adding Halosys from the gallery</span></span>
2. <span data-ttu-id="3bccc-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3bccc-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-halosys-from-the-gallery"></a><span data-ttu-id="3bccc-123">Att lägga till Halosys från galleriet</span><span class="sxs-lookup"><span data-stu-id="3bccc-123">Adding Halosys from the gallery</span></span>
<span data-ttu-id="3bccc-124">Du måste lägga till Halosys från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Halosys i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3bccc-124">To configure the integration of Halosys into Azure AD, you need to add Halosys from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3bccc-125">**Utför följande steg för att lägga till Halosys från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3bccc-125">**To add Halosys from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3bccc-126">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]
2. <span data-ttu-id="3bccc-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="3bccc-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="3bccc-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="3bccc-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Program][2]

4. <span data-ttu-id="3bccc-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="3bccc-131">Click **Add** at the bottom of the page.</span></span>

    ![Program][3]

5. <span data-ttu-id="3bccc-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>

    ![Program][4]

6. <span data-ttu-id="3bccc-135">I sökrutan skriver **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-135">In the search box, type **Halosys**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. <span data-ttu-id="3bccc-137">I resultatfönstret, Välj **Halosys**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3bccc-137">In the results pane, select **Halosys**, and then click **Complete** to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3bccc-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3bccc-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3bccc-140">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Halosys baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3bccc-140">In this section, you configure and test Azure AD single sign-on with Halosys based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3bccc-141">Azure AD måste du känna till användaren i Halosys motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3bccc-141">For single sign-on to work, Azure AD needs to know what the counterpart user in Halosys is to a user in Azure AD.</span></span> <span data-ttu-id="3bccc-142">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Halosys upprättas.</span><span class="sxs-lookup"><span data-stu-id="3bccc-142">In other words, a link relationship between an Azure AD user and the related user in Halosys needs to be established.</span></span>

<span data-ttu-id="3bccc-143">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Halosys.</span><span class="sxs-lookup"><span data-stu-id="3bccc-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Halosys.</span></span>

<span data-ttu-id="3bccc-144">Om du vill konfigurera och testa Azure AD enkel inloggning med Halosys, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3bccc-144">To configure and test Azure AD single sign-on with Halosys, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3bccc-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3bccc-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3bccc-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3bccc-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3bccc-147">**[Skapa en testanvändare Halosys](#creating-a-halosys-test-user)**  – du har en motsvarighet för Britta Simon i Halosys som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="3bccc-147">**[Creating a Halosys test user](#creating-a-halosys-test-user)** - to have a counterpart of Britta Simon in Halosys that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="3bccc-148">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3bccc-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3bccc-149">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3bccc-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3bccc-150">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3bccc-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3bccc-151">I det här avsnittet Aktivera Azure AD enkel inloggning i den klassiska portalen och konfigurera enkel inloggning i ditt Halosys program.</span><span class="sxs-lookup"><span data-stu-id="3bccc-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Halosys application.</span></span>


<span data-ttu-id="3bccc-152">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Halosys:**</span><span class="sxs-lookup"><span data-stu-id="3bccc-152">**To configure Azure AD single sign-on with Halosys, perform the following steps:**</span></span>

1. <span data-ttu-id="3bccc-153">I den klassiska portalen på den **Halosys** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3bccc-153">In the classic portal, on the **Halosys** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
     
    ![Konfigurera enkel inloggning][6] 

2. <span data-ttu-id="3bccc-155">På den **hur vill du att användarna kan logga in på Halosys** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-155">On the **How would you like users to sign on to Halosys** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. <span data-ttu-id="3bccc-157">På den **konfigurera Appinställningar** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3bccc-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    <span data-ttu-id="3bccc-159">a.</span><span class="sxs-lookup"><span data-stu-id="3bccc-159">a.</span></span> <span data-ttu-id="3bccc-160">I den **logga URL** textruta, Skriv URL: en som används av dina användare logga in i tillämpningsprogrammet Halosys med hjälp av följande mönster: `https://<company-name>.Halosys.com/client-api/api`.</span><span class="sxs-lookup"><span data-stu-id="3bccc-160">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Halosys application using the following pattern: `https://<company-name>.Halosys.com/client-api/api`.</span></span>

    <span data-ttu-id="3bccc-161">b.In den **Identifierare** textruta ange Webbadressen i följande mönster: `https://<company-name>.Halosys.com`.</span><span class="sxs-lookup"><span data-stu-id="3bccc-161">b.In the **Identifier URL** textbox, type the URL in the following pattern: `https://<company-name>.Halosys.com`.</span></span>   
         
4. <span data-ttu-id="3bccc-162">På den **Konfigurera enkel inloggning på Halosys** klickar du på **hämtar metadata**, och sedan spara filen på datorn:</span><span class="sxs-lookup"><span data-stu-id="3bccc-162">On the **Configure single sign-on at Halosys** page, click **Download metadata**, and then save the file on your computer:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. <span data-ttu-id="3bccc-164">För att få SSO konfigurerats för ditt program, Kontakta supportteamet för Halosys och ge dem med följande:</span><span class="sxs-lookup"><span data-stu-id="3bccc-164">To get SSO configured for your application, contact Halosys support team and provide them with the following:</span></span>

    <span data-ttu-id="3bccc-165">• Den hämtade **metadatafil**</span><span class="sxs-lookup"><span data-stu-id="3bccc-165">• The downloaded **metadata file**</span></span>
    
    <span data-ttu-id="3bccc-166">• Den **SAML SSO-URL**</span><span class="sxs-lookup"><span data-stu-id="3bccc-166">• The **SAML SSO URL**</span></span>
    

6. <span data-ttu-id="3bccc-167">I den klassiska portalen markerar du konfigurationen för enkel inloggning bekräftelsen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-167">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Azure AD-Single Sign-On][10]

7. <span data-ttu-id="3bccc-169">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-169">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3bccc-171">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3bccc-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="3bccc-172">I det här avsnittet kan du skapa en testanvändare i den klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3bccc-172">In this section, you create a test user in the classic portal called Britta Simon.</span></span>


![Skapa Azure AD-användare][20]

<span data-ttu-id="3bccc-174">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3bccc-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3bccc-175">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-175">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="3bccc-177">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="3bccc-177">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="3bccc-178">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-178">To display the list of users, in the menu on the top, click **Users**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3bccc-180">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-180">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="3bccc-182">På den **berätta om den här användaren** dialogrutan utför följande steg: ![att skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span><span class="sxs-lookup"><span data-stu-id="3bccc-182">On the **Tell us about this user** dialog page, perform the following steps:  ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span></span> 

    <span data-ttu-id="3bccc-183">a.</span><span class="sxs-lookup"><span data-stu-id="3bccc-183">a.</span></span> <span data-ttu-id="3bccc-184">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="3bccc-184">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="3bccc-185">b.</span><span class="sxs-lookup"><span data-stu-id="3bccc-185">b.</span></span> <span data-ttu-id="3bccc-186">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-186">In the User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3bccc-187">c.</span><span class="sxs-lookup"><span data-stu-id="3bccc-187">c.</span></span> <span data-ttu-id="3bccc-188">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-188">Click **Next**.</span></span>

6.  <span data-ttu-id="3bccc-189">På den **användarprofil** dialogrutan utför följande steg: ![att skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span><span class="sxs-lookup"><span data-stu-id="3bccc-189">On the **User Profile** dialog page, perform the following steps: ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span></span> 

    <span data-ttu-id="3bccc-190">a.</span><span class="sxs-lookup"><span data-stu-id="3bccc-190">a.</span></span> <span data-ttu-id="3bccc-191">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-191">In the **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="3bccc-192">b.</span><span class="sxs-lookup"><span data-stu-id="3bccc-192">b.</span></span> <span data-ttu-id="3bccc-193">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-193">In the **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="3bccc-194">c.</span><span class="sxs-lookup"><span data-stu-id="3bccc-194">c.</span></span> <span data-ttu-id="3bccc-195">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-195">In the **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="3bccc-196">d.</span><span class="sxs-lookup"><span data-stu-id="3bccc-196">d.</span></span> <span data-ttu-id="3bccc-197">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-197">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="3bccc-198">e.</span><span class="sxs-lookup"><span data-stu-id="3bccc-198">e.</span></span> <span data-ttu-id="3bccc-199">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-199">Click **Next**.</span></span>

7. <span data-ttu-id="3bccc-200">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-200">On the **Get temporary password** dialog page, click **create**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="3bccc-202">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3bccc-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="3bccc-204">a.</span><span class="sxs-lookup"><span data-stu-id="3bccc-204">a.</span></span> <span data-ttu-id="3bccc-205">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-205">Write down the value of the **New Password**.</span></span>

    <span data-ttu-id="3bccc-206">b.</span><span class="sxs-lookup"><span data-stu-id="3bccc-206">b.</span></span> <span data-ttu-id="3bccc-207">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="3bccc-207">Click **Complete**.</span></span>   



### <a name="creating-a-halosys-test-user"></a><span data-ttu-id="3bccc-208">Skapa en testanvändare Halosys</span><span class="sxs-lookup"><span data-stu-id="3bccc-208">Creating a Halosys test user</span></span>

<span data-ttu-id="3bccc-209">I det här avsnittet skapar du en användare som kallas Britta Simon i Halosys.</span><span class="sxs-lookup"><span data-stu-id="3bccc-209">In this section, you create a user called Britta Simon in Halosys.</span></span> <span data-ttu-id="3bccc-210">Se tillsammans med Halosys supportteamet att lägga till användare i Halosys-plattformen.</span><span class="sxs-lookup"><span data-stu-id="3bccc-210">Please work with Halosys support team to add the users in the Halosys platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3bccc-211">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3bccc-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3bccc-212">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Halosys.</span><span class="sxs-lookup"><span data-stu-id="3bccc-212">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Halosys.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3bccc-214">**Om du vill tilldela Halosys Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3bccc-214">**To assign Britta Simon to Halosys, perform the following steps:**</span></span>

1. <span data-ttu-id="3bccc-215">På den klassiska portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="3bccc-215">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3bccc-217">Välj i listan med program **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-217">In the applications list, select **Halosys**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. <span data-ttu-id="3bccc-219">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-219">In the menu on the top, click **Users**.</span></span>

    ![Tilldela användare][203]

4. <span data-ttu-id="3bccc-221">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-221">In the Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="3bccc-222">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="3bccc-222">In the toolbar on the bottom, click **Assign**.</span></span>

    ![Tilldela användare][205]


### <a name="testing-single-sign-on"></a><span data-ttu-id="3bccc-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3bccc-224">Testing single sign-on</span></span>

<span data-ttu-id="3bccc-225">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3bccc-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3bccc-226">När du klickar på panelen Halosys på åtkomstpanelen du bör få automatiskt loggat in på ditt Halosys program.</span><span class="sxs-lookup"><span data-stu-id="3bccc-226">When you click the Halosys tile in the Access Panel, you should get automatically signed-on to your Halosys application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="3bccc-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3bccc-227">Additional resources</span></span>

* [<span data-ttu-id="3bccc-228">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3bccc-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3bccc-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3bccc-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
