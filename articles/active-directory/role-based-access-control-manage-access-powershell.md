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
# <a name="manage-role-based-access-control-with-azure-powershell"></a>Hantera rollbaserad åtkomstkontroll med Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST-API](role-based-access-control-manage-access-rest.md)

Du kan använda rollbaserad åtkomstkontroll (RBAC) i hello Azure-portalen och Azure Resource Management API toomanage åtkomst tooyour prenumeration på en detaljerad nivå. Med den här funktionen kan du bevilja åtkomst för användare, grupper eller tjänstens huvudnamn i Active Directory genom att tilldela vissa roller toothem för ett visst område.

Innan du kan använda PowerShell toomanage RBAC måste hello följande krav:

* Azure PowerShell version 0.8.8 eller senare. tooinstall hello senaste versionen och koppla den med din Azure-prenumeration finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* Azure Resource Manager-cmdlets. Installera hello [Azure Resource Manager cmdlets](/powershell/azure/overview) i PowerShell.

## <a name="list-roles"></a>Lista roller
### <a name="list-all-available-roles"></a>Visa en lista över alla tillgängliga roller
toolist RBAC-roller som är tillgängliga för tilldelning och tooinspect hello operations toowhich de beviljar åtkomst, Använd `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Lista över åtgärder för en roll
toolist hello åtgärder för en viss roll använder `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell-Get AzureRmRoleDefinition för en viss roll – skärmbild](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Se vem som har åtkomst
använda toolist RBAC åtkomst tilldelningar `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Lista rolltilldelningar för ett visst område
Du kan se alla hello åtkomst tilldelningar för en angiven prenumeration, resursgrupp eller resurs. Till exempel toosee hello alla aktiva hello-tilldelningar för en resursgrupp, Använd `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment för en resursgrupp – skärmbild](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-tooa-user"></a>Lista roller tilldelad tooa användare
toolist alla hello-roller som är tilldelade tooa angivna användare och hello-roller som tilldelas toohello grupper toowhich hello användaren tillhör, Använd `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment för en användare – skärmbild](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Lista klassiska tjänstadministratören och coadmin rolltilldelningar
toolist åtkomst tilldelningar för hello klassiska prenumerationer administratör och coadministrators, Använd:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Bevilja åtkomst
### <a name="search-for-object-ids"></a>Sök efter objekt-ID
tooassign en roll måste tooidentify både hello-objekt (användare, grupp eller program) och hello omfång.

Om du inte vet hello prenumerations-ID, du kan hitta den i hello **prenumerationer** bladet på hello Azure-portalen. hur tooquery för hello prenumerations-ID, se toolearn [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) på MSDN.

Använd tooget hello objekt-ID för en Azure AD-grupp:

    Get-AzureRmADGroup -SearchString <group name in quotes>

tooget hello objekt-ID för en Azure AD huvudnamn för tjänsten eller programmet, Använd:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a>Tilldela en roll tooan programmet hello prenumeration definitionsområdet
toogrant tooan programmet access definitionsområdet hello prenumeration, Använd:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a>Tilldela en användare med rollen tooa på hello resurs Gruppomfång
toogrant åtkomst tooa användaren vid hello resurs Gruppomfång, Använd:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a>Tilldela en roll tooa grupp hello resurs definitionsområdet
toogrant tooa åtkomstgruppen definitionsområdet hello resurs, Använd:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell nya AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Ta bort åtkomst
tooremove åtkomst för användare, grupper och program, Använd:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell ta bort AzureRmRoleAssignment – skärmbild](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Skapa en anpassad roll
toocreate en anpassad roll använder hello ```New-AzureRmRoleDefinition``` kommando. Det finns två metoder för att strukturera hello roll, med hjälp av PSRoleDefinitionObject eller en JSON-mall. 

## <a name="get-actions-for-a-resource-provider"></a>Hämta åtgärder för en Resursprovider
När du skapar anpassade roller från början, är det viktigt tooknow alla hello möjliga åtgärder från hello resursleverantörer.
Använd hello ```Get-AzureRMProviderOperation``` kommandot tooget informationen.
Om du vill toocheck använder alla hello tillgängliga åtgärderna för den virtuella datorn det här kommandot:

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a>Skapa en roll med PSRoleDefinitionObject
När du använder PowerShell toocreate en anpassad roll kan du börja om från början eller Använd en av hello [inbyggda roller](role-based-access-built-in-roles.md) som utgångspunkt. hello exemplet i det här avsnittet börjar med en inbyggd roll och anpassar den med mer behörigheter. Redigera hello attribut tooadd hello *åtgärder*, *notActions*, eller *scope* som du vill och sedan spara hello ändringar som en ny roll.

hello följande exempel startar med hello *Virtual Machine-deltagare* roll och använder den toocreate en anpassad roll kallas *virtuella operatorn*. hello ny roll beviljar åtkomst tooall läsåtgärder av *Microsoft.Compute*, *Microsoft.Storage*, och *Microsoft.Network* resurs providers och beviljar åtkomst toostart, starta om och övervaka virtuella datorer. hello anpassad roll kan användas i två prenumerationer.

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

### <a name="create-role-with-json-template"></a>Skapa en roll med JSON-mall
En JSON-mall kan användas som hello definitionen av datakällan för hello anpassad roll. hello följande exempel skapar en anpassad roll som tillåter läsbehörighet toostorage och beräkna resurser kan komma åt toosupport och lägger till rollen tootwo prenumerationer. Skapa en ny fil `C:\CustomRoles\customrole1.json` med hello följande exempel. hello Id ska anges för`null` inledande rollen skapas som ett nytt ID genereras automatiskt. 

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
tooadd hello rollen toohello prenumerationer, kör hello följande PowerShell-kommando:
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>Ändra en anpassad roll
Liknande toocreating en anpassad roll kan du ändra en befintlig anpassad roll med hjälp av hello PSRoleDefinitionObject eller en JSON-mall.

### <a name="modify-role-with-psroledefinitionobject"></a>Ändra roll med PSRoleDefinitionObject
toomodify en anpassad roll först använder hello `Get-AzureRmRoleDefinition` kommandot tooretrieve hello rolldefinitionen. Därefter kontrollera hello önskade ändringar toohello rolldefinitionen. Använd slutligen hello `Set-AzureRmRoleDefinition` kommandot toosave hello ändrade rolldefinitionen.

hello följande exempel läggs hello `Microsoft.Insights/diagnosticSettings/*` åtgärden toohello *virtuella operatorn* anpassad roll.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Set AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

hello följande exempel lägger till en Azure-prenumeration toohello tilldelningsbara scope för hello *virtuella operatorn* anpassad roll.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Set AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a>Ändra roll med JSON-mall
Med hello tidigare JSON-mall kan du enkelt ändra en befintlig anpassad roll tooadd eller ta bort åtgärder. Uppdatera hello JSON-mall och lägga till hello skrivskyddade åtgärd för nätverk som visas i följande exempel hello. hello definitioner som förtecknas i hello mallen är inte kumulativt tillämpade tooan befintlig definition, vilket innebär att rollen hello visas exakt som du anger i hello mallen. Du måste också tooupdate hello Id-fältet med hello ID hello roll. Om du är osäker på det här värdet är, kan du använda hello `Get-AzureRmRoleDefinition` cmdlet tooget informationen.

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

tooupdate hello befintliga rollen, kör hello följande PowerShell-kommando:
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Ta bort en anpassad roll
toodelete en anpassad roll använder hello `Remove-AzureRmRoleDefinition` kommando.

hello följande exempel tar bort hello *virtuella operatorn* anpassad roll.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell ta bort AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Lista över anpassade roller
toolist hello roller som är tillgängliga för tilldelning på scopenivå, använder hello `Get-AzureRmRoleDefinition` kommando.

hello som följande exempel visar en lista över alla roller som är tillgängliga för tilldelning i hello valda prenumerationen.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

I följande exempel hello, hello *virtuella operatorn* anpassad roll är inte tillgänglig i hello *Production4* prenumerationen eftersom den prenumerationen finns inte i hello  **AssignableScopes** hello-rollen.

![RBAC PowerShell-Get AzureRmRoleDefinition – skärmbild](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Se även
* [Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

