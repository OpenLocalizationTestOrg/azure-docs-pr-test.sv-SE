---
title: "Självstudier: Azure Active Directory-integrering med SCC livscykel | Microsoft Docs"
description: "Lär dig hur toouse SCC livscykel med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: c10c313c5fc157ed70d2ccecfb930a8a765f8444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a><span data-ttu-id="593b1-103">Självstudier: Azure Active Directory-integrering med SCC livscykel</span><span class="sxs-lookup"><span data-stu-id="593b1-103">Tutorial: Azure Active Directory integration with SCC LifeCycle</span></span>
<span data-ttu-id="593b1-104">hello syftet med den här kursen är tooshow hello integreringen av Azure och SCC livscykel.</span><span class="sxs-lookup"><span data-stu-id="593b1-104">hello objective of this tutorial is tooshow hello integration of Azure and SCC LifeCycle.</span></span>  

<span data-ttu-id="593b1-105">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="593b1-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="593b1-106">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="593b1-106">A valid Azure subscription</span></span>
* <span data-ttu-id="593b1-107">En SCC livscykel enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="593b1-107">A SCC LifeCycle single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="593b1-108">Den här kursen hello Azure AD-användare som du har tilldelat tooSCC livscykel blir kan toosingle logga till hello program på din SCC livscykel företagets plats (service provider initierade inloggning) eller använda hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="593b1-108">After completing this tutorial, hello Azure AD users you have assigned tooSCC LifeCycle will be able toosingle sign into hello application at your SCC LifeCycle company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="593b1-109">hello-scenario som beskrivs i den här kursen består av följande byggblock hello:</span><span class="sxs-lookup"><span data-stu-id="593b1-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="593b1-110">Aktivera programmet hello-integrering för SCC livscykel</span><span class="sxs-lookup"><span data-stu-id="593b1-110">Enabling hello application integration for SCC LifeCycle</span></span>
2. <span data-ttu-id="593b1-111">Konfigurera enkel inloggning (SSO)</span><span class="sxs-lookup"><span data-stu-id="593b1-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="593b1-112">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="593b1-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="593b1-113">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="593b1-113">Assigning users</span></span>

<span data-ttu-id="593b1-114">![Scenariot](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="593b1-114">![Scenario](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-scc-lifecycle"></a><span data-ttu-id="593b1-115">Aktivera hello programmet integration för SCC livscykel</span><span class="sxs-lookup"><span data-stu-id="593b1-115">Enable hello application integration for SCC LifeCycle</span></span>
<span data-ttu-id="593b1-116">hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för SCC livscykel.</span><span class="sxs-lookup"><span data-stu-id="593b1-116">hello objective of this section is toooutline how tooenable hello application integration for SCC LifeCycle.</span></span>

<span data-ttu-id="593b1-117">**tooenable hello programintegrationstyp för SCC livscykel, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="593b1-117">**tooenable hello application integration for SCC LifeCycle, perform hello following steps:**</span></span>

1. <span data-ttu-id="593b1-118">I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="593b1-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="593b1-119">![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="593b1-119">![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="593b1-120">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="593b1-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="593b1-121">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="593b1-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="593b1-122">![Program](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "program")</span><span class="sxs-lookup"><span data-stu-id="593b1-122">![Applications](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="593b1-123">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="593b1-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="593b1-124">![Lägg till program](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "lägga till program")</span><span class="sxs-lookup"><span data-stu-id="593b1-124">![Add application](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="593b1-125">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="593b1-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="593b1-126">![Lägga till ett program från gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "lägga till ett program från gallerry")</span><span class="sxs-lookup"><span data-stu-id="593b1-126">![Add an application from gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="593b1-127">I hello **sökrutan**, typen **SCC livscykel**.</span><span class="sxs-lookup"><span data-stu-id="593b1-127">In hello **search box**, type **SCC LifeCycle**.</span></span>
   
    <span data-ttu-id="593b1-128">![Programgalleriet](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Programgalleriet")</span><span class="sxs-lookup"><span data-stu-id="593b1-128">![Application Gallery](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Application Gallery")</span></span>
7. <span data-ttu-id="593b1-129">I resultatfönstret hello väljer **SCC livscykel**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="593b1-129">In hello results pane, select **SCC LifeCycle**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="593b1-130">![SCC livscykel](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC livscykel")</span><span class="sxs-lookup"><span data-stu-id="593b1-130">![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="593b1-131">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="593b1-131">Configure single sign-on</span></span>

<span data-ttu-id="593b1-132">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooSCC livscykel med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="593b1-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooSCC LifeCycle with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="593b1-133">**tooconfigure enkel inloggning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="593b1-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="593b1-134">I hello klassiska Azure-portalen på hello **SCC livscykel** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello ** Konfigurera enkel inloggning ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="593b1-134">In hello Azure classic portal, on hello **SCC LifeCycle** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="593b1-135">![Konfigurera enkel inloggning](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="593b1-135">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="593b1-136">På hello **hur skulle du som användare toosign på tooSCC livscykel** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="593b1-136">On hello **How would you like users toosign on tooSCC LifeCycle** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="593b1-137">![Konfigurera enkel inloggning](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="593b1-137">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="593b1-138">På hello **konfigurera App-URL** i hello sidan **logga URL** textruta typen hello URL används av dina användare toosign på tooyour SCC livscykel program med hjälp av hello följa mönstret ”*https:// bs1.SCC.com/lc7/Welcome/Customer/PICTtest.aspx*”, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="593b1-138">On hello **Configure App URL** page, in hello **Sign On URL** textbox, type hello URL used by your users toosign on tooyour SCC LifeCycle application using hello following pattern "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", and then click **Next**.</span></span>
   
    <span data-ttu-id="593b1-139">![Konfigurera App-URL](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "konfigurera App-URL")</span><span class="sxs-lookup"><span data-stu-id="593b1-139">![Configure App URL](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Configure App URL")</span></span>
4. <span data-ttu-id="593b1-140">På hello **Konfigurera enkel inloggning på SCC livscykel** klickar du på **hämtar metadata**, och sedan spara metadatafilen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="593b1-140">On hello **Configure single sign-on at SCC LifeCycle** page, click **Download metadata**, and then save metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="593b1-141">![Konfigurera enkel inloggning](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="593b1-141">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="593b1-142">Vidarebefordra som Metadata filen tooSCC livscykel supportteamet.</span><span class="sxs-lookup"><span data-stu-id="593b1-142">Forward that Metadata file tooSCC LifeCycle Support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="593b1-143">Enkel inloggning har toobe aktiveras med hello SCC livscykel supportteamet.</span><span class="sxs-lookup"><span data-stu-id="593b1-143">Single sign-on has toobe enabled by hello SCC LifeCycle support team.</span></span>
   > 
   > 

6. <span data-ttu-id="593b1-144">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="593b1-144">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="593b1-145">![Konfigurera enkel inloggning](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="593b1-145">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="593b1-146">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="593b1-146">Configure user provisioning</span></span>

<span data-ttu-id="593b1-147">I ordning tooenable Azure AD-användare toolog i SCC livscykel, måste de etableras i SCC livscykel.</span><span class="sxs-lookup"><span data-stu-id="593b1-147">In order tooenable Azure AD users toolog into SCC LifeCycle, they must be provisioned into SCC LifeCycle.</span></span> <span data-ttu-id="593b1-148">Det finns inga uppgiften för du tooconfigure användaretablering tooSCC livscykel.</span><span class="sxs-lookup"><span data-stu-id="593b1-148">There is no action item for you tooconfigure user provisioning tooSCC LifeCycle.</span></span>

<span data-ttu-id="593b1-149">När en tilldelad användare försöker toolog i SCC livscykel, skapas automatiskt ett SCC livscykel-konto om det behövs.</span><span class="sxs-lookup"><span data-stu-id="593b1-149">When an assigned user tries toolog into SCC LifeCycle, an SCC LifeCycle account is automatically created if necessary.</span></span>

>[!NOTE]
><span data-ttu-id="593b1-150">Du kan använda något annat SCC livscykel användarens konto skapas verktyg eller API: er som tillhandahålls av SCC livscykel tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="593b1-150">You can use any other SCC LifeCycle user account creation tools or APIs provided by SCC LifeCycle tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="593b1-151">Tilldela användare</span><span class="sxs-lookup"><span data-stu-id="593b1-151">Assign users</span></span>
<span data-ttu-id="593b1-152">tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.</span><span class="sxs-lookup"><span data-stu-id="593b1-152">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="593b1-153">**tooassign användare tooSCC livscykel, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="593b1-153">**tooassign users tooSCC LifeCycle, perform hello following steps:**</span></span>

1. <span data-ttu-id="593b1-154">Skapa ett testkonto i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="593b1-154">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="593b1-155">På hello ** SCC livscykel ** integreringssidan för programmet, klickar du på **tilldela användare**.</span><span class="sxs-lookup"><span data-stu-id="593b1-155">On hello **SCC LifeCycle **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="593b1-156">![Tilldela användare](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="593b1-156">![Assign Users](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Assign Users")</span></span>
3. <span data-ttu-id="593b1-157">Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="593b1-157">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="593b1-158">![Ja](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ja")</span><span class="sxs-lookup"><span data-stu-id="593b1-158">![Yes](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="593b1-159">Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="593b1-159">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="593b1-160">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="593b1-160">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

