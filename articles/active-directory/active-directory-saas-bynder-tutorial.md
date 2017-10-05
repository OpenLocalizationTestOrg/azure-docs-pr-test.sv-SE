---
title: "Självstudier: Azure Active Directory-integrering med Bynder | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Bynder."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 6786d7eb6a11405278ef7267f25279f9e39b3bde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a><span data-ttu-id="27f89-103">Självstudier: Azure Active Directory-integrering med Bynder</span><span class="sxs-lookup"><span data-stu-id="27f89-103">Tutorial: Azure Active Directory integration with Bynder</span></span>
<span data-ttu-id="27f89-104">Syftet med den här kursen är att visa dig hur du integrerar Bynder med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="27f89-104">The objective of this tutorial is to show you how to integrate Bynder with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="27f89-105">Integrera Bynder med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="27f89-105">Integrating Bynder with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="27f89-106">Du kan styra i Azure AD som har åtkomst till Bynder</span><span class="sxs-lookup"><span data-stu-id="27f89-106">You can control in Azure AD who has access to Bynder</span></span>
* <span data-ttu-id="27f89-107">Du kan aktivera användarna att automatiskt hämta loggat in på Bynder enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="27f89-107">You can enable your users to automatically get signed-on to Bynder single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="27f89-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="27f89-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="27f89-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="27f89-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27f89-110">Krav</span><span class="sxs-lookup"><span data-stu-id="27f89-110">Prerequisites</span></span>
<span data-ttu-id="27f89-111">För att konfigurera Azure AD-integrering med Bynder, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="27f89-111">To configure Azure AD integration with Bynder, you need the following items:</span></span>

* <span data-ttu-id="27f89-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="27f89-112">An Azure AD subscription</span></span>
* <span data-ttu-id="27f89-113">En Bynder enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="27f89-113">A Bynder single-sign on (SSO) enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="27f89-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="27f89-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="27f89-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="27f89-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="27f89-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="27f89-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="27f89-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="27f89-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="27f89-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="27f89-118">Scenario description</span></span>
<span data-ttu-id="27f89-119">Syftet med den här kursen är att du ska testa Microsoft Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="27f89-119">The objective of this tutorial is to enable you to test Microsoft Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="27f89-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="27f89-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="27f89-121">Att lägga till Bynder från galleriet</span><span class="sxs-lookup"><span data-stu-id="27f89-121">Adding Bynder from the gallery</span></span>
2. <span data-ttu-id="27f89-122">Konfigurera och testa Microsoft Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="27f89-122">Configuring and testing Microsoft Azure AD SSO</span></span>

## <a name="add-bynder-from-the-gallery"></a><span data-ttu-id="27f89-123">Lägg till Bynder från galleriet</span><span class="sxs-lookup"><span data-stu-id="27f89-123">Add Bynder from the gallery</span></span>
<span data-ttu-id="27f89-124">Du måste lägga till Bynder från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Bynder i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="27f89-124">To configure the integration of Bynder into Azure AD, you need to add Bynder from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="27f89-125">**Utför följande steg för att lägga till Bynder från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="27f89-125">**To add Bynder from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="27f89-126">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="27f89-126">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="27f89-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="27f89-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="27f89-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="27f89-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Program][2]
4. <span data-ttu-id="27f89-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="27f89-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Program][3]
5. <span data-ttu-id="27f89-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="27f89-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Program][4]
6. <span data-ttu-id="27f89-135">I sökrutan skriver **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="27f89-135">In the search box, type **Bynder**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. <span data-ttu-id="27f89-137">Välj i resultatpanelen **Bynder**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="27f89-137">In the results panel, select **Bynder**, and then click **Complete** to add the application.</span></span>
   
    ![Markera appen i galleriet](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a><span data-ttu-id="27f89-139">Konfigurera och testa Microsoft Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="27f89-139">Configure and test Microsoft Azure AD SSO</span></span>
<span data-ttu-id="27f89-140">Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Microsoft Azure AD SSO med Bynder baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="27f89-140">The objective of this section is to show you how to configure and test Microsoft Azure AD SSO with Bynder based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="27f89-141">För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i Bynder till en användare i Azure AD är okänt.</span><span class="sxs-lookup"><span data-stu-id="27f89-141">For SSO to work, Azure AD needs to know what the counterpart user in Bynder to an user in Azure AD is.</span></span> <span data-ttu-id="27f89-142">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Bynder upprättas.</span><span class="sxs-lookup"><span data-stu-id="27f89-142">In other words, a link relationship between an Azure AD user and the related user in Bynder needs to be established.</span></span>

<span data-ttu-id="27f89-143">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Bynder.</span><span class="sxs-lookup"><span data-stu-id="27f89-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Bynder.</span></span>

<span data-ttu-id="27f89-144">Om du vill konfigurera och testa Microsoft Azure AD SSO med Bynder, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="27f89-144">To configure and test Microsoft Azure AD SSO with Bynder, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="27f89-145">**[Konfigurera Microsoft Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="27f89-145">**[Configuring Microsoft Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="27f89-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – för att testa Microsoft Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27f89-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Microsoft Azure AD Single Sign-On with Britta Simon.</span></span>
3. <span data-ttu-id="27f89-147">**[Skapa en testanvändare Bynder](#creating-a-bynder-test-user)**  – du har en motsvarighet för Britta Simon i Bynder som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="27f89-147">**[Creating a Bynder test user](#creating-a-bynder-test-user)** - to have a counterpart of Britta Simon in Bynder that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="27f89-148">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Microsoft Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="27f89-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Microsoft Azure AD Single Sign-On.</span></span>
5. <span data-ttu-id="27f89-149">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="27f89-149">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-microsoft-azure-ad-sso"></a><span data-ttu-id="27f89-150">Konfigurera Microsoft Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="27f89-150">Configuring Microsoft Azure AD SSO</span></span>
<span data-ttu-id="27f89-151">I det här avsnittet att aktivera Microsoft Azure AD SSO i den klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Bynder.</span><span class="sxs-lookup"><span data-stu-id="27f89-151">In this section, you enable Microsoft Azure AD SSO in the classic portal and configure SSO in your Bynder application.</span></span>

<span data-ttu-id="27f89-152">**Utför följande steg för att konfigurera Microsoft Azure AD SSO med Bynder:**</span><span class="sxs-lookup"><span data-stu-id="27f89-152">**To configure Microsoft Azure AD SSO with Bynder, perform the following steps:**</span></span>

1. <span data-ttu-id="27f89-153">I den klassiska portalen på den **Bynder** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27f89-153">In the classic portal, on the **Bynder** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][6] 
2. <span data-ttu-id="27f89-155">På den **hur vill du att användarna kan logga in på Bynder** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-155">On the **How would you like users to sign on to Bynder** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. <span data-ttu-id="27f89-157">På den **konfigurera Appinställningar** dialogrutan sidan om du vill konfigurera programmet i **IDP initierade läge**, utför följande steg och klickar på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="27f89-157">On the **Configure App Settings** dialog page, If you wish to configure the application in **IDP initiated mode**, perform the following steps and click **Next**:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. <span data-ttu-id="27f89-159">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.getbynder.com/sso/SAML/authenticate/`</span><span class="sxs-lookup"><span data-stu-id="27f89-159">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.getbynder.com/sso/SAML/authenticate/`</span></span>
  2. <span data-ttu-id="27f89-160">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-160">Click **Next**.</span></span>
4. <span data-ttu-id="27f89-161">Om du vill konfigurera programmet i **SP initierade läge** på den **konfigurera Appinställningar** dialogrutan sidan, klickar på den **”visa avancerade inställningar (valfritt)”** och ange sedan den **logga URL** och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-161">If you wish to configure the application in **SP initiated mode** on the **Configure App Settings** dialog page, then click on the **“Show advanced settings (optional)”** and then enter the **Sign On URL** and click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. <span data-ttu-id="27f89-163">I den **logga URL** textruta Skriv en URL med följande mönster:`https://<company name>.getbynder.com/login/`</span><span class="sxs-lookup"><span data-stu-id="27f89-163">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<company name>.getbynder.com/login/`</span></span>
  2. <span data-ttu-id="27f89-164">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-164">Click **Next**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="27f89-165">Värdet för inloggning på URL: en i den här kursen är bara en placeholfer.</span><span class="sxs-lookup"><span data-stu-id="27f89-165">The value for the Sign On URL in this tutorial is just a placeholfer.</span></span> <span data-ttu-id="27f89-166">Kontakta Bynder för att få det faktiska värdet för din miljö.</span><span class="sxs-lookup"><span data-stu-id="27f89-166">To get the actual vlaue for your environment, contact Bynder.</span></span>
   >

5. <span data-ttu-id="27f89-167">På den **Konfigurera enkel inloggning på Bynder** , utför följande steg och klickar på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="27f89-167">On the **Configure single sign-on at Bynder** page, perform the following steps and click **Next**:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. <span data-ttu-id="27f89-169">Klicka på **hämtar metadata**, och sedan spara filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="27f89-169">Click **Download metadata**, and then save the file on your computer.</span></span>
  2. <span data-ttu-id="27f89-170">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-170">Click **Next**.</span></span>
6. <span data-ttu-id="27f89-171">Kontakta supportteamet Bynder för att få SSO konfigurerats för ditt program.</span><span class="sxs-lookup"><span data-stu-id="27f89-171">To get SSO configured for your application, contact your Bynder support team.</span></span> <span data-ttu-id="27f89-172">Koppla hämtade metadatafilen och dela den med Bynder-teamet för att konfigurera enkel inloggning på sidan.</span><span class="sxs-lookup"><span data-stu-id="27f89-172">Attach the downloaded metadata file and share it with Bynder team to set up SSO on their side.</span></span>
7. <span data-ttu-id="27f89-173">I den klassiska portalen markerar du konfigurationen för enkel inloggning bekräftelsen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-173">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][10]
8. <span data-ttu-id="27f89-175">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="27f89-175">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="27f89-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="27f89-177">Create an Azure AD test user</span></span>
<span data-ttu-id="27f89-178">Syftet med det här avsnittet är att skapa en testanvändare i den klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27f89-178">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="27f89-180">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="27f89-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="27f89-181">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="27f89-181">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="27f89-183">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="27f89-183">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="27f89-184">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="27f89-184">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="27f89-186">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="27f89-186">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="27f89-188">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="27f89-188">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. <span data-ttu-id="27f89-190">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="27f89-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="27f89-191">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="27f89-191">In the User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="27f89-192">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-192">Click **Next**.</span></span>
6. <span data-ttu-id="27f89-193">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="27f89-193">On the **User Profile** dialog page, perform the following steps:</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="27f89-195">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="27f89-195">In the **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="27f89-196">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="27f89-196">In the **Last Name** textbox, type, **Simon**.</span></span> 
  3. <span data-ttu-id="27f89-197">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="27f89-197">In the **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="27f89-198">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="27f89-198">In the **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="27f89-199">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-199">Click **Next**.</span></span>
7. <span data-ttu-id="27f89-200">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="27f89-200">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="27f89-202">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="27f89-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. <span data-ttu-id="27f89-204">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="27f89-204">Write down the value of the **New Password**.</span></span>
   2. <span data-ttu-id="27f89-205">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="27f89-205">Click **Complete**.</span></span>   

### <a name="create-a-bynder-test-user"></a><span data-ttu-id="27f89-206">Skapa en testanvändare Bynder</span><span class="sxs-lookup"><span data-stu-id="27f89-206">Create a Bynder test user</span></span>
<span data-ttu-id="27f89-207">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Bynder.</span><span class="sxs-lookup"><span data-stu-id="27f89-207">The objective of this section is to create a user called Britta Simon in Bynder.</span></span> <span data-ttu-id="27f89-208">Bynder stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="27f89-208">Bynder supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="27f89-209">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="27f89-209">There is no action item for you in this section.</span></span> <span data-ttu-id="27f89-210">En ny användare skapas vid ett försök att komma åt Bynder om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="27f89-210">A new user will be created during an attempt to access Bynder if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="27f89-211">Om du behöver skapa en användare manuellt måste du kontakta supportteamet Bynder.</span><span class="sxs-lookup"><span data-stu-id="27f89-211">If you need to create an user manually, you need to contact the Bynder support team.</span></span> 
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="27f89-212">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="27f89-212">Assign the Azure AD test user</span></span>
<span data-ttu-id="27f89-213">Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure SSO genom att bevilja sin åtkomst till Bynder.</span><span class="sxs-lookup"><span data-stu-id="27f89-213">The objective of this section is to enabling Britta Simon to use Azure SSO by granting her access to Bynder.</span></span>

   ![Tilldela användare][200]

<span data-ttu-id="27f89-215">**Om du vill tilldela Bynder Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27f89-215">**To assign Britta Simon to Bynder, perform the following steps:**</span></span>

1. <span data-ttu-id="27f89-216">På den klassiska portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="27f89-216">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Tilldela användare][201]
2. <span data-ttu-id="27f89-218">Välj i listan med program **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="27f89-218">In the applications list, select **Bynder**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. <span data-ttu-id="27f89-220">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="27f89-220">In the menu on the top, click **Users**.</span></span>
   
    ![Tilldela användare][203]
4. <span data-ttu-id="27f89-222">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="27f89-222">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="27f89-223">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="27f89-223">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="27f89-225">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27f89-225">Test single sign-on</span></span>
<span data-ttu-id="27f89-226">Syftet med det här avsnittet är att testa Microsoft Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="27f89-226">The objective of this section is to test your Microsoft Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="27f89-227">När du klickar på panelen Bynder på åtkomstpanelen du bör få automatiskt loggat in på ditt Bynder program.</span><span class="sxs-lookup"><span data-stu-id="27f89-227">When you click the Bynder tile in the Access Panel, you should get automatically signed-on to your Bynder application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27f89-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="27f89-228">Additional resources</span></span>
* [<span data-ttu-id="27f89-229">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="27f89-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="27f89-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="27f89-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
