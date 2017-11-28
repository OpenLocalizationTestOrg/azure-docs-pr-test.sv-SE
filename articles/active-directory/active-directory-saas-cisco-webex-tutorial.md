---
title: "Självstudier: Azure Active Directory-integrering med Cisco Webex | Microsoft Docs"
description: "Lär dig hur toouse Cisco Webex med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: 9fc11e58a7acaa6fbfb32eeccbfbf85984950e67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a><span data-ttu-id="31210-103">Självstudier: Azure Active Directory-integrering med Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="31210-103">Tutorial: Azure Active Directory Integration with Cisco Webex</span></span>
<span data-ttu-id="31210-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="31210-104">hello objective of this tutorial is tooshow hello integration of Azure and Cisco Webex.</span></span>  
<span data-ttu-id="31210-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="31210-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="31210-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="31210-106">A valid Azure subscription</span></span>
* <span data-ttu-id="31210-107">En Cisco-Webex-klient</span><span class="sxs-lookup"><span data-stu-id="31210-107">A Cisco Webex tenant</span></span>

<span data-ttu-id="31210-108">Den här kursen hello Azure AD-användare som du har tilldelat tooCisco Webex blir kan toosingle logga till hello program på din Cisco Webex företagets plats (service provider initierade inloggning) eller använda hello [introduktion toohello Åtkomst till panelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="31210-108">After completing this tutorial, hello Azure AD users you have assigned tooCisco Webex will be able toosingle sign into hello application at your Cisco Webex company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="31210-109">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="31210-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="31210-110">Aktivera programmet hello-integrering för Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="31210-110">Enabling hello application integration for Cisco Webex</span></span>
* <span data-ttu-id="31210-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="31210-111">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="31210-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="31210-112">Configuring user provisioning</span></span>
* <span data-ttu-id="31210-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="31210-113">Assigning users</span></span>

<span data-ttu-id="31210-114">![Scenariot](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="31210-114">![Scenario](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-cisco-webex"></a><span data-ttu-id="31210-115">Aktivera hello programmet integration för Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="31210-115">Enable hello application integration for Cisco Webex</span></span>
<span data-ttu-id="31210-116">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="31210-116">hello objective of this section is toooutline how tooenable hello application integration for Cisco Webex.</span></span>

<span data-ttu-id="31210-117">**tooenable hello programintegrationstyp för Cisco Webex utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="31210-117">**tooenable hello application integration for Cisco Webex, perform hello following steps:**</span></span>

1. <span data-ttu-id="31210-118">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="31210-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="31210-119">![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="31210-119">![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="31210-120">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="31210-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="31210-121">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="31210-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="31210-122">![Program](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="31210-122">![Applications](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="31210-123">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="31210-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="31210-124">![Lägg till program](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="31210-124">![Add application](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="31210-125">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="31210-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="31210-126">![Lägga till ett program från gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="31210-126">![Add an application from gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="31210-127">I hello **sökrutan**, typen **Cisco Webex**.</span><span class="sxs-lookup"><span data-stu-id="31210-127">In hello **search box**, type **Cisco Webex**.</span></span>
   
   <span data-ttu-id="31210-128">![Programgalleriet](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="31210-128">![Application Gallery](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Application Gallery")</span></span>
7. <span data-ttu-id="31210-129">I resultatfönstret hello väljer **Cisco Webex**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="31210-129">In hello results pane, select **Cisco Webex**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="31210-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span><span class="sxs-lookup"><span data-stu-id="31210-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="31210-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="31210-131">Configure single sign-on</span></span>

<span data-ttu-id="31210-132">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooCisco Webex med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="31210-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooCisco Webex with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="31210-133">Som en del av den här proceduren är obligatoriska toocreate en Base64-kodade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="31210-133">As part of this procedure, you are required toocreate a base-64 encoded certificate.</span></span> <span data-ttu-id="31210-134">Om du inte är bekant med den här proceduren, se [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="31210-134">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="31210-135">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="31210-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="31210-136">I hello klassiska Azure-portalen på hello **Cisco Webex** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="31210-136">In hello Azure classic portal, on hello **Cisco Webex** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="31210-137">![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="31210-137">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="31210-138">På hello **hur skulle du som användare toosign på tooCisco Webex** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="31210-138">On hello **How would you like users toosign on tooCisco Webex** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="31210-139">![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="31210-139">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="31210-140">På hello **konfigurera App-URL** , utför följande steg hello, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="31210-140">On hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
   <span data-ttu-id="31210-141">![Konfigurera app-URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="31210-141">![Configure app URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Configure app URL")</span></span>   
   1. <span data-ttu-id="31210-142">I hello **vaken URL** textruta Skriv din Cisco Webex klient-URL (t.ex.: *http://contoso.webex.com*).</span><span class="sxs-lookup"><span data-stu-id="31210-142">In hello **Sing On URL** textbox, type your Cisco Webex tenant URL (e.g.: *http://contoso.webex.com*).</span></span>
   2. <span data-ttu-id="31210-143">I hello **Cisco Webex Reply URL** textruta typen din **Cisco Webex AssertionConsumerService URL** (t.ex.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company* ).</span><span class="sxs-lookup"><span data-stu-id="31210-143">In hello **Cisco Webex Reply URL** textbox, type your **Cisco Webex AssertionConsumerService URL** (e.g.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).</span></span>
4. <span data-ttu-id="31210-144">På hello **Konfigurera enkel inloggning på Cisco Webex** sida, toodownload ditt certifikat klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="31210-144">On hello **Configure single sign-on at Cisco Webex** page, toodownload your certificate, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
   <span data-ttu-id="31210-145">![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="31210-145">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="31210-146">Logga in på webbplatsen Cisco Webex företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="31210-146">In a different web browser window, log into your Cisco Webex company site as an administrator.</span></span>
6. <span data-ttu-id="31210-147">Hello-menyn överst hello **Platsadministration**.</span><span class="sxs-lookup"><span data-stu-id="31210-147">In hello menu on hello top, click **Site Administration**.</span></span>
   
   <span data-ttu-id="31210-148">![Platsadministration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Platsadministration")</span><span class="sxs-lookup"><span data-stu-id="31210-148">![Site Administration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Site Administration")</span></span>
7. <span data-ttu-id="31210-149">I hello **Hantera plats** klickar du på **SSO Configuration**.</span><span class="sxs-lookup"><span data-stu-id="31210-149">In hello **Manage Site** section, click **SSO Configuration**.</span></span>
   
   <span data-ttu-id="31210-150">![Konfiguration av SSO](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="31210-150">![SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO Configuration")</span></span>
8. <span data-ttu-id="31210-151">I hello Federerad Web SSO konfigurationsavsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="31210-151">In hello Federated Web SSO Configuration section, perform hello following steps:</span></span>
   
   <span data-ttu-id="31210-152">![Federerad enkel inloggning Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "federerad enkel inloggning konfiguration")</span><span class="sxs-lookup"><span data-stu-id="31210-152">![Federated SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federated SSO Configuration")</span></span>  
   1. <span data-ttu-id="31210-153">Från hello **Federation-protokollet** väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="31210-153">From hello **Federation Protocol** list, select **SAML 2.0**.</span></span>
   2. <span data-ttu-id="31210-154">Skapa en **Base64-kodad** fil från din hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="31210-154">Create a **Base-64 encoded** file from your downloaded certificate.</span></span>  
    >[!TIP]
    ><span data-ttu-id="31210-155">Mer information finns i [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="31210-155">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
    >  
   3. <span data-ttu-id="31210-156">Öppna din Base64-kodade certifikatet i anteckningar och kopiera hello innehållet i den.</span><span class="sxs-lookup"><span data-stu-id="31210-156">Open your base-64 encoded certificate in notepad, and then copy hello content of it.</span></span>
   4. <span data-ttu-id="31210-157">Klicka på **importera SAML Metadata**, och klistra sedan in din Base64-kodade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="31210-157">Click **Import SAML Metadata**, and then paste your base-64 encoded certificate.</span></span>
   5. <span data-ttu-id="31210-158">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sida, kopiera hello **utfärdar-URL** värdet och klistrar in den i hello **utfärdaren för SAML (IdP-ID)** textruta.</span><span class="sxs-lookup"><span data-stu-id="31210-158">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Issuer URL** value, and then paste it into hello **Issuer for SAML (IdP ID)** textbox.</span></span>
   6. <span data-ttu-id="31210-159">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sida, kopiera hello **Remote inloggnings-URL** värdet och klistrar in den i hello **kunden tjänstinloggning för enkel inloggning URL: en** textruta.</span><span class="sxs-lookup"><span data-stu-id="31210-159">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Remote Login URL** value, and then paste it into hello **Customer SSO Service Login URL** textbox.</span></span>
   7. <span data-ttu-id="31210-160">Från hello **NameID Format** väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="31210-160">From hello **NameID Format** list, select **Email address**.</span></span>
   8. <span data-ttu-id="31210-161">I hello **AuthnContextClassRef** textruta typen **urn: oasis: namn: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="31210-161">In hello **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span>
   9. <span data-ttu-id="31210-162">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sida, kopiera hello **Remote logga ut URL** värdet och klistrar in den i hello **kunden SSO-Service logga ut URL: en** textruta.</span><span class="sxs-lookup"><span data-stu-id="31210-162">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Remote Logout URL** value, and then paste it into hello **Customer SSO Service Logout URL** textbox.</span></span>
   10. <span data-ttu-id="31210-163">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="31210-163">Click **Update**.</span></span>
9. <span data-ttu-id="31210-164">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sidan Välj hello konfiguration för enkel inloggning bekräftelse och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="31210-164">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, select hello single sign-on configuration confirmation, and then click **Complete**.</span></span>
   
   <span data-ttu-id="31210-165">![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="31210-165">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="31210-166">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="31210-166">Configure user provisioning</span></span>

<span data-ttu-id="31210-167">I ordning tooenable Azure AD-användare toolog i Cisco Webex, måste de etableras i Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="31210-167">In order tooenable Azure AD users toolog into Cisco Webex, they must be provisioned into Cisco Webex.</span></span>  

* <span data-ttu-id="31210-168">Cisco Webex hello gäller är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="31210-168">In hello case of Cisco Webex, provisioning is a manual task.</span></span>

<span data-ttu-id="31210-169">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="31210-169">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="31210-170">Logga in tooyour **Cisco Webex** klient.</span><span class="sxs-lookup"><span data-stu-id="31210-170">Log in tooyour **Cisco Webex** tenant.</span></span>
2. <span data-ttu-id="31210-171">Gå för**hantera användare \> Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="31210-171">Go too**Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="31210-172">![Lägg till användare](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="31210-172">![Add users](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Add users")</span></span>
3. <span data-ttu-id="31210-173">Utför följande hello på hello Lägg till användare:</span><span class="sxs-lookup"><span data-stu-id="31210-173">On hello Add User section, perform hello following steps:</span></span>
   
   <span data-ttu-id="31210-174">![Lägg till användare](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Lägg till användare")</span><span class="sxs-lookup"><span data-stu-id="31210-174">![Add user](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Add user")</span></span>   
   1. <span data-ttu-id="31210-175">Som **kontotyp**väljer **värden**.</span><span class="sxs-lookup"><span data-stu-id="31210-175">As **Account Type**, select **Host**.</span></span>
   2. <span data-ttu-id="31210-176">Skriv hello information för en befintlig Azure AD-användare i hello följande textrutor: **förnamn, efternamn**, **användarnamn**, **e-post**, **lösenord**, **Bekräfta lösenord**.</span><span class="sxs-lookup"><span data-stu-id="31210-176">Type hello information of an existing Azure AD user into hello following textboxes: **First name, Last name**, **User name**, **Email**, **Password**, **Confirm Password**.</span></span>
   3. <span data-ttu-id="31210-177">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="31210-177">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="31210-178">Du kan använda något annat Cisco Webex användarens konto skapas verktyg eller API: er som tillhandahålls av Cisco Webex tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="31210-178">You can use any other Cisco Webex user account creation tools or APIs provided by Cisco Webex tooprovision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="31210-179">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="31210-179">Assign users</span></span>
<span data-ttu-id="31210-180">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="31210-180">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="31210-181">**tooassign användare tooCisco Webex, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="31210-181">**tooassign users tooCisco Webex, perform hello following steps:**</span></span>

1. <span data-ttu-id="31210-182">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="31210-182">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="31210-183">På hello **Cisco Webex** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="31210-183">On hello **Cisco Webex** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="31210-184">![Tilldela användare](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="31210-184">![Assign users](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Assign users")</span></span>
3. <span data-ttu-id="31210-185">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="31210-185">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="31210-186">![Ja](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="31210-186">![Yes](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="31210-187">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="31210-187">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="31210-188">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="31210-188">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

