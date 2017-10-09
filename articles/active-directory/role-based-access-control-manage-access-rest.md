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
# <a name="manage-role-based-access-control-with-hello-rest-api"></a>Hantera rollbaserad åtkomstkontroll med hello REST API
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST-API](role-based-access-control-manage-access-rest.md)

Rollbaserad åtkomstkontroll (RBAC) i hello Azure-portalen och Azure Resource Manager API kan du hantera åtkomst tooyour prenumeration och resurser på en detaljerad nivå. Med den här funktionen kan du bevilja åtkomst för användare, grupper eller tjänstens huvudnamn i Active Directory genom att tilldela vissa roller toothem för ett visst område.

## <a name="list-all-role-assignments"></a>Visa en lista med alla rolltilldelningar
Visar alla hello rolltilldelningar på hello angetts omfång och subscopes.

toolist rolltilldelningar, måste du ha tillgång för`Microsoft.Authorization/roleAssignments/read` åtgärden hello definitionsområdet. Alla hello inbyggda roller beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **hämta** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello scope som du vill toolist hello rolltilldelningar. hello som följande exempel visar hur toospecify hello omfång för olika nivåer:

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1  
   * Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{api-version}* med 2015-07-01.
3. Ersätt *{filter}* med hello villkor som du vill tilldelningslista för tooapply toofilter hello roll:

   * Lista rolltilldelningar för endast hello angiven omfång, exklusive hello rolltilldelningar på subscopes:`atScope()`    
   * Lista rolltilldelningar för en viss användare, grupp eller ett program:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
   * Visa en lista med rolltilldelningar för en viss användare, inklusive de som ärvts från grupper |`assignedTo('{objectId of user}')`

### <a name="response"></a>Svar
Statuskod: 200

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

## <a name="get-information-about-a-role-assignment"></a>Hämta information om en rolltilldelning
Hämtar information om en enda rolltilldelning som anges av hello rollen tilldelning identifierare.

tooget information om en rolltilldelning, måste du ha tillgång för`Microsoft.Authorization/roleAssignments/read` igen. Alla hello inbyggda roller beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **hämta** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello scope som du vill toolist hello rolltilldelningar. hello som följande exempel visar hur toospecify hello omfång för olika nivåer:

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1  
   * Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{roll-tilldelning-id}* med hello GUID-identifierare för hello rolltilldelning.
3. Ersätt *{api-version}* med 2015-07-01.

### <a name="response"></a>Svar
Statuskod: 200

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

## <a name="create-a-role-assignment"></a>Skapa en rolltilldelning
Skapa en roll tilldelning vid hello specificerat omfånget för hello ange huvudnamn beviljande hello angivna rollen.

toocreate en rolltilldelning måste du ha tillgång för`Microsoft.Authorization/roleAssignments/write` igen. I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **PLACERA** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello scope som du vill toocreate hello rolltilldelningar. När du skapar en rolltilldelning på en överordnad omfattning, ärver alla underordnade omfång hello samma rolltilldelning. hello som följande exempel visar hur toospecify hello omfång för olika nivåer:

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1   
   * Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{roll-tilldelning-id}* med en ny GUID som blir hello GUID-identifierare för hello ny rolltilldelning.
3. Ersätt *{api-version}* med 2015-07-01.

Ange hello värden i hello följande format för hello begärantext:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Elementnamn | Krävs | Typ | Beskrivning |
| --- | --- | --- | --- |
| roleDefinitionId |Ja |Sträng |hello identifierare för hello roll. hello identifierare hello format är:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId |Ja |Sträng |objectId av hello Azure AD-säkerhetsobjekt (användare, grupp eller tjänstens huvudnamn) toowhich hello roll har tilldelats. |

### <a name="response"></a>Svar
Statuskod: 201

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

## <a name="delete-a-role-assignment"></a>Ta bort en rolltilldelning
Ta bort en rolltilldelning i hello angetts omfång.

toodelete en rolltilldelning måste du ha åtkomst toohello `Microsoft.Authorization/roleAssignments/delete` igen. I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **ta bort** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello scope som du vill toocreate hello rolltilldelningar. hello som följande exempel visar hur toospecify hello omfång för olika nivåer:

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1  
   * Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{roll-tilldelning-id}* med hello tilldelnings-id GUID.
3. Ersätt *{api-version}* med 2015-07-01.

### <a name="response"></a>Svar
Statuskod: 200

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

## <a name="list-all-roles"></a>Visa en lista över alla roller
Listar alla hello-roller som är tillgängliga för tilldelning i hello angivna omfattningen.

toolist roller, måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/read` åtgärden hello definitionsområdet. Alla hello inbyggda roller beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **hämta** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello scope som du vill toolist hello roller. hello som följande exempel visar hur toospecify hello omfång för olika nivåer:

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1  
   * Resursen /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{api-version}* med 2015-07-01.
3. Ersätt *{filter}* med hello villkor som du vill tooapply toofilter hello lista över roller:

   * Lista roller som är tillgängliga för tilldelning på hello ange omfånget och alla dess underordnade omfattningar:`atScopeAndBelow()`
   * Sök efter en roll med exakt visningsnamn: `roleName%20eq%20'{role-display-name}'`. Använd hello URL-kodade hello exakt visningsnamn hello roll. Till exempel`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Svar
Statuskod: 200

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

## <a name="get-information-about-a-role"></a>Hämta information om en roll
Hämtar information om en enda roll som anges av hello identifierare för rollen. tooget information om en roll med hjälp av dess visningsnamn finns [lista över alla roller för](role-based-access-control-manage-access-rest.md#list-all-roles).

tooget information om en roll, måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/read` igen. Alla hello inbyggda roller beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **hämta** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello scope som du vill toolist hello rolltilldelningar. hello som följande exempel visar hur toospecify hello omfång för olika nivåer:

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1  
   * Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{roll-definition-id}* med hello GUID-identifierare för hello rolldefinitionen.
3. Ersätt *{api-version}* med 2015-07-01.

### <a name="response"></a>Svar
Statuskod: 200

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

## <a name="create-a-custom-role"></a>Skapa en anpassad roll
Skapa en anpassad roll.

toocreate en anpassad roll måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/write` åtgärden på alla hello `AssignableScopes`. I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **PLACERA** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello första *AssignableScope* av hello anpassad roll. hello som följande exempel visar hur toospecify hello omfång för olika nivåer.

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1  
   * Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{roll-definition-id}* med en ny GUID som blir hello GUID-identifierare för hello ny anpassad roll.
3. Ersätt *{api-version}* med 2015-07-01.

Ange hello värden i hello följande format för hello begärantext:

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

| Elementnamn | Krävs | Typ | Beskrivning |
| --- | --- | --- | --- |
| namn |Ja |Sträng |GUID-identifierare för hello anpassad roll. |
| properties.roleName |Ja |Sträng |Visningsnamnet för hello anpassad roll. Maximal storlek 128 tecken. |
| Properties.Description |Nej |Sträng |Beskrivning av anpassad hello-roll. Maximal storlek 1024 tecken. |
| Properties.Type |Ja |Sträng |Ställa in också ”CustomRole”. |
| Properties.permissions.Actions |Ja |String] |En matris med åtgärden strängar åtgärder för att ange hello beviljat hello anpassad roll. |
| properties.permissions.notActions |Nej |String] |En strängmatris åtgärd att ange hello operations tooexclude från hello-åtgärder som tilldelats av hello anpassad roll. |
| properties.assignableScopes |Ja |String] |En matris med omfattningar i vilka hello anpassad roll kan användas. |

### <a name="response"></a>Svar
Statuskod: 201

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

## <a name="update-a-custom-role"></a>Uppdatera en anpassad roll
Ändra en anpassad roll.

toomodify en anpassad roll måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/write` åtgärden på alla hello `AssignableScopes`. I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **PLACERA** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello första *AssignableScope* av hello anpassad roll. hello som följande exempel visar hur toospecify hello omfång för olika nivåer:

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1  
   * Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{roll-definition-id}* med hello GUID-identifierare för hello anpassad roll.
3. Ersätt *{api-version}* med 2015-07-01.

Ange hello värden i hello följande format för hello begärantext:

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

| Elementnamn | Krävs | Typ | Beskrivning |
| --- | --- | --- | --- |
| namn |Ja |Sträng |GUID-identifierare för hello anpassad roll. |
| properties.roleName |Ja |Sträng |Visningsnamn för hello uppdaterade anpassad roll. |
| Properties.Description |Nej |Sträng |Beskrivning av hello uppdatera anpassad roll. |
| Properties.Type |Ja |Sträng |Ställa in också ”CustomRole”. |
| Properties.permissions.Actions |Ja |String] |En strängmatris åtgärd att ange hello operations toowhich hello uppdatera anpassade roll som beviljar åtkomst. |
| properties.permissions.notActions |Nej |String] |En matris med åtgärden strängar att ange hello operations tooexclude från hello operations vilka hello uppdateras ger anpassad roll. |
| properties.assignableScopes |Ja |String] |En matris med omfattningar i vilka hello uppdaterade anpassad roll kan användas. |

### <a name="response"></a>Svar
Statuskod: 201

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

## <a name="delete-a-custom-role"></a>Ta bort en anpassad roll
Ta bort en anpassad roll.

toodelete en anpassad roll måste du ha tillgång för`Microsoft.Authorization/roleDefinitions/delete` åtgärden på alla hello `AssignableScopes`. I hello inbyggda roller, endast *ägare* och *administratör för användaråtkomst* beviljas åtkomst toothis igen. Mer information om rolltilldelningar och hantera åtkomst till Azure-resurser finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

### <a name="request"></a>Förfrågan
Använd hello **ta bort** metod med hello följande URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Inom hello URI, gör du följande ersättningar toocustomize hello din begäran:

1. Ersätt *{scope}* med hello scope som du vill att toodelete hello rolldefinitionen. hello som följande exempel visar hur toospecify hello omfång för olika nivåer:

   * Prenumerationen: /subscriptions/ {prenumerations-id}  
   * Resursgrupp: /subscriptions/ {prenumerations-id} / resursgrupper/myresourcegroup1  
   * Resurs: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Ersätt *{roll-definition-id}* med hello GUID rolldefinitions-ID: hello anpassad roll.
3. Ersätt *{api-version}* med 2015-07-01.

### <a name="response"></a>Svar
Statuskod: 200

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

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
