---
title: "Hantera rollbaserad åtkomstkontroll (RBAC) med Azure CLI | Microsoft Docs"
description: "Lär dig mer om att hantera rollbaserad åtkomstkontroll (RBAC) med kommandoradsgränssnittet i Azure genom att ange roller och rollen åtgärder och genom att tilldela roller till prenumeration och application-scope."
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
ms.openlocfilehash: ad644de6d23950e699d99042d27381336626caab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a><span data-ttu-id="6f93b-103">Hantera rollbaserad åtkomstkontroll med kommandoradsgränssnittet i Azure</span><span class="sxs-lookup"><span data-stu-id="6f93b-103">Manage Role-Based Access Control with the Azure command-line interface</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6f93b-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6f93b-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="6f93b-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6f93b-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="6f93b-106">REST-API</span><span class="sxs-lookup"><span data-stu-id="6f93b-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)


<span data-ttu-id="6f93b-107">Du kan använda rollbaserad åtkomstkontroll (RBAC) i Azure-portalen och Azure Resource Manager API för att hantera åtkomst till din prenumeration och resurser på en detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="6f93b-107">You can use Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Manager API to manage access to your subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="6f93b-108">Med den här funktionen kan du bevilja åtkomst för Active Directory-användare, grupper eller tjänstens huvudnamn genom att tilldela vissa roller till dem för ett visst område.</span><span class="sxs-lookup"><span data-stu-id="6f93b-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

<span data-ttu-id="6f93b-109">Innan du kan använda Azure-kommandoradsgränssnittet (CLI) för att hantera RBAC, måste du ha följande krav:</span><span class="sxs-lookup"><span data-stu-id="6f93b-109">Before you can use the Azure command-line interface (CLI) to manage RBAC, you must have the following prerequisites:</span></span>

* <span data-ttu-id="6f93b-110">Azure CLI version 0.8.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="6f93b-110">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="6f93b-111">Om du vill installera den senaste versionen och koppla den till din Azure-prenumeration, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6f93b-111">To install the latest version and associate it with your Azure subscription, see [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="6f93b-112">Azure Resource Manager i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6f93b-112">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="6f93b-113">Gå till [med hjälp av Azure CLI med Resource Manager](../xplat-cli-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6f93b-113">Go to [Using the Azure CLI with the Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

## <a name="list-roles"></a><span data-ttu-id="6f93b-114">Lista roller</span><span class="sxs-lookup"><span data-stu-id="6f93b-114">List roles</span></span>
### <a name="list-all-available-roles"></a><span data-ttu-id="6f93b-115">Visa en lista över alla tillgängliga roller</span><span class="sxs-lookup"><span data-stu-id="6f93b-115">List all available roles</span></span>
<span data-ttu-id="6f93b-116">Om du vill visa en lista över alla tillgängliga roller, använder du:</span><span class="sxs-lookup"><span data-stu-id="6f93b-116">To list all available roles, use:</span></span>

        azure role list

<span data-ttu-id="6f93b-117">I följande exempel visar en lista över *alla tillgängliga roller*.</span><span class="sxs-lookup"><span data-stu-id="6f93b-117">The following example shows the list of *all available roles*.</span></span>

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Azure RBAC kommandoraden - azure rollen list - skärmbild](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a><span data-ttu-id="6f93b-119">Lista över åtgärder för en roll</span><span class="sxs-lookup"><span data-stu-id="6f93b-119">List actions of a role</span></span>
<span data-ttu-id="6f93b-120">Om du vill visa en lista med åtgärder för en roll, använder du:</span><span class="sxs-lookup"><span data-stu-id="6f93b-120">To list the actions of a role, use:</span></span>

    azure role show "<role name>"

<span data-ttu-id="6f93b-121">I följande exempel visas åtgärder för den *deltagare* och *Virtual Machine-deltagare* roller.</span><span class="sxs-lookup"><span data-stu-id="6f93b-121">The following example shows the actions of the *Contributor* and *Virtual Machine Contributor* roles.</span></span>

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Azure RBAC kommandoraden - azure rollen show - skärmbild](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a><span data-ttu-id="6f93b-123">Listan åtkomst</span><span class="sxs-lookup"><span data-stu-id="6f93b-123">List access</span></span>
### <a name="list-role-assignments-effective-on-a-resource-group"></a><span data-ttu-id="6f93b-124">Lista rolltilldelningar gälla för en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="6f93b-124">List role assignments effective on a resource group</span></span>
<span data-ttu-id="6f93b-125">Om du vill visa rolltilldelningar som finns i en resursgrupp, använder du:</span><span class="sxs-lookup"><span data-stu-id="6f93b-125">To list the role assignments that exist in a resource group, use:</span></span>

    azure role assignment list --resource-group <resource group name>

<span data-ttu-id="6f93b-126">I följande exempel visas rolltilldelningar i den *pharma-försäljning-projecforcast* grupp.</span><span class="sxs-lookup"><span data-stu-id="6f93b-126">The following example shows the role assignments in the *pharma-sales-projecforcast* group.</span></span>

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Skärmbild av Azure RBAC kommandoraden - azure rollen tilldelningslista av grupp-](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a><span data-ttu-id="6f93b-128">Lista rolltilldelningar för en användare</span><span class="sxs-lookup"><span data-stu-id="6f93b-128">List role assignments for a user</span></span>
<span data-ttu-id="6f93b-129">Om du vill visa rolltilldelningar för en specifik användare och tilldelningar som är tilldelade till en användare, grupper, använder du:</span><span class="sxs-lookup"><span data-stu-id="6f93b-129">To list the role assignments for a specific user and the assignments that are assigned to a user's groups, use:</span></span>

    azure role assignment list --signInName <user email>

<span data-ttu-id="6f93b-130">Du kan också se rolltilldelningar som ärvs från grupper genom att ändra kommandot:</span><span class="sxs-lookup"><span data-stu-id="6f93b-130">You can also see role assignments that are inherited from groups by modifying the command:</span></span>

    azure role assignment list --expandPrincipalGroups --signInName <user email>

<span data-ttu-id="6f93b-131">I följande exempel visas rolltilldelningar som tilldelats den  *sameert@aaddemo.com*  användare.</span><span class="sxs-lookup"><span data-stu-id="6f93b-131">The following example shows the role assignments that are granted to the *sameert@aaddemo.com* user.</span></span> <span data-ttu-id="6f93b-132">Detta inkluderar roller som tilldelas direkt till användare och roller som ärvs från grupper.</span><span class="sxs-lookup"><span data-stu-id="6f93b-132">This includes roles that are assigned directly to the user and roles that are inherited from groups.</span></span>

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure RBAC kommandoraden - azure rollen tilldelningslista av användare – skärmbild](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a><span data-ttu-id="6f93b-134">Bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="6f93b-134">Grant access</span></span>
<span data-ttu-id="6f93b-135">Använd om du vill bevilja åtkomst när du har identifierat den roll som du vill tilldela:</span><span class="sxs-lookup"><span data-stu-id="6f93b-135">To grant access after you have identified the role that you want to assign, use:</span></span>

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a><span data-ttu-id="6f93b-136">Tilldela en roll till gruppen prenumerationsomfattningen</span><span class="sxs-lookup"><span data-stu-id="6f93b-136">Assign a role to group at the subscription scope</span></span>
<span data-ttu-id="6f93b-137">Om du vill tilldela en roll till en grupp i omfånget för prenumerationen, använder du:</span><span class="sxs-lookup"><span data-stu-id="6f93b-137">To assign a role to a group at the subscription scope, use:</span></span>

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="6f93b-138">I följande exempel tilldelas den *Reader* roll *Christine Koch Team* på den *prenumeration* omfång.</span><span class="sxs-lookup"><span data-stu-id="6f93b-138">The following example assigns the *Reader* role to *Christine Koch's Team* at the *subscription* scope.</span></span>

![RBAC Azure kommandoraden - azure rolltilldelning Skapa grupp – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a><span data-ttu-id="6f93b-140">Tilldela en roll till ett program i omfånget för prenumeration</span><span class="sxs-lookup"><span data-stu-id="6f93b-140">Assign a role to an application at the subscription scope</span></span>
<span data-ttu-id="6f93b-141">Om du vill tilldela en roll till ett program i omfånget för prenumerationen, använder du:</span><span class="sxs-lookup"><span data-stu-id="6f93b-141">To assign a role to an application at the subscription scope, use:</span></span>

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

<span data-ttu-id="6f93b-142">I följande exempel beviljas den *deltagare* rollen till en *Azure AD* på den valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-142">The following example grants the *Contributor* role to an *Azure AD* application on the selected subscription.</span></span>

 ![Kommandoraden för RBAC Azure - azure rolltilldelning skapa av program](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a><span data-ttu-id="6f93b-144">Tilldela en roll till en användare på Gruppomfång resurs</span><span class="sxs-lookup"><span data-stu-id="6f93b-144">Assign a role to a user at the resource group scope</span></span>
<span data-ttu-id="6f93b-145">Om du vill tilldela en roll till en användare på Gruppomfång resurs, använder du:</span><span class="sxs-lookup"><span data-stu-id="6f93b-145">To assign a role to a user at the resource group scope, use:</span></span>

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

<span data-ttu-id="6f93b-146">I följande exempel beviljas den *Virtual Machine-deltagare* roll  *samert@aaddemo.com*  användare på den *Pharma-försäljning-ProjectForcast* resurs Gruppomfång.</span><span class="sxs-lookup"><span data-stu-id="6f93b-146">The following example grants the *Virtual Machine Contributor* role to *samert@aaddemo.com* user at the *Pharma-Sales-ProjectForcast* resource group scope.</span></span>

![Kommandoraden för RBAC Azure - azure rolltilldelning skapa av användare – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a><span data-ttu-id="6f93b-148">Tilldela en roll till en grupp i omfånget för resurs</span><span class="sxs-lookup"><span data-stu-id="6f93b-148">Assign a role to a group at the resource scope</span></span>
<span data-ttu-id="6f93b-149">Om du vill tilldela en roll till en grupp definitionsområdet resurs, använder du:</span><span class="sxs-lookup"><span data-stu-id="6f93b-149">To assign a role to a group at the resource scope, use:</span></span>

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

<span data-ttu-id="6f93b-150">I följande exempel beviljas den *Virtual Machine-deltagare* rollen till en *Azure AD* på en *undernät*.</span><span class="sxs-lookup"><span data-stu-id="6f93b-150">The following example grants the *Virtual Machine Contributor* role to an *Azure AD* group on a *subnet*.</span></span>

![RBAC Azure kommandoraden - azure rolltilldelning Skapa grupp – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a><span data-ttu-id="6f93b-152">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="6f93b-152">Remove access</span></span>
<span data-ttu-id="6f93b-153">Ta bort en rolltilldelning med:</span><span class="sxs-lookup"><span data-stu-id="6f93b-153">To remove a role assignment, use:</span></span>

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

<span data-ttu-id="6f93b-154">I följande exempel tar bort den *Virtual Machine-deltagare* rolltilldelningen från den  *sammert@aaddemo.com*  användare på den *Pharma-försäljning-ProjectForcast* resurs grupp.</span><span class="sxs-lookup"><span data-stu-id="6f93b-154">The following example removes the *Virtual Machine Contributor* role assignment from the *sammert@aaddemo.com* user on the *Pharma-Sales-ProjectForcast* resource group.</span></span>
<span data-ttu-id="6f93b-155">I exemplet tar bort rolltilldelningen från en grupp i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-155">The example then removes the role assignment from a group on the subscription.</span></span>

![Azure RBAC kommandoraden - azure tilldelning Rollborttagning – skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a><span data-ttu-id="6f93b-157">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="6f93b-157">Create a custom role</span></span>
<span data-ttu-id="6f93b-158">Använd för att skapa en anpassad roll:</span><span class="sxs-lookup"><span data-stu-id="6f93b-158">To create a custom role, use:</span></span>

    azure role create --inputfile <file path>

<span data-ttu-id="6f93b-159">I följande exempel skapas en anpassad roll som kallas *virtuella operatorn*.</span><span class="sxs-lookup"><span data-stu-id="6f93b-159">The following example creates a custom role called *Virtual Machine Operator*.</span></span> <span data-ttu-id="6f93b-160">Den här anpassade rollen ger åtkomst till alla läsåtgärder av *Microsoft.Compute*, *Microsoft.Storage*, och *Microsoft.Network* resursproviders och ger åtkomst till Starta, starta om och övervaka virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6f93b-160">This custom role grants access to all read operations of *Microsoft.Compute*, *Microsoft.Storage*, and *Microsoft.Network* resource providers and grants access to start, restart, and monitor virtual machines.</span></span> <span data-ttu-id="6f93b-161">Den här anpassade rollen kan användas i två prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6f93b-161">This custom role can be used in two subscriptions.</span></span> <span data-ttu-id="6f93b-162">Det här exemplet används en JSON-fil som indata.</span><span class="sxs-lookup"><span data-stu-id="6f93b-162">This example uses a JSON file as an input.</span></span>

![JSON - anpassad rolldefinition – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure kommandoraden - azure rollen skapa – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a><span data-ttu-id="6f93b-165">Ändra en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="6f93b-165">Modify a custom role</span></span>
<span data-ttu-id="6f93b-166">Om du vill ändra en anpassad roll först använda den `azure role show` kommando för att hämta rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-166">To modify a custom role, first use the `azure role show` command to retrieve role definition.</span></span> <span data-ttu-id="6f93b-167">Andra, gör ändringarna till definitionsfilen för rollen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-167">Second, make the desired changes to the role definition file.</span></span> <span data-ttu-id="6f93b-168">Använd slutligen `azure role set` att spara ändrade rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-168">Finally, use `azure role set` to save the modified role definition.</span></span>

    azure role set --inputfile <file path>

<span data-ttu-id="6f93b-169">I följande exempel läggs den *Microsoft.Insights/diagnosticSettings/* åtgärden den **åtgärder**, och en Azure-prenumeration i **AssignableScopes** av den Virtual Machine anpassade operatörsrollen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-169">The following example adds the *Microsoft.Insights/diagnosticSettings/* operation to the **Actions**, and an Azure subscription to the **AssignableScopes** of the Virtual Machine Operator custom role.</span></span>

![JSON - ändra anpassad rolldefinition – skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Azure RBAC kommandoraden - azure rollen set - skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a><span data-ttu-id="6f93b-172">Ta bort en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="6f93b-172">Delete a custom role</span></span>
<span data-ttu-id="6f93b-173">Ta bort en anpassad roll genom att först använda den `azure role show` kommando för att fastställa den **ID** av rollen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-173">To delete a custom role, first use the `azure role show` command to determine the **ID** of the role.</span></span> <span data-ttu-id="6f93b-174">Använd sedan den `azure role delete` kommando för att ta bort rollen genom att ange den **ID**.</span><span class="sxs-lookup"><span data-stu-id="6f93b-174">Then, use the `azure role delete` command to delete the role by specifying the **ID**.</span></span>

<span data-ttu-id="6f93b-175">I följande exempel tar bort den *virtuella operatorn* anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="6f93b-175">The following example removes the *Virtual Machine Operator* custom role.</span></span>

![Azure RBAC kommandoraden - azure Rollborttagning – skärmbild](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a><span data-ttu-id="6f93b-177">Lista över anpassade roller</span><span class="sxs-lookup"><span data-stu-id="6f93b-177">List custom roles</span></span>
<span data-ttu-id="6f93b-178">Om du vill visa de roller som är tillgängliga för tilldelning på scopenivå, använder den `azure role list` kommando.</span><span class="sxs-lookup"><span data-stu-id="6f93b-178">To list the roles that are available for assignment at a scope, use the `azure role list` command.</span></span>

<span data-ttu-id="6f93b-179">Följande kommando visar alla roller som är tillgängliga för tilldelning i den valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-179">The following command lists all roles that are available for assignment in the selected subscription.</span></span>

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Azure RBAC kommandoraden - azure rollen list - skärmbild](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

<span data-ttu-id="6f93b-181">I följande exempel visas den *virtuella operatorn* anpassad roll är inte tillgänglig i den *Production4* prenumeration eftersom den prenumerationen finns inte i den **AssignableScopes** av rollen.</span><span class="sxs-lookup"><span data-stu-id="6f93b-181">In the following example, the *Virtual Machine Operator* custom role isn’t available in the *Production4* subscription because that subscription isn’t in the **AssignableScopes** of the role.</span></span>

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Azure RBAC kommandoraden - azure rollen listan för anpassade roller – skärmbild](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a><span data-ttu-id="6f93b-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6f93b-183">Next steps</span></span>
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

