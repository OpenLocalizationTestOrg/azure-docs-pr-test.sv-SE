---
title: aaaMapping dataset kolumner i Azure Data Factory | Microsoft Docs
description: "Lär dig hur toomap datakällan kolumner toodestination kolumner."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 8f78d4af675bec0a70e5f6e83ec1ffb511408b5a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="map-source-dataset-columns-toodestination-dataset-columns"></a>Mappa dataset kolumner toodestination dataset källkolumner
Kolumnmappningen kan vara används toospecify hur kolumner som anges i hello källa tabell kartan toocolumns ”struktur” anges i hello ”struktur” sink-tabellen. Hej **columnMapping** egenskapen är tillgänglig i hello **typeProperties** avsnitt i hello kopieringsaktiviteten.

Kolumnmappningen stöder hello följande scenarier:

* Alla kolumner i datauppsättningsstrukturen för hello källa är mappade tooall kolumner i hello sink datauppsättningsstrukturen.
* En delmängd av hello kolumner i datauppsättningsstrukturen för hello källa är mappade tooall kolumner i hello sink datauppsättningsstrukturen.

hello följande är felvillkor som resulterar i ett undantag:

* Färre kolumner eller fler kolumner i hello ”struktur” sink tabell än anges i hello mappning.
* Duplicera mappning.
* Resultat av SQL-fråga har inte ett kolumnnamn som anges i hello mappning.

> [!NOTE]
> hello följande exempel är för Azure SQL och Azure Blob men tillämpliga tooany datalager som har stöd för rektangulär datauppsättningar. Justera dataset och länkade tjänstdefinitioner i exempel toopoint toodata i hello relevanta datakällan.

## <a name="sample-1--column-mapping-from-azure-sql-tooazure-blob"></a>Exempel 1 – kolumnen från Azure SQL tooAzure blob
I det här exemplet hello indatatabellen har en struktur och den tooa SQL-tabellen i en Azure SQL database.

```json
{
    "name": "AzureSQLInput",
    "properties": {
        "structure": 
         [
           { "name": "userid"},
           { "name": "name"},
           { "name": "group"}
         ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

I det här exemplet hello utdatatabell har en struktur och den pekar tooa blob i ett Azure blob storage.

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
         "structure": 
          [
                { "name": "myuserid"},
                { "name": "myname" },
                { "name": "mygroup"}
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
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

hello följande JSON definierar en kopia aktivitet i en pipeline. hello kolumner från källan mappas toocolumns i kanalmottagare (**columnMappings**) med hjälp av hello **översättare** egenskapen.

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "Copy",
    "inputs":  [ { "name": "AzureSQLInput"  } ],
    "outputs":  [ { "name": "AzureBlobOutput" } ],
    "typeProperties":    {
        "source":
        {
            "type": "SqlSource"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    },
   "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
**Kolumnen mappning flöde:**

![Kolumnen mappning flöde](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-tooazure-blob"></a>Exempel 2 – kolumnen mappning med SQL-frågan från Azure SQL tooAzure blob
I det här exemplet är en SQL-fråga används tooextract data från Azure SQL i stället för att ange hello tabellnamn, kolumnnamn med hello i avsnittet ”struktur”. 

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "CopyActivity",
    "inputs":  [ { "name": " AzureSQLInput"  } ],
    "outputs":  [ { "name": " AzureBlobOutput" } ],
    "typeProperties":
    {
        "source":
        {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "Translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
        }
    },
    "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
I det här fallet är hello frågeresultat första mappade toocolumns som anges i ”struktur” i datakällan. Därefter är hello kolumner från källan ”struktur” mappade toocolumns i kanalmottagare ”struktur” med regler som angetts i columnMappings.  Anta att hello frågan returnerar 5 kolumner, två fler kolumner än de som anges i hello ”struktur” i datakällan.

**Kolumnen mappning flöde**

![Kolumnen mappning flödet-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Nästa steg
Se hello artikeln en genomgång om hur du använder Kopieringsaktiviteten: 

- [Kopiera data från Blob Storage tooSQL databas](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
