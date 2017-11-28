---
title: "Använda Azure Autoskala med gäst mått i en mall för Linux skala | Microsoft Docs"
description: "Lär dig hur tooautoscale med gäst mått i en Skaluppsättning för Linux virtuella dator mall"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: negat
ms.openlocfilehash: 7afbef943a5f15c7a72dcf7114f46d521c504424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a><span data-ttu-id="58784-103">Autoskala med gäst mått i en Linux-skala ange mall</span><span class="sxs-lookup"><span data-stu-id="58784-103">Autoscale using guest metrics in a Linux scale set template</span></span>

<span data-ttu-id="58784-104">Det finns två typer av mått i Azure som har samlats in från virtuella datorer och skala uppsättningar: vissa komma från hello värd åt virtuella datorn och andra kommer från hello gäst-VM.</span><span class="sxs-lookup"><span data-stu-id="58784-104">There are two types of metrics in Azure that are gathered from VMs and scale sets: some come from hello host VM and others come from hello guest VM.</span></span> <span data-ttu-id="58784-105">Värden mått inte kräver ytterligare inställningar eftersom de samlas in av hello värd virtuell dator gäst mått kräver oss tooinstall hello [Windows Azure-diagnostik tillägget](../virtual-machines/windows/extensions-diagnostics-template.md) eller hello [Linux Azure-diagnostik tillägget](../virtual-machines/linux/diagnostic-extension.md) i hello gäst VM.</span><span class="sxs-lookup"><span data-stu-id="58784-105">Host metrics do not require additional setup because they are collected by hello host VM, whereas guest metrics require us tooinstall hello [Windows Azure Diagnostics extension](../virtual-machines/windows/extensions-diagnostics-template.md) or hello [Linux Azure Diagnostics extension](../virtual-machines/linux/diagnostic-extension.md) in hello guest VM.</span></span> <span data-ttu-id="58784-106">En gemensam orsak toouse gäst mått i stället för värden mått är att gäst mått ger ett större antal mått än värden mått.</span><span class="sxs-lookup"><span data-stu-id="58784-106">One common reason toouse guest metrics instead of host metrics is that guest metrics provide a larger selection of metrics than host metrics.</span></span> <span data-ttu-id="58784-107">Ett exempel är minnesförbrukning mätvärden som är bara tillgängliga via gäst mått.</span><span class="sxs-lookup"><span data-stu-id="58784-107">One such example is memory-consumption metrics, which are only available via guest metrics.</span></span> <span data-ttu-id="58784-108">hello stöds värden mått visas [här](../monitoring-and-diagnostics/monitoring-supported-metrics.md), och används ofta gäst mått visas [här](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="58784-108">hello supported host metrics are listed [here](../monitoring-and-diagnostics/monitoring-supported-metrics.md), and commonly used guest metrics are listed [here](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span> <span data-ttu-id="58784-109">Den här artikeln visar hur toomodify hello [lägsta lönsam skala ange mall](./virtual-machine-scale-sets-mvss-start.md) toouse Autoskala regler baserat på gästen mätvärden för skalningsuppsättningar i Linux.</span><span class="sxs-lookup"><span data-stu-id="58784-109">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toouse autoscale rules based on guest metrics for Linux scale sets.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="58784-110">Ändra hello malldefinitionen</span><span class="sxs-lookup"><span data-stu-id="58784-110">Change hello template definition</span></span>

<span data-ttu-id="58784-111">Mall för våra lägsta lönsam skala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), och våra mallen för distribution av hello Linux skala med gästbaserat Autoskala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="58784-111">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello Linux scale set with guest-based autoscale can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json).</span></span> <span data-ttu-id="58784-112">Låt oss nu undersöka hello diff används toocreate mallen (`git diff minimum-viable-scale-set existing-vnet`) bit för bit:</span><span class="sxs-lookup"><span data-stu-id="58784-112">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="58784-113">Först måste vi lägga till parametrar för `storageAccountName` och `storageAccountSasToken`.</span><span class="sxs-lookup"><span data-stu-id="58784-113">First, we add parameters for `storageAccountName` and `storageAccountSasToken`.</span></span> <span data-ttu-id="58784-114">hello diagnostik agent lagrar mått data i en [tabellen](../cosmos-db/table-storage-how-to-use-dotnet.md) i det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="58784-114">hello diagnostics agent will store metric data in a [table](../cosmos-db/table-storage-how-to-use-dotnet.md) in this storage account.</span></span> <span data-ttu-id="58784-115">Från och med hello Linuxagenten Diagnostics version 3.0 stöds med hjälp av en lagringsåtkomstnyckel inte längre.</span><span class="sxs-lookup"><span data-stu-id="58784-115">As of hello Linux Diagnostics Agent version 3.0, using a storage access key is no longer supported.</span></span> <span data-ttu-id="58784-116">Vi måste använda en [SAS-Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="58784-116">We must use a [SAS Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "storageAccountName": {
+      "type": "string"
+    },
+    "storageAccountSasToken": {
+      "type": "securestring"
     }
   },
```

<span data-ttu-id="58784-117">Nu ska vi ändra hello skaluppsättning `extensionProfile` tooinclude hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="58784-117">Next, we modify hello scale set `extensionProfile` tooinclude hello diagnostics extension.</span></span> <span data-ttu-id="58784-118">I den här konfigurationen kan vi ange hello resurs-ID för hello skala in toocollect mått från, samt hello storage-konto och SAS-token toouse toostore hello mått.</span><span class="sxs-lookup"><span data-stu-id="58784-118">In this configuration, we specify hello resource ID of hello scale set toocollect metrics from, as well as hello storage account and SAS token toouse toostore hello metrics.</span></span> <span data-ttu-id="58784-119">Vi kan ange hur ofta hello mått aggregeras (i det här fallet varje minut) och vilka mått tootrack (i det här fallet Procent använt minne).</span><span class="sxs-lookup"><span data-stu-id="58784-119">We also specify how frequently hello metrics are aggregated (in this case every minute) and which metrics tootrack (in this case percent used memory).</span></span> <span data-ttu-id="58784-120">Mer detaljerad information om den här konfigurationen och mått än Procent använt minne finns [denna dokumentation](../virtual-machines/linux/diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="58784-120">For more detailed information on this configuration and metrics other than percent used memory, see [this documentation](../virtual-machines/linux/diagnostic-extension.md).</span></span>

```diff
                 }
               }
             ]
+          },
+          "extensionProfile": {
+            "extensions": [
+              {
+                "name": "LinuxDiagnosticExtension",
+                "properties": {
+                  "publisher": "Microsoft.Azure.Diagnostics",
+                  "type": "LinuxDiagnostic",
+                  "typeHandlerVersion": "3.0",
+                  "settings": {
+                    "StorageAccount": "[parameters('storageAccountName')]",
+                    "ladCfg": {
+                      "diagnosticMonitorConfiguration": {
+                        "performanceCounters": {
+                          "sinks": "WADMetricJsonBlob",
+                          "performanceCounterConfiguration": [
+                            {
+                              "unit": "percent",
+                              "type": "builtin",
+                              "class": "memory",
+                              "counter": "percentUsedMemory",
+                              "counterSpecifier": "/builtin/memory/percentUsedMemory",
+                              "condition": "IsAggregate=TRUE"
+                            }
+                          ]
+                        },
+                        "metrics": {
+                          "metricAggregation": [
+                            {
+                              "scheduledTransferPeriod": "PT1M"
+                            }
+                          ],
+                          "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]"
+                        }
+                      }
+                    }
+                  },
+                  "protectedSettings": {
+                    "storageAccountName": "[parameters('storageAccountName')]",
+                    "storageAccountSasToken": "[parameters('storageAccountSasToken')]",
+                    "sinksConfig": {
+                      "sink": [
+                        {
+                          "name": "WADMetricJsonBlob",
+                          "type": "JsonBlob"
+                        }
+                      ]
+                    }
+                  }
+                }
+              }
+            ]
           }
         }
       }
```

<span data-ttu-id="58784-121">Slutligen lägger vi till en `autoscaleSettings` resurs tooconfigure Autoskala baserat på de här måtten.</span><span class="sxs-lookup"><span data-stu-id="58784-121">Finally, we add an `autoscaleSettings` resource tooconfigure autoscale based on these metrics.</span></span> <span data-ttu-id="58784-122">Den här resursen har en `dependsOn` -sats som refererar till hello skala ange tooensure hello skaluppsättning finns innan du försöker tooautoscale den.</span><span class="sxs-lookup"><span data-stu-id="58784-122">This resource has a `dependsOn` clause that references hello scale set tooensure that hello scale set exists before attempting tooautoscale it.</span></span> <span data-ttu-id="58784-123">Om vi väljer ett annat mått tooautoscale på vi använder hello `counterSpecifier` från hello diagnostik tilläggets konfiguration som hello `metricName` i hello Autoskala konfiguration.</span><span class="sxs-lookup"><span data-stu-id="58784-123">If we choose a different metric tooautoscale on, we would use hello `counterSpecifier` from hello diagnostics extension configuration as hello `metricName` in hello autoscale configuration.</span></span> <span data-ttu-id="58784-124">Finns mer information om konfiguration av Autoskala hello [Autoskala metodtips](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) och hello [Azure övervakaren REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/dn931928.aspx).</span><span class="sxs-lookup"><span data-stu-id="58784-124">For more information on autoscale configuration, see hello [autoscale best practices](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) and hello [Azure Monitor REST API reference documentation](https://msdn.microsoft.com/library/azure/dn931928.aspx).</span></span>

```diff
+    },
+    {
+      "type": "Microsoft.Insights/autoscaleSettings",
+      "apiVersion": "2015-04-01",
+      "name": "guestMetricsAutoscale",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
+      ],
+      "properties": {
+        "name": "guestMetricsAutoscale",
+        "targetResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+        "enabled": true,
+        "profiles": [
+          {
+            "name": "Profile1",
+            "capacity": {
+              "minimum": "1",
+              "maximum": "10",
+              "default": "3"
+            },
+            "rules": [
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "GreaterThan",
+                  "threshold": 60
+                },
+                "scaleAction": {
+                  "direction": "Increase",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              },
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "LessThan",
+                  "threshold": 30
+                },
+                "scaleAction": {
+                  "direction": "Decrease",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              }
+            ]
+          }
+        ]
+      }
     }
   ]
 }
```





## <a name="next-steps"></a><span data-ttu-id="58784-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58784-125">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
