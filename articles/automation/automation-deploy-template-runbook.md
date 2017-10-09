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
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a><span data-ttu-id="c6810-104">Distribuera en Azure Resource Manager-mall i en Azure Automation PowerShell-runbook</span><span class="sxs-lookup"><span data-stu-id="c6810-104">Deploy an Azure Resource Manager template in an Azure Automation PowerShell runbook</span></span>

<span data-ttu-id="c6810-105">Du kan skriva en [PowerShell för Azure Automation-runbook](automation-first-runbook-textual-powershell.md) som distribuerar en Azure-resurs med hjälp av en [Azure Resource Manager-mallen](../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="c6810-105">You can write an [Azure Automation PowerShell runbook](automation-first-runbook-textual-powershell.md) that deploys an Azure resource by using an [Azure Resource Management template](../azure-resource-manager/resource-manager-create-first-template.md).</span></span>

<span data-ttu-id="c6810-106">På så sätt kan du automatisera distributionen av Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="c6810-106">By doing this, you can automate deployment of Azure resources.</span></span> <span data-ttu-id="c6810-107">Du kan underhålla Resource Manager-mallar på en central, säker plats, till exempel Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c6810-107">You can maintain your Resource Manager templates in a central,secure location such as Azure Storage.</span></span>

<span data-ttu-id="c6810-108">I det här avsnittet skapar vi en PowerShell-runbook som använder en Resource Manager-mall som lagras i [Azure Storage](../storage/common/storage-introduction.md) toodeploy ett nytt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c6810-108">In this topic, we create a PowerShell runbook that uses an Resource Manager template stored in [Azure Storage](../storage/common/storage-introduction.md) toodeploy a new Azure Storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6810-109">Krav</span><span class="sxs-lookup"><span data-stu-id="c6810-109">Prerequisites</span></span>

<span data-ttu-id="c6810-110">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="c6810-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="c6810-111">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c6810-111">Azure subscription.</span></span> <span data-ttu-id="c6810-112">Om du inte redan har ett konto kan du [aktivera dina MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller <a href="/pricing/free-account/" target="_blank">[registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="c6810-112">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="c6810-113">[Automation-konto](automation-sec-configure-azure-runas-account.md) toohold hello runbook och autentisera tooAzure resurser.</span><span class="sxs-lookup"><span data-stu-id="c6810-113">[Automation account](automation-sec-configure-azure-runas-account.md) toohold hello runbook and authenticate tooAzure resources.</span></span>  <span data-ttu-id="c6810-114">Det här kontot måste ha behörighet toostart och stoppa hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c6810-114">This account must have permission toostart and stop hello virtual machine.</span></span>
* <span data-ttu-id="c6810-115">[Azure Storage-konto](../storage/common/storage-create-storage-account.md) i vilka toostore hello Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="c6810-115">[Azure Storage account](../storage/common/storage-create-storage-account.md) in which toostore hello Resource Manager template</span></span>
* <span data-ttu-id="c6810-116">Azure Powershell installeras på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="c6810-116">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="c6810-117">Se [installera och konfigurera Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) information om hur tooget Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c6810-117">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how tooget Azure PowerShell.</span></span>

## <a name="create-hello-resource-manager-template"></a><span data-ttu-id="c6810-118">Skapa hello Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="c6810-118">Create hello Resource Manager template</span></span>

<span data-ttu-id="c6810-119">Det här exemplet använder vi en Resource Manager-mall som distribuerar ett nytt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c6810-119">For this example, we use an Resource Manager template that deploys a new Azure Storage account.</span></span>

<span data-ttu-id="c6810-120">Kopiera hello följande text i en textredigerare:</span><span class="sxs-lookup"><span data-stu-id="c6810-120">In a text editor, copy hello following text:</span></span>

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

<span data-ttu-id="c6810-121">Spara hello lokalt som `TemplateTest.json`.</span><span class="sxs-lookup"><span data-stu-id="c6810-121">Save hello file locally as `TemplateTest.json`.</span></span>

## <a name="save-hello-resource-manager-template-in-azure-storage"></a><span data-ttu-id="c6810-122">Spara hello Resource Manager-mall i Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c6810-122">Save hello Resource Manager template in Azure Storage</span></span>

<span data-ttu-id="c6810-123">Nu när vi använda PowerShell toocreate en filresurs i Azure Storage och överför hello `TemplateTest.json` fil.</span><span class="sxs-lookup"><span data-stu-id="c6810-123">Now we use PowerShell toocreate an Azure Storage file share and upload hello `TemplateTest.json` file.</span></span>
<span data-ttu-id="c6810-124">Anvisningar för hur toocreate en fil dela och överföra en fil i hello Azure-portalen finns [Kom igång med Azure File storage i Windows](../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="c6810-124">For instructions on how toocreate a file share and upload a file in hello Azure portal, see [Get started with Azure File storage on Windows](../storage/files/storage-dotnet-how-to-use-files.md).</span></span>

<span data-ttu-id="c6810-125">Starta PowerShell på den lokala datorn och kör följande kommandon toocreate en filresurs hello och överför hello Resource Manager-mall toothat filresurs.</span><span class="sxs-lookup"><span data-stu-id="c6810-125">Launch PowerShell on your local machine, and run hello following commands toocreate a file share and upload hello Resource Manager template toothat file share.</span></span>

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

## <a name="create-hello-powershell-runbook-script"></a><span data-ttu-id="c6810-126">Skapa runbook hello PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="c6810-126">Create hello PowerShell runbook script</span></span>

<span data-ttu-id="c6810-127">Nu skapar vi ett PowerShell-skript som hämtar hello `TemplateTest.json` från Azure Storage och distribuerar hello mallen toocreate ett nytt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c6810-127">Now we create a PowerShell script that gets hello `TemplateTest.json` file from Azure Storage and deploys hello template toocreate a new Azure Storage account.</span></span>

<span data-ttu-id="c6810-128">Klistra in hello följande text i en textredigerare:</span><span class="sxs-lookup"><span data-stu-id="c6810-128">In a text editor, paste hello following text:</span></span>

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

<span data-ttu-id="c6810-129">Spara hello lokalt som `DeployTemplate.ps1`.</span><span class="sxs-lookup"><span data-stu-id="c6810-129">Save hello file locally as `DeployTemplate.ps1`.</span></span>

## <a name="import-and-publish-hello-runbook-into-your-azure-automation-account"></a><span data-ttu-id="c6810-130">Importera och publicera hello runbook i Azure Automation-konto</span><span class="sxs-lookup"><span data-stu-id="c6810-130">Import and publish hello runbook into your Azure Automation account</span></span>

<span data-ttu-id="c6810-131">Nu vi använder PowerShell tooimport hello runbook i Azure Automation-konto och sedan publicera hello runbook.</span><span class="sxs-lookup"><span data-stu-id="c6810-131">Now we use PowerShell tooimport hello runbook into your Azure Automation account, and then publish hello runbook.</span></span>
<span data-ttu-id="c6810-132">Information om hur tooimport och publicera en runbook i hello Azure-portalen, se [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="c6810-132">For information about how tooimport and publish a runbook in hello Azure portal, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md).</span></span>

<span data-ttu-id="c6810-133">tooimport `DeployTemplate.ps1` i ditt Automation-konto som en PowerShell-runbook kör hello följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="c6810-133">tooimport `DeployTemplate.ps1` into your Automation account as a PowerShell runbook, run hello following PowerShell commands:</span></span>

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

## <a name="start-hello-runbook"></a><span data-ttu-id="c6810-134">Starta hello runbook</span><span class="sxs-lookup"><span data-stu-id="c6810-134">Start hello runbook</span></span>

<span data-ttu-id="c6810-135">Nu vi starta hello runbook genom att anropa hello [Start AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c6810-135">Now we start hello runbook by calling hello [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) cmdlet.</span></span>

<span data-ttu-id="c6810-136">Information om hur toostart en runbook i hello Azure-portalen, se [starta en runbook i Azure Automation](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="c6810-136">For information about how toostart a runbook in hello Azure portal, see [Starting a runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>

<span data-ttu-id="c6810-137">Kör följande kommandon i PowerShell-konsol hello hello:</span><span class="sxs-lookup"><span data-stu-id="c6810-137">Run hello following commands in hello PowerShell console:</span></span>

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

<span data-ttu-id="c6810-138">Hej runbook körs och du kan kontrollera statusen genom att köra `$job.Status`.</span><span class="sxs-lookup"><span data-stu-id="c6810-138">hello runbook runs, and you can check its status by running `$job.Status`.</span></span>

<span data-ttu-id="c6810-139">Hej runbook hämtar hello Resource Manager-mall och använder den toodeploy ett nytt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c6810-139">hello runbook gets hello Resource Manager template and uses it toodeploy a new Azure Storage account.</span></span>
<span data-ttu-id="c6810-140">Du kan se att hello nytt lagringskonto har skapats genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="c6810-140">You can see that hello new storage account was created by running hello following command:</span></span>
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a><span data-ttu-id="c6810-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c6810-141">Summary</span></span>

<span data-ttu-id="c6810-142">Klart!</span><span class="sxs-lookup"><span data-stu-id="c6810-142">That's it!</span></span> <span data-ttu-id="c6810-143">Nu kan du använda Azure Automation och Azure Storage och Resource Manager mallar toodeploy alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="c6810-143">Now you can use Azure Automation and Azure Storage, and Resource Manager templates toodeploy all your Azure resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6810-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6810-144">Next steps</span></span>

* <span data-ttu-id="c6810-145">toolearn mer om Resource Manager-mallar finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c6810-145">toolearn more about Resource Manager templates, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="c6810-146">tooget igång med Azure Storage finns [introduktion tooAzure lagring](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c6810-146">tooget started with Azure Storage, see [Introduction tooAzure Storage](../storage/common/storage-introduction.md).</span></span>
* <span data-ttu-id="c6810-147">toofind andra användbara Azure Automation-runbook finns [Azure Automation Runbook- och stänga](automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="c6810-147">toofind other useful Azure Automation runbooks, see [Runbook and module galleries for Azure Automation](automation-runbook-gallery.md).</span></span>
* <span data-ttu-id="c6810-148">toofind andra användbara Resource Manager-mallar finns [Azure Quickstart-mallar](https://azure.microsoft.com/resources/templates/)</span><span class="sxs-lookup"><span data-stu-id="c6810-148">toofind other useful Resource Manager templates, see [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/)</span></span>

