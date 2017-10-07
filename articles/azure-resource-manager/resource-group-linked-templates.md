---
title: "aaaLink mallar för Azure-distribution | Microsoft Docs"
description: "Beskriver hur länkade toouse mallar i en Azure Resource Manager-mall toocreate en modulär mall-lösning. Visar hur toopass parametervärdena, ange en parameterfil och skapas dynamiskt URL: er."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: b935b1810db5ce894d009403cd4bb945cab34ba7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Använda länkade mallar när du distribuerar Azure-resurser
Från inom en Azure Resource Manager-mall kan du länka tooanother mall, vilket gör att du toodecompose distributionen till en uppsättning riktade mallar för specifika ändamål. Precis som med decomposing ett program i flera klasser som koden har uppdelning fördelar vad gäller testning, återanvändning och läsbarhet.  

Du kan skicka parametrar från en Huvudmall tooa länkade mall och de här parametrarna kan direkt mappa tooparameters eller variabler som exponeras av hello anropar mallen. hello länkad mall kan också skicka ett utdata variabeln tillbaka toohello källmallen, aktivera en dubbelriktad datautbyte mellan mallar.

## <a name="linking-tooa-template"></a>Länka tooa mall
Du kan skapa en länk mellan två mallar genom att lägga till en distributionsresurs i hello Huvudmall punkter toohello länkade mallen. Du ställer in hello **templateLink** egenskapen toohello hello länkad mall-URI. Du kan ange parametervärden för hello länkad mall direkt i din mall eller en parameterfil. hello följande exempel används hello **parametrar** egenskapen toospecify ett parametervärde direkt.

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

Du kan ange beroenden mellan hello länkad mall och andra resurser som andra typer av resurser. Därför när andra resurser kräver ett utdatavärde från hello länkade mall kan kan du kontrollera hello länkade mallen distribueras före. Eller när hello länkade mallen är beroende av andra resurser, kan du se till andra resurser har distribuerats före hello länkade mallen. Du kan hämta ett värde från en länkad mall med hello följande syntax:

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

hello Resource Manager-tjänsten måste vara kan tooaccess hello länkad mall. Du kan inte ange en lokal fil eller en fil som endast är tillgängliga i det lokala nätverket för hello länkad mall. Du kan endast ange ett URI-värde som innehåller antingen **http** eller **https**. Ett alternativ är tooplace länkade mallen i ett lagringskonto och Använd hello URI för objektet, som visas i följande exempel hello:

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

Även om hello länkad mall måste finnas externt, behöver inte toobe allmänt tillgänglig toohello offentliga. Du kan lägga till din mall tooa privata lagringskonto som är tillgänglig tooonly hello lagringskontoägaren. Sedan kan skapa du en delad åtkomst (SAS)-signaturen token tooenable åtkomst under distributionen. Du lägger till den SAS-token toohello URI för hello länkad mall. Stegvisa instruktioner för hur du konfigurerar en mall i ett lagringskonto och generera en SAS-token finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md) eller [distribuera resurser med Resource Manager-mallar och Azure CLI](resource-group-template-deploy-cli.md). 

hello som följande exempel visar en mall för överordnade länkar tooanother mallen. hello länkad mall används med en SAS-token som skickas som en parameter.

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

Även om hello token som skickas som en säker sträng, loggas hello URI för hello länkade mallen, inklusive hello SAS-token, i hello distributionsåtgärder. toolimit exponering, ange en giltighetstid för hello-token.

Resource Manager hanterar varje länkade mall som en separat distribution. I hello distributionshistoriken för hello resursgrupp ser du separata distributioner för hello överordnade och kapslade mallar.

![distributionshistorik](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-tooa-parameter-file"></a>Länka tooa parameterfilen
hello nästa exempel använder hello **parametersLink** egenskapen toolink tooa parameterfil.

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

hello URI värde hello länkade parameterfilen får inte vara en lokal fil och måste innehålla antingen **http** eller **https**. hello parameterfilen kan också vara begränsad tooaccess via en SAS-token.

## <a name="using-variables-toolink-templates"></a>Med hjälp av variabler toolink mallar
hello föregående exempel visade hårdkodade URL-värden för hello mallen länkar. Den här metoden kan fungera för en enkel mall men det fungerar inte bra när du arbetar med en stor mängd modulära mallar. I stället kan du skapa en statisk variabel som lagrar en bas-URL för hello Huvudmall och dynamiskt skapa URL: er för hello länkade mallar från den grundläggande Webbadressen. hello fördelen med den här metoden är att du enkelt kan flytta eller förgrening hello mall eftersom du behöver bara toochange hello statisk variabel i hello Huvudmall. hello Huvudmall skickar hello rätt URI: er i hela hello nedbruten mallen.

hello följande exempel visas hur toouse en grundläggande URL toocreate två URL: er för länkade mallar (**sharedTemplateUrl** och **vmTemplate**). 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Du kan också använda [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello bas-URL för aktuella mallen för hello och använda tooget hello URL: en för andra mallar i hello samma plats. Den här metoden är användbar om du mallen ändrar placering (kanske på grund av tooversioning) eller om du vill tooavoid hårda kodning URL: er i hello mallfilen. 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a>Fullständigt exempel
hello följande exempel mallar visar en förenklad placering av länkade mallar tooillustrate flera hello begrepp i den här artikeln. Det förutsätts att hello mallar har lagts till toohello behållare i ett lagringskonto med offentlig åtkomst är inaktiverat. hello länkad mall skickar ett värde tillbaka toohello Huvudmall i hello **matar ut** avsnitt.

Hej **parent.json** filen består av:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.result.value]"
    }
  }
}
```

Hej **helloworld.json** filen består av:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
  }
}
```

I PowerShell, hämta en token för hello behållare och distribuera hello mallar med:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

I Azure CLI 2.0, hämta en token för hello behållare och distribuera hello mallar med hello följande kod:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="next-steps"></a>Nästa steg
* toolearn om hello definierar hello distributionsordning för dina resurser, se [definiera beroenden i Azure Resource Manager-mallar](resource-group-define-dependencies.md)
* toolearn hur toodefine en resurs skapa men flera förekomster av det, finns i [och skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md)

