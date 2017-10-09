---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA Molnplattform | Microsoft Docs"
description: "Lär dig hur toouse SAP HANA Molnplattform med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: cc6610969b1c7b08f776e6969a5977fc75eb9ab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a><span data-ttu-id="29094-103">Självstudier: Azure Active Directory-integration med SAP HANA-molnplattformen</span><span class="sxs-lookup"><span data-stu-id="29094-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform</span></span>
<span data-ttu-id="29094-104">hello syftet med den här kursen är tooshow hello integrering av Azure- och Molnplattform för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="29094-104">hello objective of this tutorial is tooshow hello integration of Azure and SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="29094-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="29094-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="29094-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="29094-106">A valid Azure subscription</span></span>
* <span data-ttu-id="29094-107">En Molnplattform för SAP HANA-konto</span><span class="sxs-lookup"><span data-stu-id="29094-107">A SAP HANA Cloud Platform account</span></span>

<span data-ttu-id="29094-108">Den här kursen hello Azure AD-användare som du har tilldelat tooSAP HANA Molnplattform kommer att kunna toosingle logga till hello program med hjälp av hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="29094-108">After completing this tutorial, hello Azure AD users you have assigned tooSAP HANA Cloud Platform will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

>[!IMPORTANT]
><span data-ttu-id="29094-109">Du måste toodeploy ditt eget program eller prenumerera tooan program på din SAP HANA-Molnplattform konto tootest enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="29094-109">You need toodeploy your own application or subscribe tooan application on your SAP HANA Cloud Platform account tootest single sign on.</span></span> <span data-ttu-id="29094-110">Ett program ska distribueras i hello-konto i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="29094-110">In this tutorial, an application is deployed in hello account.</span></span>
> 
> 

<span data-ttu-id="29094-111">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="29094-111">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="29094-112">Aktivera hello programintegrationstyp för SAP HANA Molnplattform</span><span class="sxs-lookup"><span data-stu-id="29094-112">Enabling hello application integration for SAP HANA Cloud Platform</span></span>
2. <span data-ttu-id="29094-113">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="29094-113">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="29094-114">Tilldela en roll tooa användare</span><span class="sxs-lookup"><span data-stu-id="29094-114">Assigning a role tooa user</span></span>
4. <span data-ttu-id="29094-115">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="29094-115">Assigning users</span></span>

<span data-ttu-id="29094-116">![Scenariot](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="29094-116">![Scenario](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-sap-hana-cloud-platform"></a><span data-ttu-id="29094-117">Aktivera hello programintegrationstyp för SAP HANA Molnplattform</span><span class="sxs-lookup"><span data-stu-id="29094-117">Enabling hello application integration for SAP HANA Cloud Platform</span></span>
<span data-ttu-id="29094-118">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för SAP HANA Molnplattform.</span><span class="sxs-lookup"><span data-stu-id="29094-118">hello objective of this section is toooutline how tooenable hello application integration for SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="29094-119">**tooenable hello programintegrationstyp för SAP HANA Molnplattform, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="29094-119">**tooenable hello application integration for SAP HANA Cloud Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="29094-120">I hello Azure Management Portal på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="29094-120">In hello Azure Management Portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="29094-121">![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="29094-121">![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="29094-122">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="29094-122">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="29094-123">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="29094-123">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="29094-124">![Program](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="29094-124">![Applications](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="29094-125">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="29094-125">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="29094-126">![Lägg till program](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="29094-126">![Add application](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="29094-127">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="29094-127">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="29094-128">![Lägga till ett program från gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="29094-128">![Add an application from gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="29094-129">I hello **sökrutan**, typen **Molnplattform för SAP HANA**.</span><span class="sxs-lookup"><span data-stu-id="29094-129">In hello **search box**, type **SAP HANA Cloud Platform**.</span></span>
   
    <span data-ttu-id="29094-130">![Programgalleriet](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="29094-130">![Application Gallery](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Application Gallery")</span></span>
7. <span data-ttu-id="29094-131">I resultatfönstret hello väljer **Molnplattform för SAP HANA**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="29094-131">In hello results pane, select **SAP HANA Cloud Platform**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="29094-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span><span class="sxs-lookup"><span data-stu-id="29094-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="29094-133">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="29094-133">Configure single sign-on</span></span>

<span data-ttu-id="29094-134">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooSAP HANA Molnplattform med ett konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="29094-134">hello objective of this section is toooutline how tooenable users tooauthenticate tooSAP HANA Cloud Platform with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="29094-135">Som en del av den här proceduren är obligatoriska tooupload en Base64-kodade certifikatet tooyour Molnplattform för SAP HANA-klient.</span><span class="sxs-lookup"><span data-stu-id="29094-135">As part of this procedure, you are required tooupload a base-64 encoded certificate tooyour SAP HANA Cloud Platform tenant.</span></span>  

<span data-ttu-id="29094-136">Om du inte är bekant med den här proceduren, se [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="29094-136">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="29094-137">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="29094-137">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="29094-138">I hello klassiska Azure-portalen på hello **Molnplattform för SAP HANA** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29094-138">In hello Azure classic portal, on hello **SAP HANA Cloud Platform** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="29094-139">![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="29094-139">![Configure single sign-on](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="29094-140">På hello **hur skulle du som användare toosign på tooSAP HANA Molnplattform** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="29094-140">On hello **How would you like users toosign on tooSAP HANA Cloud Platform** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="29094-141">![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="29094-141">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="29094-142">Inloggning i en annan webbläsarfönster toohello SAP HANA Cloud Platform Cockpit på https://account. \<liggande värden\>.ondemand.com/cockpit (t.ex.: *https://account.hanatrial.ondemand.com/cockpit*).</span><span class="sxs-lookup"><span data-stu-id="29094-142">In a different web browser window, sign on toohello SAP HANA Cloud Platform Cockpit at https://account.\<landscape host\>.ondemand.com/cockpit (e.g.: *https://account.hanatrial.ondemand.com/cockpit*).</span></span>
4. <span data-ttu-id="29094-143">Klicka på hello **förtroende** fliken.</span><span class="sxs-lookup"><span data-stu-id="29094-143">Click hello **Trust** tab.</span></span>
   
    <span data-ttu-id="29094-144">![Förtroende](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "förtroende")</span><span class="sxs-lookup"><span data-stu-id="29094-144">![Trust](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Trust")</span></span>
5. <span data-ttu-id="29094-145">Utför följande hello under förtroende management:</span><span class="sxs-lookup"><span data-stu-id="29094-145">In trust management section, perform hello following steps:</span></span>
   
    <span data-ttu-id="29094-146">![Hämta Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "hämta Metadata")</span><span class="sxs-lookup"><span data-stu-id="29094-146">![Get Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Get Metadata")</span></span>
   
   1. <span data-ttu-id="29094-147">Klicka på hello **lokal-leverantör** fliken.</span><span class="sxs-lookup"><span data-stu-id="29094-147">Click hello **Local Service Provider** tab.</span></span>
   2. <span data-ttu-id="29094-148">toodownload hello Molnplattform för SAP HANA metadatafil klickar du på **hämta Metadata**.</span><span class="sxs-lookup"><span data-stu-id="29094-148">toodownload hello SAP HANA Cloud Platform metadata file, click **Get Metadata**.</span></span>
6. <span data-ttu-id="29094-149">I hello Azure Active klassiska portalen på hello **konfigurera App-URL** , utför följande steg hello, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="29094-149">In hello Azure Active classic portal, on hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
    <span data-ttu-id="29094-150">![Konfigurera App-URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="29094-150">![Configure App URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configure App URL")</span></span>
   
   1. <span data-ttu-id="29094-151">I hello **logga URL** textruta typen hello URL som används av din toosign för användare i din **Molnplattform för SAP HANA** program.</span><span class="sxs-lookup"><span data-stu-id="29094-151">In hello **Sign On URL** textbox, type hello URL used by your users toosign into your **SAP HANA Cloud Platform** application.</span></span> <span data-ttu-id="29094-152">Det här är hello kontospecifikt URL för en skyddad resurs i ditt Molnplattform för SAP HANA-program.</span><span class="sxs-lookup"><span data-stu-id="29094-152">This is hello account-specific URL of a protected resource in your SAP HANA Cloud Platform application.</span></span> <span data-ttu-id="29094-153">hello URL är baserad på hello följer mönstret: *https://\<applicationName\>\<accountName\>.\< liggande värden\>.ondemand.com/\<sökväg\_till\_skyddade\_resurs\>*  (t.ex.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span><span class="sxs-lookup"><span data-stu-id="29094-153">hello URL is based on hello following pattern: *https://\<applicationName\>\<accountName\>.\<landscape host\>.ondemand.com/\<path\_to\_protected\_resource\>* (e.g.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="29094-154">Detta är hello URL: en i din Molnplattform för SAP HANA-program som kräver hello användaren tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="29094-154">This is hello URL in your SAP HANA Cloud Platform application that requires hello user tooauthenticate.</span></span>
     > 

   2. <span data-ttu-id="29094-155">Öppna hello hämtas Molnplattform för SAP HANA-metadatafil och välj sedan hello **ns3:AssertionConsumerService** tagg.</span><span class="sxs-lookup"><span data-stu-id="29094-155">Open hello downloaded SAP HANA Cloud Platform metadata file, and then locate hello **ns3:AssertionConsumerService** tag.</span></span>
   3. <span data-ttu-id="29094-156">Kopiera hello värdet för hello **plats** attributet och klistra in den i hello **Reply-URL för SAP HANA Cloud Platform** textruta.</span><span class="sxs-lookup"><span data-stu-id="29094-156">Copy hello value of hello **Location** attribute, and then paste it into hello **SAP HANA Cloud Platform Reply URL** textbox.</span></span>

7. <span data-ttu-id="29094-157">På hello **Konfigurera enkel inloggning på SAP HANA Molnplattform** sida, toodownload metadata, klickar du på **hämtar metadata**, och spara sedan hello på datorn.</span><span class="sxs-lookup"><span data-stu-id="29094-157">On hello **Configure single sign-on at SAP HANA Cloud Platform** page, toodownload your metadata, click **Download metadata**, and then save hello file on your computer.</span></span>
   
    <span data-ttu-id="29094-158">![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="29094-158">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configure Single Sign-On")</span></span>
8. <span data-ttu-id="29094-159">På hello SAP HANA Cloud Platform Cockpit i hello **lokal-leverantör** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="29094-159">On hello SAP HANA Cloud Platform Cockpit, in hello **Local Service Provider** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="29094-160">![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "förtroende för hantering")</span><span class="sxs-lookup"><span data-stu-id="29094-160">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Trust Management")</span></span>
   
  1. <span data-ttu-id="29094-161">Klicka på **Redigera**.</span><span class="sxs-lookup"><span data-stu-id="29094-161">Click **Edit**.</span></span>
  2. <span data-ttu-id="29094-162">Som **konfigurationstypen**väljer **anpassad**.</span><span class="sxs-lookup"><span data-stu-id="29094-162">As **Configuration Type**, select **Custom**.</span></span>
  3. <span data-ttu-id="29094-163">Som **lokala providernamn**, lämna hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="29094-163">As **Local Provider Name**, leave hello default value.</span></span>
  4. <span data-ttu-id="29094-164">toogenerate en **signeringsnyckeln** och en **signeringscertifikat** nyckelpar, klickar du på **Generera nyckel**.</span><span class="sxs-lookup"><span data-stu-id="29094-164">toogenerate a **Signing Key** and a **Signing Certificate** key pair, click **Generate Key Pair**.</span></span>
  5. <span data-ttu-id="29094-165">Som **huvudnamn spridningen**väljer **inaktiverade**.</span><span class="sxs-lookup"><span data-stu-id="29094-165">As **Principal Propagation**, select **Disabled**.</span></span>
  6. <span data-ttu-id="29094-166">Som **kraft autentisering**väljer **inaktiverade**.</span><span class="sxs-lookup"><span data-stu-id="29094-166">As **Force Authentication**, select **Disabled**.</span></span>
  7. <span data-ttu-id="29094-167">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="29094-167">Click **Save**.</span></span>

9. <span data-ttu-id="29094-168">Klicka på hello **betrodda identitetsleverantör** fliken och klicka sedan på **lägga till betrodda identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="29094-168">Click hello **Trusted Identity Provider** tab, and then click **Add Trusted Identity Provider**.</span></span>
   
    <span data-ttu-id="29094-169">![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "förtroende för hantering")</span><span class="sxs-lookup"><span data-stu-id="29094-169">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Trust Management")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="29094-170">toomanage hello lista över betrodda identitetsleverantörer, behöver du toohave valt hello typen för anpassad konfiguration i hello lokal-leverantör avsnitt.</span><span class="sxs-lookup"><span data-stu-id="29094-170">toomanage hello list of trusted identity providers, you need toohave chosen hello Custom configuration type in hello Local Service Provider section.</span></span> <span data-ttu-id="29094-171">Konfiguration av standardtyp har du en redigeras och implicit förtroende toohello SAP-ID-tjänst.</span><span class="sxs-lookup"><span data-stu-id="29094-171">For Default configuration type, you have a non-editable and implicit trust toohello SAP ID Service.</span></span> <span data-ttu-id="29094-172">Ingen har du inte för förtroendeinställningar.</span><span class="sxs-lookup"><span data-stu-id="29094-172">For None, you don't have any trust settings.</span></span>
    > 
    > 

10. <span data-ttu-id="29094-173">Klicka på hello **allmänna** fliken och klicka sedan på **Bläddra** tooupload hello hämtas metadatafil.</span><span class="sxs-lookup"><span data-stu-id="29094-173">Click hello **General** tab, and then click **Browse** tooupload hello downloaded metadata file.</span></span>
    
    <span data-ttu-id="29094-174">![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "förtroende för hantering")</span><span class="sxs-lookup"><span data-stu-id="29094-174">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Trust Management")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="29094-175">Efter överföring hello metadatafil hello värden för **URL för enkel inloggning**, **URL för enkel logga ut** och **signeringscertifikat** fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="29094-175">After uploading hello metadata file, hello values for **Single Sign-on URL**, **Single Logout URL** and **Signing Certificate** are populated automatically.</span></span>
    > 
    > 

11. <span data-ttu-id="29094-176">Klicka på hello **attribut** fliken.</span><span class="sxs-lookup"><span data-stu-id="29094-176">Click hello **Attributes** tab.</span></span>
12. <span data-ttu-id="29094-177">På hello **attribut** fliken, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="29094-177">On hello **Attributes** tab, perform hello following step:</span></span>
    
    <span data-ttu-id="29094-178">![Attribut](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="29094-178">![Attributes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attributes")</span></span> 
  * <span data-ttu-id="29094-179">Klicka på **Add Assertion-Based attributet**, och Lägg sedan till hello följande assertion-baserade attribut:</span><span class="sxs-lookup"><span data-stu-id="29094-179">Click **Add Assertion-Based Attribute**, and then add hello following assertion-based attributes:</span></span>
       
    | <span data-ttu-id="29094-180">Attributet för kontrollen</span><span class="sxs-lookup"><span data-stu-id="29094-180">Assertion Attribute</span></span> | <span data-ttu-id="29094-181">Huvudnamn attribut</span><span class="sxs-lookup"><span data-stu-id="29094-181">Principal Attribute</span></span> |
    | --- | --- |
    | <span data-ttu-id="29094-182">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName</span><span class="sxs-lookup"><span data-stu-id="29094-182">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname</span></span> |<span data-ttu-id="29094-183">Förnamn</span><span class="sxs-lookup"><span data-stu-id="29094-183">firstname</span></span> |
    | <span data-ttu-id="29094-184">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname</span><span class="sxs-lookup"><span data-stu-id="29094-184">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname</span></span> |<span data-ttu-id="29094-185">Efternamn</span><span class="sxs-lookup"><span data-stu-id="29094-185">lastname</span></span> |
    | <span data-ttu-id="29094-186">http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress</span><span class="sxs-lookup"><span data-stu-id="29094-186">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress</span></span> |<span data-ttu-id="29094-187">E-post</span><span class="sxs-lookup"><span data-stu-id="29094-187">email</span></span> 
   
     >[!NOTE]
     ><span data-ttu-id="29094-188">hello konfiguration av hello attribut beror på hur hello program på HCP utvecklas, d.v.s. vilka attribut som de förväntar sig i hello SAML-svar och under vilka namn (Principal attribut) de har åtkomst till det här attributet i hello kod.</span><span class="sxs-lookup"><span data-stu-id="29094-188">hello configuration of hello Attributes depends on how hello application(s) on HCP are developed, i.e. which attribute(s) they expect in hello SAML response and under which name (Principal Attribute) they access this attribute in hello code.</span></span>
     > 
     >  

    1.  <span data-ttu-id="29094-189">Hej **standard attributet** i hello skärmbild är precis som illustration.</span><span class="sxs-lookup"><span data-stu-id="29094-189">hello **Default Attribute** in hello screenshot is just for illustration purposes.</span></span> <span data-ttu-id="29094-190">Det är inte krävs toomake hello scenariot arbete.</span><span class="sxs-lookup"><span data-stu-id="29094-190">It is not required toomake hello scenario work.</span></span>   
    2.  <span data-ttu-id="29094-191">Hej namn och värden för **Principal attributet** visas i hello skärmbild beror på hur hello program utvecklas.</span><span class="sxs-lookup"><span data-stu-id="29094-191">hello names and values for **Principal Attribute** shown in hello screenshot depend on how hello application is developed.</span></span> <span data-ttu-id="29094-192">Det är möjligt att ditt program kräver olika mappningar.</span><span class="sxs-lookup"><span data-stu-id="29094-192">It is possible that your application requires different mappings.</span></span>
     
13. <span data-ttu-id="29094-193">I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på SAP HANA Molnplattform** dialog sidan Välj hello konfiguration för enkel inloggning bekräftelse och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="29094-193">In hello Azure classic portal, on hello **Configure single sign-on at SAP HANA Cloud Platform** dialogue page, select hello single sign-on configuration confirmation, and then click **Complete**.</span></span>
    
    <span data-ttu-id="29094-194">![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="29094-194">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configure Single Sign-On")</span></span>

###<a name="assertion-based-groups"></a><span data-ttu-id="29094-195">Assertion-baserade grupper</span><span class="sxs-lookup"><span data-stu-id="29094-195">Assertion-based groups</span></span>
<span data-ttu-id="29094-196">Som ett valfritt steg kan du konfigurera assertion-baserade grupper för identitetsprovider Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="29094-196">As an optional step, you can configure assertion-based groups for your Azure Active Directory Identity Provider.</span></span>

<span data-ttu-id="29094-197">Använda grupper för SAP HANA-Molnplattform kan du toodynamically tilldela en eller flera användare tooone eller flera roller i dina Molnplattform för SAP HANA-program som bestäms av värdena på attributen i hello SAML 2.0-kontroll.</span><span class="sxs-lookup"><span data-stu-id="29094-197">Using groups on SAP HANA Cloud Platform allows you toodynamically assign one or more users tooone or more roles in your SAP HANA Cloud Platform applications, determined by values of attributes in hello SAML 2.0 assertion.</span></span> 

<span data-ttu-id="29094-198">Innehåller till exempel om hello assertion hello attributet ”*kontraktet = temporära*”, kan du vill att alla berörda toobe tillagda toohello användargruppen ”*tillfälliga*”.</span><span class="sxs-lookup"><span data-stu-id="29094-198">For example, if hello assertion contains hello attribute "*contract=temporary*", you may want all affected users toobe added toohello group "*TEMPORARY*".</span></span> <span data-ttu-id="29094-199">Hej grupp ”*tillfälliga*” kan innehålla en eller flera roller från en eller flera program som distribueras i Molnplattform för SAP HANA-konto.</span><span class="sxs-lookup"><span data-stu-id="29094-199">hello group "*TEMPORARY*" may contain one or more roles from one or more applications deployed in your SAP HANA Cloud Platform account.</span></span>
 
<span data-ttu-id="29094-200">Använda assertion-baserade grupper när du vill tilldela toosimultaneously många användare tooone eller flera roller av program i Molnplattform för SAP HANA-konto.</span><span class="sxs-lookup"><span data-stu-id="29094-200">Use assertion-based groups when you want toosimultaneously assign many users tooone or more roles of applications in your SAP HANA Cloud Platform account.</span></span> <span data-ttu-id="29094-201">Om du vill att tooassign bara ett enda eller litet antal användare toospecific roller, rekommenderar vi att tilldela dem direkt i hello ”**tillstånd**” Hej Molnplattform för SAP HANA cockpit fliken.</span><span class="sxs-lookup"><span data-stu-id="29094-201">If you want tooassign only a single or small number of users toospecific roles, we recommend assigning them directly in hello “**Authorizations**” tab of hello SAP HANA Cloud Platform cockpit.</span></span>

## <a name="assign-a-role-tooa-user"></a><span data-ttu-id="29094-202">Tilldela en roll tooa användare</span><span class="sxs-lookup"><span data-stu-id="29094-202">Assign a role tooa user</span></span>
<span data-ttu-id="29094-203">I ordning tooenable Azure AD-användare toolog till SAP HANA Molnplattform, måste du tilldela roller i hello SAP HANA Molnplattform toothem.</span><span class="sxs-lookup"><span data-stu-id="29094-203">In order tooenable Azure AD users toolog into SAP HANA Cloud Platform, you must assign roles in hello SAP HANA Cloud Platform toothem.</span></span>

<span data-ttu-id="29094-204">**tooassign en roll tooa användare utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="29094-204">**tooassign a role tooa user, perform hello following steps:**</span></span>

1. <span data-ttu-id="29094-205">Logga in tooyour **Molnplattform för SAP HANA** cockpit.</span><span class="sxs-lookup"><span data-stu-id="29094-205">Log in tooyour **SAP HANA Cloud Platform** cockpit.</span></span>
2. <span data-ttu-id="29094-206">Utför hello följande:</span><span class="sxs-lookup"><span data-stu-id="29094-206">Perform hello following:</span></span>
   
   <span data-ttu-id="29094-207">![Tillstånd](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "tillstånd")</span><span class="sxs-lookup"><span data-stu-id="29094-207">![Authorizations](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Authorizations")</span></span>
   
  1. <span data-ttu-id="29094-208">Klicka på **auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="29094-208">Click **Authorization**.</span></span>
  2. <span data-ttu-id="29094-209">Klicka på hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="29094-209">Click hello **Users** tab.</span></span>
  3. <span data-ttu-id="29094-210">I hello **användaren** textruta typen hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="29094-210">In hello **User** textbox, type hello user’s email address.</span></span>
  4. <span data-ttu-id="29094-211">Klicka på **tilldela** tooassign hello tooa användarroll.</span><span class="sxs-lookup"><span data-stu-id="29094-211">Click **Assign** tooassign hello user tooa role.</span></span>
  5. <span data-ttu-id="29094-212">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="29094-212">Click **Save**.</span></span>

## <a name="assign-users"></a><span data-ttu-id="29094-213">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="29094-213">Assign users</span></span>
<span data-ttu-id="29094-214">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="29094-214">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="29094-215">**tooassign användare tooSAP HANA Molnplattform utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="29094-215">**tooassign users tooSAP HANA Cloud Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="29094-216">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="29094-216">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="29094-217">På hello **Molnplattform för SAP HANA** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="29094-217">On hello **SAP HANA Cloud Platform** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="29094-218">![Tilldela användare](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="29094-218">![Assign Users](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Assign Users")</span></span>
3. <span data-ttu-id="29094-219">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="29094-219">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="29094-220">![Ja](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="29094-220">![Yes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="29094-221">Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="29094-221">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="29094-222">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="29094-222">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

