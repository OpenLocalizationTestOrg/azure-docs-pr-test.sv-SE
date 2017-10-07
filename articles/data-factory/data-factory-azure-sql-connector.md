---
title: "aaaCopy data till/från Azure SQL Database | Microsoft Docs"
description: "Lär dig hur toocopy data till/från Azure SQL Database med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: d2ff16191afb028da75699c5e4d0bb310538db0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-database-using-azure-data-factory"></a>Kopiera data tooand från Azure SQL Database med Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data tooand från Azure SQL-databas. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.  

## <a name="supported-scenarios"></a>Scenarier som stöds
Du kan kopiera data **från Azure SQL Database** toohello följande datakällor:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Du kan kopiera data från hello följande datalager **tooAzure SQL-databas**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a>Autentiseringstypen som stöds
Azure SQL Database-anslutningen har stöd för grundläggande autentisering.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till eller från en Azure SQL Database med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa en **datafabriken**. En datafabrik kan innehålla en eller flera pipelines. 
2. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory. Till exempel om du kopierar data från en Azure blob storage tooan Azure SQL database skapa du två länkade tjänster toolink dina Azure storage-konto och Azure SQL database tooyour data factory. Länkad tjänstegenskaper som är specifika tooAzure SQL-databasen, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt. 
3. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata. Och du skapar en annan dataset toospecify hello SQL-tabellen i hello Azure SQL-databas som innehåller hello data som kopieras från hello blob storage. Egenskaper för datamängd som är specifika tooAzure Data Lake Store, se [egenskaper för datamängd](#dataset-properties) avsnitt.
4. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. I hello-exemplet ovan, använder du BlobSource som en källa och SqlSink som en mottagare för hello kopieringsaktiviteten. På samma sätt om du vill kopiera från Azure SQL Database tooAzure Blob Storage, använder du SqlSource och BlobSink i hello kopieringsaktiviteten. Kopiera Aktivitetsegenskaper som är specifika tooAzure SQL-databasen, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt. Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en Azure SQL Database finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-sql-database) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure SQL-databas: 

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
En Azure SQL länkade tjänsten länkar en Azure SQL database tooyour data factory. hello följande tabell innehåller beskrivning för JSON-element specifika tooAzure SQL länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **AzureSqlDatabase** |Ja |
| connectionString |Ange nödvändig information tooconnect toohello Azure SQL Database-instans för hello-egenskapen connectionString. Endast grundläggande autentisering stöds. |Ja |

> [!IMPORTANT]
> Konfigurera [Azure SQL Database-brandvägg](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) hello databasserver för[Tillåt Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Om du kopierar data tooAzure SQL-databas från utanför Azure inklusive från lokala datakällor med data factory-gateway måste du dessutom konfigurera lämplig IP-adressintervall för hello-dator som skickar data tooAzure SQL-databas.

## <a name="dataset-properties"></a>Egenskaper för datamängd
toospecify en dataset toorepresent inkommande eller utgående data i en Azure SQL database, anger du egenskapen hello typ av hello datamängden: **AzureSqlTable**. Ange hello **linkedServiceName** länkad egenskap hello dataset toohello namnet på hello Azure SQL-tjänsten.  

En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej **typeProperties** avsnittet för hello dataset av typen **AzureSqlTable** har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabellen eller vyn i hello Azure SQL Database-instans som den länkade tjänsten refererar till. |Ja |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

> [!NOTE]
> Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.

Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

Om du flyttar data från en Azure SQL database måste du ange hello källtypen i hello kopieringsaktiviteten för**SqlSource**. På samma sätt om du flyttar data tooan Azure SQL-databas måste du ange hello Mottagartypen i hello kopieringsaktiviteten för**SqlSink**. Det här avsnittet innehåller en lista över egenskaper som stöds av SqlSource och SqlSink.

### <a name="sqlsource"></a>SqlSource
I en Kopieringsaktivitet när hello källa är av typen **SqlSource**, hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| sqlReaderQuery |Använda hello anpassad fråga tooread data. |SQL-sträng. Exempel: `select * from MyTable`. |Nej |
| sqlReaderStoredProcedureName |Namnet på hello lagrad procedur som läser data från hello källtabellen. |Namnet på hello lagrad procedur. hello senaste SQL-instruktionen måste vara en SELECT-instruktion i hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |

Om hello **sqlReaderQuery** har angetts för hello SqlSource, hello Kopieringsaktiviteten kör den här frågan mot hello Azure SQL Database källa tooget hello data. Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).

Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName hello kolumner som anges i hello strukturen i hello dataset JSON är används toobuild en fråga (`select column1, column2 from mytable`) toorun mot hello Azure SQL Database. Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.

> [!NOTE]
> När du använder **sqlReaderStoredProcedureName**, du behöver fortfarande toospecify ett värde för hello **tableName** egenskap i hello dataset JSON. Det finns inga verifieringar utföras om mot den här tabellen.
>
>

### <a name="sqlsource-example"></a>SqlSource-exempel

```JSON
"source": {
    "type": "SqlSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```

**definition av hello lagrade proceduren:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqlsink"></a>SqlSink
**SqlSink** stöder hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| writeBatchTimeout |Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel ”: 00: 30:00” (30 minuter). |Nej |
| writeBatchSize |Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize. |Heltal (antalet rader) |Nej (standard: 10000) |
| sqlWriterCleanupScript |Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort. Mer information finns i [repeterbara kopiera](#repeatable-copy). |En frågesats. |Nej |
| sliceIdentifierColumnName |Ange ett kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt. Mer information finns i [repeterbara kopiera](#repeatable-copy). |Kolumnnamnet för en kolumn med datatypen för binary(32). |Nej |
| sqlWriterStoredProcedureName |Namnet på hello lagrad procedur upserts (uppdateringar/infogar) data i hello måltabellen. |Namnet på hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |
| sqlWriterTableType |Ange tabellen typen namnet toobe används i hello lagrade proceduren. Kopieringsaktiviteten tillgängliggör hello data flyttas i en temporär tabell med den här tabellen. Lagrade procedurer kan sedan koppla hello data kopieras med befintliga data. |Ett namn för tabellen. |Nej |

#### <a name="sqlsink-example"></a>SqlSink-exempel

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-tooand-from-sql-database"></a>JSON-exempel för att kopiera data tooand från SQL-databas
hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data tooand från Azure SQL Database och Azure Blob Storage. Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

### <a name="example-copy-data-from-azure-sql-database-tooazure-blob"></a>Exempel: Kopiera data från Azure SQL Database tooAzure Blob
hello definierar samma hello följande Data Factory-enheter:

1. En länkad tjänst av typen [AzureSqlDatabase](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureSqlTable](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [SqlSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar time series-data (varje timme, varje dag, etc.) från en tabell i Azure SQL database tooa blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.  

**Azure SQL Database länkade tjänsten:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Se hello [länkad Azure SQL-tjänst](#linked-service) avsnittet hello lista över egenskaper som stöds av den här länkade tjänsten.

**Azure Blob storage länkade tjänsten:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Se hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) artikel hello lista över egenskaper som stöds av den här länkade tjänsten.


**Azure SQL inkommande datamängd:**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.

Inställningen ”externa”: ”true” informerar hello Azure Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

Se hello [Azure SQL dataset Typegenskaper](#dataset) avsnittet hello lista över egenskaper som stöds av den här dataset-typen.  

**Azure Blob utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
Se hello [Azure Blob dataset Typegenskaper](data-factory-azure-blob-connector.md#dataset-properties) avsnittet hello lista över egenskaper som stöds av den här dataset-typen.  

**Kopieringsaktiviteten i en pipeline med SQL-källa och mottagare för Blob:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
I exemplet hello **sqlReaderQuery** har angetts för hello SqlSource. Hej Kopieringsaktiviteten kör den här frågan mot hello Azure SQL Database källdata tooget hello. Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).

Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner som anges i hello strukturen i hello dataset JSON används toobuild en fråga toorun mot hello Azure SQL Database. Till exempel: `select column1, column2 from mytable`. Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.

Se hello [Sql-källans](#sqlsource) avsnitt och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) hello lista över egenskaper som stöds av SqlSource och BlobSink.

### <a name="example-copy-data-from-azure-blob-tooazure-sql-database"></a>Exempel: Kopiera data från Azure Blob tooAzure SQL-databas
hello exemplet definierar hello följande Data Factory-enheter:  

1. En länkad tjänst av typen [AzureSqlDatabase](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureSqlTable](#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [SqlSink](#copy-activity-properties).

hello exemplet kopierar time series-data (varje timme, varje dag, etc.) från Azure blob tooa tabell i Azure SQL database varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Azure SQL länkade tjänsten:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Se hello [länkad Azure SQL-tjänst](#linked-service) avsnittet hello lista över egenskaper som stöds av den här länkade tjänsten.

**Azure Blob storage länkade tjänsten:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Se hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) artikel hello lista över egenskaper som stöds av den här länkade tjänsten.


**Azure Blob inkommande datamängd:**

Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid. ”externa”: ”true” inställningen informerar hello Data Factory-tjänsten att den här tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
Se hello [Azure Blob dataset Typegenskaper](data-factory-azure-blob-connector.md#dataset-properties) avsnittet hello lista över egenskaper som stöds av den här dataset-typen.

**Azure SQL Database utdatauppsättningen:**

hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i SQL Azure. Skapa hello tabell i Azure SQL med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain. Nya rader läggs toohello tabell varje timme.

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
Se hello [Azure SQL dataset Typegenskaper](#dataset) avsnittet hello lista över egenskaper som stöds av den här dataset-typen.

**Kopieringsaktiviteten i en pipeline med Blob källa och mottagare för SQL:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
Se hello [Sql Sink](#sqlsink) avsnitt och [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) hello lista över egenskaper som stöds av SqlSink och BlobSource.

## <a name="identity-columns-in-hello-target-database"></a>Identitetskolumner i hello måldatabasen
Det här avsnittet innehåller ett exempel för att kopiera data från en källtabell utan en identitet kolumnen tooa måltabellen med en identitetskolumn.

**Källtabellen:**

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Måltabellen:**

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
Observera att hello måltabellen har en identitetskolumn.

**Källa JSON-definitionen för datauppsättning**

```JSON
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Mål JSON-definitionen för datauppsättning**

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }    
}
```

Observera att som käll- och tabellen har olika schema (målet har en ytterligare kolumn med identity). I det här scenariot behöver du toospecify **struktur** egenskap i hello mål datauppsättningsdefinitionen, som inte innehåller hello identity-kolumn.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Anropa lagrade procedur från SQL-mottagare
Ett exempel för att anropa en lagrad procedur från SQL-mottagare i en Kopieringsaktivitet för en pipeline finns [anropa lagrad procedur för SQL-mottagare i en Kopieringsaktivitet](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel. 

## <a name="type-mapping-for-azure-sql-database"></a>Mappning för Azure SQL Database
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data tooand från Azure SQL Database, används hello följande mappningar från SQL too.NET typ och vice versa. hello-mappning är samma som hello Datatypsmappningen i SQL Server för ADO.NET.

| SQL Server Database Engine-typ | .NET framework-typ |
| --- | --- |
| bigint |Int64 |
| Binär |byte] |
| bitar |Booleskt värde |
| Char |Sträng, Char] |
| Datum |Datum och tid |
| Datum och tid |Datum och tid |
| datetime2 |Datum och tid |
| DateTimeOffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM-attributet (varbinary(max)) |byte] |
| flyttal |dubbla |
| Bild |byte] |
| int |Int32 |
| Money |Decimal |
| nchar |Sträng, Char] |
| ntext |Sträng, Char] |
| numeriskt |Decimal |
| nvarchar |Sträng, Char] |
| Verklig |Enskild |
| ROWVERSION |byte] |
| smalldatetime |Datum och tid |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Objektet * |
| Text |Sträng, Char] |
| time |TimeSpan |
| tidsstämpel |byte] |
| tinyint |Mottagna byte |
| Unik identifierare |GUID |
| varbinary |byte] |
| varchar |Sträng, Char] |
| xml |XML |

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Repeterbara kopia
När du kopierar data tooSQL Server-databasen läggs hello kopieringsaktiviteten datatabell toohello mottagare som standard. tooperform en UPSERT Läs [repeterbara skrivåtgärder tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel. 

När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
