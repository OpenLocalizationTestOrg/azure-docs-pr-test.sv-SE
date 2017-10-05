---
title: "Aktivera automatiskt diagnostikinställningar med hjälp av en Resource Manager-mall | Microsoft Docs"
description: "Lär dig hur du skapar diagnostikinställningar som gör det möjligt att strömma dina diagnostikloggar i Händelsehubbar eller lagra dem i ett lagringskonto med hjälp av en Resource Manager-mall."
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
ms.openlocfilehash: dde2435e976bbd14ca35cccc714ea21dcc5817b7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Aktivera automatiskt diagnostikinställningar när resursen skapas med hjälp av en Resource Manager-mall
I den här artikeln visar vi hur du kan använda en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md) Konfigurera diagnostikinställningar på en resurs när den skapas. På så sätt kan du starta strömning dina diagnostikloggar och mått i Händelsehubbar arkivering dem i ett Lagringskonto eller skicka dem till logganalys när en resurs skapas automatiskt.

Metoden för att aktivera diagnostikloggar med hjälp av en Resource Manager-mall beror på resurstypen.

* **Icke-beräkning** resurser (till exempel Nätverkssäkerhetsgrupper Logic Apps Automation) [diagnostikinställningar som beskrivs i den här artikeln](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
* **Beräkna** (BOMULLSTUSS/LAD-baserade) resurser den [BOMULLSTUSS/LAD konfigurationsfilen som beskrivs i den här artikeln](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

I den här artikeln beskriver vi hur du konfigurerar diagnostik med någon av metoderna.

Stegen är följande:

1. Skapa en mall som en JSON-fil som beskriver hur du skapar resursen och aktivera diagnostik.
2. [Distribuera mallen med hjälp av en distributionsmetod](../azure-resource-manager/resource-group-template-deploy.md).

Nedan kan vi ge ett exempel på mallen JSON-fil måste du generera för icke-beräknings- och beräkningsresurser.

## <a name="non-compute-resource-template"></a>Icke-beräkningsresurser mall
För icke-beräkningsresurser behöver du göra två saker:

1. Lägga till parametrar till blob med parametrar för lagringskontonamn, service bus regel-ID och/eller OMS logganalys arbetsyte-ID (aktivera arkivering av diagnostikloggar i ett lagringskonto, strömning av loggar för Händelsehubbar och/eller skicka loggar till logganalys).
   
    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Lägg till en resurs av typen i matrisen resurser på den resurs som du vill aktivera diagnostikloggar `[resource namespace]/providers/diagnosticSettings`.
   
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

Egenskaper för blob för Diagnostikinställningen följer [det format som beskrivs i den här artikeln](https://msdn.microsoft.com/library/azure/dn931931.aspx). Att lägga till den `metrics` egenskapen gör att du kan även skicka resurs mått till dessa samma utdata, under förutsättning att [resursen stöder Azure-Monitor mått](monitoring-supported-metrics.md).

Här är ett fullständigt exempel som skapar en Logikapp och aktiverar direktuppspelning Händelsehubbar och lagring i ett lagringskonto.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
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
Om du vill aktivera diagnostik för en beräkning resurs, till exempel en virtuell dator eller Service Fabric-kluster måste du:

1. Lägga till filnamnstillägget Azure Diagnostics resursdefinitionen VM.
2. Ange en storage-konto och/eller händelse hubb som en parameter.
3. Lägg till innehållet i filen WADCfg XML i egenskapen XMLCfg undantagstecken alla XML-tecknen korrekt.

> [!WARNING]
> Det sista steget kan vara svårt att få rätt. [Finns den här artikeln](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) ett exempel som delar upp Konfigurationsschemat diagnostik i variabler som är undantagna och formaterats korrekt.
> 
> 

Hela processen, inklusive exempel, beskrivs [i det här dokumentet](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Nästa steg
* [Läs mer om Azure diagnostikloggar](monitoring-overview-of-diagnostic-logs.md)
* [Strömma Azure diagnostikloggar i Händelsehubbar](monitoring-stream-diagnostic-logs-to-event-hubs.md)

