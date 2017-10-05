---
title: Exportera Resource Manager-mall med Azure CLI | Microsoft Docs
description: "Använda Azure Resource Manager och Azure CLI för att exportera en mall från en resursgrupp."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 617664129a5353e25da1e90c742c4b009db172ef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a><span data-ttu-id="51267-103">Exportera Azure Resource Manager-mallar med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="51267-103">Export Azure Resource Manager templates with Azure CLI</span></span>

<span data-ttu-id="51267-104">Med Resource Manager kan du exportera en Resource Manager-mall från befintliga resurser i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="51267-104">Resource Manager enables you to export a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="51267-105">Du kan använda mallen som genereras för att lära dig mer om mallsyntaxen eller för att automatisera omdistributionen av din lösning om det behövs.</span><span class="sxs-lookup"><span data-stu-id="51267-105">You can use that generated template to learn about the template syntax or to automate the redeployment of your solution as needed.</span></span>

<span data-ttu-id="51267-106">Observera att du kan exportera en mall på två sätt:</span><span class="sxs-lookup"><span data-stu-id="51267-106">It is important to note that there are two different ways to export a template:</span></span>

* <span data-ttu-id="51267-107">Du kan exportera själva mallen som du använde för en distribution.</span><span class="sxs-lookup"><span data-stu-id="51267-107">You can export the actual template that you used for a deployment.</span></span> <span data-ttu-id="51267-108">Den exporterade mallen innehåller alla parametrar och variabler exakt som de visas i den ursprungliga mallen.</span><span class="sxs-lookup"><span data-stu-id="51267-108">The exported template includes all the parameters and variables exactly as they appeared in the original template.</span></span> <span data-ttu-id="51267-109">Den här metoden är användbar när du vill hämta en mall.</span><span class="sxs-lookup"><span data-stu-id="51267-109">This approach is helpful when you need to retrieve a template.</span></span>
* <span data-ttu-id="51267-110">Du kan exportera en mall som representerar resursgruppens aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="51267-110">You can export a template that represents the current state of the resource group.</span></span> <span data-ttu-id="51267-111">Den exporterade mallen baseras inte på en mall som du har använt för distribution.</span><span class="sxs-lookup"><span data-stu-id="51267-111">The exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="51267-112">I stället skapar den en mall som är en ögonblicksbild av resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="51267-112">Instead, it creates a template that is a snapshot of the resource group.</span></span> <span data-ttu-id="51267-113">Den exporterade mallen har många hårdkodade värden och troligen inte så många parametrar som du vanligtvis definierar.</span><span class="sxs-lookup"><span data-stu-id="51267-113">The exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="51267-114">Den här metoden är användbar när du har ändrat resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="51267-114">This approach is useful when you have modified the resource group.</span></span> <span data-ttu-id="51267-115">I detta fall behöver du avbilda resursgruppen som en mall.</span><span class="sxs-lookup"><span data-stu-id="51267-115">Now, you need to capture the resource group as a template.</span></span>

<span data-ttu-id="51267-116">Båda metoderna beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="51267-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="51267-117">Distribuera en lösning</span><span class="sxs-lookup"><span data-stu-id="51267-117">Deploy a solution</span></span>

<span data-ttu-id="51267-118">För att illustrera båda metoderna för att exportera en mall, börja med att distribuera en lösning till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="51267-118">To illustrate both approaches for exporting a template, let's start by deploying a solution to your subscription.</span></span> <span data-ttu-id="51267-119">Om du redan har en resursgrupp i din prenumeration som du vill exportera, behöver du inte distribuera den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="51267-119">If you already have a resource group in your subscription that you want to export, you do not have to deploy this solution.</span></span> <span data-ttu-id="51267-120">Men handlar resten av den här artikeln mallen för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="51267-120">However, the remainder of this article refers to the template for this solution.</span></span> <span data-ttu-id="51267-121">Exempelskriptet distribuerar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="51267-121">The example script deploys a storage account.</span></span>

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="51267-122">Spara mallen från distributionshistoriken</span><span class="sxs-lookup"><span data-stu-id="51267-122">Save template from deployment history</span></span>

<span data-ttu-id="51267-123">Du kan hämta en mall från distributionshistoriken med hjälp av den [az distribution exportera](/cli/azure/group/deployment#export) kommando.</span><span class="sxs-lookup"><span data-stu-id="51267-123">You can retrieve a template from your deployment history by using the [az group deployment export](/cli/azure/group/deployment#export) command.</span></span> <span data-ttu-id="51267-124">I följande exempel sparas den mall som du distribuerar tidigare:</span><span class="sxs-lookup"><span data-stu-id="51267-124">The following example saves the template that you previously deploy:</span></span>

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

<span data-ttu-id="51267-125">Returnerar den mallen.</span><span class="sxs-lookup"><span data-stu-id="51267-125">It returns the template.</span></span> <span data-ttu-id="51267-126">Kopiera JSON och spara som en fil.</span><span class="sxs-lookup"><span data-stu-id="51267-126">Copy the JSON, and save as a file.</span></span> <span data-ttu-id="51267-127">Observera att det är exakt mallen som du använde för distribution.</span><span class="sxs-lookup"><span data-stu-id="51267-127">Notice that it is the exact template you used for deployment.</span></span> <span data-ttu-id="51267-128">Parametrar och variabler matchar mallen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="51267-128">The parameters and variables match the template from GitHub.</span></span> <span data-ttu-id="51267-129">Du kan distribuera den här mallen.</span><span class="sxs-lookup"><span data-stu-id="51267-129">You can redeploy this template.</span></span>


## <a name="export-resource-group-as-template"></a><span data-ttu-id="51267-130">Exportera resursgrupp som mall</span><span class="sxs-lookup"><span data-stu-id="51267-130">Export resource group as template</span></span>

<span data-ttu-id="51267-131">I stället för att hämta en mall från distributionshistoriken, kan du hämta en mall som representerar det aktuella tillståndet för en resursgrupp med hjälp av den [az exportera](/cli/azure/group#export) kommando.</span><span class="sxs-lookup"><span data-stu-id="51267-131">Instead of retrieving a template from the deployment history, you can retrieve a template that represents the current state of a resource group by using the [az group export](/cli/azure/group#export) command.</span></span> <span data-ttu-id="51267-132">Du kan använda det här kommandot när du har gjort många ändringar i resursgruppen och inga befintliga mallen som representerar alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="51267-132">You use this command when you have made many changes to your resource group and no existing template represents all the changes.</span></span>

```azurecli
az group export --name ExampleGroup
```

<span data-ttu-id="51267-133">Returnerar den mallen.</span><span class="sxs-lookup"><span data-stu-id="51267-133">It returns the template.</span></span> <span data-ttu-id="51267-134">Kopiera JSON och spara som en fil.</span><span class="sxs-lookup"><span data-stu-id="51267-134">Copy the JSON, and save as a file.</span></span> <span data-ttu-id="51267-135">Observera att det skiljer sig från mallen i GitHub.</span><span class="sxs-lookup"><span data-stu-id="51267-135">Notice that it is different than the template in GitHub.</span></span> <span data-ttu-id="51267-136">Det har olika parametrar och inga variabler.</span><span class="sxs-lookup"><span data-stu-id="51267-136">It has different parameters and no variables.</span></span> <span data-ttu-id="51267-137">SKU-lagring och plats är hårdkodat till värden.</span><span class="sxs-lookup"><span data-stu-id="51267-137">The storage SKU and location are hard-coded to values.</span></span> <span data-ttu-id="51267-138">I följande exempel visas den exporterade mallen, men mallen har ett något annorlunda parameternamn:</span><span class="sxs-lookup"><span data-stu-id="51267-138">The following example shows the exported template, but your template has a slightly different parameter name:</span></span>

```json
{
  "parameters": {
    "storageAccounts_mcyzaljiv7qncstandardsa_name": {
      "type": "String",
      "defaultValue": null
    }
  },
  "variables": {},
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "resources": [
    {
      "tags": {},
      "dependsOn": [],
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "type": "Microsoft.Storage/storageAccounts",
      "location": "centralus",
      "name": "[parameters('storageAccounts_mcyzaljiv7qncstandardsa_name')]",
      "properties": {}
    }
  ],
  "contentVersion": "1.0.0.0"
}
```

<span data-ttu-id="51267-139">Du kan distribuera den här mallen, men det krävs ett unikt namn för storage-konto för att gissa.</span><span class="sxs-lookup"><span data-stu-id="51267-139">You can redeploy this template, but it requires guessing a unique name for the storage account.</span></span> <span data-ttu-id="51267-140">Namnet på parametern är något annorlunda.</span><span class="sxs-lookup"><span data-stu-id="51267-140">The name of your parameter is slightly different.</span></span>

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
```

## <a name="customize-exported-template"></a><span data-ttu-id="51267-141">Anpassa exporterad mall</span><span class="sxs-lookup"><span data-stu-id="51267-141">Customize exported template</span></span>

<span data-ttu-id="51267-142">Du kan ändra den här mallen för att göra det enklare att använda och mer flexibelt.</span><span class="sxs-lookup"><span data-stu-id="51267-142">You can modify this template to make it easier to use and more flexible.</span></span> <span data-ttu-id="51267-143">Ändra Platsegenskapen om du vill använda samma plats som resursgruppen för att tillåta för flera platser:</span><span class="sxs-lookup"><span data-stu-id="51267-143">To allow for more locations, change the location property to use the same location as the resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="51267-144">Ta bort parametern för lagringskontonamn om du vill undvika att gissa ett uniques namn för storage-konto.</span><span class="sxs-lookup"><span data-stu-id="51267-144">To avoid having to guess a uniques name for storage account, remove the parameter for the storage account name.</span></span> <span data-ttu-id="51267-145">Lägga till en parameter för ett namnsuffix för lagring och lagring SKU:</span><span class="sxs-lookup"><span data-stu-id="51267-145">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

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

<span data-ttu-id="51267-146">Lägg till en variabel som konstruktioner lagringskontonamnet med funktionen uniqueString:</span><span class="sxs-lookup"><span data-stu-id="51267-146">Add a variable that constructs the storage account name with the uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="51267-147">Ange namnet på lagringskontot till variabeln:</span><span class="sxs-lookup"><span data-stu-id="51267-147">Set the name of the storage account to the variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="51267-148">Ange SKU: N i parametern:</span><span class="sxs-lookup"><span data-stu-id="51267-148">Set the SKU to the parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="51267-149">Nu ser din mall ut så här:</span><span class="sxs-lookup"><span data-stu-id="51267-149">Your template now looks like:</span></span>

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

<span data-ttu-id="51267-150">Distribuera om den ändrade mallen.</span><span class="sxs-lookup"><span data-stu-id="51267-150">Redeploy the modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51267-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51267-151">Next steps</span></span>
* <span data-ttu-id="51267-152">Information om hur du använder portalen för att exportera en mall finns i [exportera en Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="51267-152">For information about using the portal to export a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="51267-153">Om du vill definiera parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="51267-153">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="51267-154">Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="51267-154">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>