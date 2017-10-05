---
title: "Rollbaserad åtkomstkontroll med REST - Azure AD | Microsoft Docs"
description: "Hantera rollbaserad åtkomstkontroll med REST API"
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
ms.openlocfilehash: a5c19fd87ce1ae3e199bf1dfc8cf82f5653baac2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-the-rest-api"></a><span data-ttu-id="a163d-103">Hantera rollbaserad åtkomstkontroll med REST API</span><span class="sxs-lookup"><span data-stu-id="a163d-103">Manage Role-Based Access Control with the REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a163d-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a163d-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="a163d-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a163d-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="a163d-106">REST-API</span><span class="sxs-lookup"><span data-stu-id="a163d-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="a163d-107">Rollbaserad åtkomstkontroll (RBAC) i Azure-portalen och Azure Resource Manager API hjälper dig att hantera åtkomst till din prenumeration och resurser på en detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="a163d-107">Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Manager API helps you manage access to your subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="a163d-108">Med den här funktionen kan du bevilja åtkomst för Active Directory-användare, grupper eller tjänstens huvudnamn genom att tilldela vissa roller till dem för ett visst område.</span><span class="sxs-lookup"><span data-stu-id="a163d-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

## <a name="list-all-role-assignments"></a><span data-ttu-id="a163d-109">Visa en lista med alla rolltilldelningar</span><span class="sxs-lookup"><span data-stu-id="a163d-109">List all role assignments</span></span>
<span data-ttu-id="a163d-110">Listar alla rolltilldelningar i definitionsområdet och subscopes.</span><span class="sxs-lookup"><span data-stu-id="a163d-110">Lists all the role assignments at the specified scope and subscopes.</span></span>

<span data-ttu-id="a163d-111">Att lista rolltilldelningar, måste du ha tillgång till `Microsoft.Authorization/roleAssignments/read` åtgärden definitionsområdet.</span><span class="sxs-lookup"><span data-stu-id="a163d-111">To list role assignments, you must have access to `Microsoft.Authorization/roleAssignments/read` operation at the scope.</span></span> <span data-ttu-id="a163d-112">Inbyggda roller beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-112">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="a163d-113">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-113">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-114">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-114">Request</span></span>
<span data-ttu-id="a163d-115">Använd den **hämta** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-115">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

<span data-ttu-id="a163d-116">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-116">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-117">Ersätt *{scope}* med den omfattning som du vill visa en lista över rolltilldelningarna.</span><span class="sxs-lookup"><span data-stu-id="a163d-117">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="a163d-118">Följande exempel visar hur du kan ange omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="a163d-118">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="a163d-119">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-119">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-120">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-120">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="a163d-121">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-121">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-122">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-122">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="a163d-123">Ersätt *{filter}* med villkor som du vill tillämpa för att filtrera listan rollen tilldelning:</span><span class="sxs-lookup"><span data-stu-id="a163d-123">Replace *{filter}* with the condition that you wish to apply to filter the role assignment list:</span></span>

   * <span data-ttu-id="a163d-124">Lista rolltilldelningar för endast det angivna omfånget, exklusive rolltilldelningar på subscopes:`atScope()`</span><span class="sxs-lookup"><span data-stu-id="a163d-124">List role assignments for only the specified scope, not including the role assignments at subscopes: `atScope()`</span></span>    
   * <span data-ttu-id="a163d-125">Lista rolltilldelningar för en viss användare, grupp eller ett program:`principalId%20eq%20'{objectId of user, group, or service principal}'`</span><span class="sxs-lookup"><span data-stu-id="a163d-125">List role assignments for a specific user, group, or application: `principalId%20eq%20'{objectId of user, group, or service principal}'`</span></span>  
   * <span data-ttu-id="a163d-126">Visa en lista med rolltilldelningar för en viss användare, inklusive de som ärvts från grupper |`assignedTo('{objectId of user}')`</span><span class="sxs-lookup"><span data-stu-id="a163d-126">List role assignments for a specific user, including ones inherited from groups | `assignedTo('{objectId of user}')`</span></span>

### <a name="response"></a><span data-ttu-id="a163d-127">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-127">Response</span></span>
<span data-ttu-id="a163d-128">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="a163d-128">Status code: 200</span></span>

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

## <a name="get-information-about-a-role-assignment"></a><span data-ttu-id="a163d-129">Hämta information om en rolltilldelning</span><span class="sxs-lookup"><span data-stu-id="a163d-129">Get information about a role assignment</span></span>
<span data-ttu-id="a163d-130">Hämtar information om en enda rolltilldelning som anges av rollen tilldelning identifierare.</span><span class="sxs-lookup"><span data-stu-id="a163d-130">Gets information about a single role assignment specified by the role assignment identifier.</span></span>

<span data-ttu-id="a163d-131">Om du vill få information om en rolltilldelning måste du ha tillgång till `Microsoft.Authorization/roleAssignments/read` igen.</span><span class="sxs-lookup"><span data-stu-id="a163d-131">To get information about a role assignment, you must have access to `Microsoft.Authorization/roleAssignments/read` operation.</span></span> <span data-ttu-id="a163d-132">Inbyggda roller beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-132">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="a163d-133">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-133">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-134">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-134">Request</span></span>
<span data-ttu-id="a163d-135">Använd den **hämta** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-135">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="a163d-136">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-136">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-137">Ersätt *{scope}* med den omfattning som du vill visa en lista över rolltilldelningarna.</span><span class="sxs-lookup"><span data-stu-id="a163d-137">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="a163d-138">Följande exempel visar hur du kan ange omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="a163d-138">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="a163d-139">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-139">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-140">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-140">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="a163d-141">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-141">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-142">Ersätt *{roll-tilldelning-id}* med GUID-identifierare för rolltilldelningen.</span><span class="sxs-lookup"><span data-stu-id="a163d-142">Replace *{role-assignment-id}* with the GUID identifier of the role assignment.</span></span>
3. <span data-ttu-id="a163d-143">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-143">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="a163d-144">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-144">Response</span></span>
<span data-ttu-id="a163d-145">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="a163d-145">Status code: 200</span></span>

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

## <a name="create-a-role-assignment"></a><span data-ttu-id="a163d-146">Skapa en rolltilldelning</span><span class="sxs-lookup"><span data-stu-id="a163d-146">Create a Role Assignment</span></span>
<span data-ttu-id="a163d-147">Skapa en rolltilldelning i det specificerade omfånget för det angivna huvudnamnet bevilja den angivna rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-147">Create a role assignment at the specified scope for the specified principal granting the specified role.</span></span>

<span data-ttu-id="a163d-148">Om du vill skapa en rolltilldelning måste du ha tillgång till `Microsoft.Authorization/roleAssignments/write` igen.</span><span class="sxs-lookup"><span data-stu-id="a163d-148">To create a role assignment, you must have access to `Microsoft.Authorization/roleAssignments/write` operation.</span></span> <span data-ttu-id="a163d-149">I de inbyggda rollerna, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-149">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="a163d-150">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-150">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-151">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-151">Request</span></span>
<span data-ttu-id="a163d-152">Använd den **PLACERA** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-152">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="a163d-153">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-153">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-154">Ersätt *{scope}* med den omfattning som du vill skapa rolltilldelningen.</span><span class="sxs-lookup"><span data-stu-id="a163d-154">Replace *{scope}* with the scope at which you wish to create the role assignments.</span></span> <span data-ttu-id="a163d-155">När du skapar en rolltilldelning på en överordnad omfattning, ärver alla underordnade omfång samma rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="a163d-155">When you create a role assignment at a parent scope, all child scopes inherit the same role assignment.</span></span> <span data-ttu-id="a163d-156">Följande exempel visar hur du kan ange omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="a163d-156">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="a163d-157">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-157">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-158">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-158">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>   
   * <span data-ttu-id="a163d-159">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-159">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-160">Ersätt *{roll-tilldelning-id}* med en ny GUID som blir GUID-identifierare för ny rolltilldelning.</span><span class="sxs-lookup"><span data-stu-id="a163d-160">Replace *{role-assignment-id}* with a new GUID, which becomes the GUID identifier of the new role assignment.</span></span>
3. <span data-ttu-id="a163d-161">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-161">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="a163d-162">För begärantext, anger du värden i följande format:</span><span class="sxs-lookup"><span data-stu-id="a163d-162">For the request body, provide the values in the following format:</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| <span data-ttu-id="a163d-163">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="a163d-163">Element Name</span></span> | <span data-ttu-id="a163d-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="a163d-164">Required</span></span> | <span data-ttu-id="a163d-165">Typ</span><span class="sxs-lookup"><span data-stu-id="a163d-165">Type</span></span> | <span data-ttu-id="a163d-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a163d-166">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a163d-167">roleDefinitionId</span><span class="sxs-lookup"><span data-stu-id="a163d-167">roleDefinitionId</span></span> |<span data-ttu-id="a163d-168">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-168">Yes</span></span> |<span data-ttu-id="a163d-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-169">String</span></span> |<span data-ttu-id="a163d-170">Identifierare för rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-170">The identifier of the role.</span></span> <span data-ttu-id="a163d-171">Formatet på identifieraren är:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span><span class="sxs-lookup"><span data-stu-id="a163d-171">The format of the identifier is: `{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span></span> |
| <span data-ttu-id="a163d-172">principalId</span><span class="sxs-lookup"><span data-stu-id="a163d-172">principalId</span></span> |<span data-ttu-id="a163d-173">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-173">Yes</span></span> |<span data-ttu-id="a163d-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-174">String</span></span> |<span data-ttu-id="a163d-175">objectId av Azure AD är säkerhetsobjekt (användare, grupp eller tjänstens huvudnamn) som har tilldelats rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-175">objectId of the Azure AD principal (user, group, or service principal) to which the role is assigned.</span></span> |

### <a name="response"></a><span data-ttu-id="a163d-176">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-176">Response</span></span>
<span data-ttu-id="a163d-177">Statuskod: 201</span><span class="sxs-lookup"><span data-stu-id="a163d-177">Status code: 201</span></span>

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

## <a name="delete-a-role-assignment"></a><span data-ttu-id="a163d-178">Ta bort en rolltilldelning</span><span class="sxs-lookup"><span data-stu-id="a163d-178">Delete a Role Assignment</span></span>
<span data-ttu-id="a163d-179">Ta bort en rolltilldelning i det specificerade omfånget.</span><span class="sxs-lookup"><span data-stu-id="a163d-179">Delete a role assignment at the specified scope.</span></span>

<span data-ttu-id="a163d-180">Om du vill ta bort en rolltilldelning måste du ha åtkomst till den `Microsoft.Authorization/roleAssignments/delete` igen.</span><span class="sxs-lookup"><span data-stu-id="a163d-180">To delete a role assignment, you must have access to the `Microsoft.Authorization/roleAssignments/delete` operation.</span></span> <span data-ttu-id="a163d-181">I de inbyggda rollerna, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-181">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="a163d-182">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-182">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-183">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-183">Request</span></span>
<span data-ttu-id="a163d-184">Använd den **ta bort** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-184">Use the **DELETE** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="a163d-185">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-185">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-186">Ersätt *{scope}* med den omfattning som du vill skapa rolltilldelningen.</span><span class="sxs-lookup"><span data-stu-id="a163d-186">Replace *{scope}* with the scope at which you wish to create the role assignments.</span></span> <span data-ttu-id="a163d-187">Följande exempel visar hur du kan ange omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="a163d-187">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="a163d-188">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-188">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-189">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-189">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="a163d-190">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-190">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-191">Ersätt *{roll-tilldelning-id}* med rollen tilldelnings-id GUID.</span><span class="sxs-lookup"><span data-stu-id="a163d-191">Replace *{role-assignment-id}* with the role assignment id GUID.</span></span>
3. <span data-ttu-id="a163d-192">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-192">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="a163d-193">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-193">Response</span></span>
<span data-ttu-id="a163d-194">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="a163d-194">Status code: 200</span></span>

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

## <a name="list-all-roles"></a><span data-ttu-id="a163d-195">Visa en lista över alla roller</span><span class="sxs-lookup"><span data-stu-id="a163d-195">List all Roles</span></span>
<span data-ttu-id="a163d-196">Listar de roller som är tillgängliga för tilldelning i det specificerade omfånget.</span><span class="sxs-lookup"><span data-stu-id="a163d-196">Lists all the roles that are available for assignment at the specified scope.</span></span>

<span data-ttu-id="a163d-197">Lista roller måste du ha tillgång till `Microsoft.Authorization/roleDefinitions/read` åtgärden definitionsområdet.</span><span class="sxs-lookup"><span data-stu-id="a163d-197">To list roles, you must have access to `Microsoft.Authorization/roleDefinitions/read` operation at the scope.</span></span> <span data-ttu-id="a163d-198">Inbyggda roller beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-198">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="a163d-199">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-199">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-200">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-200">Request</span></span>
<span data-ttu-id="a163d-201">Använd den **hämta** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-201">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

<span data-ttu-id="a163d-202">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-202">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-203">Ersätt *{scope}* med den omfattning som du vill visa en lista över roller.</span><span class="sxs-lookup"><span data-stu-id="a163d-203">Replace *{scope}* with the scope for which you wish to list the roles.</span></span> <span data-ttu-id="a163d-204">Följande exempel visar hur du kan ange omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="a163d-204">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="a163d-205">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-205">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-206">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-206">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="a163d-207">Resursen /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-207">Resource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-208">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-208">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="a163d-209">Ersätt *{filter}* med villkor som du vill använda för att filtrera listan över roller:</span><span class="sxs-lookup"><span data-stu-id="a163d-209">Replace *{filter}* with the condition that you wish to apply to filter the list of roles:</span></span>

   * <span data-ttu-id="a163d-210">Lista roller som är tillgängliga för tilldelning i det specificerade omfånget och alla dess underordnade omfattningar:`atScopeAndBelow()`</span><span class="sxs-lookup"><span data-stu-id="a163d-210">List roles available for assignment at the specified scope and any of its child scopes: `atScopeAndBelow()`</span></span>
   * <span data-ttu-id="a163d-211">Sök efter en roll med exakt visningsnamn: `roleName%20eq%20'{role-display-name}'`.</span><span class="sxs-lookup"><span data-stu-id="a163d-211">Search for a role using exact display name: `roleName%20eq%20'{role-display-name}'`.</span></span> <span data-ttu-id="a163d-212">Använd URL-kodade formatet exakt visningsnamnet för rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-212">Use the URL encoded form of the exact display name of the role.</span></span> <span data-ttu-id="a163d-213">Till exempel`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span><span class="sxs-lookup"><span data-stu-id="a163d-213">For instance, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span></span>

### <a name="response"></a><span data-ttu-id="a163d-214">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-214">Response</span></span>
<span data-ttu-id="a163d-215">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="a163d-215">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
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

## <a name="get-information-about-a-role"></a><span data-ttu-id="a163d-216">Hämta information om en roll</span><span class="sxs-lookup"><span data-stu-id="a163d-216">Get information about a Role</span></span>
<span data-ttu-id="a163d-217">Hämtar information om en enda roll som anges av rollen definition identifierare.</span><span class="sxs-lookup"><span data-stu-id="a163d-217">Gets information about a single role specified by the role definition identifier.</span></span> <span data-ttu-id="a163d-218">För information om en roll med hjälp av dess namn, se [lista över alla roller för](role-based-access-control-manage-access-rest.md#list-all-roles).</span><span class="sxs-lookup"><span data-stu-id="a163d-218">To get information about a single role using its display name, see [List all roles](role-based-access-control-manage-access-rest.md#list-all-roles).</span></span>

<span data-ttu-id="a163d-219">Om du vill få information om en roll måste du ha tillgång till `Microsoft.Authorization/roleDefinitions/read` igen.</span><span class="sxs-lookup"><span data-stu-id="a163d-219">To get information about a role, you must have access to `Microsoft.Authorization/roleDefinitions/read` operation.</span></span> <span data-ttu-id="a163d-220">Inbyggda roller beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-220">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="a163d-221">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-221">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-222">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-222">Request</span></span>
<span data-ttu-id="a163d-223">Använd den **hämta** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-223">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="a163d-224">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-224">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-225">Ersätt *{scope}* med den omfattning som du vill visa en lista över rolltilldelningarna.</span><span class="sxs-lookup"><span data-stu-id="a163d-225">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="a163d-226">Följande exempel visar hur du kan ange omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="a163d-226">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="a163d-227">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-227">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-228">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-228">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="a163d-229">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-229">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-230">Ersätt *{roll-definition-id}* med GUID-identifierare för rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="a163d-230">Replace *{role-definition-id}* with the GUID identifier of the role definition.</span></span>
3. <span data-ttu-id="a163d-231">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-231">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="a163d-232">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-232">Response</span></span>
<span data-ttu-id="a163d-233">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="a163d-233">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
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

## <a name="create-a-custom-role"></a><span data-ttu-id="a163d-234">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="a163d-234">Create a Custom Role</span></span>
<span data-ttu-id="a163d-235">Skapa en anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="a163d-235">Create a custom role.</span></span>

<span data-ttu-id="a163d-236">Om du vill skapa en anpassad roll måste du ha tillgång till `Microsoft.Authorization/roleDefinitions/write` igen på alla de `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="a163d-236">To create a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/write` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="a163d-237">I de inbyggda rollerna, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-237">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="a163d-238">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-238">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-239">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-239">Request</span></span>
<span data-ttu-id="a163d-240">Använd den **PLACERA** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-240">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="a163d-241">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-241">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-242">Ersätt *{scope}* med först *AssignableScope* av den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-242">Replace *{scope}* with the first *AssignableScope* of the custom role.</span></span> <span data-ttu-id="a163d-243">Följande exempel visar hur du kan ange omfång för olika nivåer.</span><span class="sxs-lookup"><span data-stu-id="a163d-243">The following examples show how to specify the scope for different levels.</span></span>

   * <span data-ttu-id="a163d-244">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-244">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-245">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-245">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="a163d-246">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-246">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-247">Ersätt *{roll-definition-id}* med en ny GUID som blir GUID-identifierare för den nya anpassa rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-247">Replace *{role-definition-id}* with a new GUID, which becomes the GUID identifier of the new custom role.</span></span>
3. <span data-ttu-id="a163d-248">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-248">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="a163d-249">För begärantext, anger du värden i följande format:</span><span class="sxs-lookup"><span data-stu-id="a163d-249">For the request body, provide the values in the following format:</span></span>

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

| <span data-ttu-id="a163d-250">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="a163d-250">Element Name</span></span> | <span data-ttu-id="a163d-251">Krävs</span><span class="sxs-lookup"><span data-stu-id="a163d-251">Required</span></span> | <span data-ttu-id="a163d-252">Typ</span><span class="sxs-lookup"><span data-stu-id="a163d-252">Type</span></span> | <span data-ttu-id="a163d-253">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a163d-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a163d-254">namn</span><span class="sxs-lookup"><span data-stu-id="a163d-254">name</span></span> |<span data-ttu-id="a163d-255">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-255">Yes</span></span> |<span data-ttu-id="a163d-256">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-256">String</span></span> |<span data-ttu-id="a163d-257">GUID-identifierare för den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-257">GUID identifier of the custom role.</span></span> |
| <span data-ttu-id="a163d-258">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="a163d-258">properties.roleName</span></span> |<span data-ttu-id="a163d-259">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-259">Yes</span></span> |<span data-ttu-id="a163d-260">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-260">String</span></span> |<span data-ttu-id="a163d-261">Visningsnamn för den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-261">Display name of the custom role.</span></span> <span data-ttu-id="a163d-262">Maximal storlek 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="a163d-262">Maximum size 128 characters.</span></span> |
| <span data-ttu-id="a163d-263">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="a163d-263">properties.description</span></span> |<span data-ttu-id="a163d-264">Nej</span><span class="sxs-lookup"><span data-stu-id="a163d-264">No</span></span> |<span data-ttu-id="a163d-265">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-265">String</span></span> |<span data-ttu-id="a163d-266">Beskrivning av anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="a163d-266">Description of the custom role.</span></span> <span data-ttu-id="a163d-267">Maximal storlek 1024 tecken.</span><span class="sxs-lookup"><span data-stu-id="a163d-267">Maximum size 1024 characters.</span></span> |
| <span data-ttu-id="a163d-268">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="a163d-268">properties.type</span></span> |<span data-ttu-id="a163d-269">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-269">Yes</span></span> |<span data-ttu-id="a163d-270">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-270">String</span></span> |<span data-ttu-id="a163d-271">Ange till ”CustomRole”.</span><span class="sxs-lookup"><span data-stu-id="a163d-271">Set to "CustomRole."</span></span> |
| <span data-ttu-id="a163d-272">Properties.permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="a163d-272">properties.permissions.actions</span></span> |<span data-ttu-id="a163d-273">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-273">Yes</span></span> |<span data-ttu-id="a163d-274">String]</span><span class="sxs-lookup"><span data-stu-id="a163d-274">String[]</span></span> |<span data-ttu-id="a163d-275">En strängmatris åtgärd anger de åtgärder som tilldelats av den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-275">An array of action strings specifying the operations granted by the custom role.</span></span> |
| <span data-ttu-id="a163d-276">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="a163d-276">properties.permissions.notActions</span></span> |<span data-ttu-id="a163d-277">Nej</span><span class="sxs-lookup"><span data-stu-id="a163d-277">No</span></span> |<span data-ttu-id="a163d-278">String]</span><span class="sxs-lookup"><span data-stu-id="a163d-278">String[]</span></span> |<span data-ttu-id="a163d-279">En strängmatris åtgärd anger åtgärderna som ska undantas från de åtgärder som tilldelats av den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-279">An array of action strings specifying the operations to exclude from the operations granted by the custom role.</span></span> |
| <span data-ttu-id="a163d-280">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="a163d-280">properties.assignableScopes</span></span> |<span data-ttu-id="a163d-281">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-281">Yes</span></span> |<span data-ttu-id="a163d-282">String]</span><span class="sxs-lookup"><span data-stu-id="a163d-282">String[]</span></span> |<span data-ttu-id="a163d-283">En matris med scope där den anpassade rollen som kan användas.</span><span class="sxs-lookup"><span data-stu-id="a163d-283">An array of scopes in which the custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="a163d-284">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-284">Response</span></span>
<span data-ttu-id="a163d-285">Statuskod: 201</span><span class="sxs-lookup"><span data-stu-id="a163d-285">Status code: 201</span></span>

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

## <a name="update-a-custom-role"></a><span data-ttu-id="a163d-286">Uppdatera en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="a163d-286">Update a Custom Role</span></span>
<span data-ttu-id="a163d-287">Ändra en anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="a163d-287">Modify a custom role.</span></span>

<span data-ttu-id="a163d-288">Om du vill ändra en anpassad roll måste du ha tillgång till `Microsoft.Authorization/roleDefinitions/write` igen på alla de `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="a163d-288">To modify a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/write` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="a163d-289">I de inbyggda rollerna, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-289">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="a163d-290">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-290">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-291">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-291">Request</span></span>
<span data-ttu-id="a163d-292">Använd den **PLACERA** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-292">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="a163d-293">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-293">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-294">Ersätt *{scope}* med först *AssignableScope* av den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-294">Replace *{scope}* with the first *AssignableScope* of the custom role.</span></span> <span data-ttu-id="a163d-295">Följande exempel visar hur du kan ange omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="a163d-295">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="a163d-296">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-296">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-297">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-297">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="a163d-298">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-298">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-299">Ersätt *{roll-definition-id}* med GUID-identifierare för den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-299">Replace *{role-definition-id}* with the GUID identifier of the custom role.</span></span>
3. <span data-ttu-id="a163d-300">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-300">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="a163d-301">För begärantext, anger du värden i följande format:</span><span class="sxs-lookup"><span data-stu-id="a163d-301">For the request body, provide the values in the following format:</span></span>

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

| <span data-ttu-id="a163d-302">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="a163d-302">Element Name</span></span> | <span data-ttu-id="a163d-303">Krävs</span><span class="sxs-lookup"><span data-stu-id="a163d-303">Required</span></span> | <span data-ttu-id="a163d-304">Typ</span><span class="sxs-lookup"><span data-stu-id="a163d-304">Type</span></span> | <span data-ttu-id="a163d-305">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a163d-305">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a163d-306">namn</span><span class="sxs-lookup"><span data-stu-id="a163d-306">name</span></span> |<span data-ttu-id="a163d-307">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-307">Yes</span></span> |<span data-ttu-id="a163d-308">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-308">String</span></span> |<span data-ttu-id="a163d-309">GUID-identifierare för den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-309">GUID identifier of the custom role.</span></span> |
| <span data-ttu-id="a163d-310">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="a163d-310">properties.roleName</span></span> |<span data-ttu-id="a163d-311">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-311">Yes</span></span> |<span data-ttu-id="a163d-312">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-312">String</span></span> |<span data-ttu-id="a163d-313">Visningsnamn för den anpassade rollen som är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="a163d-313">Display name of the updated custom role.</span></span> |
| <span data-ttu-id="a163d-314">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="a163d-314">properties.description</span></span> |<span data-ttu-id="a163d-315">Nej</span><span class="sxs-lookup"><span data-stu-id="a163d-315">No</span></span> |<span data-ttu-id="a163d-316">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-316">String</span></span> |<span data-ttu-id="a163d-317">Beskrivning av den anpassade rollen som är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="a163d-317">Description of the updated custom role.</span></span> |
| <span data-ttu-id="a163d-318">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="a163d-318">properties.type</span></span> |<span data-ttu-id="a163d-319">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-319">Yes</span></span> |<span data-ttu-id="a163d-320">Sträng</span><span class="sxs-lookup"><span data-stu-id="a163d-320">String</span></span> |<span data-ttu-id="a163d-321">Ange till ”CustomRole”.</span><span class="sxs-lookup"><span data-stu-id="a163d-321">Set to "CustomRole."</span></span> |
| <span data-ttu-id="a163d-322">Properties.permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="a163d-322">properties.permissions.actions</span></span> |<span data-ttu-id="a163d-323">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-323">Yes</span></span> |<span data-ttu-id="a163d-324">String]</span><span class="sxs-lookup"><span data-stu-id="a163d-324">String[]</span></span> |<span data-ttu-id="a163d-325">En strängmatris åtgärd anger de åtgärder som den uppdaterade anpassade rollen som ger åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a163d-325">An array of action strings specifying the operations to which the updated custom role grants access.</span></span> |
| <span data-ttu-id="a163d-326">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="a163d-326">properties.permissions.notActions</span></span> |<span data-ttu-id="a163d-327">Nej</span><span class="sxs-lookup"><span data-stu-id="a163d-327">No</span></span> |<span data-ttu-id="a163d-328">String]</span><span class="sxs-lookup"><span data-stu-id="a163d-328">String[]</span></span> |<span data-ttu-id="a163d-329">En strängmatris åtgärd anger åtgärderna som ska undantas från de åtgärder som den anpassade rollen som uppdaterade ger.</span><span class="sxs-lookup"><span data-stu-id="a163d-329">An array of action strings specifying the operations to exclude from the operations which the updated custom role grants.</span></span> |
| <span data-ttu-id="a163d-330">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="a163d-330">properties.assignableScopes</span></span> |<span data-ttu-id="a163d-331">Ja</span><span class="sxs-lookup"><span data-stu-id="a163d-331">Yes</span></span> |<span data-ttu-id="a163d-332">String]</span><span class="sxs-lookup"><span data-stu-id="a163d-332">String[]</span></span> |<span data-ttu-id="a163d-333">En matris med scope där uppdaterade anpassad roll kan användas.</span><span class="sxs-lookup"><span data-stu-id="a163d-333">An array of scopes in which the updated custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="a163d-334">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-334">Response</span></span>
<span data-ttu-id="a163d-335">Statuskod: 201</span><span class="sxs-lookup"><span data-stu-id="a163d-335">Status code: 201</span></span>

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

## <a name="delete-a-custom-role"></a><span data-ttu-id="a163d-336">Ta bort en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="a163d-336">Delete a Custom Role</span></span>
<span data-ttu-id="a163d-337">Ta bort en anpassad roll.</span><span class="sxs-lookup"><span data-stu-id="a163d-337">Delete a custom role.</span></span>

<span data-ttu-id="a163d-338">Om du vill ta bort en anpassad roll måste du ha tillgång till `Microsoft.Authorization/roleDefinitions/delete` igen på alla de `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="a163d-338">To delete a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/delete` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="a163d-339">I de inbyggda rollerna, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst till den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a163d-339">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="a163d-340">Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a163d-340">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="a163d-341">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="a163d-341">Request</span></span>
<span data-ttu-id="a163d-342">Använd den **ta bort** metoden med följande URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-342">Use the **DELETE** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="a163d-343">Gör följande ersättningar att anpassa din begäran inom URI:</span><span class="sxs-lookup"><span data-stu-id="a163d-343">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="a163d-344">Ersätt *{scope}* med den omfattning som du vill ta bort rolldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="a163d-344">Replace *{scope}* with the scope at which you wish to delete the role definition.</span></span> <span data-ttu-id="a163d-345">Följande exempel visar hur du kan ange omfång för olika nivåer:</span><span class="sxs-lookup"><span data-stu-id="a163d-345">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="a163d-346">Prenumerationen: /subscriptions/ {prenumerations-id}</span><span class="sxs-lookup"><span data-stu-id="a163d-346">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="a163d-347">Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="a163d-347">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="a163d-348">Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="a163d-348">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="a163d-349">Ersätt *{roll-definition-id}* med ID: t för GUID rollen definitionen av den anpassade rollen.</span><span class="sxs-lookup"><span data-stu-id="a163d-349">Replace *{role-definition-id}* with the GUID role definition id of the custom role.</span></span>
3. <span data-ttu-id="a163d-350">Ersätt *{api-version}* med 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="a163d-350">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="a163d-351">Svar</span><span class="sxs-lookup"><span data-stu-id="a163d-351">Response</span></span>
<span data-ttu-id="a163d-352">Statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="a163d-352">Status code: 200</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a163d-353">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a163d-353">Next steps</span></span>

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
