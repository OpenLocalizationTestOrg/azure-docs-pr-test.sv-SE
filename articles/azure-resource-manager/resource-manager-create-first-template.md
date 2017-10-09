---
title: "aaaCreate första Azure Resource Manager-mall | Microsoft Docs"
description: "En stegvis guide toocreating ditt första Azure Resource Manager-mallen. Den visar hur toouse hello Mallreferens för en storage-konto toocreate hello mall."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/27/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.openlocfilehash: 92e6d6bb7094fe0e4537ee080704967862804bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a>Skapa och distribuera din första Azure Resource Manager-mall
Det här avsnittet vägleder dig genom hello stegen för att skapa din första Azure Resource Manager-mall. Resource Manager-mallarna är JSON-filer som definierar hello resurser du behöver toodeploy för din lösning. toounderstand hello begrepp som är associerade med att distribuera och hantera dina Azure lösningar finns [översikt över Azure Resource Manager](resource-group-overview.md). Om du har befintliga resurser och vill tooget en mall för dessa resurser, se [exportera en Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).

toocreate och ändra mallar, behöver du en JSON-redigerare. [Visual Studio Code](https://code.visualstudio.com/) är en enkel, plattformsoberoende kodredigerare med öppen källkod. Vi rekommenderar starkt att du använder Visual Studio Code för att skapa Resource Manager-mallar. Den här artikeln förutsätter att du använder Virtual Studio Code, men om du har en annan JSON-redigerare kan du använda den.

## <a name="prerequisites"></a>Krav

* Visual Studio Code. Om det behövs installerar du det från [https://code.visualstudio.com/](https://code.visualstudio.com/).
* En Azure-prenumeration. Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="create-template"></a>Skapa mallen

Låt oss börja med en enkel mall som distribuerar en tooyour lagringskontoprenumerationen.

1. Välj **Arkiv** > **Ny fil**. 

   ![Ny fil](./media/resource-manager-create-first-template/new-file.png)

2. Kopiera och klistra in hello följande JSON-syntax i filen:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   Lagringskontonamn har flera begränsningar som gör dem svåra tooset. hello namn måste vara mellan 3 och 24 tecken, Använd endast siffror och gemener och vara unika. hello föregående mallen använder hello [uniqueString](resource-group-template-functions-string.md#uniquestring) fungerar toogenerate ett hash-värde. toogive hashvärdet värde mer innebörd, läggs hello prefixet *lagring*. 

3. Spara filen som **azuredeploy.json** tooa lokal mapp.

   ![Spara mallen](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a>Distribuera mallen

Du är klar toodeploy den här mallen. Du kan använda PowerShell eller Azure CLI toocreate en resursgrupp. Sedan distribuerar du en resursgrupp för storage-konto toothat.

* Använd följande kommandon från hello mapp som innehåller hello mall hello för PowerShell:

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* Använd hello följande kommandon från hello mapp som innehåller hello mall för en lokal installation av Azure CLI:

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

När distributionen är klar finns ditt lagringskonto i hello resursgruppen.

## <a name="deploy-template-from-cloud-shell"></a>Distribuera mallen från Cloud Shell

Du kan använda [moln Shell](../cloud-shell/overview.md) toorun hello Azure CLI-kommandon för att distribuera mallen. Men du måste först läsa in mallen i hello filresurs för moln-gränssnittet. Om du inte har använt Cloud Shell tidigare läser du [Overview of Azure Cloud Shell](../cloud-shell/overview.md) (Översikt över Azure Cloud Shell), som innehåller information om hur du konfigurerar Cloud Shell.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).   

2. Välj din Cloud Shell-resursgrupp. hello namnmönstret är `cloud-shell-storage-<region>`.

   ![Välj resursgrupp](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. Välj hello storage-konto för moln-gränssnittet.

   ![Välj lagringskonto](./media/resource-manager-create-first-template/select-storage.png)

4. Välj **Filer**.

   ![Välj filer](./media/resource-manager-create-first-template/select-files.png)

5. Välj hello filresurs för molnet Shell. hello namnmönstret är `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Välj filresurs](./media/resource-manager-create-first-template/select-file-share.png)

6. Välj **Lägg till katalog**.

   ![Lägg till katalog](./media/resource-manager-create-first-template/select-add-directory.png)

7. Ge den namnet **templates** och välj **OK**.

   ![Namnge katalogen](./media/resource-manager-create-first-template/name-templates.png)

8. Välj den nya katalogen.

   ![Välj katalog](./media/resource-manager-create-first-template/select-templates.png)

9. Välj **Överför**.

   ![Välj Överför](./media/resource-manager-create-first-template/select-upload.png)

10. Leta upp och överför mallen.

   ![Ladda upp filen](./media/resource-manager-create-first-template/upload-files.png)

11. Öppna hello-prompt.

   ![Öppna Cloud Shell](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. Ange följande kommandon i hello molnet Shell hello:

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

När distributionen är klar finns ditt lagringskonto i hello resursgruppen.

## <a name="customize-hello-template"></a>Anpassa hello-mall

hello mallen fungerar bra, men det är inte flexibel. Den distribuerar alltid en lokalt redundant lagring tooSouth centrala USA. hello namn är alltid *lagring* följt av ett hash-värde. tooenable använder hello mall för olika scenarier, lägga till parametrar toohello mall.

hello visar följande exempel hello parametrar avsnitt med två parametrar. Hej första parametern `storageSKU` kan du toospecify hello typen av redundans. Det begränsar hello-värden som du kan skicka in toovalues som är giltiga för ett lagringskonto. Den definierar också ett standardvärde. Hej andra parametern `storageNamePrefix` är uppsättningen tooallow Maximalt 11 tecken. Den definierar ett standardvärde.

```json
"parameters": {
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
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

Lägg till en variabel med namnet under hello variabler `storageName`. Det kombinerar hello prefixvärde från hello parametrar och ett hash-värde från hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funktion. Den använder hello [toLower](resource-group-template-functions-string.md#tolower) fungerar tooconvert toolowercase för alla tecken.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

toouse dessa nya värden för ditt lagringskonto, ändra hello resursdefinitionen:

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    "kind": "Storage",
    "location": "[resourceGroup().location]",
    "tags": {},
    "properties": {}
  }
],
```

Observera att hello hello lagring kontonamn anges nu toohello variabel som du har lagt till. hello SKU namn anges toohello hello parameterns värde. hello platsen angetts hello samma plats som hello resursgrupp.

Spara filen. 

När du har slutfört hello stegen i den här artikeln mallen nu ser ut som:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
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
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a>Distribuera om mallen

Omdistribuera hello mallen med olika värden.

Om du använder PowerShell använder du:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

Om du använder Azure CLI använder du:

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

Överför ändrade mallen toohello filresursen för hello molnet Shell. Skriv över befintlig hello-fil. Använd sedan följande kommando hello:

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="clean-up-resources"></a>Rensa resurser

Rensa hello-resurser som du har distribuerat genom att ta bort resursgruppen hello när de inte längre behövs.

Om du använder PowerShell använder du:

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

Om du använder Azure CLI använder du:

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a>Nästa steg
* toolearn mer om hello strukturen i en mall finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* toolearn om hello egenskaper för ett lagringskonto finns [lagringskonton mallreferensen](/azure/templates/microsoft.storage/storageaccounts).
* tooview fullständig mallar för många olika typer av lösningar, se hello [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates/).
