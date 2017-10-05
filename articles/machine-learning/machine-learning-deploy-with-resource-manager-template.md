---
title: Distribuera en Machine Learning-arbetsytan med Azure Resource Manager | Microsoft Docs
description: "Så här distribuerar du en arbetsyta för Azure Machine Learning med Azure Resource Manager-mall"
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
ms.openlocfilehash: 9e37780428b0867da63987ec4f7f843a8abeb907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a><span data-ttu-id="3c892-103">Distribuera Machine Learning-arbetsyta med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3c892-103">Deploy Machine Learning Workspace Using Azure Resource Manager</span></span>
## <a name="introduction"></a><span data-ttu-id="3c892-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="3c892-104">Introduction</span></span>
<span data-ttu-id="3c892-105">Med en Azure Resource Manager-mall för distribution av sparar du tid genom att ge ett skalbart sätt att distribuera sammankopplade komponenter med en verifiering och försök mekanism.</span><span class="sxs-lookup"><span data-stu-id="3c892-105">Using an Azure Resource Manager deployment template saves you time by giving you a scalable way to deploy interconnected components with a validation and retry mechanism.</span></span> <span data-ttu-id="3c892-106">Om du vill konfigurera Azure Machine Learning arbetsytor, till exempel behöver du först konfigurera en Azure storage-konto och sedan distribuera din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="3c892-106">To set up Azure Machine Learning Workspaces, for example, you need to first configure an Azure storage account and then deploy your workspace.</span></span> <span data-ttu-id="3c892-107">Anta att göra detta manuellt för hundratals arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="3c892-107">Imagine doing this manually for hundreds of workspaces.</span></span> <span data-ttu-id="3c892-108">Ett enklare alternativ är att använda en Azure Resource Manager-mall för att distribuera en Azure Machine Learning-arbetsytan och alla dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="3c892-108">An easier alternative is to use an Azure Resource Manager template to deploy an Azure Machine Learning Workspace and all its dependencies.</span></span> <span data-ttu-id="3c892-109">Den här artikeln tar dig igenom processen steg för steg.</span><span class="sxs-lookup"><span data-stu-id="3c892-109">This article takes you through this process step-by-step.</span></span> <span data-ttu-id="3c892-110">En bra översikt över Azure Resource Manager finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3c892-110">For a great overview of Azure Resource Manager, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="step-by-step-create-a-machine-learning-workspace"></a><span data-ttu-id="3c892-111">Steg för steg: skapa en Machine Learning-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="3c892-111">Step-by-step: create a Machine Learning Workspace</span></span>
<span data-ttu-id="3c892-112">Vi ska skapa en Azure-resursgrupp och sedan distribuera ett nytt Azure storage-konto och en ny Azure Machine Learning-arbetsytan med hjälp av en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="3c892-112">We will create an Azure resource group, then deploy a new Azure storage account and a new Azure Machine Learning Workspace using a Resource Manager template.</span></span> <span data-ttu-id="3c892-113">När installationen är klar, ut vi viktig information om arbetsytor som har skapats (primärnyckel, workspaceID och URL: en till arbetsytan).</span><span class="sxs-lookup"><span data-stu-id="3c892-113">Once the deployment is complete, we will print out important information about the workspaces that were created (the primary key, the workspaceID, and the URL to the workspace).</span></span>

### <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="3c892-114">Skapa en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="3c892-114">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="3c892-115">En Machine Learning-arbetsytan kräver ett Azure storage-konto för att lagra dataset som är länkade till den.</span><span class="sxs-lookup"><span data-stu-id="3c892-115">A Machine Learning Workspace requires an Azure storage account to store the dataset linked to it.</span></span>
<span data-ttu-id="3c892-116">Följande mall använder namnet på resursgruppen att generera lagringskontonamn och namnet på arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="3c892-116">The following template uses the name of the resource group to generate the storage account name and the workspace name.</span></span>  <span data-ttu-id="3c892-117">Dessutom används lagringskontonamn som en egenskap när du skapar arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="3c892-117">It also uses the storage account name as a property when creating the workspace.</span></span>

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
<span data-ttu-id="3c892-118">Spara den här mallen som mlworkspace.json fil under c:\temp\.</span><span class="sxs-lookup"><span data-stu-id="3c892-118">Save this template as mlworkspace.json file under c:\temp\.</span></span>

### <a name="deploy-the-resource-group-based-on-the-template"></a><span data-ttu-id="3c892-119">Distribuera resursgrupp, baserat på mallen</span><span class="sxs-lookup"><span data-stu-id="3c892-119">Deploy the resource group, based on the template</span></span>
* <span data-ttu-id="3c892-120">Öppna PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c892-120">Open PowerShell</span></span>
* <span data-ttu-id="3c892-121">Installera moduler för Azure Resource Manager och Azure-tjänsthantering</span><span class="sxs-lookup"><span data-stu-id="3c892-121">Install modules for Azure Resource Manager and Azure Service Management</span></span>  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   <span data-ttu-id="3c892-122">De här stegen hämta och installera modulerna som krävs för att slutföra stegen.</span><span class="sxs-lookup"><span data-stu-id="3c892-122">These steps download and install the modules necessary to complete the remaining steps.</span></span> <span data-ttu-id="3c892-123">Detta behöver bara göras en gång i miljön där du kör PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="3c892-123">This only needs to be done once in the environment where you are executing the PowerShell commands.</span></span>   

* <span data-ttu-id="3c892-124">Autentisera till Azure</span><span class="sxs-lookup"><span data-stu-id="3c892-124">Authenticate to Azure</span></span>  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
<span data-ttu-id="3c892-125">Det här steget måste upprepas för varje session.</span><span class="sxs-lookup"><span data-stu-id="3c892-125">This step needs to be repeated for each session.</span></span> <span data-ttu-id="3c892-126">När autentiseringen är ska din prenumerationsinformation visas.</span><span class="sxs-lookup"><span data-stu-id="3c892-126">Once authenticated, your subscription information should be displayed.</span></span>

![Azure-konto][1]

<span data-ttu-id="3c892-128">Nu när vi har åtkomst till Azure kan vi skapa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3c892-128">Now that we have access to Azure, we can create the resource group.</span></span>

* <span data-ttu-id="3c892-129">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="3c892-129">Create a resource group</span></span>

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

<span data-ttu-id="3c892-130">Kontrollera att resursgruppen har etablerats på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="3c892-130">Verify that the resource group is correctly provisioned.</span></span> <span data-ttu-id="3c892-131">**ProvisioningState** ska vara ”lyckades”.</span><span class="sxs-lookup"><span data-stu-id="3c892-131">**ProvisioningState** should be “Succeeded.”</span></span>
<span data-ttu-id="3c892-132">Resursgruppens namn används av mallen för att generera lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="3c892-132">The resource group name is used by the template to generate the storage account name.</span></span> <span data-ttu-id="3c892-133">Lagringskontonamnet måste vara mellan 3 och 24 tecken långt och innehålla siffror och gemena bokstäver.</span><span class="sxs-lookup"><span data-stu-id="3c892-133">The storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

![Resursgrupp][2]

* <span data-ttu-id="3c892-135">Med gruppdistributionen resurs kan distribuera en ny Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="3c892-135">Using the resource group deployment, deploy a new Machine Learning Workspace.</span></span>

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

<span data-ttu-id="3c892-136">När distributionen är klar, är det enkelt komma åt egenskaper i arbetsytan som du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="3c892-136">Once the deployment is completed, it is straightforward to access properties of the workspace you deployed.</span></span> <span data-ttu-id="3c892-137">Du kan till exempel åtkomst till den primära nyckeln-Token.</span><span class="sxs-lookup"><span data-stu-id="3c892-137">For example, you can access the Primary Key Token.</span></span>

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

<span data-ttu-id="3c892-138">Ett annat sätt att hämta token på befintlig arbetsyta är att använda kommandot Invoke-AzureRmResourceAction.</span><span class="sxs-lookup"><span data-stu-id="3c892-138">Another way to retrieve tokens of existing workspace is to use the Invoke-AzureRmResourceAction command.</span></span> <span data-ttu-id="3c892-139">Exempelvis kan du visa de primära och sekundära token av alla arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="3c892-139">For example, you can list the primary and secondary tokens of all workspaces.</span></span>

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
<span data-ttu-id="3c892-140">Efter att arbetsytan har etablerats kan du också automatisera många Azure Machine Learning Studio uppgifter med hjälp av den [PowerShell-modulen för Azure Machine Learning](http://aka.ms/amlps).</span><span class="sxs-lookup"><span data-stu-id="3c892-140">After the workspace is provisioned, you can also automate many Azure Machine Learning Studio tasks using the [PowerShell Module for Azure Machine Learning](http://aka.ms/amlps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c892-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c892-141">Next Steps</span></span>
* <span data-ttu-id="3c892-142">Lär dig mer om [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3c892-142">Learn more about [authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="3c892-143">Ta en titt på den [Azure Quickstart mallar databasen](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="3c892-143">Have a look at the [Azure Quickstart Templates Repository](https://github.com/Azure/azure-quickstart-templates).</span></span> 
* <span data-ttu-id="3c892-144">Det här videoklippet om [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span><span class="sxs-lookup"><span data-stu-id="3c892-144">Watch this video about [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span></span> 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
