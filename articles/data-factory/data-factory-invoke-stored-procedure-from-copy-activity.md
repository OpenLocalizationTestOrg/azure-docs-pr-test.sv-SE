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
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Anropa lagrade procedur från kopieringsaktiviteten i Azure Data Factory
När du kopierar data till [SQL Server](data-factory-sqlserver-connector.md) eller [Azure SQL Database](data-factory-azure-sql-connector.md), kan du konfigurera hello **SqlSink** i kopiera aktiviteten tooinvoke en lagrad procedur. Du kanske vill toouse hello lagrade proceduren tooperform eventuella ytterligare behandling (sammanslagning kolumner, söka efter värden, infogning i flera tabeller, etc.) krävs innan du infogar data i toohello måltabellen. Den här funktionen utnyttjar [Table-Valued parametrar](https://msdn.microsoft.com/library/bb675163.aspx). 

hello som följande exempel visar hur tooinvoke en lagrad procedur i en SQL Server-databas från Data Factory-pipelinen (kopieringsaktiviteten):  

## <a name="output-dataset-json"></a>Utdatauppsättningen JSON
Ange hello i hello utdatauppsättningen JSON **typen** till: **SqlServerTable**. Ställa in också**AzureSqlTable** toouse med en Azure SQL database. Hej värde för **tableName** -egenskap måste matcha hello namnet på första parametern för hello lagrade proceduren.  

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

## <a name="sqlsink-section-in-copy-activity-json"></a>SqlSink avsnitt i JSON-kopieringsaktiviteten
Definiera hello **SqlSink** under hello kopieringsaktiviteten JSON på följande sätt. tooinvoke en lagrad procedur vid infoga data i hello sink/måldatabasen, ange värden för båda **SqlWriterStoredProcedureName** och **SqlWriterTableType** egenskaper. Beskrivningar av de här egenskaperna finns [SqlSink avsnitt i hello SQL Server-anslutningen artikeln](data-factory-sqlserver-connector.md#sqlsink).

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

## <a name="stored-procedure-definition"></a>Definition av lagrad procedur 
I databasen, definierar hello lagrad procedur med samma namn som hello **SqlWriterStoredProcedureName**. hello lagrade proceduren hanterar inkommande data från datalagret för hello källa och infogar data i en tabell i hello måldatabasen. hello måste hello första parametern för lagrad procedur matcha hello tabellnamn som definierats i hello dataset JSON (marknadsföring).

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Typdefinitionen för tabellen
I databasen, definierar hello typ med samma namn som hello **SqlWriterTableType**. hello schemat för hello tabelltyp måste matcha hello schemat för hello inkommande dataset.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Nästa steg
Granska hello efter anslutningen artiklar som slutförts JSON-exempel: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
