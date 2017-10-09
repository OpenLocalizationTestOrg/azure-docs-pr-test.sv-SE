---
title: aaaCreate en aktivitet loggen avisering med en Resource Manager-mall | Microsoft Docs
description: "Håll dig informerad när resurserna i Azure skapas."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: ancav
ms.openlocfilehash: 0fb8aa037b9dce54ce35498622770955f2341bc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a><span data-ttu-id="cc648-103">Skapa en aktivitet loggen avisering med en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="cc648-103">Create an activity log alert with a Resource Manager template</span></span>
<span data-ttu-id="cc648-104">Den här artikeln beskrivs hur du toouse en [Azure Resource Manager-mall](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure aktivitet loggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="cc648-104">This article shows you how toouse an [Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure activity log alerts.</span></span> <span data-ttu-id="cc648-105">Med hjälp av mallar kan du enkelt konfigurera många aviseringar som aktiverar baserat på specifika aktivitet loggen händelse villkor som en del av processen för automatisk distribution.</span><span class="sxs-lookup"><span data-stu-id="cc648-105">By using templates, you can easily set up many alerts that activate based on specific activity log event conditions as part of your automated deployment process.</span></span>

<span data-ttu-id="cc648-106">hello stegen är:</span><span class="sxs-lookup"><span data-stu-id="cc648-106">hello basic steps are:</span></span>

1. <span data-ttu-id="cc648-107">Skapa en mall som en JSON-fil som beskriver hur toocreate hello aktivitet logga avisering.</span><span class="sxs-lookup"><span data-stu-id="cc648-107">Create a template as a JSON file that describes how toocreate hello activity log alert.</span></span>

2. <span data-ttu-id="cc648-108">Distribuera hello mallen med [alla distributionsmetoden](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="cc648-108">Deploy hello template by using [any deployment method](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span>

## <a name="resource-manager-template-for-an-activity-log-alert"></a><span data-ttu-id="cc648-109">Resource Manager-mall för en aktivitet log-varning</span><span class="sxs-lookup"><span data-stu-id="cc648-109">Resource Manager template for an activity log alert</span></span>
<span data-ttu-id="cc648-110">toocreate en varning för loggen med hjälp av en Resource Manager-mall som du skapar en resurs av typen hello `microsoft.insights/activityLogAlerts`.</span><span class="sxs-lookup"><span data-stu-id="cc648-110">toocreate an activity log alert by using a Resource Manager template, you create a resource of hello type `microsoft.insights/activityLogAlerts`.</span></span> <span data-ttu-id="cc648-111">Du fylla i alla relaterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="cc648-111">Then you fill in all related properties.</span></span> <span data-ttu-id="cc648-112">Här är en mall som skapar en avisering för aktiviteten loggen.</span><span class="sxs-lookup"><span data-stu-id="cc648-112">Here's a template that creates an activity log alert.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within hello Resource Group) for hello Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not hello alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for hello Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ] 
        },
        "actions": {
          "actionGroups": 
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

<span data-ttu-id="cc648-113">Besök vår [Azure snabbstartsgalleriet](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) några exempel på aviseringen aktivitetsmallar för loggen.</span><span class="sxs-lookup"><span data-stu-id="cc648-113">Visit our [Azure Quickstart gallery](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) for some examples of activity log alert templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc648-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cc648-114">Next steps</span></span>
- <span data-ttu-id="cc648-115">Lär dig mer om [aviseringar](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="cc648-115">Learn more about [alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="cc648-116">Lär dig hur tooadd [åtgärdsgrupper med hjälp av en Resource Manager-mall](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="cc648-116">Learn how tooadd [action groups by using a Resource Manager template](monitoring-create-action-group-with-resource-manager-template.md).</span></span>
- <span data-ttu-id="cc648-117">Lär dig hur för[och skapa en aktivitet loggen avisering toomonitor alla Autoskala motorn åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="cc648-117">Learn how too[create an activity log alert toomonitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="cc648-118">Lär dig hur för[och skapa en aktivitet loggen avisering toomonitor alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="cc648-118">Learn how too[create an activity log alert toomonitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
