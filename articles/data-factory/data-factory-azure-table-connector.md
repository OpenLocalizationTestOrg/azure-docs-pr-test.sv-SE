---
title: "aaaMove data till/från Azure Table | Microsoft Docs"
description: "Lär dig hur toomove data till/från Azure Table Storage med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 3dc3da6d88854674a9108b600534bc5d07575f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-table-using-azure-data-factory"></a>Flytta data tooand från Azure-tabellen med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till och från Azure Table Storage. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten. 

Du kan kopiera data från en stöds källa data tooAzure tabellagring eller från Azure Table Storage tooany stöds sink data. En lista över datakällor som stöds som datakällor eller sänkor av hello kopieringsaktiviteten finns hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. 

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från en Azure Table Storage med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en Azure-tabellagring finns [JSON-exempel](#json-examples) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure Table Storage: 

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
Det finns två typer av länkade tjänster kan du använda toolink ett Azure blob storage tooan Azure data factory. De är: **AzureStorage** länkade tjänsten och **AzureStorageSas** länkade tjänsten. hello länkad Azure Storage-tjänst ger hello data factory med global åtkomst toohello Azure Storage. Medan hello Azure Storage SAS (signatur för delad åtkomst) länkad ger tjänst hello data factory med begränsad/Tidsbundna åtkomst toohello Azure Storage. Det finns några skillnader mellan dessa två länkade tjänster. Välj hello länkade tjänst som passar dina behov. hello följande avsnitt innehåller mer information om dessa två länkade tjänster.

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej **typeProperties** avsnittet för hello dataset av typen **AzureTable** har hello följande egenskaper.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello Azure Table-databasinstansen som den länkade tjänsten refererar till. |Ja. När ett tabellnamn har angetts utan ett azureTableSourceQuery, är alla poster från hello tabell kopierade toohello mål. Om en azureTableSourceQuery också anges är poster från hello-tabell som uppfyller frågan hello kopierade toohello mål. |

### <a name="schema-by-data-factory"></a>Schema som Data Factory
För schemafria data lagras till exempel Azure Table skapar hello Data Factory-tjänsten hello schema i något av följande sätt hello:

1. Om du anger hello strukturen för data med hjälp av hello **struktur** egenskap i hello Data Factory-tjänsten i datauppsättningsdefinitionen hello godkänner den här strukturen som hello schema. I det här fallet ges en rad inte innehåller ett värde för en kolumn, ett null-värde för den.
2. Om du inte anger hello strukturen för data med hjälp av hello **struktur** egenskap i hello datauppsättningsdefinitionen, Data Factory skapar hello schema med hjälp av hello första raden i hello data. I det här fallet missas hello första raden inte innehåller hello fullständig schemat, vissa kolumner i hello resultatet av kopieringsåtgärden.

Därför är hello bästa praxis för schemafria datakällor toospecify hello struktur data med hjälp av hello **struktur** egenskapen.

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, indata och utdata-datauppsättningar och -principer är tillgängliga för alla typer av aktiviteter.

Egenskaper i hello typeProperties avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

**AzureTableSource** stöder hello följande egenskaper i typeProperties avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| azureTableSourceQuery |Använda hello anpassad fråga tooread data. |Azure-tabellen frågesträngen. Se exemplen i hello nästa avsnitt. |Nej. När ett tabellnamn har angetts utan ett azureTableSourceQuery, är alla poster från hello tabell kopierade toohello mål. Om en azureTableSourceQuery också anges är poster från hello-tabell som uppfyller frågan hello kopierade toohello mål. |
| azureTableSourceIgnoreTableNotFound |Ange om swallow hello undantag av tabellen inte finns. |SANT<br/>FALSKT |Nej |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery-exempel
Om Azure Table kolumnen är av strängtypen:

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

Om Azure Table kolumnen är av typen datetime:

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

**AzureTableSink** stöder hello följande egenskaper i typeProperties avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Standard partitionsnyckelvärde som kan användas av hello mottagare. |Ett strängvärde. |Nej |
| azureTablePartitionKeyName |Ange namnet på hello kolumn vars värden används som partitionsnycklar. Om inget anges används AzureTableDefaultPartitionKeyValue som hello partitionsnyckel. |Ett kolumnnamn. |Nej |
| azureTableRowKeyName |Ange namnet på hello kolumn vars kolumnvärdena används som radnyckel. Om inget annat anges, kan du använda ett GUID för varje rad. |Ett kolumnnamn. |Nej |
| azureTableInsertType |hello läge tooinsert data till Azure-tabellen.<br/><br/>Den här egenskapen anger om befintliga rader i hello utdatatabell med matchande partition och radnycklar få sina värden bytas ut eller samman. <br/><br/>toolearn om hur dessa inställningar (dokument och Ersätt) fungerar, se [Insert- eller Merge-entiteten](https://msdn.microsoft.com/library/azure/hh452241.aspx) och [infoga eller ersätta entiteten](https://msdn.microsoft.com/library/azure/hh452242.aspx) avsnitt. <br/><br> Den här inställningen gäller vid hello raden nivån, inte hello tabell nivån, varken alternativet tar bort rader i hello utdatatabell som inte finns i hello indata. |sammanfoga (standard)<br/>Ersätt |Nej |
| writeBatchSize |Infogar data i hello Azure-tabellen när hello writeBatchSize eller writeBatchTimeout namn. |Heltal (antalet rader) |Nej (standard: 10000) |
| writeBatchTimeout |Infogar data i hello Azure-tabellen när hello writeBatchSize eller writeBatchTimeout namn |TimeSpan<br/><br/>Exempel ”: 00: 20:00” (20 minuter) |Nej (standard toostorage klienten standardtimeout-värdet 90 sek) |

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Mappa en kolumn tooa mål källkolumn använda hello översättare JSON-egenskapen innan du kan använda hello målkolumnen som hello azureTablePartitionKeyName.

I följande exempel hello, källkolumnen DivisionID är mappade toohello målkolumnen: DivisionID.  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
Hej DivisionID har angetts som hello partitionsnyckel.

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a>JSON-exempel
hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data tooand från Azure Table Storage och Azure Blob-databas. Dock datan kan kopieras **direkt** från någon av hello källor tooany av sänkor hello stöds. Mer information finns i avsnittet hello ”stöds datalager och format” i [flytta data med hjälp av Kopieringsaktiviteten](data-factory-data-movement-activities.md).

## <a name="example-copy-data-from-azure-table-tooazure-blob"></a>Exempel: Kopiera data från Azure Table tooAzure Blob
följande exempel visar hello:

1. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (används för blob & tabell).
2. Indata [dataset](data-factory-create-datasets.md) av typen [AzureTable](#dataset-properties).
3. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Hej [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [AzureTableSource](#activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data som tillhör toohello standardpartition i en Azure Table tooa blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Länkad Azure storage-tjänst:**

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
Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**. För hello första, anger du hello anslutningssträng som innehåller hello kontonyckel och för hello senare, anger du hello delade signatur åtkomst (SAS)-Uri. Se [länkade tjänster](#linked-service-properties) information.  

**Azure Table inkommande datamängd:**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure Table.

Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**Azure Blob utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopiera aktivitet i en pipeline med AzureTableSource och BlobSink:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**AzureTableSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som anges med **AzureTableSourceQuery** egenskapen väljer hello data från hello standardpartition toocopy varje timme.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                      {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                      {
                            "name": "AzureBlobOutput"
                      }
                ],
                "typeProperties": {
                      "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="example-copy-data-from-azure-blob-tooazure-table"></a>Exempel: Kopiera data från Azure Blob tooAzure tabell
följande exempel visar hello:

1. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (används för blob & tabell)
2. Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
3. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureTable](#dataset-properties).
4. Hej [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [AzureTableSink](#copy-activity-properties).

hello exemplet kopierar time series-data från ett Azure blob-tooan Azure-tabellen varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Länkad Azure storage (för både Azure Table & Blob)-tjänst:**

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

Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**. För hello första, anger du hello anslutningssträng som innehåller hello kontonyckel och för hello senare, anger du hello delade signatur åtkomst (SAS)-Uri. Se [länkade tjänster](#linked-service-properties) information.

**Azure Blob inkommande datamängd:**

Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid. ”externa”: ”true” inställningen informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Azure-tabellen utdatauppsättningen:**

hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i Azure Table. Skapa en Azure-tabell med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain. Nya rader läggs toohello tabell varje timme.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**Kopiera aktivitet i en pipeline med BlobSource och AzureTableSink:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**AzureTableSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
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
## <a name="type-mapping-for-azure-table"></a>Mappning för Azure-tabellen
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i två steg.

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data & från Azure Table, hello följande [mappningar som definierats av Azure Table-tjänsten](https://msdn.microsoft.com/library/azure/dd179338.aspx) används från Azure Table OData typer too.NET typ och vice versa.

| OData-datatyp | .NET-typ | Information |
| --- | --- | --- |
| Edm.Binary |byte] |En matris med byte in too64 KB. |
| Edm.Boolean |bool |Ett booleskt värde. |
| Edm.DateTime |Datum och tid |En 64-bitars värdet uttrycks som Coordinated Universal Time (UTC). hello stöds DateTime intervallet börjar från midnatt, 1 januari, 1601 e. kr. (C.E.) UTC. hello intervallet slutar vid den 31 December 9999. |
| Edm.Double |dubbla |En 64-bitars flytande punktvärdet. |
| Edm.Guid |GUID |En 128-bitars globalt unik identifierare. |
| Edm.Int32 |Int32 |En 32-bitars heltal. |
| Edm.Int64 |Int64 |En 64-bitars heltal. |
| Edm.String |Sträng |Ett värde för UTF-16-kodad. Strängvärden kanske in too64 KB. |

### <a name="type-conversion-sample"></a>Exempel konvertering
följande exempel hello är för att kopiera data från Azure Blob-tooAzure tabell med typkonverteringar.

Anta att hello-blobbdatauppsättning är CSV-format och innehåller tre kolumner. En av dem är ett datetime-kolumn med en anpassad datetime-format med hjälp av förkortade franska namn för hello veckodag.

Definiera hello Blob källa dataset enligt följande tillsammans med typdefinitioner för hello kolumner.

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
         "structure":
          [
                { "name": "userid", "type": "Int64"},
                { "name": "name", "type": "String"},
                { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
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
Hello mappning från Azure Table OData too.NET typ får definierar du hello tabell i Azure-tabellen med hello följer schemat.

**Azure tabellschemat:**

| Kolumnnamn | Typ |
| --- | --- |
| användar-ID |Edm.Int64 |
| namn |Edm.String |
| lastlogindate |Edm.DateTime |

Därefter definiera hello Azure Table dataset. Du behöver inte toospecify ”struktur” avsnitt med information om hello eftersom hello typinformation har redan angetts i hello underliggande datalagret.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

I det här fallet Data Factory Skriv automatiskt konverteringar inklusive hello Datetime-fält med hello anpassade datetime-format med hjälp av hello ”fr-fr” kulturen när du flyttar data från Blob tooAzure tabell.

> [!NOTE]
> toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestanda och finjustering
toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize, se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md).
