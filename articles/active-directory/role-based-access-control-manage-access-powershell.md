---
title: "Hantera rollbaserad åtkomstkontroll (RBAC) med Azure PowerShell | Microsoft Docs"
description: "Hur du hanterar RBAC med Azure PowerShell, inklusive där roller, tilldela roller och ta bort rolltilldelningar."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: d7b11df21650b5cb27f9c3dd8306f8d12664185e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a><span data-ttu-id="01241-103">Hantera rollbaserad åtkomstkontroll med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="01241-103">Manage Role-Based Access Control with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="01241-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="01241-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="01241-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="01241-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="01241-106">REST-API</span><span class="sxs-lookup"><span data-stu-id="01241-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="01241-107">Du kan använda rollbaserad åtkomstkontroll (RBAC) i Azure-portalen och Azure Resource Management API för att hantera åtkomst till din prenumeration på en detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="01241-107">You can use Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Management API to manage access to your subscription at a fine-grained level.</span></span> <span data-ttu-id="01241-108">Med den här funktionen kan du bevilja åtkomst för Active Directory-användare, grupper eller tjänstens huvudnamn genom att tilldela vissa roller till dem för ett visst område.</span><span class="sxs-lookup"><span data-stu-id="01241-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

<span data-ttu-id="01241-109">Innan du kan använda PowerShell för att hantera RBAC, måste följande krav:</span><span class="sxs-lookup"><span data-stu-id="01241-109">Before you can use PowerShell to manage RBAC, you need the following prerequisites:</span></span>

* <span data-ttu-id="01241-110">Azure PowerShell version 0.8.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="01241-110">Azure PowerShell version 0.8.8 or later.</span></span> <span data-ttu-id="01241-111">Om du vill installera den senaste versionen och koppla den till din Azure-prenumeration, se [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="01241-111">To install the latest version and associate it with your Azure subscription, see [how to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="01241-112">Azure Resource Manager-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="01241-112">Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="01241-113">Installera den [Azure Resource Manager cmdlets](/powershell/azure/overview) i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="01241-113">Install the [Azure Resource Manager cmdlets](/powershell/azure/overview) in PowerShell.</span></span>

## <a name="list-roles"></a><span data-ttu-id="01241-114">Lista roller</span><span class="sxs-lookup"><span data-stu-id="01241-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="01241-115">Visa en lista över alla tillgängliga roller</span><span class="sxs-lookup"><span data-stu-id="01241-115">List all available roles</span></span>
<span data-ttu-id="01241-116">Lista RBAC-roller som är tillgängliga för tilldelning och inspektera de åtgärder som de beviljar åtkomst, använder `Get-AzureRmRoleDefinition`.</span><span class="sxs-lookup"><span data-stu-id="01241-116">To list RBAC roles that are available for assignment and to inspect the operations to which they grant access, use `Get-AzureRmRoleDefinition`.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="01241-118">Lista över åtgärder för en roll</span><span class="sxs-lookup"><span data-stu-id="01241-118">List actions of a role</span></span>
<span data-ttu-id="01241-119">Om du vill visa en lista med åtgärder för en viss roll, Använd `Get-AzureRmRoleDefinition <role name>`.</span><span class="sxs-lookup"><span data-stu-id="01241-119">To list the actions for a specific role, use `Get-AzureRmRoleDefinition <role name>`.</span></span>

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell-Get AzureRmRoleDefinition för en viss roll – skärmbild](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a><span data-ttu-id="01241-121">Se vem som har åtkomst</span><span class="sxs-lookup"><span data-stu-id="01241-121">See who has access</span></span>
<span data-ttu-id="01241-122">Använd om du vill visa en lista med RBAC åtkomst tilldelningar `Get-AzureRmRoleAssignment`.</span><span class="sxs-lookup"><span data-stu-id="01241-122">To list RBAC access assignments, use `Get-AzureRmRoleAssignment`.</span></span>

### <a name="list-role-assignments-at-a-specific-scope"></a><span data-ttu-id="01241-123">Lista rolltilldelningar för ett visst område</span><span class="sxs-lookup"><span data-stu-id="01241-123">List role assignments at a specific scope</span></span>
<span data-ttu-id="01241-124">Du kan se alla access-tilldelningar för en angiven prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="01241-124">You can see all the access assignments for a specified subscription, resource group, or resource.</span></span> <span data-ttu-id="01241-125">Till exempel om du vill visa alla aktiva tilldelningar för en resursgrupp, använda `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span><span class="sxs-lookup"><span data-stu-id="01241-125">For example, to see the all the active assignments for a resource group, use `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span></span>

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment för en resursgrupp – skärmbild](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a><span data-ttu-id="01241-127">Lista roller som tilldelas en användare</span><span class="sxs-lookup"><span data-stu-id="01241-127">List roles assigned to a user</span></span>
<span data-ttu-id="01241-128">Om du vill visa en lista med alla de roller som har tilldelats en angiven användare och roller som är tilldelade till de grupper som användaren tillhör, Använd `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span><span class="sxs-lookup"><span data-stu-id="01241-128">To list all the roles that are assigned to a specified user and the roles that are assigned to the groups to which the user belongs, use `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span></span>

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment för en användare – skärmbild](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a><span data-ttu-id="01241-130">Lista klassiska tjänstadministratören och coadmin rolltilldelningar</span><span class="sxs-lookup"><span data-stu-id="01241-130">List classic service administrator and coadmin role assignments</span></span>
<span data-ttu-id="01241-131">Listan åtkomst tilldelningar för klassiska prenumerationer administratör och coadministrators, Använd:</span><span class="sxs-lookup"><span data-stu-id="01241-131">To list access assignments for the classic subscription administrator and coadministrators, use:</span></span>

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a><span data-ttu-id="01241-132">Bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="01241-132">Grant access</span></span>
### <a name="search-for-object-ids"></a><span data-ttu-id="01241-133">Sök efter objekt-ID</span><span class="sxs-lookup"><span data-stu-id="01241-133">Search for object IDs</span></span>
<span data-ttu-id="01241-134">Om du vill tilldela en roll som du behöver identifiera både objektet (användare, grupp eller program) och omfång.</span><span class="sxs-lookup"><span data-stu-id="01241-134">To assign a role, you need to identify both the object (user, group, or application) and the scope.</span></span>

<span data-ttu-id="01241-135">Om du inte vet prenumerations-ID, hittar du den i den **prenumerationer** bladet på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="01241-135">If you don't know the subscription ID, you can find it in the **Subscriptions** blade on the Azure portal.</span></span> <span data-ttu-id="01241-136">Information om hur man frågar för prenumerations-ID finns [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="01241-136">To learn how to query for the subscription ID, see [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) on MSDN.</span></span>

<span data-ttu-id="01241-137">Om du vill hämta objekt-ID för en Azure AD-grupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="01241-137">To get the object ID for an Azure AD group, use:</span></span>

    Get-AzureRmADGroup -SearchString <group name in quotes>

<span data-ttu-id="01241-138">För att få objekt-ID för ett huvudnamn för tjänsten i Azure AD eller ett program, använder du:</span><span class="sxs-lookup"><span data-stu-id="01241-138">To get the object ID for an Azure AD service principal or application, use:</span></span>

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a><span data-ttu-id="01241-139">Tilldela en roll till ett program i omfånget för prenumeration</span><span class="sxs-lookup"><span data-stu-id="01241-139">Assign a role to an application at the subscription scope</span></span>
<span data-ttu-id="01241-140">Om du vill bevilja åtkomst till ett program i omfånget för prenumerationen, använder du:</span><span class="sxs-lookup"><span data-stu-id="01241-140">To grant access to an application at the subscription scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a><span data-ttu-id="01241-142">Tilldela en roll till en användare på Gruppomfång resurs</span><span class="sxs-lookup"><span data-stu-id="01241-142">Assign a role to a user at the resource group scope</span></span>
<span data-ttu-id="01241-143">Om du vill bevilja åtkomst till en användare på Gruppomfång resurs, använder du:</span><span class="sxs-lookup"><span data-stu-id="01241-143">To grant access to a user at the resource group scope, use:</span></span>

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a><span data-ttu-id="01241-145">Tilldela en roll till en grupp i omfånget för resurs</span><span class="sxs-lookup"><span data-stu-id="01241-145">Assign a role to a group at the resource scope</span></span>
<span data-ttu-id="01241-146">Om du vill bevilja åtkomst till en grupp definitionsområdet resurs, använder du:</span><span class="sxs-lookup"><span data-stu-id="01241-146">To grant access to a group at the resource scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a><span data-ttu-id="01241-148">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="01241-148">Remove access</span></span>
<span data-ttu-id="01241-149">Ta bort åtkomst för användare, grupper och program med:</span><span class="sxs-lookup"><span data-stu-id="01241-149">To remove access for users, groups, and applications, use:</span></span>

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell ta bort AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="01241-151">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="01241-151">Create a custom role</span></span>
<span data-ttu-id="01241-152">Du kan skapa en anpassad roll med den ```New-AzureRmRoleDefinition``` kommando.</span><span class="sxs-lookup"><span data-stu-id="01241-152">To create a custom role, use the ```New-AzureRmRoleDefinition``` command.</span></span> <span data-ttu-id="01241-153">Det finns två metoder för att strukturera rollen med hjälp av PSRoleDefinitionObject eller en JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="01241-153">There are two methods of structuring the role, using PSRoleDefinitionObject or a JSON template.</span></span> 

## <a name="get-actions-for-a-resource-provider"></a><span data-ttu-id="01241-154">Hämta åtgärder för en Resursprovider</span><span class="sxs-lookup"><span data-stu-id="01241-154">Get Actions for a Resource Provider</span></span>
<span data-ttu-id="01241-155">När du skapar anpassade roller från början, är det viktigt att veta alla möjliga åtgärder från resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="01241-155">When You are creating custom roles from scratch, it is important to know all the possible operations from the resource providers.</span></span>
<span data-ttu-id="01241-156">Använd den ```Get-AzureRMProviderOperation``` kommando för att hämta informationen.</span><span class="sxs-lookup"><span data-stu-id="01241-156">Use the ```Get-AzureRMProviderOperation``` command to get this information.</span></span>
<span data-ttu-id="01241-157">Om du vill kontrollera till exempel använda det här kommandot alla tillgängliga åtgärder för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="01241-157">For example, if you want to check all the available operations for virtual Machine use this command:</span></span>

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a><span data-ttu-id="01241-158">Skapa en roll med PSRoleDefinitionObject</span><span class="sxs-lookup"><span data-stu-id="01241-158">Create role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="01241-159">När du använder PowerShell för att skapa en anpassad roll kan du börja om från början eller Använd en av de [inbyggda roller](role-based-access-built-in-roles.md) som utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="01241-159">When you use PowerShell to create a custom role, you can start from scratch or use one of the [built-in roles](role-based-access-built-in-roles.md) as a starting point.</span></span> <span data-ttu-id="01241-160">I exemplet i det här avsnittet börjar med en inbyggd roll och anpassar den med mer behörigheter.</span><span class="sxs-lookup"><span data-stu-id="01241-160">The example in this section starts with a built-in role and then customizes it with more privileges.</span></span> <span data-ttu-id="01241-161">Redigera de attribut du vill lägga till den *åtgärder*, *notActions*, eller *scope* som du vill använda och sedan spara ändringarna som en ny roll.</span><span class="sxs-lookup"><span data-stu-id="01241-161">Edit the attributes to add the *Actions*, *notActions*, or *scopes* that you want, and then save the changes as a new role.</span></span>

<span data-ttu-id="01241-162">Följande exempel startar med den *Virtual Machine-deltagare* roll och använder som att skapa en anpassad roll kallas *virtuella operatorn*.</span><span class="sxs-lookup"><span data-stu-id="01241-162">The following example starts with the *Virtual Machine Contributor* role and uses that to create a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="01241-163">Den nya rollen ger åtkomst till alla läsåtgärder av *Microsoft.Compute*, *Microsoft.Storage*, och *Microsoft.Network* providers och ger åtkomst till företagsresurser att starta , starta om och övervaka virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="01241-163">The new role grants access to all read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access to start, restart, and monitor virtual machines.</span></span> <span data-ttu-id="01241-164">Den anpassade rollen som kan användas i två prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="01241-164">The custom role can be used in two subscriptions.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

### <a name="create-role-with-json-template"></a><span data-ttu-id="01241-166">Skapa en roll med JSON-mall</span><span class="sxs-lookup"><span data-stu-id="01241-166">Create role with JSON template</span></span>
<span data-ttu-id="01241-167">En JSON-mall kan användas som källdefinitionen för den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="01241-167">A JSON template can be used as the source definition for the custom role.</span></span> <span data-ttu-id="01241-168">I följande exempel skapas en anpassad roll som ger läsbehörighet för lagring och beräkning resurser, åtkomst till stöd för, och lägger till rollen två prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="01241-168">The following example creates a custom role that allows read access to storage and compute resources, access to support, and adds that role to two subscriptions.</span></span> <span data-ttu-id="01241-169">Skapa en ny fil `C:\CustomRoles\customrole1.json` med följande exempel.</span><span class="sxs-lookup"><span data-stu-id="01241-169">Create a new file `C:\CustomRoles\customrole1.json` with the following example.</span></span> <span data-ttu-id="01241-170">Id som ska anges till `null` inledande rollen skapas som ett nytt ID genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="01241-170">The Id should be set to `null` on initial role creation as a new ID is generated automatically.</span></span> 

```
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```
<span data-ttu-id="01241-171">Kör följande PowerShell-kommando för att lägga till rollen till prenumerationerna:</span><span class="sxs-lookup"><span data-stu-id="01241-171">To add the role to the subscriptions, run the following PowerShell command:</span></span>
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a><span data-ttu-id="01241-172">Ändra en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="01241-172">Modify a custom role</span></span>
<span data-ttu-id="01241-173">Precis som skapar en anpassad roll kan du ändra en befintlig anpassad roll med hjälp av PSRoleDefinitionObject eller en JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="01241-173">Similar to creating a custom role, you can modify an existing custom role using either the PSRoleDefinitionObject or a JSON template.</span></span>

### <a name="modify-role-with-psroledefinitionobject"></a><span data-ttu-id="01241-174">Ändra roll med PSRoleDefinitionObject</span><span class="sxs-lookup"><span data-stu-id="01241-174">Modify role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="01241-175">Använd först om du vill ändra en anpassad roll i `Get-AzureRmRoleDefinition` kommando för att hämta rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="01241-175">To modify a custom role, first, use the `Get-AzureRmRoleDefinition` command to retrieve the role definition.</span></span> <span data-ttu-id="01241-176">Andra, gör ändringarna i rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="01241-176">Second, make the desired changes to the role definition.</span></span> <span data-ttu-id="01241-177">Använd slutligen den `Set-AzureRmRoleDefinition` kommando för att spara ändrade rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="01241-177">Finally, use the `Set-AzureRmRoleDefinition` command to save the modified role definition.</span></span>

<span data-ttu-id="01241-178">I följande exempel läggs den `Microsoft.Insights/diagnosticSettings/*` igen till den *virtuella operatorn* anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="01241-178">The following example adds the `Microsoft.Insights/diagnosticSettings/*` operation to the *Virtual Machine Operator* custom role.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Set AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

<span data-ttu-id="01241-180">I följande exempel lägger till en Azure-prenumeration kan tilldelas omfång för den *virtuella operatorn* anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="01241-180">The following example adds an Azure subscription to the assignable scopes of the *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Set AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a><span data-ttu-id="01241-182">Ändra roll med JSON-mall</span><span class="sxs-lookup"><span data-stu-id="01241-182">Modify role with JSON template</span></span>
<span data-ttu-id="01241-183">Med mallen tidigare JSON kan ändra du enkelt en befintlig anpassad roll om du vill lägga till eller ta bort åtgärder.</span><span class="sxs-lookup"><span data-stu-id="01241-183">Using the previous JSON template, you can easily modify an existing custom role to add or remove Actions.</span></span> <span data-ttu-id="01241-184">Uppdatera JSON-mall och lägga till den skrivskyddade åtgärden för nätverk som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="01241-184">Update the JSON template and add the read action for networking as shown in the following example.</span></span> <span data-ttu-id="01241-185">De definitioner som förtecknas i mallen tillämpas inte kumulativt på en befintlig definition, vilket innebär att rollen visas exakt som du anger i mallen.</span><span class="sxs-lookup"><span data-stu-id="01241-185">The definitions listed in the template are not cumulatively applied to an existing definition, meaning that the role appears exactly as you specify in the template.</span></span> <span data-ttu-id="01241-186">Du måste också uppdatera Id-fältet med ID i rollen.</span><span class="sxs-lookup"><span data-stu-id="01241-186">You also need to update the Id field with the ID of the role.</span></span> <span data-ttu-id="01241-187">Om du är osäker på det här värdet är, kan du använda den `Get-AzureRmRoleDefinition` för att hämta informationen.</span><span class="sxs-lookup"><span data-stu-id="01241-187">If you aren't sure what this value is, you can use the `Get-AzureRmRoleDefinition` cmdlet to get this information.</span></span>

```
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

<span data-ttu-id="01241-188">Om du vill uppdatera den befintliga rollen, kör du följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="01241-188">To update the existing role, run the following PowerShell command:</span></span>
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a><span data-ttu-id="01241-189">Ta bort en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="01241-189">Delete a custom role</span></span>
<span data-ttu-id="01241-190">Ta bort en anpassad roll genom att använda den `Remove-AzureRmRoleDefinition` kommando.</span><span class="sxs-lookup"><span data-stu-id="01241-190">To delete a custom role, use the `Remove-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="01241-191">I följande exempel tar bort den *virtuella operatorn* anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="01241-191">The following example removes the *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell ta bort AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a><span data-ttu-id="01241-193">Lista över anpassade roller</span><span class="sxs-lookup"><span data-stu-id="01241-193">List custom roles</span></span>
<span data-ttu-id="01241-194">Om du vill visa de roller som är tillgängliga för tilldelning på scopenivå, använder den `Get-AzureRmRoleDefinition` kommando.</span><span class="sxs-lookup"><span data-stu-id="01241-194">To list the roles that are available for assignment at a scope, use the `Get-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="01241-195">I följande exempel visar en lista över alla roller som är tillgängliga för tilldelning i den valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="01241-195">The following example lists all roles that are available for assignment in the selected subscription.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

<span data-ttu-id="01241-197">I följande exempel visas den *virtuella operatorn* anpassad roll är inte tillgänglig i den *Production4* prenumeration eftersom den prenumerationen finns inte i den **AssignableScopes** av rollen.</span><span class="sxs-lookup"><span data-stu-id="01241-197">In the following example, the *Virtual Machine Operator* custom role isn’t available in the *Production4* subscription because that subscription isn’t in the **AssignableScopes** of the role.</span></span>

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a><span data-ttu-id="01241-199">Se även</span><span class="sxs-lookup"><span data-stu-id="01241-199">See also</span></span>
* <span data-ttu-id="01241-200">[Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span><span class="sxs-lookup"><span data-stu-id="01241-200">[Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span></span>

