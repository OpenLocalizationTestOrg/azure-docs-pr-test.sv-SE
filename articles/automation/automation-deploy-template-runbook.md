---
title: aaaDeploy en Azure Resource Manager-mall i Azure Automation-runbook | Microsoft Docs
description: "Hur toodeploy en Azure Resource Manager-mall lagras i Azure Storage från en runbook"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: PowerShell, runbook, json, azure automation
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 07/09/2017
ms.author: eslesar
ms.openlocfilehash: f489a8e8635a48f5a6a2f1a88e1c803f56f01832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a>Distribuera en Azure Resource Manager-mall i en Azure Automation PowerShell-runbook

Du kan skriva en [PowerShell för Azure Automation-runbook](automation-first-runbook-textual-powershell.md) som distribuerar en Azure-resurs med hjälp av en [Azure Resource Manager-mallen](../azure-resource-manager/resource-manager-create-first-template.md).

På så sätt kan du automatisera distributionen av Azure-resurser. Du kan underhålla Resource Manager-mallar på en central, säker plats, till exempel Azure Storage.

I det här avsnittet skapar vi en PowerShell-runbook som använder en Resource Manager-mall som lagras i [Azure Storage](../storage/common/storage-introduction.md) toodeploy ett nytt Azure Storage-konto.

## <a name="prerequisites"></a>Krav

toocomplete den här kursen behöver du hello följande:

* En Azure-prenumeration. Om du inte redan har ett konto kan du [aktivera dina MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller <a href="/pricing/free-account/" target="_blank">[registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/).
* [Automation-konto](automation-sec-configure-azure-runas-account.md) toohold hello runbook och autentisera tooAzure resurser.  Det här kontot måste ha behörighet toostart och stoppa hello virtuella datorn.
* [Azure Storage-konto](../storage/common/storage-create-storage-account.md) i vilka toostore hello Resource Manager-mall
* Azure Powershell installeras på en lokal dator. Se [installera och konfigurera Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) information om hur tooget Azure PowerShell.

## <a name="create-hello-resource-manager-template"></a>Skapa hello Resource Manager-mall

Det här exemplet använder vi en Resource Manager-mall som distribuerar ett nytt Azure Storage-konto.

Kopiera hello följande text i en textredigerare:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

Spara hello lokalt som `TemplateTest.json`.

## <a name="save-hello-resource-manager-template-in-azure-storage"></a>Spara hello Resource Manager-mall i Azure Storage

Nu när vi använda PowerShell toocreate en filresurs i Azure Storage och överför hello `TemplateTest.json` fil.
Anvisningar för hur toocreate en fil dela och överföra en fil i hello Azure-portalen finns [Kom igång med Azure File storage i Windows](../storage/files/storage-dotnet-how-to-use-files.md).

Starta PowerShell på den lokala datorn och kör följande kommandon toocreate en filresurs hello och överför hello Resource Manager-mall toothat filresurs.

```powershell
# Login tooAzure
Login-AzureRmAccount

# Get hello access key for your storage account
$key = Get-AzureRmStorageAccountKey -ResourceGroupName 'MyAzureAccount' -Name 'MyStorageAccount'

# Create an Azure Storage context using hello first access key
$context = New-AzureStorageContext -StorageAccountName 'MyStorageAccount' -StorageAccountKey $key[0].value

# Create a file share named 'resource-templates' in your Azure Storage account
$fileShare = New-AzureStorageShare -Name 'resource-templates' -Context $context

# Add hello TemplateTest.json file toohello new file share
# "TemplatePath" is hello path where you saved hello TemplateTest.json file
$templateFile = 'C:\TemplatePath'
Set-AzureStorageFileContent -ShareName $fileShare.Name -Context $context -Source $templateFile
```

## <a name="create-hello-powershell-runbook-script"></a>Skapa runbook hello PowerShell-skript

Nu skapar vi ett PowerShell-skript som hämtar hello `TemplateTest.json` från Azure Storage och distribuerar hello mallen toocreate ett nytt Azure Storage-konto.

Klistra in hello följande text i en textredigerare:

```powershell
param (
    [Parameter(Mandatory=$true)]
    [string]
    $ResourceGroupName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountKey,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageFileName
)



# Authenticate tooAzure if running from Azure Automation
$ServicePrincipalConnection = Get-AutomationConnection -Name "AzureRunAsConnection"
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint | Write-Verbose

#Set hello parameter values for hello Resource Manager template
$Parameters = @{
    "storageAccountType"="Standard_LRS"
    }

# Create a new context
$Context = New-AzureStorageContext -StorageAccountKey $StorageAccountKey

Get-AzureStorageFileContent -ShareName 'resource-templates' -Context $Context -path 'TemplateTest.json' -Destination 'C:\Temp'

$TemplateFile = Join-Path -Path 'C:\Temp' -ChildPath $StorageFileName

# Deploy hello storage account
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterObject $Parameters 
``` 

Spara hello lokalt som `DeployTemplate.ps1`.

## <a name="import-and-publish-hello-runbook-into-your-azure-automation-account"></a>Importera och publicera hello runbook i Azure Automation-konto

Nu vi använder PowerShell tooimport hello runbook i Azure Automation-konto och sedan publicera hello runbook.
Information om hur tooimport och publicera en runbook i hello Azure-portalen, se [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md).

tooimport `DeployTemplate.ps1` i ditt Automation-konto som en PowerShell-runbook kör hello följande PowerShell-kommandon:

```powershell
# MyPath is hello path where you saved DeployTemplate.ps1
# MyResourceGroup is hello name of hello Azure ResourceGroup that contains your Azure Automation account
# MyAutomationAccount is hello name of your Automation account
$importParams = @{
    Path = 'C:\MyPath\DeployTemplate.ps1'
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Type = 'PowerShell'
}
Import-AzureRmAutomationRunbook @

# Publish hello runbook
$publishParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
}
Publish-AzureRmAutomationRunbook @publishParams
```

## <a name="start-hello-runbook"></a>Starta hello runbook

Nu vi starta hello runbook genom att anropa hello [Start AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) cmdlet.

Information om hur toostart en runbook i hello Azure-portalen, se [starta en runbook i Azure Automation](automation-starting-a-runbook.md).

Kör följande kommandon i PowerShell-konsol hello hello:

```powershell
# Set up hello parameters for hello runbook
$runbookParams = @{
    ResourceGroupName = 'MyResourceGroup'
    StorageAccountName = 'MyStorageAccount'
    StorageAccountKey = $key[0].Value # We got this key earlier
    StorageFileName = 'TemplateTest.json' 
}

# Set up parameters for hello Start-AzureRmAutomationRunbook cmdlet
$startParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
    Parameters = $runbookParams
}

# Start hello runbook
$job = Start-AzureRmAutomationRunbook @startParams
```

Hej runbook körs och du kan kontrollera statusen genom att köra `$job.Status`.

Hej runbook hämtar hello Resource Manager-mall och använder den toodeploy ett nytt Azure Storage-konto.
Du kan se att hello nytt lagringskonto har skapats genom att köra följande kommando hello:
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a>Sammanfattning

Klart! Nu kan du använda Azure Automation och Azure Storage och Resource Manager mallar toodeploy alla dina Azure-resurser.

## <a name="next-steps"></a>Nästa steg

* toolearn mer om Resource Manager-mallar finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* tooget igång med Azure Storage finns [introduktion tooAzure lagring](../storage/common/storage-introduction.md).
* toofind andra användbara Azure Automation-runbook finns [Azure Automation Runbook- och stänga](automation-runbook-gallery.md).
* toofind andra användbara Resource Manager-mallar finns [Azure Quickstart-mallar](https://azure.microsoft.com/resources/templates/)

