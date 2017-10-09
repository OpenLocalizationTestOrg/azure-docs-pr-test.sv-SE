---
title: "Självstudier: Azure Active Directory-integrering med Salesforce Sandbox | Microsoft Docs"
description: "Lär dig hur toouse Salesforce Sandbox med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: deccd6a07b57c8ba56b7e1c3f3ab311813ca017b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="32e69-103">Självstudier: Azure Active Directory-integrering med Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="32e69-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="32e69-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="32e69-104">hello objective of this tutorial is tooshow hello integration of Azure and Salesforce Sandbox.</span></span>  

>[!TIP]
><span data-ttu-id="32e69-105">Feedback, finns hello [Azure supportsidan](http://go.microsoft.com/fwlink/?LinkId=521878).</span><span class="sxs-lookup"><span data-stu-id="32e69-105">For feedback, see hello [Azure support page](http://go.microsoft.com/fwlink/?LinkId=521878).</span></span> 
> 

<span data-ttu-id="32e69-106">Sandboxar ger du hello möjlighet toocreate flera kopior av din organisation i olika miljöer för olika syften, till exempel utveckling, testning och utbildning utan säkerhetsfunktionerna hello data och program i produktionsmiljön Salesforce organisation.</span><span class="sxs-lookup"><span data-stu-id="32e69-106">Sandboxes give you hello ability toocreate multiple copies of your organization in separate environments for a variety of purposes, such as development, testing, and training, without compromising hello data and applications in your Salesforce production organization.</span></span>  

<span data-ttu-id="32e69-107">Mer information finns i [Sandbox-översikt](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span><span class="sxs-lookup"><span data-stu-id="32e69-107">For more details, see [Sandbox Overview](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span></span>

<span data-ttu-id="32e69-108">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="32e69-108">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="32e69-109">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="32e69-109">A valid Azure subscription</span></span>
* <span data-ttu-id="32e69-110">En sandbox i Salesforce.com</span><span class="sxs-lookup"><span data-stu-id="32e69-110">A sandbox in Salesforce.com</span></span>

<span data-ttu-id="32e69-111">Om du inte har en giltig sandbox i Salesforce.com ännu, måste toocontact Salesforce.</span><span class="sxs-lookup"><span data-stu-id="32e69-111">If you don’t have a valid sandbox in Salesforce.com yet, you need toocontact Salesforce.</span></span>

<span data-ttu-id="32e69-112">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="32e69-112">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="32e69-113">Att aktivera programmet hello-integrering för Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="32e69-113">Enabling hello application integration for Salesforce Sandbox</span></span>
2. <span data-ttu-id="32e69-114">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="32e69-114">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="32e69-115">Aktivera din domän</span><span class="sxs-lookup"><span data-stu-id="32e69-115">Enabling your domain</span></span>
4. <span data-ttu-id="32e69-116">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="32e69-116">Configuring user provisioning</span></span>
5. <span data-ttu-id="32e69-117">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="32e69-117">Assigning users</span></span>

<span data-ttu-id="32e69-118">![Scenariot](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="32e69-118">![Scenario](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-salesforce-sandbox"></a><span data-ttu-id="32e69-119">Aktivera hello programmet integration för Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="32e69-119">Enable hello application integration for Salesforce Sandbox</span></span>
<span data-ttu-id="32e69-120">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Salesforce begränsat läge.</span><span class="sxs-lookup"><span data-stu-id="32e69-120">hello objective of this section is toooutline how tooenable hello application integration for Salesforce sandbox.</span></span>

<span data-ttu-id="32e69-121">**tooenable hello programintegrationstyp för Salesforce sandbox utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="32e69-121">**tooenable hello application integration for Salesforce sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="32e69-122">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="32e69-122">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="32e69-123">![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="32e69-123">![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="32e69-124">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="32e69-124">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="32e69-125">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="32e69-125">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="32e69-126">![Program](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="32e69-126">![Applications](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="32e69-127">tooopen hello **Programgalleriet**, klickar du på **Lägg till en App**, och klicka sedan på **lägga till ett program för min organisation toouse**.</span><span class="sxs-lookup"><span data-stu-id="32e69-127">tooopen hello **Application Gallery**, click **Add An App**, and then click **Add an application for my organization toouse**.</span></span>
   
   <span data-ttu-id="32e69-128">![Vad vill du vill toodo? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Vad vill du vill toodo?")</span><span class="sxs-lookup"><span data-stu-id="32e69-128">![What do you want toodo?](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "What do you want toodo?")</span></span>
5. <span data-ttu-id="32e69-129">I hello **sökrutan**, typen **Salesforce Sandbox**.</span><span class="sxs-lookup"><span data-stu-id="32e69-129">In hello **search box**, type **Salesforce Sandbox**.</span></span>
   
   <span data-ttu-id="32e69-130">![Programgalleriet](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="32e69-130">![Application Gallery](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Application Gallery")</span></span>
6. <span data-ttu-id="32e69-131">I resultatfönstret hello väljer **Salesforce Sandbox**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="32e69-131">In hello results pane, select **Salesforce Sandbox**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="32e69-132">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")</span><span class="sxs-lookup"><span data-stu-id="32e69-132">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")</span></span>
   
## <a name="configur-single-sign-on-sso"></a><span data-ttu-id="32e69-133">Configur enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="32e69-133">Configur single sign-on (SSO)</span></span>

<span data-ttu-id="32e69-134">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooSalesforce med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="32e69-134">hello objective of this section is toooutline how tooenable users tooauthenticate tooSalesforce with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="32e69-135">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="32e69-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="32e69-136">I hello klassiska Azure-portalen på hello **Salesforce Sandbox** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="32e69-136">In hello Azure classic portal, on hello **Salesforce Sandbox** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="32e69-137">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="32e69-137">![Configure single sign-on](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="32e69-138">På hello **hur skulle du som användare toosign på tooSalesforce Sandbox** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="32e69-138">On hello **How would you like users toosign on tooSalesforce Sandbox** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="32e69-139">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")</span><span class="sxs-lookup"><span data-stu-id="32e69-139">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")</span></span>
3. <span data-ttu-id="32e69-140">På hello **konfigurera App-URL** i hello sidan **logga URL** textruta Skriv URL: en med hjälp av följande mönster hello `http://company.my.salesforce.com`, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="32e69-140">On hello **Configure App URL** page, in hello **Sign On URL** textbox, type your URL using hello following pattern `http://company.my.salesforce.com`, and then click **Next**.</span></span>
   
   <span data-ttu-id="32e69-141">![Konfigurera App-URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="32e69-141">![Configure App URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Configure App URL")</span></span>
4. <span data-ttu-id="32e69-142">Om du redan har konfigurerat enkel inloggning för en annan Salesforce Sandbox-instans i katalogen, så du måste också konfigurera hello **identifierare** toohave hello samma värde som hello **inloggning URL**.</span><span class="sxs-lookup"><span data-stu-id="32e69-142">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure hello **Identifier** toohave hello same value as hello **Sign on URL**.</span></span> 
 * <span data-ttu-id="32e69-143">Hej **identifierare** fältet finns genom att kontrollera hello **visa avancerade inställningar** kryssruta på hello **konfigurera App-URL** sidan hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="32e69-143">hello **Identifier** field can be found by checking hello **Show advanced settings** checkbox on hello **Configure App URL** page of hello dialog.</span></span>
5. <span data-ttu-id="32e69-144">På hello **Konfigurera enkel inloggning på Salesforce Sandbox** klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="32e69-144">On hello **Configure single sign-on at Salesforce Sandbox** page, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
   <span data-ttu-id="32e69-145">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="32e69-145">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="32e69-146">Logga in på ditt Salesforce-sandbox som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="32e69-146">In a different web browser window, log into your Salesforce sandbox as an administrator.</span></span>
7. <span data-ttu-id="32e69-147">Hello-menyn överst hello **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="32e69-147">In hello menu on hello top, click **Setup**.</span></span>
   
   <span data-ttu-id="32e69-148">![Installationsprogrammet](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="32e69-148">![Setup](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")</span></span>
8. <span data-ttu-id="32e69-149">Hello navigeringsfönstret hello vänster, klicka på **säkerhetsåtgärder**, och klicka sedan på **inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="32e69-149">In hello navigation pane on hello left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="32e69-150">![Enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="32e69-150">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On Settings")</span></span>
9. <span data-ttu-id="32e69-151">Utför följande hello på hello inställningar för enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="32e69-151">On hello Single Sign-On Settings section, perform hello following steps:</span></span>
   
   <span data-ttu-id="32e69-152">![Enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="32e69-152">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On Settings")</span></span>  
 1.  <span data-ttu-id="32e69-153">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="32e69-153">Select **SAML Enabled**.</span></span> 
 2.  <span data-ttu-id="32e69-154">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="32e69-154">Click **New**.</span></span>
10. <span data-ttu-id="32e69-155">Utför följande hello på hello SAML enkel inloggning inställningar:</span><span class="sxs-lookup"><span data-stu-id="32e69-155">On hello SAML Single Sign-On Settings section, perform hello following steps:</span></span>
    
    <span data-ttu-id="32e69-156">![SAML enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="32e69-156">![SAML Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On Settings")</span></span>  
 1. <span data-ttu-id="32e69-157">I textrutan hello, skriver hello namnet på hello-konfiguration (t.ex.: *SPSSOWAAD\_Test*).</span><span class="sxs-lookup"><span data-stu-id="32e69-157">In hello Name textbox, type hello name of hello configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 
 2. <span data-ttu-id="32e69-158">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Salesforce Sandbox** dialog sida, kopiera hello **utfärdar-URL** värdet och klistrar in den i hello **utfärdaren**textruta.</span><span class="sxs-lookup"><span data-stu-id="32e69-158">In hello Azure classic portal, on hello **Configure single sign-on at Salesforce Sandbox** dialogue page, copy hello **Issuer URL** value, and then paste it into hello **Issuer** textbox.</span></span>
 3. <span data-ttu-id="32e69-159">I hello **enhets-Id** textruta typen **https://test.salesforce.com** om det är hello första Salesforce Sandbox-instans som du lägger till tooyour directory.</span><span class="sxs-lookup"><span data-stu-id="32e69-159">In hello **Entity Id** textbox, type **https://test.salesforce.com** if this is hello first Salesforce Sandbox instance that you are adding tooyour directory.</span></span> <span data-ttu-id="32e69-160">Om du redan har lagt till en instans av Salesforce begränsat, och därefter för hello **enhets-ID** typen i hello **logga URL**, som ska vara i formatet:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="32e69-160">If you have already added an instance of Salesforce Sandbox, then for hello **Entity ID** type in hello **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>   
 4. <span data-ttu-id="32e69-161">Klicka på **Bläddra** tooupload hello hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="32e69-161">Click **Browse** tooupload hello downloaded certificate.</span></span>  
 5. <span data-ttu-id="32e69-162">Som **SAML identitetstyp**väljer **Assertion innehåller hello Federation ID från hello användarobjektet**.</span><span class="sxs-lookup"><span data-stu-id="32e69-162">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span> 
 6. <span data-ttu-id="32e69-163">Som **SAML identitet plats**väljer **identitet är i hello NameIdentifier elementet i hello ämne instruktionen**.</span><span class="sxs-lookup"><span data-stu-id="32e69-163">As **SAML Identity Location**, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>
 7. <span data-ttu-id="32e69-164">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Salesforce Sandbox** dialog sida, kopiera hello **Remote inloggnings-URL** värdet och klistrar in den i hello **identitetsleverantören. Inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="32e69-164">In hello Azure classic portal, on hello **Configure single sign-on at Salesforce Sandbox** dialogue page, copy hello **Remote Login URL** value, and then paste it into hello **Identity Provider Login URL** textbox.</span></span> 
 8. <span data-ttu-id="32e69-165">SFDC har inte stöd för SAML logga ut.</span><span class="sxs-lookup"><span data-stu-id="32e69-165">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="32e69-166">Som en tillfällig lösning kan du klistra in 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' till hello **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="32e69-166">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into hello **Identity Provider Logout URL** textbox.</span></span>
 9. <span data-ttu-id="32e69-167">Som **providern initierade begära Tjänstbindning**väljer **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="32e69-167">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 
 10. <span data-ttu-id="32e69-168">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="32e69-168">Click **Save**.</span></span>
11. <span data-ttu-id="32e69-169">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="32e69-169">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="32e69-170">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="32e69-170">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Configure Single Sign-On")</span></span>

## <a name="enable-your-domain"></a><span data-ttu-id="32e69-171">Aktivera din domän</span><span class="sxs-lookup"><span data-stu-id="32e69-171">Enable your domain</span></span>
<span data-ttu-id="32e69-172">Det här avsnittet förutsätter att du redan har skapat en domän.</span><span class="sxs-lookup"><span data-stu-id="32e69-172">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="32e69-173">Mer information finns i [definierar domännamn](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="32e69-173">For more details, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="32e69-174">**tooenable din domän, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="32e69-174">**tooenable your domain, perform hello following steps:**</span></span>

1. <span data-ttu-id="32e69-175">Hello vänstra navigeringsfönstret, klicka på **domänhantering**, och klicka sedan på **min domän.**</span><span class="sxs-lookup"><span data-stu-id="32e69-175">In hello left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
   <span data-ttu-id="32e69-176">![Min domän](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="32e69-176">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "My Domain")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="32e69-177">Kontrollera att din domän har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="32e69-177">Please make sure that your domain has been configured correctly.</span></span> 
   > 
2. <span data-ttu-id="32e69-178">I hello **sidan inloggningsinställningar** klickar du på **redigera**, sedan som **Autentiseringstjänsten**väljer hello namnet på hello SAML enkel inloggning inställningen från hello tidigare avsnittet och klicka slutligen på **spara**.</span><span class="sxs-lookup"><span data-stu-id="32e69-178">In hello **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select hello name of hello SAML Single Sign-On Setting from hello previous section, and finally click **Save**.</span></span>
   
   <span data-ttu-id="32e69-179">![Min domän](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="32e69-179">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "My Domain")</span></span>

<span data-ttu-id="32e69-180">Så snart du har en domän som har konfigurerats, bör användarna använda hello domän URL toologin toohello Salesforce sandbox.</span><span class="sxs-lookup"><span data-stu-id="32e69-180">As soon as you have a domain configured, your users should use hello domain URL toologin toohello Salesforce sandbox.</span></span>  

<span data-ttu-id="32e69-181">tooget hello värdet för hello URL på hello SSO profil du har skapat i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="32e69-181">tooget hello value of hello URL, click hello SSO profile you have created in hello previous section.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="32e69-182">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="32e69-182">Configure user provisioning</span></span>
<span data-ttu-id="32e69-183">hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooSalesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="32e69-183">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooSalesforce Sandbox.</span></span>

<span data-ttu-id="32e69-184">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="32e69-184">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="32e69-185">Välj namn-tooexpand användare-menyn i hello Salesforce-portalen i hello övre navigeringsfältet:</span><span class="sxs-lookup"><span data-stu-id="32e69-185">In hello Salesforce portal, in hello top navigation bar, select your name tooexpand your user menu:</span></span>
   
   <span data-ttu-id="32e69-186">![Mina inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Mina inställningar")</span><span class="sxs-lookup"><span data-stu-id="32e69-186">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "My Settings")</span></span>
2. <span data-ttu-id="32e69-187">Användare-menyn, Välj **Mina inställningar** tooopen din **Mina inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="32e69-187">From your user menu, select **My Settings** tooopen your **My Settings** page.</span></span>
3. <span data-ttu-id="32e69-188">Hello vänster klickar du på **personliga** tooexpand hello personliga avsnittet och klicka sedan på **Återställ mina säkerhetstoken**:</span><span class="sxs-lookup"><span data-stu-id="32e69-188">In hello left pane, click **Personal** tooexpand hello Personal section, and then click **Reset My Security Token**:</span></span>
   
   <span data-ttu-id="32e69-189">![Mina inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Mina inställningar")</span><span class="sxs-lookup"><span data-stu-id="32e69-189">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "My Settings")</span></span>
4. <span data-ttu-id="32e69-190">På hello **Återställ mina säkerhetstoken** klickar du på **återställa säkerhetstoken** toorequest ett e-postmeddelande som innehåller din Salesforce.com-säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="32e69-190">On hello **Reset My Security Token** page, click **Reset Security Token** toorequest an email that contains your Salesforce.com security token.</span></span>
   
   <span data-ttu-id="32e69-191">![Ny Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "ny Token")</span><span class="sxs-lookup"><span data-stu-id="32e69-191">![New Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "New Token")</span></span>
5. <span data-ttu-id="32e69-192">Kontrollera din inkorg för e-post från Salesforce.com med ”**salesforce.com.com säkerhet bekräftelse**” som ämne.</span><span class="sxs-lookup"><span data-stu-id="32e69-192">Check your email inbox for an email from Salesforce.com with “**salesforce.com.com security confirmation**” as subject.</span></span>
6. <span data-ttu-id="32e69-193">Granska den här e-post och kopiera hello säkerhet token-värde.</span><span class="sxs-lookup"><span data-stu-id="32e69-193">Review this email and copy hello security token value.</span></span>
7. <span data-ttu-id="32e69-194">I hello klassiska Azure-portalen på hello **salesforce Sandbox** integreringssidan för programmet, klickar du på **konfigurera användaretablering** tooopen hello **konfigurera Användaretablering**dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="32e69-194">In hello Azure classic portal, on hello **salesforce Sandbox** application integration page, click **Configure user provisioning** tooopen hello **Configure User Provisioning** dialog.</span></span>
   
   <span data-ttu-id="32e69-195">![Konfigurera användaretablering](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "konfigurera användaretablering")</span><span class="sxs-lookup"><span data-stu-id="32e69-195">![Configure user provisioning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Configure user provisioning")</span></span>
8. <span data-ttu-id="32e69-196">På hello **ange ditt Salesforce Sandbox autentiseringsuppgifter tooenable automatisk användaretablering** anger hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="32e69-196">On hello **Enter your Salesforce Sandbox credentials tooenable automatic user provisioning** page, provide hello following configuration settings:</span></span>
   
   <span data-ttu-id="32e69-197">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")</span><span class="sxs-lookup"><span data-stu-id="32e69-197">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")</span></span>   
 1. <span data-ttu-id="32e69-198">I hello **Salesforce Sandbox administratörsanvändarnamnet** textruta typen en Salesforce sandbox kontonamn som har hello **systemadministratören** profil i Salesforce.com som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="32e69-198">In hello **Salesforce Sandbox Admin User Name** textbox, type a Salesforce sandbox account name that has hello **System Administrator** profile in Salesforce.com assigned.</span></span>
 2. <span data-ttu-id="32e69-199">I hello **Salesforce Sandbox adminlösenord** textruta typen hello lösenordet för kontot.</span><span class="sxs-lookup"><span data-stu-id="32e69-199">In hello **Salesforce Sandbox Admin Password** textbox, type hello password for this account.</span></span>
 3. <span data-ttu-id="32e69-200">I hello **användartoken säkerhet** textruta klistra in hello säkerhet token-värde.</span><span class="sxs-lookup"><span data-stu-id="32e69-200">In hello **User Security Token** textbox, paste hello security token value.</span></span>
 4. <span data-ttu-id="32e69-201">Klicka på **verifiera** tooverify din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="32e69-201">Click **Validate** tooverify your configuration.</span></span>
 5. <span data-ttu-id="32e69-202">Klicka på hello **nästa** knappen tooopen hello **bekräftelse** sidan.</span><span class="sxs-lookup"><span data-stu-id="32e69-202">Click hello **Next** button tooopen hello **Confirmation** page.</span></span>
9. <span data-ttu-id="32e69-203">På hello **bekräftelse** klickar du på **Slutför** toosave din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="32e69-203">On hello **Confirmation** page, click **Complete** toosave your configuration.</span></span>
   
## <a name="assigning-users"></a><span data-ttu-id="32e69-204">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="32e69-204">Assigning users</span></span>

<span data-ttu-id="32e69-205">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="32e69-205">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="32e69-206">**tooassign användare tooSalesforce Sandbox, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="32e69-206">**tooassign users tooSalesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="32e69-207">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="32e69-207">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="32e69-208">På hello ** Salesforce Sandbox ** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="32e69-208">On hello **Salesforce Sandbox **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="32e69-209">![Tilldela användare](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="32e69-209">![Assign users](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Assign users")</span></span>
3. <span data-ttu-id="32e69-210">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="32e69-210">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="32e69-211">![Ja](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="32e69-211">![Yes](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="32e69-212">Nu bör du vänta i 10 minuter och kontrollera att hello kontot har synkroniserat tooSalesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="32e69-212">You should now wait for 10 minutes and verify that hello account has been synchronized tooSalesforce Sandbox.</span></span>

<span data-ttu-id="32e69-213">Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="32e69-213">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="32e69-214">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="32e69-214">For more details about hello Access Panel, see [Introduction toohello Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

