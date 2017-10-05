---
title: "Azure SQL database mått & diagnostikloggning | Microsoft Docs"
description: "Lär dig mer om hur du konfigurerar Azure SQL Database-resurs för att lagra Resursanvändning, anslutning och statistik för körning av frågan."
services: sql-database
documentationcenter: 
author: vvasic
manager: jhubbard
editor: 
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: vvasic
ms.openlocfilehash: bf41aa530c68ea0e94a09d1dab63237c6f42bce7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL Database-mätvärden och diagnostikloggning 
Azure SQL Database kan sända mått och diagnostikloggar för lättare övervakning. Du kan konfigurera Azure SQL Database för att lagra Resursanvändning, personal och sessioner och anslutning till en av dessa Azure-resurser:
- **Azure Storage**: För arkivering av stora mängder telemetri till ett lågt pris
- **Azure Event Hub**: för att integrera Azure SQL Database telemetri med anpassade övervakningslösning eller varm pipelines
- **Azure logganalys**: för out of box övervakningslösning med reporting, varningar och minimera funktioner 

    ![Arkitektur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Aktivera loggning

Mätvärden och diagnostikfunktionerna loggning är inte aktiverad som standard. Du kan aktivera och hantera mått och diagnostik loggning genom att använda någon av följande metoder:
- Azure Portal
- PowerShell
- Azure CLI
- REST API 
- Resource Manager-mall

Du måste ange Azure-resurs där valda data samlas in när du aktiverar mått och diagnostikloggning. Tillgängliga alternativ:
- Log Analytics
- Händelsehubb
- Azure Storage 

Du kan etablera en ny resurs i Azure eller välj en befintlig resurs. Du måste ange vilka data som ska samlas in när du har valt storage-resursen. Alternativen är:

- **[1 minut mått](sql-database-metrics-diag-logging.md#1-minute-metrics)**  -innehåller DTU-procent, DTU gränsen, CPU-procent fysiska data läsa procent, Log skriva procent, lyckade/misslyckade/Blocked av brandväggens anslutningar, sessioner procent, arbetare procent lagring, lagringsprocent, XTP-lagringsprocent

Om du anger Event Hub eller ett AzureStorage konto kan ange du en bevarandeprincip ange att data som är äldre än en vald tidsperiod tas bort. Om du anger logganalys beror bevarandeprincipen på den valda prisnivån. Läs mer om [logganalys priser](https://azure.microsoft.com/pricing/details/log-analytics/). 

Vi rekommenderar att du läser både den [översikt över mått i Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) och [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artiklar för att få en bättre förståelse av inte bara hur du aktiverar loggning, men mätvärdena och Logga kategorier som stöds av olika Azure-tjänster.

### <a name="azure-portal"></a>Azure Portal

Om du vill aktivera mätvärden och diagnostikloggar samling i Azure portal, gå till din Azure SQL-databas eller en elastisk pool-sidan och klicka sedan på **diagnostikinställningar**.

   ![Aktivera i Azure-portalen](./media/sql-database-metrics-diag-logging/enable-portal.png)

### <a name="powershell"></a>PowerShell

Om du vill aktivera mätvärden och diagnostikloggning med hjälp av PowerShell använder du följande kommandon:

- Använd följande kommando för att aktivera lagring av diagnostiska loggar i ett Lagringskonto:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Storage-konto-ID är resurs-id för lagringskontot som du vill skicka loggar.

- Om du vill aktivera strömning av diagnostiska loggar till en Händelsehubb, Använd följande kommando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Regel-ID för Service Bus är en sträng med formatet:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Använd följande kommando för att aktivera sändning av diagnostiska loggar till en logganalys-arbetsyta:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Du kan hämta resurs-id för logganalys-arbetsytan med följande kommando:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Du kan kombinera dessa parametrar för att aktivera flera alternativ för utdata.

### <a name="cli"></a>CLI

Om du vill aktivera mätvärden och diagnostikloggning med hjälp av Azure CLI, använder du följande kommandon:

- Använd följande kommando för att aktivera lagring av diagnostiska loggar i ett Lagringskonto:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Storage-konto-ID är resurs-id för lagringskontot som du vill skicka loggar.

- Om du vill aktivera strömning av diagnostiska loggar till en Händelsehubb, Använd följande kommando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Regel-ID för Service Bus är en sträng med formatet:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Använd följande kommando för att aktivera sändning av diagnostiska loggar till en logganalys-arbetsyta:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Du kan kombinera dessa parametrar för att aktivera flera alternativ för utdata.

### <a name="rest-api"></a>REST API

Läs mer om hur du [ändra diagnostikinställningar med hjälp av REST API för Azure-Monitor](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager-mall

Läs mer om hur du [aktivera diagnostikinställningar när resursen skapas med hjälp av Resource Manager-mall](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Dataströmmen till logganalys 
Azure SQL Database-mätvärden och diagnostikloggar kan strömmas till logganalys med alternativet inbyggda ”skicka till logganalys” i portalen eller genom att aktivera logganalys i diagnostikinställningen via Azure PowerShell-cmdlet: ar, Azure CLI eller Azure övervakaren REST API.

### <a name="installation-overview"></a>Installationsöversikt

Övervaka Azure SQL Database flottan är enkelt med logganalys. Tre steg krävs:

1.  Skapa resurs för logganalys
2.  Konfigurera databaser för att registrera mått och diagnostiska loggar till skapade logganalys
3.  Installera **Azure SQL Analytics** lösningar från galleriet i logganalys

### <a name="create-log-analytics-resource"></a>Skapa resurs för logganalys

1. Klicka på **ny** i den vänstra menyn.
2. Klicka på **övervakning och hantering**
3. Klicka på **logga Analytics**
4. Fyll i formuläret logganalys med ytterligare information som krävs: arbetsytans namn, prenumeration, resursgrupp, plats och prisnivån.

   ![logganalys](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostic-logs"></a>Konfigurera databaserna så att posten mått och diagnostikloggar

Det enklaste sättet att konfigurera där databaserna registrera sina mått är via Azure-portalen. Navigera till din Azure SQL Database-resurs i Azure-portalen och på **diagnostikinställningarna**. 

### <a name="install-the-azure-sql-analytics-solution-from-gallery"></a>Installera Azure SQL Analytics-lösning från galleriet  

1. Installera Azure SQL Analytics lösning när resursen logganalys har skapats och dina data flödar till den. Detta kan göras via den **lösningar galleriet** som du hittar på OMS-startsidan och på menyn sida. Hitta i galleriet, och klicka på **Azure SQL Analytics** lösningen och klicka på **Lägg till**.

   ![övervakningslösning](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. På startsidan OMS kallas en ny panel **Azure SQL Analytics** visas. Markera den här panelen öppnar Azure SQL Analytics-instrumentpanelen.

### <a name="using-azure-sql-analytics-solution"></a>Med Azure SQL Analytics-lösning

Azure SQL-Analytics är en hierarkisk instrumentpanel som gör att du kan navigera i hierarkin för Azure SQL Database-resurser. Den här funktionen kan du göra avancerade övervakning, men den också möjligt att begränsa övervakningen till precis rätt uppsättning resurser.
Instrumentpanelen innehåller listor över olika resurser under den valda resursen. Till exempel för en vald prenumeration visas alla servrar, elastiska pooler och databaser som tillhör den valda prenumerationen. Du kan dessutom se resurs användningsstatistik för resursen för elastiska pooler och databaser. Detta inkluderar diagram för DTU, CPU, IO, LOG, sessioner, arbetare, anslutningar och lagringsutrymme i GB.

## <a name="stream-into-azure-event-hub"></a>Dataströmmen till Azure Event Hub

Azure SQL Database-mätvärden och diagnostikloggar kan strömmas till Händelsehubb med hjälp av alternativet inbyggda ”dataströmmen till en händelsehubb” i portalen eller genom att aktivera Service Bus regel-Id i diagnostikinställningen via Azure PowerShell-Cmdlets, Azure CLI eller Azure övervakaren REST API. 

### <a name="what-to-do-with-metrics-and-diagnostic-logs-in-event-hub"></a>Vad du gör med mått och diagnostikloggar i Event Hub?
När valda data strömmas till Event Hub, är du ett steg till att aktivera avancerade övervakningsscenarier. Händelsehubbar fungerar som ”ytterdörren” för en händelsepipeline, och när data har samlats in i en händelsehubb kan du omvandla och lagra dessa data med hjälp av valfri leverantör av realtidsanalys eller med adaptrar för batchbearbetning/lagring. Händelsehubbar frikopplar produktionen av en händelseström från användningen av dessa händelser så att händelsekonsumenterna kan komma åt dem på sitt eget schema. För mer information om Event Hub, se:

- [Vad är Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
- [Kom igång med Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Här följer några olika sätt som du kan använda strömmande funktionen:

-   Visa tjänstens hälsa av strömmande ”varm path” data till PowerBI - med Händelsehubbar, Stream Analytics och PowerBI, kan du enkelt omvandla mätvärden och diagnostikfunktionerna data i nära realtidsinsikter på Azure-tjänster. En översikt över hur du ställer in en Händelsehubbar, bearbetning av data med Stream Analytics och Använd PowerBI som utdata, se [Stream Analytics och Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).
-   Dataströmmen loggas till tredje parts loggning och telemetri dataströmmar – med Händelsehubbar direktuppspelning av du kan hämta din mätvärden och diagnostikloggar till olika tredje parts övervaknings- och log analytics lösningar. 
-   Skapa en anpassad telemetri och loggning plattform – om du redan har en specialbyggt telemetri plattform eller är bara du tänker skapa en mycket skalbar publicera och prenumerera uppbyggnad Händelsehubbar kan du flexibelt kan mata in diagnostikloggar. Se [Dan Rosanova guide med Händelsehubbar i en global skala telemetri platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-azure-storage"></a>Dataströmmen till Azure Storage

Azure SQL Database-mätvärden och diagnostikloggar kan lagras i Azure Storage med hjälp av alternativet inbyggda ”Arkiv till ett lagringskonto” i Azure-portalen eller genom att aktivera Azure Storage i diagnostikinställningen via Azure PowerShell-Cmdlets, Azure CLI eller Azure-Monitor REST-API.

### <a name="schema-of-metrics-and-diagnostic-logs-in-the-storage-account"></a>Schemat för mått och diagnostikloggar i storage-konto

När du har konfigurerat mått och diagnostikloggar samling, skapas en lagringsbehållare i storage-konto som du har valt när de första raderna i data är tillgängliga. Strukturen för de här blobbar är:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Eller att:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Till exempel kanske en blob-namnet för 1 minut mått:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Om du vill registrera data från den elastiska poolen är blob-namnet något annat:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-azure-storage"></a>Hämta mätvärden och loggar från Azure storage

Se [hämta mätvärden och diagnostiska loggar från Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)

## <a name="1-minute-metrics"></a>1 minut mått

| |  |
|---|---|
|**Resurs**|**Mått**|
|Databas|DTU-procent DTU används, DTU gränsen, CPU-procent, fysiska data skrivskyddade procent loggen skriva procent, lyckade/misslyckade/Blocked av brandväggens anslutningar, sessioner procent, arbetare procent, lagring, lagringsprocent, XTP lagringsprocent deadlocks |
|Elastisk pool|eDTU procentandel eDTU används, eDTU gränsen, CPU-procent, fysiska data skrivskyddade procent, skriva loggen procent, sessioner procent, arbetare procent, lagring, lagringsprocent, lagringsgräns, XTP-lagringsprocent |
|||

## <a name="next-steps"></a>Nästa steg

- Läsa både den [översikt över mått i Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) och [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artiklar för att få en bättre förståelse av inte bara hur du aktiverar loggning, men kategorier mått och loggar stöd för olika Azure-tjänster.
- Läs artiklarna att lära dig om händelsehubbar:
   - [Vad är Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
   - [Kom igång med Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Se [hämta mätvärden och diagnostiska loggar från Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
