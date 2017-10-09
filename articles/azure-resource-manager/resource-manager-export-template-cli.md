---
title: aaaExport Resource Manager-mall med Azure CLI | Microsoft Docs
description: "Använd Azure Resource Manager och Azure CLI tooexport en mall från en resursgrupp."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 2d44a0a6e9717504d4c2a01254d826679b381f22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a>Exportera Azure Resource Manager-mallar med Azure CLI

Resource Manager kan tooexport Resource Manager-mall från befintliga resurser i din prenumeration. Du kan använda den genererade mallen toolearn om hello mallen syntax eller tooautomate hello Omdistributionen av din lösning efter behov.

Det är viktigt toonote att det finns två olika sätt tooexport en mall:

* Du kan exportera hello faktiska mall som du använde för en distribution. hello exporterade mallen innehåller alla hello parametrar och variabler exakt som de visas i hello ursprungliga mallen. Den här metoden är användbar när du behöver tooretrieve en mall.
* Du kan exportera en mall som representerar hello hello resursgruppen aktuella tillstånd. exporterad mall för hello baseras inte på en mall som du använde för distribution. I stället skapar en mall som är en ögonblicksbild av hello resursgruppen. hello exporterad mall har många hårdkodade värden och förmodligen inte så många parametrar som du vanligtvis definierar. Den här metoden är användbar när du har ändrat hello resursgruppen. Du måste nu toocapture hello resursgruppen som en mall.

Båda metoderna beskrivs i det här avsnittet.

## <a name="deploy-a-solution"></a>Distribuera en lösning

tooillustrate båda närmar sig för att exportera en mall, låt oss börja med att distribuera en lösning tooyour prenumeration. Om du redan har en resursgrupp i din prenumeration som du vill tooexport har du inte toodeploy den här lösningen. Men handlar hello resten av den här artikeln toohello mallen för den här lösningen. hello exempelskriptet distribuerar ett lagringskonto.

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a>Spara mallen från distributionshistoriken

Du kan hämta en mall från distributionshistoriken med hjälp av hello [az distribution exportera](/cli/azure/group/deployment#export) kommando. följande exempel sparar hello mall som du tidigare har distribuerat hello:

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

Den returnerar hello mallen. Kopiera hello JSON och spara som en fil. Observera att det är hello exakt mallen som du använde för distribution. hello parametrar och variabler matchar hello mallen från GitHub. Du kan distribuera den här mallen.


## <a name="export-resource-group-as-template"></a>Exportera resursgrupp som mall

I stället för att hämta en mall från hello distributionshistoriken, kan du hämta en mall som representerar hello aktuell status för en resursgrupp med hjälp av hello [az exportera](/cli/azure/group#export) kommando. Du kan använda det här kommandot när du har gjort många ändringar tooyour resursgruppen och inga befintliga mall representerar alla hello ändringar.

```azurecli
az group export --name ExampleGroup
```

Den returnerar hello mallen. Kopiera hello JSON och spara som en fil. Observera att det är ett annat än hello mallen i GitHub. Det har olika parametrar och inga variabler. hello lagring SKU och platsen är hårdkodat toovalues. hello följande exempel visar hello exporterad mall, men mallen har ett något annorlunda parameternamn:

```json
{
  "parameters": {
    "storageAccounts_mcyzaljiv7qncstandardsa_name": {
      "type": "String",
      "defaultValue": null
    }
  },
  "variables": {},
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "resources": [
    {
      "tags": {},
      "dependsOn": [],
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "type": "Microsoft.Storage/storageAccounts",
      "location": "centralus",
      "name": "[parameters('storageAccounts_mcyzaljiv7qncstandardsa_name')]",
      "properties": {}
    }
  ],
  "contentVersion": "1.0.0.0"
}
```

Du kan distribuera den här mallen, men det krävs ett unikt namn för hello storage-konto för att gissa. hello namnet på parametern är något annorlunda.

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
```

## <a name="customize-exported-template"></a>Anpassa exporterad mall

Du kan ändra den här mallen toomake den toouse enklare och mer flexibelt. tooallow för flera platser, ändra hello plats egenskapen toouse hello samma plats som hello resursgrupp:

```json
"location": "[resourceGroup().location]",
```

tooavoid har tooguess uniques namn för storage-konto, ta bort hello-parametern för hello lagringskontonamn. Lägga till en parameter för ett namnsuffix för lagring och lagring SKU:

```json
"parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
},
```

Lägg till en variabel som konstruktioner hello lagringskontonamnet med hello uniqueString funktionen:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Ange hello namnet på hello storage-konto toohello variabel:

```json
"name": "[variables('storageAccountName')]",
```

Ange hello SKU toohello parametern:

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

Nu ser din mall ut så här:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), parameters('storageSuffix'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "[parameters('storageAccountType')]",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Omdistribuera hello ändrade mallen.

## <a name="next-steps"></a>Nästa steg
* Information om hur du använder hello portal tooexport en mall finns i [exportera en Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).
* toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).
* Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).