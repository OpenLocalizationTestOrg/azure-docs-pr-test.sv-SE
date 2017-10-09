---
title: "aaaCreate Azure Service Bus-resurser med hjälp av Azure Resource Manager-mallar | Microsoft Docs"
description: "Använd Azure Resource Manager mallar tooautomate hello skapandet av Service Bus-resurser"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: e539902cae307b63ae7c332580e2064761331ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a><span data-ttu-id="87fb5-103">Skapa Service Bus-resurser med hjälp av Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="87fb5-103">Create Service Bus resources using Azure Resource Manager templates</span></span>

<span data-ttu-id="87fb5-104">Den här artikeln beskriver hur toocreate och distribuera Service Bus-resurser med Azure Resource Manager-mallar, PowerShell och hello Service Bus-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="87fb5-104">This article describes how toocreate and deploy Service Bus resources using Azure Resource Manager templates, PowerShell, and hello Service Bus resource provider.</span></span>

<span data-ttu-id="87fb5-105">Azure Resource Manager-mallar hjälpa dig att definiera hello resurser toodeploy för en lösning och toospecify parametrar och variabler som gör att du tooinput värden för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="87fb5-105">Azure Resource Manager templates help you define hello resources toodeploy for a solution, and toospecify parameters and variables that enable you tooinput values for different environments.</span></span> <span data-ttu-id="87fb5-106">hello mallen består av JSON och uttryck som du kan använda tooconstruct värden för din distribution.</span><span class="sxs-lookup"><span data-stu-id="87fb5-106">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="87fb5-107">Detaljerad information om hur du skriver Azure Resource Manager-mallar och en beskrivning av hello mallformat finns [struktur och syntaxen för Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="87fb5-107">For detailed information about writing Azure Resource Manager templates, and a discussion of hello template format, see [structure and syntax of Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

> [!NOTE]
> <span data-ttu-id="87fb5-108">Hej exemplen i den här artikeln visar hur toouse Azure Resource Manager toocreate en Service Bus-namnrymd och meddelanden entitet (kön).</span><span class="sxs-lookup"><span data-stu-id="87fb5-108">hello examples in this article show how toouse Azure Resource Manager toocreate a Service Bus namespace and messaging entity (queue).</span></span> <span data-ttu-id="87fb5-109">Andra mall-exempel finns hello [Azure Quickstart mallgalleriet] [ Azure Quickstart Templates gallery] och Sök efter ”Service Bus”.</span><span class="sxs-lookup"><span data-stu-id="87fb5-109">For other template examples, visit hello [Azure Quickstart Templates gallery][Azure Quickstart Templates gallery] and search for "Service Bus."</span></span>
>
>

## <a name="service-bus-resource-manager-templates"></a><span data-ttu-id="87fb5-110">Service Bus Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="87fb5-110">Service Bus Resource Manager templates</span></span>

<span data-ttu-id="87fb5-111">Dessa Service Bus Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.</span><span class="sxs-lookup"><span data-stu-id="87fb5-111">These Service Bus Azure Resource Manager templates are available for download and deployment.</span></span> <span data-ttu-id="87fb5-112">Klicka på följande länkar för information om var och en, med länkar toohello mallar på GitHub hello:</span><span class="sxs-lookup"><span data-stu-id="87fb5-112">Click hello following links for details about each one, with links toohello templates on GitHub:</span></span>

* [<span data-ttu-id="87fb5-113">Skapa ett namnområde för Service Bus</span><span class="sxs-lookup"><span data-stu-id="87fb5-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
* [<span data-ttu-id="87fb5-114">Skapa ett namnområde för Service Bus med kön</span><span class="sxs-lookup"><span data-stu-id="87fb5-114">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
* [<span data-ttu-id="87fb5-115">Skapa ett namnområde för Service Bus med ämnet och prenumerationen</span><span class="sxs-lookup"><span data-stu-id="87fb5-115">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
* [<span data-ttu-id="87fb5-116">Skapa ett namnområde för Service Bus med kön och auktorisering regel</span><span class="sxs-lookup"><span data-stu-id="87fb5-116">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
* [<span data-ttu-id="87fb5-117">Skapa ett namnområde för Service Bus med ämne, prenumeration och regel</span><span class="sxs-lookup"><span data-stu-id="87fb5-117">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a><span data-ttu-id="87fb5-118">Distribuera med PowerShell</span><span class="sxs-lookup"><span data-stu-id="87fb5-118">Deploy with PowerShell</span></span>

<span data-ttu-id="87fb5-119">hello nedan beskrivs hur toouse PowerShell toodeploy en Azure Resource Manager-mall som skapar en **Standard** tjänstnivån Service Bus-namnrymd och en kö inom det här namnområdet.</span><span class="sxs-lookup"><span data-stu-id="87fb5-119">hello following procedure describes how toouse PowerShell toodeploy an Azure Resource Manager template that creates a **Standard** tier Service Bus namespace, and a queue within that namespace.</span></span> <span data-ttu-id="87fb5-120">Det här exemplet är baserad på hello [skapa ett namnområde för Service Bus med kön](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) mall.</span><span class="sxs-lookup"><span data-stu-id="87fb5-120">This example is based on hello [Create a Service Bus namespace with queue](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) template.</span></span> <span data-ttu-id="87fb5-121">hello ungefärliga arbetsflödet är följande:</span><span class="sxs-lookup"><span data-stu-id="87fb5-121">hello approximate workflow is as follows:</span></span>

1. <span data-ttu-id="87fb5-122">Installera PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87fb5-122">Install PowerShell.</span></span>
2. <span data-ttu-id="87fb5-123">Skapa hello mall och (valfritt) en parameterfil.</span><span class="sxs-lookup"><span data-stu-id="87fb5-123">Create hello template and (optionally) a parameter file.</span></span>
3. <span data-ttu-id="87fb5-124">Logga in tooyour Azure-konto i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87fb5-124">In PowerShell, log in tooyour Azure account.</span></span>
4. <span data-ttu-id="87fb5-125">Skapa en ny resursgrupp om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="87fb5-125">Create a new resource group if one does not exist.</span></span>
5. <span data-ttu-id="87fb5-126">Testa hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="87fb5-126">Test hello deployment.</span></span>
6. <span data-ttu-id="87fb5-127">Om du vill ange hello distribution läge.</span><span class="sxs-lookup"><span data-stu-id="87fb5-127">If desired, set hello deployment mode.</span></span>
7. <span data-ttu-id="87fb5-128">Distribuera hello-mallen.</span><span class="sxs-lookup"><span data-stu-id="87fb5-128">Deploy hello template.</span></span>

<span data-ttu-id="87fb5-129">Fullständig information om hur du distribuerar Azure Resource Manager-mallar finns [distribuera resurser med Azure Resource Manager-mallar][Deploy resources with Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="87fb5-129">For complete information about deploying Azure Resource Manager templates, see [Deploy resources with Azure Resource Manager templates][Deploy resources with Azure Resource Manager templates].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="87fb5-130">Installera PowerShell</span><span class="sxs-lookup"><span data-stu-id="87fb5-130">Install PowerShell</span></span>

<span data-ttu-id="87fb5-131">Installera Azure PowerShell genom att följa instruktionerna hello i [komma igång med Azure PowerShell](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="87fb5-131">Install Azure PowerShell by following hello instructions in [Getting started with Azure PowerShell](/powershell/azure/get-started-azureps).</span></span>

### <a name="create-a-template"></a><span data-ttu-id="87fb5-132">Skapa en mall</span><span class="sxs-lookup"><span data-stu-id="87fb5-132">Create a template</span></span>

<span data-ttu-id="87fb5-133">Klona eller kopiera hello [201-servicebus-skapa-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) mall från GitHub:</span><span class="sxs-lookup"><span data-stu-id="87fb5-133">Clone or copy hello [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) template from GitHub:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by hello template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a><span data-ttu-id="87fb5-134">Skapa en fil med parametrar (valfritt)</span><span class="sxs-lookup"><span data-stu-id="87fb5-134">Create a parameters file (optional)</span></span>

<span data-ttu-id="87fb5-135">toouse valfria parameterfilen kopiera hello [201-servicebus-skapa-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) fil.</span><span class="sxs-lookup"><span data-stu-id="87fb5-135">toouse an optional parameters file, copy hello [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) file.</span></span> <span data-ttu-id="87fb5-136">Ersätt hello värdet för `serviceBusNamespaceName` du vill toocreate i den här distributionen med namnet hello hello Service Bus-namnrymd och ersätter hello värdet för `serviceBusQueueName` hello namnet på hello kö du vill använda toocreate.</span><span class="sxs-lookup"><span data-stu-id="87fb5-136">Replace hello value of `serviceBusNamespaceName` with hello name of hello Service Bus namespace you want toocreate in this deployment, and replace hello value of `serviceBusQueueName` with hello name of hello queue you want toocreate.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

<span data-ttu-id="87fb5-137">Mer information finns i hello [parametrar](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="87fb5-137">For more information, see hello [Parameters](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) topic.</span></span>

### <a name="log-in-tooazure-and-set-hello-azure-subscription"></a><span data-ttu-id="87fb5-138">Logga in tooAzure och ange hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="87fb5-138">Log in tooAzure and set hello Azure subscription</span></span>

<span data-ttu-id="87fb5-139">Kör hello följande kommando från en PowerShell-kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="87fb5-139">From a PowerShell prompt, run hello following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="87fb5-140">Du kan ange toolog på tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="87fb5-140">You are prompted toolog on tooyour Azure account.</span></span> <span data-ttu-id="87fb5-141">Efter inloggningen kör du följande kommando tooview hello tillgängliga prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="87fb5-141">After logging on, run hello following command tooview your available subscriptions.</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="87fb5-142">Det här kommandot returnerar en lista över tillgängliga Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="87fb5-142">This command returns a list of available Azure subscriptions.</span></span> <span data-ttu-id="87fb5-143">Välj en prenumeration för hello aktuell session genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="87fb5-143">Choose a subscription for hello current session by running hello following command.</span></span> <span data-ttu-id="87fb5-144">Ersätt `<YourSubscriptionId>` med hello GUID för hello Azure-prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="87fb5-144">Replace `<YourSubscriptionId>` with hello GUID for hello Azure subscription you want toouse.</span></span>

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-hello-resource-group"></a><span data-ttu-id="87fb5-145">Ange hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="87fb5-145">Set hello resource group</span></span>

<span data-ttu-id="87fb5-146">Om du inte har en befintlig resurs, skapa en ny resursgrupp med hello ** New-AzureRmResourceGroup ** kommando.</span><span class="sxs-lookup"><span data-stu-id="87fb5-146">If you do not have an existing resource group, create a new resource group with hello **New-AzureRmResourceGroup ** command.</span></span> <span data-ttu-id="87fb5-147">Ange hello namnet för hello resursgrupp och plats som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="87fb5-147">Provide hello name of hello resource group and location you want toouse.</span></span> <span data-ttu-id="87fb5-148">Exempel:</span><span class="sxs-lookup"><span data-stu-id="87fb5-148">For example:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

<span data-ttu-id="87fb5-149">Om detta lyckas visas en sammanfattning av hello ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="87fb5-149">If successful, a summary of hello new resource group is displayed.</span></span>

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-hello-deployment"></a><span data-ttu-id="87fb5-150">Testa hello-distribution</span><span class="sxs-lookup"><span data-stu-id="87fb5-150">Test hello deployment</span></span>

<span data-ttu-id="87fb5-151">Verifiera distributionen genom att köra hello `Test-AzureRmResourceGroupDeployment` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="87fb5-151">Validate your deployment by running hello `Test-AzureRmResourceGroupDeployment` cmdlet.</span></span> <span data-ttu-id="87fb5-152">När du testar hello distribution, ange parametrar exakt samma sätt som när du kör hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="87fb5-152">When testing hello deployment, provide parameters exactly as you would when executing hello deployment.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="create-hello-deployment"></a><span data-ttu-id="87fb5-153">Skapa hello-distribution</span><span class="sxs-lookup"><span data-stu-id="87fb5-153">Create hello deployment</span></span>

<span data-ttu-id="87fb5-154">toocreate hello ny distribution, kör hello `New-AzureRmResourceGroupDeployment` cmdlet, och ange hello nödvändiga parametrar när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="87fb5-154">toocreate hello new deployment, run hello `New-AzureRmResourceGroupDeployment` cmdlet, and provide hello necessary parameters when prompted.</span></span> <span data-ttu-id="87fb5-155">parametrarna för hello innehålla ett namn för din distribution, hello namnet på resursgruppen och hello sökväg eller URL toohello mallfilen.</span><span class="sxs-lookup"><span data-stu-id="87fb5-155">hello parameters include a name for your deployment, hello name of your resource group, and hello path or URL toohello template file.</span></span> <span data-ttu-id="87fb5-156">Om hello **läge** parametern anges hello standardvärdet **stegvis** används.</span><span class="sxs-lookup"><span data-stu-id="87fb5-156">If hello **Mode** parameter is not specified, hello default value of **Incremental** is used.</span></span> <span data-ttu-id="87fb5-157">Mer information finns i [inkrementell och fullständig distributioner](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).</span><span class="sxs-lookup"><span data-stu-id="87fb5-157">For more information, see [Incremental and complete deployments](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).</span></span>

<span data-ttu-id="87fb5-158">Hej följande kommandotolkar du för hello tre parametrar i hello PowerShell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="87fb5-158">hello following command prompts you for hello three parameters in hello PowerShell window:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

<span data-ttu-id="87fb5-159">toospecify en fil med parametrar i stället använda hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="87fb5-159">toospecify a parameters file instead, use hello following command.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -TemplateParameterFile <path tooparameters file>\azuredeploy.parameters.json
```

<span data-ttu-id="87fb5-160">Du kan också använda infogade parametrar när du kör hello distributions-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="87fb5-160">You can also use inline parameters when you run hello deployment cmdlet.</span></span> <span data-ttu-id="87fb5-161">hello-kommandot är följande:</span><span class="sxs-lookup"><span data-stu-id="87fb5-161">hello command is as follows:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -parameterName "parameterValue"
```

<span data-ttu-id="87fb5-162">toorun en [fullständig](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) distribution, ange hello **läge** parameter för**Slutför**:</span><span class="sxs-lookup"><span data-stu-id="87fb5-162">toorun a [complete](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) deployment, set hello **Mode** parameter too**Complete**:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="verify-hello-deployment"></a><span data-ttu-id="87fb5-163">Kontrollera distributionen av hello</span><span class="sxs-lookup"><span data-stu-id="87fb5-163">Verify hello deployment</span></span>
<span data-ttu-id="87fb5-164">Om hello resurser har distribuerats visas en sammanfattning av hello distribution i hello PowerShell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="87fb5-164">If hello resources are deployed successfully, a summary of hello deployment is displayed in hello PowerShell window:</span></span>

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a><span data-ttu-id="87fb5-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="87fb5-165">Next steps</span></span>
<span data-ttu-id="87fb5-166">Nu har du sett hello grundläggande arbetsflöde och kommandon för att distribuera en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="87fb5-166">You've now seen hello basic workflow and commands for deploying an Azure Resource Manager template.</span></span> <span data-ttu-id="87fb5-167">Mer detaljerad information finns i hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="87fb5-167">For more detailed information, visit hello following links:</span></span>

* <span data-ttu-id="87fb5-168">[Översikt över Azure Resource Manager][Azure Resource Manager overview]</span><span class="sxs-lookup"><span data-stu-id="87fb5-168">[Azure Resource Manager overview][Azure Resource Manager overview]</span></span>
* <span data-ttu-id="87fb5-169">[Distribuera resurser med Resource Manager-mallar och Azure PowerShell][Deploy resources with Azure Resource Manager templates]</span><span class="sxs-lookup"><span data-stu-id="87fb5-169">[Deploy resources with Resource Manager templates and Azure PowerShell][Deploy resources with Azure Resource Manager templates]</span></span>
* [<span data-ttu-id="87fb5-170">Skapa Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="87fb5-170">Authoring Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
