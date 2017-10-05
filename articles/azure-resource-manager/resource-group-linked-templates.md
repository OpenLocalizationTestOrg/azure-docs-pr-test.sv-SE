---
title: "Länka mallar för Azure-distribution | Microsoft Docs"
description: "Beskriver hur du kan använda länkade mallar i en Azure Resource Manager-mall för att skapa en mall för modulär lösning. Visar hur skicka parametrar värden genom att ange en parameterfil och dynamiskt skapade URL: er."
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
ms.openlocfilehash: 8b58a83ffd473500dd3f76c09e251f9208527d4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a><span data-ttu-id="a1b09-104">Använda länkade mallar när du distribuerar Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="a1b09-104">Using linked templates when deploying Azure resources</span></span>
<span data-ttu-id="a1b09-105">Från inom en Azure Resource Manager-mall, kan du länka till en annan mall, vilket gör det möjligt att dela upp din distribution till en uppsättning riktade, mallar för specifika ändamål.</span><span class="sxs-lookup"><span data-stu-id="a1b09-105">From within one Azure Resource Manager template, you can link to another template, which enables you to decompose your deployment into a set of targeted, purpose-specific templates.</span></span> <span data-ttu-id="a1b09-106">Precis som med decomposing ett program i flera klasser som koden har uppdelning fördelar vad gäller testning, återanvändning och läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="a1b09-106">As with decomposing an application into several code classes, decomposition provides benefits in terms of testing, reuse, and readability.</span></span>  

<span data-ttu-id="a1b09-107">Du kan skicka parametrar från en Huvudmall till en länkad mall och parametrarna direkt kan mappa till parametrar eller variabler som exponeras av anropa mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-107">You can pass parameters from a main template to a linked template, and those parameters can directly map to parameters or variables exposed by the calling template.</span></span> <span data-ttu-id="a1b09-108">Den länkade mallen kan också skicka en variabel utdata tillbaka till källmallen, aktiverar du en dubbelriktad datautbyte mellan mallar.</span><span class="sxs-lookup"><span data-stu-id="a1b09-108">The linked template can also pass an output variable back to the source template, enabling a two-way data exchange between templates.</span></span>

## <a name="linking-to-a-template"></a><span data-ttu-id="a1b09-109">Länka till en mall</span><span class="sxs-lookup"><span data-stu-id="a1b09-109">Linking to a template</span></span>
<span data-ttu-id="a1b09-110">Du kan skapa en länk mellan två mallar genom att lägga till en distributionsresurs i mallen pekar på den länkade mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-110">You create a link between two templates by adding a deployment resource within the main template that points to the linked template.</span></span> <span data-ttu-id="a1b09-111">Du ställer in den **templateLink** egenskapen till URI: N för den länkade mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-111">You set the **templateLink** property to the URI of the linked template.</span></span> <span data-ttu-id="a1b09-112">Du kan ange parametervärden för mallen länkade direkt i din mall eller en parameterfil.</span><span class="sxs-lookup"><span data-stu-id="a1b09-112">You can provide parameter values for the linked template directly in your template or in a parameter file.</span></span> <span data-ttu-id="a1b09-113">I följande exempel används den **parametrar** att ange ett parametervärde direkt.</span><span class="sxs-lookup"><span data-stu-id="a1b09-113">The following example uses the **parameters** property to specify a parameter value directly.</span></span>

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

<span data-ttu-id="a1b09-114">Du kan ange beroenden mellan länkade mallen och andra resurser som andra typer av resurser.</span><span class="sxs-lookup"><span data-stu-id="a1b09-114">Like other resource types, you can set dependencies between the linked template and other resources.</span></span> <span data-ttu-id="a1b09-115">Därför när andra resurser kräver ett utdatavärde från den länka mallen kan kan du kontrollera länkade mallen distribueras före datakällorna.</span><span class="sxs-lookup"><span data-stu-id="a1b09-115">Therefore, when other resources require an output value from the linked template, you can make sure the linked template is deployed before them.</span></span> <span data-ttu-id="a1b09-116">Eller när länkade mallen är beroende av andra resurser, kan du se till andra resurser har distribuerats innan länkade mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-116">Or, when the linked template relies on other resources, you can make sure other resources are deployed before the linked template.</span></span> <span data-ttu-id="a1b09-117">Du kan hämta ett värde från en länkad mall med följande syntax:</span><span class="sxs-lookup"><span data-stu-id="a1b09-117">You can retrieve a value from a linked template with the following syntax:</span></span>

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

<span data-ttu-id="a1b09-118">Resource Manager-tjänsten måste kunna få åtkomst till den länkade mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-118">The Resource Manager service must be able to access the linked template.</span></span> <span data-ttu-id="a1b09-119">Du kan inte ange en lokal fil eller en fil som endast är tillgängliga i det lokala nätverket för den länka mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-119">You cannot specify a local file or a file that is only available on your local network for the linked template.</span></span> <span data-ttu-id="a1b09-120">Du kan endast ange ett URI-värde som innehåller antingen **http** eller **https**.</span><span class="sxs-lookup"><span data-stu-id="a1b09-120">You can only provide a URI value that includes either **http** or **https**.</span></span> <span data-ttu-id="a1b09-121">Ett alternativ är att placera den länka mallen i ett lagringskonto och Använd URI för objektet, så som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a1b09-121">One option is to place your linked template in a storage account, and use the URI for that item, such as shown in the following example:</span></span>

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

<span data-ttu-id="a1b09-122">Även om mallen länkade måste finnas externt, behöver det inte vara allmänt tillgänglig för allmänheten.</span><span class="sxs-lookup"><span data-stu-id="a1b09-122">Although the linked template must be externally available, it does not need to be generally available to the public.</span></span> <span data-ttu-id="a1b09-123">Du kan lägga till mallen till en privat storage-konto som är tillgänglig för endast lagringskontoägaren.</span><span class="sxs-lookup"><span data-stu-id="a1b09-123">You can add your template to a private storage account that is accessible to only the storage account owner.</span></span> <span data-ttu-id="a1b09-124">Sedan kan skapa du en delad åtkomsttoken signatur (SAS) för att möjliggöra åtkomst under distributionen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-124">Then, you create a shared access signature (SAS) token to enable access during deployment.</span></span> <span data-ttu-id="a1b09-125">Du lägger till den SAS-token URI för den länkade mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-125">You add that SAS token to the URI for the linked template.</span></span> <span data-ttu-id="a1b09-126">Stegvisa instruktioner för hur du konfigurerar en mall i ett lagringskonto och generera en SAS-token finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md) eller [distribuera resurser med Resource Manager-mallar och Azure CLI](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a1b09-126">For steps on setting up a template in a storage account and generating a SAS token, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md) or [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> 

<span data-ttu-id="a1b09-127">I följande exempel visas en överordnad mapp som länkar till en annan mall.</span><span class="sxs-lookup"><span data-stu-id="a1b09-127">The following example shows a parent template that links to another template.</span></span> <span data-ttu-id="a1b09-128">Den länkade mallen används med en SAS-token som skickas som en parameter.</span><span class="sxs-lookup"><span data-stu-id="a1b09-128">The linked template is accessed with a SAS token that is passed in as a parameter.</span></span>

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

<span data-ttu-id="a1b09-129">Även om token som skickas som en säker sträng, loggas URI för länkade mallen, inklusive SAS-token i distributionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="a1b09-129">Even though the token is passed in as a secure string, the URI of the linked template, including the SAS token, is logged in the deployment operations.</span></span> <span data-ttu-id="a1b09-130">Ange en giltighetstid för token för att begränsa exponeringen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-130">To limit exposure, set an expiration for the token.</span></span>

<span data-ttu-id="a1b09-131">Resource Manager hanterar varje länkade mall som en separat distribution.</span><span class="sxs-lookup"><span data-stu-id="a1b09-131">Resource Manager handles each linked template as a separate deployment.</span></span> <span data-ttu-id="a1b09-132">I distributionshistoriken för resursgruppen ser du separata distributioner för den överordnade och de kapslade mallar.</span><span class="sxs-lookup"><span data-stu-id="a1b09-132">In the deployment history for the resource group, you see separate deployments for the parent and nested templates.</span></span>

![distributionshistorik](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-to-a-parameter-file"></a><span data-ttu-id="a1b09-134">Länka till en parameterfil</span><span class="sxs-lookup"><span data-stu-id="a1b09-134">Linking to a parameter file</span></span>
<span data-ttu-id="a1b09-135">I nästa exempel används den **parametersLink** egenskapen att länka till en parameterfil.</span><span class="sxs-lookup"><span data-stu-id="a1b09-135">The next example uses the **parametersLink** property to link to a parameter file.</span></span>

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

<span data-ttu-id="a1b09-136">URI-värdet för den länka parameterfilen får inte vara en lokal fil och måste innehålla antingen **http** eller **https**.</span><span class="sxs-lookup"><span data-stu-id="a1b09-136">The URI value for the linked parameter file cannot be a local file, and must include either **http** or **https**.</span></span> <span data-ttu-id="a1b09-137">Parameterfilen kan också vara begränsad åtkomst genom en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="a1b09-137">The parameter file can also be limited to access through a SAS token.</span></span>

## <a name="using-variables-to-link-templates"></a><span data-ttu-id="a1b09-138">Använda variabler för att länka mallar</span><span class="sxs-lookup"><span data-stu-id="a1b09-138">Using variables to link templates</span></span>
<span data-ttu-id="a1b09-139">I föregående exempel visade hårdkodade URL-värden för mallen länkar.</span><span class="sxs-lookup"><span data-stu-id="a1b09-139">The previous examples showed hard-coded URL values for the template links.</span></span> <span data-ttu-id="a1b09-140">Den här metoden kan fungera för en enkel mall men det fungerar inte bra när du arbetar med en stor mängd modulära mallar.</span><span class="sxs-lookup"><span data-stu-id="a1b09-140">This approach might work for a simple template but it does not work well when working with a large set of modular templates.</span></span> <span data-ttu-id="a1b09-141">I stället kan du skapa en statisk variabel som lagrar en bas-URL för den huvudsakliga mallen och dynamiskt skapa URL: er för de länkade mallarna från den grundläggande Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-141">Instead, you can create a static variable that stores a base URL for the main template and then dynamically create URLs for the linked templates from that base URL.</span></span> <span data-ttu-id="a1b09-142">Fördelen med den här metoden är att du enkelt kan flytta eller duplicera mallen eftersom du behöver ändra den statiska variabeln i huvudsakliga mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-142">The benefit of this approach is you can easily move or fork the template because you only need to change the static variable in the main template.</span></span> <span data-ttu-id="a1b09-143">Den huvudsakliga mallen skickar rätt URI: er i hela uppdelade mallen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-143">The main template passes the correct URIs throughout the decomposed template.</span></span>

<span data-ttu-id="a1b09-144">I följande exempel visas hur du skapar två webbadresserna för länkade mallar med hjälp av en bas-URL (**sharedTemplateUrl** och **vmTemplate**).</span><span class="sxs-lookup"><span data-stu-id="a1b09-144">The following example shows how to use a base URL to create two URLs for linked templates (**sharedTemplateUrl** and **vmTemplate**).</span></span> 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

<span data-ttu-id="a1b09-145">Du kan också använda [deployment()](resource-group-template-functions-deployment.md#deployment) att hämta en bas-URL för den aktuella mallen och använda den för att hämta URL för andra mallar på samma plats.</span><span class="sxs-lookup"><span data-stu-id="a1b09-145">You can also use [deployment()](resource-group-template-functions-deployment.md#deployment) to get the base URL for the current template, and use that to get the URL for other templates in the same location.</span></span> <span data-ttu-id="a1b09-146">Den här metoden är användbart om din mallplats ändras (kanske på grund av versioning) eller om du vill undvika hård kodning URL: er i mallfilen.</span><span class="sxs-lookup"><span data-stu-id="a1b09-146">This approach is useful if your template location changes (maybe due to versioning) or you want to avoid hard coding URLs in the template file.</span></span> 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a><span data-ttu-id="a1b09-147">Fullständigt exempel</span><span class="sxs-lookup"><span data-stu-id="a1b09-147">Complete example</span></span>
<span data-ttu-id="a1b09-148">Följande exempel mallar visar en förenklad placering av länkade mallar att illustrera flera av begrepp i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a1b09-148">The following example templates show a simplified arrangement of linked templates to illustrate several of the concepts in this article.</span></span> <span data-ttu-id="a1b09-149">Det förutsätts att mallarna har lagts till behållaren i ett lagringskonto med offentlig åtkomst är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="a1b09-149">It assumes the templates have been added to the same container in a storage account with public access turned off.</span></span> <span data-ttu-id="a1b09-150">Den länkade mallen skickar ett värde tillbaka till den huvudsakliga mallen i den **matar ut** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a1b09-150">The linked template passes a value back to the main template in the **outputs** section.</span></span>

<span data-ttu-id="a1b09-151">Den **parent.json** filen består av:</span><span class="sxs-lookup"><span data-stu-id="a1b09-151">The **parent.json** file consists of:</span></span>

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

<span data-ttu-id="a1b09-152">Den **helloworld.json** filen består av:</span><span class="sxs-lookup"><span data-stu-id="a1b09-152">The **helloworld.json** file consists of:</span></span>

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

<span data-ttu-id="a1b09-153">I PowerShell, hämta en token för behållaren och distribuera mallar med:</span><span class="sxs-lookup"><span data-stu-id="a1b09-153">In PowerShell, you get a token for the container and deploy the templates with:</span></span>

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

<span data-ttu-id="a1b09-154">I Azure CLI 2.0, hämta en token för behållaren och distribuera mallar med följande kod:</span><span class="sxs-lookup"><span data-stu-id="a1b09-154">In Azure CLI 2.0, you get a token for the container and deploy the templates with the following code:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a1b09-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1b09-155">Next steps</span></span>
* <span data-ttu-id="a1b09-156">Läs om hur du definierar distributionsordningen för dina resurser i [definiera beroenden i Azure Resource Manager-mallar](resource-group-define-dependencies.md)</span><span class="sxs-lookup"><span data-stu-id="a1b09-156">To learn about the defining the deployment order for your resources, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md)</span></span>
* <span data-ttu-id="a1b09-157">Information om hur du definierar en resurs men skapa många instanser av det, se [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="a1b09-157">To learn how to define one resource but create many instances of it, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>

