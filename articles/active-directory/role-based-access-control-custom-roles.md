---
title: "Skapa anpassade roller för Azure RBAC | Microsoft Docs"
description: "Lär dig hur du definierar anpassade roller med rollbaserad åtkomstkontroll för mer exakta Identitetshantering i din Azure-prenumeration."
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
ms.openlocfilehash: 8e72f2c8095d13c4b6df3c6576bd58806a3c0f2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a><span data-ttu-id="a4bc1-103">Skapa anpassade roller för rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="a4bc1-103">Create custom roles for Azure Role-Based Access Control</span></span>
<span data-ttu-id="a4bc1-104">Skapa en anpassad roll i rollbaserad åtkomstkontroll (RBAC) om ingen av de inbyggda rollerna uppfyller dina specifika behov.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-104">Create a custom role in Azure Role-Based Access Control (RBAC) if none of the built-in roles meet your specific access needs.</span></span> <span data-ttu-id="a4bc1-105">Anpassade roller kan skapas med [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure-kommandoradsgränssnittet](role-based-access-control-manage-access-azure-cli.md) (CLI) och [REST API](role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="a4bc1-105">Custom roles can be created using [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface](role-based-access-control-manage-access-azure-cli.md) (CLI), and the [REST API](role-based-access-control-manage-access-rest.md).</span></span> <span data-ttu-id="a4bc1-106">Du kan tilldela anpassade roller till användare, grupper och program på prenumerationen, resursgruppen och resursen omfattningar precis som inbyggda roller.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-106">Just like built-in roles, you can assign custom roles to users, groups, and applications at subscription, resource group, and resource scopes.</span></span> <span data-ttu-id="a4bc1-107">Anpassade roller lagras i Azure AD-klient och kan delas mellan prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-107">Custom roles are stored in an Azure AD tenant and can be shared across subscriptions.</span></span>

<span data-ttu-id="a4bc1-108">Varje klient kan skapa upp till 2000 anpassade roller.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-108">Each tenant can create up to 2000 custom roles.</span></span> 

<span data-ttu-id="a4bc1-109">I följande exempel visas en anpassad roll för övervakning och starta om virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="a4bc1-109">The following example shows a custom role for monitoring and restarting virtual machines:</span></span>

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
## <a name="actions"></a><span data-ttu-id="a4bc1-110">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="a4bc1-110">Actions</span></span>
<span data-ttu-id="a4bc1-111">Den **åtgärder** -egenskapen för en anpassad roll anger Azure operationer som rollen ger åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-111">The **Actions** property of a custom role specifies the Azure operations to which the role grants access.</span></span> <span data-ttu-id="a4bc1-112">Det är en samling med åtgärden strängar som identifierar skyddbara drift av Azure-resursprovidrar.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-112">It is a collection of operation strings that identify securable operations of Azure resource providers.</span></span> <span data-ttu-id="a4bc1-113">Åtgärden strängar följa formatet för `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-113">Operation strings follow the format of `Microsoft.<ProviderName>/<ChildResourceType>/<action>`.</span></span> <span data-ttu-id="a4bc1-114">Åtgärden strängar som innehåller jokertecken (\*) bevilja åtkomst till alla åtgärder som motsvarar den åtgärd-strängen.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-114">Operation strings that contain wildcards (\*) grant access to all operations that match the operation string.</span></span> <span data-ttu-id="a4bc1-115">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a4bc1-115">For instance:</span></span>

* <span data-ttu-id="a4bc1-116">`*/read`ger åtkomst till Läs-och skrivåtgärder för alla typer av resurser för alla Azure-resurs-providers.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-116">`*/read` grants access to read operations for all resource types of all Azure resource providers.</span></span>
* <span data-ttu-id="a4bc1-117">`Microsoft.Compute/*`ger åtkomst till alla åtgärder för alla typer av resurser i Microsoft.Compute-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-117">`Microsoft.Compute/*` grants access to all operations for all resource types in the Microsoft.Compute resource provider.</span></span>
* <span data-ttu-id="a4bc1-118">`Microsoft.Network/*/read`ger åtkomst till Läs-och skrivåtgärder för alla typer av resurser i resursen Microsoft.Network-providern på Azure.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-118">`Microsoft.Network/*/read` grants access to read operations for all resource types in the Microsoft.Network resource provider of Azure.</span></span>
* <span data-ttu-id="a4bc1-119">`Microsoft.Compute/virtualMachines/*`ger åtkomst till alla åtgärder för virtuella datorer och dess underordnade resurstyper.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-119">`Microsoft.Compute/virtualMachines/*` grants access to all operations of virtual machines and its child resource types.</span></span>
* <span data-ttu-id="a4bc1-120">`Microsoft.Web/sites/restart/Action`ger åtkomst till att starta om webbplatser.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-120">`Microsoft.Web/sites/restart/Action` grants access to restart websites.</span></span>

<span data-ttu-id="a4bc1-121">Använd `Get-AzureRmProviderOperation` (i PowerShell) eller `azure provider operations show` (i Azure CLI) till Liståtgärder över providers som Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-121">Use `Get-AzureRmProviderOperation` (in PowerShell) or `azure provider operations show` (in Azure CLI) to list operations of Azure resource providers.</span></span> <span data-ttu-id="a4bc1-122">Du kan också använda dessa kommandon för att verifiera att en sträng för åtgärden är giltig och att expandera jokertecken åtgärden strängar.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-122">You may also use these commands to verify that an operation string is valid, and to expand wildcard operation strings.</span></span>

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Skärmbild av PowerShell - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![<span data-ttu-id="a4bc1-124">Azure CLI skärmbild - azure provider operations visa ”Microsoft.Compute/virtualMachines/ \* /Action”</span><span class="sxs-lookup"><span data-stu-id="a4bc1-124">Azure CLI screenshot - azure provider operations show "Microsoft.Compute/virtualMachines/\*/action"</span></span> ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a><span data-ttu-id="a4bc1-125">NotActions</span><span class="sxs-lookup"><span data-stu-id="a4bc1-125">NotActions</span></span>
<span data-ttu-id="a4bc1-126">Använd den **NotActions** egenskapen om en uppsättning åtgärder som du vill tillåta definieras enklare genom att utesluta vissa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-126">Use the **NotActions** property if the set of operations that you wish to allow is more easily defined by excluding restricted operations.</span></span> <span data-ttu-id="a4bc1-127">Åtkomst av en anpassad roll beräknas genom att subtrahera den **NotActions** åtgärder från den **åtgärder** åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-127">The access granted by a custom role is computed by subtracting the **NotActions** operations from the **Actions** operations.</span></span>

> [!NOTE]
> <span data-ttu-id="a4bc1-128">Om en användare har tilldelats en roll som inte omfattar en åtgärd i **NotActions**, och har tilldelats en andra roll som ger åtkomst till samma åtgärd som användaren tillåts att utföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-128">If a user is assigned a role that excludes an operation in **NotActions**, and is assigned a second role that grants access to the same operation, the user is allowed to perform that operation.</span></span> <span data-ttu-id="a4bc1-129">**NotActions** är inte en neka regel – det är ett bekvämt sätt att skapa en uppsättning tillåtna åtgärder när specifika åtgärder måste exkluderas.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-129">**NotActions** is not a deny rule – it is simply a convenient way to create a set of allowed operations when specific operations need to be excluded.</span></span>
>
>

## <a name="assignablescopes"></a><span data-ttu-id="a4bc1-130">AssignableScopes</span><span class="sxs-lookup"><span data-stu-id="a4bc1-130">AssignableScopes</span></span>
<span data-ttu-id="a4bc1-131">Den **AssignableScopes** -egenskapen för den anpassade rollen som anger scope (prenumerationer, resursgrupper och resurser) för den anpassade rollen som är tillgängliga för tilldelning.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-131">The **AssignableScopes** property of the custom role specifies the scopes (subscriptions, resource groups, or resources) within which the custom role is available for assignment.</span></span> <span data-ttu-id="a4bc1-132">Du kan se den anpassade rollen som tillgängliga för tilldelning i prenumerationer eller resursgrupper som kräver det och inte skapar oreda användarupplevelsen för resten av prenumerationer eller resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-132">You can make the custom role available for assignment in only the subscriptions or resource groups that require it, and not clutter user experience for the rest of the subscriptions or resource groups.</span></span>

<span data-ttu-id="a4bc1-133">Exempel på giltiga kan tilldelas omfång:</span><span class="sxs-lookup"><span data-stu-id="a4bc1-133">Examples of valid assignable scopes include:</span></span>

* <span data-ttu-id="a4bc1-134">”/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, ”/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - gör rollen tillgängliga för tilldelning i två prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-134">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, “/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - makes the role available for assignment in two subscriptions.</span></span>
* <span data-ttu-id="a4bc1-135">”/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - gör rollen tillgängliga för tilldelning i en enda prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-135">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - makes the role available for assignment in a single subscription.</span></span>
* <span data-ttu-id="a4bc1-136">”/ prenumerationer/c276fc76-9cd4-44c9-99a7-4fd71546436e/resursgrupper/nätverk” - tillgängliggör rollen för tilldelning i resursgruppen nätverk.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-136">“/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network” - makes the role available for assignment only in the Network resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="a4bc1-137">Du måste använda minst en prenumeration, resursgrupp eller resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-137">You must use at least one subscription, resource group, or resource ID.</span></span>
>
>

## <a name="custom-roles-access-control"></a><span data-ttu-id="a4bc1-138">Anpassade roller åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="a4bc1-138">Custom roles access control</span></span>
<span data-ttu-id="a4bc1-139">Den **AssignableScopes** -egenskapen för den anpassade rollen som också styr som kan visa, ändra och ta bort rollen.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-139">The **AssignableScopes** property of the custom role also controls who can view, modify, and delete the role.</span></span>

* <span data-ttu-id="a4bc1-140">Vem som kan skapa en anpassad roll?</span><span class="sxs-lookup"><span data-stu-id="a4bc1-140">Who can create a custom role?</span></span>
    <span data-ttu-id="a4bc1-141">Ägare (och administratörer för användare) för prenumerationer, resursgrupper och resurser kan skapa anpassade roller för användning i dessa scope.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-141">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can create custom roles for use in those scopes.</span></span>
    <span data-ttu-id="a4bc1-142">Användaren som skapar rollen måste kunna utföra `Microsoft.Authorization/roleDefinition/write` igen på alla de **AssignableScopes** av rollen.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-142">The user creating the role needs to be able to perform `Microsoft.Authorization/roleDefinition/write` operation on all the **AssignableScopes** of the role.</span></span>
* <span data-ttu-id="a4bc1-143">Vem som kan ändra en anpassad roll?</span><span class="sxs-lookup"><span data-stu-id="a4bc1-143">Who can modify a custom role?</span></span>
    <span data-ttu-id="a4bc1-144">Ägare (och administratörer för användare) för prenumerationer, resursgrupper och resurser kan ändra anpassade roller i dessa scope.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-144">Owners (and User Access Administrators) of subscriptions, resource groups, and resources can modify custom roles in those scopes.</span></span> <span data-ttu-id="a4bc1-145">Användarna behöver för att kunna utföra den `Microsoft.Authorization/roleDefinition/write` igen på alla de **AssignableScopes** av en anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-145">Users need to be able to perform the `Microsoft.Authorization/roleDefinition/write` operation on all the **AssignableScopes** of a custom role.</span></span>
* <span data-ttu-id="a4bc1-146">Vem som kan visa anpassade roller?</span><span class="sxs-lookup"><span data-stu-id="a4bc1-146">Who can view custom roles?</span></span>
    <span data-ttu-id="a4bc1-147">Alla inbyggda roller i Azure RBAC Tillåt visning av roller som är tillgängliga för tilldelning.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-147">All built-in roles in Azure RBAC allow viewing of roles that are available for assignment.</span></span> <span data-ttu-id="a4bc1-148">Användare som kan utföra den `Microsoft.Authorization/roleDefinition/read` åtgärden på ett scope kan visa RBAC-roller som är tillgängliga för tilldelning i detta scope.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-148">Users who can perform the `Microsoft.Authorization/roleDefinition/read` operation at a scope can view the RBAC roles that are available for assignment at that scope.</span></span>

## <a name="see-also"></a><span data-ttu-id="a4bc1-149">Se även</span><span class="sxs-lookup"><span data-stu-id="a4bc1-149">See also</span></span>
* <span data-ttu-id="a4bc1-150">[Rollbaserad åtkomstkontroll](role-based-access-control-configure.md): komma igång med RBAC på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-150">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in the Azure portal.</span></span>
* <span data-ttu-id="a4bc1-151">Lär dig mer om att hantera åtkomst med:</span><span class="sxs-lookup"><span data-stu-id="a4bc1-151">Learn how to manage access with:</span></span>
  * [<span data-ttu-id="a4bc1-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a4bc1-152">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
  * [<span data-ttu-id="a4bc1-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a4bc1-153">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
  * [<span data-ttu-id="a4bc1-154">REST-API</span><span class="sxs-lookup"><span data-stu-id="a4bc1-154">REST API</span></span>](role-based-access-control-manage-access-rest.md)
* <span data-ttu-id="a4bc1-155">[Inbyggda roller](role-based-access-built-in-roles.md): få information om de roller som levereras som standard i RBAC.</span><span class="sxs-lookup"><span data-stu-id="a4bc1-155">[Built-in roles](role-based-access-built-in-roles.md): Get details about the roles that come standard in RBAC.</span></span>
