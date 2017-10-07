---
title: aaaDeploy resurser tooAzure | Microsoft Docs
description: "Använda Azure PowerShell eller Azure CLI toodeploy resurser tooAzure. hello resurser har definierats i en Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
ms.openlocfilehash: 0cd3f8ad45af1fb85c78899b56f6807d00b859f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-tooazure"></a>Distribuera resurser tooAzure

Det här avsnittet visar hur toodeploy resurser tooyour Azure-prenumeration. Du kan använda Azure PowerShell eller Azure CLI toodeploy en Resource Manager-mall som definierar hello infrastrukturen för lösningen.

En introduktion tooconcepts av Resource Manager finns [översikt över Azure Resource Manager](resource-group-overview.md).

## <a name="steps-for-deployment"></a>Distributionsanvisningar

Det här avsnittet förutsätter att du distribuerar hello [lagring exempelmall](#example-storage-template) i det här avsnittet. Du kan använda en annan mall, men hello-parametrar som du skickar skiljer sig från vad som anges i det här avsnittet.

När du har skapat en mall är hello allmänna steg för att distribuera mallen

1. Logga in tooyour konto
2. Välj hello prenumeration toouse (krävs endast om du har flera prenumerationer och du vill toouse ett som inte är hello standard prenumeration)
3. Skapa en resursgrupp
4. Distribuera hello mall
5. Kontrollera status för distributionen

hello följande avsnitt visar hur tooperform de steg med [PowerShell](#powershell) eller [Azure CLI](#azure-cli).

## <a name="powershell"></a>PowerShell

1. tooinstall Azure PowerShell finns [Kom igång med Azure PowerShell-cmdlets](/powershell/azure/overview).

2. tooquickly komma igång med distribution, använder hello följande cmdlets:

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  Hej `Set-AzureRmContext` cmdlet behövs bara om du vill toouse en prenumeration än standard-prenumeration. toosee alla prenumerationer och deras ID, Använd:

  ```powershell
  Get-AzureRmSubscription
  ```

3. hello distributionen kan ta några minuter toocomplete. När distributionen är klar visas ett meddelande som ser ut ungefär så här:

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. toosee ditt resurs grupp och storage-konto har distribuerats tooyour prenumeration, Använd:

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. Du kan ange mallparametrar som PowerShell-parametrar när du distribuerar en mall. hello innehöll tidigare exempel inte några mallparametrar så hello standardvärdena i hello mallen användes. toodeploy lagring för ett annat konto och ange parametervärden för hello namnprefix för lagring och hello lagringskonto SKU, använder:

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  Nu har du två lagringskonton i resursgruppen. 

## <a name="azure-cli"></a>Azure CLI

1. tooinstall Azure CLI, se [installera Azure CLI 2.0](/cli/azure/install-az-cli2).

2. tooquickly komma igång med distribution, använder hello följande kommandon:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  Hej `az account set` kommandot behövs bara om du vill toouse en prenumeration än standard-prenumeration. toosee alla prenumerationer och deras ID, Använd:

  ```azurecli
  az account list
  ```

3. hello distributionen kan ta några minuter toocomplete. När distributionen är klar visas ett meddelande som ser ut ungefär så här:

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. toosee ditt resurs grupp och storage-konto har distribuerats tooyour prenumeration, Använd:

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. Du kan ange mallparametrar som PowerShell-parametrar när du distribuerar en mall. hello innehöll tidigare exempel inte några mallparametrar så hello standardvärdena i hello mallen användes. toodeploy lagring för ett annat konto och ange parametervärden för hello namnprefix för lagring och hello lagringskonto SKU, använder:

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  Nu har du två lagringskonton i resursgruppen. 

## <a name="example-storage-template"></a>Exempellagringsmall

Använd följande exempel mallen toodeploy tooyour en lagringskontoprenumerationen hello:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "hello type of replication toouse for hello storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Nästa steg

* Detaljerad information om hur du använder PowerShell toodeploy mallar finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy).
* Detaljerad information om hur du använder Azure CLI toodeploy mallar finns [distribuera resurser med Resource Manager-mallar och Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli).



