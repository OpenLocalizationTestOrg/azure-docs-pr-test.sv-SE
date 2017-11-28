---
title: "aaaTenant admin höjer åtkomst - Azure AD | Microsoft Docs"
description: "Det här avsnittet beskriver hello inbyggda roller för rollbaserad åtkomstkontroll (RBAC)."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andredm
ms.openlocfilehash: 7996f2af3277dc40e2a1766cc4a7862a2399cdef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a><span data-ttu-id="9573b-103">Utöka behörighet som en klientadministratör med rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="9573b-103">Elevate access as a tenant admin with Role-Based Access Control</span></span>

<span data-ttu-id="9573b-104">Rollbaserad åtkomstkontroll hjälper innehavaradministratörer hämta tillfälliga rikta åtkomst så att de kan ge högre behörigheter än normalt.</span><span class="sxs-lookup"><span data-stu-id="9573b-104">Role-based Access Control helps tenant administrators get temporary elevations in access so that they can grant higher permissions than normal.</span></span> <span data-ttu-id="9573b-105">En klientadministratör kan utöka sig själv toohello åtkomst Användaradministratör vid behov.</span><span class="sxs-lookup"><span data-stu-id="9573b-105">A tenant admin can elevate herself toohello User Access Administrator role when needed.</span></span> <span data-ttu-id="9573b-106">Rollen ger hello klient admin behörigheter toogrant sig själv eller andra roller på hello ”/” omfång.</span><span class="sxs-lookup"><span data-stu-id="9573b-106">That role gives hello tenant admin permissions toogrant herself or others roles at hello "/" scope.</span></span>

<span data-ttu-id="9573b-107">Den här funktionen är viktigt eftersom det tillåter hello klient admin toosee alla hello prenumerationer som finns i en organisation.</span><span class="sxs-lookup"><span data-stu-id="9573b-107">This feature is important because it allows hello tenant admin toosee all hello subscriptions that exist in an organization.</span></span> <span data-ttu-id="9573b-108">Dessutom kan alla hello prenumerationer för automation appar (t.ex fakturering och granskning) tooaccess och ange en korrekt bild av hello tillståndet för hello organisation för hantering av fakturerings- eller tillgången.</span><span class="sxs-lookup"><span data-stu-id="9573b-108">It also allows for automation apps (like invoicing and auditing) tooaccess all hello subscriptions and provide an accurate view of hello state of hello organization for billing or asset management.</span></span>  

## <a name="how-toouse-elevateaccess-toogive-tenant-access"></a><span data-ttu-id="9573b-109">Hur toouse elevateAccess toogive klient åtkomst</span><span class="sxs-lookup"><span data-stu-id="9573b-109">How toouse elevateAccess toogive tenant access</span></span>

<span data-ttu-id="9573b-110">hello grundläggande processen fungerar med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9573b-110">hello basic process works with hello following steps:</span></span>

1. <span data-ttu-id="9573b-111">Använda REST kan anropa *elevateAccess*, vilket ger du Hej administratör för användaråtkomst rollen på ”/” omfång.</span><span class="sxs-lookup"><span data-stu-id="9573b-111">Using REST, call *elevateAccess*, which grants you hello User Access Administrator role at "/" scope.</span></span>

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. <span data-ttu-id="9573b-112">Skapa en [rolltilldelning](/rest/api/authorization/roleassignments) tooassign någon roll på ett scope.</span><span class="sxs-lookup"><span data-stu-id="9573b-112">Create a [role assignment](/rest/api/authorization/roleassignments) tooassign any role at any scope.</span></span> <span data-ttu-id="9573b-113">hello som följande exempel visar hello egenskaper för att tilldela hello rollen Läsare på ”/” scope:</span><span class="sxs-lookup"><span data-stu-id="9573b-113">hello following example shows hello properties for assigning hello Reader role at "/" scope:</span></span>

    ```
    { "properties":{
    "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/acdd72a7338548efbd42f606fba81ae7",
    "principalId": "cbc5e050-d7cd-4310-813b-4870be8ef5bb",
    "scope": "/"
    },
    "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
    }
    ```

3. <span data-ttu-id="9573b-114">När en användare åtkomst till Admin, kan du även ta bort rolltilldelningar på ”/” omfång.</span><span class="sxs-lookup"><span data-stu-id="9573b-114">While a User Access Admin, you can also delete role assignments at "/" scope.</span></span>

4. <span data-ttu-id="9573b-115">Återkalla användare åtkomst till Admin-privilegier tills de behövs igen.</span><span class="sxs-lookup"><span data-stu-id="9573b-115">Revoke your User Access Admin privileges until they're needed again.</span></span>


## <a name="how-tooundo-hello-elevateaccess-action"></a><span data-ttu-id="9573b-116">Hur tooundo hello elevateAccess åtgärd</span><span class="sxs-lookup"><span data-stu-id="9573b-116">How tooundo hello elevateAccess action</span></span>

<span data-ttu-id="9573b-117">När du anropar *elevateAccess* du skapar en rolltilldelning själv, så toorevoke dem behörighet att du behöver toodelete hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="9573b-117">When you call *elevateAccess* you create a role assignment for yourself, so toorevoke those privileges you need toodelete hello assignment.</span></span>

1.  <span data-ttu-id="9573b-118">Anropa [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) där roleName = administratör för användaråtkomst toodetermine hello namn GUID för Hej administratör för användaråtkomst roll.</span><span class="sxs-lookup"><span data-stu-id="9573b-118">Call [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) where roleName = User Access Administrator toodetermine hello name GUID of hello User Access Administrator role.</span></span> <span data-ttu-id="9573b-119">hello svar bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="9573b-119">hello response should look like this:</span></span>

    ```
    {"value":[{"properties":{
    "roleName":"User Access Administrator",
    "type":"BuiltInRole",
    "description":"Lets you manage user access tooAzure resources.",
    "assignableScopes":["/"],
    "permissions":[{"actions":["*/read","Microsoft.Authorization/*","Microsoft.Support/*"],"notActions":[]}],
    "createdOn":"0001-01-01T08:00:00.0000000Z",
    "updatedOn":"2016-05-31T23:14:04.6964687Z",
    "createdBy":null,
    "updatedBy":null},
    "id":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "type":"Microsoft.Authorization/roleDefinitions",
    "name":"18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"}],
    "nextLink":null}
    ```

    <span data-ttu-id="9573b-120">Spara hello GUID från hello *namn* parameter i det här fallet **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span><span class="sxs-lookup"><span data-stu-id="9573b-120">Save hello GUID from hello *name* parameter, in this case **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span></span>

2. <span data-ttu-id="9573b-121">Anropa [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) där principalId = egna ObjectId.</span><span class="sxs-lookup"><span data-stu-id="9573b-121">Call [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) where principalId = your own ObjectId.</span></span> <span data-ttu-id="9573b-122">Detta visar alla tilldelningar i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="9573b-122">This lists all your assignments in hello tenant.</span></span> <span data-ttu-id="9573b-123">Leta efter hello en där hello omfånget är ”/” och hello RoleDefinitionId slutar med hello rollen namnet GUID som du hittade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="9573b-123">Look for hello one where hello scope is "/" and hello RoleDefinitionId ends with hello role name GUID you found in step 1.</span></span> <span data-ttu-id="9573b-124">hello rolltilldelning bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="9573b-124">hello role assignment should look like this:</span></span>

    ```
    {"value":[{"properties":{
    "roleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "principalId":"{objectID}",
    "scope":"/",
    "createdOn":"2016-08-17T19:21:16.3422480Z",
    "updatedOn":"2016-08-17T19:21:16.3422480Z",
    "createdBy":"93ce6722-3638-4222-b582-78b75c5c6d65",
    "updatedBy":"93ce6722-3638-4222-b582-78b75c5c6d65"},
    "id":"/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
    "type":"Microsoft.Authorization/roleAssignments",
    "name":"e7dd75bc-06f6-4e71-9014-ee96a929d099"}],
    "nextLink":null}
    ```

    <span data-ttu-id="9573b-125">Spara igen, hello GUID från hello *namn* parameter i det här fallet **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span><span class="sxs-lookup"><span data-stu-id="9573b-125">Again, save hello GUID from hello *name* parameter, in this case **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span></span>

3. <span data-ttu-id="9573b-126">Slutligen anropa [bort roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) där roleAssignmentId = hello namn GUID som du hittade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="9573b-126">Finally, call [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) where roleAssignmentId = hello name GUID you found in step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9573b-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9573b-127">Next steps</span></span>

- <span data-ttu-id="9573b-128">Lär dig mer om [Hantera rollbaserad åtkomstkontroll med REST](role-based-access-control-manage-access-rest.md)</span><span class="sxs-lookup"><span data-stu-id="9573b-128">Learn more about [managing Role-Based Access Control with REST](role-based-access-control-manage-access-rest.md)</span></span>

- <span data-ttu-id="9573b-129">[Hantera åtkomst tilldelningar](role-based-access-control-manage-assignments.md) i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9573b-129">[Manage access assignments](role-based-access-control-manage-assignments.md) in hello Azure portal</span></span>
