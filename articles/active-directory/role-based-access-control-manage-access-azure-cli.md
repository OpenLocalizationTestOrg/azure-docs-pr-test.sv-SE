---
title: "aaaManage rollbaserad åtkomstkontroll (RBAC) med Azure CLI | Microsoft Docs"
description: "Lär dig hur toomanage rollbaserad åtkomstkontroll (RBAC) med hello Azure kommandoradsverktyget gränssnitt av lista över roller och rollen åtgärder och genom att tilldela roller toohello prenumeration och application-scope."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 438418e5f6ee9b98908c9c264d516eb722a4e26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-hello-azure-command-line-interface"></a><span data-ttu-id="6a204-103">Hantera rollbaserad åtkomstkontroll med hello Azure-kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="6a204-103">Manage Role-Based Access Control with hello Azure command-line interface</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6a204-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a204-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="6a204-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6a204-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="6a204-106">REST-API</span><span class="sxs-lookup"><span data-stu-id="6a204-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)


<span data-ttu-id="6a204-107">Du kan använda rollbaserad åtkomstkontroll (RBAC) i hello Azure-portalen och Azure Resource Manager API toomanage åtkomst tooyour prenumeration och resurser på en detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="6a204-107">You can use Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Manager API toomanage access tooyour subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="6a204-108">Med den här funktionen kan du bevilja åtkomst för användare, grupper eller tjänstens huvudnamn i Active Directory genom att tilldela vissa roller toothem för ett visst område.</span><span class="sxs-lookup"><span data-stu-id="6a204-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

<span data-ttu-id="6a204-109">Innan du kan använda hello Azure-kommandoradsgränssnittet (CLI) toomanage RBAC, måste du ha hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="6a204-109">Before you can use hello Azure command-line interface (CLI) toomanage RBAC, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="6a204-110">Azure CLI version 0.8.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="6a204-110">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="6a204-111">tooinstall hello senaste versionen och koppla den med din Azure-prenumeration finns [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6a204-111">tooinstall hello latest version and associate it with your Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="6a204-112">Azure Resource Manager i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6a204-112">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="6a204-113">Gå för[Using hello Azure CLI med hello Resource Manager](../xplat-cli-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6a204-113">Go too[Using hello Azure CLI with hello Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

## <a name="list-roles"></a><span data-ttu-id="6a204-114">Lista roller</span><span class="sxs-lookup"><span data-stu-id="6a204-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="6a204-115">Visa en lista över alla tillgängliga roller</span><span class="sxs-lookup"><span data-stu-id="6a204-115">List all available roles</span></span>
<span data-ttu-id="6a204-116">toolist använder alla tillgängliga roller:</span><span class="sxs-lookup"><span data-stu-id="6a204-116">toolist all available roles, use:</span></span>

        azure role list

<span data-ttu-id="6a204-117">hello följande exempel visar hello lista över *alla tillgängliga roller*.</span><span class="sxs-lookup"><span data-stu-id="6a204-117">hello following example shows hello list of *all available roles*.</span></span>

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Azure RBAC kommandoraden - azure rollen list - skärmbild](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="6a204-119">Lista över åtgärder för en roll</span><span class="sxs-lookup"><span data-stu-id="6a204-119">List actions of a role</span></span>
<span data-ttu-id="6a204-120">toolist hello åtgärder för en roll, Använd:</span><span class="sxs-lookup"><span data-stu-id="6a204-120">toolist hello actions of a role, use:</span></span>

    azure role show "<role name>"

<span data-ttu-id="6a204-121">hello följande exempel visar hello åtgärder av hello *deltagare* och *Virtual Machine-deltagare* roller.</span><span class="sxs-lookup"><span data-stu-id="6a204-121">hello following example shows hello actions of hello *Contributor* and *Virtual Machine Contributor* roles.</span></span>

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Azure RBAC kommandoraden - azure rollen show - skärmbild](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a><span data-ttu-id="6a204-123">Listan åtkomst</span><span class="sxs-lookup"><span data-stu-id="6a204-123">List access</span></span>
### <a name="list-role-assignments-effective-on-a-resource-group"></a><span data-ttu-id="6a204-124">Lista rolltilldelningar gälla för en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="6a204-124">List role assignments effective on a resource group</span></span>
<span data-ttu-id="6a204-125">toolist hello rolltilldelningar som finns i en resursgrupp, Använd:</span><span class="sxs-lookup"><span data-stu-id="6a204-125">toolist hello role assignments that exist in a resource group, use:</span></span>

    azure role assignment list --resource-group <resource group name>

<span data-ttu-id="6a204-126">hello följande exempel visar hello rolltilldelningar i hello *pharma-försäljning-projecforcast* grupp.</span><span class="sxs-lookup"><span data-stu-id="6a204-126">hello following example shows hello role assignments in hello *pharma-sales-projecforcast* group.</span></span>

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Skärmbild av Azure RBAC kommandoraden - azure rollen tilldelningslista av grupp-](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a><span data-ttu-id="6a204-128">Lista rolltilldelningar för en användare</span><span class="sxs-lookup"><span data-stu-id="6a204-128">List role assignments for a user</span></span>
<span data-ttu-id="6a204-129">toolist hello rolltilldelningar för en specifik användare och hello tilldelningar som är tilldelade tooa användargrupper, Använd:</span><span class="sxs-lookup"><span data-stu-id="6a204-129">toolist hello role assignments for a specific user and hello assignments that are assigned tooa user's groups, use:</span></span>

    azure role assignment list --signInName <user email>

<span data-ttu-id="6a204-130">Du kan också se rolltilldelningar som ärvs från grupper genom att ändra hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="6a204-130">You can also see role assignments that are inherited from groups by modifying hello command:</span></span>

    azure role assignment list --expandPrincipalGroups --signInName <user email>

<span data-ttu-id="6a204-131">hello följande exempel visar hello rolltilldelningar som beviljas toohello  *sameert@aaddemo.com*  användare.</span><span class="sxs-lookup"><span data-stu-id="6a204-131">hello following example shows hello role assignments that are granted toohello *sameert@aaddemo.com* user.</span></span> <span data-ttu-id="6a204-132">Detta inkluderar roller som tilldelas direkt toohello användare och roller som ärvs från grupper.</span><span class="sxs-lookup"><span data-stu-id="6a204-132">This includes roles that are assigned directly toohello user and roles that are inherited from groups.</span></span>

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure RBAC kommandoraden - azure rollen tilldelningslista av användare – skärmbild](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a><span data-ttu-id="6a204-134">Bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="6a204-134">Grant access</span></span>
<span data-ttu-id="6a204-135">toogrant åtkomst när du har identifierat hello roll som du vill tooassign kan använda:</span><span class="sxs-lookup"><span data-stu-id="6a204-135">toogrant access after you have identified hello role that you want tooassign, use:</span></span>

    azure role assignment create

### <a name="assign-a-role-toogroup-at-hello-subscription-scope"></a><span data-ttu-id="6a204-136">Tilldela en roll toogroup hello prenumeration definitionsområdet</span><span class="sxs-lookup"><span data-stu-id="6a204-136">Assign a role toogroup at hello subscription scope</span></span>
<span data-ttu-id="6a204-137">tooassign en roll tooa grupp definitionsområdet hello prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="6a204-137">tooassign a role tooa group at hello subscription scope, use:</span></span>

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="6a204-138">hello följande exempel tilldelas hello *Reader* roll för*Christine Koch Team* på hello *prenumeration* omfång.</span><span class="sxs-lookup"><span data-stu-id="6a204-138">hello following example assigns hello *Reader* role too*Christine Koch's Team* at hello *subscription* scope.</span></span>

![RBAC Azure kommandoraden - azure rolltilldelning Skapa grupp – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a><span data-ttu-id="6a204-140">Tilldela en roll tooan programmet hello prenumeration definitionsområdet</span><span class="sxs-lookup"><span data-stu-id="6a204-140">Assign a role tooan application at hello subscription scope</span></span>
<span data-ttu-id="6a204-141">tooassign ett program med rollen tooan definitionsområdet hello prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="6a204-141">tooassign a role tooan application at hello subscription scope, use:</span></span>

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="6a204-142">hello följande exempel beviljas hello *deltagare* rollen tooan *Azure AD* programmet på hello valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="6a204-142">hello following example grants hello *Contributor* role tooan *Azure AD* application on hello selected subscription.</span></span>

 ![Kommandoraden för RBAC Azure - azure rolltilldelning skapa av program](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a><span data-ttu-id="6a204-144">Tilldela en användare med rollen tooa på hello resurs Gruppomfång</span><span class="sxs-lookup"><span data-stu-id="6a204-144">Assign a role tooa user at hello resource group scope</span></span>
<span data-ttu-id="6a204-145">tooassign en användare med rollen tooa på hello resurs Gruppomfång, Använd:</span><span class="sxs-lookup"><span data-stu-id="6a204-145">tooassign a role tooa user at hello resource group scope, use:</span></span>

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

<span data-ttu-id="6a204-146">hello följande exempel beviljas hello *Virtual Machine-deltagare* roll för *samert@aaddemo.com*  användaren vid hello *Pharma-försäljning-ProjectForcast* resurs Gruppomfång.</span><span class="sxs-lookup"><span data-stu-id="6a204-146">hello following example grants hello *Virtual Machine Contributor* role too*samert@aaddemo.com* user at hello *Pharma-Sales-ProjectForcast* resource group scope.</span></span>

![Kommandoraden för RBAC Azure - azure rolltilldelning skapa av användare – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a><span data-ttu-id="6a204-148">Tilldela en roll tooa grupp hello resurs definitionsområdet</span><span class="sxs-lookup"><span data-stu-id="6a204-148">Assign a role tooa group at hello resource scope</span></span>
<span data-ttu-id="6a204-149">tooassign en roll tooa grupp definitionsområdet hello resurs, Använd:</span><span class="sxs-lookup"><span data-stu-id="6a204-149">tooassign a role tooa group at hello resource scope, use:</span></span>

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

<span data-ttu-id="6a204-150">hello följande exempel beviljas hello *Virtual Machine-deltagare* rollen tooan *Azure AD* på en *undernät*.</span><span class="sxs-lookup"><span data-stu-id="6a204-150">hello following example grants hello *Virtual Machine Contributor* role tooan *Azure AD* group on a *subnet*.</span></span>

![RBAC Azure kommandoraden - azure rolltilldelning Skapa grupp – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a><span data-ttu-id="6a204-152">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="6a204-152">Remove access</span></span>
<span data-ttu-id="6a204-153">Använd tooremove en rolltilldelning:</span><span class="sxs-lookup"><span data-stu-id="6a204-153">tooremove a role assignment, use:</span></span>

    azure role assignment delete --objectId <object id toofrom which tooremove role> --roleName "<role name>"

<span data-ttu-id="6a204-154">hello följande exempel tar bort hello *Virtual Machine-deltagare* rolltilldelningen från hello  *sammert@aaddemo.com*  användare på hello *Pharma-försäljning-ProjectForcast* resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6a204-154">hello following example removes hello *Virtual Machine Contributor* role assignment from hello *sammert@aaddemo.com* user on hello *Pharma-Sales-ProjectForcast* resource group.</span></span>
<span data-ttu-id="6a204-155">tar bort hello exempel hello rolltilldelning från en grupp i hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6a204-155">hello example then removes hello role assignment from a group on hello subscription.</span></span>

![Azure RBAC kommandoraden - azure tilldelning Rollborttagning – skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="6a204-157">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="6a204-157">Create a custom role</span></span>
<span data-ttu-id="6a204-158">Använd toocreate en anpassad roll:</span><span class="sxs-lookup"><span data-stu-id="6a204-158">toocreate a custom role, use:</span></span>

    azure role create --inputfile <file path>

<span data-ttu-id="6a204-159">hello följande exempel skapar en anpassad roll som kallas *virtuella operatorn*.</span><span class="sxs-lookup"><span data-stu-id="6a204-159">hello following example creates a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="6a204-160">Den här anpassade rollen ger åtkomst tooall läsåtgärder av *Microsoft.Compute*, *Microsoft.Storage*, och *Microsoft.Network* resurs providers och beviljar åtkomst toostart, starta om och övervaka virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6a204-160">This custom role grants access tooall read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access toostart, restart, and monitor virtual machines.</span></span> <span data-ttu-id="6a204-161">Den här anpassade rollen kan användas i två prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6a204-161">This custom role can be used in two subscriptions.</span></span> <span data-ttu-id="6a204-162">Det här exemplet används en JSON-fil som indata.</span><span class="sxs-lookup"><span data-stu-id="6a204-162">This example uses a JSON file as an input.</span></span>

![JSON - anpassad rolldefinition – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure kommandoraden - azure rollen skapa – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a><span data-ttu-id="6a204-165">Ändra en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="6a204-165">Modify a custom role</span></span>
<span data-ttu-id="6a204-166">toomodify en anpassad roll först använda hello `azure role show` kommandot tooretrieve rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="6a204-166">toomodify a custom role, first use hello `azure role show` command tooretrieve role definition.</span></span> <span data-ttu-id="6a204-167">Därefter kontrollera definitionsfilen för hello ändringarna toohello roll.</span><span class="sxs-lookup"><span data-stu-id="6a204-167">Second, make hello desired changes toohello role definition file.</span></span> <span data-ttu-id="6a204-168">Använd slutligen `azure role set` toosave hello ändrade rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="6a204-168">Finally, use `azure role set` toosave hello modified role definition.</span></span>

    azure role set --inputfile <file path>

<span data-ttu-id="6a204-169">hello följande exempel läggs hello *Microsoft.Insights/diagnosticSettings/* åtgärden toohello **åtgärder**, och en Azure-prenumeration toohello **AssignableScopes**av hello virtuella anpassade operatörsrollen.</span><span class="sxs-lookup"><span data-stu-id="6a204-169">hello following example adds hello *Microsoft.Insights/diagnosticSettings/* operation toohello **Actions**, and an Azure subscription toohello **AssignableScopes** of hello Virtual Machine Operator custom role.</span></span>

![JSON - ändra anpassad rolldefinition – skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Azure RBAC kommandoraden - azure rollen set - skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a><span data-ttu-id="6a204-172">Ta bort en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="6a204-172">Delete a custom role</span></span>
<span data-ttu-id="6a204-173">toodelete en anpassad roll först använda hello `azure role show` kommandot toodetermine hello **ID** hello-rollen.</span><span class="sxs-lookup"><span data-stu-id="6a204-173">toodelete a custom role, first use hello `azure role show` command toodetermine hello **ID** of hello role.</span></span> <span data-ttu-id="6a204-174">Använd sedan hello `azure role delete` kommandot toodelete hello roll genom att ange hello **ID**.</span><span class="sxs-lookup"><span data-stu-id="6a204-174">Then, use hello `azure role delete` command toodelete hello role by specifying hello **ID**.</span></span>

<span data-ttu-id="6a204-175">hello följande exempel tar bort hello *virtuella operatorn* anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="6a204-175">hello following example removes hello *Virtual Machine Operator* custom role.</span></span>

![Azure RBAC kommandoraden - azure Rollborttagning – skärmbild](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a><span data-ttu-id="6a204-177">Lista över anpassade roller</span><span class="sxs-lookup"><span data-stu-id="6a204-177">List custom roles</span></span>
<span data-ttu-id="6a204-178">toolist hello roller som är tillgängliga för tilldelning på scopenivå, använder hello `azure role list` kommando.</span><span class="sxs-lookup"><span data-stu-id="6a204-178">toolist hello roles that are available for assignment at a scope, use hello `azure role list` command.</span></span>

<span data-ttu-id="6a204-179">hello följande kommando visar alla roller som är tillgängliga för tilldelning i hello valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="6a204-179">hello following command lists all roles that are available for assignment in hello selected subscription.</span></span>

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Azure RBAC kommandoraden - azure rollen list - skärmbild](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

<span data-ttu-id="6a204-181">I följande exempel hello, hello *virtuella operatorn* anpassad roll är inte tillgänglig i hello *Production4* prenumerationen eftersom den prenumerationen finns inte i hello  **AssignableScopes** hello-rollen.</span><span class="sxs-lookup"><span data-stu-id="6a204-181">In hello following example, hello *Virtual Machine Operator* custom role isn’t available in hello *Production4* subscription because that subscription isn’t in hello **AssignableScopes** of hello role.</span></span>

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Azure RBAC kommandoraden - azure rollen listan för anpassade roller – skärmbild](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a><span data-ttu-id="6a204-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a204-183">Next steps</span></span>
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

