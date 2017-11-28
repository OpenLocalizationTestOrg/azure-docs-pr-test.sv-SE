---
title: "Självstudier: Azure Active Directory-integrering med Benefitsolver | Microsoft Docs"
description: "Lär dig hur toouse Benefitsolver med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: 5bb8511ef9be1e386956188a93e899d6ebe56ed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a><span data-ttu-id="9b8ca-103">Självstudier: Azure Active Directory-integrering med Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="9b8ca-103">Tutorial: Azure Active Directory integration with Benefitsolver</span></span>
<span data-ttu-id="9b8ca-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-104">hello objective of this tutorial is tooshow hello integration of Azure and Benefitsolver.</span></span>  

<span data-ttu-id="9b8ca-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9b8ca-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="9b8ca-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9b8ca-106">A valid Azure subscription</span></span>
* <span data-ttu-id="9b8ca-107">En Benefitsolver enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="9b8ca-107">A Benefitsolver single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="9b8ca-108">Den här kursen hello Azure AD-användare som du har tilldelat tooBenefitsolver blir toosingle kan logga in på hello program med hjälp av hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9b8ca-108">After completing this tutorial, hello Azure AD users you have assigned tooBenefitsolver will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="9b8ca-109">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="9b8ca-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="9b8ca-110">Aktivera programmet hello-integrering för Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="9b8ca-110">Enabling hello application integration for Benefitsolver</span></span>
2. <span data-ttu-id="9b8ca-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="9b8ca-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="9b8ca-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="9b8ca-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="9b8ca-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="9b8ca-113">Assigning users</span></span>

<span data-ttu-id="9b8ca-114">![Scenariot](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-114">![Scenario](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-benefitsolver"></a><span data-ttu-id="9b8ca-115">Aktivera programmet hello-integrering för Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="9b8ca-115">Enabling hello application integration for Benefitsolver</span></span>
<span data-ttu-id="9b8ca-116">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-116">hello objective of this section is toooutline how tooenable hello application integration for Benefitsolver.</span></span>

### <a name="tooenable-hello-application-integration-for-benefitsolver-perform-hello-following-steps"></a><span data-ttu-id="9b8ca-117">tooenable hello programintegrationstyp för Benefitsolver, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9b8ca-117">tooenable hello application integration for Benefitsolver, perform hello following steps:</span></span>
1. <span data-ttu-id="9b8ca-118">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="9b8ca-119">![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-119">![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="9b8ca-120">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="9b8ca-121">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="9b8ca-122">![Program](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-122">![Applications](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="9b8ca-123">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="9b8ca-124">![Lägg till program](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-124">![Add application](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="9b8ca-125">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="9b8ca-126">![Lägga till ett program från gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-126">![Add an application from gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="9b8ca-127">I hello **sökrutan**, typen **Benefitsolver**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-127">In hello **search box**, type **Benefitsolver**.</span></span>
   
   <span data-ttu-id="9b8ca-128">![Programgalleriet](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-128">![Application Gallery](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Application Gallery")</span></span>
7. <span data-ttu-id="9b8ca-129">I resultatfönstret hello väljer **Benefitsolver**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-129">In hello results pane, select **Benefitsolver**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="9b8ca-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="9b8ca-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b8ca-131">Configure single sign-on</span></span>

<span data-ttu-id="9b8ca-132">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooBenefitsolver med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooBenefitsolver with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="9b8ca-133">Tillämpningsprogrammet Benefitsolver förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **saml-token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-133">Your Benefitsolver application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **saml token attributes** configuration.</span></span> 

<span data-ttu-id="9b8ca-134">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-134">hello following screenshot shows an example for this.</span></span>

<span data-ttu-id="9b8ca-135">![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-135">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>

<span data-ttu-id="9b8ca-136">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9b8ca-136">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="9b8ca-137">I hello klassiska Azure-portalen på hello **Benefitsolver** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-137">In hello Azure classic portal, on hello **Benefitsolver** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="9b8ca-138">![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-138">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="9b8ca-139">På hello **hur skulle du som användare toosign på tooBenefitsolver** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-139">On hello **How would you like users toosign on tooBenefitsolver** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="9b8ca-140">![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-140">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="9b8ca-141">På hello **konfigurera Appinställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9b8ca-141">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
   <span data-ttu-id="9b8ca-142">![Konfigurera Appinställningar för](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "konfigurera App-inställningar")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-142">![Configure App Settings](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Configure App Settings")</span></span>
   
   1. <span data-ttu-id="9b8ca-143">I hello **logga URL** textruta typen **http://azure.benefitsolver.com**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-143">In hello **Sign On URL** textbox, type **http://azure.benefitsolver.com**.</span></span>
   2. <span data-ttu-id="9b8ca-144">I hello **Reply URL** textruta typen **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-144">In hello **Reply URL** textbox, type **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span></span>  
   3. <span data-ttu-id="9b8ca-145">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-145">Click **Next**.</span></span>
4. <span data-ttu-id="9b8ca-146">På hello **Konfigurera enkel inloggning på Benefitsolver** sida, toodownload metadata, klickar du på **hämtar metadata**, och sedan spara hello metadatafil lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-146">On hello **Configure single sign-on at Benefitsolver** page, toodownload your metadata, click **Download metadata**, and then save hello metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="9b8ca-147">![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-147">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="9b8ca-148">Skicka hello hämtas metadata filen tooyour Benefitsolver supportteamet.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-148">Send hello downloaded metadata file tooyour Benefitsolver support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="9b8ca-149">Supportteamet Benefitsolver har toodo hello faktiska SSO konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-149">Your Benefitsolver support team has toodo hello actual SSO configuration.</span></span> <span data-ttu-id="9b8ca-150">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-150">You will get a notification when SSO has been enabled for your subscription.</span></span>
   >

6. <span data-ttu-id="9b8ca-151">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-151">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="9b8ca-152">![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-152">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="9b8ca-153">Hello-menyn överst hello **attribut** tooopen hello **SAML-Token attribut** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-153">In hello menu on hello top, click **Attributes** tooopen hello **SAML Token Attributes** dialog.</span></span>
   
   <span data-ttu-id="9b8ca-154">![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-154">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attributes")</span></span>
8. <span data-ttu-id="9b8ca-155">mappningar av tooadd hello krävs, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9b8ca-155">tooadd hello required attribute mappings, perform hello following steps:</span></span>
   
   <span data-ttu-id="9b8ca-156">![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-156">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>
   
   | <span data-ttu-id="9b8ca-157">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="9b8ca-157">Attribute Name</span></span> | <span data-ttu-id="9b8ca-158">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="9b8ca-158">Attribute Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="9b8ca-159">ClientID</span><span class="sxs-lookup"><span data-stu-id="9b8ca-159">ClientID</span></span> |<span data-ttu-id="9b8ca-160">Du måste tooget det här värdet från supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-160">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="9b8ca-161">ClientKey</span><span class="sxs-lookup"><span data-stu-id="9b8ca-161">ClientKey</span></span> |<span data-ttu-id="9b8ca-162">Du måste tooget det här värdet från supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-162">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="9b8ca-163">LogoutURL</span><span class="sxs-lookup"><span data-stu-id="9b8ca-163">LogoutURL</span></span> |<span data-ttu-id="9b8ca-164">Du måste tooget det här värdet från supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-164">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="9b8ca-165">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="9b8ca-165">EmployeeID</span></span> |<span data-ttu-id="9b8ca-166">Du måste tooget det här värdet från supportteamet Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-166">You need tooget this value from your Benefitsolver support team.</span></span> |
   
   1. <span data-ttu-id="9b8ca-167">För varje datarad i hello tabellen ovan klickar du på **lägga till användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-167">For each data row in hello table above, click **add user attribute**.</span></span>
   2. <span data-ttu-id="9b8ca-168">I hello **attributnamn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-168">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
   3. <span data-ttu-id="9b8ca-169">I hello **attributvärdet** textruta väljer hello-attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-169">In hello **Attribute Value** textbox, select hello attribute value shown for that row.</span></span>
   4. <span data-ttu-id="9b8ca-170">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="9b8ca-170">Click **Complete**.</span></span>
9. <span data-ttu-id="9b8ca-171">Klicka på **tillämpa ändringarna**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-171">Click **Apply Changes**.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="9b8ca-172">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="9b8ca-172">Configure user provisioning</span></span>
<span data-ttu-id="9b8ca-173">I ordning tooenable Azure AD-användare toolog i Benefitsolver, måste de etableras i Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-173">In order tooenable Azure AD users toolog into Benefitsolver, they must be provisioned into Benefitsolver.</span></span>  

<span data-ttu-id="9b8ca-174">Hello gäller Benefitsolver är data om anställda i ditt program fyllts via en inventering från personalinformationssystemet (vanligtvis varje natt).</span><span class="sxs-lookup"><span data-stu-id="9b8ca-174">In hello case of Benefitsolver, employee data is in your application populated through a Census file from your HRIS system (typically nightly).</span></span>  

>[!NOTE]
><span data-ttu-id="9b8ca-175">Du kan använda något annat Benefitsolver användarens konto skapas verktyg eller API: er som tillhandahålls av Benefitsolver tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-175">You can use any other Benefitsolver user account creation tools or APIs provided by Benefitsolver tooprovision AAD user accounts.</span></span> 
> 

## <a name="assigning-users"></a><span data-ttu-id="9b8ca-176">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="9b8ca-176">Assigning users</span></span>
<span data-ttu-id="9b8ca-177">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-177">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

### <a name="tooassign-users-toobenefitsolver-perform-hello-following-steps"></a><span data-ttu-id="9b8ca-178">tooassign användare tooBenefitsolver utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9b8ca-178">tooassign users tooBenefitsolver, perform hello following steps:</span></span>
1. <span data-ttu-id="9b8ca-179">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-179">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="9b8ca-180">På hello ** Benefitsolver ** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-180">On hello **Benefitsolver **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="9b8ca-181">![Tilldela användare](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-181">![Assign Users](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Assign Users")</span></span>
3. <span data-ttu-id="9b8ca-182">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-182">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="9b8ca-183">![Ja](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="9b8ca-183">![Yes](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="9b8ca-184">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9b8ca-184">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="9b8ca-185">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9b8ca-185">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

