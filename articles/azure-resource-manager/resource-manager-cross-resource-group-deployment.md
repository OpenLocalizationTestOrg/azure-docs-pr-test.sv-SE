---
title: aaaDeploy Azure-resurser toomultiple resursgrupper | Microsoft Docs
description: "Visar hur tootarget mer än en Azure-resurs gruppera under distributionen."
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
ms.openlocfilehash: 93a39a26e0ca18dfcb5c6e8de95c38a64186d6de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-resources-toomore-than-one-resource-group"></a><span data-ttu-id="7f363-103">Distribuera Azure-resurser toomore än en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7f363-103">Deploy Azure resources toomore than one resource group</span></span>

<span data-ttu-id="7f363-104">Normalt distribuerar du alla hello resurser i din mall tooa enskild resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f363-104">Typically, you deploy all hello resources in your template tooa single resource group.</span></span> <span data-ttu-id="7f363-105">Det finns emellertid scenarier där du vill toodeploy en uppsättning resurser tillsammans men placera dem i olika resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="7f363-105">However, there are scenarios where you want toodeploy a set of resources together but place them in different resource groups.</span></span> <span data-ttu-id="7f363-106">Du kan till exempel vill toodeploy hello säkerhetskopiering virtuella datorn för Azure Site Recovery tooa separat resursgrupp och plats.</span><span class="sxs-lookup"><span data-stu-id="7f363-106">For example, you may want toodeploy hello backup virtual machine for Azure Site Recovery tooa separate resource group and location.</span></span> <span data-ttu-id="7f363-107">Resource Manager kan toouse kapslade mallar tootarget olika resursgrupper än hello resursgruppens namn används för hello överordnade mallen.</span><span class="sxs-lookup"><span data-stu-id="7f363-107">Resource Manager enables you toouse nested templates tootarget different resource groups than hello resource group used for hello parent template.</span></span>

<span data-ttu-id="7f363-108">hello resursgrupp är hello livscykel behållaren för hello programmet och dess mängd resurser.</span><span class="sxs-lookup"><span data-stu-id="7f363-108">hello resource group is hello lifecycle container for hello application and its collection of resources.</span></span> <span data-ttu-id="7f363-109">Du skapar hello resursgruppen utanför hello mallen och ange hello resurs grupp tootarget under distributionen.</span><span class="sxs-lookup"><span data-stu-id="7f363-109">You create hello resource group outside of hello template, and specify hello resource group tootarget during deployment.</span></span> <span data-ttu-id="7f363-110">En introduktion tooresource grupper, se [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7f363-110">For an introduction tooresource groups, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="example-template"></a><span data-ttu-id="7f363-111">Exempelmall</span><span class="sxs-lookup"><span data-stu-id="7f363-111">Example template</span></span>

<span data-ttu-id="7f363-112">tootarget en annan resurs, måste du använda en mall för kapslade eller länkade under distributionen.</span><span class="sxs-lookup"><span data-stu-id="7f363-112">tootarget a different resource, you must use a nested or linked template during deployment.</span></span> <span data-ttu-id="7f363-113">Hej `Microsoft.Resources/deployments` resurstypen ger en `resourceGroup` parameter som kan användas toospecify en annan resursgrupp för hello kapslade distributionen.</span><span class="sxs-lookup"><span data-stu-id="7f363-113">hello `Microsoft.Resources/deployments` resource type provides a `resourceGroup` parameter that enables you toospecify a different resource group for hello nested deployment.</span></span> <span data-ttu-id="7f363-114">Alla resursgrupper för hello måste finnas innan du kör hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="7f363-114">All hello resource groups must exist before running hello deployment.</span></span> <span data-ttu-id="7f363-115">hello följande exempel distribuerar två lagringskonton - en i hello resursgruppen som anges under distributionen, och en i en resursgrupp med namnet `crossResourceGroupDeployment`:</span><span class="sxs-lookup"><span data-stu-id="7f363-115">hello following example deploys two storage accounts - one in hello resource group specified during deployment, and one in a resource group named `crossResourceGroupDeployment`:</span></span>

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

<span data-ttu-id="7f363-116">Om du ställer in `resourceGroup` toohello namnet på en resursgrupp som inte finns, hello distributionen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="7f363-116">If you set `resourceGroup` toohello name of a resource group that does not exist, hello deployment fails.</span></span> <span data-ttu-id="7f363-117">Om du inte anger ett värde för `resourceGroup`, Resource Manager använder hello överordnade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7f363-117">If you do not provide a value for `resourceGroup`, Resource Manager uses hello parent resource group.</span></span>  

## <a name="deploy-hello-template"></a><span data-ttu-id="7f363-118">Distribuera hello mall</span><span class="sxs-lookup"><span data-stu-id="7f363-118">Deploy hello template</span></span>

<span data-ttu-id="7f363-119">toodeploy hello exempel mallen som du kan använda hello-portalen, Azure PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="7f363-119">toodeploy hello example template, you can use hello portal, Azure PowerShell, or Azure CLI.</span></span> <span data-ttu-id="7f363-120">För Azure PowerShell eller Azure CLI, måste du använda en Versionspost från maj 2017 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7f363-120">For Azure PowerShell or Azure CLI, you must use a release from May 2017 or later.</span></span> <span data-ttu-id="7f363-121">hello exempel förutsätter att du har sparat hello mall lokalt som en fil med namnet **crossrgdeployment.json**.</span><span class="sxs-lookup"><span data-stu-id="7f363-121">hello examples assume you have saved hello template locally as a file named **crossrgdeployment.json**.</span></span>

<span data-ttu-id="7f363-122">För PowerShell:</span><span class="sxs-lookup"><span data-stu-id="7f363-122">For PowerShell:</span></span>

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

<span data-ttu-id="7f363-123">För Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="7f363-123">For Azure CLI:</span></span>

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

<span data-ttu-id="7f363-124">När distributionen är klar kan du se två resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="7f363-124">After deployment completes, you see two resource groups.</span></span> <span data-ttu-id="7f363-125">Varje resursgrupp innehåller ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f363-125">Each resource group contains a storage account.</span></span>

## <a name="use-resourcegroup-function"></a><span data-ttu-id="7f363-126">Funktionen resourceGroup()</span><span class="sxs-lookup"><span data-stu-id="7f363-126">Use resourceGroup() function</span></span>

<span data-ttu-id="7f363-127">För mellan distributionen av resursgrupper, hello [resouceGroup() funktionen](resource-group-template-functions-resource.md#resourcegroup) matchar på olika sätt beroende på hur du anger hello kapslade mallen.</span><span class="sxs-lookup"><span data-stu-id="7f363-127">For cross resource group deployments, hello [resouceGroup() function](resource-group-template-functions-resource.md#resourcegroup) resolves differently based on how you specify hello nested template.</span></span> 

<span data-ttu-id="7f363-128">Om du bäddar in en mall i en annan mall löser resouceGroup() i hello kapslade mallen toohello överordnade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7f363-128">If you embed one template within another template, resouceGroup() in hello nested template resolves toohello parent resource group.</span></span> <span data-ttu-id="7f363-129">Ett inbäddat mallen använder hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7f363-129">An embedded template uses hello following format:</span></span>

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers tooparent resource group
    }
}
```

<span data-ttu-id="7f363-130">Om du länka tooa separat mall löser resouceGroup() i hello länkade mallen toohello kapslade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7f363-130">If you link tooa separate template, resouceGroup() in hello linked template resolves toohello nested resource group.</span></span> <span data-ttu-id="7f363-131">En länkad mall använder hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7f363-131">A linked template uses hello following format:</span></span>

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers toolinked resource group
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="7f363-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f363-132">Next steps</span></span>

* <span data-ttu-id="7f363-133">hur toodefine parametrarna i mallen, se toounderstand [förstå hello struktur och syntaxen för Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7f363-133">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="7f363-134">Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="7f363-134">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="7f363-135">Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="7f363-135">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
