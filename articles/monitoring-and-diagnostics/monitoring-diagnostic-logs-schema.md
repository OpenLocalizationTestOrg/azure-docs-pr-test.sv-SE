---
title: "aaaAzure diagnostiska loggar stöds tjänster och scheman | Microsoft Docs"
description: "Förstå tjänster och Händelseschema hello stöds för Azure diagnostikloggar."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: a3cbf5267e1bd0dc257f4fb4f42c323644046a6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Tjänster som stöds, scheman och kategorier för diagnostikloggar i Azure

[Azure-resurs diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) är loggar från Azure-resurser som beskriver hello driften av den här resursen. Dessa loggar är resurstypen specifika. I den här artikeln beskriver vi hello uppsättning stöds tjänster och händelse-schemat för händelser som sänds av varje tjänst. Den här artikeln innehåller också en fullständig lista över tillgängliga kategorier per resurstypen.

## <a name="supported-services-and-schemas-for-resource-diagnostic-logs"></a>Tjänster och scheman som stöds för resursen diagnostikloggar
hello-schemat för resursen diagnostikloggar varierar beroende på hello resurs och logg kategori.   

| Tjänst | Schemat & Docs |
| --- | --- |
| API Management | Schemat är inte tillgänglig. |
| Application Gateways |[Diagnostikloggning för Programgateway](../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Logganalys för Azure Automation](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch-diagnostikloggning](../batch/batch-diagnostics.md) |
| Kunden insikter | Schemat är inte tillgänglig. |
| Content Delivery Network | Schemat är inte tillgänglig. |
| CosmosDB | Schemat är inte tillgänglig. |
| Data Lake Analytics |[Åtkomst till diagnostikloggar för Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Åtkomst till diagnostikloggarna för Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Händelsehubbar |[Azure Event Hubs diagnostiska loggar](../event-hubs/event-hubs-diagnostic-logs.md) |
| Key Vault |[Azure Key Vault-loggning](../key-vault/key-vault-logging.md) |
| Belastningsutjämnare |[Logganalys för Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Anpassat Logic Apps B2B-spårningsschema](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Nätverkssäkerhetsgrupper |[Log Analytics för nätverkssäkerhetsgrupper (NSG)](../virtual-network/virtual-network-nsg-manage-log.md) |
| Recovery Services | Schemat är inte tillgänglig.|
| Söka |[Att aktivera och använda Sök trafik Analytics](../search/search-traffic-analytics.md) |
| Serverhantering | Schemat är inte tillgänglig. |
| Service Bus |[Azure Service Bus-diagnostikloggar](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| Stream Analytics |[Jobbet diagnostikloggar](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |

## <a name="supported-log-categories-per-resource-type"></a>Stöd för kategorier i loggen resurstyp
|Resurstyp|Kategori|Kategori visningsnamn|
|---|---|---|
|Microsoft.ApiManagement/service|GatewayLogs|Loggar relaterade tooApiManagement Gateway|
|Microsoft.Automation/automationAccounts|JobLogs|Jobbloggar|
|Microsoft.Automation/automationAccounts|JobStreams|Dataströmmar för jobbet|
|Microsoft.Automation/automationAccounts|DscNodeStatus|DSC-noden Status|
|Microsoft.Batch/batchAccounts|ServiceLog|Tjänsten loggar|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Hämtar hello mätvärden för hello slutpunkt, t.ex. bandbredd, utgång osv.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataLakeAnalytics/accounts|Granska|Granskningsloggar|
|Microsoft.DataLakeAnalytics/accounts|Begäranden|Begäran loggar|
|Microsoft.DataLakeStore/accounts|Granska|Granskningsloggar|
|Microsoft.DataLakeStore/accounts|Begäranden|Begäran loggar|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.EventHub/namespaces|ArchiveLogs|Arkivera loggar|
|Microsoft.EventHub/namespaces|OperationalLogs|Operativa loggar|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Automatisk skalning loggar|
|Microsoft.KeyVault/vaults|AuditEvent|Granskningsloggar|
|Microsoft.Logic/workflows|WorkflowRuntime|Workflow runtime diagnostiska händelser|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Integration konto spåra händelser|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Nätverkssäkerhetsgrupphändelse|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Regelräknare för Nätverkssäkerhetsgrupp|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Läsa in belastningsutjämning avisering händelser|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Läsa in hälsostatus för belastningsutjämnaren avsökning|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Gateway tillgång för programloggen|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Programloggen Gateway prestanda|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Programloggen Gateway-brandväggen|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure-säkerhetskopiering rapportdata|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery-jobb|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery-händelser|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery replikerade objekt|
|Microsoft.Search/searchServices|OperationLogs|Åtgärdsloggar|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Operativa loggar|
|Microsoft.StreamAnalytics/streamingjobs|Körning|Körning|
|Microsoft.StreamAnalytics/streamingjobs|Redigering|Redigering|

## <a name="next-steps"></a>Nästa steg

* [Mer information om diagnostikloggar](monitoring-overview-of-diagnostic-logs.md)
* [Strömma resurs diagnostikloggar för**Händelsehubbar**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Ändra resurs diagnostikinställningar med hello Azure övervakaren REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Analysera loggar från Azure storage med logganalys](../log-analytics/log-analytics-azure-storage.md)
