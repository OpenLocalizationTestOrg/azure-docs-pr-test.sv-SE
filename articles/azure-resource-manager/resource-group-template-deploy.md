---
title: aaaDeploy resurser med PowerShell och mall | Microsoft Docs
description: "Använd Azure Resource Manager och Azure PowerShell toodeploy en tooAzure resurser. hello resurser har definierats i en Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 41506811ba3c2ea5df6313db70978ade50f71161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Distribuera resurser med Resource Manager-mallar och Azure PowerShell

Det här avsnittet beskrivs hur toouse Azure PowerShell med Resource Manager mallar toodeploy tooAzure dina resurser. Om du inte är bekant med hello begrepp för att distribuera och hantera dina Azure lösningar finns [översikt över Azure Resource Manager](resource-group-overview.md).

hello Resource Manager-mall som du distribuerar kan antingen vara en lokal fil på din dator eller en extern fil som finns i en databas som GitHub. hello-mallen som du distribuerar i den här artikeln är tillgänglig i hello [exempelmall](#sample-template) avsnitt, eller som [lagring mall i GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>Distribuera en mall från den lokala datorn

När du distribuerar resurser tooAzure du:

1. Logga in tooyour Azure-konto
2. Skapa en resursgrupp som fungerar som behållare för hello för hello distribuerade resurser. hello namnet på hello resursgruppen får bara innehålla alfanumeriska tecken, punkter, understreck, bindestreck och parenteser. Det kan vara upp too90 tecken. Den kan inte sluta med en punkt.
3. Distribuera toohello resurs grupp hello-mallen som definierar hello resurser toocreate

En mall kan innehålla parametrar som möjliggör toocustomize hello distribution. Exempelvis kan du ange värden som är anpassade för en viss miljö (t.ex dev, test- och). hello exempelmall definierar en parameter för hello lagringskontot SKU.

hello följande exempel skapar en resursgrupp och distribuerar en mall från den lokala datorn:

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

hello distributionen kan ta några minuter toocomplete. När den är klar visas ett meddelande som innehåller hello resultat:

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a>Distribuera en mall från en extern källa

Istället för att lagra Resource Manager-mallar på din lokala dator kanske du hellre toostore dem i en extern plats. Du kan lagra mallar i en källkontroll (till exempel GitHub). Eller, du kan lagra dem i ett Azure storage-konto för delad åtkomst i din organisation.

toodeploy en extern mallen använder hello **TemplateUri** parameter. Använd hello URI i hello exempel toodeploy hello exempel mallen från GitHub.

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

hello kräver exemplet ovan en offentligt tillgänglig URI för hello mallen, som fungerar i de flesta scenarier eftersom mallen inte innehålla känsliga data. Om du behöver toospecify känsliga data (till exempel en adminlösenord) kan skicka värdet som en säker parameter. Om du inte vill att din mall toobe offentligt tillgänglig, kan du dock skydda det genom att lagra det i en behållare för privat lagring. Information om hur du distribuerar en mall som kräver en signatur (SAS) token för delad åtkomst finns i [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).

## <a name="parameter-files"></a>Parametern-filer

I stället för att skicka parametrar som infogade värden i skriptet kan du enklare toouse en JSON-fil som innehåller hello parametervärden. hello parameterfil måste vara i hello följande format:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Observera att hello parametrar avsnittet innehåller ett parameternamn som matchar hello-parameter som definierats i mallen (storageAccountType). hello parameterfilen innehåller ett värde för parametern hello. Det här värdet skickas automatiskt toohello mallen under distributionen. Du kan skapa flera parametern filer för olika distributionsscenarier och sedan ange hello lämpliga parameterfil. 

Kopiera hello föregående exempel och spara den som en fil med namnet `storage.parameters.json`.

toopass en lokal parameterfil använda hello **TemplateParameterFile** parameter:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

toopass externa parameterfilen använda hello **TemplateParameterUri** parameter:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

Du kan använda infogade parametrar och en lokal parameter filen i hello samma distributionsåtgärder. Du kan till exempel ange vissa värden i hello lokala parameterfilen och lägga till andra värden infogade under distributionen. Om du anger värden för en parameter i både hello lokala parameterfilen och infogade företräde hello infogade värdet.

Men när du använder en extern parameterfil, du kan inte skicka andra värden antingen infogade eller från en lokal fil. När du anger en parameterfil i hello **TemplateParameterUri** parametern, alla infogade parametrar kommer att ignoreras. Ange alla värden i hello extern fil. Om mallen innehåller något känsligt värde som du inte kan ingå i hello parameterfil, lägger du till att värdet tooa nyckelvalv eller dynamiskt tillhandahåller alla parametern värden infogad.

Om mallen innehåller en parameter med samma namn som en hello parametrar i hello PowerShell-kommando hello, PowerShell visar hello parametern från din mall med hello username@Domain **från mall**. Till exempel en parameter med namnet **ResourceGroupName** i din mall står i konflikt med hello **ResourceGroupName** parameter i hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)cmdlet. Du är tooprovide ange ett värde för **ResourceGroupNameFromTemplate**. I allmänhet bör du undvika den här förvirring genom att inte namnges parametrar med samma namn som parametrar som används för distributionsåtgärder hello.

## <a name="test-a-template-deployment"></a>Testa en för malldistribution

tootest din mall och parametern värden utan att faktiskt distribuera några resurser använder [Test AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment). 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Om inga fel identifieras hello-kommandot har slutförts utan ett svar. Om ett fel upptäcks returnerar hello kommando ett felmeddelande. Till exempel returnerar försöker toopass ett felaktigt värde för hello lagringskontot SKU, hello följande fel:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'hello provided value 'badSku' for hello template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. hello parameter value is not part of hello allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Om mallen har ett syntaxfel, returnerar hello kommando ett felmeddelande om att det inte kunde tolka hello mallen. hello-meddelande visar hello radnummer och positionen för hello Tolkningsfel.

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

toouse fullständig läge, Använd hello `Mode` parameter:

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a>Exempelmall

hello används följande mall för hello exemplen i det här avsnittet. Kopiera och spara den som en fil med namnet storage.json. toounderstand hur toocreate den här mallen finns [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a>Nästa steg
* hello exemplen i den här artikeln distribuera resurser tooa resursgrupp i prenumerationen standard. toouse en annan prenumeration finns [hantera Azure-prenumerationer](/powershell/azure/manage-subscriptions-azureps).
* En fullständig exempelskript som distribuerar en mall finns i [distributionsskriptet för Resource Manager-mallen](resource-manager-samples-powershell-deploy.md).
* hur toodefine parametrarna i mallen, se toounderstand [förstå hello struktur och syntaxen för Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

