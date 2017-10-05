---
title: "Självstudier: Azure Active Directory-integrering med Benefitsolver | Microsoft Docs"
description: "Lär dig hur du använder Benefitsolver med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: cf4529b1-3fb6-4475-82b7-2ceedcb70b3c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 8a13dd5ebd872f86247158379b28bc291a9c9d83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a><span data-ttu-id="42eab-103">Självstudier: Azure Active Directory-integrering med Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="42eab-103">Tutorial: Azure Active Directory integration with Benefitsolver</span></span>
<span data-ttu-id="42eab-104">Syftet med den här kursen är att visa integreringen av Azure och Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="42eab-104">The objective of this tutorial is to show the integration of Azure and Benefitsolver.</span></span>  

<span data-ttu-id="42eab-105">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="42eab-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="42eab-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="42eab-106">A valid Azure subscription</span></span>
* <span data-ttu-id="42eab-107">En Benefitsolver enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="42eab-107">A Benefitsolver single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="42eab-108">Den här kursen Azure AD-användare som du har tilldelat Benefitsolver kommer att kunna enkel inloggning till programmet med hjälp av den [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="42eab-108">After completing this tutorial, the Azure AD users you have assigned to Benefitsolver will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="42eab-109">Det scenario som beskrivs i den här kursen består av följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="42eab-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="42eab-110">Aktivera programintegrationstyp för Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="42eab-110">Enabling the application integration for Benefitsolver</span></span>
2. <span data-ttu-id="42eab-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="42eab-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="42eab-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="42eab-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="42eab-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="42eab-113">Assigning users</span></span>

<span data-ttu-id="42eab-114">![Scenariot](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="42eab-114">![Scenario](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-benefitsolver"></a><span data-ttu-id="42eab-115">Aktivera programintegrationstyp för Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="42eab-115">Enabling the application integration for Benefitsolver</span></span>
<span data-ttu-id="42eab-116">Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="42eab-116">The objective of this section is to outline how to enable the application integration for Benefitsolver.</span></span>

### <a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a><span data-ttu-id="42eab-117">Utför följande steg om du vill aktivera programmet-integrering för Benefitsolver:</span><span class="sxs-lookup"><span data-stu-id="42eab-117">To enable the application integration for Benefitsolver, perform the following steps:</span></span>
1. <span data-ttu-id="42eab-118">I den klassiska Azure-portalen i det vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="42eab-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="42eab-119">![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="42eab-119">![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="42eab-120">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="42eab-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="42eab-121">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="42eab-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="42eab-122">![Program](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="42eab-122">![Applications](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="42eab-123">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="42eab-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="42eab-124">![Lägg till program](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="42eab-124">![Add application](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="42eab-125">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="42eab-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="42eab-126">![Lägga till ett program från gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="42eab-126">![Add an application from gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="42eab-127">I den **sökrutan**, typen **Benefitsolver**.</span><span class="sxs-lookup"><span data-stu-id="42eab-127">In the **search box**, type **Benefitsolver**.</span></span>
   
   <span data-ttu-id="42eab-128">![Programgalleriet](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="42eab-128">![Application Gallery](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Application Gallery")</span></span>
7. <span data-ttu-id="42eab-129">I resultatfönstret, Välj **Benefitsolver**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="42eab-129">In the results pane, select **Benefitsolver**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="42eab-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span><span class="sxs-lookup"><span data-stu-id="42eab-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="42eab-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="42eab-131">Configure single sign-on</span></span>

<span data-ttu-id="42eab-132">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till Benefitsolver med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="42eab-132">The objective of this section is to outline how to enable users to authenticate to Benefitsolver with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="42eab-133">Tillämpningsprogrammet Benefitsolver förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till attributmappningar till din **saml-token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="42eab-133">Your Benefitsolver application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **saml token attributes** configuration.</span></span> 

<span data-ttu-id="42eab-134">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="42eab-134">The following screenshot shows an example for this.</span></span>

<span data-ttu-id="42eab-135">![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="42eab-135">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>

<span data-ttu-id="42eab-136">**Utför följande steg för att konfigurera enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="42eab-136">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="42eab-137">I den klassiska Azure-portalen på den **Benefitsolver** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="42eab-137">In the Azure classic portal, on the **Benefitsolver** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="42eab-138">![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="42eab-138">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="42eab-139">På den **hur vill du att användarna kan logga in på Benefitsolver** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="42eab-139">On the **How would you like users to sign on to Benefitsolver** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="42eab-140">![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="42eab-140">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="42eab-141">På den **konfigurera Appinställningar** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="42eab-141">On the **Configure App Settings** page, perform the following steps:</span></span>
   
   <span data-ttu-id="42eab-142">![Konfigurera Appinställningar för](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "konfigurera App-inställningar")</span><span class="sxs-lookup"><span data-stu-id="42eab-142">![Configure App Settings](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Configure App Settings")</span></span>
   
   1. <span data-ttu-id="42eab-143">I den **logga URL** textruta typen **http://azure.benefitsolver.com**.</span><span class="sxs-lookup"><span data-stu-id="42eab-143">In the **Sign On URL** textbox, type **http://azure.benefitsolver.com**.</span></span>
   2. <span data-ttu-id="42eab-144">I den **Reply URL** textruta typen **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span><span class="sxs-lookup"><span data-stu-id="42eab-144">In the **Reply URL** textbox, type **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span></span>  
   3. <span data-ttu-id="42eab-145">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="42eab-145">Click **Next**.</span></span>
4. <span data-ttu-id="42eab-146">På den **Konfigurera enkel inloggning på Benefitsolver** att hämta metadata, klickar du på **hämtar metadata**, och sedan spara metadatafilen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="42eab-146">On the **Configure single sign-on at Benefitsolver** page, to download your metadata, click **Download metadata**, and then save the metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="42eab-147">![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="42eab-147">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="42eab-148">Skicka hämtade metadatafilen till supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="42eab-148">Send the downloaded metadata file to your Benefitsolver support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="42eab-149">Supportteamet Benefitsolver har att göra den faktiska SSO-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="42eab-149">Your Benefitsolver support team has to do the actual SSO configuration.</span></span> <span data-ttu-id="42eab-150">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="42eab-150">You will get a notification when SSO has been enabled for your subscription.</span></span>
   >

6. <span data-ttu-id="42eab-151">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="42eab-151">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="42eab-152">![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="42eab-152">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="42eab-153">Klicka på menyn högst upp **attribut** att öppna den **SAML-Token attribut** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="42eab-153">In the menu on the top, click **Attributes** to open the **SAML Token Attributes** dialog.</span></span>
   
   <span data-ttu-id="42eab-154">![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="42eab-154">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attributes")</span></span>
8. <span data-ttu-id="42eab-155">Utför följande steg för att lägga till nödvändiga attributmappning:</span><span class="sxs-lookup"><span data-stu-id="42eab-155">To add the required attribute mappings, perform the following steps:</span></span>
   
   <span data-ttu-id="42eab-156">![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="42eab-156">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>
   
   | <span data-ttu-id="42eab-157">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="42eab-157">Attribute Name</span></span> | <span data-ttu-id="42eab-158">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="42eab-158">Attribute Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="42eab-159">ClientID</span><span class="sxs-lookup"><span data-stu-id="42eab-159">ClientID</span></span> |<span data-ttu-id="42eab-160">Du måste hämta det här värdet från supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="42eab-160">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="42eab-161">ClientKey</span><span class="sxs-lookup"><span data-stu-id="42eab-161">ClientKey</span></span> |<span data-ttu-id="42eab-162">Du måste hämta det här värdet från supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="42eab-162">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="42eab-163">LogoutURL</span><span class="sxs-lookup"><span data-stu-id="42eab-163">LogoutURL</span></span> |<span data-ttu-id="42eab-164">Du måste hämta det här värdet från supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="42eab-164">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="42eab-165">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="42eab-165">EmployeeID</span></span> |<span data-ttu-id="42eab-166">Du måste hämta det här värdet från supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="42eab-166">You need to get this value from your Benefitsolver support team.</span></span> |
   
   1. <span data-ttu-id="42eab-167">För varje datarad i tabellen ovan, klickar du på **lägga till användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="42eab-167">For each data row in the table above, click **add user attribute**.</span></span>
   2. <span data-ttu-id="42eab-168">I den **attributnamn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="42eab-168">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
   3. <span data-ttu-id="42eab-169">I den **attributvärdet** textruta Välj attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="42eab-169">In the **Attribute Value** textbox, select the attribute value shown for that row.</span></span>
   4. <span data-ttu-id="42eab-170">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="42eab-170">Click **Complete**.</span></span>
9. <span data-ttu-id="42eab-171">Klicka på **tillämpa ändringarna**.</span><span class="sxs-lookup"><span data-stu-id="42eab-171">Click **Apply Changes**.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="42eab-172">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="42eab-172">Configure user provisioning</span></span>
<span data-ttu-id="42eab-173">För att aktivera Azure AD-användare att logga in på Benefitsolver etableras de i Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="42eab-173">In order to enable Azure AD users to log into Benefitsolver, they must be provisioned into Benefitsolver.</span></span>  

<span data-ttu-id="42eab-174">När det gäller Benefitsolver är data om anställda i ditt program fyllts via en inventering från personalinformationssystemet (vanligtvis varje natt).</span><span class="sxs-lookup"><span data-stu-id="42eab-174">In the case of Benefitsolver, employee data is in your application populated through a Census file from your HRIS system (typically nightly).</span></span>  

>[!NOTE]
><span data-ttu-id="42eab-175">Du kan använda något annat Benefitsolver användarens konto skapas verktyg eller API: er som tillhandahålls av Benefitsolver att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="42eab-175">You can use any other Benefitsolver user account creation tools or APIs provided by Benefitsolver to provision AAD user accounts.</span></span> 
> 

## <a name="assigning-users"></a><span data-ttu-id="42eab-176">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="42eab-176">Assigning users</span></span>
<span data-ttu-id="42eab-177">Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.</span><span class="sxs-lookup"><span data-stu-id="42eab-177">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

### <a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a><span data-ttu-id="42eab-178">Utför följande steg om du vill tilldela Benefitsolver användare:</span><span class="sxs-lookup"><span data-stu-id="42eab-178">To assign users to Benefitsolver, perform the following steps:</span></span>
1. <span data-ttu-id="42eab-179">Skapa ett testkonto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="42eab-179">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="42eab-180">På den ** Benefitsolver ** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="42eab-180">On the **Benefitsolver **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="42eab-181">![Tilldela användare](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="42eab-181">![Assign Users](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Assign Users")</span></span>
3. <span data-ttu-id="42eab-182">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.</span><span class="sxs-lookup"><span data-stu-id="42eab-182">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="42eab-183">![Ja](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="42eab-183">![Yes](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="42eab-184">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="42eab-184">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="42eab-185">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="42eab-185">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

