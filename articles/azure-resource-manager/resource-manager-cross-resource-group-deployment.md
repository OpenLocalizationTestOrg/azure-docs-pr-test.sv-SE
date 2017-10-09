---
title: aaaDeploy Azure-resurser toomultiple resursgrupper | Microsoft Docs
description: "Visar hur tootarget mer än en Azure-resurs gruppera under distributionen."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: tomfitz
ms.openlocfilehash: 93a39a26e0ca18dfcb5c6e8de95c38a64186d6de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-resources-toomore-than-one-resource-group"></a>Distribuera Azure-resurser toomore än en resursgrupp

Normalt distribuerar du alla hello resurser i din mall tooa enskild resursgrupp. Det finns emellertid scenarier där du vill toodeploy en uppsättning resurser tillsammans men placera dem i olika resursgrupper. Du kan till exempel vill toodeploy hello säkerhetskopiering virtuella datorn för Azure Site Recovery tooa separat resursgrupp och plats. Resource Manager kan toouse kapslade mallar tootarget olika resursgrupper än hello resursgruppens namn används för hello överordnade mallen.

hello resursgrupp är hello livscykel behållaren för hello programmet och dess mängd resurser. Du skapar hello resursgruppen utanför hello mallen och ange hello resurs grupp tootarget under distributionen. En introduktion tooresource grupper, se [översikt över Azure Resource Manager](resource-group-overview.md).

## <a name="example-template"></a>Exempelmall

tootarget en annan resurs, måste du använda en mall för kapslade eller länkade under distributionen. Hej `Microsoft.Resources/deployments` resurstypen ger en `resourceGroup` parameter som kan användas toospecify en annan resursgrupp för hello kapslade distributionen. Alla resursgrupper för hello måste finnas innan du kör hello-distribution. hello följande exempel distribuerar två lagringskonton - en i hello resursgruppen som anges under distributionen, och en i en resursgrupp med namnet `crossResourceGroupDeployment`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

Om du ställer in `resourceGroup` toohello namnet på en resursgrupp som inte finns, hello distributionen misslyckas. Om du inte anger ett värde för `resourceGroup`, Resource Manager använder hello överordnade resursgruppen.  

## <a name="deploy-hello-template"></a>Distribuera hello mall

toodeploy hello exempel mallen som du kan använda hello-portalen, Azure PowerShell eller Azure CLI. För Azure PowerShell eller Azure CLI, måste du använda en Versionspost från maj 2017 eller senare. hello exempel förutsätter att du har sparat hello mall lokalt som en fil med namnet **crossrgdeployment.json**.

För PowerShell:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

För Azure CLI:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

När distributionen är klar kan du se två resursgrupper. Varje resursgrupp innehåller ett lagringskonto.

## <a name="use-resourcegroup-function"></a>Funktionen resourceGroup()

För mellan distributionen av resursgrupper, hello [resouceGroup() funktionen](resource-group-template-functions-resource.md#resourcegroup) matchar på olika sätt beroende på hur du anger hello kapslade mallen. 

Om du bäddar in en mall i en annan mall löser resouceGroup() i hello kapslade mallen toohello överordnade resursgruppen. Ett inbäddat mallen använder hello följande format:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers tooparent resource group
    }
}
```

Om du länka tooa separat mall löser resouceGroup() i hello länkade mallen toohello kapslade resursgruppen. En länkad mall använder hello följande format:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers toolinked resource group
    }
}
```

## <a name="next-steps"></a>Nästa steg

* hur toodefine parametrarna i mallen, se toounderstand [förstå hello struktur och syntaxen för Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).
