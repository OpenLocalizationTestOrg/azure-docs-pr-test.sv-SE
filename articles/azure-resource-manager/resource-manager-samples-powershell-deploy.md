---
title: "Azure PowerShell-skript exempel – distribuera mallen | Microsoft Docs"
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
ms.openlocfilehash: b7a7dda1da653d084e02e6724d2f0cb5aa76807a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-manager-template-deployment---powershell-script"></a><span data-ttu-id="173f4-103">Malldistribution Azure Resource Manager - PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="173f4-103">Azure Resource Manager template deployment - PowerShell script</span></span>

<span data-ttu-id="173f4-104">Det här skriptet distribuerar en Resource Manager-mall till en resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="173f4-104">This script deploys a Resource Manager template to a resource group in your subscription.</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="173f4-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="173f4-105">Sample script</span></span>

```powershell
<#
 .SYNOPSIS
    Deploys a template to Azure

 .DESCRIPTION
    Deploys an Azure Resource Manager template
#>

param (
    [Parameter(Mandatory)]
    #The subscription id where the template will be deployed.
    [string]$SubscriptionId,  

    [Parameter(Mandatory)]
    #The resource group where the template will be deployed. Can be the name of an existing or a new resource group.
    [string]$ResourceGroupName, 

    #Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.
    [string]$ResourceGroupLocation, 

    #The deployment name.
    [Parameter(Mandatory)]
    [string]$DeploymentName,    

    #Path to the template file. Defaults to template.json.
    [string]$TemplateFilePath = "template.json",  

    #Path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    [string]$ParametersFilePath = "parameters.json"
)

$ErrorActionPreference = "Stop"

# Login to Azure and select subscription
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

# Start the deployment
Write-Output "Starting deployment"
if ( Test-Path $ParametersFilePath ) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFilePath -TemplateParameterFile $ParametersFilePath
}
else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFilePath
}
``` 

## <a name="clean-up-deployment"></a><span data-ttu-id="173f4-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="173f4-106">Clean up deployment</span></span> 

<span data-ttu-id="173f4-107">Kör följande kommando för att ta bort resursgruppen och alla dess resurser.</span><span class="sxs-lookup"><span data-stu-id="173f4-107">Run the following command to remove the resource group and all its resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="173f4-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="173f4-108">Script explanation</span></span>

<span data-ttu-id="173f4-109">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="173f4-109">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="173f4-110">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="173f4-110">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="173f4-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="173f4-111">Command</span></span> | <span data-ttu-id="173f4-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="173f4-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="173f4-113">Registrera AzureRmResourceProvider</span><span class="sxs-lookup"><span data-stu-id="173f4-113">Register-AzureRmResourceProvider</span></span>](/powershell/module/azurerm.resources/register-azurermresourceprovider) | <span data-ttu-id="173f4-114">Registrerar en resursleverantör så att dess resurstyper kan distribueras till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="173f4-114">Registers a resource provider so its resource types can be deployed to your subscription.</span></span>  |
| [<span data-ttu-id="173f4-115">Get-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="173f4-115">Get-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/get-azurermresourcegroup) | <span data-ttu-id="173f4-116">Hämtar resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="173f4-116">Gets resource groups.</span></span>  |
| [<span data-ttu-id="173f4-117">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="173f4-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="173f4-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="173f4-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="173f4-119">Ny AzureRmResourceGroupDeployment</span><span class="sxs-lookup"><span data-stu-id="173f4-119">New-AzureRmResourceGroupDeployment</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) | <span data-ttu-id="173f4-120">Lägger till Azure-distribution till en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="173f4-120">Adds an Azure deployment to a resource group.</span></span>  |
| [<span data-ttu-id="173f4-121">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="173f4-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="173f4-122">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="173f4-122">Removes a resource group and all resources contained within.</span></span> |



## <a name="next-steps"></a><span data-ttu-id="173f4-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="173f4-123">Next steps</span></span>
* <span data-ttu-id="173f4-124">En introduktion till att distribuera mallar finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="173f4-124">For an introduction to deploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="173f4-125">Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="173f4-125">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="173f4-126">Om du vill definiera parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="173f4-126">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="173f4-127">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="173f4-127">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

