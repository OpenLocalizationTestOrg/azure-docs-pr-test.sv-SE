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
# <a name="manage-role-based-access-control-with-hello-azure-command-line-interface"></a>Hantera rollbaserad åtkomstkontroll med hello Azure-kommandoradsgränssnittet
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST-API](role-based-access-control-manage-access-rest.md)


Du kan använda rollbaserad åtkomstkontroll (RBAC) i hello Azure-portalen och Azure Resource Manager API toomanage åtkomst tooyour prenumeration och resurser på en detaljerad nivå. Med den här funktionen kan du bevilja åtkomst för användare, grupper eller tjänstens huvudnamn i Active Directory genom att tilldela vissa roller toothem för ett visst område.

Innan du kan använda hello Azure-kommandoradsgränssnittet (CLI) toomanage RBAC, måste du ha hello följande krav:

* Azure CLI version 0.8.8 eller senare. tooinstall hello senaste versionen och koppla den med din Azure-prenumeration finns [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).
* Azure Resource Manager i Azure CLI. Gå för[Using hello Azure CLI med hello Resource Manager](../xplat-cli-azure-resource-manager.md) för mer information.

## <a name="list-roles"></a>Lista roller
### <a name="list-all-available-roles"></a>Visa en lista över alla tillgängliga roller
toolist använder alla tillgängliga roller:

        azure role list

hello följande exempel visar hello lista över *alla tillgängliga roller*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Azure RBAC kommandoraden - azure rollen list - skärmbild](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Lista över åtgärder för en roll
toolist hello åtgärder för en roll, Använd:

    azure role show "<role name>"

hello följande exempel visar hello åtgärder av hello *deltagare* och *Virtual Machine-deltagare* roller.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Azure RBAC kommandoraden - azure rollen show - skärmbild](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a>Listan åtkomst
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Lista rolltilldelningar gälla för en resursgrupp
toolist hello rolltilldelningar som finns i en resursgrupp, Använd:

    azure role assignment list --resource-group <resource group name>

hello följande exempel visar hello rolltilldelningar i hello *pharma-försäljning-projecforcast* grupp.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Skärmbild av Azure RBAC kommandoraden - azure rollen tilldelningslista av grupp-](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Lista rolltilldelningar för en användare
toolist hello rolltilldelningar för en specifik användare och hello tilldelningar som är tilldelade tooa användargrupper, Använd:

    azure role assignment list --signInName <user email>

Du kan också se rolltilldelningar som ärvs från grupper genom att ändra hello-kommando:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

hello följande exempel visar hello rolltilldelningar som beviljas toohello  *sameert@aaddemo.com*  användare. Detta inkluderar roller som tilldelas direkt toohello användare och roller som ärvs från grupper.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure RBAC kommandoraden - azure rollen tilldelningslista av användare – skärmbild](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a>Bevilja åtkomst
toogrant åtkomst när du har identifierat hello roll som du vill tooassign kan använda:

    azure role assignment create

### <a name="assign-a-role-toogroup-at-hello-subscription-scope"></a>Tilldela en roll toogroup hello prenumeration definitionsområdet
tooassign en roll tooa grupp definitionsområdet hello prenumeration, Använd:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

hello följande exempel tilldelas hello *Reader* roll för*Christine Koch Team* på hello *prenumeration* omfång.

![RBAC Azure kommandoraden - azure rolltilldelning Skapa grupp – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a>Tilldela en roll tooan programmet hello prenumeration definitionsområdet
tooassign ett program med rollen tooan definitionsområdet hello prenumeration, Använd:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

hello följande exempel beviljas hello *deltagare* rollen tooan *Azure AD* programmet på hello valda prenumerationen.

 ![Kommandoraden för RBAC Azure - azure rolltilldelning skapa av program](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a>Tilldela en användare med rollen tooa på hello resurs Gruppomfång
tooassign en användare med rollen tooa på hello resurs Gruppomfång, Använd:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

hello följande exempel beviljas hello *Virtual Machine-deltagare* roll för *samert@aaddemo.com*  användaren vid hello *Pharma-försäljning-ProjectForcast* resurs Gruppomfång.

![Kommandoraden för RBAC Azure - azure rolltilldelning skapa av användare – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a>Tilldela en roll tooa grupp hello resurs definitionsområdet
tooassign en roll tooa grupp definitionsområdet hello resurs, Använd:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

hello följande exempel beviljas hello *Virtual Machine-deltagare* rollen tooan *Azure AD* på en *undernät*.

![RBAC Azure kommandoraden - azure rolltilldelning Skapa grupp – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a>Ta bort åtkomst
Använd tooremove en rolltilldelning:

    azure role assignment delete --objectId <object id toofrom which tooremove role> --roleName "<role name>"

hello följande exempel tar bort hello *Virtual Machine-deltagare* rolltilldelningen från hello  *sammert@aaddemo.com*  användare på hello *Pharma-försäljning-ProjectForcast* resursgrupp.
tar bort hello exempel hello rolltilldelning från en grupp i hello prenumeration.

![Azure RBAC kommandoraden - azure tilldelning Rollborttagning – skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Skapa en anpassad roll
Använd toocreate en anpassad roll:

    azure role create --inputfile <file path>

hello följande exempel skapar en anpassad roll som kallas *virtuella operatorn*. Den här anpassade rollen ger åtkomst tooall läsåtgärder av *Microsoft.Compute*, *Microsoft.Storage*, och *Microsoft.Network* resurs providers och beviljar åtkomst toostart, starta om och övervaka virtuella datorer. Den här anpassade rollen kan användas i två prenumerationer. Det här exemplet används en JSON-fil som indata.

![JSON - anpassad rolldefinition – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure kommandoraden - azure rollen skapa – skärmbild](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Ändra en anpassad roll
toomodify en anpassad roll först använda hello `azure role show` kommandot tooretrieve rolldefinitionen. Därefter kontrollera definitionsfilen för hello ändringarna toohello roll. Använd slutligen `azure role set` toosave hello ändrade rolldefinitionen.

    azure role set --inputfile <file path>

hello följande exempel läggs hello *Microsoft.Insights/diagnosticSettings/* åtgärden toohello **åtgärder**, och en Azure-prenumeration toohello **AssignableScopes**av hello virtuella anpassade operatörsrollen.

![JSON - ändra anpassad rolldefinition – skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Azure RBAC kommandoraden - azure rollen set - skärmbild](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Ta bort en anpassad roll
toodelete en anpassad roll först använda hello `azure role show` kommandot toodetermine hello **ID** hello-rollen. Använd sedan hello `azure role delete` kommandot toodelete hello roll genom att ange hello **ID**.

hello följande exempel tar bort hello *virtuella operatorn* anpassad roll.

![Azure RBAC kommandoraden - azure Rollborttagning – skärmbild](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Lista över anpassade roller
toolist hello roller som är tillgängliga för tilldelning på scopenivå, använder hello `azure role list` kommando.

hello följande kommando visar alla roller som är tillgängliga för tilldelning i hello valda prenumerationen.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Azure RBAC kommandoraden - azure rollen list - skärmbild](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

I följande exempel hello, hello *virtuella operatorn* anpassad roll är inte tillgänglig i hello *Production4* prenumerationen eftersom den prenumerationen finns inte i hello  **AssignableScopes** hello-rollen.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Azure RBAC kommandoraden - azure rollen listan för anpassade roller – skärmbild](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

