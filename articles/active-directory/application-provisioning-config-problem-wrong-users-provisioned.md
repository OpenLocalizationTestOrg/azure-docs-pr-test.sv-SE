---
title: "Fel uppsättning användare tillhandahålls till ett program för Azure AD-galleriet | Microsoft Docs"
description: "Lär dig att ta reda på varför en annan uppsättning användare tillhandahålls till ett program än de som du förväntade dig"
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
ms.openlocfilehash: 85b533584c8ec15a23be32e20db7de583fced6a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="wrong-set-of-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a><span data-ttu-id="f86a6-103">Fel uppsättning användare tillhandahålls till ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="f86a6-103">Wrong set of users are being provisioned to an Azure AD Gallery application</span></span>

<span data-ttu-id="f86a6-104">Vilka användare som har etablerats i appen är främst styrs av vilka användare och grupper har **tilldelade** till programmet.</span><span class="sxs-lookup"><span data-stu-id="f86a6-104">Which users are provisioned to the app is primarily driven by which users and groups have been **assigned** to the application.</span></span>

<span data-ttu-id="f86a6-105">Använda resurserna nedan för att lära dig att kontrollera vilka användare och grupper har tilldelats ett program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f86a6-105">Use the resources below to learn how to check which users and groups have been assigned to an application within Azure Active Directory.</span></span>

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="f86a6-106">Tilldela en användare direkt som en administratör</span><span class="sxs-lookup"><span data-stu-id="f86a6-106">Assign a user directly as an administrator</span></span>

<span data-ttu-id="f86a6-107">Följ stegen nedan om du vill tilldela en eller flera användare till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="f86a6-107">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="f86a6-108">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="f86a6-108">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f86a6-109">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f86a6-109">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f86a6-110">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f86a6-110">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f86a6-111">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f86a6-111">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f86a6-112">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="f86a6-112">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="f86a6-113">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="f86a6-113">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="f86a6-114">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="f86a6-114">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="f86a6-115">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f86a6-115">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f86a6-116">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="f86a6-116">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="f86a6-117">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="f86a6-117">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="f86a6-118">Ange den **fullständigt namn** eller **e-postadress** för den användare som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="f86a6-118">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="f86a6-119">Hovra över den **användare** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="f86a6-119">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="f86a6-120">Klicka på kryssrutan bredvid användarens profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="f86a6-120">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="f86a6-121">**Valfritt:** om du vill **lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="f86a6-121">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="f86a6-122">När du har valt användare klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="f86a6-122">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="f86a6-123">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll att tilldela användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="f86a6-123">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="f86a6-124">Klicka på den **tilldela** för att tilldela program till de valda användarna.</span><span class="sxs-lookup"><span data-stu-id="f86a6-124">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="f86a6-125">Om etablering har konfigurerats och körs redan för en app kan nya användare ska etableras till ett program i cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="f86a6-125">If provisioning is configured and already running for an app, new users should be provisioned to an application in approximately 10 minutes.</span></span> <span data-ttu-id="f86a6-126">Kontrollera den **granskningsloggar** mer information.</span><span class="sxs-lookup"><span data-stu-id="f86a6-126">Check the **Audit Logs** for details.</span></span>

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a><span data-ttu-id="f86a6-127">Tilldela en grupp direkt till ett program som administratör</span><span class="sxs-lookup"><span data-stu-id="f86a6-127">Assign a group directly to an application as an administrator</span></span>

<span data-ttu-id="f86a6-128">Följ stegen nedan om du vill tilldela en eller flera grupper till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="f86a6-128">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="f86a6-129">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="f86a6-129">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f86a6-130">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f86a6-130">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f86a6-131">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f86a6-131">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f86a6-132">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f86a6-132">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f86a6-133">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="f86a6-133">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="f86a6-134">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="f86a6-134">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="f86a6-135">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="f86a6-135">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="f86a6-136">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f86a6-136">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f86a6-137">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="f86a6-137">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="f86a6-138">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="f86a6-138">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="f86a6-139">Ange den **fullständig gruppnamn** i gruppen som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="f86a6-139">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="f86a6-140">Hovra över den **grupp** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="f86a6-140">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="f86a6-141">Klickar du på kryssrutan bredvid gruppen profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="f86a6-141">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="f86a6-142">**Valfritt:** om du vill **lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till den här gruppen till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="f86a6-142">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="f86a6-143">När du har valt grupper klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="f86a6-143">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="f86a6-144">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll som ska tilldelas de grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="f86a6-144">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="f86a6-145">Klicka på den **tilldela** för att tilldela program till de valda grupperna.</span><span class="sxs-lookup"><span data-stu-id="f86a6-145">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="f86a6-146">Om etablering har konfigurerats och körs redan för en app kan nya användare som ingår i gruppen ska etableras till ett program i cirka 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="f86a6-146">If provisioning is configured and already running for an app, new users contained within the group should be provisioned to an application in approximately 10 minutes.</span></span> <span data-ttu-id="f86a6-147">Kontrollera den **granskningsloggar** mer information.</span><span class="sxs-lookup"><span data-stu-id="f86a6-147">Check the **Audit Logs** for details.</span></span>

>[!IMPORTANT]
><span data-ttu-id="f86a6-148">Etablering av gruppnamn och gruppinformation förutom medlemmar, om stöd för vissa program.</span><span class="sxs-lookup"><span data-stu-id="f86a6-148">Provisioning of the group name and group details, in addition to the members, if supported for some applications.</span></span> <span data-ttu-id="f86a6-149">Du kan aktivera eller inaktivera den här funktionen genom att aktivera eller inaktivera den **mappning** för gruppobjekt visas i den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="f86a6-149">You can enable or disable this functionality by enabling or disabling the **Mapping** for group objects shown in the **Provisioning** tab.</span></span> 
>
>

<span data-ttu-id="f86a6-150">Om etablering grupper är aktiverad bör du granska attributmappning för att säkerställa ett lämpligt fält som används för ”matchande ID”.</span><span class="sxs-lookup"><span data-stu-id="f86a6-150">If provisioning groups is enabled, be sure to review the attribute mappings to ensure an appropriate field is being used for the “matching ID”.</span></span> <span data-ttu-id="f86a6-151">Detta kan vara namn eller e-post alias, som gruppen och dess medlemmar inte etableras om matchning av egenskap är tomt eller inte fyllts i för en grupp i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f86a6-151">This can be the display name or email alias, as the group and its members not be provisioned if the matching property is empty or not populated for a group in Azure AD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f86a6-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f86a6-152">Next steps</span></span>
[<span data-ttu-id="f86a6-153">Automatisera användaren etablering och avetablering för SaaS-program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f86a6-153">Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)
