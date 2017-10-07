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
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Autoskala med gäst mått i en Linux-skala ange mall

Det finns två typer av mått i Azure som har samlats in från virtuella datorer och skala uppsättningar: vissa komma från hello värd åt virtuella datorn och andra kommer från hello gäst-VM. Värden mått inte kräver ytterligare inställningar eftersom de samlas in av hello värd virtuell dator gäst mått kräver oss tooinstall hello [Windows Azure-diagnostik tillägget](../virtual-machines/windows/extensions-diagnostics-template.md) eller hello [Linux Azure-diagnostik tillägget](../virtual-machines/linux/diagnostic-extension.md) i hello gäst VM. En gemensam orsak toouse gäst mått i stället för värden mått är att gäst mått ger ett större antal mått än värden mått. Ett exempel är minnesförbrukning mätvärden som är bara tillgängliga via gäst mått. hello stöds värden mått visas [här](../monitoring-and-diagnostics/monitoring-supported-metrics.md), och används ofta gäst mått visas [här](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Den här artikeln visar hur toomodify hello [lägsta lönsam skala ange mall](./virtual-machine-scale-sets-mvss-start.md) toouse Autoskala regler baserat på gästen mätvärden för skalningsuppsättningar i Linux.

## <a name="change-hello-template-definition"></a>Ändra hello malldefinitionen

Mall för våra lägsta lönsam skala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), och våra mallen för distribution av hello Linux skala med gästbaserat Autoskala kan ses [här](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). Låt oss nu undersöka hello diff används toocreate mallen (`git diff minimum-viable-scale-set existing-vnet`) bit för bit:

Först måste vi lägga till parametrar för `storageAccountName` och `storageAccountSasToken`. hello diagnostik agent lagrar mått data i en [tabellen](../cosmos-db/table-storage-how-to-use-dotnet.md) i det här lagringskontot. Från och med hello Linuxagenten Diagnostics version 3.0 stöds med hjälp av en lagringsåtkomstnyckel inte längre. Vi måste använda en [SAS-Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

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

Nu ska vi ändra hello skaluppsättning `extensionProfile` tooinclude hello diagnostik tillägg. I den här konfigurationen kan vi ange hello resurs-ID för hello skala in toocollect mått från, samt hello storage-konto och SAS-token toouse toostore hello mått. Vi kan ange hur ofta hello mått aggregeras (i det här fallet varje minut) och vilka mått tootrack (i det här fallet Procent använt minne). Mer detaljerad information om den här konfigurationen och mått än Procent använt minne finns [denna dokumentation](../virtual-machines/linux/diagnostic-extension.md).

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

Slutligen lägger vi till en `autoscaleSettings` resurs tooconfigure Autoskala baserat på de här måtten. Den här resursen har en `dependsOn` -sats som refererar till hello skala ange tooensure hello skaluppsättning finns innan du försöker tooautoscale den. Om vi väljer ett annat mått tooautoscale på vi använder hello `counterSpecifier` från hello diagnostik tilläggets konfiguration som hello `metricName` i hello Autoskala konfiguration. Finns mer information om konfiguration av Autoskala hello [Autoskala metodtips](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) och hello [Azure övervakaren REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/dn931928.aspx).

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





## <a name="next-steps"></a>Nästa steg

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
