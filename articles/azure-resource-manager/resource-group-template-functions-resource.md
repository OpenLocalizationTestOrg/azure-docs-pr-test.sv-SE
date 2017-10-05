---
title: Mallfunktioner Azure Resource Manager - resurser | Microsoft Docs
description: "Beskriver funktionerna du använder i en Azure Resource Manager-mall för att hämta värden om resurser."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2017
ms.author: tomfitz
ms.openlocfilehash: 494ade55f21c19d9c68d5cc52756528401d9bb77
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Resursfunktioner för Azure Resource Manager-mallar

Hanteraren för filserverresurser innehåller följande funktioner för att hämta resurs värden:

* [listKeys och lista {Value}](#listkeys)
* [providers](#providers)
* [referens](#reference)
* [resourceGroup](#resourcegroup)
* [resurs-ID](#resourceid)
* [prenumeration](#subscription)

Om du vill hämta värden från parametrar, variabler eller den aktuella distributionen finns [distribution värdet funktioner](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a>listKeys och lista {Value}
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Returnerar värden för någon resurstyp av som har stöd för listan igen. Den vanligaste användningen är `listKeys`. 

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| resourceName eller resourceIdentifier |Ja |Sträng |Unik identifierare för resursen. |
| apiVersion |Ja |Sträng |API-versionen av resursen runtime-tillståndet. Normalt i formatet, **åååå-mm-dd**. |

### <a name="return-value"></a>Returvärde

Det returnerade objektet från listKeys har följande format:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Andra listfunktioner har olika returnerade format. Om du vill se format för en funktion, inkludera den i avsnittet utdata som visas i exemplet mallen. 

### <a name="remarks"></a>Kommentarer

Alla åtgärder som börjar med **lista** kan användas som en funktion i mallen. De tillgängliga åtgärderna innehåller inte bara listKeys, men även åtgärder som `list`, `listAdminKeys`, och `listStatus`. Du kan dock använda **lista** åtgärder som kräver att värden i begärandetexten. Till exempel den [lista kontots SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) åtgärden kräver begärantext som *signedExpiry*, så du inte kan använda den i en mall.

För att avgöra vilka resurstyper har en liståtgärd har du följande alternativ:

* Visa den [REST-API: et](/rest/api/) för resursprovidern och leta efter Liståtgärder. Till exempel lagringskonton har den [listKeys åtgärden](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Använd den [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-cmdlet. I följande exempel hämtas alla Liståtgärder för storage-konton:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Du kan använda kommandot Azure CLI för att filtrera listan åtgärderna:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Ange resursen med hjälp av antingen den [resourceId funktionen](#resourceid), eller formatet `{providerNamespace}/{resourceType}/{resourceName}`.


### <a name="example"></a>Exempel

I följande exempel visas hur du återställer de primära och sekundära nycklarna från ett lagringskonto i avsnittet utdata.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountId": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "storageKeysOutput": {
            "value": "[listKeys(parameters('storageAccountId'), '2016-01-01')]",
            "type" : "object"
        }
    }
}
``` 

<a id="providers" />

## <a name="providers"></a>providers
`providers(providerNamespace, [resourceType])`

Returnerar information om en resursprovider och dess resurstyper som stöds. Om du inte anger en resurstyp returnerar funktionen typerna som stöds för resursprovidern.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| providerNamespace |Ja |Sträng |Namespace av providern |
| resourceType |Nej |Sträng |Typ av resurs i det angivna namnområdet. |

### <a name="return-value"></a>Returvärde

Varje typ som stöds returneras i följande format: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Matrisen sorteringen av de returnerade värdena är inte säkert.

### <a name="example"></a>Exempel

I följande exempel visas hur du använder funktionen providern:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers('Microsoft.Web', 'sites')]",
            "type" : "object"
        }
    }
}
```

Föregående exempel returnerar ett objekt i följande format:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

<a id="reference" />

## <a name="reference"></a>Referens
`reference(resourceName or resourceIdentifier, [apiVersion])`

Returnerar ett objekt som representerar en resurs runtime-tillståndet.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| resourceName eller resourceIdentifier |Ja |Sträng |Namn eller unik identifierare för en resurs. |
| apiVersion |Nej |Sträng |API-versionen av den angivna resursen. Inkludera den här parametern när resursen inte är etablerad inom samma mall. Normalt i formatet, **åååå-mm-dd**. |

### <a name="return-value"></a>Returvärde

Varje resurstypen returnerar andra egenskaper för funktionen referens. Funktionen returnerar inte ett enda fördefinierade format. Returnera objektet om du vill visa egenskaperna för en resurstyp i avsnittet utdata som visas i exemplet.

### <a name="remarks"></a>Kommentarer

Funktionen referens hämtar sitt värde från en runtime-tillståndet och kan därför inte användas i avsnittet variables. Det kan användas i utdata avsnitt i en mall. 

Med hjälp av funktionen referens deklarera du implicit att en resurs beror på en annan resurs om den refererade resursen etableras inom samma mall. Du behöver inte också använda dependsOn-egenskapen. Funktionen utvärderas inte förrän den refererade resursen har slutfört distributionen.

Skapa en mall som returnerar objektet i avsnittet utdata om du vill se egenskapsnamn och värden för en resurstyp. Om du har en befintlig resurs av den typen returnerar mallen för objektet utan att distribuera nya resurser. 

Normalt använder du den **referens** funktionen för att returnera ett visst värde från ett objekt, till exempel slutpunkt för blob-URI eller fullständigt domännamn.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

### <a name="example"></a>Exempel

Använd följande för att distribuera och referera till resursen i samma mall:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      }
    }
}
``` 

Föregående exempel returnerar ett objekt i följande format:

```json
{
   "creationTime": "2017-06-13T21:24:46.618364Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

I följande exempel refererar till ett lagringskonto som inte har distribuerats i den här mallen. Storage-konto finns redan i samma resursgrupp.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a>resourceGroup
`resourceGroup()`

Returnerar ett objekt som representerar den aktuella resursgruppen. 

### <a name="return-value"></a>Returvärde

Det returnerade objektet är i följande format:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a>Kommentarer

Ett vanligt användningsområde för funktionen resourceGroup är att skapa resurser på samma plats som resursgruppen. I följande exempel används resursgruppens plats tilldelas platsen för en webbplats.

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a>Exempel

Följande mall returnerar egenskaperna för resursgruppen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

Föregående exempel returnerar ett objekt i följande format:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Returnerar den unika identifieraren för en resurs. Du kan använda den här funktionen när resursnamnet är tvetydigt eller inte etablerade inom samma mall. 

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nej |sträng (i GUID-format) |Standardvärdet är den aktuella prenumerationen. Ange det här värdet när du vill hämta en resurs i en annan prenumeration. |
| resourceGroupName |Nej |Sträng |Standardvärdet är aktuella resursgruppen. Ange det här värdet när du vill hämta en resurs i en annan resursgrupp. |
| resourceType |Ja |Sträng |Typ av resurs inklusive resursleverantörens namnrymd. |
| resourceName1 |Ja |Sträng |Namnet på resursen. |
| resourceName2 |Nej |Sträng |Nästa resurs namn segment om resursen är kapslad. |

### <a name="return-value"></a>Returvärde

Identifieraren returneras i följande format:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Kommentarer

De parametervärden som du anger beror på om resursen är i samma prenumeration och resurs grupp som den aktuella distributionen.

För att få resurs-ID för ett lagringskonto i samma prenumeration och resursgrupp, använder du:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

För att få resurs-ID för ett lagringskonto i samma prenumeration, men en annan resursgrupp, använder du:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

För att få resurs-ID för ett lagringskonto i en annan prenumeration och resursgrupp, använder du:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

För att få resurs-ID för en databas i en annan resursgrupp, använder du:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Du behöver ofta, Använd den här funktionen när du använder ett lagringskonto eller ett virtuellt nätverk i en annan resursgrupp. I följande exempel visas hur en resurs från en extern resursgrupp enkelt kan användas:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a>Exempel

I följande exempel returneras resurs-ID för ett lagringskonto i resursgruppen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

Utdata från det föregående exemplet med standardvärdena är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| sameRGOutput | Sträng | /subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Sträng | /subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Sträng | /subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Sträng | /subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/ServerName/Databases/databaseName |

<a id="subscription" />

## <a name="subscription"></a>prenumeration
`subscription()`

Returnerar information om prenumerationen för den aktuella distributionen. 

### <a name="return-value"></a>Returvärde

Funktionen returnerar följande format:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Exempel

I följande exempel visas funktionen prenumeration anropas i avsnittet utdata. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

## <a name="next-steps"></a>Nästa steg
* En beskrivning av avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* Om du vill slå samman flera mallar, se [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* Iterera ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).
* Information om hur du distribuerar mallen som du har skapat finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

