---
title: "Självstudier: Azure Active Directory-integrering med Cisco Webex | Microsoft Docs"
description: "Lär dig hur du använder Cisco Webex med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: b44b1a5b3e988a51db3325ec8a181651fa84e768
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a><span data-ttu-id="35669-103">Självstudier: Azure Active Directory-integrering med Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="35669-103">Tutorial: Azure Active Directory Integration with Cisco Webex</span></span>
<span data-ttu-id="35669-104">Syftet med den här kursen är att visa integreringen av Azure och Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="35669-104">The objective of this tutorial is to show the integration of Azure and Cisco Webex.</span></span>  
<span data-ttu-id="35669-105">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="35669-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="35669-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="35669-106">A valid Azure subscription</span></span>
* <span data-ttu-id="35669-107">En Cisco-Webex-klient</span><span class="sxs-lookup"><span data-stu-id="35669-107">A Cisco Webex tenant</span></span>

<span data-ttu-id="35669-108">Den här kursen Azure AD-användare som du har tilldelat Cisco Webex kommer att kunna enkel inloggning till programmet på din Cisco Webex företagets plats (service provider initierade inloggning) eller med hjälp av den [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="35669-108">After completing this tutorial, the Azure AD users you have assigned to Cisco Webex will be able to single sign into the application at your Cisco Webex company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="35669-109">Det scenario som beskrivs i den här kursen består av följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="35669-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="35669-110">Aktivera application-integrering för Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="35669-110">Enabling the application integration for Cisco Webex</span></span>
* <span data-ttu-id="35669-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="35669-111">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="35669-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="35669-112">Configuring user provisioning</span></span>
* <span data-ttu-id="35669-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="35669-113">Assigning users</span></span>

<span data-ttu-id="35669-114">![Scenariot](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="35669-114">![Scenario](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-cisco-webex"></a><span data-ttu-id="35669-115">Aktivera application-integration för Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="35669-115">Enable the application integration for Cisco Webex</span></span>
<span data-ttu-id="35669-116">Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="35669-116">The objective of this section is to outline how to enable the application integration for Cisco Webex.</span></span>

<span data-ttu-id="35669-117">**Om du vill aktivera programmet-integrering för Cisco Webex utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="35669-117">**To enable the application integration for Cisco Webex, perform the following steps:**</span></span>

1. <span data-ttu-id="35669-118">I den klassiska Azure-portalen i det vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="35669-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="35669-119">![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="35669-119">![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="35669-120">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="35669-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="35669-121">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="35669-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="35669-122">![Program](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="35669-122">![Applications](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="35669-123">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="35669-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="35669-124">![Lägg till program](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="35669-124">![Add application](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="35669-125">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="35669-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="35669-126">![Lägga till ett program från gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="35669-126">![Add an application from gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="35669-127">I den **sökrutan**, typen **Cisco Webex**.</span><span class="sxs-lookup"><span data-stu-id="35669-127">In the **search box**, type **Cisco Webex**.</span></span>
   
   <span data-ttu-id="35669-128">![Programgalleriet](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="35669-128">![Application Gallery](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Application Gallery")</span></span>
7. <span data-ttu-id="35669-129">I resultatfönstret, Välj **Cisco Webex**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="35669-129">In the results pane, select **Cisco Webex**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="35669-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span><span class="sxs-lookup"><span data-stu-id="35669-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="35669-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="35669-131">Configure single sign-on</span></span>

<span data-ttu-id="35669-132">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till Cisco Webex med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="35669-132">The objective of this section is to outline how to enable users to authenticate to Cisco Webex with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="35669-133">Som en del av den här proceduren måste krävs du för att skapa en Base64-kodade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="35669-133">As part of this procedure, you are required to create a base-64 encoded certificate.</span></span> <span data-ttu-id="35669-134">Om du inte är bekant med den här proceduren, se [hur du konverterar ett binärt certifikat till en textfil](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="35669-134">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="35669-135">**Utför följande steg för att konfigurera enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="35669-135">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="35669-136">I den klassiska Azure-portalen på den **Cisco Webex** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="35669-136">In the Azure classic portal, on the **Cisco Webex** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="35669-137">![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="35669-137">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="35669-138">På den **hur vill du att användarna kan logga in på Cisco Webex** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="35669-138">On the **How would you like users to sign on to Cisco Webex** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="35669-139">![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="35669-139">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="35669-140">På den **konfigurera App-URL** , utför följande steg, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="35669-140">On the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
   <span data-ttu-id="35669-141">![Konfigurera app-URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="35669-141">![Configure app URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Configure app URL")</span></span>   
   1. <span data-ttu-id="35669-142">I den **vaken URL** textruta Skriv din Cisco Webex klient-URL (t.ex.: *http://contoso.webex.com*).</span><span class="sxs-lookup"><span data-stu-id="35669-142">In the **Sing On URL** textbox, type your Cisco Webex tenant URL (e.g.: *http://contoso.webex.com*).</span></span>
   2. <span data-ttu-id="35669-143">I den **Cisco Webex Reply URL** textruta typen din **Cisco Webex AssertionConsumerService URL** (t.ex.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).</span><span class="sxs-lookup"><span data-stu-id="35669-143">In the **Cisco Webex Reply URL** textbox, type your **Cisco Webex AssertionConsumerService URL** (e.g.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).</span></span>
4. <span data-ttu-id="35669-144">På den **Konfigurera enkel inloggning på Cisco Webex** att ladda ned ditt certifikat klickar du på **hämta certifikat**, och sedan spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="35669-144">On the **Configure single sign-on at Cisco Webex** page, to download your certificate, click **Download certificate**, and then save the certificate file on your computer.</span></span>
   
   <span data-ttu-id="35669-145">![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="35669-145">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="35669-146">Logga in på webbplatsen Cisco Webex företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="35669-146">In a different web browser window, log into your Cisco Webex company site as an administrator.</span></span>
6. <span data-ttu-id="35669-147">Klicka på menyn högst upp **Platsadministration**.</span><span class="sxs-lookup"><span data-stu-id="35669-147">In the menu on the top, click **Site Administration**.</span></span>
   
   <span data-ttu-id="35669-148">![Platsadministration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Platsadministration")</span><span class="sxs-lookup"><span data-stu-id="35669-148">![Site Administration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Site Administration")</span></span>
7. <span data-ttu-id="35669-149">I den **Hantera plats** klickar du på **SSO Configuration**.</span><span class="sxs-lookup"><span data-stu-id="35669-149">In the **Manage Site** section, click **SSO Configuration**.</span></span>
   
   <span data-ttu-id="35669-150">![Konfiguration av SSO](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="35669-150">![SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO Configuration")</span></span>
8. <span data-ttu-id="35669-151">Utför följande steg i avsnittet Federerad Web SSO-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="35669-151">In the Federated Web SSO Configuration section, perform the following steps:</span></span>
   
   <span data-ttu-id="35669-152">![Federerad enkel inloggning Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "federerad enkel inloggning konfiguration")</span><span class="sxs-lookup"><span data-stu-id="35669-152">![Federated SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federated SSO Configuration")</span></span>  
   1. <span data-ttu-id="35669-153">Från den **Federation-protokollet** väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="35669-153">From the **Federation Protocol** list, select **SAML 2.0**.</span></span>
   2. <span data-ttu-id="35669-154">Skapa en **Base64-kodad** fil från din hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="35669-154">Create a **Base-64 encoded** file from your downloaded certificate.</span></span>  
    >[!TIP]
    ><span data-ttu-id="35669-155">Mer information finns i [hur du konverterar ett binärt certifikat till en textfil](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="35669-155">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
    >  
   3. <span data-ttu-id="35669-156">Öppna din Base64-kodade certifikatet i anteckningar och kopiera innehållet i den.</span><span class="sxs-lookup"><span data-stu-id="35669-156">Open your base-64 encoded certificate in notepad, and then copy the content of it.</span></span>
   4. <span data-ttu-id="35669-157">Klicka på **importera SAML Metadata**, och klistra sedan in din Base64-kodade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="35669-157">Click **Import SAML Metadata**, and then paste your base-64 encoded certificate.</span></span>
   5. <span data-ttu-id="35669-158">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sidan, kopiera den **utfärdar-URL** värdet och klistrar in det i den **utfärdaren för SAML (IdP-ID)** textruta.</span><span class="sxs-lookup"><span data-stu-id="35669-158">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Issuer URL** value, and then paste it into the **Issuer for SAML (IdP ID)** textbox.</span></span>
   6. <span data-ttu-id="35669-159">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sidan, kopiera den **Remote inloggnings-URL** värdet och klistrar in det i den **inloggnings-URL för kunden SSO-Service** textruta.</span><span class="sxs-lookup"><span data-stu-id="35669-159">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Remote Login URL** value, and then paste it into the **Customer SSO Service Login URL** textbox.</span></span>
   7. <span data-ttu-id="35669-160">Från den **NameID Format** väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="35669-160">From the **NameID Format** list, select **Email address**.</span></span>
   8. <span data-ttu-id="35669-161">I den **AuthnContextClassRef** textruta typen **urn: oasis: namn: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="35669-161">In the **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span>
   9. <span data-ttu-id="35669-162">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sidan, kopiera den **Remote logga ut URL** värdet och klistrar in det i den **kunden URL för enkel inloggning logga ut** textruta.</span><span class="sxs-lookup"><span data-stu-id="35669-162">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Remote Logout URL** value, and then paste it into the **Customer SSO Service Logout URL** textbox.</span></span>
   10. <span data-ttu-id="35669-163">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="35669-163">Click **Update**.</span></span>
9. <span data-ttu-id="35669-164">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sidan Välj konfiguration för enkel inloggning bekräftelsen och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="35669-164">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, select the single sign-on configuration confirmation, and then click **Complete**.</span></span>
   
   <span data-ttu-id="35669-165">![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="35669-165">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="35669-166">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="35669-166">Configure user provisioning</span></span>

<span data-ttu-id="35669-167">För att aktivera Azure AD-användare att logga in på Cisco Webex etableras de i Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="35669-167">In order to enable Azure AD users to log into Cisco Webex, they must be provisioned into Cisco Webex.</span></span>  

* <span data-ttu-id="35669-168">Cisco Webex är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="35669-168">In the case of Cisco Webex, provisioning is a manual task.</span></span>

<span data-ttu-id="35669-169">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="35669-169">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="35669-170">Logga in på ditt **Cisco Webex** klient.</span><span class="sxs-lookup"><span data-stu-id="35669-170">Log in to your **Cisco Webex** tenant.</span></span>
2. <span data-ttu-id="35669-171">Gå till **hantera användare \> lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="35669-171">Go to **Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="35669-172">![Lägg till användare](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="35669-172">![Add users](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Add users")</span></span>
3. <span data-ttu-id="35669-173">Utför följande steg på avsnittet Lägg till användare:</span><span class="sxs-lookup"><span data-stu-id="35669-173">On the Add User section, perform the following steps:</span></span>
   
   <span data-ttu-id="35669-174">![Lägg till användare](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Lägg till användare")</span><span class="sxs-lookup"><span data-stu-id="35669-174">![Add user](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Add user")</span></span>   
   1. <span data-ttu-id="35669-175">Som **kontotyp**väljer **värden**.</span><span class="sxs-lookup"><span data-stu-id="35669-175">As **Account Type**, select **Host**.</span></span>
   2. <span data-ttu-id="35669-176">Ange information för en befintlig Azure AD-användare i följande textrutor: **förnamn, efternamn**, **användarnamn**, **e-post**, **lösenord**, **Bekräfta lösenord**.</span><span class="sxs-lookup"><span data-stu-id="35669-176">Type the information of an existing Azure AD user into the following textboxes: **First name, Last name**, **User name**, **Email**, **Password**, **Confirm Password**.</span></span>
   3. <span data-ttu-id="35669-177">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="35669-177">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="35669-178">Du kan använda något annat Cisco Webex användarens konto skapas verktyg eller API: er som tillhandahålls av Cisco Webex etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="35669-178">You can use any other Cisco Webex user account creation tools or APIs provided by Cisco Webex to provision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="35669-179">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="35669-179">Assign users</span></span>
<span data-ttu-id="35669-180">Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.</span><span class="sxs-lookup"><span data-stu-id="35669-180">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="35669-181">**Utför följande steg om du vill tilldela Cisco Webex användare:**</span><span class="sxs-lookup"><span data-stu-id="35669-181">**To assign users to Cisco Webex, perform the following steps:**</span></span>

1. <span data-ttu-id="35669-182">Skapa ett testkonto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="35669-182">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="35669-183">På den **Cisco Webex** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="35669-183">On the **Cisco Webex** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="35669-184">![Tilldela användare](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="35669-184">![Assign users](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Assign users")</span></span>
3. <span data-ttu-id="35669-185">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.</span><span class="sxs-lookup"><span data-stu-id="35669-185">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="35669-186">![Ja](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="35669-186">![Yes](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="35669-187">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="35669-187">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="35669-188">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="35669-188">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

