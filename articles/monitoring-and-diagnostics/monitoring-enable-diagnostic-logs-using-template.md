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
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a><span data-ttu-id="43029-103">Aktivera automatiskt diagnostikinställningar när resursen skapas med hjälp av en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="43029-103">Automatically enable Diagnostic Settings at resource creation using a Resource Manager template</span></span>
<span data-ttu-id="43029-104">I den här artikeln visar vi hur du kan använda en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md) Konfigurera diagnostikinställningar på en resurs när den skapas.</span><span class="sxs-lookup"><span data-stu-id="43029-104">In this article we show how you can use an [Azure Resource Manager template](../azure-resource-manager/resource-group-authoring-templates.md) to configure Diagnostic Settings on a resource when it is created.</span></span> <span data-ttu-id="43029-105">På så sätt kan du starta strömning dina diagnostikloggar och mått i Händelsehubbar arkivering dem i ett Lagringskonto eller skicka dem till logganalys när en resurs skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="43029-105">This enables you to automatically start streaming your Diagnostic Logs and metrics to Event Hubs, archiving them in a Storage Account, or sending them to Log Analytics when a resource is created.</span></span>

<span data-ttu-id="43029-106">Metoden för att aktivera diagnostikloggar med hjälp av en Resource Manager-mall beror på resurstypen.</span><span class="sxs-lookup"><span data-stu-id="43029-106">The method for enabling Diagnostic Logs using a Resource Manager template depends on the resource type.</span></span>

* <span data-ttu-id="43029-107">**Icke-beräkning** resurser (till exempel Nätverkssäkerhetsgrupper Logic Apps Automation) [diagnostikinställningar som beskrivs i den här artikeln](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="43029-107">**Non-Compute** resources (for example, Network Security Groups, Logic Apps, Automation) use [Diagnostic Settings described in this article](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span>
* <span data-ttu-id="43029-108">**Beräkna** (BOMULLSTUSS/LAD-baserade) resurser den [BOMULLSTUSS/LAD konfigurationsfilen som beskrivs i den här artikeln](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span><span class="sxs-lookup"><span data-stu-id="43029-108">**Compute** (WAD/LAD-based) resources use the [WAD/LAD configuration file described in this article](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span></span>

<span data-ttu-id="43029-109">I den här artikeln beskriver vi hur du konfigurerar diagnostik med någon av metoderna.</span><span class="sxs-lookup"><span data-stu-id="43029-109">In this article we describe how to configure diagnostics using either method.</span></span>

<span data-ttu-id="43029-110">Stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="43029-110">The basic steps are as follows:</span></span>

1. <span data-ttu-id="43029-111">Skapa en mall som en JSON-fil som beskriver hur du skapar resursen och aktivera diagnostik.</span><span class="sxs-lookup"><span data-stu-id="43029-111">Create a template as a JSON file that describes how to create the resource and enable diagnostics.</span></span>
2. <span data-ttu-id="43029-112">[Distribuera mallen med hjälp av en distributionsmetod](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="43029-112">[Deploy the template using any deployment method](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

<span data-ttu-id="43029-113">Nedan kan vi ge ett exempel på mallen JSON-fil måste du generera för icke-beräknings- och beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="43029-113">Below we give an example of the template JSON file you need to generate for non-Compute and Compute resources.</span></span>

## <a name="non-compute-resource-template"></a><span data-ttu-id="43029-114">Icke-beräkningsresurser mall</span><span class="sxs-lookup"><span data-stu-id="43029-114">Non-Compute resource template</span></span>
<span data-ttu-id="43029-115">För icke-beräkningsresurser behöver du göra två saker:</span><span class="sxs-lookup"><span data-stu-id="43029-115">For non-Compute resources, you will need to do two things:</span></span>

1. <span data-ttu-id="43029-116">Lägga till parametrar till blob med parametrar för lagringskontonamn, service bus regel-ID och/eller OMS logganalys arbetsyte-ID (aktivera arkivering av diagnostikloggar i ett lagringskonto, strömning av loggar för Händelsehubbar och/eller skicka loggar till logganalys).</span><span class="sxs-lookup"><span data-stu-id="43029-116">Add parameters to the parameters blob for the storage account name, service bus rule ID, and/or OMS Log Analytics workspace ID (enabling archival of Diagnostic Logs in a storage account, streaming of logs to Event Hubs, and/or sending logs to Log Analytics).</span></span>
   
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
2. <span data-ttu-id="43029-117">Lägg till en resurs av typen i matrisen resurser på den resurs som du vill aktivera diagnostikloggar `[resource namespace]/providers/diagnosticSettings`.</span><span class="sxs-lookup"><span data-stu-id="43029-117">In the resources array of the resource for which you want to enable Diagnostic Logs, add a resource of type `[resource namespace]/providers/diagnosticSettings`.</span></span>
   
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

<span data-ttu-id="43029-118">Egenskaper för blob för Diagnostikinställningen följer [det format som beskrivs i den här artikeln](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="43029-118">The properties blob for the Diagnostic Setting follows [the format described in this article](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span> <span data-ttu-id="43029-119">Att lägga till den `metrics` egenskapen gör att du kan även skicka resurs mått till dessa samma utdata, under förutsättning att [resursen stöder Azure-Monitor mått](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="43029-119">Adding the `metrics` property will enable you to also send resource metrics to these same outputs, provided that [the resource supports Azure Monitor metrics](monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="43029-120">Här är ett fullständigt exempel som skapar en Logikapp och aktiverar direktuppspelning Händelsehubbar och lagring i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="43029-120">Here is a full example that creates a Logic App and turns on streaming to Event Hubs and storage in a storage account.</span></span>

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

## <a name="compute-resource-template"></a><span data-ttu-id="43029-121">Compute-resource-mall</span><span class="sxs-lookup"><span data-stu-id="43029-121">Compute resource template</span></span>
<span data-ttu-id="43029-122">Om du vill aktivera diagnostik för en beräkning resurs, till exempel en virtuell dator eller Service Fabric-kluster måste du:</span><span class="sxs-lookup"><span data-stu-id="43029-122">To enable diagnostics on a Compute resource, for example a Virtual Machine or Service Fabric cluster, you need to:</span></span>

1. <span data-ttu-id="43029-123">Lägga till filnamnstillägget Azure Diagnostics resursdefinitionen VM.</span><span class="sxs-lookup"><span data-stu-id="43029-123">Add the Azure Diagnostics extension to the VM resource definition.</span></span>
2. <span data-ttu-id="43029-124">Ange en storage-konto och/eller händelse hubb som en parameter.</span><span class="sxs-lookup"><span data-stu-id="43029-124">Specify a storage account and/or event hub as a parameter.</span></span>
3. <span data-ttu-id="43029-125">Lägg till innehållet i filen WADCfg XML i egenskapen XMLCfg undantagstecken alla XML-tecknen korrekt.</span><span class="sxs-lookup"><span data-stu-id="43029-125">Add the contents of your WADCfg XML file into the XMLCfg property, escaping all XML characters properly.</span></span>

> [!WARNING]
> <span data-ttu-id="43029-126">Det sista steget kan vara svårt att få rätt.</span><span class="sxs-lookup"><span data-stu-id="43029-126">This last step can be tricky to get right.</span></span> <span data-ttu-id="43029-127">[Finns den här artikeln](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) ett exempel som delar upp Konfigurationsschemat diagnostik i variabler som är undantagna och formaterats korrekt.</span><span class="sxs-lookup"><span data-stu-id="43029-127">[See this article](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) for an example that splits the Diagnostics Configuration Schema into variables that are escaped and formatted correctly.</span></span>
> 
> 

<span data-ttu-id="43029-128">Hela processen, inklusive exempel, beskrivs [i det här dokumentet](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="43029-128">The entire process, including samples, is described [in this document](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="43029-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43029-129">Next Steps</span></span>
* [<span data-ttu-id="43029-130">Läs mer om Azure diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="43029-130">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="43029-131">Strömma Azure diagnostikloggar i Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="43029-131">Stream Azure Diagnostic Logs to Event Hubs</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)

