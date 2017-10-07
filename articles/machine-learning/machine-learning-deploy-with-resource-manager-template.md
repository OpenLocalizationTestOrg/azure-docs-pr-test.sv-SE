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
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Distribuera Machine Learning-arbetsyta med hjälp av Azure Resource Manager
## <a name="introduction"></a>Introduktion
Med en Azure Resource Manager-mall för distribution av sparar du samman tid genom att ge en skalbar sätt toodeploy komponenter med en mekanism för verifiering och försök igen. tooset in Azure Machine Learning arbetsytor, till exempel, måste toofirst konfigurera ett Azure storage-konto och sedan distribuera din arbetsyta. Anta att göra detta manuellt för hundratals arbetsytor. Ett enklare alternativ är toouse toodeploy en Azure Resource Manager-mall för Azure Machine Learning-arbetsytan och alla dess beroenden. Den här artikeln tar dig igenom processen steg för steg. En bra översikt över Azure Resource Manager finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Steg för steg: skapa en Machine Learning-arbetsytan
Vi ska skapa en Azure-resursgrupp och sedan distribuera ett nytt Azure storage-konto och en ny Azure Machine Learning-arbetsytan med hjälp av en Resource Manager-mall. När hello distributionen är klar kan ut vi viktig information om hello arbetsytor som har skapats (hello primärnyckel hello workspaceID och hello URL toohello arbetsytan).

### <a name="create-an-azure-resource-manager-template"></a>Skapa en Azure Resource Manager-mall
En Machine Learning-arbetsytan kräver ett Azure storage-konto toostore hello dataset länkade tooit.
hello använder följande mallen hello namnet på hello resurs grupp toogenerate hello lagringskontonamn och hello arbetsytans namn.  Dessutom används hello lagringskontonamn som en egenskap när du skapar hello arbetsytan.

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
Spara den här mallen som mlworkspace.json fil under c:\temp\.

### <a name="deploy-hello-resource-group-based-on-hello-template"></a>Distribuera hello resursgrupp, baserat på hello mall
* Öppna PowerShell
* Installera moduler för Azure Resource Manager och Azure-tjänsthantering  

```
# Install hello Azure Resource Manager modules from hello PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install hello Azure Service Management modules from hello PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   De här stegen hämta och installera hello moduler nödvändiga toocomplete hello återstående steg. På så sätt behöver bara toobe utförs en gång i hello miljö där du kör hello PowerShell-kommandon.   

* Autentisera tooAzure  

```
# Authenticate (enter your credentials in hello pop-up window)
Add-AzureRmAccount
```
Det här steget måste toobe upprepas för varje session. När autentiseringen är ska din prenumerationsinformation visas.

![Azure-konto][1]

Nu när vi har åtkomst tooAzure kan skapa vi hello resursgrupp.

* Skapa en resursgrupp

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Kontrollera som hello resursgruppen har etablerats på rätt sätt. **ProvisioningState** ska vara ”lyckades”.
hello resursgruppens namn används av hello mallen toogenerate hello lagringskontonamnet. Hej lagringskontonamn måste vara mellan 3 och 24 tecken långt och innehålla siffror och gemena bokstäver.

![Resursgrupp][2]

* Med hello resurs distribution kan använda en ny Machine Learning-arbetsytan.

```
# Create a Resource Group, TemplateFile is hello location of hello JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

När hello distributionen är klar, är det enkelt tooaccess egenskaper för hello arbetsytan som du har distribuerat. Exempelvis kan du komma åt hello primära nyckeltoken.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Ett annat sätt tooretrieve token på befintlig arbetsyta är toouse hello Invoke-AzureRmResourceAction kommando. Du kan till exempel visa hello primära och sekundära token på alla arbetsytor.

```  
# List hello primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
När hello arbetsytan etableras, du kan också automatisera många Azure Machine Learning Studio uppgifter med hjälp av hello [PowerShell-modulen för Azure Machine Learning](http://aka.ms/amlps).

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md). 
* Ta en titt på hello [Azure Quickstart mallar databasen](https://github.com/Azure/azure-quickstart-templates). 
* Det här videoklippet om [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
