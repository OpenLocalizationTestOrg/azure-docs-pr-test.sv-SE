---
title: aaaSQL Server lagrade Proceduraktiviteten
description: "Lär dig hur du kan använda hello lagrade Proceduraktiviteten i SQL Server-tooinvoke en lagrad procedur i en Azure SQL Database eller Azure SQL Data Warehouse från Data Factory-pipelinen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: spelluru
ms.openlocfilehash: 9116f80eefc59d95e866b2ba1de2feb1bdc4b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-stored-procedure-activity"></a>SQLServer lagrade Proceduraktiviteten
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-aktivitet](data-factory-hive-activity.md) 
> * [Pig-aktivitet](data-factory-pig-activity.md)
> * [MapReduce Activity](data-factory-map-reduce.md)
> * [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
> * [Spark-aktivitet](data-factory-spark.md)
> * [Machine Learning Batch-körningsaktivitet](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md)
> * [Lagrad proceduraktivitet](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md)
> * [Anpassad aktivitet för .NET](data-factory-use-custom-activities.md)

## <a name="overview"></a>Översikt
Du kan använda data transformation aktiviteter i en Datafabrik [pipeline](data-factory-create-pipelines.md) tootransform och bearbeta rådata i förutsägelser och insikter. hello är lagrade Proceduraktiviteten en av hello omvandling av aktiviteter som har stöd för Data Factory. Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter i Data Factory.

Du kan använda hello lagrade Proceduraktiviteten tooinvoke en lagrad procedur i någon av hello följande data lagras i företaget, eller på en Azure-dator (VM): 

- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server-databas.  Om du använder SQL Server, installera Data Management Gateway på hello samma datorn att värdar hello databas eller på en separat dator som har åtkomst till toohello databas. Data Management Gateway är en komponent som ansluter datakällor på lokala/på virtuella Azure-datorn med molntjänster i en säker och hanterad sätt. Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information.

> [!IMPORTANT]
> När du kopierar data till Azure SQL Database eller SQL Server, kan du konfigurera hello **SqlSink** i kopiera aktiviteten tooinvoke en lagrad procedur med hjälp av hello **sqlWriterStoredProcedureName** egenskapen. Mer information finns i [anropa lagrade procedur från kopieringsaktiviteten](data-factory-invoke-stored-procedure-from-copy-activity.md). Information om hello egenskapen finns i följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties). Anropa en lagrad procedur vid kopiering av data till en Azure SQL Data Warehouse med hjälp av en kopieringsaktiviteten stöds inte. Men du kan använda hello lagrade proceduren aktiviteten tooinvoke en lagrad procedur i en SQL Data Warehouse. 
>  
> När du kopierar data från Azure SQL Database eller SQL Server eller Azure SQL Data Warehouse, kan du konfigurera **SqlSource** i kopiera aktiviteten tooinvoke en lagrad procedur tooread data från hello källdatabasen med hjälp av hello  **sqlReaderStoredProcedureName** egenskapen. Mer information finns i hello följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          


hello följande genomgången använder hello lagrade Proceduraktiviteten i pipeline-tooinvoke en lagrad procedur i en Azure SQL database. 

## <a name="walkthrough"></a>Genomgång
### <a name="sample-table-and-stored-procedure"></a>Exempel på en tabell och lagrade procedurer
1. Skapa följande hello **tabellen** i Azure SQL-databasen med hjälp av SQL Server Management Studio eller andra verktyg som du är nöjd med. Hej datetimestamp kolumnen är hello datum och tid när motsvarande hello-ID har genererats.

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    ID är hello unika identifieras och hello datetimestamp kolumnen är hello datum och tid när motsvarande hello-ID har genererats.
    
    ![Exempeldata](./media/data-factory-stored-proc-activity/sample-data.png)

    I det här exemplet är hello lagrad procedur i en Azure SQL Database. Om hello lagrade proceduren finns i en Azure SQL Data Warehouse och SQL Server-databas, liknar hello-metoden. SQL Server-databas måste du installera en [Data Management Gateway](data-factory-data-management-gateway.md).
2. Skapa följande hello **lagrade proceduren** som infogar data i toohello **sampletable**.

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > **Namnet** och **skiftläge** av hello parametern (DateTime i det här exemplet) måste matcha parameter som anges i hello pipeline/aktivitets-JSON. I hello lagrade Procedurdefinition, se till att  **@**  används som ett prefix för hello-parametern.

### <a name="create-a-data-factory"></a>Skapa en datafabrik
1. Logga in för[Azure-portalen](https://portal.azure.com/).
2. Klicka på **ny** på hello vänstra menyn **Intelligence + analys**, och klicka på **Data Factory**.

    ![Nya data factory](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. I hello **nya datafabriken** bladet ange **SProcDF** för hello namn. Azure Data Factory-namn är **globalt unika**. Du måste tooprefix hello namn i hello data factory med ditt namn, tooenable hello har skapats av hello fabriken.

   ![Nya data factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. Välj din **Azure-prenumeration**.
5. För **resursgruppen**, gör du något av följande hello:
   1. Klicka på **Skapa nytt** och ange ett namn för hello resursgrupp.
   2. Klicka på **använda befintliga** och välj en befintlig resursgrupp.  
6. Välj hello **plats** för hello data factory.
7. Välj **PIN-kod toodashboard** så att du kan se hello data factory på hello instrumentpanelen nästa gång du loggar in.
8. Klicka på **skapa** på hello **nya data factory** bladet.
9. Du ser hello data factory som skapas i hello **instrumentpanelen** av hello Azure-portalen. När hello datafabriken har skapats, visas hello data factory sidan som visar hello innehållet i hello data factory.

   ![Data Factory-startsida](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Skapa en Azure SQL-länkade tjänst
När du har skapat hello data factory, kan du skapa en Azure SQL-länkade tjänst som länkar din Azure SQL-databas som innehåller hello sampletable tabell och sp_sample lagrad procedur, tooyour data factory.

1. Klicka på **författare och distribuera** på hello **Datafabriken** bladet för **SProcDF** toolaunch hello Data Factory-redigeraren.
2. Klicka på **Nytt datalager** hello kommandofältet och välj **Azure SQL Database**. Du bör se hello JSON-skript för att skapa en Azure SQL länkade tjänsten i hello-redigeraren.

   ![Nytt datalager](media/data-factory-stored-proc-activity/new-data-store.png)
3. Gör följande ändringar hello i hello JSON-skript:

   1. Ersätt `<servername>` med hello namnet på din Azure SQL Database-server.
   2. Ersätt `<databasename>` med hello-databas som du skapade hello tabell och hello lagrad procedur.
   3. Ersätt `<username@servername>` med hello-användarkonto som har åtkomst till toohello databas.
   4. Ersätt `<password>` med hello lösenordet för användarkontot för hello.

      ![Nytt datalager](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. toodeploy hello länkade tjänsten klickar du på **distribuera** i hello kommandofält. Bekräfta att du ser hello AzureSqlLinkedService i hello trädet visa hello vänster.

    ![trädvy för länkad tjänst](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Skapa en datauppsättning för utdata
Du måste ange en datamängd för utdata för en lagrad procedur aktivitet även om hello lagrade proceduren inte skapar några data. Det beror på att dess hello utdatauppsättningen som enheter hello schemat för hello aktivitet (hur ofta hello aktiviteten körs - varje timme, varje dag, etc.). Hej utdatauppsättningen måste använda en **länkade tjänsten** som refererar tooan Azure SQL Database eller ett Azure SQL Data Warehouse eller en SQL Server-databas som du vill hello lagrade proceduren toorun. Hej utdatauppsättningen kan fungera som ett sätt toopass hello resultat hello lagrade proceduren för senare bearbetning av en annan aktivitet ([länkning aktiviteter](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) i hello pipeline. Data Factory kan dock inte automatiskt skriva hello utdata från en lagrad procedur toothis dataset. Det är hello lagrad procedur som skrivningar tooa SQL-tabell som hello utdata dataset pekar på. I vissa fall hello utdatauppsättningen kan vara en **dummy dataset** (en datamängd som pekar tooa tabell som inte innehåller utdata från hello verkligen lagrade proceduren). Den här dummy datauppsättningen används bara för toospecify hello schema för att köra hello lagrade proceduraktiviteten. 

1. Klicka på **... Flera** på hello verktygsfältet **ny datamängd**, och klicka på **Azure SQL**. **Ny datamängd** på hello kommandot och välj **Azure SQL**.

    ![trädvy för länkad tjänst](media/data-factory-stored-proc-activity/new-dataset.png)
2. Kopiera och klistra in hello följande JSON-skript i toohello JSON-redigerare.

    ```JSON
    {                
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. toodeploy Hej dataset, klickar du på **distribuera** i hello kommandofält. Bekräfta att du ser hello datauppsättning i hello trädvyn.

    ![Trädvy med länkade tjänster](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Skapa en pipeline med SqlServerStoredProcedure aktivitet
Nu ska vi skapa en pipeline med en lagrad procedur-aktivitet. 

Observera hello följande egenskaper: 

- Hej **typen** egenskapen för**SqlServerStoredProcedure**. 
- Hej **storedProcedureName** i typen ange egenskaper för**sp_sample** (namnet på hello lagrade proceduren).
- Hej **storedProcedureParameters** avsnittet innehåller en parameter med namnet **DataTime**. Namn och skiftläge för hello-parametern i JSON måste matcha hello namn och versaler och gemener i hello-parametern i hello lagrade Procedurdefinition. Om du behöver lägga till null för en parameter, använder hello syntax: `"param1": null` (gemener).
 
1. Klicka på **... Flera** hello kommandofältet och klicka på **ny pipeline**.
2. Kopiera och klistra in följande JSON-fragment hello:   

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "sp_sample",
                        "storedProcedureParameters": {
                            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                        }
                    },
                    "outputs": [
                        {
                            "name": "sprocsampleout"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SprocActivitySample"
                }
            ],
             "start": "2017-04-02T00:00:00Z",
             "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. toodeploy hello pipeline, klickar du på **distribuera** hello i verktygsfältet.  

### <a name="monitor-hello-pipeline"></a>Övervakaren hello pipeline
1. Klicka på **X** tooclose Data Factory-redigeraren blad toonavigate tillbaka toohello Data Factory-bladet och på **Diagram**.

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. I hello **diagramvyn**du se en översikt över hello pipelines och datauppsättningar som används i den här kursen.

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. Dubbelklicka på hello dataset i hello diagramvyn, `sprocsampleout`. Du ser hello segment i tillståndet Ready. Det bör finnas fem segment eftersom ett segment skapas för varje timme mellan hello starttid och sluttid från hello JSON.

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. När ett segment är i **klar** tillstånd, kör en `select * from sampletable` frågan mot hello Azure SQL database tooverify som hello data infogades i tabellen toohello av hello lagrade proceduren.

   ![utdata](./media/data-factory-stored-proc-activity/output.png)

   Se [övervakaren hello pipeline](data-factory-monitor-manage-pipelines.md) detaljerad information om hur du övervakar Azure Data Factory pipelines.  


## <a name="specify-an-input-dataset"></a>Ange en inkommande datauppsättning
I hello genomgången har lagrade proceduraktiviteten inte några inkommande datauppsättningar. Om du anger en inkommande datauppsättning hello lagrade proceduraktiviteten körs inte förrän hello segment av inkommande dataset är tillgänglig (i tillståndet Ready). hello dataset kan vara en externa datauppsättningen (som inte tillverkas av en annan aktivitet i hello samma rörledning) eller en intern datauppsättning som produceras av en överordnad aktivitet (hello aktivitet som körs före den här aktiviteten). Du kan ange flera indatauppsättningar för hello lagrade proceduraktiviteten. Om du gör det hello lagrade proceduraktiviteten körs bara när alla hello inkommande dataset-segment är tillgängliga (statusen Ready). hello inkommande dataset kan inte användas i hello lagrade procedur som en parameter. Det är bara använda toocheck hello beroende innan start hello lagrade proceduraktiviteten.

## <a name="chaining-with-other-activities"></a>Länkning med andra aktiviteter
Ange hello utdata från hello uppströmsaktivitet som indata för den här aktiviteten om du vill toochain en överordnad aktivitet med den här aktiviteten. När du gör det hello lagrade proceduraktiviteten körs inte förrän hello överordnade aktiviteten har slutförts och hello utdatauppsättningen hello överordnad aktivitet är tillgänglig (i redo status). Du kan ange utdata-datauppsättningar på flera överordnade aktiviteter som inkommande datauppsättningar på hello lagrade proceduraktiviteten. När du gör det hello lagrade proceduraktiviteten körs bara när alla hello inkommande dataset-segment är tillgängliga.  

I följande exempel hello, hello utdata från hello kopieringsaktiviteten är: OutputDataset som utgör indata för hello lagrade proceduraktiviteten. Därför hello lagrade proceduraktiviteten körs inte förrän hello kopieringsaktiviteten har slutförts och hello OutputDataset sektorn är tillgänglig (i tillståndet Ready). Om du anger flera indatauppsättningar hello lagrade proceduraktiviteten körs inte förrän alla hello inkommande dataset-segment är tillgängliga (statusen Ready). Hej indatauppsättningar kan inte användas direkt som parametrar toohello lagrade proceduraktiviteten. 

Mer information om länkning aktiviteter finns [flera aktiviteter i en pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob tooblob",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }

        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

På liknande sätt toolink hello store proceduraktiviteten med **underordnade aktiviteter** (hello aktiviteter som körs efter hello lagrade proceduraktiviteten har slutförts), ange hello datamängd för utdata för hello lagrade proceduraktiviteten som en Ange hello underordnad aktivitet i hello pipeline.

> [!IMPORTANT]
> När du kopierar data till Azure SQL Database eller SQL Server, kan du konfigurera hello **SqlSink** i kopiera aktiviteten tooinvoke en lagrad procedur med hjälp av hello **sqlWriterStoredProcedureName** egenskapen. Mer information finns i [anropa lagrade procedur från kopieringsaktiviteten](data-factory-invoke-stored-procedure-from-copy-activity.md). Mer information om hello egenskap finns hello följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).
>  
> När du kopierar data från Azure SQL Database eller SQL Server eller Azure SQL Data Warehouse, kan du konfigurera **SqlSource** i kopiera aktiviteten tooinvoke en lagrad procedur tooread data från hello källdatabasen med hjälp av hello  **sqlReaderStoredProcedureName** egenskapen. Mer information finns i hello följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          

## <a name="json-format"></a>JSON-format
Här är hello JSON-format för att definiera en lagrade Proceduraktiviteten:

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of hello stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

hello följande tabell beskrivs egenskaperna JSON:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| namn | Namnet på hello-aktivitet |Ja |
| description |Text som beskriver vilka hello-aktivitet används för |Nej |
| typ | Måste anges till: **SqlServerStoredProcedure** | Ja |
| Indata | Valfri. Om du anger en inkommande datauppsättning, måste det vara tillgänglig (statusen ”klar”) för hello lagrade proceduren aktiviteten toorun. hello inkommande dataset kan inte användas i hello lagrade procedur som en parameter. Det är bara använda toocheck hello beroende innan start hello lagrade proceduraktiviteten. |Nej |
| utdata | Du måste ange en datamängd för utdata för en lagrad procedur-aktivitet. Utdatauppsättningen anger hello **schema** för hello lagrade proceduraktiviteten (varje timme, varje vecka, månad, etc.). <br/><br/>Hej utdatauppsättningen måste använda en **länkade tjänsten** som refererar tooan Azure SQL Database eller ett Azure SQL Data Warehouse eller en SQL Server-databas som du vill hello lagrade proceduren toorun. <br/><br/>Hej utdatauppsättningen kan fungera som ett sätt toopass hello resultat hello lagrade proceduren för senare bearbetning av en annan aktivitet ([länkning aktiviteter](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) i hello pipeline. Data Factory kan dock inte automatiskt skriva hello utdata från en lagrad procedur toothis dataset. Det är hello lagrad procedur som skrivningar tooa SQL-tabell som hello utdata dataset pekar på. <br/><br/>I vissa fall hello utdatauppsättningen kan vara en **dummy dataset**, som används för endast toospecify hello schema för att köra hello lagrade proceduraktiviteten. |Ja |
| storedProcedureName |Ange hello namnet på hello lagrade procedur i hello Azure SQL database eller Azure SQL Data Warehouse eller SQL Server-databas som representeras av hello länkade tjänst som hello utdata tabellen använder. |Ja |
| storedProcedureParameters |Ange värden för parametrarna för lagrade procedurer. Om du behöver toopass null för en parameter, Använd hello syntax: ”param1”: null (alla gemen). Se följande exempel toolearn om hur du använder den här egenskapen hello. |Nej |

## <a name="passing-a-static-value"></a>Skicka ett statiskt värde
Nu ska vi Överväg att lägga till en annan kolumn med namnet 'Scenariot' i hello tabell som innehåller ett statiskt värde med namnet 'Dokumentera exempel'.

![Exempeldata 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

**Tabell:**

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

**Lagrade proceduren:**

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

Nu kan skicka hello **scenariot** värde för parametern och hello från hello lagrade proceduraktiviteten. Hej **typeProperties** avsnitt i föregående exempel ser ut som följande fragment hello hello:

```JSON
"typeProperties":
{
    "storedProcedureName": "sp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

**Data Factory datauppsättningen:**

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Data Factory-pipelinen**

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```