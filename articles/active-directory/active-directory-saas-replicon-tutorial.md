---
title: "Självstudier: Azure Active Directory-integrering med Replicon | Microsoft Docs"
description: "Lär dig hur du använder Replicon med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2aeeceb61191962b62892b8409218684f76c6fa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a><span data-ttu-id="47703-103">Självstudier: Azure Active Directory-integrering med Replicon</span><span class="sxs-lookup"><span data-stu-id="47703-103">Tutorial: Azure Active Directory integration with Replicon</span></span>
<span data-ttu-id="47703-104">Syftet med den här kursen är att visa integreringen av Azure och Replicon.</span><span class="sxs-lookup"><span data-stu-id="47703-104">The objective of this tutorial is to show the integration of Azure and Replicon.</span></span> <span data-ttu-id="47703-105">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="47703-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="47703-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="47703-106">A valid Azure subscription</span></span>
* <span data-ttu-id="47703-107">En Replicon-klient</span><span class="sxs-lookup"><span data-stu-id="47703-107">A Replicon tenant</span></span>

<span data-ttu-id="47703-108">Den här kursen Azure AD-användare som du har tilldelat Replicon kommer att kunna enkel inloggning till programmet på din Replicon företagets plats (service provider initierade inloggning) eller med hjälp av den [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="47703-108">After completing this tutorial, the Azure AD users you have assigned to Replicon will be able to single sign into the application at your Replicon company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="47703-109">Det scenario som beskrivs i den här kursen består av följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="47703-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="47703-110">Aktivera programintegrationstyp för Replicon</span><span class="sxs-lookup"><span data-stu-id="47703-110">Enabling the application integration for Replicon</span></span>
2. <span data-ttu-id="47703-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="47703-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="47703-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="47703-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="47703-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="47703-113">Assigning users</span></span>

<span data-ttu-id="47703-114">![Scenariot](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="47703-114">![Scenario](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-replicon"></a><span data-ttu-id="47703-115">Aktivera application-integration för Replicon</span><span class="sxs-lookup"><span data-stu-id="47703-115">Enable the application integration for Replicon</span></span>
<span data-ttu-id="47703-116">Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för Replicon.</span><span class="sxs-lookup"><span data-stu-id="47703-116">The objective of this section is to outline how to enable the application integration for Replicon.</span></span>

<span data-ttu-id="47703-117">**Utför följande steg om du vill aktivera programmet-integrering för Replicon:**</span><span class="sxs-lookup"><span data-stu-id="47703-117">**To enable the application integration for Replicon, perform the following steps:**</span></span>

1. <span data-ttu-id="47703-118">I den klassiska Azure-portalen i det vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="47703-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="47703-119">![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="47703-119">![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="47703-120">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="47703-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="47703-121">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="47703-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="47703-122">![Program](./media/active-directory-saas-replicon-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="47703-122">![Applications](./media/active-directory-saas-replicon-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="47703-123">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="47703-123">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="47703-124">![Lägg till program](./media/active-directory-saas-replicon-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="47703-124">![Add application](./media/active-directory-saas-replicon-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="47703-125">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="47703-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="47703-126">![Lägga till ett program från gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="47703-126">![Add an application from gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="47703-127">I den **sökrutan**, typen **Replicon**.</span><span class="sxs-lookup"><span data-stu-id="47703-127">In the **search box**, type **Replicon**.</span></span>
   
    <span data-ttu-id="47703-128">![Programgalleriet](./media/active-directory-saas-replicon-tutorial/IC777799.png "programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="47703-128">![Application gallery](./media/active-directory-saas-replicon-tutorial/IC777799.png "Application gallery")</span></span>
7. <span data-ttu-id="47703-129">I resultatfönstret, Välj **Replicon**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="47703-129">In the results pane, select **Replicon**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="47703-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span><span class="sxs-lookup"><span data-stu-id="47703-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="47703-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="47703-131">Configure single sign-on</span></span>

<span data-ttu-id="47703-132">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till Replicon med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="47703-132">The objective of this section is to outline how to enable users to authenticate to Replicon with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="47703-133">**Utför följande steg för att konfigurera enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="47703-133">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="47703-134">I den klassiska Azure-portalen på den **Replicon** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="47703-134">In the Azure classic portal, on the **Replicon** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="47703-135">![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777801.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="47703-135">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777801.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="47703-136">På den **hur vill du att användarna kan logga in på Replicon** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="47703-136">On the **How would you like users to sign on to Replicon** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="47703-137">![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777802.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="47703-137">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777802.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="47703-138">På den **konfigurera App-URL** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="47703-138">On the **Configure App URL** page, perform the following steps:</span></span>
   
    <span data-ttu-id="47703-139">![Konfigurera app-URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="47703-139">![Configure app URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "Configure app URL")</span></span>
  1. <span data-ttu-id="47703-140">I den **Replicon logga URL** textruta Skriv din Replicon klient-URL (t.ex.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span><span class="sxs-lookup"><span data-stu-id="47703-140">In the **Replicon Sign On URL** textbox, type your Replicon tenant URL (e.g.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span></span>
  2. <span data-ttu-id="47703-141">I den **Replicon Reply URL** textruta Skriv din Replicon **AssertionConsumerService** URL (t.ex.: *https://global.replicon.com/! / saml2/företag/sso/post*).</span><span class="sxs-lookup"><span data-stu-id="47703-141">In the **Replicon Reply URL** textbox, type your Replicon **AssertionConsumerService** URL(e.g.: *https://global.replicon.com/!/saml2/company/sso/post*).</span></span>  
      
     >[!NOTE]
     ><span data-ttu-id="47703-142">Du kan hämta Webbadressen från Replicon metadata på: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.</span><span class="sxs-lookup"><span data-stu-id="47703-142">You can get the URL from the Replicon metadata at: **https://global.replicon.com/!/saml2/\<YourCompanyKey\>**.</span></span>
     > 
     > 
 
  3. <span data-ttu-id="47703-143">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="47703-143">Click **Next**.</span></span>

4. <span data-ttu-id="47703-144">På den **Konfigurera enkel inloggning på Replicon** att hämta metadata, klickar du på **hämtar metadata**, och sedan spara metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="47703-144">On the **Configure single sign-on at Replicon** page, to download your metadata, click **Download metadata**, and then save the metadata on your computer.</span></span>
   
    <span data-ttu-id="47703-145">![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777804.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="47703-145">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777804.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="47703-146">Logga in på webbplatsen Replicon företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="47703-146">In a different web browser window, log into your Replicon company site as an administrator.</span></span>

6. <span data-ttu-id="47703-147">Utför följande steg för att konfigurera SAML 2.0:</span><span class="sxs-lookup"><span data-stu-id="47703-147">To configure SAML 2.0, perform the following steps:</span></span>
   
    <span data-ttu-id="47703-148">![Aktivera SAML-autentisering](./media/active-directory-saas-replicon-tutorial/IC777805.png "aktivera SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="47703-148">![Enable SAML authentication](./media/active-directory-saas-replicon-tutorial/IC777805.png "Enable SAML authentication")</span></span>
  
  1. <span data-ttu-id="47703-149">Visa den **EnableSAML Authentication2** dialogrutan Lägg till följande i din URL efter din nyckel för företag: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="47703-149">To display the **EnableSAML Authentication2** dialog, append the following to your URL, after your company key: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>  
    * <span data-ttu-id="47703-150">Nedan visas schemat för fullständig URL:</span><span class="sxs-lookup"><span data-stu-id="47703-150">The following shows the schema of the complete URL:</span></span>  
   <span data-ttu-id="47703-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="47703-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>
   2. <span data-ttu-id="47703-152">Klicka på den  **+**  att expandera den **v20Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="47703-152">Click the **+** to expand the **v20Configuration** section.</span></span>
   3. <span data-ttu-id="47703-153">Klicka på den  **+**  att expandera den **metaDataConfiguration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="47703-153">Click the **+** to expand the **metaDataConfiguration** section.</span></span>
   4. <span data-ttu-id="47703-154">Klicka på **Välj fil**, för att välja identitet providern metadata XML-filen och klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="47703-154">Click **Choose File**, to select your identity provider metadata XML file, and click **Submit**.</span></span>

7. <span data-ttu-id="47703-155">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="47703-155">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="47703-156">![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC778418.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="47703-156">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC778418.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="47703-157">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="47703-157">Configure user provisioning</span></span>

<span data-ttu-id="47703-158">För att aktivera Azure AD-användare att logga in på Replicon etableras de i Replicon.</span><span class="sxs-lookup"><span data-stu-id="47703-158">In order to enable Azure AD users to log into Replicon, they must be provisioned into Replicon.</span></span>  

<span data-ttu-id="47703-159">När det gäller Replicon är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="47703-159">In the case of Replicon, provisioning is a manual task.</span></span>

<span data-ttu-id="47703-160">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="47703-160">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="47703-161">Logga in på webbplatsen Replicon företag som en administratör i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="47703-161">In a web browser window, log into your Replicon company site as an administrator.</span></span>
2. <span data-ttu-id="47703-162">Gå till **Administration \> användare**.</span><span class="sxs-lookup"><span data-stu-id="47703-162">Go to **Administration \> Users**.</span></span>
   
    <span data-ttu-id="47703-163">![Användare](./media/active-directory-saas-replicon-tutorial/IC777806.png "användare")</span><span class="sxs-lookup"><span data-stu-id="47703-163">![Users](./media/active-directory-saas-replicon-tutorial/IC777806.png "Users")</span></span>
3. <span data-ttu-id="47703-164">Klicka på **+ Lägg till användaren**.</span><span class="sxs-lookup"><span data-stu-id="47703-164">Click **+Add User**.</span></span>
   
    <span data-ttu-id="47703-165">![Lägg till användare](./media/active-directory-saas-replicon-tutorial/IC777807.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="47703-165">![Add User](./media/active-directory-saas-replicon-tutorial/IC777807.png "Add User")</span></span>
4. <span data-ttu-id="47703-166">I den **användarprofil** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="47703-166">In the **User Profile** section, perform the following steps:</span></span>
   
    <span data-ttu-id="47703-167">![Användarprofil](./media/active-directory-saas-replicon-tutorial/IC777808.png "användarprofil")</span><span class="sxs-lookup"><span data-stu-id="47703-167">![User profile](./media/active-directory-saas-replicon-tutorial/IC777808.png "User profile")</span></span>
   
  1. <span data-ttu-id="47703-168">I den **inloggningsnamnet** textruta typen Azure AD Azure AD-användare som du vill etablera e-postadress.</span><span class="sxs-lookup"><span data-stu-id="47703-168">In the **Login Name** textbox, type the Azure AD email address of the Azure AD user you want to provision.</span></span>
  2. <span data-ttu-id="47703-169">Som **autentiseringstyp**väljer **SSO**.</span><span class="sxs-lookup"><span data-stu-id="47703-169">As **Authentication Type**, select **SSO**.</span></span>
  3. <span data-ttu-id="47703-170">I den **avdelning** textruta Skriv användarens avdelning.</span><span class="sxs-lookup"><span data-stu-id="47703-170">In the **Department** textbox, type the user’s department.</span></span>
  4. <span data-ttu-id="47703-171">Som **typen**väljer **administratör**.</span><span class="sxs-lookup"><span data-stu-id="47703-171">As **Employee Type**, select **Administrator**.</span></span>
  5. <span data-ttu-id="47703-172">Klicka på **spara användarprofil**.</span><span class="sxs-lookup"><span data-stu-id="47703-172">Click **Save User Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="47703-173">Du kan använda något annat Replicon användarens konto skapas verktyg eller API: er som tillhandahålls av Replicon att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="47703-173">You can use any other Replicon user account creation tools or APIs provided by Replicon to provision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="47703-174">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="47703-174">Assign users</span></span>
<span data-ttu-id="47703-175">Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.</span><span class="sxs-lookup"><span data-stu-id="47703-175">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="47703-176">**Utför följande steg om du vill tilldela Replicon användare:**</span><span class="sxs-lookup"><span data-stu-id="47703-176">**To assign users to Replicon, perform the following steps:**</span></span>

1. <span data-ttu-id="47703-177">Skapa ett testkonto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="47703-177">In the Azure classic portal, create a test account.</span></span>

2. <span data-ttu-id="47703-178">På den **Replicon** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="47703-178">On the **Replicon** application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="47703-179">![Tilldela användare](./media/active-directory-saas-replicon-tutorial/IC777809.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="47703-179">![Assign users](./media/active-directory-saas-replicon-tutorial/IC777809.png "Assign users")</span></span>

3. <span data-ttu-id="47703-180">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.</span><span class="sxs-lookup"><span data-stu-id="47703-180">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
    <span data-ttu-id="47703-181">![Ja](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="47703-181">![Yes](./media/active-directory-saas-replicon-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="47703-182">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="47703-182">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="47703-183">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="47703-183">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

