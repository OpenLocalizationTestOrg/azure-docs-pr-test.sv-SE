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
# <a name="using-linked-templates-when-deploying-azure-resources"></a><span data-ttu-id="a5633-104">Använda länkade mallar när du distribuerar Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="a5633-104">Using linked templates when deploying Azure resources</span></span>
<span data-ttu-id="a5633-105">Från inom en Azure Resource Manager-mall kan du länka tooanother mall, vilket gör att du toodecompose distributionen till en uppsättning riktade mallar för specifika ändamål.</span><span class="sxs-lookup"><span data-stu-id="a5633-105">From within one Azure Resource Manager template, you can link tooanother template, which enables you toodecompose your deployment into a set of targeted, purpose-specific templates.</span></span> <span data-ttu-id="a5633-106">Precis som med decomposing ett program i flera klasser som koden har uppdelning fördelar vad gäller testning, återanvändning och läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="a5633-106">As with decomposing an application into several code classes, decomposition provides benefits in terms of testing, reuse, and readability.</span></span>  

<span data-ttu-id="a5633-107">Du kan skicka parametrar från en Huvudmall tooa länkade mall och de här parametrarna kan direkt mappa tooparameters eller variabler som exponeras av hello anropar mallen.</span><span class="sxs-lookup"><span data-stu-id="a5633-107">You can pass parameters from a main template tooa linked template, and those parameters can directly map tooparameters or variables exposed by hello calling template.</span></span> <span data-ttu-id="a5633-108">hello länkad mall kan också skicka ett utdata variabeln tillbaka toohello källmallen, aktivera en dubbelriktad datautbyte mellan mallar.</span><span class="sxs-lookup"><span data-stu-id="a5633-108">hello linked template can also pass an output variable back toohello source template, enabling a two-way data exchange between templates.</span></span>

## <a name="linking-tooa-template"></a><span data-ttu-id="a5633-109">Länka tooa mall</span><span class="sxs-lookup"><span data-stu-id="a5633-109">Linking tooa template</span></span>
<span data-ttu-id="a5633-110">Du kan skapa en länk mellan två mallar genom att lägga till en distributionsresurs i hello Huvudmall punkter toohello länkade mallen.</span><span class="sxs-lookup"><span data-stu-id="a5633-110">You create a link between two templates by adding a deployment resource within hello main template that points toohello linked template.</span></span> <span data-ttu-id="a5633-111">Du ställer in hello **templateLink** egenskapen toohello hello länkad mall-URI.</span><span class="sxs-lookup"><span data-stu-id="a5633-111">You set hello **templateLink** property toohello URI of hello linked template.</span></span> <span data-ttu-id="a5633-112">Du kan ange parametervärden för hello länkad mall direkt i din mall eller en parameterfil.</span><span class="sxs-lookup"><span data-stu-id="a5633-112">You can provide parameter values for hello linked template directly in your template or in a parameter file.</span></span> <span data-ttu-id="a5633-113">hello följande exempel används hello **parametrar** egenskapen toospecify ett parametervärde direkt.</span><span class="sxs-lookup"><span data-stu-id="a5633-113">hello following example uses hello **parameters** property toospecify a parameter value directly.</span></span>

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

<span data-ttu-id="a5633-114">Du kan ange beroenden mellan hello länkad mall och andra resurser som andra typer av resurser.</span><span class="sxs-lookup"><span data-stu-id="a5633-114">Like other resource types, you can set dependencies between hello linked template and other resources.</span></span> <span data-ttu-id="a5633-115">Därför när andra resurser kräver ett utdatavärde från hello länkade mall kan kan du kontrollera hello länkade mallen distribueras före.</span><span class="sxs-lookup"><span data-stu-id="a5633-115">Therefore, when other resources require an output value from hello linked template, you can make sure hello linked template is deployed before them.</span></span> <span data-ttu-id="a5633-116">Eller när hello länkade mallen är beroende av andra resurser, kan du se till andra resurser har distribuerats före hello länkade mallen.</span><span class="sxs-lookup"><span data-stu-id="a5633-116">Or, when hello linked template relies on other resources, you can make sure other resources are deployed before hello linked template.</span></span> <span data-ttu-id="a5633-117">Du kan hämta ett värde från en länkad mall med hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="a5633-117">You can retrieve a value from a linked template with hello following syntax:</span></span>

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

<span data-ttu-id="a5633-118">hello Resource Manager-tjänsten måste vara kan tooaccess hello länkad mall.</span><span class="sxs-lookup"><span data-stu-id="a5633-118">hello Resource Manager service must be able tooaccess hello linked template.</span></span> <span data-ttu-id="a5633-119">Du kan inte ange en lokal fil eller en fil som endast är tillgängliga i det lokala nätverket för hello länkad mall.</span><span class="sxs-lookup"><span data-stu-id="a5633-119">You cannot specify a local file or a file that is only available on your local network for hello linked template.</span></span> <span data-ttu-id="a5633-120">Du kan endast ange ett URI-värde som innehåller antingen **http** eller **https**.</span><span class="sxs-lookup"><span data-stu-id="a5633-120">You can only provide a URI value that includes either **http** or **https**.</span></span> <span data-ttu-id="a5633-121">Ett alternativ är tooplace länkade mallen i ett lagringskonto och Använd hello URI för objektet, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a5633-121">One option is tooplace your linked template in a storage account, and use hello URI for that item, such as shown in hello following example:</span></span>

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

<span data-ttu-id="a5633-122">Även om hello länkad mall måste finnas externt, behöver inte toobe allmänt tillgänglig toohello offentliga.</span><span class="sxs-lookup"><span data-stu-id="a5633-122">Although hello linked template must be externally available, it does not need toobe generally available toohello public.</span></span> <span data-ttu-id="a5633-123">Du kan lägga till din mall tooa privata lagringskonto som är tillgänglig tooonly hello lagringskontoägaren.</span><span class="sxs-lookup"><span data-stu-id="a5633-123">You can add your template tooa private storage account that is accessible tooonly hello storage account owner.</span></span> <span data-ttu-id="a5633-124">Sedan kan skapa du en delad åtkomst (SAS)-signaturen token tooenable åtkomst under distributionen.</span><span class="sxs-lookup"><span data-stu-id="a5633-124">Then, you create a shared access signature (SAS) token tooenable access during deployment.</span></span> <span data-ttu-id="a5633-125">Du lägger till den SAS-token toohello URI för hello länkad mall.</span><span class="sxs-lookup"><span data-stu-id="a5633-125">You add that SAS token toohello URI for hello linked template.</span></span> <span data-ttu-id="a5633-126">Stegvisa instruktioner för hur du konfigurerar en mall i ett lagringskonto och generera en SAS-token finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md) eller [distribuera resurser med Resource Manager-mallar och Azure CLI](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a5633-126">For steps on setting up a template in a storage account and generating a SAS token, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md) or [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> 

<span data-ttu-id="a5633-127">hello som följande exempel visar en mall för överordnade länkar tooanother mallen.</span><span class="sxs-lookup"><span data-stu-id="a5633-127">hello following example shows a parent template that links tooanother template.</span></span> <span data-ttu-id="a5633-128">hello länkad mall används med en SAS-token som skickas som en parameter.</span><span class="sxs-lookup"><span data-stu-id="a5633-128">hello linked template is accessed with a SAS token that is passed in as a parameter.</span></span>

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

<span data-ttu-id="a5633-129">Även om hello token som skickas som en säker sträng, loggas hello URI för hello länkade mallen, inklusive hello SAS-token, i hello distributionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="a5633-129">Even though hello token is passed in as a secure string, hello URI of hello linked template, including hello SAS token, is logged in hello deployment operations.</span></span> <span data-ttu-id="a5633-130">toolimit exponering, ange en giltighetstid för hello-token.</span><span class="sxs-lookup"><span data-stu-id="a5633-130">toolimit exposure, set an expiration for hello token.</span></span>

<span data-ttu-id="a5633-131">Resource Manager hanterar varje länkade mall som en separat distribution.</span><span class="sxs-lookup"><span data-stu-id="a5633-131">Resource Manager handles each linked template as a separate deployment.</span></span> <span data-ttu-id="a5633-132">I hello distributionshistoriken för hello resursgrupp ser du separata distributioner för hello överordnade och kapslade mallar.</span><span class="sxs-lookup"><span data-stu-id="a5633-132">In hello deployment history for hello resource group, you see separate deployments for hello parent and nested templates.</span></span>

![distributionshistorik](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-tooa-parameter-file"></a><span data-ttu-id="a5633-134">Länka tooa parameterfilen</span><span class="sxs-lookup"><span data-stu-id="a5633-134">Linking tooa parameter file</span></span>
<span data-ttu-id="a5633-135">hello nästa exempel använder hello **parametersLink** egenskapen toolink tooa parameterfil.</span><span class="sxs-lookup"><span data-stu-id="a5633-135">hello next example uses hello **parametersLink** property toolink tooa parameter file.</span></span>

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

<span data-ttu-id="a5633-136">hello URI värde hello länkade parameterfilen får inte vara en lokal fil och måste innehålla antingen **http** eller **https**.</span><span class="sxs-lookup"><span data-stu-id="a5633-136">hello URI value for hello linked parameter file cannot be a local file, and must include either **http** or **https**.</span></span> <span data-ttu-id="a5633-137">hello parameterfilen kan också vara begränsad tooaccess via en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="a5633-137">hello parameter file can also be limited tooaccess through a SAS token.</span></span>

## <a name="using-variables-toolink-templates"></a><span data-ttu-id="a5633-138">Med hjälp av variabler toolink mallar</span><span class="sxs-lookup"><span data-stu-id="a5633-138">Using variables toolink templates</span></span>
<span data-ttu-id="a5633-139">hello föregående exempel visade hårdkodade URL-värden för hello mallen länkar.</span><span class="sxs-lookup"><span data-stu-id="a5633-139">hello previous examples showed hard-coded URL values for hello template links.</span></span> <span data-ttu-id="a5633-140">Den här metoden kan fungera för en enkel mall men det fungerar inte bra när du arbetar med en stor mängd modulära mallar.</span><span class="sxs-lookup"><span data-stu-id="a5633-140">This approach might work for a simple template but it does not work well when working with a large set of modular templates.</span></span> <span data-ttu-id="a5633-141">I stället kan du skapa en statisk variabel som lagrar en bas-URL för hello Huvudmall och dynamiskt skapa URL: er för hello länkade mallar från den grundläggande Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="a5633-141">Instead, you can create a static variable that stores a base URL for hello main template and then dynamically create URLs for hello linked templates from that base URL.</span></span> <span data-ttu-id="a5633-142">hello fördelen med den här metoden är att du enkelt kan flytta eller förgrening hello mall eftersom du behöver bara toochange hello statisk variabel i hello Huvudmall.</span><span class="sxs-lookup"><span data-stu-id="a5633-142">hello benefit of this approach is you can easily move or fork hello template because you only need toochange hello static variable in hello main template.</span></span> <span data-ttu-id="a5633-143">hello Huvudmall skickar hello rätt URI: er i hela hello nedbruten mallen.</span><span class="sxs-lookup"><span data-stu-id="a5633-143">hello main template passes hello correct URIs throughout hello decomposed template.</span></span>

<span data-ttu-id="a5633-144">hello följande exempel visas hur toouse en grundläggande URL toocreate två URL: er för länkade mallar (**sharedTemplateUrl** och **vmTemplate**).</span><span class="sxs-lookup"><span data-stu-id="a5633-144">hello following example shows how toouse a base URL toocreate two URLs for linked templates (**sharedTemplateUrl** and **vmTemplate**).</span></span> 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

<span data-ttu-id="a5633-145">Du kan också använda [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello bas-URL för aktuella mallen för hello och använda tooget hello URL: en för andra mallar i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="a5633-145">You can also use [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello base URL for hello current template, and use that tooget hello URL for other templates in hello same location.</span></span> <span data-ttu-id="a5633-146">Den här metoden är användbar om du mallen ändrar placering (kanske på grund av tooversioning) eller om du vill tooavoid hårda kodning URL: er i hello mallfilen.</span><span class="sxs-lookup"><span data-stu-id="a5633-146">This approach is useful if your template location changes (maybe due tooversioning) or you want tooavoid hard coding URLs in hello template file.</span></span> 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a><span data-ttu-id="a5633-147">Fullständigt exempel</span><span class="sxs-lookup"><span data-stu-id="a5633-147">Complete example</span></span>
<span data-ttu-id="a5633-148">hello följande exempel mallar visar en förenklad placering av länkade mallar tooillustrate flera hello begrepp i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a5633-148">hello following example templates show a simplified arrangement of linked templates tooillustrate several of hello concepts in this article.</span></span> <span data-ttu-id="a5633-149">Det förutsätts att hello mallar har lagts till toohello behållare i ett lagringskonto med offentlig åtkomst är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="a5633-149">It assumes hello templates have been added toohello same container in a storage account with public access turned off.</span></span> <span data-ttu-id="a5633-150">hello länkad mall skickar ett värde tillbaka toohello Huvudmall i hello **matar ut** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a5633-150">hello linked template passes a value back toohello main template in hello **outputs** section.</span></span>

<span data-ttu-id="a5633-151">Hej **parent.json** filen består av:</span><span class="sxs-lookup"><span data-stu-id="a5633-151">hello **parent.json** file consists of:</span></span>

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

<span data-ttu-id="a5633-152">Hej **helloworld.json** filen består av:</span><span class="sxs-lookup"><span data-stu-id="a5633-152">hello **helloworld.json** file consists of:</span></span>

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

<span data-ttu-id="a5633-153">I PowerShell, hämta en token för hello behållare och distribuera hello mallar med:</span><span class="sxs-lookup"><span data-stu-id="a5633-153">In PowerShell, you get a token for hello container and deploy hello templates with:</span></span>

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

<span data-ttu-id="a5633-154">I Azure CLI 2.0, hämta en token för hello behållare och distribuera hello mallar med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="a5633-154">In Azure CLI 2.0, you get a token for hello container and deploy hello templates with hello following code:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a5633-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5633-155">Next steps</span></span>
* <span data-ttu-id="a5633-156">toolearn om hello definierar hello distributionsordning för dina resurser, se [definiera beroenden i Azure Resource Manager-mallar](resource-group-define-dependencies.md)</span><span class="sxs-lookup"><span data-stu-id="a5633-156">toolearn about hello defining hello deployment order for your resources, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md)</span></span>
* <span data-ttu-id="a5633-157">toolearn hur toodefine en resurs skapa men flera förekomster av det, finns i [och skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="a5633-157">toolearn how toodefine one resource but create many instances of it, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>

