---
title: "Självstudier: Azure Active Directory-integrering med TOPdesk - säker | Microsoft Docs"
description: "Lär dig hur toouse TOPdesk - skydda med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mer!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 10fe420d1691c2845b89c779486ffd6fcd736432
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a><span data-ttu-id="7a152-103">Självstudier: Azure Active Directory-integrering med TOPdesk - säker</span><span class="sxs-lookup"><span data-stu-id="7a152-103">Tutorial: Azure Active Directory integration with TOPdesk - Secure</span></span>
<span data-ttu-id="7a152-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och TOPdesk - säkra.</span><span class="sxs-lookup"><span data-stu-id="7a152-104">hello objective of this tutorial is tooshow hello integration of Azure and TOPdesk - Secure.</span></span>  
<span data-ttu-id="7a152-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7a152-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="7a152-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7a152-106">A valid Azure subscription</span></span>
* <span data-ttu-id="7a152-107">En TOPdesk - aktiverad prenumeration säker enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a152-107">A TOPdesk - Secure single sign-on enabled subscription</span></span>

<span data-ttu-id="7a152-108">När du har slutfört den här självstudiekursen, hello Azure AD-användare som tilldelats tooTOPdesk - säker kommer att kunna toosingle logga in på hello-programmet på din TOPdesk - säkra företagets webbplats (service provider initierade inloggning) eller använda hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7a152-108">After completing this tutorial, hello Azure AD users you have assigned tooTOPdesk - Secure will be able toosingle sign into hello application at your TOPdesk - Secure company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="7a152-109">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="7a152-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="7a152-110">Att aktivera programmet hello-integrering för TOPdesk - säker</span><span class="sxs-lookup"><span data-stu-id="7a152-110">Enabling hello application integration for TOPdesk - Secure</span></span>
2. <span data-ttu-id="7a152-111">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a152-111">Configuring single sign-on</span></span>
3. <span data-ttu-id="7a152-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="7a152-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="7a152-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="7a152-113">Assigning users</span></span>

<span data-ttu-id="7a152-114">![Scenariot](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="7a152-114">![Scenario](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-topdesk---secure"></a><span data-ttu-id="7a152-115">Att aktivera programmet hello-integrering för TOPdesk - säker</span><span class="sxs-lookup"><span data-stu-id="7a152-115">Enabling hello application integration for TOPdesk - Secure</span></span>
<span data-ttu-id="7a152-116">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för TOPdesk - säker.</span><span class="sxs-lookup"><span data-stu-id="7a152-116">hello objective of this section is toooutline how tooenable hello application integration for TOPdesk - Secure.</span></span>

### <a name="tooenable-hello-application-integration-for-topdesk---secure-perform-hello-following-steps"></a><span data-ttu-id="7a152-117">tooenable hello programintegrationstyp för TOPdesk - säker, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a152-117">tooenable hello application integration for TOPdesk - Secure, perform hello following steps:</span></span>
1. <span data-ttu-id="7a152-118">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7a152-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="7a152-119">![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="7a152-119">![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")</span></span>

2. <span data-ttu-id="7a152-120">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="7a152-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="7a152-121">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a152-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="7a152-122">![Program](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="7a152-122">![Applications](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Applications")</span></span>

4. <span data-ttu-id="7a152-123">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="7a152-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="7a152-124">![Lägg till program](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="7a152-124">![Add application](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Add application")</span></span>

5. <span data-ttu-id="7a152-125">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="7a152-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="7a152-126">![Lägga till ett program från gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="7a152-126">![Add an application from gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Add an application from gallerry")</span></span>

6. <span data-ttu-id="7a152-127">I hello **sökrutan**, typen **TOPdesk - säker**.</span><span class="sxs-lookup"><span data-stu-id="7a152-127">In hello **search box**, type **TOPdesk - Secure**.</span></span>
   
    <span data-ttu-id="7a152-128">![Programgalleriet](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="7a152-128">![Application Gallery](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Application Gallery")</span></span>

7. <span data-ttu-id="7a152-129">I resultatfönstret hello väljer **TOPdesk - säker**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7a152-129">In hello results pane, select **TOPdesk - Secure**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="7a152-130">![TOPdesk - säker](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - säker")</span><span class="sxs-lookup"><span data-stu-id="7a152-130">![TOPdesk - Secure](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - Secure")</span></span>

## <a name="configuring-single-sign-on"></a><span data-ttu-id="7a152-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a152-131">Configuring single sign-on</span></span>
<span data-ttu-id="7a152-132">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooTOPdesk - skydda med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="7a152-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooTOPdesk - Secure with their account in Azure AD using federation based on hello SAML protocol.</span></span>  
<span data-ttu-id="7a152-133">Konfigurera enkel inloggning för TOPdesk - säker kräver att du tooupload en logotyp ikonfil.</span><span class="sxs-lookup"><span data-stu-id="7a152-133">Configuring single sign-on for TOPdesk - Secure requires you tooupload a logo icon file.</span></span> <span data-ttu-id="7a152-134">tooget hello ikonfil, kontakta hello TOPdesk supportteamet.</span><span class="sxs-lookup"><span data-stu-id="7a152-134">tooget hello icon file, contact hello TOPdesk support team.</span></span>

### <a name="tooconfigure-single-sign-on-perform-hello-following-steps"></a><span data-ttu-id="7a152-135">tooconfigure enkel inloggning, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a152-135">tooconfigure single sign-on, perform hello following steps:</span></span>
1. <span data-ttu-id="7a152-136">Logga in tooyour **TOPdesk - säker** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="7a152-136">Sign on tooyour **TOPdesk - Secure** company site as an administrator.</span></span>
2. <span data-ttu-id="7a152-137">I hello **TOPdesk** -menyn klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7a152-137">In hello **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="7a152-138">![Inställningar för](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="7a152-138">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

3. <span data-ttu-id="7a152-139">Klicka på **inloggningsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="7a152-139">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="7a152-140">![Inloggningsinställningar](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "inloggningsinställningar")</span><span class="sxs-lookup"><span data-stu-id="7a152-140">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

4. <span data-ttu-id="7a152-141">Expandera hello **inloggningsinställningar** -menyn och klicka sedan på **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="7a152-141">Expand hello **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="7a152-142">![Allmän](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Allmänt")</span><span class="sxs-lookup"><span data-stu-id="7a152-142">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

5. <span data-ttu-id="7a152-143">I hello **Secure** avsnitt i hello **SAML inloggningen** configuration avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a152-143">In hello **Secure** section of hello **SAML login** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="7a152-144">![Tekniska inställningar](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "tekniska inställningar")</span><span class="sxs-lookup"><span data-stu-id="7a152-144">![Technical Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technical Settings")</span></span>
   
    <span data-ttu-id="7a152-145">a.</span><span class="sxs-lookup"><span data-stu-id="7a152-145">a.</span></span> <span data-ttu-id="7a152-146">Klicka på **hämta** toodownload hello offentliga metadatafil och sedan spara det lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="7a152-146">Click **Download** toodownload hello public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="7a152-147">b.</span><span class="sxs-lookup"><span data-stu-id="7a152-147">b.</span></span> <span data-ttu-id="7a152-148">Öppna hello metadatafil och välj sedan hello **AssertionConsumerService** nod.</span><span class="sxs-lookup"><span data-stu-id="7a152-148">Open hello metadata file, and then locate hello **AssertionConsumerService** node.</span></span>
    
    <span data-ttu-id="7a152-149">![Assertion kundtjänst](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion kundtjänst")</span><span class="sxs-lookup"><span data-stu-id="7a152-149">![Assertion Consumer Service](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion Consumer Service")</span></span>
   
    <span data-ttu-id="7a152-150">c.</span><span class="sxs-lookup"><span data-stu-id="7a152-150">c.</span></span> <span data-ttu-id="7a152-151">Kopiera hello **AssertionConsumerService** värde.</span><span class="sxs-lookup"><span data-stu-id="7a152-151">Copy hello **AssertionConsumerService** value.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="7a152-152">Du behöver hello värdet i hello **konfigurera App-URL** senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="7a152-152">You will need hello value in hello **Configure App URL** section later in this tutorial.</span></span>
    > 
    > 

6. <span data-ttu-id="7a152-153">Logga in i ett annat webbläsarfönster din **klassiska Azure-portalen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="7a152-153">In a different web browser window, log into your **Azure classic portal** as an administrator.</span></span>

7. <span data-ttu-id="7a152-154">På hello **TOPdesk - säker** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello ** Konfigurera enkel inloggning ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a152-154">On hello **TOPdesk - Secure** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="7a152-155">![Konfigurera enkel inloggning](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7a152-155">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Configure Single Sign-On")</span></span>

8. <span data-ttu-id="7a152-156">På hello **hur skulle du som användare toosign på tooTOPdesk - Secure** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="7a152-156">On hello **How would you like users toosign on tooTOPdesk - Secure** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="7a152-157">![Konfigurera enkel inloggning](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7a152-157">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Configure Single Sign-On")</span></span>

9. <span data-ttu-id="7a152-158">På hello **konfigurera App-URL** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a152-158">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="7a152-159">![Konfigurera App-URL](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="7a152-159">![Configure App URL](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Configure App URL")</span></span>
   
    <span data-ttu-id="7a152-160">a.</span><span class="sxs-lookup"><span data-stu-id="7a152-160">a.</span></span> <span data-ttu-id="7a152-161">I hello **TOPdesk - säker inloggning på URL: en** textruta typen hello URL som används av dina användare toosign i din TOPdesk - säkert program (t.ex. ”:*https://qssolutions.topdesk.net*”).</span><span class="sxs-lookup"><span data-stu-id="7a152-161">In hello **TOPdesk - Secure Sign On URL** textbox, type hello URL used by your users toosign into your TOPdesk - Secure application (e.g.: "*https://qssolutions.topdesk.net*").</span></span>
   
    <span data-ttu-id="7a152-162">b.</span><span class="sxs-lookup"><span data-stu-id="7a152-162">b.</span></span> <span data-ttu-id="7a152-163">I hello **TOPdesk – offentliga Reply URL** textruta klistra in hello **TOPdesk - URL för säker AssertionConsumerService** (t.ex. ”:*https://qssolutions.topdesk.net/tas/public/login/saml*")</span><span class="sxs-lookup"><span data-stu-id="7a152-163">In hello **TOPdesk – Public Reply URL** textbox, paste hello **TOPdesk - Secure AssertionConsumerService URL** (e.g.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span></span>
   
    <span data-ttu-id="7a152-164">c.</span><span class="sxs-lookup"><span data-stu-id="7a152-164">c.</span></span> <span data-ttu-id="7a152-165">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="7a152-165">Click **Next**.</span></span>

10. <span data-ttu-id="7a152-166">På hello **Konfigurera enkel inloggning på TOPdesk - säker** sida, toodownload metadatafil klickar du på **hämtar metadata**, och sedan spara hello filen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="7a152-166">On hello **Configure single sign-on at TOPdesk - Secure** page, toodownload your metadata file, click **Download metadata**, and then save hello file locally on your computer.</span></span>
    
    <span data-ttu-id="7a152-167">![Konfigurera enkel inloggning](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7a152-167">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Configure Single Sign-On")</span></span>

11. <span data-ttu-id="7a152-168">toocreate en certifikatfil utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a152-168">toocreate a certificate file, perform hello following steps:</span></span>
    
    <span data-ttu-id="7a152-169">![Certifikatet](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "certifikat")</span><span class="sxs-lookup"><span data-stu-id="7a152-169">![Certificate](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certificate")</span></span>
    
    <span data-ttu-id="7a152-170">a.</span><span class="sxs-lookup"><span data-stu-id="7a152-170">a.</span></span> <span data-ttu-id="7a152-171">Öppna hello hämtade metadatafil.</span><span class="sxs-lookup"><span data-stu-id="7a152-171">Open hello downloaded metadata file.</span></span>
    <span data-ttu-id="7a152-172">b.</span><span class="sxs-lookup"><span data-stu-id="7a152-172">b.</span></span> <span data-ttu-id="7a152-173">Expandera hello **RoleDescriptor** nod som har en **xsi: type** av **aggregeringsdesignprocessen: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="7a152-173">Expand hello **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    <span data-ttu-id="7a152-174">c.</span><span class="sxs-lookup"><span data-stu-id="7a152-174">c.</span></span> <span data-ttu-id="7a152-175">Kopiera hello värdet för hello **X509Certificate** nod.</span><span class="sxs-lookup"><span data-stu-id="7a152-175">Copy hello value of hello **X509Certificate** node.</span></span>
    <span data-ttu-id="7a152-176">d.</span><span class="sxs-lookup"><span data-stu-id="7a152-176">d.</span></span> <span data-ttu-id="7a152-177">Spara hello kopieras **X509Certificate** värdet lokalt på din dator i en fil.</span><span class="sxs-lookup"><span data-stu-id="7a152-177">Save hello copied **X509Certificate** value locally on your computer in a file.</span></span>

12. <span data-ttu-id="7a152-178">Skydda företagets plats i hello på din TOPdesk - **TOPdesk** -menyn klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7a152-178">On your TOPdesk - Secure company site, in hello **TOPdesk** menu, click **Settings**.</span></span>
    
    <span data-ttu-id="7a152-179">![Inställningar för](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="7a152-179">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

13. <span data-ttu-id="7a152-180">Klicka på **inloggningsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="7a152-180">Click **Login Settings**.</span></span>
    
    <span data-ttu-id="7a152-181">![Inloggningsinställningar](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "inloggningsinställningar")</span><span class="sxs-lookup"><span data-stu-id="7a152-181">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

14. <span data-ttu-id="7a152-182">Expandera hello **inloggningsinställningar** -menyn och klicka sedan på **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="7a152-182">Expand hello **Login Settings** menu, and then click **General**.</span></span>
    
    <span data-ttu-id="7a152-183">![Allmän](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Allmänt")</span><span class="sxs-lookup"><span data-stu-id="7a152-183">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

15. <span data-ttu-id="7a152-184">I hello **offentliga** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7a152-184">In hello **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="7a152-185">![Lägg till](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="7a152-185">![Add](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Add")</span></span>

16. <span data-ttu-id="7a152-186">På hello **SAML configuration assistenten** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a152-186">On hello **SAML configuration assistant** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="7a152-187">![SAML Configuration assistenten](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "assistenten för SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="7a152-187">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="7a152-188">a.</span><span class="sxs-lookup"><span data-stu-id="7a152-188">a.</span></span> <span data-ttu-id="7a152-189">tooupload hämtade metadata filen under **Federationsmetadata**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="7a152-189">tooupload your downloaded metadata file, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="7a152-190">b.</span><span class="sxs-lookup"><span data-stu-id="7a152-190">b.</span></span> <span data-ttu-id="7a152-191">ditt certifikat filen under tooupload **certifikat (RSA)**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="7a152-191">tooupload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="7a152-192">c.</span><span class="sxs-lookup"><span data-stu-id="7a152-192">c.</span></span> <span data-ttu-id="7a152-193">tooupload hello logotypfilen som du har fått från supportteamet för hello TOPdesk under **logotypen ikonen**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="7a152-193">tooupload hello logo file you got from hello TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="7a152-194">d.</span><span class="sxs-lookup"><span data-stu-id="7a152-194">d.</span></span> <span data-ttu-id="7a152-195">I hello **användarnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="7a152-195">In hello **User name attribute** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="7a152-196">e.</span><span class="sxs-lookup"><span data-stu-id="7a152-196">e.</span></span> <span data-ttu-id="7a152-197">I hello **visningsnamn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7a152-197">In hello **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="7a152-198">f.</span><span class="sxs-lookup"><span data-stu-id="7a152-198">f.</span></span> <span data-ttu-id="7a152-199">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7a152-199">Click **Save**.</span></span>

17. <span data-ttu-id="7a152-200">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a152-200">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="7a152-201">![Konfigurera enkel inloggning](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7a152-201">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Configure Single Sign-On")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="7a152-202">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="7a152-202">Configuring user provisioning</span></span>
<span data-ttu-id="7a152-203">I ordning tooenable Azure AD-användare toolog i TOPdesk - säker, de måste etableras i TOPdesk - säker.</span><span class="sxs-lookup"><span data-stu-id="7a152-203">In order tooenable Azure AD users toolog into TOPdesk - Secure, they must be provisioned into TOPdesk - Secure.</span></span>  
<span data-ttu-id="7a152-204">Hello gäller TOPdesk - säker, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7a152-204">In hello case of TOPdesk - Secure, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="7a152-205">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="7a152-205">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="7a152-206">Logga in tooyour **TOPdesk - säker** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="7a152-206">Sign on tooyour **TOPdesk - Secure** company site as administrator.</span></span>
2. <span data-ttu-id="7a152-207">Hello-menyn överst hello **TOPdesk \> ny \> stödfiler \> operatorn**.</span><span class="sxs-lookup"><span data-stu-id="7a152-207">In hello menu on hello top, click **TOPdesk \> New \> Support Files \> Operator**.</span></span>
   
    <span data-ttu-id="7a152-208">![Operatorn](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")</span><span class="sxs-lookup"><span data-stu-id="7a152-208">![Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")</span></span>

3. <span data-ttu-id="7a152-209">På hello **New-operatorn** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a152-209">On hello **New Operator** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="7a152-210">![New-operatorn](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New-operatorn")</span><span class="sxs-lookup"><span data-stu-id="7a152-210">![New Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New Operator")</span></span>
   
    <span data-ttu-id="7a152-211">a.</span><span class="sxs-lookup"><span data-stu-id="7a152-211">a.</span></span> <span data-ttu-id="7a152-212">Klicka på hello på fliken Allmänt.</span><span class="sxs-lookup"><span data-stu-id="7a152-212">Click hello General tab.</span></span>
   
    <span data-ttu-id="7a152-213">b.</span><span class="sxs-lookup"><span data-stu-id="7a152-213">b.</span></span> <span data-ttu-id="7a152-214">I hello **efternamn** textruta för hello **allmänna** avsnitt, Skriv hello efternamn på en giltig Azure Active Directory-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="7a152-214">In hello **Surname** textbox of hello **General** section, type hello last name of a valid Azure Active Directory account you want tooprovision.</span></span>
   
    <span data-ttu-id="7a152-215">c.</span><span class="sxs-lookup"><span data-stu-id="7a152-215">c.</span></span> <span data-ttu-id="7a152-216">Välj en **plats** för hello konto i hello **plats** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7a152-216">Select a **Site** for hello account in hello **Location** section.</span></span>
   
    <span data-ttu-id="7a152-217">d.</span><span class="sxs-lookup"><span data-stu-id="7a152-217">d.</span></span> <span data-ttu-id="7a152-218">I hello **inloggningsnamnet** textruta för hello **TOPdesk inloggning** skriver ett inloggningsnamn för användaren.</span><span class="sxs-lookup"><span data-stu-id="7a152-218">In hello **Login Name** textbox of hello **TOPdesk Login** section, type a login name for your user.</span></span>
   
    <span data-ttu-id="7a152-219">e.</span><span class="sxs-lookup"><span data-stu-id="7a152-219">e.</span></span> <span data-ttu-id="7a152-220">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7a152-220">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="7a152-221">Du kan använda alla andra TOPdesk - säker användarkonto verktyg för att skapa eller API: er som tillhandahålls av TOPdesk - Secure tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="7a152-221">You can use any other TOPdesk - Secure user account creation tools or APIs provided by TOPdesk - Secure tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assigning-users"></a><span data-ttu-id="7a152-222">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="7a152-222">Assigning users</span></span>
<span data-ttu-id="7a152-223">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="7a152-223">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

### <a name="tooassign-users-tootopdesk---secure-perform-hello-following-steps"></a><span data-ttu-id="7a152-224">tooassign användare tooTOPdesk - säker, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a152-224">tooassign users tooTOPdesk - Secure, perform hello following steps:</span></span>
1. <span data-ttu-id="7a152-225">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a152-225">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="7a152-226">På hello ** TOPdesk - säker ** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="7a152-226">On hello **TOPdesk - Secure **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="7a152-227">![Tilldela användare](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="7a152-227">![Assign Users](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Assign Users")</span></span>

3. <span data-ttu-id="7a152-228">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="7a152-228">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="7a152-229">![Ja](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="7a152-229">![Yes](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="7a152-230">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7a152-230">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="7a152-231">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7a152-231">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

