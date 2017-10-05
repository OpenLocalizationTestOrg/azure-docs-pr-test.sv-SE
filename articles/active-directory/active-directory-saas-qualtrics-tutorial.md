---
title: "Självstudier: Azure Active Directory-integrering med Qualtrics | Microsoft Docs"
description: "Lär dig hur du använder Qualtrics med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: 2fcde595a40dafda7549f5bccb582b57585b314e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a><span data-ttu-id="2f683-103">Självstudier: Azure Active Directory-integrering med Qualtrics</span><span class="sxs-lookup"><span data-stu-id="2f683-103">Tutorial: Azure Active Directory integration with Qualtrics</span></span>
<span data-ttu-id="2f683-104">Syftet med den här kursen är att visa integreringen av Azure och Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="2f683-104">The objective of this tutorial is to show the integration of Azure and Qualtrics.</span></span>  

<span data-ttu-id="2f683-105">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2f683-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="2f683-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2f683-106">A valid Azure subscription</span></span>
* <span data-ttu-id="2f683-107">En Qualtrics enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="2f683-107">A Qualtrics single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="2f683-108">Den här kursen Azure AD-användare som du har tilldelat Qualtrics kommer att kunna enkel inloggning till programmet på din Qualtrics företagets plats (service provider initierade inloggning) eller med hjälp av den [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2f683-108">After completing this tutorial, the Azure AD users you have assigned to Qualtrics will be able to single sign into the application at your Qualtrics company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="2f683-109">Det scenario som beskrivs i den här kursen består av följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2f683-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="2f683-110">Aktivera programintegrationstyp för Qualtrics</span><span class="sxs-lookup"><span data-stu-id="2f683-110">Enabling the application integration for Qualtrics</span></span>
2. <span data-ttu-id="2f683-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="2f683-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="2f683-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="2f683-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="2f683-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="2f683-113">Assigning users</span></span>

<span data-ttu-id="2f683-114">![Scenariot](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="2f683-114">![Scenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-qualtrics"></a><span data-ttu-id="2f683-115">Aktivera programintegrationstyp för Qualtrics</span><span class="sxs-lookup"><span data-stu-id="2f683-115">Enabling the application integration for Qualtrics</span></span>
<span data-ttu-id="2f683-116">Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="2f683-116">The objective of this section is to outline how to enable the application integration for Qualtrics.</span></span>

<span data-ttu-id="2f683-117">**Utför följande steg om du vill aktivera programmet-integrering för Qualtrics:**</span><span class="sxs-lookup"><span data-stu-id="2f683-117">**To enable the application integration for Qualtrics, perform the following steps:**</span></span>

1. <span data-ttu-id="2f683-118">I den klassiska Azure-portalen i det vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2f683-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="2f683-119">![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="2f683-119">![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="2f683-120">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="2f683-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="2f683-121">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="2f683-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="2f683-122">![Program](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="2f683-122">![Applications](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="2f683-123">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="2f683-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="2f683-124">![Lägg till program](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="2f683-124">![Add application](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="2f683-125">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="2f683-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="2f683-126">![Lägga till ett program från gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="2f683-126">![Add an application from gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="2f683-127">I den **sökrutan**, typen **Qualtrics**.</span><span class="sxs-lookup"><span data-stu-id="2f683-127">In the **search box**, type **Qualtrics**.</span></span>
   
   <span data-ttu-id="2f683-128">![Programgalleriet](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="2f683-128">![Application Gallery](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")</span></span>
7. <span data-ttu-id="2f683-129">I resultatfönstret, Välj **Qualtrics**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="2f683-129">In the results pane, select **Qualtrics**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="2f683-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span><span class="sxs-lookup"><span data-stu-id="2f683-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="2f683-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2f683-131">Configure single sign-on</span></span>

<span data-ttu-id="2f683-132">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till Qualtrics med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="2f683-132">The objective of this section is to outline how to enable users to authenticate to Qualtrics with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="2f683-133">**Utför följande steg för att konfigurera enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="2f683-133">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="2f683-134">I den klassiska Azure-portalen på den **Qualtrics** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2f683-134">In the Azure classic portal, on the **Qualtrics** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="2f683-135">![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2f683-135">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="2f683-136">På den **hur vill du att användarna kan logga in på Qualtrics** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f683-136">On the **How would you like users to sign on to Qualtrics** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="2f683-137">![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2f683-137">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="2f683-138">På den **konfigurera App-URL** sidan den **Qualtrics logga URL** textruta Skriv URL: en (t.ex. ”:*https://ssotest2ut1.qualtrics.com*”), och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f683-138">On the **Configure App URL** page, in the **Qualtrics Sign On URL** textbox, type your URL (e.g.: “*https://ssotest2ut1.qualtrics.com*"), and then click **Next**.</span></span>
   
   <span data-ttu-id="2f683-139">![Konfigurera App-URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="2f683-139">![Configure App URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")</span></span>
4. <span data-ttu-id="2f683-140">På den **Konfigurera enkel inloggning på Qualtrics** klickar du på **hämtar metadata**, och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2f683-140">On the **Configure single sign-on at Qualtrics** page, click **Download metadata**, and then save the metadata file on your computer.</span></span>
   
   <span data-ttu-id="2f683-141">![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2f683-141">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="2f683-142">Skicka metadatafilen till Qualtrics supportteam.</span><span class="sxs-lookup"><span data-stu-id="2f683-142">Send the metadata file to the Qualtrics support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="2f683-143">SSO-konfigurationen har kan utföras av supportteamet Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="2f683-143">The SSO configuration has to be performed by the Qualtrics support team.</span></span> <span data-ttu-id="2f683-144">Du får ett meddelande så snart som konfigurationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2f683-144">You will get a notification as soon as the configuration has been completed.</span></span>
   > 
   > 
6. <span data-ttu-id="2f683-145">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2f683-145">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="2f683-146">![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2f683-146">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="2f683-147">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="2f683-147">Configure user provisioning</span></span>

<span data-ttu-id="2f683-148">Det finns inga uppgiften som du kan konfigurera användaretablering till Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="2f683-148">There is no action item for you to configure user provisioning to Qualtrics.</span></span> <span data-ttu-id="2f683-149">När en tilldelad användare försöker logga in med hjälp av åtkomstpanelen Qualtrics kontrollerar Qualtrics om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="2f683-149">When an assigned user tries to log into Qualtrics using the access panel, Qualtrics checks whether the user exists.</span></span>  

<span data-ttu-id="2f683-150">Om det finns inget användarkonto ännu, skapas den automatiskt av Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="2f683-150">If there is no user account available yet, it is automatically created by Qualtrics.</span></span>

## <a name="assign-users"></a><span data-ttu-id="2f683-151">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="2f683-151">Assign users</span></span>
<span data-ttu-id="2f683-152">Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.</span><span class="sxs-lookup"><span data-stu-id="2f683-152">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="2f683-153">**Utför följande steg om du vill tilldela Qualtrics användare:**</span><span class="sxs-lookup"><span data-stu-id="2f683-153">**To assign users to Qualtrics, perform the following steps:**</span></span>

1. <span data-ttu-id="2f683-154">Skapa ett testkonto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2f683-154">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="2f683-155">På den **Qualtrics** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="2f683-155">On the **Qualtrics** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="2f683-156">![Tilldela användare](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="2f683-156">![Assign Users](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")</span></span>
3. <span data-ttu-id="2f683-157">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.</span><span class="sxs-lookup"><span data-stu-id="2f683-157">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="2f683-158">![Ja](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="2f683-158">![Yes](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="2f683-159">Om du vill testa inställningarna för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f683-159">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="2f683-160">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2f683-160">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

