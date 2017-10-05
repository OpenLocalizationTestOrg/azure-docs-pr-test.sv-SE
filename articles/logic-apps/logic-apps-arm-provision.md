---
title: Skapa en logikapp med en mall i Azure | Microsoft Docs
description: "Använd en mall för Azure Resource Manager för att distribuera en logikapp för att definiera arbetsflöden."
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 161adeacd6da2b15225c8a4ddae171e19e539967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-logic-app-using-a-template"></a><span data-ttu-id="36c2f-103">Skapa en Logic App med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="36c2f-103">Create a Logic App using a template</span></span>
<span data-ttu-id="36c2f-104">Mallar tillhandahåller ett snabbt sätt att använda olika kopplingar inom en logikapp.</span><span class="sxs-lookup"><span data-stu-id="36c2f-104">Templates provide a quick way to use different connectors within a logic app.</span></span> <span data-ttu-id="36c2f-105">Logikappar innehåller Azure Resource Manager-mallar att skapa en logikapp som kan användas för att definiera arbetsflöden för företag.</span><span class="sxs-lookup"><span data-stu-id="36c2f-105">Logic apps includes Azure Resource Manager templates for you to create a logic app that can be used to define business workflows.</span></span> <span data-ttu-id="36c2f-106">Du kan definiera vilka resurser har distribuerats och hur du definierar parametrar när du distribuerar din logikapp.</span><span class="sxs-lookup"><span data-stu-id="36c2f-106">You can define which resources are deployed, and how to define parameters when you deploy your logic app.</span></span> <span data-ttu-id="36c2f-107">Du kan använda mallen för egna affärsscenarier eller anpassa den så att den uppfyller dina krav.</span><span class="sxs-lookup"><span data-stu-id="36c2f-107">You can use this template for your own business scenarios, or customize it to meet your requirements.</span></span>

<span data-ttu-id="36c2f-108">Mer information om appegenskaper logik finns [logik App arbetsflödet Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span><span class="sxs-lookup"><span data-stu-id="36c2f-108">For more details on the Logic app properties, see [Logic App Workflow Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span></span> 

<span data-ttu-id="36c2f-109">Exempel på definition själva finns [definitioner för författare Logic Apps](logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="36c2f-109">For examples of the definition itself, see [Author Logic App definitions](logic-apps-author-definitions.md).</span></span> 

<span data-ttu-id="36c2f-110">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="36c2f-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="36c2f-111">Den fullständiga mallen finns [logiska appmallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="36c2f-111">For the complete template, see [Logic App template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span></span>

## <a name="what-you-deploy"></a><span data-ttu-id="36c2f-112">Du distribuerar</span><span class="sxs-lookup"><span data-stu-id="36c2f-112">What you deploy</span></span>
<span data-ttu-id="36c2f-113">Med den här mallen kan du distribuera en logikapp.</span><span class="sxs-lookup"><span data-stu-id="36c2f-113">With this template, you deploy a logic app.</span></span>

<span data-ttu-id="36c2f-114">Klicka på följande om du vill köra distributionen automatiskt:</span><span class="sxs-lookup"><span data-stu-id="36c2f-114">To run the deployment automatically, select the following button:</span></span>  

<span data-ttu-id="36c2f-115">[![Distribuera till Azure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="36c2f-115">[![Deploy to Azure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="36c2f-116">Parametrar</span><span class="sxs-lookup"><span data-stu-id="36c2f-116">Parameters</span></span>
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a><span data-ttu-id="36c2f-117">testUri</span><span class="sxs-lookup"><span data-stu-id="36c2f-117">testUri</span></span>
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-to-deploy"></a><span data-ttu-id="36c2f-118">Resurser som ska distribueras</span><span class="sxs-lookup"><span data-stu-id="36c2f-118">Resources to deploy</span></span>
### <a name="logic-app"></a><span data-ttu-id="36c2f-119">Logikapp</span><span class="sxs-lookup"><span data-stu-id="36c2f-119">Logic app</span></span>
<span data-ttu-id="36c2f-120">Skapar logikappen.</span><span class="sxs-lookup"><span data-stu-id="36c2f-120">Creates the logic app.</span></span>

<span data-ttu-id="36c2f-121">Mallarna använder ett parametervärde för appnamnet logik.</span><span class="sxs-lookup"><span data-stu-id="36c2f-121">The templates uses a parameter value for the logic app name.</span></span> <span data-ttu-id="36c2f-122">Den anger platsen för logikappen till samma plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="36c2f-122">It sets the location of the logic app to the same location as the resource group.</span></span> 

<span data-ttu-id="36c2f-123">Denna viss definition körs en gång i timmen och pingar den plats som anges i den **testUri** parameter.</span><span class="sxs-lookup"><span data-stu-id="36c2f-123">This particular definition runs once an hour, and pings the location specified in the **testUri** parameter.</span></span> 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a><span data-ttu-id="36c2f-124">Kommandon för att köra distributionen</span><span class="sxs-lookup"><span data-stu-id="36c2f-124">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="36c2f-125">PowerShell</span><span class="sxs-lookup"><span data-stu-id="36c2f-125">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="36c2f-126">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="36c2f-126">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



