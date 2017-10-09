---
title: aaaAzure PowerShell skriptexempel - distribuera mallen | Microsoft Docs
description: "Exempelskript för att distribuera en Azure Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 536b8ccecad4ed8a4c4a4139c6bf4600e2eb9405
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-template-deployment---powershell-script"></a><span data-ttu-id="df016-103">Malldistribution Azure Resource Manager - PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="df016-103">Azure Resource Manager template deployment - PowerShell script</span></span>

<span data-ttu-id="df016-104">Det här skriptet distribuerar en Resource Manager mallen tooa resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="df016-104">This script deploys a Resource Manager template tooa resource group in your subscription.</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="df016-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="df016-105">Sample script</span></span>

```powershell
<#
 .SYNOPSIS
    Deploys a template tooAzure

 .DESCRIPTION
    Deploys an Azure Resource Manager template
#>

param (
    [Parameter(Mandatory)]
    #hello subscription id where hello template will be deployed.
    [string]$SubscriptionId,  

    [Parameter(Mandatory)]
    #hello resource group where hello template will be deployed. Can be hello name of an existing or a new resource group.
    [string]$ResourceGroupName, 

    #Optional, a resource group location. If specified, will try toocreate a new resource group in this location. If not specified, assumes resource group is existing.
    [string]$ResourceGroupLocation, 

    #hello deployment name.
    [Parameter(Mandatory)]
    [string]$DeploymentName,    

    #Path toohello template file. Defaults tootemplate.json.
    [string]$TemplateFilePath = "template.json",  

    #Path toohello parameters file. Defaults tooparameters.json. If file is not found, will prompt for parameter values based on template.
    [string]$ParametersFilePath = "parameters.json"
)

$ErrorActionPreference = "Stop"

# Login tooAzure and select subscription
Write-Output "Logging in"
Login-AzureRmAccount
Write-Output "Selecting subscription '$SubscriptionId'"
Select-AzureRmSubscription -SubscriptionID $SubscriptionId

# Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $ResourceGroupName -ErrorAction SilentlyContinue
if ( -not $ResourceGroup ) {
    Write-Output "Could not find resource group '$ResourceGroupName' - will create it"
    if ( -not $ResourceGroupLocation ) {
        $ResourceGroupLocation = Read-Host -Prompt 'Enter location for resource group'
    }
    Write-Output "Creating resource group '$ResourceGroupName' in location '$ResourceGroupLocation'"
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else {
    Write-Output "Using existing resource group '$ResourceGroupName'"
}

# Start hello deployment
Write-Output "Starting deployment"
if ( Test-Path $ParametersFilePath ) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFilePath -TemplateParameterFile $ParametersFilePath
}
else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFilePath
}
``` 

## <a name="clean-up-deployment"></a><span data-ttu-id="df016-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="df016-106">Clean up deployment</span></span> 

<span data-ttu-id="df016-107">Hello kör följande kommando tooremove hello resursgruppen och alla dess resurser.</span><span class="sxs-lookup"><span data-stu-id="df016-107">Run hello following command tooremove hello resource group and all its resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="df016-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="df016-108">Script explanation</span></span>

<span data-ttu-id="df016-109">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="df016-109">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="df016-110">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="df016-110">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="df016-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="df016-111">Command</span></span> | <span data-ttu-id="df016-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="df016-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="df016-113">Registrera AzureRmResourceProvider</span><span class="sxs-lookup"><span data-stu-id="df016-113">Register-AzureRmResourceProvider</span></span>](/powershell/module/azurerm.resources/register-azurermresourceprovider) | <span data-ttu-id="df016-114">Registrerar en resursleverantör så att dess resurstyper kan vara distribuerade tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="df016-114">Registers a resource provider so its resource types can be deployed tooyour subscription.</span></span>  |
| [<span data-ttu-id="df016-115">Get-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="df016-115">Get-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/get-azurermresourcegroup) | <span data-ttu-id="df016-116">Hämtar resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="df016-116">Gets resource groups.</span></span>  |
| [<span data-ttu-id="df016-117">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="df016-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="df016-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="df016-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="df016-119">Ny AzureRmResourceGroupDeployment</span><span class="sxs-lookup"><span data-stu-id="df016-119">New-AzureRmResourceGroupDeployment</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) | <span data-ttu-id="df016-120">Lägger till en Azure-distribution tooa resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="df016-120">Adds an Azure deployment tooa resource group.</span></span>  |
| [<span data-ttu-id="df016-121">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="df016-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="df016-122">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="df016-122">Removes a resource group and all resources contained within.</span></span> |



## <a name="next-steps"></a><span data-ttu-id="df016-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df016-123">Next steps</span></span>
* <span data-ttu-id="df016-124">En introduktion toodeploying mallar, se [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="df016-124">For an introduction toodeploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="df016-125">Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="df016-125">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="df016-126">toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="df016-126">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="df016-127">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="df016-127">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

