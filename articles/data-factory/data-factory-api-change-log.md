---
title: "aaaData Factory - .NET API ändringsloggen | Microsoft Docs"
description: "Beskriver viktiga förändringar, funktionen tillägg, felkorrigeringar etc... i en viss version av .NET-API för hello Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8208271b-7f4c-4214-b665-d2ff503c4470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: spelluru
ms.openlocfilehash: 1d44b45c3dc8f9d483d1f1cef7068edacc610932
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---net-api-change-log"></a>Azure Data Factory - ändringsloggen för .NET-API
Den här artikeln innehåller information om ändringar tooAzure Data Factory SDK i en viss version. Du kan hitta hello senaste NuGet-paketet för Azure Data Factory [här](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)

## <a name="version-4110"></a>Version 4.11.0
Funktionen tillägg:

* hello har följande länkade tjänsttyper lagts till:
  * [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
  * [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
  * [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
* hello följande dataset typer har lagts till:
  * [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
  * [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
* hello följande kopiera källan typer har lagts till:
  * [MongoDbSource](https://msdn.microsoft.com/library/mt765123.aspx)

## <a name="version-4100"></a>Version 4.10.0
* hello följande valfria egenskaper har lagts till tooTextFormat:
  * [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
  * [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
  * [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
* hello har följande länkade tjänsttyper lagts till:
  * [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
  * [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
* hello följande dataset typer har lagts till:
  * [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
* hello följande kopiera källan typer har lagts till:
  * [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
* Lägg till [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) egenskapen tooAzureMLBatchExecutionActivity
  * Aktivera skicka flera service indata tooan Azure Machine Learning-experiment

## <a name="version-491"></a>Version 4.9.1
### <a name="bug-fix"></a>Buggfix
* Föråldrad WebApi-baserad autentisering för [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Version 4.9.0
### <a name="feature-additions"></a>Funktionen tillägg
* Lägg till [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) och [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) tooCopyActivity egenskaper. Se [mellanlagrad kopiera](data-factory-copy-activity-performance.md#staged-copy) mer information om hello-funktionen.

### <a name="bug-fix"></a>Buggfix
* Introducerar en överlagring av [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) metod som tar en [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) instans.
* Markera [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) och [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) som valfria i CopySink.

## <a name="version-480"></a>Version 4.8.0
### <a name="feature-additions"></a>Funktionen tillägg
* hello följande valfria egenskaper har lagts till tooCopy aktiviteten Skriv tooenable finjustering av kopiera prestanda:
  * [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
  * [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Version 4.7.0
### <a name="feature-additions"></a>Funktionen tillägg
* Lägga till ny typ av StorageFormat [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) skriver toocopy filer i optimerad raden (ORC) kolumnformat.
* Lägg till [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) och PolyBaseSettings egenskaper tooSqlDWSink.
  * Använda hello PolyBase toocopy data till SQL Data Warehouse.

## <a name="version-461"></a>Version 4.6.1
### <a name="bug-fixes"></a>Felkorrigeringar
* Åtgärdar HTTP-begäran för att lista aktivitet windows.
  * Tar bort hello resursgruppens namn och hello datafabriksnamnet från hello nyttolasten i begäran.

## <a name="version-460"></a>Version 4.6.0
### <a name="feature-additions"></a>Funktionen tillägg
* hello följande egenskaper har lagts till för[PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
  * [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
  * [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
  * [Datauppsättningar](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
* hello följande egenskaper har lagts till för[PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
  * [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
* Lägga till nya [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) typen [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) skriver toodefine datauppsättningar vars data är i JSON-format.

## <a name="version-450"></a>Version 4.5.0
### <a name="feature-additions"></a>Funktionen tillägg
* Lägga till [lista över åtgärder för aktivitetsfönstret](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
  * Lägga till metoder tooretrieve aktivitet windows med filtreras baserat på hello entitetstyper (det vill säga datafabriker, datauppsättningar, rörledningar och aktiviteter).
* hello har följande länkade tjänsttyper lagts till:
  * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* hello följande dataset typer har lagts till:
  * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* hello följande kopiera källan typer har lagts till:     
  * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Version 4.4.0
### <a name="feature-additions"></a>Funktionen tillägg
* hello följande länkade tjänsttypen har lagts till som datakällor och egenskaperna för kopiera aktiviteter:
  * [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Se [Azure SAS länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) konceptuell information och exempel.

## <a name="version-430"></a>Version 4.3.0
### <a name="feature-additions"></a>Funktionen tillägg
* Hej följande länkade tjänsten typer haven har lagts till som datakällor för kopiera aktiviteter:
  * [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Se [flytta data från HDFS med hjälp av Data Factory](data-factory-hdfs-connector.md) konceptuell information och exempel.
  * [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Se [flytta data från ODBC datalager med Azure Data Factory](data-factory-odbc-connector.md) konceptuell information och exempel.

## <a name="version-420"></a>Version 4.2.0
### <a name="feature-additions"></a>Funktionen tillägg
* hello efter ny aktivitetstyp har lagts till: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Mer information om hello aktivitet finns [uppdaterar Azure ML-modeller med hello Uppdateringsresursaktivitet](data-factory-azure-ml-batch-execution-activity.md).
* En ny valfri egenskap [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) har lagts till toohello [AzureMLLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx).
* [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) och [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) egenskaper har lagts till toohello [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) klass.
* Att konfigurera hello tidsgränser för klienten anrop toohello Data Factory-tjänsten.

## <a name="version-410"></a>Version 4.1.0
### <a name="feature-additions"></a>Funktionen tillägg
* hello har följande länkade tjänsttyper lagts till:
  * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
  * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* hello följande aktivitetstyper har lagts till:
  * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* hello följande dataset typer har lagts till:
  * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* hello har följande typer av källa och mottagare för Kopieringsaktiviteten lagts till:
  * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
  * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)

## <a name="version-401"></a>Version 4.0.1
### <a name="breaking-changes"></a>Gör ändringar
hello följande klasser har bytt namn. hello hette nya hello ursprungliga namnen på klasser innan 4.0.0 släpper.

| Namn i 4.0.0 | Namn i 4.0.1 |
|:--- |:--- |
| AzureSqlDataWarehouseDataset |[AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx) |
| AzureSqlDataset |[AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx) |
| AzureDataset |[AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx) |
| OracleDataset |[OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx) |
| RelationalDataset |[RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx) |
| SqlServerDataset |[SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx) |

## <a name="version-400"></a>Version 4.0.0
### <a name="breaking-changes"></a>Gör ändringar
* hello följande klasser-gränssnitt har ändrats.

| Gamla namnet | Nytt namn |
|:--- |:--- |
| ITableOperations |[IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |
| Tabell |[DataSet](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) |
| TableProperties |[DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) |
| TableTypeProprerties |[DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters |[DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) |
| TableCreateOrUpdateResponse |[DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) |
| TableGetResponse |[DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) |
| TableListResponse |[DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters |[DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) |

* Hej **lista** metoder returnerar växlingsbara resultat nu. Om hello svaret innehåller ett icke-tomt **NextLink** egenskapen hello-klientprogrammet måste toocontinue hämtning hello nästa sida tills alla sidor som returneras.  Här är ett exempel:

    ```csharp
    PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
    var pipelines = new List<Pipeline>(response.Pipelines);

    string nextLink = response.NextLink;
    while (!string.IsNullOrEmpty(nextLink))
    {
        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
        pipelines.AddRange(nextResponse.Pipelines);

        nextLink = nextResponse.NextLink;
    }
    ```
* **Lista** pipeline API returnerar endast hello sammanfattning av en pipeline i stället för fullständig information. Till exempel innehålla aktiviteter i en pipeline-sammanfattning endast namn och typ.

### <a name="feature-additions"></a>Funktionen tillägg
* Hej [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) klassen stöder två nya egenskaper **SliceIdentifierColumnName** och **SqlWriterCleanupScript**, toosupport idempotent kopiera tooAzure SQL-Data Datalager. Se hello [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) artikeln för information om dessa egenskaper.
* Vi stöder nu Kör lagrad procedur mot Azure SQL Database och Azure SQL Data Warehouse källor som en del av hello Kopieringsaktiviteten. Hej [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) och [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) klasser har hello följande egenskaper: **SqlReaderStoredProcedureName** och **StoredProcedureParameters** . Se hello [Azure SQL Database](data-factory-azure-sql-connector.md#sqlsource) och [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) artiklar på Azure.com mer information om dessa egenskaper.  
