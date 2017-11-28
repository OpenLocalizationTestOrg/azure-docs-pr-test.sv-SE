---
title: "aaaDeploy en app på en skaluppsättning för virtuell dator i Azure | Microsoft Docs"
description: "Lär dig toodeploy ett enkelt autoskalning program på en virtuell dator skala anges med en Azure Resource Manager-mall."
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
ms.openlocfilehash: 6fccc310312cabfcdddfcbcd2d154fc5cc440417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-autoscaling-app-using-a-template"></a><span data-ttu-id="ecf85-103">Distribuera en app för automatisk skalning med en mall</span><span class="sxs-lookup"><span data-stu-id="ecf85-103">Deploy an autoscaling app using a template</span></span>

<span data-ttu-id="ecf85-104">[Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) är ett bra sätt toodeploy grupper av relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="ecf85-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way toodeploy groups of related resources.</span></span> <span data-ttu-id="ecf85-105">Den här kursen bygger på [distribuera en enkel skaluppsättning](virtual-machine-scale-sets-mvss-start.md) och beskriver hur toodeploy ett enkelt autoskalning program på en skala anges med en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="ecf85-105">This tutorial builds on [Deploy a simple scale set](virtual-machine-scale-sets-mvss-start.md) and describes how toodeploy a simple autoscaling application on a scale set using an Azure Resource Manager template.</span></span>  <span data-ttu-id="ecf85-106">Du kan också ställa in autoskalning med PowerShell, CLI eller hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="ecf85-106">You can also set up autoscaling using PowerShell, CLI, or hello portal.</span></span> <span data-ttu-id="ecf85-107">Mer information finns i [Översikt för autoskala](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ecf85-107">For more information, see [Autoscale overview](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

## <a name="two-quickstart-templates"></a><span data-ttu-id="ecf85-108">Två snabbstartmallar</span><span class="sxs-lookup"><span data-stu-id="ecf85-108">Two quickstart templates</span></span>
<span data-ttu-id="ecf85-109">När du distribuerar en skaluppsättning du kan installera ny programvara på en plattformbild med ett [VM-tillägg](../virtual-machines/virtual-machines-windows-extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="ecf85-109">When you deploy a scale set you can install new software on a platform image using a [VM Extension](../virtual-machines/virtual-machines-windows-extensions-features.md).</span></span> <span data-ttu-id="ecf85-110">Ett VM-tilläggt är ett litet program som innehåller en färdig konfiguration och automatisering av uppgifter på virtuella Azure-datorer, till exempel att distribuera en app.</span><span class="sxs-lookup"><span data-stu-id="ecf85-110">A VM extension is a small application that provides post-deployment configuration and automation tasks on Azure virtual machines, such as deploying an app.</span></span> <span data-ttu-id="ecf85-111">Två olika exempelmallarna finns i [Azure/azure--snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates) som visar hur toodeploy ett autoskalning program på en skala anges med VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="ecf85-111">Two different sample templates are provided in [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) which show how toodeploy an autoscaling application onto a scale set using VM extensions.</span></span>

### <a name="python-http-server-on-linux"></a><span data-ttu-id="ecf85-112">Python-HTTP-servern på Linux</span><span class="sxs-lookup"><span data-stu-id="ecf85-112">Python HTTP server on Linux</span></span>
<span data-ttu-id="ecf85-113">Hej [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) exempelmall distribuerar en enkel autoskalning program som körs på en Linux-skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ecf85-113">hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sample template deploys a simple autoscaling application running on a Linux scale set.</span></span>  <span data-ttu-id="ecf85-114">[Bottle](http://bottlepy.org/docs/dev/), en Python web framework och en enkel HTTP-server distribueras på varje virtuell dator i hello skala anges med ett anpassat skript för VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="ecf85-114">[Bottle](http://bottlepy.org/docs/dev/), a Python web framework, and a simple HTTP server are deployed on each VM in hello scale set using a custom script VM extension.</span></span> <span data-ttu-id="ecf85-115">Hej skaluppsättning skalar när Genomsnittlig CPU-belastning över alla virtuella datorer som är större än 60% och skalas när hello Genomsnittlig CPU-användning är mindre än 30%.</span><span class="sxs-lookup"><span data-stu-id="ecf85-115">hello scale set scales up when average CPU utilization across all VMs is greater than 60% and scales down when hello average CPU utilization is less than 30%.</span></span>

<span data-ttu-id="ecf85-116">Dessutom toohello skaluppsättning resurs, hello *azuredeploy.json* exempelmall deklarerar också virtuella nätverk, offentlig IP-adress, belastningsutjämnare och Autoskala inställningar resurser.</span><span class="sxs-lookup"><span data-stu-id="ecf85-116">In addition toohello scale set resource, hello *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span>  <span data-ttu-id="ecf85-117">Mer information om hur du skapar dessa resurser i en mall finns i [Linux-skala med autoskala](virtual-machine-scale-sets-linux-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="ecf85-117">For more information on creating these resources in a template, see [Linux scale set with autoscale](virtual-machine-scale-sets-linux-autoscale.md).</span></span>

<span data-ttu-id="ecf85-118">I hello *azuredeploy.json* mall, hello `extensionProfile` -egenskapen för hello `Microsoft.Compute/virtualMachineScaleSets` resurs anger tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="ecf85-118">In hello *azuredeploy.json* template, hello `extensionProfile` property of hello `Microsoft.Compute/virtualMachineScaleSets` resource specifies a custom script extension.</span></span> <span data-ttu-id="ecf85-119">`fileUris`Anger hello skript plats.</span><span class="sxs-lookup"><span data-stu-id="ecf85-119">`fileUris` specifies hello script(s) location.</span></span> <span data-ttu-id="ecf85-120">I det här fallet två filer: *workserver.py*, som definierar en enkel HTTP-server och *installserver.sh*, som installerar Bottle och startar hello HTTP-servern.</span><span class="sxs-lookup"><span data-stu-id="ecf85-120">In this case, two files: *workserver.py*, which defines a simple HTTP server, and *installserver.sh*, which installs Bottle and starts hello HTTP server.</span></span> <span data-ttu-id="ecf85-121">`commandToExecute`Anger hello kommandot toorun när hello skaluppsättning har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="ecf85-121">`commandToExecute` specifies hello command toorun after hello scale set has been deployed.</span></span>

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

### <a name="aspnet-mvc-application-on-windows"></a><span data-ttu-id="ecf85-122">ASP.NET MVC-appen i Windows</span><span class="sxs-lookup"><span data-stu-id="ecf85-122">ASP.NET MVC application on Windows</span></span>
<span data-ttu-id="ecf85-123">Hej [ASP.NET MVC-program på Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) exempelmall distribuerar en enkel ASP.NET MVC-app som körs i IIS på Windows skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ecf85-123">hello [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) sample template deploys a simple ASP.NET MVC app running in IIS on Windows scale set.</span></span>  <span data-ttu-id="ecf85-124">IIS och hello MVC-app distribueras med hello [PowerShell önskad tillståndskonfiguration (DSC)](virtual-machine-scale-sets-dsc.md) VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="ecf85-124">IIS and hello MVC app are deployed using hello [PowerShell desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) VM extension.</span></span>  <span data-ttu-id="ecf85-125">hello skala konfigurera skalor (på VM-instans i taget) när CPU-användning är större än 50% under 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="ecf85-125">hello scale set scales up (on VM instance at a time) when CPU utilization is greater than 50% for 5 minutes.</span></span> 

<span data-ttu-id="ecf85-126">Dessutom toohello skaluppsättning resurs, hello *azuredeploy.json* exempelmall deklarerar också virtuella nätverk, offentlig IP-adress, belastningsutjämnare och Autoskala inställningar resurser.</span><span class="sxs-lookup"><span data-stu-id="ecf85-126">In addition toohello scale set resource, hello *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span> <span data-ttu-id="ecf85-127">Den här mallen visas också uppgradering av programmet.</span><span class="sxs-lookup"><span data-stu-id="ecf85-127">This template also demonstrates application upgrade.</span></span>  <span data-ttu-id="ecf85-128">Mer information om hur du skapar dessa resurser i en mall finns i [Windows-skala med autoskala](virtual-machine-scale-sets-windows-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="ecf85-128">For more information on creating these resources in a template, see [Windows scale set with autoscale](virtual-machine-scale-sets-windows-autoscale.md).</span></span>

<span data-ttu-id="ecf85-129">I hello *azuredeploy.json* mall, hello `extensionProfile` -egenskapen för hello `Microsoft.Compute/virtualMachineScaleSets` resurs anger en [önskad tillståndskonfiguration (DSC)](virtual-machine-scale-sets-dsc.md) -tillägg som installerar IIS och standard webbprogrammet från en WebDeploy-paketet.</span><span class="sxs-lookup"><span data-stu-id="ecf85-129">In hello *azuredeploy.json* template, hello `extensionProfile` property of hello `Microsoft.Compute/virtualMachineScaleSets` resource specifies a [desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) extension which installs IIS and a default web app from a WebDeploy package.</span></span>  <span data-ttu-id="ecf85-130">Hej *IISInstall.ps1* skriptet installerar IIS på hello virtuella datorn och finns i hello *DSC* mapp.</span><span class="sxs-lookup"><span data-stu-id="ecf85-130">hello *IISInstall.ps1* script installs IIS on hello virtual machine and is found in hello *DSC* folder.</span></span>  <span data-ttu-id="ecf85-131">hello MVC-webbappen finns i hello *WebDeploy* mapp.</span><span class="sxs-lookup"><span data-stu-id="ecf85-131">hello MVC web app is found in hello *WebDeploy* folder.</span></span>  <span data-ttu-id="ecf85-132">hello sökvägar toohello installationsskriptet och hello webbprogrammet har definierats i hello `powershelldscZip` och `webDeployPackage` parametrar i hello *azuredeploy.parameters.json* fil.</span><span class="sxs-lookup"><span data-stu-id="ecf85-132">hello paths toohello install script and hello web app are defined in hello `powershelldscZip` and `webDeployPackage` parameters in hello *azuredeploy.parameters.json* file.</span></span> 

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

## <a name="deploy-hello-template"></a><span data-ttu-id="ecf85-133">Distribuera hello mall</span><span class="sxs-lookup"><span data-stu-id="ecf85-133">Deploy hello template</span></span>
<span data-ttu-id="ecf85-134">hello enklaste sättet toodeploy hello [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) eller [ASP.NET MVC-program på Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) mallen är toouse hello **distribuera tooAzure** knapp hittades i hello i hello viktigt-filer i GitHub.</span><span class="sxs-lookup"><span data-stu-id="ecf85-134">hello simplest way toodeploy hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) template is toouse hello **Deploy tooAzure** button found in hello in hello readme files in GitHub.</span></span>  <span data-ttu-id="ecf85-135">Du kan också använda PowerShell eller Azure CLI toodeploy hello exempelmallarna.</span><span class="sxs-lookup"><span data-stu-id="ecf85-135">You can also use PowerShell or Azure CLI toodeploy hello sample templates.</span></span>

### <a name="powershell"></a><span data-ttu-id="ecf85-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecf85-136">PowerShell</span></span>
<span data-ttu-id="ecf85-137">Kopiera hello [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) eller [ASP.NET MVC-program på Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) filer från hello GitHub-repo tooa mapp på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="ecf85-137">Copy hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) files from hello GitHub repo tooa folder on your local computer.</span></span>  <span data-ttu-id="ecf85-138">Öppna hello *azuredeploy.parameters.json* fil- och update hello standardvärdena för hello `vmssName`, `adminUsername`, och `adminPassword` parametrar.</span><span class="sxs-lookup"><span data-stu-id="ecf85-138">Open hello *azuredeploy.parameters.json* file and update hello default values of hello `vmssName`, `adminUsername`, and `adminPassword` parameters.</span></span> <span data-ttu-id="ecf85-139">Spara följande PowerShell-skript för hello*deploy.ps1* i hello samma mapp som hello *azuredeploy.json* mall.</span><span class="sxs-lookup"><span data-stu-id="ecf85-139">Save hello following PowerShell script too*deploy.ps1* in hello same folder as hello *azuredeploy.json* template.</span></span> <span data-ttu-id="ecf85-140">toodeploy hello exempel mallen kör hello *deploy.ps1* skriptet från en PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="ecf85-140">toodeploy hello sample template run hello *deploy.ps1* script from a PowerShell command window.</span></span>

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
    Write-Host "Resource group '$resourceGroupName' does not exist. toocreate a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start hello deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

### <a name="azure-cli"></a><span data-ttu-id="ecf85-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ecf85-141">Azure CLI</span></span>
```azurecli
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# -e: immediately exit if any command has a non-zero exit status
# -o: prevents errors in a pipeline from being masked
# IFS new value is less likely toocause confusing bugs when looping arrays or arguments (e.g. $@)

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
    echo "Enter a location below toocreate a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file toobe used
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

#login tooazure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set hello default subscription id
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

## <a name="next-steps"></a><span data-ttu-id="ecf85-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ecf85-142">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
