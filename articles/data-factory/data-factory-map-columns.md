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
# <a name="map-source-dataset-columns-toodestination-dataset-columns"></a><span data-ttu-id="72204-103">Mappa dataset kolumner toodestination dataset källkolumner</span><span class="sxs-lookup"><span data-stu-id="72204-103">Map source dataset columns toodestination dataset columns</span></span>
<span data-ttu-id="72204-104">Kolumnmappningen kan vara används toospecify hur kolumner som anges i hello källa tabell kartan toocolumns ”struktur” anges i hello ”struktur” sink-tabellen.</span><span class="sxs-lookup"><span data-stu-id="72204-104">Column mapping can be used toospecify how columns specified in hello “structure” of source table map toocolumns specified in hello “structure” of sink table.</span></span> <span data-ttu-id="72204-105">Hej **columnMapping** egenskapen är tillgänglig i hello **typeProperties** avsnitt i hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="72204-105">hello **columnMapping** property is available in hello **typeProperties** section of hello Copy activity.</span></span>

<span data-ttu-id="72204-106">Kolumnmappningen stöder hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="72204-106">Column mapping supports hello following scenarios:</span></span>

* <span data-ttu-id="72204-107">Alla kolumner i datauppsättningsstrukturen för hello källa är mappade tooall kolumner i hello sink datauppsättningsstrukturen.</span><span class="sxs-lookup"><span data-stu-id="72204-107">All columns in hello source dataset structure are mapped tooall columns in hello sink dataset structure.</span></span>
* <span data-ttu-id="72204-108">En delmängd av hello kolumner i datauppsättningsstrukturen för hello källa är mappade tooall kolumner i hello sink datauppsättningsstrukturen.</span><span class="sxs-lookup"><span data-stu-id="72204-108">A subset of hello columns in hello source dataset structure is mapped tooall columns in hello sink dataset structure.</span></span>

<span data-ttu-id="72204-109">hello följande är felvillkor som resulterar i ett undantag:</span><span class="sxs-lookup"><span data-stu-id="72204-109">hello following are error conditions that result in an exception:</span></span>

* <span data-ttu-id="72204-110">Färre kolumner eller fler kolumner i hello ”struktur” sink tabell än anges i hello mappning.</span><span class="sxs-lookup"><span data-stu-id="72204-110">Either fewer columns or more columns in hello “structure” of sink table than specified in hello mapping.</span></span>
* <span data-ttu-id="72204-111">Duplicera mappning.</span><span class="sxs-lookup"><span data-stu-id="72204-111">Duplicate mapping.</span></span>
* <span data-ttu-id="72204-112">Resultat av SQL-fråga har inte ett kolumnnamn som anges i hello mappning.</span><span class="sxs-lookup"><span data-stu-id="72204-112">SQL query result does not have a column name that is specified in hello mapping.</span></span>

> [!NOTE]
> <span data-ttu-id="72204-113">hello följande exempel är för Azure SQL och Azure Blob men tillämpliga tooany datalager som har stöd för rektangulär datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="72204-113">hello following samples are for Azure SQL and Azure Blob but are applicable tooany data store that supports rectangular datasets.</span></span> <span data-ttu-id="72204-114">Justera dataset och länkade tjänstdefinitioner i exempel toopoint toodata i hello relevanta datakällan.</span><span class="sxs-lookup"><span data-stu-id="72204-114">Adjust dataset and linked service definitions in examples toopoint toodata in hello relevant data source.</span></span>

## <a name="sample-1--column-mapping-from-azure-sql-tooazure-blob"></a><span data-ttu-id="72204-115">Exempel 1 – kolumnen från Azure SQL tooAzure blob</span><span class="sxs-lookup"><span data-stu-id="72204-115">Sample 1 – column mapping from Azure SQL tooAzure blob</span></span>
<span data-ttu-id="72204-116">I det här exemplet hello indatatabellen har en struktur och den tooa SQL-tabellen i en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="72204-116">In this sample, hello input table has a structure and it points tooa SQL table in an Azure SQL database.</span></span>

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

<span data-ttu-id="72204-117">I det här exemplet hello utdatatabell har en struktur och den pekar tooa blob i ett Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="72204-117">In this sample, hello output table has a structure and it points tooa blob in an Azure blob storage.</span></span>

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

<span data-ttu-id="72204-118">hello följande JSON definierar en kopia aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="72204-118">hello following JSON defines a copy activity in a pipeline.</span></span> <span data-ttu-id="72204-119">hello kolumner från källan mappas toocolumns i kanalmottagare (**columnMappings**) med hjälp av hello **översättare** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="72204-119">hello columns from source mapped toocolumns in sink (**columnMappings**) by using hello **Translator** property.</span></span>

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
<span data-ttu-id="72204-120">**Kolumnen mappning flöde:**</span><span class="sxs-lookup"><span data-stu-id="72204-120">**Column mapping flow:**</span></span>

![Kolumnen mappning flöde](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-tooazure-blob"></a><span data-ttu-id="72204-122">Exempel 2 – kolumnen mappning med SQL-frågan från Azure SQL tooAzure blob</span><span class="sxs-lookup"><span data-stu-id="72204-122">Sample 2 – column mapping with SQL query from Azure SQL tooAzure blob</span></span>
<span data-ttu-id="72204-123">I det här exemplet är en SQL-fråga används tooextract data från Azure SQL i stället för att ange hello tabellnamn, kolumnnamn med hello i avsnittet ”struktur”.</span><span class="sxs-lookup"><span data-stu-id="72204-123">In this sample, a SQL query is used tooextract data from Azure SQL instead of simply specifying hello table name and hello column names in “structure” section.</span></span> 

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
<span data-ttu-id="72204-124">I det här fallet är hello frågeresultat första mappade toocolumns som anges i ”struktur” i datakällan.</span><span class="sxs-lookup"><span data-stu-id="72204-124">In this case, hello query results are first mapped toocolumns specified in “structure” of source.</span></span> <span data-ttu-id="72204-125">Därefter är hello kolumner från källan ”struktur” mappade toocolumns i kanalmottagare ”struktur” med regler som angetts i columnMappings.</span><span class="sxs-lookup"><span data-stu-id="72204-125">Next, hello columns from source “structure” are mapped toocolumns in sink “structure” with rules specified in columnMappings.</span></span>  <span data-ttu-id="72204-126">Anta att hello frågan returnerar 5 kolumner, två fler kolumner än de som anges i hello ”struktur” i datakällan.</span><span class="sxs-lookup"><span data-stu-id="72204-126">Suppose hello query returns 5 columns, two more columns than those specified in hello “structure” of source.</span></span>

<span data-ttu-id="72204-127">**Kolumnen mappning flöde**</span><span class="sxs-lookup"><span data-stu-id="72204-127">**Column mapping flow**</span></span>

![Kolumnen mappning flödet-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a><span data-ttu-id="72204-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72204-129">Next steps</span></span>
<span data-ttu-id="72204-130">Se hello artikeln en genomgång om hur du använder Kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="72204-130">See hello article for a tutorial on using Copy Activity:</span></span> 

- [<span data-ttu-id="72204-131">Kopiera data från Blob Storage tooSQL databas</span><span class="sxs-lookup"><span data-stu-id="72204-131">Copy data from Blob Storage tooSQL Database</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
