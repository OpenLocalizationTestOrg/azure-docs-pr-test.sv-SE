---
title: "aaaCreate en anpassad avbildning Azure DevTest Labs från en VHD-fil med hjälp av PowerShell | Microsoft Docs"
description: "Automatisera genereringen av en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 39b4005fa46cdf86cf0800ca376128134bcfb650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>Skapa en anpassad avbildning från en VHD-fil med hjälp av PowerShell

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Stegvisa instruktioner

hello följande steg beskriver hur du skapar en anpassad avbildning från en VHD-fil med hjälp av PowerShell:

1. Logga in tooyour Azure-konto i PowerShell-Kommandotolken med hello följa anropet toohello **Login-AzureRmAccount** cmdlet.  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.  Välj hello önskvärt Azure-prenumeration genom att anropa hello **Select-AzureRmSubscription** cmdlet. Ersätt följande platshållare för hello hello **$subscriptionId** variabeln med ett giltigt Azure-prenumerations-ID. 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  Hämta hello lab objekt genom att anropa hello **Get-AzureRmResource** cmdlet. Ersätt följande platshållare för hello hello **$labRg** och **$labName** variabler med hello lämpliga värden för din miljö. 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  Hämta hello labblagringskontot och labblagringskontot nyckelvärden från hello lab objekt. 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  Ersätt följande platshållare för hello hello **$vhdUri** variabeln med hello URI tooyour överföra VHD-filen. Du kan hämta filen hello-VHD-URI från hello lagringskontots blob-bladet i hello Azure-portalen.

    ```PowerShell
    $vhdUri = '<Specify hello VHD URI here>'
    ```

1.  Skapa hello anpassad avbildning med hjälp av hello **ny AzureRmResourceGroupDeployment** cmdlet. Ersätt följande platshållare för hello hello **$customImageName** och **$customImageDescription** variabler toomeaningful namn för din miljö.

    ```PowerShell
    $customImageName = '<Specify hello custom image name>'
    $customImageDescription = '<Specify hello custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-toocreate-a-custom-image-from-a-vhd-file"></a>PowerShell-skriptet toocreate en anpassad avbildning från en VHD-fil

hello följande PowerShell-skript kan vara används toocreate en anpassad avbildning från en VHD-fil. Ersätt platshållarna för hello (börjar och slutar med hakparenteser) med hello lämpliga värden för dina behov. 

```PowerShell
# Log in tooyour Azure account.  
Login-AzureRmAccount

# Select hello desired Azure subscription. 
$subscriptionId = '<Specify your subscription ID here>'
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Get hello lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get hello lab storage account and lab storage account key values.
$labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
$labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value

# Set hello URI of hello VHD file.  
$vhdUri = '<Specify hello VHD URI here>'

# Set hello custom image name and description values.
$customImageName = '<Specify hello custom image name>'
$customImageDescription = '<Specify hello custom image description>'

# Set up hello parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create hello custom image. 
New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>Relaterade blogginlägg

- [Anpassade avbildningar eller formler?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiera anpassade avbildningar mellan Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Nästa steg

- [Lägga till ett VM tooyour labb](./devtest-lab-add-vm-with-artifacts.md)
