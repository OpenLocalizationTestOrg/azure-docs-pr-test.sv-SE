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
# <a name="deploy-a-web-app-linked-tooa-github-repository"></a><span data-ttu-id="26618-103">Distribuera en web app länkade tooa GitHub-databas</span><span class="sxs-lookup"><span data-stu-id="26618-103">Deploy a web app linked tooa GitHub repository</span></span>
<span data-ttu-id="26618-104">I det här avsnittet får du lära dig hur toocreate en Azure Resource Manager-mall som distribuerar ett webbprogram som är länkade tooa projekt i en GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="26618-104">In this topic, you will learn how toocreate an Azure Resource Manager template that deploys a web app that is linked tooa project in a GitHub repository.</span></span> <span data-ttu-id="26618-105">Du får lära dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="26618-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="26618-106">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.</span><span class="sxs-lookup"><span data-stu-id="26618-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="26618-107">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="26618-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="26618-108">Hello fullständig mall, se [Web App länkad tooGitHub mall](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="26618-108">For hello complete template, see [Web App Linked tooGitHub template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a><span data-ttu-id="26618-109">Vad du ska distribuera</span><span class="sxs-lookup"><span data-stu-id="26618-109">What you will deploy</span></span>
<span data-ttu-id="26618-110">Med den här mallen ska du distribuera en webbapp som innehåller hello kod från ett projekt i GitHub.</span><span class="sxs-lookup"><span data-stu-id="26618-110">With this template, you will deploy a web app that contains hello code from a project in GitHub.</span></span>

<span data-ttu-id="26618-111">toorun Hej distributionen automatiskt, klickar du på följande knapp hello:</span><span class="sxs-lookup"><span data-stu-id="26618-111">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="26618-112">[![Distribuera tooAzure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="26618-112">[![Deploy tooAzure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="26618-113">Parametrar</span><span class="sxs-lookup"><span data-stu-id="26618-113">Parameters</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a><span data-ttu-id="26618-114">repoURL</span><span class="sxs-lookup"><span data-stu-id="26618-114">repoURL</span></span>
<span data-ttu-id="26618-115">hello URL till GitHub-lagringsplats som innehåller hello projekt toodeploy.</span><span class="sxs-lookup"><span data-stu-id="26618-115">hello URL for GitHub repository that contains hello project toodeploy.</span></span> <span data-ttu-id="26618-116">Den här parametern innehåller ett standardvärde, men det här värdet är bara avsedd tooshow du hur tooprovide hello URL för databasen.</span><span class="sxs-lookup"><span data-stu-id="26618-116">This parameter contains a default value but this value is only intended tooshow you how tooprovide hello URL for repository.</span></span> <span data-ttu-id="26618-117">Du kan använda det här värdet när du testar hello mallen men du vill tooprovide hello-URL: en egen databas när du arbetar med hello mallen.</span><span class="sxs-lookup"><span data-stu-id="26618-117">You can use this value when testing hello template but you will want tooprovide hello URL your own repository when working with hello template.</span></span>

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a><span data-ttu-id="26618-118">Avdelningskontor</span><span class="sxs-lookup"><span data-stu-id="26618-118">branch</span></span>
<span data-ttu-id="26618-119">hello gren av databasen hello toouse vid distribution av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="26618-119">hello branch of hello repository toouse when deploying hello application.</span></span> <span data-ttu-id="26618-120">hello standardvärdet är master, men du kan ange hello namnet på en gren i hello databas du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="26618-120">hello default value is master, but you can provide hello name of any branch in hello repository that you wish toodeploy.</span></span>

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-toodeploy"></a><span data-ttu-id="26618-121">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="26618-121">Resources toodeploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a><span data-ttu-id="26618-122">Webbapp</span><span class="sxs-lookup"><span data-stu-id="26618-122">Web app</span></span>
<span data-ttu-id="26618-123">Skapar hello webbprogram som är länkade toohello projekt i GitHub.</span><span class="sxs-lookup"><span data-stu-id="26618-123">Creates hello web app that is linked toohello project in GitHub.</span></span> 

<span data-ttu-id="26618-124">Anger hello namnet på hello webbprogram via hello **siteName** parametern och hello platsen för hello webbprogram via hello **siteLocation** parameter.</span><span class="sxs-lookup"><span data-stu-id="26618-124">You specify hello name of hello web app through hello **siteName** parameter, and hello location of hello web app through hello **siteLocation** parameter.</span></span> <span data-ttu-id="26618-125">I hello **dependsOn** elementet hello mallen definierar hello webbprogram som är beroende av hello-tjänst som värd för planen.</span><span class="sxs-lookup"><span data-stu-id="26618-125">In hello **dependsOn** element, hello template defines hello web app as dependent on hello service hosting plan.</span></span> <span data-ttu-id="26618-126">Eftersom det är beroende av hello värd plan, skapas inte hello webbprogrammet tills hello som värd för planen har slutförts skapas.</span><span class="sxs-lookup"><span data-stu-id="26618-126">Because it is dependent on hello hosting plan, hello web app is not created until hello hosting plan has finished being created.</span></span> <span data-ttu-id="26618-127">Hej **dependsOn** element är bara använda toospecify distributionsordning.</span><span class="sxs-lookup"><span data-stu-id="26618-127">hello **dependsOn** element is only used toospecify deployment order.</span></span> <span data-ttu-id="26618-128">Om du inte markerar hello webbprogram som är beroende av hello värd plan Azure Resource Manager försöker toocreate båda resurserna på hello samma tid och du kan få ett felmeddelande om hello webbprogrammet har skapats innan hello som värd för planen.</span><span class="sxs-lookup"><span data-stu-id="26618-128">If you do not mark hello web app as dependent on hello hosting plan, Azure Resource Mananger will attempt toocreate both resources at hello same time and you may receive an error if hello web app is created before hello hosting plan.</span></span>

<span data-ttu-id="26618-129">hello webbprogrammet har också en underordnad resurs som definieras i **resurser** nedan.</span><span class="sxs-lookup"><span data-stu-id="26618-129">hello web app also has a child resource which is defined in **resources** section below.</span></span> <span data-ttu-id="26618-130">Den här underordnade resursen definierar källkontrollen för hello-projektet som distribueras med hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="26618-130">This child resource defines source control for hello project deployed with hello web app.</span></span> <span data-ttu-id="26618-131">I den här mallen är hello källkontrollen länkade tooa viss GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="26618-131">In this template, hello source control is linked tooa particular GitHub repository.</span></span> <span data-ttu-id="26618-132">Hej GitHub-lagringsplatsen är definierad med hello kod **”RepoUrl”: ”https://github.com/davidebbo-test/Mvc52Application.git”** kanske hårdkoda hello databasen URL när du vill toocreate en mall som distribuerar flera gånger en enstaka projekt men kräver hello minsta antal parametrar.</span><span class="sxs-lookup"><span data-stu-id="26618-132">hello GitHub repository is defined with hello code **"RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git"** You might hard-code hello repository URL when you want toocreate a template that repeatedly deploys a single project while requiring hello minimum number of parameters.</span></span>
<span data-ttu-id="26618-133">I stället för att hårdkoda hello databasen URL kan du lägga till en parameter för hello databasen URL och använda värdet för hello **RepoUrl** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="26618-133">Instead of hard-coding hello repository URL, you can add a parameter for hello repository URL and use that value for hello **RepoUrl** property.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="26618-134">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="26618-134">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="26618-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="26618-135">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="26618-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="26618-136">Azure CLI</span></span>

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a><span data-ttu-id="26618-137">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="26618-137">Azure CLI 2.0</span></span>

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> <span data-ttu-id="26618-138">Innehållet i JSON-fil för hello parametrar finns [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span><span class="sxs-lookup"><span data-stu-id="26618-138">For content of hello parameters JSON file, see [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span></span>
>
>

