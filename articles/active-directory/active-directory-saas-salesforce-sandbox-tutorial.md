---
title: "Självstudier: Azure Active Directory-integrering med Salesforce Sandbox | Microsoft Docs"
description: "Lär dig hur du använder Salesforce Sandbox med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!."
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
ms.openlocfilehash: 32835e79188806bb2ff319eea23b1b52ab585ab1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="a0058-103">Självstudier: Azure Active Directory-integrering med Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="a0058-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="a0058-104">Syftet med den här kursen är att visa integreringen av Azure och Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="a0058-104">The objective of this tutorial is to show the integration of Azure and Salesforce Sandbox.</span></span>  

>[!TIP]
><span data-ttu-id="a0058-105">Feedback, finns det [Azure supportsidan](http://go.microsoft.com/fwlink/?LinkId=521878).</span><span class="sxs-lookup"><span data-stu-id="a0058-105">For feedback, see the [Azure support page](http://go.microsoft.com/fwlink/?LinkId=521878).</span></span> 
> 

<span data-ttu-id="a0058-106">Sandboxar ger dig möjlighet att skapa flera kopior av din organisation i olika miljöer för olika syften, till exempel utveckling, testning och utbildning, utan att kompromissa med data och program i organisationen Salesforce produktion.</span><span class="sxs-lookup"><span data-stu-id="a0058-106">Sandboxes give you the ability to create multiple copies of your organization in separate environments for a variety of purposes, such as development, testing, and training, without compromising the data and applications in your Salesforce production organization.</span></span>  

<span data-ttu-id="a0058-107">Mer information finns i [Sandbox-översikt](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span><span class="sxs-lookup"><span data-stu-id="a0058-107">For more details, see [Sandbox Overview](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span></span>

<span data-ttu-id="a0058-108">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a0058-108">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="a0058-109">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a0058-109">A valid Azure subscription</span></span>
* <span data-ttu-id="a0058-110">En sandbox i Salesforce.com</span><span class="sxs-lookup"><span data-stu-id="a0058-110">A sandbox in Salesforce.com</span></span>

<span data-ttu-id="a0058-111">Om du inte har en giltig sandbox i Salesforce.com ännu, måste du kontakta Salesforce.</span><span class="sxs-lookup"><span data-stu-id="a0058-111">If you don’t have a valid sandbox in Salesforce.com yet, you need to contact Salesforce.</span></span>

<span data-ttu-id="a0058-112">Det scenario som beskrivs i den här kursen består av följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a0058-112">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="a0058-113">Aktivera programintegrationstyp för Salesforce begränsat läge</span><span class="sxs-lookup"><span data-stu-id="a0058-113">Enabling the application integration for Salesforce Sandbox</span></span>
2. <span data-ttu-id="a0058-114">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="a0058-114">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="a0058-115">Aktivera din domän</span><span class="sxs-lookup"><span data-stu-id="a0058-115">Enabling your domain</span></span>
4. <span data-ttu-id="a0058-116">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="a0058-116">Configuring user provisioning</span></span>
5. <span data-ttu-id="a0058-117">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="a0058-117">Assigning users</span></span>

<span data-ttu-id="a0058-118">![Scenariot](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="a0058-118">![Scenario](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-salesforce-sandbox"></a><span data-ttu-id="a0058-119">Aktivera application-integration för Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="a0058-119">Enable the application integration for Salesforce Sandbox</span></span>
<span data-ttu-id="a0058-120">Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för Salesforce begränsat läge.</span><span class="sxs-lookup"><span data-stu-id="a0058-120">The objective of this section is to outline how to enable the application integration for Salesforce sandbox.</span></span>

<span data-ttu-id="a0058-121">**Utför följande steg om du vill aktivera programintegrationstyp för Salesforce begränsat läge:**</span><span class="sxs-lookup"><span data-stu-id="a0058-121">**To enable the application integration for Salesforce sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="a0058-122">I den klassiska Azure-portalen i det vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a0058-122">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="a0058-123">![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="a0058-123">![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="a0058-124">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="a0058-124">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="a0058-125">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="a0058-125">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="a0058-126">![Program](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="a0058-126">![Applications](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="a0058-127">Öppna den **Programgalleriet**, klickar du på **Lägg till en App**, och klicka sedan på **lägga till ett program som min organisation använder**.</span><span class="sxs-lookup"><span data-stu-id="a0058-127">To open the **Application Gallery**, click **Add An App**, and then click **Add an application for my organization to use**.</span></span>
   
   <span data-ttu-id="a0058-128">![Vad vill du göra? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Vad vill du göra?")</span><span class="sxs-lookup"><span data-stu-id="a0058-128">![What do you want to do?](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "What do you want to do?")</span></span>
5. <span data-ttu-id="a0058-129">I den **sökrutan**, typen **Salesforce Sandbox**.</span><span class="sxs-lookup"><span data-stu-id="a0058-129">In the **search box**, type **Salesforce Sandbox**.</span></span>
   
   <span data-ttu-id="a0058-130">![Programgalleriet](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="a0058-130">![Application Gallery](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Application Gallery")</span></span>
6. <span data-ttu-id="a0058-131">I resultatfönstret, Välj **Salesforce Sandbox**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a0058-131">In the results pane, select **Salesforce Sandbox**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="a0058-132">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")</span><span class="sxs-lookup"><span data-stu-id="a0058-132">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")</span></span>
   
## <a name="configur-single-sign-on-sso"></a><span data-ttu-id="a0058-133">Configur enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="a0058-133">Configur single sign-on (SSO)</span></span>

<span data-ttu-id="a0058-134">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till Salesforce med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="a0058-134">The objective of this section is to outline how to enable users to authenticate to Salesforce with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="a0058-135">**Utför följande steg för att konfigurera enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="a0058-135">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="a0058-136">I den klassiska Azure-portalen på den **Salesforce Sandbox** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0058-136">In the Azure classic portal, on the **Salesforce Sandbox** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="a0058-137">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="a0058-137">![Configure single sign-on](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="a0058-138">På den **hur vill du att användarna kan logga in på Salesforce Sandbox** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a0058-138">On the **How would you like users to sign on to Salesforce Sandbox** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="a0058-139">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")</span><span class="sxs-lookup"><span data-stu-id="a0058-139">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")</span></span>
3. <span data-ttu-id="a0058-140">På den **konfigurera App-URL** sidan den **logga URL** textruta Skriv URL: en med hjälp av följande mönster `http://company.my.salesforce.com`, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a0058-140">On the **Configure App URL** page, in the **Sign On URL** textbox, type your URL using the following pattern `http://company.my.salesforce.com`, and then click **Next**.</span></span>
   
   <span data-ttu-id="a0058-141">![Konfigurera App-URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="a0058-141">![Configure App URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Configure App URL")</span></span>
4. <span data-ttu-id="a0058-142">Om du redan har konfigurerat enkel inloggning för en annan Salesforce Sandbox-instans i katalogen, så måste du även konfigurera den **identifierare** ska ha samma värde som den **inloggning URL**.</span><span class="sxs-lookup"><span data-stu-id="a0058-142">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure the **Identifier** to have the same value as the **Sign on URL**.</span></span> 
 * <span data-ttu-id="a0058-143">Den **identifierare** fältet finns genom att kontrollera den **visa avancerade inställningar** kryssruta på den **konfigurera App-URL** sida i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0058-143">The **Identifier** field can be found by checking the **Show advanced settings** checkbox on the **Configure App URL** page of the dialog.</span></span>
5. <span data-ttu-id="a0058-144">På den **Konfigurera enkel inloggning på Salesforce Sandbox** klickar du på **hämta certifikat**, och sedan spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a0058-144">On the **Configure single sign-on at Salesforce Sandbox** page, click **Download certificate**, and then save the certificate file on your computer.</span></span>
   
   <span data-ttu-id="a0058-145">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="a0058-145">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="a0058-146">Logga in på ditt Salesforce-sandbox som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a0058-146">In a different web browser window, log into your Salesforce sandbox as an administrator.</span></span>
7. <span data-ttu-id="a0058-147">Klicka på menyn högst upp **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="a0058-147">In the menu on the top, click **Setup**.</span></span>
   
   <span data-ttu-id="a0058-148">![Installationsprogrammet](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="a0058-148">![Setup](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")</span></span>
8. <span data-ttu-id="a0058-149">I navigeringsfönstret till vänster klickar du på **säkerhetsåtgärder**, och klicka sedan på **inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a0058-149">In the navigation pane on the left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="a0058-150">![Enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="a0058-150">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On Settings")</span></span>
9. <span data-ttu-id="a0058-151">Utför följande steg i avsnittet Inställningar för enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="a0058-151">On the Single Sign-On Settings section, perform the following steps:</span></span>
   
   <span data-ttu-id="a0058-152">![Enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="a0058-152">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On Settings")</span></span>  
 1.  <span data-ttu-id="a0058-153">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="a0058-153">Select **SAML Enabled**.</span></span> 
 2.  <span data-ttu-id="a0058-154">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="a0058-154">Click **New**.</span></span>
10. <span data-ttu-id="a0058-155">Utför följande steg på avsnittet SAML enkel inloggning inställningar:</span><span class="sxs-lookup"><span data-stu-id="a0058-155">On the SAML Single Sign-On Settings section, perform the following steps:</span></span>
    
    <span data-ttu-id="a0058-156">![SAML enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="a0058-156">![SAML Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On Settings")</span></span>  
 1. <span data-ttu-id="a0058-157">Skriv namnet på konfigurationen i textrutan namn (t.ex.: *SPSSOWAAD\_Test*).</span><span class="sxs-lookup"><span data-stu-id="a0058-157">In the Name textbox, type the name of the configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 
 2. <span data-ttu-id="a0058-158">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på Salesforce Sandbox** dialog sidan, kopiera den **utfärdar-URL** värdet och klistrar in det i den **utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0058-158">In the Azure classic portal, on the **Configure single sign-on at Salesforce Sandbox** dialogue page, copy the **Issuer URL** value, and then paste it into the **Issuer** textbox.</span></span>
 3. <span data-ttu-id="a0058-159">I den **enhets-Id** textruta typen **https://test.salesforce.com** om det är den första Salesforce Sandbox-instans som du lägger till din katalog.</span><span class="sxs-lookup"><span data-stu-id="a0058-159">In the **Entity Id** textbox, type **https://test.salesforce.com** if this is the first Salesforce Sandbox instance that you are adding to your directory.</span></span> <span data-ttu-id="a0058-160">Om du redan har lagt till en instans av Salesforce Sandbox sedan för den **enhets-ID** Skriv i den **logga URL**, som ska vara i formatet:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="a0058-160">If you have already added an instance of Salesforce Sandbox, then for the **Entity ID** type in the **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>   
 4. <span data-ttu-id="a0058-161">Klicka på **Bläddra** att överföra hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="a0058-161">Click **Browse** to upload the downloaded certificate.</span></span>  
 5. <span data-ttu-id="a0058-162">Som **SAML identitetstyp**väljer **Assertion innehåller Federation-ID från användarobjektet**.</span><span class="sxs-lookup"><span data-stu-id="a0058-162">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span> 
 6. <span data-ttu-id="a0058-163">Som **SAML identitet plats**väljer **identitet är i elementet NameIdentifier i instruktionen ämne**.</span><span class="sxs-lookup"><span data-stu-id="a0058-163">As **SAML Identity Location**, select **Identity is in the NameIdentifier element of the Subject statement**.</span></span>
 7. <span data-ttu-id="a0058-164">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på Salesforce Sandbox** dialog sidan, kopiera den **Remote inloggnings-URL** värdet och klistrar in det i den **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0058-164">In the Azure classic portal, on the **Configure single sign-on at Salesforce Sandbox** dialogue page, copy the **Remote Login URL** value, and then paste it into the **Identity Provider Login URL** textbox.</span></span> 
 8. <span data-ttu-id="a0058-165">SFDC har inte stöd för SAML logga ut.</span><span class="sxs-lookup"><span data-stu-id="a0058-165">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="a0058-166">Som en tillfällig lösning kan du klistra in 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' till den **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0058-166">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into the **Identity Provider Logout URL** textbox.</span></span>
 9. <span data-ttu-id="a0058-167">Som **providern initierade begära Tjänstbindning**väljer **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="a0058-167">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 
 10. <span data-ttu-id="a0058-168">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a0058-168">Click **Save**.</span></span>
11. <span data-ttu-id="a0058-169">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0058-169">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="a0058-170">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="a0058-170">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Configure Single Sign-On")</span></span>

## <a name="enable-your-domain"></a><span data-ttu-id="a0058-171">Aktivera din domän</span><span class="sxs-lookup"><span data-stu-id="a0058-171">Enable your domain</span></span>
<span data-ttu-id="a0058-172">Det här avsnittet förutsätter att du redan har skapat en domän.</span><span class="sxs-lookup"><span data-stu-id="a0058-172">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="a0058-173">Mer information finns i [definierar domännamn](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="a0058-173">For more details, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="a0058-174">**Utför följande steg om du vill aktivera din domän:**</span><span class="sxs-lookup"><span data-stu-id="a0058-174">**To enable your domain, perform the following steps:**</span></span>

1. <span data-ttu-id="a0058-175">I det vänstra navigeringsfönstret klickar du på **domänhantering**, och klicka sedan på **min domän.**</span><span class="sxs-lookup"><span data-stu-id="a0058-175">In the left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
   <span data-ttu-id="a0058-176">![Min domän](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="a0058-176">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "My Domain")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="a0058-177">Kontrollera att din domän har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="a0058-177">Please make sure that your domain has been configured correctly.</span></span> 
   > 
2. <span data-ttu-id="a0058-178">I den **sidan inloggningsinställningar** klickar du på **redigera**, sedan som **Autentiseringstjänsten**, Välj namnet på SAML enkel inloggning inställningen från föregående avsnitt och klickar slutligen **spara**.</span><span class="sxs-lookup"><span data-stu-id="a0058-178">In the **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select the name of the SAML Single Sign-On Setting from the previous section, and finally click **Save**.</span></span>
   
   <span data-ttu-id="a0058-179">![Min domän](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="a0058-179">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "My Domain")</span></span>

<span data-ttu-id="a0058-180">Så snart du har en domän som har konfigurerats, bör användarna använda domän-URL för att logga in till Salesforce sandbox.</span><span class="sxs-lookup"><span data-stu-id="a0058-180">As soon as you have a domain configured, your users should use the domain URL to login to the Salesforce sandbox.</span></span>  

<span data-ttu-id="a0058-181">Om du vill hämta värdet för URL: en, klickar du på SSO-profil som du har skapat i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a0058-181">To get the value of the URL, click the SSO profile you have created in the previous section.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="a0058-182">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="a0058-182">Configure user provisioning</span></span>
<span data-ttu-id="a0058-183">Syftet med det här avsnittet är att beskriva hur du aktiverar användaretablering för Active Directory-användarkonton till Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="a0058-183">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Salesforce Sandbox.</span></span>

<span data-ttu-id="a0058-184">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="a0058-184">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="a0058-185">Välj ditt namn för att expandera användare-menyn i Salesforce-portalen i det övre navigeringsfältet:</span><span class="sxs-lookup"><span data-stu-id="a0058-185">In the Salesforce portal, in the top navigation bar, select your name to expand your user menu:</span></span>
   
   <span data-ttu-id="a0058-186">![Mina inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Mina inställningar")</span><span class="sxs-lookup"><span data-stu-id="a0058-186">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "My Settings")</span></span>
2. <span data-ttu-id="a0058-187">Användare-menyn, Välj **Mina inställningar** att öppna din **Mina inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="a0058-187">From your user menu, select **My Settings** to open your **My Settings** page.</span></span>
3. <span data-ttu-id="a0058-188">I den vänstra rutan klickar du på **personliga** Expandera avsnittet personliga och klicka sedan på **Återställ mina säkerhetstoken**:</span><span class="sxs-lookup"><span data-stu-id="a0058-188">In the left pane, click **Personal** to expand the Personal section, and then click **Reset My Security Token**:</span></span>
   
   <span data-ttu-id="a0058-189">![Mina inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Mina inställningar")</span><span class="sxs-lookup"><span data-stu-id="a0058-189">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "My Settings")</span></span>
4. <span data-ttu-id="a0058-190">På den **Återställ mina säkerhetstoken** klickar du på **återställa säkerhetstoken** att begära ett e-postmeddelande som innehåller din Salesforce.com-säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="a0058-190">On the **Reset My Security Token** page, click **Reset Security Token** to request an email that contains your Salesforce.com security token.</span></span>
   
   <span data-ttu-id="a0058-191">![Ny Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "ny Token")</span><span class="sxs-lookup"><span data-stu-id="a0058-191">![New Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "New Token")</span></span>
5. <span data-ttu-id="a0058-192">Kontrollera din inkorg för e-post från Salesforce.com med ”**salesforce.com.com säkerhet bekräftelse**” som ämne.</span><span class="sxs-lookup"><span data-stu-id="a0058-192">Check your email inbox for an email from Salesforce.com with “**salesforce.com.com security confirmation**” as subject.</span></span>
6. <span data-ttu-id="a0058-193">Granska den här e-post och kopiera säkerhet token-värde.</span><span class="sxs-lookup"><span data-stu-id="a0058-193">Review this email and copy the security token value.</span></span>
7. <span data-ttu-id="a0058-194">I den klassiska Azure-portalen på den **salesforce Sandbox** integreringssidan för programmet, klickar du på **konfigurera användaretablering** att öppna den **konfigurera Användaretablering** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0058-194">In the Azure classic portal, on the **salesforce Sandbox** application integration page, click **Configure user provisioning** to open the **Configure User Provisioning** dialog.</span></span>
   
   <span data-ttu-id="a0058-195">![Konfigurera användaretablering](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "konfigurera användaretablering")</span><span class="sxs-lookup"><span data-stu-id="a0058-195">![Configure user provisioning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Configure user provisioning")</span></span>
8. <span data-ttu-id="a0058-196">På den **ange inloggningsinformation Salesforce begränsat läge om du vill aktivera automatisk användaretablering** anger du följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="a0058-196">On the **Enter your Salesforce Sandbox credentials to enable automatic user provisioning** page, provide the following configuration settings:</span></span>
   
   <span data-ttu-id="a0058-197">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")</span><span class="sxs-lookup"><span data-stu-id="a0058-197">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")</span></span>   
 1. <span data-ttu-id="a0058-198">I den **Salesforce Sandbox administratörsanvändarnamnet** textruta typen en Salesforce sandbox kontonamn som har den **systemadministratören** profil i Salesforce.com som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="a0058-198">In the **Salesforce Sandbox Admin User Name** textbox, type a Salesforce sandbox account name that has the **System Administrator** profile in Salesforce.com assigned.</span></span>
 2. <span data-ttu-id="a0058-199">I den **Salesforce Sandbox adminlösenord** textruta skriver du lösenordet för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="a0058-199">In the **Salesforce Sandbox Admin Password** textbox, type the password for this account.</span></span>
 3. <span data-ttu-id="a0058-200">I den **användartoken säkerhet** textruta klistra in säkerhet token-värde.</span><span class="sxs-lookup"><span data-stu-id="a0058-200">In the **User Security Token** textbox, paste the security token value.</span></span>
 4. <span data-ttu-id="a0058-201">Klicka på **verifiera** att kontrollera konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a0058-201">Click **Validate** to verify your configuration.</span></span>
 5. <span data-ttu-id="a0058-202">Klicka på den **nästa** för att öppna den **bekräftelse** sidan.</span><span class="sxs-lookup"><span data-stu-id="a0058-202">Click the **Next** button to open the **Confirmation** page.</span></span>
9. <span data-ttu-id="a0058-203">På den **bekräftelse** klickar du på **Slutför** att spara konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a0058-203">On the **Confirmation** page, click **Complete** to save your configuration.</span></span>
   
## <a name="assigning-users"></a><span data-ttu-id="a0058-204">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="a0058-204">Assigning users</span></span>

<span data-ttu-id="a0058-205">Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.</span><span class="sxs-lookup"><span data-stu-id="a0058-205">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="a0058-206">**Om du vill tilldela användare till Salesforce Sandbox, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a0058-206">**To assign users to Salesforce Sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="a0058-207">Skapa ett testkonto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0058-207">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="a0058-208">På den ** Salesforce Sandbox ** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="a0058-208">On the **Salesforce Sandbox **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="a0058-209">![Tilldela användare](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="a0058-209">![Assign users](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Assign users")</span></span>
3. <span data-ttu-id="a0058-210">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.</span><span class="sxs-lookup"><span data-stu-id="a0058-210">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="a0058-211">![Ja](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="a0058-211">![Yes](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="a0058-212">Nu bör du vänta i 10 minuter och kontrollera att kontot har synkroniserats till Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="a0058-212">You should now wait for 10 minutes and verify that the account has been synchronized to Salesforce Sandbox.</span></span>

<span data-ttu-id="a0058-213">Om du vill testa inställningarna för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a0058-213">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="a0058-214">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="a0058-214">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

