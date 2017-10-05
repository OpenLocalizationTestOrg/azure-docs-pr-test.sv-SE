---
title: "Distribuera ett program på en Azure VM-skalningsuppsättning | Microsoft Docs"
description: "Lär dig att distribuera ett enkelt autoskalningsprogram på en VM-skaluppsttning med en Azure Resource Manager-mall."
services: virtual-machine-scale-sets
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: 07883a33382cc660b043c99872312a9e77228253
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-autoscaling-app-using-a-template"></a><span data-ttu-id="759bc-103">Distribuera en app för automatisk skalning med en mall</span><span class="sxs-lookup"><span data-stu-id="759bc-103">Deploy an autoscaling app using a template</span></span>

<span data-ttu-id="759bc-104">[Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) är ett bra sätt att distribuera grupper av relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="759bc-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way to deploy groups of related resources.</span></span> <span data-ttu-id="759bc-105">Den här kursen bygger på [Distribuera en enkel skaluppsättning](virtual-machine-scale-sets-mvss-start.md) och beskriver hur du distribuerar ett enkelt program med autoskalning på en skaluppsättning med en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="759bc-105">This tutorial builds on [Deploy a simple scale set](virtual-machine-scale-sets-mvss-start.md) and describes how to deploy a simple autoscaling application on a scale set using an Azure Resource Manager template.</span></span>  <span data-ttu-id="759bc-106">Du kan också ställa in autoskalning med PowerShell, CLI eller portalen.</span><span class="sxs-lookup"><span data-stu-id="759bc-106">You can also set up autoscaling using PowerShell, CLI, or the portal.</span></span> <span data-ttu-id="759bc-107">Mer information finns i [Översikt för autoskala](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="759bc-107">For more information, see [Autoscale overview](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

## <a name="two-quickstart-templates"></a><span data-ttu-id="759bc-108">Två snabbstartmallar</span><span class="sxs-lookup"><span data-stu-id="759bc-108">Two quickstart templates</span></span>
<span data-ttu-id="759bc-109">När du distribuerar en skaluppsättning du kan installera ny programvara på en plattformbild med ett [VM-tillägg](../virtual-machines/virtual-machines-windows-extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="759bc-109">When you deploy a scale set you can install new software on a platform image using a [VM Extension](../virtual-machines/virtual-machines-windows-extensions-features.md).</span></span> <span data-ttu-id="759bc-110">Ett VM-tilläggt är ett litet program som innehåller en färdig konfiguration och automatisering av uppgifter på virtuella Azure-datorer, till exempel att distribuera en app.</span><span class="sxs-lookup"><span data-stu-id="759bc-110">A VM extension is a small application that provides post-deployment configuration and automation tasks on Azure virtual machines, such as deploying an app.</span></span> <span data-ttu-id="759bc-111">Två olika exempelmallar finns i [Azure/azure-snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates) som visar hur du distribuerar ett autoskalningsprogram på en skaluppsättning som anges med VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="759bc-111">Two different sample templates are provided in [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) which show how to deploy an autoscaling application onto a scale set using VM extensions.</span></span>

### <a name="python-http-server-on-linux"></a><span data-ttu-id="759bc-112">Python-HTTP-servern på Linux</span><span class="sxs-lookup"><span data-stu-id="759bc-112">Python HTTP server on Linux</span></span>
<span data-ttu-id="759bc-113">Exempelmallen [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) distribuerar ett enkelt autoskalningsprogram som körs på en Linux-skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="759bc-113">The [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sample template deploys a simple autoscaling application running on a Linux scale set.</span></span>  <span data-ttu-id="759bc-114">[Bottle](http://bottlepy.org/docs/dev/), ett Python webwramverk och en enkel HTTP-server distribueras på varje virtuell dator i skaluppsättningen med ett anpassat skript för VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="759bc-114">[Bottle](http://bottlepy.org/docs/dev/), a Python web framework, and a simple HTTP server are deployed on each VM in the scale set using a custom script VM extension.</span></span> <span data-ttu-id="759bc-115">Skaluppsättningen skalar upp när den genomsnittliga CPU-användningen över alla virtuella datorer är större än 60 % och skalar ner när den genomsnittliga CPU-användningen är mindre än 30 %.</span><span class="sxs-lookup"><span data-stu-id="759bc-115">The scale set scales up when average CPU utilization across all VMs is greater than 60% and scales down when the average CPU utilization is less than 30%.</span></span>

<span data-ttu-id="759bc-116">Förutom skaluppsättningsresursen deklarerar exempelmallen *azuredeploy.json* också virtuella nätverk, offentlig IP-adress, belastningsutjämnare och resurser för inställning av autoskala.</span><span class="sxs-lookup"><span data-stu-id="759bc-116">In addition to the scale set resource, the *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span>  <span data-ttu-id="759bc-117">Mer information om hur du skapar dessa resurser i en mall finns i [Linux-skala med autoskala](virtual-machine-scale-sets-linux-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="759bc-117">For more information on creating these resources in a template, see [Linux scale set with autoscale](virtual-machine-scale-sets-linux-autoscale.md).</span></span>

<span data-ttu-id="759bc-118">I mallen *azuredeploy.json* anger `extensionProfile`-egenskapen för `Microsoft.Compute/virtualMachineScaleSets`-resurs ett anpassat skripttillägg.</span><span class="sxs-lookup"><span data-stu-id="759bc-118">In the *azuredeploy.json* template, the `extensionProfile` property of the `Microsoft.Compute/virtualMachineScaleSets` resource specifies a custom script extension.</span></span> <span data-ttu-id="759bc-119">`fileUris` anger platsen för skript.</span><span class="sxs-lookup"><span data-stu-id="759bc-119">`fileUris` specifies the script(s) location.</span></span> <span data-ttu-id="759bc-120">I det här fallet två filer: *workserver.py*, som definierar en enkel HTTP-server och *installserver.sh*, som installerar Bottle och startar HTTP-servern.</span><span class="sxs-lookup"><span data-stu-id="759bc-120">In this case, two files: *workserver.py*, which defines a simple HTTP server, and *installserver.sh*, which installs Bottle and starts the HTTP server.</span></span> <span data-ttu-id="759bc-121">`commandToExecute` anger kommandot som ska köras när skaluppsättning har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="759bc-121">`commandToExecute` specifies the command to run after the scale set has been deployed.</span></span>

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "lapextension",
                "properties": {
                  "publisher": "Microsoft.Azure.Extensions",
                  "type": "CustomScript",
                  "typeHandlerVersion": "2.0",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "fileUris": [
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
                    ],
                    "commandToExecute": "bash installserver.sh"
                  }
                }
              }
            ]
          }
```

### <a name="aspnet-mvc-application-on-windows"></a><span data-ttu-id="759bc-122">ASP.NET MVC-appen i Windows</span><span class="sxs-lookup"><span data-stu-id="759bc-122">ASP.NET MVC application on Windows</span></span>
<span data-ttu-id="759bc-123">Exempelmallen [ASP.NET MVC-appen i Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) distribuerar en enkel ASP.NET MVC-app som körs i IIS på Windows-skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="759bc-123">The [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) sample template deploys a simple ASP.NET MVC app running in IIS on Windows scale set.</span></span>  <span data-ttu-id="759bc-124">IIS och MVC-appen distribueras med VM-tillägget [PowerShell önskad tillståndskonfiguration (DSC)](virtual-machine-scale-sets-dsc.md).</span><span class="sxs-lookup"><span data-stu-id="759bc-124">IIS and the MVC app are deployed using the [PowerShell desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) VM extension.</span></span>  <span data-ttu-id="759bc-125">Skaluppsättningen skalar upp (en VM-instans i taget) när CPU-användning är större än 50 % under 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="759bc-125">The scale set scales up (on VM instance at a time) when CPU utilization is greater than 50% for 5 minutes.</span></span> 

<span data-ttu-id="759bc-126">Förutom skaluppsättningsresursen deklarerar exempelmallen *azuredeploy.json* också virtuella nätverk, offentlig IP-adress, belastningsutjämnare och resurser för inställning av autoskala.</span><span class="sxs-lookup"><span data-stu-id="759bc-126">In addition to the scale set resource, the *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span> <span data-ttu-id="759bc-127">Den här mallen visas också uppgradering av programmet.</span><span class="sxs-lookup"><span data-stu-id="759bc-127">This template also demonstrates application upgrade.</span></span>  <span data-ttu-id="759bc-128">Mer information om hur du skapar dessa resurser i en mall finns i [Windows-skala med autoskala](virtual-machine-scale-sets-windows-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="759bc-128">For more information on creating these resources in a template, see [Windows scale set with autoscale](virtual-machine-scale-sets-windows-autoscale.md).</span></span>

<span data-ttu-id="759bc-129">I mallen *azuredeploy.json* anger resursen `extensionProfile` egenskapen för `Microsoft.Compute/virtualMachineScaleSets` ett [önskad tillståndskonfiguration (DSC)](virtual-machine-scale-sets-dsc.md)-tillägg som installerar IIS och en standard-webbapp från ett WebDeploy-paket.</span><span class="sxs-lookup"><span data-stu-id="759bc-129">In the *azuredeploy.json* template, the `extensionProfile` property of the `Microsoft.Compute/virtualMachineScaleSets` resource specifies a [desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) extension which installs IIS and a default web app from a WebDeploy package.</span></span>  <span data-ttu-id="759bc-130">Skriptet *IISInstall.ps1* installerar IIS på den virtuella datorn och finns i mappen *DSC*.</span><span class="sxs-lookup"><span data-stu-id="759bc-130">The *IISInstall.ps1* script installs IIS on the virtual machine and is found in the *DSC* folder.</span></span>  <span data-ttu-id="759bc-131">MVC-webbappen finns i mappen *WebDeploy*.</span><span class="sxs-lookup"><span data-stu-id="759bc-131">The MVC web app is found in the *WebDeploy* folder.</span></span>  <span data-ttu-id="759bc-132">Sökvägar till installationsskriptet och webbprogrammet har definierats i parametrarna `powershelldscZip` och `webDeployPackage` i filen *azuredeploy.parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="759bc-132">The paths to the install script and the web app are defined in the `powershelldscZip` and `webDeployPackage` parameters in the *azuredeploy.parameters.json* file.</span></span> 

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Powershell.DSC",
                "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.9",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('powershelldscUpdateTagVersion')]",
                  "settings": {
                    "configuration": {
                      "url": "[variables('powershelldscZipFullPath')]",
                      "script": "IISInstall.ps1",
                      "function": "InstallIIS"
                    },
                    "configurationArguments": {
                      "nodeName": "localhost",
                      "WebDeployPackagePath": "[variables('webDeployPackageFullPath')]"
                    }
                  }
                }
              }
            ]
          }
```

## <a name="deploy-the-template"></a><span data-ttu-id="759bc-133">Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="759bc-133">Deploy the template</span></span>
<span data-ttu-id="759bc-134">Det enklaste sättet att distribuera [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) eller mallen [ASP.NET MVC-program på Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) är att använda knappen **Distribuera till Azure** som finns i readme-filerna i GitHub.</span><span class="sxs-lookup"><span data-stu-id="759bc-134">The simplest way to deploy the [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) template is to use the **Deploy to Azure** button found in the in the readme files in GitHub.</span></span>  <span data-ttu-id="759bc-135">Du kan också använda PowerShell eller Azure CLI för att distribuera exempelmallarna.</span><span class="sxs-lookup"><span data-stu-id="759bc-135">You can also use PowerShell or Azure CLI to deploy the sample templates.</span></span>

### <a name="powershell"></a><span data-ttu-id="759bc-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="759bc-136">PowerShell</span></span>
<span data-ttu-id="759bc-137">Kopiera [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) eller filerna [ASP.NET MVC-appen i Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) från GitHub-lagringsplatsen till en mapp på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="759bc-137">Copy the [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) files from the GitHub repo to a folder on your local computer.</span></span>  <span data-ttu-id="759bc-138">Öppna filen *azuredeploy.parameters.json* och uppdatera standardvärdena för parametrarna `vmssName`, `adminUsername` och `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="759bc-138">Open the *azuredeploy.parameters.json* file and update the default values of the `vmssName`, `adminUsername`, and `adminPassword` parameters.</span></span> <span data-ttu-id="759bc-139">Spara följande PowerShell-skript till *deploy.ps1* i samma mapp som mallen *azuredeploy.json*.</span><span class="sxs-lookup"><span data-stu-id="759bc-139">Save the following PowerShell script to *deploy.ps1* in the same folder as the *azuredeploy.json* template.</span></span> <span data-ttu-id="759bc-140">För att distribuera exempelmallen, kör skriptet *deploy.ps1* från en PowerShell-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="759bc-140">To deploy the sample template run the *deploy.ps1* script from a PowerShell command window.</span></span>

```powershell
param(
 [Parameter(Mandatory=$True)]
 [string]
 $subscriptionId,

 [Parameter(Mandatory=$True)]
 [string]
 $resourceGroupName,

 [string]
 $resourceGroupLocation,

 [Parameter(Mandatory=$True)]
 [string]
 $deploymentName,

 [string]
 $templateFilePath = "template.json",

 [string]
 $parametersFilePath = "parameters.json"
)

<#
.SYNOPSIS
    Registers RPs
#>
Function RegisterRP {
    Param(
        [string]$ResourceProviderNamespace
    )

    Write-Host "Registering resource provider '$ResourceProviderNamespace'";
    Register-AzureRmResourceProvider -ProviderNamespace $ResourceProviderNamespace;
}

#******************************************************************************
# Script body
# Execution begins here
#******************************************************************************
$ErrorActionPreference = "Stop"

# sign in
Write-Host "Logging in...";
Login-AzureRmAccount;

# select subscription
Write-Host "Selecting subscription '$subscriptionId'";
Select-AzureRmSubscription -SubscriptionID $subscriptionId;

# Register RPs
$resourceProviders = @("microsoft.compute","microsoft.insights","microsoft.network");
if($resourceProviders.length) {
    Write-Host "Registering resource providers"
    foreach($resourceProvider in $resourceProviders) {
        RegisterRP($resourceProvider);
    }
}

#Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
if(!$resourceGroup)
{
    Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start the deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

### <a name="azure-cli"></a><span data-ttu-id="759bc-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="759bc-141">Azure CLI</span></span>
```azurecli
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# -e: immediately exit if any command has a non-zero exit status
# -o: prevents errors in a pipeline from being masked
# IFS new value is less likely to cause confusing bugs when looping arrays or arguments (e.g. $@)

usage() { echo "Usage: $0 -i <subscriptionId> -g <resourceGroupName> -n <deploymentName> -l <resourceGroupLocation>" 1>&2; exit 1; }

declare subscriptionId=""
declare resourceGroupName=""
declare deploymentName=""
declare resourceGroupLocation=""

# Initialize parameters specified from command line
while getopts ":i:g:n:l:" arg; do
    case "${arg}" in
        i)
            subscriptionId=${OPTARG}
            ;;
        g)
            resourceGroupName=${OPTARG}
            ;;
        n)
            deploymentName=${OPTARG}
            ;;
        l)
            resourceGroupLocation=${OPTARG}
            ;;
        esac
done
shift $((OPTIND-1))

#Prompt for parameters is some required parameters are missing
if [[ -z "$subscriptionId" ]]; then
    echo "Subscription Id:"
    read subscriptionId
    [[ "${subscriptionId:?}" ]]
fi

if [[ -z "$resourceGroupName" ]]; then
    echo "ResourceGroupName:"
    read resourceGroupName
    [[ "${resourceGroupName:?}" ]]
fi

if [[ -z "$deploymentName" ]]; then
    echo "DeploymentName:"
    read deploymentName
fi

if [[ -z "$resourceGroupLocation" ]]; then
    echo "Enter a location below to create a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file to be used
templateFilePath="template.json"

if [ ! -f "$templateFilePath" ]; then
    echo "$templateFilePath not found"
    exit 1
fi

#parameter file path
parametersFilePath="parameters.json"

if [ ! -f "$parametersFilePath" ]; then
    echo "$parametersFilePath not found"
    exit 1
fi

if [ -z "$subscriptionId" ] || [ -z "$resourceGroupName" ] || [ -z "$deploymentName" ]; then
    echo "Either one of subscriptionId, resourceGroupName, deploymentName is empty"
    usage
fi

#login to azure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set the default subscription id
az account set --name $subscriptionId

set +e

#Check for existing RG
az group show $resourceGroupName 1> /dev/null

if [ $? != 0 ]; then
    echo "Resource group with name" $resourceGroupName "could not be found. Creating new resource group.."
    set -e
    (
        set -x
        az group create --name $resourceGroupName --location $resourceGroupLocation 1> /dev/null
    )
    else
    echo "Using existing resource group..."
fi

#Start deployment
echo "Starting deployment..."
(
    set -x
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file $templateFilePath --parameters $parametersFilePath
)

if [ $?  == 0 ];
 then
    echo "Template has been successfully deployed"
fi
```

## <a name="next-steps"></a><span data-ttu-id="759bc-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="759bc-142">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
