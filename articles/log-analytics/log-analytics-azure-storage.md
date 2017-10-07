---
title: "aaaCollect Azure service loggar och mått för Log Analytics | Microsoft Docs"
description: "Konfigurera diagnostik på Azure-resurser toowrite loggar och mått tooLog Analytics."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1cede9a94ec83c4e3a95853dc2ec355d8df06d6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-azure-service-logs-and-metrics-for-use-in-log-analytics"></a>Samla in Azure-tjänstloggar och mått för användning i logganalys

Det finns fyra olika sätt för att samla in loggar och mått för Azure-tjänster:

1. Azure diagnostics direkt tooLog Analytics (*diagnostik* i följande tabell hello)
2. Azure diagnostics tooAzure lagring tooLog Analytics (*lagring* i följande tabell hello)
3. Kopplingar för Azure-tjänster (*kopplingar* i följande tabell hello)
4. Skript toocollect och skicka data till logganalys (tomma celler i den följande tabellen hello och för tjänster som inte är listade)


| Tjänst                 | Resurstyp                           | Logs        | Mått     | Lösning |
| --- | --- | --- | --- | --- |
| Programgateways    | Microsoft.Network/applicationGateways   | Diagnostik | Diagnostik | [Azure Application Gateway Analytics](log-analytics-azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-log-analytics) |
| Programinsikter    |                                         | koppling   | koppling   | [Application Insights Connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (förhandsgranskning) |
| Automation-konton     | Microsoft.Automation/AutomationAccounts | Diagnostik |             | [Mer information](../automation/automation-manage-send-joblogs-log-analytics.md)|
| Batch-konton          | Microsoft.Batch/batchAccounts           | Diagnostik | Diagnostik | |
| Klassiska molntjänster  |                                         | Lagring     |             | [Mer information](log-analytics-azure-storage-iis-table.md) |
| Cognitive Services      | Microsoft.CognitiveServices/accounts    |             | Diagnostik | |
| Data Lake analytics     | Microsoft.DataLakeAnalytics/accounts    | Diagnostik |             | |
| Data Lake store         | Microsoft.DataLakeStore/accounts        | Diagnostik |             | |
| Event Hub namnområde     | Microsoft.EventHub/namespaces           | Diagnostik | Diagnostik | |
| IoT-hubbar                | Microsoft.Devices/IotHubs               |             | Diagnostik | |
| Key Vault               | Microsoft.KeyVault/vaults               | Diagnostik |             | [KeyVault Analytics](log-analytics-azure-key-vault.md) |
| Belastningsutjämnare          | Microsoft.Network/loadBalancers         | Diagnostik |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Diagnostik | Diagnostik | |
| Nätverkssäkerhetsgrupper | Microsoft.Network/networksecuritygroups | Diagnostik |             | [Azure Nätverkssäkerhetsgruppen Analytics](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) |
| Recovery-valv         | Microsoft.RecoveryServices/vaults       |             |             | [Azure Recovery Services Analytics (förhandsgranskning)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Söktjänster         | Microsoft.Search/searchServices         | Diagnostik | Diagnostik | |
| Service Bus-namnrymd   | Microsoft.ServiceBus/namespaces         | Diagnostik | Diagnostik | [Service Bus Analytics (förhandsgranskning)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Lagring     |             | [Service Fabric Analytics (förhandsgranskning)](log-analytics-service-fabric.md) |
| SQL (v12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Diagnostik | [Azure SQL Analytics (förhandsgranskning)](log-analytics-azure-sql.md) |
| Lagring                 |                                         |             | Skript      | [Azure Storage Analytics (förhandsgranskning)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Virtuella datorer        | Microsoft.Compute/virtualMachines       | Anknytning   | Anknytning <br> Diagnostik  | |
| Skalningsuppsättningar i virtuella datorer | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Diagnostik | |
| Webbservergrupper        | Microsoft.Web/serverfarms               |             | Diagnostik | |
| Webbplatser               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Diagnostik | [Azure Web Apps Analytics (förhandsgranskning)](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) |


> [!NOTE]
> För att övervaka virtuella Azure-datorer (både Linux och Windows), vi rekommenderar att du installerar hello [Log Analytics VM-tillägget](log-analytics-azure-vm-extension.md). hello agent ger information som samlas in från virtuella datorer. Du kan också använda hello-tillägg för virtuella datorer.
>
>

## <a name="azure-diagnostics-direct-toolog-analytics"></a>Azure diagnostics direkt tooLog Analytics
Många Azure-resurser som kan toowrite diagnostikloggar och mått direkt tooLog Analytics och detta går hello rekommenderas för att samla in hello data för analys. När du använder Azure-diagnostik, skrivs data omedelbart tooLog Analytics och det finns inga behov toofirst skrivåtgärder hello data toostorage.

Azure-resurser som stöder [Azure övervakaren](../monitoring-and-diagnostics/monitoring-overview.md) kan skicka sin loggar och mått direkt tooLog Analytics.

* Hello information av hello tillgängliga mått finns för[stöds mått med Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).
* Hello information hello tillgängliga loggar finns för[stöds tjänster och schemat för diagnostikloggar](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>Aktivera diagnostik med PowerShell
Du behöver hello November 2016 (v2.3.0) eller senare versionen av [Azure PowerShell](/powershell/azure/overview).

Hej följande PowerShell-exempel visar hur toouse [Set AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) tooenable diagnostik på en nätverkssäkerhetsgrupp. hello samma metod fungerar för alla resurser som stöds – ange `$resourceId` toohello resurs-id för hello-resurs som du vill tooenable diagnostik för.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzureRmDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Aktivera diagnostik med Resource Manager-mallar

tooenable diagnostik för en resurs när den har skapats och har skickat hello diagnostik tooyour logganalys-arbetsytan kan du använda en mall liknande toohello en nedan. Det här exemplet är för ett Automation-konto men fungerar för alla resurstyper som stöds.

```json
        {
            "type": "Microsoft.Automation/automationAccounts/providers/diagnosticSettings",
            "name": "[concat(parameters('omsAutomationAccountName'), '/', 'Microsoft.Insights/service')]",
            "apiVersion": "2015-07-01",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('omsAutomationAccountName'))]",
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspaceName'))]"
            ],
            "properties": {
                "workspaceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('omsWorkspaceName'))]",
                "logs": [
                    {
                        "category": "JobLogs",
                        "enabled": true
                    },
                    {
                        "category": "JobStreams",
                        "enabled": true
                    }
                ]
            }
        }
```

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-toostorage-then-toolog-analytics"></a>Azure diagnostics toostorage sedan tooLog Analytics

För att samla in loggar från inom vissa resurser, är möjliga toosend hello loggar tooAzure lagring och sedan konfigurera logganalys tooread hello loggar från lagringsplatsen.

Log Analytics kan använda den här metoden toocollect diagnostik från Azure storage för hello efter resurser och loggar:

| Resurs | Logs |
| --- | --- |
| Service Fabric |ETWEvent <br> Arbetsloggen <br> Tillförlitliga aktören händelse <br> Tillförlitlig tjänst-händelse |
| Virtuella datorer |Linux Syslog <br> Windows-händelse <br> IIS-logg <br> Windows ETWEvent |
| Webbroller <br> Worker-roller |Linux Syslog <br> Windows-händelse <br> IIS-logg <br> Windows ETWEvent |

> [!NOTE]
> Du debiteras normala Azure datatrafikavgifter för lagring och transaktioner när du skickar diagnostik tooa storage-konto och när logganalys läser hello data från ditt lagringskonto.
>
>

Se [använda blob storage för IIS- och lagring för händelser](log-analytics-azure-storage-iis-table.md) toolearn mer om hur logganalys samlar in dessa loggar.

## <a name="connectors-for-azure-services"></a>Kopplingar för Azure-tjänster

Det finns en koppling för Application Insights, vilket gör att data som samlas in av Application Insights toobe skickas tooLog Analytics.

Mer information om hello [Application Insights connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-toocollect-and-post-data-toolog-analytics"></a>Skript toocollect och skicka data tooLog Analytics

För Azure-tjänster som inte tillhandahåller ett enkelt sätt toosend loggar och mått tooLog hello Analytics som du kan använda en Azure Automation-skriptet toocollect loggen och mått. hello skript kan sedan skicka hello data tooLog Analytics med hjälp av hello [datainsamlaren API](log-analytics-data-collector-api.md)

hello Azure mallgalleriet har [exempel på användning av Azure Automation](https://azure.microsoft.com/en-us/resources/templates/?term=OMS) toocollect data från tjänster och skicka det tooLog Analytics.

## <a name="next-steps"></a>Nästa steg

* [Använda blob storage för IIS- och lagring för händelser](log-analytics-azure-storage-iis-table.md) tooread hello loggar för Azure-tjänster som skriver diagnostik tootable lagrings- eller IIS loggar du skriftlig tooblob lagring.
* [Aktivera lösningar](log-analytics-add-solutions.md) tooprovide inblick i hello data.
* [Använd sökfrågor](log-analytics-log-searches.md) tooanalyze hello data.
