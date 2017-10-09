---
title: "Självstudier: Azure Active Directory-integrering med Qualtrics | Microsoft Docs"
description: "Lär dig hur toouse Qualtrics med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 941642e74b90bb13a5ce37ce6665cfa32cd86016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a><span data-ttu-id="0f66e-103">Självstudier: Azure Active Directory-integrering med Qualtrics</span><span class="sxs-lookup"><span data-stu-id="0f66e-103">Tutorial: Azure Active Directory integration with Qualtrics</span></span>
<span data-ttu-id="0f66e-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="0f66e-104">hello objective of this tutorial is tooshow hello integration of Azure and Qualtrics.</span></span>  

<span data-ttu-id="0f66e-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0f66e-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="0f66e-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0f66e-106">A valid Azure subscription</span></span>
* <span data-ttu-id="0f66e-107">En Qualtrics enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="0f66e-107">A Qualtrics single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="0f66e-108">Den här kursen hello Azure AD-användare som du har tilldelat tooQualtrics blir toosingle kan logga in på hello-programmet på din Qualtrics företagets webbplats (service provider initierade inloggning) eller med hjälp av hello [introduktion toohello Åtkomst till panelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0f66e-108">After completing this tutorial, hello Azure AD users you have assigned tooQualtrics will be able toosingle sign into hello application at your Qualtrics company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="0f66e-109">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="0f66e-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="0f66e-110">Aktivera programmet hello-integrering för Qualtrics</span><span class="sxs-lookup"><span data-stu-id="0f66e-110">Enabling hello application integration for Qualtrics</span></span>
2. <span data-ttu-id="0f66e-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="0f66e-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="0f66e-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="0f66e-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="0f66e-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="0f66e-113">Assigning users</span></span>

<span data-ttu-id="0f66e-114">![Scenariot](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="0f66e-114">![Scenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-qualtrics"></a><span data-ttu-id="0f66e-115">Aktivera programmet hello-integrering för Qualtrics</span><span class="sxs-lookup"><span data-stu-id="0f66e-115">Enabling hello application integration for Qualtrics</span></span>
<span data-ttu-id="0f66e-116">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="0f66e-116">hello objective of this section is toooutline how tooenable hello application integration for Qualtrics.</span></span>

<span data-ttu-id="0f66e-117">**tooenable hello programintegrationstyp för Qualtrics, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0f66e-117">**tooenable hello application integration for Qualtrics, perform hello following steps:**</span></span>

1. <span data-ttu-id="0f66e-118">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0f66e-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="0f66e-119">![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="0f66e-119">![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="0f66e-120">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="0f66e-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="0f66e-121">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="0f66e-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="0f66e-122">![Program](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="0f66e-122">![Applications](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="0f66e-123">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="0f66e-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="0f66e-124">![Lägg till program](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="0f66e-124">![Add application](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="0f66e-125">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="0f66e-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="0f66e-126">![Lägga till ett program från gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="0f66e-126">![Add an application from gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="0f66e-127">I hello **sökrutan**, typen **Qualtrics**.</span><span class="sxs-lookup"><span data-stu-id="0f66e-127">In hello **search box**, type **Qualtrics**.</span></span>
   
   <span data-ttu-id="0f66e-128">![Programgalleriet](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="0f66e-128">![Application Gallery](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")</span></span>
7. <span data-ttu-id="0f66e-129">I resultatfönstret hello väljer **Qualtrics**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="0f66e-129">In hello results pane, select **Qualtrics**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="0f66e-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span><span class="sxs-lookup"><span data-stu-id="0f66e-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="0f66e-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0f66e-131">Configure single sign-on</span></span>

<span data-ttu-id="0f66e-132">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooQualtrics med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="0f66e-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooQualtrics with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="0f66e-133">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0f66e-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="0f66e-134">I hello klassiska Azure-portalen på hello **Qualtrics** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0f66e-134">In hello Azure classic portal, on hello **Qualtrics** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="0f66e-135">![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="0f66e-135">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="0f66e-136">På hello **hur skulle du som användare toosign på tooQualtrics** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0f66e-136">On hello **How would you like users toosign on tooQualtrics** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="0f66e-137">![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="0f66e-137">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="0f66e-138">På hello **konfigurera App-URL** i hello sidan **Qualtrics logga URL** textruta Skriv URL: en (t.ex. ”:*https://ssotest2ut1.qualtrics.com*”), och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="0f66e-138">On hello **Configure App URL** page, in hello **Qualtrics Sign On URL** textbox, type your URL (e.g.: “*https://ssotest2ut1.qualtrics.com*"), and then click **Next**.</span></span>
   
   <span data-ttu-id="0f66e-139">![Konfigurera App-URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="0f66e-139">![Configure App URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")</span></span>
4. <span data-ttu-id="0f66e-140">På hello **Konfigurera enkel inloggning på Qualtrics** klickar du på **hämtar metadata**, och sedan spara hello metadatafil på datorn.</span><span class="sxs-lookup"><span data-stu-id="0f66e-140">On hello **Configure single sign-on at Qualtrics** page, click **Download metadata**, and then save hello metadata file on your computer.</span></span>
   
   <span data-ttu-id="0f66e-141">![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="0f66e-141">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="0f66e-142">Skicka hello metadata filen toohello Qualtrics supportteamet.</span><span class="sxs-lookup"><span data-stu-id="0f66e-142">Send hello metadata file toohello Qualtrics support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="0f66e-143">hello SSO konfigurationen har toobe som utförs av hello Qualtrics supportteamet.</span><span class="sxs-lookup"><span data-stu-id="0f66e-143">hello SSO configuration has toobe performed by hello Qualtrics support team.</span></span> <span data-ttu-id="0f66e-144">Du får ett meddelande som hello konfigurationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0f66e-144">You will get a notification as soon as hello configuration has been completed.</span></span>
   > 
   > 
6. <span data-ttu-id="0f66e-145">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0f66e-145">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="0f66e-146">![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="0f66e-146">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="0f66e-147">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="0f66e-147">Configure user provisioning</span></span>

<span data-ttu-id="0f66e-148">Det finns inga uppgiften för du tooconfigure användaretablering tooQualtrics.</span><span class="sxs-lookup"><span data-stu-id="0f66e-148">There is no action item for you tooconfigure user provisioning tooQualtrics.</span></span> <span data-ttu-id="0f66e-149">När en tilldelad användare försöker toolog till Qualtrics med hello åtkomstpanelen, kontrollerar Qualtrics om hello användaren finns.</span><span class="sxs-lookup"><span data-stu-id="0f66e-149">When an assigned user tries toolog into Qualtrics using hello access panel, Qualtrics checks whether hello user exists.</span></span>  

<span data-ttu-id="0f66e-150">Om det finns inget användarkonto ännu, skapas den automatiskt av Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="0f66e-150">If there is no user account available yet, it is automatically created by Qualtrics.</span></span>

## <a name="assign-users"></a><span data-ttu-id="0f66e-151">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="0f66e-151">Assign users</span></span>
<span data-ttu-id="0f66e-152">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="0f66e-152">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="0f66e-153">**tooassign användare tooQualtrics utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0f66e-153">**tooassign users tooQualtrics, perform hello following steps:**</span></span>

1. <span data-ttu-id="0f66e-154">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0f66e-154">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="0f66e-155">På hello **Qualtrics** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="0f66e-155">On hello **Qualtrics** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="0f66e-156">![Tilldela användare](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="0f66e-156">![Assign Users](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")</span></span>
3. <span data-ttu-id="0f66e-157">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="0f66e-157">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="0f66e-158">![Ja](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="0f66e-158">![Yes](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="0f66e-159">Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0f66e-159">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="0f66e-160">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0f66e-160">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

