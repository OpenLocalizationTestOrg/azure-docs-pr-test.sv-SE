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
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Utöka behörighet som en klientadministratör med rollbaserad åtkomstkontroll

Rollbaserad åtkomstkontroll hjälper innehavaradministratörer hämta tillfälliga rikta åtkomst så att de kan ge högre behörigheter än normalt. En klientadministratör kan utöka sig själv toohello åtkomst Användaradministratör vid behov. Rollen ger hello klient admin behörigheter toogrant sig själv eller andra roller på hello ”/” omfång.

Den här funktionen är viktigt eftersom det tillåter hello klient admin toosee alla hello prenumerationer som finns i en organisation. Dessutom kan alla hello prenumerationer för automation appar (t.ex fakturering och granskning) tooaccess och ange en korrekt bild av hello tillståndet för hello organisation för hantering av fakturerings- eller tillgången.  

## <a name="how-toouse-elevateaccess-toogive-tenant-access"></a>Hur toouse elevateAccess toogive klient åtkomst

hello grundläggande processen fungerar med hello följande steg:

1. Använda REST kan anropa *elevateAccess*, vilket ger du Hej administratör för användaråtkomst rollen på ”/” omfång.

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. Skapa en [rolltilldelning](/rest/api/authorization/roleassignments) tooassign någon roll på ett scope. hello som följande exempel visar hello egenskaper för att tilldela hello rollen Läsare på ”/” scope:

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

3. När en användare åtkomst till Admin, kan du även ta bort rolltilldelningar på ”/” omfång.

4. Återkalla användare åtkomst till Admin-privilegier tills de behövs igen.


## <a name="how-tooundo-hello-elevateaccess-action"></a>Hur tooundo hello elevateAccess åtgärd

När du anropar *elevateAccess* du skapar en rolltilldelning själv, så toorevoke dem behörighet att du behöver toodelete hello tilldelning.

1.  Anropa [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) där roleName = administratör för användaråtkomst toodetermine hello namn GUID för Hej administratör för användaråtkomst roll. hello svar bör se ut så här:

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

    Spara hello GUID från hello *namn* parameter i det här fallet **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.

2. Anropa [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) där principalId = egna ObjectId. Detta visar alla tilldelningar i hello-klient. Leta efter hello en där hello omfånget är ”/” och hello RoleDefinitionId slutar med hello rollen namnet GUID som du hittade i steg 1. hello rolltilldelning bör se ut så här:

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

    Spara igen, hello GUID från hello *namn* parameter i det här fallet **e7dd75bc-06f6-4e71-9014-ee96a929d099**.

3. Slutligen anropa [bort roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) där roleAssignmentId = hello namn GUID som du hittade i steg 2.

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [Hantera rollbaserad åtkomstkontroll med REST](role-based-access-control-manage-access-rest.md)

- [Hantera åtkomst tilldelningar](role-based-access-control-manage-assignments.md) i hello Azure-portalen
