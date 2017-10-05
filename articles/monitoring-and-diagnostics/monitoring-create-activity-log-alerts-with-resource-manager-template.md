---
title: Skapa en aktivitet loggen avisering med en Resource Manager-mall | Microsoft Docs
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
ms.openlocfilehash: 92076c7fe1f867919b7e02abf79cf0fb74fb7eb4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a><span data-ttu-id="5b54e-103">Skapa en aktivitet loggen avisering med en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="5b54e-103">Create an activity log alert with a Resource Manager template</span></span>
<span data-ttu-id="5b54e-104">Den här artikeln visar hur du använder en [Azure Resource Manager-mall](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) att konfigurera aviseringar för aktiviteten loggen.</span><span class="sxs-lookup"><span data-stu-id="5b54e-104">This article shows you how to use an [Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) to configure activity log alerts.</span></span> <span data-ttu-id="5b54e-105">Med hjälp av mallar kan du enkelt konfigurera många aviseringar som aktiverar baserat på specifika aktivitet loggen händelse villkor som en del av processen för automatisk distribution.</span><span class="sxs-lookup"><span data-stu-id="5b54e-105">By using templates, you can easily set up many alerts that activate based on specific activity log event conditions as part of your automated deployment process.</span></span>

<span data-ttu-id="5b54e-106">Stegen är:</span><span class="sxs-lookup"><span data-stu-id="5b54e-106">The basic steps are:</span></span>

1. <span data-ttu-id="5b54e-107">Skapa en mall som en JSON-fil som beskriver hur du skapar aktiviteten loggen aviseringen.</span><span class="sxs-lookup"><span data-stu-id="5b54e-107">Create a template as a JSON file that describes how to create the activity log alert.</span></span>

2. <span data-ttu-id="5b54e-108">Distribuera mallen med hjälp av [alla distributionsmetoden](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="5b54e-108">Deploy the template by using [any deployment method](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span>

## <a name="resource-manager-template-for-an-activity-log-alert"></a><span data-ttu-id="5b54e-109">Resource Manager-mall för en aktivitet log-varning</span><span class="sxs-lookup"><span data-stu-id="5b54e-109">Resource Manager template for an activity log alert</span></span>
<span data-ttu-id="5b54e-110">Om du vill skapa en aktivitet loggen avisering med hjälp av en Resource Manager-mall som du skapar en resurs av typen `microsoft.insights/activityLogAlerts`.</span><span class="sxs-lookup"><span data-stu-id="5b54e-110">To create an activity log alert by using a Resource Manager template, you create a resource of the type `microsoft.insights/activityLogAlerts`.</span></span> <span data-ttu-id="5b54e-111">Du fylla i alla relaterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5b54e-111">Then you fill in all related properties.</span></span> <span data-ttu-id="5b54e-112">Här är en mall som skapar en avisering för aktiviteten loggen.</span><span class="sxs-lookup"><span data-stu-id="5b54e-112">Here's a template that creates an activity log alert.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
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

<span data-ttu-id="5b54e-113">Besök vår [Azure snabbstartsgalleriet](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) några exempel på aviseringen aktivitetsmallar för loggen.</span><span class="sxs-lookup"><span data-stu-id="5b54e-113">Visit our [Azure Quickstart gallery](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) for some examples of activity log alert templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b54e-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b54e-114">Next steps</span></span>
- <span data-ttu-id="5b54e-115">Lär dig mer om [aviseringar](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="5b54e-115">Learn more about [alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="5b54e-116">Lär dig hur du lägger till [åtgärdsgrupper med hjälp av en Resource Manager-mall](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="5b54e-116">Learn how to add [action groups by using a Resource Manager template](monitoring-create-action-group-with-resource-manager-template.md).</span></span>
- <span data-ttu-id="5b54e-117">Lär dig hur du [skapa en varning om du vill övervaka alla Autoskala motorn åtgärder för din prenumeration loggen](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="5b54e-117">Learn how to [create an activity log alert to monitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="5b54e-118">Lär dig hur du [skapa en varning om du vill övervaka alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration loggen](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="5b54e-118">Learn how to [create an activity log alert to monitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
