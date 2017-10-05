---
title: Exportera Resource Manager-mall med Azure PowerShell | Microsoft Docs
description: "Använd Azure Resource Manager och Azure PowerShell för att exportera en mall från en resursgrupp."
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
ms.openlocfilehash: 7543811eb9448222b6e7c266756e68debc7d54be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a><span data-ttu-id="9d747-103">Exportera Azure Resource Manager-mallar med PowerShell</span><span class="sxs-lookup"><span data-stu-id="9d747-103">Export Azure Resource Manager templates with PowerShell</span></span>

<span data-ttu-id="9d747-104">Med Resource Manager kan du exportera en Resource Manager-mall från befintliga resurser i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9d747-104">Resource Manager enables you to export a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="9d747-105">Du kan använda mallen som genereras för att lära dig mer om mallsyntaxen eller för att automatisera omdistributionen av din lösning om det behövs.</span><span class="sxs-lookup"><span data-stu-id="9d747-105">You can use that generated template to learn about the template syntax or to automate the redeployment of your solution as needed.</span></span>

<span data-ttu-id="9d747-106">Observera att du kan exportera en mall på två sätt:</span><span class="sxs-lookup"><span data-stu-id="9d747-106">It is important to note that there are two different ways to export a template:</span></span>

* <span data-ttu-id="9d747-107">Du kan exportera själva mallen som du använde för en distribution.</span><span class="sxs-lookup"><span data-stu-id="9d747-107">You can export the actual template that you used for a deployment.</span></span> <span data-ttu-id="9d747-108">Den exporterade mallen innehåller alla parametrar och variabler exakt som de visas i den ursprungliga mallen.</span><span class="sxs-lookup"><span data-stu-id="9d747-108">The exported template includes all the parameters and variables exactly as they appeared in the original template.</span></span> <span data-ttu-id="9d747-109">Den här metoden är användbar när du vill hämta en mall.</span><span class="sxs-lookup"><span data-stu-id="9d747-109">This approach is helpful when you need to retrieve a template.</span></span>
* <span data-ttu-id="9d747-110">Du kan exportera en mall som representerar resursgruppens aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="9d747-110">You can export a template that represents the current state of the resource group.</span></span> <span data-ttu-id="9d747-111">Den exporterade mallen baseras inte på en mall som du har använt för distribution.</span><span class="sxs-lookup"><span data-stu-id="9d747-111">The exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="9d747-112">I stället skapar den en mall som är en ögonblicksbild av resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9d747-112">Instead, it creates a template that is a snapshot of the resource group.</span></span> <span data-ttu-id="9d747-113">Den exporterade mallen har många hårdkodade värden och troligen inte så många parametrar som du vanligtvis definierar.</span><span class="sxs-lookup"><span data-stu-id="9d747-113">The exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="9d747-114">Den här metoden är användbar när du har ändrat resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9d747-114">This approach is useful when you have modified the resource group.</span></span> <span data-ttu-id="9d747-115">I detta fall behöver du avbilda resursgruppen som en mall.</span><span class="sxs-lookup"><span data-stu-id="9d747-115">Now, you need to capture the resource group as a template.</span></span>

<span data-ttu-id="9d747-116">Båda metoderna beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9d747-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="9d747-117">Distribuera en lösning</span><span class="sxs-lookup"><span data-stu-id="9d747-117">Deploy a solution</span></span>

<span data-ttu-id="9d747-118">För att illustrera båda metoderna för att exportera en mall, börja med att distribuera en lösning till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9d747-118">To illustrate both approaches for exporting a template, let's start by deploying a solution to your subscription.</span></span> <span data-ttu-id="9d747-119">Om du redan har en resursgrupp i din prenumeration som du vill exportera, behöver du inte distribuera den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="9d747-119">If you already have a resource group in your subscription that you want to export, you do not have to deploy this solution.</span></span> <span data-ttu-id="9d747-120">Men handlar resten av den här artikeln mallen för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="9d747-120">However, the remainder of this article refers to the template for this solution.</span></span> <span data-ttu-id="9d747-121">Exempelskriptet distribuerar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9d747-121">The example script deploys a storage account.</span></span>

```powershell
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="9d747-122">Spara mallen från distributionshistoriken</span><span class="sxs-lookup"><span data-stu-id="9d747-122">Save template from deployment history</span></span>

<span data-ttu-id="9d747-123">Du kan hämta en mall från distributionshistoriken med hjälp av den [spara AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) kommando.</span><span class="sxs-lookup"><span data-stu-id="9d747-123">You can retrieve a template from your deployment history by using the [Save-AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) command.</span></span> <span data-ttu-id="9d747-124">I följande exempel sparas den mall som du distribuerar tidigare:</span><span class="sxs-lookup"><span data-stu-id="9d747-124">The following example saves the template that you previously deploy:</span></span>

```powershell
Save-AzureRmResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

<span data-ttu-id="9d747-125">Den returnerar placeringen av mallen.</span><span class="sxs-lookup"><span data-stu-id="9d747-125">It returns the location of the template.</span></span>

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

<span data-ttu-id="9d747-126">Öppna filen och Lägg märke till att det är exakt mallen som du använde för distribution.</span><span class="sxs-lookup"><span data-stu-id="9d747-126">Open the file, and notice that it is the exact template you used for deployment.</span></span> <span data-ttu-id="9d747-127">Parametrar och variabler matchar mallen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="9d747-127">The parameters and variables match the template from GitHub.</span></span> <span data-ttu-id="9d747-128">Du kan distribuera den här mallen.</span><span class="sxs-lookup"><span data-stu-id="9d747-128">You can redeploy this template.</span></span>

## <a name="export-resource-group-as-template"></a><span data-ttu-id="9d747-129">Exportera resursgrupp som mall</span><span class="sxs-lookup"><span data-stu-id="9d747-129">Export resource group as template</span></span>

<span data-ttu-id="9d747-130">I stället för att hämta en mall från distributionshistoriken, kan du hämta en mall som representerar det aktuella tillståndet för en resursgrupp med hjälp av den [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) kommando.</span><span class="sxs-lookup"><span data-stu-id="9d747-130">Instead of retrieving a template from the deployment history, you can retrieve a template that represents the current state of a resource group by using the [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) command.</span></span> <span data-ttu-id="9d747-131">Du kan använda det här kommandot när du har gjort många ändringar i resursgruppen och inga befintliga mallen som representerar alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="9d747-131">You use this command when you have made many changes to your resource group and no existing template represents all the changes.</span></span>

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleGroup
```

<span data-ttu-id="9d747-132">Den returnerar placeringen av mallen.</span><span class="sxs-lookup"><span data-stu-id="9d747-132">It returns the location of the template.</span></span>

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

<span data-ttu-id="9d747-133">Öppna filen och Lägg märke till att den skiljer sig från mallen i GitHub.</span><span class="sxs-lookup"><span data-stu-id="9d747-133">Open the file, and notice that it is different than the template in GitHub.</span></span> <span data-ttu-id="9d747-134">Det har olika parametrar och inga variabler.</span><span class="sxs-lookup"><span data-stu-id="9d747-134">It has different parameters and no variables.</span></span> <span data-ttu-id="9d747-135">SKU-lagring och plats är hårdkodat till värden.</span><span class="sxs-lookup"><span data-stu-id="9d747-135">The storage SKU and location are hard-coded to values.</span></span> <span data-ttu-id="9d747-136">I följande exempel visas den exporterade mallen, men mallen har ett något annorlunda parameternamn:</span><span class="sxs-lookup"><span data-stu-id="9d747-136">The following example shows the exported template, but your template has a slightly different parameter name:</span></span>

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

<span data-ttu-id="9d747-137">Du kan distribuera den här mallen, men det krävs ett unikt namn för storage-konto för att gissa.</span><span class="sxs-lookup"><span data-stu-id="9d747-137">You can redeploy this template, but it requires guessing a unique name for the storage account.</span></span> <span data-ttu-id="9d747-138">Namnet på parametern är något annorlunda.</span><span class="sxs-lookup"><span data-stu-id="9d747-138">The name of your parameter is slightly different.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a><span data-ttu-id="9d747-139">Anpassa exporterad mall</span><span class="sxs-lookup"><span data-stu-id="9d747-139">Customize exported template</span></span>

<span data-ttu-id="9d747-140">Du kan ändra den här mallen för att göra det enklare att använda och mer flexibelt.</span><span class="sxs-lookup"><span data-stu-id="9d747-140">You can modify this template to make it easier to use and more flexible.</span></span> <span data-ttu-id="9d747-141">Ändra Platsegenskapen om du vill använda samma plats som resursgruppen för att tillåta för flera platser:</span><span class="sxs-lookup"><span data-stu-id="9d747-141">To allow for more locations, change the location property to use the same location as the resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="9d747-142">Ta bort parametern för lagringskontonamn om du vill undvika att gissa ett uniques namn för storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9d747-142">To avoid having to guess a uniques name for storage account, remove the parameter for the storage account name.</span></span> <span data-ttu-id="9d747-143">Lägga till en parameter för ett namnsuffix för lagring och lagring SKU:</span><span class="sxs-lookup"><span data-stu-id="9d747-143">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

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

<span data-ttu-id="9d747-144">Lägg till en variabel som konstruktioner lagringskontonamnet med funktionen uniqueString:</span><span class="sxs-lookup"><span data-stu-id="9d747-144">Add a variable that constructs the storage account name with the uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="9d747-145">Ange namnet på lagringskontot till variabeln:</span><span class="sxs-lookup"><span data-stu-id="9d747-145">Set the name of the storage account to the variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="9d747-146">Ange SKU: N i parametern:</span><span class="sxs-lookup"><span data-stu-id="9d747-146">Set the SKU to the parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="9d747-147">Nu ser din mall ut så här:</span><span class="sxs-lookup"><span data-stu-id="9d747-147">Your template now looks like:</span></span>

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

<span data-ttu-id="9d747-148">Distribuera om den ändrade mallen.</span><span class="sxs-lookup"><span data-stu-id="9d747-148">Redeploy the modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d747-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9d747-149">Next steps</span></span>
* <span data-ttu-id="9d747-150">Information om hur du använder portalen för att exportera en mall finns i [exportera en Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="9d747-150">For information about using the portal to export a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="9d747-151">Om du vill definiera parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="9d747-151">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="9d747-152">Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="9d747-152">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
