---
title: aaaExport Resource Manager-mall med Azure PowerShell | Microsoft Docs
description: "Använd Azure Resource Manager och Azure PowerShell tooexport en mall från en resursgrupp."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 9a239b7bce8209326c0e267a4d3d69f7014bdaed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a><span data-ttu-id="c5496-103">Exportera Azure Resource Manager-mallar med PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5496-103">Export Azure Resource Manager templates with PowerShell</span></span>

<span data-ttu-id="c5496-104">Resource Manager kan tooexport Resource Manager-mall från befintliga resurser i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5496-104">Resource Manager enables you tooexport a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="c5496-105">Du kan använda den genererade mallen toolearn om hello mallen syntax eller tooautomate hello Omdistributionen av din lösning efter behov.</span><span class="sxs-lookup"><span data-stu-id="c5496-105">You can use that generated template toolearn about hello template syntax or tooautomate hello redeployment of your solution as needed.</span></span>

<span data-ttu-id="c5496-106">Det är viktigt toonote att det finns två olika sätt tooexport en mall:</span><span class="sxs-lookup"><span data-stu-id="c5496-106">It is important toonote that there are two different ways tooexport a template:</span></span>

* <span data-ttu-id="c5496-107">Du kan exportera hello faktiska mall som du använde för en distribution.</span><span class="sxs-lookup"><span data-stu-id="c5496-107">You can export hello actual template that you used for a deployment.</span></span> <span data-ttu-id="c5496-108">hello exporterade mallen innehåller alla hello parametrar och variabler exakt som de visas i hello ursprungliga mallen.</span><span class="sxs-lookup"><span data-stu-id="c5496-108">hello exported template includes all hello parameters and variables exactly as they appeared in hello original template.</span></span> <span data-ttu-id="c5496-109">Den här metoden är användbar när du behöver tooretrieve en mall.</span><span class="sxs-lookup"><span data-stu-id="c5496-109">This approach is helpful when you need tooretrieve a template.</span></span>
* <span data-ttu-id="c5496-110">Du kan exportera en mall som representerar hello hello resursgruppen aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c5496-110">You can export a template that represents hello current state of hello resource group.</span></span> <span data-ttu-id="c5496-111">exporterad mall för hello baseras inte på en mall som du använde för distribution.</span><span class="sxs-lookup"><span data-stu-id="c5496-111">hello exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="c5496-112">I stället skapar en mall som är en ögonblicksbild av hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c5496-112">Instead, it creates a template that is a snapshot of hello resource group.</span></span> <span data-ttu-id="c5496-113">hello exporterad mall har många hårdkodade värden och förmodligen inte så många parametrar som du vanligtvis definierar.</span><span class="sxs-lookup"><span data-stu-id="c5496-113">hello exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="c5496-114">Den här metoden är användbar när du har ändrat hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c5496-114">This approach is useful when you have modified hello resource group.</span></span> <span data-ttu-id="c5496-115">Du måste nu toocapture hello resursgruppen som en mall.</span><span class="sxs-lookup"><span data-stu-id="c5496-115">Now, you need toocapture hello resource group as a template.</span></span>

<span data-ttu-id="c5496-116">Båda metoderna beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c5496-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="c5496-117">Distribuera en lösning</span><span class="sxs-lookup"><span data-stu-id="c5496-117">Deploy a solution</span></span>

<span data-ttu-id="c5496-118">tooillustrate båda närmar sig för att exportera en mall, låt oss börja med att distribuera en lösning tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5496-118">tooillustrate both approaches for exporting a template, let's start by deploying a solution tooyour subscription.</span></span> <span data-ttu-id="c5496-119">Om du redan har en resursgrupp i din prenumeration som du vill tooexport har du inte toodeploy den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="c5496-119">If you already have a resource group in your subscription that you want tooexport, you do not have toodeploy this solution.</span></span> <span data-ttu-id="c5496-120">Men handlar hello resten av den här artikeln toohello mallen för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="c5496-120">However, hello remainder of this article refers toohello template for this solution.</span></span> <span data-ttu-id="c5496-121">hello exempelskriptet distribuerar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c5496-121">hello example script deploys a storage account.</span></span>

```powershell
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="c5496-122">Spara mallen från distributionshistoriken</span><span class="sxs-lookup"><span data-stu-id="c5496-122">Save template from deployment history</span></span>

<span data-ttu-id="c5496-123">Du kan hämta en mall från distributionshistoriken med hjälp av hello [spara AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) kommando.</span><span class="sxs-lookup"><span data-stu-id="c5496-123">You can retrieve a template from your deployment history by using hello [Save-AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) command.</span></span> <span data-ttu-id="c5496-124">följande exempel sparar hello mall som du tidigare har distribuerat hello:</span><span class="sxs-lookup"><span data-stu-id="c5496-124">hello following example saves hello template that you previously deploy:</span></span>

```powershell
Save-AzureRmResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

<span data-ttu-id="c5496-125">Den returnerar hello mallens hello placering.</span><span class="sxs-lookup"><span data-stu-id="c5496-125">It returns hello location of hello template.</span></span>

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

<span data-ttu-id="c5496-126">Öppna hello-filen och Observera att det är hello exakt mallen som du använde för distribution.</span><span class="sxs-lookup"><span data-stu-id="c5496-126">Open hello file, and notice that it is hello exact template you used for deployment.</span></span> <span data-ttu-id="c5496-127">hello parametrar och variabler matchar hello mallen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="c5496-127">hello parameters and variables match hello template from GitHub.</span></span> <span data-ttu-id="c5496-128">Du kan distribuera den här mallen.</span><span class="sxs-lookup"><span data-stu-id="c5496-128">You can redeploy this template.</span></span>

## <a name="export-resource-group-as-template"></a><span data-ttu-id="c5496-129">Exportera resursgrupp som mall</span><span class="sxs-lookup"><span data-stu-id="c5496-129">Export resource group as template</span></span>

<span data-ttu-id="c5496-130">I stället för att hämta en mall från hello distributionshistoriken, kan du hämta en mall som representerar hello aktuell status för en resursgrupp med hjälp av hello [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) kommando.</span><span class="sxs-lookup"><span data-stu-id="c5496-130">Instead of retrieving a template from hello deployment history, you can retrieve a template that represents hello current state of a resource group by using hello [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) command.</span></span> <span data-ttu-id="c5496-131">Du kan använda det här kommandot när du har gjort många ändringar tooyour resursgruppen och inga befintliga mall representerar alla hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="c5496-131">You use this command when you have made many changes tooyour resource group and no existing template represents all hello changes.</span></span>

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleGroup
```

<span data-ttu-id="c5496-132">Den returnerar hello mallens hello placering.</span><span class="sxs-lookup"><span data-stu-id="c5496-132">It returns hello location of hello template.</span></span>

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

<span data-ttu-id="c5496-133">Öppna hello-filen och Lägg märke till att det är ett annat än hello mallen i GitHub.</span><span class="sxs-lookup"><span data-stu-id="c5496-133">Open hello file, and notice that it is different than hello template in GitHub.</span></span> <span data-ttu-id="c5496-134">Det har olika parametrar och inga variabler.</span><span class="sxs-lookup"><span data-stu-id="c5496-134">It has different parameters and no variables.</span></span> <span data-ttu-id="c5496-135">hello lagring SKU och platsen är hårdkodat toovalues.</span><span class="sxs-lookup"><span data-stu-id="c5496-135">hello storage SKU and location are hard-coded toovalues.</span></span> <span data-ttu-id="c5496-136">hello följande exempel visar hello exporterad mall, men mallen har ett något annorlunda parameternamn:</span><span class="sxs-lookup"><span data-stu-id="c5496-136">hello following example shows hello exported template, but your template has a slightly different parameter name:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccounts_nf3mvst4nqb36standardsa_name": {
      "defaultValue": null,
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[parameters('storageAccounts_nf3mvst4nqb36standardsa_name')]",
      "apiVersion": "2016-01-01",
      "location": "southcentralus",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

<span data-ttu-id="c5496-137">Du kan distribuera den här mallen, men det krävs ett unikt namn för hello storage-konto för att gissa.</span><span class="sxs-lookup"><span data-stu-id="c5496-137">You can redeploy this template, but it requires guessing a unique name for hello storage account.</span></span> <span data-ttu-id="c5496-138">hello namnet på parametern är något annorlunda.</span><span class="sxs-lookup"><span data-stu-id="c5496-138">hello name of your parameter is slightly different.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a><span data-ttu-id="c5496-139">Anpassa exporterad mall</span><span class="sxs-lookup"><span data-stu-id="c5496-139">Customize exported template</span></span>

<span data-ttu-id="c5496-140">Du kan ändra den här mallen toomake den toouse enklare och mer flexibelt.</span><span class="sxs-lookup"><span data-stu-id="c5496-140">You can modify this template toomake it easier toouse and more flexible.</span></span> <span data-ttu-id="c5496-141">tooallow för flera platser, ändra hello plats egenskapen toouse hello samma plats som hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="c5496-141">tooallow for more locations, change hello location property toouse hello same location as hello resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="c5496-142">tooavoid har tooguess uniques namn för storage-konto, ta bort hello-parametern för hello lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="c5496-142">tooavoid having tooguess a uniques name for storage account, remove hello parameter for hello storage account name.</span></span> <span data-ttu-id="c5496-143">Lägga till en parameter för ett namnsuffix för lagring och lagring SKU:</span><span class="sxs-lookup"><span data-stu-id="c5496-143">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

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

<span data-ttu-id="c5496-144">Lägg till en variabel som konstruktioner hello lagringskontonamnet med hello uniqueString funktionen:</span><span class="sxs-lookup"><span data-stu-id="c5496-144">Add a variable that constructs hello storage account name with hello uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="c5496-145">Ange hello namnet på hello storage-konto toohello variabel:</span><span class="sxs-lookup"><span data-stu-id="c5496-145">Set hello name of hello storage account toohello variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="c5496-146">Ange hello SKU toohello parametern:</span><span class="sxs-lookup"><span data-stu-id="c5496-146">Set hello SKU toohello parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="c5496-147">Nu ser din mall ut så här:</span><span class="sxs-lookup"><span data-stu-id="c5496-147">Your template now looks like:</span></span>

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

<span data-ttu-id="c5496-148">Omdistribuera hello ändrade mallen.</span><span class="sxs-lookup"><span data-stu-id="c5496-148">Redeploy hello modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5496-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5496-149">Next steps</span></span>
* <span data-ttu-id="c5496-150">Information om hur du använder hello portal tooexport en mall finns i [exportera en Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="c5496-150">For information about using hello portal tooexport a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="c5496-151">toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="c5496-151">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="c5496-152">Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="c5496-152">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
