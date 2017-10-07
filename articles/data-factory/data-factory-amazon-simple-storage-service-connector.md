---
title: "aaaMove data från Amazon enkla Storage-tjänsten med hjälp av Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från Amazon enkla lagringstjänst (S3) med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 8a8cd2845fd1de74413bd0372f3aabfb4817549b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Flytta data från Amazon enkla Storage-tjänsten med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello kopieringsaktiviteten i Azure Data Factory toomove data från Amazon enkla lagringstjänst (S3). Den bygger på hello [Data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från Amazon S3 tooany stöds sink-datalagret. En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Data Factory stöder för närvarande endast flytta data från Amazon S3 tooother datalager, men inte flytta data från andra data lagrar tooAmazon S3.

## <a name="required-permissions"></a>Behörigheter som krävs
toocopy data från Amazon S3, kontrollera att du har beviljats hello följande behörigheter:

* `s3:GetObject`och `s3:GetObjectVersion` för Amazon S3 objekt åtgärder.
* `s3:ListBucket`för Amazon S3 Bucket-åtgärder. Om du använder hello Data Factory kopiera guiden `s3:ListAllMyBuckets` krävs också.

Mer information om hello fullständig lista över Amazon S3 behörigheter finns [att ange behörigheter i en princip](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för Amazon S3 med hjälp av olika verktyg eller API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. För en snabb genomgång, se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Stegvisa instruktioner toocreate en pipeline med en kopieringsaktiviteten finns hello [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Om du använder verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder verktyg eller API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format. Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för Amazon S3 finns hello [JSON-exempel: kopiera data från Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) i den här artikeln.

> [!NOTE]
> Mer information om fil- och komprimering de format som stöds för en kopieringsaktiviteten finns [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAmazon S3.

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
En länkad tjänst länkar en data store tooa data factory. Du skapar en länkad tjänst av typen **AwsAccessKey** toolink Amazon S3 data lagra tooyour data factory. hello i den följande tabellen finns beskrivning för JSON-element specifika tooAmazon S3 (AwsAccessKey) länkad tjänst.

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| accessKeyID |ID för hello hemliga snabbtangent. |Sträng |Ja |
| secretAccessKey |hello hemliga åtkomst själva nyckeln. |Krypterad hemliga sträng |Ja |

Här är ett exempel:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd
toospecify en dataset toorepresent mata in data i Azure Blob storage, ange hello type-egenskapen för hello dataset för**AmazonS3**. Ange hello **linkedServiceName** -egenskapen för hello dataset toohello namnet på hello Amazon S3 länkade tjänsten. En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md). 

Avsnitt som struktur, tillgänglighet och principen är liknande för alla dataset-typer (till exempel SQL-databas, Azure blob och Azure-tabellen). Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej **typeProperties** avsnittet för en dataset av typen **AmazonS3** (som omfattar hello Amazon S3 dataset) har hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| bucketName |hello S3 bucket-namn. |Sträng |Ja |
| key |hello S3 Objektnyckel. |Sträng |Nej |
| prefix |Prefix för hello S3 objekt nyckeln. Objekt vars nycklar som börjar med prefixet är markerade. Gäller endast när nyckeln är tom. |Sträng |Nej |
| Version |hello version av hello S3 objekt, om S3 versionshantering är aktiverat. |Sträng |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i hello [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv format ](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill toocopy filer som-mellan filbaserade butiker (binär kopia), hoppa över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej | |
| Komprimering | Ange hello typ och kompression för hello data. hello stöds typer är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. hello som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej | |


> [!NOTE]
> **bucketName + nyckeln** anger hello platsen för hello S3 objekt, där bucket är hello Rotbehållare för S3 objekt och nyckeln är hello fullständig sökväg toohello S3 objekt.

### <a name="sample-dataset-with-prefix"></a>Exempeldatamängd med prefix

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-dataset-with-version"></a>Exempeldatamängd (med version)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a>Dynamisk sökvägar för S3
hello föregående exempel använder fasta värden för hello **nyckeln** och **bucketName** egenskaper i hello Amazon S3 dataset.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

Du kan ha Data Factory beräkna egenskaperna dynamiskt vid körning, med hjälp av systemvariabler som SliceStart.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Du kan göra hello samma för hello **prefix** -egenskapen för en Amazon S3 datauppsättning. En lista över funktioner som stöds och variabler, se [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md).

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter, se [skapar pipelines](data-factory-create-pipelines.md). Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter. Egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp. För hello kopieringsaktiviteten varierar egenskaper beroende på hello typer av datakällor och sänkor. När en källa i hello kopieringsaktiviteten är av typen **FileSystemSource** (som omfattar Amazon S3), hello efter egenskapen finns i **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om toorecursively lista S3 objekt under hello katalog. |SANT/FALSKT |Nej |

## <a name="json-example-copy-data-from-amazon-s3-tooazure-blob-storage"></a>JSON-exempel: kopiera data från Amazon S3 tooAzure Blob storage
Det här exemplet visas hur toocopy data från Amazon S3 tooan Azure Blob storage. Dock datan kan kopieras direkt för[någon hello sänkor som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello kopieringsaktiviteten i Data Factory.

hello exempel innehåller JSON definitioner för hello följande Data Factory entiteter. Du kan använda dessa definitioner toocreate pipeline toocopy data från Amazon S3 tooBlob lagring med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).   

* En länkad tjänst av typen [AwsAccessKey](#linked-service-properties).
* En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Indata [dataset](data-factory-create-datasets.md) av typen [AmazonS3](#dataset-properties).
* Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från Amazon S3 tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

### <a name="amazon-s3-linked-service"></a>Amazon S3 länkad tjänst

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="amazon-s3-input-dataset"></a>Amazon S3 inkommande dataset

Ange **”externa”: true** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory. Ange den här egenskapen tootrue på ett inkommande datamängd som inte tillverkas av en aktivitet i hello pipeline.

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


### <a name="azure-blob-output-dataset"></a>Utdatauppsättning för Azure-blobb

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder hello år, månad, dag och timmar delar av hello starttid.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a>Kopiera aktivitet i en pipeline med en källa för Amazon S3 och en blob-mottagare

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource**, och **sink** typ har angetts för**BlobSink**.

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> toomap kolumner från en källa dataset toocolumns från en mottagare datamängd finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).


## <a name="next-steps"></a>Nästa steg
Se hello följande artiklar:

* toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (kopieringsaktiviteten) i Data Factory och olika sätt toooptimize, se hello [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md).

* Stegvisa instruktioner för att skapa en pipeline med en kopieringsaktiviteten finns hello [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
