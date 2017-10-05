---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA Molnplattform | Microsoft Docs"
description: "Lär dig hur du använder SAP HANA Molnplattform med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: e03bc2410a8d57363c558f723b3bfd0e69b3f4c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a><span data-ttu-id="d13c8-103">Självstudier: Azure Active Directory-integration med SAP HANA-molnplattformen</span><span class="sxs-lookup"><span data-stu-id="d13c8-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform</span></span>
<span data-ttu-id="d13c8-104">Syftet med den här kursen är att visa integreringen av Azure och Molnplattform för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="d13c8-104">The objective of this tutorial is to show the integration of Azure and SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="d13c8-105">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d13c8-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="d13c8-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d13c8-106">A valid Azure subscription</span></span>
* <span data-ttu-id="d13c8-107">En Molnplattform för SAP HANA-konto</span><span class="sxs-lookup"><span data-stu-id="d13c8-107">A SAP HANA Cloud Platform account</span></span>

<span data-ttu-id="d13c8-108">Den här kursen Azure AD-användare som du har tilldelat till SAP HANA Molnplattform kommer att kunna enkel inloggning till programmet med hjälp av den [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d13c8-108">After completing this tutorial, the Azure AD users you have assigned to SAP HANA Cloud Platform will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

>[!IMPORTANT]
><span data-ttu-id="d13c8-109">Du måste distribuera ditt eget program eller prenumerera på ett program på Molnplattform för SAP HANA-konto för att testa enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="d13c8-109">You need to deploy your own application or subscribe to an application on your SAP HANA Cloud Platform account to test single sign on.</span></span> <span data-ttu-id="d13c8-110">I den här självstudiekursen distribueras ett program i kontot.</span><span class="sxs-lookup"><span data-stu-id="d13c8-110">In this tutorial, an application is deployed in the account.</span></span>
> 
> 

<span data-ttu-id="d13c8-111">Det scenario som beskrivs i den här kursen består av följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d13c8-111">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="d13c8-112">Aktivera application-integrering för SAP HANA Molnplattform</span><span class="sxs-lookup"><span data-stu-id="d13c8-112">Enabling the application integration for SAP HANA Cloud Platform</span></span>
2. <span data-ttu-id="d13c8-113">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="d13c8-113">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="d13c8-114">Tilldela en roll till en användare</span><span class="sxs-lookup"><span data-stu-id="d13c8-114">Assigning a role to a user</span></span>
4. <span data-ttu-id="d13c8-115">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="d13c8-115">Assigning users</span></span>

<span data-ttu-id="d13c8-116">![Scenariot](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="d13c8-116">![Scenario](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a><span data-ttu-id="d13c8-117">Aktivera application-integrering för SAP HANA Molnplattform</span><span class="sxs-lookup"><span data-stu-id="d13c8-117">Enabling the application integration for SAP HANA Cloud Platform</span></span>
<span data-ttu-id="d13c8-118">Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för SAP HANA Molnplattform.</span><span class="sxs-lookup"><span data-stu-id="d13c8-118">The objective of this section is to outline how to enable the application integration for SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="d13c8-119">**Utför följande steg om du vill aktivera programmet-integrering för SAP HANA Molnplattform:**</span><span class="sxs-lookup"><span data-stu-id="d13c8-119">**To enable the application integration for SAP HANA Cloud Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="d13c8-120">I Azure-hanteringsportalen på det vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-120">In the Azure Management Portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="d13c8-121">![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="d13c8-121">![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="d13c8-122">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="d13c8-122">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="d13c8-123">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="d13c8-123">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="d13c8-124">![Program](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="d13c8-124">![Applications](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="d13c8-125">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="d13c8-125">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="d13c8-126">![Lägg till program](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="d13c8-126">![Add application](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="d13c8-127">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-127">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="d13c8-128">![Lägga till ett program från gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="d13c8-128">![Add an application from gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="d13c8-129">I den **sökrutan**, typen **Molnplattform för SAP HANA**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-129">In the **search box**, type **SAP HANA Cloud Platform**.</span></span>
   
    <span data-ttu-id="d13c8-130">![Programgalleriet](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="d13c8-130">![Application Gallery](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Application Gallery")</span></span>
7. <span data-ttu-id="d13c8-131">I resultatfönstret, Välj **Molnplattform för SAP HANA**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d13c8-131">In the results pane, select **SAP HANA Cloud Platform**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="d13c8-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span><span class="sxs-lookup"><span data-stu-id="d13c8-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="d13c8-133">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d13c8-133">Configure single sign-on</span></span>

<span data-ttu-id="d13c8-134">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till SAP HANA Molnplattform med ett konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="d13c8-134">The objective of this section is to outline how to enable users to authenticate to SAP HANA Cloud Platform with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="d13c8-135">Som en del av den här proceduren måste krävs du för att överföra en Base64-kodade certifikat till din Molnplattform för SAP HANA-klient.</span><span class="sxs-lookup"><span data-stu-id="d13c8-135">As part of this procedure, you are required to upload a base-64 encoded certificate to your SAP HANA Cloud Platform tenant.</span></span>  

<span data-ttu-id="d13c8-136">Om du inte är bekant med den här proceduren, se [hur du konverterar ett binärt certifikat till en textfil](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="d13c8-136">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="d13c8-137">**Utför följande steg för att konfigurera enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="d13c8-137">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="d13c8-138">I den klassiska Azure-portalen på den **Molnplattform för SAP HANA** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d13c8-138">In the Azure classic portal, on the **SAP HANA Cloud Platform** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="d13c8-139">![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="d13c8-139">![Configure single sign-on](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="d13c8-140">På den **hur vill du att användarna kan logga in på Molnplattform för SAP HANA** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-140">On the **How would you like users to sign on to SAP HANA Cloud Platform** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="d13c8-141">![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="d13c8-141">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="d13c8-142">Logga in till SAP HANA Cloud Platform Cockpit på https://account i ett annat webbläsarfönster. \<liggande värden\>.ondemand.com/cockpit (t.ex.: *https://account.hanatrial.ondemand.com/cockpit*).</span><span class="sxs-lookup"><span data-stu-id="d13c8-142">In a different web browser window, sign on to the SAP HANA Cloud Platform Cockpit at https://account.\<landscape host\>.ondemand.com/cockpit (e.g.: *https://account.hanatrial.ondemand.com/cockpit*).</span></span>
4. <span data-ttu-id="d13c8-143">Klicka på den **förtroende** fliken.</span><span class="sxs-lookup"><span data-stu-id="d13c8-143">Click the **Trust** tab.</span></span>
   
    <span data-ttu-id="d13c8-144">![Förtroende](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "förtroende")</span><span class="sxs-lookup"><span data-stu-id="d13c8-144">![Trust](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Trust")</span></span>
5. <span data-ttu-id="d13c8-145">Utför följande steg i förtroende i avsnittet:</span><span class="sxs-lookup"><span data-stu-id="d13c8-145">In trust management section, perform the following steps:</span></span>
   
    <span data-ttu-id="d13c8-146">![Hämta Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "hämta Metadata")</span><span class="sxs-lookup"><span data-stu-id="d13c8-146">![Get Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Get Metadata")</span></span>
   
   1. <span data-ttu-id="d13c8-147">Klicka på den **lokal-leverantör** fliken.</span><span class="sxs-lookup"><span data-stu-id="d13c8-147">Click the **Local Service Provider** tab.</span></span>
   2. <span data-ttu-id="d13c8-148">Hämta Molnplattform för SAP HANA-metadatafil Klicka **hämta Metadata**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-148">To download the SAP HANA Cloud Platform metadata file, click **Get Metadata**.</span></span>
6. <span data-ttu-id="d13c8-149">I Azure Active-klassiska portalen på den **konfigurera App-URL** , utför följande steg, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-149">In the Azure Active classic portal, on the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
    <span data-ttu-id="d13c8-150">![Konfigurera App-URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="d13c8-150">![Configure App URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configure App URL")</span></span>
   
   1. <span data-ttu-id="d13c8-151">I den **logga URL** textruta, Skriv URL-Adressen används av användarna att logga in på ditt **Molnplattform för SAP HANA** program.</span><span class="sxs-lookup"><span data-stu-id="d13c8-151">In the **Sign On URL** textbox, type the URL used by your users to sign into your **SAP HANA Cloud Platform** application.</span></span> <span data-ttu-id="d13c8-152">Detta är en skyddad resurs i ditt program för SAP HANA Molnplattform kontospecifikt URL.</span><span class="sxs-lookup"><span data-stu-id="d13c8-152">This is the account-specific URL of a protected resource in your SAP HANA Cloud Platform application.</span></span> <span data-ttu-id="d13c8-153">URL: en är baserad på följande mönster: *https://\<applicationName\>\<accountName\>.\< liggande värden\>.ondemand.com/\<sökväg\_till\_skyddade\_resurs\>*  (t.ex.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span><span class="sxs-lookup"><span data-stu-id="d13c8-153">The URL is based on the following pattern: *https://\<applicationName\>\<accountName\>.\<landscape host\>.ondemand.com/\<path\_to\_protected\_resource\>* (e.g.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="d13c8-154">Det här är URL-Adressen i din Molnplattform för SAP HANA-program som kräver att användaren autentiseras.</span><span class="sxs-lookup"><span data-stu-id="d13c8-154">This is the URL in your SAP HANA Cloud Platform application that requires the user to authenticate.</span></span>
     > 

   2. <span data-ttu-id="d13c8-155">Öppna den hämta filen för SAP HANA Molnplattform metadata och välj sedan den **ns3:AssertionConsumerService** tagg.</span><span class="sxs-lookup"><span data-stu-id="d13c8-155">Open the downloaded SAP HANA Cloud Platform metadata file, and then locate the **ns3:AssertionConsumerService** tag.</span></span>
   3. <span data-ttu-id="d13c8-156">Kopiera värdet för den **plats** attributet och klistrar in det i den **Reply-URL för SAP HANA Cloud Platform** textruta.</span><span class="sxs-lookup"><span data-stu-id="d13c8-156">Copy the value of the **Location** attribute, and then paste it into the **SAP HANA Cloud Platform Reply URL** textbox.</span></span>

7. <span data-ttu-id="d13c8-157">På den **Konfigurera enkel inloggning på SAP HANA Molnplattform** att hämta metadata, klickar du på **hämtar metadata**, och sedan spara filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d13c8-157">On the **Configure single sign-on at SAP HANA Cloud Platform** page, to download your metadata, click **Download metadata**, and then save the file on your computer.</span></span>
   
    <span data-ttu-id="d13c8-158">![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="d13c8-158">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configure Single Sign-On")</span></span>
8. <span data-ttu-id="d13c8-159">På SAP HANA Cloud Platform Cockpit i den **lokal-leverantör** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d13c8-159">On the SAP HANA Cloud Platform Cockpit, in the **Local Service Provider** section, perform the following steps:</span></span>
   
    <span data-ttu-id="d13c8-160">![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "förtroende för hantering")</span><span class="sxs-lookup"><span data-stu-id="d13c8-160">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Trust Management")</span></span>
   
  1. <span data-ttu-id="d13c8-161">Klicka på **Redigera**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-161">Click **Edit**.</span></span>
  2. <span data-ttu-id="d13c8-162">Som **konfigurationstypen**väljer **anpassad**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-162">As **Configuration Type**, select **Custom**.</span></span>
  3. <span data-ttu-id="d13c8-163">Som **lokala providernamn**, låt standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="d13c8-163">As **Local Provider Name**, leave the default value.</span></span>
  4. <span data-ttu-id="d13c8-164">Generera en **signeringsnyckeln** och en **signeringscertifikat** nyckelpar, klickar du på **Generera nyckel**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-164">To generate a **Signing Key** and a **Signing Certificate** key pair, click **Generate Key Pair**.</span></span>
  5. <span data-ttu-id="d13c8-165">Som **huvudnamn spridningen**väljer **inaktiverade**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-165">As **Principal Propagation**, select **Disabled**.</span></span>
  6. <span data-ttu-id="d13c8-166">Som **kraft autentisering**väljer **inaktiverade**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-166">As **Force Authentication**, select **Disabled**.</span></span>
  7. <span data-ttu-id="d13c8-167">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-167">Click **Save**.</span></span>

9. <span data-ttu-id="d13c8-168">Klicka på den **betrodda identitetsleverantör** fliken och klicka sedan på **lägga till betrodda identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-168">Click the **Trusted Identity Provider** tab, and then click **Add Trusted Identity Provider**.</span></span>
   
    <span data-ttu-id="d13c8-169">![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "förtroende för hantering")</span><span class="sxs-lookup"><span data-stu-id="d13c8-169">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Trust Management")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="d13c8-170">Om du vill hantera listan över betrodda identitetsleverantörer, måste du har valt typen för anpassad konfiguration i avsnittet lokal-leverantör.</span><span class="sxs-lookup"><span data-stu-id="d13c8-170">To manage the list of trusted identity providers, you need to have chosen the Custom configuration type in the Local Service Provider section.</span></span> <span data-ttu-id="d13c8-171">Standard konfigurationstypen har du en redigeras och implicit förtroende till SAP-ID-tjänst.</span><span class="sxs-lookup"><span data-stu-id="d13c8-171">For Default configuration type, you have a non-editable and implicit trust to the SAP ID Service.</span></span> <span data-ttu-id="d13c8-172">Ingen har du inte för förtroendeinställningar.</span><span class="sxs-lookup"><span data-stu-id="d13c8-172">For None, you don't have any trust settings.</span></span>
    > 
    > 

10. <span data-ttu-id="d13c8-173">Klicka på den **allmänna** fliken och klicka sedan på **Bläddra** att överföra metadatafilen hämtade.</span><span class="sxs-lookup"><span data-stu-id="d13c8-173">Click the **General** tab, and then click **Browse** to upload the downloaded metadata file.</span></span>
    
    <span data-ttu-id="d13c8-174">![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "förtroende för hantering")</span><span class="sxs-lookup"><span data-stu-id="d13c8-174">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Trust Management")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="d13c8-175">Efter överföring metadatafil värdena för **URL för enkel inloggning**, **URL för enkel logga ut** och **signeringscertifikat** fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d13c8-175">After uploading the metadata file, the values for **Single Sign-on URL**, **Single Logout URL** and **Signing Certificate** are populated automatically.</span></span>
    > 
    > 

11. <span data-ttu-id="d13c8-176">Klicka på fliken **Attribut**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-176">Click the **Attributes** tab.</span></span>
12. <span data-ttu-id="d13c8-177">På den **attribut** fliken, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d13c8-177">On the **Attributes** tab, perform the following step:</span></span>
    
    <span data-ttu-id="d13c8-178">![Attribut](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="d13c8-178">![Attributes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attributes")</span></span> 
  * <span data-ttu-id="d13c8-179">Klicka på **Add Assertion-Based attributet**, och Lägg sedan till följande assertion-baserade attribut:</span><span class="sxs-lookup"><span data-stu-id="d13c8-179">Click **Add Assertion-Based Attribute**, and then add the following assertion-based attributes:</span></span>
       
    | <span data-ttu-id="d13c8-180">Attributet för kontrollen</span><span class="sxs-lookup"><span data-stu-id="d13c8-180">Assertion Attribute</span></span> | <span data-ttu-id="d13c8-181">Huvudnamn attribut</span><span class="sxs-lookup"><span data-stu-id="d13c8-181">Principal Attribute</span></span> |
    | --- | --- |
    | <span data-ttu-id="d13c8-182">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName</span><span class="sxs-lookup"><span data-stu-id="d13c8-182">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname</span></span> |<span data-ttu-id="d13c8-183">Förnamn</span><span class="sxs-lookup"><span data-stu-id="d13c8-183">firstname</span></span> |
    | <span data-ttu-id="d13c8-184">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname</span><span class="sxs-lookup"><span data-stu-id="d13c8-184">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname</span></span> |<span data-ttu-id="d13c8-185">Efternamn</span><span class="sxs-lookup"><span data-stu-id="d13c8-185">lastname</span></span> |
    | <span data-ttu-id="d13c8-186">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress</span><span class="sxs-lookup"><span data-stu-id="d13c8-186">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress</span></span> |<span data-ttu-id="d13c8-187">E-post</span><span class="sxs-lookup"><span data-stu-id="d13c8-187">email</span></span> 
   
     >[!NOTE]
     ><span data-ttu-id="d13c8-188">Konfigurationen av attributen beror på hur program på HCP utvecklas, d.v.s. vilka attribut som de förväntas i SAML-svaret och under vilka namn (Principal attribut) de har åtkomst till det här attributet i koden.</span><span class="sxs-lookup"><span data-stu-id="d13c8-188">The configuration of the Attributes depends on how the application(s) on HCP are developed, i.e. which attribute(s) they expect in the SAML response and under which name (Principal Attribute) they access this attribute in the code.</span></span>
     > 
     >  

    1.  <span data-ttu-id="d13c8-189">Den **standard attributet** i skärmbilden är precis som illustration.</span><span class="sxs-lookup"><span data-stu-id="d13c8-189">The **Default Attribute** in the screenshot is just for illustration purposes.</span></span> <span data-ttu-id="d13c8-190">Det krävs inte för att göra scenariot som fungerar.</span><span class="sxs-lookup"><span data-stu-id="d13c8-190">It is not required to make the scenario work.</span></span>   
    2.  <span data-ttu-id="d13c8-191">Namn och värden för **Principal attributet** visas i skärmbilden beror på hur programmet utvecklas.</span><span class="sxs-lookup"><span data-stu-id="d13c8-191">The names and values for **Principal Attribute** shown in the screenshot depend on how the application is developed.</span></span> <span data-ttu-id="d13c8-192">Det är möjligt att ditt program kräver olika mappningar.</span><span class="sxs-lookup"><span data-stu-id="d13c8-192">It is possible that your application requires different mappings.</span></span>
     
13. <span data-ttu-id="d13c8-193">I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på SAP HANA Molnplattform** dialog sidan Välj konfiguration för enkel inloggning bekräftelsen och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-193">In the Azure classic portal, on the **Configure single sign-on at SAP HANA Cloud Platform** dialogue page, select the single sign-on configuration confirmation, and then click **Complete**.</span></span>
    
    <span data-ttu-id="d13c8-194">![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="d13c8-194">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configure Single Sign-On")</span></span>

###<a name="assertion-based-groups"></a><span data-ttu-id="d13c8-195">Assertion-baserade grupper</span><span class="sxs-lookup"><span data-stu-id="d13c8-195">Assertion-based groups</span></span>
<span data-ttu-id="d13c8-196">Som ett valfritt steg kan du konfigurera assertion-baserade grupper för identitetsprovider Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d13c8-196">As an optional step, you can configure assertion-based groups for your Azure Active Directory Identity Provider.</span></span>

<span data-ttu-id="d13c8-197">Om du använder grupper på Molnplattform för SAP HANA kan du dynamiskt tilldela en eller flera användare till en eller flera roller i din Molnplattform för SAP HANA-program, fastställs av värdena på attributen i SAML 2.0-kontroll.</span><span class="sxs-lookup"><span data-stu-id="d13c8-197">Using groups on SAP HANA Cloud Platform allows you to dynamically assign one or more users to one or more roles in your SAP HANA Cloud Platform applications, determined by values of attributes in the SAML 2.0 assertion.</span></span> 

<span data-ttu-id="d13c8-198">Om kontrollen innehåller attributet exempelvis ”*kontraktet = temporära*”, kan du vill att alla berörda användare som ska läggas till i gruppen ”*tillfälliga*”.</span><span class="sxs-lookup"><span data-stu-id="d13c8-198">For example, if the assertion contains the attribute "*contract=temporary*", you may want all affected users to be added to the group "*TEMPORARY*".</span></span> <span data-ttu-id="d13c8-199">Gruppen ”*tillfälliga*” kan innehålla en eller flera roller från en eller flera program som distribueras i Molnplattform för SAP HANA-konto.</span><span class="sxs-lookup"><span data-stu-id="d13c8-199">The group "*TEMPORARY*" may contain one or more roles from one or more applications deployed in your SAP HANA Cloud Platform account.</span></span>
 
<span data-ttu-id="d13c8-200">Använda assertion-baserade grupper när du vill tilldela många användare samtidigt till en eller flera roller för program i Molnplattform för SAP HANA-konto.</span><span class="sxs-lookup"><span data-stu-id="d13c8-200">Use assertion-based groups when you want to simultaneously assign many users to one or more roles of applications in your SAP HANA Cloud Platform account.</span></span> <span data-ttu-id="d13c8-201">Om du vill tilldela specifika roller bara ett enda eller litet antal användare, rekommenderar vi att tilldela dem direkt i den ”**tillstånd**” för SAP HANA Molnplattform cockpit.</span><span class="sxs-lookup"><span data-stu-id="d13c8-201">If you want to assign only a single or small number of users to specific roles, we recommend assigning them directly in the “**Authorizations**” tab of the SAP HANA Cloud Platform cockpit.</span></span>

## <a name="assign-a-role-to-a-user"></a><span data-ttu-id="d13c8-202">Tilldela en roll till en användare</span><span class="sxs-lookup"><span data-stu-id="d13c8-202">Assign a role to a user</span></span>
<span data-ttu-id="d13c8-203">För att aktivera Azure AD-användare att logga in på Molnplattform för SAP HANA, måste du tilldela roller i Molnplattform för SAP HANA dem.</span><span class="sxs-lookup"><span data-stu-id="d13c8-203">In order to enable Azure AD users to log into SAP HANA Cloud Platform, you must assign roles in the SAP HANA Cloud Platform to them.</span></span>

<span data-ttu-id="d13c8-204">**Utför följande steg om du vill tilldela en roll till en användare:**</span><span class="sxs-lookup"><span data-stu-id="d13c8-204">**To assign a role to a user, perform the following steps:**</span></span>

1. <span data-ttu-id="d13c8-205">Logga in på ditt **Molnplattform för SAP HANA** cockpit.</span><span class="sxs-lookup"><span data-stu-id="d13c8-205">Log in to your **SAP HANA Cloud Platform** cockpit.</span></span>
2. <span data-ttu-id="d13c8-206">Utför följande:</span><span class="sxs-lookup"><span data-stu-id="d13c8-206">Perform the following:</span></span>
   
   <span data-ttu-id="d13c8-207">![Tillstånd](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "tillstånd")</span><span class="sxs-lookup"><span data-stu-id="d13c8-207">![Authorizations](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Authorizations")</span></span>
   
  1. <span data-ttu-id="d13c8-208">Klicka på **auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-208">Click **Authorization**.</span></span>
  2. <span data-ttu-id="d13c8-209">Klicka på den **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="d13c8-209">Click the **Users** tab.</span></span>
  3. <span data-ttu-id="d13c8-210">I den **användaren** textruta Skriv användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="d13c8-210">In the **User** textbox, type the user’s email address.</span></span>
  4. <span data-ttu-id="d13c8-211">Klicka på **tilldela** tilldela användaren till en roll.</span><span class="sxs-lookup"><span data-stu-id="d13c8-211">Click **Assign** to assign the user to a role.</span></span>
  5. <span data-ttu-id="d13c8-212">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-212">Click **Save**.</span></span>

## <a name="assign-users"></a><span data-ttu-id="d13c8-213">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="d13c8-213">Assign users</span></span>
<span data-ttu-id="d13c8-214">Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.</span><span class="sxs-lookup"><span data-stu-id="d13c8-214">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="d13c8-215">**Utför följande steg för att tilldela användare till SAP HANA Molnplattform:**</span><span class="sxs-lookup"><span data-stu-id="d13c8-215">**To assign users to SAP HANA Cloud Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="d13c8-216">Skapa ett testkonto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d13c8-216">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="d13c8-217">På den **Molnplattform för SAP HANA** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="d13c8-217">On the **SAP HANA Cloud Platform** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="d13c8-218">![Tilldela användare](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="d13c8-218">![Assign Users](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Assign Users")</span></span>
3. <span data-ttu-id="d13c8-219">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.</span><span class="sxs-lookup"><span data-stu-id="d13c8-219">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="d13c8-220">![Ja](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="d13c8-220">![Yes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="d13c8-221">Om du vill testa inställningarna för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d13c8-221">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="d13c8-222">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d13c8-222">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

