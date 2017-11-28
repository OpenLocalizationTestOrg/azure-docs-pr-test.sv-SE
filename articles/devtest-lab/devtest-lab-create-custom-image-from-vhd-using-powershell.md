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
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a><span data-ttu-id="94285-103">Skapa en anpassad avbildning från en VHD-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="94285-103">Create a custom image from a VHD file using PowerShell</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="94285-104">Stegvisa instruktioner</span><span class="sxs-lookup"><span data-stu-id="94285-104">Step-by-step instructions</span></span>

<span data-ttu-id="94285-105">hello följande steg beskriver hur du skapar en anpassad avbildning från en VHD-fil med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="94285-105">hello following steps walk you through creating a custom image from a VHD file using PowerShell:</span></span>

1. <span data-ttu-id="94285-106">Logga in tooyour Azure-konto i PowerShell-Kommandotolken med hello följa anropet toohello **Login-AzureRmAccount** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="94285-106">At a PowerShell prompt, log in tooyour Azure account with hello following call toohello **Login-AzureRmAccount** cmdlet.</span></span>  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.  <span data-ttu-id="94285-107">Välj hello önskvärt Azure-prenumeration genom att anropa hello **Select-AzureRmSubscription** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="94285-107">Select hello desired Azure subscription by calling hello **Select-AzureRmSubscription** cmdlet.</span></span> <span data-ttu-id="94285-108">Ersätt följande platshållare för hello hello **$subscriptionId** variabeln med ett giltigt Azure-prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="94285-108">Replace hello following placeholder for hello **$subscriptionId** variable with a valid Azure subscription ID.</span></span> 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  <span data-ttu-id="94285-109">Hämta hello lab objekt genom att anropa hello **Get-AzureRmResource** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="94285-109">Get hello lab object by calling hello **Get-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="94285-110">Ersätt följande platshållare för hello hello **$labRg** och **$labName** variabler med hello lämpliga värden för din miljö.</span><span class="sxs-lookup"><span data-stu-id="94285-110">Replace hello following placeholders for hello **$labRg** and **$labName** variables with hello appropriate values for your environment.</span></span> 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  <span data-ttu-id="94285-111">Hämta hello labblagringskontot och labblagringskontot nyckelvärden från hello lab objekt.</span><span class="sxs-lookup"><span data-stu-id="94285-111">Get hello lab storage account and lab storage account key values from hello lab object.</span></span> 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  <span data-ttu-id="94285-112">Ersätt följande platshållare för hello hello **$vhdUri** variabeln med hello URI tooyour överföra VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="94285-112">Replace hello following placeholder for hello **$vhdUri** variable with hello URI tooyour uploaded VHD file.</span></span> <span data-ttu-id="94285-113">Du kan hämta filen hello-VHD-URI från hello lagringskontots blob-bladet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="94285-113">You can get hello VHD file's URI from hello storage account's blob blade in hello Azure portal.</span></span>

    ```PowerShell
    $vhdUri = '<Specify hello VHD URI here>'
    ```

1.  <span data-ttu-id="94285-114">Skapa hello anpassad avbildning med hjälp av hello **ny AzureRmResourceGroupDeployment** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="94285-114">Create hello custom image using hello **New-AzureRmResourceGroupDeployment** cmdlet.</span></span> <span data-ttu-id="94285-115">Ersätt följande platshållare för hello hello **$customImageName** och **$customImageDescription** variabler toomeaningful namn för din miljö.</span><span class="sxs-lookup"><span data-stu-id="94285-115">Replace hello following placeholders for hello **$customImageName** and **$customImageDescription** variables toomeaningful names for your environment.</span></span>

    ```PowerShell
    $customImageName = '<Specify hello custom image name>'
    $customImageDescription = '<Specify hello custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-toocreate-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="94285-116">PowerShell-skriptet toocreate en anpassad avbildning från en VHD-fil</span><span class="sxs-lookup"><span data-stu-id="94285-116">PowerShell script toocreate a custom image from a VHD file</span></span>

<span data-ttu-id="94285-117">hello följande PowerShell-skript kan vara används toocreate en anpassad avbildning från en VHD-fil.</span><span class="sxs-lookup"><span data-stu-id="94285-117">hello following PowerShell script can be used toocreate a custom image from a VHD file.</span></span> <span data-ttu-id="94285-118">Ersätt platshållarna för hello (börjar och slutar med hakparenteser) med hello lämpliga värden för dina behov.</span><span class="sxs-lookup"><span data-stu-id="94285-118">Replace hello placeholders (starting and ending with angle brackets) with hello appropriate values for your needs.</span></span> 

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

## <a name="related-blog-posts"></a><span data-ttu-id="94285-119">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="94285-119">Related blog posts</span></span>

- [<span data-ttu-id="94285-120">Anpassade avbildningar eller formler?</span><span class="sxs-lookup"><span data-stu-id="94285-120">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="94285-121">Kopiera anpassade avbildningar mellan Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="94285-121">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="94285-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94285-122">Next steps</span></span>

- [<span data-ttu-id="94285-123">Lägga till ett VM tooyour labb</span><span class="sxs-lookup"><span data-stu-id="94285-123">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
