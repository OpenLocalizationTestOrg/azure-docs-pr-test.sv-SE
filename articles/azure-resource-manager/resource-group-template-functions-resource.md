---
title: "Mallfunktioner för aaaAzure Resource Manager - resurser | Microsoft Docs"
description: "Beskriver hello funktioner toouse i en Azure Resource Manager mallen tooretrieve värden om resurser."
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
ms.openlocfilehash: c9d524b338b8b7ea6d8c9e0135d48e4fb8f167c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Resursfunktioner för Azure Resource Manager-mallar

Resource Manager tillhandahåller hello följande funktioner för att hämta resurs värden:

* [listKeys och lista {Value}](#listkeys)
* [providers](#providers)
* [referens](#reference)
* [resourceGroup](#resourcegroup)
* [resurs-ID](#resourceid)
* [prenumeration](#subscription)

tooget värden från parametrar, variabler eller hello aktuella distribution, se [distribution värdet funktioner](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a>listKeys och lista {Value}
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Returnerar hello värden för någon resurstyp av som har stöd för hello listan igen. hello vanligaste användningen är `listKeys`. 

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| resourceName eller resourceIdentifier |Ja |Sträng |Unik identifierare för hello resurs. |
| apiVersion |Ja |Sträng |API-versionen av resursen runtime-tillståndet. Normalt i hello-format, **åååå-mm-dd**. |

### <a name="return-value"></a>Returvärde

hello returnerade objekt från listKeys har hello följande format:

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

Andra listfunktioner har olika returnerade format. toosee hello format för en funktion, inkludera den i hello utdata avsnittet som visas i hello exempelmall. 

### <a name="remarks"></a>Kommentarer

Alla åtgärder som börjar med **lista** kan användas som en funktion i mallen. hello tillgängliga åtgärder innehåller inte bara listKeys, men även åtgärder som `list`, `listAdminKeys`, och `listStatus`. Du kan dock använda **lista** åtgärder som kräver att värden i hello begäran. Till exempel hello [lista kontots SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) åtgärden kräver begärantext som *signedExpiry*, så du inte kan använda den i en mall.

toodetermine vilka resurstyper har en liståtgärd, har du hello följande alternativ:

* Visa hello [REST-API: et](/rest/api/) för resursprovidern och leta efter Liståtgärder. Till exempel storage-konton har hello [listKeys åtgärden](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Använd hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-cmdlet. hello hämtas följande exempel alla Liståtgärder för storage-konton:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Använd följande Azure CLI kommando toofilter endast hello Liståtgärder hello:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Ange hello resursen med hjälp av antingen hello [resourceId funktionen](#resourceid), eller hello format `{providerNamespace}/{resourceType}/{resourceName}`.


### <a name="example"></a>Exempel

hello följande exempel visas hur tooreturn hello primära och sekundära nycklarna från ett lagringskonto i hello matar ut avsnitt.

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

Returnerar information om en resursprovider och dess resurstyper som stöds. Om du inte anger en resurstyp returneras hello alla typer av hello stöds för hello resursprovidern.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| providerNamespace |Ja |Sträng |Namespace hello-provider |
| resourceType |Nej |Sträng |hello typ av resurs i hello angetts namnområde. |

### <a name="return-value"></a>Returvärde

Varje typ som stöds returneras i hello följande format: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Matrisen sorteringen av hello returnerade värden inte är säkert.

### <a name="example"></a>Exempel

hello som följande exempel visar hur toouse hello providern funktionen:

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

hello föregående exempel returnerar ett objekt i hello följande format:

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
| apiVersion |Nej |Sträng |API-versionen av hello angivna resursen. Inkludera den här parametern när hello resursen inte har etablerats inom samma mall. Normalt i hello-format, **åååå-mm-dd**. |

### <a name="return-value"></a>Returvärde

Varje resurstypen returnerar andra egenskaper för hello referens-funktionen. hello funktionen returnerar inte ett enda fördefinierade format. toosee hello egenskaper för en resurstyp returnerar hello objekt i hello matar ut avsnittet enligt hello exempel.

### <a name="remarks"></a>Kommentarer

hello referens funktionen hämtar sitt värde från en runtime-tillståndet och kan därför inte användas i hello variabler avsnitt. Det kan användas i utdata avsnitt i en mall. 

Med funktionen hello referens deklarera du implicit att en resurs beror på en annan resurs om hello refererar till resursen har etablerats i samma mall. Du behöver inte tooalso Använd hello dependsOn-egenskapen. hello utvärderas funktionen inte förrän hello refererade resursen har slutfört distributionen.

Skapa en mall som hämtar hello objekt i hello utdata toosee hello egenskapsnamn och värden för en resurstyp. Om du har en befintlig resurs av den typen returnerar mallen hello-objekt utan att distribuera nya resurser. 

Normalt använder du hello **referens** fungerar tooreturn ett visst värde från ett objekt, t.ex slutpunkt för blob-hello URI eller fullständigt domännamn.

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

referens- och toodeploy hello-resurs i hello samma mall, Använd:

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

hello föregående exempel returnerar ett objekt i hello följande format:

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

hello följande exempel refererar till ett lagringskonto som inte har distribuerats i den här mallen. hello storage-konto finns redan i hello samma resursgrupp.

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

Returnerar ett objekt som representerar hello aktuella resursgruppen. 

### <a name="return-value"></a>Returvärde

hello returnerade objektet är i hello följande format:

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

Ett vanligt användningsområde för hello resourceGroup funktionen är toocreate resurser i hello samma plats som hello resursgrupp. hello följande exempel används hello resursgruppens plats tooassign hello plats för en webbplats.

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

hello returnerar följande mall hello hello resursgruppens egenskaper.

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

hello föregående exempel returnerar ett objekt i hello följande format:

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

Returnerar hello Unik identifierare för en resurs. Du använder den här funktionen när hello resursnamnet är tvetydigt eller inte etablerade inom hello samma mall. 

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nej |sträng (i GUID-format) |Standardvärdet är hello aktuell prenumeration. Ange det här värdet när du behöver tooretrieve en resurs i en annan prenumeration. |
| resourceGroupName |Nej |Sträng |Standardvärdet är aktuella resursgruppen. Ange det här värdet när du behöver tooretrieve en resurs i en annan resursgrupp. |
| resourceType |Ja |Sträng |Typ av resurs inklusive resursleverantörens namnrymd. |
| resourceName1 |Ja |Sträng |Namnet på resursen. |
| resourceName2 |Nej |Sträng |Nästa resurs namn segment om resursen är kapslad. |

### <a name="return-value"></a>Returvärde

hello identifierare returneras i hello följande format:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Kommentarer

hello parametervärden som du anger beror på om hello resursen är i hello samma prenumeration och resurs grupp som hello aktuella distributionen.

tooget hello resurs-ID för ett lagringskonto i hello samma prenumeration och resursgrupp, Använd:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

tooget hello resurs-ID för ett lagringskonto i hello samma prenumeration, men en annan resursgrupp, Använd:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

tooget hello resurs-ID för ett lagringskonto i en annan prenumeration och resursgrupp, Använd:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Använd tooget hello resurs-ID för en databas i en annan resursgrupp:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Ofta måste toouse den här funktionen när du använder ett lagringskonto eller ett virtuellt nätverk i en annan resursgrupp. hello följande exempel visas hur en resurs från en extern resursgrupp enkelt kan användas:

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

hello följande exempel returnerar hello resurs-ID för ett lagringskonto i hello resursgrupp:

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

hello utdata från hello föregående exempel med hello standardvärden är:

| Namn | Typ | Värde |
| ---- | ---- | ----- |
| sameRGOutput | Sträng | /subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Sträng | /subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Sträng | /subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Sträng | /subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/ServerName/Databases/databaseName |

<a id="subscription" />

## <a name="subscription"></a>prenumeration
`subscription()`

Returnerar information om hello prenumerationen för hello aktuella distributionen. 

### <a name="return-value"></a>Returvärde

hello funktionen returnerar hello följande format:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Exempel

hello visar följande exempel hello prenumeration funktionen anropas under hello utdata. 

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
* En beskrivning av hello avsnitt i en Azure Resource Manager-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* flera mallar finns i toomerge [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* tooiterate ett angivet antal gånger när du skapar en typ av resurs finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).
* toosee hur toodeploy hello mallen som du har skapat, finns i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

