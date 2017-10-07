---
title: "aaaRole-baserad åtkomstkontroll i hello Azure-portalen | Microsoft Docs"
description: "Kom igång med åtkomsthantering med rollbaserad åtkomstkontroll i hello Azure-portalen. Använda rollen tilldelningar tooassign behörigheter tooyour resurser."
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
ms.openlocfilehash: b87e00089b0fc93fb212b318330a6f22bfbf59e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-access-tooyour-azure-subscription-resources"></a><span data-ttu-id="c6fe7-104">Använda rollbaserad åtkomstkontroll toomanage åtkomst tooyour Azure-prenumerationsresurser</span><span class="sxs-lookup"><span data-stu-id="c6fe7-104">Use Role-Based Access Control toomanage access tooyour Azure subscription resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c6fe7-105">Hantera åtkomst enligt användare eller grupp</span><span class="sxs-lookup"><span data-stu-id="c6fe7-105">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="c6fe7-106">Hantera åtkomst enligt resurs</span><span class="sxs-lookup"><span data-stu-id="c6fe7-106">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="c6fe7-107">Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-107">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure.</span></span> <span data-ttu-id="c6fe7-108">Med RBAC kan bevilja du endast hello mängd åtkomst att användarna måste tooperform sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-108">Using RBAC, you can grant only hello amount of access that users need tooperform their jobs.</span></span> <span data-ttu-id="c6fe7-109">Den här artikeln hjälper dig att komma igång med RBAC i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-109">This article helps you get up and running with RBAC in hello Azure portal.</span></span> <span data-ttu-id="c6fe7-110">Mer information om hur RBAC kan hjälpa dig att hantera åtkomsten finns i [Vad är rollbaserad åtkomstkontroll?](role-based-access-control-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="c6fe7-110">If you want more details about how RBAC helps you manage access, see [What is Role-Based Access Control](role-based-access-control-what-is.md).</span></span>

<span data-ttu-id="c6fe7-111">Inom varje prenumeration kan du bevilja too2000 rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-111">Within each subscription, you can grant up too2000 role assignments.</span></span> 

## <a name="view-access"></a><span data-ttu-id="c6fe7-112">Visa åtkomst</span><span class="sxs-lookup"><span data-stu-id="c6fe7-112">View access</span></span>
<span data-ttu-id="c6fe7-113">Du kan se vem som har åtkomst tooa resurs, resursgrupp eller prenumeration från dess huvudblad i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c6fe7-113">You can see who has access tooa resource, resource group, or subscription from its main blade in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c6fe7-114">Vi vill till exempel toosee som har åtkomst till tooone av våra resursgrupper:</span><span class="sxs-lookup"><span data-stu-id="c6fe7-114">For example, we want toosee who has access tooone of our resource groups:</span></span>

1. <span data-ttu-id="c6fe7-115">Välj **resursgrupper** i hello hello vänstra navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-115">Select **Resource groups** in hello navigation bar on hello left.</span></span>  
    <span data-ttu-id="c6fe7-116">![Resursgrupper - ikon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span><span class="sxs-lookup"><span data-stu-id="c6fe7-116">![Resource groups - icon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span></span>
2. <span data-ttu-id="c6fe7-117">Välj hello namnet på hello resursgruppen från hello **resursgrupper** bladet.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-117">Select hello name of hello resource group from hello **Resource groups** blade.</span></span>
3. <span data-ttu-id="c6fe7-118">Välj **åtkomstkontroll (IAM)** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-118">Select **Access control (IAM)** from hello left menu.</span></span>  
4. <span data-ttu-id="c6fe7-119">hello Access control bladet visar en lista över alla användare, grupper och program som har beviljats åtkomst toohello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-119">hello Access control blade lists all users, groups, and applications that have been granted access toohello resource group.</span></span>  
   
    ![Skärmbild av bladet Användare – ärvd eller tilldelad åtkomst](./media/role-based-access-control-configure/view-access.png)

<span data-ttu-id="c6fe7-121">Observera att vissa roller är begränsade för**resursen** medan andra är **ärvda** den från ett annat omfång.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-121">Notice that some roles are scoped too**This resource** while others are **Inherited** it from another scope.</span></span> <span data-ttu-id="c6fe7-122">Åtkomst tilldelas specifikt toohello resursgruppen eller ärvs från en tilldelning toohello överordnade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-122">Access is either assigned specifically toohello resource group or inherited from an assignment toohello parent subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="c6fe7-123">Klassiska prenumerationsadministratörer och medadministratörer betraktas som ägare av hello-prenumeration i hello nya RBAC-modellen.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-123">Classic subscription admins and co-admins are considered owners of hello subscription in hello new RBAC model.</span></span>

## <a name="add-access"></a><span data-ttu-id="c6fe7-124">Lägga till åtkomst</span><span class="sxs-lookup"><span data-stu-id="c6fe7-124">Add Access</span></span>
<span data-ttu-id="c6fe7-125">Du beviljar åtkomst inifrån hello resursen, resursgruppen eller prenumerationen som är hello omfattning hello rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-125">You grant access from within hello resource, resource group, or subscription that is hello scope of hello role assignment.</span></span>

1. <span data-ttu-id="c6fe7-126">Välj **Lägg till** på hello Access control-bladet.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-126">Select **Add** on hello Access control blade.</span></span>  
2. <span data-ttu-id="c6fe7-127">Välj hello rollen tooassign från hello gärna **Välj en roll** bladet.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-127">Select hello role that you wish tooassign from hello **Select a role** blade.</span></span>
3. <span data-ttu-id="c6fe7-128">Välj hello användare, grupp eller program i katalogen som du vill toogrant åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-128">Select hello user, group, or application in your directory that you wish toogrant access to.</span></span> <span data-ttu-id="c6fe7-129">Du kan söka hello katalogen med visningsnamn, e-postadresser och objektidentifierare.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-129">You can search hello directory with display names, email addresses, and object identifiers.</span></span>  
   
    ![Skärmbild av bladet Lägg till användare – sök](./media/role-based-access-control-configure/grant-access2.png)
4. <span data-ttu-id="c6fe7-131">Välj **OK** toocreate hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-131">Select **OK** toocreate hello assignment.</span></span> <span data-ttu-id="c6fe7-132">Hej **lägger till användare** popup spårar hello pågår.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-132">hello **Adding user** popup tracks hello progress.</span></span>  
    <span data-ttu-id="c6fe7-133">![Lägga till användarförloppsindikatorn - skärmbild](./media/role-based-access-control-configure/addinguser_popup.png)</span><span class="sxs-lookup"><span data-stu-id="c6fe7-133">![Adding user progress bar - screenshot](./media/role-based-access-control-configure/addinguser_popup.png)</span></span>

<span data-ttu-id="c6fe7-134">När du har lagt till en rolltilldelning, visas den på hello **användare** bladet.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-134">After successfully adding a role assignment, it will appear on hello **Users** blade.</span></span>

## <a name="remove-access"></a><span data-ttu-id="c6fe7-135">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="c6fe7-135">Remove Access</span></span>
1. <span data-ttu-id="c6fe7-136">Håll markören över hello namnet på hello tilldelning som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-136">Hover your cursor over hello name of hello assignment that you want tooremove.</span></span> <span data-ttu-id="c6fe7-137">En kryssruta visas nästa toohello namn.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-137">A check box appears next toohello name.</span></span>
2. <span data-ttu-id="c6fe7-138">Använd hello kryssrutorna tooselect en eller flera rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-138">Use hello check boxes tooselect one or more role assignments.</span></span>
2. <span data-ttu-id="c6fe7-139">Välj **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-139">Select **Remove**.</span></span>  
3. <span data-ttu-id="c6fe7-140">Välj **Ja** tooconfirm hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-140">Select **Yes** tooconfirm hello removal.</span></span>

<span data-ttu-id="c6fe7-141">Ärvda tilldelningar kan inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-141">Inherited assignments cannot be removed.</span></span> <span data-ttu-id="c6fe7-142">Om du behöver tooremove en ärvd tilldelning måste toodo den på hello omfång där hello rolltilldelning skapades.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-142">If you need tooremove an inherited assignment, you need toodo it at hello scope where hello role assignment was created.</span></span> <span data-ttu-id="c6fe7-143">I hello **omfång** kolumn, nästa för**ärvda** finns en länk som tar dig toohello resurser där den här rollen har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-143">In hello **Scope** column, next too**Inherited** there is a link that takes you toohello resources where this role was assigned.</span></span> <span data-ttu-id="c6fe7-144">Gå toohello resurs i listan tooremove hello rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-144">Go toohello resource listed there tooremove hello role assignment.</span></span>

![Skärmbild av bladet Användare – ärvd åtkomst inaktiverar knappen Ta bort](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-toomanage-access"></a><span data-ttu-id="c6fe7-146">Andra verktyg toomanage åtkomst</span><span class="sxs-lookup"><span data-stu-id="c6fe7-146">Other tools toomanage access</span></span>
<span data-ttu-id="c6fe7-147">Du kan tilldela roller och hantera åtkomst med Azure RBAC-kommandon i andra verktyg än hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-147">You can assign roles and manage access with Azure RBAC commands in tools other than hello Azure portal.</span></span>  <span data-ttu-id="c6fe7-148">Följ hello länkar toolearn mer om hello förutsättningar och kom igång med hello Azure RBAC-kommandon.</span><span class="sxs-lookup"><span data-stu-id="c6fe7-148">Follow hello links toolearn more about hello prerequisites and get started with hello Azure RBAC commands.</span></span>

* [<span data-ttu-id="c6fe7-149">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c6fe7-149">Azure PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
* [<span data-ttu-id="c6fe7-150">Azure kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="c6fe7-150">Azure Command-Line Interface</span></span>](role-based-access-control-manage-access-azure-cli.md)
* [<span data-ttu-id="c6fe7-151">REST-API</span><span class="sxs-lookup"><span data-stu-id="c6fe7-151">REST API</span></span>](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a><span data-ttu-id="c6fe7-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6fe7-152">Next Steps</span></span>
* [<span data-ttu-id="c6fe7-153">Skapa en rapport över åtkomständringshistorik</span><span class="sxs-lookup"><span data-stu-id="c6fe7-153">Create an access change history report</span></span>](role-based-access-control-access-change-history-report.md)
* <span data-ttu-id="c6fe7-154">Se hello [inbyggda RBAC-roller](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="c6fe7-154">See hello [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>
* <span data-ttu-id="c6fe7-155">Definiera egna [anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md)</span><span class="sxs-lookup"><span data-stu-id="c6fe7-155">Define your own [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>

