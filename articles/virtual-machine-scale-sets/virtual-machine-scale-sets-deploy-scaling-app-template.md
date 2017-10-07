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
# <a name="deploy-an-autoscaling-app-using-a-template"></a>Distribuera en app för automatisk skalning med en mall

[Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) är ett bra sätt toodeploy grupper av relaterade resurser. Den här kursen bygger på [distribuera en enkel skaluppsättning](virtual-machine-scale-sets-mvss-start.md) och beskriver hur toodeploy ett enkelt autoskalning program på en skala anges med en Azure Resource Manager-mall.  Du kan också ställa in autoskalning med PowerShell, CLI eller hello-portalen. Mer information finns i [Översikt för autoskala](virtual-machine-scale-sets-autoscale-overview.md).

## <a name="two-quickstart-templates"></a>Två snabbstartmallar
När du distribuerar en skaluppsättning du kan installera ny programvara på en plattformbild med ett [VM-tillägg](../virtual-machines/virtual-machines-windows-extensions-features.md). Ett VM-tilläggt är ett litet program som innehåller en färdig konfiguration och automatisering av uppgifter på virtuella Azure-datorer, till exempel att distribuera en app. Två olika exempelmallarna finns i [Azure/azure--snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates) som visar hur toodeploy ett autoskalning program på en skala anges med VM-tillägg.

### <a name="python-http-server-on-linux"></a>Python-HTTP-servern på Linux
Hej [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) exempelmall distribuerar en enkel autoskalning program som körs på en Linux-skaluppsättning.  [Bottle](http://bottlepy.org/docs/dev/), en Python web framework och en enkel HTTP-server distribueras på varje virtuell dator i hello skala anges med ett anpassat skript för VM-tillägget. Hej skaluppsättning skalar när Genomsnittlig CPU-belastning över alla virtuella datorer som är större än 60% och skalas när hello Genomsnittlig CPU-användning är mindre än 30%.

Dessutom toohello skaluppsättning resurs, hello *azuredeploy.json* exempelmall deklarerar också virtuella nätverk, offentlig IP-adress, belastningsutjämnare och Autoskala inställningar resurser.  Mer information om hur du skapar dessa resurser i en mall finns i [Linux-skala med autoskala](virtual-machine-scale-sets-linux-autoscale.md).

I hello *azuredeploy.json* mall, hello `extensionProfile` -egenskapen för hello `Microsoft.Compute/virtualMachineScaleSets` resurs anger tillägget för anpassat skript. `fileUris`Anger hello skript plats. I det här fallet två filer: *workserver.py*, som definierar en enkel HTTP-server och *installserver.sh*, som installerar Bottle och startar hello HTTP-servern. `commandToExecute`Anger hello kommandot toorun när hello skaluppsättning har distribuerats.

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

### <a name="aspnet-mvc-application-on-windows"></a>ASP.NET MVC-appen i Windows
Hej [ASP.NET MVC-program på Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) exempelmall distribuerar en enkel ASP.NET MVC-app som körs i IIS på Windows skaluppsättning.  IIS och hello MVC-app distribueras med hello [PowerShell önskad tillståndskonfiguration (DSC)](virtual-machine-scale-sets-dsc.md) VM-tillägget.  hello skala konfigurera skalor (på VM-instans i taget) när CPU-användning är större än 50% under 5 minuter. 

Dessutom toohello skaluppsättning resurs, hello *azuredeploy.json* exempelmall deklarerar också virtuella nätverk, offentlig IP-adress, belastningsutjämnare och Autoskala inställningar resurser. Den här mallen visas också uppgradering av programmet.  Mer information om hur du skapar dessa resurser i en mall finns i [Windows-skala med autoskala](virtual-machine-scale-sets-windows-autoscale.md).

I hello *azuredeploy.json* mall, hello `extensionProfile` -egenskapen för hello `Microsoft.Compute/virtualMachineScaleSets` resurs anger en [önskad tillståndskonfiguration (DSC)](virtual-machine-scale-sets-dsc.md) -tillägg som installerar IIS och standard webbprogrammet från en WebDeploy-paketet.  Hej *IISInstall.ps1* skriptet installerar IIS på hello virtuella datorn och finns i hello *DSC* mapp.  hello MVC-webbappen finns i hello *WebDeploy* mapp.  hello sökvägar toohello installationsskriptet och hello webbprogrammet har definierats i hello `powershelldscZip` och `webDeployPackage` parametrar i hello *azuredeploy.parameters.json* fil. 

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

## <a name="deploy-hello-template"></a>Distribuera hello mall
hello enklaste sättet toodeploy hello [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) eller [ASP.NET MVC-program på Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) mallen är toouse hello **distribuera tooAzure** knapp hittades i hello i hello viktigt-filer i GitHub.  Du kan också använda PowerShell eller Azure CLI toodeploy hello exempelmallarna.

### <a name="powershell"></a>PowerShell
Kopiera hello [Python HTTP-servern på Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) eller [ASP.NET MVC-program på Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) filer från hello GitHub-repo tooa mapp på den lokala datorn.  Öppna hello *azuredeploy.parameters.json* fil- och update hello standardvärdena för hello `vmssName`, `adminUsername`, och `adminPassword` parametrar. Spara följande PowerShell-skript för hello*deploy.ps1* i hello samma mapp som hello *azuredeploy.json* mall. toodeploy hello exempel mallen kör hello *deploy.ps1* skriptet från en PowerShell-Kommandotolken.

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

### <a name="azure-cli"></a>Azure CLI
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

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
