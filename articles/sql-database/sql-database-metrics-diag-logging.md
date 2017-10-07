---
title: "aaaAzure SQL-databas mått & diagnostikloggning | Microsoft Docs"
description: "Lär dig mer om hur du konfigurerar Azure SQL Database Resursanvändning toostore resurs-, anslutnings- och statistik för körning av frågan."
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
ms.openlocfilehash: e6f9e24992ca4f84f701e1ef858e98dc7b481e28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL Database-mätvärden och diagnostikloggning 
Azure SQL Database kan sända mått och diagnostikloggar för lättare övervakning. Du kan konfigurera Azure SQL Database toostore Resursanvändning, personal och sessioner och anslutning till en av dessa Azure-resurser:
- **Azure Storage**: För arkivering av stora mängder telemetri till ett lågt pris
- **Azure Event Hub**: för att integrera Azure SQL Database telemetri med anpassade övervakningslösning eller varm pipelines
- **Azure logganalys**: för out of box för hello övervakningslösning med reporting, varningar och minimera funktioner 

    ![Arkitektur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Aktivera loggning

Mätvärden och diagnostikfunktionerna loggning är inte aktiverad som standard. Du kan aktivera och hantera mått och diagnostikloggning med någon av följande metoder hello:
- Azure Portal
- PowerShell
- Azure CLI
- REST API 
- Resource Manager-mall

När du aktiverar mått och diagnostikloggning måste toospecify hello Azure-resurs där valda data samlas in. Tillgängliga alternativ:
- Log Analytics
- Händelsehubb
- Azure Storage 

Du kan etablera en ny resurs i Azure eller välj en befintlig resurs. När du har valt hello lagringsresurs, måste du toospecify vilka data toocollect. Alternativen är:

- **[1 minut mått](sql-database-metrics-diag-logging.md#1-minute-metrics)**  -innehåller DTU-procent, DTU gränsen, CPU-procent fysiska data läsa procent, Log skriva procent, lyckade/misslyckade/Blocked av brandväggens anslutningar, sessioner procent, arbetare procent lagring, lagringsprocent, XTP-lagringsprocent

Om du anger Event Hub eller ett AzureStorage konto, kan du ange en kvarhållning princip toospecify de data som är äldre än en vald tidsperiod tas bort. Om du anger logganalys beroende hello bevarandeprincip hello valda prisnivån. Läs mer om [logganalys priser](https://azure.microsoft.com/pricing/details/log-analytics/). 

Vi rekommenderar att du läser båda hello [översikt över mått i Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) och [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artiklar toogain förstå inte bara hur tooenable loggning, men hello kategorier av mätvärden och loggfiler som stöds av hello olika Azure-tjänster.

### <a name="azure-portal"></a>Azure Portal

tooenable mått diagnostikloggar samling i hello Azure-portalen navigera tooyour Azure SQL database eller elastisk pool-sidan och klicka sedan på **diagnostikinställningar**.

   ![Aktivera i hello Azure-portalen](./media/sql-database-metrics-diag-logging/enable-portal.png)

### <a name="powershell"></a>PowerShell

tooenable mätvärden och diagnostikfunktionerna loggning Använd med hjälp av PowerShell, hello följande kommandon:

- tooenable lagring av diagnostiska loggar i ett Lagringskonto, Använd följande kommando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   hello Storage-konto-ID är hello resurs-id för hello storage-konto toowhich du vill toosend hello loggar.

- tooenable strömning av diagnostikloggar tooan Händelsehubb, Använd följande kommando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   hello regel-ID för Service Bus är en sträng med formatet:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- tooenable att skicka diagnostikloggar tooa logganalys-arbetsytan, Använd följande kommando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
   ```

- Du kan hämta hello resurs-id för logganalys-arbetsytan med hello följande kommando:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Du kan kombinera flera Utdataalternativ tooenable dessa parametrar.

### <a name="cli"></a>CLI

tooenable mått och diagnostik loggar med hello Azure CLI, Använd hello följande kommandon:

- tooenable lagring av diagnostiska loggar i ett Lagringskonto, Använd följande kommando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   hello Storage-konto-ID är hello resurs-id för hello storage-konto toowhich du vill toosend hello loggar.

- tooenable strömning av diagnostikloggar tooan Händelsehubb, Använd följande kommando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   hello regel-ID för Service Bus är en sträng med formatet:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- tooenable att skicka diagnostikloggar tooa logganalys-arbetsytan, Använd följande kommando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
   ```

Du kan kombinera flera Utdataalternativ tooenable dessa parametrar.

### <a name="rest-api"></a>REST API

Läs om hur för[ändra diagnostikinställningar med hello Azure övervakaren REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager-mall

Läs om hur för[aktivera diagnostikinställningar när resursen skapas med hjälp av Resource Manager-mall](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Dataströmmen till logganalys 
Azure SQL Database-mätvärden och diagnostiska loggar strömmas till logganalys alternativet hello inbyggda ”skicka tooLog Analytics” hello-portalen eller genom att aktivera logganalys i diagnostikinställningen via Azure PowerShell-cmdlets, Azure CLI eller Azure-Monitor REST API.

### <a name="installation-overview"></a>Installationsöversikt

Övervaka Azure SQL Database flottan är enkelt med logganalys. Tre steg krävs:

1.  Skapa resurs för logganalys
2.  Konfigurera databaserna toorecord mått och diagnostikloggar i hello skapade logganalys
3.  Installera **Azure SQL Analytics** lösningar från galleriet i logganalys

### <a name="create-log-analytics-resource"></a>Skapa resurs för logganalys

1. Klicka på **ny** hello vänstra menyn.
2. Klicka på **övervakning och hantering**
3. Klicka på **logga Analytics**
4. Fyll i hello logganalys form med hello ytterligare information som krävs: arbetsytans namn, prenumeration, resursgrupp, plats och prisnivån.

   ![logganalys](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-toorecord-metrics-and-diagnostic-logs"></a>Konfigurera databaserna toorecord mått och diagnostikloggar

hello enklaste sättet tooconfigure där databaserna registrera sina mått är via hello Azure-portalen. I hello Azure-portalen, navigera tooyour Azure SQL Database-resursen och klickar på **diagnostikinställningarna**. 

### <a name="install-hello-azure-sql-analytics-solution-from-gallery"></a>Installera hello Azure SQL Analytics lösning från galleriet  

1. Installera Azure SQL Analytics lösning när hello logganalys resursen skapas och dina data flödar till den. Detta kan göras via hello **lösningar galleriet** som du hittar på hello OMS startsida och hello sida-menyn. Hello Gallery och hitta och klickar på **Azure SQL Analytics** lösningen och klicka på **Lägg till**.

   ![övervakningslösning](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. På startsidan OMS kallas en ny panel **Azure SQL Analytics** visas. Om du markerar den här panelen öppnas hello Azure SQL instrumentpanelen.

### <a name="using-azure-sql-analytics-solution"></a>Med Azure SQL Analytics-lösning

Azure SQL-Analytics är en hierarkisk instrumentpanel som gör att du toonavigate via hello hierarkin för Azure SQL Database-resurser. Den här funktionen möjliggör du toodo övergripande övervakning men också kan du tooscope din övervakning toojust hello rätt uppsättning resurser.
Instrumentpanelen innehåller hello listor över olika resurser under hello markerad resurs. För en vald prenumeration kan du till exempel se hello servrar, elastiska pooler och databaser som tillhör toohello valda prenumerationen. Du kan dessutom se hello resurs användningsstatistik för resursen för elastiska pooler och databaser. Detta inkluderar diagram för DTU, CPU, IO, LOG, sessioner, arbetare, anslutningar och lagringsutrymme i GB.

## <a name="stream-into-azure-event-hub"></a>Dataströmmen till Azure Event Hub

Azure SQL Database-mätvärden och diagnostiska loggar strömmas till Event Hub alternativet hello inbyggda ”dataströmmen tooan event hub” hello-portalen eller genom att aktivera Service Bus regel-Id i diagnostikinställningen via Azure PowerShell-Cmdlets, Azure CLI eller Azure-Monitor REST API. 

### <a name="what-toodo-with-metrics-and-diagnostic-logs-in-event-hub"></a>Vilka toodo med mått och diagnostikloggar i Event Hub?
När hello valda data strömmas till Event Hub, är en steg-närmare tooenabling avancerade scenarier för övervakning. Händelsehubbar fungerar som hello ”ytterdörren” för en händelsepipeline, och när data har samlats in i en Händelsehubb, det kan omvandlas och lagras med hjälp av en leverantör av realtidsanalys eller adaptrar för batchbearbetning/lagring. Händelsehubbar frikopplar hello framställning av en dataström med händelser från hello användningen av dessa händelser så att händelsekonsumenterna kan komma åt hello händelser på sitt eget schema. För mer information om Event Hub, se:

- [Vad är Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
- [Kom igång med Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Här följer några olika sätt som du kan använda hello strömning kapaciteten:

-   Visa tjänstens hälsa av strömning ”varm path” data tooPowerBI - med Händelsehubbar, Stream Analytics och PowerBI, kan du enkelt omvandla mätvärden och diagnostikfunktionerna data i nära realtidsinsikter på Azure-tjänster. En översikt över hur tooset upp en Händelsehubbar bearbeta data med Stream Analytics och använda PowerBI som utdata, se [Stream Analytics och Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).
-   Dataströmmen loggar toothird parts loggning och telemetri dataströmmar – med Händelsehubbar direktuppspelning av du kan hämta din mått och diagnostikloggar i toodifferent övervaknings- och log analytics tredjepartslösningar. 
-   Skapa en anpassad telemetri och loggning plattform – om du redan har en specialbyggt telemetri plattform eller är bara du tänker skapa en mycket skalbar hello publicera och prenumerera uppbyggnad Händelsehubbar kan du tooflexibly mata in diagnostikloggar. Se [Dan Rosanova guide toousing Händelsehubbar i en global skala telemetri platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-azure-storage"></a>Dataströmmen till Azure Storage

Azure SQL Database-mätvärden och diagnostikloggar kan lagras i Azure Storage alternativet hello inbyggda ”Arkivera tooa storage-konto” i hello Azure-portalen eller genom att aktivera Azure Storage i diagnostikinställningen via Azure PowerShell-Cmdlets, Azure CLI eller Azure Övervakaren REST API.

### <a name="schema-of-metrics-and-diagnostic-logs-in-hello-storage-account"></a>Schemat för mått och diagnostikloggar i hello storage-konto

När du har konfigurerat mått och diagnostikloggar samling, skapas en lagringsbehållare i hello storage-konto som du valde när hello första datarader är tillgängliga. hello strukturen för de här blobbar är:

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

Om du vill toorecord hello data från hello elastisk Pool, är blob-namnet något annat:

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

- Läsa båda hello [översikt över mått i Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) och [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artiklar toogain förstå inte bara hur tooenable loggning, men hello mått och logga kategorier stöds av hello olika Azure-tjänster.
- Läs dessa artiklar toolearn om händelsehubbar:
   - [Vad är Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
   - [Kom igång med Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Se [hämta mätvärden och diagnostiska loggar från Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
