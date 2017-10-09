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
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a><span data-ttu-id="47479-103">Aktivera automatiskt diagnostikinställningar när resursen skapas med hjälp av en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="47479-103">Automatically enable Diagnostic Settings at resource creation using a Resource Manager template</span></span>
<span data-ttu-id="47479-104">I den här artikeln visar vi hur du kan använda en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure diagnostikinställningar för en resurs när den skapas.</span><span class="sxs-lookup"><span data-stu-id="47479-104">In this article we show how you can use an [Azure Resource Manager template](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure Diagnostic Settings on a resource when it is created.</span></span> <span data-ttu-id="47479-105">Detta gör att du tooautomatically starta strömning diagnostikloggar och mått tooEvent NAV, arkivering dem i ett Lagringskonto eller skicka dem tooLog Analytics när en resurs har skapats.</span><span class="sxs-lookup"><span data-stu-id="47479-105">This enables you tooautomatically start streaming your Diagnostic Logs and metrics tooEvent Hubs, archiving them in a Storage Account, or sending them tooLog Analytics when a resource is created.</span></span>

<span data-ttu-id="47479-106">hello-metoden för att aktivera diagnostikloggar med hjälp av en Resource Manager-mall beror på hello resurstypen.</span><span class="sxs-lookup"><span data-stu-id="47479-106">hello method for enabling Diagnostic Logs using a Resource Manager template depends on hello resource type.</span></span>

* <span data-ttu-id="47479-107">**Icke-beräkning** resurser (till exempel Nätverkssäkerhetsgrupper Logic Apps Automation) [diagnostikinställningar som beskrivs i den här artikeln](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="47479-107">**Non-Compute** resources (for example, Network Security Groups, Logic Apps, Automation) use [Diagnostic Settings described in this article](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span>
* <span data-ttu-id="47479-108">**Beräkna** (BOMULLSTUSS/LAD-baserade) resurser använder hello [BOMULLSTUSS/LAD konfigurationsfilen som beskrivs i den här artikeln](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span><span class="sxs-lookup"><span data-stu-id="47479-108">**Compute** (WAD/LAD-based) resources use hello [WAD/LAD configuration file described in this article](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span></span>

<span data-ttu-id="47479-109">I den här artikeln beskriver vi hur tooconfigure diagnostik med någon av metoderna.</span><span class="sxs-lookup"><span data-stu-id="47479-109">In this article we describe how tooconfigure diagnostics using either method.</span></span>

<span data-ttu-id="47479-110">hello stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="47479-110">hello basic steps are as follows:</span></span>

1. <span data-ttu-id="47479-111">Skapa en mall som en JSON-fil som beskriver hur toocreate hello resurs och aktivera diagnostik.</span><span class="sxs-lookup"><span data-stu-id="47479-111">Create a template as a JSON file that describes how toocreate hello resource and enable diagnostics.</span></span>
2. <span data-ttu-id="47479-112">[Distribuera hello mallen med hjälp av en distributionsmetod](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="47479-112">[Deploy hello template using any deployment method](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

<span data-ttu-id="47479-113">Vi ger nedan ett exempel på hello mall JSON-fil som du behöver toogenerate för icke-beräknings- och beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="47479-113">Below we give an example of hello template JSON file you need toogenerate for non-Compute and Compute resources.</span></span>

## <a name="non-compute-resource-template"></a><span data-ttu-id="47479-114">Icke-beräkningsresurser mall</span><span class="sxs-lookup"><span data-stu-id="47479-114">Non-Compute resource template</span></span>
<span data-ttu-id="47479-115">För icke-beräkningsresurser behöver du toodo två saker:</span><span class="sxs-lookup"><span data-stu-id="47479-115">For non-Compute resources, you will need toodo two things:</span></span>

1. <span data-ttu-id="47479-116">Lägg till parametrar toohello parametrar blob för hello lagringskontonamn, service bus regel-ID och/eller OMS logganalys arbetsyte-ID (aktivera arkivering av diagnostikloggar i ett lagringskonto, strömning av loggar tooEvent NAV och/eller skicka loggar tooLog Analytics).</span><span class="sxs-lookup"><span data-stu-id="47479-116">Add parameters toohello parameters blob for hello storage account name, service bus rule ID, and/or OMS Log Analytics workspace ID (enabling archival of Diagnostic Logs in a storage account, streaming of logs tooEvent Hubs, and/or sending logs tooLog Analytics).</span></span>
   
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
2. <span data-ttu-id="47479-117">Hello resurser matris med hello resurs som du vill tooenable diagnostikloggar, lägga till en resurs av typen `[resource namespace]/providers/diagnosticSettings`.</span><span class="sxs-lookup"><span data-stu-id="47479-117">In hello resources array of hello resource for which you want tooenable Diagnostic Logs, add a resource of type `[resource namespace]/providers/diagnosticSettings`.</span></span>
   
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

<span data-ttu-id="47479-118">hello egenskaper blob för hello Diagnostikinställningen följer [hello-format som beskrivs i den här artikeln](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="47479-118">hello properties blob for hello Diagnostic Setting follows [hello format described in this article](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span> <span data-ttu-id="47479-119">Att lägga till hello `metrics` egenskapen Aktivera tooalso skicka resurs mått toothese samma matar ut, förutsatt att [hello resursen stöder Azure-Monitor mått](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="47479-119">Adding hello `metrics` property will enable you tooalso send resource metrics toothese same outputs, provided that [hello resource supports Azure Monitor metrics](monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="47479-120">Här är ett fullständigt exempel som skapar en Logikapp och aktiverar direktuppspelning tooEvent NAV och lagringsutrymme i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="47479-120">Here is a full example that creates a Logic App and turns on streaming tooEvent Hubs and storage in a storage account.</span></span>

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

## <a name="compute-resource-template"></a><span data-ttu-id="47479-121">Compute-resource-mall</span><span class="sxs-lookup"><span data-stu-id="47479-121">Compute resource template</span></span>
<span data-ttu-id="47479-122">tooenable diagnostik för en beräkning resurs, till exempel en virtuell dator eller Service Fabric-klustret måste du:</span><span class="sxs-lookup"><span data-stu-id="47479-122">tooenable diagnostics on a Compute resource, for example a Virtual Machine or Service Fabric cluster, you need to:</span></span>

1. <span data-ttu-id="47479-123">Lägg till hello Azure Diagnostics tillägget toohello VM resursdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="47479-123">Add hello Azure Diagnostics extension toohello VM resource definition.</span></span>
2. <span data-ttu-id="47479-124">Ange en storage-konto och/eller händelse hubb som en parameter.</span><span class="sxs-lookup"><span data-stu-id="47479-124">Specify a storage account and/or event hub as a parameter.</span></span>
3. <span data-ttu-id="47479-125">Lägg till hello innehållet i WADCfg XML-filen i hello XMLCfg egenskapen undantagstecken alla XML-tecknen korrekt.</span><span class="sxs-lookup"><span data-stu-id="47479-125">Add hello contents of your WADCfg XML file into hello XMLCfg property, escaping all XML characters properly.</span></span>

> [!WARNING]
> <span data-ttu-id="47479-126">Det sista steget kan vara svårt tooget höger.</span><span class="sxs-lookup"><span data-stu-id="47479-126">This last step can be tricky tooget right.</span></span> <span data-ttu-id="47479-127">[Finns den här artikeln](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) ett exempel som att delningar hello diagnostik Konfigurationsschemat till variabler som är undantagna och formaterats korrekt.</span><span class="sxs-lookup"><span data-stu-id="47479-127">[See this article](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) for an example that splits hello Diagnostics Configuration Schema into variables that are escaped and formatted correctly.</span></span>
> 
> 

<span data-ttu-id="47479-128">hello hela processen, inklusive exempel, beskrivs [i det här dokumentet](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="47479-128">hello entire process, including samples, is described [in this document](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="47479-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47479-129">Next Steps</span></span>
* [<span data-ttu-id="47479-130">Läs mer om Azure diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="47479-130">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="47479-131">Dataströmmen Azure diagnostikloggar tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="47479-131">Stream Azure Diagnostic Logs tooEvent Hubs</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)

