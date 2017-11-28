---
title: aaaDeploy Machine Learning-arbetsytan med Azure Resource Manager | Microsoft Docs
description: "Hur toodeploy en arbetsyta för Azure Machine Learning med Azure Resource Manager-mall"
services: machine-learning
documentationcenter: 
author: ahgyger
manager: haining
editor: garye
ms.assetid: 4955ac4d-ff99-4908-aa27-69b6bfcc8e85
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/15/2017
ms.author: ahgyger
ms.openlocfilehash: 308959825bcbd670f6ce9b6dc381be767f172357
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a><span data-ttu-id="6c9c8-103">Distribuera Machine Learning-arbetsyta med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6c9c8-103">Deploy Machine Learning Workspace Using Azure Resource Manager</span></span>
## <a name="introduction"></a><span data-ttu-id="6c9c8-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="6c9c8-104">Introduction</span></span>
<span data-ttu-id="6c9c8-105">Med en Azure Resource Manager-mall för distribution av sparar du samman tid genom att ge en skalbar sätt toodeploy komponenter med en mekanism för verifiering och försök igen.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-105">Using an Azure Resource Manager deployment template saves you time by giving you a scalable way toodeploy interconnected components with a validation and retry mechanism.</span></span> <span data-ttu-id="6c9c8-106">tooset in Azure Machine Learning arbetsytor, till exempel, måste toofirst konfigurera ett Azure storage-konto och sedan distribuera din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-106">tooset up Azure Machine Learning Workspaces, for example, you need toofirst configure an Azure storage account and then deploy your workspace.</span></span> <span data-ttu-id="6c9c8-107">Anta att göra detta manuellt för hundratals arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-107">Imagine doing this manually for hundreds of workspaces.</span></span> <span data-ttu-id="6c9c8-108">Ett enklare alternativ är toouse toodeploy en Azure Resource Manager-mall för Azure Machine Learning-arbetsytan och alla dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-108">An easier alternative is toouse an Azure Resource Manager template toodeploy an Azure Machine Learning Workspace and all its dependencies.</span></span> <span data-ttu-id="6c9c8-109">Den här artikeln tar dig igenom processen steg för steg.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-109">This article takes you through this process step-by-step.</span></span> <span data-ttu-id="6c9c8-110">En bra översikt över Azure Resource Manager finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6c9c8-110">For a great overview of Azure Resource Manager, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="step-by-step-create-a-machine-learning-workspace"></a><span data-ttu-id="6c9c8-111">Steg för steg: skapa en Machine Learning-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="6c9c8-111">Step-by-step: create a Machine Learning Workspace</span></span>
<span data-ttu-id="6c9c8-112">Vi ska skapa en Azure-resursgrupp och sedan distribuera ett nytt Azure storage-konto och en ny Azure Machine Learning-arbetsytan med hjälp av en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-112">We will create an Azure resource group, then deploy a new Azure storage account and a new Azure Machine Learning Workspace using a Resource Manager template.</span></span> <span data-ttu-id="6c9c8-113">När hello distributionen är klar kan ut vi viktig information om hello arbetsytor som har skapats (hello primärnyckel hello workspaceID och hello URL toohello arbetsytan).</span><span class="sxs-lookup"><span data-stu-id="6c9c8-113">Once hello deployment is complete, we will print out important information about hello workspaces that were created (hello primary key, hello workspaceID, and hello URL toohello workspace).</span></span>

### <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="6c9c8-114">Skapa en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="6c9c8-114">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="6c9c8-115">En Machine Learning-arbetsytan kräver ett Azure storage-konto toostore hello dataset länkade tooit.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-115">A Machine Learning Workspace requires an Azure storage account toostore hello dataset linked tooit.</span></span>
<span data-ttu-id="6c9c8-116">hello använder följande mallen hello namnet på hello resurs grupp toogenerate hello lagringskontonamn och hello arbetsytans namn.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-116">hello following template uses hello name of hello resource group toogenerate hello storage account name and hello workspace name.</span></span>  <span data-ttu-id="6c9c8-117">Dessutom används hello lagringskontonamn som en egenskap när du skapar hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-117">It also uses hello storage account name as a property when creating hello workspace.</span></span>

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
<span data-ttu-id="6c9c8-118">Spara den här mallen som mlworkspace.json fil under c:\temp\.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-118">Save this template as mlworkspace.json file under c:\temp\.</span></span>

### <a name="deploy-hello-resource-group-based-on-hello-template"></a><span data-ttu-id="6c9c8-119">Distribuera hello resursgrupp, baserat på hello mall</span><span class="sxs-lookup"><span data-stu-id="6c9c8-119">Deploy hello resource group, based on hello template</span></span>
* <span data-ttu-id="6c9c8-120">Öppna PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c9c8-120">Open PowerShell</span></span>
* <span data-ttu-id="6c9c8-121">Installera moduler för Azure Resource Manager och Azure-tjänsthantering</span><span class="sxs-lookup"><span data-stu-id="6c9c8-121">Install modules for Azure Resource Manager and Azure Service Management</span></span>  

```
# Install hello Azure Resource Manager modules from hello PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install hello Azure Service Management modules from hello PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   <span data-ttu-id="6c9c8-122">De här stegen hämta och installera hello moduler nödvändiga toocomplete hello återstående steg.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-122">These steps download and install hello modules necessary toocomplete hello remaining steps.</span></span> <span data-ttu-id="6c9c8-123">På så sätt behöver bara toobe utförs en gång i hello miljö där du kör hello PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-123">This only needs toobe done once in hello environment where you are executing hello PowerShell commands.</span></span>   

* <span data-ttu-id="6c9c8-124">Autentisera tooAzure</span><span class="sxs-lookup"><span data-stu-id="6c9c8-124">Authenticate tooAzure</span></span>  

```
# Authenticate (enter your credentials in hello pop-up window)
Add-AzureRmAccount
```
<span data-ttu-id="6c9c8-125">Det här steget måste toobe upprepas för varje session.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-125">This step needs toobe repeated for each session.</span></span> <span data-ttu-id="6c9c8-126">När autentiseringen är ska din prenumerationsinformation visas.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-126">Once authenticated, your subscription information should be displayed.</span></span>

![Azure-konto][1]

<span data-ttu-id="6c9c8-128">Nu när vi har åtkomst tooAzure kan skapa vi hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-128">Now that we have access tooAzure, we can create hello resource group.</span></span>

* <span data-ttu-id="6c9c8-129">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="6c9c8-129">Create a resource group</span></span>

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

<span data-ttu-id="6c9c8-130">Kontrollera som hello resursgruppen har etablerats på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-130">Verify that hello resource group is correctly provisioned.</span></span> <span data-ttu-id="6c9c8-131">**ProvisioningState** ska vara ”lyckades”.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-131">**ProvisioningState** should be “Succeeded.”</span></span>
<span data-ttu-id="6c9c8-132">hello resursgruppens namn används av hello mallen toogenerate hello lagringskontonamnet.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-132">hello resource group name is used by hello template toogenerate hello storage account name.</span></span> <span data-ttu-id="6c9c8-133">Hej lagringskontonamn måste vara mellan 3 och 24 tecken långt och innehålla siffror och gemena bokstäver.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-133">hello storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

![Resursgrupp][2]

* <span data-ttu-id="6c9c8-135">Med hello resurs distribution kan använda en ny Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-135">Using hello resource group deployment, deploy a new Machine Learning Workspace.</span></span>

```
# Create a Resource Group, TemplateFile is hello location of hello JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

<span data-ttu-id="6c9c8-136">När hello distributionen är klar, är det enkelt tooaccess egenskaper för hello arbetsytan som du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-136">Once hello deployment is completed, it is straightforward tooaccess properties of hello workspace you deployed.</span></span> <span data-ttu-id="6c9c8-137">Exempelvis kan du komma åt hello primära nyckeltoken.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-137">For example, you can access hello Primary Key Token.</span></span>

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

<span data-ttu-id="6c9c8-138">Ett annat sätt tooretrieve token på befintlig arbetsyta är toouse hello Invoke-AzureRmResourceAction kommando.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-138">Another way tooretrieve tokens of existing workspace is toouse hello Invoke-AzureRmResourceAction command.</span></span> <span data-ttu-id="6c9c8-139">Du kan till exempel visa hello primära och sekundära token på alla arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="6c9c8-139">For example, you can list hello primary and secondary tokens of all workspaces.</span></span>

```  
# List hello primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
<span data-ttu-id="6c9c8-140">När hello arbetsytan etableras, du kan också automatisera många Azure Machine Learning Studio uppgifter med hjälp av hello [PowerShell-modulen för Azure Machine Learning](http://aka.ms/amlps).</span><span class="sxs-lookup"><span data-stu-id="6c9c8-140">After hello workspace is provisioned, you can also automate many Azure Machine Learning Studio tasks using hello [PowerShell Module for Azure Machine Learning](http://aka.ms/amlps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c9c8-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c9c8-141">Next Steps</span></span>
* <span data-ttu-id="6c9c8-142">Lär dig mer om [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6c9c8-142">Learn more about [authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="6c9c8-143">Ta en titt på hello [Azure Quickstart mallar databasen](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="6c9c8-143">Have a look at hello [Azure Quickstart Templates Repository](https://github.com/Azure/azure-quickstart-templates).</span></span> 
* <span data-ttu-id="6c9c8-144">Det här videoklippet om [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span><span class="sxs-lookup"><span data-stu-id="6c9c8-144">Watch this video about [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span></span> 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
