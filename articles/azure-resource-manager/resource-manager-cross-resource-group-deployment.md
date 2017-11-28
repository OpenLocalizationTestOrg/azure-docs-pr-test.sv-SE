---
title: Distribuera Azure-resurser till flera resursgrupper | Microsoft Docs
description: "Visar hur du kan arbeta mer än en Azure-resursgrupp under distributionen."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: tomfitz
ms.openlocfilehash: d8b041213b269775175a810e585103d3c538557f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a><span data-ttu-id="d375c-103">Distribuera Azure-resurser till mer än en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d375c-103">Deploy Azure resources to more than one resource group</span></span>

<span data-ttu-id="d375c-104">Normalt distribuerar du alla resurser i mallen som en enskild resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d375c-104">Typically, you deploy all the resources in your template to a single resource group.</span></span> <span data-ttu-id="d375c-105">Det finns emellertid scenarier där du vill distribuera en uppsättning resurser tillsammans men placera dem i olika resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="d375c-105">However, there are scenarios where you want to deploy a set of resources together but place them in different resource groups.</span></span> <span data-ttu-id="d375c-106">Exempelvis kanske du vill distribuera den säkerhetskopiera virtuella för Azure Site Recovery till en separat resursgrupp och plats.</span><span class="sxs-lookup"><span data-stu-id="d375c-106">For example, you may want to deploy the backup virtual machine for Azure Site Recovery to a separate resource group and location.</span></span> <span data-ttu-id="d375c-107">Resource Manager kan du använda kapslade mallar för olika resursgrupper än den resursgrupp som används för den överordnade mallen.</span><span class="sxs-lookup"><span data-stu-id="d375c-107">Resource Manager enables you to use nested templates to target different resource groups than the resource group used for the parent template.</span></span>

<span data-ttu-id="d375c-108">Resursgruppen är en behållare för livscykeln för programmet och dess mängd resurser.</span><span class="sxs-lookup"><span data-stu-id="d375c-108">The resource group is the lifecycle container for the application and its collection of resources.</span></span> <span data-ttu-id="d375c-109">Du skapar resursgruppen utanför mallen och ange resursgrupp som mål under distributionen.</span><span class="sxs-lookup"><span data-stu-id="d375c-109">You create the resource group outside of the template, and specify the resource group to target during deployment.</span></span> <span data-ttu-id="d375c-110">En introduktion till resursgrupper finns [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d375c-110">For an introduction to resource groups, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="example-template"></a><span data-ttu-id="d375c-111">Exempelmall</span><span class="sxs-lookup"><span data-stu-id="d375c-111">Example template</span></span>

<span data-ttu-id="d375c-112">Om du vill ange en annan resurs, måste du använda en mall för kapslade eller länkade under distributionen.</span><span class="sxs-lookup"><span data-stu-id="d375c-112">To target a different resource, you must use a nested or linked template during deployment.</span></span> <span data-ttu-id="d375c-113">Den `Microsoft.Resources/deployments` resurstypen ger en `resourceGroup` parameter som kan du ange en annan resursgrupp för den kapslade distributionen.</span><span class="sxs-lookup"><span data-stu-id="d375c-113">The `Microsoft.Resources/deployments` resource type provides a `resourceGroup` parameter that enables you to specify a different resource group for the nested deployment.</span></span> <span data-ttu-id="d375c-114">Alla resursgrupper måste finnas innan du kör distributionen.</span><span class="sxs-lookup"><span data-stu-id="d375c-114">All the resource groups must exist before running the deployment.</span></span> <span data-ttu-id="d375c-115">I följande exempel distribuerar två lagringskonton - en i resursgruppen som anges under distributionen, och en i en resursgrupp med namnet `crossResourceGroupDeployment`:</span><span class="sxs-lookup"><span data-stu-id="d375c-115">The following example deploys two storage accounts - one in the resource group specified during deployment, and one in a resource group named `crossResourceGroupDeployment`:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

<span data-ttu-id="d375c-116">Om du ställer in `resourceGroup` till namnet på en resursgrupp som inte finns misslyckas distributionen.</span><span class="sxs-lookup"><span data-stu-id="d375c-116">If you set `resourceGroup` to the name of a resource group that does not exist, the deployment fails.</span></span> <span data-ttu-id="d375c-117">Om du inte anger ett värde för `resourceGroup`, Resource Manager använder överordnade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d375c-117">If you do not provide a value for `resourceGroup`, Resource Manager uses the parent resource group.</span></span>  

## <a name="deploy-the-template"></a><span data-ttu-id="d375c-118">Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="d375c-118">Deploy the template</span></span>

<span data-ttu-id="d375c-119">Du kan använda portalen, Azure PowerShell eller Azure CLI för att distribuera mallen exempel.</span><span class="sxs-lookup"><span data-stu-id="d375c-119">To deploy the example template, you can use the portal, Azure PowerShell, or Azure CLI.</span></span> <span data-ttu-id="d375c-120">För Azure PowerShell eller Azure CLI, måste du använda en Versionspost från maj 2017 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d375c-120">For Azure PowerShell or Azure CLI, you must use a release from May 2017 or later.</span></span> <span data-ttu-id="d375c-121">I exemplen antar vi att du har sparat mallen lokalt som en fil med namnet **crossrgdeployment.json**.</span><span class="sxs-lookup"><span data-stu-id="d375c-121">The examples assume you have saved the template locally as a file named **crossrgdeployment.json**.</span></span>

<span data-ttu-id="d375c-122">För PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d375c-122">For PowerShell:</span></span>

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

<span data-ttu-id="d375c-123">För Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="d375c-123">For Azure CLI:</span></span>

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

<span data-ttu-id="d375c-124">När distributionen är klar kan du se två resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="d375c-124">After deployment completes, you see two resource groups.</span></span> <span data-ttu-id="d375c-125">Varje resursgrupp innehåller ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d375c-125">Each resource group contains a storage account.</span></span>

## <a name="use-resourcegroup-function"></a><span data-ttu-id="d375c-126">Funktionen resourceGroup()</span><span class="sxs-lookup"><span data-stu-id="d375c-126">Use resourceGroup() function</span></span>

<span data-ttu-id="d375c-127">Mellan distributionen av resursgrupper, för den [resouceGroup() funktionen](resource-group-template-functions-resource.md#resourcegroup) matchar på olika sätt beroende på hur du anger den kapslade mallen.</span><span class="sxs-lookup"><span data-stu-id="d375c-127">For cross resource group deployments, the [resouceGroup() function](resource-group-template-functions-resource.md#resourcegroup) resolves differently based on how you specify the nested template.</span></span> 

<span data-ttu-id="d375c-128">Om du bäddar in en mall i en annan mall löser resouceGroup() i den kapslade mallen till den överordnade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d375c-128">If you embed one template within another template, resouceGroup() in the nested template resolves to the parent resource group.</span></span> <span data-ttu-id="d375c-129">Ett inbäddat mallen använder följande format:</span><span class="sxs-lookup"><span data-stu-id="d375c-129">An embedded template uses the following format:</span></span>

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

<span data-ttu-id="d375c-130">Om du länkar till en separat mall löser resouceGroup() i mallen länkade till den kapslade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d375c-130">If you link to a separate template, resouceGroup() in the linked template resolves to the nested resource group.</span></span> <span data-ttu-id="d375c-131">En länkad mall används följande format:</span><span class="sxs-lookup"><span data-stu-id="d375c-131">A linked template uses the following format:</span></span>

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="d375c-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d375c-132">Next steps</span></span>

* <span data-ttu-id="d375c-133">Information om hur du definierar parametrar i mallen finns [förstå struktur och syntaxen för Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d375c-133">To understand how to define parameters in your template, see [Understand the structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="d375c-134">Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="d375c-134">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="d375c-135">Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="d375c-135">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
