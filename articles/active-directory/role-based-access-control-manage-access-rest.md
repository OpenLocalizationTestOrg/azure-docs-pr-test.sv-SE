---
title: "aaaRole-baserad åtkomstkontroll med REST - Azure AD | Microsoft Docs"
description: "Hantera rollbaserad åtkomstkontroll med hello REST API"
services: active-directory
documentationcenter: na
author: andredm7
manager: femila
editor: 
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: active-directory
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: andredm
ms.openlocfilehash: ccd402fd4fe4583288076cac23753dd067694681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-hello-rest-api"></a><span data-ttu-id="535ba-103">Hantera rollbaserad åtkomstkontroll med hello REST API</span><span class="sxs-lookup"><span data-stu-id="535ba-103">Manage Role-Based Access Control with hello REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="535ba-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="535ba-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="535ba-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="535ba-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="535ba-106">REST-API</span><span class="sxs-lookup"><span data-stu-id="535ba-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="535ba-107">Rollbaserad åtkomstkontroll (RBAC) i hello Azure-portalen och Azure Resource Manager API kan du hantera åtkomst tooyour prenumeration och resurser på en detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="535ba-107">Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Manager API helps you manage access tooyour subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="535ba-108">Med den här funktionen kan du bevilja åtkomst för användare, grupper eller tjänstens huvudnamn i Active Directory genom att tilldela vissa roller toothem för ett visst område.</span><span class="sxs-lookup"><span data-stu-id="535ba-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

## <a name="list-all-role-assignments"></a><span data-ttu-id="535ba-109">Visa en lista med alla rolltilldelningar</span><span class="sxs-lookup"><span data-stu-id="535ba-109">List all role assignments</span></span>
<span data-ttu-id="535ba-110">Visar alla hello rolltilldelningar på hello angetts omfång och subscopes.</span><span class="sxs-lookup"><span data-stu-id="535ba-110">Lists all hello role assignments at hello specified scope and subscopes.</span></span>

<span data-ttu-id="535ba-111">toolist rolltilldelningar, måste du ha tillgång för`Microsoft.Authorization/roleAssignments/read` åtgärden hello definitionsområdet.</span><span class="sxs-lookup"><span data-stu-id="535ba-111">toolist role assignments, you must have access too`Microsoft.Authorization/roleAssignments/read` operation at hello scope.</span></span> <span data-ttu-id="535ba-112">Alla hello inbyggda roller beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-112">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="535ba-113">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-113">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-114">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-114">Request</span></span>
<span data-ttu-id="535ba-115">Använd hello **hämta** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-115">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

<span data-ttu-id="535ba-116">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-116">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-117">Ersätt *{scope}* med hello scope som du vill toolist hello rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="535ba-117">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="535ba-118">hello som följande exempel visar hur toospecify hello omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="535ba-118">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="535ba-119">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-119">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-120">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-120">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="535ba-121">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-121">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-122">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-122">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="535ba-123">Ersätt *{filter}* med hello villkor som du vill tilldelningslista för tooapply toofilter hello roll:</span><span class="sxs-lookup"><span data-stu-id="535ba-123">Replace *{filter}* with hello condition that you wish tooapply toofilter hello role assignment list:</span></span>

   * <span data-ttu-id="535ba-124">Lista rolltilldelningar för endast hello angiven omfång, exklusive hello rolltilldelningar på subscopes:`atScope()`</span><span class="sxs-lookup"><span data-stu-id="535ba-124">List role assignments for only hello specified scope, not including hello role assignments at subscopes: `atScope()`</span></span>    
   * <span data-ttu-id="535ba-125">Lista rolltilldelningar för en viss användare, grupp eller ett program:`principalId%20eq%20'{objectId of user, group, or service principal}'`</span><span class="sxs-lookup"><span data-stu-id="535ba-125">List role assignments for a specific user, group, or application: `principalId%20eq%20'{objectId of user, group, or service principal}'`</span></span>  
   * <span data-ttu-id="535ba-126">Visa en lista med rolltilldelningar för en viss användare, inklusive de som ärvts från grupper |`assignedTo('{objectId of user}')`</span><span class="sxs-lookup"><span data-stu-id="535ba-126">List role assignments for a specific user, including ones inherited from groups | `assignedTo('{objectId of user}')`</span></span>

### <a name="response"></a><span data-ttu-id="535ba-127">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-127">Response</span></span>
<span data-ttu-id="535ba-128">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="535ba-128">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a><span data-ttu-id="535ba-129">Hämta information om en rolltilldelning</span><span class="sxs-lookup"><span data-stu-id="535ba-129">Get information about a role assignment</span></span>
<span data-ttu-id="535ba-130">Hämtar information om en enda rolltilldelning som anges av hello rollen tilldelning identifierare.</span><span class="sxs-lookup"><span data-stu-id="535ba-130">Gets information about a single role assignment specified by hello role assignment identifier.</span></span>

<span data-ttu-id="535ba-131">tooget information om en rolltilldelning, måste du ha tillgång för`Microsoft.Authorization/roleAssignments/read` igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-131">tooget information about a role assignment, you must have access too`Microsoft.Authorization/roleAssignments/read` operation.</span></span> <span data-ttu-id="535ba-132">Alla hello inbyggda roller beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-132">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="535ba-133">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-133">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-134">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-134">Request</span></span>
<span data-ttu-id="535ba-135">Använd hello **hämta** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-135">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="535ba-136">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-136">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-137">Ersätt *{scope}* med hello scope som du vill toolist hello rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="535ba-137">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="535ba-138">hello som följande exempel visar hur toospecify hello omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="535ba-138">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="535ba-139">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-139">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-140">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-140">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="535ba-141">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-141">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-142">Ersätt *{roll-tilldelning-id}* med hello GUID-identifierare för hello rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="535ba-142">Replace *{role-assignment-id}* with hello GUID identifier of hello role assignment.</span></span>
3. <span data-ttu-id="535ba-143">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-143">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="535ba-144">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-144">Response</span></span>
<span data-ttu-id="535ba-145">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="535ba-145">Status code: 200</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a><span data-ttu-id="535ba-146">Skapa en rolltilldelning</span><span class="sxs-lookup"><span data-stu-id="535ba-146">Create a Role Assignment</span></span>
<span data-ttu-id="535ba-147">Skapa en roll tilldelning vid hello specificerat omfånget för hello ange huvudnamn beviljande hello angivna rollen.</span><span class="sxs-lookup"><span data-stu-id="535ba-147">Create a role assignment at hello specified scope for hello specified principal granting hello specified role.</span></span>

<span data-ttu-id="535ba-148">toocreate en rolltilldelning måste du ha tillgång för`Microsoft.Authorization/roleAssignments/write` igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-148">toocreate a role assignment, you must have access too`Microsoft.Authorization/roleAssignments/write` operation.</span></span> <span data-ttu-id="535ba-149">I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-149">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="535ba-150">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-150">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-151">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-151">Request</span></span>
<span data-ttu-id="535ba-152">Använd hello **PLACERA** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-152">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="535ba-153">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-153">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-154">Ersätt *{scope}* med hello scope som du vill toocreate hello rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="535ba-154">Replace *{scope}* with hello scope at which you wish toocreate hello role assignments.</span></span> <span data-ttu-id="535ba-155">När du skapar en rolltilldelning på en överordnad omfattning, ärver alla underordnade omfång hello samma rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="535ba-155">When you create a role assignment at a parent scope, all child scopes inherit hello same role assignment.</span></span> <span data-ttu-id="535ba-156">hello som följande exempel visar hur toospecify hello omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="535ba-156">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="535ba-157">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-157">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-158">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-158">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>   
   * <span data-ttu-id="535ba-159">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-159">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-160">Ersätt *{roll-tilldelning-id}* med en ny GUID som blir hello GUID-identifierare för hello ny rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="535ba-160">Replace *{role-assignment-id}* with a new GUID, which becomes hello GUID identifier of hello new role assignment.</span></span>
3. <span data-ttu-id="535ba-161">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-161">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="535ba-162">Ange hello värden i hello följande format för hello begärantext:</span><span class="sxs-lookup"><span data-stu-id="535ba-162">For hello request body, provide hello values in hello following format:</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| <span data-ttu-id="535ba-163">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="535ba-163">Element Name</span></span> | <span data-ttu-id="535ba-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="535ba-164">Required</span></span> | <span data-ttu-id="535ba-165">Typ</span><span class="sxs-lookup"><span data-stu-id="535ba-165">Type</span></span> | <span data-ttu-id="535ba-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="535ba-166">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="535ba-167">roleDefinitionId</span><span class="sxs-lookup"><span data-stu-id="535ba-167">roleDefinitionId</span></span> |<span data-ttu-id="535ba-168">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-168">Yes</span></span> |<span data-ttu-id="535ba-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-169">String</span></span> |<span data-ttu-id="535ba-170">hello identifierare för hello roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-170">hello identifier of hello role.</span></span> <span data-ttu-id="535ba-171">hello identifierare hello format är:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span><span class="sxs-lookup"><span data-stu-id="535ba-171">hello format of hello identifier is: `{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span></span> |
| <span data-ttu-id="535ba-172">principalId</span><span class="sxs-lookup"><span data-stu-id="535ba-172">principalId</span></span> |<span data-ttu-id="535ba-173">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-173">Yes</span></span> |<span data-ttu-id="535ba-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-174">String</span></span> |<span data-ttu-id="535ba-175">objectId av hello Azure AD-säkerhetsobjekt (användare, grupp eller tjänstens huvudnamn) toowhich hello roll har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="535ba-175">objectId of hello Azure AD principal (user, group, or service principal) toowhich hello role is assigned.</span></span> |

### <a name="response"></a><span data-ttu-id="535ba-176">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-176">Response</span></span>
<span data-ttu-id="535ba-177">Statuskod: 201</span><span class="sxs-lookup"><span data-stu-id="535ba-177">Status code: 201</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a><span data-ttu-id="535ba-178">Ta bort en rolltilldelning</span><span class="sxs-lookup"><span data-stu-id="535ba-178">Delete a Role Assignment</span></span>
<span data-ttu-id="535ba-179">Ta bort en rolltilldelning i hello angetts omfång.</span><span class="sxs-lookup"><span data-stu-id="535ba-179">Delete a role assignment at hello specified scope.</span></span>

<span data-ttu-id="535ba-180">toodelete en rolltilldelning måste du ha åtkomst toohello `Microsoft.Authorization/roleAssignments/delete` igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-180">toodelete a role assignment, you must have access toohello `Microsoft.Authorization/roleAssignments/delete` operation.</span></span> <span data-ttu-id="535ba-181">I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-181">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="535ba-182">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-182">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-183">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-183">Request</span></span>
<span data-ttu-id="535ba-184">Använd hello **ta bort** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-184">Use hello **DELETE** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="535ba-185">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-185">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-186">Ersätt *{scope}* med hello scope som du vill toocreate hello rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="535ba-186">Replace *{scope}* with hello scope at which you wish toocreate hello role assignments.</span></span> <span data-ttu-id="535ba-187">hello som följande exempel visar hur toospecify hello omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="535ba-187">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="535ba-188">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-188">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-189">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-189">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="535ba-190">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-190">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-191">Ersätt *{roll-tilldelning-id}* med hello tilldelnings-id GUID.</span><span class="sxs-lookup"><span data-stu-id="535ba-191">Replace *{role-assignment-id}* with hello role assignment id GUID.</span></span>
3. <span data-ttu-id="535ba-192">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-192">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="535ba-193">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-193">Response</span></span>
<span data-ttu-id="535ba-194">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="535ba-194">Status code: 200</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a><span data-ttu-id="535ba-195">Visa en lista över alla roller</span><span class="sxs-lookup"><span data-stu-id="535ba-195">List all Roles</span></span>
<span data-ttu-id="535ba-196">Listar alla hello-roller som är tillgängliga för tilldelning i hello angivna omfattningen.</span><span class="sxs-lookup"><span data-stu-id="535ba-196">Lists all hello roles that are available for assignment at hello specified scope.</span></span>

<span data-ttu-id="535ba-197">toolist roller, måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/read` åtgärden hello definitionsområdet.</span><span class="sxs-lookup"><span data-stu-id="535ba-197">toolist roles, you must have access too`Microsoft.Authorization/roleDefinitions/read` operation at hello scope.</span></span> <span data-ttu-id="535ba-198">Alla hello inbyggda roller beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-198">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="535ba-199">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-199">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-200">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-200">Request</span></span>
<span data-ttu-id="535ba-201">Använd hello **hämta** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-201">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

<span data-ttu-id="535ba-202">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-202">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-203">Ersätt *{scope}* med hello scope som du vill toolist hello roller.</span><span class="sxs-lookup"><span data-stu-id="535ba-203">Replace *{scope}* with hello scope for which you wish toolist hello roles.</span></span> <span data-ttu-id="535ba-204">hello som följande exempel visar hur toospecify hello omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="535ba-204">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="535ba-205">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-205">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-206">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-206">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="535ba-207">Resursen /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-207">Resource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-208">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-208">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="535ba-209">Ersätt *{filter}* med hello villkor som du vill tooapply toofilter hello lista över roller:</span><span class="sxs-lookup"><span data-stu-id="535ba-209">Replace *{filter}* with hello condition that you wish tooapply toofilter hello list of roles:</span></span>

   * <span data-ttu-id="535ba-210">Lista roller som är tillgängliga för tilldelning på hello ange omfånget och alla dess underordnade omfattningar:`atScopeAndBelow()`</span><span class="sxs-lookup"><span data-stu-id="535ba-210">List roles available for assignment at hello specified scope and any of its child scopes: `atScopeAndBelow()`</span></span>
   * <span data-ttu-id="535ba-211">Sök efter en roll med exakt visningsnamn: `roleName%20eq%20'{role-display-name}'`.</span><span class="sxs-lookup"><span data-stu-id="535ba-211">Search for a role using exact display name: `roleName%20eq%20'{role-display-name}'`.</span></span> <span data-ttu-id="535ba-212">Använd hello URL-kodade hello exakt visningsnamn hello roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-212">Use hello URL encoded form of hello exact display name of hello role.</span></span> <span data-ttu-id="535ba-213">Till exempel`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span><span class="sxs-lookup"><span data-stu-id="535ba-213">For instance, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span></span>

### <a name="response"></a><span data-ttu-id="535ba-214">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-214">Response</span></span>
<span data-ttu-id="535ba-215">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="535ba-215">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access toothem, and not hello virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a><span data-ttu-id="535ba-216">Hämta information om en roll</span><span class="sxs-lookup"><span data-stu-id="535ba-216">Get information about a Role</span></span>
<span data-ttu-id="535ba-217">Hämtar information om en enda roll som anges av hello identifierare för rollen.</span><span class="sxs-lookup"><span data-stu-id="535ba-217">Gets information about a single role specified by hello role definition identifier.</span></span> <span data-ttu-id="535ba-218">tooget information om en roll med hjälp av dess visningsnamn finns [lista över alla roller för](role-based-access-control-manage-access-rest.md#list-all-roles).</span><span class="sxs-lookup"><span data-stu-id="535ba-218">tooget information about a single role using its display name, see [List all roles](role-based-access-control-manage-access-rest.md#list-all-roles).</span></span>

<span data-ttu-id="535ba-219">tooget information om en roll, måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/read` igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-219">tooget information about a role, you must have access too`Microsoft.Authorization/roleDefinitions/read` operation.</span></span> <span data-ttu-id="535ba-220">Alla hello inbyggda roller beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-220">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="535ba-221">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-221">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-222">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-222">Request</span></span>
<span data-ttu-id="535ba-223">Använd hello **hämta** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-223">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="535ba-224">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-224">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-225">Ersätt *{scope}* med hello scope som du vill toolist hello rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="535ba-225">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="535ba-226">hello som följande exempel visar hur toospecify hello omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="535ba-226">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="535ba-227">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-227">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-228">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-228">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="535ba-229">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-229">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-230">Ersätt *{roll-definition-id}* med hello GUID-identifierare för hello rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="535ba-230">Replace *{role-definition-id}* with hello GUID identifier of hello role definition.</span></span>
3. <span data-ttu-id="535ba-231">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-231">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="535ba-232">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-232">Response</span></span>
<span data-ttu-id="535ba-233">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="535ba-233">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access toothem, and not hello virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a><span data-ttu-id="535ba-234">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="535ba-234">Create a Custom Role</span></span>
<span data-ttu-id="535ba-235">Skapa en anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-235">Create a custom role.</span></span>

<span data-ttu-id="535ba-236">toocreate en anpassad roll måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/write` åtgärden på alla hello `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="535ba-236">toocreate a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/write` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="535ba-237">I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-237">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="535ba-238">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-238">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-239">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-239">Request</span></span>
<span data-ttu-id="535ba-240">Använd hello **PLACERA** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-240">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="535ba-241">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-241">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-242">Ersätt *{scope}* med hello första *AssignableScope* av hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-242">Replace *{scope}* with hello first *AssignableScope* of hello custom role.</span></span> <span data-ttu-id="535ba-243">hello som följande exempel visar hur toospecify hello omfång för olika nivåer.</span><span class="sxs-lookup"><span data-stu-id="535ba-243">hello following examples show how toospecify hello scope for different levels.</span></span>

   * <span data-ttu-id="535ba-244">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-244">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-245">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-245">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="535ba-246">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-246">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-247">Ersätt *{roll-definition-id}* med en ny GUID som blir hello GUID-identifierare för hello ny anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-247">Replace *{role-definition-id}* with a new GUID, which becomes hello GUID identifier of hello new custom role.</span></span>
3. <span data-ttu-id="535ba-248">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-248">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="535ba-249">Ange hello värden i hello följande format för hello begärantext:</span><span class="sxs-lookup"><span data-stu-id="535ba-249">For hello request body, provide hello values in hello following format:</span></span>

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| <span data-ttu-id="535ba-250">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="535ba-250">Element Name</span></span> | <span data-ttu-id="535ba-251">Krävs</span><span class="sxs-lookup"><span data-stu-id="535ba-251">Required</span></span> | <span data-ttu-id="535ba-252">Typ</span><span class="sxs-lookup"><span data-stu-id="535ba-252">Type</span></span> | <span data-ttu-id="535ba-253">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="535ba-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="535ba-254">namn</span><span class="sxs-lookup"><span data-stu-id="535ba-254">name</span></span> |<span data-ttu-id="535ba-255">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-255">Yes</span></span> |<span data-ttu-id="535ba-256">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-256">String</span></span> |<span data-ttu-id="535ba-257">GUID-identifierare för hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-257">GUID identifier of hello custom role.</span></span> |
| <span data-ttu-id="535ba-258">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="535ba-258">properties.roleName</span></span> |<span data-ttu-id="535ba-259">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-259">Yes</span></span> |<span data-ttu-id="535ba-260">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-260">String</span></span> |<span data-ttu-id="535ba-261">Visningsnamnet för hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-261">Display name of hello custom role.</span></span> <span data-ttu-id="535ba-262">Maximal storlek 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="535ba-262">Maximum size 128 characters.</span></span> |
| <span data-ttu-id="535ba-263">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="535ba-263">properties.description</span></span> |<span data-ttu-id="535ba-264">Nej</span><span class="sxs-lookup"><span data-stu-id="535ba-264">No</span></span> |<span data-ttu-id="535ba-265">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-265">String</span></span> |<span data-ttu-id="535ba-266">Beskrivning av anpassad hello-roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-266">Description of hello custom role.</span></span> <span data-ttu-id="535ba-267">Maximal storlek 1024 tecken.</span><span class="sxs-lookup"><span data-stu-id="535ba-267">Maximum size 1024 characters.</span></span> |
| <span data-ttu-id="535ba-268">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="535ba-268">properties.type</span></span> |<span data-ttu-id="535ba-269">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-269">Yes</span></span> |<span data-ttu-id="535ba-270">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-270">String</span></span> |<span data-ttu-id="535ba-271">Ställa in också ”CustomRole”.</span><span class="sxs-lookup"><span data-stu-id="535ba-271">Set too"CustomRole."</span></span> |
| <span data-ttu-id="535ba-272">Properties.permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="535ba-272">properties.permissions.actions</span></span> |<span data-ttu-id="535ba-273">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-273">Yes</span></span> |<span data-ttu-id="535ba-274">String]</span><span class="sxs-lookup"><span data-stu-id="535ba-274">String[]</span></span> |<span data-ttu-id="535ba-275">En matris med åtgärden strängar åtgärder för att ange hello beviljat hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-275">An array of action strings specifying hello operations granted by hello custom role.</span></span> |
| <span data-ttu-id="535ba-276">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="535ba-276">properties.permissions.notActions</span></span> |<span data-ttu-id="535ba-277">Nej</span><span class="sxs-lookup"><span data-stu-id="535ba-277">No</span></span> |<span data-ttu-id="535ba-278">String]</span><span class="sxs-lookup"><span data-stu-id="535ba-278">String[]</span></span> |<span data-ttu-id="535ba-279">En strängmatris åtgärd att ange hello operations tooexclude från hello-åtgärder som tilldelats av hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-279">An array of action strings specifying hello operations tooexclude from hello operations granted by hello custom role.</span></span> |
| <span data-ttu-id="535ba-280">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="535ba-280">properties.assignableScopes</span></span> |<span data-ttu-id="535ba-281">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-281">Yes</span></span> |<span data-ttu-id="535ba-282">String]</span><span class="sxs-lookup"><span data-stu-id="535ba-282">String[]</span></span> |<span data-ttu-id="535ba-283">En matris med omfattningar i vilka hello anpassad roll kan användas.</span><span class="sxs-lookup"><span data-stu-id="535ba-283">An array of scopes in which hello custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="535ba-284">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-284">Response</span></span>
<span data-ttu-id="535ba-285">Statuskod: 201</span><span class="sxs-lookup"><span data-stu-id="535ba-285">Status code: 201</span></span>

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a><span data-ttu-id="535ba-286">Uppdatera en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="535ba-286">Update a Custom Role</span></span>
<span data-ttu-id="535ba-287">Ändra en anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-287">Modify a custom role.</span></span>

<span data-ttu-id="535ba-288">toomodify en anpassad roll måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/write` åtgärden på alla hello `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="535ba-288">toomodify a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/write` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="535ba-289">I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-289">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="535ba-290">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-290">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-291">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-291">Request</span></span>
<span data-ttu-id="535ba-292">Använd hello **PLACERA** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-292">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="535ba-293">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-293">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-294">Ersätt *{scope}* med hello första *AssignableScope* av hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-294">Replace *{scope}* with hello first *AssignableScope* of hello custom role.</span></span> <span data-ttu-id="535ba-295">hello som följande exempel visar hur toospecify hello omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="535ba-295">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="535ba-296">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-296">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-297">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-297">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="535ba-298">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-298">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-299">Ersätt *{roll-definition-id}* med hello GUID-identifierare för hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-299">Replace *{role-definition-id}* with hello GUID identifier of hello custom role.</span></span>
3. <span data-ttu-id="535ba-300">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-300">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="535ba-301">Ange hello värden i hello följande format för hello begärantext:</span><span class="sxs-lookup"><span data-stu-id="535ba-301">For hello request body, provide hello values in hello following format:</span></span>

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| <span data-ttu-id="535ba-302">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="535ba-302">Element Name</span></span> | <span data-ttu-id="535ba-303">Krävs</span><span class="sxs-lookup"><span data-stu-id="535ba-303">Required</span></span> | <span data-ttu-id="535ba-304">Typ</span><span class="sxs-lookup"><span data-stu-id="535ba-304">Type</span></span> | <span data-ttu-id="535ba-305">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="535ba-305">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="535ba-306">namn</span><span class="sxs-lookup"><span data-stu-id="535ba-306">name</span></span> |<span data-ttu-id="535ba-307">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-307">Yes</span></span> |<span data-ttu-id="535ba-308">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-308">String</span></span> |<span data-ttu-id="535ba-309">GUID-identifierare för hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-309">GUID identifier of hello custom role.</span></span> |
| <span data-ttu-id="535ba-310">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="535ba-310">properties.roleName</span></span> |<span data-ttu-id="535ba-311">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-311">Yes</span></span> |<span data-ttu-id="535ba-312">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-312">String</span></span> |<span data-ttu-id="535ba-313">Visningsnamn för hello uppdaterade anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-313">Display name of hello updated custom role.</span></span> |
| <span data-ttu-id="535ba-314">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="535ba-314">properties.description</span></span> |<span data-ttu-id="535ba-315">Nej</span><span class="sxs-lookup"><span data-stu-id="535ba-315">No</span></span> |<span data-ttu-id="535ba-316">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-316">String</span></span> |<span data-ttu-id="535ba-317">Beskrivning av hello uppdatera anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-317">Description of hello updated custom role.</span></span> |
| <span data-ttu-id="535ba-318">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="535ba-318">properties.type</span></span> |<span data-ttu-id="535ba-319">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-319">Yes</span></span> |<span data-ttu-id="535ba-320">Sträng</span><span class="sxs-lookup"><span data-stu-id="535ba-320">String</span></span> |<span data-ttu-id="535ba-321">Ställa in också ”CustomRole”.</span><span class="sxs-lookup"><span data-stu-id="535ba-321">Set too"CustomRole."</span></span> |
| <span data-ttu-id="535ba-322">Properties.permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="535ba-322">properties.permissions.actions</span></span> |<span data-ttu-id="535ba-323">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-323">Yes</span></span> |<span data-ttu-id="535ba-324">String]</span><span class="sxs-lookup"><span data-stu-id="535ba-324">String[]</span></span> |<span data-ttu-id="535ba-325">En strängmatris åtgärd att ange hello operations toowhich hello uppdatera anpassade roll som beviljar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="535ba-325">An array of action strings specifying hello operations toowhich hello updated custom role grants access.</span></span> |
| <span data-ttu-id="535ba-326">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="535ba-326">properties.permissions.notActions</span></span> |<span data-ttu-id="535ba-327">Nej</span><span class="sxs-lookup"><span data-stu-id="535ba-327">No</span></span> |<span data-ttu-id="535ba-328">String]</span><span class="sxs-lookup"><span data-stu-id="535ba-328">String[]</span></span> |<span data-ttu-id="535ba-329">En matris med åtgärden strängar att ange hello operations tooexclude från hello operations vilka hello uppdateras ger anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-329">An array of action strings specifying hello operations tooexclude from hello operations which hello updated custom role grants.</span></span> |
| <span data-ttu-id="535ba-330">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="535ba-330">properties.assignableScopes</span></span> |<span data-ttu-id="535ba-331">Ja</span><span class="sxs-lookup"><span data-stu-id="535ba-331">Yes</span></span> |<span data-ttu-id="535ba-332">String]</span><span class="sxs-lookup"><span data-stu-id="535ba-332">String[]</span></span> |<span data-ttu-id="535ba-333">En matris med omfattningar i vilka hello uppdaterade anpassad roll kan användas.</span><span class="sxs-lookup"><span data-stu-id="535ba-333">An array of scopes in which hello updated custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="535ba-334">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-334">Response</span></span>
<span data-ttu-id="535ba-335">Statuskod: 201</span><span class="sxs-lookup"><span data-stu-id="535ba-335">Status code: 201</span></span>

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a><span data-ttu-id="535ba-336">Ta bort en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="535ba-336">Delete a Custom Role</span></span>
<span data-ttu-id="535ba-337">Ta bort en anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-337">Delete a custom role.</span></span>

<span data-ttu-id="535ba-338">toodelete en anpassad roll måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/delete` åtgärden på alla hello `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="535ba-338">toodelete a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/delete` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="535ba-339">I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen.</span><span class="sxs-lookup"><span data-stu-id="535ba-339">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="535ba-340">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="535ba-340">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="535ba-341">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="535ba-341">Request</span></span>
<span data-ttu-id="535ba-342">Använd hello **ta bort** metod med hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="535ba-342">Use hello **DELETE** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="535ba-343">Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:</span><span class="sxs-lookup"><span data-stu-id="535ba-343">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="535ba-344">Ersätt *{scope}* med hello scope som du vill att toodelete hello rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="535ba-344">Replace *{scope}* with hello scope at which you wish toodelete hello role definition.</span></span> <span data-ttu-id="535ba-345">hello som följande exempel visar hur toospecify hello omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="535ba-345">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="535ba-346">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="535ba-346">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="535ba-347">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="535ba-347">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="535ba-348">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="535ba-348">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="535ba-349">Ersätt *{roll-definition-id}* med hello GUID rolldefinitions-ID: hello anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="535ba-349">Replace *{role-definition-id}* with hello GUID role definition id of hello custom role.</span></span>
3. <span data-ttu-id="535ba-350">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="535ba-350">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="535ba-351">Svar</span><span class="sxs-lookup"><span data-stu-id="535ba-351">Response</span></span>
<span data-ttu-id="535ba-352">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="535ba-352">Status code: 200</span></span>

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```

## <a name="next-steps"></a><span data-ttu-id="535ba-353">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="535ba-353">Next steps</span></span>

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
