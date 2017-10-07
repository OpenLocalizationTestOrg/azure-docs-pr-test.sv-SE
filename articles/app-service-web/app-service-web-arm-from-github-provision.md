---
title: "aaaDeploy ett webbprogram som är länkade tooa GitHub-lagringsplatsen | Microsoft Docs"
description: "Använda en Azure Resource Manager mallen toodeploy en webbapp som innehåller ett projekt från en GitHub-databas."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 32739607-85fe-43c8-a4dc-1feb46d93a4d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: cephalin
ms.openlocfilehash: 8b23416c4c06a60991517e6ee4cd82bebc5a9d73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-web-app-linked-tooa-github-repository"></a>Distribuera en web app länkade tooa GitHub-databas
I det här avsnittet får du lära dig hur toocreate en Azure Resource Manager-mall som distribuerar ett webbprogram som är länkade tooa projekt i en GitHub-databas. Du får lära dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs. Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.

Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

Hello fullständig mall, se [Web App länkad tooGitHub mall](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a>Vad du ska distribuera
Med den här mallen ska du distribuera en webbapp som innehåller hello kod från ett projekt i GitHub.

toorun Hej distributionen automatiskt, klickar du på följande knapp hello:

[![Distribuera tooAzure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrar
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL
hello URL till GitHub-lagringsplats som innehåller hello projekt toodeploy. Den här parametern innehåller ett standardvärde, men det här värdet är bara avsedd tooshow du hur tooprovide hello URL för databasen. Du kan använda det här värdet när du testar hello mallen men du vill tooprovide hello-URL: en egen databas när du arbetar med hello mallen.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>Avdelningskontor
hello gren av databasen hello toouse vid distribution av programmet hello. hello standardvärdet är master, men du kan ange hello namnet på en gren i hello databas du vill toodeploy.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-toodeploy"></a>Resurser toodeploy
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Webbapp
Skapar hello webbprogram som är länkade toohello projekt i GitHub. 

Anger hello namnet på hello webbprogram via hello **siteName** parametern och hello platsen för hello webbprogram via hello **siteLocation** parameter. I hello **dependsOn** elementet hello mallen definierar hello webbprogram som är beroende av hello-tjänst som värd för planen. Eftersom det är beroende av hello värd plan, skapas inte hello webbprogrammet tills hello som värd för planen har slutförts skapas. Hej **dependsOn** element är bara använda toospecify distributionsordning. Om du inte markerar hello webbprogram som är beroende av hello värd plan Azure Resource Manager försöker toocreate båda resurserna på hello samma tid och du kan få ett felmeddelande om hello webbprogrammet har skapats innan hello som värd för planen.

hello webbprogrammet har också en underordnad resurs som definieras i **resurser** nedan. Den här underordnade resursen definierar källkontrollen för hello-projektet som distribueras med hello webbprogrammet. I den här mallen är hello källkontrollen länkade tooa viss GitHub-lagringsplatsen. Hej GitHub-lagringsplatsen är definierad med hello kod **”RepoUrl”: ”https://github.com/davidebbo-test/Mvc52Application.git”** kanske hårdkoda hello databasen URL när du vill toocreate en mall som distribuerar flera gånger en enstaka projekt men kräver hello minsta antal parametrar.
I stället för att hårdkoda hello databasen URL kan du lägga till en parameter för hello databasen URL och använda värdet för hello **RepoUrl** egenskapen.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-toorun-deployment"></a>Kommandon toorun distribution
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a>Azure CLI 2.0

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> Innehållet i JSON-fil för hello parametrar finns [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).
>
>

