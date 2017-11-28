---
title: "aaaManage rollbaserad åtkomstkontroll (RBAC) med Azure PowerShell | Microsoft Docs"
description: "Hur toomanage RBAC med Azure PowerShell, inklusive där roller, tilldela roller och ta bort rolltilldelningar."
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
ms.openlocfilehash: fa44991113e75b345177867b0bede38de4373e04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a><span data-ttu-id="8eea0-103">Hantera rollbaserad åtkomstkontroll med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8eea0-103">Manage Role-Based Access Control with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8eea0-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8eea0-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="8eea0-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8eea0-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="8eea0-106">REST-API</span><span class="sxs-lookup"><span data-stu-id="8eea0-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="8eea0-107">Du kan använda rollbaserad åtkomstkontroll (RBAC) i hello Azure-portalen och Azure Resource Management API toomanage åtkomst tooyour prenumeration på en detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="8eea0-107">You can use Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Management API toomanage access tooyour subscription at a fine-grained level.</span></span> <span data-ttu-id="8eea0-108">Med den här funktionen kan du bevilja åtkomst för användare, grupper eller tjänstens huvudnamn i Active Directory genom att tilldela vissa roller toothem för ett visst område.</span><span class="sxs-lookup"><span data-stu-id="8eea0-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

<span data-ttu-id="8eea0-109">Innan du kan använda PowerShell toomanage RBAC måste hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="8eea0-109">Before you can use PowerShell toomanage RBAC, you need hello following prerequisites:</span></span>

* <span data-ttu-id="8eea0-110">Azure PowerShell version 0.8.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8eea0-110">Azure PowerShell version 0.8.8 or later.</span></span> <span data-ttu-id="8eea0-111">tooinstall hello senaste versionen och koppla den med din Azure-prenumeration finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8eea0-111">tooinstall hello latest version and associate it with your Azure subscription, see [how tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="8eea0-112">Azure Resource Manager-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="8eea0-112">Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="8eea0-113">Installera hello [Azure Resource Manager cmdlets](/powershell/azure/overview) i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8eea0-113">Install hello [Azure Resource Manager cmdlets](/powershell/azure/overview) in PowerShell.</span></span>

## <a name="list-roles"></a><span data-ttu-id="8eea0-114">Lista roller</span><span class="sxs-lookup"><span data-stu-id="8eea0-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="8eea0-115">Visa en lista över alla tillgängliga roller</span><span class="sxs-lookup"><span data-stu-id="8eea0-115">List all available roles</span></span>
<span data-ttu-id="8eea0-116">toolist RBAC-roller som är tillgängliga för tilldelning och tooinspect hello operations toowhich de beviljar åtkomst, Använd `Get-AzureRmRoleDefinition`.</span><span class="sxs-lookup"><span data-stu-id="8eea0-116">toolist RBAC roles that are available for assignment and tooinspect hello operations toowhich they grant access, use `Get-AzureRmRoleDefinition`.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="8eea0-118">Lista över åtgärder för en roll</span><span class="sxs-lookup"><span data-stu-id="8eea0-118">List actions of a role</span></span>
<span data-ttu-id="8eea0-119">toolist hello åtgärder för en viss roll använder `Get-AzureRmRoleDefinition <role name>`.</span><span class="sxs-lookup"><span data-stu-id="8eea0-119">toolist hello actions for a specific role, use `Get-AzureRmRoleDefinition <role name>`.</span></span>

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell-Get AzureRmRoleDefinition för en viss roll – skärmbild](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a><span data-ttu-id="8eea0-121">Se vem som har åtkomst</span><span class="sxs-lookup"><span data-stu-id="8eea0-121">See who has access</span></span>
<span data-ttu-id="8eea0-122">använda toolist RBAC åtkomst tilldelningar `Get-AzureRmRoleAssignment`.</span><span class="sxs-lookup"><span data-stu-id="8eea0-122">toolist RBAC access assignments, use `Get-AzureRmRoleAssignment`.</span></span>

### <a name="list-role-assignments-at-a-specific-scope"></a><span data-ttu-id="8eea0-123">Lista rolltilldelningar för ett visst område</span><span class="sxs-lookup"><span data-stu-id="8eea0-123">List role assignments at a specific scope</span></span>
<span data-ttu-id="8eea0-124">Du kan se alla hello åtkomst tilldelningar för en angiven prenumeration, resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="8eea0-124">You can see all hello access assignments for a specified subscription, resource group, or resource.</span></span> <span data-ttu-id="8eea0-125">Till exempel toosee hello alla aktiva hello-tilldelningar för en resursgrupp, Använd `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span><span class="sxs-lookup"><span data-stu-id="8eea0-125">For example, toosee hello all hello active assignments for a resource group, use `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.</span></span>

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment för en resursgrupp – skärmbild](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-tooa-user"></a><span data-ttu-id="8eea0-127">Lista roller tilldelad tooa användare</span><span class="sxs-lookup"><span data-stu-id="8eea0-127">List roles assigned tooa user</span></span>
<span data-ttu-id="8eea0-128">toolist alla hello-roller som är tilldelade tooa angivna användare och hello-roller som tilldelas toohello grupper toowhich hello användaren tillhör, Använd `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span><span class="sxs-lookup"><span data-stu-id="8eea0-128">toolist all hello roles that are assigned tooa specified user and hello roles that are assigned toohello groups toowhich hello user belongs, use `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.</span></span>

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment för en användare – skärmbild](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a><span data-ttu-id="8eea0-130">Lista klassiska tjänstadministratören och coadmin rolltilldelningar</span><span class="sxs-lookup"><span data-stu-id="8eea0-130">List classic service administrator and coadmin role assignments</span></span>
<span data-ttu-id="8eea0-131">toolist åtkomst tilldelningar för hello klassiska prenumerationer administratör och coadministrators, Använd:</span><span class="sxs-lookup"><span data-stu-id="8eea0-131">toolist access assignments for hello classic subscription administrator and coadministrators, use:</span></span>

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a><span data-ttu-id="8eea0-132">Bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="8eea0-132">Grant access</span></span>
### <a name="search-for-object-ids"></a><span data-ttu-id="8eea0-133">Sök efter objekt-ID</span><span class="sxs-lookup"><span data-stu-id="8eea0-133">Search for object IDs</span></span>
<span data-ttu-id="8eea0-134">tooassign en roll måste tooidentify både hello-objekt (användare, grupp eller program) och hello omfång.</span><span class="sxs-lookup"><span data-stu-id="8eea0-134">tooassign a role, you need tooidentify both hello object (user, group, or application) and hello scope.</span></span>

<span data-ttu-id="8eea0-135">Om du inte vet hello prenumerations-ID, du kan hitta den i hello **prenumerationer** bladet på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-135">If you don't know hello subscription ID, you can find it in hello **Subscriptions** blade on hello Azure portal.</span></span> <span data-ttu-id="8eea0-136">hur tooquery för hello prenumerations-ID, se toolearn [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="8eea0-136">toolearn how tooquery for hello subscription ID, see [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) on MSDN.</span></span>

<span data-ttu-id="8eea0-137">Använd tooget hello objekt-ID för en Azure AD-grupp:</span><span class="sxs-lookup"><span data-stu-id="8eea0-137">tooget hello object ID for an Azure AD group, use:</span></span>

    Get-AzureRmADGroup -SearchString <group name in quotes>

<span data-ttu-id="8eea0-138">tooget hello objekt-ID för en Azure AD huvudnamn för tjänsten eller programmet, Använd:</span><span class="sxs-lookup"><span data-stu-id="8eea0-138">tooget hello object ID for an Azure AD service principal or application, use:</span></span>

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a><span data-ttu-id="8eea0-139">Tilldela en roll tooan programmet hello prenumeration definitionsområdet</span><span class="sxs-lookup"><span data-stu-id="8eea0-139">Assign a role tooan application at hello subscription scope</span></span>
<span data-ttu-id="8eea0-140">toogrant tooan programmet access definitionsområdet hello prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="8eea0-140">toogrant access tooan application at hello subscription scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a><span data-ttu-id="8eea0-142">Tilldela en användare med rollen tooa på hello resurs Gruppomfång</span><span class="sxs-lookup"><span data-stu-id="8eea0-142">Assign a role tooa user at hello resource group scope</span></span>
<span data-ttu-id="8eea0-143">toogrant åtkomst tooa användaren vid hello resurs Gruppomfång, Använd:</span><span class="sxs-lookup"><span data-stu-id="8eea0-143">toogrant access tooa user at hello resource group scope, use:</span></span>

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a><span data-ttu-id="8eea0-145">Tilldela en roll tooa grupp hello resurs definitionsområdet</span><span class="sxs-lookup"><span data-stu-id="8eea0-145">Assign a role tooa group at hello resource scope</span></span>
<span data-ttu-id="8eea0-146">toogrant tooa åtkomstgruppen definitionsområdet hello resurs, Använd:</span><span class="sxs-lookup"><span data-stu-id="8eea0-146">toogrant access tooa group at hello resource scope, use:</span></span>

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a><span data-ttu-id="8eea0-148">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="8eea0-148">Remove access</span></span>
<span data-ttu-id="8eea0-149">tooremove åtkomst för användare, grupper och program, Använd:</span><span class="sxs-lookup"><span data-stu-id="8eea0-149">tooremove access for users, groups, and applications, use:</span></span>

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell ta bort AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="8eea0-151">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="8eea0-151">Create a custom role</span></span>
<span data-ttu-id="8eea0-152">toocreate en anpassad roll använder hello ```New-AzureRmRoleDefinition``` kommando.</span><span class="sxs-lookup"><span data-stu-id="8eea0-152">toocreate a custom role, use hello ```New-AzureRmRoleDefinition``` command.</span></span> <span data-ttu-id="8eea0-153">Det finns två metoder för att strukturera hello roll, med hjälp av PSRoleDefinitionObject eller en JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="8eea0-153">There are two methods of structuring hello role, using PSRoleDefinitionObject or a JSON template.</span></span> 

## <a name="get-actions-for-a-resource-provider"></a><span data-ttu-id="8eea0-154">Hämta åtgärder för en Resursprovider</span><span class="sxs-lookup"><span data-stu-id="8eea0-154">Get Actions for a Resource Provider</span></span>
<span data-ttu-id="8eea0-155">När du skapar anpassade roller från början, är det viktigt tooknow alla hello möjliga åtgärder från hello resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="8eea0-155">When You are creating custom roles from scratch, it is important tooknow all hello possible operations from hello resource providers.</span></span>
<span data-ttu-id="8eea0-156">Använd hello ```Get-AzureRMProviderOperation``` kommandot tooget informationen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-156">Use hello ```Get-AzureRMProviderOperation``` command tooget this information.</span></span>
<span data-ttu-id="8eea0-157">Om du vill toocheck använder alla hello tillgängliga åtgärderna för den virtuella datorn det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="8eea0-157">For example, if you want toocheck all hello available operations for virtual Machine use this command:</span></span>

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a><span data-ttu-id="8eea0-158">Skapa en roll med PSRoleDefinitionObject</span><span class="sxs-lookup"><span data-stu-id="8eea0-158">Create role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="8eea0-159">När du använder PowerShell toocreate en anpassad roll kan du börja om från början eller Använd en av hello [inbyggda roller](role-based-access-built-in-roles.md) som utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="8eea0-159">When you use PowerShell toocreate a custom role, you can start from scratch or use one of hello [built-in roles](role-based-access-built-in-roles.md) as a starting point.</span></span> <span data-ttu-id="8eea0-160">hello exemplet i det här avsnittet börjar med en inbyggd roll och anpassar den med mer behörigheter.</span><span class="sxs-lookup"><span data-stu-id="8eea0-160">hello example in this section starts with a built-in role and then customizes it with more privileges.</span></span> <span data-ttu-id="8eea0-161">Redigera hello attribut tooadd hello *åtgärder*, *notActions*, eller *scope* som du vill och sedan spara hello ändringar som en ny roll.</span><span class="sxs-lookup"><span data-stu-id="8eea0-161">Edit hello attributes tooadd hello *Actions*, *notActions*, or *scopes* that you want, and then save hello changes as a new role.</span></span>

<span data-ttu-id="8eea0-162">hello följande exempel startar med hello *Virtual Machine-deltagare* roll och använder den toocreate en anpassad roll kallas *virtuella operatorn*.</span><span class="sxs-lookup"><span data-stu-id="8eea0-162">hello following example starts with hello *Virtual Machine Contributor* role and uses that toocreate a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="8eea0-163">hello ny roll beviljar åtkomst tooall läsåtgärder av *Microsoft.Compute*, *Microsoft.Storage*, och *Microsoft.Network* resurs providers och beviljar åtkomst toostart, starta om och övervaka virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8eea0-163">hello new role grants access tooall read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access toostart, restart, and monitor virtual machines.</span></span> <span data-ttu-id="8eea0-164">hello anpassad roll kan användas i två prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="8eea0-164">hello custom role can be used in two subscriptions.</span></span>

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

### <a name="create-role-with-json-template"></a><span data-ttu-id="8eea0-166">Skapa en roll med JSON-mall</span><span class="sxs-lookup"><span data-stu-id="8eea0-166">Create role with JSON template</span></span>
<span data-ttu-id="8eea0-167">En JSON-mall kan användas som hello definitionen av datakällan för hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="8eea0-167">A JSON template can be used as hello source definition for hello custom role.</span></span> <span data-ttu-id="8eea0-168">hello följande exempel skapar en anpassad roll som tillåter läsbehörighet toostorage och beräkna resurser kan komma åt toosupport och lägger till rollen tootwo prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="8eea0-168">hello following example creates a custom role that allows read access toostorage and compute resources, access toosupport, and adds that role tootwo subscriptions.</span></span> <span data-ttu-id="8eea0-169">Skapa en ny fil `C:\CustomRoles\customrole1.json` med hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="8eea0-169">Create a new file `C:\CustomRoles\customrole1.json` with hello following example.</span></span> <span data-ttu-id="8eea0-170">hello Id ska anges för`null` inledande rollen skapas som ett nytt ID genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="8eea0-170">hello Id should be set too`null` on initial role creation as a new ID is generated automatically.</span></span> 

```
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access tooAzure storage and compute resources and access toosupport",
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
<span data-ttu-id="8eea0-171">tooadd hello rollen toohello prenumerationer, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="8eea0-171">tooadd hello role toohello subscriptions, run hello following PowerShell command:</span></span>
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a><span data-ttu-id="8eea0-172">Ändra en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="8eea0-172">Modify a custom role</span></span>
<span data-ttu-id="8eea0-173">Liknande toocreating en anpassad roll kan du ändra en befintlig anpassad roll med hjälp av hello PSRoleDefinitionObject eller en JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="8eea0-173">Similar toocreating a custom role, you can modify an existing custom role using either hello PSRoleDefinitionObject or a JSON template.</span></span>

### <a name="modify-role-with-psroledefinitionobject"></a><span data-ttu-id="8eea0-174">Ändra roll med PSRoleDefinitionObject</span><span class="sxs-lookup"><span data-stu-id="8eea0-174">Modify role with PSRoleDefinitionObject</span></span>
<span data-ttu-id="8eea0-175">toomodify en anpassad roll först använder hello `Get-AzureRmRoleDefinition` kommandot tooretrieve hello rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-175">toomodify a custom role, first, use hello `Get-AzureRmRoleDefinition` command tooretrieve hello role definition.</span></span> <span data-ttu-id="8eea0-176">Därefter kontrollera hello önskade ändringar toohello rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-176">Second, make hello desired changes toohello role definition.</span></span> <span data-ttu-id="8eea0-177">Använd slutligen hello `Set-AzureRmRoleDefinition` kommandot toosave hello ändrade rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-177">Finally, use hello `Set-AzureRmRoleDefinition` command toosave hello modified role definition.</span></span>

<span data-ttu-id="8eea0-178">hello följande exempel läggs hello `Microsoft.Insights/diagnosticSettings/*` åtgärden toohello *virtuella operatorn* anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="8eea0-178">hello following example adds hello `Microsoft.Insights/diagnosticSettings/*` operation toohello *Virtual Machine Operator* custom role.</span></span>

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Set AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

<span data-ttu-id="8eea0-180">hello följande exempel lägger till en Azure-prenumeration toohello tilldelningsbara scope för hello *virtuella operatorn* anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="8eea0-180">hello following example adds an Azure subscription toohello assignable scopes of hello *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Set AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a><span data-ttu-id="8eea0-182">Ändra roll med JSON-mall</span><span class="sxs-lookup"><span data-stu-id="8eea0-182">Modify role with JSON template</span></span>
<span data-ttu-id="8eea0-183">Med hello tidigare JSON-mall kan du enkelt ändra en befintlig anpassad roll tooadd eller ta bort åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8eea0-183">Using hello previous JSON template, you can easily modify an existing custom role tooadd or remove Actions.</span></span> <span data-ttu-id="8eea0-184">Uppdatera hello JSON-mall och lägga till hello skrivskyddade åtgärd för nätverk som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="8eea0-184">Update hello JSON template and add hello read action for networking as shown in hello following example.</span></span> <span data-ttu-id="8eea0-185">hello definitioner som förtecknas i hello mallen är inte kumulativt tillämpade tooan befintlig definition, vilket innebär att rollen hello visas exakt som du anger i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-185">hello definitions listed in hello template are not cumulatively applied tooan existing definition, meaning that hello role appears exactly as you specify in hello template.</span></span> <span data-ttu-id="8eea0-186">Du måste också tooupdate hello Id-fältet med hello ID hello roll.</span><span class="sxs-lookup"><span data-stu-id="8eea0-186">You also need tooupdate hello Id field with hello ID of hello role.</span></span> <span data-ttu-id="8eea0-187">Om du är osäker på det här värdet är, kan du använda hello `Get-AzureRmRoleDefinition` cmdlet tooget informationen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-187">If you aren't sure what this value is, you can use hello `Get-AzureRmRoleDefinition` cmdlet tooget this information.</span></span>

```
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access tooAzure storage and compute resources and access toosupport",
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

<span data-ttu-id="8eea0-188">tooupdate hello befintliga rollen, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="8eea0-188">tooupdate hello existing role, run hello following PowerShell command:</span></span>
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a><span data-ttu-id="8eea0-189">Ta bort en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="8eea0-189">Delete a custom role</span></span>
<span data-ttu-id="8eea0-190">toodelete en anpassad roll använder hello `Remove-AzureRmRoleDefinition` kommando.</span><span class="sxs-lookup"><span data-stu-id="8eea0-190">toodelete a custom role, use hello `Remove-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="8eea0-191">hello följande exempel tar bort hello *virtuella operatorn* anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="8eea0-191">hello following example removes hello *Virtual Machine Operator* custom role.</span></span>

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell ta bort AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a><span data-ttu-id="8eea0-193">Lista över anpassade roller</span><span class="sxs-lookup"><span data-stu-id="8eea0-193">List custom roles</span></span>
<span data-ttu-id="8eea0-194">toolist hello roller som är tillgängliga för tilldelning på scopenivå, använder hello `Get-AzureRmRoleDefinition` kommando.</span><span class="sxs-lookup"><span data-stu-id="8eea0-194">toolist hello roles that are available for assignment at a scope, use hello `Get-AzureRmRoleDefinition` command.</span></span>

<span data-ttu-id="8eea0-195">hello som följande exempel visar en lista över alla roller som är tillgängliga för tilldelning i hello valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-195">hello following example lists all roles that are available for assignment in hello selected subscription.</span></span>

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

<span data-ttu-id="8eea0-197">I följande exempel hello, hello *virtuella operatorn* anpassad roll är inte tillgänglig i hello *Production4* prenumerationen eftersom den prenumerationen finns inte i hello  **AssignableScopes** hello-rollen.</span><span class="sxs-lookup"><span data-stu-id="8eea0-197">In hello following example, hello *Virtual Machine Operator* custom role isn’t available in hello *Production4* subscription because that subscription isn’t in hello **AssignableScopes** of hello role.</span></span>

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a><span data-ttu-id="8eea0-199">Se även</span><span class="sxs-lookup"><span data-stu-id="8eea0-199">See also</span></span>
* <span data-ttu-id="8eea0-200">[Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span><span class="sxs-lookup"><span data-stu-id="8eea0-200">[Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]</span></span>

