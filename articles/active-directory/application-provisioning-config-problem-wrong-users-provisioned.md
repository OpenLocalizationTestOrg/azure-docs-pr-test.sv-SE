---
title: "aaaWrong uppsättning användare som etablerade tooan Azure AD-galleriet program | Microsoft Docs"
description: "Lär dig hur toofind varför en annan uppsättning användare som är etablerad tooan program än de som du förväntade dig"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: adb90b12a53fb3160ce2b73b2559df92b283438e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="wrong-set-of-users-are-being-provisioned-tooan-azure-ad-gallery-application"></a><span data-ttu-id="12117-103">Fel uppsättning användare som etablerade tooan Azure AD-galleriet program</span><span class="sxs-lookup"><span data-stu-id="12117-103">Wrong set of users are being provisioned tooan Azure AD Gallery application</span></span>

<span data-ttu-id="12117-104">Vilka användare etablerade toohello app är främst drivs av vilka användare och grupper har **tilldelade** toohello program.</span><span class="sxs-lookup"><span data-stu-id="12117-104">Which users are provisioned toohello app is primarily driven by which users and groups have been **assigned** toohello application.</span></span>

<span data-ttu-id="12117-105">Använd hello resurserna nedan toolearn hur toocheck vilka användare och grupper som har tilldelats tooan program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="12117-105">Use hello resources below toolearn how toocheck which users and groups have been assigned tooan application within Azure Active Directory.</span></span>

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="12117-106">Tilldela en användare direkt som en administratör</span><span class="sxs-lookup"><span data-stu-id="12117-106">Assign a user directly as an administrator</span></span>

<span data-ttu-id="12117-107">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="12117-107">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="12117-108">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="12117-108">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="12117-109">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="12117-109">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="12117-110">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="12117-110">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="12117-111">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="12117-111">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="12117-112">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="12117-112">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="12117-113">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="12117-113">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="12117-114">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="12117-114">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="12117-115">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="12117-115">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="12117-116">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="12117-116">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="12117-117">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="12117-117">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="12117-118">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="12117-118">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="12117-119">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="12117-119">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="12117-120">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="12117-120">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="12117-121">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="12117-121">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="12117-122">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="12117-122">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="12117-123">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="12117-123">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="12117-124">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="12117-124">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="12117-125">Om etablering har konfigurerats och körs redan för en app kan nya användare ska vara etablerade tooan programmet i cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="12117-125">If provisioning is configured and already running for an app, new users should be provisioned tooan application in approximately 10 minutes.</span></span> <span data-ttu-id="12117-126">Kontrollera hello **granskningsloggar** mer information.</span><span class="sxs-lookup"><span data-stu-id="12117-126">Check hello **Audit Logs** for details.</span></span>

## <a name="assign-a-group-directly-tooan-application-as-an-administrator"></a><span data-ttu-id="12117-127">Tilldela en grupp direkt tooan programmet som administratör</span><span class="sxs-lookup"><span data-stu-id="12117-127">Assign a group directly tooan application as an administrator</span></span>

<span data-ttu-id="12117-128">tooassign en eller flera grupper tooan programmet direkt, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="12117-128">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="12117-129">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="12117-129">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="12117-130">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="12117-130">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="12117-131">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="12117-131">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="12117-132">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="12117-132">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="12117-133">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="12117-133">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="12117-134">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="12117-134">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="12117-135">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="12117-135">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="12117-136">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="12117-136">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="12117-137">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="12117-137">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="12117-138">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="12117-138">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="12117-139">Typen i hello **fullständig gruppnamn** av hello-grupp som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="12117-139">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="12117-140">Hovra över hello **grupp** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="12117-140">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="12117-141">Klicka på hello kryssrutan nästa toohello gruppens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="12117-141">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="12117-142">**Valfritt:** om du vill ha för**lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här gruppen toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="12117-142">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="12117-143">När du har valt grupper klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="12117-143">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="12117-144">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="12117-144">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="12117-145">Klicka på hello **tilldela** knappen tooassign hello programmet toohello valda grupper.</span><span class="sxs-lookup"><span data-stu-id="12117-145">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="12117-146">Om etablering har konfigurerats och körs redan för en app kan nya användare som ingår i hello grupp ska vara etablerade tooan programmet i cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="12117-146">If provisioning is configured and already running for an app, new users contained within hello group should be provisioned tooan application in approximately 10 minutes.</span></span> <span data-ttu-id="12117-147">Kontrollera hello **granskningsloggar** mer information.</span><span class="sxs-lookup"><span data-stu-id="12117-147">Check hello **Audit Logs** for details.</span></span>

>[!IMPORTANT]
><span data-ttu-id="12117-148">Etablering av hello gruppnamn och gruppera informationen i tillägget toohello medlemmar om stöd för vissa program.</span><span class="sxs-lookup"><span data-stu-id="12117-148">Provisioning of hello group name and group details, in addition toohello members, if supported for some applications.</span></span> <span data-ttu-id="12117-149">Du kan aktivera eller inaktivera den här funktionen genom att aktivera eller inaktivera hello **mappning** för gruppobjekt visas i hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="12117-149">You can enable or disable this functionality by enabling or disabling hello **Mapping** for group objects shown in hello **Provisioning** tab.</span></span> 
>
>

<span data-ttu-id="12117-150">Om etablering grupper är aktiverat, måste tooreview hello attributet mappningar tooensure ett lämpligt fält som används för hello ”matchande ID”.</span><span class="sxs-lookup"><span data-stu-id="12117-150">If provisioning groups is enabled, be sure tooreview hello attribute mappings tooensure an appropriate field is being used for hello “matching ID”.</span></span> <span data-ttu-id="12117-151">Detta kan vara hello namn eller e-post alias, som hello grupp och dess medlemmar inte etableras om hello matchning av egenskap är tomt eller inte fyllts i för en grupp i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="12117-151">This can be hello display name or email alias, as hello group and its members not be provisioned if hello matching property is empty or not populated for a group in Azure AD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12117-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12117-152">Next steps</span></span>
[<span data-ttu-id="12117-153">Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="12117-153">Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)
