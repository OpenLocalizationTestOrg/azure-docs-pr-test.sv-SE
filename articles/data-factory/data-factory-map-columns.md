---
title: Mappa dataset kolumner i Azure Data Factory | Microsoft Docs
description: "Lär dig hur du mappar källkolumner till mål-kolumner."
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
ms.openlocfilehash: a50661b377cfbbff3f1f762342cb275d5da82cea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a><span data-ttu-id="dd3fe-103">Mappa källkolumner dataset till mål-dataset kolumner</span><span class="sxs-lookup"><span data-stu-id="dd3fe-103">Map source dataset columns to destination dataset columns</span></span>
<span data-ttu-id="dd3fe-104">Kolumnmappningen kan användas för att ange hur kolumner som anges i ”strukturen” för mappning av källan till kolumner anges i ”struktur” sink-tabellen.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-104">Column mapping can be used to specify how columns specified in the “structure” of source table map to columns specified in the “structure” of sink table.</span></span> <span data-ttu-id="dd3fe-105">Den **columnMapping** egenskapen är tillgänglig i den **typeProperties** avsnittet för aktiviteten kopiera.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-105">The **columnMapping** property is available in the **typeProperties** section of the Copy activity.</span></span>

<span data-ttu-id="dd3fe-106">Kolumnmappningen har stöd för följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="dd3fe-106">Column mapping supports the following scenarios:</span></span>

* <span data-ttu-id="dd3fe-107">Alla kolumner i datauppsättning källstrukturen mappas till alla kolumner i datauppsättningsstrukturen mottagare.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-107">All columns in the source dataset structure are mapped to all columns in the sink dataset structure.</span></span>
* <span data-ttu-id="dd3fe-108">En delmängd med kolumner i datauppsättningsstrukturen källan mappas till alla kolumner i datauppsättningsstrukturen mottagare.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-108">A subset of the columns in the source dataset structure is mapped to all columns in the sink dataset structure.</span></span>

<span data-ttu-id="dd3fe-109">Följande är felvillkor som resulterar i ett undantag:</span><span class="sxs-lookup"><span data-stu-id="dd3fe-109">The following are error conditions that result in an exception:</span></span>

* <span data-ttu-id="dd3fe-110">Färre kolumner eller fler kolumner i ”struktur” sink tabell än anges i mappningen.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-110">Either fewer columns or more columns in the “structure” of sink table than specified in the mapping.</span></span>
* <span data-ttu-id="dd3fe-111">Duplicera mappning.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-111">Duplicate mapping.</span></span>
* <span data-ttu-id="dd3fe-112">Resultat av SQL-fråga har inte ett kolumnnamn som anges i mappningen.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-112">SQL query result does not have a column name that is specified in the mapping.</span></span>

> [!NOTE]
> <span data-ttu-id="dd3fe-113">Följande exempel gäller för alla datalager som har stöd för rektangulär datauppsättningar är för Azure SQL och Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-113">The following samples are for Azure SQL and Azure Blob but are applicable to any data store that supports rectangular datasets.</span></span> <span data-ttu-id="dd3fe-114">Justera dataset och länkade tjänstdefinitioner i exempel så att den pekar till data i den aktuella datakällan.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-114">Adjust dataset and linked service definitions in examples to point to data in the relevant data source.</span></span>

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a><span data-ttu-id="dd3fe-115">Exempel 1 – kolumnen mappning från Azure SQL till Azure-blob</span><span class="sxs-lookup"><span data-stu-id="dd3fe-115">Sample 1 – column mapping from Azure SQL to Azure blob</span></span>
<span data-ttu-id="dd3fe-116">I det här exemplet indatatabellen har en struktur och pekar på en SQLtabell i Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-116">In this sample, the input table has a structure and it points to a SQL table in an Azure SQL database.</span></span>

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

<span data-ttu-id="dd3fe-117">I det här exemplet utdatatabellen har en struktur och att den leder till en blobb i ett Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-117">In this sample, the output table has a structure and it points to a blob in an Azure blob storage.</span></span>

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

<span data-ttu-id="dd3fe-118">Följande JSON definierar en kopia aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-118">The following JSON defines a copy activity in a pipeline.</span></span> <span data-ttu-id="dd3fe-119">Kolumner från källan som är mappade till kolumnerna i kanalmottagare (**columnMappings**) med hjälp av den **översättare** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-119">The columns from source mapped to columns in sink (**columnMappings**) by using the **Translator** property.</span></span>

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
<span data-ttu-id="dd3fe-120">**Kolumnen mappning flöde:**</span><span class="sxs-lookup"><span data-stu-id="dd3fe-120">**Column mapping flow:**</span></span>

![Kolumnen mappning flöde](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a><span data-ttu-id="dd3fe-122">Exempel 2 – kolumnen mappning med SQL-frågan från Azure SQL till Azure-blob</span><span class="sxs-lookup"><span data-stu-id="dd3fe-122">Sample 2 – column mapping with SQL query from Azure SQL to Azure blob</span></span>
<span data-ttu-id="dd3fe-123">I det här exemplet används en SQL-fråga för att extrahera data från Azure SQL i stället för att ange namnet på tabellen och kolumnnamnen i avsnittet ”struktur”.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-123">In this sample, a SQL query is used to extract data from Azure SQL instead of simply specifying the table name and the column names in “structure” section.</span></span> 

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
<span data-ttu-id="dd3fe-124">I det här fallet mappas först resultatet av frågan till kolumner som anges i ”struktur” i datakällan.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-124">In this case, the query results are first mapped to columns specified in “structure” of source.</span></span> <span data-ttu-id="dd3fe-125">Därefter mappas kolumner från källan ”struktur” till kolumner i sink ”struktur” med regler som angetts i columnMappings.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-125">Next, the columns from source “structure” are mapped to columns in sink “structure” with rules specified in columnMappings.</span></span>  <span data-ttu-id="dd3fe-126">Anta att frågan returnerar 5 kolumner, två fler kolumner än de som anges i källan ”struktur”.</span><span class="sxs-lookup"><span data-stu-id="dd3fe-126">Suppose the query returns 5 columns, two more columns than those specified in the “structure” of source.</span></span>

<span data-ttu-id="dd3fe-127">**Kolumnen mappning flöde**</span><span class="sxs-lookup"><span data-stu-id="dd3fe-127">**Column mapping flow**</span></span>

![Kolumnen mappning flödet-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a><span data-ttu-id="dd3fe-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd3fe-129">Next steps</span></span>
<span data-ttu-id="dd3fe-130">Se artikeln en genomgång om hur du använder Kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="dd3fe-130">See the article for a tutorial on using Copy Activity:</span></span> 

- [<span data-ttu-id="dd3fe-131">Kopiera data från Blob Storage till SQL-databas</span><span class="sxs-lookup"><span data-stu-id="dd3fe-131">Copy data from Blob Storage to SQL Database</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
