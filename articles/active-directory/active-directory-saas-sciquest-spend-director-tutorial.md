---
title: "Självstudier: Azure Active Directory-integrering med SciQuest tillbringar Director | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SciQuest tillbringar Director."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 84b707668dc45e92e6151f422f1c919f638533b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a><span data-ttu-id="c5820-103">Självstudier: Azure Active Directory-integrering med SciQuest tillbringar Director</span><span class="sxs-lookup"><span data-stu-id="c5820-103">Tutorial: Azure Active Directory integration with SciQuest Spend Director</span></span>
<span data-ttu-id="c5820-104">Syftet med den här kursen är att visa dig hur du integrerar SciQuest tillbringar Director med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c5820-104">The objective of this tutorial is to show you how to integrate SciQuest Spend Director with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="c5820-105">Integrera SciQuest tillbringar Director med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c5820-105">Integrating SciQuest Spend Director with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="c5820-106">Du kan styra i Azure AD som har åtkomst till SciQuest tillbringar Director</span><span class="sxs-lookup"><span data-stu-id="c5820-106">You can control in Azure AD who has access to SciQuest Spend Director</span></span> 
* <span data-ttu-id="c5820-107">Du kan aktivera användarna att automatiskt hämta loggat in på SciQuest tillbringar Director (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c5820-107">You can enable your users to automatically get signed-on to SciQuest Spend Director (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="c5820-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c5820-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="c5820-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c5820-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5820-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c5820-110">Prerequisites</span></span>
<span data-ttu-id="c5820-111">För att konfigurera Azure AD-integrering med SciQuest tillbringar Director, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c5820-111">To configure Azure AD integration with SciQuest Spend Director, you need the following items:</span></span>

* <span data-ttu-id="c5820-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c5820-112">An Azure AD subscription</span></span>
* <span data-ttu-id="c5820-113">En SciQuest tillbringar Director enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c5820-113">A SciQuest Spend Director single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c5820-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c5820-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="c5820-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c5820-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="c5820-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c5820-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="c5820-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c5820-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="c5820-118">Scenario-beskrivning</span><span class="sxs-lookup"><span data-stu-id="c5820-118">Scenario Description</span></span>
<span data-ttu-id="c5820-119">Syftet med den här kursen är att du ska testa Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c5820-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="c5820-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c5820-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c5820-121">Att lägga till SciQuest tillbringar Director från galleriet</span><span class="sxs-lookup"><span data-stu-id="c5820-121">Adding SciQuest Spend Director from the gallery</span></span> 
2. <span data-ttu-id="c5820-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c5820-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sciquest-spend-director-from-the-gallery"></a><span data-ttu-id="c5820-123">Att lägga till SciQuest tillbringar Director från galleriet</span><span class="sxs-lookup"><span data-stu-id="c5820-123">Adding SciQuest Spend Director from the gallery</span></span>
<span data-ttu-id="c5820-124">Du måste lägga till SciQuest tillbringar Director från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SciQuest tillbringar Director i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c5820-124">To configure the integration of SciQuest Spend Director into Azure AD, you need to add SciQuest Spend Director from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c5820-125">**Utför följande steg för att lägga till SciQuest tillbringar Director från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c5820-125">**To add SciQuest Spend Director from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c5820-126">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c5820-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="c5820-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="c5820-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="c5820-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="c5820-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Program][2]

4. <span data-ttu-id="c5820-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="c5820-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Program][3]

5. <span data-ttu-id="c5820-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="c5820-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Program][4]

6. <span data-ttu-id="c5820-135">I sökrutan skriver **sciQuest tillbringar director**.</span><span class="sxs-lookup"><span data-stu-id="c5820-135">In the search box, type **sciQuest spend director**.</span></span>
   
    ![Program][5]

7. <span data-ttu-id="c5820-137">I resultatfönstret, Välj **SciQuest tillbringar Director**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c5820-137">In the results pane, select **SciQuest Spend Director**, and then click **Complete** to add the application.</span></span>
   
    ![Program][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c5820-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c5820-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c5820-140">Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Azure AD enkel inloggning med SciQuest tillbringar Director baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c5820-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with SciQuest Spend Director based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c5820-141">För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i SciQuest tillbringar Director till en användare i Azure AD är okänt.</span><span class="sxs-lookup"><span data-stu-id="c5820-141">For single sign-on to work, Azure AD needs to know what the counterpart user in SciQuest Spend Director to an user in Azure AD is.</span></span> <span data-ttu-id="c5820-142">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i SciQuest tillbringar Director upprättas.</span><span class="sxs-lookup"><span data-stu-id="c5820-142">In other words, a link relationship between an Azure AD user and the related user in SciQuest Spend Director needs to be established.</span></span>  
<span data-ttu-id="c5820-143">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="c5820-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SciQuest Spend Director.</span></span>

<span data-ttu-id="c5820-144">Om du vill konfigurera och testa Azure AD enkel inloggning med SciQuest tillbringar Director, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c5820-144">To configure and test Azure AD single sign-on with SciQuest Spend Director, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c5820-145">**[Konfigurera Azure AD enda enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c5820-145">**[Configuring Azure AD Single Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c5820-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c5820-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c5820-147">**[Skapa en testanvändare SciQuest tillbringar Director](#creating-a-halogen-software-test-user)**  – du har en motsvarighet för Britta Simon i SciQuest tillbringar Director som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="c5820-147">**[Creating a SciQuest Spend Director test user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in SciQuest Spend Director that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="c5820-148">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c5820-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c5820-149">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c5820-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-single-sign-on"></a><span data-ttu-id="c5820-150">Konfigurera Azure AD-en enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c5820-150">Configuring Azure AD Single Single Sign-On</span></span>
<span data-ttu-id="c5820-151">Syftet med det här avsnittet är att aktivera Azure AD enkel inloggning i den klassiska Azure-portalen och konfigurera enkel inloggning i ditt SciQuest tillbringar Director-program.</span><span class="sxs-lookup"><span data-stu-id="c5820-151">The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your SciQuest Spend Director application.</span></span>

<span data-ttu-id="c5820-152">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SciQuest tillbringar Director:**</span><span class="sxs-lookup"><span data-stu-id="c5820-152">**To configure Azure AD single sign-on with SciQuest Spend Director, perform the following steps:**</span></span>

1. <span data-ttu-id="c5820-153">I den klassiska Azure-portalen på den **SciQuest tillbringar Director** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning**  dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c5820-153">In the Azure classic portal, on the **SciQuest Spend Director** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][8]

2. <span data-ttu-id="c5820-155">På den **hur vill du att användarna kan logga in på SciQuest tillbringar Director** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c5820-155">On the **How would you like users to sign on to SciQuest Spend Director** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][9]

3. <span data-ttu-id="c5820-157">På den **konfigurera Appinställningar** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c5820-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span> 
   
    ![Konfigurera Appinställningar för][10]
   
     <span data-ttu-id="c5820-159">a.</span><span class="sxs-lookup"><span data-stu-id="c5820-159">a.</span></span> <span data-ttu-id="c5820-160">I den **logga URL** textruta, Skriv URL: en används av dina användare logga in i tillämpningsprogrammet SciQuest tillbringar chef med hjälp av följande mönster: *https://.* SciQuest.com/.**</span><span class="sxs-lookup"><span data-stu-id="c5820-160">In the **Sign On URL** textbox, type your URL used by your users to sign on to your SciQuest Spend Director application using the following pattern: *https://.*sciquest.com/.**</span></span>
   
     <span data-ttu-id="c5820-161">b.</span><span class="sxs-lookup"><span data-stu-id="c5820-161">b.</span></span> <span data-ttu-id="c5820-162">I den **Reply URL** textruta skriva in samma värde som du har skrivit i den **logga URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="c5820-162">In the **Reply URL** textbox, type the same value you have typed into the **Sign On URL** textbox.</span></span> 
   
     <span data-ttu-id="c5820-163">c.</span><span class="sxs-lookup"><span data-stu-id="c5820-163">c.</span></span> <span data-ttu-id="c5820-164">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c5820-164">Click **Next**.</span></span>

4. <span data-ttu-id="c5820-165">På den **Konfigurera enkel inloggning på SciQuest tillbringar Director** klickar du på **hämtar metadata**, och sedan spara metadatafilen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="c5820-165">On the **Configure single sign-on at SciQuest Spend Director** page, click **Download metadata**, and then save the metadata file locally on your computer.</span></span>
   
    ![Vad är Azure AD Connect?][11]

5. <span data-ttu-id="c5820-167">Kontakta SciQuest stöd för att aktivera den här autentiseringsmetoden använder ovan hämtade metadata.</span><span class="sxs-lookup"><span data-stu-id="c5820-167">Contact SciQuest support to enable this authentication method using the above downloaded metadata.</span></span>

6. <span data-ttu-id="c5820-168">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c5820-168">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span> 
   
    ![Vad är Azure AD Connect?][15]

7. <span data-ttu-id="c5820-170">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c5820-170">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c5820-171">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c5820-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="c5820-172">Syftet med det här avsnittet är att skapa en testanvändare i den klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c5820-172">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="c5820-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c5820-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c5820-174">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c5820-174">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Vad är Azure AD Connect?][100] 

2. <span data-ttu-id="c5820-176">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="c5820-176">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="c5820-177">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="c5820-177">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Vad är Azure AD Connect?][101] 

4. <span data-ttu-id="c5820-179">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="c5820-179">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Vad är Azure AD Connect?][102] 

5. <span data-ttu-id="c5820-181">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c5820-181">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Vad är Azure AD Connect?][103] 
   
    <span data-ttu-id="c5820-183">a.</span><span class="sxs-lookup"><span data-stu-id="c5820-183">a.</span></span> <span data-ttu-id="c5820-184">Som **typ av användare**väljer **ny användare i din organisation**.</span><span class="sxs-lookup"><span data-stu-id="c5820-184">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="c5820-185">b.</span><span class="sxs-lookup"><span data-stu-id="c5820-185">b.</span></span> <span data-ttu-id="c5820-186">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c5820-186">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="c5820-187">c.</span><span class="sxs-lookup"><span data-stu-id="c5820-187">c.</span></span> <span data-ttu-id="c5820-188">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c5820-188">Click **Next**.</span></span>

6. <span data-ttu-id="c5820-189">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c5820-189">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Vad är Azure AD Connect?][104] 
   
    <span data-ttu-id="c5820-191">a.</span><span class="sxs-lookup"><span data-stu-id="c5820-191">a.</span></span> <span data-ttu-id="c5820-192">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="c5820-192">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="c5820-193">b.</span><span class="sxs-lookup"><span data-stu-id="c5820-193">b.</span></span> <span data-ttu-id="c5820-194">I den **efternamn** txtbox typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="c5820-194">In the **Last Name** txtbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="c5820-195">c.</span><span class="sxs-lookup"><span data-stu-id="c5820-195">c.</span></span> <span data-ttu-id="c5820-196">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="c5820-196">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="c5820-197">d.</span><span class="sxs-lookup"><span data-stu-id="c5820-197">d.</span></span> <span data-ttu-id="c5820-198">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="c5820-198">In the **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="c5820-199">e.</span><span class="sxs-lookup"><span data-stu-id="c5820-199">e.</span></span> <span data-ttu-id="c5820-200">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c5820-200">Click **Next**.</span></span>

7. <span data-ttu-id="c5820-201">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c5820-201">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vad är Azure AD Connect?][105]  

8. <span data-ttu-id="c5820-203">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c5820-203">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Vad är Azure AD Connect?][106]   
   
    <span data-ttu-id="c5820-205">a.</span><span class="sxs-lookup"><span data-stu-id="c5820-205">a.</span></span> <span data-ttu-id="c5820-206">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c5820-206">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="c5820-207">b.</span><span class="sxs-lookup"><span data-stu-id="c5820-207">b.</span></span> <span data-ttu-id="c5820-208">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="c5820-208">Click **Complete**.</span></span>   

### <a name="creating-a-sciquest-spend-director-test-user"></a><span data-ttu-id="c5820-209">Skapa en testanvändare SciQuest tillbringar Director</span><span class="sxs-lookup"><span data-stu-id="c5820-209">Creating a SciQuest Spend Director test user</span></span>
<span data-ttu-id="c5820-210">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="c5820-210">The objective of this section is to create a user called Britta Simon in SciQuest Spend Director.</span></span>

<span data-ttu-id="c5820-211">Du måste Kontakta supportteamet SciQuest tillbringar Director och ange dem med information om kontot test för att hämta den skapade.</span><span class="sxs-lookup"><span data-stu-id="c5820-211">You need to contact your SciQuest Spend Director support team and provide them with the details about your test account to get it created.</span></span>

<span data-ttu-id="c5820-212">Du kan också användas just-in-time etablering, en enkel inloggning funktion som stöds av SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="c5820-212">Alternatively, you can also leverage just-in-time provisioning, a single sign-on feature that is supported by SciQuest Spend Director.</span></span>  
<span data-ttu-id="c5820-213">Om just-in-time-etablering aktiveras kan skapas användare automatiskt av SciQuest tillbringar Director under ett försök att enkel inloggning om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="c5820-213">If just-in-time provisioning is enabled, users are automatically created by SciQuest Spend Director during a single sign-on attempt if they don't exist.</span></span> <span data-ttu-id="c5820-214">Den här funktionen eliminerar behovet av att manuellt skapa användare motsvarighet för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c5820-214">This feature eliminates the need to manually create single sign-on counterpart users.</span></span>

<span data-ttu-id="c5820-215">För att få just-in-time-etablering aktiverat, måste du kontakta din supportteamet SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="c5820-215">To get just-in-time provisioning enabled, you need to contact your your SciQuest Spend Director support team.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c5820-216">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c5820-216">Assigning the Azure AD test user</span></span>
<span data-ttu-id="c5820-217">Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure enkel inloggning med sin åtkomst beviljas till SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="c5820-217">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to SciQuest Spend Director.</span></span>

![Vad är Azure AD Connect?][200]

<span data-ttu-id="c5820-219">**Om du vill tilldela SciQuest tillbringar Director Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c5820-219">**To assign Britta Simon to SciQuest Spend Director, perform the following steps:**</span></span>

1. <span data-ttu-id="c5820-220">På den klassiska Azure-portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="c5820-220">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Vad är Azure AD Connect?][201]

2. <span data-ttu-id="c5820-222">Välj i listan med program **SciQuest tillbringar Director**.</span><span class="sxs-lookup"><span data-stu-id="c5820-222">In the applications list, select **SciQuest Spend Director**.</span></span>
   
    ![Vad är Azure AD Connect?][202]

3. <span data-ttu-id="c5820-224">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="c5820-224">In the menu on the top, click **Users**.</span></span>
   
    ![Vad är Azure AD Connect?][203]

4. <span data-ttu-id="c5820-226">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="c5820-226">In the Users list, select **Britta Simon**.</span></span>
   
    ![Vad är Azure AD Connect?][204]

5. <span data-ttu-id="c5820-228">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="c5820-228">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Vad är Azure AD Connect?][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="c5820-230">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c5820-230">Testing Single Sign-On</span></span>
<span data-ttu-id="c5820-231">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c5820-231">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="c5820-232">När du klickar på panelen SciQuest tillbringar Director på åtkomstpanelen du bör få automatiskt loggat in på ditt SciQuest tillbringar Director-program.</span><span class="sxs-lookup"><span data-stu-id="c5820-232">When you click the SciQuest Spend Director tile in the Access Panel, you should get automatically signed-on to your SciQuest Spend Director application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c5820-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c5820-233">Additional Resources</span></span>
* [<span data-ttu-id="c5820-234">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c5820-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c5820-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c5820-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

