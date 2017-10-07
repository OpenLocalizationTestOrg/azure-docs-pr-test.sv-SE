---
title: "aaaPush data tooSearch index med hjälp av Data Factory | Microsoft Docs"
description: "Mer information om hur toopush data tooAzure sökindex med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: f2d973d0a2c24d6448e2d59e37e24503aa433018
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="push-data-tooan-azure-search-index-by-using-azure-data-factory"></a>Push-data tooan Azure Search index med hjälp av Azure Data Factory
Den här artikeln beskriver hur toouse hello Kopieringsaktiviteten toopush data från en stöds källdata lagrar tooAzure sökindex. Datalager stöds källa anges i hello källkolumnen av hello [källor och sänkor stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning stöds data store kombinationer och Kopieringsaktivitet.

## <a name="enabling-connectivity"></a>Aktivera anslutning
tooallow Data Factory-tjänsten Anslut tooan lokalt datalager kan du installera Data Management Gateway i din lokala miljö. Du kan installera gatewayen på hello samma dator som är värd för hello källdata eller på en separat dator tooavoid konkurrerar om resurser med hello data.

Data Management Gateway ansluter lokala data sources toocloud tjänster på en säker och hanterad sätt. Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikeln för information om Data Management Gateway.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som skickar data från en källa data store tooAzure sökindex med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data tooAzure sökindex finns [JSON-exempel: kopiera data från lokala SQL Server tooAzure sökindex](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure sökindex:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper

hello i den följande tabellen finns beskrivningar JSON-element som är specifika toohello Azure Search länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- |
| typ | hello Typegenskapen måste anges till: **AzureSearch**. | Ja |
| URL: en | URL för hello Azure Search-tjänsten. | Ja |
| key | Admin-nyckel för hello Azure Search-tjänsten. | Ja |

## <a name="dataset-properties"></a>Egenskaper för datamängd

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen. Hej **typeProperties** avsnittet är olika för varje typ av datauppsättningen. Hej typeProperties avsnittet för en dataset av hello typen **AzureSearchIndex** har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- |
| typ | hello Typegenskapen måste anges för**AzureSearchIndex**.| Ja |
| Indexnamn | Namnet på hello Azure Search index. Data Factory skapar inte hello index. hello index måste finnas i Azure Search. | Ja |


## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar pipelines](data-factory-create-pipelines.md) artikel. Egenskaper, till exempel namn, beskrivning, indata och utdatatabeller och olika principer är tillgängliga för alla typer av aktiviteter. De egenskaper som är tillgängliga i hello typeProperties avsnittet varierar beroende på varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

För Kopieringsaktiviteten när hello mottagare är av typen hello **AzureSearchIndexSink**, hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Anger om toomerge eller ersätta när ett dokument det redan finns i hello index. Se hello [WriteBehavior egenskapen](#writebehavior-property).| sammanfoga (standard)<br/>Ladda upp| Nej |
| writeBatchSize | Överför data till hello Azure Search index när hello buffertstorlek når writeBatchSize. Se hello [WriteBatchSize egenskapen](#writebatchsize-property) mer information. | 1 too1 000. Standardvärdet är 1000. | Nej |

### <a name="writebehavior-property"></a>Egenskapen WriteBehavior
AzureSearchSink upserts när data skrivs. När du skriver ett dokument, om hello dokumentnyckeln finns redan i hello Azure Search index, uppdaterar Azure Search med andra ord hello befintligt dokument i stället för att en konflikt undantag.

Hej AzureSearchSink innehåller hello följande två upsert beteenden (med hjälp av AzureSearch SDK):

- **Sammanfoga**: kombinera alla hello kolumner i hello nytt dokument med hello befintliga en. Hej värdet i hello befintliga en bevaras i kolumner med null-värde i hello nytt dokument.
- **Överför**: hello nya dokument ersätter hello befintlig. För kolumner som inte har angetts i hello nytt dokument värdet hello toonull om det finns ett icke-null-värde i hello befintligt dokument eller inte.

hello standardbeteendet är **sammanfoga**.

### <a name="writebatchsize-property"></a>Egenskapen WriteBatchSize
Azure Search-tjänsten stöder skrivning dokument som en batch. En grupp kan innehålla 1 too1, 000 åtgärder. En åtgärd som hanterar ett dokument tooperform hello överför/merge-operation.

### <a name="data-type-support"></a>Stöd för datatypen
hello anger följande tabell om en Azure Search-datatyp stöds eller inte.

| Azure Search-datatyp | Stöds i Azure Search Sink |
| ---------------------- | ------------------------------ |
| Sträng | Y |
| Int32 | Y |
| Int64 | Y |
| dubbla | Y |
| Booleskt värde | Y |
| DataTimeOffset | Y |
| Strängmatris | N |
| GeographyPoint | N |

## <a name="json-example-copy-data-from-on-premises-sql-server-tooazure-search-index"></a>JSON-exempel: kopiera data från lokala SQL Server tooAzure sökindex

följande exempel visar hello:

1.  En länkad tjänst av typen [AzureSearch](#linked-service-properties).
2.  En länkad tjänst av typen [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).
3.  Indata [dataset](data-factory-create-datasets.md) av typen [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
4.  Utdata [dataset](data-factory-create-datasets.md) av typen [AzureSearchIndex](#dataset-properties).
4.  En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) och [AzureSearchIndexSink](#copy-activity-properties).

hello exemplet kopierar time series-data från en lokal SQL Server-databasen tooan Azure Search index varje timme. hello JSON egenskaper som används i det här exemplet beskrivs i hello-exempel i följande avsnitt.

Konfigurera hello data management gateway på din lokala dator som ett första steg. hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**Azure Search länkad tjänst:**

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

**SQL Server som är länkad tjänst**

```JSON
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```

**SQL Server inkommande dataset**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i SQL Server och den innehåller en kolumn med namnet ”timestampcolumn” för tid series-data. Du kan fråga över flera tabeller i samma databas som använder en enda dataset, men en enskild tabell måste användas för hello dataset tableName typeProperty hello.

Inställningen ”externa”: ”true” informerar Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "SqlServerDataset",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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

**Azure Search utdatauppsättningen:**

hello exempel kopior data tooan Azure Search index med namnet **produkter**. Data Factory skapar inte hello index. tootest hello exempel, skapa ett index med det här namnet. Skapa hello Azure Search index med hello samma antal kolumner som hello inkommande dataset. Nya poster läggs toohello Azure Search index varje timme.

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

**Kopiera aktivitet i en pipeline med SQL-källa och mottagare för Azure Search Index:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource** och **sink** typ har angetts för**AzureSearchIndexSink**. hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

Om du vill kopiera data från ett moln-datalager i Azure Search `executionLocation` egenskapen måste anges. hello följande JSON utdrag visar hello ändring behövs under Kopieringsaktiviteten `typeProperties` som exempel. Kontrollera [kopiera data mellan moln datalager](data-factory-data-movement-activities.md#global) avsnitt för mer information och de värden som stöds.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a>Kopiera från en källa för molnet
Om du vill kopiera data från ett moln-datalager i Azure Search `executionLocation` egenskapen måste anges. hello följande JSON utdrag visar hello ändring behövs under Kopieringsaktiviteten `typeProperties` som exempel. Kontrollera [kopiera data mellan moln datalager](data-factory-data-movement-activities.md#global) avsnitt för mer information och de värden som stöds.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

Du kan också mappa kolumner från källan dataset toocolumns från sink datauppsättning i aktivitetsdefinitionen för hello kopia. Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestanda- och justering  
Se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som effekten av dataflyttning (Kopieringsaktiviteten) och olika sätt toooptimize den.

## <a name="next-steps"></a>Nästa steg
Se hello följande artiklar:

* [Kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stegvisa instruktioner för att skapa en pipeline med en kopia-aktivitet.
