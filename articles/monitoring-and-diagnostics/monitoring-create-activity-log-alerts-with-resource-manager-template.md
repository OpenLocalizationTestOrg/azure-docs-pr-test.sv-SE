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
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a>Skapa en aktivitet loggen avisering med en Resource Manager-mall
Den här artikeln beskrivs hur du toouse en [Azure Resource Manager-mall](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure aktivitet loggen aviseringar. Med hjälp av mallar kan du enkelt konfigurera många aviseringar som aktiverar baserat på specifika aktivitet loggen händelse villkor som en del av processen för automatisk distribution.

hello stegen är:

1. Skapa en mall som en JSON-fil som beskriver hur toocreate hello aktivitet logga avisering.

2. Distribuera hello mallen med [alla distributionsmetoden](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

## <a name="resource-manager-template-for-an-activity-log-alert"></a>Resource Manager-mall för en aktivitet log-varning
toocreate en varning för loggen med hjälp av en Resource Manager-mall som du skapar en resurs av typen hello `microsoft.insights/activityLogAlerts`. Du fylla i alla relaterade egenskaper. Här är en mall som skapar en avisering för aktiviteten loggen.

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

Besök vår [Azure snabbstartsgalleriet](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) några exempel på aviseringen aktivitetsmallar för loggen.

## <a name="next-steps"></a>Nästa steg
- Lär dig mer om [aviseringar](monitoring-overview-alerts.md).
- Lär dig hur tooadd [åtgärdsgrupper med hjälp av en Resource Manager-mall](monitoring-create-action-group-with-resource-manager-template.md).
- Lär dig hur för[och skapa en aktivitet loggen avisering toomonitor alla Autoskala motorn åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Lär dig hur för[och skapa en aktivitet loggen avisering toomonitor alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
