---
title: "aaaMove data till/från Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur du ska flytta data till/från Azure Cosmos DB samlingen med hjälp av Azure Data Factory"
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: bd23ce4e004a972ce6f3e4165cfdea4f0c18fecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-cosmos-db-using-azure-data-factory"></a>Flytta data tooand från Azure Cosmos-databasen med Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till och från Azure Cosmos DB (DocumentDB-API). Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten. 

Du kan kopiera data från en stöds källa data tooAzure Cosmos DB eller från Azure Cosmos DB tooany stöds sink data. En lista över datakällor som stöds som datakällor eller sänkor av hello kopieringsaktiviteten finns hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. 

> [!IMPORTANT]
> Azure DB Cosmos-anslutningen har endast stöd för DocumentDB-API.

toocopy data som-är till/från JSON-filer eller en annan Cosmos DB samling finns [Import/Export JSON-dokument](#importexport-json-documents).

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från Azure Cosmos DB med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från Cosmos DB finns [JSON-exempel](#json-examples) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooCosmos DB: 

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell ger en beskrivning för JSON-element specifika tooAzure Cosmos DB länkade tjänsten.

| **Egenskap** | **Beskrivning** | **Krävs** |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **DocumentDb** |Ja |
| connectionString |Ange information som behövs för tooconnect tooAzure Cosmos-DB-databas. |Ja |

Exempel:

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns toohello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för hello dataset av typen **DocumentDbCollection** har hello följande egenskaper.

| **Egenskap** | **Beskrivning** | **Krävs** |
| --- | --- | --- |
| Samlingsnamn |Namnet på hello Cosmos DB dokumentsamlingen. |Ja |

Exempel:

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a>Schema som Data Factory
För schemafria data butiker, till exempel Azure Cosmos DB skapar hello Data Factory-tjänsten hello schema i något av följande sätt hello:  

1. Om du anger hello strukturen för data med hjälp av hello **struktur** egenskap i hello Data Factory-tjänsten i datauppsättningsdefinitionen hello godkänner den här strukturen som hello schema. Om en rad inte innehåller ett värde för en kolumn, tillhandahålls i det här fallet ett null-värde för den.
2. Om du inte anger hello strukturen för data med hjälp av hello **struktur** egenskap i hello datauppsättningsdefinitionen, hello Data Factory-tjänsten skapar hello schema med hjälp av hello första raden i hello data. I det här fallet om hello första raden inte innehåller fullständig hello-schemat, kommer vissa kolumner att saknas i hello resultatet av kopieringsåtgärden.

Därför är hello bästa praxis för schemafria datakällor toospecify hello struktur data med hjälp av hello **struktur** egenskapen.

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns toohello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

> [!NOTE]
> Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.

Egenskaper i hello typeProperties avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp och vid kopieringsaktiviteten de varierar beroende på hello typer av datakällor och sänkor.

Vid kopieringsaktiviteten när datakällan är av typen **DocumentDbCollectionSource** hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:

| **Egenskap** | **Beskrivning** | **Tillåtna värden** | **Krävs** |
| --- | --- | --- | --- |
| DocumentDB |Ange hello frågan tooread data. |Frågesträng stöds av Azure Cosmos DB. <br/><br/>Exempel:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nej <br/><br/>Om inget anges hello SQL-instruktionen som körs:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Specialtecken tooindicate som hello dokumentet är kapslad |Valfritt tecken. <br/><br/>Azure Cosmos-DB är en NoSQL store för JSON-dokument, där kapslade strukturer är tillåtna. Azure Data Factory aktiverar toodenote användarhierarkin via nestingSeparator, vilket är ””. i hello exemplen ovan. Med hello avgränsare hello kopieringsaktiviteten genererar hello ”Name”-objekt med tre underordnade element första mellersta och sista, bl.a too"Name.First”, ”Name.Middle” och ”Name.Last” i hello tabell definition. |Nej |

**DocumentDbCollectionSink** stöder hello följande egenskaper:

| **Egenskap** | **Beskrivning** | **Tillåtna värden** | **Krävs** |
| --- | --- | --- | --- |
| nestingSeparator |Det krävs ett specialtecken i hello källa kolumnen namn tooindicate som kapslade dokument. <br/><br/>Till exempel ovan: `Name.First` i hello utdata producerar tabellen hello följande JSON-strukturen i hello Cosmos DB dokument:<br/><br/>”Name”: {<br/>    ”Första”: ”John”<br/>}, |Tecken som används tooseparate kapslingsnivåer.<br/><br/>Standardvärdet är `.` (punkt). |Tecken som används tooseparate kapslingsnivåer. <br/><br/>Standardvärdet är `.` (punkt). |
| writeBatchSize |Antalet parallella begäranden tooAzure Cosmos DB toocreate servicedokument.<br/><br/>Du kan finjustera hello prestanda vid kopiering av data till och från Cosmos-databas med hjälp av den här egenskapen. Du kan förvänta dig bättre prestanda om du ökar writeBatchSize eftersom flera parallella begäranden tooCosmos DB skickas. Men du behöver tooavoid begränsning som kan utlösa hello felmeddelande: ”begär frekvensen är stor”.<br/><br/>Begränsning bestäms av ett antal faktorer, bland annat storlek dokument, antalet villkoren i dokument, indexering princip målsamling osv. Kopieringen, kan du använda en bättre samling (t.ex. S3) toohave hello de flesta genomströmning tillgänglig (2 500 begärande enheter per sekund). |Integer |Nej (standard: 5) |
| writeBatchTimeout |Vänta tills hello åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel ”: 00: 30:00” (30 minuter). |Nej |

## <a name="importexport-json-documents"></a>Importera och exportera JSON-dokument
Den här Cosmos-DB-anslutningen kan du enkelt

* Importera JSON-dokument från olika källor till Cosmos-DB, inklusive Azure Blob Azure Data Lake, lokalt filsystem eller andra filbaserade butiker som stöds av Azure Data Factory.
* Exportera JSON-dokument från Cosmos DB collecton till olika filbaserade butiker.
* Migrera data mellan två Cosmos DB samlingar som-är.

tooachieve sådana schema-oberoende kopiera, 
* När du använder guiden Kopiera Kontrollera hello **”exportera som-tooJSON filer eller Cosmos DB samlingen”** alternativet.
* När med JSON redigering inte anger hello ”struktur” avsnittet i Cosmos DB datauppsättning/ar eller egenskapen ”nestingSeparator” i Cosmos DB källor/mottagare i en Kopieringsaktivitet. tooimport från / exportera tooJSON filer, ange formattyp i hello filen store dataset som ”JsonFormat”, config ”filePattern” och hoppa över hello rest-formatinställningar, se [JSON-format](data-factory-supported-file-and-compression-formats.md#json-format) avsnittet detaljer.

## <a name="json-examples"></a>JSON-exempel
hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data tooand från Azure Cosmos DB och Azure Blob Storage. Dock datan kan kopieras **direkt** från någon av hello källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

## <a name="example-copy-data-from-azure-cosmos-db-tooazure-blob"></a>Exempel: Kopiera data från Azure Cosmos DB tooAzure Blob
hello exemplet nedan visar:

1. En länkad tjänst av typen [DocumentDb](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [DocumentDbCollection](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [DocumentDbCollectionSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data i Azure Cosmos DB tooAzure Blob. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Azure Cosmos-DB länkade tjänsten:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
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
**Azure dokumentet DB indatauppsättning:**

hello exemplet förutsätter att du har en samling med namnet **Person** i Azure DB som Cosmos-databasen.

Inställningen ”externa”: ”true” och ange externalData principinformation hello Azure Data Factory-tjänsten hello tabellen är externa toohello data factory och inte kommer från en aktivitet i hello data factory.

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Azure Blob utdatauppsättningen:**

Data är kopierade tooa nya blob varje timme med hello sökvägen för hello blob reflektion hello specifika datetime med timme granularitet.

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Exempel JSON-dokumentet i hello Person samling i en Cosmos-DB-databas:

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
Cosmos DB stöder förfrågningar till dokument med hjälp av en SQL som syntax över hierarkiska JSON-dokument.

Exempel: 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

hello följande pipeline kopierar data från hello Person samling i hello Azure Cosmos DB databasen tooan Azure blob. Som en del av hello kopiera aktivitet hello har inkommande och utgående datauppsättningar angetts.  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-tooazure-cosmos-db"></a>Exempel: Kopiera data från Azure Blob-tooAzure Cosmos DB 
hello exemplet nedan visar:

1. En länkad tjänst av typen [DocumentDb](#azure-documentdb-linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [DocumentDbCollection](#azure-documentdb-dataset-type-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).

hello exemplet kopierar data från Azure blob-tooAzure Cosmos DB. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

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
**Azure Cosmos-DB länkade tjänsten:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Azure Blob inkommande datamängd:**

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
**Azure Cosmos-DB utdatauppsättningen:**

hello exemplet kopierar tooa datainsamling med namnet ”Person”.

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
hello följande pipeline kopierar data från Azure Blob toohello Person samling i hello Cosmos DB. Som en del av hello kopiera aktivitet hello har inkommande och utgående datauppsättningar angetts.

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
Om hello Exempelindata blob som

```
1,John,,Doe
```
Hello utdata JSON i Cosmos DB blir som:

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
Azure Cosmos-DB är en NoSQL store för JSON-dokument, där kapslade strukturer är tillåtna. Azure Data Factory aktiverar toodenote användarhierarkin via **nestingSeparator**, vilket är ””. i det här exemplet. Med hello avgränsare hello kopieringsaktiviteten genererar hello ”Name”-objekt med tre underordnade element första mellersta och sista, bl.a too"Name.First”, ”Name.Middle” och ”Name.Last” i hello tabell definition.

## <a name="appendix"></a>Bilaga
1. **Fråga:** hello Kopieringsaktiviteten stöd för uppdatering av befintliga poster?

    **Svar:** Nej.
2. **Fråga:** hur redan har ett nytt försök till en kopia tooAzure Cosmos DB behandlar kopieras poster?

    **Svar:** om poster har ett ”ID”-fält och hello kopieringsåtgärden försöker tooinsert post med hello samma ID, hello kopieringsåtgärden genererar ett fel.  
3. **Fråga:** stöder Data Factory [intervall eller hash-baserad Datapartitionering](../documentdb/documentdb-partition-data.md)?

    **Svar:** Nej.
4. **Fråga:** kan jag ange mer än en Azure DB som Cosmos-samlingen för en tabell?

    **Svar:** Nej. Endast en samling kan anges just nu.

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
