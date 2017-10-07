---
title: aaaDeploy resurser med Azure CLI och mall | Microsoft Docs
description: "Använd Azure Resource Manager och Azure CLI toodeploy en tooAzure resurser. hello resurser har definierats i en Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 9f8bb9a8720399390a407030d2d32bcd97d32f13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Distribuera resurser med Resource Manager-mallar och Azure CLI

Det här avsnittet beskrivs hur toouse Azure CLI 2.0 med Resource Manager mallar toodeploy tooAzure dina resurser. Om du inte är bekant med hello begrepp för att distribuera och hantera dina Azure lösningar finns [översikt över Azure Resource Manager](resource-group-overview.md).  

hello Resource Manager-mall som du distribuerar kan antingen vara en lokal fil på din dator eller en extern fil som finns i en databas som GitHub. hello-mallen som du distribuerar i den här artikeln är tillgänglig i hello [exempelmall](#sample-template) avsnitt, eller som en [lagring mall i GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Om du inte har Azure CLI är installerad kan du använda hello [moln Shell](#deploy-template-from-cloud-shell).

## <a name="deploy-local-template"></a>Distribuera lokala mall

När du distribuerar resurser tooAzure du:

1. Logga in tooyour Azure-konto
2. Skapa en resursgrupp som fungerar som behållare för hello för hello distribuerade resurser. hello namnet på hello resursgruppen får bara innehålla alfanumeriska tecken, punkter, understreck, bindestreck och parenteser. Det kan vara upp too90 tecken. Den kan inte sluta med en punkt.
3. Distribuera toohello resurs grupp hello-mallen som definierar hello resurser toocreate

En mall kan innehålla parametrar som möjliggör toocustomize hello distribution. Exempelvis kan du ange värden som är anpassade för en viss miljö (t.ex dev, test- och). hello exempelmall definierar en parameter för hello lagringskontot SKU. 

hello följande exempel skapar en resursgrupp och distribuerar en mall från den lokala datorn:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

hello distributionen kan ta några minuter toocomplete. När den är klar visas ett meddelande som innehåller hello resultat:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Distribuera externa mall

Istället för att lagra Resource Manager-mallar på din lokala dator kanske du hellre toostore dem i en extern plats. Du kan lagra mallar i en källkontroll (till exempel GitHub). Eller, du kan lagra dem i ett Azure storage-konto för delad åtkomst i din organisation.

toodeploy en extern mallen använder hello **mall-uri** parameter. Använd hello URI i hello exempel toodeploy hello exempel mallen från GitHub.
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

hello kräver exemplet ovan en offentligt tillgänglig URI för hello mallen, som fungerar i de flesta scenarier eftersom mallen inte innehålla känsliga data. Om du behöver toospecify känsliga data (till exempel en adminlösenord) kan skicka värdet som en säker parameter. Om du inte vill att din mall toobe offentligt tillgänglig, kan du dock skydda det genom att lagra det i en behållare för privat lagring. Information om hur du distribuerar en mall som kräver en signatur (SAS) token för delad åtkomst finns i [distribuera privata mallar med SAS-token](resource-manager-cli-sas-token.md).

## <a name="deploy-template-from-cloud-shell"></a>Distribuera mallen från Cloud Shell

Du kan använda [moln Shell](../cloud-shell/overview.md) toorun hello Azure CLI-kommandon för att distribuera mallen. Men du måste först läsa in mallen i hello filresurs för moln-gränssnittet. Om du inte har använt Cloud Shell tidigare läser du [Overview of Azure Cloud Shell](../cloud-shell/overview.md) (Översikt över Azure Cloud Shell), som innehåller information om hur du konfigurerar Cloud Shell.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).   

2. Välj din Cloud Shell-resursgrupp. hello namnmönstret är `cloud-shell-storage-<region>`.

   ![Välj resursgrupp](./media/resource-group-template-deploy-cli/select-cs-resource-group.png)

3. Välj hello storage-konto för moln-gränssnittet.

   ![Välj lagringskonto](./media/resource-group-template-deploy-cli/select-storage.png)

4. Välj **Filer**.

   ![Välj filer](./media/resource-group-template-deploy-cli/select-files.png)

5. Välj hello filresurs för molnet Shell. hello namnmönstret är `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Välj filresurs](./media/resource-group-template-deploy-cli/select-file-share.png)

6. Välj **Lägg till katalog**.

   ![Lägg till katalog](./media/resource-group-template-deploy-cli/select-add-directory.png)

7. Ge den namnet **templates** och välj **OK**.

   ![Namnge katalogen](./media/resource-group-template-deploy-cli/name-templates.png)

8. Välj den nya katalogen.

   ![Välj katalog](./media/resource-group-template-deploy-cli/select-templates.png)

9. Välj **Överför**.

   ![Välj Överför](./media/resource-group-template-deploy-cli/select-upload.png)

10. Leta upp och överför mallen.

   ![Ladda upp filen](./media/resource-group-template-deploy-cli/upload-files.png)

11. Öppna hello-prompt.

   ![Öppna Cloud Shell](./media/resource-group-template-deploy-cli/start-cloud-shell.png)

12. Ange följande kommandon i hello molnet Shell hello:

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

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

toopass en lokal parameterfil använda `@` toospecify en lokal fil med namnet storage.parameters.json.

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>Testa en för malldistribution

tootest din mall och parametern värden utan att faktiskt distribuera några resurser använder [az distribution Validera](/cli/azure/group/deployment#validate). 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

Om inga fel identifieras returnerar hello kommando information om hello Testa distributionen. Observera att hello särskilt **fel** värdet är null.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Om ett fel upptäcks returnerar hello kommando ett felmeddelande. Till exempel returnerar försöker toopass ett felaktigt värde för hello lagringskontot SKU, hello följande fel:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'hello provided value 'badSKU' for hello template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. hello parameter value is not part of hello allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Om mallen har ett syntaxfel, returnerar hello kommando ett felmeddelande om att det inte kunde tolka hello mallen. hello-meddelande visar hello radnummer och positionen för hello Tolkningsfel.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

toouse fullständig läge, Använd hello `mode` parameter:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
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
* hello exemplen i den här artikeln distribuera resurser tooa resursgrupp i prenumerationen standard. toouse en annan prenumeration finns [hantera Azure-prenumerationer](/cli/azure/manage-azure-subscriptions-azure-cli).
* En fullständig exempelskript som distribuerar en mall finns i [distributionsskriptet för Resource Manager-mallen](resource-manager-samples-cli-deploy.md).
* hur toodefine parametrarna i mallen, se toounderstand [förstå hello struktur och syntaxen för Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-cli-sas-token.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).
