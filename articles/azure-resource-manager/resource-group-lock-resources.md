---
title: "aaaLock Azure-resurser tooprevent ändringar | Microsoft Docs"
description: "Förhindra användare från att uppdatera eller ta bort viktiga Azure-resurser genom att använda ett lås för alla användare och roller."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 1f0d8911b2b129069bd2f01a9a4ec0a3cf700118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="lock-resources-tooprevent-unexpected-changes"></a>Låsa resurser tooprevent oväntade ändringar 
Som administratör kan behöva du toolock en prenumeration, resursgrupp eller resurs tooprevent andra användare i din organisation av misstag tas bort eller ändra viktiga resurser. Du kan ange hello Lås nivå för**CanNotDelete** eller **ReadOnly**. 

* **CanNotDelete** innebär behöriga användare kan läsa och ändra en resurs fortfarande, men de kan inte ta bort hello resurs. 
* **ReadOnly** innebär att behöriga användare kan läsa en resurs, men de kan inte ta bort eller uppdatera hello resursen. Tillämpa den här Lås är liknande toorestricting alla behöriga användare toohello behörigheterna av hello **Reader** roll. 

## <a name="how-locks-are-applied"></a>Hur Lås tillämpas

När du använder ett lås på en överordnad omfattning, alla resurser inom detta scope ärver hello samma Lås. Även resurser som du senare lägger till Ärv hello överordnade hello Lås. hello mest restriktiva Lås i hello arv företräde.

Till skillnad från rollbaserad åtkomstkontroll använder du management Lås tooapply en begränsning för alla användare och roller. toolearn om att ange behörigheter för användare och roller, se [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).

Hanteraren för filserverresurser Lås gäller toooperations som sker i plan för hello, som består av åtgärder som skickas för`https://management.azure.com`. hello Lås begränsar inte hur resurser utföra sina egna funktioner. Resursändringar är begränsad, men resursåtgärder är inte begränsade. Till exempel en ReadOnly-lås på en SQL-databas hindrar dig från att ta bort eller ändra hello-databasen, men det hindrar dig inte från att skapa, uppdatera eller ta bort data i hello-databas. Data transaktioner tillåts eftersom dessa åtgärder inte skickas för`https://management.azure.com`.

Tillämpa **ReadOnly** kan leda till toounexpected resultat eftersom vissa åtgärder som ser ut som att läsa operations faktiskt kräver ytterligare åtgärder. Till exempel placera en **ReadOnly** lås på ett lagringskonto som förhindrar att alla användare med hello nycklar. hello lista nycklar operationen hanteras via en POST-begäran eftersom hello returnerade nycklar är tillgängliga för skrivåtgärder. För ett annat exempel är att placera en **ReadOnly** lås på en App Service-resurs som förhindrar att Visual Studio Server Explorer visar filer för hello resursen eftersom den interaktionen kräver skrivåtkomst.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Vem kan skapa eller ta bort lås i din organisation
toocreate eller ta bort lås för hantering, måste du ha tillgång för`Microsoft.Authorization/*` eller `Microsoft.Authorization/locks/*` åtgärder. I hello inbyggda roller, endast **ägare** och **administratör för användaråtkomst** beviljas dessa åtgärder.

## <a name="portal"></a>Portalen
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Mall
hello visas följande exempel en mall som skapar ett lås på ett lagringskonto. hello storage-konto på vilka tooapply hello Lås har angetts som en parameter. hello namnet på hello Lås skapas genom att sammanbinda hello resursnamnet med **/Microsoft.Authorization/** och hello namnet på hello Lås i det här fallet **myLock**.

hello-typ som är specifika toohello resurstyp. (Ange hello typen too"Microsoft.Storage/storageaccounts/providers/locks för lagring”,.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a>PowerShell
Du Lås distribueras resurser med Azure PowerShell med hjälp av hello [ny AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) kommando.

toolock en resurs, ange hello namnet på hello resursen och dess resurstypen dess resursgruppens namn.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

toolock en resursgrupp, ange hello namn hello resursgruppen.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

tooget information om ett lås används [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock). tooget alla hello Lås i din prenumeration, Använd:

```powershell
Get-AzureRmResourceLock
```

tooget alla låsningar för en resurs, Använd:

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

tooget alla Lås för en resursgrupp, Använd:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Azure PowerShell innehåller andra kommandon för att arbeta lås som [Set AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) tooupdate ett lås och [ta bort AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) toodelete ett lås.

## <a name="azure-cli"></a>Azure CLI

Du Lås distribueras resurser med Azure CLI med hjälp av hello [az Lås skapa](/cli/azure/lock#create) kommando.

toolock en resurs, ange hello namnet på hello resursen och dess resurstypen dess resursgruppens namn.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

toolock en resursgrupp, ange hello namn hello resursgruppen.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

tooget information om ett lås används [az Lås listan](/cli/azure/lock#list). tooget alla hello Lås i din prenumeration, Använd:

```azurecli
az lock list
```

tooget alla låsningar för en resurs, Använd:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

tooget alla Lås för en resursgrupp, Använd:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Azure CLI finns andra kommandon för att arbeta lås som [az Lås uppdatering](/cli/azure/lock#update) tooupdate ett lås och [az Lås ta bort](/cli/azure/lock#delete) toodelete ett lås.

## <a name="rest-api"></a>REST API
Du kan låsa distribuerade resurser med hello [REST API för hantering av Lås](https://docs.microsoft.com/rest/api/resources/managementlocks). hello REST-API kan du toocreate och ta bort lås och hämta information om befintliga Lås.

toocreate ett lås som kör:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

hello scope kan vara en prenumeration, resursgrupp eller resurs. hello lock-namn är vad du vill toocall hello Lås. Api-versionen, Använd **2015-01-01**.

Inkludera en JSON-objekt som anger hello egenskaperna för hello Lås i hello begäran.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Nästa steg
* Mer information om hur du arbetar med resurslås finns [Lås ned din Azure-resurser](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* toolearn logiskt organisera dina resurser, se [med hjälp av taggar tooorganize dina resurser](resource-group-using-tags.md)
* toochange vilken resursgrupp som en resurs finns i, se [flytta resurser toonew resursgruppen.](resource-group-move-resources.md)
* Du kan tillämpa begränsningar och konventioner över din prenumeration med anpassade principer. Mer information finns i [använda princip toomanage resurser och kontrollera åtkomst](resource-manager-policy.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

