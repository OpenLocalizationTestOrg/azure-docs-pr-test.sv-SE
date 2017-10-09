---
title: "aaaInvoke lagrade proceduren från Azure Data Factory-Kopieringsaktiviteten | Microsoft Docs"
description: "Lär dig hur kopieringsaktiviteten tooinvoke en lagrad procedur i Azure SQL Database eller SQL Server från ett Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 986377118afb8c08607c2325fcc3ab00b3de9268
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a><span data-ttu-id="abd35-103">Anropa lagrade procedur från kopieringsaktiviteten i Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="abd35-103">Invoke stored procedure from copy activity in Azure Data Factory</span></span>
<span data-ttu-id="abd35-104">När du kopierar data till [SQL Server](data-factory-sqlserver-connector.md) eller [Azure SQL Database](data-factory-azure-sql-connector.md), kan du konfigurera hello **SqlSink** i kopiera aktiviteten tooinvoke en lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="abd35-104">When copying data into [SQL Server](data-factory-sqlserver-connector.md) or [Azure SQL Database](data-factory-azure-sql-connector.md), you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure.</span></span> <span data-ttu-id="abd35-105">Du kanske vill toouse hello lagrade proceduren tooperform eventuella ytterligare behandling (sammanslagning kolumner, söka efter värden, infogning i flera tabeller, etc.) krävs innan du infogar data i toohello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="abd35-105">You may want toouse hello stored procedure tooperform any additional processing (merging columns, looking up values, insertion into multiple tables, etc.) is required before inserting data in toohello destination table.</span></span> <span data-ttu-id="abd35-106">Den här funktionen utnyttjar [Table-Valued parametrar](https://msdn.microsoft.com/library/bb675163.aspx).</span><span class="sxs-lookup"><span data-stu-id="abd35-106">This feature takes advantage of [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).</span></span> 

<span data-ttu-id="abd35-107">hello som följande exempel visar hur tooinvoke en lagrad procedur i en SQL Server-databas från Data Factory-pipelinen (kopieringsaktiviteten):</span><span class="sxs-lookup"><span data-stu-id="abd35-107">hello following sample shows how tooinvoke a stored procedure in a SQL Server database from a Data Factory pipeline (copy activity):</span></span>  

## <a name="output-dataset-json"></a><span data-ttu-id="abd35-108">Utdatauppsättningen JSON</span><span class="sxs-lookup"><span data-stu-id="abd35-108">Output dataset JSON</span></span>
<span data-ttu-id="abd35-109">Ange hello i hello utdatauppsättningen JSON **typen** till: **SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="abd35-109">In hello output dataset JSON, set hello **type** to: **SqlServerTable**.</span></span> <span data-ttu-id="abd35-110">Ställa in också**AzureSqlTable** toouse med en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="abd35-110">Set it too**AzureSqlTable** toouse with an Azure SQL database.</span></span> <span data-ttu-id="abd35-111">Hej värde för **tableName** -egenskap måste matcha hello namnet på första parametern för hello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="abd35-111">hello value for **tableName** property must match hello name of first parameter of hello stored procedure.</span></span>  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a><span data-ttu-id="abd35-112">SqlSink avsnitt i JSON-kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="abd35-112">SqlSink section in copy activity JSON</span></span>
<span data-ttu-id="abd35-113">Definiera hello **SqlSink** under hello kopieringsaktiviteten JSON på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="abd35-113">Define hello **SqlSink** section in hello copy activity JSON as follows.</span></span> <span data-ttu-id="abd35-114">tooinvoke en lagrad procedur vid infoga data i hello sink/måldatabasen, ange värden för båda **SqlWriterStoredProcedureName** och **SqlWriterTableType** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="abd35-114">tooinvoke a stored procedure while inserting data into hello sink/destination database, specify values for both **SqlWriterStoredProcedureName** and **SqlWriterTableType** properties.</span></span> <span data-ttu-id="abd35-115">Beskrivningar av de här egenskaperna finns [SqlSink avsnitt i hello SQL Server-anslutningen artikeln](data-factory-sqlserver-connector.md#sqlsink).</span><span class="sxs-lookup"><span data-stu-id="abd35-115">For descriptions of these properties, see [SqlSink section in hello SQL Server connector article](data-factory-sqlserver-connector.md#sqlsink).</span></span>

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a><span data-ttu-id="abd35-116">Definition av lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="abd35-116">Stored procedure definition</span></span> 
<span data-ttu-id="abd35-117">I databasen, definierar hello lagrad procedur med samma namn som hello **SqlWriterStoredProcedureName**.</span><span class="sxs-lookup"><span data-stu-id="abd35-117">In your database, define hello stored procedure with hello same name as **SqlWriterStoredProcedureName**.</span></span> <span data-ttu-id="abd35-118">hello lagrade proceduren hanterar inkommande data från datalagret för hello källa och infogar data i en tabell i hello måldatabasen.</span><span class="sxs-lookup"><span data-stu-id="abd35-118">hello stored procedure handles input data from hello source data store, and inserts data into a table in hello destination database.</span></span> <span data-ttu-id="abd35-119">hello måste hello första parametern för lagrad procedur matcha hello tabellnamn som definierats i hello dataset JSON (marknadsföring).</span><span class="sxs-lookup"><span data-stu-id="abd35-119">hello name of hello first parameter of stored procedure must match hello tableName defined in hello dataset JSON (Marketing).</span></span>

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a><span data-ttu-id="abd35-120">Typdefinitionen för tabellen</span><span class="sxs-lookup"><span data-stu-id="abd35-120">Table type definition</span></span>
<span data-ttu-id="abd35-121">I databasen, definierar hello typ med samma namn som hello **SqlWriterTableType**.</span><span class="sxs-lookup"><span data-stu-id="abd35-121">In your database, define hello table type with hello same name as **SqlWriterTableType**.</span></span> <span data-ttu-id="abd35-122">hello schemat för hello tabelltyp måste matcha hello schemat för hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="abd35-122">hello schema of hello table type must match hello schema of hello input dataset.</span></span>

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a><span data-ttu-id="abd35-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="abd35-123">Next steps</span></span>
<span data-ttu-id="abd35-124">Granska hello efter anslutningen artiklar som slutförts JSON-exempel:</span><span class="sxs-lookup"><span data-stu-id="abd35-124">Review hello following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="abd35-125">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="abd35-125">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="abd35-126">SQL Server</span><span class="sxs-lookup"><span data-stu-id="abd35-126">SQL Server</span></span>](data-factory-sqlserver-connector.md)
