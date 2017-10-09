---
title: "aaaAutomatically aktivera diagnostikinställningar med hjälp av en Resource Manager-mall | Microsoft Docs"
description: "Lär dig hur toouse en resurshanterare toocreate diagnostiska mallinställningar som gör att du toostream din diagnostik loggar tooEvent nav eller lagra dem i ett lagringskonto."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a8a88a8c-4a48-4df6-8f7e-d90634d39c57
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/14/2017
ms.author: johnkem
ms.openlocfilehash: 8f38731107029928029c6d940da7bd076fea5d49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Aktivera automatiskt diagnostikinställningar när resursen skapas med hjälp av en Resource Manager-mall
I den här artikeln visar vi hur du kan använda en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure diagnostikinställningar för en resurs när den skapas. Detta gör att du tooautomatically starta strömning diagnostikloggar och mått tooEvent NAV, arkivering dem i ett Lagringskonto eller skicka dem tooLog Analytics när en resurs har skapats.

hello-metoden för att aktivera diagnostikloggar med hjälp av en Resource Manager-mall beror på hello resurstypen.

* **Icke-beräkning** resurser (till exempel Nätverkssäkerhetsgrupper Logic Apps Automation) [diagnostikinställningar som beskrivs i den här artikeln](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
* **Beräkna** (BOMULLSTUSS/LAD-baserade) resurser använder hello [BOMULLSTUSS/LAD konfigurationsfilen som beskrivs i den här artikeln](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

I den här artikeln beskriver vi hur tooconfigure diagnostik med någon av metoderna.

hello stegen är följande:

1. Skapa en mall som en JSON-fil som beskriver hur toocreate hello resurs och aktivera diagnostik.
2. [Distribuera hello mallen med hjälp av en distributionsmetod](../azure-resource-manager/resource-group-template-deploy.md).

Vi ger nedan ett exempel på hello mall JSON-fil som du behöver toogenerate för icke-beräknings- och beräkningsresurser.

## <a name="non-compute-resource-template"></a>Icke-beräkningsresurser mall
För icke-beräkningsresurser behöver du toodo två saker:

1. Lägg till parametrar toohello parametrar blob för hello lagringskontonamn, service bus regel-ID och/eller OMS logganalys arbetsyte-ID (aktivera arkivering av diagnostikloggar i ett lagringskonto, strömning av loggar tooEvent NAV och/eller skicka loggar tooLog Analytics).
   
    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for hello Service Bus Namespace in which hello Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for hello Log Analytics workspace toowhich logs will be sent."
      }
    }
    ```
2. Hello resurser matris med hello resurs som du vill tooenable diagnostikloggar, lägga till en resurs av typen `[resource namespace]/providers/diagnosticSettings`.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "timeGrain": "PT1M",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

hello egenskaper blob för hello Diagnostikinställningen följer [hello-format som beskrivs i den här artikeln](https://msdn.microsoft.com/library/azure/dn931931.aspx). Att lägga till hello `metrics` egenskapen Aktivera tooalso skicka resurs mått toothese samma matar ut, förutsatt att [hello resursen stöder Azure-Monitor mått](monitoring-supported-metrics.md).

Här är ett fullständigt exempel som skapar en Logikapp och aktiverar direktuppspelning tooEvent NAV och lagringsutrymme i ett lagringskonto.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for hello Service Bus Namespace in which hello Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for hello Log Analytics workspace toowhich logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
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
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Compute-resource-mall
tooenable diagnostik för en beräkning resurs, till exempel en virtuell dator eller Service Fabric-klustret måste du:

1. Lägg till hello Azure Diagnostics tillägget toohello VM resursdefinitionen.
2. Ange en storage-konto och/eller händelse hubb som en parameter.
3. Lägg till hello innehållet i WADCfg XML-filen i hello XMLCfg egenskapen undantagstecken alla XML-tecknen korrekt.

> [!WARNING]
> Det sista steget kan vara svårt tooget höger. [Finns den här artikeln](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) ett exempel som att delningar hello diagnostik Konfigurationsschemat till variabler som är undantagna och formaterats korrekt.
> 
> 

hello hela processen, inklusive exempel, beskrivs [i det här dokumentet](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Nästa steg
* [Läs mer om Azure diagnostikloggar](monitoring-overview-of-diagnostic-logs.md)
* [Dataströmmen Azure diagnostikloggar tooEvent hubbar](monitoring-stream-diagnostic-logs-to-event-hubs.md)

