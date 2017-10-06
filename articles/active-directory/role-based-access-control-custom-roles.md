---
title: "aaaCreate anpassade roller för Azure RBAC | Microsoft Docs"
description: "Lär dig hur toodefine anpassade roller med rollbaserad åtkomstkontroll för mer exakta identity management i din Azure-prenumeration."
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
ms.openlocfilehash: 60df12632ef6c086d5feeb1809196d7c4ee5e021
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a>Skapa anpassade roller för rollbaserad åtkomstkontroll
Skapa en anpassad roll i rollbaserad åtkomstkontroll (RBAC) om ingen av hello inbyggda roller uppfyller dina specifika behov. Anpassade roller kan skapas med [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure-kommandoradsgränssnittet](role-based-access-control-manage-access-azure-cli.md) (CLI) och hello [REST API](role-based-access-control-manage-access-rest.md). Du kan tilldela anpassade roller toousers, grupper och program på prenumerationen, resursgruppen och resursen omfattningar precis som inbyggda roller. Anpassade roller lagras i Azure AD-klient och kan delas mellan prenumerationer.

Varje klient kan skapa upp too2000 anpassade roller. 

hello visas följande exempel en anpassad roll för övervakning och starta om virtuella datorer:

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
## <a name="actions"></a>Åtgärder
Hej **åtgärder** -egenskapen för en anpassad roll anger hello Azure operations toowhich hello rollen ger åtkomst. Det är en samling med åtgärden strängar som identifierar skyddbara drift av Azure-resursprovidrar. Åtgärden strängar följer hello format för `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Åtgärden strängar som innehåller jokertecken (\*) och bevilja åtkomst tooall åtgärder som matchar hello åtgärden sträng. Exempel:

* `*/read`ger åtkomst till tooread åtgärder för alla typer av resurser för alla Azure-resurs-providers.
* `Microsoft.Compute/*`ger åtkomst till tooall åtgärder för alla typer av resurser i hello Microsoft.Compute-resursprovidern.
* `Microsoft.Network/*/read`ger åtkomst till tooread åtgärder för alla typer av resurser i hello Microsoft.Network resource provider för Azure.
* `Microsoft.Compute/virtualMachines/*`ger åtkomst till tooall åtgärder för virtuella datorer och dess underordnade resurstyper.
* `Microsoft.Web/sites/restart/Action`ger åtkomst till toorestart webbplatser.

Använd `Get-AzureRmProviderOperation` (i PowerShell) eller `azure provider operations show` (i Azure CLI) toolist drift av Azure-resursprovidrar. Du kan också använda dessa kommandon tooverify att en sträng för åtgärden är giltig och tooexpand jokertecken åtgärden strängar.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Skärmbild av PowerShell - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI skärmbild - azure provider operations visa ”Microsoft.Compute/virtualMachines/ \* /Action” ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Använd hello **NotActions** egenskapen om hello uppsättning åtgärder som du vill tooallow definieras enklare genom att utesluta vissa åtgärder. hello åtkomst av en anpassad roll beräknas genom att subtrahera hello **NotActions** åtgärder från hello **åtgärder** åtgärder.

> [!NOTE]
> Om en användare har tilldelats en roll som inte omfattar en åtgärd i **NotActions**, och har tilldelats en andra roll som beviljar åtkomst toohello samma åtgärd, hello användaren är tillåten tooperform åtgärden. **NotActions** är inte en neka-regel – det är bara ett bekvämt sätt toocreate en uppsättning tillåtna åtgärder när specifika åtgärder måste toobe uteslutas.
>
>

## <a name="assignablescopes"></a>AssignableScopes
Hej **AssignableScopes** -egenskapen för hello anpassad roll anger hello scope (prenumerationer, resursgrupper och resurser) inom vilken hello anpassade roll som är tillgängliga för tilldelning. Du kan göra hello anpassad roll tillgängliga för tilldelning i hello-prenumerationer eller resursgrupper som kräver och inte oreda användarens upplevelse i hello resten av hello prenumerationer eller resursgrupper.

Exempel på giltiga kan tilldelas omfång:

* ”/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e”, ”/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624” - tillgängliggör hello roll för tilldelning i två prenumerationer.
* ”/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e” - gör hello rollen tillgängliga för tilldelning i en enda prenumeration.
* ”/ prenumerationer/c276fc76-9cd4-44c9-99a7-4fd71546436e/resursgrupper/nätverk” - gör hello roll som är tillgängliga för tilldelning endast i resursgruppen för hello nätverk.

> [!NOTE]
> Du måste använda minst en prenumeration, resursgrupp eller resurs-ID.
>
>

## <a name="custom-roles-access-control"></a>Anpassade roller åtkomstkontroll
Hej **AssignableScopes** hello anpassad roll också bestämmer vem som kan visa, ändra och ta bort hello roll.

* Vem som kan skapa en anpassad roll?
    Ägare (och administratörer för användare) för prenumerationer, resursgrupper och resurser kan skapa anpassade roller för användning i dessa scope.
    hello användaren skapar hello roll måste toobe kan tooperform `Microsoft.Authorization/roleDefinition/write` åtgärden på alla hello **AssignableScopes** hello-rollen.
* Vem som kan ändra en anpassad roll?
    Ägare (och administratörer för användare) för prenumerationer, resursgrupper och resurser kan ändra anpassade roller i dessa scope. Användare behöver toobe kan tooperform hello `Microsoft.Authorization/roleDefinition/write` åtgärden på alla hello **AssignableScopes** av en anpassad roll.
* Vem som kan visa anpassade roller?
    Alla inbyggda roller i Azure RBAC Tillåt visning av roller som är tillgängliga för tilldelning. Användare som kan utföra hello `Microsoft.Authorization/roleDefinition/read` åtgärden på ett scope kan visa hello RBAC-roller som är tillgängliga för tilldelning i detta scope.

## <a name="see-also"></a>Se även
* [Rollbaserad åtkomstkontroll](role-based-access-control-configure.md): komma igång med RBAC på hello Azure-portalen.
* Lär dig hur toomanage åt med:
  * [PowerShell](role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
  * [REST-API](role-based-access-control-manage-access-rest.md)
* [Inbyggda roller](role-based-access-built-in-roles.md): få information om hello roller som redan finns i RBAC.
