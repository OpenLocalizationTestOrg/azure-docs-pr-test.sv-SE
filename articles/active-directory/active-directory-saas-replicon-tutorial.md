---
title: "Självstudier: Azure Active Directory-integrering med Replicon | Microsoft Docs"
description: "Lär dig hur toouse Replicon med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: 4949eaf959265cfa4f732a2b73317fffe6312a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a><span data-ttu-id="beba5-103">Självstudier: Azure Active Directory-integrering med Replicon</span><span class="sxs-lookup"><span data-stu-id="beba5-103">Tutorial: Azure Active Directory integration with Replicon</span></span>
<span data-ttu-id="beba5-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och Replicon.</span><span class="sxs-lookup"><span data-stu-id="beba5-104">hello objective of this tutorial is tooshow hello integration of Azure and Replicon.</span></span> <span data-ttu-id="beba5-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="beba5-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="beba5-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="beba5-106">A valid Azure subscription</span></span>
* <span data-ttu-id="beba5-107">En Replicon-klient</span><span class="sxs-lookup"><span data-stu-id="beba5-107">A Replicon tenant</span></span>

<span data-ttu-id="beba5-108">Den här kursen hello Azure AD-användare som du har tilldelat tooReplicon blir toosingle kan logga in på hello-programmet på din Replicon företagets webbplats (service provider initierade inloggning) eller med hjälp av hello [introduktion toohello åtkomst Panelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="beba5-108">After completing this tutorial, hello Azure AD users you have assigned tooReplicon will be able toosingle sign into hello application at your Replicon company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="beba5-109">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="beba5-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="beba5-110">Aktivera programmet hello-integrering för Replicon</span><span class="sxs-lookup"><span data-stu-id="beba5-110">Enabling hello application integration for Replicon</span></span>
2. <span data-ttu-id="beba5-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="beba5-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="beba5-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="beba5-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="beba5-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="beba5-113">Assigning users</span></span>

<span data-ttu-id="beba5-114">![Scenariot](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="beba5-114">![Scenario](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-replicon"></a><span data-ttu-id="beba5-115">Aktivera hello program-integration för Replicon</span><span class="sxs-lookup"><span data-stu-id="beba5-115">Enable hello application integration for Replicon</span></span>
<span data-ttu-id="beba5-116">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Replicon.</span><span class="sxs-lookup"><span data-stu-id="beba5-116">hello objective of this section is toooutline how tooenable hello application integration for Replicon.</span></span>

<span data-ttu-id="beba5-117">**tooenable hello programintegrationstyp för Replicon, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="beba5-117">**tooenable hello application integration for Replicon, perform hello following steps:**</span></span>

1. <span data-ttu-id="beba5-118">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="beba5-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="beba5-119">![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="beba5-119">![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="beba5-120">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="beba5-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="beba5-121">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="beba5-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="beba5-122">![Program](./media/active-directory-saas-replicon-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="beba5-122">![Applications](./media/active-directory-saas-replicon-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="beba5-123">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="beba5-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="beba5-124">![Lägg till program](./media/active-directory-saas-replicon-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="beba5-124">![Add application](./media/active-directory-saas-replicon-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="beba5-125">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="beba5-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="beba5-126">![Lägga till ett program från gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="beba5-126">![Add an application from gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="beba5-127">I hello **sökrutan**, typen **Replicon**.</span><span class="sxs-lookup"><span data-stu-id="beba5-127">In hello **search box**, type **Replicon**.</span></span>
   
    <span data-ttu-id="beba5-128">![Programgalleriet](./media/active-directory-saas-replicon-tutorial/IC777799.png "programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="beba5-128">![Application gallery](./media/active-directory-saas-replicon-tutorial/IC777799.png "Application gallery")</span></span>
7. <span data-ttu-id="beba5-129">I resultatfönstret hello väljer **Replicon**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="beba5-129">In hello results pane, select **Replicon**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="beba5-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span><span class="sxs-lookup"><span data-stu-id="beba5-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="beba5-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="beba5-131">Configure single sign-on</span></span>

<span data-ttu-id="beba5-132">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooReplicon med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="beba5-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooReplicon with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="beba5-133">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="beba5-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="beba5-134">I hello klassiska Azure-portalen på hello **Replicon** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="beba5-134">In hello Azure classic portal, on hello **Replicon** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="beba5-135">![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777801.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="beba5-135">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777801.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="beba5-136">På hello **hur skulle du som användare toosign på tooReplicon** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="beba5-136">On hello **How would you like users toosign on tooReplicon** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="beba5-137">![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777802.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="beba5-137">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777802.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="beba5-138">På hello **konfigurera App-URL** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="beba5-138">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="beba5-139">![Konfigurera app-URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="beba5-139">![Configure app URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "Configure app URL")</span></span>
  1. <span data-ttu-id="beba5-140">I hello **Replicon logga URL** textruta Skriv din Replicon klient-URL (t.ex.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span><span class="sxs-lookup"><span data-stu-id="beba5-140">In hello **Replicon Sign On URL** textbox, type your Replicon tenant URL (e.g.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span></span>
  2. <span data-ttu-id="beba5-141">I hello **Replicon Reply URL** textruta Skriv din Replicon **AssertionConsumerService** URL (t.ex.: *https://global.replicon.com/! / saml2/företag/sso/post*).</span><span class="sxs-lookup"><span data-stu-id="beba5-141">In hello **Replicon Reply URL** textbox, type your Replicon **AssertionConsumerService** URL(e.g.: *https://global.replicon.com/!/saml2/company/sso/post*).</span></span>  
      
     >[!NOTE]
     ><span data-ttu-id="beba5-142">Du kan hämta hello URL från hello Replicon metadata på: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.</span><span class="sxs-lookup"><span data-stu-id="beba5-142">You can get hello URL from hello Replicon metadata at: **https://global.replicon.com/!/saml2/\<YourCompanyKey\>**.</span></span>
     > 
     > 
 
  3. <span data-ttu-id="beba5-143">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="beba5-143">Click **Next**.</span></span>

4. <span data-ttu-id="beba5-144">På hello **Konfigurera enkel inloggning på Replicon** sida, toodownload metadata, klickar du på **hämtar metadata**, och sedan spara hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="beba5-144">On hello **Configure single sign-on at Replicon** page, toodownload your metadata, click **Download metadata**, and then save hello metadata on your computer.</span></span>
   
    <span data-ttu-id="beba5-145">![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777804.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="beba5-145">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777804.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="beba5-146">Logga in på webbplatsen Replicon företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="beba5-146">In a different web browser window, log into your Replicon company site as an administrator.</span></span>

6. <span data-ttu-id="beba5-147">tooconfigure SAML 2.0 utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="beba5-147">tooconfigure SAML 2.0, perform hello following steps:</span></span>
   
    <span data-ttu-id="beba5-148">![Aktivera SAML-autentisering](./media/active-directory-saas-replicon-tutorial/IC777805.png "aktivera SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="beba5-148">![Enable SAML authentication](./media/active-directory-saas-replicon-tutorial/IC777805.png "Enable SAML authentication")</span></span>
  
  1. <span data-ttu-id="beba5-149">toodisplay hello **EnableSAML Authentication2** dialogrutan bifoga hello följande tooyour URL efter din nyckel för företag: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="beba5-149">toodisplay hello **EnableSAML Authentication2** dialog, append hello following tooyour URL, after your company key: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>  
    * <span data-ttu-id="beba5-150">hello följande visar hello schemat för hello fullständiga URL: en:</span><span class="sxs-lookup"><span data-stu-id="beba5-150">hello following shows hello schema of hello complete URL:</span></span>  
   <span data-ttu-id="beba5-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="beba5-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>
   2. <span data-ttu-id="beba5-152">Klicka på hello  **+**  tooexpand hello **v20Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="beba5-152">Click hello **+** tooexpand hello **v20Configuration** section.</span></span>
   3. <span data-ttu-id="beba5-153">Klicka på hello  **+**  tooexpand hello **metaDataConfiguration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="beba5-153">Click hello **+** tooexpand hello **metaDataConfiguration** section.</span></span>
   4. <span data-ttu-id="beba5-154">Klicka på **Välj fil**, tooselect din identitet providern metadata XML-filen och klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="beba5-154">Click **Choose File**, tooselect your identity provider metadata XML file, and click **Submit**.</span></span>

7. <span data-ttu-id="beba5-155">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="beba5-155">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="beba5-156">![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC778418.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="beba5-156">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC778418.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="beba5-157">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="beba5-157">Configure user provisioning</span></span>

<span data-ttu-id="beba5-158">I ordning tooenable Azure AD-användare toolog i Replicon, måste de etableras i Replicon.</span><span class="sxs-lookup"><span data-stu-id="beba5-158">In order tooenable Azure AD users toolog into Replicon, they must be provisioned into Replicon.</span></span>  

<span data-ttu-id="beba5-159">Hello gäller Replicon är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="beba5-159">In hello case of Replicon, provisioning is a manual task.</span></span>

<span data-ttu-id="beba5-160">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="beba5-160">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="beba5-161">Logga in på webbplatsen Replicon företag som en administratör i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="beba5-161">In a web browser window, log into your Replicon company site as an administrator.</span></span>
2. <span data-ttu-id="beba5-162">Gå för**Administration \> användare**.</span><span class="sxs-lookup"><span data-stu-id="beba5-162">Go too**Administration \> Users**.</span></span>
   
    <span data-ttu-id="beba5-163">![Användare](./media/active-directory-saas-replicon-tutorial/IC777806.png "användare")</span><span class="sxs-lookup"><span data-stu-id="beba5-163">![Users](./media/active-directory-saas-replicon-tutorial/IC777806.png "Users")</span></span>
3. <span data-ttu-id="beba5-164">Klicka på **+ Lägg till användaren**.</span><span class="sxs-lookup"><span data-stu-id="beba5-164">Click **+Add User**.</span></span>
   
    <span data-ttu-id="beba5-165">![Lägg till användare](./media/active-directory-saas-replicon-tutorial/IC777807.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="beba5-165">![Add User](./media/active-directory-saas-replicon-tutorial/IC777807.png "Add User")</span></span>
4. <span data-ttu-id="beba5-166">I hello **användarprofil** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="beba5-166">In hello **User Profile** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="beba5-167">![Användarprofil](./media/active-directory-saas-replicon-tutorial/IC777808.png "användarprofil")</span><span class="sxs-lookup"><span data-stu-id="beba5-167">![User profile](./media/active-directory-saas-replicon-tutorial/IC777808.png "User profile")</span></span>
   
  1. <span data-ttu-id="beba5-168">I hello **inloggningsnamnet** textruta typen hello Azure AD e-postadress för hello Azure AD-användare som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="beba5-168">In hello **Login Name** textbox, type hello Azure AD email address of hello Azure AD user you want tooprovision.</span></span>
  2. <span data-ttu-id="beba5-169">Som **autentiseringstyp**väljer **SSO**.</span><span class="sxs-lookup"><span data-stu-id="beba5-169">As **Authentication Type**, select **SSO**.</span></span>
  3. <span data-ttu-id="beba5-170">I hello **avdelning** textruta skriver hello användarens avdelning.</span><span class="sxs-lookup"><span data-stu-id="beba5-170">In hello **Department** textbox, type hello user’s department.</span></span>
  4. <span data-ttu-id="beba5-171">Som **typen**väljer **administratör**.</span><span class="sxs-lookup"><span data-stu-id="beba5-171">As **Employee Type**, select **Administrator**.</span></span>
  5. <span data-ttu-id="beba5-172">Klicka på **spara användarprofil**.</span><span class="sxs-lookup"><span data-stu-id="beba5-172">Click **Save User Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="beba5-173">Du kan använda något annat Replicon användarens konto skapas verktyg eller API: er som tillhandahålls av Replicon tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="beba5-173">You can use any other Replicon user account creation tools or APIs provided by Replicon tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="beba5-174">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="beba5-174">Assign users</span></span>
<span data-ttu-id="beba5-175">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="beba5-175">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="beba5-176">**tooassign användare tooReplicon utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="beba5-176">**tooassign users tooReplicon, perform hello following steps:**</span></span>

1. <span data-ttu-id="beba5-177">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="beba5-177">In hello Azure classic portal, create a test account.</span></span>

2. <span data-ttu-id="beba5-178">På hello **Replicon** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="beba5-178">On hello **Replicon** application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="beba5-179">![Tilldela användare](./media/active-directory-saas-replicon-tutorial/IC777809.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="beba5-179">![Assign users](./media/active-directory-saas-replicon-tutorial/IC777809.png "Assign users")</span></span>

3. <span data-ttu-id="beba5-180">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="beba5-180">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="beba5-181">![Ja](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="beba5-181">![Yes](./media/active-directory-saas-replicon-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="beba5-182">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="beba5-182">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="beba5-183">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="beba5-183">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

