---
title: aaaCreate en logikapp med en mall i Azure | Microsoft Docs
description: "Använd en logikapp för toodeploy en Azure Resource Manager-mall för att definiera arbetsflöden."
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
ms.openlocfilehash: efbacb534fc7f11e9b593aae4383480ce3a1752f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-logic-app-using-a-template"></a><span data-ttu-id="712dd-103">Skapa en Logic App med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="712dd-103">Create a Logic App using a template</span></span>
<span data-ttu-id="712dd-104">Mallar tillhandahåller ett snabbt sätt toouse olika kopplingar inom en logikapp.</span><span class="sxs-lookup"><span data-stu-id="712dd-104">Templates provide a quick way toouse different connectors within a logic app.</span></span> <span data-ttu-id="712dd-105">Logikappar innehåller Azure Resource Manager-mallar för toocreate en logikapp som kan använda toodefine business arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="712dd-105">Logic apps includes Azure Resource Manager templates for you toocreate a logic app that can be used toodefine business workflows.</span></span> <span data-ttu-id="712dd-106">Du kan definiera vilka resurser har distribuerats och hur toodefine parametrar när du distribuerar din logikapp.</span><span class="sxs-lookup"><span data-stu-id="712dd-106">You can define which resources are deployed, and how toodefine parameters when you deploy your logic app.</span></span> <span data-ttu-id="712dd-107">Du kan använda den här mallen för egna affärsscenarier eller anpassa den toomeet dina krav.</span><span class="sxs-lookup"><span data-stu-id="712dd-107">You can use this template for your own business scenarios, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="712dd-108">Mer information om hello logik appegenskaper finns [logik App arbetsflödet Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span><span class="sxs-lookup"><span data-stu-id="712dd-108">For more details on hello Logic app properties, see [Logic App Workflow Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span></span> 

<span data-ttu-id="712dd-109">Exempel på hello definition själva finns [definitioner för författare Logic Apps](logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="712dd-109">For examples of hello definition itself, see [Author Logic App definitions](logic-apps-author-definitions.md).</span></span> 

<span data-ttu-id="712dd-110">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="712dd-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="712dd-111">Hello fullständig mall, se [logiska appmallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="712dd-111">For hello complete template, see [Logic App template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span></span>

## <a name="what-you-deploy"></a><span data-ttu-id="712dd-112">Du distribuerar</span><span class="sxs-lookup"><span data-stu-id="712dd-112">What you deploy</span></span>
<span data-ttu-id="712dd-113">Med den här mallen kan du distribuera en logikapp.</span><span class="sxs-lookup"><span data-stu-id="712dd-113">With this template, you deploy a logic app.</span></span>

<span data-ttu-id="712dd-114">toorun hello distributionen väljer automatiskt hello efter knappen:</span><span class="sxs-lookup"><span data-stu-id="712dd-114">toorun hello deployment automatically, select hello following button:</span></span>  

<span data-ttu-id="712dd-115">[![Distribuera tooAzure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="712dd-115">[![Deploy tooAzure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="712dd-116">Parametrar</span><span class="sxs-lookup"><span data-stu-id="712dd-116">Parameters</span></span>
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a><span data-ttu-id="712dd-117">testUri</span><span class="sxs-lookup"><span data-stu-id="712dd-117">testUri</span></span>
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-toodeploy"></a><span data-ttu-id="712dd-118">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="712dd-118">Resources toodeploy</span></span>
### <a name="logic-app"></a><span data-ttu-id="712dd-119">Logikapp</span><span class="sxs-lookup"><span data-stu-id="712dd-119">Logic app</span></span>
<span data-ttu-id="712dd-120">Skapar hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="712dd-120">Creates hello logic app.</span></span>

<span data-ttu-id="712dd-121">hello mallar använder ett parametervärde för hello logik appens namn.</span><span class="sxs-lookup"><span data-stu-id="712dd-121">hello templates uses a parameter value for hello logic app name.</span></span> <span data-ttu-id="712dd-122">Anger hello platsen för hello logik app toohello samma plats som hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="712dd-122">It sets hello location of hello logic app toohello same location as hello resource group.</span></span> 

<span data-ttu-id="712dd-123">Denna viss definition körs en gång i timmen och pingar hello plats som anges i hello **testUri** parameter.</span><span class="sxs-lookup"><span data-stu-id="712dd-123">This particular definition runs once an hour, and pings hello location specified in hello **testUri** parameter.</span></span> 

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


## <a name="commands-toorun-deployment"></a><span data-ttu-id="712dd-124">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="712dd-124">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="712dd-125">PowerShell</span><span class="sxs-lookup"><span data-stu-id="712dd-125">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="712dd-126">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="712dd-126">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



