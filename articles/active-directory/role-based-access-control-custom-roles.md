---
title: "aaaCreate anpassade roller för Azure RBAC | Microsoft Docs"
description: "Lär dig hur toodefine anpassade roller med rollbaserad åtkomstkontroll för mer exakta identity management i din Azure-prenumeration."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/11/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 60df12632ef6c086d5feeb1809196d7c4ee5e021
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a><span data-ttu-id="086f8-103">Skapa anpassade roller för rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="086f8-103">Create custom roles for Azure Role-Based Access Control</span></span>
<span data-ttu-id="086f8-104">Skapa en anpassad roll i rollbaserad åtkomstkontroll (RBAC) om ingen av hello inbyggda roller uppfyller dina specifika behov.</span><span class="sxs-lookup"><span data-stu-id="086f8-104">Create a custom role in Azure Role-Based Access Control (RBAC) if none of hello built-in roles meet your specific access needs.</span></span> <span data-ttu-id="086f8-105">Anpassade roller kan skapas med [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure-kommandoradsgränssnittet](role-based-access-control-manage-access-azure-cli.md) (CLI) och hello [REST API](role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="086f8-105">Custom roles can be created using [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface](role-based-access-control-manage-access-azure-cli.md) (CLI), and hello [REST API](role-based-access-control-manage-access-rest.md).</span></span> <span data-ttu-id="086f8-106">Du kan tilldela anpassade roller toousers, grupper och program på prenumerationen, resursgruppen och resursen omfattningar precis som inbyggda roller.</span><span class="sxs-lookup"><span data-stu-id="086f8-106">Just like built-in roles, you can assign custom roles toousers, groups, and applications at subscription, resource group, and resource scopes.</span></span> <span data-ttu-id="086f8-107">Anpassade roller lagras i Azure AD-klient och kan delas mellan prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="086f8-107">Custom roles are stored in an Azure AD tenant and can be shared across subscriptions.</span></span>

<span data-ttu-id="086f8-108">Varje klient kan skapa upp too2000 anpassade roller.</span><span class="sxs-lookup"><span data-stu-id="086f8-108">Each tenant can create up too2000 custom roles.</span></span> 

<span data-ttu-id="086f8-109">hello visas följande exempel en anpassad roll för övervakning och starta om virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="086f8-109">hello following example shows a custom role for monitoring and restarting virtual machines:</span></span>

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a><span data-ttu-id="086f8-110">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="086f8-110">Actions</span></span>
<span data-ttu-id="086f8-111">Hej **åtgärder** -egenskapen för en anpassad roll anger hello Azure operations toowhich hello rollen ger åtkomst.</span><span class="sxs-lookup"><span data-stu-id="086f8-111">hello **Actions** property of a custom role specifies hello Azure operations toowhich hello role grants access.</span></span> <span data-ttu-id="086f8-112">Det är en samling med åtgärden strängar som identifierar skyddbara drift av Azure-resursprovidrar.</span><span class="sxs-lookup"><span data-stu-id="086f8-112">It is a collection of operation strings that identify securable operations of Azure resource providers.</span></span> <span data-ttu-id="086f8-113">Åtgärden strängar följer hello format för `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span><span class="sxs-lookup"><span data-stu-id="086f8-113">Operation strings follow hello format of `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span></span> <span data-ttu-id="086f8-114">Åtgärden strängar som innehåller jokertecken (\*) och bevilja åtkomst tooall åtgärder som matchar hello åtgärden sträng.</span><span class="sxs-lookup"><span data-stu-id="086f8-114">Operation strings that contain wildcards (\*) grant access tooall operations that match hello operation string.</span></span> <span data-ttu-id="086f8-115">Exempel:</span><span class="sxs-lookup"><span data-stu-id="086f8-115">For instance:</span></span>

* <span data-ttu-id="086f8-116">`*/read`ger åtkomst till tooread åtgärder för alla typer av resurser för alla Azure-resurs-providers.</span><span class="sxs-lookup"><span data-stu-id="086f8-116">`*/read` grants access tooread operations for all resource types of all Azure resource providers.</span></span>
* <span data-ttu-id="086f8-117">`Microsoft.Compute/*`ger åtkomst till tooall åtgärder för alla typer av resurser i hello Microsoft.Compute-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="086f8-117">`Microsoft.Compute/*` grants access tooall operations for all resource types in hello Microsoft.Compute resource provider.</span></span>
* <span data-ttu-id="086f8-118">`Microsoft.Network/*/read`ger åtkomst till tooread åtgärder för alla typer av resurser i hello Microsoft.Network resource provider för Azure.</span><span class="sxs-lookup"><span data-stu-id="086f8-118">`Microsoft.Network/*/read` grants access tooread operations for all resource types in hello Microsoft.Network resource provider of Azure.</span></span>
* <span data-ttu-id="086f8-119">`Microsoft.Compute/virtualMachines/*`ger åtkomst till tooall åtgärder för virtuella datorer och dess underordnade resurstyper.</span><span class="sxs-lookup"><span data-stu-id="086f8-119">`Microsoft.Compute/virtualMachines/*` grants access tooall operations of virtual machines and its child resource types.</span></span>
* <span data-ttu-id="086f8-120">`Microsoft.Web/sites/restart/Action`ger åtkomst till toorestart webbplatser.</span><span class="sxs-lookup"><span data-stu-id="086f8-120">`Microsoft.Web/sites/restart/Action` grants access toorestart websites.</span></span>

<span data-ttu-id="086f8-121">Använd `Get-AzureRmProviderOperation` (i PowerShell) eller `azure provider operations show` (i Azure CLI) toolist drift av Azure-resursprovidrar.</span><span class="sxs-lookup"><span data-stu-id="086f8-121">Use `Get-AzureRmProviderOperation` (in PowerShell) or `azure provider operations show` (in Azure CLI) toolist operations of Azure resource providers.</span></span> <span data-ttu-id="086f8-122">Du kan också använda dessa kommandon tooverify att en sträng för åtgärden är giltig och tooexpand jokertecken åtgärden strängar.</span><span class="sxs-lookup"><span data-stu-id="086f8-122">You may also use these commands tooverify that an operation string is valid, and tooexpand wildcard operation strings.</span></span>

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Skärmbild av PowerShell - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![<span data-ttu-id="086f8-124">Azure CLI skärmbild - azure provider operations visa ”Microsoft.Compute/virtualMachines/ \* /Action”</span><span class="sxs-lookup"><span data-stu-id="086f8-124">Azure CLI screenshot - azure provider operations show "Microsoft.Compute/virtualMachines/\*/action"</span></span> ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a><span data-ttu-id="086f8-125">NotActions</span><span class="sxs-lookup"><span data-stu-id="086f8-125">NotActions</span></span>
<span data-ttu-id="086f8-126">Använd hello **NotActions** egenskapen om hello uppsättning åtgärder som du vill tooallow definieras enklare genom att utesluta vissa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="086f8-126">Use hello **NotActions** property if hello set of operations that you wish tooallow is more easily defined by excluding restricted operations.</span></span> <span data-ttu-id="086f8-127">hello åtkomst av en anpassad roll beräknas genom att subtrahera hello **NotActions** åtgärder från hello **åtgärder** åtgärder.</span><span class="sxs-lookup"><span data-stu-id="086f8-127">hello access granted by a custom role is computed by subtracting hello **NotActions** operations from hello **Actions** operations.</span></span>

> [!NOTE]
> <span data-ttu-id="086f8-128">Om en användare har tilldelats en roll som inte omfattar en åtgärd i **NotActions**, och har tilldelats en andra roll som beviljar åtkomst toohello samma åtgärd, hello användaren är tillåten tooperform åtgärden.</span><span class="sxs-lookup"><span data-stu-id="086f8-128">If a user is assigned a role that excludes an operation in **NotActions**, and is assigned a second role that grants access toohello same operation, hello user is allowed tooperform that operation.</span></span> <span data-ttu-id="086f8-129">**NotActions** är inte en neka-regel – det är bara ett bekvämt sätt toocreate en uppsättning tillåtna åtgärder när specifika åtgärder måste toobe uteslutas.</span><span class="sxs-lookup"><span data-stu-id="086f8-129">**NotActions** is not a deny rule – it is simply a convenient way toocreate a set of allowed operations when specific operations need toobe excluded.</span></span>
>
>

## <a name="assignablescopes"></a><span data-ttu-id="086f8-130">AssignableScopes</span><span class="sxs-lookup"><span data-stu-id="086f8-130">AssignableScopes</span></span>
<span data-ttu-id="086f8-131">Hej **AssignableScopes** -egenskapen för hello anpassad roll anger hello scope (prenumerationer, resursgrupper och resurser) inom vilken hello anpassade roll som är tillgängliga för tilldelning.</span><span class="sxs-lookup"><span data-stu-id="086f8-131">hello **AssignableScopes** property of hello custom role specifies hello scopes (subscriptions, resource groups, or resources) within which hello custom role is available for assignment.</span></span> <span data-ttu-id="086f8-132">Du kan göra hello anpassad roll tillgängliga för tilldelning i hello-prenumerationer eller resursgrupper som kräver och inte oreda användarens upplevelse i hello resten av hello prenumerationer eller resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="086f8-132">You can make hello custom role available for assignment in only hello subscriptions or resource groups that require it, and not clutter user experience for hello rest of hello subscriptions or resource groups.</span></span>

<span data-ttu-id="086f8-133">Exempel på giltiga kan tilldelas omfång:</span><span class="sxs-lookup"><span data-stu-id="086f8-133">Examples of valid assignable scopes include:</span></span>

* <span data-ttu-id="086f8-134">”/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, ”/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - tillgängliggör hello roll för tilldelning i två prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="086f8-134">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, “/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - makes hello role available for assignment in two subscriptions.</span></span>
* <span data-ttu-id="086f8-135">”/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - gör hello rollen tillgängliga för tilldelning i en enda prenumeration.</span><span class="sxs-lookup"><span data-stu-id="086f8-135">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - makes hello role available for assignment in a single subscription.</span></span>
* <span data-ttu-id="086f8-136">”/ prenumerationer/c276fc76-9cd4-44c9-99a7-4fd71546436e/resursgrupper/nätverk” - gör hello roll som är tillgängliga för tilldelning endast i resursgruppen för hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="086f8-136">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network” - makes hello role available for assignment only in hello Network resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="086f8-137">Du måste använda minst en prenumeration, resursgrupp eller resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="086f8-137">You must use at least one subscription, resource group, or resource ID.</span></span>
>
>

## <a name="custom-roles-access-control"></a><span data-ttu-id="086f8-138">Anpassade roller åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="086f8-138">Custom roles access control</span></span>
<span data-ttu-id="086f8-139">Hej **AssignableScopes** hello anpassad roll också bestämmer vem som kan visa, ändra och ta bort hello roll.</span><span class="sxs-lookup"><span data-stu-id="086f8-139">hello **AssignableScopes** property of hello custom role also controls who can view, modify, and delete hello role.</span></span>

* <span data-ttu-id="086f8-140">Vem som kan skapa en anpassad roll?</span><span class="sxs-lookup"><span data-stu-id="086f8-140">Who can create a custom role?</span></span>
    <span data-ttu-id="086f8-141">Ägare (och administratörer för användare) för prenumerationer, resursgrupper och resurser kan skapa anpassade roller för användning i dessa scope.</span><span class="sxs-lookup"><span data-stu-id="086f8-141">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can create custom roles for use in those scopes.</span></span>
    <span data-ttu-id="086f8-142">hello användaren skapar hello roll måste toobe kan tooperform `Microsoft.Authorization/roleDefinition/write` åtgärden på alla hello **AssignableScopes** hello-rollen.</span><span class="sxs-lookup"><span data-stu-id="086f8-142">hello user creating hello role needs toobe able tooperform `Microsoft.Authorization/roleDefinition/write` operation on all hello **AssignableScopes** of hello role.</span></span>
* <span data-ttu-id="086f8-143">Vem som kan ändra en anpassad roll?</span><span class="sxs-lookup"><span data-stu-id="086f8-143">Who can modify a custom role?</span></span>
    <span data-ttu-id="086f8-144">Ägare (och administratörer för användare) för prenumerationer, resursgrupper och resurser kan ändra anpassade roller i dessa scope.</span><span class="sxs-lookup"><span data-stu-id="086f8-144">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can modify custom roles in those scopes.</span></span> <span data-ttu-id="086f8-145">Användare behöver toobe kan tooperform hello `Microsoft.Authorization/roleDefinition/write` åtgärden på alla hello **AssignableScopes** av en anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="086f8-145">Users need toobe able tooperform hello `Microsoft.Authorization/roleDefinition/write` operation on all hello **AssignableScopes** of a custom role.</span></span>
* <span data-ttu-id="086f8-146">Vem som kan visa anpassade roller?</span><span class="sxs-lookup"><span data-stu-id="086f8-146">Who can view custom roles?</span></span>
    <span data-ttu-id="086f8-147">Alla inbyggda roller i Azure RBAC Tillåt visning av roller som är tillgängliga för tilldelning.</span><span class="sxs-lookup"><span data-stu-id="086f8-147">All built-in roles in Azure RBAC allow viewing of roles that are available for assignment.</span></span> <span data-ttu-id="086f8-148">Användare som kan utföra hello `Microsoft.Authorization/roleDefinition/read` åtgärden på ett scope kan visa hello RBAC-roller som är tillgängliga för tilldelning i detta scope.</span><span class="sxs-lookup"><span data-stu-id="086f8-148">Users who can perform hello `Microsoft.Authorization/roleDefinition/read` operation at a scope can view hello RBAC roles that are available for assignment at that scope.</span></span>

## <a name="see-also"></a><span data-ttu-id="086f8-149">Se även</span><span class="sxs-lookup"><span data-stu-id="086f8-149">See also</span></span>
* <span data-ttu-id="086f8-150">[Rollbaserad åtkomstkontroll](role-based-access-control-configure.md): komma igång med RBAC på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="086f8-150">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in hello Azure portal.</span></span>
* <span data-ttu-id="086f8-151">Lär dig hur toomanage åt med:</span><span class="sxs-lookup"><span data-stu-id="086f8-151">Learn how toomanage access with:</span></span>
  * [<span data-ttu-id="086f8-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="086f8-152">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
  * [<span data-ttu-id="086f8-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="086f8-153">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
  * [<span data-ttu-id="086f8-154">REST-API</span><span class="sxs-lookup"><span data-stu-id="086f8-154">REST API</span></span>](role-based-access-control-manage-access-rest.md)
* <span data-ttu-id="086f8-155">[Inbyggda roller](role-based-access-built-in-roles.md): få information om hello roller som redan finns i RBAC.</span><span class="sxs-lookup"><span data-stu-id="086f8-155">[Built-in roles](role-based-access-built-in-roles.md): Get details about hello roles that come standard in RBAC.</span></span>
