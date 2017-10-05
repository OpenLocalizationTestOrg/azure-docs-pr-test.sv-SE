---
title: "Rollbaserad åtkomstkontroll i Azure Portal | Microsoft Docs"
description: "Kom igång med åtkomsthantering med rollbaserad åtkomstkontroll på Azure Portal. Använd rolltilldelningar för att tilldela behörigheter i dina resurser."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 9df7f7851ef1fc6b4ed03b981aa5062d6b0913ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-role-based-access-control-to-manage-access-to-your-azure-subscription-resources"></a><span data-ttu-id="4b4cd-104">Använda rollbaserad åtkomstkontroll för att hantera åtkomsten till dina Azure-prenumerationsresurser</span><span class="sxs-lookup"><span data-stu-id="4b4cd-104">Use Role-Based Access Control to manage access to your Azure subscription resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4b4cd-105">Hantera åtkomst enligt användare eller grupp</span><span class="sxs-lookup"><span data-stu-id="4b4cd-105">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="4b4cd-106">Hantera åtkomst enligt resurs</span><span class="sxs-lookup"><span data-stu-id="4b4cd-106">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="4b4cd-107">Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-107">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure.</span></span> <span data-ttu-id="4b4cd-108">Med RBAC kan du bevilja exakt den åtkomstnivå som användarna behöver för att kunna utföra sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-108">Using RBAC, you can grant only the amount of access that users need to perform their jobs.</span></span> <span data-ttu-id="4b4cd-109">Den här artikeln hjälper dig att komma igång med RBAC på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-109">This article helps you get up and running with RBAC in the Azure portal.</span></span> <span data-ttu-id="4b4cd-110">Mer information om hur RBAC kan hjälpa dig att hantera åtkomsten finns i [Vad är rollbaserad åtkomstkontroll?](role-based-access-control-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="4b4cd-110">If you want more details about how RBAC helps you manage access, see [What is Role-Based Access Control](role-based-access-control-what-is.md).</span></span>

<span data-ttu-id="4b4cd-111">Du kan bevilja upp till 2 000 rolltilldelningar inom varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-111">Within each subscription, you can grant up to 2000 role assignments.</span></span> 

## <a name="view-access"></a><span data-ttu-id="4b4cd-112">Visa åtkomst</span><span class="sxs-lookup"><span data-stu-id="4b4cd-112">View access</span></span>
<span data-ttu-id="4b4cd-113">Du kan se vem som har åtkomst till en resurs, resursgrupp eller prenumeration från dess huvudblad på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4b4cd-113">You can see who has access to a resource, resource group, or subscription from its main blade in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4b4cd-114">Vi vill till exempel se vem som har åtkomst till en av våra resursgrupper:</span><span class="sxs-lookup"><span data-stu-id="4b4cd-114">For example, we want to see who has access to one of our resource groups:</span></span>

1. <span data-ttu-id="4b4cd-115">Välj **Resursgrupper** i navigeringsfältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-115">Select **Resource groups** in the navigation bar on the left.</span></span>  
    <span data-ttu-id="4b4cd-116">![Resursgrupper - ikon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span><span class="sxs-lookup"><span data-stu-id="4b4cd-116">![Resource groups - icon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span></span>
2. <span data-ttu-id="4b4cd-117">Välj namnet på resursgruppen från bladet **Resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-117">Select the name of the resource group from the **Resource groups** blade.</span></span>
3. <span data-ttu-id="4b4cd-118">Välj **Åtkomstkontroll (IAM)** från den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-118">Select **Access control (IAM)** from the left menu.</span></span>  
4. <span data-ttu-id="4b4cd-119">Åtkomstkontroll-bladet listar alla användare, grupper och program som har getts åtkomst till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-119">The Access control blade lists all users, groups, and applications that have been granted access to the resource group.</span></span>  
   
    ![Skärmbild av bladet Användare – ärvd eller tilldelad åtkomst](./media/role-based-access-control-configure/view-access.png)

<span data-ttu-id="4b4cd-121">Observera att vissa roller är begränsade till **resursen** medan andra **ärver** den från ett annat omfång.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-121">Notice that some roles are scoped to **This resource** while others are **Inherited** it from another scope.</span></span> <span data-ttu-id="4b4cd-122">Åtkomst tilldelas antingen specifikt till resursgruppen eller ärvs från en tilldelning till den överordnade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-122">Access is either assigned specifically to the resource group or inherited from an assignment to the parent subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="4b4cd-123">Administratörer och medadministratörer för klassiska prenumerationer betraktas som ägare till prenumerationen i den nya RBAC-modellen.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-123">Classic subscription admins and co-admins are considered owners of the subscription in the new RBAC model.</span></span>

## <a name="add-access"></a><span data-ttu-id="4b4cd-124">Lägga till åtkomst</span><span class="sxs-lookup"><span data-stu-id="4b4cd-124">Add Access</span></span>
<span data-ttu-id="4b4cd-125">Du beviljar åtkomst inifrån resursen, resursgruppen eller prenumerationen som rolltilldelningen omfattar.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-125">You grant access from within the resource, resource group, or subscription that is the scope of the role assignment.</span></span>

1. <span data-ttu-id="4b4cd-126">Välj **Lägg till** på åtkomstkontroll-bladet.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-126">Select **Add** on the Access control blade.</span></span>  
2. <span data-ttu-id="4b4cd-127">Välj den roll som du vill tilldela från bladet **Välj en roll**.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-127">Select the role that you wish to assign from the **Select a role** blade.</span></span>
3. <span data-ttu-id="4b4cd-128">Välj den användare, den grupp eller det program i katalogen som du vill bevilja åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-128">Select the user, group, or application in your directory that you wish to grant access to.</span></span> <span data-ttu-id="4b4cd-129">Du kan söka i katalogen med visningsnamn, e-postadresser och objektidentifierare.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-129">You can search the directory with display names, email addresses, and object identifiers.</span></span>  
   
    ![Skärmbild av bladet Lägg till användare – sök](./media/role-based-access-control-configure/grant-access2.png)
4. <span data-ttu-id="4b4cd-131">Skapa tilldelningen genom att välja **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-131">Select **OK** to create the assignment.</span></span> <span data-ttu-id="4b4cd-132">Du kan följa förloppet på popup-menyn **Lägger till användare**.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-132">The **Adding user** popup tracks the progress.</span></span>  
    <span data-ttu-id="4b4cd-133">![Lägga till användarförloppsindikatorn - skärmbild](./media/role-based-access-control-configure/addinguser_popup.png)</span><span class="sxs-lookup"><span data-stu-id="4b4cd-133">![Adding user progress bar - screenshot](./media/role-based-access-control-configure/addinguser_popup.png)</span></span>

<span data-ttu-id="4b4cd-134">När du har lagt till en rolltilldelning visas den på bladet **Användare**.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-134">After successfully adding a role assignment, it will appear on the **Users** blade.</span></span>

## <a name="remove-access"></a><span data-ttu-id="4b4cd-135">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="4b4cd-135">Remove Access</span></span>
1. <span data-ttu-id="4b4cd-136">Håll markören över namnet på tilldelningen som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-136">Hover your cursor over the name of the assignment that you want to remove.</span></span> <span data-ttu-id="4b4cd-137">En kryssruta visas bredvid namnet.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-137">A check box appears next to the name.</span></span>
2. <span data-ttu-id="4b4cd-138">Använd kryssrutorna på för att välja en eller flera rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-138">Use the check boxes to select one or more role assignments.</span></span>
2. <span data-ttu-id="4b4cd-139">Välj **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-139">Select **Remove**.</span></span>  
3. <span data-ttu-id="4b4cd-140">Bekräfta borttagningen genom att välja **Ja**.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-140">Select **Yes** to confirm the removal.</span></span>

<span data-ttu-id="4b4cd-141">Ärvda tilldelningar kan inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-141">Inherited assignments cannot be removed.</span></span> <span data-ttu-id="4b4cd-142">Om du behöver ta bort en ärvd tilldelning så måste du göra det i samma omfång som där rolltilldelningen skapades.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-142">If you need to remove an inherited assignment, you need to do it at the scope where the role assignment was created.</span></span> <span data-ttu-id="4b4cd-143">I kolumnen **Omfång** bredvid **Ärvd** finns en länk som leder till resurserna där den här rollen har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-143">In the **Scope** column, next to **Inherited** there is a link that takes you to the resources where this role was assigned.</span></span> <span data-ttu-id="4b4cd-144">Gå till resursen som visas där om du vill ta bort rolltilldelningen.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-144">Go to the resource listed there to remove the role assignment.</span></span>

![Skärmbild av bladet Användare – ärvd åtkomst inaktiverar knappen Ta bort](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a><span data-ttu-id="4b4cd-146">Andra verktyg för att hantera åtkomst</span><span class="sxs-lookup"><span data-stu-id="4b4cd-146">Other tools to manage access</span></span>
<span data-ttu-id="4b4cd-147">Du kan tilldela roller och hantera åtkomst med Azure RBAC-kommandon i andra verktyg än Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-147">You can assign roles and manage access with Azure RBAC commands in tools other than the Azure portal.</span></span>  <span data-ttu-id="4b4cd-148">Följ länkarna om du vill lära dig mer om kraven och komma igång med Azure RBAC-kommandona.</span><span class="sxs-lookup"><span data-stu-id="4b4cd-148">Follow the links to learn more about the prerequisites and get started with the Azure RBAC commands.</span></span>

* [<span data-ttu-id="4b4cd-149">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b4cd-149">Azure PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
* [<span data-ttu-id="4b4cd-150">Azure kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="4b4cd-150">Azure Command-Line Interface</span></span>](role-based-access-control-manage-access-azure-cli.md)
* [<span data-ttu-id="4b4cd-151">REST-API</span><span class="sxs-lookup"><span data-stu-id="4b4cd-151">REST API</span></span>](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a><span data-ttu-id="4b4cd-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b4cd-152">Next Steps</span></span>
* [<span data-ttu-id="4b4cd-153">Skapa en rapport över åtkomständringshistorik</span><span class="sxs-lookup"><span data-stu-id="4b4cd-153">Create an access change history report</span></span>](role-based-access-control-access-change-history-report.md)
* <span data-ttu-id="4b4cd-154">Gå till [Inbyggda RBAC-roller](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="4b4cd-154">See the [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>
* <span data-ttu-id="4b4cd-155">Definiera egna [anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md)</span><span class="sxs-lookup"><span data-stu-id="4b4cd-155">Define your own [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>

