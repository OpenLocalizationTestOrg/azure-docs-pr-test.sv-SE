---
title: aaaUse Azure Resource Manager-mallar tooCreate och konfigurera en Log Analytics-arbetsyta | Microsoft Docs
description: "Du kan använda Azure Resource Manager mallar toocreate och konfigurera logganalys arbetsytor."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: d21ca1b0-847d-4716-bb30-2a8c02a606aa
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: json
ms.topic: article
ms.date: 06/01/2017
ms.author: richrund
ms.openlocfilehash: c8f413e982f5eeed73f463524ff6f239f26c9127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-log-analytics-using-azure-resource-manager-templates"></a>Hantera Log Analytics med hjälp av Azure Resource Manager-mallar
Du kan använda [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) toocreate och konfigurera logganalys arbetsytor. Exempel på hello uppgifter du kan utföra med mallar:

* Skapa en arbetsyta
* Lägg till en lösning
* Skapa sparade sökningar
* Skapa en datorgrupp
* Aktivera insamling av IIS-loggar från datorer med Windows hello-agenten installerad
* Samla in prestandaräknare från Linux och Windows-datorer
* Samla in händelser från syslog på Linux-datorer 
* Samla in händelser från händelseloggarna i Windows
* Samla in anpassade händelseloggar
* Lägg till hello log analytics agent tooan virtuella Azure-datorn
* Konfigurera log analytics tooindex data som samlas in med hjälp av Azure-diagnostik

Den här artikeln innehåller en mall-exempel som visar några av hello-konfiguration som du kan utföra från mallar.

## <a name="create-and-configure-a-log-analytics-workspace"></a>Skapa och konfigurera en Log Analytics-arbetsyta
hello följande mall exempel visas hur du:

1. Skapa en arbetsyta, inklusive inställningen datalagring
2. Lägg till lösningar toohello arbetsytan
3. Skapa sparade sökningar
4. Skapa en datorgrupp
5. Aktivera insamling av IIS-loggar från datorer med Windows hello-agenten installerad
6. Samla in logisk Disk prestandaräknarna från Linux-datorer (% noder i procent; Ledigt utrymme i MB; Använt utrymme; i % Disköverföringar/sek; Diskläsningar/sek; Diskskrivningar/sek)
7. Samla in syslog-händelser från Linux-datorer
8. Samla in händelser för fel- och varningsmeddelandena från hello programhändelseloggen från Windows-datorer
9. Tillgängligt minne i megabyte prestandaräknaren samla in från Windows-datorer
10. Samla in en anpassad logg 
11. Samla in IIS-loggar och Windows-händelseloggar som skrivits av Azure-diagnostik tooa storage-konto

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "workspaceName"
      }
    },
    "serviceTier": {
      "type": "string",
      "allowedValues": [
        "Free",
        "Standalone",
        "PerNode"
      ],
      "metadata": {
        "description": "Service Tier: Free, Standalone, or PerNode"
    }
      },
    "dataRetention": {
      "type": "int",
      "defaultValue": 30,
      "minValue": 7,
      "maxValue": 730,
      "metadata": {
        "description": "Number of days of retention. Free plans can only have 7 days, Standalone and OMS plans include 30 days for free"
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "East US",
        "West Europe",
        "Southeast Asia",
        "Australia Southeast"
      ]
    },
    "applicationDiagnosticsStorageAccountName": {
        "type": "string",
        "metadata": {
          "description": "Name of hello storage account with Azure diagnostics output"
        }
    },
    "applicationDiagnosticsStorageAccountResourceGroup": {
        "type": "string",
        "metadata": {
          "description": "hello resource group name containing hello storage account with Azure diagnostics output"
        }
    }
  },
  "variables": {
    "Updates": {
      "Name": "[Concat('Updates', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "Updates"
    },
    "AntiMalware": {
      "Name": "[concat('AntiMalware', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "AntiMalware"
    },
    "SQLAssessment": {
      "Name": "[Concat('SQLAssessment', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "SQLAssessment"
    },
    "diagnosticsStorageAccount": "[resourceId(parameters('applicationDiagnosticsStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-11-01-preview",
      "type": "Microsoft.OperationalInsights/workspaces",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "Name": "[parameters('serviceTier')]"
        },
    "retention": "[parameters('dataRetention')]"
      },
      "resources": [
        {
          "apiVersion": "2015-11-01-preview",
          "name": "VMSS Queries2",
          "type": "savedSearches",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "Category": "VMSS",
            "ETag": "*",
            "DisplayName": "VMSS Instance Count",
            "Query": "Type:Event Source=ServiceFabricNodeBootstrapAgent | dedup Computer | measure count () by Computer",
            "Version": 1
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsEvent1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsEvent",
          "properties": {
            "eventLogName": "Application",
            "eventTypes": [
              {
                "eventType": "Error"
              },
              {
                "eventType": "Warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsPerfCounter1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsPerformanceCounter",
          "properties": {
            "objectName": "Memory",
            "instanceName": "*",
            "intervalSeconds": 10,
            "counterName": "Available MBytes"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleIISLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "IISLogs",
          "properties": {
            "state": "OnPremiseEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslog",
          "properties": {
            "syslogName": "kern",
            "syslogSeverities": [
              {
                "severity": "emerg"
              },
              {
                "severity": "alert"
              },
              {
                "severity": "crit"
              },
              {
                "severity": "err"
              },
              {
                "severity": "warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslogCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerf1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceObject",
          "properties": {
            "performanceCounters": [
              {
                "counterName": "% Used Inodes"
              },
              {
                "counterName": "Free Megabytes"
              },
              {
                "counterName": "% Used Space"
              },
              {
                "counterName": "Disk Transfers/sec"
              },
              {
                "counterName": "Disk Reads/sec"
              },
              {
                "counterName": "Disk Writes/sec"
              }
            ],
            "objectName": "Logical Disk",
            "instanceName": "*",
            "intervalSeconds": 10
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerfCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleCustomLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "CustomLog",
          "properties": {
            "customLogName": "sampleCustomLog1",
            "description": "test custom log datasources",
            "inputs": [
              {
                "location": {
                  "fileSystemLocations": {
                    "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ],
                    "linuxFileTypeLogPaths": [ "/var/logs" ]
                  }
                },
                "recordDelimiter": {
                  "regexDelimiter": {
                    "pattern": "\\n",
                    "matchIndex": 0,
                    "matchIndexSpecified": true,
                    "numberedGroup": null
                  }
                }
              }
            ],
            "extractions": [
              {
                "extractionName": "TimeGenerated",
                "extractionType": "DateTime",
                "extractionProperties": {
                  "dateTimeExtraction": {
                    "regex": null,
                    "joinStringRegex": null
                  }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleCustomLogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "CustomLogCollection",
          "properties": {
            "state": "LinuxLogsEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('workspaceName'))]",
          "type": "storageinsightconfigs",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "containers": [ 
              "wad-iis-logfiles" 
            ],
            "tables": [
              "WADWindowsEventLogsTable"
            ],
            "storageAccount": {
              "id": "[variables('diagnosticsStorageAccount')]",
              "key": "[listKeys(variables('diagnosticsStorageAccount'),'2015-06-15').key1]"
            }
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('Updates').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('Updates').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('Updates').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('AntiMalware').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('AntiMalware').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('AntiMalware').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('AntiMalware').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('SQLAssessment').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('SQLAssessment').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('SQLAssessment').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('SQLAssessment').GalleryName)]",
            "promotionCode": ""
          }
        }
      ]
    }
  ],
  "outputs": {
    "workspaceOutput": {
      "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-11-01-preview')]",
      "type": "object"
    }
  }
}

```
### <a name="deploying-hello-sample-template"></a>Distribuera hello exempelmall
toodeploy hello exempelmall:

1. Spara bifogade hello-exempel i en fil, till exempel`azuredeploy.json` 
2. Redigera hello toohave hello mallkonfigurationen du vill
3. Använd PowerShell eller hello kommandoraden toodeploy hello mall

#### <a name="powershell"></a>PowerShell
`New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json`

#### <a name="command-line"></a>Kommandorad
```
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```


## <a name="example-resource-manager-templates"></a>Exempel Resource Manager-mallar
hello Azure quickstart mallgalleriet innehåller flera mallar för Log Analytics, inklusive:

* [Distribuera en virtuell dator som kör Windows med hello Log Analytics VM-tillägget](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
* [Distribuera en virtuell dator som kör Linux med hello Log Analytics VM-tillägget](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
* [Övervaka Azure Site Recovery med en befintlig logganalys-arbetsyta](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
* [Övervaka Azure Web Apps med hjälp av en befintlig logganalys-arbetsyta](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
* [Övervaka SQL Azure med hjälp av en befintlig logganalys-arbetsyta](https://azure.microsoft.com/documentation/templates/101-sqlazure-oms-monitoring/)
* [Distribuera ett Service Fabric-kluster och övervaka med en befintlig logganalys-arbetsyta](https://azure.microsoft.com/documentation/templates/service-fabric-oms/)
* [Distribuera ett Service Fabric-kluster och skapa en logganalys-arbetsytan toomonitor den](https://azure.microsoft.com/documentation/templates/service-fabric-vmss-oms/)

## <a name="next-steps"></a>Nästa steg
* [Distribuera agenter till virtuella Azure-datorer med hjälp av Resource Manager-mallar](log-analytics-azure-vm-extension.md)

